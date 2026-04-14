---
id: ARB-003
title: OSS Registration + Quarterly Filing RO
domain: romanian-finance
source: Arbitrage Pro Research — Legal Compliance Guide 2026-03-21
version: "2.0"
forge_score: "3.70"
business_mapping: ["arbitrage-pro"]
---

# ARB-003: OSS Registration + Quarterly Filing RO

## Obiectiv
Inregistreaza-te in regimul One-Stop Shop (OSS) prin portalul ANAF SPV si depune corect declaratiile trimestriale de TVA pentru vanzari B2C cross-border in EU, conform Directivei EU 2017/2455 si Codului Fiscal Romania, cand depasesti pragul cumulativ de €10,000/an pentru vanzari catre consumatori din alte tari EU (eBay, Amazon, vanzari directe), centralizand plata TVA-ului la ANAF in loc de inregistrare TVA separata in fiecare stat membru.

## Pasi

### Pasul 1 — Determina daca depasesti pragul de €10,000
**Pragul €10,000 se aplica astfel:**
- Se calculeaza CUMULATIV pe toate tarile EU destinatare (nu per tara)
- Include DOAR vanzari B2C (catre consumatori finali persoane fizice)
- NU include vanzari B2B (acestea merg pe reverse charge — ARB-002)
- NU include vanzari domestice Romania (OLX vanzari catre romani)

**Analiza pentru arbitrage pro:**

| Canal | Tip | Conteaza la prag? |
|-------|-----|-------------------|
| OLX Romania | Domestic B2C | NU |
| eBay vanzare catre cumparator german | Cross-border B2C | DA |
| eBay vanzare catre SRL francez | Cross-border B2B | NU (reverse charge) |
| Amazon.de vanzare catre particulari | Cross-border B2C | DA |
| Vanzare directa la firma italiana | B2B | NU |

**Sub €10,000/an:** Aplici TVA Romania (19%/21%) pe toate vanzarile EU B2C.
**Peste €10,000/an:** OBLIGATORIU sa aplici TVA-ul tarii destinatarului (ex: 21% DE, 21% NL, 20% FR).

### Pasul 2 — Inregistreaza-te in OSS prin ANAF SPV
**Procedura:**
1. Acceseaza portalul ANAF SPV (anaf.ro → Spatiul Privat Virtual)
2. Sectiunea "One-Stop Shop" → "Cerere de inregistrare regim OSS — Uniune"
3. Completeaza datele firmei (CUI, denumire, adresa, persoana de contact)
4. Selecteaza "Regim OSS Uniune" (pentru bunuri vandute din Romania catre EU)
5. Depune cererea electronic
6. ANAF confirma inregistrarea — efect de la primul trimestru urmator

**Nota:** Inregistrarea este VOLUNTARA sub prag, OBLIGATORIE peste prag. Poti opta sa te inregistrezi preventiv daca anticipezi depasirea.

### Pasul 3 — Configureaza cotele TVA per tara destinatara
**Cotele standard TVA EU (2025-2026) cele mai relevante pentru eBay/Amazon:**

| Tara | Cota TVA standard | Nota |
|------|-------------------|------|
| Germania (DE) | 19% | Piata principala eBay |
| Franta (FR) | 20% | |
| Italia (IT) | 22% | |
| Spania (ES) | 21% | |
| Olanda (NL) | 21% | |
| Belgia (BE) | 21% | |
| Austria (AT) | 20% | |
| Polonia (PL) | 23% | |
| Romania (RO) | 19% (posibil 21%) | Domestic, nu OSS |

**Configureaza in software-ul de contabilitate cota per tara destinatara.**

### Pasul 4 — Emite facturi cu TVA-ul tarii destinatarului
- Pe fiecare vanzare B2C cross-border, aplica TVA-ul tarii cumparatorului
- Factura trebuie sa mentioneze: "TVA aplicat conform regimului OSS — [cota]% [tara]"
- eBay/Amazon pot aplica automat TVA-ul corect pe platformele lor (marketplace facilitator)
- Pentru vanzari directe (site propriu, OLX international), TU aplici cota corecta

### Pasul 5 — Tine evidenta detaliata pe 10 ani
**EU impune pastrarea evidentelor 10 ani (nu 5 ani ca in regim normal RO).**

**Evidenta obligatorie per tranzactie:**
- Data vanzarii
- Descrierea bunului
- Pretul de vanzare (cu si fara TVA)
- Cota TVA aplicata si tara destinatarului
- Adresa cumparatorului (dovada tarii de destinatie)
- Factura/bon emis

**Dovezi acceptate pentru tara destinatarului:**
- Adresa de livrare
- Adresa IP (pentru digital)
- Date bancare/card ale cumparatorului

### Pasul 6 — Completeaza declaratia trimestriala OSS
**Periodicitate: TRIMESTRIAL (nu lunar)**

| Trimestru | Perioada | Termen depunere |
|-----------|----------|-----------------|
| Q1 | Ian-Mar | 30 aprilie |
| Q2 | Apr-Iun | 31 iulie |
| Q3 | Iul-Sep | 31 octombrie |
| Q4 | Oct-Dec | 31 ianuarie |

**Continut declaratie:**
- Vanzari B2C detaliate per tara EU destinatara
- Baza impozabila per tara
- Cota TVA aplicata per tara
- TVA total datorat per tara
- TVA total de plata (suma tuturor tarilor)

**Depunere:** Exclusiv electronic prin ANAF SPV → sectiunea OSS

### Pasul 7 — Plateste TVA centralizat la ANAF
- Plata se face intr-un singur cont ANAF (nu in fiecare tara)
- ANAF redistribuie automat TVA-ul catre tarile destinatare
- Termen plata = acelasi ca termenul de depunere a declaratiei
- Moneda: RON (ANAF calculeaza echivalentul)
- **Penalitati intarziere:** dobanzi + majorari standard ANAF

### Pasul 8 — Monitorizare si excluderi
**Excludere din OSS (riscuri):**
- 3 trimestre consecutive fara vanzari cross-border → excludere automata
- Nedepunere repetata a declaratiilor → excludere + obligatie inregistrare TVA in fiecare tara
- Frauda sau neconformitate grava → excludere + penalitati

**Monitorizare continua:**
- Verifica lunar volumul vanzarilor cross-border
- Actualizeaza cotele TVA daca se modifica in vreo tara EU
- Reconciliaza lunar vanzarile din platforme (eBay/Amazon) cu evidenta contabila

## Verificare
- [ ] Pragul de €10,000 calculat corect (doar B2C cross-border, nu B2B sau domestic)
- [ ] Inregistrare OSS confirmata de ANAF pe SPV
- [ ] Cotele TVA per tara configurate corect in software contabilitate
- [ ] Declaratii trimestriale OSS depuse la termen (30 apr, 31 iul, 31 oct, 31 ian)
- [ ] TVA platit centralizat la ANAF in termenul legal
- [ ] Evidenta pastrata 10 ani cu detalii per tranzactie si tara

## Instrumente
- ANAF SPV (anaf.ro) — inregistrare OSS, depunere declaratii, plata
- Software contabilitate cu modul OSS (Oblio, SmartBill)
- eBay/Amazon seller dashboards — rapoarte vanzari per tara
- commenda.io — ghid implementare OSS Romania
- VATupdate.com/romania — modificari cote TVA EU

## Note
- Marketplace facilitator: eBay si Amazon pot colecta si declara TVA-ul in numele tau pentru anumite tranzactii. Verifica daca platforma actioneaza ca "deemed supplier" — in acest caz, TU nu mai declari acele vanzari in OSS.
- B2B pe eBay: Daca cumparatorul furnizeaza un numar de TVA valid, tranzactia iese din OSS si merge pe reverse charge.
- OSS NU inlocuieste D300 sau D301 — acestea continua sa se depuna separat pentru operatiuni intracomunitare B2B.
- Conectat: ARB-001 (margin scheme pe vanzarile nationale), ARB-002 (reverse charge B2B), ARB-005 (structura entitate).
