# Roadmap por fases

## MVP-1 — Radar diario (foco actual)

**Objetivo:** cada mañana, Keedio recibe en Telegram un resumen de licitaciones relevantes con
recomendación Go/No-Go.

Repos: `tender-platform-docs`, `tender-shared-contracts`, `tender-infra`, `tender-api`,
`tender-ingestion-service`, `tender-ai-analysis-service`, `tender-telegram-bot`.

Entregables:
- Ingesta PLACSP + TED, normalización, dedupe, filtro CPV/keywords.
- API + PostgreSQL con modelo núcleo y migraciones.
- Análisis IA del anuncio + score Go/No-Go (scoring embebido).
- Radar diario por Telegram con botones de acción.
- Docker Compose para levantar todo en local.

## MVP-2 — Análisis documental completo

Repo añadido: `tender-document-service`. Descarga PCAP/PPT, extrae texto, chunking; el análisis IA
pasa a leer el **pliego completo**, no solo el anuncio.

## MVP-3 — Dashboard interno

Repo añadido: `tender-web-dashboard`. Radar, priorización, ficha de expediente, estados,
asignación de responsables.

## MVP-4 — Chat documental (RAG)

Se amplían `tender-document-service`, `tender-ai-analysis-service`, `tender-api`,
`tender-web-dashboard`. Preguntas sobre una licitación con **citas del pliego**. Entra
`tender-observability`.

## MVP-5 — Inteligencia de mercado

Repo añadido: `tender-market-intelligence-service`. Competidores, histórico de adjudicaciones,
bajas medias, organismos compradores, CPV estratégicos, contratos recurrentes.

## MVP-6 — Generador de oferta

Repo añadido: `tender-offer-generator`. Informe Go/No-Go, matriz de cumplimiento, checklist
administrativo, índice de memoria técnica, borrador de propuesta (DOCX/XLSX/PDF).

## Milestones (GitHub)

```
MVP-1 Radar diario
MVP-2 Análisis documental
MVP-3 Dashboard
MVP-4 Chat con pliegos
MVP-5 Inteligencia de mercado
MVP-6 Generador de oferta
```
