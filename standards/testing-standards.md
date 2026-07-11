# Testing Standards

Convenciones de testing para todos los proyectos del equipo.

## Stack por defecto (NestJS)

- **Unit tests:** Jest (incluido en NestJS).
- **Archivos:** `*.spec.ts` junto al código fuente.
- **E2E (cuando aplique):** `test/` con supertest.

## Scripts npm requeridos

| Script | Propósito |
|--------|-----------|
| `test` | Ejecutar todos los tests |
| `test:watch` | Modo watch para desarrollo |
| `test:cov` | Coverage report |
| `lint` | Linter (ESLint) |
| `build` | Compilar sin errores |

Estos scripts deben coincidir entre **local pre-push** y **CI**.

## Cuándo escribir tests

| Tipo de cambio | Test requerido |
|----------------|----------------|
| Lógica de negocio (servicios, reglas) | Unit test obligatorio |
| Parsing/transformación de datos | Unit test con fixtures |
| Integración con API externa | Test con mock de la API |
| Endpoints HTTP | Test E2E o integration (si el issue lo pide) |
| Config/setup puro | Smoke test mínimo (health check) |

## Convenciones

- Un archivo de test por archivo de producción cuando hay lógica testeable.
- Usar fixtures/mocks para APIs externas (Gemini, Telegram, RSS). No llamar APIs reales en tests.
- Tests deben ser determinísticos: no dependen de red, hora del sistema ni orden de ejecución.
- Nombrar tests descriptivamente: `should return false when sentiment is neutral`.

## Pre-push (Developer Agent)

Antes de cada push, ejecutar en orden:

```bash
npm run verify:lockfile   # Linux npm ci via Docker (catch lockfile drift)
npm run lint
npm run test
npm run build
```

### Por qué `verify:lockfile`

GitHub Actions corre en Linux. Un `package-lock.json` generado o tocado en
macOS puede pasar `npm ci` localmente y fallar en CI con errores del estilo:

```text
npm error Missing: @emnapi/core@x.y.z from lock file
```

Eso rompe el stage **Install dependencies** antes de lint/test/build. El
script `verify:lockfile` reproduce `npm ci` en `node:22` Linux sobre una
copia limpia del lockfile (sin montar `node_modules` del host).

Si falla tras cambiar deps:

```bash
docker run --rm -v "$PWD:/app" -w /app node:22-bookworm-slim \
  npm install --package-lock-only --ignore-scripts
npm run verify:lockfile
```

Si alguno falla, no pushear. Corregir primero.

## CI (GitHub Actions)

El pipeline de CI debe ejecutar los mismos comandos:

```bash
npm ci
npm run lint
npm run test
npm run build
```

Trigger: push a ramas `issue-*` y PRs hacia `main`.

## Criterios de aceptación en Issues

Cuando un issue incluya lógica testeable, al menos un criterio de aceptación
debe mencionar tests explícitamente. Ejemplo:

- [ ] Existe test unitario con feed RSS mockeado.
- [ ] CI verde en el PR.

## Coverage

- MVP: no hay umbral mínimo obligatorio.
- Post-MVP: considerar umbral mínimo (ej. 70%) en CI.
