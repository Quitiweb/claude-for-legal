---
name: derechos-interesado
description: >
  Tramita un ejercicio de derechos del interesado (acceso, rectificación, supresión, oposición,
  limitación, portabilidad, decisiones automatizadas) y redacta la respuesta — verifica
  identidad, localiza datos sistema por sistema, evalúa excepciones, redacta el acuse de recibo
  y la respuesta sustantiva. Úsala cuando llegue una solicitud, el usuario pegue un escrito de
  ejercicio de derechos, o diga "ha llegado un ejercicio de derechos", "solicitud de acceso",
  "derecho al olvido", o "alguien quiere sus datos".
argument-hint: "[pega la solicitud, o descríbela]"
---

# /derechos-interesado

1. Carga `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md` → Proceso de
   ejercicio de derechos (lista de sistemas, método de verificación, plazo).
2. Ejecuta el flujo.
3. Clasifica el derecho. Comprueba disparadores de escalado — si saltan, encamina antes de seguir.
4. Recorre: verificar identidad → barrer la lista de sistemas → análisis de excepciones → borrador.
5. Salida: borrador de respuesta. NO enviar — una persona revisa y envía.
6. Registra el ejercicio según el proceso de la casa.

**Antes de pegar la solicitud:** contendrá datos personales del interesado. Confirma que tu
sesión y el almacenamiento de salidas cumplen tus requisitos de tratamiento. Redacta/oculta lo
que no necesites (adjuntos de identidad, hilos no relacionados). No pongas el nombre del
interesado en nombres de archivo.

```
/proteccion-datos:derechos-interesado
[pega el correo de la solicitud]
```

---

# Respuesta al ejercicio de derechos

## Contexto de asunto

Comprueba `## Espacios de asunto`. Si `Activado` es `✗` (por defecto en asesoría interna), omite
este párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Propósito

Un ejercicio de derechos tiene un **plazo** (1 mes, prorrogable 2 — art. 12.3 RGPD), un proceso
(verificar, localizar, evaluar excepciones, responder) y muchos sitios donde puede torcerse. Esta
skill recorre cada paso y redacta la respuesta.

## Supuesto de jurisdicción

Asume el ámbito de tu configuración (RGPD + LOPDGDD). Si el interesado, el tratamiento o el
responsable están en otra jurisdicción, puede no aplicar tal cual.

## Carga el proceso

Lee `## Proceso de ejercicio de derechos`: lista de sistemas (todos los sitios donde viven los
datos), método de verificación de identidad, plazo, quién tramita lo rutinario y quién recibe lo
escalado. Si la lista de sistemas está vacía o desactualizada, márcalo — no se puede completar un
ejercicio de derechos sin saber dónde mirar.

## Flujo

### Paso 1: Clasifica la solicitud

Identifica qué derecho se invoca:
- **Acceso (art. 15)** — copia de sus datos + información sobre el tratamiento.
- **Rectificación (art. 16)** — corregir datos inexactos o incompletos.
- **Supresión / "derecho al olvido" (art. 17)** — eliminar sus datos (con excepciones).
- **Limitación (art. 18)** — pausar el tratamiento mientras se dirime una disputa.
- **Portabilidad (art. 20)** — sus datos en formato estructurado, de uso común y lectura mecánica.
- **Oposición (art. 21)** — oponerse a un tratamiento (a menudo marketing/perfilado).
- **Decisiones automatizadas (art. 22)** — no ser objeto de decisión basada solo en tratamiento
  automatizado con efectos jurídicos o significativos.

Algunas son combinaciones — "dad de baja mi cuenta y enviadme antes mis datos" = supresión +
portabilidad. Trátalas como solicitudes enlazadas.

**Investiga la regla aplicable.** Cita el artículo del RGPD y, en su caso, de la LOPDGDD, con
referencia precisa: alcance del derecho, excepciones, plazos. Nota la vigencia y marca la
incertidumbre.

> **Sin suplir en silencio.** Si la fuente de investigación devuelve poco para los derechos,
> excepciones o plazos, informa y para.
> **Niveles de etiqueta de fuente:** `[asentado — última comprobación AAAA-MM-DD]` para
> referencias estables comprobadas (p. ej., art. 12.3 RGPD plazo de 1 mes); `[verificar]` para
> citas de conocimiento del modelo reales que conviene verificar (criterios de la AEPD,
> resoluciones, desarrollos de la LOPDGDD); `[verificar-preciso]` para citas con apartado/letra
> concretos (máximo riesgo de fabricación, verificar siempre). Lo recuperado mantiene su fuente
> (`[BOE]`/`[EUR-Lex]`/`[AEPD]`); web sigue `[web — verificar]`; lo del usuario `[aportado por el
> usuario]`. Nunca quites ni colapses las etiquetas.

### Paso 2: Verifica identidad

Según el método del config (art. 12.6 RGPD). Enfoques habituales: verificación en sesión
autenticada; coincidencia de correo en ficha (suele bastar en solicitudes de bajo riesgo);
verificación adicional para cuentas sensibles o supresiones (pregunta de control, etc.).

**Calibra al riesgo.** Sobre-verificar convierte el proceso en una barrera (mala imagen ante la
AEPD). Infra-verificar arriesga entregar datos de otra persona a un impostor. La verificación de
identidad debe ser **proporcionada** y no excesiva (no pidas un DNI completo si basta menos).

Si no se puede verificar:
```markdown
No hemos podido verificar que esta solicitud provenga de la persona cuyos datos se refieren. Para
continuar, le pedimos [paso de verificación]. No podemos facilitar datos personales en respuesta a
una solicitud que no podemos verificar.
```
Esto detiene (discutiblemente) el cómputo, pero no te sientes encima: responde para pedir la
verificación en pocos días, no el día 29.

### Paso 3: Localiza los datos

Recorre la lista de sistemas del config. Por cada sistema:

| Sistema | ¿Consultado? | ¿Datos? | Qué |
|---|---|---|---|
| Base de datos de producción | | | |
| Analítica (p. ej., Matomo, Amplitude) | | | |
| Tickets de soporte (p. ej., Zendesk) | | | |
| CRM (p. ej., Salesforce, HubSpot) | | | |
| Email marketing | | | |
| Logs | | | |
| Copias de seguridad | | | (a menudo exentas de supresión inmediata — ver abajo) |
| Encargados (terceros) | | | (pueden necesitar instrucción para suprimir) |

Si eres **encargado** en un sector B2B: el "interesado" suele ser el usuario final *de tu
cliente*. Comprueba si en realidad es un ejercicio que debe tramitar tu cliente (responsable). El
art. 28 suele exigir que el encargado **asista** al responsable y le **traslade** las solicitudes.

### Paso 4: Análisis de excepciones

No todo se entrega o suprime. **Investiga la regla aplicable.** Para cada elemento, identifica
toda excepción/limitación con base de buena fe (p. ej., derechos de terceros, secreto profesional,
secretos comerciales, seguridad, obligación legal de conservar, ejercicio o defensa de
reclamaciones, conservación por copias de seguridad). El **art. 23 RGPD** y los **arts. específicos
de la LOPDGDD** (y normativa sectorial: fiscal, mercantil, blanqueo, laboral) modulan estos límites.
Cita norma con referencia precisa; verifica vigencia.

**No reduzcas la lista por un juicio subjetivo.** La skill propone excepciones con base de buena
fe y marca las inciertas; el abogado reduce la lista antes de enviar. Soltar una excepción que
luego aplicaba es costoso (una vez revelado, la excepción desaparece de hecho). Sobre-afirmar una
excepción plausible lo corrige el abogado en revisión. Prefiere el error recuperable. Cada
excepción propuesta lleva: **"propuesta — requiere revisión letrada antes de invocarla. La AEPD
escruta las excepciones en bloque; el abogado reduce esta lista, la skill no."**

Preguntas recurrentes: ¿el registro contiene datos de *otras personas* que hay que ocultar antes
de entregar? ¿Hay obligación legal de conservación que bloquee la supresión (fiscal, mercantil,
prevención de blanqueo)? Cítala. ¿Hay un litigio en curso? **Documenta toda excepción invocada** —
si la AEPD pregunta por qué no suprimiste algo, "teníamos obligación legal" necesita una cita.

### Paso 5: Redacta la respuesta — DOS CARTAS

> **Comprobación previa de conector.** Antes de emitir cualquier carta o el análisis interno de
> excepciones, comprueba si una fuente de investigación (BOE/EUR-Lex/AEPD o un MCP configurado)
> responde. Recógelo en la nota de revisión, que va en el **análisis interno**, NO en las cartas
> al interesado. Si no hay conector, regístralo en la línea **Fuentes:** — las excepciones, los
> plazos y los mecanismos de prórroga son especialmente propensos a fabricación; verifícalos
> antes de invocar cualquier excepción ante un interesado o la AEPD.

El RGPD espera un **acuse de recibo** pronto, separado de la respuesta sustantiva. Produce ambos;
no los fundas en una sola carta que espera al día 30.

- **Paso 5a — Acuse de recibo.** En días desde la recepción (objetivo: mismo día a 3-5 días,
  siempre dentro del plazo). Confirma recepción, reformula la solicitud, indica el plazo y la
  fecha objetivo, y pide la verificación de identidad pendiente. NO contiene la entrega
  sustantiva.
- **Paso 5b — Respuesta sustantiva.** La entrega, confirmación de supresión o exportación de
  portabilidad. Sale en plazo, solo tras verificar identidad y completar localización + excepciones.

**Antes de enviar cualquiera de las dos cartas al interesado:** lee `## Quién usa esto`. Si el Rol
es No jurista:

> Enviar una respuesta de ejercicio de derechos tiene consecuencias jurídicas — el contenido, las
> excepciones invocadas y las omisiones son revisables por la AEPD. ¿Lo has revisado con un
> abogado/a? Si sí, adelante. Si no, aquí tienes un resumen de una página: [interesado, derecho
> invocado, qué se localizó en cada sistema, qué se retiene y bajo qué excepción, situación de la
> verificación, plazo, y las tres cosas que preguntar antes de enviar].
>
> Para encontrar abogado/a: el Servicio de Orientación Jurídica (SOJ) de tu Colegio de la
> Abogacía es el punto de partida más rápido.

No pases esta puerta sin un sí explícito.

> **Nota:** ambas cartas son entregables externos al interesado. **No** incluyas la cabecera de
> documento de trabajo. Las notas, logs y análisis de excepciones internos sí la llevan (según
> `## Salidas`).
>
> **Antes de enviar:** es un borrador para revisión letrada, no una respuesta para enviar. Enviar
> compromete una postura, puede perder excepciones y puede iniciar el reloj de la autoridad. Un
> abogado/a revisa, edita y aprueba antes de que cualquier carta llegue al interesado.

#### Paso 5a — Plantilla de acuse de recibo

```markdown
Asunto: Hemos recibido su solicitud de derechos — [Empresa] — [fecha]

Estimado/a [Nombre]:

Hemos recibido su solicitud de [acceso / supresión / portabilidad / rectificación / oposición /
limitación] el [fecha de recepción].

**Su solicitud, según la entendemos:** [reformulación en una frase].

**Qué ocurre ahora:**
- Nuestra fecha objetivo para la respuesta sustantiva es [fecha — a más tardar 1 mes desde la
  recepción; SLA interno si es más estricto]. [Si falta verificación: "Necesitamos [paso] antes de
  continuar — ver abajo."]
- Si necesitamos más tiempo por complejidad o número de solicitudes, se lo comunicaremos antes de
  que venza el plazo inicial, explicando los motivos, y podremos prorrogarlo dos meses adicionales
  (art. 12.3 RGPD).
- Esta solicitud no conlleva coste, salvo que resulte manifiestamente infundada o excesiva
  (art. 12.5 RGPD).

[Si falta verificación de identidad:]
**Para verificar su identidad,** le pedimos [paso concreto y proporcionado]. Trabajamos en paralelo.

Si tiene dudas, contacte con [contacto de privacidad / DPD].

[Remitente]
```

**Regla de inicio del cómputo.** El plazo empieza con la recepción de la solicitud, no con la
verificación de identidad — salvo que la normativa o el criterio de la AEPD digan otra cosa para
el caso. No detengas tácitamente el reloj por la verificación.

#### Paso 5b — Plantillas de respuesta sustantiva

**Respuesta a solicitud de acceso (art. 15):**
```markdown
Asunto: Su solicitud de acceso a datos — [Empresa] — [fecha]

Recibimos su solicitud el [fecha] de una copia de los datos personales que tratamos sobre usted.

**Qué hemos encontrado:** tratamos las siguientes categorías asociadas a [identificador]:

| Categoría | Origen | Finalidad | Base (art. 6) | Conservación |
|---|---|---|---|---|
| [Datos de cuenta: nombre, correo] | Usted, al registrarse | Gestión de la cuenta | Contrato | Hasta la baja |
| [Datos de uso] | Nuestro servicio | Analítica, mejora | Interés legítimo | [plazo] |

**Sus datos se adjuntan** en [formato]. [Entrega segura — archivo cifrado, enlace con caducidad.]
**Encargados/cesiones:** compartimos datos con [lista o enlace a la página de subencargados].
**Sus demás derechos:** puede solicitar [rectificación / supresión / portabilidad / limitación /
oposición]. Para ello, [método]. Puede además **reclamar ante la AEPD** (www.aepd.es).
**Datos no incluidos:**
- [Categoría] — [excepción y motivo, p. ej., "logs de seguridad — su revelación comprometería las
  medidas de seguridad"].
- [Se han ocultado datos de terceros en la correspondencia de soporte.]
```

**Respuesta a solicitud de supresión (art. 17):**
```markdown
Asunto: Su solicitud de supresión — [Empresa] — [fecha]

Recibimos su solicitud el [fecha] de suprimir los datos personales que tratamos sobre usted.

**Qué hemos suprimido:**
| Categoría | Sistema | Suprimido el |
|---|---|---|
| [Cuenta y perfil] | Producción | [fecha] |

**Qué hemos conservado y por qué:**
| Categoría | Motivo | Conservado hasta |
|---|---|---|
| [Registros de facturación] | Obligación legal (conservación fiscal/mercantil, [cita]) | [fecha] |
| [Copias de seguridad] | Se suprimirán en la próxima rotación | [fecha] |

**Encargados:** hemos instruido a [lista] para suprimir sus datos de sus sistemas.
Su cuenta está cerrada. Puede reclamar ante la AEPD si no está conforme. Dudas: [contacto].
```

### Paso 6: Regístralo

Los ejercicios de derechos se auditan. Registra: fecha de recepción, fecha de verificación, fecha
de respuesta, qué se entregó/suprimió, excepciones invocadas y base, y quién lo tramitó. Si usáis
una herramienta de seguimiento, crea el registro allí; si no, un archivo de log sirve.

## Disparadores de escalado

Según `## Escalado`, escala cuando: el solicitante sea (o pueda ser) parte contraria, abogado de la
contraria o periodista; el alcance sea inusual ("todos los datos, incluidas comunicaciones internas
sobre mí"); haya un litigio sobre los datos de esa persona (supresión + litigio = conflicto, decide
el abogado); el solicitante dispute una respuesta anterior; o se mencione/copie a la AEPD.

## Gestión del plazo

**Regla de las dos cartas.** Todo ejercicio produce un acuse (pronto — objetivo mismo día a 3-5
días) Y una respuesta sustantiva (en plazo). Una sola carta combinada enviada el día 30 es un fallo
de proceso aunque sea correcta de fondo.

**Plazo:** 1 mes desde la recepción; prórroga de 2 meses adicionales por complejidad o número de
solicitudes, informando al interesado dentro del primer mes (art. 12.3). Si el config registra un
SLA interno más estricto, úsalo y anota el respaldo legal. Si vas a necesitar prórroga, comunícala
bien antes del primer vencimiento.

## Lo que esta skill NO hace

- No consulta los sistemas directamente. Te guía por el checklist; una persona (o una herramienta
  conectada) hace las consultas.
- No decide excepciones en casos límite. Las marca para el abogado.
- No envía la respuesta. Redacta, revisa, una persona envía.
