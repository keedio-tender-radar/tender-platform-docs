# Requisitos funcionales

Notación: `FR-<dominio>-<n>`. Prioridad MoSCoW (Must/Should/Could). Fase indica MVP donde entra.

## Ingesta (`tender-ingestion-service`)

| ID | Requisito | Prioridad | Fase |
|----|-----------|-----------|------|
| FR-ING-1 | Leer licitaciones nuevas de PLACSP | Must | 1 |
| FR-ING-2 | Leer licitaciones nuevas de TED | Must | 1 |
| FR-ING-3 | Normalizar campos comunes (CPV, importe, plazo, órgano, objeto) | Must | 1 |
| FR-ING-4 | Deduplicar contra lo ya ingerido | Must | 1 |
| FR-ING-5 | Filtro rápido por CPV y keywords del perfil Keedio | Must | 1 |
| FR-ING-6 | Detectar cambios en licitaciones existentes | Should | 1 |
| FR-ING-7 | Job diario programado | Must | 1 |
| FR-ING-8 | Conectores autonómicos | Could | 2+ |

## Backend (`tender-api`)

| ID | Requisito | Prioridad | Fase |
|----|-----------|-----------|------|
| FR-API-1 | CRUD de licitaciones y estados | Must | 1 |
| FR-API-2 | Persistir análisis IA y scores | Must | 1 |
| FR-API-3 | Registrar acciones de usuario | Must | 1 |
| FR-API-4 | Endpoints para el bot (resumen, top, urgentes, ficha) | Must | 1 |
| FR-API-5 | Migraciones (Alembic) | Must | 1 |
| FR-API-6 | Autenticación de servicios y usuarios | Must | 1 |
| FR-API-7 | Endpoint de chat documental (RAG) | Should | 4 |

## Análisis IA (`tender-ai-analysis-service`)

| ID | Requisito | Prioridad | Fase |
|----|-----------|-----------|------|
| FR-AI-1 | Resumen ejecutivo del anuncio | Must | 1 |
| FR-AI-2 | Extracción de requisitos y criterios de adjudicación | Must | 1 |
| FR-AI-3 | Detección de solvencia técnica y económica exigida | Must | 1 |
| FR-AI-4 | Detección de riesgos e incompatibilidades | Should | 1 |
| FR-AI-5 | Recomendación preliminar explicada | Must | 1 |
| FR-AI-6 | Análisis sobre pliego completo (no solo anuncio) | Should | 2 |
| FR-AI-7 | Respuestas con citas del pliego (RAG) | Should | 4 |

## Scoring (`tender-scoring-service`)

| ID | Requisito | Prioridad | Fase |
|----|-----------|-----------|------|
| FR-SCO-1 | Score 0–100 por las 9 dimensiones | Must | 1 |
| FR-SCO-2 | Reglas duras (exclusión, partner, revisión urgente) | Must | 1 |
| FR-SCO-3 | Recomendación GO/NO-GO/REVISAR/PARTNER | Must | 1 |
| FR-SCO-4 | Desglose y factores explicativos | Must | 1 |
| FR-SCO-5 | Reglas y pesos como datos versionables (YAML) | Should | 1 |

## Telegram (`tender-telegram-bot`)

| ID | Requisito | Prioridad | Fase |
|----|-----------|-----------|------|
| FR-TG-1 | Radar diario al canal | Must | 1 |
| FR-TG-2 | Alertas urgentes por plazo crítico | Must | 1 |
| FR-TG-3 | Botones inline de acción | Must | 1 |
| FR-TG-4 | Comandos (/resumen, /top, /urgentes, /licitacion) | Must | 1 |
| FR-TG-5 | Registrar la acción en `tender-api` | Must | 1 |

## Documentos (`tender-document-service`)

| ID | Requisito | Prioridad | Fase |
|----|-----------|-----------|------|
| FR-DOC-1 | Descargar PCAP/PPT/anexos | Must | 2 |
| FR-DOC-2 | Extraer texto (PDF/DOCX/XLSX/HTML) | Must | 2 |
| FR-DOC-3 | Chunking + detección de secciones | Must | 2 |
| FR-DOC-4 | Guardar documentos en MinIO/S3 y texto en BD | Must | 2 |
| FR-DOC-5 | Generar embeddings para RAG | Should | 4 |

## No funcionales (resumen)

- **Coste IA controlado:** análisis profundo solo tras superar el cribado.
- **Idempotencia:** no duplicar licitaciones ni notificaciones.
- **Trazabilidad:** todo score/recomendación/acción auditable.
- **Seguridad:** secretos fuera del código (ver [security-requirements.md](security-requirements.md)).
- **Observabilidad:** métricas de ingesta, coste IA, envíos y acciones (fase 4).
