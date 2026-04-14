---
id: PP-002
title: Chargeback Management and Dispute Resolution SOP
domain: payment-processing
source: Fraud Prevention (Udemy) + Payment Gateway Models and Business Strategies — Fintech (Udemy)
version: 2.0
forge_score: "3.75"
business_mapping: []
---

# PP-002: Chargeback Management and Dispute Resolution SOP

## Obiectiv
Implementa un sistem complet de prevenire, urmarire si contestare (representment) a chargeback-urilor pentru a mentine ratio-ul sub pragurile critice Visa/Mastercard, a recupera fonduri din dispute contestabile cu rate de success realiste per reason code, si a proteja contul de procesare de la terminare. Acoperire completa: reason codes Visa 10.4 (fraud), 13.1 (merch not received), Mastercard 4837, 4853 + rate de success la representment.

## Pasi

### Pasul 1 — Intelegerea completa a reason codes si ratelor de succes la representment
**Ce este un chargeback**: cardholder-ul solicita bancii emitente inversarea unei tranzactii. Banca emitenta debiteaza fondurile direct din contul merchantului prin procesator, FARA acordul merchantului.

**Praguri critice Visa/Mastercard (2025-2026):**
| Program | Threshold | Consecinta | Timeline |
|---------|-----------|-----------|---------|
| Visa VDMP (Dispute Monitoring) | > 0.65% CB ratio SAU > 75 CB/luna | Monitorizare, fees suplimentare $50/CB | Auto-enrollare |
| Visa VEDP (Excessive Dispute) | > 0.9% CB ratio SAU > 1000 CB/luna | Fine $25K-50K/luna, potential terminare | Escalare dupa 4 luni VDMP |
| MC Excessive Chargeback Program | > 1.0% CB ratio timp de 2 luni consecutive | Monitorizare, remediere plan obligatoriu | 3-12 luni remediere |
| MC High Excessive Chargeback | > 1.5% CB ratio | Terminare cont, fine $25K-100K | Actiune imediata |

**Formula CB ratio**: `Nr. CB primite luna curenta / Nr. tranzactii procesate luna PRECEDENTA x 100`

**Reason codes complete cu rate de succes la representment:**

**VISA Reason Codes:**
| Code | Descriere | Contestabil? | Rata succes representment | Strategie |
|------|----------|:----------:|:------------------------:|-----------|
| 10.1 | EMV Liability Shift Counterfeit | Rar | 10-15% | Dovediti 3DS sau chip read |
| 10.2 | EMV Liability Shift Non-Counterfeit | Rar | 10-15% | Dovediti chip authentication |
| **10.4** | **Card Absent Environment (FRAUD)** | **Partial** | **20-30%** | **3DS proof, device fingerprint, IP log, delivery confirmation** |
| 11.1 | Card Recovery Bulletin | NU | 0% | Prevenire: verificati BIN in real-time |
| 12.1 | Late Presentment | NU | 5% | Procesati in termenul autorizarii |
| 12.2 | Incorrect Transaction Code | DA | 50-60% | Dovediti codul corect |
| 12.5 | Incorrect Amount | DA | 40-50% | Dovediti suma corecta |
| **13.1** | **Merchandise/Service Not Received** | **DA** | **40-60%** | **Tracking + delivery confirmation + comunicari client** |
| 13.2 | Cancelled Recurring | DA | 30-40% | Dovediti ca nu a cerut anulare sau ca a consumat serviciul |
| **13.3** | **Not as Described / Defective** | **DA** | **30-45%** | **Descriere produs, T&C, comunicari pre-vanzare** |
| 13.6 | Credit Not Processed | DA | 50-60% | Dovediti ca refund-ul a fost procesat sau nu era datorat |
| 13.7 | Cancelled Merchandise/Services | DA | 35-45% | Dovada ca nu a cerut anulare in termeni |

**MASTERCARD Reason Codes:**
| Code | Descriere | Contestabil? | Rata succes | Strategie |
|------|----------|:----------:|:-----------:|-----------|
| **4837** | **No Cardholder Authorization (FRAUD)** | **Partial** | **20-30%** | **3DS proof, AVS match, device data** |
| 4840 | Fraudulent Processing of Transactions | NU | 5-10% | Doar cu dovezi extraordinare |
| **4853** | **Cardholder Dispute (goods/services)** | **DA** | **40-55%** | **Delivery proof, T&C, communications** |
| 4855 | Goods or Services Not Provided | DA | 45-60% | Tracking, delivery confirmation |
| 4860 | Credit Not Processed | DA | 50-60% | Dovada procesarii refund-ului |
| 4863 | Cardholder Does Not Recognize Transaction | Partial | 25-35% | Descriptor clar, comunicari post-tranzactie |

**Regula generala rate de succes:**
- **Fraud (Visa 10.4, MC 4837)**: 20-30% win rate — dificil, dar posibil cu 3DS + device data
- **Service disputes (Visa 13.x, MC 4853)**: 40-60% win rate — contestati MEREU daca aveti dovezi
- **Processing errors (Visa 12.x)**: 40-50% win rate — de obicei erori tehnice, documentatie clara

### Pasul 2 — Prevenirea chargeback-urilor (layer defensiv — cel mai eficient)
**Masuri tehnice anti-fraud (implementati TOATE):**

1. **3D Secure 2 (3DS2)**: OBLIGATORIU pentru toate tranzactiile card-not-present.
   - Shift de liability: daca 3DS trece cu succes, CB tip fraud devine responsabilitatea bancii emitente, nu a merchantului.
   - Reduce CB fraud cu 70-80%.
   - 3DS2 (vs. 3DS1) are friction mai mic — abandonment rate e doar 5-10% mai mare vs. fara 3DS.

2. **AVS (Address Verification System)**: verificati adresa de facturare. Decline daca mismatch complet.

3. **CVV2/CVC2 obligatoriu**: cereti intotdeauna codul de securitate de pe card.

4. **Velocity checks**: blocati:
   - Acelasi card > 3 tranzactii in 1 ora
   - Acelasi IP > 5 tranzactii in 1 ora
   - Acelasi device fingerprint > 3 tranzactii/zi

5. **Fraud scoring real-time** (Seon.io, MaxMind, Kount):
   - Scor 0-100 per tranzactie
   - Auto-decline la scor > [threshold configurat, ex: 80]
   - Review manual la scor 50-80
   - Costul: $0.01-0.05 per tranzactie — ieftin vs. costul unui CB ($25-75)

6. **BIN validation**: verificati:
   - Card prepaid → risc ridicat → decline sau 3DS obligatoriu
   - Tara card ≠ tara IP → red flag → review manual
   - Card emis in tara high-fraud (listele variaza) → friction suplimentara

**Masuri operationale anti-chargeback:**
- **Descriptor clar pe extrasul de cont**: daca descriptorul nu corespunde cu businessul, cardholder-ul contesta ("nu recunosc"). Configurati: `SAPTEFOCURI.RO +40XXX` sau `[BRAND] [TELEFON]`.
- **Email de confirmare imediat post-tranzactie**: include produsul/serviciul, suma, si CUM se pot contacta pentru orice problema.
- **Politica de refund clara si ACCESIBILA**: daca refund-ul este usor de obtinut, cardholder-ul il solicita la VOI, nu la banca (refund nu se numara in CB ratio, chargeback DA).
- **Customer support accesibil**: telefon sau chat live reduce dramatic CB-urile "unable to reach merchant" (Visa 13.x, MC 4853).

### Pasul 3 — Sistem de alerte timpurii (Ethoca + Verifi CDRN)
**Ethoca Alerts (Mastercard) si Verifi CDRN (Visa):**
Trimit alerte INAINTE ca CB-ul sa fie formalizat — aveti o fereastra de 24-72h sa rambursati si sa preveniti CB-ul.

**Cum functioneaza:**
1. Cardholder-ul contesta la banca emitenta
2. Banca emitenta trimite alerta la Ethoca/Verifi INAINTE de a submite CB-ul
3. Merchantul primeste alerta
4. Merchantul rambursaraza IMEDIAT (in 24h)
5. CB-ul NU se mai formeaza → NU se numara in CB ratio

**Cost**: 15-40 USD per alerta — MULT mai ieftin decat un CB format (25-75 EUR fee + pierderea fondurilor + impact pe ratio).

**Implementare:**
- Direct: contractati Ethoca si Verifi separat (complex)
- Prin intermediar (RECOMANDAT): Chargebacks911, Midigator, Sift — integreaza ambele + management automat
- Cost intermediar: fee lunar + per-alert fee

**Monitoring intern obligatoriu:**
- Alerta automata cand CB ratio depaseste 0.5% (early warning — sub VDMP threshold)
- Dashboard ZILNIC cu: CB noi primite, status dispute in curs, CB ratio curent, reserve balance
- Raport saptamanal trimis persoanei responsabile de risk management

### Pasul 4 — SOP de contestare (Representment) per reason code
**Deadline CRITIC**: 7-30 zile (variabil per procesator si schema card) sa trimiteti compelling evidence. Verificati deadline-ul exact in dashboardul procesatorului — depasirea = pierderea automata a disputei.

**Compelling evidence per reason code:**

**Pentru Visa 10.4 / MC 4837 (Fraud — 20-30% win rate):**
- Proof of 3DS authentication (daca a trecut 3DS, liability shift = WIN automat)
- Device fingerprint al tranzactiei
- IP address + geolocation match cu adresa de facturare
- Log de acces al utilizatorului post-plata (screenshots activitate in cont)
- AVS match result
- Comunicari anterioare cu cardholder-ul (email, chat) care dovedesc ca persoana reala a autorizat

**Pentru Visa 13.1 / MC 4855 (Merchandise Not Received — 40-60% win rate):**
- Tracking number + carrier confirmation of delivery
- Recipient signature (daca exista)
- Dovada ca adresa de livrare = adresa de facturare (sau adresa aleasa de client)
- Email-uri de confirmare trimise si deschise (open tracking)
- Pentru servicii digitale: log de acces post-plata, screenshots activitate, IP log

**Pentru Visa 13.3 / MC 4853 (Not as Described — 30-45% win rate):**
- Descriere produs/serviciu EXACT cum a fost prezentata la momentul cumpararii (screenshot pagina)
- Terms & Conditions acceptate de client (timestamp acceptare T&C)
- Comunicari cu clientul despre produs (a mentionat satisfactie inainte de CB?)
- Dovada ca nu a contactat support inainte de a deschide CB (contra argumentul "am incercat sa rezolv direct")

**Template scrisoare de contestare (Rebuttal Letter):**
```
CHARGEBACK REPRESENTMENT

Transaction ID: [XXXXXXXXX]
ARN (Acquirer Reference Number): [XX]
Date: [XX.XX.XXXX]
Amount: [EUR/USD XXX.XX]
Reason Code: [10.4 / 13.1 / 4837 / 4853 / etc.]
Cardholder: [Name on card]

Merchant: [Nume companie, adresa, site, contact]

CONTESTAM aceasta disputa pe baza urmatoarelor dovezi:

1. [Prima dovada — concisa, cu document index atasat]
   Attachment: [A1 — descriere]

2. [A doua dovada — concisa, cu document atasat]
   Attachment: [A2 — descriere]

3. [A treia dovada]
   Attachment: [A3]

[Optional: 4. Dovada ca cardholder-ul a fost informat clar despre politica de refund si nu a solicitat refund inainte de a deschide CB]

CONCLUZIE:
Tranzactia este legitima si serviciul/produsul a fost livrat conform termenilor agreati de cardholder. Solicitam returnarea fondurilor in favoarea merchantului.

Documente atasate: [lista numerotata]

Data: [XX.XX.XXXX]
Semnatura: [Nume, functie]
```

### Pasul 5 — Gestionarea situatiilor critice (CB ratio > 1%)
Daca CB ratio depaseste 1% sau primiti notificare de la procesator / Visa / Mastercard:

**Actiuni imediate (primele 48 ore):**
1. NU opriti procesarea brusc — poate declansa investigatie suplimentara si panic la procesator
2. Identificati SURSA CB-urilor: care produs/serviciu, care canal de trafic (affiliate?), care geo, care tip tranzactie
3. Opriti sursa specifica generatoare (campanie, partener, SKU, geo)
4. Contactati account managerul de la procesator PROACTIV — fiti voi primii care comunicati problema si planul

**Plan de remediere (document scris, trimis procesatorului in 5 zile lucratoare):**
```
CHARGEBACK REMEDIATION PLAN

1. ROOT CAUSE ANALYSIS:
   - Sursa principala CB: [descriere specifica]
   - Volum afectat: [X CB din total Y tranzactii]
   - Perioada: [date]

2. MASURI IMPLEMENTATE IMEDIAT:
   - [Masura 1: ex. "Oprit campania affiliate X care genera 60% din CB-uri"]
   - [Masura 2: ex. "Implementat 3DS obligatoriu pe toate tranzactiile"]
   - [Masura 3: ex. "Activat Ethoca alerts"]

3. PROIECTII:
   - 30 zile: CB ratio estimat [X]%
   - 60 zile: CB ratio estimat [X]%
   - 90 zile: CB ratio sub [0.65]%

4. MONITORING PLAN:
   - Raport zilnic CB ratio intern
   - Raport saptamanal catre procesator
   - Review trimestrial masuri preventive
```

### Pasul 6 — Tracking, reporting si optimizare continua
**KPI de monitorizat ZILNIC:**
| Metrica | Frecventa | Target | Red flag |
|---------|----------|--------|----------|
| CB ratio curent | Zilnic | < 0.65% | > 0.5% |
| Nr. CB noi | Zilnic | < [X]/zi | Spike vs. media |
| Alerte Ethoca/Verifi | Zilnic | Rezolvate in 24h | Alerte neactionate |
| Dispute in curs | Saptamanal | Track status | Deadlines apropiindu-se |
| Win rate representment | Lunar | > 40% (consumer disputes) | < 25% |
| Suma recuperata | Lunar | Track ROI | Scadere trend |

**Raport lunar de chargeback:**
| Metric | Luna curenta | Luna precedenta | Target |
|--------|-------------|----------------|--------|
| CB ratio | | | < 0.65% |
| Nr. CB primite | | | |
| Nr. CB contestate | | | 100% eligibile |
| Win rate (fraud 10.x) | | | 20-30% |
| Win rate (service 13.x) | | | 40-60% |
| Win rate (MC 4853) | | | 40-55% |
| Suma recuperata EUR | | | |
| Alerte Ethoca rezolvate | | | 100% in 24h |
| Reserve balance | | | Conform calendarului |

### Pasul 7 — Optimizare continua si invatare din pattern-uri
**Analiza trimestriala CB (30 minute):**
1. Ce reason codes domina? (fraud → imbunatatiti 3DS + fraud scoring | service → imbunatatiti delivery + support)
2. Ce produse/servicii genereaza cel mai mult CB? (eliminati sau ajustati)
3. Ce canale de achizitie genereaza CB? (affiliate cu trafic toxic → terminati parteneriatul)
4. Ce geo-uri genereaza CB? (blocati tarile cu fraud rate > 5%)
5. Care e best time-to-contest? (contestarile in primele 48h au win rate mai mare)

**Automatizari recomandate:**
- Auto-refund la alerta Ethoca/Verifi (fara interventie manuala — time is critical)
- Auto-flag tranzactii de la repeat offenders (same card cu CB anterior)
- Auto-export daily CB report din dashboard procesator in Google Sheets

## Verificare
- [ ] Ethoca + Verifi CDRN activate (direct sau prin intermediar)
- [ ] 3DS2 implementat pentru TOATE tranzactiile card-not-present
- [ ] Descriptor card recognoscibil configurat (brand + telefon)
- [ ] SOP de contestare documentat cu template-uri per reason code
- [ ] Dashboard CB ratio monitorizat zilnic
- [ ] CB ratio < 0.65% in mod consistent (sub VDMP threshold)
- [ ] Win rate >= 40% pentru dispute contestabile (13.x, 4853)
- [ ] Plan de remediere pregatit (template) in caz de spike
- [ ] Raport lunar CB complet si distribuit

## Instrumente
- Chargebacks911 (management complet CB + Ethoca/Verifi integration)
- Midigator (alternativa Chargebacks911)
- Seon.io / MaxMind / Kount (fraud scoring pre-tranzactie)
- Dashboard procesator (sursa primara de date CB)
- Google Sheets (tracking si reporting CB)

## Note
- **Cel mai eficient mod de a reduce CB-urile este PREVENIREA, nu contestarea** — investiti mai mult in layerele de pre-tranzactie (3DS, fraud scoring, velocity checks) decat in procesul de dispute.
- **Win rate de 40-60% la dispute de serviciu (13.x, 4853) este realist** cu documentatie buna — NU renuntati la contestare.
- **Win rate de 20-30% la fraud (10.4, 4837)** este mai mic dar tot merita contestat daca aveti 3DS proof sau device data puternica.
- **Nu contestati CB-urile de fraud fara niciun fel de dovada** — pierdere garantata + cost de timp + uneori poate irrita bancauritenta.
- **Daca CB ratio ajunge la 1.5%+**: este mai rapid sa migrati la alt procesator si sa "resetati" decat sa asteptati remediere pe contul existent — dar migratia trebuie facuta cu disclosure complet la noul procesator (nu ascundeti istoricul).

## Cortex Logging
```json
{
  "type": "procedure_execution",
  "collection": "payment-processing",
  "tags": ["chargeback", "dispute", "representment", "fraud-prevention", "PP-002"],
  "entry": {
    "procedure_id": "PP-002",
    "timestamp": "{{ISO_8601}}",
    "executor": "{{agent_id}}",
    "cb_ratio_current": "{{CB_%}}",
    "cb_count_month": "{{nr_CB}}",
    "disputes_contested": "{{nr_contested}}",
    "disputes_won": "{{nr_won}}",
    "win_rate": "{{win_%}}",
    "amount_recovered_eur": "{{EUR}}",
    "ethoca_verifi_active": "{{true|false}}",
    "remediation_triggered": "{{true|false}}",
    "outcome": "healthy | warning | critical | remediation",
    "notes": "{{free_text}}"
  }
}
```

## Enforcement Loop

### WHERE
- Activat pe orice business care proceseaza plati cu cardul (indiferent de risc).
- Se aplica in mod continuu — prevenirea si monitorizarea CB sunt operatiuni zilnice.
- VK: `PP-002-WHERE-ACTIVE`

### WHEN
- Zilnic: monitorizare CB ratio si alerte Ethoca/Verifi.
- La fiecare CB primit: evaluare daca e contestabil + initiere representment in 48h.
- La CB ratio > 0.5%: activare early warning + escalare.
- La CB ratio > 1%: activare plan de remediere Pasul 5.
- Lunar: raport complet CB (template Pasul 6).
- Trimestrial: analiza pattern-uri (Pasul 7).
- VK: `PP-002-WHEN-TRIGGERED`

### HOW
- Prevenirea (Pasul 2) se implementeaza COMPLET inainte de go-live (nu dupa primul CB).
- Ethoca/Verifi (Pasul 3) se activeaza in paralel cu integrarea gateway-ului.
- Representment (Pasul 4) se executa cu template-ul standard — nu ad-hoc.
- Deadline-ul de contestare (7-30 zile) este HARD — se verifica imediat la primirea CB.
- VK: `PP-002-HOW-EXECUTED`

### CONNECT
- PP-001 (Gateway Selection) — diversificarea pe 2 procesatori reduce impactul CB pe un singur cont.
- PP-003 (PCI-DSS) — 3DS2 si fraud prevention sunt cerinte PCI care reduc si CB-urile.
- PP-005 (High-Risk Onboarding) — CB mitigation plan este document obligatoriu la aplicare.
- VK: `PP-002-CONNECT-LINKED`

### VERIFY
- [ ] 3DS2 implementat pe TOATE tranzactiile CNP (Pasul 2).
- [ ] Ethoca + Verifi CDRN active (Pasul 3).
- [ ] SOP de contestare documentat cu template rebuttal letter (Pasul 4).
- [ ] CB ratio < 0.65% in mod consistent.
- [ ] Alerte Ethoca/Verifi rezolvate in 24h (100%).
- [ ] Win rate representment >= 40% pe dispute contestabile.
- VK: `PP-002-VERIFY-COMPLETE`

## Model Routing
| Faza | Model | Justificare |
|------|-------|-------------|
| Monitorizare zilnica CB ratio | **Haiku** | Verificare numerica simpla, alertare pe threshold |
| Evaluare contestabilitate per reason code | **Sonnet** | Matching reason code cu strategie din tabel |
| Redactare rebuttal letter | **Sonnet** | Scriere structurata cu template, compelling evidence |
| Analiza root cause CB spike | **Opus** | Pattern recognition complex, multiple surse de date |
| Plan de remediere (CB > 1%) | **Opus** | Strategie critica, impact pe business continuity |

## Metrics
| Metrica | Formula | Target | Red Flag | Frecventa |
|---------|---------|--------|----------|-----------|
| CB ratio | Nr. CB luna curenta / Nr. tranzactii luna precedenta x 100 | < 0.65% | > 0.5% | Zilnic |
| Win rate fraud (10.4/4837) | CB castigate fraud / CB contestate fraud x 100 | 20-30% | < 15% | Lunar |
| Win rate service (13.x/4853) | CB castigate service / CB contestate service x 100 | 40-60% | < 25% | Lunar |
| Ethoca/Verifi response time | Timp mediu rezolvare alerta | < 24h | > 48h | Zilnic |
| Suma recuperata | Total EUR recuperat din representment | Trend crescator | Trend descrescator | Lunar |
| Contestare rate | CB contestate / CB eligibile x 100 | 100% | < 80% | Lunar |
