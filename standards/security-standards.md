# Security Standards

Reglas de seguridad para todos los proyectos del equipo. Aplican a Developer,
Reviewer y cualquier agente que escriba o revise código.

## Secrets y configuración

- Nunca commitear `.env`, tokens, API keys ni credenciales.
- Usar `.env.example` con placeholders y descripciones, sin valores reales.
- Validar env vars al boot; fallar con mensaje claro si falta una obligatoria.
- Inyectar secrets vía módulo de config, nunca `process.env` disperso.
- Nunca loguear valores de secrets (tokens, passwords, API keys).
- En logs, enmascarar datos sensibles (ej. `***` para tokens).

## Entrada externa no confiable

- Tratar todo contenido RSS/scraping/API externa como no confiable.
- Sanitizar o limitar longitud de contenido antes de persistir o enviar a IA.
- Validar URLs antes de fetch (esquema http/https, no file:// ni IPs internas).
- Timeout en requests HTTP externos.
- No ejecutar código embebido en feeds o respuestas externas.

## Integraciones con IA

- No enviar secrets en prompts.
- Limitar tamaño del contenido enviado al modelo.
- No confiar ciegamente en la salida del modelo: validar y parsear con schema.
- Manejar rate limits y errores sin exponer internals en logs.
- Si Gemini (u otro proveedor) se usa en CI/review **y** en el producto,
  usar **proyectos y API keys separados** (ej. `GEMINI_API_KEY_REVIEWER` vs
  `GEMINI_API_KEY_FINANCE`) para no compartir free tier / rate limits.
- En pipelines de alto volumen, preferir cola + delay configurable entre
  requests antes de depender solo de reintentos ante 429.

## Base de datos

- Usar queries parametrizadas (ORM/query builder). Nunca concatenar SQL.
- Migraciones versionadas; no modificar schema manualmente en producción.
- Constraints unique donde aplique deduplicación.

## API y endpoints

- Endpoints de test/debug protegidos o deshabilitados en producción.
- Validar input con DTOs y class-validator (NestJS) o equivalente.
- No exponer stack traces al cliente en producción.

## Dependencias

- Preferir dependencias mantenidas y con buena reputación.
- Documentar en PR si se agrega dependencia con acceso a red o filesystem.

## Checklist para PRs (Reviewer)

- [ ] No hay secrets en código, logs ni commits.
- [ ] `.env.example` actualizado si hay nuevas variables.
- [ ] Input externo sanitizado/validado.
- [ ] Queries parametrizadas.
- [ ] Endpoints de debug no expuestos sin protección.
