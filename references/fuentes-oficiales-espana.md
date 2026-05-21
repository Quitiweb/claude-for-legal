# Fuentes oficiales españolas — catálogo compartido

*Capa España de `claude-for-legal`. Catálogo de fuentes primarias y oficiales que los
plugins localizados deben usar como fuente de verdad, en lugar de las fuentes
estadounidenses (FTC, SEC, Westlaw, CourtListener) del repositorio original.*

**Última verificación: 2026-05-21.**

> **⚠️ Comprobación de vigencia.** Si la fecha anterior tiene más de 90 días, trata
> este archivo como potencialmente desactualizado y verifica cada entrada contra la
> fuente antes de confiar en ella. Las URL y las API cambian. Cuando actualices una
> entrada, actualiza también la fecha de arriba.

---

## Cómo se citan las fuentes en los plugins españoles

El sistema de etiquetas de procedencia del repositorio original se mantiene, pero las
etiquetas de herramienta de investigación se sustituyen por las fuentes españolas y
europeas:

| Etiqueta | Significado |
|---|---|
| `[BOE]` | El texto se obtuvo del BOE (boletín o legislación consolidada) en esta sesión. |
| `[CENDOJ]` | La resolución judicial se obtuvo del buscador del CENDOJ en esta sesión. |
| `[EUR-Lex]` | La norma o sentencia de la UE se obtuvo de EUR-Lex en esta sesión. |
| `[AEPD]` | La guía, resolución o criterio se obtuvo de la AEPD en esta sesión. |
| `[web — verificar]` | Procedente de una búsqueda web; contrastar con la fuente primaria. |
| `[aportado por el usuario]` | El usuario pegó o enlazó la fuente. |
| `[conocimiento del modelo — verificar]` | Todo lo demás. Es el valor por defecto. |
| `[asentado — última comprobación AAAA-MM-DD]` | Referencia estable comprobada contra fuente primaria en la fecha indicada. |

La etiqueta describe la **procedencia**, no la confianza. No se asciende una etiqueta a
una categoría más fiable porque la cita "parezca correcta".

---

## 1. Legislación — BOE (Agencia Estatal Boletín Oficial del Estado)

- **Portal:** https://www.boe.es/
- **Buscador general:** https://www.boe.es/buscar/
- **Legislación consolidada (texto refundido y actualizado):** https://www.boe.es/legislacion/
- **Datos abiertos / API:** https://www.boe.es/datosabiertos/api/api.php
  - API REST de **legislación consolidada**: permite recuperar normas consolidadas por
    distintos criterios; devuelve metadatos básicos y la URL del texto HTML consolidado.
    Endpoint base: `/datosabiertos/api/legislacion-consolidada`.
  - También expone los sumarios diarios del **BOE** y del **BORME** (Registro Mercantil).
  - FAQ técnico: https://www.boe.es/datosabiertos/faq/consolidada.php
- **Uso:** la legislación consolidada del BOE es la fuente de verdad para el texto vigente
  de leyes y reglamentos españoles. Citar siempre el texto consolidado, no una versión
  publicada años atrás. La API es pública y reutilizable (no requiere scraping).

## 2. Jurisprudencia — CENDOJ (Centro de Documentación Judicial)

- **Buscador del CENDOJ:** https://www.poderjudicial.es/search/ (acceso libre al texto
  completo de resoluciones del Tribunal Supremo, Audiencia Nacional, TSJ, Audiencias
  Provinciales y otros órganos).
- **Identificador de resoluciones:** **ROJ** (Repositorio Oficial de Jurisprudencia) y
  **ECLI** (European Case Law Identifier). Citar por ECLI cuando sea posible.
- **Tribunal Constitucional (recursos de amparo, cuestiones de inconstitucionalidad):**
  https://hj.tribunalconstitucional.es/ (Buscador de Jurisprudencia Constitucional).
- **Uso:** para sentencias citar órgano, fecha, nº de recurso/procedimiento y ECLI/ROJ.

## 3. Derecho de la Unión Europea — EUR-Lex

- **Portal:** https://eur-lex.europa.eu/
- **Uso:** texto oficial de reglamentos y directivas de la UE (RGPD, Directiva de bases
  de datos 96/9/CE, etc.), sentencias del **TJUE** (Tribunal de Justicia de la UE),
  decisiones de la Comisión (p. ej., cláusulas contractuales tipo, decisiones de
  adecuación). Citar por número de acto (p. ej., «Reglamento (UE) 2016/679») y CELEX.
- **CDFUE / tratados:** disponibles en EUR-Lex.

## 4. Protección de datos — AEPD y autoridades autonómicas

- **AEPD (Agencia Española de Protección de Datos):** https://www.aepd.es/
  - **Guías y criterios:** https://www.aepd.es/guias
  - **Preguntas frecuentes (FAQ):** https://www.aepd.es/preguntas-frecuentes
  - **Sede electrónica (notificación de brechas, presentación de escritos):**
    https://sedeaepd.gob.es/
  - **Listas de tratamientos con/ sin EIPD obligatoria** (art. 35.4 y 35.5 RGPD): publicadas
    por la AEPD.
  - **Herramientas:** Facilita_RGPD, Gestiona_EIPD, Comunica-Brecha (RGPD).
- **Autoridades autonómicas de control** (competentes para tratamientos de sus
  administraciones públicas y entes de su ámbito):
  - **APDCAT** — Autoritat Catalana de Protecció de Dades: https://apdcat.gencat.cat/
  - **AVPD / DBEB** — Agencia Vasca de Protección de Datos: https://www.avpd.euskadi.eus/
  - **CTPDA** — Consejo de Transparencia y Protección de Datos de Andalucía:
    https://www.ctpdandalucia.es/
- **EDPB (Comité Europeo de Protección de Datos):** https://edpb.europa.eu/ — directrices
  (guidelines) que la AEPD aplica (p. ej., WP/EDPB sobre EIPD, brechas, consentimiento).

## 5. Normas clave por materia (texto consolidado en BOE)

### Protección de datos
- **RGPD** — Reglamento (UE) 2016/679 (en EUR-Lex; aplicación directa en España).
- **LOPDGDD** — Ley Orgánica 3/2018, de 5 de diciembre, de Protección de Datos Personales
  y garantía de los derechos digitales.
- **LSSI-CE** — Ley 34/2002, de servicios de la sociedad de la información y de comercio
  electrónico (cookies, comunicaciones comerciales).
- **Ley General de Telecomunicaciones** (Ley 11/2022) — comunicaciones electrónicas.

### Mercantil y contratos (para el plugin `mercantil`, en localización)
- **Código de Comercio** (Real Decreto de 22 de agosto de 1885).
- **Código Civil** (Real Decreto de 24 de julio de 1889) — teoría general del contrato,
  obligaciones, prescripción.
- **LSC** — Real Decreto Legislativo 1/2010, Ley de Sociedades de Capital.
- **LCGC** — Ley 7/1998, sobre condiciones generales de la contratación.
- **TRLGDCU** — Real Decreto Legislativo 1/2007, defensa de consumidores y usuarios
  (cláusulas abusivas en contratos con consumidores).
- **Ley 3/2004** de lucha contra la morosidad en operaciones comerciales (plazos de pago).

## 6. Profesión y deontología (sustituye a las referencias a la ABA y los *state bars*)

- **Secreto profesional del abogado:** art. 542.3 **LOPJ** (Ley Orgánica 6/1985 del Poder
  Judicial); **Estatuto General de la Abogacía Española** (Real Decreto 135/2021), arts. 21
  y 22; art. 199 del **Código Penal** (revelación de secretos por profesional).
- **Encontrar abogado / orientación jurídica:** Colegios de la Abogacía (ICAM, ICAB, etc.)
  — Servicio de Orientación Jurídica (SOJ) y turno de oficio; **Consejo General de la
  Abogacía Española** (https://www.abogacia.es/).
- **Delegado de Protección de Datos (DPD/DPO):** arts. 37-39 RGPD y 34-37 LOPDGDD;
  esquemas de certificación de DPD acreditados por la ENAC bajo el esquema de la AEPD.

## 7. Base de datos de pago (integración prevista — requiere licencia)

- **Tirant Lo Blanch (tirant.com / Tirant Online):** legislación, jurisprudencia, doctrina
  y formularios. Es un producto de **suscripción** protegido, además, por el **derecho
  *sui generis* sobre bases de datos** (Directiva 96/9/CE; arts. 133 y ss. del Texto
  Refundido de la Ley de Propiedad Intelectual). **No se debe extraer ni volcar su
  contenido sin autorización.** La integración debe hacerse mediante **API/licencia
  oficial**; contactar con Tirant. Hasta que exista licencia, los plugins se anclan en las
  fuentes abiertas de este catálogo.
- Otras bases de pago del mercado (referencia, mismo tratamiento de licencia): Aranzadi
  (Thomson Reuters), vLex, Iberley, La Ley (Wolters Kluwer).

---

*Este archivo es la capa de fuentes compartida. Cada plugin localizado mantiene además su
propio `references/currency-watch.md` con los puntos de su materia que más se mueven.*
