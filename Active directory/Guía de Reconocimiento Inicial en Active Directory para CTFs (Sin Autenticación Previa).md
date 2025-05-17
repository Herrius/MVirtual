# Guía de Reconocimiento Inicial en Active Directory para CTFs (Sin Autenticación Previa)
La primera fase de un ataque a Active Directory (AD) en un CTF es el **Reconocimiento**. Incluso sin credenciales, se puede obtener una cantidad sorprendente de información. Esta guía resume algunas herramientas y técnicas para esta etapa crucial, ilustradas con ejemplos y hallazgos reales de la máquina **Ledger (thm.local, IP: 10.10.41.115)** de TryHackMe.

## Nmap: El Punto de Partida Esencial
Todo comienza con un escaneo de puertos. `nmap` es la herramienta estándar. Se suele realizar un escaneo rápido de todos los puertos, seguido de un escaneo más detallado con detección de versiones y scripts en los puertos abiertos.
**Resultados del Escaneo Nmap en `10.10.41.115` (Dominio: `thm.local0.`)**
Los siguientes comandos fueron ejecutados contra `10.10.41.115`:
1.  `nmap --privileged -sS -p- -Pn -n -min-rate 5000 -oG ports 10.10.41.115` (Escaneo de todos los puertos TCP)
2.  `nmap --privileged -sVC -p<puertos_abiertos_detectados> -oG services 10.10.41.115` (Escaneo de servicios y scripts)

**Servicios Clave Identificados en `10.10.41.115` y su Relevancia (Sin Autenticación):**
*   **53/tcp (DNS - Simple DNS Plus):** Crucial para identificar el dominio, controladores de dominio (DCs) y otros servicios mediante consultas SRV. Es el primer paso para mapear la infraestructura de AD.
*   **88/tcp (Kerberos - Microsoft Windows Kerberos):** Confirma que es un KDC (parte del DC). No se puede usar sin credenciales, pero es un indicador.
*   **135/tcp, 593/tcp y múltiples puertos altos (ej. 49664-49670) (RPC - Microsoft Windows RPC):** El RPC Endpoint Mapper (135) y otros servicios RPC son vitales. Si se obtiene acceso a `IPC$` (vía SMB), se pueden usar para enumerar usuarios, grupos, etc.
*   **139/tcp (NetBIOS-SSN) y 445/tcp (Microsoft-DS - SMB):** Fundamentales para intentar sesiones nulas o con `guest` (sin contraseña) para listar comparticiones (ej. `IPC$`, `SYSVOL`) y obtener información del dominio.
*   **389/tcp, 636/tcp (LDAP/LDAPS - Microsoft Windows AD LDAP):** El escaneo `-sVC` reveló:
    *   Dominio: `thm.local0.`
    *   Sitio: `Default-First-Site-Name`
    *   Permite intentos de enlace anónimo para extraer información del directorio (RootDSE, OUs, usuarios si los permisos son laxos).
*   **3268/tcp, 3269/tcp (Global Catalog LDAP/LDAPS):** Similar a LDAP, pero para búsquedas en todo el bosque.
*   **80/tcp, 443/tcp (HTTP/HTTPS - Microsoft IIS httpd 10.0):** Servidores web que deben ser investigados en busca de portales (OWA, CertSrv, etc.), información expuesta o vulnerabilidades.
*   **47001/tcp (HTTP - Microsoft HTTPAPI httpd 2.0):** Otro servicio web, posiblemente WinRM o una interfaz de administración.
*   **9389/tcp (ADWS - .NET Message Framing):** Interfaz moderna para AD.

**Conclusión del Escaneo Nmap en `10.10.41.115`:**
La máquina `10.10.41.115` es un Controlador de Dominio para `thm.local0.`. El escaneo Nmap ha proporcionado una lista rica de servicios para investigar, especialmente DNS para mapeo, LDAP para consultas anónimas, SMB para sesiones nulas/guest, y los servicios HTTP para enumeración web.

## Inspección de Servicios Clave en Ledger (thm.local, 10.10.41.115)

### DNS: Mapeando el Dominio
**1. Intento de Transferencia de Zona (AXFR)**
*   **Comando Ejecutado (Real en Ledger):**
    ```bash
    dig axfr @10.10.41.115 thm.local
    ```
*   **Ejemplo de INFORMACIÓN EXITOSA (Inventado - no ocurrió en Ledger):**
    ```
    ; <<>> DiG 9.XX.X <<>> axfr @10.10.41.115 thm.local
    thm.local.              3600    IN      SOA     labyrinth.thm.local. hostmaster.thm.local. 28 900 600 86400 3600
    labyrinth.thm.local.    3600    IN      A       10.10.41.115
    ;; Transfer complete.
    ```
*   **Resultado Real Obtenido en Ledger:**
    ```
    ; Transfer failed.
    ```
*   **Conclusión (Real en Ledger):** La transferencia de zona DNS **falló**.

**2. Consultas de Registros SRV usando `nslookup` (Real en Ledger)**

*   **Extracto de Comandos y Salida Real en Ledger:**
    ```
    > server 10.10.41.115
    > set type=SRV
    > _ldap._tcp.thm.local
    _ldap._tcp.thm.local    service = 0 100 389 labyrinth.thm.local.
    > _kerberos._tcp.thm.local
    _kerberos._tcp.thm.local        service = 0 100 88 labyrinth.thm.local.
    ```
*   **Conclusión (Real en Ledger):** El host `labyrinth.thm.local` proporciona LDAP (389) y Kerberos (88) para `thm.local`, identificándolo como el DC/KDC.

**3. Consultas de Registros SRV y A usando `dig` (Real en Ledger)**

*   **Extracto de Comando y Salida Real en Ledger (para LDAP):**
    ```bash
    dig SRV _ldap._tcp.dc._msdcs.thm.local @10.10.41.115
    ```
    ```
    ;; ANSWER SECTION:
    _ldap._tcp.dc._msdcs.thm.local. 600 IN  SRV     0 100 389 labyrinth.thm.local.
    ;; ADDITIONAL SECTION:
    labyrinth.thm.local.    3600    IN      A       10.10.41.115
    ```
*   **Conclusión (Real en Ledger):** Se confirma que `labyrinth.thm.local` (IP `10.10.41.115`) es el DC.

**Conclusión General de DNS (Real en Ledger):**
Se identificó el dominio `thm.local`, el DC `labyrinth.thm.local` (IP `10.10.41.115`), y los puertos de servicios clave.

### LDAP: Buscando Secretos en el Directorio (Énfasis en Enlace Anónimo)
**Enumeración LDAP/LDAPS Anónima (Sin Credenciales Explícitas):**
Cuando intentas obtener información de LDAP (389) o LDAPS (636) sin proporcionar un usuario y contraseña, estás realizando un "enlace anónimo". Lo que obtengas depende de la configuración del servidor.

**Información que A MENUDO se puede Obtener Anónimamente:**

1.  **Información del RootDSE:**
    *   Casi siempre accesible. Proporciona el `defaultNamingContext` (DN base del dominio, ej: `DC=thm,DC=local`), versiones LDAP, y niveles funcionales.
    *   **Comando:** `ldapsearch -x -H ldap://10.10.41.115 -s base -b "" "(objectClass=*)" namingContexts defaultNamingContext`

2.  **Información Limitada de Configuración y Esquema:** A veces, DNs de estos contextos.

3.  **Listado Básico de Objetos:** Si los permisos son muy laxos, podrías listar DNs de OUs o incluso algunos usuarios bajo el `defaultNamingContext`.
    *   **Comando:** `ldapsearch -x -H ldap://10.10.41.115 -b "DC=thm,DC=local" -s sub "(objectClass=*)" dn`

**Hallazgo Específico Destacado en Ledger (Real - probablemente no anónimo puro, sino con `guest` o enumeración dirigida):**
```
# SUSANNA_MCKNIGHT, Test, ITS, Tier 1, thm.local
dn: CN=SUSANNA_MCKNIGHT,OU=Test,OU=ITS,OU=Tier 1,DC=thm,DC=local
description: Please change it: CHANGEME2023!
sAMAccountName: SUSANNA_MCKNIGHT
```
*   **Conclusión (Real en Ledger):** Se descubrió una contraseña potencial: `CHANGEME2023!` para `SUSANNA_MCKNIGHT`. *Nota: Es menos común obtener este nivel de detalle con un enlace LDAP puramente anónimo; a menudo requiere algún tipo de autenticación de bajo nivel o permisos específicos.*

### SMB: Probando el Acceso a Comparticiones (Énfasis en Sesión Nula/Guest)
*(Los siguientes resultados de `smbclient`, `smbmap`, `crackmapexec`, y `nxc` son **reales** de Ledger, usando `guest` con contraseña vacía, que es una forma de acceso no autenticado o de muy bajo privilegio).*

**Objetivo:** Determinar si una sesión nula o la cuenta `guest` (con contraseña vacía) puede autenticarse y qué comparticiones son visibles/accesibles.

**1. `smbclient -L //10.10.41.115 -U 'guest%'` (Real en Ledger)**
*   **Salida Real:**
```
    Sharename       Type      Comment
    ---------       ----      -------
    ADMIN$          Disk      Remote Admin
    C$              Disk      Default share
    IPC$            IPC       Remote IPC
    NETLOGON        Disk      Logon server share 
    SYSVOL          Disk      Logon server share 
    Reconnecting with SMB1 for workgroup listing.
    do_connect: Connection to 10.10.41.115 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
```
*   **Conclusión (Real en Ledger):** `guest` pudo listar shares estándar.

**2. `smbmap -H 10.10.41.115 -u 'guest' -p ''` (Real en Ledger)**
*   **Salida Real (Extracto):**
    ```
    [+] IP: 10.10.41.115:445        Name: labyrinth.thm.local       Status: Authenticated
            Disk                                                    Permissions     Comment
            ----                                                    -----------     -------
            ADMIN$                                                  NO ACCESS       Remote Admin
            IPC$                                                    READ ONLY       Remote IPC
    ```
*   **Conclusión (Real en Ledger):** `guest` se autentica pero **solo tiene `READ ONLY` sobre `IPC$`**.

**3. `crackmapexec smb 10.10.41.115 -u 'guest' -p '' --shares` (Real en Ledger)**
**4. `nxc smb 10.10.41.115 -u 'guest' -p '' --shares` (Real en Ledger)**
*   **Salida Real (Similar para ambos, Extracto):**
    ```
    SMB         10.10.41.115    445    LABYRINTH        [+] thm.local\guest: 
    SMB         10.10.41.115    445    LABYRINTH        Share           Permissions     Remark
    SMB         10.10.41.115    445    LABYRINTH        -----           -----------     ------
    SMB         10.10.41.115    445    LABYRINTH        IPC$            READ            Remote IPC
    ```
*   **Conclusión (Real en Ledger):** Ambas herramientas confirman la autenticación de `guest` y el acceso de `READ` a `IPC$`.

### RPC: Profundizando con el Acceso `IPC$` Obtenido

*(La siguiente sección es conocimiento general y los ejemplos de salida son **inventados**, basados en el acceso `IPC$` que `guest` tiene en Ledger).*

Dado el acceso `READ ONLY` a `IPC$` para `guest` en Ledger, se puede intentar usar `rpcclient`:

**`impacket-rpcdump @10.10.41.115` (Ejemplo Inventado de Posible Salida)**
Podría revelar interfaces RPC clave como `LSAR`, `SAMR`, `SRVS`.
*   **Utilidad:** Mapea las interfaces RPC disponibles.

**`rpcclient -U "guest%" -N 10.10.41.115` (Ejemplo Inventado de Posibles Hallazgos)**
Si `guest` tiene permisos suficientes a través de `IPC$`:
*   `enumdomusers`: Podría listar usuarios del dominio (ej: `Administrator`, `SUSANNA_MCKNIGHT`).
*   `enumdomgroups`: Podría listar grupos (ej: `Domain Admins`).
*   `querydominfo`: Podría revelar el SID del dominio.
*   `querydispinfo`: Podría revelar la política de contraseñas.
*   **Utilidad:** Podría volcar listas de usuarios, grupos, y políticas del dominio.
