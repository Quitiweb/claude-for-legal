# Plugin de Protección de Datos (España)

Flujos de trabajo de privacidad para España, anclados en **RGPD + LOPDGDD** y en los
criterios de la **AEPD**: revisión de contratos de encargo de tratamiento, respuesta al
ejercicio de derechos dentro de plazo, generación de Evaluaciones de Impacto (EIPD) y triaje
de tratamientos. Se construye en torno a un perfil de práctica aprendido de tu política de
privacidad, tu plantilla de encargo y una EIPD de referencia.

> Adaptación a España del plugin `privacy-legal` de [`claude-for-legal`](../README.md) de
> Anthropic. **No es una traducción**: las fuentes (AEPD, BOE, CENDOJ, EUR-Lex), las normas
> (RGPD/LOPDGDD) y el deber de secreto profesional están reanclados al Derecho español.

**Cada salida es un borrador para revisión letrada** — citado, marcado y con puertas de
control — no una conclusión jurídica. El plugin hace el trabajo: lee los documentos, aplica
tu manual, encuentra los problemas, redacta. Un abogado/a revisa, verifica y decide. Las
citas se etiquetan por fuente; los actos consecuentes (enviar, firmar, presentar a la AEPD)
están detrás de una confirmación explícita.

## Para quién es

| Rol | Flujos principales |
|---|---|
| **Abogado/a de privacidad / DPD** | Revisión de encargos, sign-off de EIPD, análisis de cumplimiento |
| **Responsable de programa de privacidad** | Ejercicio de derechos, intake de EIPD, revisión de proveedores |
| **Asesoría de producto** | EIPD para lanzamientos |
| **Soporte / Atención al cliente** | Primera línea del ejercicio de derechos (con escalado) |

## Primera ejecución: la entrevista inicial

El plugin te entrevista para aprender: ¿eres responsable o encargado?, ¿qué normativa aplica
de verdad?, ¿qué aceptas y qué no en un contrato de encargo? Después lee tres documentos
semilla — tu política de privacidad, tu plantilla de encargo y una EIPD que te guste — y
aprende tus posturas reales y tu estilo.

Tu configuración se guarda en
`~/.claude/plugins/config/claude-for-legal/proteccion-datos/CLAUDE.md` y sobrevive a las
actualizaciones del plugin.

```
/proteccion-datos:entrevista-inicial
```

## Comandos

| Comando | Hace |
|---|---|
| `/proteccion-datos:entrevista-inicial` | Entrevista inicial / configuración |
| `/proteccion-datos:triaje-tratamiento [actividad]` | ¿Necesita esto una EIPD? Clasificación + condiciones |
| `/proteccion-datos:evaluacion-impacto [tratamiento]` | Genera una EIPD en tu formato |
| `/proteccion-datos:revision-encargo [archivo]` | Revisa un contrato de encargo (detecta dirección) |
| `/proteccion-datos:derechos-interesado` | Tramita y redacta la respuesta al ejercicio de derechos |

## Skills

| Skill | Propósito |
|---|---|
| **entrevista-inicial** | Escribe el CLAUDE.md a partir de la entrevista + documentos semilla |
| **triaje-tratamiento** | ¿Necesita EIPD / puede seguir? Conflictos con la política + traspasos |
| **revision-encargo** | Revisión bidireccional (encargado/responsable) de contratos de encargo (art. 28) |
| **evaluacion-impacto** | EIPD en formato propio, con comprobación de coherencia con la política |
| **derechos-interesado** | Verificación de identidad → barrido de sistemas → excepciones → borrador |

## Inicio rápido

### 1. Configuración
```
/proteccion-datos:entrevista-inicial
```
Ten a mano: la URL de tu política de privacidad pública, tu contrato de encargo estándar y
una EIPD de referencia.

### 2. Triar un tratamiento nuevo
```
/proteccion-datos:triaje-tratamiento "Marketing quiere usar datos de comportamiento para personalizar anuncios"
```
Salida: PROCEDE / EIPD RECOMENDADA / EIPD OBLIGATORIA / DETENER — con tabla de condiciones,
base de legitimación y oferta de arrancar la EIPD en la misma conversación.

### 3. Revisar un contrato de encargo
```
/proteccion-datos:revision-encargo encargo-proveedor.pdf
```
Salida: dirección detectada, revisión cláusula a cláusula contra tu manual, redlines y
comprobación de coherencia con la política.

### 4. Tramitar un ejercicio de derechos
```
/proteccion-datos:derechos-interesado
```
Te guía: clasificar → verificar identidad → localizar → excepciones → redactar. Usa tu lista
de sistemas del CLAUDE.md de configuración.

## Estado de esta versión piloto

Esta es la **v0.1.0** (piloto). Skills incluidas: entrevista-inicial, triaje-tratamiento,
evaluacion-impacto, revision-encargo, derechos-interesado. En la hoja de ruta:
**personalizar**, **espacio-asunto** (multicliente), **monitor-politica** (deriva de la
política) y **analisis-cumplimiento** (diff de una norma nueva contra tu política).

## Notas

- La revisión de encargos es bidireccional: la misma skill maneja el encargo del cliente
  (defender flexibilidad operativa, somos encargado) y el del proveedor (proteger los datos,
  somos responsable). La dirección se detecta o se pregunta.
- El formato de la EIPD sale de tu EIPD semilla. Si no la diste, usa una estructura genérica
  — reejecuta la configuración con una EIPD de referencia para arreglarlo.
- Las fuentes oficiales (AEPD, BOE, CENDOJ, EUR-Lex) están catalogadas en
  `../references/fuentes-oficiales-espana.md`. El glosario EN→ES está en
  `../references/glosario-juridico-en-es.md`.
