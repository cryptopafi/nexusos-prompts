---
id: IGC-005
title: Self-Exclusion Workflow and Player Protection
domain: igaming-compliance
source: Responsible Gambling and Player Protection (Udemy) + UKGC LCCP SRC 3.5 + MGA PPD + GGL GlüStV + GAMSTOP API docs
version: 2.0
forge_score: "estimated H/3.8"
business_mapping: ["smsads", "gambling-seo"]
---

# Self-Exclusion Workflow and Player Protection

## Obiectiv
Implementați un workflow complet, imediat și auditabil pentru procesarea cererilor de self-exclusion, garantând că jucătorii excluși nu pot accesa serviciul de gambling, nu sunt expuși la marketing, și nu pot crea conturi noi, în conformitate cu cerințele UKGC (LCCP SRC 3.5), MGA (Player Protection Directive), GGL (OASIS), și alți regulatori. Acest workflow este una dintre cele mai inspectate arii de compliance — eșecurile în self-exclusion generează cele mai mari amenzi.

## Pași

### Pas 1: Tipuri de self-exclusion și durate per jurisdicție
1. Definiți tipurile disponibile pe platforma dvs.:

   **A. Self-exclusion voluntară (operator-level)**:
   - Inițiată de jucător direct din contul online sau prin suport
   - Durate standard: 6 luni, 1 an, 2 ani, 5 ani, PERMANENT
   - NU oferiți opțiunea <6 luni (cerință UKGC SRC 3.5.1 și MGA PPD)

   **B. Self-exclusion prin suport clienți**:
   - Jucătorul contactează via telefon, email, live chat, postal
   - Procesată imediat — target: blocare în <15 minute de la primirea cererii
   - SLA maxim: 24h (dar aceasta este limita UKGC — best practice: sub 1h)

   **C. Self-exclusion via schemă națională** (operator TREBUIE să o respecte):
   | Schemă | Jurisdicție | Durate | Verificare |
   |--------|------------|--------|-----------|
   | GAMSTOP | UK | 6 luni, 1 an, 5+ ani | La înregistrare + zilnic batch |
   | OASIS | Germania | Minim 1 an, max nelimitat | La înregistrare + la fiecare login |
   | Spelpaus | Suedia | 1 lună, 3 luni, 6 luni, 1 an, indefinit | La fiecare login |
   | CRUKS | Olanda | Minim 6 luni | La înregistrare |
   | ROFUS | Danemarca | 1 lună, 3 luni, 6 luni, permanent | La înregistrare + login |
   | ONJN register | România | Minim 6 luni | La înregistrare |

   **D. Self-exclusion forțată de operator (operator-initiated)**:
   - Aplicată fără consimțământul jucătorului când gambling harm este evident și sever
   - Trebuie documentată complet cu rațional medical/comportamental
   - MLRO + senior management approval obligatoriu
   - Informați jucătorul DUPĂ activare (nu înainte) cu motivul și resursele RG

2. Publicați clar pe site cum poate un jucător solicita self-exclusion:
   - Secțiune dedicată "Responsible Gambling" accesibilă din meniul principal (nu ascunsă în footer)
   - Link direct în contul jucătorului (setări → self-exclusion)
   - Menționată în T&C, Privacy Policy, și pe pagina de contact

### Pas 2: Procesarea imediată a cererii — timeline operațional
1. **La primirea cererii** (via cont online, email, telefon, live chat, postal):

   **T+0 (IMEDIAT — sub 15 minute):**
   - Înregistrați cererea: timestamp, canal, identificator jucător, durata solicitată
   - Blocați contul pentru: depozite NOI, joc NOU, accesare bonusuri
   - Jucătorul POATE vedea soldul și POATE retrage fonduri existente
   - Opriți TOATE comunicările de marketing: email, SMS, push, postal, retargeting ads, social media custom audiences
   - Marcați contul cu flag "SELF-EXCLUDED" vizibil în toate sistemele interne

   **T+0 (IMEDIAT):**
   - Dacă jucătorul are pariuri active / mâini de poker în progres:
     - Pariuri sportive: rămân active până la settlement → returnare netă la self-exclusion
     - Casino/slots: sesiunea curentă se încheie
     - Poker: mâna curentă se finalizează → sell-out automat din turnee

   **T+max 24h (target: sub 1h):**
   - Confirmați jucătorului prin email:
     - Data de start a excluderii
     - Data de end (dacă durată definită) sau "permanent"
     - Soldul curent și instrucțiuni de retragere
     - Resurse RG: helpline, GamCare, BeGambleAware, linie națională
     - Ce înseamnă excluderea: nu va putea juca, nu va primi marketing, cum poate retrage fondurile
   - Template email:
     ```
     Subject: Self-Exclusion Confirmation — [Brand]

     Dear [First Name],

     We confirm your self-exclusion from [Brand] effective
     [DD/MM/YYYY] until [DD/MM/YYYY / permanently].

     During this period:
     - Your account is blocked for deposits and gameplay
     - You will not receive any marketing communications
     - Your current balance of [amount] can be withdrawn

     To withdraw your funds, please [instructions].

     If you need support, please contact:
     - GamCare: 0808 8020 133 / gamcare.org.uk
     - BeGambleAware: begambleaware.org
     - [Local helpline per jurisdiction]
     - Gamblers Anonymous: gamblersanonymous.org

     This decision cannot be reversed during the exclusion period.

     [Brand] Responsible Gambling Team
     ```

2. Dacă cererea vine prin suport: procesați IMEDIAT. NU delegați, NU amânați, NU transferați la alt departament.
3. **NU cereți jucătorului să justifice cererea** — self-exclusion este un drept, nu o favoare. NU încercați să-l convingeți să rămână. NU oferiți bonusuri de retenție.
4. **NU aplicați cooling-off ÎNAINTE de self-exclusion** — self-exclusion intră în vigoare imediat (cooling-off-ul se aplică doar la RE-activare).

### Pas 3: Sincronizarea cu schemele naționale — integrare tehnică
1. **GAMSTOP (UK)**:
   - Integrați API-ul GAMSTOP în procesul de înregistrare: check la fiecare registration attempt
   - Verificare batch zilnică a bazei de clienți existenți
   - Dacă un client existent apare pe GAMSTOP: blocare imediată cont
   - Păstrați log sincronizare: data, ora, rezultat, acțiune luată
   - **Testare**: trimestrial cu cazuri de test reale (GAMSTOP oferă sandbox)
   - SLA GAMSTOP: max 24h de la înregistrarea pe GAMSTOP, operatorul trebuie să blocheze

2. **OASIS (Germania/GGL)**:
   - Integrare obligatorie pentru toate licențele GGL
   - Verificare la FIECARE login (nu doar la înregistrare) — OASIS poate fi actualizat oricând
   - Verificare suplimentară la depozit
   - Dacă OASIS check eșuează (timeout/eroare): NU permiteți joc; fail-safe = blocare

3. **Spelpaus (Suedia)**:
   - Verificare la fiecare login pentru jucători suedezi (BankID + Spelpaus check)
   - Dacă activ pe Spelpaus: logout imediat + blocare cont

4. **CRUKS (Olanda/KSA)**:
   - Verificare la înregistrare și la reactivarea conturilor

5. **ONJN Register (România)**:
   - Verificare la înregistrare — coordonare cu baza de date ONJN
   - Sincronizare periodică (zilnică recomandată)

6. **Monitorizare sincronizare**: alertă automată dacă sincronizarea cu orice schemă eșuează >4h. Failsafe: blocare noi înregistrări până la restabilirea sincronizării.

### Pas 4: Prevenirea re-înregistrării — multi-layer detection
1. **Exclusion database internă** cu:
   - Numele complet + variante (cu/fără diacritice, maiden name)
   - Data nașterii
   - Adresa email + variante (gmail dot trick, +alias)
   - Numărul de telefon
   - IBAN/card number
   - Device fingerprint(s)
   - IP addresses folosite
   - Browser fingerprint

2. **La fiecare înregistrare nouă** — verificare automată multi-strat:
   - **Strat 1**: Match exact pe email, telefon, card/IBAN
   - **Strat 2**: Fuzzy match pe nume + DOB (similaritate >85%)
   - **Strat 3**: Device fingerprint match (FingerprintJS Pro, Iovation)
   - **Strat 4**: IP/geolocation match (dacă residential IP = cel al jucătorului exclus)
   - **Strat 5**: Behavioral biometrics (pattern de typing, mouse movement — experimental)

3. **Verificare metode de plată**: dacă un card/IBAN a fost folosit pe un cont exclus, blocați automat orice cont nou care folosește aceeași metodă.

4. **Dacă un jucător exclus reușește bypass (breach)**:
   - Identificați IMEDIAT și blocați noul cont
   - Returnați fondurile nete: (depozite - retrageri efectuate)
   - Câștigurile nete nu sunt confiscate în unele jurisdicții — verificați policy-ul local
   - Escaladare la MLRO
   - Evaluați dacă este incident reportabil la regulator:
     - UKGC: DA, raportare obligatorie. Self-exclusion breach = "key event" care trebuie raportat
     - MGA: DA, raportare în cadrul compliance reporting
     - GGL: DA, OASIS breach trebuie raportat
   - Root cause analysis: de CE a eșuat detecția? Corectați vulnerabilitatea.

5. **Penalități pentru self-exclusion breach**:
   - **LeoVegas (UK, 2021)**: £1.32M — marketing trimis jucătorilor self-excluși
   - **In Touch Games (UK, 2021)**: £6.1M — eșecuri self-exclusion sistematice
   - **Casumo (UK, 2021)**: £6M — jucători excluși au reușit să se re-înregistreze
   - **BGO (UK, 2020)**: £2M — self-excluded players received marketing

### Pas 5: Gestionarea fondurilor la self-exclusion
1. La activarea self-exclusion, informați jucătorul imediat despre:
   - Soldul curent disponibil
   - Modalitatea de retragere (buton de retragere rămâne activ)
   - Termenul în care poate retrage (30 zile recomandat)
2. Procesați cererile de retragere ale jucătorilor excluși cu **PRIORITATE** — SLA: 24h (nu standard 3-5 zile).
3. **Fonduri neretrase**:
   - Contactați jucătorul la 7 zile: reminder de retragere
   - Contactați jucătorul la 21 zile: al doilea reminder
   - La 30 zile fără retragere: escaladare la compliance — evaluați opțiunile:
     - Transfer la contul bancar din dosarul KYC (dacă verificat)
     - Menținere în cont în custodie (dacă nu aveți cont bancar verificat)
     - Transfer la fond RG/autoritate (dacă legislația o cere)
   - La 12 luni nereclamate: consultați legal per jurisdicție
4. **Bonusuri și free spins** la momentul excluderii: anulate, valoare zero. Documentați în T&C-uri.
5. **Pending bets / tournament entries**: settlement conform regulilor normale, apoi fondurile disponibile pentru retragere.
6. **Loyalty points / VIP status**: înghețate pe durata excluderii, disponibile la re-activare (dacă aplicabil).

### Pas 6: Marketing suppression — implementare completă
1. La activarea self-exclusion, jucătorul trebuie eliminat din TOATE canalele de marketing:

   | Canal | Acțiune | Timeline | Verificare |
   |-------|---------|----------|-----------|
   | Email marketing | Suppress list + hard suppress | Imediat | Verificare pre-send la fiecare campanie |
   | SMS | Suppress list | Imediat | Verificare pre-send |
   | Push notifications | Unsubscribe forțat | Imediat | Device token removed |
   | Postal | Adresă adăugată la do-not-mail | Imediat + verificare la fiecare mail run |
   | Social media (retargeting) | Excludere din custom audiences | <24h | Verificare săptămânală |
   | Programmatic display | Excludere din DMP/DSP | <24h | Verificare la campanie nouă |
   | Affiliate marketing | Suppress email/phone la afiliat (dacă partajat) | <24h | Contractual cu afiliat |
   | In-app notifications | Dezactivate | Imediat | Verificare la app update |

2. **Verificare lunară**: rulați un check automatizat — niciun jucător exclus nu apare în audiențele de marketing active.
3. **Afiliați**: furnizați suppress lists actualizate zilnic afiliaților care au acces la date de contact ale jucătorilor. Contract: afiliatul confirmă că nu va contacta jucătorii din suppress list.

### Pas 7: Re-activarea contului după self-exclusion — protocol
1. La expirarea unei excluderi cu durată definită: **NU re-activați automat contul**. Jucătorul trebuie să solicite ACTIV re-activarea.
2. **Cooling-off period între cererea de re-activare și re-activare efectivă**:
   - UKGC: 24h minim (LCCP SRC 3.5.6 — practice: unele jurisdicții solicită 7 zile)
   - GGL/OASIS: cooling-off mai lung, dependent de durata excluderii
   - MGA: 24h minim
   - ONJN: 24h
3. **La re-activare**:
   - Contactați jucătorul cu mesaj "safer gambling" — oferiți instrumentele RG proactiv
   - Setați automat limite de depozit conservatoare (ex. limita anterioară SAU €100/zi, care este mai mică)
   - Jucătorul poate crește limitele ulterior — cu cooling-off standard
   - Efectuați un RG check: întrebați jucătorul dacă dorește să discute despre instrumentele de protecție
4. **Re-activare PERMANENT exclusion**:
   - UKGC: NU este posibilă în primii 5 ani minimum
   - Unele jurisdicții: permanent = permanent, fără posibilitate de reversare
   - Dacă jurisdicția permite re-activare după permanent: minimum 5 ani + MLRO approval + evaluare RG
5. Documentați TOATE re-activările cu: data cererii, data re-activării, cooling-off respectat, limite setate, interacțiune RG efectuată.

### Pas 8: Audit și testing — verificare continuă
1. **Test lunar**: creați un test account cu self-exclusion activă și verificați:
   - Nu poate depozita
   - Nu poate juca
   - Nu primește marketing (email test, SMS test)
   - Nu poate crea un cont nou cu aceleași date
   - GAMSTOP/OASIS check funcționează
2. **Audit trimestrial**: sample 10 cazuri reale de self-exclusion și verificați:
   - A fost procesată în SLA?
   - Marketing-ul a fost oprit complet?
   - Fondurile au fost gestionate corect?
   - Documentația este completă?
3. **Raport lunar self-exclusion** cu metrici:
   - Număr excluderi noi (voluntare + forțate)
   - Canale de solicitare (online/telefon/email/chat)
   - Timp mediu de la cerere la activare
   - Breaches detectate (re-înregistrări detectate)
   - Re-activări procesate
   - Fonduri returnate
4. **Incident response**: dacă un breach este detectat (jucător exclus joacă), activați:
   - Blocare imediată
   - Investigare root cause (<24h)
   - Raportare la regulator (dacă cerut)
   - Fix implementat și testat
   - Lessons learned documentat

## Verificare
- [ ] Opțiune self-exclusion vizibilă și accesibilă (meniu principal + setări cont)
- [ ] Procesare cerere sub 15 minute (target) / max 24h SLA
- [ ] Blocare automată depozite + joc la activare
- [ ] Marketing suppression complet pe TOATE canalele (8 canale verificate)
- [ ] Integrare API scheme naționale (GAMSTOP/OASIS/Spelpaus/CRUKS) testată
- [ ] Exclusion database internă cu multi-layer detection (5 straturi)
- [ ] Device fingerprinting activ
- [ ] Procedură breach documentată (detectare, blocare, raportare, root cause)
- [ ] Procedură returnare fonduri documentată cu timeline
- [ ] Cooling-off la re-activare configurat corect per jurisdicție
- [ ] Limite conservative automate la re-activare
- [ ] Email confirmare excludere cu resurse RG configurat
- [ ] Suppress list afiliat actualizată zilnic
- [ ] Test lunar self-exclusion efectuat și documentat
- [ ] Raport lunar self-exclusion creat

## Instrumente
- **GAMSTOP API** (gamstop.co.uk) — UK national exclusion
- **OASIS** (oasis-sperrvermerk.de) — Germany
- **Spelpaus API** (spelinspektionen.se) — Sweden
- **CRUKS API** (kansspelautoriteit.nl) — Netherlands
- **ROFUS** (rofus.nu) — Denmark
- **FingerprintJS Pro** (fingerprint.com) — device fingerprinting
- **Iovation** (transunion.com/solution/iovation) — device intelligence
- **GamCare** (gamcare.org.uk) — resources for players
- **BeGambleAware** (begambleaware.org) — UK resource
- **Gamblers Anonymous** (gamblersanonymous.org.uk)
- **Jucătorul Responsabil** (onjn.gov.ro) — România
- **BetBlocker** (betblocker.org) — free self-exclusion app (recomandați jucătorilor)
- **Gamban** (gamban.com) — device-level blocking (recomandați jucătorilor)

## Note
- **Self-exclusion este cea mai inspectată arie de RG compliance** — UKGC include verificarea self-exclusion în TOATE compliance assessments. Un singur email de marketing la un jucător exclus poate genera amendă de milioane.
- **Afiliații**: includeți în materialele de marketing link-uri și informații despre self-exclusion. Nu promovați niciodată "bypass self-exclusion" sau "joacă în ciuda excluderii" — aceasta poate duce la pierderea licenței de afiliere.
- **Jucătorii care contactează suportul și menționează probleme cu jocul** (chiar și indirect: "Am pierdut prea mult", "Nu mă mai pot opri") trebuie escaladați IMEDIAT la echipa RG — nu tratați ca reclamație standard. Oferiți proactiv self-exclusion.
- **Data retention self-exclusion**: Datele de self-exclusion trebuie păstrate INDEFINIT (sau minim 5 ani după expirare) — sunt necesare pentru prevenirea re-înregistrării.
- **Cross-brand exclusion**: Dacă operați multiple branduri, self-exclusion pe un brand trebuie să se extindă la toate brandurile operate sub aceeași licență (cerință UKGC, MGA).
