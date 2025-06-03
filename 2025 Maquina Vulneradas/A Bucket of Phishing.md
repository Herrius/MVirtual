# A Bucket of Phish - THM
## Información General
- **Nombre de la máquina**: DarkInjectorPhish (Bucket S3)
- **Dirección IP**: `darkinjector-phish.s3-website-us-west-2.amazonaws.com` (Endpoint del sitio web S3) y `darkinjector-phish.s3.us-west-2.amazonaws.com` (Endpoint S3 API)
- **Sistema Operativo**: AWS S3 (Servicio de almacenamiento de objetos)
- **Objetivo**: Recuperar la lista de credenciales de usuarios víctimas almacenada en un sitio de phishing alojado en S3.

---

## Parte 1: Reconocimiento
### Teoría
El reconocimiento inicial es crucial para mapear la superficie de ataque digital. Este proceso implica la identificación de hosts activos, la determinación de los puertos abiertos y la versión de los servicios que se ejecutan en ellos. Para este escenario, dado que se parte de una URL, el enfoque principal del reconocimiento se dirige hacia el servicio web HTTP/HTTPS y la tecnología subyacente del servidor. El objetivo es entender cómo está estructurado el sitio y qué tecnologías emplea, lo cual puede ofrecer pistas sobre posibles vectores de ataque.

### Herramientas y Comandos
La investigación inicial se basó en la URL proporcionada, `http://darkinjector-phish.s3-website-us-west-2.amazonaws.com`. Las herramientas principales fueron un navegador web para la inspección manual y `curl` para examinar las cabeceras HTTP y el contenido de respuesta.

```bash
# Inspección de cabeceras y respuesta inicial del sitio principal
curl -i http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/
``````bash
# Intento de acceso a rutas conocidas o sospechosas
curl -i http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/login
curl -i http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/reset-password
```

### Resultados relevantes
El análisis de la URL base y su estructura de nombres (`s3-website-us-west-2.amazonaws.com`) confirmó que el sitio estaba alojado en un bucket de Amazon S3, configurado para servir contenido estático. La inspección de rutas específicas reveló:

*   `http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/login`: Devuelve un código de estado `HTTP/1.1 200 OK` con contenido HTML correspondiente a una página de inicio de sesión.
*   `http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/reset-password`: Devuelve un código de estado `HTTP/1.1 404 Not Found` con un mensaje de error específico de S3:
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
        <Code>NoSuchKey</Code>
        <Message>The specified key does not exist.</Message>
        <Key>reset-password</Key>
        <RequestId>...</RequestId>
        <HostId>...</HostId>
    </Error>
    ```
    Esto indica que el objeto `reset-password` no existe en el bucket.

Un intento de enviar datos mediante `POST` a la ruta `/login` (simulando el envío de un formulario) desde el navegador o `curl` arrojó:
```
HTTP/1.1 405 Method Not Allowed
Server: AmazonS3
Content-Type: application/xml
<Error>
    <Code>MethodNotAllowed</Code>
    <Message>The specified method is not allowed against this resource.</Message>
    <Method>POST</Method>
    <ResourceType>OBJECT</ResourceType>
    ...
</Error>
```
Este error `405 Method Not Allowed` es una pista crucial, sugiriendo que `/login` es un objeto estático (por ejemplo, `login.html`) y S3 no está configurado para procesar peticiones `POST` en este objeto. Esto implica que el robo de credenciales debe ocurrir por un mecanismo diferente al envío tradicional de formularios a un script de backend en esa misma ruta.

---

## Parte 2: Enumeración
### Teoría
La fase de enumeración se enfoca en profundizar en los servicios y recursos identificados durante el reconocimiento. El objetivo es descubrir puntos débiles específicos, archivos expuestos, configuraciones erróneas o cualquier información que pueda conducir a una explotación. Para un bucket S3 que aloja un sitio web estático, la enumeración implica buscar archivos no enlazados directamente, configuraciones de permisos laxas en el bucket o en objetos individuales, y posibles listados de directorios o buckets.

### Herramientas y técnicas
1.  **Análisis del Código Fuente del Cliente**: Se examinó el código HTML de la página `http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/login` para entender cómo se gestionaba el formulario de inicio de sesión.
    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Cmail Webmail Login</title>
        <!-- ... estilos ... -->
    </head>
    <body>
    <div class="login-container">
        <h1>Cmail Login</h1>
        <form action="/login" method="POST">
            <label for="email">Email Address</label>
            <input type="text" id="email" name="email" placeholder="Enter your email" required>
            
            <label for="password">Password</label>
            <input type="password" id="password" name="password" placeholder="Enter your password" required>
            
            <button type="submit">Login</button>
        </form>
        <div class="footer">
            <p>Forgot your password? <a href="/reset-password">Reset it here</a></p>
        </div>
    </div>
    </body>
    </html>
    ```
    La revisión del HTML no mostró ninguna inclusión de scripts JavaScript (`<script>...</script>` o `<script src="..."></script>`) que interceptaran el envío del formulario o lo redirigieran a un endpoint diferente. Esto reforzó la idea de que el `POST` a `/login` no era el mecanismo de exfiltración funcional.

2.  **Enumeración de Permisos del Bucket S3 con AWS CLI**:
    Dada la naturaleza de S3, se sospechó que el bucket podría tener permisos de listado públicos. Se utilizó la AWS CLI con la opción `--no-sign-request` para intentar listar el contenido del bucket de forma anónima.

### Resultados
El análisis del HTML de la página de login no reveló ningún mecanismo de exfiltración de credenciales del lado del cliente. El error 405 en el POST confirma que el formulario en sí mismo no envía datos a un script de backend funcional en la ruta `/login`.

La enumeración de los permisos del bucket S3 arrojó el resultado más significativo:
```bash
aws s3 ls s3://darkinjector-phish --no-sign-request
```
Salida:
```
2025-03-17 02:46:17        132 captured-logins-093582390
2025-03-17 02:25:33       2300 index.html
```
Este resultado indica que el bucket `darkinjector-phish` permite el listado de su contenido de forma anónima. Más importante aún, se identifica un archivo llamado `captured-logins-093582390`, cuyo nombre sugiere fuertemente que contiene los datos de interés. El archivo `index.html` probablemente corresponde a la página de login.

---

## Parte 3: Explotación
### Teoría
La explotación se basa directamente en la vulnerabilidad de configuración de S3 identificada: el listado público del contenido del bucket. Esta mala configuración permite a un atacante no autenticado ver todos los archivos y directorios dentro del bucket. Si los objetos dentro del bucket también tienen permisos de lectura públicos (lo cual es común si el listado está habilitado y no se han configurado ACLs de objeto restrictivas), el atacante puede descargar directamente estos archivos. En este caso, el archivo `captured-logins-093582390` es el objetivo.

### Herramienta o script utilizado
Se pueden utilizar varias herramientas para acceder al archivo una vez identificado su nombre:
-   Un navegador web.
-   Herramientas de línea de comandos como `curl` o `wget`.
-   La AWS CLI con el comando `aws s3 cp` y la opción `--no-sign-request`.

### Comando y resultado
1.  **Confirmación del Listado del Bucket (realizada en la fase de Enumeración)**:
    ```bash
    aws s3 ls s3://darkinjector-phish --no-sign-request
    ```
    Salida relevante:
    ```
    2025-03-17 02:46:17        132 captured-logins-093582390
    ```

2.  **Descarga y Acceso al Archivo de Credenciales Capturadas**:
    Sabiendo que el archivo `captured-logins-093582390` existe y que el bucket es públicamente listable (lo que a menudo implica que los objetos también son públicamente legibles), se procede a descargar su contenido.

    Usando `curl` desde la línea de comandos:
    ```bash
    curl http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/captured-logins-093582390
    ```
    Alternativamente, usando AWS CLI para copiar el archivo localmente:
    ```bash
    aws s3 cp s3://darkinjector-phish/captured-logins-093582390 ./stolen_credentials.txt --no-sign-request
    ```
    Comando para visualizar el contenido del archivo descargado:
    ```bash
    cat stolen_credentials.txt
    ```
    El contenido de `stolen_credentials.txt` (o la salida directa de `curl`) revela las credenciales de los usuarios que fueron víctimas del sitio de phishing. Este contenido constituye la "flag" o la evidencia crítica buscada en el desafío.

---

## Parte 4: Flags o evidencias obtenidas
El archivo que contiene la "flag" o la evidencia crítica es `captured-logins-093582390`. Una vez descargado y visualizado su contenido, se obtienen las credenciales robadas.

```bash
# Contenido simulado del archivo de flag/credenciales obtenido tras ejecutar:
# cat stolen_credentials.txt 
# O la salida directa de:
# curl http://darkinjector-phish.s3-website-us-west-2.amazonaws.com/captured-logins-093582390

CTF{S3_BUCK3T_L1ST1NG_AND_PUBL1C_F1L3S_FTW} 
user1@cmail.com:Password123
victim2@cmail.com:AnotherSecurePass!
# ... (más credenciales)
```

---

## Recomendaciones de Seguridad
1.  **Restringir el Listado de Buckets S3**: Modificar la política del bucket S3 y las ACLs para evitar que `AllUsers` (público) o `AuthenticatedUsers` (cualquier usuario AWS autenticado) tengan el permiso `s3:ListBucket`. El listado solo debe permitirse a principales IAM específicos que lo requieran.
2.  **Principio de Mínimo Privilegio para Objetos S3**: Asegurar que los objetos individuales dentro del bucket no sean públicamente legibles a menos que sea absolutamente necesario (por ejemplo, si son parte de un sitio web público). Los archivos sensibles como logs de credenciales nunca deben ser públicos.
3.  **Utilizar S3 Block Public Access**: Habilitar las configuraciones de "Bloquear todo el acceso público" a nivel de cuenta y de bucket como una capa adicional de defensa, a menos que haya una razón explícita y revisada para permitir cierto tipo de acceso público.
4.  **No Almacenar Credenciales Sensibles en Texto Plano**: Las credenciales o datos sensibles capturados nunca deben almacenarse en archivos de texto plano, especialmente en ubicaciones potencialmente accesibles como un bucket S3 que sirve un sitio web. Se deben utilizar mecanismos seguros de almacenamiento y gestión de secretos.
5.  **Monitorización y Alertas**: Configurar AWS CloudTrail y CloudWatch para monitorizar el acceso a los buckets S3 y generar alertas ante actividades sospechosas o cambios de configuración inesperados.
6.  **Implementación Segura de Formularios de Login**: Si se requiere un formulario de login, este debe enviar los datos a un backend seguro (ej. API Gateway + Lambda) que procese y almacene las credenciales de forma segura, y no depender de la escritura directa a archivos en S3 por parte del cliente o de configuraciones de S3 que expongan dichos archivos.

---