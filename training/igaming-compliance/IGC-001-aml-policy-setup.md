---
id: IGC-001
title: AML Policy Setup for iGaming Affiliates
domain: igaming-compliance
source: Anti-Money Laundering in Gambling (Udemy) + FATF Recommendations + 6AMLD + MGA/UKGC/GGL Guidance
version: 2.0
forge_score: "estimated H/3.8"
business_mapping: ["smsads", "gambling-seo"]
---

# AML Policy Setup for iGaming Affiliates

## Obiectiv
Stabiliți o politică AML (Anti-Money Laundering) documentată, operațională și auditabilă pentru operațiuni iGaming sau afiliere, aliniată la FATF Recommendations (R.10, R.20, R.22), Directiva 6AMLD (UE), și cerințele specifice ale regulatorilor din toate jurisdicțiile active (MGA, UKGC, GGL, Curaçao GCB, Kahnawake, ONJN, CONAJZAR, Kenya BCLB). Politica trebuie să fie revizuită periodic, adaptată la profilul de risc al business-ului, și capabilă să reziste unui audit regulatoriu fără preaviz.

## Pași

### Pas 1: Evaluarea riscului de bază (Business Risk Assessment — BRA)
1. Identificați tipurile de produse oferite (casino, sports betting, poker, fantasy sports, lotteries, bingo, esports betting) și canalele de distribuție (web, mobile app, retail, afiliere SMS/push, SEO organic).
2. Mapați TOATE jurisdicțiile în care operați sau generați trafic — fiecare jurisdicție are praguri AML diferite:
   - **MGA (Malta)**: CDD obligatoriu la €2,000 depozit cumulat; EDD la €10,000; monitorizare continuă obligatorie.
   - **UKGC (UK)**: CDD la verificarea identității (pre-depozit din 2019); EDD "financial triggers" la £2,000 net deposits/£500 net loss (Customer Interaction Guidance 2024).
   - **GGL (Germania)**: CDD la înregistrare obligatoriu; limită hard €1,000/lună; LUVAS system centralizat.
   - **Curaçao GCB**: CDD la $2,000 retragere (reforma 2023 — cerințe mai stricte decât CIGA-ul anterior).
   - **Kahnawake (Canada)**: CDD la CAD $3,000; EDD la CAD $10,000; raportare la FINTRAC.
   - **ONJN (România)**: CDD la înregistrare obligatoriu; EDD la 5,000 RON/lună; raportare ONPCSB; verificare ANAF Source of Funds.
   - **CONAJZAR (Paraguay)**: CDD bazat pe Ley 1015/97; praguri de raportare SEPRELAD de 40,000,000 PYG (~$5,500).
   - **Kenya BCLB**: CDD la KES 1,000,000 (~$7,700); raportare FRC (Financial Reporting Centre) obligatorie.
3. Evaluați profilul clientelei: volum tranzacții, metode de plată acceptate (crypto ridică riscul AML semnificativ — FATF R.15), naționalități frecvente, canale de achiziție (SMS/push = risc mai ridicat de third-party deposits).
4. Documentați riscurile identificate într-un **Risk Register** structurat cu:
   - Categorie risc (produs, jurisdicție, canal, client, tranzacție)
   - Rating: LOW / MEDIUM / HIGH per subcategorie
   - Mitigări aplicate per risc
   - Owner (responsabil intern)
   - Data ultimei revizuiri
5. FATF Recommendation 1 — Risk-Based Approach: documentați explicit cum aplicați RBA la fiecare decizie AML. Regulatorii solicită dovezi că BRA nu este un document static ci un instrument activ.
6. Stabiliți frecvența de revizuire BRA: minim anual, sau la orice schimbare materială: piață nouă, produs nou, metodă de plată nouă, schimbare legislativă, incident AML.
7. **Benchmark penalități BRA absent sau inadecvat**:
   - MGA: €23,500 per finding (Directive 1/2018, Art. 18); poate ajunge la suspendare licență.
   - UKGC: Entain — £17M (2022) parțial pentru BRA inadecvat; 888 — £9.4M (2022).
   - GGL: Retragere licență tipologie sportsbetting (Tipico — €4.7M, 2023).
   - Curaçao GCB: Revocare sublicență fără drept de apel (reforma 2023).

### Pas 2: Redactarea documentului de politică AML
1. Structurați documentul cu secțiunile obligatorii (minim — regulatorii verifică completitudinea):
   - **Scop și domeniu de aplicare** — cine este acoperit, ce activități
   - **Definiții** — ML, TF, PEP, RCA, STR/SAR, CDD, EDD, SDD
   - **Cadrul legal aplicabil** — lista tuturor legilor per jurisdicție
   - **Business Risk Assessment** — referință la BRA separat sau inclus
   - **Proceduri CDD/EDD/SDD** — cu praguri și declanșatoare per jurisdicție
   - **Ongoing Monitoring** — reguli de monitorizare continuă
   - **Raportare internă și externă SAR/STR** — cu timelines și autorități
   - **Roluri și responsabilități** — MLRO, Board, angajați
   - **Training** — frecvență, conținut, evaluare
   - **Record Keeping** — durate retenție per jurisdicție
   - **Sancțiuni interne** — consecințe pentru nerespectare
   - **Revizuire și actualizare** — calendar și triggers
2. Includeți referințe explicite la legislația aplicabilă per jurisdicție:
   - UE: Directive 6AMLD (2018/1673), AMLD5 (2018/843), upcoming AMLA/AMLR (2024 regs)
   - UK: Proceeds of Crime Act 2002 (POCA), Money Laundering Regulations 2017 (MLR 2017), Terrorism Act 2000
   - Malta: Prevention of Money Laundering Act (PMLA), Implementing Procedures Part I (IPs)
   - Germania: Geldwäschegesetz (GwG) — German AML Act
   - România: Legea 129/2019 privind prevenirea și combaterea spălării banilor
   - Paraguay: Ley 1015/97, reglementări SEPRELAD
   - Kenya: Proceeds of Crime and Anti-Money Laundering Act 2009 (POCAMLA), FRC regulations
3. Definiți pragurile de monitorizare per jurisdicție (nu aplicați un prag universal):

   | Jurisdicție | CDD Standard | EDD Trigger | Raportare automată |
   |-------------|-------------|-------------|-------------------|
   | MGA | €2,000 cumulat | €10,000 cumulat | €15,000 single |
   | UKGC | Pre-depozit (ID) | £2,000 net dep / £500 net loss | Toate SAR-urile |
   | GGL | La înregistrare | €1,000/lună (hard cap) | La LUVAS automat |
   | Curaçao | $2,000 retragere | $10,000 | $15,000 single |
   | ONJN | La înregistrare | 5,000 RON/lună | 15,000 EUR echivalent |
   | Kahnawake | CAD $3,000 | CAD $10,000 | CAD $10,000+ (FINTRAC) |
   | CONAJZAR | PYG 40M (~$5,500) | PYG 100M | La SEPRELAD |
   | Kenya BCLB | KES 1M (~$7,700) | KES 5M | La FRC |

4. Specificați rolurile FATF R.22 (DNFBP — Designated Non-Financial Businesses and Professions): operatorii iGaming sunt clasificați DNFBP în FATF. Aceasta înseamnă obligații CDD identice cu sectorul financiar, dar cu praguri specifice gambling-ului.
5. Adăugați secțiunea de Terrorist Financing (CTF): FATF R.5-8 specifice — screeningul obligatoriu față de liste de sancțiuni terorism (UN 1267, OFAC SDN, EU terorism list).
6. Adăugați clauza de actualizare: politica se revizuiește la orice modificare legislativă, la schimbarea profilului de risc, la incident AML, sau minim anual — data ultimei revizii vizibilă pe document.

### Pas 3: Desemnarea MLRO și structurii de raportare
1. Numiți un MLRO (Money Laundering Reporting Officer) cu experiență verificabilă în compliance financiar sau iGaming:
   - **UKGC**: MLRO trebuie să treacă Fit and Proper Person assessment (Personal Management Licence).
   - **MGA**: MLRO trebuie aprobat de MGA și domiciliat în Malta sau UE.
   - **GGL**: Geldwäschebeauftragter — trebuie notificat la FIU Germany.
   - **ONJN**: Ofițer de conformitate notificat la ONPCSB.
   - **Kahnawake**: Key Person registration obligatorie.
2. Documentați linia de raportare: Angajat → MLRO → Board/Director → Autoritate (FIU).
3. Creați un canal intern securizat pentru raportarea suspiciunilor (SAR intern) cu câmpurile obligatorii:
   - Persoana raportoare (poate fi anonim în unele jurisdicții)
   - Data și ora identificării suspiciunii
   - Identificatorul clientului (cont, nume, document)
   - Descrierea activității suspecte
   - Valoarea tranzacțiilor relevante
   - Motivul suspiciunii (cu referință la tipologie sau red flag)
   - Documente/screenshot-uri anexate
   - Urgența (standard / escaladare imediată)
4. Stabiliți SLA pentru procesarea rapoartelor interne:
   - MLRO trebuie să confirme primirea în max 4h (zilele lucrătoare).
   - Decizie inițială (escaladare externă sau investigare suplimentară) în max 48h.
   - UK specific: pentru Consent SAR, decizia trebuie luată ÎNAINTE de executarea tranzacției suspecte.
5. Documentați protecția raportorilor: MLRO și angajații nu pot fi sancționați pentru raportări de bună-credință (protected disclosure — POCA s.338 UK; PMLA Art. 28 Malta).
6. **Deputy MLRO**: Numiți un backup MLRO pentru perioadele de indisponibilitate ale MLRO principal — regulatorii verifică continuitatea funcției.
7. **MLRO Annual Report**: MLRO trebuie să prezinte Board-ului un raport anual cu: număr SAR-uri primite și raportate, tipologii identificate, eficacitatea monitorizării, training efectuat, recomandări de îmbunătățire. MGA și UKGC solicită explicit acest raport.

### Pas 4: Implementarea procedurilor CDD (Customer Due Diligence) — FATF R.10
1. Definiți cele 3 niveluri CDD conform FATF Recommendation 10:
   - **SDD (Simplified Due Diligence)**: Risc scăzut demonstrat — verificare identitate de bază, fără sursă fonduri. Aplicabil numai unde BRA justifică și jurisdicția permite (UK: SDD limitat; GGL: nu permite SDD).
   - **CDD Standard**: Verificare identitate (ID + PoA) + sursă fonduri dacă atinge pragul. Aplicabil majorității clienților.
   - **EDD (Enhanced Due Diligence)**: Identitate + sursă fonduri + sursă avere + monitorizare continuă sporită + aprobare senior management. Obligatoriu pentru: PEP-uri, țări FATF grey/black list, tranzacții neobișnuite, praguri depășite.
2. Creați checklist-uri detaliate per nivel:

   **SDD Checklist:**
   - [ ] Verificare identitate electronică (EAV)
   - [ ] Confirmare vârstă legală
   - [ ] Screening sancțiuni de bază

   **CDD Standard Checklist:**
   - [ ] Document identitate valid (pașaport, CI, permis conducere)
   - [ ] Proof of Address (factură utilități / extras bancar — max 3 luni)
   - [ ] Screening PEP + sancțiuni + adverse media
   - [ ] Source of Funds dacă atinge pragul jurisdicțional
   - [ ] Liveness check / selfie match cu document

   **EDD Checklist:**
   - [ ] Toate elementele CDD Standard
   - [ ] Source of Wealth documentat (contracte, acte proprietate, declarații fiscale)
   - [ ] Source of Funds detaliat cu documente suport
   - [ ] Investigație background (adverse media extinsă)
   - [ ] Aprobarea scrisă a MLRO sau senior management
   - [ ] Plan de monitorizare continuă definit (frecvență, praguri)
   - [ ] Revizuire periodică la 3-6 luni

3. Stabiliți momentele de declanșare CDD per jurisdicție:
   - La înregistrare (GGL, ONJN — obligatoriu)
   - Înainte de primul depozit (UKGC — obligatoriu din 2019)
   - La atingerea pragului tranzacțional (MGA, Curaçao)
   - La modificarea profilului de risc
   - La suspiciuni (oricând, indiferent de sumă)
4. Integrați CDD în fluxul de onboarding: NU permiteți retrageri înainte de finalizarea CDD complet. Pe unele piețe (UKGC), NU permiteți nici depozite înainte de verificarea identității.
5. **Ongoing monitoring** (FATF R.10, paragraful 7): revizuire periodică a conturilor active:
   - HIGH RISK: lunar
   - MEDIUM RISK: trimestrial
   - LOW RISK: anual
   - Orice cont cu volum >€5,000/lună: automat MEDIUM minimum
6. **Instrumente CDD recomandate**:
   - **Sumsub** — KYC automation, liveness, document fraud detection, 220+ țări, pricing per verificare
   - **Jumio** — AI-powered ID verification, biometric, AML screening integrat
   - **Onfido** — document verification + liveness, bun pentru volume mari
   - **ComplyAdvantage** — PEP/sancțiuni/adverse media real-time
   - **World-Check (Refinitiv/LSEG)** — cea mai mare bază de date PEP globală
   - **Dow Jones Risk & Compliance** — enterprise-grade screening

### Pas 5: Proceduri de raportare externă SAR/STR — FATF R.20
1. FATF Recommendation 20: obligația de a raporta "promptly" orice tranzacție suspectă la FIU. Identificați autoritatea per jurisdicție:

   | Jurisdicție | Autoritate FIU | Portal / Metodă | Termen legal |
   |-------------|---------------|-----------------|-------------|
   | UK | NCA (National Crime Agency) | UKFIU SAR Online | Pre-tranzacție (Consent SAR) sau post-tranzacție |
   | Malta | FIAU | goAML portal | "Promptly" — practice: 24-48h |
   | Germania | FIU Germany | goAML portal | "Unverzüglich" (without delay) |
   | România | ONPCSB | Portal electronic ONPCSB | 10 zile de la identificare |
   | Curaçao | FIU Curaçao | MOT portal | 14 zile |
   | Kahnawake | FINTRAC | FINTRAC portal | 30 zile (STR); 5 zile (terrorist property) |
   | Paraguay | SEPRELAD | Portal SEPRELAD | 48h de la identificare |
   | Kenya | FRC | FRC Kenya portal | "Promptly" — practice: 7 zile |

2. **SAR Template — câmpuri obligatorii** (adaptat per jurisdicție):
   ```
   SUSPICIOUS ACTIVITY REPORT — INTERNAL
   ================================================
   Ref. No: [auto-generated]
   Date of Report: [DD/MM/YYYY]
   Reporting Entity: [Company name + licence no.]
   MLRO: [Name + contact]

   SUBJECT INFORMATION:
   - Full Name: [as per KYC]
   - Date of Birth: [DD/MM/YYYY]
   - Nationality: [country]
   - Account ID: [platform ID]
   - Registration Date: [DD/MM/YYYY]
   - KYC Status: [SDD/CDD/EDD]
   - PEP Status: [Y/N — if Y, category]

   SUSPICIOUS ACTIVITY DETAILS:
   - Date(s) of suspicious activity: [range]
   - Type: [structuring / pass-through / chip-dumping / third-party / other]
   - Total value involved: [amount + currency]
   - Payment methods used: [card/crypto/e-wallet/bank transfer]
   - Description of activity: [free text — detailed narrative]
   - Red flags identified: [list specific indicators]

   SUPPORTING EVIDENCE:
   - [ ] Transaction logs attached
   - [ ] KYC documents attached
   - [ ] Device/IP data attached
   - [ ] Communication logs attached
   - [ ] Screening results (PEP/sanctions) attached

   MLRO DECISION:
   - [ ] Report to FIU
   - [ ] Close — insufficient evidence (reason: ___)
   - [ ] Continue monitoring (review date: ___)

   Signature MLRO: _______________ Date: _______________
   ```

3. **Tipping-off prevention**: Nimeni NU informează clientul că a fost raportat sau investigat:
   - UK: Section 333A POCA 2002 — infracțiune penală, max 2 ani închisoare.
   - Malta: PMLA Art. 29 — infracțiune penală.
   - Instruiți echipa suport: dacă clientul întreabă de ce contul este blocat, răspunsul standard este "Verificări de securitate de rutină."
4. **UK Consent SAR**: Dacă trebuie să executați o tranzacție dar aveți suspiciuni, cereți "consent" NCA ÎNAINTE. Moratorium: 7 zile lucrătoare. Dacă NCA nu răspunde, puteți proceda. Dacă NCA refuză consent — NU executați tranzacția.
5. Păstrați registrul tuturor SAR-urilor trimise cu: data, referință, autoritatea, răspuns primit. Retenție:
   - UK: 5 ani de la închiderea relației de business
   - Malta: 5 ani (PMLA)
   - Germania: 5 ani (GwG §8)
   - România: 5 ani (Legea 129/2019)
   - Kenya: 7 ani (POCAMLA)

### Pas 6: Data Retention per jurisdicție
1. Stabiliți duratele de retenție pentru datele AML per jurisdicție:

   | Jurisdicție | Retenție AML | Calculat de la |
   |-------------|-------------|---------------|
   | MGA (Malta) | 5 ani | Închidere cont / ultimă tranzacție |
   | UKGC (UK) | 5 ani | Închidere relație de business |
   | GGL (Germania) | 5 ani | Încetarea relației de business |
   | Curaçao GCB | 5 ani | Ultima tranzacție |
   | ONJN (România) | 5 ani | Încetarea relației |
   | Kahnawake | 5 ani | FINTRAC compliance |
   | CONAJZAR (Paraguay) | 5 ani | SEPRELAD requirement |
   | Kenya BCLB | 7 ani | POCAMLA requirement |

2. Implementați politici de retenție automate cu alertă de ștergere la expirare — dar NU ștergeți dacă există investigații active.
3. Reconciliați cu GDPR: datele AML sunt reținute pe baza obligației legale (GDPR Art. 6.1.c) — jucătorul NU poate solicita ștergerea lor înainte de expirarea perioadei legale.

### Pas 7: Training, audit intern și penalități de referință
1. Programați sesiuni de training AML obligatorii:
   - La angajare: training inițial complet (4-8h)
   - Anual: refresh training (2-4h)
   - La schimbări legislative: update session (1-2h)
   - MLRO: training specializat continuu (certificări ACAMS, ICA recomandate)
2. Testați personalul cu scenarii practice iGaming-specifice:
   - "Un client depune €900 de 5 ori în aceeași zi — ce faci?" (structuring)
   - "Un client depune €5,000, joacă 2 minute, retrage €4,800 — ce faci?" (pass-through)
   - "Un client din Myanmar depune cu card UK — ce faci?" (jurisdicție HIGH RISK)
3. Efectuați audit intern la 6 luni de la implementare, apoi anual:
   - Verificați că procedurile sunt urmate efectiv (sample testing)
   - Verificați că alertele sunt procesate în SLA
   - Verificați că training-ul este documentat
   - Verificați că SAR-urile au fost raportate corect
4. **Penalități de referință — ce riscați dacă AML policy lipsește sau este inadecvat**:
   - **Entain (UK, 2022)**: £17M — deficiențe AML + RG, inclusiv monitorizare inadecvată a VIP-urilor.
   - **888 Holdings (UK, 2022)**: £9.4M — eșecuri în Social Responsibility și AML.
   - **Betway (UK, 2020)**: £11.6M — acceptarea depozitelor fără verificarea sursei fondurilor.
   - **Genesis Global (Malta, 2024)**: Suspendare licență — AML deficiencies multiple.
   - **Tipico (Germania, 2023)**: €4.7M — eșecuri AML și GwG non-compliance.
   - **Mr Green (Malta, 2020)**: €2.43M — AML și Player Protection failures.
   - **Kenya**: BCLB poate suspenda licența fără preaviz pentru AML non-compliance (POCAMLA s.15).

### Pas 8: Responsabilități specifice afiliaților (SMSads context)
1. **Afiliații puri (fără procesare plăți)** au obligații AML reduse dar NU zero:
   - Verificați că TOȚI operatorii parteneri sunt licențiați în jurisdicțiile unde generați trafic.
   - Păstrați evidența licențelor operatorilor (număr licență, jurisdicție, dată expirare).
   - NU promovați operatori nelicențiați — în UK este infracțiune (Gambling Act 2005, s.330).
2. **Pentru canale SMS/push (SMSads)**:
   - Verificați că listele de numere de telefon nu conțin persoane excluse (self-exclusion) — operatorul trebuie să furnizeze suppress list.
   - NU trimiteți campanii SMS pentru gambling în jurisdicții unde gambling advertising este interzis sau restricționat (Italia: ban total gambling ads din 2019 — Decreto Dignità).
   - Mențineți un registru al campaniilor cu: jurisdicție, operator promovat, licență operator, dată campanie, volum trimis.
3. **Duty to report**: Dacă observați activitate suspectă în campaniile voastre (ex. trafic masiv de la IP-uri asociate cu fraud rings, click farms, sau conturi create automat), raportați IMEDIAT operatorului partener.
4. **Contractul de afiliere**: Includeți clauze AML:
   - Operatorul confirmă că este licențiat și AML-compliant
   - Operatorul furnizează suppress lists actualizate
   - Dreptul afiliatului de a termina contractul dacă operatorul pierde licența
   - Obligația de cooperare la investigații regulatorii

## Verificare
- [ ] Business Risk Assessment (BRA) completat, datat și semnat
- [ ] Risk Register cu rating per categorie și mitigări documentate
- [ ] Document AML Policy cu toate secțiunile obligatorii — semnat de Board
- [ ] Praguri CDD/EDD documentate per jurisdicție (tabel complet)
- [ ] MLRO desemnat, aprobat de regulator (unde aplicabil), backup MLRO numit
- [ ] Canal intern SAR funcțional cu template complet
- [ ] SLA procesare rapoarte interne documentat (48h MLRO)
- [ ] Checklist-uri CDD/EDD/SDD create per nivel cu toate câmpurile
- [ ] Autoritate FIU identificată per jurisdicție cu portal și termen
- [ ] Procedură raportare externă SAR documentată cu consent SAR (UK)
- [ ] Tipping-off prevention comunicat tuturor angajaților
- [ ] Data retention periods per jurisdicție implementate
- [ ] Training schedule creat — prima sesiune executată și documentată
- [ ] Penalități de referință comunicate managementului
- [ ] Licențe operatori parteneri verificate și documentate (afiliați)
- [ ] Suppress list pentru campanii SMS/push configurată

## Instrumente
- **FATF Recommendations** (fatf-gafi.org) — referință primară globală, R.1-40
- **EGBA AML Guidelines** — best practices operatori europeni
- **Sumsub** (sumsub.com) — KYC/AML automation, 220+ țări
- **Jumio** (jumio.com) — AI ID verification + AML
- **Onfido** (onfido.com) — document verification + biometric
- **ComplyAdvantage** (complyadvantage.com) — real-time screening
- **World-Check / Refinitiv** (lseg.com/en/data-analytics/financial-data/risk-intelligence) — PEP/sanctions database
- **Dow Jones Risk & Compliance** (dowjones.com) — enterprise screening
- **Chainalysis** / **Elliptic** — blockchain analytics pentru crypto gambling
- **goAML Portal** — raportare FIU (Malta, Germania, altele)
- **UKFIU SAR Portal** — raportare NCA (UK)
- **ONPCSB Portal** — raportare FIU România

## Note
- **FATF R.22 — DNFBP**: Operatorii de gambling sunt clasificați DNFBP, deci au obligații AML identice cu instituțiile financiare. Afiliații nu sunt clasificați DNFBP direct, dar sunt expuși la riscul de "facilitating unlicensed gambling" dacă promovează operatori non-complianți.
- **Crypto gambling** ridică riscul AML la HIGH automat — necesită monitoring on-chain (Chainalysis, Elliptic), Travel Rule compliance (FATF R.16), și CDD sporit la toate depozitele crypto.
- **Curaçao reform 2023**: GCB (Gambling Control Board) a înlocuit CIGA. Noile sublicențe au cerințe AML semnificativ mai stricte — toți deținătorii de sublicențe vechi trebuie să re-aplice.
- **AMLA/AMLR (UE, 2024)**: Noul Anti-Money Laundering Authority (AMLA) cu sediul la Frankfurt va supraveghea direct entitățile cu risc AML ridicat din UE, inclusiv operatorii iGaming mari. Implementare: 2026-2027.
- **Paraguay + Kenya** sunt piețe emergente cu reglementare în evoluție rapidă — monitorizați schimbările legislative lunar, nu trimestrial.
- Revizuiți politica AML ori de câte ori adăugați o nouă metodă de plată, intrați pe o piață nouă, sau primiți un new finding de la audit.
