---
type: procedure
created: 2026-03-20
status: active
slug: smsads-p1-vertical-discovery
tags: [procedure, nexus]
---

# SMSADS-P1 — Vertical Discovery for SMS Carrier Marketing

**Domeniu**: affiliate | **Sub-domeniu**: smsads-vertical-discovery
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Scope**: Framework sistematic pentru descoperirea si rankingul verticalelor de afiliere compatibile cu livrarea SMS carrier via VOX, pornind de la taxonomia IAB per GEO si calculul EC/1K SMS estimat per vertical.

---

## 1. Problema

### De ce exista aceasta procedura

SMSads opereaza exclusiv in verticalul betting/gambling — un singur vertical cu un singur operator activ (MarathonBet Bolivia). Diversificarea verticala e necesara strategic dar nu exista niciun proces repetat de identificare si prioritizare a verticalelor noi.

**Consecinte concrete ale lipsei acestei proceduri**:
- Bolivia are 693K contacte in Technology, 523K in Software, 457K in Apps — segmente perfect compatibile cu App Downloads (CPI $1-3/install, CR 3-8%), VPN/Antivirus (CPA $10-40), Fintech — toate neutilizate
- Evaluarea verticalelor se face ad-hoc, la recomandarea 3SNET, fara scoring sistematic
- VOX Bolivia are 33 categorii IAB; niciodata nu au fost evaluate complet prin prisma compatibilitatii SMS si disponibilitatii de oferte
- Romania deblocata in April 2026 cu 20M SMS si 45 categorii IAB — fara M-124 si SMSADS-P1 completate acum, lansarea va fi reactiva, nu strategica
- Cost al inactiunii: EC/1K SMS $0 pe 80%+ din audienta Bolivia disponibila

**Situatii acoperite de aceasta procedura**:
- Adaugarea unui GEO nou VOX (ex: Myanmar)
- Revizuirea trimestriala a verticalelor active per GEO
- Evaluarea unui vertical propus de AM 3SNET sau de echipa
- Pre-lansare Romania: evaluarea completa a verticalelor pentru April 2026

**WHY NOW**: Romania se deblocheaza April 2026. Myanmar e planificat. Bolivia are audienta masiva subutilizata. Fiecare saptamana fara ranking vertical sistematic = pierdere de oportunitate cuantificabila.

---

## 2. Procedura

### Pas 1: Audit Categorii IAB VOX per GEO

**Tool**: VOX dashboard + request formal catre AM VOX (Ehsan)

**Actiune**:
1. Solicita lista completa de categorii IAB per operator pentru GEO-ul tinta:
   - Bolivia (Viva): 33 categorii (date disponibile partial)
   - Romania (Vodafone): 45 categorii (date disponibile partial)
   - Myanmar (U9): TBD — request obligatoriu inainte de scoring
2. Pentru fiecare categorie, documenteaza: denumire IAB, dimensiune audienta, coverage % din totalul GEO
3. Mapeaza fiecare categorie IAB la verticalele de oferte afiliere compatibile:

| Categorie IAB VOX | Vertical Primar | Vertical Secundar | Vertical Evitat |
|---|---|---|---|
| Technology (693K BO) | App Downloads, VPN/Antivirus | Betting, Fintech | B2B SaaS |
| Software (523K BO) | App Downloads, VPN | Fintech | B2B Enterprise |
| Apps (457K BO) | App Downloads (CPI direct) | Mobile Games | N/A |
| Arts&Entertainment (246K BO) | Betting/Casino, Streaming | Lifestyle apps | Trading/Forex |
| Business&Finance (116K BO / 2.1M RO) | Trading/Forex, Credite | Crypto | Betting |
| Finance&Insurance (1.7M RO) | Trading CFD, Asigurari | Credite | Betting |
| Health (1.2M RO) | Nutra/Suplimente, Pharma | Wellness apps | Gambling |
| CPG (1.4M RO) | E-commerce, Health | Lifestyle | B2B |

**Output (artifact)**: `smsads-iab-mapping-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 5 categorii IAB documentate cu dimensiune exacta si mapping vertical complet. FAIL daca dimensiunea audientei nu e confirmata oficial de VOX (nu estimata).

---

### Pas 2: Scoring Compatibilitate SMS per Vertical

**Tool**: Scoring manual pe 5 dimensiuni (1-5 per dimensiune)

**Actiune**:
Evalueaza fiecare vertical candidat pe 5 dimensiuni:

| Dimensiune | Scor 1 | Scor 3 | Scor 5 |
|---|---|---|---|
| **Risc Regulatory Carrier** | Interzis de carrier (adult, arme) | Permis cu disclaimer obligatoriu | Permis fara restrictii (apps, VPN) |
| **Lungime Path Conversie** | >5 pasi SMS→conversie | 3 pasi (click→landing→lead) | 1-2 pasi (click→install / click→submit) |
| **Consent Carrier** | Opt-in strict obligatoriu per GDPR | Soft consent acceptat | Light/None (Bolivia Viva) |
| **Payout per Conversie** | <$5 CPA, conversie dificila | $10-30 CPA, conversie moderata | >$50 CPA sau RevShare recurent |
| **IAB Audience Match** | Audienta <50K in categorie relevanta | 100-300K in categorie relevanta | >300K in categorie relevanta |

**Formula scoring**:
```
Scor Compatibilitate = (Risc × 0.25) + (Path × 0.20) + (Consent × 0.20) + (Payout × 0.20) + (Audience × 0.15)
```

**Regula eliminare imediata**: Scor 1 la Risc Regulatory Carrier = DISCARD automat (adult, arme, tutun fara licenta).

**Exemplu calcul Bolivia — App Downloads**:
- Risc: 5 (apps fara restrictii) × 0.25 = 1.25
- Path: 5 (click→install) × 0.20 = 1.00
- Consent: 5 (Light/None Bolivia) × 0.20 = 1.00
- Payout: 3 ($1.50 CPI × volume) × 0.20 = 0.60
- Audience: 5 (Apps 457K + Software 523K + Tech 693K = 1.5M) × 0.15 = 0.75
- **Total: 4.60/5.00 — PRIORITATE 1**

**Output (artifact)**: `smsads-compat-scores-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: Scor calculat pentru minim 5 verticale candidate. FAIL daca oricare din cele 5 dimensiuni e absenta (scor incomplet = invalid).

---

### Pas 3: Discovery Oferte Disponibile per Vertical

**Tool**: Dashboard 3SNET (#30063) + MaxBounty + ClickDealer + Mobidea + Perform[cb] + mcp__brave-search

**Actiune**:
1. Logheaza-te in fiecare retea si filtreaza ofertele actuale pentru GEO-ul tinta:
   - Filtru minim: CPA > $10 SAU RevShare > 20% SAU CPL > $3
   - GEO: Bolivia (BO), Romania (RO), Myanmar (MM)
2. Consulta AM 3SNET (Timur @Aff3snet): "Ce verticale noi ai pentru Bolivia/Romania in luna urmatoare?"
3. Pentru fiecare vertical candidat, identifica minim 1 oferta activa si documenteaza:

| Vertical | Network | Offer Name | Payout | GEO | Status |
|---|---|---|---|---|---|
| App Downloads | ClickDealer | [app CPI BO] | $1.50/install | Bolivia | de verificat |
| VPN/Antivirus | MaxBounty | [VPN offer] | $25 CPA | Global | de verificat |
| Trading/Forex | MaxBounty | AvaTrade | $250 CPA (cap 15) | Bolivia | confirmat |
| Trading/Forex | MaxBounty | [CFD offer RO] | $150-200 CPA | Romania | de verificat |
| Health/Nutra | MaxBounty | [nutra offer RO] | $30-50 CPA | Romania | de verificat |

**Output (artifact)**: `smsads-offers-raw-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: Minim 1 oferta concreta identificata pentru fiecare vertical candidat cu scor >3.0. FAIL daca verticale cu scor >4.0 raman fara oferta identificata.

---

### Pas 4: Estimare EC/1K SMS per Vertical

**Tool**: Calculator / spreadsheet

**Actiune**:
Calculeaza EC/1K SMS estimat pentru fiecare vertical cu oferta disponibila:

```
EC/1K SMS = CTR% × CR% × Payout_net
```

Unde `Payout_net = Payout_brut × 0.50` (split VOX 50/50)

**Benchmarks CTR per vertical si GEO**:

| Vertical | Bolivia CTR | Romania CTR | Justificare |
|---|---|---|---|
| Betting/Gambling | 3-6% | 5-9% | Brand awareness, piata matura RO |
| App Downloads | 5-9% | 4-7% | Actiune simpla, fricare zero |
| VPN/Antivirus | 3-5% | 3-6% | Interes functional, nu emotional |
| Trading/Forex | 2-4% | 2-5% | Incredere necesara, mai putini impulse clicks |
| Health/Nutra | 2-4% | 3-5% | Interes personal, copy empatic |
| Credite/Loans | 2-4% | 3-6% | Cerere mare RO, trust necesar |

**Benchmarks CR (landing page) per vertical**:

| Vertical | CR Estimat | Nota |
|---|---|---|
| App Downloads (install) | 4-8% | Fricare minima |
| Credite/Loans (lead) | 1-4% | Formular simplu, cerere reala |
| VPN/Antivirus (install/sub) | 2-5% | Demo free ajuta CR |
| Trading/Forex (lead/demo) | 0.8-2.5% | Capital necesar inhiba |
| Health/Nutra (purchase) | 1-3% | Credibilitate necesara |
| Betting (registration) | 0.5-1.5% | KYC + deposit bariere |

**Exemplu EC/1K SMS — App Downloads Bolivia**:
- CTR 6% × CR 5% × $0.75 (CPI $1.50 net dupa VOX split) = $0.00225/SMS → **$2.25/1K SMS**
- Nota: volume potential nelimitat, scala bine

**Exemplu EC/1K SMS — Trading/Forex Bolivia (AvaTrade $250 CPA)**:
- CTR 3% × CR 1% × $125 net = $0.0375/SMS → **$37.50/1K SMS**
- Nota: cap 15 conversii/luna = max $1,875 net/luna

**Output (artifact)**: `smsads-ec1k-estimate-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: EC/1K calculat pentru toate verticalele cu oferta identificata. FAIL daca EC/1K nu e calculat cu ambele benchmarks (CTR + CR) separat documentate.

---

### Pas 5: Ranking Final si Matrice de Prioritizare

**Tool**: Spreadsheet / tabel markdown

**Actiune**:
Construieste matricea finala combinand scorurile din Pasii 2-4:

```
Scor Final = (Compatibilitate SMS × 0.35) + (EC/1K Score × 0.40) + (Disponibilitate Oferta × 0.25)
```

**Normalizare EC/1K Score (1-5)**:
- EC/1K > $50 = 5 | $20-50 = 4 | $10-20 = 3 | $5-10 = 2 | <$5 = 1

**Normalizare Disponibilitate Oferta**:
- Oferta activa + aprobata pe cont = 5 | Oferta disponibila, neaprobata = 3 | Oferta de cautat/negociat = 1

**Matrice output template**:

| Rank | Vertical | GEO | Compatib. (1-5) | EC/1K SMS | EC/1K Score (1-5) | Offer Status | Scor Final | Prioritate |
|---|---|---|---|---|---|---|---|---|
| 1 | App Downloads | Bolivia | 4.60 | $2.25/1K | 1 | de cautat | 2.59 | PILOT (dupa aprobare) |
| 2 | Trading/Forex | Bolivia | 3.80 | $37.50/1K | 4 | activa AvaTrade | 3.88 | PRIORITATE 1 |
| 3 | Trading/Forex | Romania | 3.70 | $30-40/1K est | 4 | de cautat | 3.48 | PRIORITATE 1 (April) |
| 4 | VPN/Antivirus | Bolivia | 4.20 | $8-15/1K est | 2 | de cautat MaxBounty | 3.17 | PILOT |
| 5 | Health/Nutra | Romania | 3.50 | $15-25/1K est | 3 | de cautat MaxBounty | 3.10 | BACKLOG (April) |

**Output (artifact)**: `smsads-vertical-ranking-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Top 5 verticale rankate cu scor compozit calculat si prioritate asignata. FAIL daca exista verticale cu scor >3.5 marcate BACKLOG fara justificare scrisa.

---

### Pas 6: Handoff catre SMSADS-P2 + Cortex Save

**Tool**: mcp__cortex__cortex_store

**Actiune**:
1. Salveaza ranking final in Cortex (vezi Sectiunea 3)
2. Pentru verticalele Prioritate 1: initializeaza SMSADS-P2 (Product Finder) pentru scoring detaliat al ofertelor
3. Creeaza task in task tracker: "SMSADS-P2 pentru {Vertical} — {GEO}" cu deadline +7 zile

**Output (artifact)**: Cortex entry confirmat + task SMSADS-P2 creat
**Checkpoint PASS**: Cortex save confirmat cu JSON valid. FAIL daca handoff SMSADS-P2 nu e initiat pentru top 2 verticale.

---

## 3. Cortex Logging

```json
{
  "text": "SMSADS-P1 executat: ranking verticale {GEO} {YYYY-MM}",
  "collection": "business_clickwin",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SMSADS-P1-VERTICAL-DISCOVERY",
    "rule_id": "TRAIN-S-001",
    "domain": "smsads",
    "sub_domain": "vertical-discovery",
    "version": "1.0",
    "tags": ["smsads", "VOX", "vertical-discovery", "IAB", "EC-per-1K", "Bolivia", "Romania"],
    "geo_analyzed": [],
    "verticals_scored": 0,
    "top_vertical": "",
    "ec1k_top_vertical": "",
    "networks_checked": ["3SNET", "MaxBounty", "ClickDealer", "Mobidea", "Perform[cb]"],
    "execution_date": "",
    "executed_by": "",
    "artifacts": {
      "iab_mapping": "smsads-iab-mapping-{GEO}-{YYYY-MM}.md",
      "compat_scores": "smsads-compat-scores-{GEO}-{YYYY-MM}.csv",
      "offers_raw": "smsads-offers-raw-{GEO}-{YYYY-MM}.csv",
      "ec1k_estimate": "smsads-ec1k-estimate-{GEO}-{YYYY-MM}.csv",
      "vertical_ranking": "smsads-vertical-ranking-{GEO}-{YYYY-MM}.md"
    }
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- La activarea oricarui GEO VOX nou (obligatoriu inainte de prima campanie)
- La review trimestrial al verticalelor active per GEO
- La propunerea unui vertical nou de catre AM 3SNET sau echipa
- Pre-lansare Romania (April 2026): executie OBLIGATORIE cu minim 4 saptamani inainte

### WHEN
- Trigger GEO nou: executie in max 72h de la confirmarea accesului VOX
- Trigger trimestrial: Q1, Q2, Q3, Q4 (prima saptamana)
- Trigger propunere vertical: executie in max 48h de la propunere

### HOW (violation detection)
- VIOLATION: Campanie lansata intr-un vertical nou fara SMSADS-P1 completat = BLOCARE
- VIOLATION: Vertical cu scor <2.5 acceptat fara aprobare scrisa Pafi = BLOCARE
- WARNING: SMSADS-P1 neactualizat >90 zile pentru GEO activ = WARNING automat
- Runner: verificare manuala la session start + notificare in briefing Genie

### CONNECT
- **SMSADS-P2** (Product Finder) — primeste top 2-3 verticale din SMSADS-P1 pentru scoring detaliat oferte
- **SMSADS-P3** (Competitor Intel) — primeste verticalele selectate pentru analiza competitiva
- **SMSADS-P4** (Network Discovery) — primeste GEO-ul analizat pentru discovery retele
- **M-122** (SMS Vertical Discovery) — procedura similara; SMSADS-P1 o extinde cu focus pipeline specific
- **M-125** (VOX Taxonomy Segmentation) — primeste verticalele aprobate si le segmenteaza IAB
- `procedure-health.json` → adauga entry: `SMSADS-P1-VERTICAL-DISCOVERY`

### VERIFY (checkpoint procedural — emis ca VK in sesiune)
La finalul executiei procedurii, verifica:
- [ ] Procedura executata complet? (toti pasii 1-6 cu artifacts documentate)
- [ ] Output satisface criteriile din §1? (top 5 verticale rankate, EC/1K calculat per vertical)
- [ ] VK emis in sesiune? (ambele linii vizibile pentru Pafi)
- [ ] Daca oricare = NU → procedura NU e completa, nu marca DONE

**Doua VK-uri obligatorii (VK-H-001)**:
1. `[PROC] SMSADS-P1 | §1 §2 §3 §4 | {GEO} | {nr_verticale} verticale rankate | complete`
2. `[CORTEX] "SMSADS-P1: ranking verticale {GEO} {YYYY-MM}" | FORGE | rule: TRAIN-S-001 | v1.0`

### MODEL ROUTING

| Activitate | Model | Motivul |
|---|---|---|
| Pas 1: Audit IAB categorii | Sonnet | Structurare date VOX existente, fara sinteza complexa |
| Pas 2: Scoring compatibilitate | Sonnet | Calcul tabelar deterministic per dimensiune |
| Pas 3: Discovery oferte | Sonnet | Agregare date din multiple retele, task repetitiv |
| Pas 4: Estimare EC/1K SMS | Opus | Modelare economica + benchmark interpretation + context SMSads |
| Pas 5: Ranking si matrice | Opus | Sinteza scoring + rationament prioritizare strategic |
| Pas 6: Cortex save + handoff | Sonnet | Task mecanic de save si creare task |

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|---|---|---|
| VOX Platform | Sursa date IAB categorii + volume | Request formal catre Ehsan (AM VOX) |
| 3SNET Dashboard | Oferte disponibile per GEO/vertical | go.3snet.co / cont #30063 |
| MaxBounty Dashboard | Oferte finance, health, apps | maxbounty.com |
| ClickDealer Dashboard | Oferte CPI/apps/mobile | clickdealer.com |
| Mobidea Dashboard | Oferte mobile/carrier billing | mobidea.com |
| SMSADS-P2 | Downstream: primeste top verticale | `SMSADS-P2-product-finder.md` |
| M-125 | Downstream: segmentare IAB campanii | `~/.nexus/procedures/training/affiliate/M-125-vox-taxonomy-segmentation.md` |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---|---|---|
| Verticale noi identificate per GEO per trimestru | Diversificare pipeline | Minim 3 cu scor >3.0 |
| EC/1K SMS estimat vs. EC/1K SMS real | Acuratete estimare | <30% deviatie dupa 30 zile campanie |
| Timp executie completa SMSADS-P1 | Eficienta procedurii | <3 ore per GEO |
| Verticale Prioritate 1 lansate in 30 zile | Viteza go-to-market | 100% din identificate |
| Revenue procentual din verticale non-gambling (6 luni) | Diversificare revenue | >20% din total |

---

## Checklist Pre-Publicare

- [x] Regula asociata: TRAIN-S-001
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint cu 4 checks obligatorii
- [x] Doi VK-uri obligatorii per VK-H-001 specificati
- [x] MODEL ROUTING prezent cu justificare per pas
- [x] Artifacts cu naming convention per fiecare pas
- [x] Formule de scoring concrete (compatibilitate + EC/1K + scor final)
- [x] Checkpoints PASS/FAIL per pas
- [x] Conexiuni la SMSADS-P2, P3, P4 + M-122, M-125 documentate
- [x] Limbaj romana cu termeni tehnici in engleza
