# N. Bandit Level 13 → Level 14 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es acceder al usuario `bandit14` utilizando una clave privada SSH en lugar de una contraseña. La clave se encuentra en el archivo `sshkey.private`. Una vez conectados como `bandit14`, podremos leer la contraseña para el siguiente nivel en `/etc/bandit_pass/bandit14`.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit13`
- **Contraseña**: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `chmod` - Cambia los permisos de archivos.
- `ssh` - Conéctate a otro usuario mediante SSH.
- `cat` - Muestra el contenido de un archivo.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`.

### 2. Verificar la clave privada SSH

Listamos los archivos en el directorio:

```sh
ls -l
```

Salida esperada:

```sh
total 4
-rw-r----- 1 bandit14 bandit13 1679 Sep 19 07:08 sshkey.private
```

La clave privada `sshkey.private` nos permitirá conectarnos al siguiente nivel.

### 3. Conectar como `bandit14` usando la clave privada

Para conectarnos con la clave privada, ejecutamos:

```sh
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

Si aparece un mensaje de autenticidad del host, escribimos `yes` y presionamos Enter.

Salida esperada:

```sh
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

### 4. Leer la contraseña del siguiente nivel

Una vez dentro como `bandit14`, verificamos los archivos disponibles:

```sh
ls
```

Como no hay archivos visibles, accedemos directamente a la ubicación donde se almacena la contraseña:

```sh
cat /etc/bandit_pass/bandit14
```

Salida esperada:

```sh
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

Esta es la contraseña para el siguiente nivel.

### 5. Acceder al Nivel 14

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`.

## Notas Adicionales

- Si aparece un error de permisos al usar la clave SSH, podemos intentar cambiar los permisos de la clave con:
  ```sh
  chmod 600 sshkey.private
  ```
  Sin embargo, es posible que este comando no tenga efecto debido a restricciones del servidor.
- `ssh -i` nos permite autenticarnos con una clave privada en lugar de una contraseña.

¡Felicidades! Has completado el Nivel 13 y estás listo para continuar con el Nivel 14.

