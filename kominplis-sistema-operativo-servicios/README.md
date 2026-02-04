# Kominplis — Operational System Automation

## Contexto
Empresa de servicios de limpieza con alta recurrencia, múltiples clientes y coordinación diaria de operaciones.

## Problema
- Servicios gestionados manualmente
- Información repartida entre calendarios, hojas y mensajes
- Falta de control sobre lo ejecutado vs lo facturado
- Alto riesgo de errores y dependencia humana

## Impacto de negocio
- Reducción de errores operativos diarios
- Visibilidad completa de servicios ejecutados vs facturados
- Menos dependencia de personas clave
- Base sólida para escalar sin aumentar estructura

## Solución
Diseño e implantación de un sistema operativo automático que:
1. Centraliza los servicios solicitados
2. Sincroniza calendarios operativos
3. Registra cada servicio ejecutado
4. Prepara la información para facturación
5. Mantiene trazabilidad completa del proceso

```md
## Entrada automática vía iCal (cliente)

Además de la entrada manual mediante formularios, el sistema integra
calendarios iCal proporcionados por los clientes.

Esto permite:
- Sincronizar automáticamente los servicios programados por el cliente
- Eliminar completamente la intervención manual
- Mantener el sistema alineado en tiempo real con el calendario externo

Desde el punto de vista operativo, el iCal actúa como una fuente de verdad
externa, asegurando que cualquier cambio realizado por el cliente se refleja
automáticamente en el sistema interno.

Desde el punto de vista técnico:
- El iCal se consume como feed de eventos
- Los eventos se normalizan y validan antes de entrar en el sistema
- Se evita la duplicidad entre entradas manuales y automáticas
```

## Diagrama de arquitectura
```mermaid
flowchart TD
    A[Cliente / Equipo operativo] --> B[Google Form<br/>Entrada manual de solicitudes]
    A --> B2[iCal Cliente<br/>Sincronización automática]

    B --> C[Google Sheets<br/>Registro y estado del servicio]
    B2 --> C

    C --> D[Apps Script<br/>Validación · Normalización · Reglas]
    D --> E[n8n<br/>Orquestación · Decisiones · Logs]
    E --> F[Google Calendar<br/>Planificación y ejecución]
    F --> G[Google Sheets<br/>Resumen / registro final trazable]
    G --> H[Facturación<br/>Preparación de líneas por cliente / piso]
    H --> I[Notificaciones<br/>Incidencias o cambios]
```
## Stack técnico
- Google Forms
- Google Sheets
- Google Calendar
- Google Apps Script
- n8n
- Automatizaciones de validación y sincronización

## Resultado
- Control total de la operativa diaria
- Eliminación de errores de coordinación
- Base sólida para facturación automática
- Sistema escalable sin añadir personal

## Decisiones clave
- Separar entrada de datos, ejecución y facturación
- Priorizar fiabilidad y trazabilidad sobre complejidad

## Arquitectura y decisiones técnicas
- Sistema basado en eventos (formularios y calendario)
- Separación clara entre entrada de datos, ejecución y facturación
- Validaciones automáticas para evitar inconsistencias
- Prioridad absoluta a fiabilidad frente a complejidad

