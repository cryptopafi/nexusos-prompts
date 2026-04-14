---
id: MPM-006
title: WhatsApp Business API Automation Setup
domain: mobile-push-marketing
source: Automate WhatsApp & SMS Marketing (SMSrush) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# WhatsApp Business API Automation Setup

## Obiectiv
Configurează WhatsApp Business API (prin BSP oficial sau wrapper) pentru trimiterea de campanii bulk, automatizarea răspunsurilor și conversii pe verticalele betting/casino — respectând politicile stricte Meta, regulile de opt-in WhatsApp și gestionând riscul de ban, cu focus pe GEO-uri LATAM/Africa unde WhatsApp e canalul #1 de messaging.

## Pași

### Pas 1: Alegerea furnizorului WhatsApp Business API — BSP oficial vs. wrapper
1. **BSP Oficial (Business Solution Provider)** — recomandat pe termen lung:
   - **360dialog** (360dialog.com): BSP oficial Meta, cel mai ieftin ($0.005-0.08/mesaj per GEO), full API access, template approval direct
   - **Twilio** (twilio.com): API puternic, integrat cu SMS, $0.005/mesaj + Meta pricing per GEO. Sandbox gratuit pentru dev
   - **MessageBird (Bird)** (messagebird.com): enterprise, multi-channel, deliverability premium
   - **WATI** (wati.io): CRM + WhatsApp, interfață no-code, bun pentru customer support + marketing. $49/lună
   - **Respond.io** (respond.io): omnichannel inbox, chatbot builder. $79/lună
   - **Gupshup** (gupshup.io): BSP oficial, focus LATAM/India, pricing competitiv
2. **Wrapper neoficial** (risc mai mare, start rapid):
   - **SMSrush** (smsrush.com): bulk WhatsApp prin wrapper, nu necesită Meta Business verified. RISC: ban număr
   - **WAppSender** și alte tools: grey area, funcționează dar Meta blochează activ
   - **WARNING**: Meta banează agresiv conturile care trimit bulk fără BSP. Numărul personal = risc de ban permanent
3. **Recomandare per situație**:
   - Start rapid + test: SMSrush wrapper (acceptă riscul de ban pe număr de test)
   - Scale + compliance: 360dialog sau Twilio (BSP oficial, Meta approved)
   - CRM + automation: WATI sau Respond.io
4. **WhatsApp penetration per GEO SMSads** (de ce contează):
   - Bolivia: 89% din population, #1 messaging app
   - Peru: 92%, #1
   - Kenya: 67% (dar crește rapid, concurență de la Telegram)
   - Chile: 94%, #1
   - Concluzie: WhatsApp e CRITICAL pentru LATAM, important pentru Kenya

### Pas 2: Setup cont WhatsApp Business API (BSP oficial — 360dialog/Twilio)
1. **Cerințe Meta**:
   - **Meta Business Account** verificat (business.facebook.com)
   - **Număr de telefon** dedicat (nu poate fi folosit pe WhatsApp personal simultan)
   - **Business Name** aprobat (Meta verifică identitatea)
   - **Display Name**: max 20 caractere, trebuie să reflecte business-ul real
2. **Setup pas cu pas (360dialog)**:
   - Creează cont pe 360dialog.com → Connect Meta Business Account
   - Adaugă număr de telefon → verificare prin SMS sau call
   - Submit business verification documents (CUI/registration, site web)
   - Așteaptă aprobare Meta: 1-7 zile (media: 2-3 zile)
   - Post-aprobare: primești API endpoint + API key
3. **Setup pas cu pas (Twilio)**:
   - Cont Twilio → Messaging → WhatsApp → Request access
   - Conectare Meta Business Account (OAuth flow)
   - Adaugă Twilio Sender (număr dedicat)
   - Sandbox disponibil IMEDIAT pentru development (join sandbox number)
4. **Limite mesaje (tiers Meta)**:
   - Tier 1 (cont nou): 1,000 conversații/24h
   - Tier 2: 10,000 conversații/24h (atins după quality rating bun pe 7 zile)
   - Tier 3: 100,000 conversații/24h
   - Tier 4 (Unlimited): 1M+/24h
   - **Upgrade automat**: Meta upgradează tier-ul automat bazat pe volume + quality score

### Pas 3: Template Messages — creare și aprobare Meta
1. **Ce sunt Template Messages**: mesaje pre-aprobate de Meta, singurele permise pentru outbound marketing (outside 24h customer service window)
2. **Categorii template**:
   - **Marketing**: promoții, oferte, anunțuri. CEL MAI STRICT review, mai scump
   - **Utility**: confirmări, actualizări cont, reminders. Review moderat
   - **Authentication**: OTP, verificare. Review rapid, cel mai ieftin
3. **Creare template betting/casino** (atenție — gambling e "sensitive category" la Meta):
   - Template name: `welcome_bonus_es` (lowercase, underscores)
   - Limbă: Spanish (Peru/Bolivia/Chile) sau English (Kenya)
   - Header: Image (banner bonus) sau Text ("Oferta Especial")
   - Body (cu variabile):
     ```
     Hola {{1}}! 🎰

     Tienes un bono de bienvenida de {{2}}% esperándote.

     Regístrate ahora y recibe hasta ${{3}} en tu primera recarga.

     👉 {{4}}

     *Aplican T&C. Solo mayores de 18 años.
     ```
   - Footer: "Responde STOP para desuscribirte"
   - Buttons: Quick Reply ("Me interesa" / "No, gracias") sau CTA URL
4. **Tips aprobarea templates**:
   - NU folosi cuvinte explicit gambling ("apostar", "casino", "gambling") dacă nu ești verificat ca gambling business
   - Folosește limbaj indirect: "bonus", "oferta", "juega gratis", "registrate"
   - Include T&C și disclaimer 18+
   - Include opt-out clar
   - Response rate la Meta review: 24-72h
   - Dacă rejected → modifică și re-submit (Meta dă motivul rejecției)
5. **Template pricing per GEO** (Marketing category, 2026):
   | GEO | Cost per conversation |
   |---|---|
   | Bolivia | ~$0.04 |
   | Peru | ~$0.05 |
   | Kenya | ~$0.03 |
   | Chile | ~$0.06 |

### Pas 4: Import contacte, segmentare și compliance WhatsApp
1. **Reguli stricte Meta pentru contacte**:
   - DOAR contacte cu opt-in explicit pentru WhatsApp (separat de opt-in SMS!)
   - Contactul trebuie să fi dat consimțământ SPECIFIC pentru a primi mesaje WhatsApp de la business-ul tău
   - Dovada opt-in trebuie stocată (timestamp, sursa, text consimțământ)
   - Meta poate cere dovada — dacă nu ai, contul e suspendat
2. **Surse valide de opt-in WhatsApp**:
   - Click-to-WhatsApp Ads (Facebook/Instagram) — opt-in implicit prin inițierea conversației
   - Web form cu checkbox specific: "Sunt de acord să primesc mesaje WhatsApp de la [Brand]"
   - Keyword opt-in: user trimite "START" sau "BONUS" la numărul tău WhatsApp
   - QR code pe materiale fizice → scanare = opt-in
3. **Import în platformă** (360dialog/WATI):
   - CSV format: `phone,name,segment,opt_in_date,source`
   - Numere în format internațional: +51xxxxxxxxx
   - Tag-uri per segment: fresh_lead, registered, active, dormant
4. **Quality Score Meta** (CRITIC — determină dacă poți trimite):
   - Green (High): delivery rate bună, opt-out rate mică → poți scala
   - Yellow (Medium): warning, reduce volumul
   - Red (Low): Meta reduce limita de mesaje sau suspendă contul
   - Factori: block rate, report spam rate, template quality
5. **Regula de aur**: mai bine 5,000 mesaje cu quality green decât 50,000 cu quality red → red = ban

### Pas 5: Design campanie WhatsApp și personalizare mesaje
1. **Format mesaj WhatsApp** (mai bogat decât SMS):
   - Text simplu: max 4,096 caractere — dar keep it short (200-500 chars optimal)
   - Imagine + caption: 1 imagine (JPEG/PNG, max 5MB) + text
   - Video: max 16MB (MP4) — bun pentru demo jocuri/highlights sportive
   - Document: PDF (T&C, bonus details)
   - Location: pin pe hartă (pentru betting shops fizice)
   - Interactive buttons: Quick Reply (max 3) sau CTA URL (max 2)
2. **Template campanie betting (exemplu complet)**:
   ```
   Header: [Image: banner cu bonus 200%]

   Body:
   ¡Hola {{first_name}}! 🏆

   Esta semana tenemos algo especial para ti:

   ✅ Bono {{bonus_percent}}% en tu próximo depósito
   ✅ Hasta ${{max_bonus}} de bono
   ✅ {{free_spins}} giros gratis en {{game_name}}

   ¿Listo para jugar? 👇

   Footer: Aplican T&C. +18. Responde STOP para desuscribirte.

   Buttons:
   [CTA URL: "Depositar Ahora" → https://smsads.com/deposit?ref={{ref_id}}]
   [Quick Reply: "Más Info"]
   [Quick Reply: "No, Gracias"]
   ```
3. **Spintax pentru WhatsApp** (dacă folosești wrapper/SMSrush):
   ```
   {Hola|Hey|Saludos} {{name}}!
   {Tenemos|Preparamos|Hay} una {oferta especial|sorpresa|promocion} para ti.
   {Deposita ahora|Registrate|Empieza a jugar}: {{link}}
   ```
4. **Rich media best practices**:
   - Imagine: 800x418px (landscape) sau 800x800px (square). <300KB pentru load rapid
   - Video: 15-30 secunde, subtitrate (multi useri sunt pe mute)
   - Butoane interactive cresc CTR cu 30-50% vs. text-only messages

### Pas 6: Chatbot și auto-reply automation
1. **Auto-reply rules** (platformă / WATI / SMSrush):
   - Keyword: "BONUS" / "BONO" → Reply: template cu detalii bonus + link înregistrare
   - Keyword: "STOP" / "PARA" → Reply: "Has sido desuscrito. No recibirás más mensajes." + remove din listă
   - Keyword: "AYUDA" / "HELP" → Reply: "Contacta nuestro soporte: [email] o escribe tu pregunta aquí."
   - Default (keyword necunoscut): "Escribe BONUS para ofertas o STOP para desuscribirte."
2. **Conversation flow betting/casino** (chatbot):
   ```
   User: [Orice mesaj sau click pe ad]
   Bot: "¡Hola! 👋 ¿Qué te interesa?
         1️⃣ Bono de Bienvenida
         2️⃣ Apuestas Deportivas
         3️⃣ Casino / Tragamonedas
         4️⃣ Soporte"

   User: "1"
   Bot: "🎉 ¡Excelente! Tu bono de bienvenida es de 200% hasta $500.
         Regístrate aquí: [link]
         ¿Necesitas ayuda con el registro?"

   User: "sí"
   Bot: "Perfecto. Sigue estos pasos:
         1. Ingresa al link
         2. Completa el formulario (30 seg)
         3. Deposita mínimo $10
         4. Tu bono se acredita automáticamente
         ¿Listo? 👉 [link]"
   ```
3. **Business hours auto-reply**: "Gracias por escribirnos. Nuestro horario es L-V 9:00-21:00. Te responderemos mañana. Mientras tanto, visita: [link]"
4. **Escalation to human**: dacă user-ul scrie 3+ mesaje fără rezolvare → notificare la agent uman
5. **Tools pentru chatbot**:
   - WATI: flow builder visual (drag & drop)
   - Respond.io: omnichannel chatbot
   - Twilio Studio: flow builder programmatic
   - ManyChat: WhatsApp chatbot (în beta, expanding)

### Pas 7: Lansarea campaniei bulk WhatsApp
1. **Pre-launch checklist**:
   - [ ] Template aprobat de Meta (status: APPROVED)
   - [ ] Lista importată cu opt-in valid
   - [ ] Variabile personalizate mapate corect
   - [ ] Auto-reply STOP configurate
   - [ ] Quality Score = Green
2. **Send speed management** (CRUCIAL — Meta monitorizează):
   - BSP oficial: platformele gestionează throttling automat
   - Wrapper (SMSrush): MAXIM 50 mesaje/minut. Peste 50 = risc ban
   - Recomandare start: 20-30 msg/minut → crește treptat pe 3-5 zile
3. **Scheduling**:
   - Bolivia/Peru/Chile: 10:00-12:00 sau 18:00-21:00 (local time)
   - Kenya: 10:00-12:00 sau 17:00-20:00 EAT
   - WhatsApp seara: CTR +15-25% vs. dimineața (useri relaxați, scrolling)
4. **Launch** → monitorizare primele 60 min:
   - Mesaje livrate ✓ (single check mark)
   - Mesaje citite ✓✓ albastru (double blue check)
   - Răspunsuri primite (chatbot funcționează?)
   - Block/Report rate (dacă crește → STOP imediat)

### Pas 8: Monetizare și conversie — funnel WhatsApp betting/casino
1. **WhatsApp funnel stages**:
   ```
   Click-to-WhatsApp Ad → Welcome Message (chatbot) → Bonus Offer →
   Registration Link → Deposit Reminder (24h) → Active Player Nurture
   ```
2. **Click-to-WhatsApp Ads** (cel mai eficient acquisition channel):
   - Facebook/Instagram ads cu buton "Send WhatsApp Message"
   - User-ul inițiază conversația (= opt-in automat, Meta compliant!)
   - Cost per conversation start: $0.10-0.50 (LATAM), $0.05-0.20 (Kenya)
   - Conversion rate conversation → registration: 20-40%
3. **24h service window** (gratuit):
   - După ce user-ul trimite un mesaj, ai 24h să răspunzi GRATUIT (fără cost template)
   - Profită de fereastra: trimite bonus info, ajută la registration, push deposit
   - După 24h: trebuie template message (plătit)
4. **Drip sequence post-registration**:
   - 0h: "Felicitaciones! Tu bono de 200% está listo. Deposita ahora: [link]"
   - 24h (dacă no deposit): "{{name}}, tu bono expira en 48h. No lo pierdas: [link]"
   - 72h: "Último recordatorio: tu bono de ${{amount}} te espera. [link]"
   - 7 zile (dacă deposit făcut): "Gracias por jugar! Aquí tienes 25 giros gratis: [link]"
5. **KPIs funnel WhatsApp**:
   - Open rate: 80-95% (cel mai mare din orice canal)
   - CTR pe link: 15-40%
   - Registration rate: 20-40% din conversations
   - FTD rate: 15-30% din registrations
   - Cost per FTD via WhatsApp: $5-20 (LATAM), $3-10 (Kenya) — foarte competitiv vs. SMS/push

### Pas 9: Risk management — evitarea ban-ului WhatsApp
1. **Cauze frecvente de ban**:
   - Trimitere la contacte fără opt-in (cel mai comun)
   - Block rate >5% din mesaje trimise
   - Conținut explicit gambling fără business verification
   - Send speed prea mare pe wrapper (>100 msg/min pe SMSrush)
   - Template-uri rejcected trimise manual (bypass review)
2. **Prevenție**:
   - MEREU opt-in valid înainte de trimitere
   - Quality Score monitorizat ZILNIC — dacă devine Yellow → reduce volum 50%
   - Include "Responde STOP" în fiecare mesaj → opt-out ușor reduce report rate
   - Nu trimite la contacte care nu au răspuns la ultimele 3 mesaje (cold contacts)
   - Sender warming: cont nou → 100 msg/zi → 500 → 2,000 → 10,000 (escalare pe 2-3 săpt)
3. **Backup plan** (dacă numărul e banat):
   - Ai mereu un al doilea număr WhatsApp Business pregătit (diferit BSP)
   - Datele de contacte sunt pe platformă, nu pe telefon → poți conecta număr nou
   - Appeal ban: Meta Business Help Center → formular de apel (success rate: 20-30%)
4. **BSP oficial vs. wrapper risk comparison**:
   | Factor | BSP Oficial (360dialog) | Wrapper (SMSrush) |
   |---|---|---|
   | Ban risk | Low (Meta compliant) | High (detected & blocked) |
   | Template review | Required (1-72h) | Not required |
   | Send speed | Managed by platform | Manual throttle |
   | Cost | Higher ($0.03-0.08/msg) | Lower ($0.01-0.03/msg) |
   | Long-term viability | Sustainable | Risky |

### Pas 10: Analytics și optimizare continuă
1. **Metrici WhatsApp**:
   - **Sent**: mesaje trimise (1 check)
   - **Delivered**: livrate (2 checks)
   - **Read**: citite (2 blue checks) — target: >70%
   - **Replied**: răspunsuri primite — target: >5-10%
   - **Link CTR**: clicks pe link — target: 15-30%
   - **Opt-out rate**: STOP-uri — target: <1%
   - **Block rate**: useri care te-au blocat — target: <2%
2. **A/B testing templates**:
   - Testează: mesaj text vs. imagine + text (imagine câștigă cu 20-30% CTR)
   - Testează: Quick Reply buttons vs. CTA URL (Quick Reply = mai mult engagement, CTA URL = mai multe click-uri)
   - Testează: tone formal vs. informal (LATAM: informal câștigă)
3. **Raportare lunară**:
   - Total conversations inițiate
   - Cost per conversation
   - Registration rate din conversations
   - FTD rate
   - Revenue atribuit WhatsApp
   - Quality Score trend
   - Comparație cu SMS și Push (cost per FTD cross-channel)

## Verificare
- [ ] BSP oficial ales (360dialog/Twilio) sau wrapper (SMSrush) cu risc acceptat
- [ ] Meta Business Account verificat (dacă BSP oficial)
- [ ] Număr WhatsApp Business dedicat conectat
- [ ] Template messages create și aprobate de Meta
- [ ] Lista de contacte cu opt-in WhatsApp specific valid
- [ ] Auto-reply configurate (BONUS, STOP, HELP, default)
- [ ] Chatbot flow funcțional (testat end-to-end)
- [ ] Send speed configurat (max 50 msg/min wrapper, automat pe BSP)
- [ ] Quality Score monitorizat (target: Green)
- [ ] Backup număr WhatsApp pregătit (al doilea BSP)
- [ ] Drip sequence post-registration configurată
- [ ] Analytics tracked: read rate, CTR, opt-out rate, block rate

## Instrumente
- **360dialog**: 360dialog.com — BSP oficial Meta, cel mai ieftin, full API
- **Twilio**: twilio.com — WhatsApp API + SMS, sandbox gratuit
- **WATI**: wati.io — WhatsApp CRM + chatbot visual builder ($49/mo)
- **Respond.io**: respond.io — omnichannel messaging hub + automation
- **Gupshup**: gupshup.io — BSP oficial, focus LATAM/India
- **SMSrush**: smsrush.com — wrapper bulk WhatsApp + SMS (risc ban)
- **ManyChat**: manychat.com — WhatsApp chatbot (expanding features)
- **Meta Business Suite**: business.facebook.com — Click-to-WhatsApp Ads setup

## Note
- WhatsApp = canalul #1 în LATAM (90%+ penetration) — OBLIGATORIU pentru SMSads Bolivia/Peru/Chile
- Open rate WhatsApp: 80-95% (vs. 98% SMS dar cu read tracking real, vs. 20% email)
- CTR WhatsApp: 15-40% (vs. 8-15% SMS betting, vs. 0.1-0.5% push ad-network) — cel mai performant canal
- Meta e FOARTE strict pe gambling content — business verification obligatorie pentru scale
- Wrapper (SMSrush) = start rapid + cheap dar risc ban. BSP = mai scump dar sustainable
- Cost per FTD via WhatsApp LATAM: $5-20 — competitiv cu orice alt canal
- Click-to-WhatsApp Ads = cel mai eficient acquisition method (opt-in implicit, cost mic)
- Greșeala #1: trimite bulk pe număr personal → ban permanent în ore
- Greșeala #2: ignori Quality Score → Meta reduce limite → nu mai poți trimite deloc
- Greșeala #3: nu ai backup number → un ban = pierderea completă a canalului
- WhatsApp + SMS combo: WhatsApp pentru engagement + conversie, SMS pentru reach (99% mobile coverage vs. 67-94% WhatsApp)
