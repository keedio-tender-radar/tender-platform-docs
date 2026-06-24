# ADR 001 — Arquitectura multi-repo

- **Estado:** Aceptada
- **Fecha:** 2026-06-24

## Contexto

Keedio Tender Radar agrupa dominios bien diferenciados (ingesta, documentos, IA, scoring,
notificación, dashboard, infra). Hay que decidir entre monorepo y multi-repo.

## Decisión

**Multi-repo** bajo la organización `keedio-tender-radar`, un repositorio por servicio/dominio,
con un repo de **contratos compartidos** (`tender-shared-contracts`) y otro de **documentación**
(`tender-platform-docs`).

## Justificación

- Ciclos de vida y despliegues independientes por servicio.
- Permisos y `CODEOWNERS` por equipo/área.
- CI más simple y rápido por repo.
- Los contratos compartidos evitan acoplamiento de formatos.

## Consecuencias

- Coste de coordinación entre repos (versionado de contratos).
- Necesidad de convención de nombres (`tender-<dominio>-<tipo>`) y de un índice central (este repo).
- El contrato (`tender-shared-contracts`) debe versionarse con cuidado (semver) para no romper
  consumidores.

## Alternativas descartadas

- **Monorepo:** más simple al inicio, pero acopla despliegues y permisos; se descarta por el
  tamaño objetivo de la plataforma.
