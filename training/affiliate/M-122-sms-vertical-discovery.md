---
type: procedure
created: 2026-03-17
status: active
slug: m-122-sms-vertical-discovery
tags: [procedure, nexus]
---

# M-122 — SMS Vertical Discovery & Diversification

**Status**: ACTIVE
**Versiune**: 2.0
**Regula asociata**: TRAIN-S-001
**Domeniu**: SMSads / VOX Carrier Marketing
**Ultima actualizare**: 2026-03-05

**Scope**: Procedura sistematica pentru descoperirea si prioritizarea verticalelor noi (dincolo de betting/gambling) care pot fi monetizate prin livrare SMS carrier via VOX. Acoperă audituri IAB per GEO, scoring de compatibilitate SMS, discovery de retele de afiliere per vertical si constructia unei matrice de prioritizare actionale pentru echipa Pafi + Leo + Kody.

---

## Sectiunea 1 — Problema

### De ce este necesara aceasta procedura

**Risc concentrare verticala**: SMSads opereaza in prezent exclusiv in vertical betting/gambling. Dependenta de un singur vertical expune business-ul la:
- Schimbari de politica ale operatorilor de retea (VOX poate restrânge gambling oricand)
- Blocari temporare sau permanente per GEO (ex: Romania blocat pana in April)
- Fluctuatii de oferte pe 3SNET (oferte expirate, schimbari de payout)
- Zero venituri daca tracking-ul se strica (situatia actuala Bolivia)

**Oportunitati ratate**: VOX Bolivia are 693K contacte in Technology, 523K in Software, 116K in Business&Finance — audiențe perfect compatibile cu Finance/Trading, App Downloads si Fintech, verticale cu CPA ridicat ($50-$250) si conversii mai bune decat gambling in unele GEO.

**Ce se întâmpla fara aceasta procedura**:
- Verticale noi se lanseaza pe baza de instinct, nu analiza
- Oferte incompatibile cu SMS carrier delivery sunt testate (risipa buget)
- Retele relevante (MaxBounty, ClickDealer) nu sunt consultate sistematic
- Audienta VOX IAB disponibila ramane subutilizata

---

## Sectiunea 2 — Procedura

### Pas 1 — Audit Categorii VOX per GEO si Mapare la Verticale de Oferte

**Tool**: Fisier intern VOX + manual mapping

**Actiune**:
1. Extrage lista completa de categorii IAB disponibile din dashboard VOX pentru fiecare GEO activ (Bolivia, Romania, Myanmar planned)
2. Mapeaza fiecare categorie IAB la verticale de oferte afiliere compatibile:

| Categorie IAB VOX | Vertical Oferta Afiliere | Exemple Produse |
|---|---|---|
| Business&Finance | Trading/Forex, Credite, Crypto | AvaTrade, XTB, Binance |
| Technology | App Downloads, SaaS, Antivirus | Google Play CPI, Norton |
| Software | App Downloads, VPN, Tools | ExpressVPN, utility apps |
| Health | Nutraceutice, Pharma, Fitness | Keto suplimente, CBD (legal) |
| Travel | Hotel Booking, Asigurari Calatorie | Booking.com, SafetyWing |
| Arts&Entertainment | Streaming, Gaming (non-gambling) | Spotify, casual games |
| Web Services | Hosting, Domain, Digital Tools | Hostinger, Namecheap |
| Business&Industrial | B2B SaaS, HR Tools, Logistics | CRM, ERP offers |

3. Noteaza dimensiunea audientei pentru fiecare categorie per GEO:
   - Bolivia: Technology 693K, Software 523K, Apps 457K, Business&Finance 116K
   - Romania: Business&Finance 2.1M, Finance&Insurance 1.7M, Health 1.2M, Software 1.0M

**Output (artifact)**: `vertical-iac-mapping-{GEO}-{YYYY-MM}.md`
**Checkpoint**: Minim 3 verticale candidate identificate per GEO cu audienta >100K

---

### Pas 2 — Scoring Compatibilitate Vertical-SMS

**Tool**: Scoring manual + STM Forum research + adplexity.com/mobile

**Actiune**:
Evalueaza fiecare vertical candidat pe 4 dimensiuni, scor 1-5 fiecare:

| Dimensiune | Scor 1 | Scor 5 |
|---|---|---|
| **Risc Regulatory Carrier** | Interzis de carrier (adult, arme) | Permis fara restrictii (apps, travel) |
| **Lungime Path Conversie** | >5 pasi SMS→convert | 1-2 pasi (click→install / click→lead) |
| **Consent Carrier** | Opt-in strict obligatoriu | Light consent / nu e necesar |
| **Payout/Effort Ratio** | <$5 CPA, conversie dificila | >$50 CPA sau RevShare recurent |

Criterii de eliminare imediata (scor 1 la regulatory = DISCARD):
- Adult/Erotic: blocat de toti carrierii VOX
- Arme, tutun: reglementat strict
- Pharma cu reteta: risc legal ridicat

**Output (artifact)**: `vertical-sms-compat-scores-{GEO}-{YYYY-MM}.csv`
**Checkpoint**: Scor compat compozit calculat pentru minim 5 verticale

---

### Pas 3 — Discovery Retele Afiliere per Vertical

**Tool**: mcp__brave-search / mcp__tavily__tavily_search + listing manual retele

**Actiune**:
Mapeaza retele la verticale si verifica disponibilitate oferte in GEO-ul tinta:

| Vertical | Retea Primara | Retea Secundara | GEO Coverage |
|---|---|---|---|
| Gambling/Betting | 3SNET | Bet-at-home Direct | Global (excl. restrictii) |
| Trading/Forex | MaxBounty | ClickDealer, Admitad | Global |
| Health/Nutra | MaxBounty | ClickDealer, CrakRevenue | US, EU, LATAM |
| App Downloads (CPI) | ClickDealer | Mobidea, Perform[cb] | Global |
| Consumer Finance/Loans | MaxBounty | Perform[cb], LeadsPedia | LATAM, EU |
| Crypto/Exchange | Admitad | MaxBounty | Global |
| VPN/Antivirus | MaxBounty | ClickDealer | Global |

Pasi:
1. Creeaza cont pe MaxBounty daca nu exista (`maxbounty.com`)
2. Creeaza cont pe ClickDealer daca nu exista (`clickdealer.com`)
3. Verifica in fiecare retea: oferte disponibile in Bolivia + Romania, filtrare dupa vertical
4. Noteaza: offer name, payout, payout model (CPA/RevShare/CPL), GEO, tracking URL disponibil

**Output (artifact)**: `network-offers-raw-{GEO}-{YYYY-MM}.csv`
**Checkpoint**: Minim 2 oferte concrete gasite pentru fiecare vertical candidat

---

### Pas 4 — Validare Demand Vertical per GEO

**Tool**: Google Keyword Planner + mcp__brave-search + mcp__tavily__tavily_search

**Actiune**:
1. Pentru fiecare vertical candidat, defineste 3-5 keywords principale in limba locala:
   - Bolivia (Espanol): ex. "prestamos rapidos Bolivia", "trading forex Bolivia", "descargar aplicacion"
   - Romania (Romana): ex. "credite rapide", "trading forex Romania", "aplicatii mobile"

2. Verifica search volume lunar via Google Keyword Planner (seteaza GEO exact)
3. Criteriu de validare: >500 cautari/luna = cerere existenta, >2000 = cerere semnificativa

4. Verifica si tendinta (crescatoare / stabila / descrescatoare) via Google Trends

5. Verifica disponibilitate oferte 3SNET pentru acel vertical in acel GEO (confirma pas 3)

**Output (artifact)**: `demand-validation-{vertical}-{GEO}-{YYYY-MM}.md`
**Checkpoint**: Volum de cautare documentat pentru top 3 verticale per GEO

---

### Pas 5 — Competitive Research SMS pentru Vertical

**Tool**: AdPlexity Mobile (adplexity.com/mobile) + STM Forum + mcp__brave-search

**Actiune**:
1. Cauta pe AdPlexity Mobile: filtreaza dupa GEO + vertical + canal SMS/push
   - Identifica: cati advertiseri activi, landinguri, oferte promovate
   - Noteaza: nivel saturaie (Low <5 advertiseri / Medium 5-15 / High >15)

2. Cauta pe STM Forum (`stmforum.com`) thread-uri despre vertical + GEO:
   - "SMS marketing Bolivia finance"
   - "carrier SMS [vertical] [GEO] 2025/2026"
   - Noteaza insights: ce functioneaza, ce nu, limitari carrier

3. Cauta pe Google: "SMS affiliate marketing [vertical] Bolivia/Romania site:affiliatefix.com OR site:affpaying.com"

4. Evaluare saturatie: Low = oportunitate, Medium = analizeza diferentiere, High = evita sau diferentiaza puternic

**Output (artifact)**: `competitive-sms-{vertical}-{GEO}-{YYYY-MM}.md`
**Checkpoint**: Nivel saturatie documentat pentru fiecare vertical analizat

---

### Pas 6 — Matrice de Prioritizare — Top 5 Verticale per GEO

**Tool**: Spreadsheet / tabel markdown

**Actiune**:
Construieste matricea de scoring compozit (detalii in sectiunea Matrice de mai jos).

Scor final = (Audienta IAB × 0.25) + (SMS Compat × 0.25) + (Offer Availability × 0.20) + (Demand Volume × 0.15) + (Regulatory Risk Invers × 0.15)

Selecteaza top 5 verticale per GEO pentru testare, cu recomandare de prioritate:
- **Prioritate 1**: Lansare imediata (test pilot cu buget mic)
- **Prioritate 2**: Lansare dupa validarea Prioritatii 1
- **Prioritate 3-5**: Backlog / research continuu

**Output (artifact)**: `vertical-priority-matrix-{GEO}-{YYYY-MM}.md`
**Checkpoint**: Top 5 verticale rankate cu scor compozit calculat si status recomandat

---

### Pas 7 — Cortex Save si Handoff catre M-123

**Tool**: mcp__cortex__cortex_store

**Actiune**:
1. Salveaza rezultatele in Cortex (vezi Sectiunea 3)
2. Trimite top 5 verticale catre M-123 (Offer Commission Scoring) pentru scoring detaliat
3. Creeaza task in tracker pentru lansare pilot vertical Prioritate 1

**Output (artifact)**: Cortex entry confirmat + task creat
**Checkpoint**: Cortex save confirmat, handoff M-123 initiat

---

## Vertical Scoring Matrix Template

| Vertical | Categorie IAB VOX | Audienta (1-5) | SMS Compat (1-5) | Disponibilitate Oferte (1-5) | Demand Volume (1-5) | Risc Regulatory (1-5 inv) | TOTAL (/25) | Status |
|---|---|---|---|---|---|---|---|---|
| Trading/Forex | Business&Finance | 3 (116K Bolivia) | 4 | 4 (AvaTrade $250 CPA) | 2 (niche B2B) | 3 (disclaimer necesar) | 16 | TEST PILOT |
| Health/Nutra | Health | 4 (1.2M Romania) | 3 | 4 (MaxBounty) | 3 (demand mediu) | 3 (evita claims medicale) | 17 | BACKLOG |
| App Downloads | Technology/Apps | 5 (1.15M Bolivia) | 5 | 5 (ClickDealer CPI) | 4 (demand ridicat) | 5 (fara restrictii) | 24 | PRIORITATE 1 |
| Credite/Loans | Finance&Insurance | 5 (1.7M Romania) | 4 | 3 (MaxBounty, Perform[cb]) | 3 (demand mediu) | 2 (GDPR strict Romania) | 17 | AFTER April |
| VPN/Antivirus | Technology/Software | 5 (1.2M Bolivia) | 5 | 5 (MaxBounty) | 4 (demand ridicat) | 5 (fara restrictii) | 24 | PRIORITATE 1 |

**Nota interpretare**:
- Total 20-25: Lansare imediata recomandata
- Total 14-19: Test pilot dupa validare suplimentara
- Total <14: Backlog sau excludere

---

## Sectiunea 3 — Cortex Logging

```json
{
  "type": "procedure_execution",
  "procedure": "M-122",
  "rule_id": "TRAIN-S-001",
  "domain": "smsads",
  "sub_domain": "vertical-discovery",
  "version": "2.0",
  "status": "ACTIVE",
  "tags": ["smsads", "VOX", "vertical-discovery", "SMS", "IAB", "Bolivia", "Romania"],
  "metadata": {
    "geos_analyzed": [],
    "verticals_identified": [],
    "top_vertical_per_geo": {},
    "networks_checked": ["3SNET", "MaxBounty", "ClickDealer", "Mobidea", "Perform[cb]"],
    "execution_date": "",
    "next_review_date": "",
    "executed_by": ""
  },
  "artifacts": {
    "iac_mapping": "vertical-iac-mapping-{GEO}-{YYYY-MM}.md",
    "compat_scores": "vertical-sms-compat-scores-{GEO}-{YYYY-MM}.csv",
    "network_offers": "network-offers-raw-{GEO}-{YYYY-MM}.csv",
    "demand_validation": "demand-validation-{vertical}-{GEO}-{YYYY-MM}.md",
    "competitive_research": "competitive-sms-{vertical}-{GEO}-{YYYY-MM}.md",
    "priority_matrix": "vertical-priority-matrix-{GEO}-{YYYY-MM}.md"
  }
}
```

---

## Sectiunea 4 — Enforcement Loop

**WHERE**: Se executa la adaugarea oricarui GEO nou SAU la review trimestrial al verticalelor active.

**WHEN**:
- Trimestrial (Q1, Q2, Q3, Q4) pentru GEO-urile active
- La onboarding orice GEO nou (obligatoriu inainte de prima campanie)
- La semnalarea de catre echipa a unui potential vertical nou

**HOW**:
- VIOLATION: Lansarea unei campanii intr-un vertical nou fara sa fi fost executata M-122 pentru acel GEO = BLOCARE
- VIOLATION: Alocarea de buget unui vertical fara scor compozit calculat = BLOCARE
- WARNING: Vertical cu scor compozit <14 in matrice acceptat fara aprobare explicita Pafi = WARNING

**CONNECT**:
- **M-114** (GEO Scoring): M-122 se bazeaza pe datele de GEO scoring pentru a determina care GEO-uri sunt active
- **M-115** (Offer Discovery): M-122 alimenteaza M-115 cu verticalele validate pentru discovery oferte detaliate
- **M-123** (Offer Commission Scoring): M-122 trimite top verticale catre M-123 pentru scoring comision
- **M-124**: Framework de lansare campanii — primeste output M-122+M-123
- **M-125**: Compliance per GEO — trebuie consultat pentru fiecare vertical nou (consent model, restrictii)

**VERIFY** (5 checkuri obligatorii la finalizare):
1. [ ] Toti pasii 1-7 completati cu artifacts documentate
2. [ ] Minim 3 verticale candidate per GEO activ analizate
3. [ ] Scoring compozit calculat pentru fiecare vertical (nu estimat)
4. [ ] Top 5 matrice completata cu status (PRIORITATE 1/2/BACKLOG)
5. [ ] Cortex save confirmat + handoff M-123 initiat pentru Prioritate 1

**MODEL ROUTING**:
- Opus 4.6: Pas 2 (scoring compatibilitate), Pas 6 (matrice prioritizare, analiza risc)
- Sonnet 4.6: Pas 1 (audit IAB mapping), Pas 3 (discovery retele), Pas 4 (demand validation), Pas 5 (competitive research)

---

## Sectiunea 5 — Dependente

| Dependenta | Tip | Detalii |
|---|---|---|
| M-114 GEO Scoring | Upstream (required) | Determina GEO-urile active pe care se aplica M-122 |
| M-115 Offer Discovery | Downstream (feeds) | Primeste lista de verticale validate |
| M-123 Offer Commission Scoring | Downstream (feeds) | Primeste top 5 verticale per GEO |
| M-118 Compliance | Parallel (required) | Verifica legalitate per vertical per GEO |
| M-125 Compliance per GEO | Parallel (required) | Consent model per vertical (Bolivia vs Romania) |
| VOX Dashboard | External tool | Sursa de date IAB per GEO |
| 3SNET Account | External | Verificare oferte gambling |
| MaxBounty Account | External | Verificare oferte finance/health/apps |
| ClickDealer Account | External | Verificare oferte CPI/apps |
| AdPlexity Mobile | External tool | Competitive intelligence SMS |

---

## Sectiunea 6 — Metrics

| Metrica | Target | Frecventa Masurare |
|---|---|---|
| Verticale noi identificate per GEO per trimestru | Minim 3 | Trimestrial |
| Verticale cu scor compat >15 per GEO | Minim 2 | La fiecare executie M-122 |
| Timp mediu executie completa M-122 | <4 ore | Per executie |
| Verticale Prioritate 1 lansate in 30 zile | 100% din identificate | Lunar |
| Revenue din verticale non-gambling (target 6 luni) | >20% din total revenue | Lunar |
| Acuratete scor compozit vs performanta reala | Revisit la 60 zile dupa lansare | Bimensual |
| Retele de afiliere active (conturi create + verificate) | Minim 4 retele | La prima executie, apoi anual |

---

## Checklist Pre-Publicare

- [x] Scopul procedurii este clar definit (2-3 propozitii)
- [x] Problema este articulata cu risc concret (pierdere revenue, risc concentrare)
- [x] Toti pasii au Tool, Output (artifact cu naming convention), Checkpoint
- [x] Matrice de scoring inclusa cu exemple reale (Bolivia + Romania)
- [x] Cortex JSON valid cu domain/sub_domain/tags corecte
- [x] Enforcement Loop complet (WHERE/WHEN/HOW/CONNECT/VERIFY/MODEL ROUTING)
- [x] CONNECT refera proceduri existente si viitoare corect (M-114, M-115, M-123, M-124, M-125)
- [x] VERIFY are exact 5 checkuri concrete si masurabile
- [x] Dependente documentate complet (upstream + downstream + externe)
- [x] Metrics cu target numeric si frecventa definita
- [x] Fara sectiuni vide sau placeholder-uri necompletate
- [x] Limbaj: romana cu termeni tehnici in engleza (CPA, RevShare, IAB, CTR)
