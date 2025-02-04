# 7 Bandit Level 7 → Level 8 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en el archivo `data.txt` junto a la palabra "millionth".

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit7`
- **Contraseña**: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

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
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`.

### 2. Identificar el archivo con la contraseña

Listamos los archivos en el directorio home:

```sh
ls
```

Salida esperada:

```sh
data.txt
```

### 3. Buscar la línea con la palabra "millionth"

Usamos `grep` para encontrar la línea con la contraseña:

```sh
grep "millionth" data.txt
```

Salida esperada:

```sh
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

La segunda columna es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 8

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- `grep` es ideal para buscar palabras clave dentro de archivos.
- Si el archivo contiene muchas líneas, usa `less data.txt` o `head data.txt` para inspeccionarlo manualmente.
- Si el terminal se desconfigura, usa `reset`.

¡Felicidades! Has completado el Nivel 7 y estás listo para continuar con el Nivel 8.

