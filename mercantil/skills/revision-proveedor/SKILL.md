---
name: revision-proveedor
description: >
  Referencia: revisión de un contrato con proveedor (o de servicios) entrante contra el manual del
  equipo en `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md`. Marca desviaciones,
  evalúa el riesgo, genera lenguaje de redline concreto y encamina al aprobador. La carga
  /mercantil:revision cuando detecta un contrato de servicios, contrato marco o similar.
user-invocable: false
---

# Revisión de contrato con proveedor

## Contexto de asunto

Comprueba `## Espacios de asunto`. Si `Activado` es `✗` (por defecto en asesoría interna), omite
este párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Comprobación de destino

Antes de producir la salida, comprueba a dónde va; ver `## Salvaguardas comunes → Comprobación de
destino`. Cuando el destino parezca fuera del círculo, ofrece versión interna / depurada / ambas.

## Propósito

Leer un contrato con proveedor contra el manual que este equipo usa de verdad (en el CLAUDE.md),
encontrar cada término que se desvía y decirle al abogado qué hacer con cada uno — con lenguaje de
redline concreto, no un vago "considerar revisar". La salida es un memorándum accionable en una
pasada: cada incidencia con severidad, impacto de negocio, propuesta de arreglo y, si procede,
escalado.

## Precondición: carga el manual

**Antes de leer el contrato, lee el CLAUDE.md.** Si falta o tiene marcadores:

> No has configurado tu perfil aún — así adapto las posturas del manual, el escalado y el estilo.
> **Dos opciones:** (1) `/mercantil:entrevista-inicial` (2 min); (2) di **"provisional"** y reviso
> contra valores genéricos (España, riesgo medio, rol jurista, sin manual — marco los riesgos
> habituales de proveedor desde primeros principios) etiquetando todo `[PROVISIONAL]`.

**¿Qué lado?** Determina el lado. Suele ser obvio: si la contraparte es un proveedor que te
suministra bienes o servicios, eres compras; si es un cliente que compra tu producto, eres comercial.
Si no es obvio (reventa, partenariado, reparto de ingresos), pregunta. Lee la sección del manual del
lado correspondiente. Anota el lado en la salida. Si el lado está `[Sin configurar]`, detente y pide
`/mercantil:entrevista-inicial --side <lado>`.

El manual del CLAUDE.md es la fuente de verdad: posturas estándar (las *suyas*, no las del mercado),
alternativas aceptadas, lo que nunca aceptan, quién aprueba qué, y el rompe-tratos a comprobar
primero. Si el contrato tiene el rompe-tratos, márcalo arriba del memo y para la revisión detallada
— no tiene sentido pasar 30 minutos en límites de responsabilidad si el contrato cede algo
inaceptable.

## Flujo

### Paso 1: Orientación

Lee el contrato entero una vez, rápido. Responde: ¿qué tipo de contrato es?; ¿qué somos
(cliente/proveedor)?; contraparte (¿grande que no negocia, o startup que sí?); valor (anual/total si
consta); plazo y prórroga; ¿hay anexo de protección de datos (encargo)?; ¿hay orden de pedido?

**Valor económico.** Si el contrato no indica importe (el marco fija términos y la orden lleva el
precio, lo habitual), **detente y pregunta** antes de aplicar umbrales de escalado: pide el valor de
la orden, o si está por encima/debajo del umbral, o enruta conservadoramente al aprobador superior.
No asumas un valor en silencio y luego lo uses para el enrutado.

**Protección de datos por referencia.** Si el contrato incorpora un anexo de encargo "disponible en
[URL]" por referencia, el encargo es parte del contrato pero no lo tienes delante. Anótalo en la
orientación y en el memo, y **ofrece traspasarlo a `/proteccion-datos:revision-encargo`** (que está
hecho para el trabajo de encargo del art. 28 — subencargados, CCT, brechas). No procedas como si el
encargo no existiera cuando se incorpora por referencia.

### Paso 2: Comprobación del rompe-tratos

Comprueba "lo único" del CLAUDE.md primero. Si está presente:

```markdown
## ⛔ ROMPE-TRATOS PRESENTE
**Cláusula [X.X]** contiene [el rompe-tratos]. Según el manual, es un no rotundo. Recomiendo:
- [ ] Rebatir — proponer [lenguaje alternativo concreto]
- [ ] Retirarse — si la contraparte no se mueve, no firmamos
La revisión de abajo se incluye por completitud pero es irrelevante hasta resolver esto.
```

### Paso 3: Comparación cláusula a cláusula

Para cada categoría del manual, encuentra la cláusula correspondiente y compara. Por cada desviación:

```markdown
### [Cláusula X.X]: [nombre del problema]
**El manual dice:** [postura estándar, citada del CLAUDE.md]
**El contrato dice:**
> "[cita exacta]"
**Hueco:** [Falta término | Más débil que el estándar | Más débil que la alternativa | Estructura no estándar | Inaceptable]
**Riesgo legal:** 🔴 Crítico | 🟠 Alto | 🟡 Medio | 🟢 Bajo
**Fricción de negocio:** 🔴 Bloquea | 🟠 Ralentiza | 🟡 Confunde | 🟢 Invisible
**Por qué importa:** [una o dos frases en lenguaje claro — qué sale mal para el negocio si queda así]
**Redline propuesto:**
> "[lenguaje de reemplazo concreto — listo para pegar en un marcado]"
**Si no se mueven:** [la alternativa del CLAUDE.md, o "escalar a [persona]" si no hay alternativa]
```

**Calibración de severidad:** 🔴 Crítico = no firmar sin arreglar (término en la lista "nunca
aceptamos" o rompe-tratos); 🟠 Alto = empujar fuerte, escalar si no se mueven (fuera del rango de
alternativas); 🟡 Medio = empujar en primera ronda, aceptar si es el último punto abierto; 🟢 Bajo =
anotar, no gastar capital. Siempre contra el CLAUDE.md. Si un término no mapea a una postura,
pregunta a qué grupo pertenece y ofrece registrarlo.

#### Procedimiento de la limitación de responsabilidad

**El importe del límite es la parte menos importante.** No produzcas una sola línea "comprobar
límite". Trabaja las cuatro dimensiones y enúncialas:

1. **Directos vs. indirectos / lucro cesante.** ¿El límite cubre TODA la responsabilidad o solo el
   daño directo? Un límite sobre daño directo con lucro cesante sin límite es una postura distinta de
   un límite agregado. Enuncia ambos tratamientos. *(En Derecho español, daños = daño emergente +
   lucro cesante, art. 1106 CC; no existe la categoría anglosajona de "consequential" — interpreta
   la cláusula en esos términos.)*
2. **La base del límite — cítala literal.** "12 meses" puede significar honorarios pagados en los 12
   meses previos, pagaderos en el periodo en curso, los del pedido actual, o el total pagado. Pueden
   diferir en un orden de magnitud. Cita el texto exacto; si es ambiguo, márcalo.
3. **Interacción límite-salvedades.** Un límite con indemnidad sin tope para datos, PI y
   confidencialidad es funcionalmente ilimitado para las reclamaciones que de verdad surgen. Enumera
   qué queda POR ENCIMA (las salvedades) y qué POR DEBAJO, y valora si la superficie limitada es
   significativa. **Límite imperativo:** el **art. 1102 CC** impide excluir o limitar la
   responsabilidad por **dolo** — una cláusula que pretenda hacerlo es nula; márcalo siempre que el
   límite barra el dolo.
4. **Tu postura del manual por dimensión.** El perfil debería tener posturas para límite directo,
   indirectos, lista de salvedades y base. Si solo hay un campo, anótalo y sugiere desglosarlo.

#### Comprobación de jurisdicción

**El manual aplica una preferencia de ley/fuero; la exigibilidad varía.** Comprueba la ley aplicable
real del contrato antes de aceptar las posturas al pie de la letra:

- **¿Hay un consumidor?** Si una parte es consumidor (persona física fuera de su actividad), aplica
  el **TRLGDCU** (cláusulas abusivas) y reglas de fuero protectoras — el régimen cambia por completo.
  `[verificar]`
- **Ley/ fuero extranjeros.** Si el contrato elige Derecho o tribunales extranjeros, el análisis del
  marco español puede no transferir. Comprueba **Roma I** (ley aplicable) y **Bruselas I bis**
  (competencia); la sumisión expresa entre empresarios suele ser válida pero hay límites. `[verificar]`
- **Pactos de no competencia/no captación.** Validez limitada; cuidado con el Derecho de la
  competencia (Ley 15/2007) y, si afectan a empleados, con el art. 21 ET. `[verificar]`
- **Exclusiones de responsabilidad.** El dolo no es excluible (art. 1102 CC); con consumidores, las
  exclusiones están muy limitadas. `[verificar]`
- **Plazos de pago.** La Ley 3/2004 fija máximos imperativos (60 días B2B); un plazo superior es
  nulo en lo que exceda. `[verificar]`

Cuando la postura del manual choque con la ley aplicable del contrato, márcalo: "Tu manual prefiere
[X], pero este contrato se rige por [Y] donde [X] es [inexigible / limitado / desplazado por norma
imperativa]. `[verificar]`"

### Paso 4: Términos favorables y huecos

Dos listas cortas: **Mejor que nuestro estándar** (lo que el proveedor da de más — moneda de cambio)
y **Falta por completo** (cláusulas estándar ausentes: restricciones a la cesión, derechos de
auditoría, fuerza mayor, requisitos de seguro).

### Paso 5: Encaminamiento del escalado

Comprueba la matriz del CLAUDE.md contra el valor del contrato, la presencia de 🔴 críticos y los
disparadores automáticos (responsabilidad sin límite, cesión de PI, etc.). Indica claramente quién
debe aprobar.

```markdown
## Encaminamiento de aprobación
Según [valor / severidad], requiere:
- [ ] **[Nombre/rol]** — [motivo]
- [ ] **Visto bueno de negocio** sobre [término comercial concreto]
**Siguiente paso recomendado:** [Enviar redlines | Escalar a Dirección Jurídica antes de responder | Pedir input de negocio sobre X]
```

**Antes de enviar redlines a la contraparte:** lee `## Quién usa esto`. Si el Rol es No jurista:

> Enviar redlines es un acto jurídico — la contraparte tratará cada edición como nuestra postura
> negociadora. ¿Lo has revisado con un abogado/a? Si sí, adelante. Si no, aquí tienes un resumen de
> una página: [contraparte, tipo de contrato, redlines propuestos, posturas del manual detrás, las
> alternativas, y qué preguntar antes de enviar]. Para encontrar abogado/a: el SOJ de tu Colegio de
> la Abogacía es el punto de partida más rápido.

No pases esta puerta sin un sí explícito.

## Granularidad del redline

**Edita con la menor granularidad posible.** Sustituye una palabra antes que una frase; una frase
antes que una oración; reestructura un subapartado antes que sustituir la oración; la cláusula entera
solo cuando esté tan lejos que la edición quirúrgica sea más difícil de leer — y dilo en la
transmisión. Ante la duda, más pequeño.

## Paso 6: Montar el memo

Antepón la cabecera de documento de trabajo del config (`## Salidas`). Este memo y el contrato pueden
estar sujetos a secreto profesional; distribúyelo solo dentro del círculo y quita la cabecera antes
de cualquier entrega externa.

> **Sin suplir en silencio.** Si la fuente de investigación devuelve poco para una regla que el memo
> necesita (validez de una limitación, alcance de indemnidad, elección de ley), informa y para.
> **Etiqueta de fuente** en cada cita: `[BOE]`/`[CENDOJ]`/`[EUR-Lex]`, `[web — verificar]`,
> `[conocimiento del modelo — verificar]`, `[aportado por el usuario]`.

```markdown
[CABECERA — según config ## Salidas]

# Revisión de contrato: [contraparte] [tipo]
**Revisado:** [fecha] · **Valor:** [importe] / [plazo] · **Nuestro papel:** [Cliente/Proveedor]

## Conclusión
[Dos frases. ¿Podemos firmar? ¿Qué tiene que cambiar primero?]
**Incidencias (riesgo legal):** [N]🔴 [N]🟠 [N]🟡 [N]🟢
**Incidencias (fricción de negocio):** [N]🔴 [N]🟠 [N]🟡 [N]🟢
**Aprobación de:** [nombre]

## Comprobación del rompe-tratos
[✅ Limpio | ⛔ Presente — ver arriba]

## Incidencias por severidad
[bloques del Paso 3, agrupados Crítico → Bajo]

## Términos favorables
[lista]
## Cláusulas que faltan
[lista]

## Encaminamiento de aprobación
[del Paso 5]

## Paquete de redlines
[Si se pide: lenguaje consolidado listo para marcado]
```

## Integración: CLM y DocuSign

Si hay CLM conectado: comprueba si la contraparte ya tiene acuerdos con nosotros (informa la postura),
saca la plantilla de flujo del tipo de acuerdo y ofrece crear el registro con el memo adjunto. Si
DocuSign está conectado y el acuerdo está listo para firma (todo verde o aceptado), ofrece generar el
sobre y encaminar a los firmantes en el orden de la matriz. **No envíes nada a firma sin instrucción
explícita.**

**Antes de generar un sobre de firma o encaminar a firma:** lee `## Quién usa esto`. Si el Rol es No
jurista, aplica la misma puerta de revisión letrada (resumen de una página + sí explícito).

## Controles de calidad antes de entregar

- [ ] CLAUDE.md cargado y citado — no posturas genéricas de mercado
- [ ] Rompe-tratos comprobado primero
- [ ] Cada incidencia tiene lenguaje de reemplazo concreto
- [ ] Severidades calibradas (no todo es Crítico)
- [ ] Aprobador nombrado, no "escalar a jurídico"
- [ ] Contexto de la contraparte considerado (grande vs. startup)

## Cierra con el árbol de próximos pasos

Cierra con el árbol según `## Salidas`, personalizado a lo producido.
