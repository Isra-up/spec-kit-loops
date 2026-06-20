# PROMPT 5 — Implement

**Comando:** `/speckit.implement`

El agente implementa todas las tareas en orden, respetando la constitución
en cada paso.

---

```
/speckit.implement
```

---

El agente ejecuta tarea por tarea dentro de los raíles de la constitución.
Cuando termina, ejecuta la suite de tests y confirma que los tests originales
siguen pasando y que los nuevos también pasan.

## Verificación — el momento que lo cambia todo

```bash
# 1. Todos los tests: los originales + los nuevos, todos en verde
pytest -q

# 2. ¿Las vistas siguen sin tocar la base de datos? Debe seguir siendo 0
grep -rn "db.session" app/views/*.py | wc -l

# 3. Ver qué ha cambiado
git diff --stat
```

**Resultado esperado:**

- Los tests originales en verde, sin tocar ni uno.
- Los tests nuevos también en verde.
- Las vistas nuevas respetan el patrón de arquitectura.

---

Esto es la diferencia entre soltar "hazme X" al agente y rezar,
y darle un marco donde solo puede hacerlo bien.
