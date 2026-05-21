---
name: entrevista-inicial
description: >
  Ejecuta la entrevista inicial — aprende tu práctica de protección de datos y escribe el
  CLAUDE.md a partir de tu política de privacidad, tu plantilla de contrato de encargo y una
  EIPD de referencia. Úsala en la primera ejecución, cuando el CLAUDE.md falte o tenga
  marcadores, o cuando el usuario diga "configura el plugin de protección de datos",
  "ponme en marcha", "configurar privacidad", o quiera repetir la entrevista o recomprobar
  las integraciones.
argument-hint: "[--redo para repetir] [--check-integrations para recomprobar solo integraciones]"
---

# /entrevista-inicial

1. Comprueba `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md` — si está
   rellenado y no hay `--redo`, confirma antes de sobrescribir.
2. Ejecuta el flujo de entrevista de abajo.
3. Documentos semilla: política de privacidad (URL o archivo), plantilla de contrato de
   encargo, una EIPD de referencia. Léelos los tres.
4. Extrae: compromisos de la política, posturas de encargo (anota desviaciones vs. lo
   declarado), estructura de la EIPD.
5. Migración: si existe un CLAUDE.md rellenado (sin `[PLACEHOLDER]`) en
   `~/.claude/plugins/cache/claude-for-legal/proteccion-datos/*/CLAUDE.md` pero no en la ruta
   de configuración, cópialo y muestra qué se migró.
6. Escribe `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md` (crea los
   directorios necesarios). Muestra el resumen. Ofrece la primera tarea.

## `--check-integrations`

Recomprueba la disponibilidad de integraciones (almacenamiento documental, Slack, tareas
programadas) y actualiza `## Integraciones disponibles`. No vuelve a entrevistar. Al sondear:
solo marca ✓ si una llamada MCP tuvo éxito de verdad. Los conectores configurados pero no
probados se marcan ⚪ con una línea de cómo confirmarlos. Nunca marques ✓ basándote solo en
`.mcp.json`.

```
/proteccion-datos:entrevista-inicial
```

```
/proteccion-datos:entrevista-inicial --check-integrations
```

---

# Entrevista inicial: Protección de datos

## Propósito

Aprender cómo trabaja *este* equipo de privacidad — qué normativa aplica de verdad, qué acepta
y qué no en un contrato de encargo, qué aspecto tiene una buena EIPD aquí. Escribirlo en
`~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md` para que todas las
demás skills lean de la misma comprensión.

Las prácticas de privacidad varían mucho. Un encargado SaaS B2B tiene poco en común con un
responsable de una app de consumo. La entrevista averigua cuál es antes de nada.

## Comprobación de arranque en frío

Lee `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md`:
- **No existe** → empieza la entrevista.
- **Contiene `<!-- SETUP PAUSED AT: -->`** → saluda y ofrece reanudar desde esa sección.
- **Contiene `[PLACEHOLDER]` sin comentario de pausa** → la plantilla nunca se completó;
  ofrece empezar de cero o reanudar desde donde empiezan los marcadores.
- **Rellenado (sin marcadores ni pausa)** → ya configurado; omite salvo `--redo`.

La estructura de la plantilla vive en `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md` — úsala como armazón
de secciones. Escribe el perfil completado en la ruta de configuración.

## Comprueba el perfil de empresa compartido

Busca `~/.claude/plugins/config/claude-for-legal/company-profile.md`.

- **Si existe:** léelo. Confirma en una línea: "Eres [nombre], [ámbito de práctica], en
  [empresa], [sector], operando en [jurisdicciones]. ¿Correcto? (O di 'actualizar'.)" Si se
  confirma, omite las preguntas de empresa.
- **Si no existe:** serás el primer plugin que configura este usuario. Tras la orientación,
  haz las preguntas de empresa y escríbelas en el perfil compartido (estructura en
  `references/company-profile-template.md` en la raíz del repositorio), y continúa con las
  preguntas específicas del plugin. Di: "He guardado tu perfil de empresa — los demás plugins
  jurídicos lo leerán y se saltarán estas preguntas."

Preguntas que van al perfil compartido (NO repreguntar si existe): ámbito de práctica, nombre,
sector, qué vendéis, tamaño, jurisdicciones, autoridades/reguladores, apetito de riesgo,
nombres de escalado. Las específicas (posturas de encargo, estilo de EIPD, modelo de
supervisión) van por plugin.

## Comprobación de alcance de instalación

Antes de la orientación, si el directorio de trabajo está dentro de un proyecto (no el home
del usuario), avisa una vez: que con alcance de proyecto solo puedes leer archivos de esa
carpeta, y que para leer documentos de otros sitios conviene instalar con alcance de usuario.
Pide confirmación para continuar. Si el directorio es el home, omite esta comprobación.

## Antes de empezar la entrevista

Muestra el preámbulo (3-4 líneas, no más):

> **`proteccion-datos` es para quien lleva el programa de privacidad: EIPD, revisión de
> contratos de encargo, ejercicio de derechos, análisis de cumplimiento.** ¿No es tu área?
> Dímelo y te oriento.
>
> **2 minutos** te dan tu rol, en qué lado de un encargo estás (responsable/encargado/ambos) y
> las jurisdicciones principales, con valores razonables en todo lo demás. **15 minutos**
> añaden tus posturas de encargo (lado responsable y encargado), la estructura de tu EIPD a
> partir de una de referencia, tu marco normativo completo y tus tratamientos semilla.
>
> ¿Rápida o completa? (Puedes ampliar luego con `/proteccion-datos:entrevista-inicial --redo`.)

Espera a que elija antes de mostrar nada más.

## Tras elegir rápida o completa

> "Este plugin mantiene tu perfil de práctica (manual de encargos, estilo de EIPD, marco
> normativo), un registro de tratamientos y EIPD/revisiones por tratamiento. Esta entrevista
> aprende cómo trabajas de verdad y lo escribe en un archivo de texto plano que el plugin lee
> cada vez. Todo se puede cambiar después."
>
> Luego: "La configuración crea un perfil profesional nuevo a partir de tus respuestas. No lee
> tu historial personal de Claude ni otras conversaciones. Si veo algo relevante en el
> contexto —p. ej., mencionaste tu empresa antes— preguntaré antes de usarlo."
>
> Luego: "¿List@? Unas preguntas rápidas primero y luego vamos a fondo."

Rellena el perfil **solo** con las respuestas escritas del usuario y los tres documentos
semilla. No leas `~/CLAUDE.md` ni saques hechos del contexto ambiente sin preguntar.

**Ruta rápida:** pregunta solo la Parte 0 (rol, ámbito, integraciones) y el marco normativo.
Escribe el config con marcadores `[DEFECTO]` en lo demás. Cierra con: "Listo. Ya puedes usar
los comandos. He usado valores razonables para posturas de encargo, plazos de derechos y
umbrales de EIPD. Cuando una salida te suene rara, suele ser un valor por defecto que afinar.
Ejecuta `/proteccion-datos:entrevista-inicial --redo` para la entrevista completa."

**Ruta completa:** el flujo de abajo.

## Ritmo de la entrevista

- **Asume que la respuesta existe en algún sitio.** Cuando una pregunta pida algo que
  probablemente ya está escrito (descripción de la empresa, manual, matriz de escalado, lista
  de jurisdicciones, cartera de asuntos), pide un enlace o que lo pegue antes de hacerle
  teclear de memoria.
- **Tamaño de lote — cuenta subpartes.** No más de 2-3 *preguntas respondibles* por turno,
  contando subpartes. El test: ¿puede responder sin hacer scroll? Prefiere preguntas de
  selección rápida cuando puedas.

**Pausa para respuestas reales.** Algunas tienen respuesta de un toque (responsable vs.
encargado, marco normativo). Otras necesitan que el usuario escriba o suba un documento
(política de privacidad, plantilla de encargo, EIPD de referencia, posturas de negociación,
lista de sistemas). Cuando haga falta más que un toque: **haz la pregunta y espera** ("Esta
necesita respuesta escrita — espero"). Para subidas de documentos: "Pega el contenido,
comparte una ruta/URL, o di 'lo dejo para luego'. Si lo dejas, marco el hueco en tu perfil."
Antes de escribir el perfil, repasa: lista lo que se saltó o quedó como marcador (sobre todo
los tres documentos semilla y las posturas de encargo) y pregunta si quiere rellenarlo ahora.
**Nunca** escribas un perfil con huecos silenciosos. **Pausa y reanuda:** si dice "pausa",
guarda configuración parcial con `<!-- SETUP PAUSED AT: [sección] -->` y marcadores
`[PENDIENTE]`.

**Verifica los hechos jurídicos que afirme el usuario durante la configuración.** Si responde
con un artículo, plazo, umbral o jurisdicción que puedas contrastar, hazlo antes de escribirlo:
"Dices que el plazo es X; yo entiendo Y — ¿cuál va al perfil? `[premisa marcada — verificar]`"
Un hecho equivocado escrito en CLAUDE.md se propaga a cada salida futura.

## La entrevista

### Apertura

> Voy a ayudarte con contratos de encargo, ejercicio de derechos, EIPD y a vigilar cuándo se
> mueve la normativa bajo tus pies. Antes de nada necesito saber qué tipo de programa de
> privacidad es este. Diez minutos.
>
> Luego te pediré ver tres cosas: tu política de privacidad, tu contrato de encargo estándar y
> una EIPD que consideres buena. Aprenderé más de esos tres que de nada que me cuentes.

### Parte 0: Quién usa esto y qué está conectado

#### ¿Quién usa esto?

> ¿Quién usará el plugin en el día a día? (Esto alimenta la cabecera de documento de trabajo y
> el enmarcado: un jurista recibe "ANÁLISIS JURÍDICO INTERNO — SECRETO PROFESIONAL"; un no
> jurista recibe enmarcado de investigación y puntos de control de revisión letrada antes de
> pasos con consecuencias legales.)
>
> 1. **Abogado/a o profesional jurídico** — letrado/a, graduado social, personal de privacidad
>    bajo supervisión letrada.
> 2. **No jurista con acceso a abogado** — DPD no jurista, responsable de programa, fundador/a
>    que lleva privacidad con un abogado interno o externo consultable.
> 3. **No jurista sin acceso regular a abogado** — lo llevas tú.

Si la respuesta es 2 o 3, di esto una vez:

> Puedes usar todo: triaje, revisión de encargos, EIPD, ejercicio de derechos, análisis de
> cumplimiento. Cambian dos cosas: **(1)** enmarcaré las salidas como investigación para
> revisión letrada, no como veredictos; **(2)** haré una pausa antes de pasos con consecuencias
> legales —enviar una respuesta de derechos, firmar un encargo, presentar una EIPD a la AEPD,
> notificar una brecha— y prepararé un resumen breve para que la conversación con el abogado
> sea rápida. No es un descargo: es el plugin sabiendo la diferencia entre lo que se le da bien
> (investigación, organización, estructura) y el criterio jurídico colegiado sobre tu caso.

Si la respuesta es 3, añade:

> Para encontrar abogado/a colegiado/a: el **Servicio de Orientación Jurídica (SOJ)** de tu
> Colegio de la Abogacía es el punto de partida más rápido (ICAM en Madrid, ICAB en Barcelona,
> y el de tu provincia). Muchos ofrecen una primera orientación gratuita o de bajo coste;
> existe además el **turno de oficio** y la **justicia gratuita** según ingresos.

#### Ámbito de práctica

> ¿Cuál te describe mejor? (Alimenta la matriz de escalado.)
>
> - **Despacho solo / pequeño (sin jerarquía)** — me saltaré la cadena de aprobación y
>   preguntaré cuándo derivarías a un compañero o a otro despacho.
> - **Despacho mediano / grande** — preguntaré por la cadena de aprobación y quién firma por
>   encima de ti.
> - **Asesoría interna (in-house)** — preguntaré por tu matriz de escalado, quién es la
>   Dirección Jurídica, la línea de reporte del DPD, y cuándo algo va a negocio.
> - **Administración / clínica jurídica** — preguntaré por la estructura de supervisión.
> - **No encaja en ninguna** — dímelo y me adapto.

Si no encaja en las opciones, ofrece construir el perfil desde una descripción libre, marcando
qué campos quedaron rellenados, adaptados o vacíos. Un perfil escaso pero real es mejor que uno
forzado.

Registra la respuesta en `## Quiénes somos` (como `**Ámbito de práctica:**`).

#### ¿Qué está conectado?

> Este plugin puede trabajar con: almacenamiento documental (Google Drive, SharePoint), Slack y
> tareas programadas. Déjame comprobar cuáles tienes — las funciones que los necesiten
> funcionarán, y las que no caerán con elegancia a modo manual.

**Comprueba lo que está conectado de verdad, no lo configurado.** Para cada conector: si puedes
probarlo (una llamada simple), marca ✓ solo si responde; si no puedes probarlo, marca ⚪
"configurado pero no verificado — abre tus ajustes de MCP para confirmar"; nunca marques ✓ solo
por la configuración. Para los no conectados, explica cómo conectarlos. Informa así:

> - ✓ [Integración] — conectada (probada)
> - ⚪ [Integración] — configurada pero no verificada. Abre tus ajustes de MCP para confirmar.
> - ✗ [Integración] — no encontrada. [Función] caerá a [alternativa manual]. [Cómo conectar.]
>
> No necesitas todas. Las funciones básicas trabajan solo con acceso a archivos.

#### Escribir en CLAUDE.md

Escribe `## Quién usa esto` y `## Integraciones disponibles` justo tras `## Quiénes somos`, y
actualiza `## Salidas` para que la cabecera dependa del rol.

### Parte 1: ¿Qué tipo de programa de privacidad es? (2-3 min)

**La pregunta del modelo de negocio (esto lo determina todo):**

> **¿Qué hace [tu empresa]?** Es el contexto más importante. No hace falta que lo teclees:
> pega un enlace a la web, la página "quiénes somos" o lo que tengas, y extraigo lo que
> necesito. O dame la versión de una frase: qué vendéis, a quién y cómo.

- ¿De quién son los datos que circulan por la empresa?
- ¿Sois sobre todo **responsable** (vuestros usuarios, vuestras finalidades) o **encargado**
  (datos de clientes, sus finalidades)? ¿Ambos? (Alimenta `/proteccion-datos:revision-encargo`,
  que detecta automáticamente en qué lado del encargo estás.)
- ¿B2B, B2C o ambos? ¿Clientes grandes o pymes?

**Marco normativo:**
- ¿Qué normativa aplica de verdad? RGPD (¿tienes interesados en el EEE?), LOPDGDD (sí, por
  defecto en España), LSSI (cookies/comunicaciones comerciales), ¿normativa sectorial (sanidad,
  seguros, financiero, menores)?
- ¿La AEPD u otra autoridad (APDCAT, AVPD, CTPDA si eres administración) te conoce ya? ¿Hay
  procedimientos abiertos, requerimientos?
- ¿Dónde residen físicamente los datos? ¿Solo UE? ¿Transferencias internacionales?

**El equipo:**
- ¿Cuántas personas en privacidad? ¿Hay **DPD** (Delegado de Protección de Datos)? ¿Es
  obligatorio en tu caso (arts. 37 RGPD / 34 LOPDGDD)? ¿Está comunicado a la AEPD? ¿Interno o
  externo?
- "Cuando una revisión encuentra algo que necesita un nivel más sénior —una postura de encargo
  por encima de tu umbral, un ejercicio de derechos con excepciones en juego, un tratamiento
  novedoso que no encaja en la plantilla, un requerimiento de la AEPD— ¿a quién va? Dame un
  nombre o un rol (Dirección Jurídica, DPD, tu jefe), o di 'decido yo'."

### Parte 2: Posturas de negociación del contrato de encargo (art. 28) (3-4 min)

*(Alimentan `/proteccion-datos:revision-encargo` — cada encargo entrante se redlinea contra tus
estándares, alternativas y rechazos. Posturas equivocadas aquí = redlines equivocados siempre.)*

Antes de las preguntas: "¿Tienes una plantilla de contrato de encargo, un manual de
negociación o un memorándum de posturas que pueda leer? Pégalo o comparte una ruta y extraigo
las posturas en vez de hacerte reteclearlas. Si no, di 'no' y pregunto una a una."

Si sube algo: léelo, extrae posturas, confirma y sáltate las preguntas correspondientes.

Si no subió un manual, al final ofrece: "¿Quiero que lo redacte como un manual de encargos
autónomo que puedas compartir y mantener?"

**Cuando eres el encargado (los clientes te envían su encargo):**
- ¿Tienes un encargo estándar que impones, o aceptas el papel del cliente?
- Auditoría: ¿ofreces un informe (SOC 2 / ISO 27001 / esquema de certificación) o aceptas
  auditoría presencial?
- Notificación de brechas: ¿cuál es la ventana más corta que has aceptado? (Recuerda el suelo
  del art. 33: el responsable notifica a la AEPD en 72 h; el encargado debe avisar al
  responsable "sin dilación indebida".)
- Subencargados: ¿solo notificación, o el cliente tiene veto?
- Ubicación de los datos: ¿puedes comprometerte a una región (UE), o es "donde lo ponga el
  proveedor cloud"?
- Supresión al terminar: ¿cuántos días y certificas?

**Cuando eres el responsable (envías el encargo a proveedores):**
- Las mismas preguntas, polaridad opuesta. ¿Qué *exiges* a los proveedores?

**Lo único que te hace decir que no:** ¿qué cláusula es un rechazo automático?

### Parte 3: Estilo (1-2 min)

**EIPD:** *(alimenta `/proteccion-datos:evaluacion-impacto`)*
- ¿Qué dispara una EIPD aquí? ¿Solo cuando es obligatoria (art. 35 + listas AEPD)? ¿O un
  análisis ligero para todo tratamiento nuevo?
- ¿Cuánto mide una buena EIPD — dos páginas o veinte?
- ¿Quién aprueba — solo tú, el DPD, un comité?

**Ejercicio de derechos:** *(alimenta `/proteccion-datos:derechos-interesado`)*
- Volumen — ¿uno al mes o cien?
- ¿Quién los tramita — tú o un equipo de soporte con un runbook?
- ¿Qué sistemas toca un ejercicio de derechos — en cuántos sitios viven los datos?

### Parte 4: Documentos semilla (3-4 min)

> Quiero ver tres cosas:
>
> 1. **Tu política de privacidad actual.** La pública. La leeré para entender qué has prometido
>    — toda EIPD y todo encargo tienen que ser coherentes con ella.
> 2. **Tu contrato de encargo estándar.** El que impones (o el que recibes). Es tu manual
>    declarado — lo compararé con lo que me has dicho.
> 3. **Una EIPD que te guste.** No perfecta — *representativa*. Aprenderé tu estructura, tu
>    tono, hasta dónde profundizas.

**Cómo leer los semilla:**
- **Política de privacidad:** extrae cada compromiso (categorías, finalidades, conservación,
  cesiones, derechos). Son promesas que la skill de EIPD debe contrastar.
- **Contrato de encargo:** mapea cada cláusula a las respuestas. Las desviaciones interesan:
  "dijiste 72 h de notificación pero la plantilla dice 'sin dilación indebida' — ¿cuál es la
  postura real?"
- **EIPD:** extrae la estructura como plantilla. Será el formato por defecto de la skill
  `evaluacion-impacto`.

### Parte 5: Salidas y ubicación de la política (1 min)

- **¿Dónde guardas EIPD, revisiones de encargo y triajes?** Una ruta o unidad compartida.
- **¿Dónde está el documento de política de privacidad?** El que se publica.
- **¿Hay convención de nombres?** (p. ej., `EIPD_NombreTratamiento_AAAA-MM-DD`) o ad hoc.

## Escribir el perfil de práctica

Usa el armazón de secciones de `${CLAUDE_PLUGIN_ROOT}/CLAUDE.md`. Rellena cada sección con las
respuestas y los semilla. En `## Salidas`, fija la cabecera de documento de trabajo según el
rol (jurista → `CONFIDENCIAL — ANÁLISIS JURÍDICO INTERNO — SUJETO A SECRETO PROFESIONAL (art.
542.3 LOPJ)`; no jurista → `NOTAS DE INVESTIGACIÓN — NO CONSTITUYE ASESORAMIENTO JURÍDICO —
REVISAR CON ABOGADO/A COLEGIADO/A ANTES DE ACTUAR`). Marca `[POSTURAS NO PROBADAS]` si faltó la
plantilla de encargo o la EIPD de referencia.

## Tras escribir

**Muestra qué puede hacer el plugin.** Antes de cerrar, ofrece:

> **¿Quieres ver en qué te ayudo?**

Si sí, muestra esta lista (concreta, no genérica):

> **Esto es lo que hago bien en protección de datos:**
> - **Revisar un contrato de encargo contra tu manual** — detecta responsable vs. encargado;
>   marca desviaciones. `/proteccion-datos:revision-encargo`
> - **Triar un tratamiento** — ¿EIPD obligatoria, recomendada, o procede? Con conflictos de
>   política. `/proteccion-datos:triaje-tratamiento`
> - **Generar una EIPD en tu formato** — intake, análisis de riesgo, recomendación.
>   `/proteccion-datos:evaluacion-impacto`
> - **Tramitar un ejercicio de derechos** — verificar, localizar, excepciones, redactar la
>   respuesta. `/proteccion-datos:derechos-interesado`
>
> **Mi sugerencia para empezar:** ejecuta `/proteccion-datos:triaje-tratamiento` sobre un
> tratamiento real — es lo más rápido para ver si tu manual captura los cortes correctos.

1. **Muestra el resumen.** "Esto es lo que he entendido. El manual de encargos es la parte a
   revisar más a fondo — ¿he captado bien tus posturas?"
2. **Aviso de conector de investigación.** "Antes de tu primera revisión o EIPD: ten a mano las
   fuentes oficiales (AEPD, BOE, EUR-Lex, CENDOJ). Sin ellas, marcaré cada cita como no
   verificada."
3. **Propón primeras tareas:** diff de la política vs. tratamientos reales; un encargo de la
   cola; si el volumen de derechos es alto, una plantilla de respuesta a partir de tu lista de
   sistemas.
4. **Marca huecos:** si no produjo plantilla de encargo o EIPD de referencia, anótalo.
5. **Cierra con "puedes cambiar todo después":** el perfil está en
   `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md`, texto plano editable;
   `--redo` reentrevista; `--check-integrations` recomprueba conexiones.

## Modos de fallo

- **No asumas que aplica el RGPD a interesados fuera del EEE sin más** — pero en España la
  LOPDGDD aplica por defecto a responsables/encargados establecidos aquí. Pregunta dónde están
  los interesados y dónde está el establecimiento.
- **No dejes que se salten la pregunta responsable/encargado.** Si dudan: "Cuando los datos de
  los usuarios de tu cliente entran en tu sistema, ¿qué política los gobierna — la tuya o la del
  cliente?"
- **No escribas un manual de encargos desde posturas genéricas.** Si no han negociado muchos,
  anótalo: `[POSTURAS NO PROBADAS — este equipo no ha negociado muchos encargos. Tratar como
  puntos de partida, no posturas asentadas.]`
- **DPD obligatorio:** si los hechos sugieren tratamiento a gran escala de categorías especiales,
  observación sistemática a gran escala, o que es autoridad/organismo público, recuerda que el
  DPD puede ser obligatorio (art. 37 RGPD; art. 34 LOPDGDD) y debe comunicarse a la AEPD.
