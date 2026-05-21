# Glosario jurídico EN → ES (capa España)

*Equivalencias para localizar los plugins de `claude-for-legal` a la práctica española.
No es una traducción literal: muchos conceptos del *common law* no tienen equivalente
exacto en Derecho continental y se indican como tales. Mantener estas equivalencias
constantes en todos los plugins.*

**Última verificación: 2026-05-21.**

## Protección de datos

| Inglés (original) | Español (España / UE) | Nota |
|---|---|---|
| GDPR | RGPD — Reglamento (UE) 2016/679 | Aplicación directa. |
| (US) CCPA / CPRA / state privacy laws | LOPDGDD (Ley Orgánica 3/2018) | No hay equivalente estatal; el régimen es RGPD + LOPDGDD. |
| Supervisory authority / DPA (regulator) | Autoridad de control — **AEPD** (y autonómicas) | Cuidado: "DPA" como regulador ≠ "DPA" como contrato. |
| DPA (Data Processing Agreement) | Contrato de **encargo de tratamiento** (art. 28 RGPD) | El acuerdo entre responsable y encargado. |
| Controller / Processor | **Responsable / Encargado** del tratamiento | |
| Sub-processor | **Subencargado** | art. 28.2 y 28.4 RGPD. |
| Data subject | **Interesado** / afectado | |
| DSAR (Data Subject Access Request) | **Ejercicio de derechos** del interesado | Acceso, rectificación, supresión, oposición, limitación, portabilidad. |
| Rights: access/rectification/erasure/restriction/portability/objection | Acceso, rectificación, supresión, limitación, portabilidad, oposición | Arts. 15-22 RGPD. Mnemotécnico habitual: ARSOPL. |
| Right to be forgotten | Derecho de **supresión** ("al olvido") | art. 17 RGPD. |
| PIA (Privacy Impact Assessment) | (interno) Análisis de privacidad | Documento interno previo a la EIPD formal. |
| DPIA (Data Protection Impact Assessment) | **EIPD** — Evaluación de Impacto relativa a la Protección de Datos | art. 35 RGPD. |
| Prior consultation | **Consulta previa** a la autoridad de control | art. 36 RGPD (si el riesgo residual es alto). |
| Records of Processing Activities (RoPA) | **RAT** — Registro de Actividades de Tratamiento | art. 30 RGPD. |
| Personal data breach | **Brecha** / violación de seguridad de los datos personales | Notificación 72 h (art. 33); comunicación al interesado (art. 34). |
| Lawful basis | **Base de legitimación / licitud** | art. 6 RGPD. |
| Legitimate interest (balancing test) | Interés legítimo (juicio de ponderación) | art. 6.1.f RGPD. |
| Special category data | **Categorías especiales** de datos | art. 9 RGPD. |
| Standard Contractual Clauses (SCC) | **Cláusulas Contractuales Tipo (CCT)** | Decisión (UE) 2021/914. |
| Adequacy decision | **Decisión de adecuación** | Cap. V RGPD. |
| DPO | **DPD** — Delegado de Protección de Datos | arts. 37-39 RGPD; 34-37 LOPDGDD. |

## Profesión, deber de confidencialidad y documentos de trabajo

| Inglés (original) | Español (España) | Nota |
|---|---|---|
| Attorney-client privilege | **Secreto profesional** del abogado | art. 542.3 LOPJ; EGAE (RD 135/2021). NO es idéntico al *privilege*. |
| Attorney work product (FRCP 26(b)(3)) | *(sin equivalente directo)* | En España no existe la doctrina del *work product*. Usar marcado de confidencialidad + secreto profesional. |
| Privileged & confidential (header) | **CONFIDENCIAL — sujeto a secreto profesional** | Ver cabecera por rol en el CLAUDE.md del plugin. |
| Waiver of privilege | Pérdida de confidencialidad | El concepto de "waiver" por distribución amplia no opera igual; ver matiz en el plugin. |
| Bar association / state bar | **Colegio de la Abogacía** (ICAM, ICAB…) | Orientación: SOJ, turno de oficio. |
| ABA Formal Opinion | *(sin equivalente)* | Referirse al EGAE y al Código Deontológico de la Abogacía Española. |
| Outside counsel | Abogacía externa / despacho externo | |
| In-house counsel | Asesoría jurídica interna / *in-house* | |

## Litigación y proceso (referencia para futuros plugins)

| Inglés (original) | Español (España) | Nota |
|---|---|---|
| Discovery / disclosure | *(sin equivalente)* — diligencias preliminares, exhibición documental | Arts. 256 y ss. y 328-330 LEC. Mucho más limitado. |
| Deposition | *(sin equivalente)* — interrogatorio de testigos/partes en juicio | No hay *depositions* previas al juicio. |
| Statute of limitations | **Plazo de prescripción / caducidad** | CC y leyes especiales. |
| Complaint / pleading | Demanda / escrito de alegaciones | LEC / LECrim. |
| Civil procedure rules | **LEC** — Ley 1/2000 de Enjuiciamiento Civil | |
| Criminal procedure | **LECrim** — Ley de Enjuiciamiento Criminal | |
| Class action | Acción colectiva / de cesación | Régimen distinto; transposición de la Directiva (UE) 2020/1828. |

## Mercantil y contratos (referencia para el plugin `mercantil`)

| Inglés (original) | Español (España) | Nota |
|---|---|---|
| NDA | **Acuerdo de confidencialidad** (NDA) | |
| MSA / SaaS agreement | Contrato marco de servicios / contrato SaaS | |
| Governing law / jurisdiction | Ley aplicable / sumisión a fueros | Reglamento Roma I y LEC; cuidado con consumidores. |
| Indemnification | Indemnización / cláusula de indemnidad | |
| Limitation of liability | Limitación de responsabilidad | Límites del art. 1102-1103 CC (dolo no es renunciable). |
| Unfair contract terms | **Cláusulas abusivas** / condiciones generales | TRLGDCU y LCGC. |
| Late payment | **Morosidad** comercial | Ley 3/2004; plazos de pago. |

---

*Cuando un concepto no tenga equivalente en Derecho español, NO se inventa: se marca como
"sin equivalente directo" y se explica el mecanismo español más cercano. Aplicar el
principio de reconocimiento de jurisdicción del CLAUDE.md: nunca aplicar doctrina anglosajona
a hechos españoles con apariencia de certeza.*
