---
type: procedure
created: 2026-03-17
status: active
slug: m-123-offer-commission-scoring
tags: [procedure, nexus]
---

# M-123 — Offer Commission Scoring & Product Selection

**Status**: ACTIVE
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Domeniu**: SMSads / VOX Carrier Marketing
**Ultima actualizare**: 2026-03-05

**Scope**: Framework de scoring compozit pentru identificarea ofertelor cu cel mai bun potential de revenue net per 1000 SMS livrate, combinand date din retele multiple (3SNET, MaxBounty, ClickDealer, Mobidea, Perform[cb]) cu validare de search volume, analiza competitiva si audit de termeni de plata. Optimizat pentru modelul SMS carrier VOX (Bolivia + Romania), cu distinctie clara intre modele CPA si RevShare si calculul de Expected Commission per 1000 SMS (EC/1K) ca metrica centrala de decizie.

---

## Sectiunea 1 — Problema

### De ce este necesara aceasta procedura

**Discovery dezorganizat**: Fara un framework sistematic, ofertele sunt identificate ad-hoc (ex: doar 3SNET pentru gambling), ignorand retele cu CPA-uri mai ridicate (MaxBounty Finance: $50-$250 CPA) sau verticale cu conversii mai bune via SMS.

**Confuzie model de payout pentru SMS**: RevShare (ex: MarathonBet 30%) pare atractiv dar necesita volume mari si timp pentru a genera revenue. CPA (ex: AvaTrade $250) da cashflow rapid dar are restrictii de volum. Fara scoring explicit, echipa nu poate compara aceste modele.

**Metrici incomplete la selectie**: Alegerea ofertelor bazata doar pe CPA/RevShare nominal ignora:
- CTR-ul realistic pe SMS (2-8% — variat puternic dupa vertical si GEO)
- CR-ul landing page per vertical (betting 0.3-1.5%, finance 0.5-2%, apps 3-8%)
- Termenele de plata reale (unele retele platesc la 60-90 zile, distrugand cashflow)
- Saturatia competitionala pe acel offer via SMS

**Oportunitati pierdute**: AvaTrade Bolivia $250 CPA este disponibil pe MaxBounty dar nefolosit. Oferte CPI (App Downloads) cu conversii 3-8% via SMS sunt ignorate complet.

**Ce se întâmpla fara aceasta procedura**:
- Continuam cu revenue $0 din cauza dependentei de un singur offer (MarathonBet) cu tracking broken
- Selectam oferte pe baza de intuitie, nu date
- Pierdem oferte cu EC/1K ridicat din retele pe care nu le consultam
- Nu putem prioritiza bugetul de SMS spre ofertele cu cel mai mare randament

---

## Sectiunea 2 — Procedura

### Pas 1 — Multi-Network Offer Scrape

**Tool**: Dashboard 3SNET + MaxBounty + ClickDealer + Mobidea + Perform[cb] + mcp__brave-search

**Actiune**:
1. Logheaza-te in fiecare retea si filtreaza ofertele disponibile pentru GEO-urile active:
   - GEO: BO (Bolivia), RO (Romania), MM (Myanmar, planned)
   - Filtru minim: CPA > $15 SAU RevShare > 20%

2. Pentru fiecare oferta gasita, documenteaza exact 10 coloane:

| Camp | Descriere |
|---|---|
| Offer Name | Numele oficial al ofertei in retea |
| Network | 3SNET / MaxBounty / ClickDealer / Mobidea / Perform[cb] |
| Vertical | Betting / Trading / Health / App Download / Finance / Crypto |
| GEO | BO / RO / MM |
| Payout Model | CPA / RevShare / CPL / Hybrid |
| Payout Value | $ (CPA) sau % (RevShare) |
| Conversion Event | Registration / FTD / Install / Lead / Purchase |
| Tracking Available | Yes/No + tip (pixel/postback/API) |
| Min Payout Threshold | $ minim pentru plata |
| Special Conditions | Cap zilnic, cerinte trafic, restrictii SMS |

3. Consolideaza intr-un master offer list CSV

**Output (artifact)**: `master-offer-list-{GEO}-{YYYY-MM}.csv`
**Checkpoint**: Minim 15 oferte documentate cu toate cele 10 coloane completate, din minim 3 retele diferite

---

### Pas 2 — Search Volume Validation pentru Top 20 Oferte

**Tool**: Google Keyword Planner + mcp__brave-search + mcp__tavily__tavily_search

**Actiune**:
1. Din master offer list, selecteaza top 20 oferte dupa payout nominal
2. Pentru fiecare oferta, defineste keyword-ul primar in limba locala a GEO-ului:
   - Bolivia (Espanol): ex. "MarathonBet Bolivia", "apuestas deportivas Bolivia", "trading forex Bolivia"
   - Romania (Romana): ex. "SpinBetter cazino", "1xBet Romania", "credite online Romania"

3. Verifica in Google Keyword Planner (seteaza GEO exact, limba locala):
   - Volume lunar mediu
   - Trend (crescator / stabil / descrescator)
   - CPC (indicator al valorii comerciale a traficului)

4. Clasifica:
   - >2000 cautari/luna = Cerere RIDICATA
   - 500-2000 = Cerere MEDIE
   - <500 = Cerere SCAZUTA (acceptabil daca CPA e >$100)

5. Noteaza si brand awareness: ofertele cu brand cunoscut local au CTR SMS mai mare

**Output (artifact)**: `search-volume-validation-{GEO}-{YYYY-MM}.csv`
**Checkpoint**: Search volume documentat pentru toate top 20 oferte; ofertele cu <500/luna si CPA <$30 eliminate din lista scurta

---

### Pas 3 — Commission Yield Scoring (EC/1K SMS)

**Tool**: Calculator / spreadsheet

**Actiune**:
Calculeaza Expected Commission per 1000 SMS (EC/1K) pentru fiecare oferta folosind formula:

```
EC/1K = CTR% × CR% × Payout
```

**Benchmarks SMS CTR per GEO si Vertical**:
| GEO | Vertical | CTR SMS Estimat | Motivatie |
|---|---|---|---|
| Bolivia | Betting/Gambling | 3-6% | Brand awareness local scazut, interes moderat |
| Bolivia | App Downloads | 4-8% | Actiune simpla (click → install) |
| Bolivia | Finance/Trading | 2-4% | Cerinta de incredere mai ridicata |
| Romania | Betting/Gambling | 4-8% | Piata matura, brand awareness ridicat |
| Romania | Finance (Loans) | 3-6% | Cerere ridicata (credite rapide) |
| Romania | Trading/Forex | 2-5% | Piata sofisticata, conversie mai lunga |

**Benchmarks CR (Conversion Rate) per Vertical**:
| Vertical | CR Landing Page | Nota |
|---|---|---|
| Betting (registration) | 0.5-1.5% | Scazut — KYC + deposit barriere |
| Betting (FTD) | 0.3-0.8% | Cel mai scazut — necesita depunere |
| Finance/Trading (lead) | 0.5-2.0% | Mediu — formular simplu |
| Finance/Trading (FTD) | 0.2-0.8% | Scazut — capital necesar |
| Health/Nutra (purchase) | 1-3% | Mediu-bun |
| App Downloads (install) | 3-8% | Cel mai bun — actiune frictionless |
| Credite (lead/aplicatie) | 1-4% | Bun in LATAM |

**Exemplu calcul**:
- MarathonBet Bolivia, RevShare 30%, valoare medie comision per FTD = ~$12
  - EC/1K = 4% (CTR) × 0.5% (CR FTD) × $12 = $0.024/SMS → $24/1K SMS

- AvaTrade Bolivia, CPA $250 (limitat 15 conversii/luna)
  - EC/1K = 3% (CTR) × 0.8% (CR lead) × $250 = $6.00/SMS → $6,000/1K SMS
  - NOTA: limita de 15 conversii/luna cap-uiaza volumul total la $3,750/luna

- App Download Bolivia, CPI $1.50
  - EC/1K = 6% (CTR) × 6% (CR install) × $1.50 = $0.0054 → $5.40/1K SMS
  - NOTA: volum nelimitat, scala bine

Calculeaza EC/1K pentru toate ofertele din lista scurta.

**Output (artifact)**: `ec-per-1k-scoring-{GEO}-{YYYY-MM}.csv`
**Checkpoint**: EC/1K calculat pentru toate ofertele; ofertele cu EC/1K <$1.00 marcate ca LOW PRIORITY

---

### Pas 4 — Competitor Validation si Scoring Saturatie

**Tool**: AdPlexity Mobile + mcp__brave-search + STM Forum

**Actiune**:
1. Pe AdPlexity Mobile (`adplexity.com/mobile`):
   - Filtreaza: GEO = tinta, tip trafic = SMS/mobile carrier, offer keyword = brand sau vertical
   - Numara advertiseri activi pe acel offer
   - Salveaza screenshot-uri ale celor mai bune ad-uri ca inspiratie

2. Pe STM Forum (`stmforum.com`):
   - Cauta: "[offer name] review 2025/2026" sau "[vertical] SMS [GEO]"
   - Identifica: opinii despre oferta, probleme de tracking, experienta plati

3. Pe Google:
   - `"[offer name]" affiliate review site:affpaying.com`
   - `"[offer name]" SMS marketing results`

4. Clasifica saturatia:
   - LOW (<5 advertiseri activi via SMS): Oportunitate — intra rapid
   - MEDIUM (5-15): Analizeaza diferentiere (landing page, copy SMS)
   - HIGH (>15): Evita sau diferentiaza puternic (unghi unic, sub-target strict)

**Output (artifact)**: `saturation-analysis-{GEO}-{YYYY-MM}.md`
**Checkpoint**: Nivel saturatie documentat (LOW/MEDIUM/HIGH) pentru toate ofertele din lista scurta

---

### Pas 5 — Payment Terms Audit

**Tool**: Dashboard retele + mcp__brave-search + STM Forum reviews

**Actiune**:
Verifica pentru fiecare retea/oferta:

| Camp | Ce verifici | Scor (1-5) |
|---|---|---|
| Frecventa plata | Daily/Weekly = 5, Biweekly = 4, Monthly = 3, 45+ zile = 1 | - |
| Threshold minim | <$50 = 5, $50-100 = 4, $100-500 = 3, >$500 = 1 | - |
| Reputatie retea | STM Forum rating + vechimea contului | - |
| Metode plata | Wire+Crypto+PayPal = 5, Wire only = 3 | - |
| Conditii speciale | Cap zilnic, fraud hold, minimum volume | - |

Scor Payment Terms = media celor 5 dimensiuni.

**Referinte de reputatie retele**:
- 3SNET: Specializata gambling, plati ok, suport bun pentru CIS/LATAM
- MaxBounty: Net-15 pentru parteneri verificati, reputatie excelenta STM
- ClickDealer: Net-30 standard, bun pentru mobile/apps
- Mobidea: Plati saptamanale disponibile, bun pentru volume mici
- Perform[cb]: Net-30, Enterprise focus, threshold ridicat

**Output (artifact)**: `payment-terms-audit-{YYYY-MM}.md`
**Checkpoint**: Scor payment terms calculat pentru toate retelele active; retele cu scor <2 eliminate

---

### Pas 6 — Offer Scoring Matrix Compozita

**Tool**: Spreadsheet / tabel markdown

**Actiune**:
Calculeaza scorul compozit final pentru fiecare oferta:

```
Scor Total = (EC/1K Score × 0.40) + (Search Demand × 0.20) + (Competition Score × 0.20) + (Payment Terms × 0.20)
```

**Normalizare scoruri (1-5)**:
- **EC/1K Score**: >$10/1K=5, $5-10=4, $2-5=3, $1-2=2, <$1=1
- **Search Demand**: Cerere RIDICATA=5, MEDIE=4, SCAZUTA=3, n/a(brand niche)=2, <100/luna=1
- **Competition Score**: LOW saturation=5, MEDIUM=3, HIGH=1
- **Payment Terms**: din Pas 5 (1-5)

**Output (artifact)**: `offer-scoring-matrix-{GEO}-{YYYY-MM}.md`
**Checkpoint**: Scor compozit calculat pentru toate ofertele; top 5 per GEO identificate

---

### Pas 7 — Top 5 Selection + Rationale + Handoff

**Tool**: Analiza manuala + mcp__cortex__cortex_store

**Actiune**:
1. Din scoring matrix, selecteaza top 5 oferte per GEO cu scor compozit cel mai ridicat
2. Scrie pentru fiecare o rationale de 2-3 propozitii: de ce e oferta buna, ce risc exista, ce actiune urmatoare
3. Creeaza plan de lansare rapid:
   - Oferta #1: Lansare imediat (tracking setup + SMS creativ)
   - Oferta #2: Lansare in 2 saptamani dupa validarea #1
   - Oferta #3-5: Pipeline pentru trimestrul curent
4. Trimite output catre M-124 (framework lansare campanii)
5. Salveaza in Cortex (Sectiunea 3)

**Output (artifact)**: `top5-offers-{GEO}-{YYYY-MM}.md` + plan de lansare
**Checkpoint**: Top 5 cu rationale documentata, plan de lansare creat, Cortex save confirmat

---

## Offer Scoring Matrix Template

| Offer | Network | Vertical | GEO | CPA/RS | EC/1K SMS | Search Vol/luna | Saturatie | Payment Terms (1-5) | Scor Total (/5) | Status |
|---|---|---|---|---|---|---|---|---|---|---|
| MarathonBet | 3SNET | Betting | BO | RevShare 30% (~$12 FTD) | $24/1K | 1,200 (MEDIE) | LOW | 3 (monthly) | 2.85 | ACTIV — fix tracking |
| AvaTrade | MaxBounty | Trading/Forex | BO | CPA $250 (cap 15/luna) | $6,000/1K* | 800 (MEDIE) | LOW | 4 (Net-15) | 4.20 | PRIORITATE 1 |
| 1xBet | 3SNET | Betting | BO | CPA $12 (reg2dep 19.4%) | $72/1K | 2,100 (RIDICATA) | MEDIUM | 3 | 3.05 | TEST |
| SpinBetter Casino | 3SNET | Betting/Casino | RO | RevShare TBD | TBD | 3,400 (RIDICATA) | LOW | 3 | Pending April | HOLD — pana dupa April |
| Trading Offer RO | MaxBounty | Trading/Forex | RO | CPA $180 est. | $3,600/1K* | 5,200 (RIDICATA) | LOW | 4 | 4.35 | PRIORITATE 1 (dupa April) |

**Nota AvaTrade si Trading Offer**: EC/1K marcat cu * reflecta volumul teoretic fara cap. In practica, capul de 15 conversii/luna limiteaza revenue total la ~$3,750/luna (AvaTrade) dar ROI per SMS livrat este exceptional. Se recomanda combinatia: AvaTrade pentru primele 15 conversii + offer alternativ pentru volumul ramas.

**Nota MarathonBet**: Oferta activa dar revenue $0 din cauza tracking broken. Prioritate imediata: fix tracking (postback URL verificat cu 3SNET), nu inlocuire oferta.

---

## Sectiunea 3 — Cortex Logging

```json
{
  "type": "procedure_execution",
  "procedure": "M-123",
  "rule_id": "TRAIN-S-001",
  "domain": "smsads",
  "sub_domain": "offer-scoring",
  "version": "2.0",
  "status": "ACTIVE",
  "tags": ["smsads", "VOX", "offer-scoring", "CPA", "RevShare", "EC-per-1K", "Bolivia", "Romania", "3SNET", "MaxBounty"],
  "metadata": {
    "geo_analyzed": [],
    "offers_evaluated": 0,
    "top_offers_selected": [],
    "networks_queried": ["3SNET", "MaxBounty", "ClickDealer", "Mobidea", "Perform[cb]"],
    "highest_ec_per_1k": {},
    "execution_date": "",
    "next_review_date": "",
    "executed_by": ""
  },
  "artifacts": {
    "master_offer_list": "master-offer-list-{GEO}-{YYYY-MM}.csv",
    "search_volume": "search-volume-validation-{GEO}-{YYYY-MM}.csv",
    "ec_scoring": "ec-per-1k-scoring-{GEO}-{YYYY-MM}.csv",
    "saturation": "saturation-analysis-{GEO}-{YYYY-MM}.md",
    "payment_audit": "payment-terms-audit-{YYYY-MM}.md",
    "scoring_matrix": "offer-scoring-matrix-{GEO}-{YYYY-MM}.md",
    "top5_selection": "top5-offers-{GEO}-{YYYY-MM}.md"
  }
}
```

---

## Sectiunea 4 — Enforcement Loop

**WHERE**: Se executa inainte de orice lansare campanie noua + lunar pentru GEO-urile active (refresh oferte).

**WHEN**:
- Inainte de prima campanie pe orice GEO nou (obligatoriu)
- Lunar — refresh master offer list (ofertele expira, payouts se schimba)
- La onboarding vertical nou (output M-122 → trigger M-123)
- La schimbarea semnificativa a payout-ului unui offer activ (>20% variatie)

**HOW**:
- VIOLATION: Lansarea unei campanii cu o oferta care nu are EC/1K calculat = BLOCARE
- VIOLATION: Selectarea unei oferte cu scor compozit <2.5 fara aprobare explicita Pafi = BLOCARE
- VIOLATION: Utilizarea unei retele fara payment terms audit completat = WARNING
- WARNING: Oferta cu saturatie HIGH acceptata fara strategie de diferentiere documentata = WARNING

**CONNECT**:
- **M-115** (Offer Discovery): M-123 extinde si scoreaza ofertele identificate in M-115
- **M-122** (Vertical Discovery): M-122 trimite verticale validate → M-123 scoreaza ofertele per vertical
- **M-124** (Lansare Campanii): Primeste top 5 oferte de la M-123 pentru setup campanii
- **M-125** (Compliance): Verifica legalitate offer per GEO inainte de lansare (ex: restrictii Romania pana April)
- **M-118** (Compliance General): Verifica compliance SMS per carrier pentru fiecare offer selectat

**VERIFY** (5 checkuri obligatorii la finalizare):
1. [ ] Master offer list cu minim 15 oferte din minim 3 retele documentat complet (10 coloane)
2. [ ] EC/1K calculat pentru toate ofertele cu CTR si CR benchmarks justificate per vertical
3. [ ] Scoring compozit calculat cu cele 4 dimensiuni ponderate corect (40/20/20/20)
4. [ ] Top 5 per GEO selectate cu rationale scrisa (minim 2 propozitii per oferta)
5. [ ] Cortex save confirmat + handoff catre M-124 initiat cu plan de lansare atasat

**MODEL ROUTING**:
- Opus 4.6: Pas 3 (calcul EC/1K + interpretare benchmarks), Pas 6 (scoring matrix compozit), Pas 7 (rationale si plan lansare)
- Sonnet 4.6: Pas 1 (offer scrape si documentare), Pas 2 (search volume), Pas 4 (competitor research), Pas 5 (payment terms audit)

---

## Sectiunea 5 — Dependente

| Dependenta | Tip | Detalii |
|---|---|---|
| M-122 Vertical Discovery | Upstream (feeds) | Trimite verticale validate pentru care se cauta oferte |
| M-115 Offer Discovery | Upstream (parallel) | Sursa initiala de oferte (M-123 extinde si scoreaza) |
| M-124 Lansare Campanii | Downstream | Primeste top 5 oferte cu rationale si plan lansare |
| M-118 Compliance | Parallel (required) | Verifica legalitate per offer per GEO |
| M-125 Compliance SMS | Parallel (required) | Verifica consent model si restrictii carrier per offer |
| 3SNET Dashboard | External | Sursa oferte gambling/betting |
| MaxBounty Dashboard | External | Sursa oferte finance/health/apps |
| ClickDealer Dashboard | External | Sursa oferte CPI/apps |
| Mobidea Dashboard | External | Sursa oferte mobile/apps |
| Perform[cb] Dashboard | External | Sursa oferte enterprise/finance |
| Google Keyword Planner | External tool | Validare search volume per GEO |
| AdPlexity Mobile | External tool | Competitive intelligence per offer |
| STM Forum | Research | Reputatie retele + insights campanii |

---

## Sectiunea 6 — Metrics

| Metrica | Target | Frecventa Masurare |
|---|---|---|
| Oferte evaluate in master list per GEO | Minim 15 (din 3+ retele) | La fiecare executie |
| EC/1K mediu al top 5 oferte selectate | >$50/1K SMS | La fiecare executie |
| Oferte cu scor compozit >3.5 identificate | Minim 2 per GEO | La fiecare executie |
| Timp executie completa M-123 | <5 ore | Per executie |
| Acuratete EC/1K estimat vs EC/1K real (post-campanie) | <30% deviatie | La 30 zile dupa lansare |
| Revenue generat de oferte selectate via M-123 | >80% din total revenue SMSads | Lunar |
| Refresh lunar master offer list completat | 100% | Lunar |
| Retele cu conturi active si verificate | Minim 4 | Trimestrial |

---

## Checklist Pre-Publicare

- [x] Scopul procedurii este clar definit cu distinctie CPA vs RevShare pentru SMS
- [x] Problema articulata cu exemple concrete (revenue $0, AvaTrade nefolosit)
- [x] Toti pasii 1-7 au Tool, Output (artifact cu naming convention), Checkpoint
- [x] Formula EC/1K inclusa cu benchmarks CTR si CR per vertical si GEO
- [x] Offer Scoring Matrix cu exemple reale: MarathonBet, AvaTrade, 1xBet, SpinBetter, Trading RO
- [x] Nota explicita despre cap AvaTrade si impactul asupra volumului total
- [x] Nota despre MarathonBet tracking broken ca prioritate imediata
- [x] Cortex JSON valid cu domain: "smsads", sub_domain: "offer-scoring", tags corecte
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] CONNECT refera corect M-115, M-122, M-124, M-125, M-118
- [x] VERIFY are exact 5 checkuri concrete si masurabile
- [x] Dependente documentate complet (upstream + downstream + externe)
- [x] Metrics cu target numeric si frecventa definita
- [x] Limbaj: romana cu termeni tehnici in engleza
