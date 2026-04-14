---
id: IGC-008
title: GDPR Data Handling for Gambling Operations
domain: igaming-compliance
source: AML/KYC Diploma (Udemy) + GDPR Art. 5-9, 12-22, 25, 30, 32-34 + ICO Guidance + ANSPDCP + ePrivacy Directive
version: 2.0
forge_score: "estimated H/3.8"
business_mapping: ["smsads", "gambling-seo"]
---

# GDPR Data Handling for Gambling Operations

## Obiectiv
Implementați un framework GDPR complet adaptat specificului iGaming — unde se colectează date biometrice (KYC liveness), financiare (tranzacții, conturi bancare), comportamentale (sesiuni joc, mize), și de sănătate indirectă (RG risk scores, self-exclusion) — asigurând conformitate cu GDPR, ePrivacy Directive, și legislația de protecția datelor per jurisdicție, cu capacitatea de a răspunde la orice DSAR, audit sau investigație regulatorie.

## Pași

### Pas 1: Data Mapping complet — Record of Processing Activities (ROPA)
1. Creați un **ROPA** (GDPR Art. 30 — obligatoriu pentru orice entitate cu >250 angajați SAU procesare de categorii speciale de date, ceea ce include TOȚI operatorii iGaming):

   | Categorie date | Exemple | Scop | Baza legală (Art. 6/9) | Retenție | Locație stocare | Terți destinatari | Securitate |
   |---------------|---------|------|----------------------|---------|----------------|-------------------|-----------|
   | **Identificare** | Nume, DOB, adresă, naționalitate, CNP | KYC/AML | Art. 6.1.c (obligație legală) | 5 ani post-închidere cont (7 Kenya) | EU servers, encrypted | Sumsub/Onfido (DPA), regulator | AES-256, RBAC |
   | **Financiare** | IBAN, card number, istoric tranzacții | Furnizare serviciu + AML | Art. 6.1.b (contract) + Art. 6.1.c (AML) | 5-7 ani per jurisdicție | EU servers, PCI DSS | PSP (Nuvei etc.), bancă, regulator | AES-256, tokenizare card |
   | **Biometrice** | Selfie, liveness check video | KYC verification | Art. 9.2.a (consimțământ explicit) SAU Art. 9.2.g (substantial public interest) | 5 ani post-închidere | Furnizor KYC (DPA obligatoriu) | Sumsub/Onfido | Criptare individuală |
   | **Comportamentale joc** | Sesiuni, mize, câștiguri, jocuri preferate | Furnizare serviciu + RG monitoring | Art. 6.1.b (contract) + Art. 6.1.f (interes legitim pt RG) | Durata cont + 5 ani | Platform DB, encrypted | Regulator la cerere | Pseudonimizare analytics |
   | **RG / Sănătate indirectă** | Risk score, self-exclusion status, interacțiuni RG | Protecția jucătorului | Art. 9.2.g (substantial public interest) sau Art. 6.1.f (LI) cu documentare LIA | Indefinit (self-exclusion); 5 ani (interacțiuni) | Platform DB, access restricționat | Scheme self-exclusion (GAMSTOP etc.) | RBAC, audit log |
   | **Marketing** | Email, telefon, preferințe, click behavior | Marketing direct | Art. 6.1.a (consimțământ) | Până la retragere consimțământ + 30 zile processing | CRM, email platform | Mailchimp/Brevo (DPA) | Encrypt in transit |
   | **Tehnice** | IP, device fingerprint, cookies, session logs | Securitate + fraud prevention | Art. 6.1.f (interes legitim) | 2 ani (logs), per cookie policy (cookies) | Server logs, CDN | Security tools | Log rotation, access control |

2. Per categorie documentați OBLIGATORIU:
   - Scopul exact al procesării
   - Baza legală (cu justificare — nu doar "legitimate interest" fără LIA)
   - Durata de retenție exactă cu referință legală
   - Locația stocării (țară, furnizor)
   - Terții care primesc datele (cu baza legală a transferului)
   - Măsurile de securitate aplicate

3. **Transferuri internaționale de date** (GDPR Art. 44-49):
   - Dacă folosiți cloud US (AWS, Google Cloud, Azure): necesitați mecanism de transfer valid
   - Post-Schrems II: Standard Contractual Clauses (SCCs) + Transfer Impact Assessment (TIA)
   - Alternative: hosting exclusiv UE, Binding Corporate Rules (BCR)
   - Verificați că furnizorul KYC (Sumsub etc.) procesează datele în UE — dacă nu, SCC obligatoriu

4. Actualizați ROPA la ORICE modificare a procesării — este document VIU, nu static. Desemnați un owner (DPO sau Compliance) responsabil de actualizare.

### Pas 2: Bazele legale per tip de date — documentare detaliată
1. **Executarea contractului (Art. 6.1.b)**:
   - Date necesare pentru furnizarea serviciului: cont, autentificare, tranzacții, activitate de joc, asistență clienți
   - NU necesită consimțământ separat — dar trebuie informat jucătorul (Privacy Policy)
   - Limitare: nu puteți extinde procesarea pe baza contractului dincolo de ce este necesar pentru furnizarea serviciului

2. **Obligație legală (Art. 6.1.c)**:
   - Date AML/KYC: colectare și retenție obligatorie prin lege (6AMLD, POCA, GwG, Legea 129/2019)
   - Date RG: unele jurisdicții (UKGC) impun colectarea datelor comportamentale pentru protecția jucătorului
   - NU necesită consimțământ — dar INFORMAȚI jucătorul
   - **IMPORTANT**: Nu folosiți consimțământul ca bază pentru AML/KYC — jucătorul nu poate "refuza" verificarea AML

3. **Interes legitim (Art. 6.1.f)**:
   - Fraud detection, securitate IT, analytics intern, prevenire bonus abuse, RG monitoring
   - **Obligatoriu**: Legitimate Interest Assessment (LIA) per activitate de procesare
   - **LIA template** pentru iGaming:
     ```
     LEGITIMATE INTEREST ASSESSMENT
     ===============================
     Processing activity: [ex: Player behavior monitoring for RG]
     Purpose: [ex: Identify players at risk of gambling harm]
     Is LI the appropriate basis? [Yes — necessity for player protection]
     Necessity test: [Could purpose be achieved with less data? No —
       behavioral patterns require continuous monitoring]
     Balancing test: [Player's rights vs. operator's interest]
       - Impact on individual: Moderate (monitoring without explicit consent)
       - Reasonable expectation: Yes — players expect operators to monitor for safety
       - Safeguards: Pseudonymization, access control, opt-out for non-essential analytics
     Decision: LI is appropriate with safeguards applied.
     Reviewer: [DPO name] | Date: [DD/MM/YYYY]
     ```

4. **Consimțământ (Art. 6.1.a)**:
   - NUMAI pentru: marketing opțional (email, SMS, push), cookies non-esențiale (analytics, advertising), newsletter-uri
   - Cerințe consimțământ GDPR: liber, specific, informat, neambiguu, acțiune pozitivă (no pre-ticked boxes)
   - Drept de retragere ușoară (la fel de ușor ca acordarea) — documentați mecanismul

5. **Categorii speciale (Art. 9)** — date biometrice și RG:
   - Date biometrice (selfie liveness): Art. 9.2.a (consimțământ explicit) — obțineți consimțământ separat la KYC
   - Date RG (risk score, self-exclusion): Art. 9.2.g (substantial public interest) sau Art. 9.2.j (research/statistics cu safeguards) — documentați baza legală cu referință la legislația gambling specifică
   - NICIODATĂ nu procesați date Art. 9 fără baza legală documentată explicit

6. **ePrivacy Directive** (complement GDPR, relevant pentru SMSads):
   - SMS/email marketing: necesită consimțământ explicit SEPARAT de consimțământul GDPR general
   - Soft opt-in (customer relationship existing): permis în unele jurisdicții pentru produse similare — NU presupuneți că se aplică la gambling

### Pas 3: Drepturile jucătorilor (Data Subject Rights) — implementare completă
1. **Dreptul de acces (Art. 15)** — DSAR (Data Subject Access Request):
   - SLA: max 30 zile calendaristice (extensie +60 zile pentru cereri complexe, cu notificare)
   - Furnizați:
     - Categoriile de date procesate
     - Scopul procesării per categorie
     - Destinatarii datelor (terți)
     - Durata retenție
     - Drepturile jucătorului (ștergere, rectificare, portabilitate, opoziție)
     - Sursa datelor (dacă nu de la jucător direct)
     - Existența deciziilor automatizate (inclusiv profiling)
   - Format: document structurat (PDF recomandat)

2. **Dreptul la rectificare (Art. 16)**: Corectați datele incorecte la cerere — SLA: 30 zile. Notificați terții care au primit datele (Art. 19).

3. **Dreptul la ștergere / "Right to be Forgotten" (Art. 17)**:
   - **ATENȚIE — LIMITARE iGaming**: datele AML/KYC NU pot fi șterse înainte de expirarea perioadei legale (5-7 ani)
   - **Ce PUTEȚI șterge la cerere**: date de marketing, preferințe joc (dacă nu necesare AML), cookies, analytics data, comunicări opționale
   - **Ce NU puteți șterge**: documente KYC, istoric tranzacții, SAR-uri, self-exclusion records, date necesare obligației legale
   - **Response template**:
     ```
     Dear [Name],
     We have processed your erasure request. The following data has been deleted:
     - Marketing preferences and communication history
     - Cookie data and browsing analytics
     - [Other applicable categories]

     The following data is retained as required by law:
     - KYC/AML records (retained under [6AMLD/POCA/etc.] for 5 years
       after account closure)
     - Transaction history (same legal basis)
     - Self-exclusion records (retained for player protection)

     If you have questions, contact our DPO at [dpo@operator.com].
     ```

4. **Dreptul la portabilitate (Art. 20)**: Furnizați datele în format machine-readable (JSON sau CSV). Aplicabil NUMAI datelor bazate pe consimțământ sau contract. SLA: 30 zile.

5. **Dreptul la restricționare (Art. 18)**: "Freeze" procesarea la contestare — mențineți datele dar nu le procesați activ. Durata: până la rezoluție.

6. **Dreptul la opoziție (Art. 21)**:
   - Marketing direct: opriți IMEDIAT la opoziție — fără excepție
   - Procesare pe interes legitim: evaluați dacă aveți "compelling legitimate grounds" care prevalează

7. **Dreptul de a nu fi supus deciziilor automatizate (Art. 22)**:
   - Relevant dacă folosiți AI/algorithmic decisions: risk scoring automat, bonus eligibility, self-exclusion automată
   - Jucătorul are dreptul la intervenție umană, exprimarea punctului de vedere, contestare decizie
   - Documentați TOATE procesele de decizie automatizată cu impact pe jucător

8. **Portal DSAR dedicat**: email dpo@[operator].com sau formular web. Documentați TOATE cererile:
   - Data primirii
   - Tip cerere (acces, ștergere, rectificare etc.)
   - Data răspunsului
   - Ce s-a furnizat/efectuat
   - Motivul refuzului (dacă refuzat — parțial sau total)

### Pas 4: Securitatea datelor — cerințe tehnice și organizatorice (GDPR Art. 32)
1. **Pseudonimizare** (Art. 32.1.a):
   - Datele analitice și comportamentale: separați ID-ul jucătorului de datele personale
   - Analytics intern: folosiți pseudonime sau ID-uri hash, nu date reale
   - RG risk scoring: pseudonimizat în rapoartele agregate

2. **Criptare** (Art. 32.1.a):
   - At-rest: AES-256 pentru toate datele personale
   - In-transit: TLS 1.3 minim (TLS 1.2 acceptat cu cipher suites puternice)
   - Documente KYC: criptare individuală per document cu cheie per-user
   - Card data: tokenizare (PCI DSS Level 1 dacă procesați direct)

3. **Access control** (Art. 32.1.b):
   - RBAC (Role-Based Access Control): fiecare angajat accesează NUMAI datele necesare rolului
   - Principiul "need to know"
   - Revizuire access rights lunar
   - Multi-Factor Authentication (MFA) obligatoriu pentru accesul la date personale
   - Privileged access management (PAM) pentru admin accounts

4. **Audit logs** (Art. 32.1.d):
   - Loguri complete: cine a accesat ce date, când, de pe ce device
   - Retenție logs: minim 2 ani
   - Loguri imutabile (append-only / WORM)
   - Alertare la accesuri anormale (ex: volume access, after-hours, bulk export)

5. **Backup și disaster recovery**:
   - Backup zilnic minim (recomandat: continuu/incremental)
   - Test de restore trimestrial documentat
   - RTO (Recovery Time Objective): definit și testat
   - RPO (Recovery Point Objective): definit și testat
   - Backup-uri criptate, stocate separat de producție

6. **Data breach notification** (GDPR Art. 33-34):
   - **La DPA (Data Protection Authority)**: max 72h de la conștientizarea breach-ului dacă există risc pentru persoane
   - **La persoanele afectate**: "without undue delay" dacă breach prezintă "high risk" pentru drepturile lor
   - DPA per jurisdicție:
     - UK: ICO (ico.org.uk) — notificare online
     - Malta: IDPC (idpc.org.mt)
     - România: ANSPDCP (dataprotection.ro)
     - Germania: LfDI per Land
   - **Breach response template**:
     ```
     DATA BREACH INCIDENT REPORT
     ============================
     Date discovered: [DD/MM/YYYY HH:MM]
     Date occurred (est.): [DD/MM/YYYY]
     Category of breach: [Confidentiality / Integrity / Availability]
     Description: _______________
     Data affected: [categories + volume]
     Individuals affected: [number, estimated]
     Risk level: [Low / Medium / High]
     DPA notification required: [Yes / No — with justification]
     Individual notification required: [Yes / No]
     Containment actions taken: _______________
     Root cause: _______________
     Remediation: _______________
     DPO sign-off: _______________ Date: ___
     ```
   - Documentați FIECARE incident (chiar dacă nu necesită notificare) în **Breach Register**.

### Pas 5: Cookie compliance, marketing GDPR și ePrivacy — SMSads focus
1. **Cookie Consent Management Platform (CMP)**:
   - Implementați CMP certificat IAB TCF 2.2: OneTrust, Cookiebot, Usercentrics
   - Categorii obligatorii:
     - **Esențiale** (fără consimțământ): autentificare, securitate, load balancing
     - **Funcționale** (consimțământ): preferințe limbă, remember login
     - **Analitice** (consimțământ): Google Analytics, Hotjar, session recording
     - **Marketing** (consimțământ): retargeting pixels, conversion tracking, social media
   - Granularitate: jucătorul poate accepta/refuza per categorie
   - Rejectare la fel de ușoară ca acceptarea (no dark patterns)

2. **Email marketing GDPR**:
   - Opt-in explicit (acțiune pozitivă, no pre-ticked boxes)
   - Double opt-in recomandat (confirmare email)
   - Unsubscribe funcțional în fiecare email (link clar, nu ascuns)
   - Procesare unsubscribe: max 10 zile lucrătoare (target: imediat)
   - Identitate sender clar (nu "noreply@" generic)

3. **SMS marketing (SMSads — atenție specială)**:
   - **Consimțământ SMS separat**: nu presupuneți că opt-in-ul email acoperă SMS
   - **ePrivacy Directive**: SMS = "electronic mail" — necesită consimțământ prealabil
   - **PECR (UK)**: Privacy and Electronic Communications Regulations — consent explicit pentru SMS marketing
   - **Opt-out**: Reply STOP trebuie să funcționeze imediat
   - **Identificare sender**: număr de telefon sau shortcode identificabil, nu anonim
   - **Suppress lists**: verificați ÎNAINTE de fiecare campanie:
     - Self-exclusion suppress list (GAMSTOP etc.)
     - Marketing opt-out suppress list
     - DNC (Do Not Call) registers per jurisdicție
   - **Frecvență**: max 1 SMS/săptămână per destinatar (best practice)
   - **Orar**: nu trimiteți 21:00-09:00
   - **Record keeping**: păstrați dovada consimțământului per destinatar: data, canal, text consimțământ, IP (dacă online)

4. **Suppress lists** (suppress > send):
   - Mențineți suppress list unificată cu:
     - Jucători self-excluși (surse: intern + GAMSTOP/OASIS/Spelpaus)
     - Jucători care au dat unsubscribe (email + SMS + push — separat)
     - Jucători care au exercitat dreptul de opoziție (Art. 21)
   - Verificați suppress list ÎNAINTE de fiecare send — nu după
   - Sincronizați cu CRM/email platform/SMS platform zilnic

### Pas 6: Data retention per jurisdicție — tabel reconciliat GDPR + AML
1. **Tabel de retenție** (regula: cea mai lungă perioadă aplicabilă prevalează):

   | Categorie date | GDPR max (purpose limitation) | AML obligatoriu | Jurisdicție specifică | Perioadă aplicată |
   |---------------|------------------------------|----------------|----------------------|------------------|
   | KYC documents | Duration of purpose | 5 ani post-cont (6AMLD) | Kenya: 7 ani (POCAMLA) | 5 ani (7 Kenya) |
   | Transaction history | Duration of purpose | 5 ani (6AMLD) | Kenya: 7 ani | 5 ani (7 Kenya) |
   | SAR records | N/A (legal obligation) | 5 ani post-SAR | - | 5 ani |
   | Self-exclusion | Indefinit (player protection) | N/A | UKGC: indefinit | Indefinit |
   | Marketing preferences | Until withdrawal + 30 days | N/A | - | Until withdrawal + 30 days |
   | Session/game logs | 2 ani (security) | 5 ani dacă relevant AML | UKGC: 5 ani | 5 ani |
   | Cookie data | Per cookie policy (max 13 luni ePrivacy) | N/A | - | Per cookie policy |
   | Audit logs | 2 ani | 5 ani | - | 5 ani |
   | IP/device data | 2 ani (security) | 5 ani dacă relevant AML | - | 2-5 ani |

2. Implementați politici de retenție AUTOMATE cu alertă de ștergere la expirare.
3. NU ștergeți automat dacă:
   - Există investigație activă (SAR, regulator, legal)
   - Există DSAR pending
   - Există litigiu activ sau anticipat (litigation hold)

### Pas 7: DPO, DPIA, training și documentație continuă
1. **DPO (Data Protection Officer)** — Art. 37:
   - Recomandabil pentru TOȚI operatorii iGaming (procesare la scară mare de categorii speciale)
   - Cerințe: independent, raportează direct la Board, nu poate fi penalizat pentru exercitarea funcției
   - Înregistrați DPO la DPA națională (ICO UK, IDPC Malta, ANSPDCP România)
   - Publicați datele de contact DPO pe site (Privacy Policy + pagină de contact)

2. **DPIA (Data Protection Impact Assessment)** — Art. 35:
   - OBLIGATORIU pentru:
     - Procesare la scară mare de categorii speciale (biometrice, sănătate/RG)
     - Profilare sistematică (player risk scoring, behavioral tracking)
     - Monitorizare sistematică la scară mare (transaction monitoring)
   - Efectuați DPIA ÎNAINTE de lansarea oricărui feature nou care implică procesare de date sensibile
   - Consultați DPO în procesul DPIA
   - Mențineți registru DPIA-uri efectuate

3. **Privacy by Design & Default** (Art. 25):
   - Orice feature nou: DPIA gate obligatoriu
   - Default settings: minimum data collection (nu opt-out, ci opt-in pentru non-esențiale)
   - Data minimization: nu colectați date pe care nu le folosiți

4. **Training GDPR**:
   - Toți angajații cu acces la date personale: training la angajare + anual
   - Conținut: ce este GDPR, categorii de date, drepturi jucători, breach reporting, security practices
   - Evaluare: test obligatoriu (pass mark 80%)
   - Documentați: data, participanți, conținut, scor

5. **Privacy Policy & Cookie Policy**:
   - Revizuiți anual sau la orice modificare semnificativă
   - Versiuni vechi: arhivate cu data de valabilitate
   - Limbaj clar și accesibil (nu legalese impenetrabil)
   - Disponibilă în limba fiecărei jurisdicții active
   - Menționează explicit: categoriile de date, bazele legale, duratele de retenție, terții, drepturile, DPO contact

### Pas 8: Penalități GDPR și context afiliat
1. **Penalități GDPR de referință** (iGaming și adjacent):
   - **Betsson (Suedia, 2023)**: SEK 19.2M — deficiențe protecția datelor
   - **Kindred (Suedia, 2023)**: SEK 100M — parțial GDPR non-compliance
   - **Generic GDPR**: max 4% din cifra de afaceri anuală globală sau €20M (Art. 83.5)
   - **UK (ICO)**: max £17.5M sau 4% turnover (UK GDPR post-Brexit)

2. **Context afiliat (SMSads)**:
   - Afiliatul este Controller sau Processor?
     - Dacă afiliatul deține baza de date de contacte (numere telefon) = **Controller** → obligații GDPR complete
     - Dacă afiliatul trimite campanii pe baza datelor furnizate de operator = **Processor** → DPA obligatoriu cu operatorul
   - **Controller afiliat**: trebuie să aibă Privacy Policy proprie, consimțământ propriu, DSAR capability
   - **Record consimțământ SMS**: păstrați dovada (timestamp, text, sursa) — regulatorul poate solicita
   - **DPA cu operatorul**: dacă partajați date de jucători, aveți DPA (Data Processing Agreement) semnat cu fiecare operator

## Verificare
- [ ] ROPA completat cu toate 7 categoriile de date (Art. 30)
- [ ] Baze legale documentate per categorie (cu LIA unde aplicabil)
- [ ] Transferuri internaționale evaluate — SCC/adequacy decision documentate
- [ ] Portal DSAR funcțional (email + formular)
- [ ] SLA 30 zile DSAR documentat și testat
- [ ] Template răspuns ștergere (cu limitarea AML) creat
- [ ] Criptare at-rest (AES-256) și in-transit (TLS 1.3) implementată
- [ ] Access control RBAC configurat și auditat lunar
- [ ] Audit logs imutabile implementate (2 ani retenție)
- [ ] CMP implementat și configurabil per categorie cookie
- [ ] Consimțământ SMS separat implementat (SMSads)
- [ ] Suppress list unificată sincronizată zilnic cu toate platformele
- [ ] Breach notification procedure documentată (72h la DPA)
- [ ] Breach Register creat
- [ ] Data retention tabel per jurisdicție implementat cu automație
- [ ] DPIA efectuat pentru procesări de risc ridicat
- [ ] DPO desemnat și înregistrat la DPA
- [ ] Privacy Policy și Cookie Policy revizuite și actuale
- [ ] Training GDPR completat pentru personalul relevant
- [ ] DPA (Data Processing Agreement) semnat cu toți furnizorii terți

## Instrumente
- **OneTrust** (onetrust.com) — CMP + GDPR management platform
- **Cookiebot** (cookiebot.com) — CMP accesibilă
- **Usercentrics** (usercentrics.com) — CMP cu TCF 2.2
- **TrustArc** (trustarc.com) — enterprise GDPR
- **ICO** (ico.org.uk) — UK DPA + DSAR guidance + breach reporting
- **ANSPDCP** (dataprotection.ro) — DPA România
- **IDPC Malta** (idpc.org.mt) — DPA Malta
- **BfDI Germania** (bfdi.bund.de) — DPA federală Germania
- **EDPB Guidelines** (edpb.europa.eu) — ghiduri oficiale UE
- **Mailchimp / Brevo** — suppress list management
- **Vaultree** / **Baffle** — encryption-in-use (emerging tech)

## Note
- **Conflictul AML vs. GDPR** (retenție 5 ani vs. drept ștergere): rezolvare — mențineți datele AML pe baza obligației legale (Art. 6.1.c), informați jucătorul CLAR că nu pot fi șterse, dar puteți șterge datele de marketing/comportamentale non-AML.
- **Transferul datelor KYC la Sumsub/Onfido**: necesită DPA (Data Processing Agreement) semnat, verificare că furnizorul este GDPR-compliant și procesează în UE (sau SCC dacă non-UE).
- **SMS marketing (SMSads)**: triple compliance — (1) GDPR consent (2) ePrivacy/PECR consent (3) gambling advertising rules locale. Verificați TOATE TREI.
- **UK post-Brexit**: UK GDPR (Data Protection Act 2018) este substantiv similar cu EU GDPR dar separat. Dacă operați în UK + EU, trebuie compliance cu AMBELE.
- **Kenya Data Protection Act 2019**: similar cu GDPR, operat de ODPC (Office of the Data Protection Commissioner). Consent obligatoriu, breach notification, data localization provisions.
