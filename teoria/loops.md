# Loops — Teoría

## ¿Qué es?

Loops es una funcionalidad de Claude Code que permite ejecutar una tarea
o comando de forma **recurrente o iterativa**, ya sea en un intervalo
de tiempo fijo o dejando que Claude decida el ritmo.

---

## Tipos de loops

### 1. Loop con intervalo fijo

Ejecuta un comando cada N tiempo:

```
/loop 5m /sync-session
```

Esto correría `/sync-session` cada 5 minutos.

Intervalos válidos: `30s`, `5m`, `1h`, etc.

### 2. Loop sin intervalo (auto-pacing)

Claude decide cuándo hacer la siguiente iteración según el contexto:

```
/loop /sync-session
```

Útil cuando no sabes cada cuánto debe correr — Claude evalúa y decide.

### 3. Loop autónomo

Claude trabaja de forma independiente sin necesidad de que estés presente,
ejecutando tareas en segundo plano.

---

## ¿Para qué sirve?

- **Monitoreo**: revisar el estado de un build o deploy cada X minutos
- **Automatización**: ejecutar tareas repetitivas sin intervención manual
- **Babysitting de procesos**: vigilar que algo siga corriendo correctamente
- **Sincronización periódica**: como hacer `/sync-session` al final de cada
  bloque de trabajo

---

## Cómo funciona internamente

Cuando usas `/loop`, Claude Code:

1. Ejecuta la tarea indicada
2. Espera el intervalo definido (o calcula uno si no se especificó)
3. Repite desde el paso 1
4. Se detiene cuando el usuario interrumpe o cuando la tarea concluye

El loop persiste mientras la sesión esté activa.

---

## Loops programados (schedule)

Existe también `/schedule` para tareas que deben correr en momentos
específicos, no solo en loops continuos:

```
/schedule "todos los lunes a las 9am" /sync-session
```

Esto crea un agente en la nube que corre de forma autónoma según el cron.

---

## Diferencia entre /loop y /schedule

| | `/loop` | `/schedule` |
|---|---|---|
| Dónde corre | Sesión activa local | Nube (autónomo) |
| Requiere sesión abierta | Sí | No |
| Intervalo | Fijo o auto | Cron / fecha específica |
| Uso típico | Monitoreo en vivo | Tareas automáticas recurrentes |

---

## Buenas prácticas

- Usar loops para tareas **idempotentes** (que se pueden repetir sin problema)
- Evitar loops muy cortos (menos de 1 minuto) en tareas pesadas
- Combinar con skills: `/loop 1h /sync-session` para sincronizar cada hora
