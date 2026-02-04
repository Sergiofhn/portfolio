# Workflows n8n – Lead Scraping & Enrichment

Este directorio contiene los workflows n8n que implementan el sistema completo de captación, scraping y enriquecimiento de leads B2B.

Cada workflow tiene una responsabilidad clara y desacoplada para garantizar escalabilidad, control y facilidad de mantenimiento.

---

## WF0 – Alta de campañas

**Archivo:** `workflows/WF0-alta-campanas.json`

### Objetivo
Dar de alta nuevas campañas de scraping y dejarlas listas para ser procesadas por el worker.

### Funciones principales
- Lectura de formularios o entradas manuales
- Normalización de datos de campaña
- Registro en Google Sheets
- Estado inicial controlado

---

## WF1 – Worker de campañas (Scraping)

**Archivo:** `workflows/WF1-worker-campanas.json`

### Objetivo
Procesar campañas activas y generar leads de forma automática.

### Flujo resumido
1. Selección FIFO de campaña pendiente
2. Bloqueo de campaña para evitar concurrencia
3. Generación de query optimizada
4. Scraping de Google Maps
5. Limpieza y deduplicado de URLs
6. Scraping de webs
7. Extracción y validación de emails
8. Registro del lead en Google Sheets

### Decisiones clave
- Rate limiting explícito
- Deduplicación antes de scrapear
- Separación estricta de responsabilidades
- IA solo para texto, no para scraping

---

## WF2 – Agente Icebreaker

**Archivo:** `workflows/WF2-agente-icebreaker.json`

### Objetivo
Enriquecer leads existentes y generar mensajes personalizados listos para contacto comercial.

### Funciones
- Lectura de leads pendientes
- Análisis contextual de la empresa
- Generación de icebreaker con IA
- Actualización del lead en Google Sheets

---

## Notas generales
- Todos los workflows están diseñados para ejecutarse de forma independiente
- El sistema tolera errores parciales sin romper el pipeline completo
- Preparado para escalar por campañas, clientes o mercados

