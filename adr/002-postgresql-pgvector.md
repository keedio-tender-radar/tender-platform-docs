# ADR 002 — PostgreSQL + pgvector como almacén principal

- **Estado:** Aceptada
- **Fecha:** 2026-06-24

## Contexto

Necesitamos un almacén transaccional para licitaciones, análisis, scores y acciones, y un almacén
vectorial para el RAG sobre pliegos (fase 4).

## Decisión

**PostgreSQL** como base de datos principal, con la extensión **pgvector** para los embeddings.
Documentos binarios en almacenamiento de objetos (MinIO/S3), no en la BD.

## Justificación

- Un único motor para datos relacionales y vectoriales reduce piezas de infraestructura en el MVP.
- pgvector es suficiente para el volumen previsto inicial.
- SQLAlchemy + Alembic dan modelo y migraciones robustas.

## Consecuencias

- Si el volumen vectorial crece mucho, migrar a un vector store dedicado (**Qdrant**) — el
  `retriever` del análisis IA se diseña tras una interfaz para permitirlo.
- Operar pgvector requiere imagen de Postgres con la extensión (cubierto en `tender-infra`).

## Alternativas descartadas

- **Qdrant desde el día 1:** añade una pieza más sin necesidad en el MVP.
- **Documentos en BD:** infla la base y complica backups; se usa MinIO/S3.
