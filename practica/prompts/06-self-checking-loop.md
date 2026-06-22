# Prompt 06 — Self-Checking Loop

Fuente: Anatoli Kopadze — "Loops explained: Claude, GPT, Mira and what actually works"

Un loop manual que puedes pegar en cualquier LLM sin infraestructura.
El modelo itera sobre su propio output hasta que cumple los criterios — en lugar
de entregarte lo primero que pareció cercano.

---

## Cuándo usarlo

- Cuando necesitas calidad alta en una sola sesión sin montar un loop automatizado
- Para probar si una tarea es buena candidata para un loop real antes de construirlo
- Redacción, análisis, diseño, cualquier tarea donde "terminado" tenga criterios claros

---

## El prompt (copia y pega)

```
Trabajarás en un loop hasta que la tarea cumpla el estándar.

TAREA:
[describe exactamente lo que quieres producido]

CRITERIOS DE ÉXITO (sé estricto, sin aprobaciones blandas):
- [criterio 1]
- [criterio 2]
- [criterio 3]

PROTOCOLO DE LOOP, repite cada turno:
1. PLAN   — enuncia el siguiente paso a dar.
2. HACER  — produce o mejora el trabajo.
3. VERIFICAR — puntúa el resultado del 1 al 10 por cada criterio.
               Sé brutalmente honesto. Lista exactamente qué sigue débil.
4. DECIDIR — si todos los criterios están en 8 o más, escribe "FINAL" y detente.
             Si no, escribe "ITERANDO" y continúa, corrigiendo primero
             el punto más débil del último VERIFICAR.

REGLAS:
- Nunca lo declares terminado hasta que cada criterio esté en 8 o más.
- Cada pasada debe corregir el puntaje más bajo del último VERIFICAR.
- No me hagas preguntas. Haz una suposición razonable, anótala y sigue.

Comienza. Corre el loop hasta FINAL.
```

---

## Cómo funciona

El modelo redacta → califica su propio trabajo contra tus criterios → encuentra
el punto débil → reescribe → repite hasta pasar la barra.

Lo que hace diferente a esto de un prompt normal:

| Prompt normal | Self-checking loop |
|---|---|
| Entrega lo primero que pareció bueno | Itera hasta cumplir el estándar |
| Tú decides si está listo | El criterio numérico decide |
| Una sola pasada | Múltiples pasadas con mejora dirigida |

---

## Limitación importante

Tú sigues siendo el trigger — abres el chat, pegas el prompt, esperas.
Cierra la pestaña y el loop desaparece.

Para un loop que corra solo, en schedule, sin que estés presente,
necesitas el stack completo: automation + skill + gate + schedule.
Ver `teoria/loops.md` → sección "El orden correcto para construir un loop".
