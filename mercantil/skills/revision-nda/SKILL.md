---
name: revision-nda
description: >
  Referencia: triaje rápido de NDA (acuerdos de confidencialidad) entrantes en VERDE / ÁMBAR /
  ROJO para que el equipo solo invierta tiempo de abogado en los que lo necesitan. Pensado para
  que comercial y negocio se autosirvan antes de pasar a jurídico. La carga
  /mercantil:revision cuando detecta un NDA.
user-invocable: false
---

# Revisión de NDA (acuerdo de confidencialidad)

## Contexto de asunto

Comprueba `## Espacios de asunto` en el CLAUDE.md de práctica. Si `Activado` es `✗` (por defecto en
asesoría interna), omite este párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Comprobación de destino

Antes de producir la salida, comprueba a dónde va; ver `## Salvaguardas comunes → Comprobación de
destino` en el CLAUDE.md. Distribuir un análisis interno ampliamente pierde la confidencialidad de
facto. Cuando el destino parezca fuera del círculo, márcalo y ofrece versión interna / depurada /
ambas.

## Propósito

La mayoría de los NDA entrantes están bien. Unos pocos tienen minas. Esta skill los ordena en menos
de un minuto para que jurídico solo lea los que importan. Un VERDE no necesita más que una firma; un
ÁMBAR necesita que un abogado mire una o dos cosas; un ROJO se para antes de perder tiempo.

## Carga el manual primero

**¿Qué lado?** Determina en qué lado está la empresa para este NDA. Suele ser obvio: si la
contraparte evalúa tu producto, eres comercial; si evalúas el suyo, eres compras. Un NDA mutuo
también tiene lado — de quién es el papel y en qué dirección va la evaluación. Si no es obvio,
pregunta. Lee la sección correspondiente del manual. Anota el lado en la salida. Si el lado está
`[Sin configurar]`, detente y pide ejecutar `/mercantil:entrevista-inicial --side <lado>`.

**Antes de triar, lee `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md` → `## Manual`
→ lado → posturas de triaje de NDA.** Esa sección es la fuente de verdad de qué hace un NDA VERDE,
ÁMBAR o ROJO para *este* equipo en *este* lado. Esta skill no trae posturas por defecto — la ley, el
mercado y la tolerancia al riesgo varían demasiado. Si el manual no cubre un término que aparece,
pregunta al usuario su postura por defecto y regístrala en el CLAUDE.md.

## Comprobación de alcance

**Antes de revisar lo propio de un NDA, comprueba si el documento hace más de lo que su nombre
sugiere.** Un NDA mutuo puede esconder: pactos de no captación o no competencia, exclusividad,
cesión de PI, derecho de adquisición preferente, cláusulas de nación más favorecida, o cláusulas de
sumisión/arbitraje que gobiernan mucho más que las disputas de confidencialidad.

Si el NDA contiene obligaciones más allá de la confidencialidad: **ÁMBAR automático** con
independencia del análisis de términos. Márcalas: "Este documento se titula NDA pero contiene [no
captación / cesión de PI / exclusividad / arbitraje amplio]. Es más que un NDA. Derívalo a revisión
letrada." *(Recuerda: los pactos de no competencia/no captación tienen validez limitada en España —
cuidado con el Derecho de la competencia y, si afectan a empleados, con el art. 21 ET.)*

## El triaje

Clasifica el NDA en uno de tres grupos aplicando las posturas del manual. Las definiciones de grupo
son estables; los *criterios* que llenan cada grupo vienen del manual.

### VERDE — a firma

El NDA satisface cada postura del manual y ningún término dispara una bandera roja. Comprobaciones
típicas: mutualidad, plazo, supervivencia, salvedades, ley aplicable, pactos restrictivos, costas.
Confírmalas contra el CLAUDE.md antes de declarar VERDE.

**VERDE requiere posturas de NDA revisadas por abogado.** Es el único camino a firma sin revisión
letrada. No puede emitirse contra posturas por defecto o ausentes. Si el perfil no tiene una sección
de posturas de NDA revisada por abogado: "No puedo emitir VERDE sin posturas de NDA revisadas por
abogado. Ejecuta `/mercantil:entrevista-inicial --side <lado>` con tu asesoría, o deriva este NDA a
revisión. ÁMBAR es la opción correcta cuando faltan posturas — saca el NDA a una persona que pueda
decidir."

**Salida (VERDE):** antepón la cabecera de documento de trabajo del config (`## Salidas`).

```markdown
[CABECERA — según config ## Salidas]

## Triaje de NDA: [contraparte]
VERDE — a firma

### Resumen
Sin banderas rojas bajo el manual. A firma por el proceso estándar.

| Comprobación | Estado | Referencia del manual |
|---|---|---|
| [cada comprobación] | [pasa/falla] | [sección CLAUDE.md] |

**Siguiente paso:** [Enviar al flujo estándar de NDA del CLM | Enviar a [aprobador] para firma]
```

**Antes de pasar de VERDE a firma:** lee `## Quién usa esto`. Si el Rol es No jurista:

> Este paso tiene consecuencias legales (firmar un NDA vincula a la empresa). ¿Lo has revisado con
> un abogado/a? Si sí, adelante. Si no, aquí tienes un resumen de una página: [contraparte,
> dirección del NDA, comprobaciones hechas, lo que el manual no cubría, qué podría salir mal si se
> firma tal cual, y las tres cosas que preguntar]. Para encontrar abogado/a: el SOJ de tu Colegio
> de la Abogacía es el punto de partida más rápido.

No pases esta puerta sin un sí explícito.

### ÁMBAR — necesita ojos de abogado en puntos concretos

Uno o más términos se desvían del manual sin ser rompe-tratos categóricos, O aparece un término que
el manual no cubre. Saca cada punto individualmente para que el aprobador decida.

```markdown
[CABECERA — según config ## Salidas]

## Triaje de NDA: [contraparte]
ÁMBAR — para [aprobador del CLAUDE.md]

### Resumen
- [edición accionable de una línea, p. ej. "Tachar el pacto de no captación (cláusula 6)"]

### Puntos marcados
**1. [Asunto]** — Cláusula [X]
   Qué: [una línea]
   Por qué se marca: [qué postura del manual toca, o "el manual calla sobre esto"]
   **Riesgo legal:** [🔴/🟠/🟡/🟢] | **Fricción de negocio:** [🔴 Bloquea / 🟠 Ralentiza / 🟡 Confunde / 🟢 Invisible]
   Resolución probable: [aceptar / rebatir X / depende del contexto]

### Lo demás
| Comprobación | Estado | Referencia del manual |
|---|---|---|
| [las que pasaron] | pasa | [sección] |

**Siguiente paso:** Pregunta a [aprobador] por los puntos marcados, luego a firma si conforme.
```

### ROJO — parar, hablar con jurídico primero

El NDA toca una postura de la lista "nunca aceptamos", o su estructura es incompatible con la postura
estándar (NDA unilateral cuando el manual exige mutuo; plazo perpetuo cuando el manual lo limita; ley
aplicable en la lista "nunca").

```markdown
[CABECERA — según config ## Salidas]

## Triaje de NDA: [contraparte]
ROJO — no enviar, hablar con jurídico primero

### Resumen
- [edición accionable de una línea]

### Incidencias críticas
**1. [Asunto]** — Cláusula [X]
   > "[cita exacta]"
   Por qué es un problema: [riesgo concreto; cita la postura del manual que viola]
   **Riesgo legal:** [🔴/🟠/🟡/🟢] | **Fricción de negocio:** [🔴/🟠/🟡/🟢]
   Respuesta recomendada: [usar nuestro papel | rebatir con lenguaje concreto | retirarse]

**Siguiente paso:** Envía este triaje a [Dirección Jurídica del CLAUDE.md]. No lo envíes al flujo
del CLM. No le digas a la contraparte que firmaremos.
```

## Granularidad del redline

**Edita con la menor granularidad posible.** Sustituye una palabra antes que una frase; una frase
antes que una oración; reestructura un subapartado antes que sustituir la oración; la cláusula
entera solo cuando esté tan lejos que la edición quirúrgica sea más difícil de leer — y entonces,
dilo en la transmisión. Ante la duda, más pequeño.

## Supuesto de jurisdicción

Este triaje aplica las posturas de ley aplicable y pactos restrictivos del CLAUDE.md. La
exigibilidad (pactos de no competencia/no captación, costas, sumisión) varía por jurisdicción. Si el
NDA implica una jurisdicción fuera de la postura configurada, márcalo: el triaje puede no transferir
tal cual.

## Reglas de salida

**Filtro de complejidad:** si abordar un punto exigiera redactar nuevo lenguaje o reestructurar una
cláusula, no lo intentes. Escribe: "Cláusula [X] — derivar a jurídico." En el resumen solo van
acciones simples y mecánicas (tachar, borrar, sustituir una palabra/frase).

**Regla del NDA limpio:** si pasa todas las comprobaciones sin marcas, el resumen dice solo: "Sin
banderas rojas. A firma por el proceso estándar." No produzcas un informe largo para un NDA limpio.

## Referencia de comprobaciones

Para cada una, el grupo (VERDE/ÁMBAR/ROJO) lo determina el CLAUDE.md; esta skill lista las
*categorías*, no fija umbrales.

- **Mutualidad.** ¿Mutuo o unilateral? Aplica la postura. Si el NDA es unilateral, antes de marcar
  ROJO pregunta si la empresa es la única que revela, si es para una divulgación limitada concreta,
  y si tiene que ver con M&A/empleo/inversión (si sí, deriva — esta skill es para NDA comerciales).
- **Definición de información confidencial.** Alcance (solo marcada vs. todo lo divulgado),
  requisitos de marcado, ventana de confirmación de divulgaciones orales.
- **Salvedades.** Las cinco típicas: información pública (sin culpa); ya poseída; desarrollada de
  forma independiente; recibida de tercero sin restricción; exigida por ley o autoridad (con aviso
  al divulgador cuando sea posible).
- **Residuales.** Cláusula que permite usar lo retenido en la memoria. ¿Aceptable y bajo qué
  condiciones? Postura del manual.
- **Plazo y supervivencia.** Plazo inicial, supervivencia de las obligaciones, trato de los
  secretos empresariales. *(La Ley 1/2019 de Secretos Empresariales protege el secreto mientras
  conserve su carácter — un plazo de confidencialidad corto puede dejar desprotegido un secreto que
  seguiría siéndolo. Considéralo.)*
- **Pactos restrictivos.** No captación (empleados, clientes), no competencia, exclusividad.
  Sensibles a la jurisdicción y al Derecho de la competencia; aplica el manual.
- **Costas.** Cláusulas de reembolso de costas; ¿mutuas, unilaterales, a la parte vencedora?
- **Salvedad de copias de seguridad.** ¿La cláusula de destrucción/devolución exceptúa los sistemas
  estándar de copia y archivo? Postura del manual.
- **Cláusula penal.** Si hay penalización por incumplimiento, recuerda que en España la cláusula
  penal (arts. 1152-1155 CC) es válida pero **moderable judicialmente** si el incumplimiento es
  parcial o la pena resulta desproporcionada. Márcala si es desorbitada.
- **Ley aplicable y fuero.** Per CLAUDE.md → `Ley aplicable y fuero`.

## Contexto de la contraparte

**NDA de grandes empresas:** las grandes contrapartes rara vez negocian NDA. Calibra: ¿la bandera
roja es de verdad un rompe-tratos o solo "distinto de nuestro modelo"? Si la relación importa, la
decisión es si aceptar su papel — escala esa decisión, no la tomes.

**NDA de startups:** suelen aceptar nuestro papel. Si el suyo tiene problemas, lo más rápido suele
ser "usemos el nuestro".

## Acción de cierre

Lee `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md` → `## Preferencias de triaje de
NDA` → `closing_action`. Si está configurada, añádela al final de cada salida. Si no, añade: "Encamina
el NDA final por tu proceso de aprobación estándar."

## Lo que esta skill NO hace

- No negocia. Ordena.
- No redacta un NDA. Si la respuesta es "usa nuestro papel", el usuario lo saca del CLM/sistema.
- No decide los ÁMBAR. Los saca para una persona.
- No fija ninguna postura. Las posturas viven en el CLAUDE.md.

## Cierra con el árbol de próximos pasos

Cierra con el árbol según `## Salidas`, personalizado a lo producido.
