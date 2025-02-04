# Bandit Level 2 → Level 3 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en un archivo llamado `spaces in this filename` ubicado en el directorio home.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit2`
- **Contraseña**: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

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
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`.

### 2. Listar archivos en el directorio home

Una vez dentro, listamos los archivos del directorio actual con:

```sh
ls -la
```

Salida esperada:

```sh
-rw-r-----  1 bandit3 bandit2   33 Sep 19 07:08 spaces in this filename
```

El archivo con la contraseña tiene espacios en su nombre, lo que puede generar problemas al manipularlo.

### 3. Leer el contenido del archivo con espacios

Para ver su contenido, usamos `cat` con comillas dobles o escape de espacios:

```sh
cat "spaces in this filename"
```

o

```sh
cat spaces\ in\ this\ filename
```

Salida esperada:

```sh
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

Esta es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 3

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- Cuando un archivo contiene espacios en su nombre, es recomendable usar comillas o caracteres de escape (`\`) para acceder a él.
- En caso de problemas, usar `ls -q` puede ayudar a identificar caracteres ocultos en los nombres de archivo.

¡Felicidades! Has completado el Nivel 2 y estás listo para continuar con el Nivel 3.