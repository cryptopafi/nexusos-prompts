---
type: procedure
created: 2026-03-17
status: active
slug: m-114-geo-scoring-framework
tags: [procedure, nexus]
---

# Affiliate Geo Scoring Framework — Country-Product Match — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Framework sistematic de scoring geografic pentru potrivirea oferte-afiliat cu piete internationale (0-100 compozit). Acoperire completa: compliance gate, market sizing, payment compatibility, cost trafic, si localizare — cu output rankare top 5 GEO si entry strategy per piata.

---

## 1. Problema

Fara scoring sistematic, selectia GEO e bazata pe intuitie. Ofertele de gambling/finance/nutra au performanta radical diferita pe piete diferite din cauza compliance, payment methods, costuri trafic, si saturare competitiva. Selectie manuala = risc ROI negativ + pierdere oportunitati pe piete subexplorate.

**Situatii acoperite:**
- Evaluare piata noua pentru vertical gambling/finance/nutra
- Comparatie intre GEO-uri candidate pentru aceeasi oferta
- Re-scoring periodic la schimbare conditii (legislatie, payment, concurenta)
- Validare GEO deja selectat contra benchmark
- Prioritizare piete emergente LATAM/Africa vs piete saturate

**Ce se intampla fara procedura:**
- GEO selectat pe baza de "feeling" → ROI negativ in 60%+ cazuri
- Payment method incompatibil descoperit post-lansare → CR 0, buget pierdut
- Piete subexplorate ignorate (Kenya, Chile) in favoarea pietelor saturate
- Compliance violation pe piete grey → cont suspendat, pierdere licenta
- Resurse alocate pe piete cu cost trafic prohibitiv → CPA > payout

## 2. Procedura

### Pas 1: EXTRACT OFFER PARAMETERS

Parse oferta completa si extrage parametrii structurati:
- **Vertical**: gambling / finance / nutra / dating
- **Payout model**: CPA / rev-share / hybrid (detalii: suma, procent, conditii)
- **Traffic accepted**: SMS / social / search / native / push / email
- **Geo restrictions**: whitelist / blacklist explicit
- **Minimum deposit**: suma + moneda
- **Device targeting**: mobile / desktop / all
- **Operator**: brand, licente, reputatie

Campuri lipsa = flag INCOMPLETE. Oferta cu 3+ campuri lipsa → contactare affiliate manager inainte de continuare.

**Tool**: Claude Opus — parsare si structurare parametri oferta din sursa (affiliate dashboard, email, PDF terms).
**Output**: Document `offer-params-{offer_id}.json` cu toate campurile completate conform template-ului de mai jos.
**Checkpoint**: Toate 7 campurile principale completate. Zero campuri cu valoare "unknown" in output final.

**Template Output:**

| Camp | Valoare | Sursa |
|------|---------|-------|
| `offer_id` | ID unic oferta | Dashboard afiliat |
| `vertical` | gambling / finance / nutra / dating | Terms oferta |
| `payout_model` | CPA / rev-share / hybrid | Dashboard |
| `payout_value` | Suma + conditii | Dashboard |
| `traffic_types` | Lista canale acceptate | Terms oferta |
| `geo_whitelist` | Lista ISO codes | Dashboard / AM |
| `geo_blacklist` | Lista ISO codes | Dashboard / AM |
| `min_deposit` | Suma + moneda | Operator site |
| `device` | mobile / desktop / all | Terms oferta |
| `operator` | Brand name | Dashboard |
| `licenses` | Lista licente active | Operator site |

### Pas 2: COMPILE GEO CANDIDATE LIST

Genereaza 15-20 tari candidat din:
1. Whitelist oferta (obligatoriu — toate tarile din whitelist incluse)
2. Cross-referintat cu pietele active prioritare: Peru, Kenya, Chile
3. Extensii: Colombia, Tanzania, Nigeria, Ecuador, Ghana, Uganda, Bolivia, Paraguay
4. Eliminare tari cu verticalul interzis complet (ex: gambling ilegal fara grey area)

Rationale scurta per GEO: de ce e inclus (market size, growth, low competition, etc.).

**Tool**: Claude Opus — compilare lista pe baza whitelist + research piete emergente; Perplexity Pro — verificare status legal curent per GEO.
**Output**: Document `geo-candidates-{offer_id}.md` cu lista 15-20 GEO si rationale per GEO.
**Checkpoint**: Minim 15 GEO candidati. Toate tarile din whitelist oferta prezente. Rationale non-empty per GEO.

### Pas 3: COMPLIANCE GATE

Per GEO candidat, verifica:
- Statut legal vertical: **licenciat** / **grey** / **ilegal**
- Cerinta licenta locala pentru operator
- Restrictii advertising pe canale (social, search, native, SMS)
- Restrictii specifice (varsta, disclaimer, geo-fencing)
- Sursa verificare: regulator oficial, M-118 (Gambling Compliance), rapoarte recente

**Score**:
- **VERDE** = licenciat, fara restrictii majore advertising
- **GALBEN** = grey area sau restrictii partiale (procedeaza cu cautie, necesita review M-118)
- **ROSU** = ilegal sau restrictii totale → **ELIMINAT AUTOMAT**

**Referinte compliance per piata prioritara:**
- Peru: ASJ (Asociacion Peruana de Apuestas Deportivas) — licenciat, restrictii moderate
- Kenya: BCLB (Betting Control and Licensing Board) — licenciat, taxe 20% pe castiguri
- Chile: SJC (Superintendencia de Juegos de Casino) — grey → licenciat (tranzitie), restrictii advertising
- Colombia: Coljuegos — licenciat complet, regulator matur
- Nigeria: Lagos State Lotteries Board — grey, fragmentat per stat

**Tool**: Perplexity Pro — verificare status legal curent; Claude Opus — analiza regulatorie; M-118 — framework compliance detaliat.
**Output**: Tabel `compliance-gate-{offer_id}.md` cu coloanele: GEO | Status (VERDE/GALBEN/ROSU) | Regulator | Detalii | Sursa. Toate ROSU eliminate din pipeline.
**Checkpoint**: Fiecare GEO are status atribuit cu sursa citata. Zero GEO fara status. Toate ROSU eliminate — nu apar in pasii urmatori.

### Pas 4: MARKET SIZING SCORE (0-25)

Per GEO ramas, evalueaza:
- Populatie 18-45 (sursa: World Bank / UN Population Division)
- Penetrare internet (%) (sursa: ITU / DataReportal)
- Penetrare smartphone (%) (sursa: GSMA / Statista)
- Piata estimata vertical in USD (sursa: Statista / H2GC pentru gambling, Research & Markets)
- Nr operatori activi cu licenta locala (sursa: regulator oficial)

**Formula normalizare**: `Score = (raw_value / max_in_set) * 25`
Aplicata pe fiecare sub-indicator, apoi media ponderata: Populatie 20%, Internet 20%, Smartphone 20%, Piata 25%, Operatori 15%.

**Tool**: Perplexity Pro — data gathering; Claude Opus — calcul normalizare si ponderare.
**Output**: Tabel in `market-sizing-{offer_id}.md` cu coloanele: GEO | Pop 18-45 | Internet% | Smartphone% | Market USD | Operatori | Score 0-25. Surse citate per data point.
**Checkpoint**: Toate GEO-urile ramase din Pas 3 au scor calculat. Surse citate pentru fiecare valoare. Score intre 0-25 per GEO.

### Pas 5: PAYMENT + CONVERSION SCORE (0-25)

Per GEO, evalueaza:
- Payment methods acceptate de oferta vs metode locale dominante:
  - Kenya: **Mpesa** (obligatoriu), Airtel Money
  - Peru: **Yape/Plin** (obligatoriu), BCP, Interbank
  - Chile: **Webpay/Khipu** (obligatoriu), Banco Estado
  - Colombia: PSE, Nequi, Daviplata
  - Nigeria: OPay, Paystack, bank transfer
  - Tanzania: M-Pesa TZ, Tigo Pesa
- Mobile money penetration (%)
- Affordability ratio: min deposit / salariu mediu lunar (sub 2% = ideal, peste 5% = penalizare)
- CR benchmark vertical pe GEO (daca disponibil din M-115 sau Voluum historical)

**Penalizare automata**: -15 puncte daca payment method primar local lipseste din oferta.

**Tool**: Claude Opus — analiza compatibility; Perplexity Pro — verificare payment methods operator.
**Output**: Tabel in `payment-score-{offer_id}.md` cu coloanele: GEO | Payment Match | Mobile Money% | Affordability | CR Benchmark | Score 0-25. Penalizari documentate explicit.
**Checkpoint**: Payment method primar verificat per GEO (sursa: pagina depozit operator). Penalizare -15 aplicata unde lipseste. Score 0-25 per GEO.

### Pas 6: TRAFFIC + COST SCORE (0-25)

Per GEO, evalueaza:
- CPM/CPC estimat per canal:
  - TikTok Ads (CPM)
  - Meta Ads (CPM/CPC)
  - Google Ads (CPC)
  - Native (Taboola/Outbrain CPM)
- Concurenta afiliat pe vertical (SimilarWeb, SpyFu)
- Canale disponibile fara restrictie pe vertical
- EPC benchmark (daca disponibil)

**Formula**: Score invers proportional cu costul — GEO ieftin = scor mai mare. `Score = (1 - cost_normalized) * 25`

**Tool**: Google Keyword Planner — CPC estimates; SpyFu — competitor analysis; SimilarWeb — traffic benchmarks; Claude Opus — calcul scoring.
**Output**: Tabel in `traffic-score-{offer_id}.md` cu coloanele: GEO | CPM TikTok | CPM Meta | CPC Google | CPM Native | Concurenta (low/mid/high) | Score 0-25.
**Checkpoint**: Minim 2 canale cu estimari cost per GEO. Score calculat cu formula documentata. Canale restrictionate pe vertical marcate explicit.

### Pas 7: COMPOSITE SCORE + RANKING

Combina toate scorurile:
- **Compliance**: GO / NO-GO (gate, nu scor numeric — deja filtrat la Pas 3)
- **Market Size**: 0-25 (din Pas 4)
- **Payment/Conversion**: 0-25 (din Pas 5)
- **Traffic/Cost**: 0-25 (din Pas 6)
- **Bonus localizare**: 0-25 (limba suportata de operator, creative locale disponibile, brand recognition local)

**Scor compozit**: 0-100

**Template Scoring Matrix:**

| GEO | Compliance | Market (0-25) | Payment (0-25) | Traffic (0-25) | Localizare (0-25) | TOTAL (0-100) | Rank |
|-----|-----------|---------------|----------------|----------------|-------------------|---------------|------|
| PE | VERDE | 22 | 24 | 19 | 21 | 86 | 1 |
| KE | VERDE | 18 | 20 | 23 | 16 | 77 | 2 |
| CL | GALBEN | 20 | 22 | 17 | 18 | 77 | 3 |
| CO | VERDE | 21 | 18 | 15 | 17 | 71 | 4 |
| NG | GALBEN | 17 | 14 | 22 | 12 | 65 | 5 |

*Nota: exemplu ilustrativ. Valorile reale se calculeaza per oferta.*

ROI estimat per GEO cu interval de incredere:
- **High confidence**: GEO cu data historical → interval ±15%
- **Medium confidence**: GEO nou, vertical existent → interval ±25%
- **Low confidence**: GEO nou, vertical nou → interval ±40%

**Tool**: Claude Opus — compilare scoring final, ranking, ROI estimation.
**Output**: Document `geo-ranking-{offer_id}.md` cu: (1) scoring matrix complet, (2) top 5 GEO rankate, (3) ROI estimat per GEO cu interval de incredere.
**Checkpoint**: Top 5 GEO cu scor compozit calculat. Zero GEO ROSU in output. ROI estimat cu interval de incredere explicit per GEO. Scoring matrix completa cu toate 4 dimensiunile.

### Pas 8: ENTRY STRATEGY

Per top 3 GEO, detaliaza:
- **Canal intrare recomandat**: cel cu cel mai bun raport cost/conversie (din Pas 6)
- **Tip continut**: review site / landing page / social content / push (din M-004, M-009)
- **Timeline test**: 2-4 saptamani cu milestones concrete
- **Buget minim test**: USD, alocat per canal
- **KPI decizie**: CR minim, CPA maxim, ROAS target pt continuare/stop
- **Compliance requirements**: disclaimeri obligatorii, restrictii per canal (din Pas 3 + M-118)
- **Next steps**: link catre M-115 (Offer Discovery) daca GEO validat

**Tool**: Claude Opus — strategie intrare; Voluum — setup campanie test; M-115 — pipeline B (offer discovery pe GEO validat).
**Output**: Document `entry-strategy-{offer_id}.md` cu plan detaliat per top 3 GEO (canal, buget, timeline, KPI).
**Checkpoint**: Fiecare GEO din top 3 are plan complet cu: canal, buget, timeline (saptamani), KPI cu valori numerice. Buget total test calculat.

**KPIs procedura**:
- Top 5 GEO generat in <2h
- Scor validat contra benchmark (±15% variatie acceptata)
- ROI cu interval de incredere per GEO
- 0 GEO ROSU in output final

## 3. Cortex Logging

```json
{
  "text": "M-114 Geo Scoring Framework — Scoring sistematic 0-100 per GEO cu compliance gate, market sizing, payment compatibility, traffic cost si localizare bonus",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "M-114",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "geo-scoring",
    "pipeline": "A",
    "version": "2.0",
    "status": "ACTIVE",
    "tags": ["affiliate", "geo-scoring", "gambling", "LATAM", "Africa", "training", "compliance", "payment-methods"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (piata noua)
- La selectie piata noua pentru orice oferta
- La re-evaluare periodica GEO-uri active (trimestrial)

### WHEN
- La fiecare oferta noua evaluata pentru piete internationale
- La schimbare conditii piata (legislatie, payment, concurenta)
- La onboarding piata noua in portofoliu
- Post-eveniment major: schimbare regulator, noua licenta, criza economica

### HOW (violation detection)
- Scor compozit absent pentru GEO selectat → **VIOLATION**
- GEO selectat fara parcurgerea Compliance Gate → **VIOLATION CRITICA**
- Payment compatibility neverificata → **VIOLATION**
- Oferta lansata pe GEO fara Entry Strategy documentata → **VIOLATION**
- Scoring matrix incompleta (dimensiune lipsa) → **VIOLATION**
- GEO cu status ROSU prezent in output final → **VIOLATION CRITICA**

### CONNECT
- M-001 (Niche Validation) — input vertical si nisa
- M-007 (Offer Selection) — input oferta si parametri
- M-002 (Program Evaluation) — validare program operator
- M-115 (Offer Discovery per GEO) — Pipeline B, consuma output M-114 (top GEO-uri validate)
- M-116 (Localization Engine) — consuma GEO scoring pentru cultural brief
- M-118 (Gambling Compliance) — framework compliance detaliat la Pas 3

### VERIFY
1. Scoring matrix are toate 4 dimensiunile completate per GEO — verificare structurala document output
2. Zero GEO ROSU in output final — scan automat compliance gate vs ranking output
3. Payment method primar verificat contra pagina depozit operator — screenshot sau link sursa documentat
4. ROI estimat cu interval de incredere explicit (nu single-point) — verificare format output
5. Post-test (30 zile): compara ROI real vs estimat per GEO — accuracy tracking document
6. Recalibrare scoruri daca accuracy <70% — trigger re-scoring cu date actualizate

### MODEL ROUTING
- **Scoring + research**: Claude Opus (complex reasoning, multi-factor analysis)
- **Data gathering**: Perplexity Pro (real-time data, regulatory updates)
- **Quick lookups**: Claude Sonnet (cost optimization pe verificari rapide)

## 5. Dependente

- **Tools**: Perplexity Pro, Voluum, Claude Opus, Google Keyword Planner, SpyFu, SimilarWeb
- **Proceduri input**: M-001 (Niche Validation), M-007 (Offer Selection), M-002 (Program Evaluation)
- **Proceduri framework**: M-118 (Gambling Compliance)
- **Data sources**: World Bank (populatie), ITU/DataReportal (internet penetration), GSMA/Statista (smartphone), Statista/H2GC (market size gambling), payment provider docs
- **Output consumed by**: M-115 (Offer Discovery per GEO), M-116 (Localization Engine)

## 6. Metrics

| Metric | Target | Frecventa | Sursa |
|--------|--------|-----------|-------|
| GEO accuracy | >70% GEO selectate cu ROI pozitiv post-test | Monthly | Voluum ROI reports |
| Scoring time | <2h per oferta | Per execution | Time tracking |
| Coverage | 100% oferte evaluate cu scoring | Trimestrial | Audit log |
| Compliance accuracy | 0 violations post-lansare | Continuous | Compliance review docs |
| ROI estimation accuracy | ±15% vs real | Post 30-day test | Voluum vs scoring doc |
| Payment compatibility rate | 100% oferte selectate cu payment match | Per execution | Payment verification docs |
| Scoring matrix completeness | 4/4 dimensiuni per GEO | Per execution | Output audit |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6)
- [x] Pasi numerotati secvential (1-8) cu Tool, Output (artifact name) si Checkpoint (criterii masurabile) per pas
- [x] Tools specificate per pas (nu doar lista generica)
- [x] KPIs masurabile cu target numeric si sursa
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] Cortex logging JSON valid cu metadata block complet (type, procedure, rule_id, domain, sub_domain, pipeline, version, status, tags)
- [x] Dependente listate (tools + proceduri + data sources)
- [x] Metrics cu target, frecventa si sursa masurare
- [x] Compliance Gate ca hard gate cu referinte regulatori per piata (ASJ, BCLB, SJC, Coljuegos)
- [x] Connected procedures referenced bidirectional (M-114 <-> M-115, M-114 -> M-116)
- [x] Scoring matrix TEMPLATE cu exemplu complet la Pas 7
- [x] VERIFY cu 6 verificari concrete si metoda specifica per verificare
