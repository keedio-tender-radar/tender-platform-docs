# Perfil de empresa (Keedio)

Este documento describe el **perfil que alimenta el scoring**. Los valores concretos viven como
datos versionables en `tender-scoring-service/rules/` (YAML); aquí se documenta su significado.

> Los listados de CPV/keywords de abajo son una **base inicial** a refinar con negocio.

## Catálogo y posicionamiento

Keedio se posiciona en **datos, IA, integración y cloud**: ingeniería de datos, plataformas
analíticas, big data, MLOps/IA aplicada, integración de sistemas y APIs, y servicios cloud/devops.

## Keywords positivas (encaje técnico ↑)

```
datos, big data, data lake, data warehouse, gobierno del dato, calidad de dato,
analítica, business intelligence, cuadro de mando,
inteligencia artificial, machine learning, modelos, RAG, procesamiento de lenguaje,
integración, API, interoperabilidad, ETL, ingesta,
cloud, kubernetes, contenedores, devops, observabilidad,
plataforma tecnológica, modernización, migración
```

## Keywords negativas (fuera de catálogo ↓)

```
obra civil, construcción, mobiliario, limpieza, vigilancia, catering,
suministro eléctrico, jardinería, transporte, material de oficina,
mantenimiento de edificios, señalización
```

## CPV preferidos (ejemplos a validar)

```
72000000  Servicios TI: consultoría, desarrollo, internet y apoyo
72300000  Servicios de datos
72310000  Servicios de tratamiento de datos
72500000  Servicios informáticos
48000000  Paquetes de software y sistemas de información
```

## CPV excluidos (ejemplos)

```
45000000  Trabajos de construcción
90900000  Servicios de limpieza
79710000  Servicios de seguridad
```

## Umbrales

| Parámetro | Valor inicial | Uso |
|-----------|---------------|-----|
| Presupuesto mínimo rentable | 50.000 € | Por debajo, penaliza dimensión presupuesto |
| Presupuesto objetivo | 150.000–800.000 € | Banda ideal |
| Plazo mínimo operativo | 7 días | Por debajo → REVISAR urgente |
| Margen objetivo | (definir con negocio) | Dimensión presupuesto/margen |

## Solvencia típica de Keedio (para contraste)

- **Económica:** cifra de negocio anual y patrimonio (valores reales en datos privados, no en git).
- **Técnica:** proyectos de referencia en datos/IA/integración/cloud, certificaciones y equipo.

El análisis IA extrae la solvencia **exigida** por cada pliego y el scoring la contrasta con estos
valores para puntuar las dimensiones de solvencia y la necesidad de partner.

## Reglas de partner

- Si la solvencia técnica exigida supera la de Keedio pero la cubre un socio habitual → `PARTNER`.
- Si el volumen/lotes exceden capacidad de entrega en solitario → `PARTNER`.

> Datos sensibles (cifras de solvencia reales, lista de partners) **no** se versionan en git;
> se inyectan por configuración/secretos en `tender-scoring-service`.
