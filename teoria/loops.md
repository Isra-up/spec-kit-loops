# Loops — Teoría

Fuente: "How to Create Loops with Claude" (Boris Cherny, Peter Steinberger, Addy Osmani)

## La idea central

> Un prompt te da una respuesta. Un loop te da un sistema que sigue
> trabajando después de que cierras la laptop.

Boris Cherny (director de Claude Code en Anthropic) ya no escribe prompts —
tiene loops corriendo que escriben prompts por él y deciden qué hacer.
Su trabajo es **diseñar loops**, no escribir mensajes.

---

## ¿Qué es un loop?

Un loop es un **objetivo recursivo**:
- Defines un propósito
- El agente itera contra ese propósito
- El loop sigue corriendo hasta que se cumple una condición real de parada

El agente olvida todo entre corridas. **El loop no olvida.** Ese único
hecho es toda la arquitectura.

---

## Las 6 partes de un loop (Addy Osmani)

Todo loop funcional es una combinación de estos seis componentes:

| Componente | ¿Qué es? |
|---|---|
| **Automations** | Lo que hace que sea un loop y no una ejecución única: un cron job, un webhook, un hook en Claude Code que dispara sin que escribas nada |
| **Worktrees** | Copias aisladas del repo para que agentes paralelos no se pisen entre sí |
| **Skills** | Manuales de procedimiento que el agente lee en lugar de que le expliques desde cero cada vez |
| **Connectors** | Integraciones con servicios externos (GitHub, Slack, CI, etc.) |
| **Sub-agents** | Agentes especializados que el loop principal delega |
| **Memory** | Un archivo de estado en disco (markdown) que sobrevive entre corridas |

---

## El archivo de memoria (PROGRESS.md / STATE.md)

El archivo de memoria es la pieza más importante de cualquier loop.

- Al inicio de cada corrida, el agente lo lee primero
- Al final de cada corrida, escribe qué pasó y qué sigue
- Sin él, cada corrida empieza desde cero sin importar cuántas corridas
  hubo antes

### Estructura recomendada

```markdown
## Última corrida
[qué se hizo]

## En progreso
[qué está a medias]

## Bloqueado
[qué no avanzó y por qué]

## Siguiente paso
[qué debe hacer el agente en la próxima corrida]
```

Mantenlo corto — un archivo de 2000 líneas es peor que no tener archivo.

---

## Separar al escritor del revisor

El modelo que escribió el código es demasiado benevolente al calificar
su propio trabajo. Un solo agente que escribe y luego se autoevalúa
marcará su trabajo como "listo" más veces de las que debería.

La solución es el **patrón evaluador-optimizador**:
1. Agente A genera
2. Agente B critica contra un estándar objetivo (tests, build, linter)
3. El loop repite hasta que el chequeo pase

El verificador necesita una condición dura, no una opinión.
"El test suite pasa" es una condición real. "El agente dice que terminó" no lo es.

---

## Condición de parada real

Un loop sin condición de parada real falla silenciosamente
(el "Ralph Wiggum loop": el agente emite una señal de completado antes
de terminar y el loop sale creyendo que el trabajo está hecho).

Condiciones reales de parada:
- El test suite pasa
- El build tiene éxito
- El ticket se mueve a Done con CI en verde

Siempre agrega un **máximo de iteraciones** como respaldo (10-20 es razonable).
Si el loop llega al techo sin cumplir la condición real, debe detenerse
y marcar para revisión humana.

---

## La escalera de autonomía (Boris Cherny)

No todos los loops deben correr desatendidos desde el primer día:

| Nivel | Comportamiento |
|---|---|
| **1** | Solo sugiere — el humano decide todo |
| **2** | Genera borradores — el humano los aplica |
| **3** | Aplica cambios de bajo riesgo — requiere aprobación antes de publicar |
| **4** | Aplica y completa automáticamente con logs de auditoría |

Empieza todo loop nuevo en nivel 1 o 2. Súbelo de nivel solo cuando
el loop produzca consistentemente trabajo que aprobarías sin cambios.
**El nivel 4 se gana, no se asume.**

---

## ¿Necesitas un loop? El test de 4 preguntas

Fuente: Anatoli Kopadze — "Loops explained: Claude, GPT, Mira and what actually works"

Antes de construir un loop, verifica que las 4 sean verdad. Si falla una, quédate con un buen prompt manual:

| Pregunta | Si la respuesta es NO… |
|---|---|
| ¿La tarea se repite al menos semanalmente? | El costo de setup nunca se amortiza |
| ¿Algo puede rechazar automáticamente el output malo? | El loop solo da vueltas sin avanzar |
| ¿El agente puede completar el trabajo de principio a fin solo? | Te devuelve la mitad a ti de todas formas |
| ¿"Terminado" es objetivo, no un juicio de valor? | Un humano sigue ganando en ese paso |

---

## La métrica que importa: costo por cambio aceptado

No midas tokens gastados ni loops ejecutados. Mide cuántos resultados del loop **aceptas sin modificar**.

> Si el loop te da 10 resultados y tiras 6, estás haciendo tú el trabajo que el loop debía ahorrarte.

**Por debajo del 50% de tasa de aceptación, el loop cuesta más de lo que da.**

Úsalo como semáforo antes de escalar:
- ≥ 80% → el loop está listo para subir de nivel de autonomía
- 50–80% → ajusta el verificador o los criterios de éxito
- < 50% → detén el loop y rediseña; no lo pongas en schedule todavía

---

## El orden correcto para construir un loop

Skippear pasos es exactamente cómo los loops se queman mientras duermes:

```
1. Haz UNA corrida manual confiable primero
2. Conviértela en una skill (guarda las instrucciones en un archivo)
3. Envuelve la skill en un loop (agrega el gate + condición de parada)
4. ENTONCES ponlo en un schedule
```

Primero prueba. Luego endurece. Luego automatiza.

---

## Costos de tokens

Un loop corriendo desatendido toda la noche puede generar una factura alta.

Antes de dejar correr cualquier loop sin supervisión:
1. Córrelo manualmente 3-5 iteraciones y mide tokens por iteración
2. Multiplica por el máximo de iteraciones = costo máximo por corrida
3. Multiplica por frecuencia del cron = costo diario máximo

El patrón escritor+revisor que sube la calidad también **duplica el costo**, porque dos modelos leen el trabajo en lugar de uno.

Además: construye una **allowlist de comandos** para loops que ejecutan
shell. Restringe solo a lo que la tarea necesita (`npm`, `git`, `ls`, `cat`).
Un agente con acceso shell irrestricto en un loop desatendido convierte
un problema de costos en un problema de seguridad.

---

## Cómo empieza un loop

Empieza con **un solo trigger**:

```
"Cada mañana a las 8am, lee los fallos de CI de ayer,
los issues abiertos y los commits recientes,
y escribe los hallazgos en un archivo markdown."
```

Esa única automatización ya es más leverage que cien prompts bien redactados,
porque corre sin ti.

No intentes construir el sistema de seis partes desde el día uno.

---

## Cómo escalan los loops

El segundo loop se conecta al primero:

- Loop 1 (diario): encuentra trabajo y escribe hallazgos al STATE.md
- Loop 2 (diario): lee el STATE.md y ejecuta el item de mayor prioridad

Ninguno necesita al otro para funcionar, pero juntos forman un pipeline
que mueve trabajo de "descubierto" a "en progreso" sin que toques ninguno.

---

## El cambio de trabajo que produce

Una vez que tienes loops corriendo, tu trabajo cambia:

- **Antes:** abres un chat y escribes una pregunta
- **Después:** abres una bandeja de entrada de triage y revisas lo que
  los loops encontraron durante la noche

Dejas de decidir a nivel de tarea. Decides a nivel de diseño de loop.
Los loops escriben los prompts por ti. Tu atención se mueve a las partes
que necesitan un humano: el checkpoint de revisión, la condición de parada,
y el próximo loop que vale la pena construir.
