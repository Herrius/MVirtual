# M. Bandit Level 12 → Level 13 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en el archivo `data.txt`. Este archivo es un volcado hexadecimal (`hexdump`) de un archivo que ha sido repetidamente comprimido. Debemos revertir estas transformaciones para obtener el contenido original.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit12`
- **Contraseña**: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `xxd -r` - Reconvierte un volcado hexadecimal a su archivo original.
- `file` - Identifica el tipo de un archivo.
- `cp` - Copia archivos.
- `mv` - Renombra archivos.
- `tar`, `gzip`, `bzip2`, `gunzip` - Descomprime archivos.
- `mktemp -d` - Crea un directorio temporal con un nombre aleatorio.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit12@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`.

### 2. Crear un directorio temporal en `/tmp`

Para evitar problemas con permisos, creamos un directorio de trabajo en `/tmp`:

```sh
mktemp -d
```

Salida esperada:

```sh
/tmp/tmp.xxxxxx
```

Accedemos al directorio generado:

```sh
cd /tmp/tmp.xxxxxx
```

### 3. Copiar y reconvertir el archivo `data.txt`

Copiamos el archivo `data.txt` a nuestro directorio temporal:

```sh
cp ~/data.txt .
```

Luego, reconvertimos el volcado hexadecimal a su archivo original:

```sh
xxd -r data.txt compress
```

Verificamos el tipo de archivo:

```sh
file compress
```

### 4. Extraer el contenido del archivo paso a paso

El archivo ha sido comprimido varias veces, por lo que iteramos los siguientes pasos según el tipo de archivo detectado:

#### a) Si es un archivo `gzip`

```sh
mv compress compress.gz
gunzip compress.gz
```

#### b) Si es un archivo `bzip2`

```sh
mv compress compress.bz2
bzip2 -d compress.bz2
```

#### c) Si es un archivo `tar`

```sh
tar -xf compress
```

#### d) Verificar el archivo extraído y repetir el proceso

Continuamos inspeccionando y descomprimiendo hasta obtener un archivo de texto:

```sh
file descompress.out
gunzip -c descompress.out > descompress
file descompress
tar -xf descompress
ls
file data5.bin
tar -xf data5.bin
ls
file data6.bin
bzip2 -dk data6.bin
file data6.bin.out
tar -xf data6.bin.out
ls
file data8.bin
gunzip -c data8.bin > data9
file data9
```

Una vez que `file data9` indica que es un `ASCII text`, podemos leer el contenido:

```sh
cat data9
```

Salida esperada:

```sh
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

Esta es la contraseña para el siguiente nivel.

### 5. Acceder al Nivel 13

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`.

## Notas Adicionales

- `xxd -r` reconvierte un volcado hexadecimal a su archivo original.
- `file` nos ayuda a determinar el tipo de archivo antes de descomprimirlo.
- `mktemp -d` es útil para trabajar con archivos temporales sin modificar archivos del sistema.
- La descompresión debe repetirse en varias etapas hasta obtener el archivo de texto final.

¡Felicidades! Has completado el Nivel 12 y estás listo para continuar con el Nivel 13.

