---
type: procedure
created: 2026-03-17
status: active
slug: m-115-offer-discovery-per-geo
tags: [procedure, nexus]
---

# Affiliate Offer Discovery per GEO — Product-Country Match — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Scope**: Descoperire sistematica de oferte afiliat optime pentru piete geografice deja selectate. Acoperire completa: retele gambling-specific + CPA generice, payment compatibility gate, payout model LTV calcul, saturare competitiva, si ranking top 5 oferte per GEO cu action plan.

---

## 1. Problema

Cu GEO-urile alese, selectia ofertelor se face manual, din retelele CPA generice, fara acoperire platforme gambling-specifice. Oferte cu payout-uri 40-80% superioare pe platforme ca Income Access, SoftSwiss, EveryMatrix raman neexplorate. Payment method compatibility nu e verificata sistematic → CR 0 pe oferte incompatibile.

**Situatii acoperite:**
- Intrare pe piata noua cu GEO deja validat (output M-114)
- Refresh oferte pe GEO activ (trimestrial sau la scadere CR)
- Extindere portofoliu oferte pe GEO existent
- Comparatie inter-retele pentru acelasi operator/GEO
- Evaluare rev-share vs CPA pe baza LTV calculat

**Ce se intampla fara procedura:**
- Oferte descoperite doar pe 2-3 retele CPA generice → miss pe 40-80% payout premium
- Payment method incompatibil descoperit post-lansare → buget pierdut, CR 0
- Oferte saturate selectate (10+ afiliati activi) → CPC inflat, ROI negativ
- Rev-share vs CPA ales fara calcul LTV → pierdere venituri pe termen lung
- Operatori fara licenta locala selectati → compliance violation, cont suspendat

## 2. Procedura

### Pas 1: PROFILE GEO-URI INPUT

Per GEO primit din M-114 (Pipeline A), compileaza profil complet:
- **Statut legal**: licenciat / grey / restrictii specifice (din M-114 Pas 3)
- **Payment methods dominante**:
  - Kenya: Mpesa (93% mobile money), Airtel Money
  - Peru: Yape (15M+ useri), Plin, BCP direct, Interbank
  - Chile: Webpay (standard), Khipu, Banco Estado, MACH
  - Colombia: PSE, Nequi (20M+), Daviplata
  - Nigeria: OPay, Paystack, Flutterwave, bank transfer
  - Tanzania: M-Pesa TZ, Tigo Pesa
- **Limbi locale**: es-PE, es-CL, sw-KE, en-KE, en-NG
- **Device split**: mobile vs desktop ratio
- **Canale publicitare disponibile**: fara restrictii pe vertical
- **Sezonalitate**: evenimente sportive locale, holidays, pay cycles
- **Compliance notes**: disclaimeri obligatorii, restrictii advertising (din M-118)

**Tool**: Claude Opus — compilare profil din output M-114 si research suplimentar; Perplexity Pro — verificare payment methods si sezonalitate curenta.
**Output**: Document `geo-profile-{GEO}.json` cu toate campurile completate (GEO Profile Card).
**Checkpoint**: Toate 7 campurile completate per GEO. Payment methods cu sursa verificata. Statut legal consistent cu M-114.

### Pas 2: QUERY RETELE GAMBLING-SPECIFIC

Platforme de interogat (in ordine prioritate):
1. **Income Access** — premium casino/sportsbook programs
2. **SoftSwiss Affiliates** — crypto-friendly operators
3. **EveryMatrix** — white-label operators LATAM/Africa
4. **Kindred Affiliates** (Unibet, 32Red) — piete reglementate
5. **Bet365 Affiliates** — global, LATAM strong
6. **BetConstruct** — B2B platform, multiple operators

Per platforma, filtreaza pe:
- GEO target (whitelist)
- Traffic type accepted
- Payout model disponibil
- Minimum requirements (traffic volume, site quality)

**Tool**: Browser manual — navigare dashboards gambling affiliate; Claude Opus — structurare si comparatie oferte gasite.
**Output**: Document `offers-gambling-specific-{GEO}.md` cu tabel: Platforma | Operator | Payout Model | CPA/Rev-share% | GEO | Traffic Req | Contact/Signup.
**Checkpoint**: Minim 4 din 6 platforme interogate. Minim 5 oferte gasite per GEO. Fiecare oferta cu payout model explicit.

### Pas 3: QUERY RETELE CPA GENERICE

Platforme de interogat:
1. **MaxBounty** — gambling vertical (filtru explicit)
2. **Perform[cb]** — performance offers
3. **GlobalWide Media** — emerging markets
4. **ClickDealer** — LATAM/Africa coverage
5. **Mobidea** — mobile-first, push/pop
6. **Advidi** — gambling/dating specialist

Filtre aplicate:
- GEO target
- Traffic type match
- Payout minim: CPA >$80 sau rev-share >25%
- Vertical: gambling / casino / sportsbook / betting

**Tool**: Browser manual — navigare dashboards CPA; Claude Opus — filtrare si structurare oferte.
**Output**: Document `offers-cpa-generic-{GEO}.md` cu 20-30 oferte candidate. Merge cu lista din Pas 2 intr-un master list.
**Checkpoint**: Minim 3 din 6 platforme interogate. Payout minim respectat (CPA >$80 sau rev-share >25%). Lista merged cu Pas 2 = minim 15 oferte totale per GEO.

### Pas 4: SCORE PE PAYMENT COMPATIBILITY

Per oferta candidat, verifica daca operatorul accepta depozite prin payment methods locale:

| GEO | Payment Method Primar | Obligatoriu |
|-----|----------------------|-------------|
| Kenya | Mpesa | DA |
| Peru | Yape / Plin | DA |
| Chile | Webpay / Khipu | DA |
| Colombia | PSE / Nequi | DA |
| Nigeria | OPay / Paystack | DA |
| Tanzania | M-Pesa TZ / Tigo Pesa | DA |

**Regula**: Daca oferta NU accepta payment method primar local → **ELIMINAT AUTOMAT**.

Verificare surse (in ordine prioritate):
1. Pagina depozit operator (direct — screenshot recomandat)
2. Terms & conditions operator
3. Affiliate manager confirmation (daca neclar — documentat in scris)

**Tool**: Browser manual — verificare pagina depozit operator; Claude Opus — analiza terms; contact AM daca neclar.
**Output**: Document `payment-filter-{GEO}.md` cu tabel: Oferta | Operator | Payment Primar Present (DA/NU) | Sursa Verificare | Status (PASS/ELIMINAT).
**Checkpoint**: Fiecare oferta verificata contra payment method primar. Sursa verificare documentata per oferta. Toate oferte fara payment match eliminate — nu apar in Pas 5+.

### Pas 5: SCORE PAYOUT MODEL

Per oferta ramasa, calculeaza valoarea asteptata pe 6 luni:

**CPA Model**:
- `EV_CPA = payout_CPA x conversii_estimate_6mo`
- `conversii_estimate = traffic_lunar x CR_benchmark x 6`

**Rev-Share Model**:
- `EV_revshare = (depozit_mediu x frecventa_lunara x 6 x revshare%) - cost_trafic_6mo`
- `depozit_mediu` = sursa: operator average sau benchmark vertical
- `frecventa` = 2-4x/luna gambling, 1x/luna finance

**Regula decizie**:
- Daca `LTV_revshare > 4 x CPA` → preferinta rev-share
- Daca `LTV_revshare < 2 x CPA` → preferinta CPA
- Intre 2-4x → hybrid daca disponibil, altfel CPA (safer)

**Tool**: Claude Opus — calcul LTV si EV per model; Voluum historical data — CR benchmarks per vertical/GEO.
**Output**: Document `payout-analysis-{GEO}.md` cu tabel: Oferta | EV_CPA_6mo | EV_RevShare_6mo | Ratio | Model Recomandat | Rationale.
**Checkpoint**: EV calculat pentru ambele modele (CPA si rev-share) per oferta. Regula decizie aplicata cu rationale documentata. Asumptii (CR, depozit mediu, frecventa) explicitate.

### Pas 6: COMPETITOR SATURATION CHECK

Per oferta/GEO, masoara saturare:

**Metode verificare**:
1. Google Search: `"[operator] [tara] review"` — nr rezultate organice
2. Google Search: `"[operator] bonus [tara]"` — nr pagini affiliate
3. Meta Ad Library: reclame active pe GEO cu operator name
4. TikTok Creative Center: ads pe vertical/GEO
5. SimilarWeb: top referrers catre operator (nr affiliate sites)

**Score saturare** (0-10):
- 0-3 = **Oportunitate** (low competition) — prioritate maxima
- 4-6 = **Moderat** (viable cu diferentiere) — proceed cu strategie unica
- 7-9 = **Saturat** (ROI risc, evita fara avantaj clar) — deprioritize
- 10 = **Hyper-saturat** (skip) — elimina din ranking

**Tool**: Google Search — organic results count; Meta Ad Library — active ads; TikTok Creative Center — competitor creatives; SimilarWeb — referrer analysis.
**Output**: Document `saturation-check-{GEO}.md` cu tabel: Oferta | Operator | Google Results | Meta Ads Active | TikTok Ads | SimilarWeb Referrers | Score (0-10).
**Checkpoint**: Minim 3 din 5 metode aplicate per oferta. Score calculat si documentat cu surse. Oferte cu score 10 eliminate din ranking.

### Pas 7: RANK TOP 5 PER GEO

Scor compozit final per oferta:

- Payment_compat: GO/NOGO gate (deja filtrat la Pas 4)
- Payout_EV_normalized: 0-30 (din Pas 5)
- Saturation_inverse: 0-30 (din Pas 6, inversat: low saturation = high score)
- Compliance_bonus: 0-20 (operator cu licenta locala = +20, fara = 0)
- Localization_bonus: 0-20 (operator cu site in limba locala + payment local prominent = +20)

**Template Offer Comparison Table:**

| # | Operator | Oferta ID | Payout Model | EV 6mo (USD) | Payout EV (0-30) | Saturation (0-30) | Compliance (0-20) | Localizare (0-20) | TOTAL (0-100) | Canal Recomandat | Format Creativ |
|---|----------|-----------|-------------- |-------------- |-------------------|-------------------|-------------------|-------------------|---------------|------------------|----------------|
| 1 | BetOperator A | OF-001 | Rev-share 35% | $4,200 | 28 | 26 | 20 | 18 | 92 | TikTok Ads | Review site + social |
| 2 | CasinoX | OF-015 | CPA $120 | $3,600 | 24 | 22 | 20 | 16 | 82 | Meta Ads | Landing page |
| 3 | SportBet Y | OF-042 | Hybrid | $3,100 | 21 | 24 | 15 | 14 | 74 | Google Ads | Review site |
| 4 | Operator Z | OF-088 | CPA $95 | $2,850 | 19 | 20 | 20 | 12 | 71 | Native | Push + LP |
| 5 | BetBrand W | OF-103 | Rev-share 30% | $2,400 | 16 | 18 | 15 | 10 | 59 | Social organic | Content |

*Nota: exemplu ilustrativ. Valorile reale se calculeaza per GEO/oferta.*

**Deliverable final per oferta selectata**:
- Operator + oferta ID
- Scor compozit cu breakdown pe 4 dimensiuni
- Payout model recomandat + EV 6mo
- Canal intrare recomandat (cel cu cel mai bun raport cost/CR)
- Format creativ recomandat (review site / landing / social / push)
- Saturation score cu surse
- Next step actionabil (signup, contact AM, etc.)

**Tool**: Claude Opus — compilare ranking, normalizare scoruri, generare action plan.
**Output**: Document `offer-ranking-{GEO}.md` cu: (1) offer comparison table complet, (2) top 5 oferte rankate cu breakdown, (3) action plan per oferta.
**Checkpoint**: Top 5 oferte per GEO cu scor compozit calculat. Fiecare oferta cu toate 4 dimensiunile scoruite. Action plan cu next step concret per oferta. Zero oferte fara payment compatibility in ranking.

**KPIs procedura**:
- Min 3 oferte valide per GEO cu payment compatibility 100%
- Payout model cu calcul LTV documentat per oferta
- Saturation score <5 pentru min 2 oferte per GEO
- Output complet in <3h per GEO

## 3. Cortex Logging

```json
{
  "text": "M-115 Offer Discovery per GEO — Descoperire sistematica oferte afiliat cu payment gate, LTV scoring, saturare, si ranking top 5 per piata geografica",
  "collection": "procedures",
  "metadata": {
    "type": "procedure",
    "procedure": "M-115",
    "rule_id": "TRAIN-S-001",
    "domain": "affiliate",
    "sub_domain": "offer-discovery",
    "pipeline": "B",
    "version": "2.0",
    "status": "ACTIVE",
    "tags": ["affiliate", "offer-discovery", "gambling", "LATAM", "Africa", "training", "payment-methods", "LTV"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- WISH Step H (intrare piata noua)
- La activare GEO nou din Pipeline A (M-114)
- La refresh trimestrial oferte pe GEO activ

### WHEN
- La fiecare GEO nou activat in portofoliu
- La scadere CR >20% pe oferta curenta (trigger re-discovery)
- La onboarding vertical nou pe GEO existent
- Trimestrial: refresh oferte pe toate GEO-urile active
- Post-schimbare reglementari locale (operator pierde/obtine licenta)

### HOW (violation detection)
- Oferta selectata fara scoring payment compatibility → **VIOLATION CRITICA**
- Oferta selectata fara saturation check → **VIOLATION**
- Payout model ales fara calcul LTV → **VIOLATION**
- Doar retele CPA generice interogate (gambling-specific skipped) → **VIOLATION**
- Mai putin de 3 oferte valide per GEO in output → **WARNING** (re-run)
- Oferta fara licenta locala selectata pe piata reglementata → **VIOLATION CRITICA**
- Offer comparison table incompleta (dimensiune lipsa) → **VIOLATION**

### CONNECT
- M-006 (CPA Networks) — sursa retele generice
- M-007 (Offer Selection) — framework selectie general
- M-002 (Program Evaluation) — validare program operator
- M-112 (Commission Negotiation) — post-selectie, negotiere payout mai bun
- M-114 (Geo Scoring Framework) — Pipeline A, furnizeaza GEO-uri input validate cu scoring
- M-116 (Localization Engine) — consuma output oferte selectate pentru localizare
- M-118 (Gambling Compliance) — framework compliance la Pas 1 si Pas 4

### VERIFY
1. Minim 4 din 6 platforme gambling-specific interogate — verificare log query per platforma in output Pas 2
2. Payment method primar verificat per oferta cu sursa documentata — scan document payment-filter
3. EV calculat pentru ambele modele (CPA + rev-share) per oferta — verificare structurala payout-analysis
4. Saturation check cu minim 3 metode aplicate per oferta — verificare saturation-check document
5. Top 5 ranking cu toate 4 dimensiunile scoruite — verificare offer comparison table completeness
6. Post-lansare (14 zile): CR real vs benchmark per oferta — Voluum report
7. Post-lansare (30 zile): EV real vs calculat — comparatie Voluum vs payout-analysis

### MODEL ROUTING
- **Discovery + scoring**: Claude Opus (multi-source analysis, LTV calculations, ranking logic)
- **Saturation research**: Perplexity Pro (real-time search data, competitor analysis)
- **Quick filters**: Claude Sonnet (cost optimization pe filtrare rapida, payment verification)
- **Payment verification**: manual + operator docs (browser direct)

## 5. Dependente

- **Tools**: Income Access, SoftSwiss, EveryMatrix, MaxBounty, Mobidea, Advidi, Claude Opus, Google Search, Meta Ad Library, TikTok Creative Center, SimilarWeb, Voluum
- **Proceduri input**: M-114 (Geo Scoring — furnizeaza GEO-uri validate cu scoring)
- **Proceduri conectate**: M-006 (CPA Networks), M-007 (Offer Selection), M-002 (Program Evaluation), M-112 (Commission Negotiation), M-118 (Gambling Compliance)
- **Data sources**: Operator websites (deposit methods, terms), affiliate program dashboards, Ad Libraries (Meta, TikTok), SimilarWeb
- **Output consumed by**: M-116 (Localization Engine — oferte selectate per GEO), campaign setup

## 6. Metrics

| Metric | Target | Frecventa | Sursa |
|--------|--------|-----------|-------|
| Oferte valide per GEO | Min 3 cu payment compat 100% | Per execution | Payment filter doc |
| Discovery time | <3h per GEO | Per execution | Time tracking |
| Payment compatibility rate | 100% pentru oferte selectate | Per execution | Payment verification docs |
| Saturation score | <5 pentru min 2 oferte/GEO | Per execution | Saturation check doc |
| LTV accuracy | ±20% vs real la 6 luni | Post 6-month review | Voluum vs payout analysis |
| Payout premium captured | >30% vs generic CPA-only discovery | Quarterly | Comparison gambling-specific vs generic |
| CR post-launch | >benchmark vertical/GEO | 14-day post-launch | Voluum report |
| Platform coverage | 4+ din 6 gambling-specific interogate | Per execution | Query log |

## Checklist Pre-Publicare

- [x] Toate sectiunile FORGE completate (1-6)
- [x] Pasi numerotati secvential (1-7) cu Tool, Output (artifact name) si Checkpoint (criterii masurabile) per pas
- [x] Tools specificate per pas (nu doar lista generica)
- [x] KPIs masurabile cu target numeric si sursa
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] Cortex logging JSON valid cu metadata block complet (type, procedure, rule_id, domain, sub_domain, pipeline, version, status, tags)
- [x] Dependente listate (tools + proceduri + data sources)
- [x] Metrics cu target, frecventa si sursa masurare
- [x] Payment compatibility ca hard gate (eliminare automata) cu tabel referinta
- [x] Connected procedures referenced bidirectional (M-114 <-> M-115, M-115 -> M-116)
- [x] Offer comparison TABLE template cu exemplu complet la Pas 7
- [x] VERIFY cu 7 verificari concrete si metoda specifica per verificare
