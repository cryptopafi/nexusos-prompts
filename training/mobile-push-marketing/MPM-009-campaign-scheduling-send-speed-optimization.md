---
id: MPM-009
title: Campaign Scheduling and Send Speed Optimization
domain: mobile-push-marketing
source: Automate WhatsApp & SMS Marketing (SMSrush) + SMS Marketing Strategies (Udemy) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# Campaign Scheduling and Send Speed Optimization

## Obiectiv
Optimizează momentul trimiterii și viteza de livrare a campaniilor SMS, push și WhatsApp pentru a maximiza open rate, CTR și conversiile, minimizând riscul de blacklistare, carrier throttling și dezabonări — cu scheduling adaptat per GEO (Bolivia, Peru, Kenya, Chile), per vertical (betting/casino) și per tip de eveniment (sport live, flash sale, reactivare).

## Pași

### Pas 1: Analiza datelor istorice — propriile date bat benchmark-urile
1. **Dacă ai campanii anterioare** (gold standard):
   - Extrage rapoartele tuturor campaniilor din ultimele 90 zile
   - Tabelează: ora trimiterii × CTR × conversion rate × opt-out rate
   - Construiește **heatmap propriu** (Google Sheets sau Python):
     ```
     Rows: zilele săptămânii (L-D)
     Columns: intervalele orare (08-10, 10-12, 12-14, 14-16, 16-18, 18-20, 20-22)
     Values: CTR mediu per celulă
     Color scale: roșu (slab) → verde (bun)
     ```
   - Identifică top 3 intervale cu CTR maxim și top 3 cu conversii maxime (pot fi diferite!)
   - Pattern-uri comune: CTR maxim ≠ conversie maximă (click dimineața, convertesc seara)
2. **Dacă ești la primele campanii** (benchmark-uri industrie, ajustează per GEO):
   - **SMS promoțional**: peak la **10:00-12:00** și **17:00-19:00** (local time)
   - **Push notifications**: peak la **09:00-10:00** și **19:00-21:00**
   - **WhatsApp**: peak la **10:00-12:00** și **20:00-22:00** (seara, scrolling personal)
   - **Betting pre-match**: **2-4h înainte de meciul important** (engagement maxim)
   - **Casino/Slots**: **20:00-23:00** (useri relaxați, dispuși să joace)
3. **Benchmarks CTR per oră (betting/casino SMS, LATAM)**:
   | Interval | CTR mediu | Note |
   |---|---|---|
   | 08:00-10:00 | 6-8% | Dimineața, trezire, check telefon |
   | 10:00-12:00 | 10-14% | Peak #1, activitate maximă |
   | 12:00-14:00 | 5-7% | Pauza de masă, atenție redusă |
   | 14:00-16:00 | 6-9% | Post-lunch, activitate moderată |
   | 16:00-18:00 | 8-11% | Pre-seară, anticipare eveniment |
   | 18:00-20:00 | 11-15% | Peak #2, betting/casino prime time |
   | 20:00-22:00 | 9-13% | Seara, casino/slots peak |

### Pas 2: Scheduling strategic per GEO și timezone
1. **Ore optime per GEO SMSads** (testate, cu timezone):
   | GEO | Timezone | Peak #1 | Peak #2 | Betting pre-match | Evită |
   |---|---|---|---|---|---|
   | Bolivia | BOT (UTC-4) | 10:00-12:00 | 18:00-20:00 | 2-4h pre-match | <08:00, >21:00 |
   | Peru | PET (UTC-5) | 10:00-12:00 | 17:00-19:00 | 2-4h pre-match | <08:00, >21:00 |
   | Kenya | EAT (UTC+3) | 10:00-12:00 | 18:00-20:00 | 2-4h pre-match | <08:00, >21:00 |
   | Chile | CLT (UTC-4/-3 DST) | 10:00-12:00 | 18:00-20:00 | 2-4h pre-match | <08:00, >21:00 |
2. **Timezone management** (CRITICAL — eroare de timezone = mesaje la 3AM = dezabonări masive):
   - MEREU setează timezone-ul AUDIENȚEI, nu al serverului
   - Dacă ai audiențe din mai multe timezone-uri → SEGMENTEAZĂ și schedule SEPARAT
   - Exemplu greșit: schedule la 10:00 UTC → 12:00 România, 05:00 Peru, 13:00 Kenya
   - Exemplu corect: 3 campanii separate: Peru 10:00 PET, Kenya 10:00 EAT, Bolivia 10:00 BOT
3. **Multi-GEO campaign scheduling template**:
   ```
   Campanie: Welcome Bonus Marzo 2026
   - Peru: 10:00 PET (15:00 UTC) — batch 1
   - Bolivia: 10:00 BOT (14:00 UTC) — batch 2
   - Chile: 10:00 CLT (14:00 UTC) — batch 3
   - Kenya: 10:00 EAT (07:00 UTC) — batch 4 (earliest!)
   ```
4. **DST (Daylight Saving Time) trap**:
   - Chile: CLT = UTC-4 (iarna) → CLST = UTC-3 (vara). Schimbare: apr/oct
   - Peru, Bolivia, Kenya: NU au DST (timezone fixe)
   - Verifică DST MEREU înainte de schedule (tool: timeanddate.com)

### Pas 3: Scheduling per tip de campanie (betting/casino specific)
1. **Pre-match betting SMS**:
   - Trimite cu **2-4h înainte** de meci (engagement maxim, user-ul are timp să parieze)
   - Exemplu: meci la 20:00 → SMS la 16:00-17:00
   - Reminder: al doilea SMS la 30-60 min înainte de meci (dacă user-ul nu a pariat)
   - NU trimite DUPĂ ce meciul a început (frustrant, pierdere)
2. **Live betting push** (real-time):
   - Push notification în timpul meciului: "⚽ Scor 1-0! Cota live {odds} pe {team}!"
   - Timing: imediat după un gol sau eveniment important
   - Automatizare: feed de date sportive → trigger push automat
   - CTR pe live push: 1-3% (de 5-10x mai mare decât push normal pe ad-network)
3. **Casino/Slots campanii**:
   - Seara: 20:00-22:00 (prime time casino, useri relaxați)
   - Weekend: Vineri seara 20:00 + Sâmbătă dimineața 10:00
   - Free spins: trimite dimineața (10:00) — user-ul joacă gratis pe parcursul zilei
4. **Flash sale / Limited time offer**:
   - Trimite la START EXACT al ofertei (nu înainte, nu cu delay)
   - Send speed MAXIM — toți trebuie să primească în max 30 min
   - Reminder: la 50% din timpul oferit ("Mai ai 12h! Bono {bonus_pct}% expira la miezul noptii")
   - Last call: la 2h înainte de expirare ("ULTIMELE 2h! {first_name}, nu pierde bono de {max_bonus}")
5. **Welcome sequence (drip)**:
   - T+0h: Welcome SMS (instant post-registration)
   - T+24h: Reminder deposit (dacă no deposit)
   - T+72h: Free spins offer (dacă no deposit)
   - T+7d: Last chance bonus (scadere urgentă)
   - T+14d: Reactivare mesaj (dacă încă no deposit → move to dormant segment)

### Pas 4: Configurarea send speed — throttling per volum și canal
1. **De ce send speed e critic**:
   - Prea rapid: operatorul detectează spike anormal → throttle (reduce delivery) sau BLOCK
   - Prea lent: campania durează ore → mesajele ajung la ore suboptimale
   - Sweet spot: suficient de rapid pentru timing bun, suficient de lent pentru deliverability
2. **Send speed matrix (SMS)**:
   | Volum | Send speed | Durată | Strategia |
   |---|---|---|---|
   | <1,000 | 50-100/min | 10-20 min | Single batch, fără restricții |
   | 1,000-5,000 | 100-200/min | 25-50 min | Single batch cu throttle |
   | 5,000-20,000 | 200-400/min | 50 min - 1.5h | 2-3 batch-uri cu pauze 10 min |
   | 20,000-50,000 | 300-500/min | 1-3h | 4-5 batch-uri cu pauze 15 min |
   | 50,000-100,000 | 400-700/min | 2-4h | 5-10 batch-uri cu monitoring activ |
   | >100,000 | 500-1,000/min | 4-8h | Batch 15-20K, pauze 20-30 min |
3. **Send speed WhatsApp** (MULT mai conservator — Meta bans):
   | Wrapper (SMSrush) | BSP Oficial (360dialog) |
   |---|---|
   | 20-30 msg/min (start) | Managed by platform |
   | Max 50 msg/min (after warming) | Auto-throttle per tier |
   | >50 = HIGH ban risk | Up to 1,000/min on Tier 4 |
4. **Send speed Push notifications**:
   - OneSignal: managed automatic (no user throttling needed)
   - Ad networks (PropellerAds, etc.): delivery gestionat de platformă
   - Lista proprie: OneSignal Intelligent Delivery = STO automat
5. **Configurare throttling per platformă**:
   - **Twilio**: programatic cu `asyncio.sleep()` între batch-uri sau Messaging Service cu rate limit
   - **SMSrush**: "Enable Control Sending Speed" → slider msg/min
   - **TextMagic**: Advanced Settings → Message rate limit → setează nr/min
   - **Africa's Talking**: rate limiting per API key, configurable

### Pas 5: Send Time Optimization automată (STO)
1. **Ce este STO**: algoritm ML care analizează istoricul individual al fiecărui subscriber și trimite mesajul la ORA OPTIMĂ pentru acel user specific
2. **Platforme cu STO nativ**:
   - **OneSignal Intelligent Delivery**: trimite push în fereastra de 24h, per-user la ora optimă
   - **Braze Intelligent Timing**: SMS + push + email, ML-based
   - **Klaviyo Smart Send Time**: SMS + email optimization
   - **Pushnami**: AI-based delivery optimization
3. **Activare OneSignal Intelligent Delivery**:
   - Creează push campaign → "Delivery" section → selectează "Intelligent Delivery"
   - Campania se livrează pe parcursul a 24h, fiecare user la ora lui optimă
   - Necesită: minim 2-4 săptămâni de date per user (OneSignal colectează automat)
4. **Rezultate STO vs. broadcast fix** (benchmarks):
   - Open rate: +15-30% față de broadcast la oră fixă
   - CTR: +10-20%
   - Opt-out rate: -25-40% (mai puține dezabonări)
5. **Limitări STO**:
   - Nu funcționează pentru flash sales (trebuie toți să primească simultan)
   - Necesită date istorice per user (minimum 2 săptămâni)
   - Nu e disponibil pe toate platformele SMS (mai ales OneSignal = push only)
6. **DIY STO pentru SMS** (fără platformă):
   - Trackează ora la care fiecare contact a click-uit ultimele 5 SMS-uri
   - Calculează media → acela e "optimal send time" per contact
   - Segmentează lista per preferred time: "morning" (08-12), "afternoon" (12-17), "evening" (17-22)
   - Trimite 3 batch-uri separate la orele potrivite fiecărui segment

### Pas 6: Evitarea orelor problematice și a restricțiilor legale
1. **Ore INTERZISE legal**:
   - **TCPA (SUA)**: 8:00 AM - 9:00 PM (ora locală) — strict enforced, amendă per mesaj
   - **India TRAI**: 09:00-21:00 — strict enforced, DLT blocks outside hours
   - **EU/GDPR**: nu există ore fixe legale, dar best practice 08:00-21:00
   - **LATAM + Kenya**: best practice 08:00-21:00, nicio reglementare explicită (dar opt-out crește dramatic la ore antisociale)
2. **Zile de evitat**:
   - **Luni dimineața** (08:00-10:00): inbox aglomerat de peste weekend → CTR -20%
   - **Vineri după-amiaza** (16:00+, B2B): oamenii sunt mental în weekend → CTR -15%
   - **Sărbători naționale** (standard): CTR mai mic pe promoțional generic
   - **EXCEPȚIE BETTING**: zilele de meci MAJOR (Champions League, World Cup) = CELE MAI BUNE zile. CTR +50-100%
3. **Zile optime per vertical betting/casino**:
   - **Marți-Joi**: cel mai consistent CTR pentru promoțional standard
   - **Vineri seara**: casino/slots peak (start weekend)
   - **Sâmbătă**: ziua cu cele mai multe meciuri sportive → betting peak
   - **Duminică seara** (19:00-21:00): surprinzător de bun pentru casino (useri plictisiți)
4. **Ferestre betting per eveniment sportiv**:
   - Copa Libertadores: meciuri Mar-Joi seara (21:00-22:00 local) → SMS la 17:00-18:00
   - Premier League Kenya: Sâmbătă 14:00-17:00 EAT → SMS la 10:00-11:00
   - Eliminatorias CONMEBOL: diverse ore → SMS adaptat per meci specific
   - Liga 1 Peru: Duminică → SMS Sâmbătă seara (previzualizare) + Duminică dimineața

### Pas 7: Campanii urgente și neplanificate — protocol rapid
1. **Flash sale cu fereastră 2-6h**:
   - Trimite la START EXACT cu "Send immediately" (nu schedule)
   - Send speed MAXIM — toți trebuie să primească în max 30 min
   - Trimite DOAR la Tier A contacts (cei mai activi) — nu ai timp de HLR/validare
   - Reminder la jumătatea perioadei: "Mai ai {remaining_hours}h!"
2. **Eveniment sport neașteptat** (transfer, scandal, meci amical anunțat brusc):
   - Scurtează mesajul la esențial: hook + CTA + link (sub 120 chars)
   - Prioritizează: segmentul care a pariat pe sportul respectiv
   - Send speed: maxim permis (500-1000/min)
3. **Campanie planificată cu schedule automat**:
   - Creează campania cu **48h în avans**
   - Schedule la ora exactă cu timezone VERIFICAT
   - Setează backup: alert (email/SMS la tine) dacă campania NU se lansează (monitoring)
   - Double-check: revizuiește la 24h și la 2h înainte de launch

### Pas 8: Batching inteligent — distribuția optimă a volumului
1. **De ce batching (nu all-at-once)**:
   - Detectare timpurie: dacă batch 1 are delivery <90% → oprești înainte de a arde întreaga listă
   - Anti-spam: distribuția pe ore evită spike-urile care trigger-uiesc filtre
   - A/B testing: trimite 10% ca test batch, apoi 90% cu varianta câștigătoare
2. **Strategie batching recomandată (50K SMS)**:
   ```
   Batch 1 (seed test): 500 contacte → Tier A → ora 10:00
   → Monitorizare 1h: delivery >95%? CTR >5%? Opt-out <2%?
   → Dacă DA: continue

   Batch 2: 10,000 contacte → ora 10:30
   → Monitorizare 30 min

   Batch 3: 15,000 contacte → ora 11:30
   → Monitorizare 30 min

   Batch 4: 15,000 contacte → ora 12:30
   → Monitorizare

   Batch 5 (remainder): 9,500 contacte → ora 13:30
   ```
3. **Batching pe zile** (pentru volume foarte mari >200K):
   - Ziua 1: 50K (segment cel mai activ)
   - Ziua 2: 50K (al doilea segment)
   - Ziua 3: 50K (al treilea segment)
   - Ziua 4: 50K (segment cold/test)
   - Avantaj: monitor performance per zi, ajustare mesaj/ofertă

### Pas 9: Monitorizare post-trimitere și optimizare continuă
1. **Monitoring checklist primele 4h**:
   - [ ] Delivery rate per batch (target >95%)
   - [ ] CTR trend (crește, stabil, scade?)
   - [ ] Opt-out-urile procesate (STOP-uri)
   - [ ] Landing page up și funcțional (verificare URL)
   - [ ] Conversii (registrations, deposits) — matching cu așteptările?
2. **Ajustare în timp real**:
   - Delivery rate scade <90% → reduce send speed cu 50%, investighează
   - CTR scade sub 3% (betting) → push alternativă, verifică landing page
   - Bounce rate landing page >60% → posibil landing page down, verifică IMEDIAT
3. **Post-campaign documentation** (pentru optimizare viitoare):
   ```
   Campaign Tracker Row:
   Data | GEO | Segment | Oră | Nr SMS | Delivered% | CTR% | Conv | FTDs |
   Cost | Revenue | ROI% | Opt-out% | Best hour | Worst hour | Notes
   ```
4. **Construcția heatmap-ului propriu** (pe 3-6 luni):
   - Adaugă fiecare campanie în tracker
   - După 20+ campanii: ai propriul heatmap per GEO per canal
   - Heatmap-ul propriu > orice benchmark din industrie (e specific audienței tale)
5. **Optimization cycle**:
   - Săptămâna 1-4: testează ore diferite → identifică peak-urile
   - Săptămâna 5-8: confirmă peak-urile cu volum mai mare
   - Săptămâna 9+: optimizează STO per segment → maximize ROI
   - Lunar: review heatmap, ajustează pe baza sezonalității

## Verificare
- [ ] Ore de trimitere alese pe baza datelor istorice SAU benchmark-urilor per GEO
- [ ] Timezone CORECT setat (timezone audiență, NU al serverului!)
- [ ] DST verificat dacă GEO-ul are DST (Chile are, Peru/Bolivia/Kenya nu)
- [ ] Send speed configurat conform volumului (tabel matrix)
- [ ] Batch-uri planificate cu pauze între ele
- [ ] Ore problematice evitate (<08:00, >21:00 local time)
- [ ] Campanii betting: schedule cu 2-4h înainte de meci
- [ ] Flash sale: send speed maxim, trimitere la start exact
- [ ] Seed test (500 contacte) planificat înainte de full send
- [ ] STO activat unde disponibil (OneSignal Intelligent Delivery)
- [ ] Monitoring activ primele 4h post-lansare cu escalation protocol
- [ ] Date de performanță per oră documentate în tracker
- [ ] Heatmap propriu în construcție (minim 20 campanii pentru date semnificative)

## Instrumente
- **SMSrush**: Control Sending Speed built-in (slider msg/min)
- **Twilio**: rate limiting programatic, Messaging Service config
- **TextMagic**: Advanced Settings → Message rate limit
- **Africa's Talking**: rate limiting per API key
- **OneSignal**: Intelligent Delivery (STO nativ pentru push)
- **Braze**: Intelligent Timing (SMS + push + email)
- **Google Sheets**: heatmap orar propriu (construiești pe 3-6 luni de date)
- **timeanddate.com**: verificare timezone și DST per GEO
- **Cron / LaunchAgent**: scheduling automatizat la oră exactă (dacă API-based sending)

## Note
- Ora corectă de trimitere = diferența între 5% CTR și 15% CTR. E CEL MAI IMPORTANT factor controlabil
- Timezone greșit = mesaje la 3AM = opt-out rate 10-20% = dezastru
- Betting prime time: 2-4h înainte de meci. Casino prime time: 20:00-22:00 seara
- Duminică seara (19:00-21:00) = surprinzător de bun pentru casino/slots (useri plictisiți pre-luni)
- STO (Intelligent Delivery) bate orice oră fixă cu 15-30% — activează-l când ai date suficiente
- Kenya EAT (UTC+3) e cu 7-8h diferență față de LATAM (UTC-4/-5) — nu combina NICIODATĂ în aceeași campanie
- Batching salvează bani: dacă batch 1 are delivery <90%, oprești și investigezi, în loc de a arde 50K SMS pe o rută blocată
- Greșeala #1: trimitere all-at-once la 100K → operator throttle → 30% din mesaje ajung cu 6h delay → CTR dezastruos
- Greșeala #2: schedule la 10:00 UTC pentru toate GEO-urile → Peru primește la 05:00 AM → opt-out masiv
- Greșeala #3: nu monitorizezi primele 2h → landing page e down → 5,000 click-uri pierdute → $0 revenue
