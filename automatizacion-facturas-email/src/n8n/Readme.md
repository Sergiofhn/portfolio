# n8n Workflows — Invoice Processing (Eloi)

Esta carpeta contiene **los workflows de n8n** responsables de la automatización completa del procesamiento de facturas recibidas por email.

Cada workflow tiene una **responsabilidad clara**, siguiendo una arquitectura desacoplada y auditable.

## Visión general

El sistema está dividido en **dos workflows principales**:

- **WF1 — Ingesta de facturas**
- **WF2 — Procesado y organización de facturas**

Esta separación permite:
- Aislar errores
- Escalar el sistema
- Mantener trazabilidad completa
- Simplificar mantenimiento y evolución

## WF1 — Ingesta de facturas  
**Email → Drive temporal → Google Sheets**

Archivo:  
`WF1 - Ingesta facturas (Email → Drive temporal → Sheets).json`

### Responsabilidad
Detectar nuevas facturas entrantes y **registrarlas de forma segura**, sin realizar aún procesamiento ni extracción de datos.

### Qué hace este workflow

- Escucha emails entrantes mediante Gmail Trigger
- Filtra correos relevantes
- Extrae adjuntos
- Guarda cada factura en una carpeta temporal de Google Drive
- Registra cada archivo en Google Sheets
- Etiqueta y marca el email como procesado

### Resultado
- Facturas ingeridas correctamente
- Registro inicial trazable
- Listas para ser procesadas por WF2

## WF2 — Procesado de facturas  
**Drive temporal → OCR → proveedor → mover → actualizar fila + Gmail**

Archivo:  
`WF2 — Procesado (Drive temporal → OCR → proveedor → mover → actualizar fila + Gmail).json`

### Responsabilidad
Procesar las facturas previamente ingeridas, **extraer información clave**, validarla y organizar los archivos de forma estructurada.

### Qué hace este workflow

- Lee facturas pendientes desde Drive / Sheets
- Realiza OCR del documento
- Extrae datos estructurados (proveedor, fecha, importe, número)
- Valida campos críticos
- Clasifica la factura por:
  - Año
  - Proveedor
  - Mes
- Crea carpetas dinámicamente si no existen
- Mueve el archivo a su ubicación final
- Actualiza el estado y los datos en Google Sheets
- Gestiona etiquetas en Gmail
- Notifica el resultado por Telegram

### Manejo de errores

- Si faltan datos críticos:
  - El archivo se mueve a carpeta de ERROR
  - Se actualiza el estado en Sheets
  - Se envía notificación detallada

## Estados gestionados

| Estado | Descripción |
|------|------------|
| Ingestado | Detectado y registrado (WF1) |
| Procesando | En ejecución |
| Procesado | Correcto |
| ERROR | Fallo detectado |

## Archivos adicionales

- Capturas de pantalla del diseño de los workflows
- JSON exportados directamente desde n8n para referencia y versionado

## Principios de diseño

- Un workflow = una responsabilidad
- Estados explícitos y auditables
- Manejo de errores como parte del flujo principal
- IA usada solo para extracción, no como fuente de verdad
- Trazabilidad completa: email → archivo → registro

## Nota

Estos workflows están pensados para ser:
- Reutilizables
- Extendibles a otros clientes
- Integrables con ERP u otros sistemas
