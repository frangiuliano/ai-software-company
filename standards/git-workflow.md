# Git Workflow Standards

Convenciones de git para todos los proyectos del equipo. Cualquier agente o
desarrollador que trabaje en un Issue debe seguir estas reglas.

## Branches

Formato:

```
issue-<number>-<short-slug-in-english>
```

Ejemplo: `issue-1-rss-news-collector`

Reglas:

- Todo en minúsculas.
- Todo en inglés.
- Usar guiones (`-`), nunca espacios ni underscores.
- El slug debe ser corto y descriptivo del Issue.

## Commits

Usar [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(issue-<number>): <short description in imperative English>
```

Ejemplos:

```
feat(issue-1): add RSS feed fetch service
fix(issue-1): prevent duplicates by content hash
test(issue-1): add RSS item parsing tests
docs(issue-1): document collector environment variables
```

Tipos estándar:

| Tipo       | Cuándo usarlo                                      |
|------------|----------------------------------------------------|
| `feat`     | Funcionalidad nueva                                |
| `fix`      | Corrección de bug                                  |
| `refactor` | Cambio de código sin cambiar comportamiento        |
| `test`     | Agregar o modificar tests                          |
| `docs`     | Solo documentación                                 |
| `chore`    | Config, dependencias, tooling                      |

### Un commit = un cambio lógico

Cada commit debe representar un cambio que tenga sentido revertir o revisar
de forma aislada. Si migrás el schema de DB y agregás el endpoint que lo usa,
son dos commits distintos.

## Pull Requests

### Título

```
[Issue #<number>] <issue title>
```

Ejemplo: `[Issue #1] Crear News Collector desde RSS feeds`

El título del PR usa el nombre del Issue tal como está en GitHub (puede estar
en español). La rama sigue siendo en inglés.

### Descripción

La estructura del PR está definida en `.github/PULL_REQUEST_TEMPLATE.md` de
cada repo de producto. GitHub autocompleta la descripción con ese template al
crear un PR — no improvisar otra estructura.

Secciones requeridas:

- **Resumen** — qué se implementó, en 2-3 líneas.
- **Cambios** — lista de cambios concretos.
- **Cómo probarlo** — pasos para validar localmente.
- **Notas para el reviewer** — decisiones no obvias, trade-offs, dudas.
- **Requiere antes de deploy** — env vars, migraciones, servicios externos.
  Escribir "Ninguno" si no aplica.
- **Checklist** — tests, criterios de aceptación, alcance del Issue.

### Flujo completo por Issue

1. Crear rama desde `main`: `issue-<n>-<slug>`
2. Implementar con commits atómicos siguiendo Conventional Commits.
3. Push de la rama al remoto.
4. Abrir PR con título y descripción según este estándar.
5. No incluir código fuera del alcance definido en el Issue.

## Dónde vive cada cosa

| Qué                          | Dónde                                          |
|------------------------------|------------------------------------------------|
| Convenciones git (este archivo) | `ai-software-company/standards/git-workflow.md` |
| Convenciones de Issues y labels | `ai-software-company/standards/issue-workflow.md` |
| PR template                  | `<product-repo>/.github/PULL_REQUEST_TEMPLATE.md` |
| Extensiones de workflow por proyecto | `<product-repo>/.github/ISSUE_WORKFLOW.md` |
| Prompt del Developer Agent   | `ai-software-company/agents/developer/prompt.md` |
| Prompt del Product Owner Agent | `ai-software-company/agents/product-owner/prompt.md` |

El PR template y el issue workflow por proyecto deben replicarse en cada repo
de producto. GitHub solo lee el PR template desde `.github/` del repo donde
se abre el PR.
