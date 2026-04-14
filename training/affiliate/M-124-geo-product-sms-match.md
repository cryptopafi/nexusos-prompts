---
type: procedure
created: 2026-03-20
status: active
slug: m-124-geo-product-sms-match
tags: [procedure, nexus]
---

# M-124 — GEO-Product Affinity Scoring for SMS Carrier

**Domeniu**: affiliate | **Sub-domeniu**: geo-product-match
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Conexiuni**: M-114, M-118, M-122, M-123, M-125
**Trigger**: Quarterly review + la activarea unui nou GEO VOX

---

## Scope

Framework de scoring sistematic pentru identificarea celei mai bune combinații GEO × Produs pentru livrare SMS via carrier (VOX). Integrează: disponibilitatea VOX per GEO, categoriile IAB disponibile și dimensiunea lor, ofertele active pe rețelele de afiliere, și constrângerile de consent per operator GSM. Output final: ranking top 5 GEO × Produs cu entry strategy per slot și next actions clare.

---

## Section 1 — Problema

Fără un framework sistematic de GEO-product matching, campaniile SMS sunt lansate pe baza intuiției și oportunismului de moment, nu a datelor. Consecințe directe:

**Exemple concrete de decizii suboptimale în SMSads**:
- Bolivia a fost selectat pentru betting NU pentru că datele indicau potrivire optimă, ci pentru că 3SNET avea oferta disponibilă și VOX Bolivia era primul GEO activ.
- Romania Finance: 1.7M contacte în categoriile Finance&Insurance + Business&Finance stau neutilizate până în aprilie — zero strategie de pre-lansare pentru ce produs se va trimite primei.
- Myanmar: 1.85M contacte în segmentul Finance sunt identificate dar ignorate în absența unui scoring care să cuantifice potențialul vs. efortul de activare.

**Ce se pierde fără M-124**:
- Lansări în GEO-uri cu IAB categories nepotrivite ofertei active (ex: trimis casino offer la Business&Industrial — categorie dominantă Romania, 2.2M, relevanță scăzută pentru gambling)
- Budget alocat pe GEO-uri cu consent model incompatibil cu oferta (ex: oferte care cer opt-in trimise în GEO cu Light/None consent — risc compliance M-118)
- Oportunități cu EC/1K SMS ridicat ratate: AvaTrade $250 CPA în Bolivia cu 116K Business&Finance — testul de proof-of-concept pentru verticalul non-betting a întârziat luni
- Nu există vizibilitate comparativă: care e mai bun — 500K SMS în Bolivia betting sau 200K SMS în Romania trading (BLOCKED, dar planificarea se face acum)?

M-124 transformă decizia de lansare dintr-un reflex în un proces calculat cu score obiectiv.

---

## Section 2 — Procedura

### Step 1 — VOX GEO Inventory

Documentează toate GEO-urile VOX active, planificate și în research. Actualizează la fiecare nou contact cu VOX sau la schimbare de status.

**GEO Inventory Template**:

| GEO | Operator | Total Volume | Top IAB Categories | Consent Model | SMS Cost Est. | Status | Note |
|-----|----------|-------------|-------------------|---------------|---------------|--------|------|
| Bolivia | Viva | 2.4M | Technology 693K, Software 523K, Apps 457K, Arts&Ent 246K, Web Services 144K, Travel 130K, Business&Finance 116K, Business&Industrial 100K | Light/None | ~$0.005-0.01/SMS | ACTIVE | 33 categorii IAB totale. RevShare 50/50 |
| Romania | Vodafone | 20M | Business&Industrial 2.2M, Business&Finance 2.1M, Logistics 1.7M, Finance&Insurance 1.7M, CPG 1.4M, Health 1.2M, Software 1.0M, Tech 981K | Opt-in REQUIRED | ~$0.02-0.05/SMS | BLOCKED until April 2026 | 45 categorii IAB. Opt-in baza existenta Vodafone |
| Myanmar | U9 | 50M | Finance 1.85M (confirmat), alte categorii TBD | TBD (research necesar) | TBD | PLANNED | Contact VOX pentru detalii complete categorii IAB |

**Campuri obligatorii per GEO nou**:
- Operator name + tip acces (direct GSM vs. aggregator)
- Total volume reachable (nu total subscribers al operatorului)
- IAB categories disponibile cu dimensiune exactă (request formal catre VOX)
- Consent model: opt-in explicit / light consent / none (historical data)
- Cost estimate per SMS (solicita media card VOX)
- Status: ACTIVE / BLOCKED (cu data) / PLANNED / RESEARCH
- Restricții de conținut specifice operatorului

### Step 2 — Product-to-IAB Mapping

Pentru fiecare categorie de produs afiliat, identifică categoriile IAB VOX care reprezintă cea mai bună audiență targetabilă. Maparea se bazează pe comportamentul de consum și interesele implicite ale segmentului IAB.

**Logica de mapare**:
- Betting/Casino → utilizatori care consumă entertainment, sporturi, tehnologie (nu utilizatorii B2B)
- Trading/Forex → utilizatori cu interes financiar explicit sau de business
- Health/Suplimente → categorie Health directă + CPG (Consumer Packaged Goods)
- Apps (mobile) → Technology, Software, Apps (match direct pe interese digitale)
- E-commerce → CPG + Travel (comportament de cumpărare online)

**Product-to-IAB Mapping Table**:

| Product Category | Primary IAB Match | Secondary IAB Match | Avoid | Audience Bolivia | Audience Romania | Audience Myanmar |
|-----------------|-------------------|--------------------|----|-----------------|-----------------|-----------------|
| Betting / Casino | Arts&Entertainment, Technology | Software, Apps | Business&Industrial, Logistics | ~939K (246K+693K) | ~3.2M (est.) | TBD |
| Trading / Forex / CFD | Business&Finance, Finance&Insurance | Business&Industrial, Software | Arts&Entertainment, Travel | ~216K (116K+100K) | ~3.8M (2.1M+1.7M) | ~1.85M |
| Health / Suplimente | Health | CPG | Logistics, Web Services | N/A (categorie neconfirmata BO) | ~2.6M (1.2M+1.4M) | TBD |
| App Downloads (mobile) | Apps, Software, Technology | Web Services | Business&Industrial, Logistics | ~1.5M (457K+523K+693K) | ~2.0M (est.) | TBD |
| E-commerce | CPG, Travel | Business&Finance | Logistics (B2B) | ~130K Travel direct | ~1.4M CPG | TBD |

**Instrucțiuni de actualizare**: La adăugarea unui nou produs/vertical, completează maparea ÎNAINTE de a lansa campania. Consultă M-122 (SMS Vertical Discovery) pentru identificarea ofertei.

### Step 3 — Affinity Scoring per GEO x Product

Calculează un composite score 0-100 pentru fiecare combinație GEO × Produs. Formula cu ponderi:

```
Affinity Score =
  (IAB Audience Size Score × 0.30) +
  (Offer Availability Score × 0.25) +
  (Consent Compatibility Score × 0.25) +
  (EC/1K SMS Score × 0.20)
```

**Calculul fiecărui sub-score (0-10 scale → normalizat)**:

**IAB Audience Size Score** (0-10):
- Audiența relevantă (primary + secondary IAB) ca % din total volum GEO
- >50% din total = 10 | 30-50% = 8 | 15-30% = 6 | 5-15% = 4 | <5% = 2

**Offer Availability Score** (0-10):
- Ofertă activă + testată pe 3SNET/rețea = 10
- Ofertă disponibilă dar netestată = 7
- Ofertă disponibilă în GEO adjacent (poate fi extinsă) = 4
- Nicio ofertă identificată = 0

**Consent Compatibility Score** (0-10):
- GEO None/Light + produs fără restricții stricte = 10
- GEO Opt-in + produs cu ofertă curată (fără gambling flags) = 8
- GEO Opt-in + produs gambling = 5 (verificare M-118 obligatorie)
- GEO cu restricții legale identificate = 0-3 (blocat de M-118)

**EC/1K SMS Score** (0-10) — din M-123:
- EC/1K SMS > $50 = 10 | $20-50 = 8 | $10-20 = 6 | $5-10 = 4 | <$5 = 2
- Dacă nu există date M-123: estimare conservativă = 4

**Exemplu calcul — Bolivia × Betting (MarathonBet)**:
- IAB Audience: Arts&Ent 246K + Technology 693K = 939K / 2.4M = 39% → Score 8
- Offer Availability: MarathonBet activ, testat = 10
- Consent: Light/None + betting = 10
- EC/1K SMS: date reale din campanii active → assume $15/1K = 6
- **Total: (8×0.30) + (10×0.25) + (10×0.25) + (6×0.20) = 2.4+2.5+2.5+1.2 = 8.6/10 → Score 86**

### Step 4 — Regulatory Gate

Înainte de a include o combinație GEO × Produs în priority matrix, validează conformitatea legală. Referință principală: M-118 (Compliance Framework).

**Gambling / Betting** (referință M-118):
- Bolivia: Verifică dacă operatorul (ex: MarathonBet) deține licență boliviană sau operează în zona gri acceptată
- Romania: Licență ONJN obligatorie pentru orice ofertă gambling. Verificare înainte de lansare aprilie
- Myanmar: Research necesar — statut legal betting online

**Finance / Trading / Forex** (non-gambling):
- Bolivia: Verifică cu ASFI (Autoridad de Supervisión del Sistema Financiero) dacă oferta (ex: AvaTrade) poate fi promovată fără licență locală
- Romania: Verifică cu ASF (Autoritatea de Supraveghere Financiară) — CFD/forex advertising restrictions
- Myanmar: Research necesar — statut legal brokers forex

**Health / Suplimente**:
- Bolivia: DGSS (Dirección General de Servicios de Salud) — restricții claim-uri medicale
- Romania: ANMDM — interdicție claim-uri terapeutice în SMS marketing

**Output Regulatory Gate**: pentru fiecare combinație, notează: CLEAR / CONDITIONAL (cu condiții specifice) / BLOCKED (cu motiv).

### Step 5 — Competition Density Check

Înainte de a aloca buget, evaluează dacă spațiul SMS pentru produs × GEO este deja saturat.

**Surse de verificare**:
1. **STM Forum** (stmforum.com) — thread-uri pe GEO specific + traffic source SMS
2. **AdPlexity Mobile** — monitorizare campanii mobile active per GEO, dacă abonament disponibil
3. **3SNET Account Manager** — întreabă direct: "Câți afiliați activi lucrează SMS pentru [Ofertă] în [GEO]?"
4. **Observație directă** — primești SMS cu oferte competitive ca utilizator local? (test număr local)
5. **VOX direct** — VOX știe câți clienți activi au per ofertă/vertical (pot refuza să dezvăluie, dar merită întrebat)

**Scoring competition**:
- 0 competitori SMS confirmați = low competition (oportunitate)
- 1-3 competitori = moderate (intrare posibilă cu diferențiere copy)
- 4+ competitori activi = saturație (reconsideră sau intră cu ofertă diferită)

### Step 6 — Economic Model

Calculează Break-Even volume pentru a determina viabilitatea campaniei.

**Formula Break-Even**:
```
BE_volume (SMS) = Campaign Setup Cost / (EC/1K SMS / 1000)
```

**Unde**:
- Campaign Setup Cost = costul fix de pregătire (setup tracker Binom, creare landing, traducere copy) — estimat $200-500 per campanie nouă
- EC/1K SMS = Earnings per 1,000 SMS trimise (din M-123 sau estimare)

**La RevShare VOX 50/50**:
- Payout brut MarathonBet: 30% din pierderi jucători
- Payout net SMSads (după VOX split): 15% din pierderi
- Dacă un jucător pierde $100 → SMSads primeste $15
- EC/1K SMS = (rata conversie / 1000) × $15 per jucător = variabil

**Payout minimum pentru viabilitate**:
```
Min_Payout_per_Conversion = (Cost/SMS × 1000) / (1000 × CR_estimat)
```

Exemplu: Cost/SMS = $0.01, CR = 0.5% (5 conversii / 1000 SMS)
→ Min payout viabil = $10 / 0.5% = $2 per conversie (dacă CPA model)
→ Pentru RevShare: trebuie generat minim $2/conversie LTV

**Semnale de neviabilitate**:
- EC/1K SMS < cost/SMS × 1000 (pierzi bani per SMS trimis)
- Break-Even volume > volumul disponibil în segmentul IAB targetat

### Step 7 — Priority Matrix Output

Combină toate score-urile și output-urile în matricea finală. Actualizează trimestrial sau la fiecare activare GEO nou.

**GEO × Product Priority Matrix**:

| Rank | GEO | Product | Affinity Score | IAB Audience Relevant | EC/1K SMS Est. | Regulatory Status | Blocker | Next Action |
|------|-----|---------|---------------|----------------------|----------------|-------------------|---------|-------------|
| 1 | Bolivia | Betting (MarathonBet) | 86/100 | 939K (Arts&Ent + Tech) | Date reale disponibile | CLEAR | Niciun blocker activ | Scale campanii existente; testează segmentare M-125 |
| 2 | Romania | Finance / Trading | 82/100 | 3.8M (Business&Finance + Finance&Insurance) | TBD — estimare $25-40/1K | CONDITIONAL (ASF check) | BLOCKED until April 2026 | Pregătire: selectează ofertă forex/CFD (M-122), verifică ASF (M-118), pregătește landing RO |
| 3 | Bolivia | App Downloads | 74/100 | 1.5M (Apps + Software + Tech) | TBD | CLEAR | Lipsă ofertă activă pe 3SNET | Caută ofertă app install BO pe 3SNET/alte rețele (M-115) |
| 4 | Romania | Health / Suplimente | 65/100 | 2.6M (Health + CPG) | TBD | CONDITIONAL (ANMDM claims) | BLOCKED until April + compliance claims | Research oferte health RO fără claim-uri terapeutice |
| 5 | Myanmar | Finance / Trading | 61/100 | 1.85M (Finance confirmat) | TBD | RESEARCH (legal status unclear) | VOX detalii incomplete; legal research necesar | Request full category breakdown de la VOX; research legal framework Myanmar forex |

**Interpretarea matrix**:
- Score >80: lansare prioritară sau scale imediat
- Score 60-80: pregătire activă, blocker-e de rezolvat
- Score 40-60: research suplimentar necesar
- Score <40: deprioritizează sau elimină

---

## Section 3 — Cortex Logging

La finalizarea fiecărui scoring cycle, salvează în Cortex:

```json
{
  "domain": "smsads",
  "sub_domain": "geo-product-match",
  "procedure": "M-124",
  "version": "1.0.0",
  "cycle_date": "YYYY-MM-DD",
  "active_geos": ["Bolivia", "Romania", "Myanmar"],
  "top_combination": {
    "geo": "Bolivia",
    "product": "Betting",
    "score": 86,
    "status": "ACTIVE"
  },
  "priority_matrix": [
    {"rank": 1, "geo": "Bolivia", "product": "Betting", "score": 86, "blocker": "none"},
    {"rank": 2, "geo": "Romania", "product": "Finance/Trading", "score": 82, "blocker": "BLOCKED until April"},
    {"rank": 3, "geo": "Bolivia", "product": "App Downloads", "score": 74, "blocker": "no offer"},
    {"rank": 4, "geo": "Romania", "product": "Health", "score": 65, "blocker": "BLOCKED + compliance"},
    {"rank": 5, "geo": "Myanmar", "product": "Finance", "score": 61, "blocker": "research incomplete"}
  ],
  "next_review": "YYYY-MM-DD (Q+3 months or new GEO activation)",
  "notes": ""
}
```

---

## Section 4 — Enforcement Loop

**WHERE**: Se execută la activarea oricărui GEO VOX nou SAU la review trimestrial al combinațiilor active. Rulat de affiliate manager înainte de orice decizie de lansare campanie.

**WHEN**:
- Quarterly (la 3 luni): rescorare completă a tuturor combinațiilor
- La activarea unui GEO VOX nou: scoring imediat înainte de prima campanie
- La adăugarea unei oferte noi pe 3SNET: re-evaluare Offer Availability Score pentru GEO-urile active

**HOW**:
- VIOLATION: lansare campanie SMS în GEO nou fără M-124 completat = operațiune blocată
- VIOLATION: alocare buget pe combinație cu Regulatory Status = BLOCKED
- WARNING: sub-scor Consent Compatibility ignorat → compliance risk automat M-118

**CONNECT**:
- M-114 (GEO Scoring) — alimentează date de bază per GEO (market size, maturity)
- M-118 (Compliance) — gate obligatoriu la Step 4; nicio combinație nu trece fără clearance
- M-122 (SMS Vertical Discovery) — furnizează lista de verticale disponibile per GEO
- M-123 (Offer Commission Scoring) — furnizează EC/1K SMS pentru sub-score-ul economic
- M-125 (VOX Taxonomy Segmentation) — primește top combinații din M-124 și le optimizează prin segmentare IAB

**VERIFY** (5 checks obligatorii la finalizare):
- [ ] Scoring matrix completată cu minim 5 combinații GEO × Produs evaluate
- [ ] Regulatory Gate completat pentru toate combinațiile (CLEAR/CONDITIONAL/BLOCKED explicit)
- [ ] Affinity Score calculat cu formula completă (4 sub-score-uri cu ponderi sumând 1.0)
- [ ] Economic model (Break-Even volume) calculat pentru top 3 combinații
- [ ] Priority Matrix exportată cu Next Action definit per slot

**MODEL ROUTING**:

| Step | Model | Justificare |
|------|-------|-------------|
| Step 1 — VOX GEO Inventory | Sonnet | Structurare date existente, fără sinteză complexă |
| Step 2 — Product-to-IAB Mapping | Sonnet | Mapare tabelară, logică deterministă |
| Step 3 — Affinity Scoring | Opus | Calcul composite scoring + interpretare context SMSads |
| Step 4 — Regulatory Gate | Opus | Analiza legală multi-jurisdicție, risc ridicat |
| Step 5 — Competition Density | Sonnet | Agregare date externe, fără judecată complexă |
| Step 6 — Economic Model | Opus | Modelare financiară + recomandare viabilitate |
| Step 7 — Priority Matrix | Sonnet | Sinteza tabelară a output-urilor precedente |

**PROHIBIT**:
- Lansarea unei campanii SMS în GEO nou fără M-124 completat
- Alocarea bugetului pe produs × GEO cu Regulatory Status = BLOCKED
- Ignorarea sub-scorului Consent Compatibility (risc compliance direct)

---

## Section 5 — Dependente

| Dependenta | Tip | Cum se obtine |
|-----------|-----|---------------|
| VOX Platform Data | Input principal | Request direct catre account manager VOX: full IAB category list + volume per operator |
| 3SNET Offer List | Input principal | Dashboard 3SNET + contact AM pentru GEO-uri noi |
| M-122 Output | Input procedural | Ruleaza M-122 (SMS Vertical Discovery) inainte de M-124 |
| M-123 Output | Input procedural | EC/1K SMS benchmarks din M-123 (Offer Commission Scoring) |
| M-118 Clearance | Gate obligatoriu | Ruleaza M-118 (Compliance) la Step 4 pentru fiecare GEO × Produs |
| Binom Tracker | Date de performanta | Date istorice CR/CTR din campaniile active (Kody access) |
| STM Forum / AdPlexity | Intelligence extern | Abonament AdPlexity Mobile recomandat pentru Step 5 |

---

## Section 6 — Metrics

| Metric | Target | Sursa |
|--------|--------|-------|
| Combinatii evaluate per cycle | Minim 5 GEO × Produs | M-124 matrix |
| Acuratete scoring vs. performanta reala | ±20% fata de EC/1K SMS actual | Comparatie M-123 post-campanie |
| Lead time lansare campanie (cu M-124) | <2 saptamani de la decizie | Timestamp decizie → prima trimitere |
| Combinatii BLOCKED corect identificate | 100% (zero false negative) | M-118 audit post-hoc |
| Coverage GEO-uri VOX documentate | 100% din GEO-uri active + planned | VOX account manager confirmation |

---

## Checklist Pre-Publicare

- [x] Scope definit clar (GEO × Produs scoring sistematic)
- [x] Problema documentata cu exemple concrete SMSads
- [x] Step 1: VOX GEO Inventory template complet cu date reale (BO, RO, MM)
- [x] Step 2: Product-to-IAB mapping table cu toate verticalele principale
- [x] Step 3: Formula scoring cu ponderi + exemplu calcul Bolivia × Betting
- [x] Step 4: Regulatory gate cu autoritati per GEO si vertical
- [x] Step 5: Competition density check cu surse concrete
- [x] Step 6: Economic model cu formula Break-Even si exemplu numeric
- [x] Step 7: Priority Matrix template cu 5 combinatii exemple
- [x] Section 3: Cortex JSON complet
- [x] Section 4: Enforcement Loop cu conexiuni M-114, M-118, M-122, M-123, M-125
- [x] Section 5: Dependente documentate
- [x] Section 6: Metrics cu targets
- [x] Procedura in romana cu termeni tehnici in engleza
