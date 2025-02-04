# Walkthrough: Bandit Level 27 to Level 28

## Objetivo del Nivel

En este nivel, se nos proporciona acceso a un repositorio Git alojado en un servidor SSH interno. Debemos clonar este repositorio y examinar su contenido para encontrar la contraseña que nos permitirá avanzar al siguiente nivel.

---

## Pasos para Resolver el Nivel

### 1. Acceder al servidor Bandit27

Debemos conectarnos al servidor usando SSH:

```bash
ssh bandit27@bandit.labs.overthewire.org -p 2220
```

Introduce la contraseña obtenida en el nivel anterior.

---

### 2. Clonar el repositorio Git

El repositorio se encuentra en `ssh://bandit27-git@localhost/home/bandit27-git/repo`. Para clonarlo, primero cambiamos al directorio `/tmp` para asegurarnos de tener permisos de escritura:

```bash
cd /tmp
```

Ahora clonamos el repositorio usando Git:

```bash
git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
```

Cuando se solicite la contraseña, ingresa la misma que usaste para conectarte como `bandit27`.

---

### 3. Explorar el contenido del repositorio

Después de clonar el repositorio, accedemos a la carpeta recién creada:

```bash
cd repo
```

Listamos los archivos dentro del repositorio:

```bash
ls -la
```

Debería aparecer un archivo `README`. Lo examinamos con:

```bash
cat README
```

El contenido del archivo mostrará la contraseña para `bandit28`:

```bash
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```

---

## Conclusión

Al completar este nivel, aprendimos cómo interactuar con un repositorio Git remoto usando SSH. En el siguiente nivel, trabajaremos con el historial del repositorio para descubrir versiones anteriores de archivos.

Ahora podemos conectarnos al siguiente nivel usando:

```bash
ssh bandit28@bandit.labs.overthewire.org -p 2220
```

