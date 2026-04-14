---
type: procedure
created: 2026-03-17
status: active
slug: m-116-localization-engine
tags: [procedure, nexus]
---

# AI-First Localization Engine — Content + Funnel per GEO — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Productie sistematica de landing pages, ad creative si WhatsApp/SMS funnels localizate cultural si lingvistic per piata geografica, cu compliance gambling integrat (Peru ASJ, Kenya BCLB, Chile SJC).

---

## 1. Problema

Continutul generic in engleza pe piete non-anglofone produce CR cu 30-50% mai slab decat continutul localizat cultural. Peru/Chile necesita spaniola nativa cu adaptari regionale; Kenya necesita engleza localizata + elemente swahili. Fara procedura standardizata: traduceri literale fara adaptare culturala, funnels cu ton gresit, referinte culturale ofensatoare, si materiale care incalca reglementari locale (ASJ Peru, BCLB Kenya, SJC Chile).

**Situatii acoperite:**
- Lansare oferta pe piata noua non-engleza (gambling, finance, nutra)
- Adaptare creative existente pentru GEO diferit
- Construire funnel WhatsApp/SMS localizat cultural
- A/B test localizare vs generic per GEO
- Re-localizare materiale existente dupa schimbare reglementari

**Ce se intampla fara procedura:**
- Traducere literala fara adaptare culturala → CR scazut, brand perception negativa
- Referinte culturale nepotrivite → ofensa publica, cont suspendat
- Compliance omis → sanctiuni ASJ/BCLB/SJC, pierdere licenta operator
- Tone mismatch per GEO → disconnect cu audienta, bounce rate ridicat
- Payment methods locale lipsa din copy → conversie 0 pe deposit

## 2. Procedura

### Pas 1: CULTURAL BRIEF STRUCTURAT

Completeaza OBLIGATORIU urmatorul template per GEO tinta. Toate campurile sunt mandatory — camp gol = BLOCKER, nu se trece la Pas 2.

**Template Cultural Brief:**

| Camp | Descriere | Exemplu Peru |
|------|-----------|--------------|
| `geo_code` | Cod ISO tara | PE |
| `language_primary` | Limba principala + variant regional | Castellano neutru cu peruanisme (jerga, causa, bacán) |
| `language_secondary` | Limba secundara (daca aplicabil) | Quechua (doar salutari in regiuni targetate) |
| `tone` | Ton comunicare principal | Familial + aspirational; tuteo natural; evitare formalisme |
| `cultural_references_allowed` | Referinte culturale permise in creative | Fotbal (Alianza Lima, Universitario, seleccion), ceviche, Fiestas Patrias, Machu Picchu |
| `cultural_references_forbidden` | Referinte interzise / sensibile | Sendero Luminoso, conflict politic, referinte religioase directe, stereotipuri indigene |
| `disclaimer_text` | Text disclaimer conform regulator local | "Juega responsablemente. Solo mayores de 18 anos. Apuestas sujetas a regulacion ASJ. Si tienes problemas con el juego, llama al [linie nationala]." |
| `payment_methods_local` | Metode plata locale obligatorii in copy | Yape, Plin, BCP transferencia, Interbank app |
| `sport_references` | Referinte sportive locale pentru hooks | Liga 1 Peru, Copa Libertadores, clasificatorias mundialistas |
| `greeting_style` | Stil salut/adresare in funnel | "Hola causa!" (informal), "Hola!" (standard), evitare "usted" |
| `currency_format` | Format moneda locala | S/ (Sol peruano), ex: "S/ 50 de bono" |
| `trust_signals_local` | Elemente de incredere locale | "Licencia ASJ", testimoniale locale cu nume peruane, logo metode plata locale |
| `restricted_imagery` | Imagini interzise de regulator | Nu imagini de minori, nu imagini sugerand castiguri garantate (ASJ) |
| `device_preference` | Dispozitiv dominant pe GEO | Mobile 87% (Android dominant); optimizare mobile-first obligatorie |

**Tool**: Claude Opus — generare brief pe baza research GEO din M-114 si M-115.
**Output**: Document `cultural-brief-{GEO}.json` cu toate 14 campurile completate.
**Checkpoint**: Toate 14 campuri completate, validate contra M-114 scoring output si M-118 compliance.

### Pas 2: LOCALIZARE LANDING PAGE COPY

Adapteaza template LP din M-004 aplicand cultural brief-ul de la Pas 1:
- Headline: pain point local in limba locala (nu traducere din engleza)
- Body: referinte culturale permise din brief, ton conform `tone` field
- CTA: localizat complet ("Juega Ahora y Gana tu Bono" nu "Play Now")
- Trust signals: licenta locala, testimoniale cu nume locale, logo-uri payment methods din `payment_methods_local`
- Disclaimer: textul EXACT din `disclaimer_text` vizibil pre-scroll
- Moneda: formatul din `currency_format` in toate referintele la sume

**Regula FUNDAMENTALA**: Rewriting cultural complet, NU traducere literala. Fiecare propozitie trebuie sa sune nativ pentru un vorbitor local.

**Tool**: Claude Opus — rewriting cultural; M-004 template ca baza structurala.
**Output**: Document `lp-copy-{GEO}-v1.md` cu toate sectiunile LP (headline, subheadline, body x3, CTA x2, footer, disclaimer).
**Checkpoint**: LP copy verificata contra cultural brief — fiecare camp din brief reflectat in copy. Zero cuvinte/expresii din engleza ramase (exceptie: brand names).

### Pas 3: GENERARE AD CREATIVES LOCALIZATE

Produce seturi de creative adaptate cultural:

**(a) Video scripts (15-30s)**
- Hook primele 3 secunde: referinta sportiva/culturala locala din `sport_references`
- Naratiune in `language_primary` cu tonul din `tone`
- CTA clar in limba locala cu mentiune bonus in `currency_format`
- Disclaimer audibil sau text overlay conform `disclaimer_text`

**(b) Image prompts**
- Directive culturale explicite: demografie locala, vestimentatie, setare urbana recunoscabila
- Excluderi din `restricted_imagery` si `cultural_references_forbidden`
- Integrare logo-uri payment locale

**(c) Voiceover text**
- Accent regional specificat (Peru: accent limeno standard; Kenya: kenyan english formal; Chile: accent santiaguino moderat)

**Tool**: Claude Opus (scripts + prompts), Midjourney (image generation), ElevenLabs (voiceover localizat).
**Output**: 3 variante video script + 5 image prompts + voiceover text — toate in folder `creatives-{GEO}/`.
**Checkpoint**: Fiecare creative validat contra `cultural_references_forbidden` si `restricted_imagery` din brief. Zero referinte interzise.

### Pas 4: CONSTRUIRE FUNNEL WHATSAPP/SMS LOCALIZAT

Construieste secventa de 5 mesaje localizate (detalii complete in M-120):

- **Mesaj 1** (Day 0): Welcome + bonus in `currency_format`, salut conform `greeting_style`
- **Mesaj 2** (Day 1): Tutorial depozit cu `payment_methods_local` specific
- **Mesaj 3** (Day 2): Social proof local (testimonial cu nume locale) + reminder
- **Mesaj 4** (Day 4): Urgency — bonus expira, CTA direct
- **Mesaj 5** (Day 7): Re-engagement cu oferta secundara

Limite: SMS max 160 char / WhatsApp max 500 char. Canal: WhatsApp Business API (Peru/Chile) sau SMS (Kenya unde WhatsApp business mai putin penetrat).

Disclaimer obligatoriu in Mesaj 1 si Mesaj 5 conform regulator local.

**Tool**: Claude Opus (copy mesaje), Twilio/Wati (delivery), M-120 (framework funnel complet).
**Output**: Document `funnel-{GEO}-{canal}.md` cu 5 mesaje + timing + disclaimer inclus.
**Checkpoint**: Fiecare mesaj sub limita de caractere. Disclaimer prezent in M1 si M5. Payment methods locale mentionate in M2.

### Pas 5: SETUP TRACKING LOCALIZAT

Configureaza tracking separat per GEO conform M-121:
- Campanii Voluum separate per GEO cu naming convention `{vertical}-{GEO}-{canal}-{data}`
- Domenii tracking localizate (ccTLD sau subdomain per GEO)
- SubID-uri per canal + creative variant
- Postback URLs per oferta-GEO
- Dashboard per GEO cu metrici in moneda locala

**Tool**: Voluum (tracking setup), M-121 (server-side tracking framework).
**Output**: Screenshot/export config `tracking-setup-{GEO}.png` cu toate elementele confirmate.
**Checkpoint**: Campanie Voluum activa, postback functional (test conversion trimis si confirmat), dashboard cu moneda locala corecta.

### Pas 6: A/B TEST MATRIX

Defineste si lanseaza testele obligatorii:

| Test | Variante | KPI Primar | Split |
|------|----------|------------|-------|
| LP localizata vs engleza generica | Localizata / Generica | CR (conversie) | 60/40 |
| Ad hook local vs generic | Cultural hook / Generic hook | CTR | 50/50 |
| WhatsApp vs Email (LATAM) | WhatsApp funnel / Email funnel | Open rate + CR | 50/50 |
| CTA copy A vs B | 2 variante CTA localizat | CR | 50/50 |

**Stop rules**:
- Sub 100 clicks/varianta = NU decide, continua test
- Peste 500 clicks + delta > 15% = kill loser, scala winner
- Delta sub 5% la 1000 clicks = no significant difference, pastreaza ambele

**Tool**: Voluum (split testing), M-008 (A/B testing framework).
**Output**: Document `ab-matrix-{GEO}.md` cu testele definite, KPI, stop rules, si timeline.
**Checkpoint**: Toate 4 testele lansate simultan. Tracking functional pe ambele variante (verificat cu test click).

### Pas 7: COMPLIANCE REVIEW FINAL

Verificare conformitate TOATE materialele produse (LP, creatives, funnel) contra reglementari locale.

**Checklist per GEO (10 puncte):**

**Peru (ASJ — Asociacion Peruana de Apuestas Deportivas):**
1. Disclaimer ES vizibil pre-scroll pe LP
2. Restrictie "solo mayores de 18 anos" prezenta
3. Zero promisiuni castiguri garantate
4. Mentiune "juego responsable" in fiecare material
5. Nu imagini minori

**Kenya (BCLB — Betting Control and Licensing Board):**
1. Disclaimer EN vizibil pe LP
2. "Must be 18+" explicit
3. Zero cash imagery in creative
4. Nu targeting minori in ad settings
5. Licenta BCLB referentiata

**Chile (SJC — Superintendencia de Juegos de Casino):**
1. Disclaimer ES conform SJC guidelines
2. Restrictie 18+ prezenta
3. "Juega con responsabilidad" obligatoriu
4. Nu promisiuni castiguri garantate
5. Referinta la linie de ajutor dependenta

**Tool**: Claude Opus (compliance audit), M-118 (gambling compliance framework complet).
**Output**: Document `compliance-review-{GEO}.md` cu 10/10 puncte marcate GO/NO-GO per material.
**Checkpoint**: Scor 10/10 per GEO = GO. Sub 10/10 = BLOCKER, revenire la pasul relevant pentru corectie. Zero materiale publicate fara 10/10.

## 3. Cortex Logging

```json
{
  "text": "M-116 Localization Engine — Cultural brief + LP + creatives + funnel localizate per GEO cu compliance gambling integrat",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "M-116",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "localization",
    "pipeline": "A",
    "version": "2.0",
    "status": "ACTIVE",
    "tags": ["affiliate", "localization", "gambling", "LATAM", "Africa", "training", "cultural-adaptation", "compliance"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (lansare pe GEO nou)
- La fiecare set de active creat per GEO
- La re-localizare materiale post-schimbare reglementari

### WHEN
- La fiecare oferta noua pe piata non-engleza
- La adaptare creative existente pentru GEO diferit
- La schimbare reglementari locale (ASJ/BCLB/SJC update)
- Trimestrial: re-validare cultural briefs existente

### HOW (violation detection)
- LP/creative fara localizare culturala pe piata non-engleza → **VIOLATION**
- Cultural brief incomplet (campuri lipsa) → **VIOLATION CRITICA** (blocker Pas 2)
- WhatsApp/SMS funnel absent pe piata cu email penetration sub 40% → **VIOLATION**
- Creative cu traducere literala fara adaptare culturala → **VIOLATION**
- Compliance checklist sub 10/10 si material publicat → **VIOLATION CRITICA**
- Materiale fara disclaimer regulator local → **VIOLATION CRITICA**
- Payment methods locale lipsa din LP si funnel → **VIOLATION**

### CONNECT
- M-004 (Landing Page Template) — baza structurala LP la Pas 2
- M-005 (Email Funnel) — adaptat WhatsApp la Pas 4
- M-008 (A/B Testing Framework) — test matrix la Pas 6
- M-009 (Creative Ads) — baza creative la Pas 3
- M-113 (Affiliate Content Marketing) — content strategy localizat
- M-114 (Geo Scoring Framework) — input scoring GEO la Pas 1
- M-115 (Offer Discovery per GEO) — input oferte disponibile per piata
- M-117 (Competitive Intelligence) — benchmark competitori locali
- M-118 (Gambling Compliance) — framework compliance la Pas 7
- M-119 (TikTok Native Ads) — creative TikTok localizate
- M-120 (WhatsApp/SMS Funnel) — framework complet funnel la Pas 4
- M-121 (Server-Side Tracking) — tracking setup la Pas 5

### VERIFY
1. Cultural brief are toate 14 campurile completate — verificare automata camp-by-camp
2. LP copy contine zero cuvinte/expresii engleza (exceptie brand names) — scan automat
3. Fiecare creative validat contra `cultural_references_forbidden` — checklist manual
4. Compliance review 10/10 per GEO documentat cu GO explicit — audit trail
5. WhatsApp/SMS funnel respecta limitele de caractere (160/500) — count automat
6. Tracking Voluum functional cu test conversion confirmat — postback test
7. A/B tests au minimum 4 teste lansate simultan — dashboard Voluum check

### MODEL ROUTING
- **Claude Opus**: Cultural brief, LP copy, creative scripts, compliance review, funnel copy (complex reasoning + cultural nuance)
- **Midjourney**: Image generation cu directive culturale
- **ElevenLabs**: Voiceover localizat cu accent regional
- **Claude Sonnet**: Quick translations, character count validation, format checks (cost optimization)

## 5. Dependente

- **Tools**: Claude Opus, Midjourney, ElevenLabs, Twilio/Wati (WhatsApp + SMS), Voluum, Meta Ad Library
- **Proceduri input**: M-114 (GEO scoring), M-115 (offer discovery), M-118 (compliance framework)
- **Proceduri framework**: M-004 (LP template), M-005 (email funnel), M-008 (A/B testing), M-009 (creative ads), M-120 (WhatsApp/SMS funnel), M-121 (server-side tracking)
- **Proceduri adjacent**: M-113 (content marketing), M-117 (competitive intelligence), M-119 (TikTok ads)
- **Data sources**: Cultural research per GEO, regulator websites (ASJ, BCLB, SJC), payment provider documentation
- **Output consumed by**: M-117 (competitive benchmark cu materiale localizate), M-119 (TikTok creative localizate)

## 6. Metrics

| Metric | Target | Frecventa | Sursa |
|--------|--------|-----------|-------|
| CR lift localizare vs engleza | +30% min | Per campanie (dupa 500 clicks) | Voluum A/B test |
| WhatsApp open rate LATAM | >70% | Saptamanal | Twilio/Wati dashboard |
| WhatsApp open rate Africa | >60% | Saptamanal | Twilio/Wati dashboard |
| Compliance pass rate | 100% (10/10 per GEO) | Per material | Compliance review doc |
| Productie set complet per GEO | <4h | Per GEO | Time tracking |
| Cultural brief completeness | 14/14 campuri | Per GEO | Brief audit |
| A/B test coverage | 4/4 teste per GEO | Per lansare | Voluum |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6)
- [x] Pasi numerotati secvential (1-7) cu output clar si checkpoint per pas
- [x] Tools specificate per pas (nu doar lista generica)
- [x] KPIs masurabile cu target numeric si sursa
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] Cortex logging JSON valid cu metadata block complet (type, procedure, rule_id, domain, sub_domain, pipeline, version, status, tags)
- [x] Dependente listate (tools + proceduri + data sources)
- [x] Metrics cu target, frecventa si sursa masurare
- [x] Compliance disclaimers per GEO prezente (Peru ASJ, Kenya BCLB, Chile SJC)
- [x] Conexiuni cu proceduri existente validate (M-004, M-005, M-008, M-009, M-113, M-114, M-115, M-117, M-118, M-119, M-120, M-121)
- [x] Cultural brief template cu 14 campuri mandatory si exemplu Peru
- [x] Referinta explicita M-120 la Pas 4
