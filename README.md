# tender-platform-docs

> 📚 Centro de verdad funcional, estratégica y técnica de **Keedio Tender Radar**.

Keedio Tender Radar es la plataforma interna de Keedio para **inteligencia de contratación
pública**: lee licitaciones (PLACSP, TED), las analiza con IA, las puntúa según el encaje con
Keedio (Go/No-Go) y entrega un **radar diario por Telegram**. En fases posteriores añade chat
documental sobre los pliegos (RAG), dashboard web, inteligencia de mercado y generación de oferta.

## Índice

| Documento | Contenido |
|-----------|-----------|
| [product-vision.md](product-vision.md) | Visión, problema, propuesta de valor, alcance |
| [mvp-roadmap.md](mvp-roadmap.md) | Fases MVP-1 … MVP-6 y entregables |
| [functional-requirements.md](functional-requirements.md) | Requisitos funcionales por dominio |
| [technical-architecture.md](technical-architecture.md) | Arquitectura multi-repo, servicios, datos, flujos |
| [scoring-model.md](scoring-model.md) | Modelo de puntuación Go/No-Go (100 pts) |
| [go-no-go-methodology.md](go-no-go-methodology.md) | Metodología de decisión GO/NO-GO/REVISAR/PARTNER |
| [telegram-notification-format.md](telegram-notification-format.md) | Formato del radar diario y alertas |
| [keedio-company-profile.md](keedio-company-profile.md) | Perfil de Keedio que alimenta el scoring |
| [market-intelligence-methodology.md](market-intelligence-methodology.md) | Inteligencia de mercado (fase 5) |
| [security-requirements.md](security-requirements.md) | Requisitos de seguridad y gestión de secretos |
| [adr/](adr/) | Architecture Decision Records |
| [diagrams/](diagrams/) | Diagramas (Mermaid) |

## Repositorios de la organización

Núcleo del MVP (fase 1): `tender-platform-docs`, `tender-shared-contracts`, `tender-infra`,
`tender-api`, `tender-ingestion-service`, `tender-ai-analysis-service`, `tender-telegram-bot`.

Fases posteriores: `tender-document-service`, `tender-web-dashboard`, `tender-scoring-service`,
`tender-market-intelligence-service`, `tender-offer-generator`, `tender-observability`.

> Convención de repos: `tender-<dominio>-<tipo>`. Los contratos compartidos viven en
> [`tender-shared-contracts`](https://github.com/keedio-tender-radar/tender-shared-contracts).
