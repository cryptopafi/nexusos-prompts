---
id: MPM-002
title: Push Subscriber Acquisition & Opt-in Strategy
domain: mobile-push-marketing
source: The Complete Push Notification Course (Udemy) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# Push Subscriber Acquisition & Opt-in Strategy

## Obiectiv
Construiește o bază proprie de abonați push notification pe site/landing page propriu cu rată de opt-in >15%, folosind strategii testate de opt-in, segmentare la momentul abonării, monetizare prin push ads și practici conforme GDPR/ePrivacy — cu focus pe verticale betting/casino și GEO-uri LATAM/Africa.

## Pași

### Pas 1: Alegerea platformei de push notification pentru site propriu
1. **Evaluare platforme per nevoi și buget**:
   - **OneSignal** (onesignal.com): gratuit până la 10K subscribers, SDK complet, API puternic, segmentare avansată. Best general-purpose choice
   - **PushEngage** (pushengage.com): plan gratuit 200 subscribers, excelent pentru e-commerce cu drip campaigns. $9/mo pentru 2.5K subs
   - **iZooto** (izooto.com): focus pe publishers, monetizare directă prin push ads. Revenue share model — bun dacă vrei monetizare fără efort
   - **Pushnami** (pushnami.com): AI-based send time optimization, enterprise-level. Best ROI per push la scale
   - **WebPushr** (webpushr.com): gratuit 10K subscribers, interfață simplă, fără branding pe plan gratuit
   - **Gravitec** (gravitec.net): automatizare puternică, good for content sites. Free tier generos
2. **Pentru monetizare prin push ads** (publisher model):
   - **Monetag** (monetag.com, subsidiara PropellerAds): primești push subscribers, ei trimit ads, tu câștigi per click. $0.5-3 RPM
   - **RollerAds** (rollerads.com): similar Monetag, bun pe Tier 2-3
   - **Subscribers.com**: platforma Zeropark pentru push subscription monetization
3. Creează cont pe platforma aleasă → notează **App ID** și **API Key**
4. **Dual strategy** (recomandat): OneSignal pentru lista ta proprie + Monetag pentru monetizare traficului rezidual

### Pas 2: Instalarea SDK-ului pe site (corect din prima)
1. **WordPress** — instalare plugin:
   - Plugins → Add New → search "OneSignal" → Install → Activate
   - Settings → OneSignal → introduce App ID și REST API Key
   - Configurare: "Prompt users to subscribe: Auto-prompt after delay"
   - Set delay: 15 secunde (nu instant — user-ul trebuie să vadă site-ul întâi)
2. **Site custom (HTML/JS)** — instalare manuală:
   ```html
   <!-- Adaugă în <head> -->
   <script src="https://cdn.onesignal.com/sdks/web/v16/OneSignalSDK.page.js" defer></script>
   <script>
     window.OneSignalDeferred = window.OneSignalDeferred || [];
     OneSignalDeferred.push(async function(OneSignal) {
       await OneSignal.init({
         appId: "YOUR-APP-ID",
         allowLocalhostAsSecureOrigin: true, // doar dev
         promptOptions: {
           slidedown: {
             prompts: [{
               type: "push",
               autoPrompt: true,
               text: {
                 actionMessage: "Vrei notificări cu oferte exclusive?",
                 acceptButton: "Da, vreau!",
                 cancelButton: "Nu acum"
               },
               delay: { pageViews: 2, timeDelay: 15 }
             }]
           }
         }
       });
     });
   </script>
   ```
3. Uploadează **service worker** în root-ul site-ului:
   - `OneSignalSDKWorker.js` — descarcă din OneSignal dashboard
   - TREBUIE să fie în root (https://site.ro/OneSignalSDKWorker.js), altfel push-urile nu funcționează
4. **Verificare HTTPS**: push notifications funcționează DOAR pe HTTPS. Dacă site-ul e HTTP, instalează SSL mai întâi (Let's Encrypt = gratuit)
5. Testează pe 3 browsere diferite (Chrome, Firefox, Edge) și pe mobile (Android Chrome)

### Pas 3: Optimizarea prompt-ului de opt-in (diferența între 3% și 20% opt-in rate)
1. **Regula de aur: NU afișa promptul instant**:
   - Promptul nativ al browserului apare o singură dată — dacă user-ul dă "Block", ai pierdut acel visitor PENTRU TOTDEAUNA
   - Soluția: **custom slidedown** (soft prompt) mai întâi → dacă user click "Da" → declanșează promptul nativ
   - Delay recomandat: 15-30 secunde SAU după 2+ pagini vizitate
2. **Custom slidedown/overlay** (crește opt-in de la 3-5% la 15-25%):
   - Mesaj clar cu beneficiul: "Primești oferte exclusive și alerte de preț direct pe telefon"
   - Butoane cu text acționabil: "Da, vreau oferte!" / "Poate mai târziu"
   - Design: bară sus (slidedown) sau modal centrat — ambele funcționează, testează
   - Culori contrastante pe butonul principal (verde/portocaliu pe fundal alb)
3. **Native vs. Custom prompt comparison**:
   - Native browser prompt: 3-7% opt-in rate, ireversibil dacă user-ul blochează
   - Custom slidedown: 12-25% opt-in rate, user-ul poate vedea din nou mai târziu
   - Two-step (slidedown → native): 15-30% opt-in rate — BEST PRACTICE
4. **Contextual prompt** — afișează pe pagini cu intent ridicat:
   - Pagină de produs → "Vrei să fii notificat când scade prețul?"
   - Pagină de rezultate (betting) → "Primește scoruri live pe telefon!"
   - Blog post → "Vrei articole similare direct pe desktop?"
5. **Welcome notification** — primii 5 minute după opt-in:
   - Configurează auto-send: "Mulțumim! Iată prima ta ofertă exclusivă: [link]"
   - CTR pe welcome push: 15-30% (cel mai mare CTR pe care-l vei avea vreodată pe acel subscriber)
   - Includere welcome offer/bonus = monetizare imediată

### Pas 4: Strategii avansate de creștere a ratei de opt-in
1. **Exit-intent trigger** (desktop):
   - Detectează când cursor-ul se mișcă spre X-ul tabului
   - Afișează overlay: "Înainte să pleci — activează notificările pentru oferte exclusive!"
   - Rezultat tipic: +3-5% opt-in rate adițional
2. **Scroll-triggered prompt**:
   - Declanșează după 50-70% scroll (user-ul a citit conținutul = interes confirmat)
   - Mai puțin intruziv, opt-in rate 8-15%
3. **Incentive-based opt-in** (cel mai puternic):
   - "Abonează-te și primești 10% reducere" → cod promo afișat după opt-in
   - "Rotește roata norocului" → gamificarea opt-in-ului (crește rata cu 40-60%)
   - "Descarcă ghidul gratuit" → deliver prin prima push notification
4. **Social proof în mesajul de opt-in**:
   - "Alătură-te celor 5,247 abonați care primesc oferte exclusive"
   - Numărul trebuie să fie real și actualizat (sau cel puțin plauzibil)
5. **Content locking** (agresiv dar eficient):
   - Blochează conținutul premium (rezultate live, predicții) până la opt-in push
   - Folosit mult în betting vertical: "Abonează-te pentru pronosticuri gratuite"
   - Opt-in rate: 25-40% dar calitatea subscribers e mixtă
6. **A/B test continuous**: testează minim 2 variante de mesaj/design opt-in timp de 2 săptămâni
   - Testează: textul mesajului, culoarea butonului, delay-ul, trigger-ul (scroll vs. time)
   - Folosește OneSignal A/B test built-in sau Google Optimize (gratuit)

### Pas 5: Segmentarea abonaților de la momentul opt-in
1. **Tags automate la abonare** (OneSignal):
   ```javascript
   OneSignalDeferred.push(async function(OneSignal) {
     // Tag bazat pe pagina de opt-in
     await OneSignal.User.addTag("source", "betting-page");
     // Tag bazat pe GEO (detectat automat de OneSignal)
     // Tag bazat pe device (detectat automat)
   });
   ```
2. **Segmente recomandate**:
   - **Per sursă**: homepage, betting page, casino page, blog, landing page
   - **Per GEO**: Romania, Peru, Bolivia, Kenya, Chile (OneSignal detectează automat)
   - **Per device**: mobile, desktop, tablet (automat)
   - **Per activitate**: nou (<7 zile), activ (click în ultimele 30 zile), dormant (>30 zile fără click)
   - **Per engagement tier**: high (>3 click-uri/lună), medium (1-3), low (0)
3. **Segmentare comportamentală** (avansat):
   - Vizitat pagina de depunere → tag: `intent=deposit` → push cu bonus depunere
   - Vizitat 3+ pagini de slot-uri → tag: `interest=slots` → push cu free spins
   - Implementare prin JavaScript SDK: `OneSignal.User.addTag("interest", "slots")`
4. **Segmentare per canal de achiziție**:
   - Organic search → tag: `acquisition=organic`
   - Paid traffic (Facebook, Google) → tag: `acquisition=paid_{source}`
   - Referral → tag: `acquisition=referral_{site}`
5. **Valoare segment**: segmentele targetate generează 2-4x CTR vs. broadcast nediscriminatoriu

### Pas 6: Monetizarea bazei de subscribers (publisher model)
1. **Monetizare directă** — trimite propriile push-uri promoționale:
   - CTR pe lista proprie: 2-8% (vs. 0.1-0.5% pe ad-network)
   - Monetizare: affiliate offers (betting, casino, e-commerce)
   - Frecvență recomandată: 1-2 push-uri/zi, max 3/zi pentru oferte foarte relevante
2. **Monetizare prin ad network** (hands-off):
   - Instalează **Monetag** / **RollerAds** pe site → ei trimit ads la subscribers-ii tăi
   - Revenue: $0.50-3.00 RPM (per mie impresii), variabil per GEO
   - GEO profitabile: US/UK $2-5 RPM, EU $1-3, LATAM $0.5-1.5, Africa $0.3-1
3. **Model hibrid** (maxim revenue):
   - 70% din push-uri = propriile oferte (control, mai profitabil pe termen lung)
   - 30% = monetizare prin Monetag/RollerAds (venit pasiv pe subscribers care nu click-uiesc pe al tău)
4. **Revenue per subscriber per lună** (benchmarks):
   - GEO Tier 1: $0.05-0.15/subscriber/lună
   - GEO Tier 2: $0.02-0.08/subscriber/lună
   - GEO Tier 3: $0.01-0.04/subscriber/lună
   - Bază de 100K subscribers Tier 2 ≈ $2,000-8,000/lună
5. **Subscriber lifetime**: abonatul mediu rămâne activ 60-120 zile înainte de a dezactiva sau ignora push-urile

### Pas 7: Conformitate GDPR/ePrivacy și gestionarea dezabonărilor
1. **GDPR (UE + UK)** — obligatoriu dacă ai subscribers din EU:
   - Push opt-in = consimțământ explicit (browserul cere permisiunea = dovadă automată)
   - Actualizează **Privacy Policy** cu secțiune despre push notifications
   - Menționează: ce date colectezi, scopul, cum se dezabonează
2. **ePrivacy Directive** (Article 5(3)) — cookie/tracking consent:
   - Service worker-ul push = procesare date → include în cookie banner
   - Informează user-ul ÎNAINTE de prompt: "Folosim notificări push pentru oferte. Datele sunt prelucrate conform GDPR."
3. **Dezabonare facilă**:
   - Pagină dedicată de dezabonare cu instrucțiuni pas-cu-pas per browser
   - Link în fiecare push notification: "Dezabonare: site.ro/unsub"
   - OneSignal: activează "Subscription Page" personalizată
4. **Sunset policy** (igienă listă):
   - Subscribers inactivi >90 zile → trimite push de re-engagement: "Ne lipsești! Iată o ofertă specială"
   - Dacă nu click-uiesc nici pe re-engagement → marchează ca dormant
   - Subscribers inactivi >180 zile → exclude din campanii (economisești impresii, crești CTR mediu)
5. **Monitorizare opt-out rate**:
   - Normal: 0.5-2% per campanie
   - Dacă >3% per campanie → frecvența e prea mare sau conținutul e irelevant
   - Acțiune: reduce frecvența și segmentează mai bine

### Pas 8: Creștere accelerată a bazei — acquisition channels
1. **Paid traffic → push subscribers** (cel mai scalabil):
   - Rulează campanii Facebook/Google Ads cu landing page optimizat pentru push opt-in
   - Cost per subscriber: $0.01-0.10 (LATAM/Africa), $0.10-0.50 (EU/US)
   - ROI calculation: dacă subscriber = $0.05 achiziție și generează $0.08/lună pe 3 luni = $0.24 lifetime value → 4.8x ROI
2. **SEO content** (cel mai ieftin pe termen lung):
   - Creează conținut relevant (predicții sportive, recenzii casino, ghiduri)
   - Push opt-in overlay pe fiecare articol
   - Cost per subscriber: ~$0 (doar costul content creation)
3. **Cross-promotion** (list swap):
   - Parteneriat cu site-uri complementare: "Abonații noștri primesc push-uri de la partener și vice versa"
   - Atenție la GDPR: consimțământul trebuie să fie specific, nu general
4. **Social media organic**:
   - CTA în bio: "Activează notificările pentru oferte exclusive: [link]"
   - Stories cu swipe-up către pagina de opt-in
5. **Obiective de creștere realiste**:
   - Luna 1: 0 → 1,000 subscribers (testare, optimizare opt-in)
   - Luna 3: 5,000-10,000 subscribers (paid acquisition + organic)
   - Luna 6: 20,000-50,000 subscribers (scale-up)
   - Luna 12: 100,000+ subscribers (full monetization operational)

### Pas 9: Monitorizarea KPI-urilor și optimizare continuă
1. **KPIs zilnice** (primele 30 zile):
   - Noi subscribers/zi (trend)
   - Opt-in rate per pagină (care pagini convertesc cel mai bine?)
   - Opt-out rate per campanie push
2. **KPIs săptămânale**:
   - Total subscribers activi vs. dormanți
   - CTR mediu pe push-urile trimise
   - Revenue per subscriber (dacă monetizezi)
3. **KPIs lunare** (raport):
   - Net subscriber growth: new - churned
   - Subscriber lifetime value (LTV)
   - Cost per acquisition (CPA) per channel
   - Revenue per subscriber per lună
   - Heatmap: care ore/zile generează cel mai bun CTR
4. **Optimizare pe baza datelor**:
   - Opt-in rate <5% → schimbă mesajul, adaugă incentive, testează delay
   - CTR <1% pe lista proprie → segmentează mai bine, personalizează mai mult
   - Opt-out rate >3% → reduce frecvența, filtrează conținutul

## Verificare
- [ ] Platformă push aleasă și cont creat (OneSignal recomandat)
- [ ] SDK/plugin instalat corect pe site (HTTPS obligatoriu)
- [ ] Service worker uploadat în root-ul site-ului
- [ ] Custom slidedown configurat cu delay (min 15s sau 2 page views)
- [ ] Welcome notification configurată (auto, la 5 min post-opt-in)
- [ ] Minim 4 segmente create (sursa, GEO, activitate, interes)
- [ ] A/B test activ pe mesajul/design-ul de opt-in
- [ ] Privacy Policy actualizată cu secțiunea push notifications
- [ ] Sunset policy definită (re-engagement la 90 zile, exclude la 180)
- [ ] Monetizare configurată (propriile push-uri + Monetag/RollerAds)
- [ ] Dashboard monitorizat zilnic (primele 30 zile) apoi săptămânal
- [ ] Obiective de creștere lunare setate și tracked

## Instrumente
- **OneSignal**: onesignal.com — #1 push platform, plan gratuit generos, segmentare avansată
- **PushEngage**: pushengage.com — bun pentru e-commerce cu drip campaigns
- **WebPushr**: webpushr.com — simplu, gratuit 10K, fără branding
- **iZooto**: izooto.com — monetizare directă prin push ads
- **Gravitec**: gravitec.net — automatizare puternică, content sites
- **Monetag**: monetag.com — monetizare push subscribers prin ad network (PropellerAds)
- **RollerAds**: rollerads.com — alternativă monetizare push
- **Google Tag Manager**: pentru declanșarea evenimentelor de opt-in
- **Hotjar**: heatmaps + session recordings pentru analiza comportamentului pre-opt-in

## Note
- Rata medie de opt-in: 3-7% native prompt → 12-25% custom slidedown → 25-40% cu incentive
- Push subscribers sunt mai valoroși decât social media followers: reach 100% vs. 2-5% organic
- iOS Safari suportă push din iOS 16.4+ (PWA required) — dar adoptarea e lentă, <5% din iOS users
- Un subscriber pe lista proprie = 10-50x mai valoros decât o impresie pe ad network (CTR 5-8% vs 0.1-0.3%)
- Segmentarea de la momentul opt-in multiplică CTR-ul cu 2-4x față de broadcast
- Greșeala #1: opt-in agresiv (popup instant, fără delay) → user-ul dă Block → l-ai pierdut PENTRU TOTDEAUNA
- Greșeala #2: nu trimite welcome push → pierzi momentul de maxim engagement (primele 5 min)
- Greșeala #3: broadcast la toată lista fără segmentare → opt-out rate mare, CTR mic
- Subscriber base de 100K pe GEO Tier 2 = asset valoros ($2K-8K/lună monetizare)
