# Plugin de Mercantil y contratos (España)

Flujos de trabajo de contratación para España, anclados en el **Código de Comercio**, el
**Código Civil** (obligaciones y contratos), la **LCGC** (condiciones generales), la **Ley
1/2019 de Secretos Empresariales**, la **Ley 3/2004 de Morosidad** y los reglamentos europeos
(**Roma I**, **Bruselas I bis**). Revisión de NDA, contratos con proveedores y SaaS contra el
manual de la casa, seguimiento de renovaciones, escalados y resúmenes para negocio.

> Adaptación a España del plugin `commercial-legal` de
> [`claude-for-legal`](../README.md) de Anthropic. **No es una traducción**: las fuentes
> (Westlaw/FTC → BOE/CENDOJ/EUR-Lex), las normas y conceptos como el *attorney work product*
> (→ secreto profesional) están reanclados al Derecho español. La protección de datos se
> traspasa al plugin `proteccion-datos` (contrato de encargo, art. 28 RGPD).

**Cada salida es un borrador para revisión letrada** — citado, marcado y con puertas de
control — no una conclusión jurídica. El plugin lee los documentos, aplica tu manual, encuentra
las desviaciones y redacta. Un abogado/a revisa, verifica y decide. Los actos consecuentes
(enviar redlines, firmar, aceptar una renovación) están detrás de una confirmación explícita.

## Para quién es

| Rol | Flujos principales |
|---|---|
| **Asesoría jurídica de contratos** | Revisión de proveedores/SaaS, redlines, escalado |
| **Responsable de contratos / compras** | Triaje de NDA, seguimiento de renovaciones |
| **Comercial / desarrollo de negocio** | Autoservicio de triaje de NDA antes de pasar a jurídico |
| **Negocio (responsable de presupuesto)** | Resúmenes para negocio |

## Primera ejecución: la entrevista inicial

El plugin te entrevista para aprender: ¿en qué lado estás (comercial/compras)?, ¿cuáles son tus
posturas estándar, alternativas y rechazos?, ¿quién aprueba qué? Después lee tus acuerdos
semilla y aprende tu manual y tu estilo.

Tu configuración se guarda en `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md` y
sobrevive a las actualizaciones.

```
/mercantil:entrevista-inicial
```

## Comandos

| Comando | Hace |
|---|---|
| `/mercantil:entrevista-inicial` | Entrevista inicial / configuración |
| `/mercantil:revision [archivo]` | Detecta el tipo de acuerdo y enruta a la revisión correcta |
| `/mercantil:revision-nda [archivo]` | Triaje de NDA en VERDE / ÁMBAR / ROJO |
| `/mercantil:revision-proveedor [archivo]` | Revisión de contrato con proveedor contra el manual |
| `/mercantil:revision-saas [archivo]` | Revisión de SaaS/suscripción (renovación, precio, datos, SLA) |
| `/mercantil:seguimiento-renovaciones` | Qué renueva pronto y cuándo hay que preavisar |
| `/mercantil:escalado` | Encamina una incidencia al aprobador y redacta la petición |
| `/mercantil:resumen-negocio` | Traduce una revisión a un resumen que negocio leerá |

## Skills

| Skill | Propósito |
|---|---|
| **entrevista-inicial** | Escribe el CLAUDE.md a partir de la entrevista + acuerdos semilla |
| **revision** | Router: identifica la estructura del acuerdo y enruta a la(s) skill(s) |
| **revision-nda** | Triaje rápido de NDA (acuerdo de confidencialidad) en VERDE/ÁMBAR/ROJO |
| **revision-proveedor** | Revisión cláusula a cláusula contra el manual, con redlines |
| **revision-saas** | Overlay SaaS: prórroga, escalado de precio, salida de datos, SLA, subencargados |
| **seguimiento-renovaciones** | Mantiene el registro de renovaciones y avisa antes del preaviso |
| **escalado** | Empareja una incidencia con la matriz y redacta la petición al aprobador |
| **resumen-negocio** | Comprime una revisión en un resumen para el responsable de negocio |

## Inicio rápido

### 1. Configuración
```
/mercantil:entrevista-inicial
```
Ten a mano: tu plantilla de contrato, tu manual de negociación (si lo hay) y un par de acuerdos
firmados representativos.

### 2. Revisar un contrato entrante
```
/mercantil:revision contrato-proveedor.pdf
```
Detecta si es NDA / contrato de servicios / SaaS, enruta y produce un único memorándum:
desviación a desviación, redline concreto, aprobador nombrado.

### 3. Triar un NDA
```
/mercantil:revision-nda nda-contraparte.pdf
```
Salida: VERDE (a firma) / ÁMBAR (un par de cosas para el aprobador) / ROJO (parar, hablar con
jurídico).

### 4. Ver renovaciones próximas
```
/mercantil:seguimiento-renovaciones
```
Qué renueva en los próximos 90 días y la fecha límite de preaviso, en bandas de urgencia.

## Estado de esta versión piloto

Esta es la **v0.1.0** (piloto). Skills incluidas: entrevista-inicial, revision (router),
revision-nda, revision-proveedor, revision-saas, seguimiento-renovaciones, escalado,
resumen-negocio. En la hoja de ruta: **personalizar**, **espacio-asunto** (multicliente),
**historial-modificaciones**, **revision-propuestas** y los agentes programados
(vigía de renovaciones, monitor de manual, debrief de operación).

## Notas

- La revisión es bidireccional: la misma lógica maneja el lado **comercial** (vendemos) y el
  lado **compras** (compramos). El lado se detecta o se pregunta; cambia cada postura del manual.
- La parte de **protección de datos** de un contrato (contrato de encargo, art. 28 RGPD;
  transferencias internacionales) se traspasa a `/proteccion-datos:revision-encargo`.
- Las fuentes oficiales (BOE, CENDOJ, EUR-Lex) están catalogadas en
  `../references/fuentes-oficiales-espana.md`. El glosario EN→ES está en
  `../references/glosario-juridico-en-es.md`.
