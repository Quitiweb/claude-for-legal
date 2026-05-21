---
name: escalado
description: >
  Encamina una incidencia contractual al aprobador correcto según la matriz de escalado del
  CLAUDE.md, y redacta la petición. Úsala cuando el usuario diga "quién tiene que aprobar esto",
  "escala esto", "¿necesita visto bueno de Dirección Jurídica?", "encamina esto para aprobación",
  o cuando otra skill encuentra una incidencia que supera la autoridad del revisor.
argument-hint: "[describe la incidencia, o referencia un memo de revisión]"
---

# /escalado

Nombra al aprobador de una incidencia según la matriz del CLAUDE.md y redacta el mensaje para que no
estés escribiendo "¿tienes un momento?" a las 19:00.

## Instrucciones

1. **Carga `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md`** → sección `## Escalado`.
   Si falta, dilo — el perfil necesita edición.
2. **Caracteriza la incidencia:** umbral económico / desviación de término / disparador automático /
   decisión de negocio.
3. **Empareja con la matriz y nombra al aprobador.** Sé específico — una persona o rol, no "dirección
   legal".
4. **Redacta la petición** según la plantilla: qué dice el contrato, qué dice el manual, opciones con
   recomendación, fecha de decisión.
5. **No envíes.** Redacta, muestra, deja que el abogado envíe.

## Ejemplos

```
/mercantil:escalado
El contrato marco de Acme tiene responsabilidad sin límite — ¿quién aprueba y qué digo?
```

---

## Contexto de asunto

Comprueba `## Espacios de asunto`. Si `Activado` es `✗` (por defecto en asesoría interna), omite este
párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Propósito

Todo equipo de contratos tiene una matriz de escalado, escrita o no. Esta skill lee la escrita (en el
CLAUDE.md), empareja una incidencia, nombra al aprobador y redacta la petición.

## Carga la matriz

**¿Qué lado?** Determina el lado del contrato cuya incidencia se escala — un término que está bien en
un lado puede ser un no rotundo en el otro. Lee la sección del manual del lado correspondiente y anota
el lado en la petición.

Lee `## Escalado`. Estructura esperada: quién puede aprobar, hasta qué umbral sin escalar, a quién
escala, por qué vía. Más los **disparadores automáticos** (escalan con independencia del importe):
típicamente responsabilidad sin límite, cesión de PI, cualquier cosa de una lista "nunca aceptamos".

## Flujo

### Paso 1: Caracteriza la incidencia
- **Umbral económico:** el valor supera la autoridad de alguien.
- **Desviación de término:** un término está fuera de las alternativas del manual.
- **Disparador automático:** está presente uno de los puntos de "escalar siempre".
- **Decisión de negocio:** no es una decisión jurídica — necesita al responsable de negocio.

No escales lo que en realidad está bien. Si el término está dentro de las alternativas del CLAUDE.md,
no necesita subir.

### Paso 2: Empareja con la matriz
```
¿Es un disparador automático? → SÍ: escala a [persona del disparador]
¿El valor supera el umbral del revisor? → SÍ: escala a quien tenga autoridad a ese nivel
¿La desviación está fuera de todas las alternativas? → SÍ: escala a quien apruebe no estándar
→ Si no: el revisor puede aprobar — sin escalado
```

### Paso 3: Nombra al aprobador
Sé específico. No "escalar a dirección legal" — nombra a la persona o rol del CLAUDE.md. Si la matriz
no cubre la situación: "La matriz no cubre [situación]. Sugiero preguntar a [Dirección Jurídica] que
es quien lo lleva."

### Paso 4: Redacta la petición
El aprobador debe poder decidir solo con el mensaje — sin "déjame abrir el contrato".
```markdown
**Escalando a:** [nombre]
**Vía:** [Slack #canal / correo / reunión — per CLAUDE.md]
**Urgencia:** [fecha límite si la hay]
---
Hola [nombre] —
Necesito tu decisión sobre el [contraparte] [tipo de acuerdo]. [Una frase de contexto.]
**La incidencia:** [lenguaje claro, un párrafo. Qué quieren, por qué se sale de nuestro estándar, cuál
es el riesgo real.]
**Qué dice el contrato:**
> "[cita exacta]"
**Qué dice nuestro manual:** [cita del CLAUDE.md]
**Opciones:**
1. **Aceptar** — [una línea de por qué podría estar bien]
2. **Rebatir con:** "[contra-lenguaje propuesto]" — [una línea de reacción probable]
3. **Retirarse** — [una línea de si es realista dado el contexto]
**Mi recomendación:** [qué opción y por qué, breve]
**Necesito decisión antes de:** [fecha, si la hay]
[Enlace al memo completo]
```

### Paso 5: Registra el escalado
Si el equipo usa un sistema de tickets o flujos del CLM, regístralo. Si no, anota en el memo que se
envió, a quién y cuándo.

## Calibración: ante la duda, escala con nota

El coste de un escalado innecesario son ~30 segundos del aprobador. El de un escalado omitido es
firmar un término no aprobado, una puerta de un solo sentido. Los costes no son simétricos. **Ante la
duda, escala.** La calibración vive en el CLAUDE.md, no en esta skill: claramente dentro de
alternativas → no escalar; claramente fuera o en la lista automática → escalar; incierto → escala con
la duda anotada explícitamente. No suprimas un escalado porque sobre-escalar pueda acostumbrar al
aprobador a leer en diagonal — eso lo resuelve el abogado ajustando umbrales en el manual.

## Lo que esta skill NO hace

- No aprueba nada. Encamina.
- No decide entre las opciones. La redacción incluye recomendación, pero decide el aprobador.
- No envía el mensaje — lo redacta. El abogado lo envía tras leerlo.
