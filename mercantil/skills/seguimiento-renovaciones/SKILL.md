---
name: seguimiento-renovaciones
description: >
  Muestra los contratos con fechas límite de preaviso próximas y avisa antes de que se cierren las
  ventanas, trabajando desde un registro de renovaciones mantenido. Úsala cuando el usuario
  pregunte "qué renueva pronto", "qué renovaciones tocan", "¿se nos pasó una ventana de
  cancelación?", "añade esto al seguimiento de renovaciones", o de forma programada. Recibe
  traspasos de revision-saas.
argument-hint: "[--days N para cambiar la ventana | --missed para ventanas vencidas]"
---

# /seguimiento-renovaciones

Saca lo que renueva y cuándo hay que preavisar para cancelar.

## Instrucciones

1. **Lee `~/.claude/plugins/config/claude-for-legal/mercantil/registro-renovaciones.yaml`** (la
   carpeta de configuración — sobrevive a las actualizaciones).
2. **Modo por defecto:** Modo 2 — lo que viene en los próximos 90 días, agrupado por urgencia con
   intervalos semiabiertos para que cada fecha caiga en una sola banda: 🔴 0–13 días, 🟠 14–44 días,
   🟡 45–89 días. Los días 14, 45 y 90 son límites — cada uno pertenece a una sola banda.
3. **`--days N`:** cambia la ventana.
4. **`--missed`:** Modo 4 — fechas de preaviso vencidas sin cancelación registrada.
5. **Si el registro está vacío y el CLM está conectado:** ofrece el Modo 3 — escanear el CLM y
   cargar en bloque.
6. **La salida incluye acciones recomendadas:** a quién avisar (el responsable de negocio de cada
   entrada), cuáles tienen precio sin tope (consigue alternativa antes de perder la palanca).

## Ejemplos

```
/mercantil:seguimiento-renovaciones
```
```
/mercantil:seguimiento-renovaciones --days 180
```

---

## Propósito

Nadie lee un contrato dos veces. La fecha de renovación se extrae una vez, en la revisión, y luego
vive en algún sitio — idealmente uno que te grite 45 días antes de la fecha de preaviso, no 45
después. Esta skill mantiene el registro y saca lo que viene.

## El registro

Vive en `~/.claude/plugins/config/claude-for-legal/mercantil/registro-renovaciones.yaml`. Cada
entrada (esquema completo en `references/registro-renovaciones.yaml`):

```yaml
- contraparte: "Acme SaaS, S.L."
  acuerdo: "Contrato de suscripción a la plataforma Acme"
  fin_plazo_actual: 2026-06-15      # avanza tras cada prórroga; calcula los preavisos desde aquí
  mecanismo_renovacion: "prórroga tácita anual"
  dias_preaviso: 60
  metodo_preaviso: "burofax"        # correo / portal / burofax / certificado / según contrato §X
  buffer_transito_dias: 3           # 0 electrónico; 3-5 burofax/certificado nacional
  preaviso_efectivo: 2026-04-16     # fin_plazo_actual menos dias_preaviso, ajustado a día hábil
  enviar_antes_de: 2026-04-13       # preaviso_efectivo menos buffer_transito_dias — la fecha de ENVÍO
  precio_renovacion: "tarifa vigente (sin tope)"
  valor_anual: 48000
  responsable_negocio: "jane@empresa.com"
  estado: "activo"                  # activo | cancelado | renovado | caducado
  notas: "Precio sin tope — revisar antes de renovar."
```

**Tránsito del preaviso — avisa por `enviar_antes_de`, no por `preaviso_efectivo`.** Una ventana de
60 días con burofax es en realidad ~57 días. Calcula `enviar_antes_de = preaviso_efectivo -
buffer_transito_dias` y dispara los avisos (las bandas 🔴/🟠/🟡 del Modo 2) por `enviar_antes_de`. Una
columna de detalle muestra `preaviso_efectivo`, `metodo_preaviso` y `buffer_transito_dias`.

**Renovaciones rotativas — el registro que no avanza es correcto una sola vez.** Guarda
`fin_plazo_inicial` para el histórico, pero calcula los preavisos desde `fin_plazo_actual`. Cuando una
prórroga se dispara (pasa la ventana sin preaviso), pregunta y actualiza `fin_plazo_actual` y recalcula
`preaviso_efectivo` y `enviar_antes_de`.

## Comprobación de día hábil en cada fecha de preaviso

**La fecha del registro debe ser el último DÍA HÁBIL en que el preaviso es eficaz, no la fecha de
calendario.** Una fecha en fin de semana o festivo es la forma más común de que se pase un plazo.

1. **Calcula la fecha de calendario:** `preaviso_calendario = fin_plazo_actual − dias_preaviso`.
2. **Ajuste a día hábil según la ley aplicable.** En España, sábados, domingos y **festivos
   nacionales y autonómicos/locales** del lugar relevante no son hábiles a estos efectos. Si cae en
   inhábil, retrocede al día hábil anterior. Retrocede SIEMPRE, nunca avances (avanzar = preaviso
   tras cerrarse la ventana). Si no puedes determinar el calendario de festivos local, márcalo:
   "El ajuste usa festivos nacionales como referencia. Verifica el calendario local antes de fiarte."
3. **Comprueba la propia regla del contrato.** Busca "día hábil", "recepción", "se entenderá
   recibido", o una cláusula de método de preaviso. Si el contrato define "día hábil" o el mecanismo
   de recepción (burofax con acuse), esa definición manda. Marca cualquier discrepancia.
4. **Registra ambas fechas** (`preaviso_calendario` crudo; `preaviso_efectivo` último día hábil;
   nota de por qué difieren) y la procedencia `[cálculo del modelo — verificar contra la cláusula]`.
5. **Dispara los avisos por la fecha EFECTIVA (de envío), no la de calendario.**

## Modos

### Modo 1: Ingerir una renovación (traspaso de revisión)
Cuando revision-saas o revision-proveedor encuentran una cláusula de renovación, traspasan un
registro. Añádelo. Si la contraparte ya tiene entrada, pregunta si reemplaza (renovado) o es
adicional.

### Modo 2: Lo que viene (por defecto, 90 días)
Bandas semiabiertas por días hasta `enviar_antes_de`: 🔴 0–13, 🟠 14–44, 🟡 45–89.

```markdown
## Renovaciones — próximos 90 días
### 🔴 Enviar preaviso en 0–13 días
| Contraparte | Enviar antes de | Renovación | Valor anual | Responsable | Notas |
|---|---|---|---|---|---|
| [nombre] | **[fecha]** | [fecha] | [n]€ | [correo] | [notas] |
### 🟠 Enviar en 14–44 días
[misma tabla]
### 🟡 Enviar en 45–89 días
[misma tabla]

**Acciones recomendadas:**
- [ ] [Contraparte] — avisar a [responsable]: ¿lo mantenemos?
- [ ] [Contraparte] — precio sin tope; pide presupuesto alternativo antes de perder palanca
```
Si el registro tiene más de ~10 renovaciones en la ventana, o cuando el usuario lo pida, ofrece el
panel (ver CLAUDE.md `## Salidas → Oferta de panel`): conteos por banda, línea de tiempo de preavisos,
tabla ordenable.

### Modo 3: Escanear el CLM para poblar el registro
Si los MCP están conectados y el registro está vacío o desfasado: consulta el CLM por acuerdos
activos con fecha de renovación; extrae mecánica de renovación; marca aquellos cuya fecha no se pueda
determinar de los metadatos (necesitan que una persona lea el contrato). Carga única; luego la
ingesta ocurre en la revisión.

### Modo 4: Ventanas vencidas (el informe de malas noticias)
```markdown
## Ventanas de preaviso vencidas
| Contraparte | Preaviso vencía | Renovación | Estado |
|---|---|---|---|
| [nombre] | [fecha] | [fecha] | Se prorroga el [fecha] |
**Opciones:**
- Negociar cancelación tardía (rara vez funciona, pero merece preguntar)
- Aceptar la prórroga y marcar ya el preaviso del año que viene
- Revisar otros derechos de resolución (resolución por incumplimiento, art. 1124 CC, si lo hay)
```

## Puerta: aceptar o rechazar una renovación

Seguir una fecha es investigación. *Actuar* — enviar un preaviso de no renovación, dejar que la
prórroga se dispare, o firmar una renovación — es un paso con consecuencias.

**Antes de aceptar o rechazar una renovación (incluido enviar un preaviso de no renovación o dejar que
la prórroga corra pasada la fecha):** lee `## Quién usa esto`. Si el Rol es No jurista:

> Este paso tiene consecuencias legales (te comprometes a otro periodo o terminas la relación). ¿Lo
> has revisado con un abogado/a? Si sí, adelante. Si no, aquí tienes un resumen de una página:
> [contraparte, fin de plazo y fecha de preaviso, mecanismo de precio, qué pasa si no hacemos nada,
> alternativas, y qué preguntar antes de que se cierre la ventana]. Para encontrar abogado/a: el SOJ
> de tu Colegio de la Abogacía es el punto de partida más rápido.

No pases esta puerta sin un sí explícito.

## Integración: agente vigía-renovaciones

El agente vigía-renovaciones (en preparación) ejecuta esta skill de forma programada (semanal por
defecto) y publica el informe "lo que viene" al canal del CLAUDE.md → `## Estilo de la casa`. El Modo
2 es su salida principal.

## Lo que esta skill NO hace

- No cancela contratos. Te dice cuándo decidir.
- No decide si renovar. Saca la fecha y el responsable de negocio.
- No lee contratos para encontrar fechas — eso pasa en la revisión. Si un contrato está en el registro
  sin fecha, se añadió a mano y alguien tiene que rellenar el hueco.
