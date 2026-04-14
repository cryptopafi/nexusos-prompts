---
id: MPM-007
title: SMS/Push Compliance Framework (GDPR, TCPA, DLT, Regional)
domain: mobile-push-marketing
source: SMS Marketing Text Message Marketing Strategies (Udemy) + 10yr performance marketing experience
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["smsads"]
---

# SMS/Push Compliance Framework (GDPR, TCPA, DLT, Regional)

## Obiectiv
Implementează un framework complet de conformitate pentru campaniile SMS, push și WhatsApp, acoperind GDPR (Europa), TCPA (SUA), DLT (India), regulamentele gambling per GEO (Bolivia, Peru, Kenya, Chile) și politicile Meta WhatsApp — astfel încât să operezi 100% legal, să eviți amenzile și să construiești o bază de contacte de calitate pe termen lung.

## Pași

### Pas 1: Maparea cadrului legal per GEO și canal
1. **GDPR (EU/EEA + UK)** — se aplică dacă targetezi rezidenți EU:
   - Baza legală: **consimțământ explicit** (Article 6(1)(a) + Article 7)
   - Consimțământul: liber, specific, informat, neechivoc, retractabil
   - Amende: până la €20M sau 4% din cifra de afaceri globală anuală (se aplică cea mai mare)
   - Autoritate RO: **ANSPDCP** (dataprotection.ro)
   - ePrivacy Directive (2002/58/EC): reglementează specific electronic communications (SMS, push, email)
2. **TCPA (SUA)** — se aplică dacă trimiți SMS la numere US:
   - Necesită **Prior Express Written Consent** pentru SMS marketing
   - Interzice autodialer fără consimțământ
   - Amendă: $500-$1,500 PER MESAJ ilegal (class action lawsuits comune — risc financiar enorm)
   - Ore permise: 8:00 AM - 9:00 PM (ora locală destinatarului)
   - CTIA (Cellular Telecommunications Industry Association): guidelines suplimentare obligatorii
   - **10DLC registration**: obligatoriu pentru A2P (Application-to-Person) messaging pe numere US
3. **DLT (India — Distributed Ledger Technology)**:
   - TRAI (Telecom Regulatory Authority of India) a implementat DLT blockchain
   - OBLIGATORIU: Entity Registration + Template Registration + Consent Registration
   - Fiecare mesaj trebuie **Template ID** aprobat + **Entity ID** + **Header ID**
   - Fără DLT registration = 0% delivery rate pe operatorii indieni
   - Platforme DLT: Jio DLT, BSNL DLT, Airtel DLT
4. **Reglementări per GEO SMSads**:
   | GEO | Regulator telecom | Regulator gambling | Gambling status | SMS marketing status |
   |---|---|---|---|---|
   | Bolivia | ATT | AJ | Legal, reglementat | Permis cu opt-in |
   | Peru | OSIPTEL + MTC | MINCETUR | Legal online, reglementat | Permis, Sender ID approval |
   | Kenya | CA Kenya | BCLB | Legal, heavily regulated | Permis, strict content rules |
   | Chile | SUBTEL | SCJ + Ley Juego | În curs reglementare | Permis cu opt-in |
5. **Meta WhatsApp Policy**:
   - Gambling/betting = "Sensitive Category" — necesită business verification + template compliance
   - Prohibit: conținut explicit gambling către minori, promisiuni de câștig garantat
   - Ban policy: 3 strikes → permanent ban pe Business Account

### Pas 2: Implementarea sistemului de opt-in per canal
1. **SMS Opt-in (standard minim legal)**:
   - **Single opt-in**: user introduce numărul + bifează căsuță (NU pre-bifată)
   - Text căsuță: "Sunt de acord să primesc mesaje SMS cu oferte de la [Brand]. Pot dezabona oricând răspunzând STOP."
   - **Double opt-in** (recomandat — calitate mai bună, dovadă mai puternică):
     1. User se înscrie pe formular
     2. Primește SMS: "Confirmă abonarea la [Brand]. Răspunde DA pentru confirmare."
     3. User răspunde DA → abonat activ
   - **Keyword opt-in** (pentru acquisition): user trimite "BONUS" la shortcode → auto-reply cu confirmare
2. **Push Notification Opt-in**:
   - Browser prompt = consimțământ nativ (browserul cere permisiunea)
   - Custom slidedown ÎNAINTE de prompt-ul nativ (see MPM-002)
   - GDPR: browser prompt = baza legală de consimțământ, DAR informează user-ul despre scopul înainte
3. **WhatsApp Opt-in** (cel mai strict):
   - Opt-in SPECIFIC pentru WhatsApp (separat de SMS!)
   - Surse valide: web form, click-to-WhatsApp ad, keyword opt-in, QR code
   - Text: "Vreau să primesc mesaje WhatsApp cu oferte de la [Brand] la numărul [+XX...]"
   - Meta poate solicita dovada opt-in — dacă nu ai, cont suspendat
4. **Dovada consimțământului** (obligatoriu GDPR + TCPA):
   - Stochează: timestamp, IP address, sursa, versiunea textului, forma de consimțământ
   - Format record:
     ```json
     {
       "phone": "+51921234567",
       "channel": "sms",
       "timestamp": "2026-03-20T10:30:00Z",
       "ip": "190.12.34.56",
       "source": "web_form_homepage",
       "consent_text_version": "v2.3",
       "consent_method": "checkbox_explicit",
       "double_opt_in_confirmed": true,
       "confirmation_timestamp": "2026-03-20T10:31:15Z"
     }
     ```
   - Retenție: păstrează pe toată durata relației + 5 ani după (TCPA statute of limitations = 4 ani)

### Pas 3: Gestionarea opt-out-urilor (dezabonărilor) — automatizare completă
1. **Mecanisme de opt-out per canal**:
   - **SMS**: keyword "STOP" / "UNSUBSCRIBE" / "CANCEL" / "QUIT" / "END" (toate 5 obligatoriu TCPA)
   - **Push**: dezabonare din setările browserului (gestionat automat)
   - **WhatsApp**: keyword "STOP" / "PARA" / "CANCELAR" + block option nativ
2. **Procesare automată opt-out** (nu manual — risc legal):
   - SMS: platformă detectează STOP → auto-reply confirmare → adaugă blacklist → exclude din TOATE campaniile viitoare
   - Timeline: procesare IMEDIATĂ (best practice). Legal: max 10 zile GDPR, 10 business days TCPA
   - Auto-reply confirmare: "Has sido desuscrito de [Brand]. No recibirás más mensajes."
3. **Blacklist management**:
   - Blacklist centralizat (nu per campanie — GLOBAL)
   - Verificare blacklist OBLIGATORIE la fiecare campanie, pe fiecare canal
   - Format: `blacklist_sms.csv` + `blacklist_whatsapp.csv` — separate per canal
   - Un număr pe blacklist SMS poate fi încă ok pentru WhatsApp (și vice versa) — respectă canalul specific
4. **Category opt-out** (avansat):
   - Global STOP: nu mai primește NIMIC (obligatoriu să funcționeze)
   - Category STOP: "STOP PROMO" → nu mai primește promoțional dar rămâne pentru transacțional
   - Implementare DB: `sms_promo_optout: boolean`, `sms_transactional_optout: boolean`, `wa_optout: boolean`
5. **Testing lunar**: testează mecanismul STOP end-to-end pe fiecare canal, fiecare lună

### Pas 4: Compliance specifică gambling/betting per GEO
1. **Peru (MINCETUR — cel mai permisiv din lista SMSads)**:
   - Gambling online LEGAL, reglementat de MINCETUR (Ministerio de Comercio Exterior y Turismo)
   - Licență necesară pentru operare (obținută de operatorul de betting, nu de affiliate)
   - SMS marketing betting permis cu: opt-in, identificare brand, disclaimer "Juega responsablemente", "18+"
   - Ore interzise SMS promo: înainte de 08:00 și după 21:00
   - Obligatoriu: menționarea licenței MINCETUR în comunicări
2. **Kenya (BCLB — Betting Control and Licensing Board)**:
   - Gambling LEGAL dar STRICT reglementat
   - Betting companies trebuie licență BCLB
   - SMS gambling: OBLIGATORIU "Bet Responsibly" în fiecare mesaj
   - Obligatoriu menționare: "18+" sau "Not for persons under 18"
   - Interzis: advertising care targetează minori sau persoane vulnerabile
   - Taxă 20% pe câștiguri (Excise Duty) — nu poți promite câștiguri nete fără menționarea taxei
   - KICA (Kenya Information and Communications Act): prevede sancțiuni pentru spam
3. **Bolivia (AJ — Autoridad de Fiscalización del Juego)**:
   - Gambling reglementat, predominant offline
   - Online gambling: zonă gri, nu explicit interzis dar nici reglementat complet
   - SMS marketing: permis cu opt-in, ATT monitorizează
   - Recomandare: tratează ca reglementat — include disclaimers, opt-out, identificare
4. **Chile (SCJ + Ley de Juego Online)**:
   - Lege online gambling adoptată 2024, implementare în curs
   - Licențiere operatori în curs (2025-2026)
   - SMS marketing gambling: permis dacă operatorul are licență (sau în process)
   - SUBTEL: reglementare telecomunicații, Sender ID approval necesar
   - Recomandare: include "Solo mayores de 18 años", "Juega con responsabilidad"
5. **Template disclaimers obligatorii per GEO**:
   - Peru: "Juega responsablemente. Solo mayores de 18 años. Licencia MINCETUR."
   - Kenya: "Bet Responsibly. 18+. Licensed by BCLB."
   - Bolivia: "Solo para mayores de 18 años. Juega con responsabilidad."
   - Chile: "Juega con responsabilidad. Solo mayores de 18 años."

### Pas 5: Infrastructura de date conformă GDPR
1. **Baza de date contacte — câmpuri obligatorii**:
   ```sql
   CREATE TABLE contacts (
     id UUID PRIMARY KEY,
     phone VARCHAR(20) NOT NULL,
     phone_hash VARCHAR(64), -- SHA-256 for pseudonymization
     first_name VARCHAR(100),
     country_code VARCHAR(3),
     -- Consent tracking
     sms_opt_in_date TIMESTAMP,
     sms_opt_in_source VARCHAR(100),
     sms_opt_in_ip VARCHAR(45),
     sms_consent_version VARCHAR(10),
     sms_double_opt_in_confirmed BOOLEAN DEFAULT FALSE,
     wa_opt_in_date TIMESTAMP,
     wa_opt_in_source VARCHAR(100),
     push_opt_in_date TIMESTAMP,
     -- Opt-out tracking
     sms_optout BOOLEAN DEFAULT FALSE,
     sms_optout_date TIMESTAMP,
     wa_optout BOOLEAN DEFAULT FALSE,
     wa_optout_date TIMESTAMP,
     push_optout BOOLEAN DEFAULT FALSE,
     -- Metadata
     segment VARCHAR(50),
     last_interaction_date TIMESTAMP,
     created_at TIMESTAMP DEFAULT NOW(),
     updated_at TIMESTAMP DEFAULT NOW()
   );
   ```
2. **Data retention policy**:
   - Abonați activi: date păstrate pe durata relației
   - Dezabonați: ștergere date personale în 30 zile. Păstrează DOAR hash-ul numărului în blacklist (previne re-abonare accidentală)
   - Inactivi >12 luni: re-consent campaign sau delete
3. **Dreptul la ștergere (Right to be Forgotten / GDPR Article 17)**:
   - Procedură: la cerere, ștergere toate datele personale în 30 zile
   - Excepție: hash-ul numărului rămâne în blacklist (interes legitim: previne re-trimitere)
   - Documentează: data cererii, acțiuni, data completării
4. **Data Processing Agreements (DPA)**:
   - Semnează DPA cu FIECARE furnizor care procesează date UE:
     - Twilio: DPA disponibil în dashboard → Legal → Data Processing Addendum
     - MessageBird: DPA descărcabil din Trust Center
     - 360dialog: DPA inclus în contract
     - SMSrush: verifică dacă oferă DPA (dacă nu → risc GDPR)
5. **Securitate**:
   - Criptare at rest (database encryption)
   - Criptare in transit (HTTPS/TLS obligatoriu)
   - Access control: doar persoanele autorizate au acces
   - Nu exporta liste neencriptate pe email, Slack, Google Drive public

### Pas 6: Anti-spam best practices — content compliance
1. **Identificare obligatorie** în fiecare mesaj:
   - Sender ID sau text: "[Brand]:" la începutul SMS
   - Nu trimite anonim sau cu Sender ID înșelător (spoofing = ilegal)
2. **Opt-out instruction** — obligatorie în fiecare mesaj promoțional:
   - SMS: "STOP pt dezabon." sau "Txt STOP to stop" sau "Responde STOP"
   - GDPR: de preferat în FIECARE mesaj
   - TCPA: obligatoriu în fiecare mesaj, plus reminder periodic
3. **Interdicții de conținut** (cross-jurisdicție):
   - NU promite câștiguri garantate: "Vei câștiga €5000" ❌ → "Poți câștiga până la €5000" ✅
   - NU conținut deceptiv sau misleading
   - NU targetare minori (18+ obligatoriu pentru gambling)
   - NU conținut violent, discriminatoriu, obscen
   - NU phishing sau impersonare (pretinde a fi bancă, operator, etc.)
4. **Frecvența maximă legală/recommended**:
   - Promoțional: max 4-6 SMS/lună (gambling tolerează mai mult decât alte verticale)
   - Niciodată >1 SMS promoțional/zi
   - Transacțional: fără limită (dar trebuie să fie REAL transacțional, nu promoțional deghizat)
5. **Ore interzise** (legal + best practice):
   - EU (GDPR/ePrivacy): nu există ore fixe legale, dar best practice: 08:00-21:00
   - US (TCPA): 8:00 AM - 9:00 PM (ora locală — calculează per timezone!)
   - India: 09:00-21:00 (TRAI regulation, strict enforced)
   - Peru/Bolivia/Chile/Kenya: best practice 08:00-21:00, verifică reglementări locale

### Pas 7: Audit periodic de conformitate — calendar
1. **Audit SĂPTĂMÂNAL** (5 min):
   - Opt-out-urile din ultima săptămână sunt procesate? ✓
   - Blacklist actualizat? ✓
   - Quality Score WhatsApp = Green? ✓
2. **Audit LUNAR** (30 min):
   - Verifică că blacklist-ul e aplicat în TOATE campaniile
   - Verifică dovezile de consimțământ (sampling: verifică 10 random contacts)
   - Calculează opt-out rate trending — crește? scade?
   - Testează mecanismul STOP end-to-end pe fiecare canal
   - Verifică DPA-urile cu furnizorii — au expirat? s-au schimbat T&C?
3. **Audit TRIMESTRIAL** (2h):
   - Revizuiește textul de consimțământ — mai e actual? legislația s-a schimbat?
   - Testează COMPLET: opt-in → campanie → opt-out → verificare blacklist
   - Verifică furnizorii: sunt încă conformi GDPR? Au DPA valid?
   - Actualizează Privacy Policy dacă ai adăugat canale/furnizori noi
   - Review gambling regulation updates per GEO (MINCETUR, BCLB, etc.)
4. **Audit ANUAL** (full review):
   - Registrul de activități de prelucrare (GDPR Article 30) — actualizare completă
   - Data retention cleanup: șterge datele expirate
   - Re-evaluate furnizorii (pricing, compliance, deliverability)
   - Training echipă pe updates legislative

### Pas 8: Răspuns la incidente — playbook
1. **La primirea unei cereri GDPR (Subject Access Request, Erasure Request)**:
   - Confirmare primire în 72h
   - Rezolvare completă în 30 zile
   - Documentare: data cererii, tipul, acțiuni, data rezolvării
   - Procedură automatizată: script care extrage/șterge toate datele per phone number din toate sistemele
2. **La o breșă de securitate** (date scurse):
   - Notifică ANSPDCP (sau autoritatea locală) în **72h** dacă breșa afectează drepturi/libertăți
   - Notifică persoanele afectate dacă riscul e ridicat
   - Documentare: ce date, câte persoane, cauza, măsuri luate
   - Post-incident: root cause analysis + fix + prevenție
3. **La o investigație/reclamație** de la autoritate (ANSPDCP, ICO, FCC):
   - Cooperare completă
   - Furnizează dovezile de consimțământ = PRIMA LINIE DE APĂRARE
   - Contact avocat specializat GDPR/telecom IMEDIAT
4. **La un ban WhatsApp**:
   - Nu panica — appeal în 24h prin Meta Business Help Center
   - Pregătește dovezi: opt-in records, template approvals, quality history
   - Activează backup number pe BSP alternativ
   - Success rate appeal: 20-30% (mai mare dacă ai dovezi solide)

### Pas 9: Compliance automation — tools și implementare
1. **Automation opt-out processing**:
   - Twilio: `incomingPhoneNumber → webhook → parse "STOP" → add to blacklist → auto-reply`
   - SMS platform built-in: majoritatea platformelor au auto-STOP processing
   - WhatsApp: auto-reply rule pe "STOP" → tag removal → blacklist
2. **Automation consent tracking**:
   - Web form → on submit → store consent record in database with timestamp + IP
   - API integration: Twilio Consent API, OneSignal auto-consent tracking
3. **Automation data retention**:
   - Cron job lunar: identifică contacte dezabonate >30 zile → delete PII, keep hash in blacklist
   - Cron job anual: identifică contacte inactive >12 luni → trigger re-consent campaign
4. **Monitoring alerts**:
   - Alert dacă opt-out rate >2% pe campanie
   - Alert dacă delivery rate <90%
   - Alert dacă WhatsApp Quality Score devine Yellow
   - Alert dacă DPA cu furnizor expiră în 30 zile
5. **Compliance dashboard** (Google Sheets sau Notion):
   ```
   Columns: GEO | Canal | Opt-in Count | Opt-out Count | Opt-out Rate | Last Audit | DPA Status | Notes
   ```

## Verificare
- [ ] Cadru legal mapat per GEO și canal (GDPR, TCPA, DLT, gambling regulations)
- [ ] Formularul de opt-in NU are căsuța pre-bifată
- [ ] Text consimțământ clar, specific per canal (SMS ≠ WhatsApp ≠ Push)
- [ ] Double opt-in implementat pe SMS (recomandat)
- [ ] Dovada consimțământului stocată (timestamp + IP + source + version)
- [ ] Mecanismul STOP funcțional pe TOATE canalele (testat end-to-end)
- [ ] Blacklist GLOBAL actualizat și verificat la fiecare campanie
- [ ] Disclaimers gambling incluse per GEO (18+, Bet Responsibly, licență)
- [ ] Sender ID identificabil în toate mesajele
- [ ] Opt-out instruction în fiecare mesaj promoțional
- [ ] Privacy Policy actualizată cu secțiunile SMS/Push/WhatsApp
- [ ] DPA semnat cu fiecare furnizor SMS/Push/WhatsApp
- [ ] Calendar audit configurat (săptămânal + lunar + trimestrial)
- [ ] Proceduri incident response documentate

## Instrumente
- **ANSPDCP**: dataprotection.ro — GDPR România
- **BCLB Kenya**: bclb.go.ke — Betting Control and Licensing Board
- **MINCETUR Peru**: mincetur.gob.pe — gambling regulation Peru
- **CTIA**: ctia.org — SMS guidelines US
- **FCC TCPA**: fcc.gov/consumers/guides/stop-unwanted-robocalls-and-texts — TCPA guidelines
- **Twilio / MessageBird**: DPA-uri descărcabile din panoul legal
- **Termly / iubenda**: generatoare Privacy Policy conforme GDPR
- **Osano / CookieYes**: cookie + consent management platforms

## Note
- GDPR + ePrivacy = gold standard. Dacă ești GDPR compliant, ești aproape compliant și în alte jurisdicții
- TCPA e cel mai PERICULOS financiar: $1,500/mesaj × class action = milioane $ (cazuri reale: Capital One $75M, Dish Network $280M)
- DLT India: dacă nu ești registrat, delivery rate = 0%. Obligatoriu pentru oricine trimite SMS în India
- Gambling SMS: Peru = cel mai permisiv, Kenya = cel mai strict, Bolivia/Chile = moderate
- Consimțământul pre-bifat = amendă GARANTATĂ la audit GDPR. Niciodată pre-bifat
- Dovezile de consimțământ = PRIMA TA APĂRARE. Fără dovezi = prezumpție de vinovăție
- Costul non-compliance e MEREU mai mare decât costul compliance. O amendă GDPR = 100x costul implementării corecte
- WhatsApp ban = pierdere canal cu 80-95% open rate. Compliance WhatsApp = prioritate maximă
- Greșeala #1 care duce la amenzi: opt-out procesat manual, cu delay. AUTOMATIZEAZĂ procesarea STOP
