# ADR 003 — Telegram como canal de notificación del MVP

- **Estado:** Aceptada
- **Fecha:** 2026-06-24

## Contexto

El MVP debe entregar el radar diario y permitir decisiones rápidas (interesa/descartar/partner)
con la mínima fricción para el equipo de Keedio.

## Decisión

Usar **Telegram** como canal principal de notificación y acción en el MVP, mediante un bot con
mensajes y **botones inline**.

## Justificación

- Adopción inmediata: el equipo ya usa Telegram; sin necesidad de un frontend para empezar.
- Botones inline permiten capturar decisiones (`tender.user_action_received`) sin salir del chat.
- API de bots simple y gratuita.

## Consecuencias

- El dashboard web (fase 3) será el canal rico; Telegram queda como canal de alertas/acción rápida.
- Hay que gestionar el token del bot como secreto y el mapeo usuario→chat.
- Formato limitado (texto + botones); el detalle profundo va a `/licitacion <id>` o al dashboard.

## Alternativas descartadas

- **Email:** menos interactivo, peor para acción rápida.
- **Slack/Teams:** posibles en el futuro; Telegram es el de menor fricción para el equipo ahora.
