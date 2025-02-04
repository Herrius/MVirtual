# Bandit Level 9 → Level 10 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en el archivo `data.txt` y se encuentra dentro de una de las pocas cadenas de texto legibles, precedida por varios caracteres `=`.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit9`
- **Contraseña**: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `grep` - Filtra contenido dentro de archivos.
- `strings` - Extrae texto legible de archivos binarios.
- `sort` - Ordena líneas de un archivo.
- `uniq` - Muestra solo líneas únicas de un archivo.
- `base64` - Decodifica contenido en base64.
- `tr` - Transforma caracteres.
- `tar`, `gzip`, `bzip2` - Manipulan archivos comprimidos.
- `xxd` - Crea una representación hexadecimal de un archivo.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`.

### 2. Identificar el archivo con la contraseña

Listamos los archivos en el directorio home:

```sh
ls
```

Salida esperada:

```sh
data.txt
```

### 3. Extraer cadenas de texto legibles

El archivo contiene muchas líneas de caracteres no legibles, por lo que usamos `strings` para extraer las partes que pueden ser leídas:

```sh
strings data.txt | grep '===='
```

Salida esperada:

```sh
}========== the
3JprD========== passwordi
~fDV3========== is
D9========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

La última línea contiene la contraseña del siguiente nivel.

### 4. Acceder al Nivel 10

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit10@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`.

## Notas Adicionales

- `strings` ayuda a extraer texto legible dentro de archivos binarios o con caracteres no imprimibles.
- `grep` filtra las líneas con `====`, que marcan la contraseña.
- Si el archivo es muy grande, usa `less data.txt` para inspeccionarlo manualmente.
- Si el terminal se desconfigura, usa `reset`.

¡Felicidades! Has completado el Nivel 9 y estás listo para continuar con el Nivel 10.

