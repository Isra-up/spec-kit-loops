# Spec-kit — Teoría

Fuente: https://github.com/github/spec-kit

## ¿Qué es?

Spec-kit es un toolkit open source creado por **GitHub** que implementa
**Spec-Driven Development (SDD)** — una metodología donde las especificaciones
se convierten en el corazón del desarrollo, no solo en documentación desechable.

La idea central: en lugar de empezar a codear directamente ("vibe coding"),
primero escribes una especificación clara y estructurada, y luego el AI
la ejecuta para generar el código.

> "Specifications become executable" — las specs generan implementaciones,
> no solo las guían.

---

## El problema que resuelve

El desarrollo tradicional con AI tiende a ser improvisado:
- Le dices al AI "construye X" sin contexto
- El AI adivina la arquitectura, el stack, los requisitos
- El resultado es inconsistente o no es lo que querías

Spec-kit fuerza a pensar primero, codear después.

---

## Flujo de trabajo (comandos slash)

### Comandos principales (en orden)

| Comando | ¿Qué hace? |
|---|---|
| `/speckit.constitution` | Define los principios y reglas del proyecto |
| `/speckit.specify` | Describe QUÉ quieres construir (sin hablar de tech) |
| `/speckit.clarify` | Aclara requisitos ambiguos antes de planear |
| `/speckit.plan` | Define el stack técnico y la arquitectura |
| `/speckit.tasks` | Divide el plan en tareas accionables |
| `/speckit.implement` | El AI ejecuta todas las tareas y construye |
| `/speckit.converge` | Revisa el código vs la spec y detecta lo que falta |

### Comandos opcionales

| Comando | ¿Qué hace? |
|---|---|
| `/speckit.analyze` | Verifica consistencia entre spec, plan y tareas |
| `/speckit.checklist` | Genera listas de validación de calidad |
| `/speckit.taskstoissues` | Convierte tareas en GitHub Issues |

---

## Ejemplo real del flujo

```bash
# 1. Principios del proyecto
/speckit.constitution Create principles focused on code quality and testing

# 2. Qué quiero construir (sin mencionar tech stack)
/speckit.specify Build an app to organize photos into albums grouped by date,
with drag-and-drop reordering

# 3. Aclarar dudas antes de planear
/speckit.clarify

# 4. Definir el cómo técnico
/speckit.plan Use Vite, vanilla HTML/CSS/JS, SQLite for metadata

# 5. Generar tareas
/speckit.tasks

# 6. Implementar todo
/speckit.implement
```

---

## Estructura que genera en tu proyecto

```
.specify/
  memory/
    constitution.md       ← principios del proyecto
  templates/
    spec-template.md
    plan-template.md
    tasks-template.md
  scripts/bash/           ← scripts de automatización

specs/
  001-nombre-feature/
    spec.md               ← qué se construye y por qué
    plan.md               ← cómo se construye (tech)
    data-model.md         ← estructura de datos
    tasks.md              ← lista de tareas
    research.md           ← investigación del stack

CLAUDE.md                 ← generado automáticamente
```

---

## Instalación

Requiere `uv` (gestor de paquetes Python moderno):

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
specify init mi-proyecto --integration copilot
```

Funciona con Claude Code, GitHub Copilot y más de 30 agentes de AI.

---

## Principio clave

Spec-kit separa el **QUÉ** del **CÓMO**:

- `/speckit.specify` → solo el qué (requisitos, user stories)
- `/speckit.plan` → solo el cómo (stack, arquitectura)
- `/speckit.implement` → ejecución

Esta separación evita que el AI tome decisiones técnicas antes de que
tú hayas pensado en los requisitos.

---

## Diferencia con CLAUDE.md

CLAUDE.md es un concepto separado (documentación de contexto para Claude Code).
Spec-kit lo genera automáticamente durante `/speckit.plan`, pero son herramientas
distintas con propósitos distintos.
