---
name: triaje-tratamiento
description: >
  Determina rápidamente si un tratamiento necesita una EIPD obligatoria (art. 35 RGPD), una
  EIPD recomendada, o puede seguir — saca a la luz conflictos con la política de privacidad y
  encamina al siguiente paso. Úsala cuando el usuario pregunte "¿esto necesita EIPD?", "tría
  este tratamiento", "revisión de privacidad de X", "¿esto está bien desde privacidad?", o
  describa un tratamiento, función de producto o relación con un proveedor nuevos.
argument-hint: "[describe el tratamiento o la función]"
---

# /triaje-tratamiento

1. Lee `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md`. Confirma que la
   práctica está configurada — si no, detente y dirige a la configuración.
2. Ejecuta el flujo. Aclara el tratamiento si es vago.
3. Disparador interno → comprobación de EIPD obligatoria (art. 35 + listas AEPD) → conflicto
   con la política.
4. Salida: clasificación (PROCEDE / EIPD RECOMENDADA / EIPD OBLIGATORIA / DETENER), motivos,
   tabla de condiciones si procede, traspasos.
5. Ofrece continuar con la generación de la EIPD si hace falta evaluación.

```
/proteccion-datos:triaje-tratamiento "Nueva función que usa datos de comportamiento para personalizar recomendaciones"
```

---

# Triaje de tratamientos

## Contexto de asunto

Comprueba `## Espacios de asunto` en el CLAUDE.md de práctica. Si `Activado` es `✗` (por
defecto en asesoría interna), omite este párrafo — las skills usan el contexto a nivel de
práctica. Si está activado y no hay asunto activo, pregunta a qué asunto corresponde antes del
trabajo sustantivo. *(La gestión de espacios de asunto está en preparación en esta versión.)*

---

## Comprobación de destino

Antes de producir la salida, comprueba a dónde va. Si el usuario nombra un destino (un canal,
una lista, una contraparte, "todos"), pregunta si está dentro del círculo de confidencialidad.
Distribuir un análisis interno ampliamente pierde la confidencialidad de facto y expone el
análisis (incluso a un eventual requerimiento de la AEPD). Cuando el destino parezca fuera del
círculo, márcalo y ofrece (a) la versión interna, (b) una versión depurada, o (c) ambas. Ver
`## Salvaguardas comunes → Comprobación de destino` en el CLAUDE.md.

## Propósito

Responder la pregunta que surge antes de que nadie haga una EIPD: "¿esto necesita siquiera
una?" Y si la necesita, de qué tipo y qué la bloquea.

El triaje es más rápido que la EIPD pero está aguas arriba. No escribe la evaluación —
determina si hace falta y en qué términos. La salida es una de cuatro clasificaciones:

- **PROCEDE** — No hace falta EIPD. Aplican las salvaguardas estándar.
- **EIPD RECOMENDADA** — Conviene una evaluación antes o junto al despliegue (alto riesgo
  probable, pero sin disparador obligatorio claro).
- **EIPD OBLIGATORIA** — El art. 35 RGPD y/o la lista de la AEPD (art. 35.4) la exigen.
  Probable implicación del DPD; si el riesgo residual sigue alto, consulta previa a la AEPD
  (art. 36).
- **DETENER** — El tratamiento choca con la política de privacidad o carece de base de
  legitimación tal como se describe. Necesita rediseño antes de seguir.

## Supuesto de jurisdicción

Este triaje asume el ámbito de tu configuración (RGPD + LOPDGDD, interesados en España/EEE). Si
el tratamiento, el responsable o los interesados caen bajo otra jurisdicción, la clasificación
puede no aplicar tal cual. Ver `## Reconocimiento de jurisdicción` en el CLAUDE.md.

## Lee primero el config

Antes de triar, lee `~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md`. Los
criterios de disparo de EIPD, el marco normativo y los compromisos de la política son
autoritativos. El razonamiento genérico de protección de datos no sustituye a lo que esta
organización ha comprometido de verdad.

Si el archivo falta o tiene `[PLACEHOLDER]`:

> No has configurado tu perfil aún — así adapto los criterios de EIPD, el marco normativo y los
> compromisos de tu política.
>
> **Dos opciones:**
> - Ejecuta `/proteccion-datos:entrevista-inicial` (2 min) y luego trío adaptado a TU práctica.
> - Di **"provisional"** y trío contra valores genéricos (España, RGPD+LOPDGDD, riesgo medio,
>   rol jurista, sin manual), etiquetando todo `[PROVISIONAL — configura tu perfil]`.

## Proceso de triaje

### Paso 1: Entiende el tratamiento

Si la descripción es vaga, pregunta antes de clasificar. Concreta:
- ¿Qué datos se recogen o tratan? ¿Qué categorías? ¿Hay **categorías especiales** (art. 9:
  salud, biometría, ideología, religión, afiliación sindical, orientación/vida sexual, origen
  étnico) o **datos penales** (art. 10)?
- ¿Quiénes son los interesados — clientes, empleados, terceros, **menores**?
- ¿Cuál es la finalidad? ¿Qué problema resuelve?
- ¿Es recogida nueva, o reutilización de datos que ya tienes (cambio de finalidad)?
- ¿Interviene un proveedor (encargado)? ¿Nuevo o existente?
- ¿Hay **decisiones automatizadas o elaboración de perfiles** (art. 22) que afecten a alguien?
- ¿Contexto de despliegue — interno, de cara a cliente, público?

"Función nueva" y "tratamiento de datos" no bastan para triar bien.

### Paso 2: Comprueba el disparador interno

Lee `## Estilo de EIPD` → Disparador. Aplícalo. Si el disparador interno se cumple → como
mínimo **EIPD RECOMENDADA**. Si no, sigue al Paso 3 antes de concluir PROCEDE.

### Paso 3: Comprobación de obligatoriedad (art. 35 RGPD + listas AEPD)

**Antes de los criterios generales, haz la pregunta de los recubrimientos sectoriales
españoles.** Si el tratamiento toca una categoría regulada de forma específica, esa norma
sectorial suele ser el marco controlante, no solo el RGPD general:

> **Recubrimientos sectoriales — pregunta primero:**
> ¿El tratamiento toca…
> - **Datos de salud / historia clínica** (Ley 41/2002 de autonomía del paciente; categoría
>   especial art. 9 RGPD; normativa autonómica)?
> - **Datos de menores** (consentimiento directo a partir de **14 años** en España, art. 7
>   LOPDGDD; por debajo, consentimiento de titulares de la patria potestad; art. 8 RGPD)?
> - **Solvencia patrimonial / sistemas de información crediticia** (art. 20 LOPDGDD; requisitos
>   específicos de inclusión en ficheros de morosos)?
> - **Datos penales o de infracciones/sanciones** (art. 10 RGPD; art. 10 LOPDGDD — tratamiento
>   muy restringido)?
> - **Videovigilancia / control laboral / geolocalización de empleados** (art. 22 LOPDGDD;
>   arts. 87-90 LOPDGDD sobre derechos digitales en el ámbito laboral; criterios de la AEPD)?
> - **Comunicaciones electrónicas, cookies o marketing** (LSSI art. 21-22; Guía de cookies de
>   la AEPD)?
> - **Otro régimen sectorial** (seguros, telecomunicaciones, sector público — esquemas del ENS)?
>
> Si sí a alguno: investiga y cita la disposición concreta antes de seguir. La norma sectorial
> suele añadir restricciones que el marco general no recoge.

Para cada régimen del `## Marco normativo aplicable`, **investiga los disparadores obligatorios
vigentes de EIPD**. Cita norma o criterio con referencia precisa. La AEPD publica y revisa las
**listas del art. 35.4** (tratamientos que requieren EIPD) y del **art. 35.5** (los que no).
Como criterio práctico (Directrices WP248/EDPB), suele exigirse EIPD cuando concurren **dos o
más** criterios de alto riesgo. No te fíes de un checklist estático: verifica vigencia.

Si **cualquier** régimen aplicable cumple el disparador obligatorio → **EIPD OBLIGATORIA**,
con independencia del disparador interno.

**Indicadores fuertes (no necesariamente obligatorios, pero conviene hacerla):**
- Tecnología nueva o uso novedoso de tecnología existente.
- Datos de menores.
- Combinar conjuntos de datos no recogidos juntos.
- Datos que podrían permitir discriminación.
- Tratamientos que el interesado no esperaría.
- Publicidad comportamental, audiencias similares, seguimiento entre sitios (recurrente en
  empresas de consumo; saca conflictos de política y recubrimientos sectoriales).

Uno o más indicadores fuertes sin disparador obligatorio investigado → escala a **EIPD
RECOMENDADA** (y márcalo).

### Paso 4: Conflicto con la política de privacidad

Lee `## Compromisos de la política de privacidad`. Contrasta el tratamiento con cada
compromiso. Conflictos típicos: recoger una categoría no anunciada; ceder a un proveedor para
fines propios cuando la política dice que no se cede; conservar más de lo declarado; usar para
una finalidad nueva sin nueva base; crear una categoría que el proceso de derechos no cubre.

Si hay conflicto directo → **DETENER**. No "procede con cautela": el conflicto debe resolverse
(actualizar la política o rediseñar) antes de seguir.

### Paso 5: Clasificación y salida

---

### Conclusión
[EIPD obligatoria / EIPD recomendada / Procede — una frase del porqué]

---

**TRATAMIENTO:** [enúncialo como lo entiendes]
**CLASIFICACIÓN:** [PROCEDE / EIPD RECOMENDADA / EIPD OBLIGATORIA / DETENER]
**¿Disparador interno?** [Sí / No]
**¿Disparador obligatorio art. 35 / lista AEPD?** [Sí — [criterio] / No]
**¿Conflicto con la política?** [Ninguno / Sí — [conflicto concreto]]

**Motivos:** [1-3 frases.]

---

*Si EIPD RECOMENDADA u OBLIGATORIA — condiciones antes de seguir:*

| Requisito | Responsable | ¿Hecho? |
|---|---|---|
| [EIPD en formato completo] | [Privacidad / DPD] | ☐ |
| [Juicio de ponderación de interés legítimo, si esa es la base] | [Privacidad] | ☐ |
| [Consulta al DPD] | [DPD] | ☐ |
| [Contrato de encargo (art. 28) firmado] | [Privacidad / Jurídico] | ☐ |
| [Actualización de la política antes del lanzamiento] | [Privacidad] | ☐ |
| [Mecanismo de consentimiento construido y probado] | [Producto] | ☐ |
| [Proceso de derechos cubre la nueva categoría] | [Privacidad / Producto] | ☐ |
| [Inscripción en el RAT (art. 30)] | [Privacidad / DPD] | ☐ |

**Base de legitimación (art. 6):** [Consentimiento / Contrato / Interés legítimo / Obligación
legal / Misión de interés público — o "sin determinar — se decide en la EIPD"]

**Siguiente paso — ofrece continuar:**

Tras un resultado EIPD RECOMENDADA u OBLIGATORIA, cierra siempre con:
> "¿Quieres que empiece la EIPD ahora? Puedo lanzar el intake y producir el documento sin que
> ejecutes otro comando."

Si dice sí, carga la skill `evaluacion-impacto` y continúa en la misma conversación, pasando la
descripción y los disparadores ya identificados. Si dice no, el triaje queda en pie; la EIPD se
puede ejecutar luego con `/proteccion-datos:evaluacion-impacto [tratamiento]`.

---

*Si DETENER:*

**Conflicto:** [compromiso o principio concreto en conflicto]
**Para seguir, una de estas tiene que cambiar:**
- [Opción A — rediseñar el tratamiento para que no cree el conflicto]
- [Opción B — actualizar la política para cubrirlo (revisar si la actualización es a su vez
  coherente con la base de legitimación)]

No ofrezcas un camino si no lo hay. Si el tratamiento no se reconcilia con los compromisos o
la base de licitud, dilo.

### Paso 6: Traspasos

**IA:** si el tratamiento implica un sistema de IA que decide o influye en decisiones sobre
personas: "Esto implica IA. Probablemente haga falta evaluar el encaje con el **Reglamento de
IA de la UE (Reglamento (UE) 2024/1689)** además de la EIPD — no son sustitutos. Verifica qué
obligaciones del RIA están ya en vigor y la posición de AEPD/AESIA."

**Producto:** si es una función o lanzamiento nuevo, recuerda involucrar a quien lleve producto.

Solo marca traspasos realmente relevantes. No los pongas ambos como relleno.

---

## Triaje por lotes

Si el usuario presenta una lista de funciones o un backlog — tabla resumen primero, luego
expande cada entrada que no sea PROCEDE:

| # | Tratamiento | Clasificación | Condición / bloqueo clave |
|---|---|---|---|
| 1 | [tratamiento] | 🟢 Procede | — |
| 2 | [tratamiento] | 🟡 EIPD recomendada | Falta juicio de base de legitimación; sin encargo |
| 3 | [tratamiento] | 🟠 EIPD obligatoria | Categorías especiales a gran escala |
| 4 | [tratamiento] | 🔴 Detener | Conflicto con la política — limitación de finalidad |

---

## Casos límite y modos de fallo

**"Está anonimizado" no implica PROCEDE automático.** Pregunta cómo se anonimiza y si la
reidentificación es realista. Los datos **seudonimizados siguen siendo datos personales** bajo
el RGPD.

**"Ya hacemos algo parecido" no es un triaje.** Un tratamiento previo nunca evaluado no
convalida uno nuevo. Si el nuevo difiere en escala, finalidad o categoría, tríalo de cero.

**"Es solo un piloto" no se salta el triaje.** Un piloto con datos reales está sujeto a los
mismos disparadores.

**"El proveedor lleva toda la privacidad."** El proveedor lleva la infraestructura. Tú sigues
siendo el responsable que determina las finalidades. Si fluyen datos personales al proveedor,
hace falta contrato de encargo (art. 28) y el triaje aplica a la finalidad.

**Los datos inferidos cuentan.** Si el tratamiento genera un dato inferido (un *scoring*, una
preferencia predicha), trátalo como dato personal. No dejes que "solo calculamos una
puntuación" oculte lo que representa.

## Cierra con el árbol de próximos pasos

Cierra con el árbol de próximos pasos según `## Salidas` del CLAUDE.md. Personaliza las opciones
a lo que esta skill acaba de producir. El árbol es la salida; el abogado elige.
