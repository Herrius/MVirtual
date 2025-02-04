# Z. Bandit Level 25 → Level 27 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es iniciar sesión en `bandit26` desde `bandit25`, pero la shell predeterminada de `bandit26` no es `/bin/bash`, lo que significa que debemos encontrar otra forma de acceder y obtener la contraseña para el siguiente nivel.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit25`
- **Contraseña**: Se obtiene en el nivel anterior.

## Conceptos Clave

- **Shells restringidas**: Algunos usuarios en sistemas Unix/Linux tienen shells restringidas o configuradas de manera que impidan la ejecución de ciertos comandos.
- **Archivos ****`/etc/passwd`**** y ****`/etc/shells`**: En `passwd` podemos ver qué shell tiene asignado un usuario, y en `shells` podemos ver qué opciones están disponibles.

## Comandos Útiles

Para resolver este nivel, utilizaremos los siguientes comandos:

- `ls` - Lista archivos en un directorio.
- `cat <archivo>` - Muestra el contenido de un archivo.
- `file <archivo>` - Muestra el tipo de archivo.
- `ssh -i <archivo> <usuario>@<host>` - Inicia una sesión SSH con una clave privada.
- `whoami` - Muestra el usuario actual.

## Resolución Paso a Paso

### 1. Explorar los Archivos Disponibles

Iniciamos sesión en `bandit25`:

```sh
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

Listamos los archivos en el directorio:

```sh
ls
```

Salida esperada:

```sh
bandit26.sshkey
```

Vemos que hay un archivo llamado `bandit26.sshkey`, que probablemente sea una clave privada para conectarnos a `bandit26`.

### 2. Verificar el Archivo `bandit26.sshkey`

Utilizamos el comando `file` para ver el tipo de archivo:

```sh
file bandit26.sshkey
```

Salida esperada:

```sh
bandit26.sshkey: OpenSSH private key
```

Esto confirma que es una clave privada de SSH.

### 3. Iniciar Sesión en `bandit26`

Intentamos conectarnos utilizando la clave privada:

```sh
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

Salida esperada:

```sh
This is an OverTheWire game server.
...
```

Sin embargo, la sesión se cierra inmediatamente porque `bandit26` no tiene una shell estándar.

### 4. Identificar la Shell de `bandit26`

Consultamos el archivo `/etc/passwd` para ver qué shell tiene asignado `bandit26`:

```sh
cat /etc/passwd | grep bandit26
```

Salida esperada:

```sh
bandit26:x:11026:11026::/home/bandit26:/usr/bin/showtext
```

Esto indica que la shell de `bandit26` es `/usr/bin/showtext` en lugar de `/bin/bash`.

### 5. Analizar `showtext`

Para entender qué hace `showtext`, podemos inspeccionarlo con `file`:

```sh
file /usr/bin/showtext
```

Salida esperada:

```sh
/usr/bin/showtext: POSIX shell script, ASCII text executable
```

Esto nos indica que es un script de shell. Lo leemos con:

```sh
cat /usr/bin/showtext
```

Salida esperada:

```sh
#!/bin/sh
echo "";
echo "";
more /home/bandit26/text.txt
```

El script ejecuta `more`, un programa interactivo para visualizar archivos de texto.

### 6. Romper la Restricción

Podemos aprovechar `more` para abrir una shell interactiva:

1. Nos conectamos de nuevo con la clave privada:

   ```sh
   ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
   ```

2. Durante la ejecución de `more`, presionamos `v` para abrir el editor `vi`.

3. Dentro de `vi`, ejecutamos `:shell` para abrir una shell interactiva.

4. Ahora podemos ejecutar comandos dentro de `bandit26`.

### 7. Obtener la Contraseña de `bandit27`

Dentro de la shell de `bandit26`, leemos la contraseña de `bandit27`:

```sh
cat /etc/bandit_pass/bandit27
```

Salida esperada:

```sh
<contraseña_de_bandit27>
```

### 8. Acceder al Nivel 27

Con la contraseña obtenida, iniciamos sesión en `bandit27`:

```sh
ssh bandit27@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña obtenida.

## Notas Adicionales

- Algunos usuarios pueden tener shells restringidas para evitar acceso completo.
- `more` permite abrir un editor de texto, lo que puede ser explotado para acceder a una shell.
- `vi` tiene opciones avanzadas como `:shell` que permiten ejecutar comandos.

¡Felicidades! Has completado el Nivel 25 y estás listo para continuar con el Nivel 26.

