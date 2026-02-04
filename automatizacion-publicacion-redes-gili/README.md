# Social Media Publishing Automation — Gili

## Contexto
Proyecto que requiere publicación recurrente en redes sociales sin intervención manual.

## Problema
- Publicaciones hechas a mano
- Falta de consistencia en días y horarios
- Pérdida de tiempo en tareas repetitivas

## Solución
Sistema automático que:
1. Selecciona contenido desde una fuente central
2. Genera el copy correspondiente
3. Publica en redes sociales según calendario definido
4. Registra las publicaciones realizadas

## Stack técnico
- n8n
- Google Drive
- Google Sheets
- APIs de redes sociales
- LLM para generación de copy (cuando aplica)

## Resultado
- Publicaciones consistentes sin trabajo manual
- Ahorro de tiempo operativo
- Proceso reutilizable para otros proyectos

## Decisiones clave
- Separar contenido, copy y publicación
- Automatizar calendario para evitar olvidos

## Arquitectura del sistema
El sistema se apoya en una fuente de verdad central (Google Sheets) y
una orquestación automática que separa claramente:

- Contenido (archivos multimedia)
- Programación (fechas y frecuencias)
- Publicación (red social destino)
- Registro (estado y resultado)

Esto permite reutilizar el mismo flujo para múltiples clientes y
plataformas sin duplicar lógica.

## Qué demuestra este proyecto
- Diseño de automatizaciones complejas orientadas a negocio
- Modelado de calendarios y frecuencias reales
- Orquestación robusta sin dependencia manual
- Capacidad de escalar contenido sin aumentar carga operativa

´´´
flowchart LR
    A[Scheduler / Trigger] --> B[Google Sheets - Datos]
    B --> C[Validación y reglas]
    C --> D[Selección de contenido]
    D --> E[Preparación media]
    E --> F{Red social destino}
    F -->|Instagram| G[Publicar Instagram]
    F -->|TikTok| H[Publicar TikTok]
    G --> I[Actualizar estado]
    H --> I
    I --> J[Registro y notificación]
´´´

