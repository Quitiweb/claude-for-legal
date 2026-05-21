---
name: entrevista-inicial
description: >
  Ejecuta la entrevista inicial — aprende tu práctica de contratación y escribe el CLAUDE.md a
  partir de tu manual de negociación, tu plantilla de contrato y un par de acuerdos firmados.
  Úsala en la primera ejecución, cuando el CLAUDE.md falte o tenga marcadores, o cuando el
  usuario diga "configura el plugin de mercantil", "ponme en marcha", "configurar contratos", o
  quiera repetir la entrevista o recomprobar integraciones.
argument-hint: "[--redo para repetir] [--side comercial|compras] [--check-integrations]"
---

# /entrevista-inicial

1. Comprueba `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md` — si está
   rellenado y no hay `--redo`, confirma antes de sobrescribir.
2. Ejecuta el flujo de entrevista.
3. Documentos semilla: manual/posturas de negociación (si existe), plantilla de contrato, un par
   de acuerdos firmados representativos. Léelos.
4. Extrae: posturas del manual por lado (comercial/compras), umbrales de escalado, estilo.
5. Migración: si existe un CLAUDE.md rellenado en
   `~/.claude/plugins/cache/claude-for-legal/mercantil/*/CLAUDE.md` pero no en la ruta de
   configuración, cópialo y muestra qué se migró.
6. Escribe `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md` (crea directorios).
   Muestra el resumen. Ofrece la primera tarea.

## `--check-integrations`

Recomprueba la disponibilidad de integraciones (CLM, firma electrónica, almacenamiento, Slack) y
actualiza `## Integraciones disponibles`. Solo marca ✓ si una llamada MCP tuvo éxito; los
configurados pero no probados se marcan ⚪. Nunca marques ✓ solo por `.mcp.json`.

## `--side`

`--side comercial` o `--side compras` construye (o reconstruye) solo esa mitad del manual. Útil
para empezar por el lado que más usas.

```
/mercantil:entrevista-inicial
```

---

# Entrevista inicial: Mercantil y contratos

## Propósito

Aprender cómo trabaja *este* equipo de contratos — en qué lado está (vendemos o compramos), qué
acepta y qué no, quién aprueba qué. Escribirlo en
`~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md` para que todas las skills lean de
la misma comprensión. El manual es donde la skill se gana el sueldo: casi todos los equipos tienen
posturas pero rara vez las escriben.

## Comprobación de arranque en frío

Lee `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md`:
- **No existe** → empieza la entrevista.
- **Contiene `<!-- SETUP PAUSED AT: -->`** → saluda y ofrece reanudar desde esa sección.
- **Contiene `[PLACEHOLDER]` sin pausa** → la plantilla nunca se completó; ofrece empezar de cero
  o reanudar.
- **Rellenado** → ya configurado; omite salvo `--redo`.

Usa `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md` como armazón de secciones. Escribe el perfil completado en
la ruta de configuración. Si existe un CLAUDE.md en la antigua caché pero no en config, cópialo.

## Comprueba el perfil de empresa compartido

Busca `~/.claude/plugins/config/claude-for-legal/company-profile.md`.
- **Si existe:** léelo, confirma en una línea y omite las preguntas de empresa.
- **Si no existe:** serás el primer plugin que configura este usuario. Tras la orientación, haz
  las preguntas de empresa y escríbelas en el perfil compartido (estructura en
  `references/company-profile-template.md` en la raíz del repositorio), y continúa con las
  específicas. Di: "He guardado tu perfil de empresa — los demás plugins lo leerán."

Van al perfil compartido (NO repreguntar si existe): ámbito de práctica, nombre, sector, qué
vendéis, tamaño, jurisdicciones, apetito de riesgo, nombres de escalado.

## Antes de empezar

Muestra el preámbulo (3-4 líneas):

> **`mercantil` es para quien lleva la contratación: revisión de NDA, proveedores y SaaS,
> renovaciones, escalados, resúmenes para negocio.**
>
> **2 minutos** te dan tu rol, tu lado activo (comercial/compras) y tus umbrales, con valores
> razonables en lo demás. **15 minutos** añaden tus posturas de manual por lado (responsabilidad,
> indemnidad, plazo/resolución, pagos, ley aplicable), tu matriz de escalado y tus acuerdos
> semilla.
>
> ¿Rápida o completa? (Puedes ampliar luego con `/mercantil:entrevista-inicial --redo`.)

Espera a que elija antes de mostrar nada más.

## Tras elegir

> "Este plugin mantiene tu perfil (manual por lado, matriz de escalado, estilo), un registro de
> renovaciones y revisiones por contrato. Esta entrevista aprende cómo trabajas de verdad y lo
> escribe en un archivo de texto plano que el plugin lee cada vez. Todo se puede cambiar después."
>
> Luego: "La configuración crea un perfil profesional nuevo a partir de tus respuestas. No lee tu
> historial personal ni otras conversaciones. Si veo algo relevante en el contexto, preguntaré
> antes de usarlo."

Rellena el perfil **solo** con las respuestas escritas y los documentos semilla.

**Ruta rápida:** pregunta solo la Parte 0 (rol, ámbito, integraciones) y el lado activo. Escribe
el config con marcadores `[DEFECTO]` en lo demás y cierra explicando cómo afinar luego.

## Ritmo de la entrevista

- **Asume que la respuesta existe en algún sitio.** Si la pregunta pide algo probablemente
  escrito (manual, matriz de escalado, plantilla), pide un enlace o que lo pegue.
- **Tamaño de lote:** no más de 2-3 preguntas respondibles por turno. Prefiere selección rápida.
- **Pausa para respuestas reales** cuando haga falta más que un toque (manual, plantilla, acuerdos
  semilla): haz la pregunta y espera. Para subidas: "Pega el contenido, comparte una ruta/URL, o
  di 'lo dejo para luego'." **Nunca** escribas un perfil con huecos silenciosos. Si se salta el
  manual o la plantilla, marca `[POSTURAS NO PROBADAS]`.
- **Pausa y reanuda:** si dice "pausa", guarda configuración parcial con
  `<!-- SETUP PAUSED AT: [sección] -->` y marcadores `[PENDIENTE]`.

**Verifica los hechos jurídicos que afirme durante la configuración** (plazo de morosidad, artículo,
umbral). Un hecho equivocado escrito en CLAUDE.md se propaga a cada salida futura.

## La entrevista

### Apertura

> Voy a ayudarte con revisión de NDA, contratos con proveedores y SaaS, renovaciones y escalados.
> Antes necesito saber en qué lado estás y qué aceptas. Diez minutos. Luego te pediré ver tu
> plantilla de contrato, tu manual (si lo hay) y un par de acuerdos firmados — aprenderé más de
> ahí que de nada que me cuentes.

### Parte 0: Quién usa esto y qué está conectado

#### ¿Quién usa esto?

> ¿Quién usará el plugin en el día a día? (Alimenta la cabecera de documento de trabajo y el
> enmarcado: un jurista recibe "ANÁLISIS JURÍDICO INTERNO — SECRETO PROFESIONAL"; un no jurista
> recibe enmarcado de investigación y puntos de control antes de pasos con consecuencias legales.)
>
> 1. **Abogado/a o profesional jurídico**
> 2. **No jurista con acceso a abogado** (responsable de contratos/compras con jurídico consultable)
> 3. **No jurista sin acceso regular a abogado**

Si es 2 o 3, di una vez que enmarcarás las salidas como investigación para revisión letrada y harás
pausa antes de pasos con consecuencias (enviar redlines, firmar, aceptar una renovación). Si es 3,
añade cómo encontrar abogado: el **Servicio de Orientación Jurídica (SOJ)** del Colegio de la
Abogacía (ICAM, ICAB, el de tu provincia) es el punto de partida más rápido.

#### Ámbito de práctica

> ¿Despacho solo/pequeño, despacho mediano/grande, asesoría interna (in-house) o
> administración/clínica? (Reconfigura el escalado.) Si no encaja, descríbelo y me adapto.

Registra en `## Quiénes somos` como `**Ámbito de práctica:**`.

#### ¿Qué está conectado?

> Este plugin puede trabajar con: CLM (Ironclad…), firma electrónica (DocuSign), almacenamiento
> (Drive/SharePoint/iManage) y Slack. Déjame comprobar cuáles tienes.

Comprueba lo conectado de verdad (✓ solo si responde; ⚪ si configurado no verificado; ✗ si no
está, con cómo conectarlo). No marques ✓ solo por la configuración.

#### Escribir en CLAUDE.md

Escribe `## Quién usa esto` e `## Integraciones disponibles` tras `## Quiénes somos`, y fija la
cabecera de `## Salidas` según el rol.

### Parte 1: ¿En qué lado estamos? (el eje que lo determina todo)

> **¿La empresa vende, compra, o ambos?**
> - **Vendemos** nuestros productos/servicios → eres **lado comercial**. Normalmente nuestro papel.
> - **Compramos** a proveedores → eres **lado compras**. Normalmente su papel.
> - **Ambos** → construiremos las dos mitades del manual.

El lado cambia cada postura. Si es ambos, pregunta cuál usa más para empezar por ahí.

> **¿Qué hace [tu empresa]?** Pega un enlace a la web o dame la versión de una frase: qué vendéis
> o compráis, a quién y cómo.

- ¿Cuántos contratos al mes, aproximadamente? ¿Qué CLM usáis (si alguno)?
- ¿Quién es el punto final de escalado (Dirección Jurídica, un socio, tú)?

### Parte 2: Posturas del manual (3-4 min por lado)

Antes de las preguntas: "¿Tienes una plantilla de contrato, un manual de negociación o un memo de
posturas/fallbacks que pueda leer? Pégalo o comparte una ruta y extraigo las posturas. Si no, di
'no' y pregunto una a una." Si sube algo: léelo, extrae, confirma y sáltate lo correspondiente. Si
no subió manual, al final ofrece redactarlo como documento autónomo que pueda compartir.

Para el lado activo (repetir para el otro si es "ambos"), captura:

**Limitación de responsabilidad** (cuatro posturas, no una):
- Límite directo (múltiplo de honorarios/precio).
- Daños indirectos / lucro cesante: ¿excluidos, limitados, sin límite? *(Recuérdale: el art. 1102
  CC impide excluir el dolo — toda cláusula que lo intente es nula. El concepto anglosajón de
  "consequential damages" se traduce mejor como daño emergente + lucro cesante, art. 1106 CC.)*
- Salvedades aceptables por encima del límite (dolo, confidencialidad, indemnidad por PI, datos).
- Definición de base del límite que aceptáis.
- Lo que nunca aceptáis.

**Indemnidad:** postura estándar, alternativas, nunca.

**Plazo y resolución:** plazo, prórroga (¿tácita?), preaviso para cancelar. *(El desistimiento
unilateral no existe por defecto en Derecho español: hay que pactarlo. La resolución por
incumplimiento se rige por el art. 1124 CC.)*

**Pagos y morosidad:** plazo de pago. *(Ley 3/2004: máximo 60 días naturales B2B; 30 por defecto.
Interés de demora y compensación por costes de cobro. `[verificar vigencia del tipo]`.)*

**Protección de datos:** postura básica. *(El detalle —contrato de encargo art. 28— se traspasa a
`/proteccion-datos:revision-encargo`.)*

**Ley aplicable y fuero:** preferida, aceptable, escalar, nunca. *(Roma I; Bruselas I bis;
arbitraje Ley 60/2003.)*

**Triaje de NDA:** ¿qué hace un NDA VERDE / ÁMBAR / ROJO? (mutualidad, plazo, supervivencia,
salvedades, pactos restrictivos, ley aplicable). *(Pactos de no captación/no competencia:
validez limitada; cuidado con Derecho de la competencia y, en su caso, art. 21 ET.)* Registra
en `## Manual` → lado → posturas de triaje de NDA.

**Lo único:** ¿qué cláusula es un rechazo automático en cada lado?

### Parte 3: Escalado (1-2 min)

> ¿Quién puede aprobar qué, y a partir de qué umbral (€) se escala? Dame la matriz: quién aprueba,
> hasta qué importe sin escalar, a quién escala, por qué vía. ¿Qué escala SIEMPRE con
> independencia del importe (responsabilidad sin límite, cesión de PI, lo de una lista 'Nunca')?

### Parte 4: Estilo y documentos semilla (3-4 min)

> Quiero ver: (1) tu plantilla de contrato, (2) tu manual/posturas si las tienes por escrito, (3)
> un par de acuerdos firmados representativos. Aprenderé tu estructura, tu tono y tus posturas
> reales.

También: tono en redlines; quién lee los resúmenes para negocio y qué extensión; dónde va el
producto de trabajo (CLM/Drive/Slack); a qué canal/correo van los avisos de renovación;
`closing_action` para el triaje de NDA y `confirm_routing` para el router.

## Escribir el perfil

Usa el armazón de `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md`. Rellena cada sección con las respuestas y los
semilla. En `## Salidas`, fija la cabecera según el rol (jurista → `CONFIDENCIAL — ANÁLISIS
JURÍDICO INTERNO — SUJETO A SECRETO PROFESIONAL (art. 542.3 LOPJ)`; no jurista → `NOTAS DE
INVESTIGACIÓN — NO CONSTITUYE ASESORAMIENTO JURÍDICO — REVISAR CON ABOGADO/A COLEGIADO/A ANTES DE
ACTUAR`). Marca `[POSTURAS NO PROBADAS]` si faltaron manual o plantilla.

## Tras escribir

**Muestra qué puede hacer el plugin** (lista concreta):

> - **Revisar un contrato contra tu manual** — `/mercantil:revision`
> - **Triar un NDA** — VERDE/ÁMBAR/ROJO — `/mercantil:revision-nda`
> - **Revisar un SaaS** — prórroga, precio, salida de datos, SLA — `/mercantil:revision-saas`
> - **Ver renovaciones próximas** — `/mercantil:seguimiento-renovaciones`
>
> **Mi sugerencia:** ejecuta `/mercantil:revision` sobre un contrato real — es lo más rápido para
> ver si tu manual captura los cortes correctos.

1. **Muestra el resumen.** "El manual es la parte a revisar más a fondo — ¿he captado tus posturas?"
2. **Fuentes:** "Antes de tu primera revisión, ten a mano BOE, CENDOJ y EUR-Lex. Sin ellas, marcaré
   cada cita como no verificada."
3. **Propón primeras tareas** (un contrato de la cola; cargar el registro de renovaciones desde el
   CLM si está conectado).
4. **Marca huecos** (sin plantilla → "primera vez que un cliente pida una, negociarás de cero").
5. **Cierra con "puedes cambiar todo después"** (`--redo`, `--check-integrations`, editar el archivo).

## Modos de fallo

- **No dejes que se salten la pregunta del lado.** Si dudan: "Cuando firmas este contrato, ¿estás
  vendiendo tu producto o comprando el de otro?"
- **No escribas un manual desde posturas genéricas.** Si no han negociado muchos, anótalo:
  `[POSTURAS NO PROBADAS — tratar como puntos de partida, no posturas asentadas.]`
- **No confundas el regulador con el contrato:** "DPA" como autoridad (AEPD) ≠ contrato de encargo.
  La parte de datos se traspasa a `proteccion-datos`.
