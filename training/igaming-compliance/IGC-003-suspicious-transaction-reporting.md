---
id: IGC-003
title: Suspicious Transaction Identification and Reporting
domain: igaming-compliance
source: Anti-Money Laundering in Gambling (Udemy) + FATF R.20 STR + FIAU/NCA Guidance + EGBA Typologies
version: 2.0
forge_score: "estimated H/3.8"
business_mapping: ["smsads", "gambling-seo"]
---

# Suspicious Transaction Identification and Reporting

## Obiectiv
Definiți un proces sistematic, documentat și auditabil pentru identificarea tranzacțiilor suspecte în operațiunile iGaming, raportarea internă la MLRO și raportarea externă la FIU (FATF R.20 — STR), în conformitate cu obligațiile legale din toate jurisdicțiile active (MGA, UKGC, GGL, Curaçao, Kahnawake, ONJN, CONAJZAR, Kenya BCLB), fără a alerta clientul investigat (tipping-off prevention).

## Pași

### Pas 1: Configurarea sistemului de monitoring tranzacții — reguli și praguri
1. Definiți regulile de alertare automată în sistemul de procesare plăți / platforma de gaming. Categorii de alerte obligatorii:

   **A. Structuring / Smurfing** (fragmentarea deliberată sub praguri):
   - 3+ depozite sub pragul CDD în aceeași zi (ex. 3x €900 = alert)
   - Depozite la intervale regulate sub prag pe parcursul unei săptămâni
   - Depozite de la multiple metode de plată sub prag în aceeași zi

   **B. Pass-through / Wash trading**:
   - Depozit urmat de retragere fără activitate de joc semnificativă (<10% din depozit jucat)
   - Raport câștig/pierdere anormal — client pierde <2% din total depozite pe perioade lungi
   - Retragere la metodă diferită de cea de depozit (ex. depozit card → retragere cont bancar)

   **C. Schimbări bruște de comportament**:
   - Depozit >200% față de media pe 30 zile
   - Frecvență de joc crescută >300% față de media
   - Schimbare bruscă de produs (ex. de la slots €1 la high-roller table)

   **D. Indicatori geo/device**:
   - Login din jurisdicție HIGH RISK fără istoric anterior
   - Utilizare VPN detectată (discrepanță IP vs. geolocație declarată)
   - Multiple conturi de pe același device fingerprint
   - IP asociat cu datacenter/proxy (nu residential)

   **E. Third-party indicators**:
   - Plăți de la card/cont bancar cu alt nume decât titularul contului
   - Depozite de la business account pentru cont personal
   - Crypto depozite de la wallet-uri flagged (mixing services, darknet)

   **F. Gambling-specific indicators**:
   - Win rate statistică improbabilă: >60% pe >100 pariuri (posibil match-fixing, coluziune)
   - Mize inconsistente cu depozitul (depozit €100, miză imediată €100 pe un singur pariu)
   - Pariuri opuse pe același eveniment de pe conturi diferite dar device similar (arbitrage fraud)

2. Setați threshold-uri per jurisdicție conform cerințelor locale:

   | Jurisdicție | Alert automat (singulă tranzacție) | Alert automat (cumulat/perioadă) | Raportare obligatorie |
   |-------------|-----------------------------------|--------------------------------|---------------------|
   | UKGC | Orice tranzacție neobișnuită | £2,000 net dep / £500 net loss (financial trigger) | Toate suspiciunile → NCA |
   | MGA | €2,000 single | €10,000 cumulat 30 zile | €15,000 single → FIAU automat |
   | GGL | €1,000/lună HARD (LUVAS) | Orice depășire limită | Toate suspiciunile → FIU DE |
   | Curaçao | $2,000 single | $10,000 cumulat | $15,000 → FIU Curaçao |
   | ONJN | 5,000 RON single | 15,000 EUR echiv. cumulat | 15,000 EUR → ONPCSB |
   | Kahnawake | CAD $3,000 single | CAD $10,000 cumulat | CAD $10,000 → FINTRAC |
   | CONAJZAR | PYG 40M single | PYG 100M cumulat | PYG 40M+ → SEPRELAD |
   | Kenya BCLB | KES 1M single | KES 5M cumulat | KES 1M+ → FRC |

3. Testați regulile de alertare cu scenarii simulate la implementare și trimestrial.
4. Documentați toate regulile configurate cu data implementării, rațiunea, și ultima testare.
5. Asignați responsabil pentru review-ul zilnic al alertelor — nu lăsați alerte nerevizuite >24h.

### Pas 2: Typologii de spălare a banilor specifice iGaming — registru actualizat
1. **Layering prin gambling** (cea mai comună tipologie iGaming):
   - Clientul depune fonduri de proveniență suspectă → joacă minim (pentru aparență de activitate legitimă) → retrage "câștiguri" curate.
   - **Red flags**: bet size inconsistent cu depozitul; play time anormal de scurt (<5 min); retragere imediată după atingerea unui prag minim de joc; raport pierdere <5% pe perioade lungi.
   - **Exemplu real**: UKGC case — client depune £50,000 pe parcursul a 2 săptămâni, joacă slots cu mize mici (£1-5), retrage £48,000 ca "câștiguri." Operatorul nu a raportat → amendă £2.3M.

2. **Chip dumping** (poker/jocuri head-to-head):
   - Jucătorii complici transferă chips deliberat unui singur cont prin pierderi intentionate.
   - **Red flags**: pierderi repetate între aceleași conturi; all-in pe mâini slabe; același IP/device; pattern de joc non-aleator.
   - **Monitorizare**: analizați perechi de jucători cu rată de interacțiune anormal de mare.

3. **Bonus abuse cu fonduri suspecte**:
   - Crearea de conturi multiple pentru colectarea bonusurilor welcome. Depozitul inițial provine din fonduri suspecte.
   - **Red flags**: IP comun, device fingerprint similar, email-uri secvențiale (name1@, name2@), date personale similare.
   - **Context SMSads**: campanii SMS pot fi exploatate de fraud rings care creează conturi multiple rapid.

4. **Crypto mixing / tumbling**:
   - Depozite crypto din wallet-uri mixing (Tornado Cash, ChipMixer) sau cu proveniență necunoscută.
   - **Red flags**: adrese crypto flagged în Chainalysis/Elliptic; depozite din multiple wallet-uri neconectate; sumă exactă rotundă (ex. 1.0000 BTC).
   - **Tool-uri**: Chainalysis KYT (Know Your Transaction), Elliptic Lens, Crystal Blockchain.

5. **Tranzacții third-party**:
   - Plăți de la o persoană diferită de titularul contului de joc.
   - **Red flags**: nume diferit pe card/cont bancar vs. contul de joc; plăți de la business account; plăți de la persoane din jurisdicții HIGH RISK.

6. **Refund abuse / chargeback laundering**:
   - Client depune cu card de credit → joacă → solicită chargeback la bancă pretinzând fraudă → fondurile "curate" rămân în cont.
   - **Red flags**: multiple chargebacks de la același client; pattern de depozit → chargeback fără joc real.

7. **Cuckoo smurfing** (avansată):
   - Rețele de spălare folosesc conturi de gambling ale unor persoane neimplicate (conturi compromise sau persoane recrutate ca "muli").
   - **Red flags**: contul primește depozite terță parte; comportament de joc complet diferit de istoric; retrageri rapide la metode noi.

8. Mențineți un **Typologies Register** actualizat cu:
   - Fiecare tipologie documentată cu: descriere, red flags, referință (FATF/EGBA/regulator)
   - Data ultimei actualizări
   - Cazuri interne identificate (anonimizate)
   - Surse: FATF Typologies Reports, EGBA AML Typologies Paper, FIAU Case Studies, NCA SAR Intelligence Report

### Pas 3: Procesul intern de raportare — de la alert la MLRO
1. Când un angajat identifică o suspiciune (alert automat sau observație manuală), completează **Formularul SAR Intern** în max 24h de la identificare:

   ```
   INTERNAL SUSPICIOUS ACTIVITY REPORT
   ====================================
   Ref: SAR-INT-[YYYY]-[sequential]
   Date: [DD/MM/YYYY HH:MM]
   Reporter: [Name, Role, Department]
   Reporter contact: [email/phone]

   SUBJECT:
   - Account ID: _______________
   - Full Name: _______________
   - DOB: _______________
   - Nationality: _______________
   - KYC Status: [SDD/CDD/EDD]
   - PEP Status: [Y/N]
   - Account age: _______________
   - Total lifetime deposits: _______________
   - Total lifetime withdrawals: _______________

   SUSPICIOUS ACTIVITY:
   - Date(s): _______________
   - Typology suspected: [structuring/pass-through/chip-dumping/
     third-party/crypto-mixing/bonus-abuse/other: ___]
   - Total value involved: _______________
   - Description (detailed narrative):
     ________________________________________________
     ________________________________________________
   - Specific red flags identified:
     1. _______________
     2. _______________
     3. _______________
   - How identified: [automated alert / manual observation /
     customer contact / third-party tip]

   EVIDENCE ATTACHED:
   - [ ] Transaction logs
   - [ ] KYC documents
   - [ ] Device/IP data
   - [ ] Game session logs
   - [ ] Communication records
   - [ ] Screening results (PEP/sanctions)
   - [ ] Blockchain analysis (if crypto)

   URGENCY: [Standard / Immediate Escalation]
   ```

2. Formularul se trimite EXCLUSIV la MLRO prin canalul securizat — nu se discută cazul cu colegi, management, sau clientul.
3. MLRO înregistrează cazul în **Registrul Investigațiilor Interne** cu:
   - Număr unic de referință
   - Timestamp primire
   - Categorie suspiciune
   - Status: OPEN / INVESTIGATING / REPORTED / CLOSED
4. MLRO are la dispoziție **48 ore** pentru investigare inițială și decizie:
   - **CLOSE**: Suspiciune neconfirmată — motivul documentat complet
   - **MONITOR**: Monitorizare sporită — plan de monitorizare definit cu duration și triggers
   - **REPORT**: Raportare externă la FIU — SAR extern completat și trimis
5. **Chiar dacă MLRO decide să NU raporteze extern, motivul trebuie documentat complet** — regulatorii verifică deciziile de ne-raportare la fel de riguros ca raportările.

### Pas 4: Investigarea internă de către MLRO — protocol detaliat
1. Colectați TOATE informațiile disponibile:
   - Istoricul complet al tranzacțiilor (depozite, retrageri, transferuri interne)
   - Dosarul KYC complet (documente, screening results, istoricul verificărilor)
   - Istoricul de joc (sesiuni, mize, câștiguri, pierderi, tipuri de jocuri)
   - Device/IP data (dispozitive folosite, locații, VPN detection)
   - Comunicările cu clientul (suport, email, chat)
   - Alertele anterioare asociate contului
2. Verificați clientul pe:
   - Liste sancțiuni: OFAC SDN, UN, EU, HM Treasury — tool: ComplyAdvantage/World-Check
   - PEP databases — verificați dacă statusul s-a schimbat de la ultima verificare
   - Adverse media — căutare manuală + automată pe cuvinte cheie
3. Analizați comportamentul față de profilul de risc:
   - Există deviații semnificative de la pattern-ul stabilit?
   - Compariți cu segmentul similar de clienți (peer analysis)
   - Evaluați gravitatea și consistența red flags
4. Dacă necesitați informații suplimentare de la client — **NU dezvăluiți investigația**:
   - Formulare neutre: "Avem nevoie să actualizăm informațiile KYC" sau "Verificare de securitate de rutină"
   - NU: "Investigăm tranzacțiile dvs." sau "Avem suspiciuni"
5. Documentați cronologic fiecare pas al investigației: ce informații ați verificat, ce ați descoperit, cum ați interpretat.
6. Consultați legal/compliance extern dacă cazul este complex sau implică sume mari (>€50,000).
7. **Timeline maximă investigație**: 30 zile. Dacă nu puteți concluziona în 30 zile, raportați la FIU cu informațiile disponibile și continuați investigația.

### Pas 5: Raportarea externă — SAR/STR per jurisdicție
1. Dacă MLRO decide raportare externă, completați SAR-ul pe portalul autorității relevante:

   | Jurisdicție | Autoritate FIU | Portal | Format | Termen |
   |-------------|---------------|--------|--------|--------|
   | UK | NCA | UKFIU SAR Online (ukfinancialintelligence.org) | Web form | Prompt — pre-tranzacție pt Consent SAR |
   | Malta | FIAU | goAML (goa.fiau.org.mt) | goAML XML | "Without delay" — practice: 24-48h |
   | Germania | FIU DE | goAML DE (fiu-gateway.de) | goAML XML | "Unverzüglich" (immediately) |
   | România | ONPCSB | Portal ONPCSB (raportare.onpcsb.ro) | Formular electronic | 10 zile de la identificare |
   | Curaçao | FIU Curaçao (MOT) | MOT portal (motcuracao.org) | Web form | 14 zile |
   | Kahnawake | FINTRAC | FINTRAC portal (fintrac-canafe.gc.ca) | Web XML | 30 zile STR / 5 zile terrorist property |
   | Paraguay | SEPRELAD | Portal SEPRELAD | Formulario ROS | 48h |
   | Kenya | FRC | FRC Kenya portal (frc.go.ke) | Web form | 7 zile working days |

2. **Câmpuri SAR extern obligatorii** (common denominators toate jurisdicțiile):
   - Identificarea completă a reporting entity (operator)
   - Identificarea completă a subiectului (name, DOB, nationality, address, account)
   - Descrierea detaliată a activității suspecte (narrative — cât mai detaliat posibil)
   - Valoarea totală a tranzacțiilor relevante
   - Perioada de activitate suspectă
   - Tipologia suspectată
   - Acțiuni deja luate (cont blocat, fonduri înghețate etc.)
   - Documente suport atașate

3. **Tipping-off prevention** — reguli penale per jurisdicție:
   - **UK**: Section 333A POCA 2002 — max 2 ani închisoare + amendă nelimitată
   - **Malta**: PMLA Art. 29 — max 4 ani închisoare + €1.2M amendă
   - **Germania**: §46 GwG — amendă administrativă + penală
   - **România**: Legea 129/2019 Art. 35 — 3 luni-5 ani închisoare
   - **Instrucție universală echipa suport**: "Dacă clientul întreabă de ce contul este restricționat, răspundeți: 'Verificare de securitate de rutină. Vă vom contacta când procesul este finalizat.'"

4. **UK Consent SAR** (specific):
   - Dacă suspectați dar trebuie să executați o tranzacție (ex. retragere pending): solicitați CONSENT NCA ÎNAINTE
   - Moratorium: 7 zile lucrătoare — dacă NCA nu răspunde, puteți proceda ("deemed consent")
   - Dacă NCA refuză consent: NU executați tranzacția; puteți primi extensie moratorium de 31 zile
   - Documentați: data solicitării consent, referința NCA, răspunsul

5. Păstrați copie a SAR-ului trimis și orice confirmare/referință — retenție: minim 5 ani (7 ani Kenya).

### Pas 6: Acțiuni post-raportare și monitorizare continuă
1. După SAR, continuați monitorizarea contului — **nu închideți contul automat** dacă aceasta ar:
   - Alerta clientul (tipping off)
   - Compromite o investigație în curs a autorităților
   - Împiedica autoritățile să urmărească fluxurile financiare
2. Decideți "exit strategy" (închidere cont) în coordonare cu MLRO + legal:
   - Dacă autoritățile cer menținerea contului activ → mențineți
   - Dacă nu există instrucțiuni → evaluați riscul de a menține cont activ vs. riscul de tipping off
   - Documentați decizia cu rațional complet
3. **Production Orders / Court Orders**: Dacă autoritățile solicită informații suplimentare:
   - Furnizați tot ce solicită în termenul specificat
   - Contactați legal IMEDIAT la primirea cererii
   - NU informați clientul despre cererea autorității
4. Actualizați profilul de risc al clientului: MINIMUM HIGH RISK după orice SAR.
5. **Lessons learned** după fiecare caz:
   - A funcționat sistemul de alertare? (alert automat vs. detecție manuală)
   - A fost procesul urmat corect? (SLA-uri respectate?)
   - Ce îmbunătățiri sunt necesare? (reguli noi, threshold ajustări)
   - Documentați și implementați îmbunătățirile

### Pas 7: Penalități de referință și context afiliat (SMSads)
1. **Penalități reale pentru eșecuri STR/SAR**:
   - **Entain (UK, 2022)**: £17M — inter alia, eșecul de a raporta tranzacții suspecte ale VIP-urilor
   - **William Hill (UK, 2023)**: £19.2M — eșecuri AML inclusiv monitorizare tranzacții inadecvată
   - **Betway (UK, 2020)**: £11.6M — acceptarea depozitelor fără verificarea sursei fondurilor
   - **Genesis Global (Malta, 2024)**: Suspendare licență — AML deficiencies multiple
   - **Kindred (Suedia, 2023)**: SEK 100M (~€9M) — non-compliance cu cerințele RG+AML
   - **Tipico (Germania, 2023)**: €4.7M — GwG non-compliance
   - **Kenya**: BCLB poate suspenda fără preaviz + FRC poate impune amenzi + referire penală (POCAMLA s.15)

2. **Context afiliat — responsabilități STR**:
   - Afiliații nu raportează direct la FIU (nu au obligație STR directă)
   - DAR: afiliații trebuie să raporteze operatorului orice activitate suspectă observată în campaniile lor:
     - Trafic masiv din IP-uri asociate cu fraud rings
     - Conversii anormal de rapide (cont creat → depozit major în <5 min de la click)
     - Aceleași date de contact (email, telefon) pe multiple conturi
   - **SMSads specific**: Dacă o campanie SMS generează un spike anormal de conturi noi cu depozite mari rapide, OPRIȚI campania și investigați împreună cu operatorul
   - Documentați toate raportările la operator cu dată, conținut, răspunsul operatorului

### Pas 8: Metrici de performanță și raportare management
1. Generați **raport lunar STR** cu indicatorii:
   - Număr alerte automate generate (per regulă/categorie)
   - Număr alerte review-uite manual
   - Număr SAR-uri interne primite
   - Număr SAR-uri externe trimise la FIU
   - Rata false positive vs. true positive
   - Timp mediu de la alertă la decizie MLRO
   - Timp mediu de la decizie la raportare externă
   - Acțiuni luate post-SAR (conturi blocate, fonduri înghețate)
2. **Red flags de performanță** (indicatori că sistemul NU funcționează):
   - 0 SAR-uri pe trimestru la volum >1,000 conturi active → sistemul este prea relaxat
   - >50% false positive → threshold-urile sunt prea sensibile, risc de alert fatigue
   - Timp mediu de la alertă la decizie >7 zile → SLA încălcate sistematic
3. Prezentați raportul Board-ului trimestrial și MLRO-ului lunar.
4. Includeți recomandări de îmbunătățire în fiecare raport.

## Verificare
- [ ] Reguli de alertare automată configurate per categorie (A-F) și testate
- [ ] Threshold-uri setate per jurisdicție conform tabelului
- [ ] Typologies Register creat și populat cu 7+ tipologii
- [ ] Formular SAR Intern creat și accesibil echipei (canal securizat)
- [ ] Registru Investigații Interne funcțional (RBAC, acces restricționat MLRO)
- [ ] SLA 48h MLRO documentat și comunicat
- [ ] Protocol investigare MLRO documentat (7 pași)
- [ ] Autoritate FIU identificată per jurisdicție cu portal și termen
- [ ] Procedură tipping-off prevention comunicată tuturor angajaților (cu penalități)
- [ ] Proces Consent SAR (UK) documentat
- [ ] Retenție SAR-uri configurată (5-7 ani per jurisdicție)
- [ ] Procedură exit strategy documentată
- [ ] Raport lunar STR template creat cu indicatorii definiți
- [ ] Protocol raportare afiliat-operator pentru activitate suspectă (SMSads)
- [ ] Lessons learned process documentat

## Instrumente
- **goAML Portal** — raportare FIU: Malta, Germania, alte jurisdicții (standard UNODC)
- **UKFIU SAR Portal** — ukfinancialintelligence.org (raportare NCA UK)
- **ONPCSB România** — raportare.onpcsb.ro
- **FINTRAC** — fintrac-canafe.gc.ca (Canada/Kahnawake)
- **SEPRELAD Paraguay** — portal electronic
- **FRC Kenya** — frc.go.ke
- **Chainalysis KYT** — blockchain transaction monitoring
- **Elliptic Lens** — blockchain analytics
- **Crystal Blockchain** — crypto compliance
- **ComplyAdvantage** — real-time AML screening și monitoring continuu
- **NICE Actimize** — enterprise transaction monitoring
- **Featurespace ARIC** — AI-powered transaction monitoring
- **FATF Typologies Reports**: fatf-gafi.org/publications/methodsandtrends

## Note
- **FATF R.20**: Raportarea promptă la FIU este obligație legală — întârzierile pot atrage sancțiuni penale pentru MLRO personal (nu doar pentru companie).
- **Raportarea de bună-credință** oferă protecție legală completă (safe harbour) — nu există răspundere pentru "raportare incorectă" dacă suspiciunea a fost rezonabilă. NU vă temeți să raportați.
- **Pattern-uri "low and slow"**: În iGaming, spălarea este adesea pe sume mici pe perioade lungi — configurați alerte pe 30/90/180 zile, nu doar zilnice.
- **Multi-operator laundering**: Aceleași fonduri pot fi "spălate" prin mai mulți operatori — dacă aveți acces la date cross-operator (ex. prin scheme de self-exclusion sau grup corporativ), folosiți-le.
- **Post-Brexit**: UK și UE au liste de sancțiuni separate — screeniți față de ambele dacă operați în ambele jurisdicții.
