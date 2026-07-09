# Architecture Standards

Convenciones de arquitectura para todos los proyectos del equipo. El Architect
Agent asesora sobre estas reglas; el Developer las implementa.

## Principios generales

- **Modularidad por dominio:** un módulo NestJS por bounded context (ej.
  `news`, `analysis`, `notifications`).
- **Separación de capas:** controller → service → repository. No lógica de
  negocio en controllers.
- **Single Responsibility:** cada servicio tiene una responsabilidad clara.
- **Dependency Injection:** usar el DI de NestJS, no instanciar dependencias
  manualmente.
- **Config externalizada:** toda config en env vars validadas, nunca hardcodeada.

## Estructura de módulos (NestJS)

```
src/
├── main.ts
├── app.module.ts
├── config/           # Config module, env validation
├── health/           # Health check
├── news/             # RSS collection, persistence
│   ├── news.module.ts
│   ├── news.service.ts
│   ├── news.controller.ts (si aplica)
│   └── news.repository.ts (o entity + TypeORM)
├── analysis/         # Gemini integration
├── notifications/    # Telegram
└── pipeline/         # Orchestration cron (Issue final)
```

## Decisiones que requieren Architect

Documentar en PR ("Notas para el reviewer") o crear ADR breve cuando:

- Elegir ORM (TypeORM vs Prisma).
- Agregar un módulo nuevo cross-cutting (cache, queue, auth).
- Cambiar estructura de módulos existente.
- Introducir patrón async (event-driven, message queue).

## Decisiones que NO requieren Architect

- Implementar un servicio dentro de un módulo ya definido.
- Agregar un endpoint a un módulo existente.
- Escribir tests, migraciones, DTOs.

## Documentación por proyecto

Cada repo de producto debe tener (cuando aplique):

| Archivo | Cuándo crearlo |
|---------|----------------|
| `README.md` | Desde el primer issue |
| `ARCHITECTURE.md` | Después del issue de scaffold (#8) |
| ADRs en `docs/adr/` | Solo para decisiones no obvias |

## Checklist para PRs (Reviewer)

- [ ] Código en el módulo correcto.
- [ ] No hay lógica de negocio en controllers.
- [ ] Dependencias inyectadas, no instanciadas.
- [ ] Config via env vars, no hardcodeada.
