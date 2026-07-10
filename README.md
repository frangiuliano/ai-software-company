# ai-software-company

Marco de trabajo para un equipo de agentes de desarrollo de software.

## Estructura

```
ai-software-company/
├── agents/
│   ├── product-owner/   # Transforma ideas en Issues de GitHub
│   ├── architect/       # Asesora sobre diseño y estructura
│   ├── devops/          # Asesora sobre CI/CD, infra y deploy
│   ├── developer/       # Implementa Issues en repos de producto
│   └── reviewer/        # Revisa PRs antes del merge
├── .cursor/commands/    # Slash commands (/dev, /rev, /po, /arch, /ops)
├── standards/
│   ├── git-workflow.md          # Branches, commits y PRs
│   ├── issue-workflow.md        # Labels y orden de ejecución
│   ├── agent-workflow.md        # Cuándo actúa cada agente
│   ├── security-standards.md    # Reglas de seguridad
│   ├── testing-standards.md     # Convenciones de testing y CI
│   └── architecture-standards.md # Estructura de módulos y capas
└── templates/
    ├── cursor/                  # Slash commands para bootstrap
    └── github/
        ├── labels.json         # Labels estándar para bootstrap
        └── ISSUE_WORKFLOW.md   # Template de extensiones por proyecto
```

## Flujo del equipo

```
PO → (Architect si aplica) → Issues → Developer → PR → CI → Review (/rev o AI Review opt-in) → Merge manual
         ↑                              ↑
       DevOps (CI/infra)            DevOps (si CI falla)
```

Detalle completo en [`standards/agent-workflow.md`](standards/agent-workflow.md).

## Estándares

| Estándar | Propósito |
|----------|-----------|
| [`git-workflow.md`](standards/git-workflow.md) | Branches, commits, PRs |
| [`issue-workflow.md`](standards/issue-workflow.md) | Labels, orden, selección de issues |
| [`agent-workflow.md`](standards/agent-workflow.md) | Triggers y encadenamiento de agentes |
| [`security-standards.md`](standards/security-standards.md) | Secrets, input externo, IA |
| [`testing-standards.md`](standards/testing-standards.md) | Tests, pre-push, CI |
| [`architecture-standards.md`](standards/architecture-standards.md) | Módulos, capas, decisiones |

## Agentes

| Agente | Tipo | Atajo | Prompt (fuente de verdad) |
|--------|------|-------|---------------------------|
| Product Owner | Ejecución | `/po` | `agents/product-owner/prompt.md` |
| Architect | Asesoría | `/arch` | `agents/architect/prompt.md` |
| DevOps | Asesoría | `/ops` | `agents/devops/prompt.md` |
| Developer | Ejecución | `/dev` | `agents/developer/prompt.md` |
| Reviewer | Gate | `/rev` (default) o `ai-review.yml` (opt-in) | `agents/reviewer/prompt.md` |

Security y QA no tienen agente dedicado: viven en estándares + Reviewer + CI.

### Slash commands

En Agent chat, escribí `/` y elegí el comando. Ejemplos:

- `/dev siguiente` — próximo issue hasta PR
- `/dev 8` — issue #8 hasta PR
- `/rev 12` — review del PR #12
- `/po <idea>` — crear Issues
- `/arch <pregunta>` — asesoría de diseño
- `/ops <tema>` — CI / infra / AI Review

Detalle de instalación: [`templates/cursor/README.md`](templates/cursor/README.md).

## Templates por repo de producto

Al crear un repo de producto, copiá:

| Origen | Destino en el repo de producto |
|--------|-------------------------------|
| `templates/github/ISSUE_WORKFLOW.md` | `.github/ISSUE_WORKFLOW.md` |
| `templates/github/labels.json` | Usar para crear labels en GitHub (ver `issue-workflow.md`) |
| `templates/cursor/commands/*.md` | `.cursor/commands/` |

El PR template (`.github/PULL_REQUEST_TEMPLATE.md`) también vive en cada repo
de producto porque GitHub lo requiere ahí.
