# Comparativa de Comandos Nmap: Explorando el Ruido Generado

## Introducción
En esta investigación realizo una comparación entre los distintos comandos de Nmap, enfocados en cuánto "ruido" generan en la red. Me baso en un entorno controlado utilizando Kali Linux, dos interfaces de red activas y capturas con Wireshark para registrar el comportamiento.
## Topología de Red
Mi sistema Kali tiene dos interfaces activas:
```bash
eth0: 192.168.184.129
eth1: 192.168.179.138
```
- En `eth0` solo está conectada la máquina Kali.
- En `eth1` está montada una pequeña red de máquinas virtuales con servicios expuestos.
## Descubrimiento Inicial
Se utilizaron los siguientes comandos para identificar qué equipos estaban activos:
```bash
nmap -sn 192.168.184.129/24
nmap -sn 192.168.179.138/24
```
Resultado:
```
192.168.184.0/24: 3 hosts activos
192.168.179.0/24: 7 hosts activos
```
Por ello se eligió la red `192.168.179.0/24` para el escaneo profundo.
## Escaneo de Puertos con Nmap
Se ejecutó el siguiente comando para obtener los puertos abiertos del host `192.168.179.134`:
```bash
nmap -sS -p- -Pn 192.168.179.134
```
Resultado:
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-04-12 14:33 EDT
Nmap scan report for 192.168.179.134
Host is up (0.0014s latency).
Not shown: 65529 closed tcp ports (reset)
PORT      STATE SERVICE
22/tcp    open  ssh
80/tcp    open  http
111/tcp   open  rpcbind
139/tcp   open  netbios-ssn
443/tcp   open  https
32768/tcp open  filenet-tms
MAC Address: 00:0C:29:0B:41:7C (VMware)

Nmap done: 1 IP address (1 host up) scanned in 22.08 seconds
```
## Comparativa de Ruido: Nmap -sS vs Nmap -A
Se capturó el tráfico generado por:
- `nmap -sS -p <puertos> -Pn`
- `nmap -p <puertos> -A`
### Resultados capturados con Wireshark (CSV procesado en Python):
#### Codigo de python
```Python
import pandas as pd

# Rutas de los archivos cargados
file_ss_p_pn = "/mnt/data/nmap ss p pn.csv"
file_p_a = "/mnt/data/nmap p a.csv"

# Cargar los datos
df_ss = pd.read_csv(file_ss_p_pn)
df_a = pd.read_csv(file_p_a)

# Crear un resumen comparativo
summary = {
    "Comando": ["nmap -sS -p <puertos> -Pn", "nmap -p <puertos> -A"],
    "Total de paquetes": [len(df_ss), len(df_a)],
    "Protocolos únicos": [df_ss['Protocol'].nunique(), df_a['Protocol'].nunique()],
    "Protocolos usados": [", ".join(df_ss['Protocol'].unique()), ", ".join(df_a['Protocol'].unique())],
    "Paquetes con SYN": [
        df_ss['Info'].str.contains('SYN', na=False).sum(),
        df_a['Info'].str.contains('SYN', na=False).sum()
    ],
    "Paquetes con ACK": [
        df_ss['Info'].str.contains('ACK', na=False).sum(),
        df_a['Info'].str.contains('ACK', na=False).sum()
    ],
    "Paquetes con RST": [
        df_ss['Info'].str.contains('RST', na=False).sum(),
        df_a['Info'].str.contains('RST', na=False).sum()
    ],
    "Paquetes con UDP": [
        df_ss['Protocol'].str.contains('UDP', na=False).sum(),
        df_a['Protocol'].str.contains('UDP', na=False).sum()
    ],
    "Paquetes con ICMP": [
        df_ss['Protocol'].str.contains('ICMP', na=False).sum(),
        df_a['Protocol'].str.contains('ICMP', na=False).sum()
    ]
}

# Convertir a DataFrame para visualización
comparison_df = pd.DataFrame(summary)
import ace_tools as tools; tools.display_dataframe_to_user(name="Comparativa de comandos Nmap", dataframe=comparison_df)
```
#### Resultados

| Comando          | Total de paquetes | Protocolos únicos | Protocolos usados                                                                                    | Paquetes con SYN | Paquetes con ACK | Paquetes con RST | Paquetes con UDP | Paquetes con ICMP |
| ---------------- | ----------------- | ----------------- | ---------------------------------------------------------------------------------------------------- | ---------------- | ---------------- | ---------------- | ---------------- | ----------------- |
| nmap -sS -p- -Pn | 131093            | 4                 | ARP, DNS, TCP, NBNS                                                                                  | 65541            | 65535            | 65535            | 0                | 0                 |
| nmap -p- -A      | 132564            | 21                | ARP, DNS, TCP, SSH, HTTP, Portmap, NBSS, SSLv3, TLSv1, TLSv1.2, TLSv1.3, TCPwrapped, ICMP, UDP, etc. | 65830            | 66706            | 65569            | 4                | 8                 |

#### Conclusión
- `nmap -A` genera un 1.1% más paquetes.
- Usa más protocolos de capa de aplicación (HTTP, SSL, NBSS, etc).
- Envía paquetes ICMP y UDP adicionales.
- Es más ruidoso y fácilmente detectable por IDS o firewalls.

## Escaneo con Detección de Versiones
Posteriormente se inspeccionaron los puertos detectados mediante:
```bash
nmap -sV -p 22,80,111,139,443,32768 192.168.179.134
```
Resultado:
```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-04-12 14:35 EDT
Nmap scan report for 192.168.179.134
Host is up (0.00046s latency).

PORT      STATE SERVICE     VERSION
22/tcp    open  ssh         OpenSSH 2.9p2 (protocol 1.99)
80/tcp    open  http        Apache httpd 1.3.20 ((Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b)
111/tcp   open  rpcbind     2 (RPC #100000)
139/tcp   open  netbios-ssn Samba smbd (workgroup: IMYGROUP)
443/tcp   open  ssl/https   Apache/1.3.20 (Unix)  (Red-Hat/Linux) mod_ssl/2.8.4 OpenSSL/0.9.6b
32768/tcp open  status      1 (RPC #100024)
MAC Address: 00:0C:29:0B:41:7C (VMware)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 17.62 seconds
```

## Comparativa de Ruido: Nmap -A vs Nmap -sV -A

| Comando                    | Total de paquetes | Protocolos únicos | SYN | ACK  | RST | UDP | ICMP |
| -------------------------- | ----------------- | ----------------- | --- | ---- | --- | --- | ---- |
| nmap -A -p 22,80,111,...   | 1443              | 20                | 297 | 1125 | 40  | 4   | 8    |
| nmap -sV -p 22,80,111,...  | 474               | 25                | 92  | 327  | 9   | 0   | 0    |

#### Conclusión
- `nmap -A` genera 3 veces más tráfico que `-sV -A`.
- Aunque `-sV` contacta más protocolos para identificar versiones, lo hace de forma más eficiente.


## Conclusiones Generales

- Si se busca sigilo, `nmap -sS -p <puertos> -Pn` es el camino.
- `nmap -A` debe reservarse para auditorías internas o cuando el ruido no es un problema.
- `nmap -sV` aporta un punto medio entre detalle y eficiencia.
- Capturar tráfico con Wireshark permite evidenciar claramente el impacto de cada tipo de escaneo.

## Recomendaciones
- Utilizar `-sS -Pn` para descubrimiento inicial.
- Combinar `-sV` con puertos específicos para enumeración precisa.
- Evitar `-A` en redes sensibles o entornos productivos sin autorización.
- Automatizar comparativas con Python para analizar patrones de tráfico.

