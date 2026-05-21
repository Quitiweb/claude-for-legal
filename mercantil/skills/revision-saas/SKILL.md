---
name: revision-saas
description: >
  Referencia: revisión de contratos SaaS / de suscripción con atención a los términos que más
  pesan en estos acuerdos — prórroga automática, escalado de precio, portabilidad de datos, ANS
  (SLA) y derechos sobre subencargados. La carga /mercantil:revision cuando detecta un SaaS o
  suscripción.
user-invocable: false
---

# Revisión de SaaS / suscripción

## Contexto de asunto

Comprueba `## Espacios de asunto`. Si `Activado` es `✗` (por defecto en asesoría interna), omite
este párrafo. *(Espacios de asunto en preparación en esta versión.)*

---

## Propósito

Los contratos SaaS tienen un perfil de riesgo distinto del de un contrato puntual. El dinero se
acumula con las renovaciones, los datos se acumulan y el coste de cambiar crece cada mes. Esta skill
corre la comprobación estándar del manual (la del revision-proveedor) y añade un overlay específico
de SaaS sobre los términos que más muerden.

## Supuesto de jurisdicción

Los términos SaaS (preaviso de prórroga, topes de escalado de precio, portabilidad de datos,
subencargados) son sensibles a la jurisdicción. Si hay un **consumidor** implicado, el régimen cambia
(TRLGDCU). Si el contrato elige Derecho o fuero extranjeros, el análisis puede no transferir; comprueba
**Roma I** y **Bruselas I bis**. Aplica las posturas del CLAUDE.md y márcalo cuando la ley aplicable
difiera.

> **Sin suplir en silencio.** Si la fuente de investigación devuelve poco para una norma imperativa
> que afecte (plazo de pago de la Ley 3/2004, cláusulas abusivas si hay consumidor), informa y para.
> **Etiqueta de fuente** en cada cita: `[BOE]`/`[CENDOJ]`/`[EUR-Lex]`, `[web — verificar]`,
> `[conocimiento del modelo — verificar]`, `[aportado por el usuario]`.

## Carga el manual

**¿Qué lado?** Suele ser obvio: si la contraparte te vende su plataforma, eres compras; si tú eres el
proveedor SaaS, eres comercial. Si no es obvio (reventa, marca blanca), pregunta. Lee la sección del
lado correspondiente; si está `[Sin configurar]`, pide `/mercantil:entrevista-inicial --side <lado>`.

Corre primero todas las comprobaciones estándar del manual (responsabilidad, indemnidad, plazo/
resolución, ley aplicable) como en revision-proveedor. Luego busca, en `## Manual` → lado → posturas
SaaS, las posturas sobre preaviso de prórroga, escaladores de precio aceptables, derechos de
exportación de datos, umbrales de ANS, derechos sobre subencargados y aviso de descatalogación. Esta
skill no trae valores por defecto — los números correctos varían. Si el manual no cubre un término
SaaS que aparece, pregunta y regístralo.

## Overlay SaaS

Para cada categoría, lista lo que encuentres y compáralo con la postura del CLAUDE.md. No apliques
umbrales fijos de esta skill.

### 1. Prórroga automática (tácita reconducción)

La forma más común de que un SaaS salga mal: nadie ve la ventana de preaviso y quedamos atados otro
año a precio más alto. Comprueba contra las posturas SaaS del CLAUDE.md:
- **Duración de la prórroga** (igual, mayor, multianual).
- **Ventana de preaviso para cancelar** (días antes de la renovación).
- **Método de preaviso** (correo, burofax, certificado, portal). *(En España, para acreditar el
  preaviso suele usarse burofax con acuse — tiene tránsito; ver traspaso al registro.)*
- **Precio en la renovación** (igual, IPC, tarifa vigente, discrecional sin tope).

**Extrae y registra** la fecha exacta de renovación y la ventana de preaviso, se marque o no. Esto
alimenta `seguimiento-renovaciones`.

### 2. Escalado de precio
- **Escalador anual** (% fijo, IPC, sin tope).
- **Precio por exceso de uso** (tarifa publicada, premium, sin especificar).
- **Alcance de "honorarios"** (solo suscripción vs. "servicios adicionales" definido en sentido amplio).

### 3. Portabilidad y salida de datos

Cuando (no si) dejemos al proveedor, ¿podemos sacar nuestros datos? Comprueba:
- **Formato de exportación** (abierto/estándar, propietario documentado, "comercialmente razonable").
- **Disponibilidad** (autoservicio, a petición durante el plazo, solo al terminar).
- **Acceso posterior** (días para exportar tras la terminación).
- **Coste de exportación** (gratis, por horas, por GB/registro).
- **Certificación de borrado** (a petición, ninguna, retención de derivados).

La retención de derivados "anonimizados" o "agregados" es una postura material — confirma la del
CLAUDE.md y márcala. *(Recuerda: bajo el RGPD, los datos seudonimizados siguen siendo personales; la
parte de datos personales se traspasa a `/proteccion-datos`.)*

### 4. Disponibilidad y ANS (SLA)

Solo importa si el negocio depende de verdad de que el servicio esté disponible. Si es una
herramienta accesoria, sáltalo — no gastes capital en SLA de una encuesta. Comprueba: compromiso de
disponibilidad (%, o "esfuerzos razonables"); periodo de medición; remedio (créditos — cómo se
calculan, si tienen tope, si son remedio único); exclusiones de mantenimiento; interacción del
crédito-como-remedio-único con el límite de responsabilidad.

### 5. Subencargados

Es una cuestión de protección de datos, pero específica de SaaS porque la lista *cambia* a lo largo
de la suscripción. Comprueba: lista actual (publicada, a petición, no disponible); notificación de
cambios (preaviso, o ninguno); derechos de oposición (bloqueo, aviso-y-resolución, solo aviso,
ninguno). *(El detalle del art. 28 RGPD se traspasa a `/proteccion-datos:revision-encargo`.)*

### 6. Cambios de servicio y descatalogación

Comprueba: cambios materiales adversos (derecho a resolver por degradación material, solo aviso, sin
restricción); plazo de aviso de descatalogación de funciones de las que dependemos; paridad de
funciones en el reemplazo.

## Derechos de IA y aprendizaje automático

**Procedimiento de derechos de datos para IA/ML.** No te limites a comprobar si existe una cláusula de
entrenamiento. Es el punto de negociación emergente nº 1. Trabaja:
1. **Concesión explícita.** ¿El contrato concede al proveedor usar los datos/contenido del cliente
   para entrenar o mejorar modelos? Lado compras: suele ser un NO. Lado comercial: es ingreso si lo
   consigues, riesgo reputacional si abusas.
2. **Concesión implícita vía política.** ¿Incorpora la política de privacidad del proveedor por
   referencia? ¿Puede añadir derechos de entrenamiento por actualización unilateral? Cuidado con
   "mejora del servicio" y definiciones de "datos de uso" que sacan logs/telemetría de la definición
   de datos del cliente.
3. **Estándar de anonimización.** Si solo entrena con datos "anonimizados", ¿cuál es el estándar?
   "Anonimizado" sin definición es débil. *(Bajo el RGPD, anonimización real es un listón alto;
   seudonimización ≠ anonimización.)*
4. **Contaminación competitiva.** ¿Sirve el proveedor a competidores? Entrenar con tus datos podría
   filtrar inteligencia competitiva. ¿Hay compromiso de aislamiento?
5. **Alcance y durabilidad del opt-out.** ¿Cubre todos los usos de IA? ¿Sobrevive a renovaciones y
   cambios de términos? ¿Por usuario o por organización?
6. **Titularidad de los outputs.** Si el producto genera outputs con IA, ¿quién los posee? ¿Puede el
   proveedor usarlos como ejemplos de entrenamiento? Comprueba subencargados de IA de terceros (el
   proveedor puede enviar datos a un LLM de tercero — aparece en la lista de subencargados).
7. **Cadena regulatoria aguas abajo.** ¿El uso de tus datos para IA te crea exposición a ti?
   Obligaciones de implementador bajo el **Reglamento de IA de la UE (Reglamento (UE) 2024/1689)**;
   RGPD. Verifica qué obligaciones del RIA están ya en vigor.

Empareja cada punto con una postura del manual. Si el contrato calla en los siete, eso es un
hallazgo: "Silencio sobre derechos de IA/ML — pide prohibición explícita o una salvedad definida por
cada una de las siete dimensiones."

## Procedimiento de la limitación de responsabilidad

Igual que en revision-proveedor: trabaja las cuatro dimensiones (directos vs. indirectos; base del
límite citada literal; interacción límite-salvedades; postura del manual por dimensión). Recuerda el
límite imperativo del **art. 1102 CC**: el dolo no es excluible; márcalo siempre que el límite lo
barra.

## Comprobación de jurisdicción

Igual que en revision-proveedor: ¿hay consumidor (TRLGDCU)?; ley/fuero extranjeros (Roma I /
Bruselas I bis); pactos restrictivos; exclusiones de responsabilidad (dolo no excluible); plazos de
pago (Ley 3/2004, máximos imperativos). Cuando la postura del manual choque con la ley aplicable,
márcalo.

## Granularidad del redline

**Edita con la menor granularidad posible** (palabra → frase → subapartado → oración → cláusula).
Ante la duda, más pequeño.

## Salida

Usa la estructura de memo de revision-proveedor, con una sección SaaS añadida tras las comprobaciones
estándar del manual. El memo ya lleva la cabecera.

**Doble severidad.** Cada hallazgo SaaS lleva ambos ejes (ver CLAUDE.md `## Doble severidad`):
riesgo legal y fricción de negocio. Salida de datos, prórroga y escalado de precio son los más
propensos a ser 🟢 legal / 🔴 negocio — la cláusula es válida, pero es la razón por la que un cliente
no puede irse o una renovación sorprende a finanzas. Sácalos a la severidad de negocio, no a la legal.

```markdown
### Conclusión
[Puedes firmar / Hay que pelear X primero / Retirarse — una frase del porqué]

### Derechos de IA y aprendizaje automático
[El punto de negociación SaaS nº 1. Marca: cláusulas de entrenamiento explícitas, "mejora del
servicio", definiciones de datos de uso, titularidad de outputs, subencargados de IA, opt-out vs.
opt-in. Si calla: "Silencio sobre IA/ML — pedir prohibición explícita o salvedad definida."]

## Hallazgos SaaS
### Prórroga
**Fecha de renovación:** [fecha]
**Ventana de preaviso:** Cancelar antes del [fecha] ([N] días antes)
**Mecanismo de precio:** [como esté redactado]
**Encaje con el manual:** [dentro / desviación / no cubierto]
**Marcar para seguimiento-renovaciones:** [sí — y el registro que necesita]

### Escalado de precio
[hallazgos contra el manual]
### Salida de datos
[hallazgos — esto lo lee el responsable de negocio]
### ANS (SLA)
[hallazgos, o "Omitido — el servicio no es crítico según [interesado]"]
### Subencargados
[hallazgos contra el manual; el detalle de datos → /proteccion-datos]
### Cambios de servicio
[hallazgos contra el manual]
```

## Traspasos

**A seguimiento-renovaciones:** cuando encuentres la fecha de renovación y la ventana de preaviso,
tráspasalas. El registro espera estos campos (esquema completo en
`skills/seguimiento-renovaciones/references/registro-renovaciones.yaml`):

```yaml
contraparte:          [nombre]
acuerdo:              [título]
fecha_firma:          [fecha ISO]
fin_plazo_inicial:    [fecha ISO]
mecanismo_renovacion: [p. ej., "prórroga tácita anual"]
dias_preaviso:        [entero]
metodo_preaviso:      [correo / burofax / certificado / portal / según contrato §X]
buffer_transito_dias: [0 electrónico; 3-5 burofax/certificado]
preaviso_efectivo:    [fecha ISO — fin de plazo menos días de preaviso, ajustado a día hábil]
precio_renovacion:    [mecanismo como esté redactado]
valor_anual:          [entero, si consta]
responsable_negocio:  [correo, si se conoce]
clm_id:               [id si está disponible]
estado:               activo
```
Si un campo no es determinable, déjalo fuera y anota cuáles faltan para que una persona los rellene.

**A escalado:** si alguna comprobación SaaS toca la lista "nunca aceptamos" o un disparador
automático del CLAUDE.md, `escalado` lo encamina.

## Qué pelear

Los proveedores SaaS, sobre todo los grandes, negocian su papel con poca disposición. Elige batallas
*según el manual* — la sección de posturas SaaS debería distinguir términos en los que el equipo
siempre empuja, los que pelea solo en operaciones materiales, y los que deja pasar. Calibra por valor
y coste de cambio: una herramienta de 5.000 €/año con alternativas fáciles recibe menos que una
plataforma de 500.000 €/año sobre la que construiremos.

## Cierra con el árbol de próximos pasos

Cierra con el árbol según `## Salidas`, personalizado a lo producido.
