---
type: procedure
created: 2026-03-20
status: active
slug: smsads-p2-product-finder
tags: [procedure, nexus]
---

# SMSADS-P2 — Product Finder for SMS Carrier Campaigns

**Domeniu**: affiliate | **Sub-domeniu**: smsads-product-finder
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Scope**: Identificarea si selectia celor mai profitabile produse/oferte per vertical si GEO pentru campaniile SMS VOX, cu calcul EC/1K SMS si scoring compozit din retele multiple (3SNET, MaxBounty, ClickDealer, Mobidea, Perform[cb]).

---

## 1. Problema

### De ce exista aceasta procedura

Fara un proces sistematic de product finding, SMSads alege ofertele pe baza ofertei reactive de la 3SNET — singurul network consultat in mod curent. Aceasta abordare ignora o piata larga de oferte cu EC/1K SMS superior.

**Date concrete care ilustreaza problema**:
- AvaTrade Bolivia ($250 CPA, MaxBounty) e disponibila si testabila dar nefolosita — in timp ce se trimite la MarathonBet ($12 payout estimat per FTD, RevShare 30%)
- EC/1K SMS AvaTrade estimat ($37-75/1K) vs MarathonBet ($24/1K) — 1.5x-3x diferenta de randament pe acelasi trafic SMS
- 1xBet Bolivia (#5598, CPA $12) e in negociere fara evaluare structurata a CR-ului si a competitiei SMS
- Niciodata nu au fost evaluate oferte din MaxBounty, ClickDealer sau Mobidea pentru Bolivia sau Romania

**Confuzii de model de payout specifice SMS**:
- RevShare (ex: MarathonBet 30% brut → 15% net dupa VOX split) pare atractiv dar necesita LTV pozitiv al jucatorului
- CPA fix (ex: AvaTrade $250 → $125 net) da cashflow rapid dar are cap de volum
- Fara EC/1K SMS calculat cu CTR × CR × Payout_net, comparatia intre modele e imposibila

**Cost al inactiunii**:
- Fiecare campanie lansata fara product scoring = risc de EC/1K SMS suboptimal
- Romania cu 20M SMS si 45 categorii IAB se deblocheaza April 2026 — fara SMSADS-P2 completat acum, selectia ofertelor va fi din nou reactiva
- SMSads nu poate scala fara un pipeline de produse validate: in prezent exista un singur offer activ (MarathonBet) cu tracking rupt

### Situatii acoperite
- Selectie oferta initiala pentru un GEO + vertical nou
- Evaluare oferta propusa de AM 3SNET
- Refresh lunar al ofertelor active (payouts se schimba, oferte expira)
- Pre-lansare Romania: building produse pipeline pentru April 2026

---

## 2. Procedura

### Pas 1: Multi-Network Offer Scrape per Vertical + GEO

**Tool**: Dashboard 3SNET + MaxBounty + ClickDealer + Mobidea + Perform[cb] + mcp__brave-search

**Actiune**:
1. Acceseaza fiecare retea si filtreaza dupa vertical + GEO primit din SMSADS-P1:
   - Filtru minim: CPA > $15 SAU RevShare > 20% SAU CPL > $3
   - GEO: BO (Bolivia), RO (Romania), MM (Myanmar)
2. Consulta AM-ul din fiecare retea: "Care sunt top 3 oferte pentru [vertical] in [GEO] luna asta?"
3. Documenteaza fiecare oferta gasita in format standard (10 coloane):

| Offer Name | Network | Vertical | GEO | Payout Model | Payout Value | Conversion Event | Tracking | Min Payout | Conditii Speciale |
|---|---|---|---|---|---|---|---|---|---|
| MarathonBet #5572 | 3SNET | Betting | BO | RevShare | 30% brut | FTD | Postback | $100 | ReconciliereLuna+1 |
| AvaTrade | MaxBounty | Trading | BO | CPA | $250 | Lead verificat | Pixel+postback | $50 | Cap 15 conv/luna |
| 1xBet #5598 | 3SNET | Betting | BO | CPA | $12 | Reg2Dep | Postback | $50 | Reg2dep 19.4% |
| SpinBetter #3642 | 3SNET | Casino | RO | RevShare | TBD | FTD | Postback | $100 | Asteapta April |

4. Consolideaza in `smsads-offers-master-{GEO}-{YYYY-MM}.csv`

**Output (artifact)**: `smsads-offers-master-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: Minim 10 oferte documentate din minim 3 retele diferite, toate 10 coloane completate. FAIL daca toate ofertele vin dintr-o singura retea.

---

### Pas 2: CR Benchmarking per Vertical

**Tool**: Date istorice Binom (Kody) + STM Forum + mcp__brave-search + mcp__tavily__tavily_search

**Actiune**:
1. Extrage din Binom (via Kody) CR-ul actual din campaniile active Bolivia:
   - MarathonBet: clicks → conversii (daca tracking e functional)
   - Orice alt offer activ cu date >1000 clicks
2. Completeaza cu benchmarks din STM Forum + resurse publice pentru ofertele fara date istorice:

**Benchmarks CR (Conversion Rate) per Vertical si GEO**:

| Vertical | Conversion Event | Bolivia CR Est | Romania CR Est | Sursa |
|---|---|---|---|---|
| Betting (registration) | Registration | 0.5-1.5% | 0.8-2.0% | STM Forum + dat, Binom |
| Betting (FTD) | First Deposit | 0.3-0.8% | 0.5-1.2% | STM Forum |
| Casino (FTD) | First Deposit | 0.4-1.0% | 0.6-1.5% | STM Forum |
| Trading/Forex (lead) | Lead/Demo | 0.8-2.5% | 1.0-3.0% | MaxBounty AM intel |
| App Downloads (install) | Install | 3.5-8.0% | 3.0-7.0% | ClickDealer intel |
| Health/Nutra (purchase) | Purchase | 1.0-3.0% | 1.5-4.0% | MaxBounty intel |
| VPN/Antivirus (trial) | Trial signup | 2.0-5.0% | 2.5-6.0% | MaxBounty intel |
| Credite/Loans (lead) | Lead aplicatie | 1.5-4.0% | 2.0-5.0% | STM Forum |

3. Noteaza daca oferta are landing page dedicat SMS sau redirect direct la operator (LP dedicat = CR mai mare)

**Output (artifact)**: `smsads-cr-benchmarks-{vertical}-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: CR benchmark documentat pentru toate verticalele din master offer list. FAIL daca CR e 0 sau lipsa pentru orice vertical cu oferta disponibila.

---

### Pas 3: Calcul EC/1K SMS per Oferta

**Tool**: Calculator / spreadsheet

**Actiune**:
Calculeaza EC/1K SMS (Expected Commission per 1000 SMS) pentru fiecare oferta:

```
EC/1K SMS = CTR% × CR% × Payout_net
Payout_net = Payout_brut × 0.50 (split VOX 50/50)
```

**Benchmarks CTR SMS per Vertical si GEO** (pentru oferte fara date istorice):

| Vertical | Bolivia CTR | Romania CTR |
|---|---|---|
| Betting/Casino | 3-6% | 5-9% |
| App Downloads | 5-9% | 4-7% |
| VPN/Antivirus | 3-5% | 3-6% |
| Trading/Forex | 2-4% | 2-5% |
| Health/Nutra | 2-4% | 3-5% |
| Credite/Loans | 2-4% | 3-6% |

**Calcule obligatorii cu valorile MINIME si MAXIME ale CTR si CR**:
```
EC/1K_min = CTR_min% × CR_min% × Payout_net
EC/1K_max = CTR_max% × CR_max% × Payout_net
EC/1K_central = (EC/1K_min + EC/1K_max) / 2
```

**Exemplu — MarathonBet Bolivia RevShare 30% brut (Payout_net ~15%)**:
- Valoare medie pe FTD estimata: $80 pierdere jucator × 15% = $12/FTD
- EC/1K_min = 3% × 0.3% × $12 = $0.01080 → $10.80/1K SMS
- EC/1K_max = 6% × 0.8% × $12 = $0.05760 → $57.60/1K SMS
- EC/1K_central = **$34.20/1K SMS**

**Exemplu — AvaTrade Bolivia CPA $250 (cap 15 conv/luna)**:
- Payout_net = $125 per conversie
- EC/1K_min = 2% × 0.8% × $125 = $0.0200 → $20.00/1K SMS
- EC/1K_max = 4% × 2.5% × $125 = $0.1250 → $125.00/1K SMS
- EC/1K_central = **$72.50/1K SMS** (dar limitat la $1,875 net/luna total)

**Exemplu — App Downloads Bolivia CPI $1.50**:
- Payout_net = $0.75/install
- EC/1K_min = 5% × 3.5% × $0.75 = $0.001313 → $1.31/1K SMS
- EC/1K_max = 9% × 8.0% × $0.75 = $0.005400 → $5.40/1K SMS
- EC/1K_central = **$3.36/1K SMS** (dar fara cap de volum, scala pe milioane SMS)

**Output (artifact)**: `smsads-ec1k-offers-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: EC/1K (min + max + central) calculat pentru toate ofertele din master list. FAIL daca oricare oferta are EC/1K = 0 sau lipsa.

---

### Pas 4: Validare Saturatie SMS per Oferta

**Tool**: AdPlexity Mobile + STM Forum + mcp__brave-search + intrebare directa AM 3SNET

**Actiune**:
1. **AdPlexity Mobile** (daca abonament disponibil, $149/mo):
   - Filtreaza: GEO = BO/RO + tip trafic = mobile/SMS + keyword = brand/vertical
   - Numara advertiseri activi per oferta
2. **STM Forum** (stmforum.com):
   - Cauta: "[offer name] SMS Bolivia/Romania 2025/2026"
   - Identifica: afiliati care declara volume SMS, probleme de tracking, CR real
3. **Intrebare directa AM 3SNET** (Timur @Aff3snet):
   - "Cati afiliati SMS activi aveti pe MarathonBet Bolivia?"
   - "Primim cap de volum pe AvaTrade? SMS-ul nostru afecteaza CR-ul altora?"
4. **Scorare saturatie**:
   - 0 afiliati SMS confirmati = LOW (oportunitate first-mover)
   - 1-3 afiliati = MEDIUM (intrare posibila cu diferentiere copy)
   - 4+ afiliati = HIGH (reconsiderata sau intrat cu oferta diferita)

**Output (artifact)**: `smsads-saturation-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Nivel saturatie documentat (LOW/MEDIUM/HIGH) pentru top 5 oferte. FAIL daca niciun canal de verificare nu a fost folosit (nu se accepta "assumed LOW").

---

### Pas 5: Audit Termeni de Plata

**Tool**: Dashboard retele + STM Forum reviews + mcp__brave-search

**Actiune**:
Verifica si scoreaza pentru fiecare retea/oferta (1-5 per dimensiune):

| Dimensiune | Scor 1 | Scor 3 | Scor 5 |
|---|---|---|---|
| Frecventa plata | NET60+ zile | NET30 | Weekly / NET15 |
| Prag minim plata | >$500 | $100-500 | <$50 |
| Reputatie retea (STM Forum) | Plangeri frecvente | Neutra | Top-rated, >5 ani activa |
| Metode plata | Wire only | Wire + 1 metoda | Wire + Crypto + PayPal/Wise |
| Conditii speciale | Fraud hold >30 zile | Hold standard | Fara hold sau hold <7 zile |

**Scoruri de referinta retele principale**:
- **3SNET**: plati ok pentru gambling, suport bun LATAM, ~NET30 (reconciliere luna urmatoare per contul actual)
- **MaxBounty**: NET15 pentru parteneri verificati, reputatie excelenta STM, metode multiple
- **ClickDealer**: NET30 standard, bun pentru mobile/apps, stabil
- **Mobidea**: Plati saptamanale disponibile, bun pentru volume mici, threshold mic
- **Perform[cb]**: NET30, focus enterprise, threshold ridicat ($500+)

**Output (artifact)**: `smsads-payment-audit-{YYYY-MM}.md`
**Checkpoint PASS**: Scor payment terms calculat pentru toate retelele active (minim 3). FAIL daca retelele cu scor <2 nu sunt marcate explicit cu motivatie.

---

### Pas 6: Scoring Compozit si Top 5 Selectie

**Tool**: Spreadsheet / tabel markdown

**Actiune**:
Calculeaza scorul compozit final:

```
Scor Compozit = (EC/1K Score × 0.45) + (Saturatie Score × 0.25) + (Payment Terms × 0.20) + (CR Confidence × 0.10)
```

**Normalizare scoruri (1-5)**:
- **EC/1K Score**: >$50/1K=5, $20-50=4, $10-20=3, $5-10=2, <$5=1
- **Saturatie Score**: LOW=5, MEDIUM=3, HIGH=1
- **Payment Terms**: scor calculat in Pas 5 (1-5)
- **CR Confidence**: date reale Binom=5, benchmark STM Forum=3, estimare pura=1

**Matrice output**:

| Rank | Offer | Network | Vertical | GEO | EC/1K Central | Saturatie | Payment(1-5) | CR Conf. | Scor Total | Status |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | AvaTrade | MaxBounty | Trading | BO | $72.50/1K | LOW | 4 | 1 | 4.0 | PRIORITATE 1 (cap 15) |
| 2 | MarathonBet | 3SNET | Betting | BO | $34.20/1K | MEDIUM | 3 | 1 | 3.2 | ACTIV — fix tracking |
| 3 | 1xBet #5598 | 3SNET | Betting | BO | $25.00/1K | MEDIUM | 3 | 1 | 2.9 | TEST dupa tracking fix |
| 4 | [App CPI BO] | ClickDealer | App Downloads | BO | $3.36/1K | LOW | 3 | 1 | 2.3 | PILOT (volume nelimitat) |
| 5 | SpinBetter #3642 | 3SNET | Casino | RO | TBD | LOW | 3 | 1 | Pending April | HOLD |

**Output (artifact)**: `smsads-offer-scoring-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Top 5 per GEO selectate cu scor compozit calculat si rationale scrisa (minim 1 propozitie per oferta). FAIL daca scorurile lipsesc pentru orice dimensiune.

---

### Pas 7: Plan de Lansare + Handoff

**Tool**: mcp__cortex__cortex_store

**Actiune**:
1. Din top 5, defineste planul de lansare:
   - Oferta #1 (scor maxim): Lansare imediata — tracking setup Binom (Kody) + SMS creativ (Pas 3 M-125) in max 7 zile
   - Oferta #2: Lansare in 2 saptamani dupa validarea #1 (minim 5K SMS livrate, CR masurat)
   - Oferta #3-5: Pipeline trimestrul curent
2. Initializeaza SMSADS-P3 (Competitor Intel) pentru ofertele Prioritate 1
3. Initializeaza M-125 (VOX Taxonomy Segmentation) pentru segmentarea IAB a campaniei
4. Salveaza in Cortex (Sectiunea 3)

**Output (artifact)**: `smsads-launch-plan-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Plan de lansare cu deadlines concrete per oferta. Cortex save confirmat. FAIL daca Kody nu e notificat pentru tracking setup al ofertei #1.

---

## 3. Cortex Logging

```json
{
  "text": "SMSADS-P2 executat: product finder {vertical} {GEO} {YYYY-MM}",
  "collection": "business_clickwin",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SMSADS-P2-PRODUCT-FINDER",
    "rule_id": "TRAIN-S-001",
    "domain": "smsads",
    "sub_domain": "product-finder",
    "version": "1.0",
    "tags": ["smsads", "VOX", "product-finder", "EC-per-1K", "CPA", "RevShare", "Bolivia", "Romania", "3SNET", "MaxBounty"],
    "geo_analyzed": [],
    "vertical_analyzed": "",
    "offers_evaluated": 0,
    "top_offer_selected": "",
    "top_offer_ec1k": "",
    "networks_queried": ["3SNET", "MaxBounty", "ClickDealer", "Mobidea", "Perform[cb]"],
    "execution_date": "",
    "executed_by": "",
    "artifacts": {
      "offers_master": "smsads-offers-master-{GEO}-{YYYY-MM}.csv",
      "cr_benchmarks": "smsads-cr-benchmarks-{vertical}-{GEO}-{YYYY-MM}.md",
      "ec1k_offers": "smsads-ec1k-offers-{GEO}-{YYYY-MM}.csv",
      "saturation": "smsads-saturation-{GEO}-{YYYY-MM}.md",
      "payment_audit": "smsads-payment-audit-{YYYY-MM}.md",
      "offer_scoring": "smsads-offer-scoring-{GEO}-{YYYY-MM}.md",
      "launch_plan": "smsads-launch-plan-{GEO}-{YYYY-MM}.md"
    }
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Inainte de orice campanie SMS noua pe orice vertical + GEO
- Lunar: refresh master offer list (ofertele expira, payouts se schimba)
- La output SMSADS-P1: top 2-3 verticale selectate → trigger SMSADS-P2 imediat
- Pre-lansare Romania: executie OBLIGATORIE cu minim 3 saptamani inainte de April 2026

### WHEN
- Trigger P1 output: executie in max 48h de la finalizarea SMSADS-P1
- Trigger lunar: prima zi lucratoare din luna
- Trigger oferta noua 3SNET: evaluare in max 24h de la notificare AM

### HOW (violation detection)
- VIOLATION: Campanie lansata fara EC/1K calculat pentru oferta respectiva = BLOCARE
- VIOLATION: Oferta selectata cu scor compozit <2.0 fara aprobare scrisa Pafi = BLOCARE
- VIOLATION: Master offer list neactualizata >30 zile pentru GEO activ = WARNING
- WARNING: Oferta cu saturatie HIGH acceptata fara strategie diferentiere documentata = WARNING
- Runner: verificare la session start Genie + notificare la 30 zile de la ultima rulare

### CONNECT
- **SMSADS-P1** (Vertical Discovery) — upstream; trimite verticale validate → SMSADS-P2 scoreaza ofertele per vertical
- **SMSADS-P3** (Competitor Intel) — paralel; ofertele prioritizate in P2 sunt analizate competitiv in P3
- **SMSADS-P4** (Network Discovery) — paralel; P4 descopera retele noi care pot furniza oferte pentru P2
- **M-125** (VOX Taxonomy Segmentation) — downstream; primeste oferta #1 pentru segmentare IAB
- **M-126** (Competitor SEO Intel) — furnizeaza oferte identificate la competitori → pot intra in P2 master list
- `procedure-health.json` → adauga entry: `SMSADS-P2-PRODUCT-FINDER`

### VERIFY
- [ ] Procedura executata complet? (toti pasii 1-7 cu artifacts documentate)
- [ ] Output satisface criteriile? (top 5 oferte cu EC/1K calculat si scor compozit)
- [ ] VK emis in sesiune? (ambele linii vizibile pentru Pafi)
- [ ] Daca oricare = NU → procedura NU e completa

**Doua VK-uri obligatorii (VK-H-001)**:
1. `[PROC] SMSADS-P2 | §1 §2 §3 §4 | {GEO}/{vertical} | top offer: {offer} EC/1K: ${X} | complete`
2. `[CORTEX] "SMSADS-P2: product finder {vertical} {GEO}" | FORGE | rule: TRAIN-S-001 | v1.0`

### MODEL ROUTING

| Activitate | Model | Motivul |
|---|---|---|
| Pas 1: Multi-network offer scrape | Sonnet | Agregare date tabelara, task repetitiv |
| Pas 2: CR benchmarking | Sonnet | Structurare date existente, benchmark lookup |
| Pas 3: Calcul EC/1K SMS | Opus | Modelare economica cu variabilitate + interpretare SMSads context |
| Pas 4: Validare saturatie | Sonnet | Agregare date externe, fara judecata complexa |
| Pas 5: Audit termeni plata | Sonnet | Structurare date, calcul scor mecanic |
| Pas 6: Scoring compozit + selectie | Opus | Sinteza multi-dimensionala + rationament prioritizare |
| Pas 7: Plan lansare + handoff | Sonnet | Structurare plan, task mecanic de save |

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|---|---|---|
| 3SNET Dashboard + AM Timur | Oferte betting/gambling | go.3snet.co / @Aff3snet Telegram |
| MaxBounty Dashboard | Oferte finance, health, apps | maxbounty.com |
| ClickDealer Dashboard | Oferte CPI/apps/mobile | clickdealer.com |
| Mobidea Dashboard | Oferte mobile/carrier billing | mobidea.com |
| Perform[cb] Dashboard | Oferte enterprise/finance | performcb.com |
| Binom Tracker | Date CR reale din campaniile active | Kody acces |
| SMSADS-P1 | Upstream: furnizeaza verticalele selectate | `SMSADS-P1-vertical-discovery.md` |
| M-125 | Downstream: segmentare IAB pentru oferta #1 | `M-125-vox-taxonomy-segmentation.md` |
| AdPlexity Mobile | Validare saturatie (optional) | adplexity.com/mobile |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---|---|---|
| Oferte evaluate per GEO per luna | Acoperire pipeline | Minim 10 din 3+ retele |
| EC/1K SMS mediu top 5 oferte | Calitate selectie | >$20/1K SMS |
| Acuratete EC/1K estimat vs real | Calibrare benchmarks | <30% deviatie la 30 zile post-lansare |
| Refresh lunar master offer list | Actualizare pipeline | 100% completat lunar |
| Timp executie completa SMSADS-P2 | Eficienta procedurii | <4 ore per GEO + vertical |

---

## Checklist Pre-Publicare

- [x] Regula asociata: TRAIN-S-001
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint cu 4 checks obligatorii
- [x] Doi VK-uri obligatorii per VK-H-001 specificati
- [x] MODEL ROUTING prezent cu justificare per pas
- [x] Formule EC/1K cu min/max/central + exemple numerice concrete (AvaTrade, MarathonBet, App Downloads)
- [x] Checkpoints PASS/FAIL per pas
- [x] Conexiuni la SMSADS-P1, P3, P4, M-125, M-126 documentate
- [x] Split VOX 50/50 reflectat in calcule (Payout_net = Payout_brut × 0.50)
- [x] Limbaj romana cu termeni tehnici in engleza
