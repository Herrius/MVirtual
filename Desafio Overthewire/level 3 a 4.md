# 3 Bandit Level 3 → Level 4 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en un archivo oculto dentro del directorio `inhere`.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit3`
- **Contraseña**: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `ls -la` - Muestra archivos ocultos.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `find` - Busca archivos en el sistema.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`.

### 2. Listar archivos en el directorio home

Una vez dentro, listamos los archivos del directorio actual con:

```sh
ls -la
```

Salida esperada:

```sh
drwxr-xr-x  2 root root 4096 Sep 19 07:08 inhere
```

Esto indica que el archivo está dentro del directorio `inhere`.

### 3. Acceder al directorio `inhere`

Nos movemos al directorio con:

```sh
cd inhere
```

Luego listamos los archivos, incluyendo los ocultos:

```sh
ls -la
```

Salida esperada:

```sh
-rw-r----- 1 bandit4 bandit3   33 Sep 19 07:08 ...Hiding-From-You
```

### 4. Leer el contenido del archivo oculto

Para ver su contenido, usamos `cat`:

```sh
cat ...Hiding-From-You
```

Salida esperada:

```sh
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

Esta es la contraseña para el siguiente nivel.

### 5. Acceder al Nivel 4

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- Para ver archivos ocultos, usa `ls -la`.
- Si el nombre de archivo es extraño, usa `ls -q` para ver caracteres especiales.
- En caso de problemas, utiliza `find . -type f` para buscar archivos dentro del directorio.

¡Felicidades! Has completado el Nivel 3 y estás listo para continuar con el Nivel 4.

