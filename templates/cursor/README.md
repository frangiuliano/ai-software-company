# Cursor slash commands (agent shortcuts)

Atajos finos que disparan los agentes del marco. La lógica vive en
`agents/*/prompt.md`; estos archivos solo invocan el rol correcto.

## Instalación en un repo de producto

```bash
mkdir -p .cursor/commands
cp /path/to/ai-software-company/templates/cursor/commands/*.md .cursor/commands/
```

Opcional (todos tus proyectos):

```bash
mkdir -p ~/.cursor/commands
cp /path/to/ai-software-company/templates/cursor/commands/*.md ~/.cursor/commands/
```

## Uso

En el chat de Agent, escribí `/` y elegí el comando:

| Comando | Ejemplo | Efecto |
|---------|---------|--------|
| `/dev` | `/dev siguiente` | Próximo issue → implementación → PR (sugiere `Chat: #N …` para rename manual) |
| `/dev` | `/dev 8` o `/dev #8` | Issue N → implementación → PR (sugiere `Chat: #N …` para rename manual) |
| `/rev` | `/rev 12` o `/rev siguiente` | Review del PR (modo Cursor) |
| `/po` | `/po <idea>` | Crear/actualizar Issues |
| `/arch` | `/arch <pregunta>` | Asesoría de arquitectura |
| `/ops` | `/ops <tema CI/infra>` | Asesoría DevOps |

El workspace debe poder resolver `ai-software-company` (multi-root, sibling, o path conocido).
