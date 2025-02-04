# 4 Bandit Level 4 → Level 5 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en el único archivo de texto legible dentro del directorio `inhere`.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit4`
- **Contraseña**: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `ls -la` - Muestra archivos ocultos y con permisos.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `file` - Identifica el tipo de un archivo.
- `find` - Busca archivos en el sistema.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`.

### 2. Listar archivos en el directorio home

```sh
ls
```

Salida esperada:

```sh
inhere
```

El directorio `inhere` contiene el archivo con la contraseña.

### 3. Acceder al directorio `inhere`

```sh
cd inhere
ls
```

Salida esperada:

```sh
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```

Hay varios archivos con nombres que comienzan con `-`, lo que puede dificultar su manipulación.

### 4. Identificar el archivo legible

Dado que solo uno es un archivo de texto legible, usamos el comando `file` para inspeccionar cada uno:

```sh
file *
```

Salida esperada:

```sh
-file00: data
-file01: data
-file02: data
-file03: data
-file04: data
-file05: data
-file06: data
-file07: data
-file08: ASCII text
-file09: data
```

El archivo `-file08` es el único de tipo `ASCII text`, lo que indica que es legible.

### 5. Leer el contenido del archivo

Para visualizar la contraseña, usamos `cat` asegurándonos de evitar que `-` sea interpretado como opción:

```sh
cat ./-file08
```

Salida esperada:

```sh
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

Esta es la contraseña para el siguiente nivel.

### 6. Acceder al Nivel 5

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- Si tu terminal se desconfigura, usa el comando `reset`.
- Para archivos con nombres que comienzan con `-`, usa `./` antes del nombre.
- Si no estás seguro del contenido de un archivo, usa `file` antes de `cat`.

¡Felicidades! Has completado el Nivel 4 y estás listo para continuar con el Nivel 5.

