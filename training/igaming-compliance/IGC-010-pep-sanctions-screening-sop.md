---
id: IGC-010
title: PEP and Sanctions Screening SOP
domain: igaming-compliance
source: AML/KYC Diploma (Udemy) + Anti-Money Laundering in Gambling (Udemy) + FATF R.12/R.22 PEP + OFAC/OFSI Guidance + ComplyAdvantage/World-Check docs
version: 2.0
forge_score: "estimated H/3.8"
business_mapping: ["smsads", "gambling-seo"]
---

# PEP and Sanctions Screening SOP

## Obiectiv
Implementați un Standard Operating Procedure (SOP) complet pentru screeningul PEP (Politically Exposed Persons — FATF R.12) și al listelor de sancțiuni internaționale, asigurând că operatorul iGaming nu furnizează servicii persoanelor sancționate (interdicție absolută), că relațiile cu PEP-uri sunt gestionate cu EDD sporit, și că procesul de screening este continuu, auditabil și rezistent la false negatives.

## Pași

### Pas 1: Categorii de screening obligatoriu — taxonomie completă
1. **SANCȚIUNI INTERNAȚIONALE** — persoane și entități față de care orice tranzacție este COMPLET INTERZISĂ:

   | Lista | Emitent | Aplicabilitate | Frecvență actualizare | Portal verificare |
   |-------|---------|---------------|----------------------|------------------|
   | **OFAC SDN** | US Treasury | Extra-teritorială (oricine cu USD sau relație US) | Zilnică | sanctionssearch.ofac.treas.gov |
   | **UN Consolidated** | UN Security Council | Universală (toate statele membre ONU) | La adopție rezoluție | un.org/securitycouncil |
   | **EU Consolidated** | Consiliul UE | Toate entitățile din UE + servicii către UE | Frecventă (la adopție) | data.europa.eu |
   | **HM Treasury / OFSI** | UK Government | UK + extra-teritorial (UK persons) | Frecventă | gov.uk/ofsi |
   | **FATF High-Risk Jurisdictions** | FATF | Global (țări, nu persoane) | Feb/Jun/Oct (plenare) | fatf-gafi.org |
   | **Australia DFAT** | DFAT Australia | Entități australiene | Periodic | dfat.gov.au |
   | **Canada SEMA** | Global Affairs Canada | Entități canadiene | Periodic | international.gc.ca |

   - **OFAC extra-teritorialitate**: Chiar dacă NU operați în SUA, dacă folosiți USD, servicii bancare americane, cloud hosting US, sau aveți orice "US nexus" — sunteți supuși OFAC. Sancțiune: civil penalties nelimitate + criminal prosecution.
   - **Post-Brexit**: UK are liste de sancțiuni SEPARATE de UE. Dacă operați în ambele → screeniți față de AMBELE independent.

2. **PEP (Politically Exposed Persons)** — FATF R.12: nu sunt interziși, dar necesită EDD obligatoriu:

   **PEP Category 1 — Foreign PEPs** (risc maxim, EDD CEL MAI STRICT):
   - Șefi de stat / șefi de guvern
   - Miniștri (inclusiv adjuncți)
   - Parlamentari / membri legislativi
   - Judecători Curte Supremă / Constituțională
   - Guvernatori bănci centrale
   - Ambasadori
   - Ofițeri militari de rang înalt
   - Membri board-uri companii de stat (SOE)

   **PEP Category 2 — Domestic PEPs** (risc ridicat):
   - Aceleași funcții ca PEP 1 dar în țara de operare a operatorului
   - FATF: EDD "where appropriate" — în practică, majoritatea regulatorilor cer EDD identic

   **PEP Category 3 — International Organization PEPs**:
   - Directori/membri board ONU, NATO, FMI, Banca Mondială, BEI, BERD etc.

   **RCA — Relatives and Close Associates** (EDD se extinde):
   - Soț/soție sau partener echivalent
   - Copii și soții/soțiile lor
   - Părinți
   - Asociați de afaceri cunoscuți
   - Persoane juridice al căror beneficiar real este PEP-ul

3. **ADVERSE MEDIA** — nu obligatoriu legal dar STRONG BEST PRACTICE:
   - Screening față de știri negative: fraud, money laundering, corruption, bribery, tax evasion, sanctions violations, organized crime, terrorism
   - Categorii monitorizate: Financial Crime, Regulatory Action, Criminal Proceedings, Terrorism Financing, Human Trafficking, Drug Trafficking, Cybercrime

4. Documentați în policy EXACT care liste screeniți, ce categorii PEP, și frecvența screening-ului.

### Pas 2: Selectarea și configurarea tool-urilor de screening
1. **Comparație furnizori screening**:

   | Furnizor | Acoperire | PEP database | Sancțiuni | Adverse Media | Actualizare | Model pricing | Best for |
   |----------|-----------|-------------|-----------|--------------|------------|--------------|---------|
   | **World-Check (Refinitiv/LSEG)** | 200+ țări, 5.7M+ profiles | Cel mai mare PEP DB global | Toate listele majore | Da (650+ surse) | Intraday | Per seat/API call | Enterprise, bănci, operatori mari |
   | **Dow Jones Risk & Compliance** | 200+ țări, 4.5M+ profiles | Extins | Toate listele majore | Da (800+ surse) | Intraday | Per seat/API call | Enterprise |
   | **ComplyAdvantage** | 200+ țări, real-time | Bun | Toate listele majore | Da (AI-powered) | Real-time (AI continuous monitoring) | Per API call | Mid-size, tech-first companies |
   | **LexisNexis WorldCompliance** | Global | Extins | Toate listele majore | Da | Multiple/zi | Per seat/call | Enterprise, US-focused |
   | **Sumsub AML** | Global | Inclus în KYC platform | Inclus | Inclus | Real-time | Bundled cu KYC | Operatori care vor KYC+AML integrat |
   | **Acuris Risk Intelligence** | Global | Bun | Majore | Da | Zilnic | Per contract | Mid-size |
   | **Sanction Scanner** | Global | Bun | Majore | Limitat | Real-time | Per API call | Startup/SME, cost-effective |

2. **Configurare screening la momentele-cheie**:

   | Moment | Tip screening | Target | SLA acțiune |
   |--------|--------------|--------|------------|
   | **Înregistrare cont** | Full: Sanctions + PEP + Adverse Media | Toți clienții noi | Blocare cont dacă match (pre-activare) |
   | **Modificare profil** (nume, cetățenie) | Re-screening complet | Clientul modificat | 24h |
   | **Ongoing monitoring — HIGH RISK** | Re-screening complet | Conturi PEP, EDD, flagged | Zilnic (automat) |
   | **Ongoing monitoring — MEDIUM RISK** | Re-screening complet | Conturi cu volum crescut | Săptămânal (automat) |
   | **Ongoing monitoring — LOW RISK** | Re-screening complet | Toate conturile active | Lunar (batch) |
   | **Update liste sancțiuni** | Re-screening bazei de clienți | Toți clienții activi | Imediat (trigger-based) |
   | **Depozit/retragere peste prag** | Quick screening | Clientul tranzacției | Pre-procesare tranzacție |

3. **Fuzzy matching configuration**:
   - Threshold recomandat: >80% similaritate → alert
   - Configurați variante de matching:
     - Transliterare (Кириллица → Latin, عربي → Latin)
     - Diacritice (É/è → E/e)
     - Ordine inversată (Last, First vs. First Last)
     - Abrevieri comune (Mohammed → Moh., Muhammad, Muhammed etc.)
     - Name variants database (Robert → Rob, Bob, Bobby)
   - FALSE POSITIVE RATE REALIST: așteptați 3-8% false positive rate. Echipa trebuie dimensionată pentru review manual.

4. **Testare configurare**:
   - Testați cu persoane CUNOSCUTE pe liste sancțiuni (ex: entități OFAC SDN publice)
   - Testați cu variante de nume (scriere alternativă, diacritice, ordine inversată)
   - Verificați că sistemul detectează corect matchurile
   - Documentați rezultatele testelor

5. **Log complet**: fiecare screening logged cu timestamp, input data, rezultat, acțiunea luată.

### Pas 3: Protocol PEP — Enhanced Due Diligence detaliat
1. **Când un client este identificat PEP sau RCA**:

   **T+0 (IMEDIAT)**:
   - Marchează contul: PEP flag activ + categoria (PEP1/PEP2/PEP3/RCA)
   - Activează EDD mode pe cont
   - Notifică MLRO
   - **NU blocați automat contul** — PEP-urile nu sunt interzise, dar necesită due diligence sporit

   **T+24h**:
   - Colectați documente EDD suplimentare:
     - Source of Funds (SoF): de unde provin depozitele (salariu, investiții, dividende)
     - Source of Wealth (SoW): cum a acumulat averea (carieră, afaceri, moștenire)
     - Motivația utilizării serviciului de gambling (de ce joacă la acest operator)
   - Efectuați investigație de background:
     - Funcția ocupată sau ocupată anterior (inclusiv durata mandatelor)
     - Jurisdicția funcției
     - Investigații sau scandaluri publice asociate
     - Adverse media extended search

   **T+48h**:
   - MLRO sau senior management review + decizie documentată:
     - **ACCEPT**: Relația este acceptată cu plan de monitorizare sporit
     - **REJECT**: Relația este refuzată (cu motivul documentat)
     - **SUSPEND**: Necesită informații suplimentare înainte de decizie

2. **EDD checklist PEP** (toate obligatorii):
   ```
   PEP ENHANCED DUE DILIGENCE CHECKLIST
   ======================================
   Account: _______________
   PEP Name: _______________
   PEP Category: [PEP1 / PEP2 / PEP3 / RCA]
   Function: _______________
   Jurisdiction: _______________

   IDENTIFICATION:
   - [ ] Full CDD completed (ID + PoA)
   - [ ] PEP status confirmed via [screening tool]
   - [ ] PEP category documented

   SOURCE OF FUNDS:
   - [ ] SoF documentation received
   - [ ] SoF consistent with declared income/function
   - [ ] SoF documents verified (payslips, bank statements, tax returns)

   SOURCE OF WEALTH:
   - [ ] SoW documentation received
   - [ ] SoW consistent with career/function history
   - [ ] SoW documents verified

   BACKGROUND CHECK:
   - [ ] Function/mandate verified
   - [ ] Adverse media search completed (result: ___)
   - [ ] Sanctions re-check completed (result: ___)
   - [ ] Corruption index of jurisdiction checked
     (TI CPI score: ___ / 100)

   RISK ASSESSMENT:
   - PEP risk level: [HIGH / VERY HIGH]
   - Jurisdiction risk: [LOW / MEDIUM / HIGH] (per CPI + FATF)
   - Overall risk: [HIGH / VERY HIGH]

   APPROVAL:
   - [ ] MLRO review completed
   - [ ] Senior management approval (required for PEP1)
   - Decision: [ACCEPT / REJECT / SUSPEND]
   - Reason: _______________
   - Monitoring plan: [Monthly / Quarterly]
   - Next review date: _______________

   Sign-off: _______________ Date: ___
   ```

3. **Monitorizare continuă PEP** (post-acceptare):
   - Orice tranzacție >€5,000 → alert automat pentru review
   - Raport lunar al activității contului PEP → MLRO review
   - Re-screening trimestrial complet (sancțiuni + adverse media)
   - Comportament monitoring: deviații de la profilul declarat = escaladare

4. **Statutul PEP — durată post-funcție**:
   - FATF R.12: EDD se menține "for a reasonable period of time" (minim 12 luni) după încheierea funcției
   - Practică de piață: 12-24 luni
   - UKGC guidance: evaluare risk-based, dar minim 12 luni
   - MGA IPs: minim 12 luni
   - **Recomandare**: 18 luni post-funcție, apoi evaluare risk-based pentru dezactivare PEP flag

5. **PEP din jurisdicții cu Corruption Perception Index (TI CPI) sub 40**: EDD MAXIM automat + aprobare Board (nu doar MLRO). Verificați CPI anual: transparency.org/cpi

### Pas 4: Protocol SANCȚIUNI — blocare imediată
1. **ALERTĂ SANCȚIUNI = BLOCARE IMEDIATĂ — ZERO TOLERANȚĂ**:

   **T+0 (IMEDIAT — sub 15 minute)**:
   - Blocați tranzacția sau contul IMEDIAT
   - NU procesați NICIO plată: nici retragere, nici depozit
   - Înghețați orice fonduri existente în cont
   - Escaladare la MLRO + Legal

   **T+1h**:
   - MLRO verifică: este TRUE MATCH sau FALSE POSITIVE?

2. **Verificare FALSE POSITIVE** (obligatoriu înainte de acțiune permanentă):
   - Comparați: numele complet (inclusiv middle names), data nașterii, naționalitate, adresă
   - Verificați foto (dacă disponibilă pe lista de sancțiuni — unele publică foto)
   - Dacă clientul are document cu DATE DIFERITE de persoana sancționată → probabil FALSE POSITIVE
   - **Criteriu de confirmare**: minim 3 din 4 data points trebuie să corespundă (full name + DOB + nationality + address) pentru TRUE MATCH
   - **Dacă FALSE POSITIVE**:
     - Deblocați contul
     - Documentați complet în audit trail: motivul determinării FALSE POSITIVE, evidențe comparate
     - Marcați clientul ca "CLEARED — sanctions false positive" pentru a evita re-blocare la viitoarele screening-uri (whitelist cu review periodic)

3. **Dacă TRUE MATCH** — acțiuni obligatorii cu consecințe penale:

   **ACȚIUNE IMEDIATĂ (ore, nu zile)**:
   - Blocați contul DEFINITIV
   - Înghețați TOATE fondurile (asset freeze — obligație legală)
   - **NU returnați fondurile** — returnarea la persoană sancționată este INFRACȚIUNE
   - NU informați clientul de motivul blocării (tipping off)

   **RAPORTARE (obligatorie per jurisdicție)**:
   | Jurisdicție | Raportare la | Termen | Forma |
   |-------------|------------|--------|-------|
   | UK | OFSI + NCA (SAR) | IMEDIAT (within hours) | OFSI reporting form + SAR |
   | Malta | FIAU + Sanctions Monitoring Board | IMEDIAT | goAML + notificare separată |
   | Germania | FIU DE + BaFin | IMEDIAT | goAML |
   | România | ONPCSB | IMEDIAT | Portal electronic |
   | SUA (dacă OFAC nexus) | OFAC | 10 zile (blocking report) | OFAC reporting portal |
   | EU (generic) | Autoritatea competentă per stat | IMEDIAT | Per jurisdicție |
   | Kenya | FRC | IMEDIAT | FRC portal |
   | Kahnawake | KGC + FINTRAC | 5 zile | FINTRAC portal |

   **POST-RAPORTARE**:
   - Cooperați COMPLET cu autoritățile
   - NU eliberați fondurile fără autorizare explicită de la autoritate (OFSI licence, OFAC licence)
   - Consultați legal IMEDIAT — responsabilitatea este atât organizațională cât și personală (MLRO)

4. **Consecințe penale ale nerespectării sancțiunilor**:
   - **OFAC (US)**: Civil penalties: $250,000-$1,000,000+ per violation; Criminal: up to 20 years imprisonment
   - **OFSI (UK)**: Criminal offence, up to 7 years imprisonment (Sanctions and Anti-Money Laundering Act 2018)
   - **EU**: Sanctions evasion is criminal offence in all member states

### Pas 5: Adverse media screening — protocol detaliat
1. Configurați screening adverse media cu cuvinte cheie RELEVANTE iGaming:
   - Financial crime: money laundering, fraud, embezzlement, tax evasion, bribery, corruption
   - Criminal: convicted, arrested, indicted, sentenced, investigation
   - Regulatory: sanctions, banned, disqualified, suspended, fined
   - Terrorism: terrorism financing, designated, listed
   - Other: trafficking, organized crime, cybercrime

2. **Categorii adverse media monitorizate** (configurați în tool):
   - Financial Crime (cel mai relevant pentru iGaming)
   - Regulatory Action (fines, bans, licence revocations)
   - Criminal Proceedings (arrests, convictions)
   - Terrorism / Terrorism Financing
   - Human Trafficking / Modern Slavery
   - Drug Trafficking
   - Corruption / Bribery (esențial pentru PEP cross-referencing)

3. **Evaluare MLRO la alertă adverse media** — framework de decizie:

   | Factor | LOW severity | MEDIUM severity | HIGH severity |
   |--------|------------|----------------|--------------|
   | **Timing** | >5 ani, rezolvat | 1-5 ani | <1 an sau în desfășurare |
   | **Sursă** | Blog/tabloid fără confirmare | Media respectabilă | Comunicat oficial autoritate |
   | **Gravitate** | Acuzații minore | Investigație activă | Condamnare sau sancțiune |
   | **Relevanță** | Irelevant pentru gambling/financial | Adiacent | Direct financial crime |
   | **Jurisdicție** | Jurisdicție cu presă politizată | Neutru | Jurisdicție de operare |

   **Acțiuni per severitate**:
   - **LOW**: Notare în profilul clientului, nicio acțiune imediată, revizuire la next scheduled review
   - **MEDIUM**: EDD activat, monitoring sporit, MLRO review în 48h
   - **HIGH**: Evaluare blocare cont, escaladare legal, potențial SAR
   - **CRITICAL** (ex: condamnare financiar-penală confirmată): Tratați ca sancțiune — blocare + investigare

4. Adverse media NU este bază suficientă SINGURĂ pentru blocare definitivă — trebuie coroborată. Excepție: dacă adverse media revelă informații care ar trebui să conducă la SAR.

### Pas 6: Ongoing monitoring — batch re-screening și actualizare liste
1. **Re-screening automat batch** (critical — persoane care nu erau sancționate/PEP se pot adăuga oricând):

   | Frecvență | Target | Tip screening | Trigger |
   |-----------|--------|--------------|---------|
   | Zilnic | HIGH RISK conturi + PEP-uri | Full (sanctions + PEP + adverse media) | Automat |
   | Săptămânal | MEDIUM RISK conturi | Full | Automat |
   | Lunar | ALL active accounts | Sanctions + PEP | Batch automat |
   | La update listă | ALL active accounts | Sanctions only | Trigger de la furnizor |

2. **Protocol la actualizare majoră liste sancțiuni**:
   - Furnizorul vă notifică (real-time pentru ComplyAdvantage, intraday pentru World-Check)
   - Declanșați re-screening IMEDIAT al bazei de clienți activi
   - Dacă match → protocol blocare (Pas 4) se activează IMEDIAT
   - SLA: max 4h de la notificare update până la finalizarea re-screening-ului
   - Documentați: data update, data re-screening, rezultate, acțiuni

3. **Look-back screening trimestrial**:
   - Re-screeniți un sample aleator de 10% din conturile inactive (closed but within retention period)
   - Scopul: detectarea persoanelor care au fost sancționate DUPĂ închiderea contului (informație relevantă pentru compliance record)

4. **Actualizare furnizor screening**:
   - Verificați lunar că furnizorul acoperă TOATE listele din Pas 1
   - Verificați frecvența reală de actualizare (nu doar promisă)
   - Contract clause: furnizorul notifică orice întrerupere de serviciu în <1h

### Pas 7: Raportare și metrici de performanță
1. **Raport lunar screening** cu metrici:
   ```
   MONTHLY PEP & SANCTIONS SCREENING REPORT
   ==========================================
   Period: [Month/Year]
   Prepared by: [MLRO / Compliance]
   Date: [DD/MM/YYYY]

   SCREENING VOLUME:
   - Registration screenings: ___
   - Ongoing monitoring screenings: ___
   - Triggered re-screenings: ___
   - Total screenings: ___

   ALERTS:
   - Total alerts generated: ___
   - Sanctions alerts: ___
   - PEP alerts: ___
   - Adverse media alerts: ___

   RESOLUTION:
   - False positives: ___ (___%)
   - True PEP matches (EDD applied): ___
   - True sanctions matches (blocked): ___
   - Adverse media — action taken: ___
   - Pending review: ___

   PERFORMANCE:
   - Average resolution time (alert to decision): ___
   - False positive rate: ___%
   - Alerts >48h unresolved: ___

   INCIDENTS:
   - Sanctions true matches: [detail each]
   - PEP high-risk accepted: [detail each]
   - PEP rejected: [detail each]
   - Adverse media escalations: [detail each]

   RECOMMENDATIONS:
   1. ___
   2. ___
   ```

2. **Red flags de performanță** (indicatori de sistem deficient):
   - 0 PEP matches pe trimestru la >5,000 conturi active → threshold prea strict sau acoperire insuficientă
   - >15% false positive rate → threshold prea lax, generează alert fatigue
   - Alerte nerezolvate >48h → resurse insuficiente sau SLA nerespectate
   - Nicio alertă adversă media → tool poate fi dezactivat sau configurat greșit

3. Prezentați raportul Board-ului trimestrial și MLRO-ului lunar.
4. Includeți recomandări concrete în fiecare raport.

### Pas 8: Penalități, data retention, și context afiliat
1. **Penalități de referință — sancțiuni screening failure**:
   - **Standard Chartered (US/OFAC, 2019)**: $1.1 BILLION — facilitarea tranzacțiilor cu Iran, Cuba, Myanmar (banking, dar principiul se aplică)
   - **ING (OFAC/EU, 2012)**: $619M — stripped/altered wire transfers to sanctioned countries
   - **AB Bankas Snoras (FIAU Malta, 2020)**: €6.5M — AML + sanctions screening deficiencies
   - **iGaming specific**: operatorii care procesează tranzacții cu persoane sancționate riscă: revocare licență + amenzi + proces penal personal MLRO
   - **UKGC**: "The Commission may prosecute any person who facilitates gambling by a sanctioned person" (Gambling Act + Sanctions Act)

2. **Data retention screening records**:

   | Tip record | Retenție | Baza legală |
   |-----------|---------|------------|
   | Screening results (all) | 5 ani post-relație | AML legislation |
   | PEP EDD documentation | 5 ani post-relație (7 Kenya) | AML + FATF R.12 |
   | Sanctions true match files | 5 ani + indefinit dacă investigație | AML + sanctions law |
   | False positive documentation | 5 ani | Audit trail |
   | Adverse media findings | 5 ani | Best practice |
   | MLRO decisions | 5 ani | AML legislation |

3. **Testare anuală sistem** (obligatoriu):
   - Teste cu nume cunoscute pe liste sancțiuni → verificați detecție
   - Teste cu variante de scriere → verificați fuzzy matching
   - Teste cu PEP-uri noi (recent adăugate) → verificați actualizare
   - Documentați rezultatele cu gap analysis și acțiuni corective

4. **Context afiliat — responsabilități screening**:
   - Afiliații NU au responsabilitate directă de PEP/sanctions screening (aceasta revine operatorului)
   - **DAR**: afiliații trebuie să:
     - Verifice că operatorul partener are procese de screening certificate
     - Verifice că operatorul este licențiat și nu a fost sancționat pentru deficiențe AML/sanctions
     - Monitorizeze news despre operatorii parteneri — un operator sancționat = risc reputațional major
   - **Risc afiliat**: dacă un afiliat promovează un operator care este ulterior sancționat pentru non-compliance, afiliatul poate pierde:
     - Comisioanele acumulate (clawback)
     - Relația cu alți operatori (reputational contagion)
     - Licența de afiliere (dacă UK)
   - **Best practice afiliat**: includeți în contractul de afiliere dreptul de a termina imediat dacă operatorul primește sancțiune regulatorie majoră

## Verificare
- [ ] Liste de sancțiuni acoperite documentate (OFAC, UN, EU, HM Treasury/OFSI, FATF)
- [ ] Categorii PEP definite (PEP1/2/3 + RCA) cu exemple
- [ ] Furnizor screening selectat, configurat și testat (ComplyAdvantage / World-Check / Dow Jones / Sumsub)
- [ ] Fuzzy matching configurat la prag 80% cu variante de matching
- [ ] Screening automat la înregistrare funcțional și testat
- [ ] Ongoing monitoring configurat per nivel de risc (zilnic/săptămânal/lunar)
- [ ] Protocol EDD pentru PEP documentat cu checklist complet
- [ ] Aprobare MLRO + senior management pentru PEP1 obligatoriu
- [ ] Protocol blocare imediată sancțiuni documentat cu SLA sub 15 minute
- [ ] Procedură verificare false positive documentată cu criteriu 3/4 data points
- [ ] Raportare la autoritate per jurisdicție documentată (OFSI/OFAC/FIAU/FIU/ONPCSB/FRC)
- [ ] Adverse media screening configurat cu categorii și cuvinte cheie
- [ ] Framework evaluare severitate adverse media creat
- [ ] Re-screening batch automat configurat (zilnic HIGH, săptămânal MEDIUM, lunar ALL)
- [ ] Protocol la actualizare majoră liste sancțiuni (SLA 4h)
- [ ] Look-back screening trimestrial în calendar
- [ ] Raport lunar screening template creat cu toate metricile
- [ ] Audit trail retenție 5 ani configurată
- [ ] Testare anuală documentată (persoane cunoscute + fuzzy matching)
- [ ] Licențe și compliance status operatori parteneri verificate (afiliați)

## Instrumente
- **World-Check / Refinitiv (LSEG)** (lseg.com) — cea mai mare bază de date PEP globală (5.7M+ profiles)
- **Dow Jones Risk & Compliance** (dowjones.com) — enterprise PEP + sanctions + adverse media
- **ComplyAdvantage** (complyadvantage.com) — real-time AI-powered AML screening
- **LexisNexis WorldCompliance** (risk.lexisnexis.com) — enterprise screening
- **Sumsub AML Screening** (sumsub.com) — integrat cu KYC platform
- **Sanction Scanner** (sanctionscanner.com) — cost-effective screening API
- **OFAC SDN Search**: sanctionssearch.ofac.treas.gov
- **EU Consolidated List**: data.europa.eu/data/datasets/consolidated-list
- **UN Sanctions**: un.org/securitycouncil/content/un-sc-consolidated-list
- **HM Treasury / OFSI**: gov.uk/government/publications/financial-sanctions-consolidated-list-of-targets
- **FATF High-Risk Jurisdictions**: fatf-gafi.org/en/countries/black-and-grey-lists
- **Transparency International CPI**: transparency.org/cpi (Corruption Perception Index — referință pentru PEP risk)
- **Chainalysis / Elliptic** — sancțiuni crypto (wallet-level screening)

## Note
- **PEP-urile nu sunt criminali prin definiție** — obiectivul EDD este să înțelegeți sursa fondurilor și averii, nu să refuzați automat toți funcționarii publici. Un PEP cu SoF/SoW documentat transparent poate fi client acceptabil.
- **Liste sancțiuni se actualizează CONSTANT** — asigurați-vă că furnizorul are actualizări intraday (nu doar zilnic). ComplyAdvantage oferă real-time; World-Check oferă intraday.
- **OFAC are aplicabilitate EXTRA-TERITORIALĂ** — dacă procesați o singură tranzacție în USD sau prin bancă cu corespondent US, sunteți supuși OFAC. Consecinșele sunt SEVERE (miliarde $ amenzi în sectorul bancar).
- **Crypto sanctions screening**: dacă acceptați crypto, screeniți wallet addresses față de liste sancțiuni crypto:
  - Tornado Cash wallet addresses (sanctioned by OFAC)
  - Wallets asociate cu ransomware (Lazarus Group etc.)
  - Tool-uri: Chainalysis KYT, Elliptic Lens, TRM Labs
- **Post-PEP period**: nu dezactivați PEP EDD în ziua în care persoana iese din funcție. Riscul de corupție poate persista ani de zile. Minim 18 luni post-funcție, apoi evaluare risk-based.
- **FATF review-uri (februarie, iunie, octombrie)**: la fiecare FATF plenary, grey/black list se actualizează. Monitorizați și ajustați risk scoring-ul conturilor din țările afectate.
