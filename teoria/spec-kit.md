# Spec-kit — Teoría

## ¿Qué es?

Spec-kit es una forma de escribir **especificaciones estructuradas** que le dicen
a Claude Code exactamente qué construir, cómo comportarse y qué reglas seguir
dentro de un proyecto.

En lugar de darle instrucciones sueltas en cada conversación, defines el contexto
una sola vez y Claude lo respeta en todas las sesiones.

---

## Componente principal: CLAUDE.md

El archivo `CLAUDE.md` en la raíz del proyecto es el corazón de Spec-kit.
Claude Code lo lee automáticamente al iniciar cada sesión en ese directorio.

### ¿Qué va en CLAUDE.md?

- **Descripción del proyecto**: qué hace, para qué sirve
- **Stack tecnológico**: lenguajes, frameworks, herramientas
- **Convenciones de código**: estilo, nomenclatura, estructura de carpetas
- **Comandos frecuentes**: cómo correr tests, builds, deploys
- **Restricciones**: qué NO debe hacer Claude (ej. no modificar ciertos archivos)
- **Contexto del equipo**: quién es el usuario, nivel técnico, preferencias

### Ejemplo mínimo

```markdown
# Mi Proyecto

## Stack
- Python 3.11
- FastAPI
- PostgreSQL

## Comandos
- Correr servidor: `uvicorn main:app --reload`
- Tests: `pytest`

## Reglas
- Siempre usar type hints en funciones
- No modificar archivos en /legacy/
```

---

## Niveles de CLAUDE.md

Puedes tener varios archivos en distintas capas:

| Archivo | Alcance |
|---|---|
| `~/.claude/CLAUDE.md` | Global — aplica a todos tus proyectos |
| `/proyecto/CLAUDE.md` | Proyecto — aplica solo a este repo |
| `/proyecto/subcarpeta/CLAUDE.md` | Módulo — aplica solo a esa carpeta |

Claude los combina todos, del más general al más específico.

---

## Beneficios

- No repetir instrucciones en cada sesión
- Comportamiento consistente entre sesiones
- Funciona como documentación viva del proyecto
- Comparte reglas con el equipo (el archivo vive en git)

---

## Recursos para profundizar

- Documentación oficial de Claude Code: sección "CLAUDE.md"
- Skill `/init` en Claude Code: genera un CLAUDE.md automáticamente
  analizando tu proyecto
