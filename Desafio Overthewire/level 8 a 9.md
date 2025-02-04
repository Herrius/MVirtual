# I. Bandit Level 8 → Level 9 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en el archivo `data.txt` y es la única línea de texto que aparece solo una vez.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit8`
- **Contraseña**: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `grep` - Filtra contenido dentro de archivos.
- `sort` - Ordena líneas de un archivo.
- `uniq` - Muestra solo líneas únicas de un archivo.
- `strings` - Extrae texto legible de archivos binarios.
- `base64` - Decodifica contenido en base64.
- `tr` - Transforma caracteres.
- `tar`, `gzip`, `bzip2` - Manipulan archivos comprimidos.
- `xxd` - Crea una representación hexadecimal de un archivo.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`.

### 2. Identificar el archivo con la contraseña

Listamos los archivos en el directorio home:

```sh
ls
```

Salida esperada:

```sh
data.txt
```

### 3. Encontrar la única línea única

Ordenamos el archivo y usamos `uniq -u` para encontrar la línea que solo aparece una vez:

```sh
sort data.txt | uniq -u
```

Salida esperada:

```sh
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

Esta es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 9

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- `sort` ordena las líneas del archivo antes de pasarlas a `uniq`.
- `uniq -u` devuelve solo las líneas que aparecen una vez en un archivo ordenado.
- Si el archivo es muy grande, usa `less data.txt` para inspeccionarlo manualmente.
- Si el terminal se desconfigura, usa `reset`.

¡Felicidades! Has completado el Nivel 8 y estás listo para continuar con el Nivel 9.

