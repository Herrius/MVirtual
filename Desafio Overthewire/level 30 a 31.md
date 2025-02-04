# Walkthrough: Bandit Level 30 to Level 31

## Objetivo del Nivel

En este nivel, debemos acceder a un repositorio Git alojado en un servidor SSH interno y encontrar la contraseña que nos permitirá avanzar al siguiente nivel. La clave estará oculta en el repositorio, por lo que necesitamos examinarlo en detalle.

---

## Pasos para Resolver el Nivel

### 1. Acceder al servidor Bandit30

Conéctate al servidor usando SSH:

```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220
```

Introduce la contraseña obtenida en el nivel anterior.

---

### 2. Crear un directorio temporal y clonar el repositorio Git

Para evitar problemas de permisos, primero creamos un directorio en `/tmp` y nos movemos a él:

```bash
mkdir -p /tmp/bandit30_repo && cd /tmp/bandit30_repo
```

Ahora clonamos el repositorio usando Git:

```bash
git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
```

Cuando se solicite la contraseña, ingresa la misma que usaste para conectarte como `bandit30`.

---

### 3. Explorar el contenido del repositorio

Después de clonar el repositorio, accedemos a la carpeta recién creada:

```bash
cd repo
```

Listamos los archivos disponibles:

```bash
ls
```

Observamos que hay un archivo `README.md`. Lo abrimos para ver su contenido:

```bash
cat README.md
```

El archivo parece estar vacío o no contiene la contraseña directamente. Necesitamos buscar en el historial de Git.

---

### 4. Examinar los objetos perdidos en el repositorio

Dado que no encontramos la contraseña en `README.md`, inspeccionamos los objetos perdidos en el repositorio con:

```bash
git fsck --lost-found
```

Si esto no muestra nada útil, verificamos los commits previos con:

```bash
git reflog
```

Esto nos mostrará un historial de acciones recientes. Si no encontramos nada, podemos buscar en el contenido del repositorio utilizando:

```bash
git grep -i "pass" $(git rev-list --all)
```

Si este comando no devuelve resultados, intentamos buscar en los archivos internos de Git:

```bash
grep -Ri "password" .git
```

---

### 5. Buscar etiquetas en el repositorio

Otra forma de encontrar información oculta es verificar si hay etiquetas en el repositorio:

```bash
git tag
```

Si hay una etiqueta llamada `secret`, podemos inspeccionarla con:

```bash
git show secret
```

Esto revelará la contraseña:

```plaintext
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```

---

## Conclusión

Al completar este nivel, aprendimos cómo inspeccionar el historial y las etiquetas en un repositorio Git para recuperar información oculta. Ahora podemos conectarnos al siguiente nivel usando:

```bash
ssh bandit31@bandit.labs.overthewire.org -p 2220
```