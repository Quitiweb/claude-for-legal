---
name: revision
description: >
  Revisa un contrato entrante (NDA, contrato con proveedor o SaaS) contra tu manual. Identifica
  la estructura del acuerdo por los títulos, enruta a la skill correcta (revision-nda,
  revision-proveedor, revision-saas) e integra la salida en un único memorándum. Úsala cuando el
  usuario diga "revisa este contrato", "comprueba este contrato de servicios", "¿está bien este
  NDA?", "mira este SaaS", o adjunte un acuerdo entrante.
argument-hint: '[ruta de archivo | enlace de Drive | [ID de CLM] | pega el texto]'
---

# /revision

Revisa un acuerdo entrante contra el manual de `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md`.
Identifica la estructura por los títulos, selecciona la(s) skill(s) y —si confirm_routing está
activado— confirma con el usuario antes de continuar.

## Instrucciones

1. **Carga `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md`.** Si hay marcadores,
   detente: "Ejecuta `/mercantil:entrevista-inicial` primero — necesito aprender tu manual antes
   de revisar contra él." Lee también `## Preferencias de revisión` → `confirm_routing` (si falta,
   trátalo como `true`).

2. **Consigue el acuerdo:** de ruta, enlace de Drive, [ID de CLM] o texto pegado. Si no hay nada,
   pregunta.

3. **Lee la estructura del documento — títulos primero.** Antes del cuerpo, extrae el título del
   acuerdo principal y todos los títulos de anexos, exhibits, addenda y schedules. Esta es la
   señal de enrutado. No te fíes solo de palabras del cuerpo — un contrato de servicios de 40
   páginas con "confidencial" por todas partes no es un NDA.

4. **Selecciona la(s) skill(s) según la estructura:**

   | El título del documento/sección contiene | Skill |
   |---|---|
   | Acuerdo de confidencialidad, NDA, Confidencialidad (como acuerdo *principal*) | **revision-nda** |
   | Contrato marco de servicios, prestación de servicios, hoja de encargo, consultoría | **revision-proveedor** |
   | Suscripción, SaaS, servicios en la nube, licencia de software con cuotas recurrentes, orden de pedido con prórroga | **revision-saas** (overlay sobre revision-proveedor) |
   | Contrato/anexo de encargo de tratamiento, DPA, protección de datos | nota para **revision-proveedor** → sección de protección de datos; traspaso a `/proteccion-datos:revision-encargo` |
   | Acuerdo de nivel de servicio, ANS/SLA (como anexo) | nota para **revision-saas** → sección de SLA |

   Pueden aplicar varias. Combinaciones habituales: contrato marco + anexo de encargo →
   revision-proveedor con la parte de datos traspasada; suscripción SaaS + orden + SLA →
   revision-saas. Si la estructura es ambigua tras leer títulos, lee las dos primeras páginas del
   cuerpo para resolverlo y luego enruta.

5. **Confirma el enrutado si está activado.** Si `confirm_routing` es `true` (o falta):

   ```
   Voy a revisarlo como: [tipo(s) de acuerdo].
   Documentos identificados:
   - [Título principal] → [skill]
   - [Título anexo] → [cómo se tratará]
   ¿Correcto? (sí / no — o dime qué he entendido mal)
   ```
   Espera confirmación. Si lo corrige, aplica su instrucción. Si `confirm_routing` es `false`:
   procede en silencio y registra la decisión de enrutado al inicio del memorándum.

6. **Ejecuta la(s) skill(s).** Sigue cada flujo por completo. Si aplican varias, ejecútalas en
   secuencia e **integra la salida en un único memorándum** — no produzcas memorandos separados.

7. **Comprueba escalados:** si alguna incidencia supera la autoridad del revisor según la matriz
   del CLAUDE.md, invoca **escalado** para encaminar y redactar la petición.

8. **Ofrece seguimientos:**
   - Resumen para negocio (`resumen-negocio`).
   - Redline en .docx con control de cambios.
   - Crear el registro en el CLM (si está conectado).
   - Añadir al registro de renovaciones (`seguimiento-renovaciones`) si hay prórroga.

## Configurar confirm_routing

En `~/.claude/plugins/config/claude-for-legal/mercantil/CLAUDE.md` → `## Preferencias de revisión`:
`confirm_routing: true` (por defecto). Según crece la confianza, ponlo en `false`. La entrevista
inicial pregunta por esta preferencia.

## Ejemplos

```
/mercantil:revision contrato-proveedor.pdf
```
```
/mercantil:revision
[pega el texto del acuerdo]
```

## Salida

Memorándum completo según el formato de la skill. Decisión de enrutado al inicio. Desviación a
desviación, redline concreto, aprobador nombrado. Guardado donde el CLAUDE.md → estilo de la casa
diga que va el producto de trabajo.
