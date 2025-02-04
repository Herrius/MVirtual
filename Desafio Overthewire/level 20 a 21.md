# Bandit Level 20 → Level 21 - Walkthrough

## Objetivo del Nivel

El objetivo de este nivel es utilizar un binario con permisos `setuid` llamado `suconnect`, que conecta a un puerto en localhost y verifica la contraseña del nivel anterior. Si la contraseña es correcta, devolverá la contraseña del siguiente nivel.

## ¿Qué es `setuid`?

El bit `setuid` es un permiso especial en sistemas Unix/Linux que permite a un archivo ejecutable ejecutarse con los privilegios del propietario del archivo, en lugar de los privilegios del usuario que lo ejecuta. En este caso, `suconnect` se ejecuta con los permisos de `bandit21`, lo que nos permite acceder a información restringida.

## Información de Conexión

- **Host**: `bandit.labs.overthewire.org`
- **Puerto**: `2220`
- **Usuario**: `bandit20`
- **Contraseña**: Se obtiene en el nivel anterior.

## Comandos Útiles

Para resolver este nivel, utilizaremos los siguientes comandos:

- `nc -l -p <puerto>` - Inicia un servidor de escucha en un puerto específico.
- `./suconnect <puerto>` - Conecta al puerto especificado en localhost.
- `echo "<mensaje>" | nc -l -p <puerto>` - Envia un mensaje al servidor de escucha.

## Resolución Paso a Paso

### 1. Identificar el binario `setuid`

Al listar los archivos en el directorio de inicio, encontramos un binario llamado `suconnect`:

```sh
ls
```

Salida esperada:

```sh
suconnect
```

Para obtener más información sobre este archivo, usamos `file`:

```sh
file suconnect
```

Salida esperada:

```sh
suconnect: setuid ELF 32-bit LSB executable...
```

Esto confirma que `suconnect` tiene permisos `setuid` y se ejecuta con los privilegios del usuario propietario.

### 2. Iniciar un servidor de escucha

Abrimos un segundo terminal y ejecutamos:

```sh
echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l -p 1112
```

Este comando inicia un servidor de escucha en el puerto `1112` y envía la contraseña cuando se establece la conexión.

### 3. Ejecutar `suconnect`

En el primer terminal, ejecutamos `suconnect` apuntando al puerto `1112`:

```sh
./suconnect 1112
```

Salida esperada:

```sh
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
```

### 4. Obtener la contraseña del siguiente nivel

En el segundo terminal, el servidor de escucha recibirá y devolverá la contraseña del nivel 21:

```sh
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

### 5. Acceder al Nivel 21

Usamos la contraseña obtenida para iniciar sesión en `bandit21`:

```sh
ssh bandit21@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresamos la contraseña `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`.

## Notas Adicionales

- `suconnect` se ejecuta con permisos elevados y verifica la contraseña ingresada.
- Un servidor de escucha es útil para interactuar con programas que esperan entradas de red.
- Verifica que el puerto utilizado no esté ocupado antes de ejecutar el servidor de escucha.

¡Felicidades! Has completado el Nivel 20 y estás listo para continuar con el Nivel 21.

