## **Objetivo**

Explotar una vulnerabilidad de inyección SQL en un servicio basado en SQLite para obtener los nombres de usuario y contraseñas almacenados en la base de datos.

##  **Reconocimiento Inicial**

1. **Conexion al servicio:**
    
    - Nos conectamos al servicio utilizando Netcat en la dirección IP `10.10.124.79` y puerto `1337`:

![](light%20(3).png)      
      
      **Descripción del Servicio:**  
		El servicio solicita un nombre de usuario y realiza validaciones en un backend SQL.

---

## **Identificación de Vulnerabilidades**

1. **Prueba de Comillas Simples:** Al introducir una comilla simple (`'`), el sistema devuelve un error de sintaxis:

![](light%20(4).png)     
    
    Esto confirma una posible **vulnerabilidad de inyección SQL**, ya que las entradas no están correctamente sanitizadas.
    
2. **Restricciones Detectadas:**
    - Palabras clave como `SELECT`, `UNION`, y `FROM` están bloqueadas.

        ![](light%20(5).png)      

    - Caracteres como `/*`, `--`, y `%0b` son rechazados.

        ![](light%20(6).png)

    - El uso de mayúsculas/minúsculas en las palabras clave (`SeLeCt`, `UnIoN`) elude el filtro.

      ![](light%20(7).png)

---

## **Explotación**

1. **Confirmación del Backend:** Se verificó que el backend es SQLite ejecutando la siguiente consulta:

![](light%20(8).png)

**Resultado:** Devuelve la versión de SQLite.
    
2. **Enumeración de Tablas:** Para identificar las tablas disponibles en la base de datos, se utilizó la consulta:

![](light%20(9).png)

**Resultado:**  
    Enumeró todas las tablas y sus esquemas, identificando una tabla relevante llamada `admintable`.

3. **Extracción de Credenciales:** Con la tabla identificada, se realizaron las siguientes consultas para extraer nombres de usuario y contraseñas:
    - **Nombres de Usuario:**

    ![](light%20(1).png)
    - **Contraseñas:**

    ![](light%20(2).png)
    
    **Resultado:** Obtuve la lista completa de nombres de usuario y contraseñas.
---
## **Conclusión**
- **Vulnerabilidad Explotada:** La máquina presenta una inyección SQL debido al manejo deficiente de las entradas del usuario.
- **Impacto:**  
    Acceso completo a credenciales almacenadas en la base de datos.
- **Solución Realizable:**
    - Implementar consultas parametrizadas para evitar inyecciones.
    - Filtrar y validar las entradas del usuario.
    - Evitar exponer mensajes de error detallados que den pistas sobre el backend.

---
