# 0 Bandit Level 0 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es conectarse al servidor mediante SSH utilizando las credenciales proporcionadas. Una vez dentro, podemos acceder a la página del Nivel 1 para continuar.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit0`
- **Contraseña**: `bandit0`

## Conceptos Claves

Antes de resolver este nivel, es importante comprender algunos conceptos clave:

- **SSH (Secure Shell)**: Protocolo de red que permite la comunicación segura entre dos dispositivos a través de una conexión encriptada.
- **Autenticación en SSH**: Puede realizarse mediante usuario/contraseña o mediante claves SSH. En este nivel utilizaremos usuario y contraseña.
- **Puertos de red**: SSH generalmente usa el puerto 22, pero en este caso utilizaremos el puerto 2220.
- **Terminal o consola**: Necesaria para ejecutar los comandos de conexión.

## Resolución Técnica Paso a Paso

### 1. Verificar si tienes SSH instalado

Antes de conectarte, es recomendable verificar si SSH está instalado en tu sistema. En Linux y macOS puedes comprobarlo con:

```sh
ssh -V
```

Si estás en Windows, puedes utilizar el cliente OpenSSH integrado en PowerShell o instalar PuTTY como alternativa.

### 2. Entender la estructura del comando SSH

El comando SSH para conectarse a un servidor remoto sigue la estructura:

```sh
ssh usuario@host -p puerto
```

Donde:
- `usuario` es el nombre de usuario que se usará para la conexión.
- `host` es el nombre de dominio o dirección IP del servidor remoto.
- `-p` permite especificar un puerto diferente al predeterminado (22 en SSH estándar).

### 3. Conectarse al servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

- Esto inicia una conexión SSH al servidor `bandit.labs.overthewire.org`.
- Se especifica el usuario `bandit0`.
- Se indica que el puerto a utilizar es el `2220`.

### 4. Ingresar la contraseña

Cuando el sistema lo solicite, ingresamos la contraseña `bandit0`. (Recuerda que en Linux, al escribir contraseñas en la terminal, no se mostrarán caracteres visibles como asteriscos `*` o puntos `.` para mayor seguridad.)

### 5. Confirmar acceso exitoso

Si la conexión es exitosa, veremos un mensaje de bienvenida de OverTheWire, indicando que estamos dentro del sistema remoto.

### 6. Explorar el entorno

Al estar dentro, podemos listar los archivos disponibles con:

```sh
ls
```

Esto nos permitirá identificar qué archivos están disponibles y cómo podemos proceder al siguiente nivel.Una vez realizado el archivo encontrado es `readme` el cual lo leeremos con
```sh
head readme
```
Y tendremos la contraseña para el siguiente nivel

### 7. Acceder al Nivel 1

Para continuar, dirígete a la [página del Nivel 1](https://overthewire.org/wargames/bandit/bandit1.html) y sigue las instrucciones proporcionadas.
