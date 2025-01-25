#### **Objetivo**

Explotar una vulnerabilidad de inyección SQL en un servicio basado en SQLite para obtener los nombres de usuario y contraseñas almacenados en la base de datos.

##  **Reconocimiento Inicial**

1. **Conexion al servicio:**
    
    - Nos conectamos al servicio utilizando Netcat en la dirección IP `10.10.124.79` y puerto `1337`:
      ![](Pasted%20image%2020250125102922.png)
      **Descripción del Servicio:**  
		El servicio solicita un nombre de usuario y realiza validaciones en un backend SQL.

---

#### **Identificación de Vulnerabilidades**

1. **Prueba de Comillas Simples:** Al introducir una comilla simple (`'`), el sistema devuelve un error de sintaxis:
     ![](Pasted%20image%2020250125103744.png)
    Esto confirma una posible **vulnerabilidad de inyección SQL**, ya que las entradas no están correctamente sanitizadas.
    
2. **Restricciones Detectadas:**
    - Palabras clave como `SELECT`, `UNION`, y `FROM` están bloqueadas.
      ![](Pasted%20image%2020250125103923.png)
    - Caracteres como `/*`, `--`, y `%0b` son rechazados.
      ![](Pasted%20image%2020250125104217.png)
    - El uso de mayúsculas/minúsculas en las palabras clave (`SeLeCt`, `UnIoN`) elude el filtro.
      ![](Pasted%20image%2020250125104340.png)

---

#### **Explotación**

1. **Confirmación del Backend:** Se verificó que el backend es SQLite ejecutando la siguiente consulta:
   ![](Pasted%20image%2020250125105755.png)
	**Resultado:** Devuelve la versión de SQLite.
    
2. **Enumeración de Tablas:** Para identificar las tablas disponibles en la base de datos, se utilizó la consulta:
    ![](Pasted%20image%2020250125110150.png)
    
    **Resultado:**  
    Enumeró todas las tablas y sus esquemas, identificando una tabla relevante llamada `admintable`.
3. **Extracción de Credenciales:** Con la tabla identificada, se realizaron las siguientes consultas para extraer nombres de usuario y contraseñas:
    - **Nombres de Usuario:**
    ![](Pasted%20image%2020250125110256.png)
    - **Contraseñas:**
        ![](Pasted%20image%2020250125110508.png)
    **Resultado:** Obtuve la lista completa de nombres de usuario y contraseñas.
---
#### **Conclusión**
- **Vulnerabilidad Explotada:** La máquina presenta una inyección SQL debido al manejo deficiente de las entradas del usuario.
- **Impacto:**  
    Acceso completo a credenciales almacenadas en la base de datos.
- **Solución Realizable:**
    - Implementar consultas parametrizadas para evitar inyecciones.
    - Filtrar y validar las entradas del usuario.
    - Evitar exponer mensajes de error detallados que den pistas sobre el backend.

---
