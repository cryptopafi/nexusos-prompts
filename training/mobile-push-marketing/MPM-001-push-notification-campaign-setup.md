---
id: MPM-001
title: Push Notification Campaign Setup (PropellerAds/Evadav)
domain: mobile-push-marketing
source: The Complete Push Notification Course (Udemy) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# Push Notification Campaign Setup (PropellerAds/Evadav)

## Obiectiv
Configurează o campanie push notification profitabilă de la zero pe ad-networks (PropellerAds, Evadav, RichAds, Zeropark, Pushground, Mondiad), de la crearea contului și structura campaniei până la lansare cu targeting precis, buget controlat și tracking complet — cu focus pe verticalele betting/casino și GEO-uri LATAM/Africa.

## Pași

### Pas 1: Alegerea platformei și crearea contului advertiser
1. **Evaluare platforme per vertical și GEO**:
   - **PropellerAds** (propellerads.com): cel mai mare volum global, excelent pe Tier 2-3 (Bolivia, Peru, Kenya, Chile). SmartCPA automat. Minimum deposit $100. Best for: betting/casino/sweepstakes
   - **Evadav** (evadav.com): trafic push foarte bun pe Tier 2-3, interface simplu, bun pe adult+betting. Minimum $100
   - **RichAds** (richads.com): premium push cu AI optimization, micro-bidding per zone. Minimum $150. Best for: gambling vertical cu volume medii
   - **Zeropark** (zeropark.com): performance-focused, rule-based automation, excelent pe pop+push combo. Minimum $200
   - **Pushground** (pushground.com): self-serve, transparent pricing, ideal testing. Minimum $50
   - **Mondiad** (mondiad.com): push + native, bun pe LATAM. Minimum $100
2. Înregistrează-te ca **Advertiser** (nu Publisher) — folosește date reale de business (facturare, VAT dacă aplicabil)
3. Adaugă metodă de plată: card de credit/debit, PayPal, Wire Transfer (pentru volume >$5K), crypto (PropellerAds, RichAds acceptă BTC/USDT)
4. Activează 2FA pe cont — obligatoriu, conturile fără 2FA sunt primele targetate de fraud
5. Notează **Account ID** și **API Key** (necesar pentru automatizări și bid scripts ulterioare)
6. **Pro tip**: creează conturi pe 2-3 platforme simultan — vei diversifica traficul și vei compara CPM/CPC per GEO

### Pas 2: Structurarea campaniei și alegerea formatului push
1. **Formate push disponibile**:
   - **Classic Push** (WebPush): funcționează pe desktop + Android. NU pe iOS Safari. CTR mediu: 0.1-0.5%. Cel mai mare volum disponibil
   - **In-Page Push (IPP)**: banner-style push care apare pe pagină, NU necesită opt-in. Funcționează pe iOS! CTR mediu: 0.05-0.3%. Volum mare, calitate mai scăzută
   - **Calendar Push** (iOS calendar events): nișă, funcționează doar pe iOS. CTR: 0.3-1%. Volum mic dar calitate bună
2. **Convenție de naming** (obligatoriu pentru organizare la scale):
   ```
   [CLIENT]-[GEO]-[VERTICAL]-[FORMAT]-[DATA]-[VARIANTA]
   Ex: SMSADS-PE-BETTING-PUSH-20260320-v1
   Ex: SMSADS-KE-CASINO-IPP-20260320-v2
   ```
3. **Model de pricing** — alege strategic:
   - **CPC** (Cost Per Click): recomandat la start, plătești doar pentru click-uri reale. PropellerAds: $0.003-0.05 CPC pe Tier 3
   - **CPM** (Cost Per Mille): bun când ai CTR ridicat confirmat (>0.3%), mai ieftin per click la volume
   - **SmartCPC** (PropellerAds): algoritm care optimizează CPC automat — activează după 500+ click-uri
   - **Target CPA** (PropellerAds SmartCPA): setezi CPA dorit, algoritmul optimizează automat. Activează DOAR după 50+ conversii pe campanie
4. **Buget de testare**:
   - GEO Tier 3 (Bolivia, Kenya): $20-30/zi este suficient pentru date statistice
   - GEO Tier 2 (Peru, Chile, Romania): $30-50/zi
   - GEO Tier 1 (US, UK, DE): $50-100/zi minimum
   - Setează budget daily + budget total (safety cap)

### Pas 3: Configurarea targeting-ului granular
1. **GEO targeting** — setează per țară (nu per regiune, la start):
   - SMSads focus: Bolivia (BO), Peru (PE), Kenya (KE), Chile (CL)
   - Testează fiecare GEO separat (campanii separate per țară) — nu combina GEO-uri în aceeași campanie, distorsionează datele
2. **Device targeting**:
   - Mobile: 70-85% din traficul push — prioritar pentru betting/casino
   - Desktop: 15-30%, CTR mai mare dar volum mai mic
   - Tablet: volum neglijabil, exclude sau include cu mobile
3. **OS targeting** (crucial pentru push):
   - **Android**: cel mai mare volum push clasic, Chrome dominant. Versiuni: Android 8+ (sub 8 = dispozitive vechi, conversie slabă)
   - **Windows**: al doilea volum, bun pentru desktop push. Win 10+ recomandat
   - **iOS**: DOAR prin In-Page Push sau Calendar Push (Safari nu suportă clasic push)
   - **macOS**: volum mic, CTR bun. Sonoma+ recomandat
4. **Browser targeting**: Chrome (80%+ volum), Firefox, Edge, Opera. La start lasă "All browsers"
5. **Frequency capping** (CRITIC pentru profitabilitate):
   - Campanie nouă: **1 impresie/user/24h** — audiență proaspătă, CTR maxim
   - Campanie matură cu date: poți crește la 2-3/user/24h dacă CTR rămâne stabil
   - Niciodată >3/zi — creative fatigue distruge CTR-ul
6. **Connection type**: WiFi only vs. Carrier (3G/4G). Betting/casino: WiFi preferred (user la relaxare, conversie mai bună)
7. **ISP targeting** (avansat): pe unele platforme poți targeta per carrier — util pentru oferte mobile operator-specific
8. **User activity targeting** (PropellerAds specific):
   - High activity: useri care click-uiesc frecvent pe push-uri. CPC mai mare, CTR mai bun
   - Medium activity: balanță bună cost/calitate — START HERE
   - Low activity: CPC cel mai mic, CTR foarte scăzut (0.01-0.05%). Bun doar pentru volum mare cu oferte simple

### Pas 4: Crearea creative-urilor (titlu + descriere + imagini)
1. Pregătește minim **5 variante de creative** pentru A/B testing automat al platformei
2. **Titlu** (max 30 caractere): include beneficiul principal sau urgența
   - Betting: "⚽ Pariază azi — Bonus 100%" | "🎰 50 Rotiri Gratuite Acum"
   - Casino: "🃏 Câștigă €500 — Înscrie-te!" | "🎲 Bonus Fără Depunere"
3. **Descriere** (max 40 caractere): call-to-action clar
   - "Înregistrare în 30 sec" | "Bonus instant la primul depozit" | "Joacă acum →"
4. **Iconiță** (192x192px, PNG/JPG):
   - Varianta A: Logo brand ofertă
   - Varianta B: Emoji mare relevant (🎰, ⚽, 💰)
   - Varianta C: Cifră mare pe fundal contrastant ("100%", "€500")
5. **Banner/Hero Image** (492x328px): imagine vizuală atractivă
   - Fundal gradient cu text overlay minimal
   - Produs/serviciu vizibil clar
   - NU text supraîncărcat (max 3-4 cuvinte pe imagine)
6. Upload variante → platforma rotește și optimizează automat (Auto-Optimization)
7. **URL destinație** cu parametri de tracking:
   ```
   https://offer.com/landing?sub1={zoneid}&sub2={clickid}&sub3={campaignid}&sub4={creativeid}&sub5={country}
   ```
   - `{zoneid}` = publisher site-ul (pentru whitelist/blacklist)
   - `{clickid}` = ID unic click (pentru postback conversie)
   - `{creativeid}` = care creative a generat click-ul

### Pas 5: Configurarea tracking-ului și pixelilor de conversie
1. **Tracker extern** (recomandat pentru control complet):
   - **Voluum** ($199/lună): best in class, real-time, AI optimization
   - **BeMob** (plan gratuit disponibil): bun pentru start, interface clean
   - **RedTrack** ($149/lună): bun pentru echipe, multi-user
   - **Binom** ($69/lună, self-hosted): cel mai rapid, control total date
2. **Setup tracking flow**:
   ```
   Push Ad → Tracker (Voluum/BeMob) → Landing Page → Offer Page → Conversion Pixel → Postback to Tracker → Postback to Ad Network
   ```
3. **Postback URL** — configurează în ad network pentru tracking conversii:
   - PropellerAds: Account → Postback → add tracker postback URL
   - Format tipic: `https://tracker.voluum.com/postback?clickid={clickid}&payout={payout}`
4. **Conversie de test**: efectuează o conversie test end-to-end și verifică:
   - Click înregistrat în tracker ✓
   - Redirect corect la landing page ✓
   - Conversie raportată înapoi la ad network ✓
5. **Fără tracker** (alternativă simplă): activează conversion pixel nativ al platformei
   - Inserează pixel pe thank you page
   - Configurează evenimentele: Registration, Deposit, Purchase

### Pas 6: Pre-launch checklist și lansare
1. **Revizuiește setările complet** (10 min de review salvează sute de $):
   - Budget daily și total corect?
   - GEO, device, OS conform planului?
   - Creative-uri uploadate cu titlu/desc/icon/banner?
   - URL destinație funcționează rapid (<3s load time)?
   - Tracking postback configurat?
   - Frequency capping setat?
2. **Verifică landing page**:
   - Se încarcă sub 3 secunde pe mobile (test cu Google PageSpeed)
   - Funcționează corect pe Android Chrome (dispozitivul dominant)
   - Link-urile de pe landing page duc corect la ofertă
   - Parametrii de tracking se pasează corect (verifică URL-ul final)
3. **Submit campanie** → intră în review:
   - PropellerAds: 0-4h (campaign review)
   - Evadav: 2-12h
   - RichAds: 1-6h
   - Zeropark: 0-2h
4. **IMPORTANT**: dacă campania e respinsă, citește motivul exact și corectează. Cele mai comune rejecții:
   - Creative misleading (promite câștiguri garantate)
   - Landing page non-compliant (lipsă Terms & Conditions)
   - Ofertă restricționată în GEO-ul ales

### Pas 7: Monitorizare primele 48h și reacție rapidă
1. **Primele 2-4h** — verifică datele brute:
   - Impresii vin? (dacă 0 impresii după 2h → bid prea mic sau targeting prea restrictiv)
   - CTR > 0.05%? Dacă nu → creative-urile sunt slabe, schimbă imediat
   - Click-uri ajung la tracker? (verifică discrepanțe tracker vs. ad network)
2. **Primele 24h** = faza de learning a algoritmului:
   - NU modifica setările brusc
   - NU crește/scade bid-ul cu mai mult de 20%
   - Lasă algoritmul să acumuleze date (minim 1,000 click-uri pentru decizii)
3. **La 48h** — prima optimizare:
   - Identifică **zone** (publisher sites) cu CTR mare dar 0 conversii → **blacklist** imediat
   - Identifică zone cu conversii profitabile → notează pentru viitoare **whitelist campaign**
   - Compară creative-urile: oprește pe cele cu CTR sub media campaniei
   - Verifică cost per conversie vs. payout ofertă — ești profitabil?

### Pas 8: Scalare sau Kill — decizia la 72h
1. **Dacă campania e profitabilă** (CPA < 70% din payout):
   - Crește bugetul zilnic cu 30-50% (nu dubla brusc — algoritmul pierde learning)
   - Creează **whitelist campaign** doar cu zonele profitabile → bid cu 20-30% mai mult
   - Extinde la GEO-uri similare (Peru profitabil → testează Chile, Bolivia)
   - Adaugă creative-uri fresh (3-5 noi) pentru a preveni fatigue
2. **Dacă campania e break-even** (CPA = 80-110% din payout):
   - Blacklistează top 10 zone cu cel mai mare cost fără conversii
   - Testează 5 creative-uri noi
   - Ajustează frequency capping (de la 2/zi la 1/zi)
   - Dă-i încă 48h cu optimizări
3. **Dacă campania pierde** (CPA > 130% din payout sau 0 conversii la 500+ click-uri):
   - **Kill campania** — nu arunca bani pe o campanie moartă
   - Analizează: era traficul relevant? Landing page funcționa? Oferta convertea pe alte surse?
   - Deschide campanie nouă cu angle diferit, creative-uri noi, sau GEO diferit
4. **Greșeala #1 a începătorilor**: nu au stop-loss mental. Setează ÎNAINTE de lansare: "Dacă cheltuiesc $X fără conversie, opresc." Rule of thumb: 2-3x payout-ul ofertei = budget maxim de test

### Pas 9: Optimizare continuă (săptămânal)
1. **Zone management** (cel mai impactful):
   - Exportă raportul per zone → sortează după cost
   - Blacklistează zonele cu cost > 2x CPA target și 0 conversii
   - Whitelist zonele cu conversii constante → campanie separată whitelist
2. **Creative refresh** (la fiecare 5-7 zile):
   - CTR-ul scade natural cu 15-30% pe săptămână pe aceleași creative-uri
   - Adaugă 3-5 variante noi, oprește cele mai slabe
   - Documentează creative-urile câștigătoare într-un **swipe file**
3. **Bid optimization**:
   - Crește bid pe zonele profitabile cu 10-15%
   - Scade bid pe campaniile cu CTR în decline
   - Testează modele de pricing diferite (CPC → CPM dacă CTR > 0.3%)
4. **Audience refinement**:
   - Dacă 80% din conversii vin de pe Android → exclude desktop
   - Dacă user activity Medium performează cel mai bine → exclude Low
   - Dacă Chrome domină → exclude browserele minore

## Verificare
- [ ] Conturi create pe minim 2 ad networks cu 2FA activat
- [ ] Convenție de naming aplicată (CLIENT-GEO-VERTICAL-FORMAT-DATA)
- [ ] Campanie creată cu CPC model și buget zilnic de test setat
- [ ] Targeting: GEO unic per campanie, device/OS/browser configurat
- [ ] Frequency capping: 1/user/24h la start
- [ ] Minim 5 creative variante uploadate (titlu + descriere + iconiță + banner)
- [ ] URL destinație cu parametri tracking complet ({zoneid}, {clickid}, {creativeid})
- [ ] Tracker extern configurat cu postback funcțional (test end-to-end PASS)
- [ ] Landing page se încarcă <3s pe mobile
- [ ] Campanie aprobată și în status ACTIVE
- [ ] Monitorizare la 2h, 24h, 48h programată
- [ ] Stop-loss definit înainte de lansare (max $X fără conversie)

## Instrumente
- **PropellerAds**: propellerads.com — cel mai mare volum, SmartCPA, bun pe Tier 2-3
- **Evadav**: evadav.com — push specializat, trafic Tier 2-3, interface simplu
- **RichAds**: richads.com — AI optimization, micro-bidding, premium push
- **Zeropark**: zeropark.com — rule-based automation, pop+push combo
- **Pushground**: pushground.com — self-serve transparent, ideal testing cu buget mic
- **Mondiad**: mondiad.com — push + native, bun pe LATAM
- **Voluum**: voluum.com — tracker #1 pentru affiliates
- **BeMob**: bemob.com — tracker gratuit pentru start
- **RedTrack**: redtrack.io — tracker cu collaboration features
- **Canva / Photopea**: design iconițe (192x192) și bannere (492x328)

## Note
- CTR benchmarks push clasic pe ad-networks: 0.05% (slab) → 0.15-0.3% (mediu) → 0.5-2% (excelent, whitelist campaigns)
- In-Page Push CTR: 0.03-0.15% — volum mare dar calitate mai scăzută
- CPC benchmarks Tier 3 (BO, KE): $0.003-0.01 | Tier 2 (PE, CL, RO): $0.01-0.03 | Tier 1 (US, UK): $0.03-0.10
- Betting/casino vertical: CPA-urile sunt mari ($20-100+ per FTD) dar și payouts sunt mari — ROI 50-200% e normal
- PropellerAds SmartCPA e magic DUPĂ 50+ conversii — nu-l activa înainte, algoritmul nu are date suficiente
- In-Page Push rezolvă problema iOS (Safari nu suportă push clasic) — crucial dacă GEO-ul tău are penetrare iPhone mare
- Niciodată nu lansa campanie fără tracker/pixel de conversie — altfel arunci bani orb
- Greșeală frecventă: combinarea mai multor GEO-uri într-o campanie. Costul per click diferă enorm între țări → un GEO ieftin mănâncă tot bugetul
- Rule of thumb buget de test: 5-10x CPA target per campanie. Dacă CPA target = $10, testează cu $50-100 înainte de a trage concluzii
