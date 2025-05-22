# Lofi
**Objetivo:** Bypassear el filtro del servidor y encontrar la flag.

---

## **1. Identificación de la Vulnerabilidad**

En la premisa inicial de la página de TryHackMe, se indica que el parámetro de la URL podría ser explotado. Esto sugiere que hay una vulnerabilidad relacionada con **Inclusión de Archivos Locales (LFI)**.

![](lofi%20(1).png)

Al observar la URL objetivo, identificamos un parámetro `?page` que parece ser manipulable:
```
http://10.10.26.18/?page=
```
![](lofi%20(2).png)

Se intentó explotar esta vulnerabilidad utilizando traversal de directorios básicos.

---

## **2. Primer Intento: Traversal de Directorios**

El primer intento fue incluir el archivo `/etc/passwd` para confirmar la existencia de la vulnerabilidad. Se utilizó la siguiente URL:

```
http://10.10.26.18/?page=/../../../../etc/passwd
```

Sin embargo, al probar este enfoque, nos encontramos con que el servidor bloquea la solicitud, indicando la presencia de un filtro.

![](lofi%20(3).png)

---

## **3. Bypass del Filtro**

Para bypassar el filtro, simplemente eliminamos el primer `/` en el traversal de directorios. Esto permitió que el servidor procesara la solicitud. La URL modificada fue:

```
http://10.10.26.18/?page=../../../../etc/passwd
```

El servidor devolvió el contenido del archivo `/etc/passwd`, confirmando la existencia de la vulnerabilidad de LFI.

![](lofi%20(4).png)


---

## **4. Localización de la Flag**

Con el filtro bypassado, el siguiente paso fue buscar el archivo `flag.txt`, que típicamente se encuentra en la raíz del sistema (`/`).

Se utilizó la siguiente URL para intentar acceder a la flag:

```
http://10.10.26.18/?page=../../../../../../../../flag.txt
```

Al realizar esta solicitud, obtuvimos el contenido del archivo `flag.txt`, completando con éxito el objetivo.

![](lofi%20(5).png)


