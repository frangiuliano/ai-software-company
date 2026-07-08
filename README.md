# ai-software-company

Marco de trabajo para un equipo de agentes de desarrollo de software.

## Estructura

```
ai-software-company/
├── agents/
│   ├── product-owner/   # Transforma ideas en Issues de GitHub
│   └── developer/       # Implementa Issues en repos de producto
├── standards/
│   ├── git-workflow.md    # Convenciones de branches, commits y PRs
│   └── issue-workflow.md  # Labels, orden de ejecución y algoritmo de selección
└── templates/
    └── github/
        ├── labels.json         # Labels estándar para bootstrap en repos nuevos
        └── ISSUE_WORKFLOW.md   # Template de extensiones por proyecto
```

## Estándares

Las convenciones de git (branches, commits, PRs) viven en
[`standards/git-workflow.md`](standards/git-workflow.md). Todos los agentes
y desarrolladores deben seguirlas.

Las convenciones de labels y flujo de Issues viven en
[`standards/issue-workflow.md`](standards/issue-workflow.md). El Product Owner
asigna labels al crear el backlog; el Developer los usa para elegir el
próximo issue sin instrucciones adicionales.

## Templates por repo de producto

Al crear un repo de producto, copiá desde `templates/github/`:

| Archivo | Destino en el repo de producto |
|---------|-------------------------------|
| `ISSUE_WORKFLOW.md` | `.github/ISSUE_WORKFLOW.md` |
| `labels.json` | Usar para crear labels en GitHub (ver `issue-workflow.md`) |

El PR template (`.github/PULL_REQUEST_TEMPLATE.md`) también vive en cada repo
de producto porque GitHub lo requiere ahí.
