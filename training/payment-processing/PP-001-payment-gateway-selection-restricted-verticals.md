---
id: PP-001
title: Payment Gateway Selection for Restricted Verticals
domain: payment-processing
source: Payment Gateway Models and Business Strategies — Fintech (Udemy)
version: 2.0
forge_score: "3.75"
business_mapping: []
---

# PP-001: Payment Gateway Selection for Restricted Verticals

## Obiectiv
Evalua si selecta un payment gateway adecvat pentru business-uri din verticalele restrictionate sau high-risk (gambling MCC 7995, nutraceuticals MCC 5912, direct marketing MCC 5967, crypto, adult, travel), evitand freeze-urile de cont si terminarile abrupte, cu intelegerea completa a structurii de costuri (MDR, rolling reserve, chargeback fees) si a gateway-urilor care accepta fiecare verticala.

## Pasi

### Pasul 1 — Clasificarea business-ului dupa MCC (Merchant Category Code) si nivel de risc
Inainte de orice outreach la procesatori, trebuie sa stiti exact unde va incadrati.

**High-Risk MCC Codes (necesita procesatori specializati — mainstream NU accepta):**
| MCC | Verticala | Risc | Note |
|-----|----------|------|------|
| 7995 | Gambling (online casino, sports betting, poker, lotteries) | FOARTE RIDICAT | Necesita licenta de gambling (MGA, UKGC, Curacao). Stripe/PayPal INTERZIS |
| 5912 | Drug Stores, Pharma, Nutraceuticals (suplimente, CBD, kratom) | RIDICAT | Trial offers cu rebilling sunt red flag maxim |
| 5967 | Direct Marketing — Inbound Telemarketing, Outbound Teleservices | RIDICAT | Include SMSads, carrier billing, premium SMS short codes |
| 5816 | Digital Goods (in-game, virtual goods) | MEDIU-RIDICAT | Asociat frecvent cu fraud |
| 5966 | Direct Marketing — Outbound | RIDICAT | MLM, bizops, subscription boxes |
| 7801 | Government Licensed Online Casinos (Gambling — Lottery) | FOARTE RIDICAT | Necesita licenta guvernamentala |
| 6051 | Non-Financial Institutions — Quasi Cash, Crypto | RIDICAT | Crypto exchanges, wallets |

**Verticalele MEDIUM RISK (pot obtine cont standard cu documentatie solida):**
- Subscription businesses cu trial offers si rebilling (MCC 5968)
- Software licentiat / key resellers
- Dropshipping (fara stoc propriu)
- E-commerce cu livrare internationala
- Travel agencies (OTA, ticket resellers — risc de chargeback ridicat dar MCC standard 4722)

**Regula de aur**: daca business-ul dvs. are MCC 7995, 5912, sau 5967 — NU aplicati la Stripe, PayPal, Square sau orice procesator mainstream. Contul va fi aprobat initial (screening-ul lor e automat) dar terminat la prima tranzactie suspecta sau dupa acumularea de volume. Terminarea poate include raportare la MATCH (blacklist 5 ani).

**Context SMSads / Carrier Billing:**
- Direct Carrier Billing (DCB) si premium SMS short codes sunt clasificate sub MCC 5967 (Direct Marketing).
- Gateway-uri specializate: Boku, Fortumo (DCB pure), DIMOCO, Centili — acestea sunt agregatori de carrier billing, nu gateway-uri de card traditional.
- Daca SMSads combina carrier billing cu card processing: aveti nevoie de 2 procesatori separati (unul pentru DCB, unul pentru carduri).

### Pasul 2 — Matricea de gateway-uri per verticala (cine accepta CE)
**Comparatie gateway-uri pentru verticalele cheie:**

| Gateway | Gambling (7995) | Nutra (5912) | Direct Marketing (5967/SMSads) | Crypto (6051) | Comision MDR | Rolling Reserve | Note |
|---------|:-:|:-:|:-:|:-:|------------|----------------|------|
| **Stripe** | NU | NU | NU | NU (partial) | 1.4%+0.25 EUR | 0% | Mainstream — NU pentru high-risk |
| **PayPal** | NU | NU | NU | NU | 2.49%+0.35 | 0% | Mainstream — NU pentru high-risk |
| **Worldpay (FIS)** | DA | DA | DA | Partial | 3-6% | 5-10% | Tier 1, reputat, accepta gambling cu licenta |
| **Nuvei (ex-Safecharge)** | DA | DA | DA | DA | 3-7% | 5-10% | Publicly traded, tier 1, forte pe iGaming |
| **Emerchantpay** | DA | DA | Partial | Partial | 3-6% | 5-10% | Renumit in iGaming EU |
| **Trust Payments** | DA | DA | DA | DA | 3-6% | 5-10% | PCI Level 1, flexible |
| **Payvision (ING)** | DA | DA | DA | DA | 3-5% | 5-8% | Achizitionat de ING, solid |
| **Coinspaid** | NU (fiat) | NU | NU | DA (crypto) | 0.8-1% | 0% | DOAR crypto settlement |
| **Skrill/Neteller (Paysafe)** | DA (preferat) | Partial | Partial | DA | 3-5% | Variabil | Wallet-based, preferat in gambling |
| **Checkout.com** | DA (selectiv) | NU | NU | DA (selectiv) | 2.5-4% | 0-5% | Tier 1, selectiv pe high-risk |

**Procesatori specializati carrier billing / DCB (pentru SMSads context):**
| Procesator | Tip | Regiuni | Note |
|------------|-----|---------|------|
| Boku | DCB pure | Global (60+ carriers) | Lider DCB, nu face card processing |
| Fortumo | DCB + carrier billing | EU, LATAM, APAC | Achizitionat de Boku |
| DIMOCO | Premium SMS + DCB | EU (focus DACH) | Specializat SMS short codes |
| Centili | DCB | EU, MENA | Micro-payments mobile |
| Digital Virgo | DCB + content billing | Global | Content publisher focused |

### Pasul 3 — Pregatirea documentatiei pentru aplicare
Procesatorii high-risk cer documentatie mult mai riguroasa decat Stripe.

**Documente obligatorii (standard pentru orice high-risk gateway):**
- [ ] Certificate de inregistrare companie (apostilat sau cu traducere certificata daca non-EU)
- [ ] Memorandum/Articles of Association
- [ ] Extras de cont bancar business (ultimele 6 luni — demonstreaza cash flow real)
- [ ] Istoricul de procesare (daca exista): statements de la procesatorul anterior (minimum 6 luni)
- [ ] Chargeback ratio (daca exista): raport oficial de la procesatorul anterior
- [ ] Documente identitate director(i) (pasaport + utility bill pentru adresa, max 3 luni)
- [ ] **Licente de business obligatorii per verticala:**
  - Gambling: licenta MGA (Malta), UKGC (UK), Curacao eGaming, ONJN (Romania)
  - Pharma/nutra: licente de comercializare suplimente (variaza pe jurisdictie)
  - Carrier billing: acorduri cu operatorii de telefonie (carrier agreements)
- [ ] Site-ul FUNCTIONAL cu: T&C, Privacy Policy, Refund Policy, Contact vizibil, logo-uri Visa/MC, preturi clare
- [ ] **Daca gambling**: sectiune "Responsible Gambling" cu link-uri catre organizatii de ajutor, limite de depunere vizibile, optiune de auto-excludere

**Documente suplimentare frecvent cerute:**
- Business plan / descriere business (1-2 pagini): ce vindeti, cui, cum convertiti
- Model de revenue si volume estimate: nr. tranzactii/luna, valoare medie, volum total
- Customer acquisition channels: SEO, Ads, affiliate, organic
- Lista furnizorilor si partenerilor cheie
- Chargeback mitigation plan (ce faceti proactiv — vezi PP-002)

### Pasul 4 — Evaluarea si compararea ofertelor (minimum 3 gateway-uri)
Obtineti oferte de la minimum 3 procesatori inainte de a semna.

**Costurile de procesare (toate negociabile):**
| Cost | Ce este | Tipic low-risk | Tipic high-risk | Negociere |
|------|---------|---------------|----------------|-----------|
| MDR | Rata per tranzactie | 1.4-2.5% | 3-8%+ | Cu volume proiectat in scris |
| Setup fee | Cost unic activare | 0 | 0-5000 EUR | Negociabil daca volume estimat e ridicat |
| Monthly minimum | Fee lunar daca nu atingeti volum | 0-50 EUR | 500-2000 EUR | Negociabil prime 3-6 luni |
| Rolling reserve | % retinut ca garantie | 0% | 5-10% pe 180 zile | Reducere dupa 6 luni clean |
| Chargeback fee | Per incident | 15-25 EUR | 25-50 EUR | Rar negociabil |
| Refund fee | Per refund procesat | 0-5 EUR | 5-15 EUR | Negociabil |
| PCI compliance fee | Anual | 0-100 EUR | 100-500 EUR/an | Inclus uneori in MDR |

**Criterii non-cost (la fel de importante):**
- Stabilitatea financiara a procesatorului (cati ani in business, afilieti bancare — procesatori mici dispar)
- Settlement frequency (cat de des primiti fondurile: zilnic T+1, saptamanal, bilunar)
- Suport tehnic: calitate API documentation, SDK-uri disponibile, sandbox environment, support responsiveness
- Metode de plata acceptate: card (Visa, MC, Amex), SEPA, SWIFT, crypto, e-wallets (Skrill, Neteller), DCB
- Dashboard si reporting (real-time vs. daily batch)
- Limite de volum si scalabilitate
- Redundanta (backup acquiring bank — daca acquiring bank-ul cade, procesatorul are alta ruta?)

### Pasul 5 — Negocierea termenilor contractuali
**Clauze de negociat OBLIGATORIU inainte de semnare:**

1. **Rolling reserve** (cel mai important termen financiar):
   - Standard: 5-10% retinut pe 180 zile
   - Negociati: reducere la 5% dupa 6 luni de procesare cu CB ratio < 0.5%
   - Negociati: reducere perioada la 90 zile dupa 12 luni clean
   - **Calcul impact cash flow**: la 10% RR pe 180 zile cu volum 100K EUR/luna = 60K EUR imobilizati permanent

2. **Termination clause**:
   - Evitati: terminare fara cauza cu 30 zile notice SI reserve retinut 180+ zile
   - Negociati: return complet al reserve-ului in 90 zile de la terminare (nu 180+)
   - Cereti: dreptul de a migra tranzactiile recurring la alt procesator (portabilitate)

3. **Chargeback threshold**:
   - Standard Visa/MC: terminare la 1% CB ratio
   - High-risk procesatori: unii accepta 2-3% inainte de terminare — cereti explicit in contract
   - Cereti: etapa de remediere (30-60 zile) inainte de terminare, nu terminare imediata

4. **Volume caps si scalare**:
   - Cereti: cap initial rezonabil (nu 10K EUR/luna daca proiectati 100K)
   - Cereti: review automat al capului la 3 luni pe baza performantei
   - Cereti: proces clar de cerere increase (nu "la discretia procesatorului")

5. **Currency settlement** (vezi PP-004 pentru detalii):
   - Negociati settlement in moneda dorita (EUR preferat pentru EU)
   - Cereti: FX markup explicit in contract (nu "la rata zilei" — care e rata?)

### Pasul 6 — Selectarea strategiei de aplicare (direct vs. broker)
**Abordarea directa (direct la procesator):**
- Pros: fara fee de broker, relatie directa, MDR mai mic
- Cons: fara advocacy, respingere fara explicatie, re-aplicarea dificila dupa reject
- Recomandare: daca aveti experienta in procesare sau business straightforward

**Abordarea prin broker ISO (Independent Sales Organization):**
- Pros: broker-ul cunoaste ce procesatoare accepta ce verticale, advocacy activ, rata de aprobare mai mare
- Cons: spread pe rata de procesare (0.2-0.5% extra permanent)
- Brokeri consacrati: PaymentCloud.com, HighRiskMerchantAccount.com, Paymentsuite.com
- Recomandare: prima aplicare SAU multiple rejecturi anterioare SAU verticala exotica

**Aplicare simultana**: aplicati la 2-3 procesatoare simultan (nu 10 — creeaza red flags de "merchant shopping"). Scopul: comparati ofertele si aveti backup.

### Pasul 7 — Integrarea tehnica si monitorizarea post-lansare
**Integrare:**
- Utilizati API-ul oficial (nu solutii third-party care creeaza layer suplimentar si adauga latenta)
- **Implementati 3D Secure 2 (3DS2) obligatoriu** — reduce fraudele, shift de liability pe emitent, si reduce rata de CB (vezi PP-002)
- Testati complet in sandbox: tranzactii de succes cu toate card brands, declinuri (toate decline codes), refund-uri, void, partial capture
- Implementati fraud scoring propriu IN PARALEL cu gateway-ul: Seon.io, Kount, MaxMind GeoIP
- Implementati velocity checks: blocati same card / same IP / same device fingerprint multiple orders in X minute

**Monitoring lunar obligatoriu (dashboard):**
| Metrica | Target | Red flag |
|---------|--------|----------|
| Chargeback ratio | < 0.65% (Visa VDMP threshold) | > 0.5% = early warning |
| Fraud rate | < 0.1% | > 0.3% = investigati sursa |
| Decline rate | < 25% | > 30% = probleme de routing sau fraud |
| Reserve balance | Conform calendarului | Discrepante = contactati procesatorul |
| Authorization rate | > 85% | < 75% = probleme BIN routing |

### Pasul 8 — Diversificarea pe 2 procesatori (primary + backup)
**OBLIGATORIU pentru orice business high-risk**: niciodata un singur procesator.

**Strategie de diversificare:**
- **Primary** (70% din volum): procesatorul cu cele mai bune conditii
- **Backup** (30% din volum): al doilea procesator, activ in paralel
- Daca primary cade sau termina contul: rutati 100% pe backup imediat (failover automatic sau manual)
- Tranzactiile declining pe primary se pot ruta automat pe backup (cascade routing)

**Beneficii suplimentare ale diversificarii:**
- Comparatie reala de performanta (decline rate, settlement speed)
- Leverage in negocieri: "Alt procesator ofera MDR cu 0.5% mai putin — puteti egala?"
- Protectie impotriva schimbarilor de politica (un procesator poate decide sa iasa din verticala voastra)

## Verificare
- [ ] Business clasificat corect in categoria de risc cu MCC identificat
- [ ] Gateway-uri candidate identificate din matricea per verticala (minimum 3)
- [ ] Documentatie completa pregatita inainte de prima aplicare
- [ ] Minimum 3 oferte comparate inainte de semnare
- [ ] Rolling reserve si termination clause negociate explicit in contract
- [ ] Integrare testata complet in sandbox inainte de go-live
- [ ] 3DS2 implementat
- [ ] Dashboard de monitoring configurat (CB%, fraud rate, decline rate, reserve balance)
- [ ] Backup processor activ (minimum aplicat, ideal integrat)

## Instrumente
- MATCH List check: verificat prin broker ISO sau banca acquirer
- Seon.io / Kount / MaxMind (fraud prevention)
- Chargebacks911 (chargeback management + Ethoca/Verifi integration)
- Postman (testare API integrare)
- ProcessingCompare.com (comparator gateway-uri — orientativ)

## Note
- **Niciodata nu mintiti in aplicatie despre natura business-ului** — "merchant category miscoding" este frauda si poate duce la inchiderea contului + raportare la MATCH (blacklist 5 ani Visa/MC).
- **Diversificati INTOTDEAUNA pe 2 procesatori** — daca procesatorul primar cade sau termina contul, business-ul nu se opreste.
- **Rolling reserve-ul este capital imobilizat** — calculati-l in cash flow projections. La 10% RR pe 180 zile cu volum de 100K EUR/luna, aveti 60K EUR imobilizati care nu se elibereaza niciodata complet (flowing reserve).
- **Carrier billing (SMSads context)**: DCB nu este card processing — este un flux separat prin operatorul de telefonie. Aveti nevoie de agregator DCB (Boku, Fortumo) PLUS gateway de card separat. Nu incercati sa procesati DCB prin gateway de carduri.
- **Skrill/Neteller** (Paysafe Group) sunt wallet-urile preferate in gambling — multi jucatori le folosesc in loc de card direct. Integrarea lor este complementara gateway-ului de card, nu inlocuitoare.

## Cortex Logging
```json
{
  "type": "procedure_execution",
  "collection": "payment-processing",
  "tags": ["gateway-selection", "high-risk", "MCC", "rolling-reserve", "PP-001"],
  "entry": {
    "procedure_id": "PP-001",
    "timestamp": "{{ISO_8601}}",
    "executor": "{{agent_id}}",
    "business_mcc": "{{MCC_code}}",
    "gateways_evaluated": ["{{gateway_1}}", "{{gateway_2}}", "{{gateway_3}}"],
    "gateway_selected": "{{selected_gateway}}",
    "mdr_negotiated": "{{MDR_%}}",
    "rolling_reserve": "{{RR_%_and_days}}",
    "outcome": "approved | rejected | pending",
    "notes": "{{free_text}}"
  }
}
```

## Enforcement Loop

### WHERE
- Activat cand un business din verticala restrictionata (MCC 7995, 5912, 5967, 6051) are nevoie de procesare de plati.
- Se aplica inainte de orice outreach la procesatori sau semnare de contracte.
- VK: `PP-001-WHERE-ACTIVE`

### WHEN
- La lansarea unui business nou cu componenta de plati high-risk.
- La terminarea unui cont de procesare existent (necesita gateway nou).
- La review semestrial al termenilor contractuali.
- La adaugarea unei noi verticale de business care schimba MCC-ul.
- VK: `PP-001-WHEN-TRIGGERED`

### HOW
- Pasii 1-8 se executa secvential. NU se sare peste niciun pas.
- Minimum 3 gateway-uri evaluate inainte de selectie finala.
- Documentatia se pregateste COMPLET inainte de prima aplicare.
- Negocierea termenilor se face cu template-ul din Pasul 5, nu ad-hoc.
- VK: `PP-001-HOW-EXECUTED`

### CONNECT
- PP-002 (Chargeback Management) — CB mitigation plan se refera la PP-002.
- PP-003 (PCI-DSS) — gateway-ul trebuie sa suporte nivelul PCI necesar.
- PP-004 (Multi-Currency Settlement) — settlement currency negociat in contract.
- PP-005 (High-Risk Onboarding) — procesul de aplicare si documentatie detaliat in PP-005.
- VK: `PP-001-CONNECT-LINKED`

### VERIFY
- [ ] MCC identificat si clasificat corect (Pasul 1).
- [ ] Minimum 3 gateway-uri din matricea Pasul 2 evaluate cu oferte.
- [ ] Documentatie completa (Pasul 3 checklist 100%).
- [ ] Termeni negociati conform Pasul 5 (rolling reserve, termination, volume caps).
- [ ] Backup processor activ (Pasul 8).
- VK: `PP-001-VERIFY-COMPLETE`

## Model Routing
| Faza | Model | Justificare |
|------|-------|-------------|
| Clasificare MCC + evaluare gateway | **Sonnet** | Lookup tabelar, matching — nu necesita reasoning profund |
| Negociere termeni contractuali | **Opus** | Analiza juridica, trade-off-uri complexe |
| Integrare tehnica (API, 3DS2) | **Sonnet** | Executie tehnica standard |
| Monitorizare post-lansare | **Haiku** | Verificare metrici simple, alertare pe threshold |

## Metrics
| Metrica | Formula | Target | Red Flag | Frecventa |
|---------|---------|--------|----------|-----------|
| Gateway approval rate | Nr. aprobari / Nr. aplicatii x 100 | > 66% (2 din 3) | < 33% | Per aplicare |
| MDR negociat vs. default | MDR obtinut / MDR default x 100 | < 80% din default | = default (0% discount) | Per contract |
| Rolling reserve % | RR negociat | 5-7% | 10%+ | Per contract |
| Time to go-live | Zile de la aplicare la prima tranzactie | < 30 zile | > 60 zile | Per onboarding |
| Backup processor status | DA/NU — al doilea procesator activ | DA | NU (single point of failure) | Lunar |
| CB ratio post-lansare | Nr. CB / Nr. tranzactii luna precedenta x 100 | < 0.65% | > 0.5% | Lunar |
