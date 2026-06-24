# Metodología de inteligencia de mercado (fase 5)

Inspirada en herramientas tipo Tendios. Aprovecha el histórico público de adjudicaciones para dar
contexto competitivo a cada oportunidad y anticipar oportunidades futuras. Vive en
`tender-market-intelligence-service` y no es imprescindible para el MVP-1.

## Preguntas que responde

- ¿Quién suele **ganar** este tipo de contratos (CPV/órgano)?
- ¿A qué **baja económica media** se adjudican?
- ¿Qué **organismos** compran lo que Keedio ofrece, y con qué recurrencia?
- ¿Qué **CPV** son estratégicos por volumen/frecuencia?
- ¿Qué **contratos recurrentes** están próximos a vencer (oportunidad futura)?

## Fuentes

- Histórico de adjudicaciones de PLACSP/TED (formalizaciones).
- Datos abiertos de contratación.

## Análisis

| Módulo | Salida |
|--------|--------|
| Competidores | Adjudicatarios frecuentes por CPV/órgano; cuota estimada |
| Pricing | Baja media y tendencia de presupuesto por categoría |
| Compradores | Órganos recurrentes y su patrón de compra |
| CPV | Tendencia y estacionalidad de CPV estratégicos |
| Recurrencia | Contratos que vencen pronto → posible nueva licitación |

## Uso en el producto

- Enriquecer la ficha de una licitación con "quién suele ganar esto" y "baja esperada".
- Informe periódico de mercado para dirección.
- Señales proactivas: avisar de contratos recurrentes próximos a salir.

> Es analítica sobre datos públicos agregados; no influye en el scoring del MVP-1, pero en fases
> avanzadas puede ponderar la dimensión competitiva.
