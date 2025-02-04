# AC. Walkthrough: Bandit Level 29 to Level 30

## Objetivo del Nivel

En este nivel, se nos proporciona acceso a un repositorio Git alojado en un servidor SSH interno. Debemos clonar este repositorio y examinar su historial para encontrar la contraseña que nos permitirá avanzar al siguiente nivel.

---

## Pasos para Resolver el Nivel

### 1. Acceder al servidor Bandit29

Debemos conectarnos al servidor usando SSH:

```bash
ssh bandit29@bandit.labs.overthewire.org -p 2220
```

Introduce la contraseña obtenida en el nivel anterior.

---

### 2. Clonar el repositorio Git

El repositorio se encuentra en `ssh://bandit29-git@localhost/home/bandit29-git/repo`. Para clonarlo, primero cambiamos al directorio `/tmp` para asegurarnos de tener permisos de escritura:

```bash
cd /tmp
```

Ahora clonamos el repositorio usando Git:

```bash
git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
```

Cuando se solicite la contraseña, ingresa la misma que usaste para conectarte como `bandit29`.

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

Veremos algo como esto:

```plaintext
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```

Esto nos indica que la contraseña no está en el archivo actual, sino en el historial del repositorio.

---

### 4. Revisar el historial de commits

Para encontrar la contraseña, revisamos el historial de commits con:

```bash
git log --oneline --all --graph
```

Esto mostrará una lista de commits previos. Buscamos uno que haya modificado el archivo `README.md`. Podemos inspeccionar un commit específico con:

```bash
git show <ID_DEL_COMMIT>
```

Por ejemplo, si encontramos un commit con ID `081ac38`, ejecutamos:

```bash
git show 081ac38
```

Esto revelará una modificación en el archivo `README.md`, donde encontraremos la contraseña:

```diff
- password: <no passwords in production!>
+ password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```

---

## Conclusión

Al completar este nivel, aprendimos cómo examinar ramas y commits en un repositorio Git para recuperar información eliminada o modificada en el historial.

Ahora podemos conectarnos al siguiente nivel usando:

```bash
ssh bandit30@bandit.labs.overthewire.org -p 2220
```