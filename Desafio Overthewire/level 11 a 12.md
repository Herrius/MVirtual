# Bandit Level 11 → Level 12 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en el archivo `data.txt` y está cifrada con un cifrado ROT13.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit11`
- **Contraseña**: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `grep` - Filtra contenido dentro de archivos.
- `strings` - Extrae texto legible de archivos binarios.
- `base64` - Decodifica y codifica contenido en Base64.
- `tr` - Transforma caracteres.
- `tar`, `gzip`, `bzip2` - Manipulan archivos comprimidos.
- `xxd` - Crea una representación hexadecimal de un archivo.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`.

### 2. Identificar el archivo con la contraseña

Listamos los archivos en el directorio home:

```sh
ls
```

Salida esperada:

```sh
data.txt
```

### 3. Decodificar el contenido en ROT13

El archivo está cifrado con ROT13, por lo que lo decodificamos usando el siguiente comando:

```sh
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Salida esperada:

```sh
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

Esta es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 12

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`.

## Notas Adicionales

- `tr 'A-Za-z' 'N-ZA-Mn-za-m'` aplica el cifrado ROT13.
- Si el archivo es muy grande, usa `less data.txt` para inspeccionarlo manualmente.
- Si el terminal se desconfigura, usa `reset`.

¡Felicidades! Has completado el Nivel 11 y estás listo para continuar con el Nivel 12.

