# AE. Walkthrough: Bandit Level 31 to Level 32

## Objetivo del Nivel

En este nivel, debemos acceder a un repositorio Git alojado en un servidor SSH interno y realizar un cambio en él para poder avanzar al siguiente nivel. Se nos indica que debemos añadir un archivo específico con contenido determinado y hacer un push de este cambio al repositorio remoto.

---

## Pasos para Resolver el Nivel

### 1. Acceder al servidor Bandit31

Conéctate al servidor usando SSH:

```bash
ssh bandit31@bandit.labs.overthewire.org -p 2220
```

Introduce la contraseña obtenida en el nivel anterior.

---

### 2. Crear un directorio temporal y clonar el repositorio Git

Para evitar problemas de permisos, primero creamos un directorio en `/tmp` y nos movemos a él:

```bash
mkdir -p /tmp/bandit31_repo && cd /tmp/bandit31_repo
```

Ahora clonamos el repositorio usando Git:

```bash
git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
```

Cuando se solicite la contraseña, ingresa la misma que usaste para conectarte como `bandit31`.

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

Aquí se nos indica que debemos agregar un archivo llamado `key.txt` con el contenido `May I come in?` y hacer push al repositorio.

---

### 4. Crear y agregar el archivo requerido

Creamos el archivo `key.txt` y le añadimos el contenido indicado:

```bash
echo 'May I come in?' > key.txt
```

Agregamos el archivo al índice de Git:

```bash
git add -f key.txt
```

Confirmamos que el archivo está listo para ser enviado:

```bash
git status
```

Realizamos el commit con un mensaje descriptivo:

```bash
git commit -m "Añadiendo key.txt con el mensaje requerido"
```

---

### 5. Hacer push del cambio al repositorio remoto

Para completar la tarea, hacemos push al repositorio remoto:

```bash
git push -u origin master
```

Cuando se solicite la contraseña, ingresa la misma que usaste para conectarte como `bandit31`.

Si todo se realizó correctamente, el servidor responderá con un mensaje indicando que el cambio fue validado y mostrará la contraseña del siguiente nivel:

```plaintext
3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
```

---

## Conclusión

En este nivel aprendimos a interactuar con un repositorio Git para modificar su contenido y hacer push de los cambios. Ahora podemos conectarnos al siguiente nivel usando:

```bash
ssh bandit32@bandit.labs.overthewire.org -p 2220
```

