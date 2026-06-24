# Modelo de scoring Go/No-Go

El score mide el **encaje de una licitación con Keedio** en una escala de **0 a 100**, repartida
en 9 dimensiones. Es la base de la recomendación (ver [go-no-go-methodology.md](go-no-go-methodology.md)).

## Dimensiones y pesos

| # | Dimensión | Peso | Qué mide |
|---|-----------|-----:|----------|
| 1 | Encaje técnico | 30 | Afinidad del objeto con el catálogo Keedio (datos, IA, integración, cloud) |
| 2 | Presupuesto y margen | 15 | Importe vs umbral mínimo rentable y margen esperado |
| 3 | Solvencia técnica | 15 | ¿Keedio cumple la solvencia técnica exigida? |
| 4 | Solvencia económica | 10 | ¿Keedio cumple la solvencia económica/financiera? |
| 5 | Plazo disponible | 10 | Días hasta fin de presentación vs esfuerzo de oferta |
| 6 | Necesidad de partner | 5 | ¿Se puede ir solo o hace falta UTE/subcontrata? |
| 7 | Complejidad documental | 5 | Volumen y dificultad de la documentación administrativa |
| 8 | Riesgo contractual | 5 | Penalizaciones, SLA, condiciones gravosas |
| 9 | Riesgo de incompatibilidad | 5 | Conflictos, exclusiones, requisitos imposibles |
| | **Total** | **100** | |

## Cálculo

Cada dimensión produce un sub-score `0..peso`. El total es la suma. Las dimensiones de **riesgo**
(7, 8, 9) puntúan a la inversa: a mayor riesgo, menor sub-score.

```
score_total = Σ sub_score_i        (0..100)
```

Entradas del cálculo:

- **Anuncio normalizado** (CPV, importe, plazo, objeto, órgano de contratación).
- **Perfil Keedio** (ver [keedio-company-profile.md](keedio-company-profile.md)): keywords
  positivas/negativas, CPV preferidos/excluidos, umbrales de presupuesto y solvencia, reglas de
  partner.
- **Análisis IA** (cuando la licitación supera el cribado): requisitos, solvencia exigida,
  criterios de adjudicación, riesgos detectados.

## Bandas de recomendación

| Score | Recomendación | Significado |
|------:|---------------|-------------|
| ≥ 80 | **GO** | Oportunidad prioritaria; preparar oferta |
| 60–79 | **GO con revisión** / **REVISAR** | Encaje bueno con dudas a confirmar (solvencia, plazo) |
| 40–59 | **PARTNER** / **REVISAR** | Solo viable con partner o requiere análisis humano |
| < 40 | **NO-GO** | No encaja; descartar |

Las bandas son orientativas: ciertas **reglas duras** fuerzan la recomendación
independientemente del score (p. ej. CPV excluido → NO-GO; solvencia imposible → NO-GO;
plazo < mínimo operativo → REVISAR urgente). Ver metodología.

## Explicabilidad

Cada score se persiste con el desglose por dimensión y una lista de *factores* (positivos y
negativos) en lenguaje natural, para mostrarlos en Telegram y dashboard. El contrato de datos
está en `tender-shared-contracts` (`score.py` / `score.schema.json`).

## Versionado del modelo

El modelo de scoring evoluciona: cada `tender_scores` guarda `model_version`. Los pesos y reglas
viven como datos (YAML en `tender-scoring-service/rules/`), no hardcodeados, para poder ajustarlos
sin desplegar código.
