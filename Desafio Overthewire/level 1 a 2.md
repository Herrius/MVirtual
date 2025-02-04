# B. Bandit Level 1 → Level 2 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en un archivo llamado `-` ubicado en el directorio home.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit1`
- **Contraseña**: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:
- `ls` - Lista archivos en un directorio.
- `cat` - Muestra el contenido de un archivo.
- `cd` - Cambia de directorio.
- `file` - Identifica el tipo de archivo.
- `du` - Muestra el uso de espacio de archivos y directorios.
- `find` - Busca archivos en el sistema.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`.

### 2. Listar archivos en el directorio home

Una vez dentro, listamos los archivos del directorio actual:

```sh
ls
```

Salida esperada:

```sh
-
```

El archivo con la contraseña se llama `-`, lo que puede causar problemas al tratar de abrirlo de manera convencional.

### 3. Leer el contenido del archivo `-`

Para ver su contenido, usamos el comando `cat` con la ruta relativa:

```sh
cat ./-
```

Salida esperada:

```sh
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

Esta es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 2

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- Si intentas `cat -`, el sistema interpretará `-` como una opción en lugar de un nombre de archivo, por lo que se debe usar `cat ./-`.
- En caso de problemas con nombres de archivo inusuales, puedes usar `ls -la` para ver detalles o `file ./-` para verificar su tipo.

¡Felicidades! Has completado el Nivel 1 y estás listo para continuar con el Nivel 2.

