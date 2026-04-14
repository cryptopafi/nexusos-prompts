---
id: MPM-005
title: Bulk SMS Deliverability Management
domain: mobile-push-marketing
source: SMS Marketing A-Z with Free Softwares (Udemy) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# Bulk SMS Deliverability Management

## Obiectiv
Maximizează rata de livrare a campaniilor bulk SMS (target >95%) prin gestionarea rutelor SMS per GEO, validarea HLR, rotația Sender ID, throttling inteligent, content filtering avoidance și monitorizarea proactivă a bounce-urilor — cu focus pe piețele SMSads (Bolivia, Peru, Kenya, Chile) și verticalele betting/casino.

## Pași

### Pas 1: Înțelegerea infrastructurii de livrare SMS — cum funcționează rutele
1. **Traseul unui SMS**:
   ```
   Platforma ta (Twilio/MessageBird) → SMSC furnizor → Rutare internațională →
   SMSC operator local (Claro/Safaricom) → Rețea mobilă → Telefonul destinatarului
   ```
2. **Tipuri de rute** (în ordinea calității):
   - **Tier 1 (Direct Route)**: contract direct cu operatorul național. Cel mai fiabil (>98% delivery), cel mai scump. Sender ID delivery: 100%
   - **Tier 2 (Aggregator)**: furnizor cu acces la rute Tier 1 prin partener. Fiabilitate bună (93-97%). Sender ID: poate fi înlocuit
   - **Tier 3 (Grey Route / SIM Farm)**: mesajul e trimis prin SIM cards fizice. Ieftin ($0.005/SMS) dar delivery rate 50-80%, risc blocare, Sender ID = număr random
   - **OTT Bypass**: mesaj trimis prin WhatsApp/Viber API — NU e SMS tradițional, deliverability separată
3. **Rute recomandate per GEO SMSads**:
   | GEO | Operator principal | Ruta Tier 1 | Furnizor recomandat | Cost/SMS |
   |---|---|---|---|---|
   | Bolivia | Tigo, Entel, Viva | Tier 1-2 | Twilio, Plivo | $0.02-0.04 |
   | Peru | Claro, Movistar, Entel | Tier 1 | Twilio, MessageBird | $0.03-0.05 |
   | Kenya | Safaricom, Airtel | Tier 1 | Africa's Talking, BulkSMS | $0.01-0.02 |
   | Chile | Entel, Movistar, Claro | Tier 1-2 | Twilio, MessageBird | $0.03-0.05 |
4. **Cum verifici calitatea rutei**:
   - Întreabă furnizorul: "Ce tip de rută folosiți pentru [GEO]? Tier 1 direct sau aggregated?"
   - Test practice: trimite 100 SMS → delivery rate >97% = ruta Tier 1
   - Dacă Sender ID apare ca număr random în loc de brand → ruta e Tier 3 (grey)
5. **Niciodată grey routes pentru betting/casino**: operatorii locali monitorizează conținut gambling — pe grey route risc de blocare imediată

### Pas 2: Validarea și curățarea listei pre-trimitere (HLR + hygiene)
1. **HLR Lookup (Home Location Register)** — verificare în timp real dacă numărul e activ:
   - **Twilio Lookup**: $0.005/lookup — cel mai integrat dacă folosești Twilio
   - **hlr-lookups.com**: $0.005/lookup bulk, independent
   - **BulkSMS HLR**: integrat cu platforma BulkSMS
   - **Africa's Talking**: HLR inclus gratuit în planul lor pentru Africa
   - Filtrează: numere inactive, portate, dezactivate → ELIMINĂ din campanie
   - ROI al HLR: costă $50 pentru 10K lookups, salvează $100-200 în SMS-uri nelivrate
2. **Validare format** (regex E.164):
   - Pattern: `^\+[1-9]\d{6,14}$`
   - Peru: `^\+51[9]\d{8}$` (mobile starts with 9, 9 digits after country code)
   - Kenya: `^\+254[17]\d{8}$` (Safaricom 7xx, Airtel 1xx)
   - Bolivia: `^\+591[67]\d{7}$` (mobile 6xx/7xx, 8 digits)
   - Chile: `^\+56[9]\d{8}$` (mobile starts with 9)
3. **Eliminare duplicate**: un număr apare o singură dată per campanie
4. **Blacklist enforcement**: verifică FIECARE campanie contra blacklist intern (numere STOP)
5. **List quality scoring**:
   - **Tier A** (trimite cu încredere): HLR activ + interacțiune în 30 zile → delivery rate >98%
   - **Tier B** (trimite cu precauție): HLR activ dar >30 zile fără interacțiune → delivery rate 93-97%
   - **Tier C** (trimite slow cu monitoring): numere nevalidate sau >6 luni vechi → delivery rate 70-85%, send speed lent cu monitoring activ
6. **Frecvența validării HLR**:
   - Liste noi: HLR obligatoriu înainte de prima trimitere
   - Liste active: re-validare HLR la fiecare 90 zile
   - După campanie cu delivery <90%: re-validare HLR imediată

### Pas 3: Gestionarea Sender ID per GEO
1. **Sender ID alphanumeric** (ex: "SMSADS"):
   - Avantaj: brand recognition, profesionalism
   - Dezavantaj: unidirectional (nu primești răspunsuri), necesită pre-înregistrare
   - **Reguli per GEO**:
     - Peru: OSIPTEL reglementează Sender ID. Înregistrare prin furnizor (Twilio handles automatic)
     - Kenya: CA (Communications Authority) necesită aprobarea. Africa's Talking procesează în 2-5 zile
     - Bolivia: ATT reglementare. Procesare prin furnizor, 5-10 zile
     - Chile: SUBTEL. Procesare automată prin Twilio/MessageBird
2. **Număr virtual (Long Virtual Number - LVN)**:
   - Permite two-way SMS (primești STOP, DA, etc.)
   - Necesar pentru campanii cu opt-in/opt-out interactiv
   - Cost: $1-5/lună per număr
3. **Shortcode** (5-6 cifre, ex: 12345):
   - Recunoscut ca marketing, deliverability excelentă
   - Cost ridicat: $500-3,000/lună (dedicat) sau $50-100/lună (shared)
   - Recomandat doar pentru volume >100K SMS/lună
4. **Rotație Sender ID** (obligatoriu pentru volume >50K SMS/campanie):
   - Folosește 2-4 Sender ID-uri diferite în rotație
   - Distribuie 25% din volum pe fiecare Sender ID
   - Reduce riscul de blocare pe un singur ID de către operator
   - Implementare: assign diferite Sender ID la diferite batch-uri ale listei
5. **Sender ID warming** (pentru Sender ID nou):
   - Săptămâna 1: max 1,000 SMS/zi
   - Săptămâna 2: max 5,000 SMS/zi
   - Săptămâna 3: max 15,000 SMS/zi
   - Săptămâna 4+: volum normal
   - Skip warming = risc de blocare imediată pe un Sender ID nou

### Pas 4: Optimizarea send speed (throttling inteligent)
1. **De ce viteza contează**: operatorii detectează spike-uri de trafic anormale → throttle sau block
2. **Reguli send speed per volum și GEO**:
   | Volum campanie | Send speed | Durată estimată | Batch size |
   |---|---|---|---|
   | <5,000 SMS | 50-100 SMS/min | 50-100 min | 1 batch |
   | 5,000-20,000 | 100-300 SMS/min | 1-3h | 2-3 batch-uri |
   | 20,000-100,000 | 200-500 SMS/min | 3-8h | 5-10 batch-uri |
   | >100,000 | 300-1,000 SMS/min | 6-12h | batch 10-20K |
3. **Eșalonare pe batch-uri** (pentru volume >20K):
   - Trimite batch 1 (5-10K) → monitorizare 30 min → dacă delivery >95% → batch 2
   - Între batch-uri: pauză 15-30 min (lasă ruta să se "reseteze")
   - Avantaj: dacă detectezi problemă (delivery drop), oprești înainte de a arde toată lista
4. **Distribuție pe ore** (nu trimite totul într-o singură oră):
   - Împarte 50K SMS pe 4h: 12,500/h → ~208/min → sustainable
5. **Configurare throttling** în platformă:
   - Twilio: programatic prin `MessageResource.create()` cu `time.sleep()` între batch-uri
   - SMSrush: "Enable Control Sending Speed" → slider mesaje/minut
   - TextMagic: Advanced Settings → Message rate limit
   - Africa's Talking: rate limiting built-in, configurabil per API key

### Pas 5: Content filtering avoidance — cum nu fii blocat de operatori
1. **Ce scanează operatorii** (content filtering):
   - Cuvinte trigger: "GRATUIT", "CÂȘTIGAT", "URGENT", "ACT NOW", "FREE MONEY"
   - Link-uri suspecte: domenii cu reputație proastă, link-uri scurtate necunoscute
   - Mesaje identice trimise în volum mare (fingerprinting)
   - Conținut gambling/betting explicit în țări unde e ilegal sau restrictionat
2. **Tehnici de evitare** (white hat):
   - **Spintax** — variația mesajului (vezi MPM-008 pentru detalii):
     ```
     {Bonus|Oferta|Promotie} {200|doua sute}% {la prima depunere|pentru tine|exclusiv}
     ```
     Rezultat: fiecare SMS e ușor diferit → greu de detectat ca bulk
   - **Personalizare**: `{first_name}` în fiecare SMS → mesaje unice
   - **Link-uri branded**: `smsads.link/bonus` > `bit.ly/xyz123` (branded domain = reputație)
   - **Evită cuvinte blacklisted**: înlocuiește "GRATUIT" cu "fara cost", "URGENT" cu "limitat"
3. **Content rules per GEO betting/casino**:
   - **Peru**: gambling online LEGAL (regulat de MINCETUR). Poți menționa explicit betting/casino
   - **Kenya**: gambling LEGAL dar heavily regulated (BCLB). Trebuie disclaimer "Bet Responsibly. 18+"
   - **Bolivia**: gambling reglementat de AJ (Autoridad de Fiscalización del Juego). Caution cu conținut explicit
   - **Chile**: gambling online ÎN CURS de reglementare (Ley de Juego Online 2024). Tratează ca grey area
4. **Link warming** (pentru domenii noi):
   - Domeniu nou pentru SMS links → trimite volume mici primele 2 săptămâni
   - Construiește reputația link-ului: 100 SMS/zi → 500 → 2,000 → scale
   - Alternativ: folosește branded shortener cu domeniu vechi (>6 luni)
5. **Template pre-approval** (pe unele rute):
   - India (DLT): OBLIGATORIU template pre-aprobat. Fiecare mesaj trebuie să aibă Template ID
   - Alte GEO-uri: nu e obligatoriu dar pre-approval accelerează deliverability

### Pas 6: Monitorizarea delivery și error code management
1. **Real-time monitoring** (primele 2h post-launch):
   - Delivery rate per minut — dacă scade brusc <85% → STOP campanie
   - Error rate per tip: invalid number, carrier reject, timeout, phone off
   - Comparare delivery rate per operator (dacă platforma oferă):
     - Kenya: Safaricom vs. Airtel — dacă unul blochează, investigare specifică
     - Peru: Claro vs. Movistar — diferențe de deliverability posibile
2. **Error codes și acțiuni**:
   | Code | Semnificație | Acțiune |
   |---|---|---|
   | UNDELIVERABLE | Număr inexistent/dezactivat | Elimină din listă permanent |
   | REJECTED | Operator refuzat (Sender ID, content) | Investigare: Sender ID aprobat? Content filtrat? |
   | EXPIRED | Nu s-a livrat în 24-48h (telefon oprit) | Re-trimite o dată; dacă persistent, marchează dormant |
   | FAILED | Eroare tehnică/rută | Re-trimite prin rută alternativă |
   | THROTTLED | Operator limitează volumul | Reduce send speed cu 50% |
   | UNKNOWN | Status necunoscut | Așteaptă delivery report (poate dura 24-48h) |
3. **Acțiuni bazate pe rate de erori**:
   - UNDELIVERABLE > 5% al listei → lista e murdară, HLR lookup obligatoriu
   - REJECTED > 1% → contactează furnizorul, posibil rută blocată sau Sender ID neaprobat
   - EXPIRED > 10% → GEO-ul are probleme de rețea sau ora trimiterii e greșită (useri cu telefoanele oprite)
4. **Delivery report lag**: unele rute raportează delivery cu delay 1-24h. Nu lua decizia de optimizare înainte de 4h de la trimitere
5. **Export bounce-uri** (post fiecare campanie):
   - Exportă CSV cu toate numerele failed/undeliverable
   - Adaugă în baza de date: `delivery_status = failed`, `last_fail_date = 2026-03-20`
   - La 3 fail-uri consecutive → elimină din listă permanent

### Pas 7: Managementul reputației sender-ului (anti-spam)
1. **Spam signals** ce declanșează blocare la operator:
   - Opt-out rate >2% per campanie (indicator #1)
   - Volume neobișnuit de mari de la un Sender ID nou
   - Mesaje identice trimise la mii de numere
   - Link-uri cu reputație proastă (domeniu flagged de Google Safe Browsing)
   - Reclamații de spam directe la operator
2. **Frecvența maximă recomandată** (per contact per lună):
   - Promoțional betting/casino: 4-6 SMS/lună (mai tolerant decât alte verticale)
   - Promoțional general: 2-4 SMS/lună
   - Transacțional (confirmări, OTP, bonus granted): fără limită
   - Eveniment sport (live scores, bet reminders): max 1/zi în zilele cu meciuri
3. **Whitelist request** (pentru volume >50K SMS/lună pe un singur GEO):
   - Contactează furnizorul SMS pentru whitelisting la operatori
   - Furnizează: business registration, sample messages, opt-in proof
   - Timeline: 1-4 săptămâni pentru aprobare
   - Post-whitelist: delivery rate crește de la 90-93% la 97-99%
4. **Feedback loop** (monitorizare continuă):
   - Verifică săptămânal: opt-out rate, delivery rate, conversion rate
   - Trend negativ pe 2 săptămâni consecutive → acțiune imediată (reduce frecvența, curăță lista)

### Pas 8: Multi-route failover și redundanță
1. **De ce ai nevoie de route failover**: dacă ruta primară e blocată/down, campania continuă pe ruta secundară
2. **Setup multi-route**:
   - Ruta primară: Twilio (Tier 1 direct pentru Peru)
   - Ruta secundară: MessageBird (Tier 1-2 agregator pentru Peru)
   - Ruta terțiară: Africa's Talking (pentru Kenya specific)
3. **Automatic failover** (implementare):
   - Twilio: configurează "Messaging Service" cu multiple senders → failover automat
   - Custom: API retry logic → dacă prima API returnează eroare → re-send prin a doua platformă
4. **Cost optimization** (rutare inteligentă):
   - Rutează prin cel mai ieftin furnizor care are delivery rate >95% pentru acel GEO
   - Exemplu: Kenya → Africa's Talking ($0.01/SMS, delivery 97%) > Twilio ($0.02/SMS, delivery 98%)
   - Diferența de 1-2% delivery poate merita costul dublu doar dacă CPA per conversie e mult mai mic

### Pas 9: Raportare deliverability și plan de îmbunătățire
1. **Raport post-campanie** (obligatoriu la 48h):
   - Total trimise → Livrate → Failed (per error type) → Delivery rate
   - Click-uri → CTR → Conversii → Cost per conversie
   - Opt-out-uri → Opt-out rate
   - Per operator breakdown (dacă disponibil)
2. **Raport lunar deliverability**:
   - Delivery rate mediu per GEO (trend pe 4 săptămâni)
   - Volumul de bounce-uri per categorie
   - Opt-out rate cumulat
   - Cost per SMS delivered (nu per SMS trimis — costul real)
   - Comparație furnizor: Twilio vs. MessageBird vs. Africa's Talking per GEO
3. **Acțiuni la delivery <92%**:
   - [ ] HLR lookup complet pe întreaga listă
   - [ ] Contactare furnizor pentru investigare rută
   - [ ] Test cu furnizor alternativ (switch temporary)
   - [ ] Revizuire conținut mesaje (posibil filtrate)
   - [ ] Verificare Sender ID approval status
4. **KPI targets deliverability**:
   | Metric | Target | Red flag |
   |---|---|---|
   | Delivery rate | >95% | <90% |
   | Opt-out rate | <1% | >2% |
   | Invalid numbers | <3% | >5% |
   | Carrier reject | <0.5% | >1% |

## Verificare
- [ ] Furnizor SMS cu rute Tier 1 ales per fiecare GEO target
- [ ] HLR lookup efectuat pe întreaga listă (sau lista e <30 zile veche)
- [ ] Format numere validat per GEO (E.164 + regex per țară)
- [ ] Duplicate eliminate, blacklist opt-out aplicat
- [ ] Sender ID înregistrat și aprobat per GEO
- [ ] Sender ID warming plan definit (dacă ID e nou)
- [ ] Send speed configurat per volum (tabel de referință)
- [ ] Spintax/personalizare implementat (anti-fingerprinting)
- [ ] Link-uri branded cu reputație bună (nu URL shortener generic)
- [ ] Monitoring activ primele 2h post-launch cu escalation protocol
- [ ] Error codes procesate și numere invalide exportate
- [ ] Rută failover configurată (minim 2 furnizori)
- [ ] Raport deliverability completat la 48h

## Instrumente
- **Twilio Lookup**: twilio.com/lookup — HLR validation ($0.005/nr)
- **Africa's Talking**: africastalking.com — HLR inclus, rute directe Africa
- **BulkSMS HLR**: bulksms.com — HLR bulk
- **hlr-lookups.com**: validare HLR independent, bulk pricing
- **MessageBird**: messagebird.com — enterprise routes, failover built-in
- **Google Safe Browsing**: transparencyreport.google.com — verifică reputația domeniului tău de link-uri
- **Google Sheets**: tracking deliverability log per campanie

## Note
- Delivery rate <90% = STOP, nu trimite mai departe. Investighează cauza (lista murdară SAU rută slabă)
- HLR lookup costă $0.005/număr dar salvează $0.02-0.05 per SMS nelivrat — ROI 4-10x
- Grey routes (SIM farms) sunt tentante (ieftin) dar delivery rate 50-80% + risc de blocare permanent = costă mai mult pe termen lung
- Sender ID warming e ca email IP warming — skip it și ruta se blochează în zile
- Operatorii din Kenya (Safaricom) sunt FOARTE stricti pe conținut gambling — include "Bet Responsibly. 18+" obligatoriu
- Peru (MINCETUR reglementat) — cel mai SMS-friendly GEO din lista SMSads pentru betting
- Bolivia — ATT monitoring activ, raportează volume de test mici înainte de scale
- Diacriticele (ñ în spaniolă e GSM-7, dar ă,î,ș,ț în română NU sunt) → verifică encoding per limbă
- Spaniolă standard (fără diacritice extra): ñ = GSM-7 OK, ¿ = GSM-7 OK, á/é/í/ó/ú = GSM-7 OK
