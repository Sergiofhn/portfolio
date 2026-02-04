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
