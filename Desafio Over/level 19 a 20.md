# Bandit Level 19 → Level 20 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es usar un binario con permisos `setuid` que se encuentra en el directorio de inicio para obtener la contraseña del siguiente nivel. 

## ¿Qué es `setuid`?

El bit `setuid` es un permiso especial en sistemas Unix/Linux que permite a un archivo ejecutable ejecutarse con los privilegios del propietario del archivo, en lugar de los privilegios del usuario que lo ejecuta. Esto es útil en programas que requieren acceso elevado, como aquellos que gestionan autenticación o archivos protegidos. En este caso, el binario `bandit20-do` nos permitirá ejecutar comandos con los permisos del usuario `bandit20`.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit19`
- **Contraseña**: Se obtiene en el nivel anterior.

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en el directorio.
- `file` - Determina el tipo de archivo.
- `./<archivo>` - Ejecuta un binario.
- `cat` - Muestra el contenido de un archivo.

## Resolución Paso a Paso

### 1. Identificar el binario con `setuid`

Al listar los archivos en el directorio de inicio, encontramos un binario llamado `bandit20-do`:

```sh
ls
```

Salida esperada:

```sh
bandit20-do
```

Para obtener más información sobre este archivo, usamos `file`:

```sh
file bandit20-do
```

Salida esperada:

```sh
bandit20-do: setuid ELF 32-bit LSB executable...
```

Esto confirma que el binario tiene permisos `setuid` y se ejecuta con los privilegios del usuario propietario (en este caso, `bandit20`).

### 2. Ejecutar el binario

Ejecutamos el binario sin argumentos para ver su propósito:

```sh
./bandit20-do
```

Salida esperada:

```sh
Run a command as another user.
  Example: ./bandit20-do id
```

Esto indica que podemos ejecutar comandos con los permisos de `bandit20`.

### 3. Leer la contraseña del siguiente nivel

Ejecutamos `cat` usando el binario para leer la contraseña almacenada en `/etc/bandit_pass/bandit20`:

```sh
./bandit20-do cat /etc/bandit_pass/bandit20
```

Salida esperada:

```sh
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

Este es el password para el siguiente nivel.

### 4. Acceder al Nivel 20

Usamos la contraseña obtenida para iniciar sesión en `bandit20`:

```sh
ssh bandit20@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`.

## Notas Adicionales

- Los binarios `setuid` permiten ejecutar comandos con los permisos del propietario del archivo.
- Es importante verificar qué comandos podemos ejecutar con `./bandit20-do`.

¡Felicidades! Has completado el Nivel 19 y estás listo para continuar con el Nivel 20.

