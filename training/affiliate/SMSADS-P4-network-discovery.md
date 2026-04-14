---
type: procedure
created: 2026-03-20
status: active
slug: smsads-p4-network-discovery
tags: [procedure, nexus]
---

# SMSADS-P4 — Network Discovery for SMS Carrier Marketing

**Domeniu**: affiliate | **Sub-domeniu**: smsads-network-discovery
**Versiune**: 2.0 | **Data**: 2026-03-20
**Autor**: Genie Slim Core
**Status**: ACTIVE
**Regula asociata**: TRAIN-S-001
**Forge Score**: estimated H/3.5
**Scope**: Descoperirea si rankingul retelelor de afiliere disponibile per GEO, cu evaluarea acoperirii verticalelor, reputatiei (STM Forum rating + vechime), tipurilor de comision (CPA/RevShare/Hybrid), termenelor de plata si disponibilitatii unui account manager direct — pentru a construi un portofoliu de retele alternative la 3SNET pe fiecare piata VOX activa.

---

## 1. Problema

### De ce exista aceasta procedura

SMSads depinde exclusiv de 3SNET pentru toate ofertele. Aceasta dependenta unica creeaza vulnerabilitati masive si blocheaza revenue.

**Exemple concrete de impact negativ al dependentei de un singur network**:
- Bolivia: 3SNET are MaxBounty (AvaTrade $250 CPA) ca alternativa superioara EC/1K — dar contul MaxBounty nu e creat, oferta nu e aplicata, revenue $0 din aceasta sursa
- Romania: 3SNET nu a putut aproba casino rapid fara stats din alte GEO → blocaj total pana April. Daca aveam Mobidea sau ClickDealer activ, puteam lansa cu alte oferte pe Romania deja
- Myanmar: 3SNET confirma accesul dar lista lor de oferte BO/MM e limitata — Mobidea si Toro Advertising au oferte WAP/carrier pentru SEA care nu apar pe 3SNET
- Negociere payout: fara alternative demonstrate, 3SNET nu are presiune sa imbunatateasca termenii

**Riscuri de concentrare**:
- 3SNET sufera o intrerupere tehnica sau isi modifica TOS → SMSads ramane fara oferte active
- AM Timur pleaca sau contul nostru e suspendat → zero oferte, zero revenue
- Oferta favorita (MarathonBet) e dezlistata de 3SNET → nu avem backup pre-aprobat

**Cost al inactiunii**:
- Fiecare luna fara conturi active pe MaxBounty + ClickDealer + Mobidea = oportunitate ratata
- AvaTrade $250 CPA (MaxBounty) = EC/1K SMS 3-5x superior fata de MarathonBet (3SNET) — pierdere cuantificabila per 1000 SMS
- Perform[cb] are oferte finance pentru Myanmar/SEA neaccesibile prin 3SNET

**WHY NOW**: Bolivia Test 2 (300K SMS) in pregatire. Romania se deblocheaza April. Myanmar planificat. Fiecare GEO nou necesita evaluarea retelelor disponibile inainte de prima campanie — nu dupa.

### Situatii acoperite
- Evaluarea retelelor disponibile la activarea unui GEO VOX nou
- Diversificarea portofoliului de retele pentru GEO-urile active (Bolivia, Romania)
- Negocierea termenilor cu 3SNET pe baza alternativelor evaluate
- Discovery retele specializate per vertical (ex: Mobidea pentru WAP billing SEA)

---

## 2. Procedura

### Pas 1: Mapping Retele per GEO si Vertical

**Tool**: mcp__brave-search + mcp__tavily__tavily_search + Business of Apps network catalog

**Actiune**:
Construieste harta completa a retelelor disponibile per GEO:

1. Cauta pe Business of Apps (businessofapps.com/affiliate/networks/):
   - Filtreaza: GEO = Bolivia / Romania / Myanmar / global
   - Filtreaza: vertical = betting / trading / health / apps / mobile
   - Exporta top 20 retele per vertical

2. Cauta pe STM Forum: "best affiliate network Bolivia", "Romania CPA network", "Myanmar WAP billing"

3. Cauta pe mcp__brave-search:
   ```
   "affiliate network" Bolivia CPA RevShare 2025
   "CPA network" Romania betting casino 2025
   "WAP billing network" Myanmar SEA 2025
   "mobile affiliate network" LATAM CPA
   ```

**Mapping obligatoriu per retea identificata**:

| Network | Specializare | GEO Coverage (relevant) | Verticals Acoperite | Payout Model | Reputatie (STM) | Notita |
|---|---|---|---|---|---|---|
| **3SNET** | Gambling, CIS/LATAM | Bolivia, Romania, Myanmar Q1 | Betting, Casino | CPA + RevShare | Buna, specializata | Cont activ #30063 |
| **MaxBounty** | CPA general, finance | Global inclus Bolivia, Romania | Finance, Health, Apps, Crypto | CPA, CPL | Excelenta (top STM) | Cont de creat |
| **ClickDealer** | Mobile, CPI, nutra | Global, mobile focus | App Downloads, Nutra, Finance | CPA, CPI | Buna | Cont de creat |
| **Mobidea** | Mobile, WAP billing, carrier | Global Tier 2/3, SEA, MENA | WAP Content, Mobile, Dating | CPA, CPL | Buna, mobile specialist | Cont de creat |
| **Perform[cb]** | Enterprise, finance | Global, US/EU focus | Finance, Insurance, B2B | CPA, CPL | Excelenta | Threshold ridicat ($500+) |
| **Toro Advertising** | Mobile Tier 2/3 | LATAM, Africa, SEA | Mobile, WAP, Carrier billing | CPA | Buna | Relevanta Myanmar/SEA |
| **AdCombo** | Nutra, COD | LATAM, Africa, EU | Health, E-commerce | CPA (COD) | Buna | Bolivia nutra potential |

**Output (artifact)**: `smsads-network-map-{GEO}-{YYYY-MM}.md`
**Checkpoint PASS**: Minim 6 retele documentate cu toate coloanele completate. FAIL daca retelele au coloane goale pentru GEO Coverage sau Payout Model.

---

### Pas 2: Scoring Retele per GEO + Vertical

**Tool**: Scoring manual pe 5 dimensiuni (1-5 per dimensiune)

**Actiune**:
Evalueaza fiecare retea pe 5 dimensiuni:

| Dimensiune | Scor 1 | Scor 3 | Scor 5 |
|---|---|---|---|
| **Oferte disponibile in GEO** | Nicio oferta in GEO tinta | 1-2 oferte relevante | 5+ oferte relevante cu CPA >$15 |
| **Verticals acoperite** | Un singur vertical | 2-3 verticale | 4+ verticale (diversificare) |
| **Reputatie STM Forum** | Plangeri frecvente, shaving confirmat | Neutra, putine reviews | Top-rated, >5 ani, reviews pozitive |
| **Termeni plata** | NET60+ / prag >$500 / wire only | NET30 / prag $100-500 | NET15 sau weekly / prag <$100 / metode multiple |
| **AM Direct Disponibil** | Niciun AM alocat, email generic | AM de contactat dar fara raspuns rapid | AM dedicat confirmat, raspuns <24h |

**Formula scor compozit**:
```
Scor Retea = (Oferte GEO × 0.30) + (Verticals × 0.20) + (Reputatie × 0.25) + (Termeni plata × 0.15) + (AM Disponibil × 0.10)
```

**Exemple calcule**:

**MaxBounty pentru Bolivia**:
- Oferte GEO: 5 (AvaTrade $250 CPA, 10+ finance offers) × 0.30 = 1.50
- Verticals: 5 (finance, health, apps, crypto, e-comm) × 0.20 = 1.00
- Reputatie: 5 (top STM Forum, #1 rated annual) × 0.25 = 1.25
- Termeni plata: 5 (NET15 verified partners, $50 threshold, wire+check+ACH) × 0.15 = 0.75
- AM Disponibil: 4 (AM alocat dupa aplicare, nu dedicat inainte) × 0.10 = 0.40
- **Scor: 4.90/5.00 — PRIORITATE 1**

**3SNET pentru Bolivia (referinta)**:
- Oferte GEO: 4 (MarathonBet, 1xBet, alte gambling) × 0.30 = 1.20
- Verticals: 2 (gambling exclusiv) × 0.20 = 0.40
- Reputatie: 4 (buna, specializata gambling) × 0.25 = 1.00
- Termeni plata: 3 (NET30, reconciliere luna urmatoare) × 0.15 = 0.45
- AM Disponibil: 5 (Timur @Aff3snet, raspuns rapid) × 0.10 = 0.50
- **Scor: 3.55/5.00 — ACTIV (cont existent)**

**Output (artifact)**: `smsads-network-scores-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: Scor calculat pentru minim 5 retele, toate 5 dimensiuni prezente. FAIL daca oricare dimensiune e estimata fara sursa de date (toate scorurile necesita justificare scrisa).

---

### Pas 3: Verificare Oferte Disponibile per Network in GEO

**Tool**: Dashboard fiecarei retele (dupa creare cont) sau AM contact + mcp__brave-search

**Actiune**:
Pentru fiecare retea cu scor >3.0:

1. **Daca cont existent**: Logheaza-te si filtreaza ofertele per GEO + vertical:
   - MaxBounty: Offers > Browse > filtreaza GEO + Category
   - ClickDealer: Offers > filtreaza Country + Vertical
   - Mobidea: Dashboard > Top Offers > filtreaza GEO + Carrier

2. **Daca cont lipsa**: Contacteaza AM al retelei si cere lista oferte disponibile pentru GEO tinta:
   - Email template: "We are an SMS carrier marketing company with direct operator partnerships in Bolivia and Romania. We are looking for CPA offers for [vertical]. Can you share available offers for BO/RO?"
   - Solicita: offer name, payout, GEO coverage, tracking type, conversion event

3. Documenteaza per retea:

| Network | Offer Name | Vertical | GEO | Payout | Model | CR Est | Tracking | Status |
|---|---|---|---|---|---|---|---|---|
| MaxBounty | AvaTrade | Trading | BO | $250 | CPA | 1-2.5% | Pixel+Postback | APLICAT |
| MaxBounty | [Finance offer RO] | Finance | RO | $150 | CPA | est | Pixel | DE APLICAT (April) |
| ClickDealer | [App CPI BO] | App Downloads | BO | $1.50 | CPI | 4-8% | Postback | DE CAUTAT |
| Mobidea | [WAP content BO] | Mobile Content | BO | $0.50 | CPL | 5-10% | API | DE EVALUAT |

**Output (artifact)**: `smsads-offers-per-network-{GEO}-{YYYY-MM}.csv`
**Checkpoint PASS**: Minim 2 oferte documentate per retea cu scor >3.0. FAIL daca retelele cu scor >4.0 nu au oferte verificate (nu estimate).

---

### Pas 4: Reputatie Check si Due Diligence

**Tool**: STM Forum + mcp__brave-search + mcp__tavily__tavily_search + AffPaying.com

**Actiune**:
Pentru fiecare retea candidata cu scor >3.0, verifica:

1. **STM Forum** (stmforum.com):
   - Search: "[network name] review 2025" / "[network name] shaving" / "[network name] payment"
   - Identifica: reviews recente (ultimele 6 luni), plangeri de shaving, probleme de plata
   - Noteaza: rating general (1-5 stele), numarul de reviews

2. **AffPaying.com**:
   - Search retea → verifica rating, reviews utilizatori, data ultimei plati confirmate
   - Flag: rating <3.5 sau plangeri de "payment delays" in ultimele 3 luni = RED FLAG

3. **mThink Blue Book** (mthink.com) — ranking annual:
   - Top 20 CPA networks (annual ranking) → verifica daca reteaua apare
   - Prezenta in top 20 = semnal de stabilitate si volum

4. **Verifica direct cu AM**:
   - "Sunteti direct advertiser sau broker pentru ofertele din Bolivia/Romania?"
   - "Care este frecventa platilor si care este threshold-ul minim?"
   - Direct advertiser (nu broker) = risc de shaving mai mic, plati mai rapide

**Flag-uri de risc**:
- STM reviews mai vechi de 12 luni = insuficient (retea poate fi schimbata)
- Rating AffPaying <3.5 = DUE DILIGENCE extins sau exclude
- "Payment delays" mentionate in ultimele 6 luni = CAUTION
- Nu apare in nicio lista de retele cunoscute = EXCLUDE (risc frauda)
- Nu poate confirma daca e direct advertiser sau broker = CAUTION

**Output (artifact)**: `smsads-network-due-diligence-{YYYY-MM}.md`
**Checkpoint PASS**: Due diligence completat pentru toate retelele cu scor >3.0, cu surse citate. FAIL daca retelele cu RED FLAG sunt incluse in ranking final fara justificare.

---

### Pas 5: Contact AM Direct + Negociere Termeni

**Tool**: Email direct + Telegram + mcp__brave-search (pentru gasire contacte AM)

**Actiune**:
1. Identifica contactul AM per retea:
   - MaxBounty: form aplicare pe site → AM alocat automat dupa aprobare
   - ClickDealer: advertiser@clickdealer.com sau LinkedIn search "ClickDealer account manager"
   - Mobidea: publishers@mobidea.com sau TG community Mobidea
   - Perform[cb]: form pe site (network enterprise, >$1000/luna volume initial cerut)
   - Toro Advertising: publishers@toro.red

2. Template mesaj de contact:
   ```
   Subiect: SMS Carrier Marketing Partnership — Bolivia/Romania

   Buna ziua,

   Suntem Elevate Marketing LLC (smsads.net), operand campanii SMS direct prin parteneri operator GSM
   in Bolivia (Viva, 2.4M contacte) si Romania (Vodafone, 20M contacte).
   Avem cont activ pe 3SNET si cautam sa diversificam cu oferte [vertical].

   Suntem interesati de oferte [vertical] pentru GEO-urile BO/RO.
   Putem discuta disponibilitatea si termenii?

   Volume estimat: [X SMS/luna per oferta]
   Tracker: Binom (self-hosted)
   Tracking type preferat: Postback S2S

   Multumim,
   [Pafi / echipa Elevate Marketing]
   ```

3. In negociere, solicita:
   - Confirmare daca e direct advertiser (nu broker)
   - Posibilitate de NET15 sau plati saptamanale pentru volume confirmate
   - Cap de volum per oferta (daca exista)
   - Bonus payout la depasirea unui prag de volume

**Output (artifact)**: `smsads-am-contacts-{YYYY-MM}.md` (cu status per retea: contactat / raspuns primit / cont creat / pending)
**Checkpoint PASS**: Minim 2 retele noi contactate, raspuns primit sau cont creat. FAIL daca MaxBounty nu e contactat (prioritate maxima).

---

### Pas 6: Ranking Final Retele si Action Plan

**Tool**: Spreadsheet + mcp__cortex__cortex_store

**Actiune**:
Construieste tabelul de ranking final per GEO:

**Ranking pentru Bolivia (exemplu)**:

| Rank | Network | Scor (1-5) | Oferte Disponibile BO | Verticals | Payment | AM Contact | Status | Actiune Urmatoare |
|---|---|---|---|---|---|---|---|---|
| 1 | MaxBounty | 4.90 | AvaTrade $250 CPA + 10+ | Finance, Health, Apps | NET15, $50 min | de contactat | PRIORITATE 1 — creare cont imediata | Aplica cont + cerere AvaTrade BO |
| 2 | ClickDealer | 4.20 | App CPI BO (de verificat) | App Downloads, Nutra | NET30, $100 min | publishers@clickdealer.com | PRIORITATE 2 | Creare cont + cerere oferte App BO |
| 3 | Mobidea | 3.80 | WAP content BO (de verificat) | Mobile WAP, Dating | Weekly, $50 min | publishers@mobidea.com | PRIORITATE 2 | Creare cont + cerere oferte BO |
| 4 | Toro Advertising | 3.40 | Mobile LATAM (de verificat) | Mobile, WAP | NET15 | contact form | BACKLOG | Research oferte BO |
| 5 | 3SNET | 3.55 | MarathonBet, 1xBet, alte | Gambling | NET30 | @Aff3snet Timur | ACTIV | Fix tracking + negociaza payout |

**Action Plan obligatoriu cu Deadline**:

| # | Actiune | Owner | Deadline | Prioritate |
|---|---|---|---|---|
| 1 | Creare cont MaxBounty + aplicare AvaTrade BO | Pafi | +3 zile | HIGH |
| 2 | Creare cont ClickDealer + cerere oferte App Downloads BO | Pafi | +5 zile | HIGH |
| 3 | Creare cont Mobidea + cerere oferte WAP BO | Pafi | +7 zile | MEDIUM |
| 4 | Contact AM MaxBounty pentru conditii NET15 | Pafi | +3 zile | HIGH |
| 5 | Negociaza cu 3SNET payout imbunatatit (alternativele validate) | Pafi | +14 zile | MEDIUM |

**Output (artifact)**: `smsads-network-ranking-{GEO}-{YYYY-MM}.md` + `smsads-action-plan-networks-{YYYY-MM}.md`
**Checkpoint PASS**: Ranking completat, action items cu Owner + Deadline. FAIL daca MaxBounty e absent din top 3 fara justificare.

---

## 3. Cortex Logging

```json
{
  "text": "SMSADS-P4 executat: network discovery {GEO} {YYYY-MM}",
  "collection": "business_clickwin",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SMSADS-P4-NETWORK-DISCOVERY",
    "rule_id": "TRAIN-S-001",
    "domain": "smsads",
    "sub_domain": "network-discovery",
    "version": "1.0",
    "tags": ["smsads", "affiliate-networks", "network-discovery", "3SNET", "MaxBounty", "ClickDealer", "Mobidea", "Perform-cb", "Bolivia", "Romania"],
    "geo_analyzed": "",
    "networks_evaluated": 0,
    "networks_scored": [],
    "top_network": "",
    "top_network_score": 0,
    "new_accounts_created": [],
    "ams_contacted": [],
    "execution_date": "",
    "executed_by": "",
    "artifacts": {
      "network_map": "smsads-network-map-{GEO}-{YYYY-MM}.md",
      "network_scores": "smsads-network-scores-{GEO}-{YYYY-MM}.csv",
      "offers_per_network": "smsads-offers-per-network-{GEO}-{YYYY-MM}.csv",
      "due_diligence": "smsads-network-due-diligence-{YYYY-MM}.md",
      "am_contacts": "smsads-am-contacts-{YYYY-MM}.md",
      "network_ranking": "smsads-network-ranking-{GEO}-{YYYY-MM}.md",
      "action_plan": "smsads-action-plan-networks-{YYYY-MM}.md"
    }
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- La activarea oricarui GEO VOX nou: executie OBLIGATORIE inainte de prima campanie
- Review semestrial: evaluarea completa a retelelor active (payout-uri se schimba, retele noi apar)
- Trigger on-demand: la orice blocaj 3SNET (oferta retrasa, cont suspendat, AM indisponibil)
- Pre-lansare Romania (April 2026): executie OBLIGATORIE pana la sfarsitul lunii martie

### WHEN
- Trigger GEO nou: executie in max 5 zile de la confirmarea accesului VOX
- Trigger semestrial: luna 1 si luna 7 a fiecarui an
- Trigger blocaj: executie de urgenta in max 24h

### HOW (violation detection)
- VIOLATION: Campanie lansata intr-un GEO nou fara SMSADS-P4 completat = BLOCARE
- VIOLATION: Dependenta exclusiva de un singur network >6 luni fara alternative evaluate = WARNING escalat la Pafi
- VIOLATION: Cont MaxBounty inexistent dupa >30 zile de la SMSADS-P4 completat = WARNING
- WARNING: Due diligence neactualizat >6 luni per retea activa = WARNING
- Runner: verificare la session start Genie; verificare trimestriala in briefing proiect

### CONNECT
- **SMSADS-P1** (Vertical Discovery) — upstream: GEO-ul analizat in P1 → trigger P4 pentru evaluarea retelelor din acel GEO
- **SMSADS-P2** (Product Finder) — downstream: retelele descoperite si aprobate in P4 intra in master offer list din P2
- **SMSADS-P3** (Competitor Intel) — complementar: networkurile identificate la competitori in P3 → evaluate sistematic in P4
- **M-123** (Offer Commission Scoring) — paralel: ofertele din retelele noi descoperite in P4 → scoring detaliat in M-123
- **M-122** (SMS Vertical Discovery) — complementar: retelele noi pot deschide verticale noi neacoperite de 3SNET
- `procedure-health.json` → adauga entry: `SMSADS-P4-NETWORK-DISCOVERY`

### VERIFY
- [ ] Procedura executata complet? (toti pasii 1-6 cu artifacts documentate)
- [ ] Output satisface criteriile? (minim 5 retele evaluate + ranking + action plan cu Owner + Deadline)
- [ ] VK emis in sesiune? (ambele linii vizibile pentru Pafi)
- [ ] Daca oricare = NU → procedura NU e completa

**Doua VK-uri obligatorii (VK-H-001)**:
1. `[PROC] SMSADS-P4 | §1 §2 §3 §4 | {GEO} | {N} retele evaluate | top: {network} scor {X} | complete`
2. `[CORTEX] "SMSADS-P4: network discovery {GEO}" | FORGE | rule: TRAIN-S-001 | v1.0`

### MODEL ROUTING

| Activitate | Model | Motivul |
|---|---|---|
| Pas 1: Mapping retele per GEO | Sonnet | Cautari structurate, agregare tabelara |
| Pas 2: Scoring retele | Opus | Scoring compozit multi-dimensional + interpretare context SMSads + negociere leverage |
| Pas 3: Verificare oferte per network | Sonnet | Login + filtrare oferte, task mecanic |
| Pas 4: Reputatie check due diligence | Sonnet | Agregare reviews, identificare flag-uri de risc |
| Pas 5: Contact AM + negociere | Opus | Redactare mesaje de contact + strategie negociere payout |
| Pas 6: Ranking final + action plan | Opus | Sinteza + prioritizare strategica + action items |

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|---|---|---|
| Business of Apps network catalog | Sursa primara de retele | businessofapps.com/affiliate/networks/ |
| STM Forum (membership activ) | Reputatie check retele | stmforum.com |
| AffPaying.com | Rating independent retele | affpaying.com |
| mThink Blue Book | Ranking annual retele | mthink.com |
| 3SNET AM Timur | Referinta si comparatie | @Aff3snet Telegram |
| MaxBounty | Retea Prioritate 1 de creat | maxbounty.com |
| ClickDealer | Retea Prioritate 2 de creat | clickdealer.com |
| Mobidea | Retea Prioritate 2 de creat | mobidea.com |
| SMSADS-P1 | Upstream: GEO-ul tinta | `SMSADS-P1-vertical-discovery.md` |
| SMSADS-P2 | Downstream: primeste oferte noi | `SMSADS-P2-product-finder.md` |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---|---|---|
| Retele evaluate per GEO per semestru | Acoperire pipeline | Minim 6 retele evaluate |
| Retele cu conturi active si verificate | Diversificare portofolu retele | Minim 3 (in afara de 3SNET) in 90 zile |
| Oferte disponibile din retele non-3SNET | Reducere dependenta | Minim 5 oferte active din alte retele |
| EC/1K SMS mediu (non-3SNET vs 3SNET) | Calitate diversificare | Non-3SNET EC/1K >= 3SNET EC/1K |
| Timp aplicare cont nou pana la prima oferta aprobata | Viteza onboarding | <14 zile de la aplicare |
| AM contactati cu raspuns primit | Acoperire contact | 100% din retele cu scor >3.0 |

---

## Checklist Pre-Publicare

- [x] Regula asociata: TRAIN-S-001
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint cu 4 checks obligatorii
- [x] Doi VK-uri obligatorii per VK-H-001 specificati
- [x] MODEL ROUTING prezent cu justificare per pas
- [x] Formula scoring compozita cu 5 dimensiuni + exemple calcule (MaxBounty + 3SNET)
- [x] Due diligence flag-uri de risc documentate (shaving, payment delays, broker vs direct)
- [x] Template mesaj contact AM inclus (Pas 5)
- [x] Action plan cu Owner + Deadline in Pas 6
- [x] Checkpoints PASS/FAIL per pas
- [x] Conexiuni la SMSADS-P1, P2, P3, M-122, M-123 documentate
- [x] Riscul de concentrare (dependenta 3SNET) cuantificat in §1
- [x] Limbaj romana cu termeni tehnici in engleza
