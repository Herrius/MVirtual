# Walkthrough: Bandit Level 32 to Level 33

## Objetivo del Nivel

En este nivel, debemos escapar de un entorno de shell restringido, donde la mayoría de los comandos habituales están bloqueados. El objetivo es obtener acceso a una shell normal y extraer la contraseña del siguiente nivel.

---

## Pasos para Resolver el Nivel

### 1. Acceder al servidor Bandit32

Conéctate al servidor usando SSH:

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

Introduce la contraseña obtenida en el nivel anterior.

---

### 2. Explorar el Entorno Restringido

Después de iniciar sesión, notarás que el shell solo acepta comandos en mayúsculas y bloquea comandos estándar como `ls`, `cat`, `man` y `sh`. Esto indica que estamos en una "uppercase shell" que restringe la ejecución de comandos en minúsculas.

Intentar los comandos comunes devolverá errores de permiso:

```plaintext
>> ls
sh: 1: LS: Permission denied
>> sh
sh: 1: SH: Permission denied
```

Debemos encontrar una forma de ejecutar comandos en minúsculas sin ser bloqueados.

---

### 3. Escapar de la Restricción

Podemos aprovechar que la shell restringida podría permitir la ejecución de comandos mediante el uso de una variable especial. Ejecuta lo siguiente:

```plaintext
>> $0
```

Esto nos lleva a una shell normal (`bash`). Ahora tenemos acceso a todos los comandos sin restricciones.

Verificamos que podemos ejecutar comandos básicos:

```bash
ls -la
```

Esto mostrará un archivo binario llamado `uppershell`, que pertenece a `bandit33` y tiene permisos SUID.

---

### 4. Obtener la Contraseña del Siguiente Nivel

Como ahora tenemos una shell normal, simplemente ejecutamos:

```bash
cat /etc/bandit_pass/bandit33
```

Esto mostrará la contraseña para el usuario `bandit33`:

```plaintext
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```

---

## Conclusión

Este nivel nos enseñó a evadir restricciones impuestas en un shell limitado usando `$0` para obtener acceso a una shell sin restricciones. Ahora podemos conectarnos al siguiente nivel usando:

```bash
ssh bandit33@bandit.labs.overthewire.org -p 2220
```

