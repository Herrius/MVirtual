# Bandit Level 15 → Level 16 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es enviar la contraseña del nivel actual (`bandit15`) al puerto `30001` en `localhost` utilizando cifrado SSL/TLS y obtener la contraseña para el siguiente nivel.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit15`
- **Contraseña**: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `echo` - Muestra una cadena de texto.
- `openssl s_client` - Conecta a un servicio con SSL/TLS.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`.

### 2. Conectar al puerto `30001` usando SSL/TLS

Usamos `openssl s_client` para conectarnos al puerto:

```sh
openssl s_client -connect localhost:30001
```

Salida esperada (fragmento relevante):

```sh
CONNECTED(00000003)
---
SSL handshake has read ...
Verify return code: 18 (self-signed certificate)
---
```

### 3. Enviar la contraseña del nivel actual

Una vez conectado, escribimos la contraseña del nivel actual y presionamos `Enter`:

```sh
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

Salida esperada:

```sh
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

La última línea es la contraseña para el siguiente nivel.

### 4. Acceder al Nivel 16

Usamos esta contraseña para iniciar sesión en el siguiente nivel:

```sh
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la nueva contraseña obtenida: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`.

## Notas Adicionales

- `openssl s_client` permite conectarse a servicios que requieren SSL/TLS.
- Si ves mensajes como `DONE`, `RENEGOTIATING` o `KEYUPDATE`, revisa la documentación de `openssl s_client`.

¡Felicidades! Has completado el Nivel 15 y estás listo para continuar con el Nivel 16.

