---
id: PP-005
title: High-Risk Merchant Account Onboarding
domain: payment-processing
source: Payment Gateway Models and Business Strategies — Fintech (Udemy) + Fraud Prevention (Udemy)
version: 2.0
forge_score: "3.75"
business_mapping: []
---

# PP-005: High-Risk Merchant Account Onboarding

## Obiectiv
Naviga procesul complet de onboarding pentru un cont de merchant high-risk (gambling MCC 7995, nutraceuticals MCC 5912, direct marketing MCC 5967 inclusiv SMSads/carrier billing) — de la pre-screening MATCH si eligibilitate pana la go-live — maximizand rata de aprobare, negociind rolling reserve-ul optim (target: 5-7% pe 90-120 zile vs. default 10% pe 180 zile) si stabilind o relatie solida cu procesatorul care permite scalare ulterioara.

## Pasi

### Pasul 1 — Pre-screening complet si verificare eligibilitate
Inainte de a aplica, verificati ca nu exista blocaje fundamentale care vor duce la respingere garantata.

**1.1 — Verificarea MATCH List (Terminated Merchant File):**
- MATCH (Member Alert to Control High-Risk) este baza de date Mastercard cu merchantii terminati din cauza fraud, CB excesiv, sau alti factori.
- Odataa pe MATCH, este extrem de dificil sa obtineti un cont nou timp de **5 ani** (perioada de retentie MATCH).
- **Cum verificati**: cereți o verificare de la un broker ISO, de la banca acquirer potentiala, sau de la procesatorul la care aplicati (ei verifica oricum — dar verificati INAINTE pentru a nu pierde timp).
- Daca sunteti pe MATCH: optiunile sunt foarte limitate — procesatori de nisa care accepta MATCH merchants (exista, dar costurile sunt 2-3x mai mari).

**1.2 — Verificarea sanctiunilor internationale:**
| Lista | URL verificare | Ce verificati |
|-------|---------------|--------------|
| OFAC (SUA) | `sanctionssearch.ofac.treas.gov` | Directori, companie, UBO |
| EU Sanctions | `data.europa.eu` sanctions list | Directori, companie, UBO |
| UK Sanctions (OFSI) | `gov.uk/government/organisations/office-of-financial-sanctions-implementation` | Directori, companie |
| UN Sanctions | `un.org/securitycouncil/sanctions` | Companie, jurisdictie |

Daca oricare din directori, UBO (Ultimate Beneficial Owner > 25%), sau companie apare pe liste de sanctiuni: **onboarding imposibil** la orice procesator legitim.

**1.3 — Verificarea jurisdictiei:**
| Jurisdictie companie | Acceptabilitate | Note |
|---------------------|----------------|------|
| EU (Romania, Malta, Cipru, Estonia) | BUNA | Procesatori EU accepta usor |
| UK | BUNA (cu licente) | Post-Brexit, licente UK separate |
| Curacao, Gibraltar | ACCEPTABILA | Populare gambling, dar scrutin crescut |
| Seychelles, BVI, Vanuatu | DIFICILA | "Substanta" ceruta intensiv |
| Rusia, Belarus, Iran, DPRK, Myanmar | IMPOSIBILA | Sanctionate/restricionate |

### Pasul 2 — Structurarea companiei pentru onboarding optim
**Jurisdictia optima per verticala:**
| Verticala | Jurisdictie recomandata | Licenta necesara |
|----------|----------------------|-----------------|
| Gambling online | Malta (MGA), UK (UKGC) | Malta Gaming Authority / UK Gambling Commission |
| Gambling Romania | Romania (ONJN) | Licenta ONJN |
| Forex / CFD | Cipru (CySEC), Malta (MFSA) | CySEC license / MFSA Category 3 |
| Crypto exchange | Estonia (FIU), Lituania | Crypto license / EMI |
| Nutraceuticals | Romania, UK, orice EU | Fara licenta speciala dar documentatie ingrediente |
| SMSads / Carrier billing (MCC 5967) | Romania, Malta, UK | Carrier agreements, fara licenta separata |

**Structura corporativa recomandata (separare risc):**
```
Holding Company (detine IP, brand, active non-procesare)
      ↓
Operating Company (proceseaza platile, relatia cu procesatorul)
```
- Separati entitatile: daca procesatorul termina contul Operating Company, Holding-ul si brand-ul raman protejate.
- Minimum 1 director cu rezidenta UE (pentru procesatori europeni).
- UBO (Ultimate Beneficial Owner) declarat complet pana la persoana fizica finala.

**Cerinta de substanta economica (crescut in importanta din 2024):**
Procesatorii cer tot mai des dovada de substanta REALA:
- Angajati sau contractori (contracte de munca/servicii)
- Birou/coworking (factura de birou, adresa reala)
- Activitate bancara reala (nu cont deschis si gol — minimum 3 luni de activitate)
- Operatiuni reale (clienti, trafic, revenue demonstrabil)

### Pasul 3 — Construirea dosarului de onboarding (KYB + KYC complet)
**Documente KYB (Know Your Business — companie):**
- [ ] Certificate of Incorporation (apostilat sau traducere certificata daca non-EU)
- [ ] Memorandum and Articles of Association
- [ ] Certificate of Registered Office + Certificate of Directors
- [ ] Certificate of Good Standing sau echivalent (emis recent, max 3 luni)
- [ ] Extras de cont bancar business (ultimele 6 luni — demonstreaza cash flow real)
- [ ] Istoricul de procesare (daca exista): statements de la procesatorul anterior (minimum 6 luni)
- [ ] **Licente de business aplicabile:**
  - Gambling: licenta MGA/UKGC/ONJN/Curacao + numar licenta
  - Crypto: licenta crypto/EMI + numar licenta
  - Nutraceuticals: certificari ingrediente + conformitate etichetare
  - Carrier billing: acorduri cu operatorii de telefonie (carrier agreements — critice pentru MCC 5967)

**Documente KYC (Know Your Customer — persoane fizice):**
- [ ] Pasaport valabil directori + UBO > 25% (copie notariala sau apostilata)
- [ ] Proof of Address per persoana: utility bill sau extras de cont personal (max 3 luni, adresa completa)
- [ ] Daca UBO ≠ directori: organigrama de proprietate cu toti actionarii pana la persoana fizica finala
- [ ] Daca holding company: KYB al holdingului + KYC al directorilor holdingului (recurisv)
- [ ] Criminal background check directori (unii procesatori cer — cereti in avans daca e necesar)

**Business Profile Document (1-3 pagini, CRITIC — diferentiaza aprobarea de respingere):**
```
BUSINESS PROFILE — [COMPANIE]

1. BUSINESS DESCRIPTION:
   Ce vindem: [descriere clara, nu jargon]
   Cui vindem: [target demographic, geo]
   Cum ajung clientii la noi: [acquisition channels — SEO, Ads, affiliate, organic]

2. REVENUE MODEL:
   Tipuri revenue: [subscription/one-time/upsell/commission]
   Valoare medie tranzactie: [X EUR]
   Nr. estimat tranzactii/luna: [X]
   Volum total estimat/luna: [X EUR]
   Target la 6 luni: [X EUR]
   Target la 12 luni: [X EUR]

3. TARGET GEOGRAPHIES (top 5):
   1. [Tara — % din volum]
   2. [Tara — %]
   ...

4. CHARGEBACK MITIGATION PLAN:
   - 3DS2 implementat pe toate tranzactiile
   - Ethoca/Verifi alerts activate
   - Fraud scoring via [Seon/Kount/MaxMind]
   - Politica de refund clara si accesibila
   - Support disponibil [email/telefon/chat]
   - Descriptor recognoscibil: [BRAND TELEFON]

5. WEBSITE & COMPLIANCE:
   - URL: [url]
   - T&C: [URL T&C]
   - Privacy Policy: [URL PP]
   - Refund Policy: [URL RP]
   - Contact: [adresa + email + telefon]
```

**Website compliance checklist (procesatorul va audita site-ul):**
- [ ] URL functional si site LIVE (nu "coming soon")
- [ ] Terms & Conditions complete (jurisdiction, governing law, dispute resolution)
- [ ] Privacy Policy GDPR-conforma
- [ ] Refund Policy clara (proces, timeline, conditii, cum se solicita)
- [ ] Contact page cu adresa fizica, email, telefon
- [ ] Logo-uri Visa/MC afisate corect
- [ ] Preturi afisate clar cu moneda
- [ ] Daca subscription: cum se anuleaza, clar si accesibil (sub 2 click-uri)
- [ ] Daca gambling: sectiune "Responsible Gambling" + link-uri organizatii ajutor + auto-excludere + limite depunere + varsta minima 18+
- [ ] Daca nutraceuticals: disclaimere FDA/EFSA "nu inlocuieste sfatul medical" + lista ingrediente complet
- [ ] SSL valid (HTTPS pe tot site-ul)
- [ ] Cookie consent banner GDPR-conform

### Pasul 4 — Alegerea strategiei de aplicare si timing
**Direct vs. Broker ISO — decizie:**
| Factor | Direct | Broker ISO |
|--------|--------|-----------|
| Cost | Fara fee broker (MDR mai mic cu 0.2-0.5%) | Fee broker (spread permanent pe MDR) |
| Advocacy | Zero — aplicati singuri | Broker cunoaste procesatorul, advocate activ |
| Rata aprobare | Mai mica (fara pre-screening) | Mai mare (broker pre-screen-uieste) |
| Speed | 2-4 saptamani | 1-3 saptamani (broker are relatie existenta) |
| Re-aplicare | Dificila dupa reject (flag intern) | Broker poate redirectiona la alt procesator |
| Recomandare | Experienta in procesare, verticala clara | Prima aplicare, multiple rejecturi, verticala exotica |

**Aplicare simultana la multiple procesatoare:**
- Aplicati la 2-3 procesatoare simultan (NU 10 — creeaza red flags de "merchant shopping")
- Scopul: comparati ofertele si aveti backup daca primul respinge
- NU mentionati la Procesatorul A ca ati aplicat si la B si C (decat daca intreaba — atunci fiti onesti)

**Timing optim de aplicare:**
- Aplicati cu minimum 6 saptamani inainte de data dorita de go-live (unele onboardings dureaza 4-8 saptamani)
- Daca aveti historicul de procesare curat (6+ luni statements cu CB ratio < 0.5%): aplicati cu incredere
- Daca nu aveti istoric: procesatorul va fi mai conservativ pe termeni (MDR mai mare, RR mai mare, cap mai mic)

### Pasul 5 — Negocierea termenilor contractuali (rolling reserve, MDR, clauze critice)
**Tabel de negociere — termeni default vs. target:**
| Termen | Default tipic | Target negociere | Strategie de negociere |
|--------|-------------|-----------------|----------------------|
| **Rolling Reserve %** | 10% | **5-7%** | "Oferim istoric curat + CB mitigation plan. Review la 6 luni?" |
| **Rolling Reserve period** | 180 zile | **90-120 zile** | Mai dificil — procesatorul tine la asta. Cereti review la 12 luni |
| **MDR (rata)** | 5-8% | **3-5%** | Oferiti volume proiectii IN SCRIS + exclusivitate partial |
| **Setup fee** | 1000-5000 EUR | **0-500 EUR** | Negociabil daca volumul estimat e ridicat |
| **Monthly minimum** | 500-2000 EUR | **0-500 EUR** | Negociabil prime 3-6 luni (ramp-up period) |
| **Payout frequency** | Saptamanal | **Zilnic (T+3)** | Negociabil dupa 3-6 luni procesare curata |
| **Volume cap initial** | 50K EUR/luna | **100K+ EUR** | Justificati cu business plan + trafic demonstrabil |

**Clauze ROSII (deal-breakers — refuzati sau renegociati):**
- Terminare fara cauza cu retinere reserve > 180 zile
- Personal liability al directorilor pentru CB-urile companiei
- Exclusivitate completa (procesator unic — trebuie sa aveti dreptul la backup)
- Auto-reinnoire contract fara notificare 60+ zile inainte
- Dreptul procesatorului de a modifica MDR unilateral fara notice
- Clauze de non-compete pe verticala (nu puteti aplica la alt procesator)

**Clauze de CERUT (protectie merchant):**
- Review de termeni la 6 luni pe baza performantei (reducere MDR si RR daca CB ratio < 0.5%)
- Portabilitate token-uri (daca migrezi, token-urile se transfera sau se furnizeaza PAN encrypted)
- Notice period 30 zile pentru orice schimbare de termeni
- Dreptul de a contesta orice deducere din settlement cu dovezi in 5 zile lucratoare

### Pasul 6 — Integrarea tehnica si go-live
**Procesul de integrare (1-4 saptamani):**
1. Primiti credentialele API (sandbox/test + productie) si documentatia tehnica
2. Integrați API-ul in mediul de staging/test
3. Testati TOATE scenariile:
   - Plata reusita (toate card brands: Visa, MC, Amex daca suportat)
   - Plata declinata (toate decline codes relevante)
   - Refund complet si partial
   - Void (anulare pre-settlement)
   - 3DS2 flow (authentication successful, failed, frictionless)
   - Recurring/subscription (daca aplicabil)
   - Webhook-uri (notifications de la procesator)
4. Trimiteti procesatorului lista de teste efectuate cu dovezi (screenshots, log-uri, transaction IDs)
5. Procesatorul face review tehnic si activeaza productia
6. Prima tranzactie in productie — monitorizati-o manual

**Limite initiale (standard practice):**
- Procesatorul seteaza initial limite lunare conservatoare (ex: 50K EUR/luna)
- Limitele cresc dupa 3-6 luni de procesare fara incidente
- **Comunicati PROACTIV** daca asteptati un spike de volum (lansare, campanie, sezon) — procesatorii preferă sa stie in avans decat sa vada spike neanutat si sa opreasca procesarea

### Pasul 7 — Primele 90 de zile — perioada critica de stabilire
**Zilele 1-30 (CRITIC — procesatorul monitorizeaza intens):**
- Monitorizati ZILNIC: CB ratio, decline rate, fraud rate (dashboard + raport intern)
- CB ratio > 0.5% in primele 30 zile = risc de terminare imediata sau freeze fonduri
- Adresati orice problema tehnica imediat (tranzactii duplicate, erori de autorizare, callback-uri esuate)
- Mentineti comunicare PROACTIVA cu account managerul: raport saptamanal cu KPIs
- NU ramp-ati volumul brusc — cresteti gradual (50% max per saptamana)

**Zilele 31-60 (stabilizare):**
- Verificati ca rolling reserve se acumuleaza corect (comparati cu calculul propriu)
- Cereti primul review de performanta de la account manager
- Daca totul e curat: cereti crestere volume cap (cu date de suport)
- Diversificati: daca nu ati facut-o, aplicati la al doilea procesator (backup)

**Zilele 61-90 (optimizare):**
- Analizati decline rates pe BIN/geo/card type — identificati si rezolvati routing issues
- Comparati authorization rate cu benchmark-ul procesatorului (cereti-l)
- Pregatiti cererea de review termeni la 6 luni (reducere MDR, reducere RR)
- Verificati ca settlement-urile se primesc la timp si in sumele corecte

### Pasul 8 — Scalare si relatia pe termen lung cu procesatorul
**Review la 6 luni (cereti proactiv):**
- Prezentati: CB ratio mediu, fraud rate, volume procesat, zero incidente
- Cereti: reducere MDR cu 0.5-1%, reducere RR de la 10% la 5-7%, crestere volume cap
- Cereti: payout mai frecvent (de la saptamanal la T+3 sau T+2)

**Review anual:**
- Comparati termenii cu ofertele actuale de pe piata (cereti oferte de la alti procesatori ca benchmark)
- Renegociati contractul complet daca termenii sunt sub-optime
- Evaluati daca procesatorul actual mai este cel mai bun pentru volumul si mix-ul curent

**Relatia cu account managerul — reguli:**
- Un account manager dedicat care va cunoaste business-ul va poate proteja in perioadele dificile
- Comunicati proactiv (rapoarte, spike-uri, probleme) — NU asteptati sa va contacteze ei
- Raspundeti IMEDIAT la orice cerere de la compliance/risk team (intarzierile = red flag)
- Invitati account managerul la o intalnire video trimestriala (construieste relatie personala)

## Verificare
- [ ] MATCH List verificat pre-aplicare (companie + directori)
- [ ] Sanctiuni OFAC/EU/UN verificate
- [ ] KYB + KYC complet si fara documente lipsa sau expirate
- [ ] Website 100% compliant (toate sectiunile legale prezente si functionale)
- [ ] Business Profile Document scris si revizuit
- [ ] Minimum 2 procesatoare contactate (nu 1 singur)
- [ ] Contract semnat fara clauze rosii identificate
- [ ] Rolling reserve negociat (target: 5-7%, nu 10%)
- [ ] Integrare testata complet in staging (toate scenariile)
- [ ] Primele 30 zile monitorizate zilnic
- [ ] Al doilea procesator (backup) cel putin in aplicare

## Instrumente
- MATCH List: verificat prin broker ISO sau banca acquirer
- OFAC Search: `sanctionssearch.ofac.treas.gov`
- DocuSign (semnare contracte digitale)
- Postman (testare API integrare)
- Loggly / Datadog / Grafana (monitoring tranzactii productie)
- Google Sheets (tracking KPIs: CB ratio, decline rate, fraud rate, reserve balance)

## Note
- **Un dosar complet si un website compliant injumatatesc timpul de onboarding** (2 saptamani vs. 6 saptamani) si cresc dramatic sansele de aprobare.
- **NICIODATA nu furnizati informatii false sau incomplete la KYB** — "false representations" este motiv de terminare imediata + raportare la MATCH (blacklist 5 ani).
- **Relatia cu account managerul este la fel de importanta ca termenii contractuali** — un AM dedicat care va cunoaste business-ul va poate proteja cand CB ratio face spike temporar.
- **Rolling reserve 10% pe 180 zile la 100K EUR/luna = 60K EUR imobilizati permanent** (flowing reserve nu se elibereaza niciodata complet pana la terminare). Negociati AGRESIV pe acest termen — diferenta intre 10% si 5% = 30K EUR capital de lucru eliberat.
- **Carrier billing (SMSads context)**: onboarding la un agregator DCB (Boku, Fortumo) este SEPARAT de onboarding la un gateway de card. Procesele sunt diferite: DCB cere carrier agreements directe, nu licenta de gambling. Aplicati la ambele in paralel.
- **Prima aplicare este cea mai dificila** — dupa ce aveti 6-12 luni de processing history curat, al doilea si al treilea procesator vor fi semnificativ mai usor de obtinut si cu termeni mai buni.

## Cortex Logging
```json
{
  "type": "procedure_execution",
  "collection": "payment-processing",
  "tags": ["onboarding", "high-risk", "KYB", "KYC", "merchant-account", "PP-005"],
  "entry": {
    "procedure_id": "PP-005",
    "timestamp": "{{ISO_8601}}",
    "executor": "{{agent_id}}",
    "business_mcc": "{{MCC_code}}",
    "jurisdiction": "{{country}}",
    "match_list_clear": "{{true|false}}",
    "sanctions_clear": "{{true|false}}",
    "processors_applied": ["{{processor_1}}", "{{processor_2}}"],
    "approval_status": "approved | rejected | pending | conditional",
    "rolling_reserve_negotiated": "{{RR_%_and_days}}",
    "mdr_negotiated": "{{MDR_%}}",
    "days_to_golive": "{{nr_days}}",
    "outcome": "live | onboarding | rejected | blocked",
    "notes": "{{free_text}}"
  }
}
```

## Enforcement Loop

### WHERE
- Activat cand un business high-risk (MCC 7995, 5912, 5967, 6051) necesita cont de procesare nou.
- Se aplica si la migrarea de la un procesator la altul.
- VK: `PP-005-WHERE-ACTIVE`

### WHEN
- La lansarea unui business nou cu componenta de plati high-risk.
- La terminarea unui cont existent (necesita onboarding la alt procesator).
- La adaugarea unei verticale noi care schimba profilul de risc.
- La expirarea licentelor de business (gambling, crypto) — re-onboarding potential necesar.
- VK: `PP-005-WHEN-TRIGGERED`

### HOW
- Pre-screening MATCH + sanctiuni (Pasul 1) se executa INAINTE de orice outreach.
- Dosarul KYB/KYC (Pasul 3) se pregateste COMPLET inainte de prima aplicare.
- Website compliance checklist se verifica 100% inainte de submit.
- Aplicare simultana la 2-3 procesatoare (nu mai mult, nu mai putin).
- Primele 90 zile (Pasul 7) se monitorizeaza zilnic — este perioada critica.
- VK: `PP-005-HOW-EXECUTED`

### CONNECT
- PP-001 (Gateway Selection) — matricea de gateway-uri per verticala se foloseste pentru selectie.
- PP-002 (Chargeback Management) — CB mitigation plan este document obligatoriu in dosarul de onboarding.
- PP-003 (PCI-DSS) — SAQ trebuie completat inainte sau in paralel cu onboarding-ul.
- PP-004 (Multi-Currency Settlement) — settlement currency se negociaza in procesul de onboarding.
- VK: `PP-005-CONNECT-LINKED`

### VERIFY
- [ ] MATCH List verificat — clear (Pasul 1.1).
- [ ] Sanctiuni OFAC/EU/UN verificate — clear (Pasul 1.2).
- [ ] KYB + KYC complet, fara documente lipsa sau expirate (Pasul 3).
- [ ] Website 100% compliant (Pasul 3 checklist).
- [ ] Business Profile Document scris si revizuit (Pasul 3).
- [ ] Contract semnat fara clauze rosii (Pasul 5).
- [ ] Rolling reserve negociat la 5-7% (Pasul 5).
- [ ] Integrare testata complet in staging (Pasul 6).
- [ ] Primele 30 zile monitorizate zilnic (Pasul 7).
- VK: `PP-005-VERIFY-COMPLETE`

## Model Routing
| Faza | Model | Justificare |
|------|-------|-------------|
| Pre-screening MATCH + sanctiuni | **Haiku** | Verificare binara (clear/blocked) pe liste |
| Structurare companie + jurisdictie | **Opus** | Decizie strategica cu implicatii juridice pe termen lung |
| Construire dosar KYB/KYC | **Sonnet** | Compilare documentatie structurata |
| Negociere termeni contractuali | **Opus** | Analiza juridica, trade-off-uri complexe |
| Integrare tehnica + go-live | **Sonnet** | Executie tehnica standard |
| Monitorizare primele 90 zile | **Haiku** | Verificare metrici zilnice, alertare pe threshold |

## Metrics
| Metrica | Formula | Target | Red Flag | Frecventa |
|---------|---------|--------|----------|-----------|
| Approval rate | Nr. aprobari / Nr. aplicatii x 100 | > 66% | < 33% | Per batch aplicare |
| Time to approval | Zile de la aplicare la aprobare | < 21 zile | > 42 zile | Per aplicare |
| Time to go-live | Zile de la aprobare la prima tranzactie live | < 14 zile | > 28 zile | Per onboarding |
| Rolling reserve negociat | RR % obtinut | 5-7% | 10%+ | Per contract |
| MDR negociat | MDR % obtinut | 3-5% | > 7% | Per contract |
| CB ratio primele 30 zile | Nr. CB / Nr. tranzactii x 100 | < 0.5% | > 0.5% | Zilnic (primele 90 zile) |
| KYB completeness | Documente complete / Documente cerute x 100 | 100% | < 100% | Pre-aplicare |
