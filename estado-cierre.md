# Estado de cierre — Keedio Tender Radar

Documento de handoff. Resume el estado del sistema al cerrar el desarrollo, qué queda por
**activar** (operativo) y cómo operarlo. Para el detalle operativo ver `operations-runbook.md`.

## 1. Qué hace la plataforma

Ciclo completo de inteligencia de contratación pública:

**Detección** → ingesta (PLACSP, TED, portales ATOM) → OCR de pliegos → análisis IA → scoring
Go/No-Go → radar diario (web + Telegram).

**Oferta** → comprensión estructurada del pliego (criterios/solvencia/plazos) + inteligencia del
órgano (qué licita, a quién adjudica) + **memoria técnica sección por sección** + resumen,
matriz, documentos exigidos, carta, estrategia de puja + 2 modelos Excel + Word/PDF + segunda
pasada de crítica/mejora.

**Mercado** → competidores, concentración (HHI), baja media, perfil de comprador, informe PDF,
**reconciliación automática con adjudicaciones oficiales** (rellena ganada/perdida).

**Gestión** → expediente con carpetas y ficheros (InsForge Storage), auto-relleno, panel de
progreso, win-rate del pipeline.

**Telegram** → centro de mando móvil: radar, ficha accionable, «Preparar oferta», marcar
resultado, y avisos proactivos (cierre próximo, oferta lista, resultado oficial).

## 2. Servicios desplegados (Fly + InsForge)

| Servicio | Rol |
|---|---|
| `tender-api` | API núcleo (licitaciones, scoring, expediente, mercado, admin) |
| `tender-ai-analysis-service` | Análisis IA, scoring, generación de borradores, ingesta |
| `tender-document-service` | Extracción/OCR de pliegos |
| `tender-visual-rag` | RAG documental (pregúntale al pliego) |
| `tender-telegram-bot` | Bot y notificaciones |
| Dashboard (InsForge Site) | Web de operación |

## 3. Qué queda por ACTIVAR (operativo, no código)

1. **🔴 Proteger la API de lectura** — hoy está abierta. Define `READ_API_TOKEN` en el
   `.compute.env` de tender-api y redespliega; el dashboard lo obtiene al autenticar y los
   servicios internos usan `RUN_TOKEN`. Pasos en `operations-runbook.md` §6.b. **Crítico**: la
   API contiene borradores de oferta y expedientes confidenciales.
2. **Enlace en avisos Telegram** — define `DASHBOARD_URL` en el `.compute.env` de tender-api para
   que los avisos («oferta lista», etc.) incluyan el enlace a la ficha.
3. **Identidad de Keedio en la oferta** — ajusta `KEEDIO_PROFILE` (env de ai-analysis) con las
   certificaciones y proyectos de referencia reales para que la «evidencia» sea concreta.

## 4. Crons activos (InsForge schedules)

Ingesta diaria, scoring, re-análisis, radar/alertas/recordatorios a Telegram, snapshot; semanales:
adjudicaciones, informe de mercado, **backup de BD** (`tender-db-backup`, 03:30), **retención**
(`tender-data-retention`, lun 04:00), **reconciliación** (`tender-reconcile-outcomes`, lun 06:00),
**resultados** (`tender-weekly-results`, lun 08:30).

## 5. Cómo operarlo (flujo típico)

1. Llega el **radar diario** (Telegram/web). Marcas «Interesa» en las oportunidades GO.
2. En la ficha, **«Preparar oferta»** genera los borradores (≈5 min; llega aviso «oferta lista»).
3. Revisas y ajustas **memoria/resumen/matriz**; descargas el **expediente (.zip)** o Word/PDF.
4. Al presentar, marcas **«Presentada»**; cuando salga la adjudicación oficial, la reconciliación
   rellena **ganada/perdida** (o lo marcas a mano) → alimenta el **win-rate**.

## 6. Afinado con uso real (mejora, no pendiente de código)

- **Scoring/umbrales**: se recalibran con el histórico de ganadas/perdidas (Perfil → recalibrar).
- **Perfil de órgano y mercado**: mejoran al acumular más adjudicaciones.
- **OCR**: se valida con pliegos escaneados concretos.
- **Profundidad de oferta**: gpt-4o-mini rinde ~1–1,5k chars/apartado; para nivel superior,
  demandar longitud mínima por sección o un modelo más potente.

---

Sistema entregado, verificado en producción y automatizado. Desarrollo cerrado; lo pendiente es
operativo (activaciones de arriba) o de rodaje con datos reales.
