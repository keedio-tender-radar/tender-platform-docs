# Visión de producto

## El problema

La contratación pública en España y la UE genera **miles de licitaciones al día** repartidas en
PLACSP, TED y portales autonómicos. Para una empresa como Keedio, detectar a tiempo las pocas que
encajan con su catálogo (datos, IA, integración, cloud) es un trabajo manual, lento y propenso a
perder oportunidades por plazos vencidos o por no leer los pliegos a fondo.

Síntomas actuales:

- Revisión manual y dispersa de portales públicos.
- Oportunidades detectadas tarde (plazos ajustados para preparar oferta).
- Decisión Go/No-Go subjetiva y sin trazabilidad.
- Lectura de pliegos (PCAP/PPT) costosa y repetitiva.
- Sin memoria de mercado (quién gana, a qué baja, qué organismos compran qué).

## La propuesta

**Keedio Tender Radar**: una plataforma interna que automatiza el ciclo
*detectar → analizar → puntuar → decidir → preparar*.

> Cada mañana, Keedio recibe en Telegram un resumen priorizado de las licitaciones relevantes,
> con una recomendación Go/No-Go explicada, y puede actuar con un toque (interesa, descartar,
> partner, generar informe).

## Propuesta de valor

| Para | Valor |
|------|-------|
| Preventa / negocio | Menos tiempo de cribado, más oportunidades a tiempo, decisión trazable |
| Dirección | Visión de pipeline de contratación pública y de mercado |
| Equipo técnico | Lectura asistida de pliegos (resumen, requisitos, solvencia, riesgos) |

## Alcance

**Dentro (MVP):** ingesta PLACSP/TED, normalización, filtrado por CPV/keywords, análisis IA del
anuncio (y pliego cuando sea relevante), scoring Go/No-Go, radar diario por Telegram con acciones.

**Dentro (fases siguientes):** chat documental con citas del pliego (RAG), dashboard web,
inteligencia de mercado (competidores, bajas, organismos), generación de oferta (Go/No-Go report,
matriz de cumplimiento, índice de memoria técnica).

**Fuera (por ahora):** envío telemático de ofertas a las plataformas, firma electrónica, gestión
contractual posterior a la adjudicación.

## Principios

1. **No decidir solo por keywords.** El filtro rápido criba; la decisión se apoya en el análisis
   del contenido (anuncio y, si es relevante, pliegos completos).
2. **Explicabilidad.** Todo score y recomendación va acompañado del *por qué*.
3. **Humano en el bucle.** El sistema recomienda; la persona decide (desde Telegram o dashboard).
4. **Trazabilidad.** Cada licitación guarda su análisis, score y las acciones tomadas.
5. **Coste de IA controlado.** Análisis profundo solo cuando la licitación supera el cribado.
