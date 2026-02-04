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

```mermaid
flowchart TD
  A["Gmail<br/>Facturas entrantes"] --> B["WF1 - Ingesta facturas"]

  subgraph WF1["WF1 - Ingesta: Email -> Drive temporal -> Sheets"]
    B --> C["Validacion email y adjuntos"]
    C --> D["Guardar archivo en Drive temporal"]
    D --> E["Registrar fila en Google Sheets"]
    E --> F["Etiquetar y marcar email en Gmail"]
  end

  E --> G["WF2 - Procesado y clasificacion"]

  subgraph WF2["WF2 - Procesado: OCR -> Clasificacion -> Archivo"]
    G --> H["OCR / Extraccion de datos"]
    H --> I["Validacion de datos criticos"]

    I -->|OK| J["Clasificacion por Ano / Proveedor / Mes"]
    I -->|ERROR| K["Carpeta ERROR + registro del fallo"]

    J --> L["Mover archivo a Drive definitivo"]
    L --> M["Actualizar estado en Google Sheets"]
  end

  M --> N["Notificacion por Telegram"]
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
