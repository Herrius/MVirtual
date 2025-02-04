# S. Bandit Level 18 → Level 19 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la contraseña en el archivo `readme` dentro del directorio de inicio. Sin embargo, el sistema ha sido modificado para desconectarnos inmediatamente al iniciar sesión con SSH.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit18`
- **Contraseña**: Se obtiene en el nivel anterior.

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ssh` - Para conectarnos al servidor.
- `ls` - Lista archivos en el directorio.
- `cat` - Muestra el contenido de un archivo.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor y listar archivos sin desconectarse

Dado que al iniciar sesión el sistema nos desconecta de inmediato, podemos ejecutar el comando `ls` directamente al conectarnos con `ssh -t`:

```sh
ssh -t bandit18@bandit.labs.overthewire.org -p 2220 ls
```

Salida esperada:

```sh
readme
Connection to bandit.labs.overthewire.org closed.
```

Esto confirma que el archivo `readme` existe en el directorio de inicio.

### 2. Leer el contenido del archivo sin iniciar sesión

Dado que la sesión se cierra automáticamente, debemos ejecutar `cat readme` directamente en la conexión SSH:

```sh
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

Cuando se solicite la contraseña, ingresamos la obtenida en el nivel anterior.

Salida esperada:

```sh
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```

Este es el password para el siguiente nivel.

### 3. Acceder al Nivel 19

Usamos la contraseña obtenida para iniciar sesión en `bandit19`:

```sh
ssh bandit19@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`.

## Notas Adicionales

- `ssh -t` nos permite ejecutar un comando en el servidor antes de ser desconectados.
- Usar `cat` directamente en la conexión SSH es una forma efectiva de evitar la desconexión automática.

¡Felicidades! Has completado el Nivel 18 y estás listo para continuar con el Nivel 19.

