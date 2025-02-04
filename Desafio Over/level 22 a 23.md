# Bandit Level 22 → Level 23 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es analizar el uso de `cron`, el programador de tareas en sistemas Unix/Linux, para descubrir un script que se ejecuta periódicamente y extrae la contraseña del siguiente nivel.

## ¿Qué es `cron`?

`cron` es un daemon en sistemas Unix/Linux que ejecuta tareas programadas en intervalos específicos definidos en archivos `cronjob`. En este nivel, debemos encontrar el `cronjob` que ejecuta un script para recuperar la contraseña de `bandit23`.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit22`
- **Contraseña**: Se obtiene en el nivel anterior.

## Comandos Útiles

Para resolver este nivel, utilizaremos los siguientes comandos:

- `ls -l /etc/cron.d/` - Lista los archivos `cronjob` en el directorio de configuración.
- `cat /etc/cron.d/<cronjob>` - Muestra el contenido de un `cronjob`.
- `cat <ruta_del_script>` - Muestra el contenido del script ejecutado por `cron`.
- `echo <cadena> | md5sum` - Calcula el hash MD5 de una cadena.

## Resolución Paso a Paso

### 1. Identificar el `cronjob`

Listamos los archivos `cronjob` en el directorio `/etc/cron.d/`:

```sh
ls -l /etc/cron.d/
```

Salida esperada:

```sh
total 24
-rw-r--r-- 1 root root 122 Sep 19 07:08 cronjob_bandit23
...
```

Vemos que hay un archivo `cronjob_bandit23`, que es el que nos interesa.

### 2. Inspeccionar el `cronjob`

Leemos el contenido de `cronjob_bandit23`:

```sh
cat /etc/cron.d/cronjob_bandit23
```

Salida esperada:

```sh
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

Esto indica que el script `/usr/bin/cronjob_bandit23.sh` se ejecuta cada minuto.

### 3. Analizar el script

Leemos el contenido del script:

```sh
cat /usr/bin/cronjob_bandit23.sh
```

Salida esperada:

```sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

Este script extrae la contraseña de `bandit23` y la almacena en `/tmp/` con un nombre basado en el hash MD5 de la cadena `I am user bandit23`.

### 4. Calcular el nombre del archivo

Ejecutamos el siguiente comando para obtener el hash MD5:

```sh
echo "I am user bandit23" | md5sum | cut -d ' ' -f 1
```

Salida esperada:

```sh
8ca319486bfbbc3663ea0fbe81326349
```

### 5. Obtener la contraseña

Leemos el archivo generado por el script en `/tmp/`:

```sh
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

Salida esperada:

```sh
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga
```

### 6. Acceder al Nivel 23

Usamos la contraseña obtenida para iniciar sesión en `bandit23`:

```sh
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`.

## Notas Adicionales

- `cron` permite ejecutar tareas programadas automáticamente.
- Podemos analizar `cronjobs` para descubrir tareas que manipulan archivos sensibles.
- El uso de `md5sum` en este caso se basa en la identificación del usuario.

¡Felicidades! Has completado el Nivel 22 y estás listo para continuar con el Nivel 23.

