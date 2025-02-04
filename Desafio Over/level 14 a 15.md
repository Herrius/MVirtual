# Bandit Level 14 → Level 15 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es enviar la contraseña del nivel actual (`bandit14`) al puerto `30000` en `localhost` y obtener la contraseña para el siguiente nivel.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit14`
- **Contraseña**: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `echo` - Muestra una cadena de texto.
- `nc` (netcat) - Envía y recibe datos a través de la red.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`.

### 2. Enviar la contraseña al puerto `30000`

Usamos el comando `echo` para enviar la contraseña al puerto especificado usando `nc`:

```sh
echo "MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS" | nc localhost 30000
```

Salida esperada:

```sh
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

La última línea es la contraseña para el siguiente nivel.

### 3. Acceder al Nivel 15

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`.

## Notas Adicionales

- `nc` (netcat) es una herramienta útil para interactuar con servicios de red.
- `echo` envía la contraseña como entrada al puerto especificado.

¡Felicidades! Has completado el Nivel 14 y estás listo para continuar con el Nivel 15.

