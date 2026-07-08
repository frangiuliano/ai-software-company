# Issue Workflow Standards

Convenciones de labels y flujo de trabajo para Issues de GitHub. Cualquier
agente o desarrollador que cree, priorice o implemente Issues debe seguir
estas reglas.

Este estándar es **agnóstico al proyecto**. Cada repo de producto puede
definir extensiones en `.github/ISSUE_WORKFLOW.md` (ver sección final).

## Vocabulario de labels

| Label | Propósito | Quién lo asigna |
|-------|-----------|-----------------|
| `order-01` … `order-99` | Secuencia de ejecución (NN con cero a la izquierda) | Product Owner |
| `status:ready` | Dependencias cumplidas, disponible para tomar | Product Owner o Developer |
| `status:in-progress` | Un dev/agente lo está implementando | Developer |
| `status:blocked` | Esperando que se cierre otro issue | Product Owner o Developer |
| `type:foundation` | Setup, infraestructura, DB | Product Owner |
| `type:feature` | Funcionalidad de producto | Product Owner |
| `type:bug` | Corrección de bug | Product Owner |
| `type:chore` | Mantenimiento, tooling, deps | Product Owner |
| `scope:<nombre>` | Agrupación de fase (ej. `scope:mvp`, `scope:v1`) | Product Owner |

### Reglas de labels

- **`order-NN` es la fuente de verdad para el orden de ejecución**, no el
  número de issue de GitHub. Un issue #8 puede ser `order-01`.
- **`Dependencias` en el body del issue** es la fuente de verdad para
  bloqueos entre issues.
- Los labels `status:*` son recomendados pero opcionales; el algoritmo de
  selección puede inferir `blocked` leyendo dependencias.
- Un issue solo debe tener un label `order-*` y un label `status:*` a la vez.
- Un issue solo debe tener un label `type:*`.

## Secciones obligatorias en el body del issue

Además del template del Product Owner, todo issue debe incluir:

```markdown
## Dependencias
- Issue #N (descripción breve)
(o "Ninguna" si es issue raíz)

## Orden de ejecución
**N de M** — Descripción de posición en el backlog.

## Bloquea a
- Issue #N (descripción breve)
(o "Ninguno" si no bloquea a nadie)
```

Las secciones del body y los labels deben ser **consistentes entre sí**.

## Algoritmo: elegir el próximo issue (Developer)

Seguí estos pasos en orden:

1. Leé `.github/ISSUE_WORKFLOW.md` del repo de producto (si existe) para
   conocer extensiones de labels.
2. Listá issues abiertos del repo.
3. Filtrá los que tengan label `order-*`.
4. Ordená por `order-NN` ascendente (01 antes que 02).
5. Para cada issue en ese orden:
   a. Leé la sección **Dependencias** del body.
   b. Si algún issue referenciado sigue abierto → saltá (está bloqueado).
   c. Si otro issue del mismo `scope:*` ya tiene `status:in-progress` →
      saltá (un scope, un issue a la vez).
   d. Si pasa los filtros → **ese es el issue a tomar**. Pará.
6. Si ningún issue pasa los filtros, reportá qué está bloqueando el avance.

### Al empezar un issue

1. Leé el issue completo (contexto, alcance, fuera de alcance, criterios).
2. Agregá el label `status:in-progress`.
3. Remové `status:ready` y `status:blocked` si existen.
4. Creá la rama según `git-workflow.md`.

### Al terminar un issue

1. Abrí el PR según `git-workflow.md`.
2. Incluí `Closes #N` en el body del PR para que GitHub cierre el issue al
   mergear.
3. No remuevas `status:in-progress` manualmente; el cierre del issue lo
   resuelve.

## Algoritmo: crear y ordenar issues (Product Owner)

1. Dividí el trabajo en issues implementables en 1-3 días.
2. Asigná labels a cada issue según el vocabulario de este estándar.
3. Numerá secuencialmente con `order-01`, `order-02`, etc.
4. Escribí dependencias cruzadas en el body (`Dependencias`, `Bloquea a`).
5. Agrupá por fase con `scope:*` si aplica (ej. `scope:mvp`).
6. Verificá que no haya ciclos de dependencia.

## Bootstrap de labels en un repo nuevo

Al crear un repo de producto:

1. Copiá `templates/github/labels.json` de este repo.
2. Creá los labels en GitHub:

```bash
# Ejemplo con gh CLI (ajustar owner/repo)
while IFS= read -r line; do
  name=$(echo "$line" | jq -r '.name')
  color=$(echo "$line" | jq -r '.color')
  desc=$(echo "$line" | jq -r '.description')
  gh label create "$name" --color "$color" --description "$desc" --force
done < <(jq -c '.[]' templates/github/labels.json)
```

3. Copiá `templates/github/ISSUE_WORKFLOW.md` a
   `<product-repo>/.github/ISSUE_WORKFLOW.md` y completá las extensiones.

## Dónde vive cada cosa

| Qué | Dónde |
|-----|-------|
| Contrato de labels y algoritmos (este archivo) | `ai-software-company/standards/issue-workflow.md` |
| Template de labels para bootstrap | `ai-software-company/templates/github/labels.json` |
| Extensiones por proyecto (opcional) | `<product-repo>/.github/ISSUE_WORKFLOW.md` |
| Prompt del Developer Agent | `ai-software-company/agents/developer/prompt.md` |
| Prompt del Product Owner Agent | `ai-software-company/agents/product-owner/prompt.md` |

## Extensiones por proyecto

Si un repo necesita labels adicionales (ej. `priority:p0`), documentalos en
`<product-repo>/.github/ISSUE_WORKFLOW.md` como **extensión** de este
estándar, nunca como reemplazo. El Developer debe leer ese archivo antes de
elegir un issue.
