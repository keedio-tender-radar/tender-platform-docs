# Formato de notificación Telegram

El bot (`tender-telegram-bot`) entrega dos tipos de mensaje: el **radar diario** y las **alertas
urgentes**. El contrato de datos de la notificación está en `tender-shared-contracts`
(`notification.py` / `notification.schema.json`).

## Radar diario

Se envía cada mañana (hora configurable) al canal interno de Keedio.

```
📊 Keedio Tender Radar — Resumen diario (24/06/2026)

Licitaciones analizadas: 184
Relevantes: 9   ·   Prioritarias: 3   ·   Descartadas: 175

TOP oportunidades

1. Plataforma de datos sanitarios
   Score: 92/100 · GO prioritario
   Presupuesto: 620.000 € · Plazo: 15/07/2026
   Órgano: Servicio de Salud …

2. Servicio de IA documental
   Score: 87/100 · GO con revisión
   Presupuesto: 280.000 € · Plazo: 18/07/2026

3. Integración de APIs corporativas
   Score: 81/100 · REVISAR solvencia
   Presupuesto: 410.000 € · Plazo: 22/07/2026
```

Cada oportunidad del TOP lleva botones inline:

```
[✅ Interesa] [❌ Descartar] [🤝 Partner] [📄 Informe]
```

## Alerta urgente

Para licitaciones de score alto con **plazo crítico** o cambios relevantes:

```
🚨 Licitación urgente

Servicio de IA documental — Score 87/100 (GO con revisión)
⏳ Cierre en 4 días (18/07/2026)
Presupuesto: 280.000 €

[✅ Interesa] [👀 Revisar solvencia] [📌 Prioritaria] [❌ Descartar]
```

## Comandos del bot

```
/start        Alta y vinculación del usuario
/resumen      Reenvía el último radar diario
/top          Top oportunidades abiertas
/urgentes     Licitaciones con plazo crítico
/licitacion <id>   Ficha de una licitación
/descartadas  Últimas descartadas (para revertir)
/go           Listado de las marcadas GO
/ayuda        Ayuda
```

## Botones de acción (inline)

| Botón | Acción registrada (`tender.user_action_received`) |
|-------|---------------------------------------------------|
| ✅ Interesa | `interested` |
| ❌ Descartar | `discarded` |
| 🤝 Revisar con partner | `partner` |
| 📄 Generar informe | `generate_report` (fase 6) |
| 🧾 Matriz de cumplimiento | `compliance_matrix` (fase 6) |
| 📌 Prioritaria | `prioritize` |
| 👀 Revisar solvencia | `review_solvency` |

## Reglas de formato

- Importes en euros con separador de miles; fechas `DD/MM/YYYY`.
- Truncar títulos largos a una línea; el detalle completo en `/licitacion <id>` o dashboard.
- Mensajes idempotentes: una misma licitación no se notifica dos veces el mismo día salvo cambio
  relevante (`tender.updated`).
- El envío se registra como `tender.telegram_notification_sent` para trazabilidad y métricas.
