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

## Diagrama de arquitectura

```text
Cliente / Equipo operativo
  ↓
Google Form (entrada de solicitudes)
  ↓
Google Sheets (registro y estado del servicio)
  ↓
Apps Script (validación + normalización + reglas)
  ↓
n8n (orquestación: decisiones, rutas, logs)
  ↓
Google Calendar (planificación y ejecución)
  ↓
Google Sheets (Resumen / registro final trazable)
  ↓
Facturación (preparación de líneas por cliente / piso)
  ↓
Notificaciones (si hay incidencias o cambios)
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

