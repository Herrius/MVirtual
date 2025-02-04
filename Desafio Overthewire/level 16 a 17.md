# Bandit Level 16 → Level 17 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es encontrar un puerto en el rango `31000-32000` que acepte la contraseña del nivel actual (`bandit16`), identificar si usa SSL/TLS y obtener las credenciales para el siguiente nivel.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit16`
- **Contraseña**: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

## Comandos Útiles

Para resolver este nivel, puedes necesitar los siguientes comandos:

- `nmap` - Escanea puertos abiertos.
- `openssl s_client` - Conecta a un servicio con SSL/TLS.

## Resolución Paso a Paso

### 1. Iniciar sesión en el servidor

Ejecutamos el siguiente comando en la terminal:

```sh
ssh bandit16@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`.

### 2. Escanear los puertos abiertos en el rango `31000-32000`

Ejecutamos `nmap` para encontrar puertos activos en `localhost`:

```sh
nmap -p 31000-32000 localhost
```

Salida esperada:

```sh
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

Estos son los puertos abiertos en el sistema.

### 3. Identificar puertos con SSL/TLS

Probamos cada puerto con `openssl s_client` para ver si requiere cifrado:

```sh
openssl s_client -connect localhost:31046 -quiet
openssl s_client -connect localhost:31518 -quiet
openssl s_client -connect localhost:31691 -quiet
openssl s_client -connect localhost:31790 -quiet
openssl s_client -connect localhost:31960 -quiet
```

Si el puerto devuelve un error de protocolo inesperado, probablemente no usa SSL/TLS. Si responde con un certificado `SnakeOil` y permite ingresar la contraseña, es el correcto.

### 4. Enviar la contraseña al puerto correcto

Una vez identificado el puerto correcto (en este caso, `31790`), enviamos la contraseña y obtenemos la clave privada SSH para `bandit17`:

```sh
openssl s_client -connect localhost:31790 -quiet
```

Ingresamos la contraseña:

```sh
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

Salida esperada:

```sh
Correct!
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

### 5. Guardar la clave privada SSH

Copiamos la clave en un archivo en nuestro sistema local:

```sh
nano bandit17.key
```

Pegamos la clave y guardamos (`CTRL+X`, `Y`, `Enter`). Luego, cambiamos los permisos:

```sh
chmod 600 bandit17.key
```

### 6. Acceder al Nivel 17

Usamos la clave privada para iniciar sesión en `bandit17`:

```sh
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

## Notas Adicionales

- `nmap` nos ayuda a encontrar puertos abiertos.
- `openssl s_client` identifica conexiones SSL/TLS.
- La clave privada obtenida nos permite acceder al siguiente nivel.

¡Felicidades! Has completado el Nivel 16 y estás listo para continuar con el Nivel 17.

