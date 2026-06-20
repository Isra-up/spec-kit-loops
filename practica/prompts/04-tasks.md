# PROMPT 4 — Tasks

**Comando:** `/speckit.tasks`

Convierte el plan en tareas concretas y ordenadas. No lleva un prompt extenso:
el propio comando descompone el plan en tareas pequeñas, ordenadas por
dependencias y testeables de forma independiente.

---

```
/speckit.tasks
```

---

**Resultado:** `specs/###-nombre-feature/tasks.md` con la lista ordenada
respetando las capas del proyecto:

1. Modelos y relaciones
2. Repositorios con type hints
3. Vistas y registro
4. Tests (endpoints + repositorios)

Cada tarea es lo bastante pequeña para que el agente la ejecute sin perderse.
Las que se pueden ejecutar en paralelo van marcadas con `[P]`.
