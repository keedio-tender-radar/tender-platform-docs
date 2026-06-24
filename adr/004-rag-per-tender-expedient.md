# ADR 004 — RAG por expediente de licitación

- **Estado:** Aceptada
- **Fecha:** 2026-06-24

## Contexto

El chat documental (fase 4) debe responder preguntas sobre los pliegos con citas. Hay que decidir
el alcance del contexto recuperado.

## Decisión

El RAG se hace **por expediente**: la recuperación de fragmentos se **filtra siempre por el
`tender_id`** de la licitación consultada. Cada respuesta incluye citas a los fragmentos de **ese**
expediente.

## Justificación

- Evita "contaminación" entre licitaciones (respuestas que mezclan pliegos distintos).
- Las citas son verificables contra el documento concreto.
- El filtrado por `tender_id` mantiene el espacio de búsqueda pequeño y la latencia baja.

## Consecuencias

- Los `tender_chunks` guardan `tender_id` y `document_id`; el `retriever` filtra por ellos.
- No hay búsqueda "global" entre expedientes en el chat (eso es trabajo de inteligencia de
  mercado, fase 5, con otra metodología).

## Alternativas descartadas

- **RAG global** sobre todos los pliegos: mayor riesgo de respuestas mezcladas y citas ambiguas.
