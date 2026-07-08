# ai-software-company

Marco de trabajo para un equipo de agentes de desarrollo de software.

## Estructura

```
ai-software-company/
├── agents/
│   ├── product-owner/   # Transforma ideas en Issues de GitHub
│   └── developer/       # Implementa Issues en repos de producto
└── standards/
    └── git-workflow.md  # Convenciones de branches, commits y PRs
```

## Estándares

Las convenciones de git (branches, commits, PRs) viven en
[`standards/git-workflow.md`](standards/git-workflow.md). Todos los agentes
y desarrolladores deben seguirlas.

El PR template (`.github/PULL_REQUEST_TEMPLATE.md`) vive en cada repo de
producto porque GitHub lo requiere ahí.
