---
id: MPM-008
title: Message Personalization with Dynamic Tags & Spintax
domain: mobile-push-marketing
source: Automate WhatsApp & SMS Marketing (SMSrush) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# Message Personalization with Dynamic Tags & Spintax

## Obiectiv
Implementează personalizare avansată a mesajelor SMS, push și WhatsApp prin variabile dinamice (merge tags), Spintax (spin syntax) și personalizare comportamentală — pentru a crește CTR-ul cu 20-40%, evita filtrele anti-spam ale operatorilor și maximiza conversiile pe verticalele betting/casino în GEO-uri LATAM/Africa.

## Pași

### Pas 1: Variabilele dinamice — taxonomie completă
1. **Ce sunt variabilele dinamice**: placeholder-i în template care sunt înlocuiți cu date individuale per contact la trimitere
2. **Sintaxa per platformă**:
   | Platformă | Sintaxă | Exemplu |
   |---|---|---|
   | SMSrush | `{first_name}` | `{first_name}` |
   | Twilio | `{{first_name}}` (Liquid) | `{{first_name}}` |
   | TextMagic | `{first_name}` | `{first_name}` |
   | OneSignal (push) | `{{first_name}}` | `{{first_name}}` |
   | 360dialog (WA) | `{{1}}` (numbered) | `{{1}}` |
   | WATI (WA) | `{{first_name}}` | `{{first_name}}` |
3. **Categorii de variabile** (de la basic la avansat):
   - **Identitate**: `{first_name}`, `{last_name}`, `{phone}`
   - **GEO**: `{city}`, `{country}`, `{timezone}`
   - **Promoțional**: `{promo_code}`, `{discount_percent}`, `{bonus_amount}`, `{max_bonus}`
   - **Comportamental**: `{last_bet_sport}`, `{favorite_team}`, `{last_game_played}`, `{deposit_amount}`
   - **Temporal**: `{expiry_date}`, `{expiry_hours}`, `{event_date}`, `{match_time}`
   - **Financiar**: `{balance}`, `{loyalty_points}`, `{cashback_amount}`, `{pending_bonus}`
   - **Referral**: `{referral_code}`, `{referral_bonus}`, `{friends_invited}`
4. **Variabile specifice betting/casino**:
   - `{free_spins}`: rotiri gratuite disponibile
   - `{welcome_bonus}`: procentul bonus welcome
   - `{team_a}` / `{team_b}`: echipele din meci (dinamic per eveniment)
   - `{odds}`: cota pariurilor
   - `{jackpot_amount}`: suma jackpot curent
   - `{win_amount}`: ultima sumă câștigată (reactivare)

### Pas 2: Pregătirea bazei de date pentru personalizare maximă
1. **Structura CSV complet populat** (betting/casino):
   ```csv
   phone,first_name,country,city,segment,promo_code,bonus_pct,free_spins,fav_team,last_deposit,currency,lang
   +51921234567,Carlos,PE,Lima,fresh_lead,PERUBONUS,200,50,Alianza Lima,0,PEN,es
   +254721234567,James,KE,Nairobi,active_player,KEWIN,150,25,Gor Mahia,500,KES,en
   +59171234567,Maria,BO,Santa Cruz,dormant,BOLIVOLVER,100,30,Bolivar,200,BOB,es
   +56921234567,Diego,CL,Santiago,registered_no_dep,CHILEBONO,200,50,Colo-Colo,0,CLP,es
   ```
2. **Curățarea datelor de personalizare**:
   - Capitalize prenume: "carlos" → "Carlos" (Excel: `=PROPER(A2)`, Python: `.str.title()`)
   - Elimină spații extra: " Carlos " → "Carlos"
   - Verifică câmpuri critice: prenume, cod promo, suma bonus — NU pot fi goale
   - Standardizează valută: PEN (Peru), KES (Kenya), BOB (Bolivia), CLP (Chile)
3. **Fallback values** (CRITIC — fără fallback un câmp gol = mesaj rupt):
   - Sintaxa fallback: `{first_name|Amigo}` → dacă prenume lipsește, afișează "Amigo"
   - Fallback-uri recomandate per limbă:
     - Spaniolă: `{first_name|Amigo}`, `{city|tu ciudad}`
     - Engleză: `{first_name|Friend}`, `{city|your city}`
   - TESTEAZĂ fallback-urile cu un contact care are câmpuri goale înainte de full send
4. **Data enrichment** (populează câmpuri lipsă):
   - GEO: detectabil automat din prefix număr (+51 = Peru, +254 = Kenya)
   - Limba: determinabilă din GEO (Peru/Bolivia/Chile = es, Kenya = en)
   - Segment: bazat pe activitate (last_deposit > 0 = active, last_deposit = 0 && registered = registered_no_dep)

### Pas 3: Template-uri SMS personalizate per obiectiv (betting/casino)
1. **Welcome / Registration push** (fresh leads):
   ```
   {Hola|Saludos} {first_name|Amigo}! {free_spins} giros GRATIS te esperan. Sin deposito.
   Juega ahora: smsads.link/{promo_code} STOP dezabon
   ```
   Caractere worst-case: ~140 (GSM-7 safe)

2. **First Deposit push** (registered, no deposit):
   ```
   {first_name|Amigo}, tu bono de {bonus_pct}% te espera! Hasta ${max_bonus}.
   Deposita min $10: smsads.link/{promo_code} STOP
   ```

3. **Event-based / Sport** (active players):
   ```
   {first_name}! {team_a} vs {team_b} HOY {match_time}. Cota {odds}.
   Tu apuesta GRATIS de $20: smsads.link/live STOP
   ```

4. **Reactivare** (dormant >30 zile):
   ```
   {first_name}, te extrañamos! Bono {bonus_pct}% + {free_spins} giros gratis.
   Vuelve ahora: smsads.link/{promo_code} Valido 48h. STOP
   ```

5. **VIP / High Roller**:
   ```
   {first_name}, como jugador VIP tienes acceso exclusivo: Cashback {cashback}% esta semana.
   Deposita: smsads.link/vip STOP
   ```

6. **Kenya (English)**:
   ```
   {first_name|Friend}! Get {free_spins} FREE spins. No deposit needed.
   Play now: smsads.link/{promo_code} Via M-PESA. STOP
   ```

7. **Calcul lungime cu variabile**:
   - Estimează WORST CASE: cel mai lung prenume (15 chars) + cel mai lung cod promo (12 chars)
   - Dacă worst case > 160 chars → scurtează textul fix (nu variabilele)
   - Tool: character counter manual sau script Python
   ```python
   def check_sms_length(template, max_values):
       filled = template
       for key, val in max_values.items():
           filled = filled.replace(f"{{{key}}}", val)
       print(f"Length: {len(filled)} chars ({'OK' if len(filled) <= 160 else 'TOO LONG'})")
   ```

### Pas 4: Spintax — variație anti-spam sistematică
1. **Ce este Spintax**: tehnică de generare a variațiilor mesajului prin alternative separate de `|` între `{}`
2. **Diferența Spintax vs. Dynamic Tags**:
   - Dynamic tags: date diferite per CONTACT (`{first_name}` = "Carlos" pentru unul, "Maria" pentru altul)
   - Spintax: text diferit ALEATORIU (`{Hola|Saludos|Hey}` = variația aleatorie indiferent de contact)
   - COMBINAREA ambelor = mesaje unice per contact + variație anti-spam
3. **Spintax syntax** (standard):
   ```
   {opțiunea1|opțiunea2|opțiunea3}
   ```
4. **Implementare Spintax per element de mesaj**:
   - **Greeting**: `{Hola|Saludos|Hey|Que tal|Buenos dias}`
   - **Hook**: `{Tenemos|Preparamos|Hay|Descubre|Aprovecha}`
   - **Ofertă**: `{oferta especial|sorpresa|promocion exclusiva|regalo|bono}`
   - **CTA**: `{Registrate|Empieza|Juega|Deposita|Aprovecha} {ahora|hoy|ya}`
   - **Link intro**: `{Link|Detalles|Acceso|Mas info}`
5. **Exemplu complet SMS cu Spintax + Dynamic Tags**:
   ```
   {Hola|Saludos|Hey} {first_name|Amigo}!
   {Tenemos|Hay} {un bono|una promo|una oferta} de {bonus_pct}% para ti.
   {Registrate|Empieza} ahora: smsads.link/{promo_code}
   {STOP dezabon|Txt STOP|Responde STOP}
   ```
   Variații posibile: 3 × 2 × 3 × 2 × 3 = **108 variante unice** — imposibil de detectat ca bulk
6. **Spintax nestat** (variații în variații):
   ```
   {Hola {first_name}|{first_name}, saludos|Que tal {first_name}}
   ```
   Fiecare opțiune conține o structură diferită a salutului

### Pas 5: Spintax avansat — anti-spam la nivel de operator
1. **De ce operatorii blochează mesajele identice**:
   - Content fingerprinting: hash-ul mesajului identic × volum mare = spam flag
   - Pattern detection: aceeași structură la mii de numere = mass marketing
   - Link blacklisting: același URL scurtat × volum mare = suspect
2. **Strategii anti-fingerprinting**:
   - **Variație primele 20 caractere** (zona cea mai analizată de filtre):
     ```
     {Atencion|Noticias|Novedad|Exclusivo|Para ti}, {first_name}!
     ```
   - **Variație CTA** (zona a doua mai analizată):
     ```
     {Registrate aqui|Empieza ahora|Juega ya|Descubre mas|Accede}: {link}
     ```
   - **URL variation** (cel mai puternic anti-fingerprint):
     - URL unic per contact: `smsads.link/b/{contact_id}` → fiecare SMS are link diferit
     - URL rotation: 3-5 domenii diferite (`smsads.link`, `sms-bonus.com`, `play-now.link`)
     - Branded shortener cu tracking: fiecare contact primește URL unic trackable
   - **Invisible characters** (grey hat — folosește cu precauție):
     - Zero-width spaces (U+200B) inserate aleatoriu → fiecare mesaj e tehnic unic
     - Funcționează pe unele rute, alții le filtrează
     - NU recomand pe Tier 1 routes — risc de flagging
3. **Exemplu campanie 50,000 SMS anti-spam maxim**:
   ```
   {Atencion|Noticias|Exclusivo|Importante|Novedad} {first_name|Amigo}!
   {Bono|Promo|Oferta|Regalo} {de|del} {bonus_pct}% {para ti|exclusivo|limitado}.
   {Hasta|Max} ${max_bonus}. {Valido|Disponible} {48h|2 dias|hoy y manana}.
   {Link|Info|Detalii}: smsads.link/{promo_code}
   {STOP dezabon|Txt STOP|Responde STOP para no recibir mas}
   ```
   Variații: 5×2×4×2×2×3×3 = **4,320 variante** — eficient anti-spam

### Pas 6: Personalizare comportamentală (avansat — best ROI)
1. **Behavioral segmentation + personalizare combo**:
   - User a pariat pe fotbal → `{last_bet_sport}` = "futbol" → SMS cu ofertă fotbal
   - User a jucat slots → `{last_game_played}` = "Book of Dead" → SMS cu free spins pe acel slot
   - User a depus >$100 → VIP segment → ofertă cashback premium
2. **Trigger-based personalization** (real-time):
   - Abandonare coș / deposit page: SMS la 1h → "{first_name}, tu bono te espera. Completa tu deposito: {link}"
   - Câștig mare: SMS post-win → "Felicidades {first_name}! Ganaste ${win_amount}. Tu proximo bono: {link}"
   - Inactivitate 7 zile: "{first_name}, hace {days_inactive} dias que no juegas. Vuelve con {free_spins} giros gratis!"
3. **Personalizare GEO avansată**:
   - Detectare city din prefix telefon → "{first_name}, oferta especial para jugadores de {city}!"
   - Eveniment sportiv local → "Alianza Lima vs Universitario HOY! Pariaza cu cota {odds}"
   - Metoda de plată locală → "Deposita via {payment_method}" (Yape PE, M-PESA KE, Tigo Money BO)
4. **Implementare în Twilio** (API):
   ```python
   # Personalizare dinamică la trimitere
   message = client.messages.create(
       body=f"Hola {contact['first_name']}! Bono {contact['bonus_pct']}% "
            f"+ {contact['free_spins']} giros gratis. "
            f"Deposita via {contact['payment_method']}: "
            f"smsads.link/{contact['promo_code']} STOP",
       from_=sender_id,
       to=contact['phone']
   )
   ```

### Pas 7: Personalizare push notifications (OneSignal / PushEngage)
1. **OneSignal personalizare avansată**:
   ```javascript
   // Tag-uri per user (setate la behavior tracking)
   OneSignal.User.addTags({
     "first_name": "Carlos",
     "favorite_sport": "football",
     "last_deposit": "50",
     "segment": "active_player",
     "lang": "es"
   });
   ```
2. **Push template personalizat**:
   ```
   Title: "{{first_name}}, {{free_spins}} giros gratis!"
   Body: "Juega {{last_game}} con bonus {{bonus_pct}}%. Expira en {{expiry_hours}}h."
   URL: "https://smsads.com/play?ref={{user_id}}&promo={{promo_code}}"
   ```
3. **Segmented push** (personalizare per segment):
   - Segment "Football Fans" (tag: favorite_sport = football):
     - Title: "⚽ {{team_a}} vs {{team_b}} — Cota {{odds}}!"
     - Body: "{{first_name}}, tu apuesta GRATIS de $20 te espera"
   - Segment "Slot Players" (tag: favorite_sport = slots):
     - Title: "🎰 {{free_spins}} Giros en {{game_name}}!"
     - Body: "{{first_name}}, juega GRATIS — sin deposito"
4. **Behavioral triggers push** (automatizat):
   - Coș abandonat → push la 1h: "{{first_name}}, tu bono te espera en el carrito"
   - Page visit ×3 → push: "{{first_name}}, el precio bajó para {{product_name}}"
   - Inactivitate 7d → re-engagement push: "{{first_name}}, {{free_spins}} giros gratis si vuelves hoy"

### Pas 8: QA și testare pre-trimitere — protocul complet
1. **Test matrix** (minim 8 test messages):
   | Test Case | Descriere | Verificare |
   |---|---|---|
   | TC-01 | Contact cu toate câmpurile complete | Variabilele afișate corect |
   | TC-02 | Contact cu prenume gol | Fallback afișat ("Amigo") |
   | TC-03 | Contact cu prenume lung (15+ chars) | SMS sub 160 chars |
   | TC-04 | Contact cu caractere speciale (ñ, ü) | Encoding corect (GSM-7 sau Unicode) |
   | TC-05 | Contact din GEO diferit (PE vs KE) | Limba/valută corectă |
   | TC-06 | Spintax output verification | 10 variante generate, toate cu sens |
   | TC-07 | Link tracking funcțional | Click înregistrat în tracker |
   | TC-08 | Opt-out funcțional | STOP procesat corect |
2. **Preview în platformă**:
   - SMSrush/TextMagic: preview cu date reale ale primilor 10 contacte
   - Twilio: test API call → verifică output în logs
   - OneSignal: "Preview" per segment
3. **Checklist encoding**:
   - Caractere GSM-7 safe (160 chars/SMS): A-Z, a-z, 0-9, !, @, #, $, %, &, *, (, ), ?, ñ, ¡, ¿, á, é, í, ó, ú
   - Caractere care FORȚEAZĂ Unicode (70 chars/SMS): ă, î, â, ș, ț (română), emoji, caractere chinezești/arabe
   - Validare: rulează prin GSM-7 validator înainte de full send
4. **Spintax validation**:
   - Tool online: textmechanic.com/text-tools/randomization-tools/spintax-spinner/
   - Generează 20 variante → verifică vizual că TOATE au sens
   - Verifică că nicio variantă nu depășește 160 chars

### Pas 9: Tracking personalizat — atribuirea conversiilor per variabilă
1. **URL unic per contact** (perfect attribution):
   ```
   smsads.link/{promo_code}?uid={contact_id}&src=sms&camp=welcome_bo_mar26
   ```
   - `{promo_code}`: cod unic per contact → tracking exact cine a convertit
   - `uid`: ID intern contact → linkare cu baza de date
   - `src` + `camp`: UTM-uri pentru Google Analytics
2. **Promo code tracking**:
   - Generează cod unic per contact: `PERU-{first3letters}-{random4digits}` → "PERU-CAR-7842"
   - La conversie: user introduce codul → match cu contactul → atribuire perfectă
   - Avantaj: funcționează și dacă user-ul copiază link-ul manual (nu depinde de tracking pixel)
3. **A/B testing personalizare**:
   - Test: mesaj cu prenume vs. fără prenume → măsoară CTR
   - Test: mesaj cu sumă bonus vs. procent bonus → măsoară conversie
   - Test: mesaj cu echipă favorită vs. ofertă generală → măsoară CTR
   - Min 1,000 contacte per variantă pentru semnificativitate
4. **Raportare personalizare impact**:
   ```
   | Element personalizat | CTR fără | CTR cu | Lift |
   |---|---|---|---|
   | Prenume | 6.2% | 8.1% | +30.6% |
   | Echipă favorită | 7.5% | 11.3% | +50.7% |
   | Sumă bonus specifică | 5.8% | 7.9% | +36.2% |
   | Metoda de plată locală | 8.0% | 10.5% | +31.3% |
   ```

## Verificare
- [ ] CSV pregătit cu TOATE coloanele necesare (identitate + promo + comportamental + GEO)
- [ ] Fallback-uri definite pentru TOATE variabilele critice
- [ ] Template SMS testat cu 8 test cases (tabel de mai sus)
- [ ] Lungime SMS verificată worst-case (≤160 chars GSM-7)
- [ ] Encoding verificat (fără caractere Unicode accidentale)
- [ ] Spintax implementat cu minim 3 variații per element cheie
- [ ] Spintax preview: 20 variante generate și verificate vizual
- [ ] Variații totale >100 per campanie (anti-fingerprinting)
- [ ] URL-uri unice per contact generate (promo code sau contact ID)
- [ ] Personalizare comportamentală implementată (minim 2 variabile beyond first_name)
- [ ] Push notifications cu tags și segmented personalization
- [ ] Test final trimis la 8 numere proprii cu date diverse

## Instrumente
- **SMSrush**: {variabile} și spintax nativ
- **Twilio**: Liquid templates, API personalization, Content API
- **TextMagic**: merge fields nativi, preview per contact
- **OneSignal**: personalizare push cu tags + liquid syntax
- **TextMechanic**: textmechanic.com — spintax spinner și preview
- **Excel / Python pandas**: curățare, standardizare, generare coduri promo unice
- **GSM-7 Validator**: messente.com/documentation/tools/sms-length-calculator

## Note
- Personalizare cu prenume = +15-25% CTR. Personalizare comportamentală (echipă, joc) = +30-50% CTR
- Spintax cu 100+ variații = practic imposibil de detectat ca bulk de operatori
- Cel mai important câmp: prenumele în primele 10 caractere (captează atenția instant)
- Fallback-urile lipsă = cel mai comun bug. "{first_name}" literal în SMS = dezastru de brand la 10K destinatari
- URL-uri unice per contact = perfect attribution dar necesită infrastructure de generare + redirect
- Encoding trap: ñ (GSM-7 OK) dar ñ cu accent (ǹ) = Unicode. Verifică MEREU cu validator
- Personalizarea fără date de calitate = dezastru. "Hola null!" sau "Bonus 0%" = worse than no personalization
- Rule of thumb: fiecare nivel de personalizare (prenume → GEO → comportament → timing) adaugă 15-25% CTR
- La 4 nivele de personalizare simultane: CTR poate fi 2-3x față de mesaj generic
