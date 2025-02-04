# F. Bandit Level 5 → Level 6 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en un archivo dentro del directorio `inhere` con las siguientes características:

- Es un archivo legible por humanos.
- Tiene exactamente 1033 bytes de tamaño.
- No es ejecutable.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit5`
- **Contraseña**: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `file` - Identifica el tipo de un archivo.
- `find` - Busca archivos en el sistema.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `lrIWWI6bB37kxfiCQZqUdOIYfr6eEeqR`.

### 2. Acceder al directorio `inhere`

```sh
ls
cd inhere
ls
```

Salida esperada:

```sh
maybehere00  maybehere03  maybehere06  maybehere09  maybehere12  maybehere15  maybehere18
maybehere01  maybehere04  maybehere07  maybehere10  maybehere13  maybehere16  maybehere19
maybehere02  maybehere05  maybehere08  maybehere11  maybehere14  maybehere17
```

Este directorio contiene múltiples subdirectorios que pueden contener la contraseña.

### 3. Buscar el archivo con las características específicas

Utilizamos `find` para buscar un archivo que cumpla con los criterios dados:

```sh
find . -type f -size 1033c
```

Salida esperada:

```sh
./maybehere07/.file2
```

### 4. Leer el contenido del archivo

Para visualizar la contraseña, usamos `cat`:

```sh
cat ./maybehere07/.file2
```

Salida esperada:

```sh
HWasnPhtq9AVKe0dmk45nxy20cvUa6EGl
```

Esta es la contraseña para el siguiente nivel.

### 5. Acceder al Nivel 6

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- `find` permite buscar archivos basados en tamaño y tipo.
- `1033c` en `find` indica que estamos buscando un archivo de 1033 bytes.
- Si el terminal se desconfigura, usa `reset`.

¡Felicidades! Has completado el Nivel 5 y estás listo para continuar con el Nivel 6.

