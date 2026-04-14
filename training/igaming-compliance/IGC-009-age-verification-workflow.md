---
id: IGC-009
title: Age Verification and Access Control Workflow
domain: igaming-compliance
source: Responsible Gambling and Player Protection (Udemy) + UKGC LCCP SRC 3.2 + MGA PPD + GGL GlüStV + Kenya BCLB Regs
version: 2.0
forge_score: "estimated H/3.8"
business_mapping: ["smsads", "gambling-seo"]
---

# Age Verification and Access Control Workflow

## Obiectiv
Implementați un workflow robust de verificare a vârstei și control al accesului care previne COMPLET accesul minorilor la serviciile de gambling, în conformitate cu cerințele UKGC (SRC 3.2 — age verification BEFORE gambling), MGA, GGL, și alți regulatori, minimizând fricțiunea pentru adulții legitimi și rezistând la tentative de bypass (inclusiv documente falsificate și deepfakes).

## Pași

### Pas 1: Cerințe de vârstă per jurisdicție — tabel complet
1. Documentați vârsta legală de gambling per jurisdicție:

   | Jurisdicție | Vârstă minimă | Când se verifică | Regulator | Specificități |
   |-------------|--------------|------------------|-----------|--------------|
   | **UK** | 18 | ÎNAINTE de orice depozit sau joc cu bani reali (SRC 3.2.1) | UKGC | No "72h window" — AV obligatoriu pre-depozit din 2019 |
   | **Malta** | 18 | La înregistrare sau înainte de prima depunere (72h provisional play permis) | MGA | Provisional play: numai cu depozit max €150 |
   | **Germania** | 18 | La înregistrare — OBLIGATORIU | GGL | LUVAS centralizat; verificare electronică preferată |
   | **România** | 18 | La înregistrare — OBLIGATORIU (CI + CNP) | ONJN | Verificare online CI via DEPABD database |
   | **Suedia** | 18 | La înregistrare (BankID obligatoriu) | Spelinspektionen | BankID = age verification automată |
   | **Olanda** | 18 | La înregistrare | KSA | DigiD/iDIN verification |
   | **Curaçao** | 18 | La înregistrare sau retragere (practice variabilă) | GCB | Post-reforma 2023: verificare mai strictă |
   | **Kahnawake** | 18 | La înregistrare | KGC | FINTRAC requires age verification |
   | **Paraguay** | 18 | La înregistrare | CONAJZAR | Verificare cédula de identidad |
   | **Kenya** | 18 | La înregistrare | BCLB | Verificare National ID / M-Pesa linked |
   | **SUA (variate)** | 18-21 | Variabilă per stat | Per stat | Nevada/NJ: 21; alte state: 18 |
   | **Australia** | 18 | La înregistrare | ACMA | Verificare electronică preferată |
   | **Canada** | 18-19 | Per provincie | Per provincie | BC/ON: 19; Alberta: 18 |

2. **Regula platformei**: folosiți vârsta minimă CEA MAI STRICTĂ din jurisdicțiile active atunci când geolocația este incertă.
3. Documentați cerința de TIMING per jurisdicție — aceasta este crucială:
   - **UK (UKGC SRC 3.2.1)**: AV ÎNAINTE de ORICE depozit. "Age verification at withdrawal" NU mai este acceptat din 2019.
   - **MGA**: Provisional play window de max 72h cu max €150 depozit — dar verificarea trebuie finalizată înainte de retragere.
   - **GGL**: AV la înregistrare — nicio activitate permisă fără verificare.
   - **ONJN**: AV la înregistrare — CNP verificat electronic.

### Pas 2: Metode de verificare — implementare stratificată
1. **Nivel 1 — Self-declaration** (filtru UX, NU verificare legală):
   - La înregistrare: câmp data nașterii sau bifă "Confirm am 18+ ani"
   - INSUFICIENT ca metodă unică pentru ORICE jurisdicție serioasă
   - Rol: prim filtru + colectare DOB pentru verificare electronică

2. **Nivel 2 — Verificare Electronică Automată (EAV)** — PREFERATĂ (viteză + acuratețe):

   | Tool | Metodă | Acoperire | Timp verificare | Precizie | Cost estimat |
   |------|--------|-----------|----------------|---------|-------------|
   | **Jumio** | Document scan + AI + liveness | 200+ țări, 5,000+ doc types | <60 sec | >99% | €1-3/verificare |
   | **Onfido** | Document + liveness + data cross-check | 195+ țări, 2,500+ doc types | <90 sec | >99% | €1-2.5/verificare |
   | **Sumsub** | Document + liveness + database check | 220+ țări, 6,500+ doc types | <30 sec | >99% | €0.5-2/verificare |
   | **Yoti** | Age estimation din selfie (no document) | Global | <10 sec | ~95% (>99% pt 25+) | €0.1-0.5/verificare |
   | **Experian AV** | Credit header data match | UK, US, AU | <5 sec | >99% | Per contract |
   | **Equifax/TransUnion** | Credit bureau data | UK, US, CA | <5 sec | >99% | Per contract |
   | **GBG IDscan** | Document + data verification | Global | <60 sec | >99% | Per contract |

   - **Recomandare**: combinație Sumsub/Jumio (document + liveness) ca primar + Experian (database) ca fallback

3. **Nivel 3 — Verificare Documentară Manuală** (fallback):
   - Document scan uploadat + review manual de echipa KYC
   - Mai lent (1-24h) dar sigur
   - Folosit când EAV eșuează (document necunoscut, calitate slabă, country not covered)

4. **Nivel 4 — Verificare națională electronică** (unde disponibil):
   - **Suedia**: BankID — verificare automată 100% (vârstă + identitate)
   - **Olanda**: DigiD / iDIN — verificare automată
   - **România**: DEPABD database check (CI + CNP)
   - **Germania**: PostIdent / VideoIdent / eID

5. **Flow de verificare recomandat**:
   ```
   Înregistrare
      ↓
   Self-declaration (DOB)
      ↓
   EAV automat (Sumsub/Jumio)
      ↓
   ┌─ PASS → Cont activat complet
   ├─ INCONCLUSIVE → Database check (Experian) → PASS/FAIL
   └─ FAIL → Manual review
                ↓
          ┌─ PASS → Cont activat
          └─ FAIL → Cont blocat definitiv + notificare
   ```

6. Documentați rata de succes per nivel și optimizați continuu.

### Pas 3: Age-gating — acces restricționat pre-verificare
1. **Pre-registration age gate**:
   - Pop-up ÎNAINTE de pagina de înregistrare:
     ```
     ┌────────────────────────────────────────────┐
     │  This website contains gambling content.    │
     │  You must be 18+ to continue.              │
     │                                             │
     │  ┌─────────────────┐ ┌──────────────────┐  │
     │  │ I am 18 or over │ │ I am under 18    │  │
     │  └─────────────────┘ └──────────────────┘  │
     │                                             │
     │  For help with gambling: [helpline]         │
     └────────────────────────────────────────────┘
     ```
   - "I am under 18" → redirect la pagină informativă cu resurse (GamCare, helpline) — NU redirectați la competitor
   - Nu stocați click-ul ca dovadă legală de vârstă
   - Setați cookie (session cookie, nu persistent) pentru a nu re-afișa la fiecare pagină

2. **Restricted access mode** (pre-verificare completă):
   - UKGC: ZERO acces la joc real (nici demo cu aparență de joc real fără disclaimer clar)
   - MGA (provisional play): max €150 depozit, joc permis, dar retragere blocată
   - GGL: ZERO acces fără verificare completă
   - Afișare: "Contul dvs. este în verificare. Veți putea juca după finalizare."

3. **Blocking mechanism**:
   - Dacă AV nu finalizat în 72h (sau termenul jurisdicțional): blocare cont COMPLET
   - Fondurile depozitate (dacă MGA provisional play): disponibile pentru retragere, NU pentru joc
   - Reminder automat la 24h și 48h: "Finalizați verificarea pentru a vă activa contul"

4. **URL direct access prevention**:
   - Testați că un user neverificat NU poate accesa jocuri prin URL direct (bypass registration flow)
   - Middleware/guard pe TOATE endpoint-urile de joc care verifică: (1) autentificat (2) AV completat (3) nu self-excluded

5. **Marketing materials — age disclosure**:
   - "18+" sau "21+ where applicable" vizibil pe TOATE materialele
   - Nu în fine print — dimensiune proporțională cu materialul

### Pas 4: Anti-fraud age verification — deepfake și document fraud protection
1. **Document fraud detection** (critică în 2025+):
   - AI-powered document authentication: verificare hologramă, microtext, MRZ, NFC chip (unde disponibil)
   - Template matching: comparare cu template-ul oficial al documentului din țara emitentă
   - Metadata analysis: verificare EXIF, compression artifacts, editing traces
   - **Instrumente**: Sumsub Document Fraud Detection, Onfido Fraud Detection Signals, Jumio Informed Authentication

2. **Deepfake liveness detection**:
   - Liveness check obligatoriu: detectare față reală vs. foto imprimată, ecran, mască, video replay
   - **Generația actuală**: 3D liveness (depth sensing), texture analysis, motion analysis
   - Challenge-response: solicitare acțiune live (clipește, rotește capul, zâmbește) — mai greu de deepfake-uit
   - **Instrumente**: iProov (cel mai avansat), FaceTec (3D liveness), Sumsub Liveness, Onfido Motion
   - **Statistică**: 7% din tentativele de fraudă KYC în iGaming în 2024 au folosit documente generate AI (Sumsub Identity Fraud Report 2024)

3. **Device intelligence**:
   - Detectare emulator/simulator (indică tentativă automată de fraudă)
   - Verificare că camera este reală (nu virtual camera / video feed)
   - Browser fingerprinting: detectare dacă user-ul a mai încercat cu alt document de pe același device

4. **Manual review escalation triggers** (automat → manual):
   - Document quality score sub prag
   - Liveness confidence score sub prag
   - Discrepanță între photo ID și selfie
   - Document din țară cu rată ridicată de fraudă documentară
   - Vârstă declarată 18-19 (risc crescut — minori la limită)

### Pas 5: Prevenirea accesului minorilor la conturi parentale
1. **T&C explicit**: "Titularul contului este responsabil de prevenirea accesului minorilor la cont."
2. **Parental controls resources** — afișați în Help Center:
   - **NetNanny** (netnanny.com) — filtrare conținut
   - **Qustodio** (qustodio.com) — monitoring + filtrare
   - **BetBlocker** (betblocker.org) — GRATUIT, specific gambling
   - **Gamban** (gamban.com) — device-level gambling blocking
   - **Apple Screen Time** / **Google Family Link** — built-in parental controls
3. **Session timeout** automat: 30 minute inactivitate → logout (reduce riscul acces neautorizat)
4. **Recomandare în cont**: "Nu salvați credențialele pe dispozitive partajate" — afișat în setări de securitate
5. **Behavioral anomaly detection**: dacă pattern-ul de joc se schimbă radical (ex: de la poker la slots colorat, mize mici inconsistente cu istoricul), evaluați posibilitatea de acces minor → trigger verificare suplimentară
6. **Two-factor authentication (2FA)**: oferiți și promovați 2FA — reduce accesul neautorizat (inclusiv minori cu parola părintelui)

### Pas 6: Gestionarea cazurilor de minori identificați — protocol incident
1. **Protocol IMEDIAT la identificare minor**:

   **T+0 (IMEDIAT)**:
   - Blocați contul complet — ZERO tranzacții suplimentare
   - Marcați contul cu flag "UNDERAGE — BLOCKED"
   - NU permiteți retragerea CÂȘTIGURILOR — fondurile sunt reținute pending investigație

   **T+24h**:
   - Returnați DEPOZITELE nete (depozit total - retrageri deja efectuate) la metoda de plată originală
   - Câștigurile nete: NU se returnează jucătorului minor (sunt profit din activitate ilegală)
   - Documentați decizia financiară complet

   **T+24h**:
   - Închideți contul definitiv — marcat permanent
   - Adăugați datele la exclusion database (prevent re-registration)
   - Blocați device fingerprint asociat

   **T+48h**:
   - Raportați incidentul la regulator:
     - **UKGC**: raportare obligatorie promptă (key event). Forma: email la Compliance Manager UKGC sau via portal
     - **MGA**: raportare în cadrul compliance reporting
     - **GGL**: raportare la Glücksspielaufsichtsbehörde
     - **ONJN**: raportare la ONJN
     - **Kenya BCLB**: raportare la BCLB
   - Conținut raportare: cum a trecut de verificare, sume depozitate/câștigate/retrase, cum a fost identificat, acțiuni corective luate

2. **Root Cause Analysis (RCA)** — obligatoriu după fiecare incident:
   - De CE a eșuat sistemul de verificare?
   - Document fals? Furt identitate adult? Bug în EAV? Cont parental folosit?
   - Ce corecții sunt necesare? (update tool, ajustare threshold, training echipă)
   - Timeline implementare corecție
   - Documentați RCA complet — regulatorul va solicita

3. **Evidență completă per incident**:
   ```
   UNDERAGE GAMBLING INCIDENT REPORT
   ==================================
   Incident ID: UG-[YYYY]-[seq]
   Date discovered: [DD/MM/YYYY]
   Account ID: _______________
   Declared DOB: _______________
   Actual DOB (if known): _______________
   Age at discovery: _______________

   HOW VERIFICATION WAS BYPASSED:
   - [ ] False document
   - [ ] Identity theft (using adult's ID)
   - [ ] EAV tool failure
   - [ ] Parental account access
   - [ ] Other: _______________

   FINANCIAL SUMMARY:
   - Total deposited: _______________
   - Total withdrawn: _______________
   - Net deposits returned: _______________
   - Net winnings confiscated: _______________

   DETECTION METHOD:
   - [ ] Re-verification (periodic KYC)
   - [ ] Customer contact / complaint
   - [ ] Third-party report (parent, school, authority)
   - [ ] Behavioral anomaly detection
   - [ ] Other: _______________

   REGULATORY REPORTING:
   - Regulator notified: [Yes/No]
   - Date notified: _______________
   - Reference: _______________

   ROOT CAUSE ANALYSIS:
   _______________

   CORRECTIVE ACTIONS:
   1. _______________
   2. _______________

   Sign-off: _______________ Date: ___
   ```

### Pas 7: Audit, testing și metrici
1. **Raport lunar age verification** cu metrici:
   - Verificări totale efectuate
   - Rata de succes per metodă (EAV automat vs. manual)
   - Timp mediu de verificare
   - Conturi blocate pentru neverificare (+ câți au finalizat ulterior)
   - Minori identificați (target: 0 — dar dacă este 0 constant, evaluați dacă sistemul detectează suficient)
   - False rejection rate (adulți legitimii respinși incorect)

2. **Testing trimestrial**:
   - **Penetration test AV**: echipa de securitate încearcă să bypass-eze age verification cu:
     - Documente false (generate AI)
     - Deepfake selfie
     - VPN + date false
     - Cont nou cu datele unui cont blocat
   - Documentați rezultatele și remediați vulnerabilitățile

3. **Benchmarking furnizori EAV**: anual, evaluați dacă furnizorul actual este încă best-in-class. Tehnologia evoluează rapid (deepfake detection, NFC chip reading, biometrics).

4. **Compliance calendar**:
   - Lunar: raport metrici
   - Trimestrial: penetration test + furnizor review
   - Anual: audit complet AV workflow + training refresh

### Pas 8: Context afiliat — responsabilități AV specifice (SMSads)
1. **Afiliatul NU verifică vârsta jucătorilor** — aceasta este responsabilitatea exclusivă a operatorului.
2. **Ce face afiliatul**:
   - NU direcționați trafic de pe platforme predominant utilizate de minori:
     - YouTube Kids — NICIODATĂ
     - TikTok dacă audiența este majoritar <18 — evitați sau verificați demographics
     - Snapchat Discover — evitați pentru gambling
     - Fortnite/Roblox/Minecraft ad networks — NICIODATĂ
   - Includeți "18+" vizibil pe TOATE materialele de afiliere
   - Landing pages: age gate pop-up înainte de conținut gambling
   - SMS campaigns (SMSads): nu trimiteți la numere asociate cu abonamente de minori (dacă informația este disponibilă)

3. **Contractul de afiliere**: includeți clauza că afiliatul nu va dirija trafic din surse cu audiență preponderent minoră, și va include age disclosure pe toate materialele.

## Verificare
- [ ] Vârstă legală documentată per jurisdicție (tabel complet)
- [ ] Timing verificare AV configurat per jurisdicție (pre-depozit UK, la înregistrare GGL/ONJN)
- [ ] Furnizor EAV integrat și testat (Sumsub/Jumio/Onfido)
- [ ] Liveness check cu deepfake detection activ
- [ ] Database fallback (Experian/Equifax) configurat
- [ ] Age gate pre-înregistrare implementat
- [ ] Restricted access mode pre-verificare configurat (zero play UK, €150 MGA)
- [ ] URL direct access prevention testat
- [ ] Bloc cont după 72h neverificare activ
- [ ] Protocol minor identificat documentat (incident template)
- [ ] RCA process definit și documentat
- [ ] Raportare regulator procedure documentat per jurisdicție
- [ ] Parental controls resources în Help Center
- [ ] Session timeout (30 min) configurat
- [ ] 2FA disponibil și promovat
- [ ] Raport lunar AV template creat
- [ ] Penetration test trimestrial în calendar
- [ ] Materialele de afiliere conțin "18+" (verificat)

## Instrumente
- **Sumsub** (sumsub.com) — document + liveness + fraud detection (220+ țări)
- **Jumio** (jumio.com) — AI age verification + iProov liveness
- **Onfido** (onfido.com) — document + Motion liveness
- **Yoti** (yoti.com) — age estimation (selfie-only, no document)
- **FaceTec** (facetec.com) — 3D liveness (deepfake resistant)
- **iProov** (iproov.com) — advanced liveness (banking-grade)
- **Experian Age Verification** (experian.co.uk) — database match UK
- **GBG IDscan** (gbgplc.com) — document + data verification
- **BetBlocker** (betblocker.org) — gambling self-exclusion app (recomandați)
- **Gamban** (gamban.com) — device-level gambling block (recomandați)
- **GamCare for Parents** (gamcare.org.uk/gamfam)
- **FingerprintJS** (fingerprint.com) — device fingerprinting

## Note
- **UKGC a clarificat**: "age verification at withdrawal" NU este acceptat. Trebuie ÎNAINTE de depozit. Operatorii care nu s-au aliniat au primit sancțiuni semnificative (regulatory settlements £M+).
- **Deepfakes sunt o amenințare reală**: în 2024, fraud attempts cu documente AI au crescut 300% YoY. Alegeți furnizori cu cel mai recent deepfake detection (iProov, FaceTec 3D).
- **Combinare AV + KYC**: efectuați ambele simultan la înregistrare pentru a reduce fricțiunea totală. Un singur flow de upload document care verifică: (1) vârstă (2) identitate (3) PEP/sancțiuni.
- **Kenya mobile gambling**: M-Pesa linked verification este standard — integrați verificarea cu M-Pesa registration data (care include verificare ID/vârstă).
- **Cost optimization**: Yoti (selfie-only, €0.1-0.5) ca prim pas → dacă vârstă >25 clar → PASS fără document. Dacă 18-25 → document verification standard. Reduce costul per verificare cu 40-60%.
