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

### CLAUDE.md minimalista

El patrón recomendado es mantener CLAUDE.md corto y hacer que apunte a los
artefactos de la spec, en lugar de duplicar su contenido:

```markdown
<!-- SPECKIT START -->
For additional context, read the current plan:
`specs/001-nombre-feature/plan.md`

Supporting artifacts:
- Spec: `specs/001-nombre-feature/spec.md`
- Data model: `specs/001-nombre-feature/data-model.md`
- Constitution: `.specify/memory/constitution.md`
<!-- SPECKIT END -->
```

La regla: si algo ya está escrito en la spec, no lo copies en CLAUDE.md.
Un CLAUDE.md que crece indefinidamente es síntoma de que la spec no existe.

---

## SDD en producción

Fuente: https://github.com/JoaquinRuiz/spec-driven-development-produccion

### El escenario

Tienes un proyecto que ya está en producción — con tests, usuarios y convenciones
establecidas — y quieres añadir features con AI sin romper nada. No hace falta
reescribir ni migrar: SDD se mete dentro del proyecto existente.

```bash
specify init --here --integration claude
# si el directorio da problemas por no estar vacío:
specify init --here --integration claude --force
# (tener el proyecto en git antes de usar --force)
```

La diferencia con un proyecto nuevo es un solo flag: `--here`.

---

### La constitución fotografía, no inventa

En un proyecto existente, la constitución no crea reglas nuevas:
**documenta como ley las convenciones que el proyecto ya sigue**.

El agente lee el código antes de escribir nada. Si el prompt dice "57 tests"
pero el proyecto tiene 98, el agente corrige el dato — no se fía del input
humano, se fía del código real.

> La constitución son los raíles. El agente solo puede moverse dentro de ellos.

---

### Constitución versionada

Una constitución seria tiene versión semántica, fecha de ratificación y un
proceso explícito para enmiendas. No es una lista de preferencias — es un
contrato que puede evolucionar de forma controlada.

**Versión semántica:**

| Tipo de cambio | Cuándo |
|---|---|
| **MAJOR** | Eliminación o redefinición incompatible de un principio |
| **MINOR** | Añadir un principio nuevo o ampliar materialmente la guía |
| **PATCH** | Aclaraciones, correcciones de redacción sin cambio semántico |

**Proceso de enmienda:**
1. Rediseñar la propuesta para cumplir el principio existente
2. Si no es posible, documentar la justificación y proponer una enmienda
3. La propuesta **no se implementa** mientras contradiga un principio vigente
4. Las excepciones temporales se registran con alcance y fecha de revisión — no existen excepciones implícitas

**Cabecera mínima de una constitución versionada:**

```markdown
# Nombre del Proyecto — Constitución

[principios...]

**Version**: 1.0.0 | **Ratified**: YYYY-MM-DD | **Last Amended**: YYYY-MM-DD
```

---

### Quality gates

Dos comandos de validación que conviene ejecutar antes de avanzar de fase:

| Comando | Cuándo usarlo | Qué detecta |
|---|---|---|
| `/speckit.clarify` | Después de `/speckit.specify` | Ambigüedades en requisitos antes de planear |
| `/speckit.analyze` | Después de `/speckit.plan` | Incoherencias entre constitución, spec y plan |

Ejecutarlos evita que el AI planifique o implemente sobre suposiciones incorrectas.

**Las clarifications quedan en la spec.**
Las preguntas que hace el agente y sus respuestas se registran en `spec.md`
como decisiones de negocio versionadas — no desaparecen al cerrar el chat:

```markdown
## Clarifications

### Session YYYY-MM-DD

- Q: ¿Cómo calcular el umbral de rebalanceo? → A: Desviación absoluta en
  puntos porcentuales (por defecto 5 p.p., configurable).
- Q: ¿Qué ventana histórica usar? → A: 10 años; si faltan datos, informar
  la limitación, no inventar valores.
```

Esto convierte las ambigüedades resueltas en parte permanente de la documentación.

---

### Flujo completo para proyecto existente

```bash
# Instalar spec-kit en el proyecto
specify init --here --integration claude

# Dentro de Claude Code, en orden:
/speckit.constitution   # fotografiar las reglas actuales
/speckit.specify        # el qué y el porqué (sin detalles técnicos)
/speckit.clarify        # quality gate: resolver ambigüedades
/speckit.plan           # el cómo, respetando la constitución
/speckit.analyze        # quality gate: validación cruzada
/speckit.tasks          # tareas ordenadas por dependencias
/speckit.implement      # ejecución

# Verificar que nada se rompió
pytest -q
```

---

### Verificación post-implementación

Tres comprobaciones que confirman que el agente respetó las convenciones:

```bash
# 1. Todos los tests (originales + nuevos) en verde
pytest -q

# 2. Las vistas no tocan la base de datos (patrón Repository)
grep -rn "db.session" app/views/*.py | wc -l   # debe ser 0

# 3. Ver exactamente qué cambió
git diff --stat
```

Esto es la diferencia entre decirle al agente "hazme X" y rezar,
y darle un marco donde solo puede hacerlo bien.

---

## Degradación limpia

Fuente: https://github.com/JoaquinRuiz/roboadvisor-sdd

Cuando el sistema depende de un componente no crítico (un LLM, una API externa,
un servicio de caché), diseñarlo para que funcione sin ese componente es parte
de la constitución, no una decisión de implementación de último minuto.

**El patrón:**
- La capa determinista (lógica de negocio, cálculos, decisiones) funciona siempre
- La capa de mejora (LLM, explicaciones, enriquecimiento) puede fallar
- El sistema muestra lo que tiene, no un error

**Ejemplo aplicado:**
```
Sin Ollama disponible:
  ✓ Muestra la cartera y las métricas
  ✗ No muestra la explicación en lenguaje natural
  → Nunca bloquea al usuario
```

**Cómo incluirlo en la constitución:**

```markdown
### Degradación limpia (MUST)
Si [componente X] no está disponible, el sistema MUST seguir funcionando
mostrando [funcionalidad core]. Solo [funcionalidad de mejora] queda
deshabilitada. Nunca se muestra un error genérico al usuario.
```

Este principio va en la constitución, no en el plan técnico, porque es
una decisión de negocio: ¿qué le prometemos al usuario cuando algo falla?
