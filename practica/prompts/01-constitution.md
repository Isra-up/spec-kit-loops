# PROMPT 1 — Constitución

**Comando:** `/speckit.constitution`

Fotografiamos las reglas que el proyecto ya tiene de facto. No inventamos reglas
nuevas: documentamos como ley las convenciones existentes. El agente lee el
código antes de escribir nada.

> El agente no se fía de lo que le dices: lee el proyecto y corrige datos
> incorrectos antes de escribir la constitución. Eso es exactamente lo que queremos.

---

```
/speckit.constitution

Crea la constitución de este proyecto existente. No estamos inventando reglas
nuevas: estamos documentando como ley las convenciones que el proyecto ya sigue.
Analiza el código antes de escribir nada.

Principios NO negociables (MUST):

1. PATRÓN REPOSITORY OBLIGATORIO. Toda la persistencia vive en
   app/repositories/. Las vistas (app/views/) NUNCA importan ni usan
   db.session ni hacen queries directas. Una vista solo: recibe el
   request, valida la entrada, llama al repositorio y serializa la respuesta.

2. TYPE HINTS COMPLETOS en todos los métodos de los repositorios: parámetros
   tipados y return type explícito, usando Optional donde corresponda y los
   tipos de los modelos, nunca Any.

3. LOS TESTS EXISTENTES SON SAGRADOS. No se modifican bajo ningún concepto.
   Cualquier cambio debe dejar los tests pasando exactamente igual.

4. NO ROMPER LA API PÚBLICA. No se cambian las URLs existentes, ni los métodos
   HTTP, ni el formato de las respuestas JSON ya existentes. Las features
   nuevas añaden endpoints, no modifican los actuales.

5. STACK CONGELADO. No se añaden dependencias nuevas sin justificación explícita.

6. SERIALIZACIÓN COHERENTE. La serialización a JSON sigue el mismo patrón
   que el resto del proyecto.

7. TODA FEATURE NUEVA TRAE SUS TESTS. Tests de endpoint (vía HTTP) y, si crea
   un repositorio, tests unitarios del repositorio.

Escribe la constitución reflejando estos principios y la arquitectura real
que encuentres en el código.
```

---

**Resultado:** `.specify/memory/constitution.md`

A partir de aquí, cada vez que el agente planifique o implemente algo pasará por un
**Constitution Check**: verifica que lo que va a hacer no viola ninguno de estos
principios.

---

## Versionar la constitución

Añade esta cabecera al final del prompt para que el agente genere una
constitución con versión desde el primer día:

```
Incluye también gobernanza: cómo estos principios deben guiar las decisiones
técnicas, qué hacer cuando una propuesta entre en conflicto con un principio,
y cómo se versiona la constitución si necesita evolucionar. Termina el
documento con esta línea:

**Version**: 1.0.0 | **Ratified**: YYYY-MM-DD | **Last Amended**: YYYY-MM-DD
```

**Semver para la constitución:**
- `MAJOR` — eliminar o redefinir incompatiblemente un principio
- `MINOR` — añadir principio nuevo o ampliar materialmente la guía
- `PATCH` — aclaraciones y correcciones de redacción sin cambio semántico
