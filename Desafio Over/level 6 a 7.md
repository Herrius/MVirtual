# Bandit Level 6 → Level 7 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña para el siguiente nivel, que está almacenada en un archivo en algún lugar del servidor con las siguientes características:

- Está **propiedad del usuario** `bandit7`.
- Está **propiedad del grupo** `bandit6`.
- Tiene exactamente **33 bytes** de tamaño.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit6`
- **Contraseña**: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `cd` - Cambia de directorio.
- `cat` - Muestra el contenido de un archivo.
- `file` - Identifica el tipo de un archivo.
- `find` - Busca archivos en el sistema.
- `grep` - Filtra contenido dentro de archivos.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`.

### 2. Buscar el archivo con las características específicas

Usamos el comando `find` para buscar un archivo que:
- Sea un archivo normal (`-type f`).
- Tenga como propietario a `bandit7` (`-user bandit7`).
- Tenga como grupo `bandit6` (`-group bandit6`).
- Tenga exactamente **33 bytes** de tamaño (`-size 33c`).

```sh
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

Salida esperada:

```sh
/var/lib/dpkg/info/bandit7.password
```

El parámetro `2>/dev/null` oculta los mensajes de error de `Permission denied`.

### 3. Leer el contenido del archivo

Para visualizar la contraseña, usamos `cat`:

```sh
cat /var/lib/dpkg/info/bandit7.password
```

Salida esperada:

```sh
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

Esta es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 7

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida.

## Notas Adicionales

- `find` es una herramienta poderosa para buscar archivos con criterios específicos.
- Si ves muchos errores de permisos, usa `2>/dev/null` para ocultarlos.
- Si el terminal se desconfigura, usa `reset`.

¡Felicidades! Has completado el Nivel 6 y estás listo para continuar con el Nivel 7.

