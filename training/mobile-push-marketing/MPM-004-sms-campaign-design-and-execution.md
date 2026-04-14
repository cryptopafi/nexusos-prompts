---
id: MPM-004
title: SMS Campaign Design and Execution SOP
domain: mobile-push-marketing
source: SMS Marketing A-Z with Free Softwares (Udemy) + SMS Marketing Text Message Strategies (Udemy) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# SMS Campaign Design and Execution SOP

## Obiectiv
Proiectează și execută o campanie SMS profitabilă de la zero — de la definirea obiectivului, construirea listei și alegerea platformei, la redactarea mesajului cu copywriting testat pe verticalele betting/casino, trimiterea cu deliverability optimizată și măsurarea ROI — respectând GDPR (EU), TCPA (US), DLT (India) și regulile locale per GEO (Bolivia, Peru, Kenya, Chile).

## Pași

### Pas 1: Definirea obiectivului, audienței și unit economics
1. **Un singur obiectiv principal** per campanie (nu combina):
   - **Lead generation**: click pe link → înregistrare/sign-up (betting/casino principal)
   - **First Time Deposit (FTD)**: obiectiv principal SMSads — user-ul depune prima dată
   - **Reactivare**: clienți care au depus dar nu au mai jucat >30 zile
   - **Retenție**: reminder la clienți activi (bonus weekend, free bet, cashback)
   - **Upsell**: clienți existenți → oferte premium (VIP program, higher stakes)
2. **Segmentarea audienței** (nu trimite la toată lista):
   - Fresh leads (niciodată contactat) → ofertă welcome bonus
   - Registered but no deposit → ofertă "no deposit bonus" sau "free bet"
   - Active players (depozit în ultimele 30 zile) → free spins, cashback
   - Dormant (>60 zile inactiv) → "Ne-ai lipsit! Bonus reactivare 100%"
   - High rollers / VIP → oferte exclusive, limită mare
3. **Unit economics ÎNAINTE de trimitere** (obligatoriu):
   ```
   Cost per SMS (Bolivia): ~$0.02-0.04
   Lista: 10,000 contacte
   Cost total: $200-400
   CTR estimat (SMS betting): 8-15%
   Click-uri estimate: 800-1,500
   Conversion rate (registration): 15-25%
   Registrări estimate: 120-375
   FTD rate: 20-30% din registrări
   FTDs estimate: 24-112
   Payout per FTD: $20-50
   Revenue estimate: $480-5,600
   ROI estimate: 120% - 2,700%
   ```
4. Dacă unit economics nu e pozitivă → NU trimite. Ajustează: ofertă mai bună, segment mai targeted, sau GEO diferit

### Pas 2: Alegerea platformei SMS per GEO și vertical
1. **Platforme recomandate per use case**:
   - **Twilio** (twilio.com): API-first, cel mai fiabil, scalabil. $0.0079/SMS US, pricing variabil per GEO. Best for: volume medii-mari, automatizări
   - **MessageBird** (messagebird.com, now Bird): enterprise, rute directe, multi-channel. Best for: volume mari, deliverability premium
   - **SMSrush** (smsrush.com): WhatsApp + SMS combo, plan freemium, bun pe LATAM. Best for: start rapid, buget mic
   - **ClickSend** (clicksend.com): multi-channel, pay-as-you-go. Best for: echipe mici, interfață simplă
   - **BulkSMS** (bulksms.com): specializat bulk, rute bune Africa/Tier3. Best for: Kenya, volume mari Africa
   - **TextMagic** (textmagic.com): interfață simplă, plată per SMS. Best for: volum mic-mediu EU
   - **Plivo** (plivo.com): alternativă Twilio mai ieftină, API similar. Best for: dezvoltatori, buget limitat
   - **Africa's Talking** (africastalking.com): specializat Africa, rute directe Kenya/Nigeria/Ghana. Best for: GEO Africa exclusiv
2. **Recomandare per GEO SMSads**:
   - Bolivia: Twilio sau Plivo (rute disponibile, cost ~$0.03/SMS)
   - Peru: Twilio sau MessageBird (rute Tier 1 Claro/Movistar)
   - Kenya: Africa's Talking (rute directe Safaricom/Airtel, cost ~$0.01/SMS) sau BulkSMS
   - Chile: Twilio (rute Entel/Movistar/Claro) sau MessageBird
3. **Setup cont**:
   - Creează cont → verifică identitatea (obligatoriu pentru Sender ID personalizat)
   - Înregistrează **Sender ID** alphanumeric (ex: "SMSADS"):
     - Peru: necesită pre-aprobare OSIPTEL
     - Kenya: necesită pre-aprobare la Communications Authority of Kenya
     - Bolivia: ATT (Autoridad de Regulación y Fiscalización de Telecomunicaciones)
     - Chile: SUBTEL approval
   - Adaugă credit/metodă de plată
   - Trimite SMS de test la propriul număr

### Pas 3: Construirea și igienizarea listei de contacte
1. **Surse de contacte (LEGAL)**:
   - Opt-in web form pe landing page (double opt-in recomandat)
   - Campanii push/display cu landing page de lead gen
   - Referral programs (user existent invită prieten)
   - Social media lead ads (Facebook/Instagram lead forms)
   - NU cumpăra liste "cold" fără opt-in → spam, blacklistare, amenzi
2. **Format fișier CSV**:
   ```csv
   phone,first_name,country,segment,opt_in_date,source
   +51921234567,Carlos,PE,fresh_lead,2026-03-15,fb_lead_ad
   +254721234567,James,KE,registered_no_dep,2026-03-10,web_form
   +59171234567,Maria,BO,active_player,2026-02-28,referral
   ```
3. **Curățarea listei** (salvează 15-30% din buget):
   - Format E.164: `+[cod_tara][numar]` (Peru: +51, Kenya: +254, Bolivia: +591, Chile: +56)
   - Elimină duplicate (Excel → Data → Remove Duplicates, sau `sort -u` în terminal)
   - Elimină numere de fix (nu pot primi SMS)
   - Elimină numere cu lungime incorectă (Peru mobile: 9 cifre după +51)
   - **HLR Lookup** (Home Location Register): verifică în timp real dacă numărul e activ
     - Servicii: Twilio Lookup ($0.005/lookup), hlr-lookups.com, BulkSMS HLR
     - Filtrează numerele inactive → economisești costul SMS-urilor care nu se livrează
4. **Blacklist check**: verifică numerele din lista de opt-out (STOP) și exclude-le
5. **Segmentare calitate**:
   - Tier A: HLR activ + a interacționat în 30 zile → trimite primul
   - Tier B: HLR activ + fără interacțiune recentă → trimite cu volum mai mic
   - Tier C: numere nevalidate sau vechi (>6 luni) → HLR lookup obligatoriu înainte de trimitere

### Pas 4: Redactarea mesajului SMS — copywriting care convertește
1. **Regula de aur encoding**:
   - GSM-7 encoding: 160 caractere/SMS. Caractere permise: A-Z, 0-9, spații, punctuație standard
   - Unicode encoding: 70 caractere/SMS. Se activează AUTOMAT dacă folosești: diacritice (ă,î,â,ș,ț), emoji, caractere speciale
   - **Soluție**: scrie FĂRĂ diacritice și emoji în SMS → rămâi la 160 chars
   - Peste 160 chars = 2 SMS = COST DUBLU. Peste 306 chars = 3 SMS = COST TRIPLU
2. **Structura SMS performant** (testat pe betting/casino):
   ```
   [BRAND]: [Hook] + [Oferta] + [CTA] + [Link] + [Opt-out]
   ```
   Exemplu (152 chars, GSM-7):
   ```
   SMSADS: Bonus 200% la prima depunere! Pana la $500. Inregistreaza-te ACUM: bit.ly/smsads-bo STOP pt dezabon.
   ```
3. **Template-uri per obiectiv** (betting/casino):
   - **Welcome/Registration**:
     ```
     SMSADS: 50 rotiri GRATUITE fara depunere! Joaca acum si castiga real: bit.ly/xyz STOP dezabon
     ```
   - **First Deposit**:
     ```
     SMSADS: Carlos, bonus 200% la prima depunere. Pana la $500. Depune acum: bit.ly/xyz STOP dezabon
     ```
   - **Reactivare**:
     ```
     SMSADS: Ne-ai lipsit! Bonus 100% reactivare + 25 free spins. Valabil 48h: bit.ly/xyz STOP dezabon
     ```
   - **Eveniment sport**:
     ```
     SMSADS: Peru vs Chile AZI! Pariaza cu cota 3.50. Primul pariu GRATUIT pana la $20: bit.ly/xyz STOP
     ```
4. **Tehnici copywriting SMS** (fiecare crește CTR cu 5-15%):
   - **Personalizare**: "Carlos, bonus-ul tau de $100 expira in 2h" → +15-25% CTR
   - **Urgență**: "Doar AZI", "Expira in 2h", "Ultimele 50 locuri"
   - **Exclusivitate**: "Doar pentru tine", "Oferta VIP"
   - **Cifre specifice**: "$500 bonus" > "bonus mare", "cota 3.50" > "cota buna"
   - **Social proof**: "2,847 jucatori online acum"
5. **Link tracking** (obligatoriu):
   - Scurtează cu: Bitly (bitly.com), Rebrandly (branded URLs), sau tracker propriu
   - Adaugă UTM: `?utm_source=sms&utm_campaign=welcome_bonus_bo_mar2026`
   - NU folosi link-uri nebranded (bit.ly/xyz123 e ok, dar branded.link/bonus e mai bun)
6. **Opt-out obligatoriu**: "STOP pt dezabon." sau "Txt STOP to stop" — MEREU inclus

### Pas 5: Configurarea și scheduling-ul campaniei
1. **În platformă** → Create Campaign → selectează lista/segmentul
2. **Personalizare cu merge tags**:
   - Twilio: `{{first_name}}`
   - TextMagic/SMSrush: `{first_name}`
   - Setează fallback: `{first_name|Salut}` → dacă prenumele lipsește, folosește "Salut"
3. **Send speed** (CRITIC pentru deliverability):
   - <5,000 SMS: fără restricții speciale
   - 5,000-20,000: 100-300 SMS/minut
   - 20,000-100,000: 200-500 SMS/minut (eșalonare pe 2-4h)
   - >100,000: batch-uri de 20K cu pauze de 30 min între ele
   - WhatsApp: max 50 msg/minut (risc ban la viteze mai mari)
4. **Scheduling optimal per GEO**:
   - Bolivia: 10:00-12:00 și 18:00-20:00 BOT (UTC-4)
   - Peru: 10:00-12:00 și 17:00-19:00 PET (UTC-5)
   - Kenya: 10:00-12:00 și 18:00-20:00 EAT (UTC+3)
   - Chile: 10:00-12:00 și 18:00-20:00 CLT (UTC-4)
   - **NU trimite** înainte de 08:00 sau după 21:00 (ora locală)
   - **Betting**: trimite cu 2-4h înainte de meciul important (engagement maxim)
5. **Preview și test final**:
   - Preview mesajul cu date reale pentru primii 5 contacte
   - Trimite test la propriul număr din fiecare GEO
   - Verifică: link funcționează, personalizare corectă, encoding ok (fără caractere ciudate)

### Pas 6: Trimitere seed test, apoi full launch
1. **Seed test** (MEREU înainte de full send):
   - Trimite la 100-200 contacte din lista Tier A
   - Monitorizează 2h: rata de livrare, click-uri, opt-out-uri
   - Dacă delivery rate <90% → STOP, investigare rută/conținut
   - Dacă opt-out rate >3% → mesajul e prea agresiv, reformulează
   - Dacă CTR <2% → mesajul/oferta nu rezonează, testează alt angle
2. **Full launch** (dacă seed test e ok):
   - Lansează către lista completă cu send speed configurat
   - Monitorizare activă primele 2h:
     - Dashboard: delivered vs. failed
     - Tracker: click-uri, bounce rate pe landing page
     - Auto-reply: procesare STOP-uri
3. **Escalation protocol**:
   - Delivery rate scade sub 85% → reduce send speed cu 50%
   - Delivery rate scade sub 70% → STOP campanie, contact furnizor
   - Bounce rate pe landing page >60% → verifică link-ul, posibil landing page down

### Pas 7: Monitorizarea în timp real și managementul răspunsurilor
1. **Dashboard monitoring** (primele 4h post-launch):
   - Delivered %: target >95% (sub 90% = problemă rută sau listă murdară)
   - Failed / Undelivered: categorisează erorile (invalid number, carrier block, phone off)
   - Click-uri (dacă link tracking activ): target CTR >5% pe betting/casino
2. **Inbound management** (two-way SMS):
   - Auto-reply STOP → "Ai fost dezabonat. Nu vei mai primi mesaje." + adaugă în blacklist IMEDIAT
   - Auto-reply DA/YES → "Bonus-ul tău: [link]. Depune acum pentru 200%!"
   - Auto-reply INFO → "Contactează-ne: [email/whatsapp]"
3. **Procesare opt-out-uri** (legal obligatoriu):
   - STOP primit → scos din TOATE listele în 24h (GDPR: imediat)
   - Export numerele opt-out → adaugă în blacklist global
   - NU trimite niciodată la un număr care a trimis STOP (amenzi până la €20M GDPR)
4. **Export bounce-uri**: exportă numerele invalide → curăță din baza de date

### Pas 8: Raportare post-campanie și analiza ROI
1. **La 48h după trimitere**, calculează metrici:
   - **Delivery rate**: delivered/sent × 100 (target >95%)
   - **CTR**: clicks/delivered × 100 (target SMS betting: 8-15%, general: 3-5%)
   - **Conversion rate**: conversions/clicks × 100 (target registration: 15-25%)
   - **FTD rate**: deposits/registrations × 100 (target: 20-30%)
   - **Cost per click**: total cost / clicks
   - **Cost per FTD**: total cost / FTDs
   - **ROI**: (revenue - cost) / cost × 100
2. **SMS open rate**: 98% (industry standard) — DAR nu e măsurabil direct. CTR e proxy-ul real
3. **Compare vs. benchmark per GEO**:
   | Metric | Bolivia | Peru | Kenya | Chile |
   |---|---|---|---|---|
   | Delivery rate | >93% | >95% | >90% | >95% |
   | CTR (betting) | 8-12% | 10-15% | 12-18% | 8-12% |
   | Cost/SMS | $0.02-04 | $0.03-05 | $0.01-02 | $0.03-05 |
   | Avg CPA (FTD) | $8-15 | $10-20 | $5-12 | $12-25 |
4. **Documentare în tracker** (Google Sheets/Notion):
   ```
   Data | GEO | Segment | Nr SMS | Delivered% | CTR% | Conversions | FTDs | Cost | Revenue | ROI% | Notes
   ```

### Pas 9: Iterare și optimizare continuă
1. **A/B testing** (cel puțin 1 test per campanie):
   - Split lista 50/50 → Varianta A vs. Varianta B
   - Testează: hook-ul (urgență vs. beneficiu), CTA, oferta, ora de trimitere
   - Min 500 contacte per variantă pentru semnificativitate
2. **Frequency management** (previne burnout):
   - Promoțional: max 2-4 SMS/lună per contact
   - Eveniment sport: max 1/zi în zilele cu meciuri
   - Transacțional (confirmări, OTP): fără limită
   - Dacă opt-out rate crește >2% → reduce frecvența imediat
3. **List hygiene** (lunar):
   - HLR lookup pe contacte care nu au interacționat >60 zile
   - Remove numere invalide și inactive
   - Refresh segmentarea (mutare între Tier A/B/C)
4. **Escalarea campaniilor profitabile**:
   - ROI >200% → crește lista (paid acquisition de leads)
   - ROI 50-200% → optimizează (A/B testing, better targeting)
   - ROI <50% → pivot (alt GEO, altă ofertă, alt segment)

## Verificare
- [ ] Obiectiv clar definit (un singur obiectiv per campanie)
- [ ] Unit economics calculată ÎNAINTE de trimitere (ROI estimat pozitiv)
- [ ] Platformă SMS aleasă per GEO cu Sender ID aprobat
- [ ] Lista importată, curățată (E.164, fără duplicate, HLR validated)
- [ ] Blacklist opt-out aplicat (numerele STOP excluse)
- [ ] Mesaj SMS ≤ 160 caractere GSM-7 (fără diacritice/emoji)
- [ ] Toate elementele incluse: brand, ofertă, CTA, link trackabil, STOP
- [ ] Personalizare cu merge tags + fallback definit
- [ ] Send speed limitat per volum (tabel de mai sus)
- [ ] Scheduling la ore optime per GEO (timezone corect!)
- [ ] Seed test (100-200 contacte) trimis și validat
- [ ] Auto-reply pentru STOP configurat și testat
- [ ] Raport post-campanie completat la 48h cu ROI calculat

## Instrumente
- **Twilio**: twilio.com — API scalabil, rute globale, Lookup API pentru HLR
- **MessageBird**: messagebird.com — enterprise, deliverability premium
- **Africa's Talking**: africastalking.com — specializat Africa (Kenya, Nigeria, Ghana)
- **SMSrush**: smsrush.com — WhatsApp + SMS combo, freemium
- **BulkSMS**: bulksms.com — bulk volume Africa/Tier3
- **TextMagic**: textmagic.com — simplu, pay per SMS, bun EU
- **Plivo**: plivo.com — alternativă Twilio mai ieftină
- **Bitly / Rebrandly**: scurtare link cu click tracking
- **hlr-lookups.com**: validare HLR independentă
- **Google Sheets / Notion**: tracking campanii + ROI calculator

## Note
- 1 SMS = 160 caractere GSM-7 sau 70 caractere Unicode (cu diacritice/emoji). Diacriticele DUBLEAZĂ costul SMS-ului
- Open rate SMS: 98% (vs. 20% email) — cel mai puternic canal de messaging direct
- CTR SMS betting/casino: 8-15% (vs. 1-3% push ad-network, 2-5% email)
- Kenya e cel mai ieftin GEO pentru SMS ($0.01-0.02/SMS) cu CTR ridicat (12-18%) — ROI excelent
- Trimiterea fără opt-in = amenzi GDPR până la 4% din cifra de afaceri anuală sau €20M
- MEREU seed test (100-200 contacte) înainte de full send — un mesaj greșit la 50K SMS = dezastru de brand + bani aruncați
- Greșeala #1 care arde bugetul: lista nevalidată (20-30% numere invalide = bani aruncați)
- Greșeala #2: combinarea GEO-urilor cu encoding diferit (spaniolă fără diacritice vs. cu ñ → encoding issues)
- Greșeala #3: frecvență prea mare (>4 SMS/lună promoțional) → opt-out rate explodează la 5-10%
