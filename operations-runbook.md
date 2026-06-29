# Runbook de operaciones — Keedio Tender Radar (v1.0.0)

Guía operativa de la plataforma desplegada. Para arquitectura ver `technical-architecture.md`.

## 1. Topología desplegada

**Backend:** 14 servicios en Fly.io vía InsForge compute (scale-to-zero), región `cdg`.
Servicios clave y URLs (`<svc>-67495beb-4f2f-47fc-9c8d-ec14a563525c.fly.dev`):

| Servicio | Rol |
|---|---|
| `tender-api` | API núcleo (licitaciones, scoring store, perfil, documentos, runs) |
| `tender-ai-analysis-service` | Análisis + scoring (LLM OpenRouter free + fallback rule-based); aloja también la ingesta |
| `tender-document-service` | Extracción de pliegos (PDF/DOCX/XLSX/HTML) |
| `tender-telegram-bot` | Radar diario + alertas + comandos (webhook) |
| `tender-visual-rag` | "Pregúntale al pliego" (fallback QA extractivo) |

**Frontend:** dashboard Next.js estático en InsForge Sites → `https://vz4wf92x.insforge.site`.
**BD:** PostgreSQL InsForge OSS self-hosted (`vz4wf92x.eu-central.database.insforge.app`).

## 2. Cron (9 schedules, UTC)

| Hora | Job | Endpoint |
|---|---|---|
| 06:00 | Ingesta PLACSP+TED | ai-analysis `/run-ingestion` |
| 06:30 | Scoring | ai-analysis `/run` |
| 06:45 | Alertas GO | bot `/send-alerts` |
| 07:00 | Re-análisis con pliego | api `/api/tenders/reanalyze-relevant` |
| 07:30 | Radar diario | bot `/send-digest` |
| 07:32 | Foto diaria | api `/api/tenders/daily-snapshot` |
| 08:00 | Recordatorios | bot `/send-reminders` |
| `*/5 6-21` | Keep-warm | api `/health` |
| `0 5 1 * *` | Recalibración de umbrales | api `/api/profile/recalibrate` |

Estado en vivo: **Dashboard → Estado** (servicios, LLM, últimas ejecuciones). Los fallos de job
alertan al chat de Telegram configurado.

## 3. Despliegue

```bash
# Backend (desde la raíz del repo; .compute.env tiene los secretos, gitignored):
npx --yes @insforge/cli@latest compute deploy ./<servicio> --name <servicio> \
  --port 8000 --region cdg --memory 512 --env-file .compute.env
# (tender-api/ai-analysis: ejecutar antes `bash scripts/vendor-contracts.sh`)

# Dashboard (Sites):
npx --yes @insforge/cli@latest deployments deploy ./tender-web-dashboard \
  --env '{"NEXT_PUBLIC_API_URL":"https://tender-api-...fly.dev"}'
```

> **Nota CLI:** si `npx @insforge/cli` falla por módulo no encontrado, usar `npx --yes @insforge/cli@latest`.
> **Nota verificación:** algunos entornos devuelven `000` al hacer `curl -w %{http_code}` contra
> `*.fly.dev` (renegociación TLS de schannel); el **cuerpo sí llega** → verificar leyendo el body.

## 4. Backups de la BD

```bash
bash tender-infra/scripts/backup-db.sh         # → tender-infra/backups/tender-db-<ts>.sql
npx --yes @insforge/cli@latest db import <fichero>.sql   # restaurar (¡SOBRESCRIBE!)
```
La carpeta `backups/` está gitignored. Programar el script en una máquina del operador y
sincronizar la carpeta a almacenamiento externo. La BD es pequeña (~13 MB).

## 5. Migraciones de esquema

- **Runtime actual:** `create_all` + auto-ALTER idempotente (`database._COLUMN_MIGRATIONS`) al
  arrancar `tender-api`. Añadir columna nueva = añadirla al modelo **y** a `_COLUMN_MIGRATIONS`.
- **Alembic** está listo con baseline completo (`alembic/versions/3f05d2a4945b...`). Para adoptarlo
  como fuente en prod (1 sola vez, sin recrear nada): `alembic stamp 3f05d2a4945b`; después
  `alembic revision --autogenerate` + `alembic upgrade head` para cambios futuros.

## 6. Secretos y acceso

- `.compute.env` (raíz, **gitignored**) contiene todos los secretos del backend.
- **Dashboard:** gate activo con `DASHBOARD_PASSWORD` (en `.compute.env`). El dashboard pide la
  contraseña al entrar.
- **CI (pytest):** requiere el secreto de Actions `CONTRACTS_PAT` (PAT read-only sobre
  `tender-shared-contracts`) **o** hacer público ese repo (solo contratos, sin secretos). Sin ello,
  CI ejecuta ruff pero **omite pytest**.
- **Rotación:** rotar `OPENROUTER_API_KEY`, `TELEGRAM_BOT_TOKEN`, `RUN_TOKEN` y credenciales de BD
  periódicamente (`compute deploy --env-file` para propagar).

## 7. Troubleshooting

| Síntoma | Causa / Acción |
|---|---|
| "Failed to fetch" en el dashboard | Cold start o redeploy de API. El cliente reintenta ~25 s; recargar. Evitar redeploys de API mientras se usa. |
| LLM en `rule-based` | Falta `OPENROUTER_API_KEY` en el env de **ai-analysis** → redeploy con `--env-file`. Verificar en `/health` (`llm_enabled`). |
| Botones de Telegram no responden | Webhook sin fijar → falta `PUBLIC_URL` del bot en `.compute.env`; redeploy. Verificar con `getWebhookInfo`. |
| Plan/scoring "de plantilla" | La licitación no tiene `url` de pliego, o doc-service caído. Botón **🔍 Analizar pliego**. |
| Estado sin "últimas ejecuciones" | Aún no ha corrido el cron con la imagen nueva; se puebla en el siguiente ciclo. |
