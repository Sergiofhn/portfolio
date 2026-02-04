# Invoice Processing Automation (Email → System)

## Contexto
Empresa de servicios con alto volumen de facturas recibidas por email.

## Problema
- Facturas recibidas en múltiples formatos (PDF, adjuntos, emails)
- Procesamiento manual y repetitivo
- Errores humanos y falta de control
- Dependencia de una persona clave

## Impacto de negocio
- Ahorro significativo de tiempo administrativo
- Reducción de errores humanos en el registro
- Mayor control y trazabilidad de facturas
- Proceso auditable y repetible

## Solución
Sistema automático que:
1. Detecta nuevas facturas entrantes por email
2. Extrae información clave (proveedor, fecha, importe, concepto)
3. Valida los datos
4. Registra la información en el sistema
5. Notifica incidencias o facturas incompletas

´´´
flowchart TD
    A[Gmail<br/>Facturas entrantes] --> B[WF1 · Ingesta facturas]

    subgraph WF1 [WF1 · Ingesta (Email → Drive → Sheets)]
        B --> C[Validación email y adjuntos]
        C --> D[Guardar en Drive temporal]
        D --> E[Registrar fila en Google Sheets]
        E --> F[Etiquetar y marcar email]
    end

    E --> G[WF2 · Procesado y clasificación]

    subgraph WF2 [WF2 · Procesado (OCR → Clasificación → Archivo)]
        G --> H[OCR / Extracción de datos]
        H --> I[Validación de datos críticos]
        I -->|OK| J[Clasificación por Año / Proveedor / Mes]
        I -->|ERROR| K[Carpeta ERROR + notificación]
        J --> L[Mover archivo a Drive definitivo]
        L --> M[Actualizar estado en Sheets]
    end

    M --> N[Notificación Telegram]
    K --> N
´´´

## Stack técnico
- n8n
- Gmail
- Google Drive
- Google Sheets / ERP
- LLM para extracción estructurada (cuando aplica)

## Resultado
- Reducción drástica del trabajo manual
- Menos errores en el registro de facturas
- Mayor trazabilidad y control del proceso

## Decisiones clave
- Separar detección, extracción y validación para evitar fallos en cascada
- Mantener el sistema auditable y fácil de mantener

## Arquitectura y decisiones técnicas
- Pipeline modular: detección → extracción → validación → registro
- Manejo de errores independiente por etapa
- IA usada solo para extracción estructurada, no como fuente de verdad

