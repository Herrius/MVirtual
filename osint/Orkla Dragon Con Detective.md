## 1. INFORMACIÓN GENERAL DEL CASO
- **Título:** Orkla: Dragon Con Detective  
- **Tipo de objetivo:** Otro (Desafíos temáticos con componentes de ubicación, imágenes y transcripción)  
- **Fecha de inicio:** 09/04/2025  
- **Fecha de finalización:** 09/04/2025  
- **Propósito del ejercicio:** Entrenamiento práctico en OSINT aplicado a entornos lúdicos/eventos reales  
- **Herramientas utilizadas:** Google Dorking, Google Maps, What3Words, Whisper, buscadores, análisis de imágenes y metadatos.

---

## 2. RESUMEN EJECUTIVO

Este informe documenta la resolución de cuatro ejercicios OSINT vinculados a ubicaciones, audio transcrito, reconocimiento visual y análisis de eventos. Se logró identificar con éxito:

- Una máquina Coca-Cola Freestyle dentro de un hotel en Atlanta.  
- Una ubicación exacta derivada de una frase en audio mediante what3words.  
- La localización de una estatua de bronce en Baker Street relacionada con Dragon Con.  
- El participante de una maratón mediante análisis de una imagen pixelada y consulta de registros históricos.  

Todas las resoluciones se realizaron aplicando herramientas abiertas y lógica OSINT sin intervención de técnicas invasivas.

---

## 3. METODOLOGÍA

### Desafío 1: Ubicación de Coca-Cola Freestyle

El objetivo consistía en identificar la ubicación más cercana de una máquina Coca-Cola Freestyle.

- Se utilizó Google para encontrar el sitio oficial.  
- Se accedió a la función “Find Location” del sitio web, ingresando **Atlanta** como ciudad.  
- Se validó que la ubicación más cercana era el propio hotel sede del evento, confirmado mediante Google Maps.

![Paso inicial](Pasted%20image%2020250409164015.png)  
![Resultado en buscador](Pasted%20image%2020250409164131.png)  
![Formulario de búsqueda](Pasted%20image%2020250409164238.png)  
![Resultado final](Pasted%20image%2020250409164435.png)

---

### Desafío 2: Frase codificada en audio (outgoing.bongo.skip)

El ejercicio presentaba una pista sonora.

- Se utilizó *Whisper*, herramienta de IA para transcripción de audio, obteniendo la frase: **outgoing bongo skip**.  
- Al buscar esta frase en Google, se identificó el sitio **what3words**, sistema que codifica ubicaciones mediante combinaciones de palabras.  
- El enlace conducía a la ubicación exacta solicitada.

![Premisa del reto](Pasted%20image%2020250409164647.png)  
![Resultado Whisper](Pasted%20image%2020250409164812.png)  
![Resultados de búsqueda](Pasted%20image%2020250409165829.png)  
![Ubicación exacta en What3Words](Pasted%20image%2020250409165928.png)

---

### Desafío 3: Identificación de estatua

La pista incluía una imagen con la descripción “bronze statue” y la mención a Baker St.

- Se investigó la relación entre la calle Baker St., el evento *Dragon Con* y estatuas notables.  
- Se aplicaron técnicas de Google Dorking para encontrar fotos específicas del lugar.  
- La imagen obtenida reveló la estatua y el contexto del desafío.

![Imagen del reto](Pasted%20image%2020250409171405.png)  
![Aplicación de Dorking](Pasted%20image%2020250409171532.png)  
![Resultado revelador](Pasted%20image%2020250409171553.png)

---

### Desafío 4: Participante en maratón (imagen pixelada)

El reto consistía en identificar a un corredor en una imagen borrosa.

- Se buscaron versiones de mayor calidad de la imagen.  
- Se encontró una imagen nítida que permitió visualizar el número de dorsal.  
- Utilizando el sitio *Boston Marathon Results Archive*, se consultaron los datos por año, país, edad y número.  
- Se identificó correctamente al participante.

![Caso planteado](Pasted%20image%2020250409174029.png)  
![Imagen mejorada](Pasted%20image%2020250409174149.png)  
![Imagen nítida](Pasted%20image%2020250409174215.png)  
![Consulta de resultados](Pasted%20image%2020250409174312.png)  
![Datos del dorsal](Pasted%20image%2020250409174359.png)  
![Resultado encontrado](Pasted%20image%2020250409174451.png)

---

## 4. HALLAZGOS

- Se resolvieron exitosamente los cuatro desafíos utilizando herramientas y técnicas OSINT.  
- Se identificaron ubicaciones exactas, datos visuales y registros de personas sin necesidad de acceso privilegiado.  
- El uso de transcripción de audio, combinada con análisis de imágenes y bases públicas, permitió confirmar identidades y lugares.  
- No se accedió a datos sensibles, pero se demuestra cómo puede reconstruirse información significativa desde fuentes abiertas.

---

## 5. ANÁLISIS Y CONCLUSIÓN

Este ejercicio demuestra que las técnicas OSINT pueden aplicarse con alta efectividad para reconstruir información real a partir de datos mínimos.

- La huella digital de eventos, fotos y registros públicos permite rastrear identidades y ubicaciones.  
- No se encontraron datos sensibles, pero sí evidencia de cómo actores maliciosos podrían armar perfiles completos a partir de fuentes abiertas.  
- El reto reforzó habilidades como dorking, análisis de imágenes, validación cruzada, uso de inteligencia artificial y análisis geoespacial.

---

## 6. RECOMENDACIONES

- Minimizar la exposición de datos personales en imágenes públicas de eventos.  
- Considerar el uso de metadatos anónimos en fotos.  
- Limitar el acceso público a bases de datos históricas si no son necesarias.  
- Promover la concientización sobre el impacto de la huella digital en redes sociales y eventos masivos.

---

## 7. ANEXOS (CAPTURAS Y EVIDENCIAS)

Las imágenes utilizadas durante el informe se encuentran incrustadas en las secciones respectivas. Se recomienda guardar localmente estas imágenes junto al archivo markdown para mantener la estructura visual.

---

