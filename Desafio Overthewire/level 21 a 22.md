# V. Bandit Level 21 → Level 22 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es analizar el uso de `cron`, el programador de tareas en sistemas Unix/Linux, para descubrir un script que se ejecuta periódicamente y extrae la contraseña del siguiente nivel.

## ¿Qué es `cron`?

`cron` es un daemon en sistemas Unix/Linux que ejecuta tareas programadas en intervalos específicos definidos en archivos `cronjob`. En este nivel, debemos encontrar el `cronjob` que ejecuta un script para recuperar la contraseña de `bandit22`.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit21`
- **Contraseña**: Se obtiene en el nivel anterior.

## Comandos Útiles

Para resolver este nivel, utilizaremos los siguientes comandos:

- `ls -l /etc/cron.d/` - Lista los archivos `cronjob` en el directorio de configuración.
- `cat /etc/cron.d/<cronjob>` - Muestra el contenido de un `cronjob`.
- `cat <ruta_del_script>` - Muestra el contenido del script ejecutado por `cron`.

## Resolución Paso a Paso

### 1. Identificar el `cronjob`

Listamos los archivos `cronjob` en el directorio `/etc/cron.d/`:

```sh
ls -l /etc/cron.d/
```

Salida esperada:

```sh
total 24
-rw-r--r-- 1 root root 120 Sep 19 07:08 cronjob_bandit22
-rw-r--r-- 1 root root 122 Sep 19 07:08 cronjob_bandit23
...
```

Vemos que hay un archivo `cronjob_bandit22`, que es el que nos interesa.

### 2. Inspeccionar el `cronjob`

Leemos el contenido de `cronjob_bandit22`:

```sh
cat /etc/cron.d/cronjob_bandit22
```

Salida esperada:

```sh
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

Esto indica que el script `/usr/bin/cronjob_bandit22.sh` se ejecuta cada minuto.

### 3. Analizar el script

Leemos el contenido del script:

```sh
cat /usr/bin/cronjob_bandit22.sh
```

Salida esperada:

```sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Este script extrae la contraseña de `bandit22` y la almacena en `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`.

### 4. Obtener la contraseña

Leemos el archivo generado por el script:

```sh
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Salida esperada:

```sh
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

### 5. Acceder al Nivel 22

Usamos la contraseña obtenida para iniciar sesión en `bandit22`:

```sh
ssh bandit22@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`.

## Notas Adicionales

- `cron` permite ejecutar tareas programadas automáticamente.
- Se puede analizar `cronjobs` para descubrir tareas que manipulan archivos sensibles.
- Los archivos en `/tmp/` pueden ser accesibles por otros usuarios si tienen los permisos adecuados.

¡Felicidades! Has completado el Nivel 21 y estás listo para continuar con el Nivel 22.

