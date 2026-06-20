# PROMPT 3 — Plan

**Comando:** `/speckit.plan`

El **cómo** técnico. La constitución revisa todo lo que pongamos: el plan
tiene que respetar las convenciones del proyecto sí o sí.

---

```
/speckit.plan

Diseña la implementación técnica de [nombre de la feature] respetando
ESTRICTAMENTE la constitución del proyecto y la arquitectura existente:

- [Nuevo modelo o entidad]: campos, relaciones, cascadas.
- [Nuevo repositorio]: siguiendo EXACTAMENTE el mismo estilo que los
  repositorios existentes. Toda la lógica de persistencia aquí.
- [Nuevas vistas o endpoints]: la vista NO toca la base de datos.
  Valida, llama al repositorio y serializa, igual que el resto.
- [Registro]: dónde y cómo registrar los nuevos componentes siguiendo
  el patrón actual.
- Plan de tests: qué archivos de test crear y qué cubrir.

No cambies modelos, vistas ni tests existentes. Antes de dar el plan
por bueno, revisa que no haya nada sobre-diseñado.
```

---

**Resultado:** `specs/###-nombre-feature/plan.md`

Al planificar, el agente vuelve a comprobar la constitución. Él solito respeta
las convenciones porque se las hemos escrito como ley.

## Quality gate siguiente: `/speckit.analyze`

Antes de continuar, ejecuta `/speckit.analyze`. Hace una **validación cruzada**
entre la constitución, el spec y el plan para detectar incoherencias.
Muy recomendable en proyectos en producción.
