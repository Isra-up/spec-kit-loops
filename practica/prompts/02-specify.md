# PROMPT 2 — Specify

**Comando:** `/speckit.specify`

El **qué** y el **porqué** de la feature. Sin detalles técnicos: nada de modelos,
nada de base de datos. Solo el comportamiento esperado y las reglas de negocio.

---

```
/speckit.specify

[Nombre de la feature]

Contexto de negocio: [por qué los usuarios necesitan esto, qué problema resuelve].

Comportamiento esperado:

- [Acción que puede hacer el usuario 1]
- [Acción que puede hacer el usuario 2]
- [Acción que puede hacer el usuario 3]

Reglas de negocio:

- [Restricción o validación 1]
- [Restricción o validación 2]
- [Comportamiento en cascada o dependencia]
```

---

**Resultado:** `specs/###-nombre-feature/spec.md` con historias de usuario
y criterios de aceptación.

**Léelo siempre antes de continuar.** Aquí es donde se corrigen malentendidos
antes de que cuesten código.

## Quality gate siguiente: `/speckit.clarify`

Después de leer el spec, ejecuta `/speckit.clarify`. El agente pregunta sobre
zonas poco definidas:

- ¿Qué código HTTP devolver en cada caso de error?
- ¿Qué campos incluir en la respuesta JSON?
- ¿Cómo manejar casos límite?

Responde las preguntas, el agente refina el spec, y sigues con la cabeza tranquila.

**Las respuestas quedan registradas en `spec.md`** como sección `## Clarifications`,
versionadas junto al código. No desaparecen al cerrar el chat — son decisiones
de negocio permanentes:

```markdown
## Clarifications

### Session YYYY-MM-DD

- Q: [pregunta del agente] → A: [tu respuesta]
- Q: [pregunta del agente] → A: [tu respuesta]
```

Si en el futuro alguien pregunta "¿por qué se hizo así?", la respuesta está en la spec.
