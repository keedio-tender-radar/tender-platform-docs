# Requisitos de seguridad

## Secretos

- **Nunca** en git: tokens de Telegram, claves de IA (OpenAI/Azure), credenciales de BD/MinIO.
- Inyectar por **variables de entorno** y, en CI/CD, por **GitHub Actions secrets** / gestor de
  secretos del entorno.
- Cada repo incluye `.env.example` (solo nombres, sin valores) y `.env` en `.gitignore`.
- Rotación periódica de tokens; revocar de inmediato si se filtran.

## Organización GitHub

- 2FA obligatorio para todos los miembros.
- **Branch protection** en `main`: PR obligatorio, status checks (CI) en verde, sin push directo.
- `CODEOWNERS` por repo para revisión por área.
- **Dependabot** y **secret scanning** activados.
- Entornos `dev` / `staging` / `prod` con secretos separados.

## Servicios

- Autenticación servicio-a-servicio (clave interna o mTLS) además de auth de usuario en el API.
- Principio de mínimo privilegio en credenciales de BD y almacenamiento de objetos.
- Validación de entrada en todos los endpoints (Pydantic) y límites de tamaño en descargas de
  documentos.
- Las fuentes públicas se consultan solo por sus canales oficiales; respetar rate limits.

## Datos

- Los pliegos son públicos, pero el **perfil de Keedio**, su solvencia real y las decisiones de
  negocio son **internos**: no exponerlos en repos públicos ni en logs.
- Logs sin secretos ni PII; revisar mensajes de error que reflejen respuestas de terceros.

## Cumplimiento

- Trazabilidad de decisiones (auditoría de acciones Go/No-Go).
- Retención y borrado de datos según política interna de Keedio.
