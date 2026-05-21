---
name: evaluacion-impacto
description: >
  Genera una Evaluación de Impacto relativa a la Protección de Datos (EIPD) en formato propio
  para un tratamiento, función o producto nuevos, usando la estructura aprendida de tu EIPD
  semilla y el contenido mínimo del art. 35.7 RGPD. Úsala cuando el usuario diga "haz una
  EIPD", "evaluación de impacto de", "¿necesitamos EIPD para esto?", "revisa esta función desde
  privacidad", o describa un tratamiento nuevo.
argument-hint: "[nombre o descripción del tratamiento]"
---

# /evaluacion-impacto

1. Carga `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md` → Estilo de EIPD
   (disparador, estructura, profundidad, aprobación).
2. Ejecuta el flujo.
3. Comprueba: ¿hace falta EIPD de verdad? (Disparador interno + art. 35 RGPD + listas de la
   AEPD — cita fuentes, verifica vigencia.)
4. Intake: preguntas al equipo de producto. Puede tirar de un PRD si se aporta.
5. Escribe la EIPD en formato propio. Incluye comprobación de coherencia con la política.
6. Salida con lista de condiciones y responsables nombrados. Encamina a la aprobación.

```
/proteccion-datos:evaluacion-impacto "Función de compartir ubicación"
```

---

# Generación de EIPD

## Contexto de asunto

Comprueba `## Espacios de asunto` en el CLAUDE.md de práctica. Si `Activado` es `✗` (por defecto
en asesoría interna), omite este párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Comprobación de destino

Antes de producir la salida, comprueba a dónde va; ver `## Salvaguardas comunes → Comprobación
de destino` en el CLAUDE.md. Una EIPD interna lleva cabecera de confidencialidad; pero recuerda:
la EIPD es un documento que la normativa obliga a tener y que la AEPD puede requerir — no la
trates como blindada frente a la autoridad.

## Propósito

Una EIPD es una conversación con el equipo de producto, capturada. Pregunta: qué datos, por
qué, cuánto tiempo, quién los ve, qué puede salir mal. Esta skill estructura esa conversación y
la escribe en el formato de este equipo (el aprendido de la EIPD semilla) cumpliendo el
contenido mínimo del **art. 35.7 RGPD**.

## Supuesto de jurisdicción

Esta evaluación asume el ámbito de tu configuración (RGPD + LOPDGDD). Si el tratamiento, el
responsable o los interesados caen bajo otra jurisdicción, el análisis puede no aplicar tal cual.

## Carga el contexto previo de este tratamiento

Antes de escribir una EIPD nueva, revisa la carpeta de salidas por trabajo previo sobre el mismo
tratamiento o contraparte (ruta en `## Salidas`). Busca:
- **Triajes previos** (`triaje-tratamiento`) — su valoración de riesgo, condiciones obligatorias
  y preocupaciones son el punto de entrada de la EIPD.
- **EIPD previas** del mismo tratamiento — una EIPD que sustituye a otra debe reconciliar (qué
  cambió, qué se mantiene). Una EIPD que produce en silencio conclusiones distintas a una previa
  sobre el mismo tratamiento es una contradicción que quien revisa no puede ver.
- **Revisiones de encargo** de los proveedores implicados — informan el análisis de
  subencargados, transferencias y conservación.

Si hay trabajo previo, cítalo: "El triaje previo ([fecha]) valoró esto como [nivel] y exigió
[condiciones]. Esta EIPD parte de ahí — [qué se satisface, qué queda, qué se reescopa]."
**Arrastra la severidad de aguas arriba como suelo** (ver `## Salvaguardas comunes`). Si no hay
nada previo, dilo: "Sin triaje ni EIPD previos sobre este tratamiento; arranque en frío."

## Carga el estilo de la casa

Lee `## Estilo de EIPD`: disparador, estructura de la EIPD semilla, profundidad, quién aprueba.
Si hay estructura semilla en el config, **úsala** — el objetivo es que esta EIPD se parezca a
las otras de este equipo, no a una genérica.

## Paso 0: ¿Hace falta EIPD?

Comprueba el disparador del config (la respuesta de la casa). Además, **investiga los
disparadores obligatorios vigentes**: art. 35.1/35.3 RGPD y la **lista del art. 35.4 publicada
por la AEPD**; criterio práctico de **dos o más** criterios de alto riesgo (Directrices
WP248/EDPB). Cita norma o criterio con referencia precisa y verifica vigencia.

> **Sin suplir en silencio.** Si la consulta a la fuente de investigación devuelve pocos o
> ningún resultado para los disparadores o las bases, informa de lo hallado y para. No rellenes
> el hueco desde web o conocimiento del modelo sin preguntar.
>
> **Etiqueta de fuente.** Etiqueta cada cita: `[BOE]`, `[EUR-Lex]`, `[AEPD]` para lo recuperado
> de esas fuentes; `[web — verificar]` para búsqueda web; `[conocimiento del modelo — verificar]`
> para lo recordado del entrenamiento; `[aportado por el usuario]` para lo que aporte el usuario.

Más allá de lo obligatorio, trata como **indicadores fuertes** (conviene hacerla aunque no sea
estrictamente obligatoria): tecnología nueva, datos de menores, combinar conjuntos no recogidos
juntos, datos que permitan discriminación, tratamientos inesperados para el interesado.

Si no aplica disparador obligatorio ni interno → "No parece que esto necesite EIPD. Aquí tienes
una nota de un párrafo para el expediente que explica por qué, por si alguien pregunta."

## El intake

Antes de escribir, consigue respuestas del equipo de producto. Conversacional está bien.

### Qué y por qué
- ¿La función/producto/cambio?
- ¿Qué problema resuelve para el usuario?
- ¿Qué datos personales toca? Sé específico — "datos de usuario" no es respuesta. ¿Qué campos?
- ¿Algo es recogida nueva, o todo son datos que ya tienes (cambio de finalidad)?
- ¿El tratamiento — almacenamiento, análisis, cesión, decisiones automatizadas?

### Base de legitimación y comprobaciones específicas
- Identifica la **base del art. 6** para cada finalidad (consentimiento / contrato / interés
  legítimo / obligación legal / interés vital / misión de interés público). Si es **interés
  legítimo**, esboza el **juicio de ponderación** (finalidad legítima, necesidad, equilibrio con
  los derechos del interesado). Si es **consentimiento**, cómo se obtiene (libre, específico,
  informado, inequívoco; art. 7).
- Si hay **categorías especiales (art. 9)** o **datos penales (art. 10)**, identifica la
  excepción/ habilitación aplicable — sin ella, el tratamiento no es lícito.
- Cesiones y comunicaciones: ¿alguna requiere base adicional? ¿Marketing y cookies bajo LSSI?
- **Transferencias internacionales** (Cap. V): si salen datos fuera del EEE, ¿qué mecanismo?
  (Decisión de adecuación / Cláusulas Contractuales Tipo 2021/914 / garantías + evaluación de
  transferencia). Verifica vigencia.

### Quién y dónde
- ¿Quién dentro de la empresa ve estos datos? ¿Ingeniería, soporte, analítica?
- ¿Terceros? ¿Encargados, socios, analítica? ¿Hay contrato de encargo?
- ¿Dónde se almacenan? ¿Qué región? ¿Infraestructura nueva o existente?
- ¿Cuánto se conservan? ¿Hay calendario de supresión o viven para siempre? (Principio de
  limitación del plazo, art. 5.1.e.)

### Qué puede salir mal
- Si estos datos se filtran, ¿qué daño sufre la persona?
- ¿Podrían usarse para discriminar, aun sin querer?
- ¿Sorprendería al usuario que esto ocurra? (El "test de lo inquietante" — no es estándar legal,
  pero es útil.)
- ¿Hay forma de oponerse/optar fuera? ¿Debería haberla?

## Escribir la EIPD

**Usa la estructura semilla del config.** Si no se capturó, usa esta por defecto, que cubre el
contenido mínimo del **art. 35.7 RGPD**. Antepón la cabecera de documento de trabajo del config
(`## Salidas`).

```markdown
[CABECERA DE DOCUMENTO DE TRABAJO — según config ## Salidas]

# Evaluación de Impacto (EIPD): [nombre del tratamiento]

**Elaborada por:** [nombre] | **Fecha:** [fecha] | **Estado:** BORRADOR / APROBADA
**Responsable del tratamiento:** [nombre] | **Revisión (DPD/Privacidad):** [nombre]

---

## Resumen ejecutivo
[Dos frases: qué es, si es admisible. P. ej., "La función X trata datos de ubicación para Y.
El tratamiento es coherente con la política y usa el consentimiento como base. Dos medidas
recomendadas; sin bloqueantes."]

**Riesgo global:** [Quien revisa lo fija: 🟢 Bajo / 🟡 Medio / 🟠 Alto / 🔴 Muy alto]

---

## 1. Descripción sistemática del tratamiento (art. 35.7.a)
**Qué:** [la función, en lenguaje claro]
**Categorías de datos:** [campos concretos — no "datos de usuario"]
**Interesados:** [clientes / usuarios / empleados / menores / etc.]
**Finalidad(es):** [por qué]
**¿Recogida nueva?** [sí — estos campos / no — reutilización]
**Encargados/cesiones:** [quién, para qué]
**Conservación:** [plazo y mecanismo de supresión]
**Transferencias internacionales:** [no / sí — mecanismo del Cap. V]

---

## 2. Necesidad y proporcionalidad (art. 35.7.b)
| Finalidad | Base (art. 6) | Necesidad / proporcionalidad | Notas |
|---|---|---|---|
| [finalidad 1] | [base] | [por qué es necesario y proporcionado] | [si IL: ponderación; si consentimiento: cómo se obtiene] |

[Categorías especiales (art. 9) / datos penales (art. 10): excepción/habilitación aplicable.]

---

## 3. Flujo de datos
**Recogida:** [cómo/dónde entran] · **Almacenamiento:** [sistema, región, cifrado] ·
**Acceso:** [quién, con qué controles] · **Cesión/encargo:** [terceros, contrato de encargo] ·
**Conservación:** [plazo, supresión]

---

## 4. Coherencia con la política de privacidad
| Compromiso de la política | ¿Coherente? | Notas |
|---|---|---|
| [compromiso del config] | 🟢 / 🟡 | |
[Si hay 🟡: actualizar la política antes del lanzamiento, o cambiar el tratamiento.]

---

## 5. Riesgos para los derechos y libertades y medidas (art. 35.7.c y d)
| # | Riesgo | Probabilidad | Impacto | Medida | Estado | Responsable |
|---|---|---|---|---|---|---|
| 1 | [riesgo concreto, ligado al diseño — no "brecha de datos" genérico] | B/M/A | B/M/A | [control concreto] | Hecho / Previsto / Hueco | [nombre] |

**Riesgo residual tras medidas:** [valoración]
[Si el riesgo residual sigue alto → **consulta previa a la AEPD (art. 36)** antes de iniciar.]

---

## 6. Derechos de los interesados
| Derecho | ¿Se puede ejercer? | Cómo |
|---|---|---|
| Acceso (art. 15) | | |
| Rectificación (art. 16) | | |
| Supresión (art. 17) | | |
| Limitación (art. 18) | | |
| Portabilidad (art. 20) | | |
| Oposición (art. 21) | | |
| Decisiones automatizadas (art. 22) | | |

---

## 7. Recomendación
[APROBADA / APROBADA CON CONDICIONES / CAMBIOS NECESARIOS / NO APROBADA]
**Condiciones (si las hay):**
- [ ] [cosa concreta antes del lanzamiento]
**¿Consulta previa a la AEPD (art. 36)?** [No / Sí — porque el riesgo residual es alto]
**Aprobación:** [nombre, fecha]
```

## Calidad de los riesgos

Los riesgos deben ser **concretos y ligados al diseño**, no genéricos.

| Riesgo malo | Por qué | Mejor |
|---|---|---|
| "Brecha de datos" | Aplica a todo; no dice nada | "El historial de ubicación es accesible por soporte desde el panel admin sin registro de auditoría — un interno malicioso podría rastrear a un usuario sin dejar rastro" |
| "Incumplimiento del RGPD" | Circular — la EIPD evalúa el cumplimiento | Nombra el artículo concreto y el hueco |
| "A los usuarios no les gustará" | Vago | "Usuarios que se opusieron a marketing podrían recibir esto porque este flujo no comprueba la oposición" |

Apunta a 2-5 riesgos reales, no a 15 inflados.

## Diff con la política

Toda EIPD contrasta con los compromisos de la política. Deriva típica: la política dice "recogemos
X, Y, Z" y la función recoge W; la política dice "no cedemos datos" y la función comparte con un
socio publicitario; la política dice conservación "mientras la cuenta esté activa" y la función
conserva tras la baja. Marca cada desajuste — uno de los dos tiene que cambiar antes del
lanzamiento.

## Traspasos

- **A producto:** lista de condiciones con responsables y fechas. No "mejorar la seguridad" —
  "añadir registro de auditoría a la consulta de ubicación del panel admin, responsable: [lead de
  ing.], antes del lanzamiento."
- **Al proceso de aprobación:** según `## Estilo de EIPD` → quién aprueba (DPD/comité).

## Puerta: presentar una EIPD o consulta previa a la AEPD

Producir una EIPD interna es investigación y documentación. **Presentar una consulta previa a la
AEPD (art. 36)** — o aportar la EIPD a la autoridad en respuesta a un requerimiento — es el acto
consecuente.

**Antes de presentar a la AEPD:** lee `## Quién usa esto`. Si el Rol es No jurista:

> Presentar a la AEPD tiene consecuencias jurídicas — el documento pasa a formar parte del
> expediente y cualquier omisión o error material se convierte en exposición sancionadora.
> ¿Lo has revisado con un abogado/a? Si sí, adelante. Si no, aquí tienes un resumen de una página
> para llevarle:
>
> [Genera un resumen de 1 página: régimen y autoridad, por qué se presenta (consulta previa por
> riesgo residual alto, o requerimiento), riesgos identificados, riesgo residual tras medidas,
> dudas marcadas, y las tres cosas que preguntar al abogado antes de presentar.]
>
> Para encontrar abogado/a: el Servicio de Orientación Jurídica (SOJ) de tu Colegio de la
> Abogacía es el punto de partida más rápido.

No pases esta puerta sin un sí explícito.

## Cierra con el árbol de próximos pasos

Cierra con el árbol según `## Salidas` del CLAUDE.md, personalizado a lo producido.

## Lo que esta skill NO hace

- No aprueba el tratamiento. Una persona firma la EIPD.
- No diseña la medida. Describe qué hay que mitigar; ingeniería diseña el arreglo.
- No sustituye a la **consulta previa** formal a la AEPD cuando es preceptiva — la prepara y
  marca cuándo procede.
