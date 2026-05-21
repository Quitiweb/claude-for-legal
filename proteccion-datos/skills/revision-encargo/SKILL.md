---
name: revision-encargo
description: >
  Revisa un contrato de encargo de tratamiento (art. 28 RGPD) contra tu manual — detecta
  automáticamente si eres encargado (el cliente te envía su contrato) o responsable (el
  proveedor) y aplica la mitad correcta del manual. Úsala cuando el usuario diga "revisa este
  encargo", "comprueba este contrato de encargo", "el cliente ha mandado su DPA", "¿está bien
  este encargo?", o adjunte un contrato de encargo / DPA.
argument-hint: "[archivo | enlace de Drive | pega el texto]"
---

# /revision-encargo

1. Carga `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md` → Manual de
   encargos. Si hay marcadores, detente y propón configuración.
2. Consigue el contrato. Determina la dirección: ¿somos encargado (contrato del cliente) o
   responsable (del proveedor)? Pregunta si es ambiguo.
3. Ejecuta el flujo — cláusula a cláusula contra la fila correcta del manual.
4. Comprobación de coherencia con la política.
5. Salida: memorándum de revisión con redlines. Guarda según el estilo de la casa.

```
/proteccion-datos:revision-encargo encargo-cliente.pdf
```

---

# Revisión de contrato de encargo (art. 28 RGPD)

## Contexto de asunto

Comprueba `## Espacios de asunto`. Si `Activado` es `✗` (por defecto en asesoría interna), omite
este párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Propósito

Los contratos de encargo vienen en dos sabores y la revisión es casi opuesta en cada uno. Cuando
un cliente envía su encargo, somos el **encargado** y defendemos nuestra flexibilidad operativa.
Cuando lo enviamos a un proveedor, somos el **responsable** y protegemos nuestros datos (y los de
nuestros clientes). Ambas leen del mismo manual del CLAUDE.md pero de filas opuestas.

## Primero: ¿qué dirección?

- **Somos el encargado** → el cliente nos envía su contrato → lee `## Manual de encargos →
  Cuando somos el encargado`.
- **Somos el responsable** → enviamos un contrato a un proveedor (o revisamos el suyo) → lee
  `Cuando somos el responsable`.

Si no está claro, pregunta. Equivocarse invierte cada recomendación.

## Supuesto de jurisdicción

Esta revisión asume el ámbito de tu configuración (RGPD + LOPDGDD). Si responsable, encargado o
interesados están en otra jurisdicción, puede no aplicar tal cual.

## Carga el contexto previo de esta contraparte

Revisa la carpeta de salidas por trabajo previo sobre esta contraparte o tratamiento (ruta en
`## Salidas`): triajes (valoración y condiciones que esta revisión debe honrar o apartarse
explícitamente), EIPD (medidas que el encargo debe implementar), revisiones de encargo previas
(qué se aceptó, qué se marcó). Cita lo previo y **arrastra la severidad como suelo**. Si no hay
nada, dilo.

## Carga el manual

Lee `## Manual de encargos` y `## Compromisos de la política de privacidad` — el encargo no puede
prometer algo que la política contradiga.

## Recubrimiento sectorial (pregunta antes del repaso cláusula a cláusula)

¿Los datos que circulan por este encargo incluyen alguna categoría con régimen sectorial español?
El RGPD/LOPDGDD pone un suelo; la norma sectorial suele añadir otro que no aparece en el manual
genérico.

> ¿El tratamiento toca…
> - **Datos de salud / historia clínica** (Ley 41/2002; categoría especial art. 9)? → el encargo
>   necesita medidas reforzadas y, en su caso, base habilitante específica.
> - **Datos de menores** (consentimiento desde 14 años, art. 7 LOPDGDD)?
> - **Solvencia / ficheros de morosidad** (art. 20 LOPDGDD)?
> - **Datos penales** (art. 10)? → tratamiento muy restringido; comprobar habilitación.
> - **Sector público / ENS** (Esquema Nacional de Seguridad — niveles y medidas)?
> Si sí a alguno: investiga y cita la disposición; marca los huecos sectoriales junto a los del
> RGPD.

Si no aplica recubrimiento, dilo: "no se identifican categorías con régimen sectorial; n/a."

## Repaso cláusula a cláusula

### Contenido mínimo del art. 28.3 (compruébalo en todo encargo)

El art. 28.3 RGPD exige que el contrato regule, como mínimo: objeto, duración, naturaleza y
finalidad; tipo de datos y categorías de interesados; obligaciones del responsable; y que el
encargado: (a) trate solo según **instrucciones documentadas**; (b) garantice el **deber de
confidencialidad** del personal; (c) aplique **medidas de seguridad** (art. 32); (d) respete las
condiciones para **subencargados** (art. 28.2 y 28.4); (e) **asista** al responsable con los
derechos de los interesados; (f) asista en seguridad, brechas y EIPD/consulta previa; (g)
**suprima o devuelva** los datos al terminar; (h) ponga a disposición la información para
**auditorías** y permita inspecciones.

Las posturas *numéricas y sustantivas concretas* (plazos de aviso, ventanas de notificación,
suelos aceptables) vienen del `## Manual de encargos`. Los suelos regulatorios vienen de la norma
primaria — **investiga la regla vigente** y cita fuentes antes de afirmar un suelo.

> **Sin suplir en silencio.** Si la fuente de investigación devuelve poco para una ventana de
> brecha, un mecanismo de transferencia o una regla de subencargados, informa y para.
> **Etiqueta de fuente** en cada cita: `[BOE]`/`[EUR-Lex]`/`[AEPD]`, `[web — verificar]`,
> `[conocimiento del modelo — verificar]`, `[aportado por el usuario]`, o `[asentado — última
> comprobación AAAA-MM-DD]` para referencias estables comprobadas.

| Cláusula | Qué buscar | Campo del manual | Peleas habituales |
|---|---|---|---|
| **Roles** | Designación clara responsable/encargado; coincide con la realidad | — | Etiquetan "corresponsables" cuando no lo son |
| **Alcance / instrucciones** | Limitado a instrucciones documentadas; finalidades definidas | — | Ampliadores abiertos ("y fines relacionados") |
| **Subencargados** | Lista actual; mecanismo de cambio (art. 28.2/28.4) | Cambios de subencargados | Autorización en bloque vs. veto vs. solo aviso |
| **Seguridad** | Anexo con controles o estándar concreto (art. 32) | Estándar de seguridad | "Medidas técnicas y organizativas apropiadas" sin anexo = promesa vacía |
| **Brechas** | Disparador definido y plazo | Notificación de brechas | El encargado debe avisar al responsable **sin dilación indebida**; el responsable notifica a la AEPD en **72 h** (art. 33). "Sin dilación" sin plazo es vago |
| **Auditoría** | Método (informe/certificación vs. presencial), frecuencia, aviso, coste | Derechos de auditoría | Auditoría presencial con aviso corto |
| **Transferencias internacionales** | Mecanismo del Cap. V identificado; garantías; evaluación de transferencia | Transferencias | Mecanismos ausentes o caducos |
| **Supresión/devolución** | Plazo tras terminar; certificación; salvedad de copias de seguridad | Supresión al terminar | "Supresión comercialmente razonable" = ¿? |
| **Responsabilidad** | Dentro del límite del contrato marco o separada; salvedades | Responsabilidad por los datos | Responsabilidad por brecha sin límite = existencial |

### Cuando somos el encargado: revisión defensiva

El encargo del cliente intenta empujarnos carga operativa. Para cada cláusula, compara con el
manual; donde el cliente pida fuera del manual, vuelve a la postura estándar y prepárate para la
alternativa aceptable.

| Cláusula | Riesgo | Búsqueda / manual |
|---|---|---|
| Veto por subencargado | No podemos añadir infraestructura sin aprobación cliente a cliente | Postura de cambios de subencargados |
| Auditoría presencial con aviso corto | Inviable a escala | Postura de auditoría |
| Ventana de brecha agresiva | Suele exigir avisar antes de saber qué pasó | Investiga el suelo regulatorio; compara con el manual |
| Residencia de datos rígida (un solo país/CPD) | Puede no encajar con la arquitectura | Postura de ubicación; confirma qué puedes comprometer |
| Responsabilidad de encargado sin límite | Apuesta la empresa | Postura de responsabilidad |
| El cliente puede dar "instrucciones" vinculantes | Control operativo abierto | Define instrucciones como "documentadas en el contrato o pactadas por escrito" |
| Supresión en plazo muy corto | La rotación de copias lo hace imposible | Postura de supresión; documenta salvedad de backups |

### Cuando somos el responsable: revisión protectora

El encargo del proveedor intenta no darnos nada.

| Cláusula | Hueco | Búsqueda / manual |
|---|---|---|
| Sin lista de subencargados | No sabemos quién toca nuestros datos | Exige lista actual publicada + aviso previo |
| "Seguridad estándar del sector" | No significa nada | Exige anexo con controles concretos o estándar nombrado (ISO 27001, ENS, SOC 2) |
| Sin plazo de notificación de brechas | Nos avisan cuando quieran | Investiga el suelo; exige la postura del manual |
| Sin derechos de auditoría | No podemos verificar nada | Exige al menos un informe/certificación independiente |
| El proveedor usa los datos para "mejora del servicio" | Posible uso/entrenamiento sobre nuestros datos | Táchalo; tratamiento limitado a prestarnos el servicio |
| Sin mecanismo de transferencia internacional | No hay base lícita de transferencia | **Investiga el mecanismo vigente** del corredor (origen/destino, adecuación, CCT 2021/914, garantías + evaluación). Cita fuentes |
| Sin compromiso de supresión | Los datos viven para siempre | Exige supresión + certificación a petición |

## Coherencia con la política de privacidad

El encargo que firmes no puede prometer algo que la política no cubra, y viceversa. Comprueba
finalidades, "no cedemos datos" vs. cláusulas de cesión, categorías de subencargados nombradas.
Marca desajustes (suele ser la política desactualizada, pero alguien debe arreglar uno de los
dos).

## Granularidad del redline

**Edita con la menor granularidad posible.** Un redline es un artefacto de negociación, no una
reescritura. Sustituir cláusulas enteras señala "tiramos tu redacción" — es agresivo y obliga a
releer todo. Edición quirúrgica = "tenemos peticiones concretas". Por defecto: sustituye una
**palabra** antes que una frase; una **frase** antes que una oración; reestructura un
**subapartado** antes que sustituir la oración; sustituye la **cláusula entera** solo cuando esté
tan lejos de tu postura que la edición quirúrgica sea más difícil de leer — y entonces, dilo en
la transmisión. Ante la duda, más pequeño.

## Salida

Antepón la cabecera de documento de trabajo del config (`## Salidas`).

```markdown
[CABECERA DE DOCUMENTO DE TRABAJO — según config ## Salidas]

# Revisión de encargo: [contraparte]

**Dirección:** [Somos encargado / Somos responsable]
**Revisado:** [fecha]
**Anexo a:** [contrato marco / autónomo]

---

## Conclusión
[Dos frases. ¿Podemos firmar? ¿Qué tiene que cambiar?]
**Incidencias:** [N]🟢 [N]🟡 [N]🟠 [N]🔴

---

## Cláusula a cláusula
[Por cada cláusula del núcleo: qué dice el encargo, qué dice el manual, el hueco, el riesgo y el
redline propuesto. Bloques breves y autocontenidos.]

---

## Coherencia con la política
[🟢 Coherente | 🟡 Marcas: lista]

---

## Redlines recomendados
[Consolidados — listos para devolver]

---

## Si no se mueven
[Por cada incidencia: la alternativa del config, o el escalado si no hay alternativa]
```

## Nota de transferencias internacionales

Si el encargo contempla transferencias fuera del EEE, **investiga los requisitos vigentes** del
corredor: régimen aplicable, decisión de adecuación en vigor, mecanismo requerido/disponible
(Cláusulas Contractuales Tipo y su versión/módulo, normas corporativas vinculantes, garantías
del art. 46, excepciones del art. 49), si hace falta evaluación de transferencia y medidas
complementarias. Cita fuentes primarias y verifica vigencia (adecuación, versiones de CCT y
medidas cambian). Si falta mecanismo y hay transferencia, es un 🔴 — no hay base lícita.

## Puerta: firmar un encargo

Revisar es investigación. **Firmar** —o instruir a alguien para que lo haga— es el acto
consecuente.

**Antes de firmar o instruir la firma:** lee `## Quién usa esto`. Si el Rol es No jurista:

> Firmar un encargo es un acto jurídico — vincula a la organización a obligaciones de protección
> de datos que alcanzan a la AEPD y a los interesados. ¿Lo has revisado con un abogado/a? Si sí,
> adelante. Si no, aquí tienes un resumen de una página:
>
> [Genera un resumen: contraparte, dirección, cláusulas que se desvían del manual y cómo se
> resolvieron, decisiones de alternativa abiertas, y las tres cosas que preguntar antes de
> ejecutar.]
>
> Para encontrar abogado/a: el Servicio de Orientación Jurídica (SOJ) de tu Colegio de la
> Abogacía es el punto de partida más rápido.

No pases esta puerta sin un sí explícito.

## Cierra con el árbol de próximos pasos

Cierra con el árbol según `## Salidas`, personalizado a lo producido.

## Lo que esta skill NO hace

- No redacta un encargo de cero. Si la respuesta es "usa nuestra plantilla", tira de la ruta de
  documentos semilla del config.
- No hace la evaluación de transferencia — marca cuándo hace falta.
- No decide aceptar términos fuera de las alternativas. Los encamina por la tabla de escalado.
