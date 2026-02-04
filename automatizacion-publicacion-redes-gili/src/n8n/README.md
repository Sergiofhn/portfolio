Flujo n8n – Publicación Automatizada en Redes Sociales
Descripción general

Este workflow de n8n automatiza de principio a fin la publicación de contenido en redes sociales (TikTok e Instagram) a partir de una hoja de Google Sheets.

El sistema:

Decide cuándo se puede publicar.

Prepara y convierte vídeos automáticamente.

Publica en la red social correspondiente.

Actualiza el estado en Google Sheets.

Notifica el resultado por Telegram (éxito o error).

El objetivo es eliminar tareas manuales, evitar errores de publicación y tener trazabilidad completa del contenido.

🎯 Qué problema resuelve

Antes:

Publicaciones manuales.

Errores de horario.

Falta de control sobre qué se ha publicado y qué no.

Cero feedback automático.

Ahora:

Publicaciones automáticas y controladas.

Estados claros por fila.

Logs, enlaces y notificaciones en tiempo real.

Escalable a múltiples clientes y plataformas.

🧠 Flujo lógico (alto nivel)

Trigger programado

Lectura de Google Sheets

Gate de publicación (validaciones)

Marcado “En proceso”

Preparación de datos

Descarga y conversión del vídeo

Selección de red social

Publicación

Gestión de OK / ERROR

Actualización en Sheets

Notificación por Telegram

🧩 Desglose del flujo por bloques
1️⃣ Schedule Trigger

Ejecuta el flujo de forma periódica.

Controla cuándo se revisan posibles publicaciones.

2️⃣ Datos Generales (Google Sheets)

Lee la hoja ARCHIVOS.

Filtra solo filas con:

Status = "Listo para publicar"

Campos clave usados:

Cliente

Copy

Video Link

Fecha.Hora

Fecha.Hora_key

Type

row_number

fileId

3️⃣ Gate de publicación (media)

Nodo crítico de control.

Valida:

Estado correcto.

Fecha/Hora ≤ ahora (zona horaria Europe/Madrid).

Dentro de ventana horaria permitida.

Solo los ítems válidos continúan el flujo.

4️⃣ Marcar fila como “En proceso”

Actualiza la fila en Google Sheets.

Evita dobles publicaciones.

Deja trazabilidad clara del estado intermedio.

5️⃣ Identificación y normalización de datos

Normaliza cliente, tipo y formato.

Determina:

Plataforma destino.

Perfil/cuenta de publicación.

Prepara datos técnicos para los siguientes nodos.

6️⃣ Formateo de datos

Estructura final de campos:

caption

video_url

cliente

plataforma

fecha programada

Todo listo para publicar sin lógica adicional más adelante.

7️⃣ Descarga y conversión de vídeo

Descarga el archivo desde Google Drive.

Llama a un microservicio de conversión (FFmpeg).

Optimiza el vídeo según red social.

Limpia binarios para evitar consumo innecesario de memoria.

8️⃣ Switch por tipo de contenido

Decide automáticamente el camino según Type:

video → TikTok

reel → Instagram Reels

image → Instagram (preparado, actualmente desactivado)

9️⃣ Publicación en la red social

Usa UploadPost API.

Publica o programa según configuración.

Devuelve:

URL del post

Metadatos técnicos

Estado de éxito o error

🔟 Gestión de resultados (OK / ERROR)
✅ Si todo va bien:

Estado → Publicado

Guarda Enlace_Post

Construye mensaje de confirmación

Envía notificación por Telegram

❌ Si hay error:

Estado → ERROR

Registra motivo

Envía mensaje detallado por Telegram

Mantiene trazabilidad completa

📬 Notificaciones Telegram

Cada ejecución relevante genera un mensaje con:

Cliente

Plataforma

Fila

Título del contenido

Fecha/Hora

Enlace (si existe)

Resultado final

Esto permite monitorización sin entrar en n8n.

🗂 Dependencias y servicios externos

n8n

Google Sheets

Google Drive

UploadPost API

Microservicio FFmpeg

Telegram Bot

🧱 Estados posibles en Google Sheets
Estado	Significado
Listo para publicar	Pendiente de validación
En proceso	En ejecución
Publicado	Publicación correcta
ERROR	Fallo detectado
🚀 Escalabilidad

El flujo está preparado para:

Múltiples clientes.

Múltiples cuentas por red social.

Nuevas plataformas (YouTube Shorts, LinkedIn, etc.).

Reglas horarias personalizadas.

🧠 Filosofía del sistema

No es “automatizar por automatizar”.

Es:

Control

Orden

Trazabilidad

Menos dependencia humana

Más foco en crear contenido, no en publicarlo
