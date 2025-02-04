# 10 Bandit Level 10 → Level 11 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en el archivo `data.txt` y está codificada en formato Base64.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit10`
- **Contraseña**: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

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
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`.

### 2. Identificar el archivo con la contraseña

Listamos los archivos en el directorio home:

```sh
ls
```

Salida esperada:

```sh
data.txt
```

### 3. Decodificar el contenido en Base64

El archivo está codificado en Base64, por lo que lo decodificamos usando el siguiente comando:

```sh
base64 -d data.txt
```

Salida esperada:

```sh
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

Esta es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 11

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`.

## Notas Adicionales

- `base64 -d` decodifica archivos codificados en Base64.
- Si el archivo es muy grande, usa `less data.txt` para inspeccionarlo manualmente.
- Si el terminal se desconfigura, usa `reset`.

¡Felicidades! Has completado el Nivel 10 y estás listo para continuar con el Nivel 11.

