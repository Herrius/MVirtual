# R. Bandit Level 17 → Level 18 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar la única línea modificada entre los archivos `passwords.old` y `passwords.new`, ya que contiene la contraseña para el siguiente nivel.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit17`
- **Contraseña**: Se obtiene en el nivel anterior.

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `ls` - Lista archivos en el directorio.
- `head` - Muestra las primeras líneas de un archivo.
- `diff` - Compara dos archivos línea por línea.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit17@bandit.labs.overthewire.org -p 2220
```

Ingresamos la contraseña obtenida en el nivel anterior.

### 2. Listar los archivos disponibles

```sh
ls
```

Salida esperada:

```sh
passwords.new  passwords.old
```

### 3. Inspeccionar los archivos

Podemos verificar el contenido con `head`:

```sh
head passwords.new
head passwords.old
```

Ambos archivos contienen varias líneas de texto con contraseñas.

### 4. Comparar los archivos

Para encontrar la única línea diferente, usamos el comando `diff`:

```sh
diff passwords.new passwords.old
```

Salida esperada:

```sh
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> ktfgBvpMzWKR5ENj26IbLGSblgUG9CzB
```

La línea `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO` es la única diferente, lo que indica que esta es la contraseña para `bandit18`.

### 5. Acceder al Nivel 18

Usamos la contraseña obtenida para iniciar sesión en `bandit18`:

```sh
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`.

## Notas Adicionales

- `diff` es una herramienta clave para comparar archivos de texto.
- Asegúrate de copiar la línea correcta desde `diff`, ya que la sintaxis muestra el cambio entre los dos archivos.

¡Felicidades! Has completado el Nivel 17 y estás listo para continuar con el Nivel 18.

