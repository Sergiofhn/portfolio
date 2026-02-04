# Flujo n8n – Publicación Automatizada en Redes Sociales

## Descripción general

Este workflow de **n8n** automatiza de principio a fin la **publicación de contenido en redes sociales (TikTok e Instagram)** a partir de una hoja de **Google Sheets**.

El sistema:

- Decide cuándo se puede publicar
- Prepara y convierte vídeos automáticamente
- Publica en la red social correspondiente
- Actualiza el estado en Google Sheets
- Notifica el resultado por Telegram (éxito o error)

El objetivo es **eliminar tareas manuales**, evitar errores de publicación y garantizar **trazabilidad completa del contenido**.

---

## Qué problema resuelve

### Antes
- Publicaciones manuales
- Errores de horario
- Falta de control sobre qué se ha publicado y qué no
- Cero feedback automático

### Ahora
- Publicaciones automáticas y controladas
- Estados claros por fila
- Logs, enlaces y notificaciones en tiempo real
- Sistema escalable a múltiples clientes y plataformas

---

## Flujo lógico (alto nivel)

1. Trigger programado  
2. Lectura de Google Sheets  
3. Gate de publicación (validaciones)  
4. Marcado “En proceso”  
5. Preparación de datos  
6. Descarga y conversión del vídeo  
7. Selección de red social  
8. Publicación  
9. Gestión de OK / ERROR  
10. Actualización en Sheets  
11. Notificación por Telegram  

---

## Desglose del flujo por bloques

### 1️ Schedule Trigger
- Ejecuta el flujo de forma periódica
- Controla cuándo se revisan posibles publicaciones

### 2️ Datos Generales (Google Sheets)
- Lee la hoja **ARCHIVOS**
- Filtra solo filas con:
  - `Status = "Listo para publicar"`

Campos clave utilizados:
- Cliente  
- Copy  
- Video Link  
- Fecha.Hora  
- Fecha.Hora_key  
- Type  
- row_number  
- fileId  

### 3️ Gate de publicación (media)
Nodo crítico de control.

Valida:
- Estado correcto
- Fecha/Hora ≤ ahora (zona horaria `Europe/Madrid`)
- Dentro de la ventana horaria permitida

Solo los ítems válidos continúan el flujo.

### 4️ Marcar fila como “En proceso”
- Actualiza la fila en Google Sheets
- Evita dobles publicaciones
- Deja trazabilidad clara del estado intermedio

### 5️ Identificación y normalización de datos
- Normaliza cliente, tipo y formato
- Determina:
  - Plataforma destino
  - Perfil / cuenta de publicación
- Prepara datos técnicos para los siguientes nodos

### 6️ Formateo de datos
Estructura final de campos:
- `caption`
- `video_url`
- `cliente`
- `plataforma`
- `fecha programada`

Datos listos para publicar sin lógica adicional aguas abajo.

### 7️ Descarga y conversión de vídeo
- Descarga el archivo desde Google Drive
- Llama a un microservicio de conversión (FFmpeg)
- Optimiza el vídeo según la red social
- Limpia binarios para evitar consumo innecesario de memoria

### 8️ Switch por tipo de contenido
Decide automáticamente el camino según `Type`:

- `video` → TikTok  
- `reel` → Instagram Reels  
- `image` → Instagram (preparado, actualmente desactivado)

### 9️ Publicación en la red social
- Uso de **UploadPost API**
- Publicación o programación según configuración
- Devuelve:
  - URL del post
  - Metadatos técnicos
  - Estado de éxito o error

### 10 Gestión de resultados (OK / ERROR)

#### ✅ Si todo va bien
- Estado → `Publicado`
- Guarda `Enlace_Post`
- Construye mensaje de confirmación
- Envía notificación por Telegram

#### ❌ Si hay error
- Estado → `ERROR`
- Registra el motivo del fallo
- Envía mensaje detallado por Telegram
- Mantiene trazabilidad completa del incidente

## Notificaciones Telegram

Cada ejecución relevante genera un mensaje con:
- Cliente
- Plataforma
- Fila afectada
- Título del contenido
- Fecha/Hora
- Enlace (si existe)
- Resultado final

Esto permite **monitorización sin necesidad de acceder a n8n**.

## Dependencias y servicios externos

- n8n
- Google Sheets
- Google Drive
- UploadPost API
- Microservicio FFmpeg
- Telegram Bot

## Estados posibles en Google Sheets

| Estado              | Significado                     |
|--------------------|---------------------------------|
| Listo para publicar | Pendiente de validación          |
| En proceso          | En ejecución                     |
| Publicado           | Publicación correcta             |
| ERROR               | Fallo detectado                  |

## Escalabilidad

El flujo está preparado para:
- Múltiples clientes
- Múltiples cuentas por red social
- Nuevas plataformas (YouTube Shorts, LinkedIn, etc.)
- Reglas horarias personalizadas

## Filosofía del sistema

No es “automatizar por automatizar”.

Es:
- Control
- Orden
- Trazabilidad
- Menos dependencia humana
- Más foco en crear contenido, no en publicarlo
