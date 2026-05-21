---
name: resumen-negocio
description: >
  Traduce una revisión de contrato a un resumen que el responsable de negocio sí leerá. No es un
  memo jurídico — es una respuesta de dos minutos a "¿puedo firmar esto y qué necesito saber?".
  Úsala cuando el usuario diga "resume para negocio", "explícaselo a compras", "resumen no
  jurídico", o cuando una revisión está hecha y tiene que salir a alguien fuera de jurídico.
---

# Resumen para negocio

## Contexto de asunto

Comprueba `## Espacios de asunto`. Si `Activado` es `✗` (por defecto en asesoría interna), omite este
párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Comprobación de destino

Antes de producir la salida, comprueba a dónde va; ver `## Salvaguardas comunes → Comprobación de
destino`. Cuando el destino parezca fuera del círculo, ofrece versión interna / depurada / ambas.

## Propósito

El responsable de negocio que pidió este contrato no quiere un memo jurídico. Quiere saber: ¿puedo
firmarlo, cuál es la trampa y qué tengo que hacer? Esta skill toma una revisión hecha y la convierte
en eso.

## ¿Qué lado?

El memo subyacente se corrió contra el lado comercial o el de compras. Mantén ese enmarcado. Un
resumen del lado compras dice "esto es lo que recibimos y lo que cedemos"; uno del lado comercial
dice "esto es lo que vendemos y de qué respondemos". Comprueba el lado (debería constar al inicio del
memo) y ajusta la voz. Si no es obvio, pregunta al abogado.

## Calibración de audiencia

Lee `## Estilo de la casa` → quién lee los resúmenes, qué extensión. Si no se especifica, por defecto:
compras o un responsable de área, dos párrafos máximo, sin tecnicismos jurídicos.

| Audiencia | Le importa | No le importa |
|---|---|---|
| **Compras** | Precio, mecánica de renovación, ruta de aprobación | Estructura del límite de responsabilidad |
| **Responsable de área (presupuesto)** | Si su equipo puede usarlo, qué pasa si falla, coste | Alcance de la indemnidad |
| **Finanzas** | Coste total, riesgo de precio en renovación, compromisos | Ley aplicable |
| **Seguridad / IT** | Tratamiento de datos, subencargados, certificaciones, dónde residen | Lo demás |
| **Patrocinador ejecutivo** | Si nos va a poner en un aprieto, si jurídico bloquea | Detalles |

Pregunta para quién es si no es obvio.

## El resumen

### Tope de extensión — obligatorio
- **Un párrafo** para el veredicto y qué es esto (términos de negocio, lenguaje claro).
- **Un párrafo** para la trampa — lo que sorprendería al interesado luego si nadie se lo dice ahora.
- **Una lista de 2-3 acciones** que el interesado tiene que hacer (como mucho tres).
- **Una línea de cierre** con el momento de la aprobación.

**Menos de 200 palabras en total.** Si escribes más, estás metiendo detalle que el interesado no
necesita — tiene el memo para eso.

### Alcance de la cita — disciplina
Al citar una cláusula, cita la **oración condicional completa**, no una versión truncada. "Salvo lo
previsto en la orden de pedido, la renovación de suscripciones promocionales pasa a tarifa de lista"
significa algo distinto de "la renovación pasa a tarifa de lista". Si la cita completa no cabe,
parafrasea en vez de truncar.

### Formato

Antepón la cabecera del config (`## Salidas`).
```markdown
[CABECERA — según config ## Salidas]
<!-- Quita la cabecera si se reenvía fuera del círculo (a negocio, contraparte, proveedor). -->

**[Contraparte] [tipo de acuerdo]** — [LISTO PARA FIRMAR | NECESITA CAMBIOS | BLOQUEADO]

[Un párrafo: qué hace este acuerdo, en términos de negocio. No "contrato marco de servicios para la
prestación de analítica en la nube" — "es el contrato de la herramienta de paneles que quiere
marketing".]

[Un párrafo: lo que el interesado necesita saber. La trampa, si la hay. P. ej.: "Aviso: esto se
prorroga cada año por tácita reconducción y hay que avisar 60 días antes. Lo he añadido al
seguimiento, pero conviene que lo sepas." O: "Acuerdo limpio, sin sorpresas, a firma."]

**Verifica las entradas del seguimiento antes de afirmarlas.** Antes de decir "lo he añadido al
seguimiento" (o equivalente), verifica que `seguimiento-renovaciones` se ha ejecutado para este
contrato (busca una salida que lo nombre). Si no: o lo ejecutas primero, o escribes el resumen sin
afirmarlo e incluyes una acción: "Añadir al seguimiento de renovaciones — pendiente". Afirmar una
entrada que no existe es peor que omitir la tranquilidad.

**Qué tienes que hacer:**
- [ ] [acción, si la hay — "confirmar que el equipo está conforme con que los datos residan en la UE"
  o "nada — encamino a firma"]

**Aprobación:** [quién aprueba y plazo esperado]
```

### Qué traducir

| Hallazgo jurídico | Traducción de negocio |
|---|---|
| "Responsabilidad limitada a 12 meses de honorarios" | "Si rompen algo, lo máximo que podemos recuperar es un año de lo que les pagamos." |
| "Sin resolución por conveniencia" | "Una vez firmado, estamos atados todo el plazo — no podemos cancelar sin más si dejamos de usarlo." |
| "Prórroga tácita con preaviso de 60 días" | "Esto se renueva solo cada año. Para cancelar hay que avisarles dos meses antes de la fecha de renovación." |
| "Sin indemnidad por PI" | "Si alguien nos demanda diciendo que esta herramienta infringe su patente, el proveedor no está obligado a defendernos." |
| "Lista de subencargados no comunicada" | "No sabemos qué otras empresas tendrán acceso a nuestros datos a través de ellos." |
| "Borrado de datos en 30 días tras la terminación" | "Cuando cancelemos, borran nuestros datos en un mes. Exporta lo que necesites antes." |
| "Créditos de ANS limitados al 10% de la cuota mensual" | "Si el servicio se cae, nos devuelven un crédito pequeño. No cubrirá el coste de la caída para el negocio." |

### Qué NO incluir
- Números de cláusula
- Términos definidos entre comillas
- La palabra "indemnización" (di "nos cubren si" / "les cubrimos si")
- Matrices de riesgo con puntos de colores (salvo que este interesado los haya pedido antes)
- Salvedades de que esto no es asesoramiento jurídico — el interesado sabe quién lo envía

## Cuando la revisión encontró problemas

Si hay 🔴 o 🟠, el segundo párrafo es "esto es lo que estamos rebatiendo y por qué".
```markdown
[CABECERA — según config ## Salidas]
<!-- Quita la cabecera si se reenvía fuera del círculo. -->

**[Contraparte] [tipo]** — NECESITA CAMBIOS

[Qué es, un párrafo.]

Volvemos a ellos con [N] cosas antes de que esté listo. La principal: [la incidencia crítica en
lenguaje claro — "quieren derecho a usar nuestros datos para mejorar su producto, lo que significa que
la instancia de nuestros competidores se vuelve más lista con nuestros datos"]. Les hemos pedido
quitarlo. [Evaluación realista: "probablemente acepten" / "puede ser un punto de fricción — te
mantengo al tanto".]

**Qué tienes que hacer:**
- [ ] Nada aún — te aviso cuando vuelva de ellos.
  O
- [ ] [Decisión de negocio: "Si no ceden en X, ¿te vale Y, o nos retiramos?"]
```

## Reconciliación de escalados

La revisión aguas arriba puede nombrar varios destinos de escalado (Dirección Jurídica, Seguridad,
DPD, Finanzas, responsable de negocio) en distintos hallazgos. `escalado` encamina uno cada vez. Sin
reconciliación, Dirección Jurídica ve el memo y los otros nunca.

Antes de producir el resumen, lee el memo y cuadra: (1) cuenta los destinos que la revisión nombró
(de-duplica por persona); (2) cuenta los escalados realmente redactados (busca borradores de
`escalado` en la carpeta); (3) reconcilia. Incluye un bloque corto encima de la lista de acciones:

```markdown
**Estado de escalados:** [M] de [N] destinos encaminados. Pendientes de acción:
- [Aprobador] — [una línea del hallazgo que lo nombró]
```
Si todos están encaminados: `**Estado de escalados:** [N] de [N] encaminados.` Si no hubo escalados,
omite el bloque. **El bloque de reconciliación está exento del tope de 200 palabras** (es
administración, no narrativa). No omitas a un aprobador nombrado porque el interesado no lo reconozca:
la reconciliación es interna, le dice al abogado que envía si todo el encaminamiento está hecho.

## Traspasos

**De revision-proveedor / revision-saas:** esas skills producen el memo completo. Esta lo lee y lo
comprime. No reviséis el contrato otra vez — lee la revisión.
**Al interesado:** por el canal que diga el CLAUDE.md. Si es Slack, bajo 150 palabras. Si es correo,
el formato de arriba vale.

## Nota sobre el tono

Los interesados recuerdan dos cosas de jurídico: si les bloqueó y si tuvo sentido. Esta skill es cómo
jurídico tiene sentido. Escribe como si se lo explicaras a un colega listo en un café, no como un memo
para el expediente. Si el resumen honesto es "esto está bien, fírmalo", dilo. No infles una revisión
limpia en tres párrafos para parecer concienzudo.
