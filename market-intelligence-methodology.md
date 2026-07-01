# Metodología de inteligencia de mercado (fase 5)

Inspirada en herramientas tipo Tendios. Aprovecha el histórico público de adjudicaciones para dar
contexto competitivo a cada oportunidad y anticipar oportunidades futuras. Vive en
`tender-market-intelligence-service` y no es imprescindible para el MVP-1.

## Preguntas que responde

- ¿Quién suele **ganar** este tipo de contratos (CPV/órgano)?
- ¿A qué **baja económica media** se adjudican?
- ¿Qué **organismos** compran lo que Keedio ofrece, y con qué recurrencia?
- ¿Qué **CPV** son estratégicos por volumen/frecuencia?
- ¿Qué **contratos recurrentes** están próximos a vencer (oportunidad futura)?

## Fuentes

- Histórico de adjudicaciones de PLACSP/TED (formalizaciones).
- Datos abiertos de contratación.

## Análisis

| Módulo | Salida |
|--------|--------|
| Competidores | Adjudicatarios frecuentes por CPV/órgano; cuota estimada |
| Pricing | Baja media y tendencia de presupuesto por categoría |
| Compradores | Órganos recurrentes y su patrón de compra |
| CPV | Tendencia y estacionalidad de CPV estratégicos |
| Recurrencia | Contratos que vencen pronto → posible nueva licitación |

## Uso en el producto

- Enriquecer la ficha de una licitación con "quién suele ganar esto" y "baja esperada".
- Informe periódico de mercado para dirección.
- Señales proactivas: avisar de contratos recurrentes próximos a salir.

> Es analítica sobre datos públicos agregados; no influye en el scoring del MVP-1, pero en fases
> avanzadas puede ponderar la dimensión competitiva.

## Implementación (MVP-5)

Por la restricción de cuota de compute (5 servicios), la inteligencia de mercado se implementa
**nativa** en `tender-api` (donde viven los datos), con la ingesta de adjudicaciones **fusionada**
en `tender-ai-analysis-service` (sin servicio nuevo), siguiendo el patrón de fases previas.

- **Fuente:** notas de formalización de **TED v3** (`notice-type=can-standard`, POST sin auth). El
  conector (`TedAwardsConnector`) parsea best-effort adjudicatario/importe adjudicado/presupuesto;
  los campos de ganador en eForms varían por lote, así que pueden requerir afinado con datos reales.
- **Modelo:** tabla `awards` (adjudicatario, importe adjudicado vs presupuesto → **baja**, órgano,
  CPV, fecha). Ingesta idempotente `POST /api/market/awards` (upsert por `source+source_id`).
- **Analítica:** `GET /api/market/{competitors,pricing,buyers,cpv,overview}` +
  `GET /api/market/tender/{id}/context` (quién suele ganar la categoría + baja esperada del expediente).
- **Job/cron:** `POST /run-awards` en ai-analysis; schedule semanal `tender-weekly-awards`.
- **Dashboard:** sección "Adjudicaciones" en `/market` (competidores + baja media) y panel
  "Contexto de mercado" en la ficha.
- **Pendiente:** PLACSP formalizaciones como segunda fuente; afinar el mapeo de campos de ganador
  de TED con datos reales.
