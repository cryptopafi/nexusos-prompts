---
id: IGC-002
title: KYC Document Verification Checklist per Jurisdiction
domain: igaming-compliance
source: AML/KYC Diploma (Udemy) + FATF R.10 CDD + MGA IPs + UKGC LCCP + GGL GwG
version: 2.0
forge_score: "estimated H/3.8"
business_mapping: ["smsads", "gambling-seo"]
---

# KYC Document Verification Checklist per Jurisdiction

## Obiectiv
Standardizați procesul de verificare KYC (Know Your Customer) pentru jucători din jurisdicții diferite, asigurând că documentele colectate îndeplinesc cerințele FATF R.10 (CDD), ale regulatorilor locali (MGA, UKGC, GGL, Curaçao GCB, Kahnawake, ONJN, CONAJZAR, Kenya BCLB), și că procesul este rapid, non-fricțional, auditabil și rezistent la fraudă documentară (inclusiv deepfake).

## Pași

### Pas 1: Maparea cerințelor KYC per jurisdicție — Master Compliance Table
1. Creați un tabel master cu jurisdicțiile active. Actualizați trimestrial sau la orice modificare regulatorie.

   | Jurisdicție | Regulator | Când se face KYC | Documente obligatorii | Prag EDD | Specificități |
   |-------------|-----------|-----------------|----------------------|----------|--------------|
   | **UK** | UKGC | ÎNAINTE de primul depozit (2019 rule) | ID + PoA; SoF la £2,000 net dep | £2,000 net dep / £500 net loss | Financial triggers din Customer Interaction Guidance 2024; no free play pre-KYC pe unele produse |
   | **Malta** | MGA | La înregistrare sau înainte de prima retragere | ID + PoA | €2,000 depozit cumulat → CDD full; €10,000 → EDD | Implementing Procedures Part I specifică: 72h window pentru "provisional play" |
   | **Germania** | GGL | La înregistrare — obligatoriu | ID + IBAN verificat | €1,000/lună limită HARD (LUVAS) | Verificare centralizată prin LUVAS system; BankID-style verification |
   | **Curaçao** | GCB | La retragere >$2,000 sau la suspiciuni | ID minim; PoA recomandat | $10,000 cumulat | Post-reforma 2023: cerințe mai stricte decât CIGA |
   | **Kahnawake** | KGC | La înregistrare sau CAD $3,000 | ID + PoA | CAD $10,000 → EDD | Raportare FINTRAC; PCMLTFA compliance |
   | **România** | ONJN | La înregistrare — obligatoriu | CI + CNP verificat ANAF | 5,000 RON/lună → EDD | Verificare online CI via DEPABD; ANAF cross-check pt SoF |
   | **Paraguay** | CONAJZAR | PYG 40M (~$5,500) | Cédula de Identidad + RUC | PYG 100M → EDD | SEPRELAD regulations; documente notarizate pt SoW |
   | **Kenya** | BCLB | La înregistrare (mobile gambling) | National ID / Passport + M-Pesa verification | KES 1M (~$7,700) → EDD | Mobile-first market; M-Pesa linked verification; FRC compliance |

2. Marcați jurisdicțiile HIGH RISK care necesită EDD automat: țări FATF grey list (monitorizare crescută), FATF black list (interdicție sau EDD maxim), și țări cu conflict armat activ.
3. **FATF Grey List curentă (2024-2025)** — verificați actualizarea la fiecare meeting FATF (februarie, iunie, octombrie): include (exemplu) Bulgaria, Cameroon, Croatia, Nigeria, South Africa, Tanzania, Vietnam. Jucătorii din aceste țări → EDD automat indiferent de sumă.

### Pas 2: Definirea documentelor acceptate per tip — cu standarde de acceptare
1. **Dovadă identitate (ID)** — documente acceptate, în ordine de preferință:
   - Pașaport internațional (preferat global — MRZ machine-readable, cel mai greu de falsificat)
   - Carte de Identitate națională cu chip (EU: standard eIDAS)
   - Permis de conducere (acceptat UK, Canada, Australia; NU acceptat ca ID primar în Malta, România)
   - **NU acceptați**: documente expirate, fotocopii, screenshot-uri, documente deteriorate cu date ilizibile, documente fără foto
   - **Cerințe imagine**: toate 4 colțurile vizibile, fără glare/reflecții, text lizibil, rezoluție min 300 DPI, fișier original (nu screenshot de screenshot)

2. **Dovadă adresă (PoA)** — document separat de ID, cu adresa fizică:
   - Factură utilități: electricitate, gaz, apă (max 3 luni) — NU telefon mobil în UK/Malta
   - Extras de cont bancar oficial (max 3 luni) — cu antet bancă, adresă completă
   - Scrisoare oficială guvernamentală / fiscală (max 6 luni)
   - Certificat de rezidență emis de autoritate (unele țări LATAM)
   - **NU acceptați**: facturi PayPal/Skrill/e-wallet, scrisori de la prieteni/familie, extrase de cont crypto, Amazon/eBay receipts

3. **Sursă de fonduri (SoF)** — obligatoriu la pragurile EDD:
   - Fluturași de salariu (ultimele 3 luni) cu numele angajatorului și valoarea netă
   - Extras de cont bancar cu tranzacții regulate de salariu (ultimele 3 luni)
   - Declarație fiscală anuală (tax return)
   - Dovadă dividende / câștiguri investiții
   - Contract de vânzare (dacă fondurile provin din vânzarea unui activ)
   - **Declarație auto-certificată SoF**: în unele jurisdicții se poate accepta o declarație semnată de client care explică sursa fondurilor — dar NUMAI ca supliment, nu ca document unic

4. **Sursă de avere (SoW — doar EDD/PEP)**:
   - Contracte de proprietate / acte de vânzare imobile
   - Testament / certificat de moștenire
   - Contracte de afaceri / acte constitutive companii
   - Portofoliu investiții certificat de broker
   - Declarații fiscale multi-anuale care demonstrează acumularea

5. Creați un **Document Acceptance Guide vizual** pentru echipa de verificare cu:
   - Imagini exemplu "ACCEPTED" vs "REJECTED" per tip document
   - Checklist rapid per document (colțuri vizibile? data validă? adresa completă?)
   - Referință la jurisdicția specifică (ex: "PoA mobile bill — accepted Malta: NO, UK: NO, Curaçao: YES")

### Pas 3: Procesul de colectare și upload documente — securitate și UX
1. Implementați un portal KYC cu upload securizat:
   - TLS 1.3 în tranzit, AES-256 at rest
   - Storage pe servere în jurisdicție aprobată (EU pentru MGA; UK pentru UKGC)
   - Acces RBAC (Role-Based Access Control) — numai personalul KYC autorizat
   - NU acceptați documente via email neencriptat, WhatsApp, sau alte canale nesecurizate
2. Specificați cerințele tehnice afișate clar la upload:
   - Format: JPG, PNG, PDF (nu HEIC, BMP, TIFF)
   - Dimensiune max: 10MB per document
   - Rezoluție min: 300 DPI
   - Instrucțiuni vizuale: "Plasați documentul pe fundal închis, toate colțurile vizibile, fără glare"
3. Integrați **liveness check** la upload ID (cerință UKGC 2024 best practice):
   - Client face selfie live → AI compară cu foto din document
   - Detectare anti-spoofing: no printed photos, no screen replay, no deepfake masks
   - **Instrumente**: Sumsub Liveness, Onfido Motion, Jumio iProov, FaceTec
4. Trimiteți instrucțiuni clare la solicitarea KYC — email template cu:
   - Ce documente sunt necesare (specific per jurisdicția clientului)
   - Exemple vizuale "corect vs greșit"
   - Link direct la portalul de upload
   - SLA: "Verificarea durează de obicei 1-4 ore în timpul programului"
5. Setați termen de răspuns client:
   - 72h pentru documente inițiale
   - Reminder automat la 48h
   - Dacă nu răspunde la 72h: blocare cont (nu ștergere — păstrați date pentru audit)
   - Al doilea reminder la 5 zile, cu avertisment de blocare
6. Confirmați primirea documentelor automat via email: număr ticket, SLA procesare, instrucțiuni dacă documentele sunt respinse.

### Pas 4: Verificarea documentelor — protocol detaliat în 7 pași
1. **Verificare autenticitate document**:
   - Elemente de securitate: hologramă, watermark, microtext, MRZ (Machine Readable Zone pe pașaport)
   - **AI document fraud detection**: Sumsub Document Check, Onfido Document Report, Jumio Informed Authentication
   - Verificare manipulare digitală: metadata EXIF analysis, pixel-level tampering detection
   - Red flag-uri: fonturi inconsistente, aliniere text diferită, shadow inconsistent, colțuri tăiate suspect

2. **Verificare date personale**:
   - Numele din document = numele din contul de joc (exact match sau justified variation)
   - Data nașterii confirmă vârsta legală în jurisdicția contului
   - Documentul nu este expirat (data de expirare > data verificării)
   - Numărul documentului este valid conform formatului țării emitente

3. **Verificare Proof of Address**:
   - Adresa din PoA = adresa declarată la înregistrare
   - Document emis în ultimele 3 luni (6 luni pentru documente guvernamentale)
   - Numele pe PoA = numele pe ID (match exact)
   - Adresa este completă (stradă, număr, oraș, cod poștal, țară)

4. **Cross-document verification**:
   - ID și PoA aparțin aceleiași persoane (match nume)
   - Foto din ID corespunde selfie-ului liveness check
   - Dacă SoF furnizat: numele de pe fluturașul de salariu = numele din ID

5. **Verificare baze de date externe**:
   - **Sancțiuni**: OFAC SDN, UN Consolidated, EU Sanctions, HM Treasury/OFSI (UK)
   - **PEP**: World-Check, ComplyAdvantage, Dow Jones — clasificare pe 4 categorii (PEP1-3 + RCA)
   - **Adverse media**: screening automatizat cu cuvinte cheie (fraud, money laundering, corruption)
   - **Instrumente**: ComplyAdvantage (real-time), World-Check (batch + real-time), Dow Jones (enterprise)

6. **Decizie documentată**:
   - **PASS**: Toate verificările trecute → cont activat complet
   - **PENDING**: Documente suplimentare necesare → client notificat, cont parțial blocat
   - **FAIL**: Verificare eșuată → cont blocat, escaladare MLRO
   - **SUSPICIOUS**: Indicii de fraudă documentară → cont blocat, SAR intern, escaladare MLRO

7. **Audit trail per verificare**:
   - Timestamp verificare
   - ID-ul operatorului care a verificat (sau "AUTOMATED" dacă AI)
   - Rezultatul fiecărei verificări individuale
   - Decizia finală cu motivul (mai ales pentru FAIL/SUSPICIOUS)
   - Documente păstrate în dosar digital securizat

### Pas 5: Gestionarea cazurilor speciale — protocoale detaliate
1. **Document expirat**: Solicitați document actualizat. Cont parțial blocat: poate vedea sold, poate retrage fonduri existente, NU poate depozita sau juca. Termen: 14 zile pentru resubmitere.
2. **Discrepanță nume** (căsătorie, divorț, schimbare legală):
   - Solicitați certificat de căsătorie / certificat de schimbare de nume ca document suplimentar
   - Actualizați contul doar după primirea documentului suport
   - Documentați în audit trail
3. **Adresă din altă țară decât contul**: Escaladare automată la MLRO. Verificare: motivație legitimă (expat, student, călătorii) vs. potențial structuring cross-border.
4. **Document din jurisdicție HIGH RISK** (FATF grey/black list): EDD automat indiferent de sumă. Documente suplimentare obligatorii: SoF + SoW + referință bancară.
5. **Client refuză KYC**:
   - Blocați retragerile imediat
   - Informați clientul că fondurile rămân disponibile timp de 30 zile
   - După 30 zile fără KYC: escaladare MLRO pentru decizie (returnare fonduri la sursă sau înghețare)
   - Documentați refuzul și motivul
6. **Document în limbă non-latină** (arabă, chineză, coreeană etc.):
   - Acceptați cu traducere certificată (certified translation) pentru MGA/UKGC
   - Unele platforme AI (Sumsub) suportă documente multi-script automat
   - Traducerea trebuie efectuată de translator autorizat, nu Google Translate
7. **Minori identificați post-verificare**: Protocol separat — vezi IGC-009. Blocare imediată, returnare depozite, raportare regulator.

### Pas 6: Re-KYC și Ongoing Document Monitoring
1. **Re-KYC periodic** — obligatoriu pentru menținerea conformității:
   - HIGH RISK conturi: re-verificare la 12 luni
   - MEDIUM RISK: re-verificare la 24 luni
   - LOW RISK: re-verificare la 36 luni sau la trigger event
2. **Trigger events** care declanșează re-KYC imediat:
   - Document expirat
   - Schimbare semnificativă de comportament (volum, frecvență)
   - Client apare pe listă PEP/sancțiuni (ongoing monitoring alert)
   - Client solicită schimbare de date personale (nume, adresă, cetățenie)
   - Incident de fraudă sau SAR asociat contului
3. **Document expiry monitoring**: Setați alerte automate la 30 zile înainte de expirarea documentelor KYC — notificați clientul proactiv.
4. **Look-back review**: Anual, selectați un eșantion aleator de 10% din conturile verificate și efectuați re-verificare completă. Documentați rezultatele.

### Pas 7: Retenție date KYC și GDPR reconciliation
1. Stocați TOATE documentele KYC conform perioadelor de retenție AML per jurisdicție (vezi IGC-001, Pas 6) — minim 5 ani după închiderea contului (7 ani pentru Kenya).
2. Mențineți audit trail imutabil: cine a verificat, când, ce decizie, ce documente — acesta nu se modifică niciodată post-factum.
3. Implementați access control strict:
   - Numai personalul KYC autorizat accesează documentele
   - Fiecare acces este logat (cine, când, ce document, de ce)
   - Revizuire access logs lunar
4. **GDPR reconciliation**:
   - Documentele KYC nu pot fi șterse la cererea jucătorului înainte de expirarea perioadei AML (baza legală: Art. 6.1.c GDPR — obligație legală)
   - Informați jucătorul clar în Privacy Policy despre această limitare
   - Datele de marketing asociate contului POT fi șterse la cerere (dreptul de ștergere se aplică)
5. La cerere MLRO/regulator: puteți furniza complet dosarul KYC în max 24h — testați această capabilitate trimestrial.

### Pas 8: Responsabilități afiliat vs. operator — delimitare clară
1. **Afiliatul (SMSads/SEO) NU are acces la documentele KYC ale jucătorilor** — aceasta este responsabilitatea exclusivă a operatorului.
2. **Ce verifică afiliatul**:
   - Operatorul partener are proceduri KYC certificate (certificare ISO 27001, audit MGA/UKGC, compliance report disponibil)
   - Operatorul partener este licențiat activ (verificare pe site-ul regulatorului: UKGC Register, MGA Licensed Operators List)
   - Operatorul furnizează suppress lists actualizate (self-exclusion + marketing opt-out)
3. **Ce face afiliatul dacă observă anomalii**:
   - Trafic cu conversie anormal de mare de la un singur IP/device → raportare la operator (posibil multi-accounting)
   - Conturi create cu pattern suspect (nume similare, email-uri secvențiale) → raportare la operator
   - Clienți care contactează afiliatul cu plângeri legate de KYC refuzat → redirecționare la operator, NU intermediere

## Verificare
- [ ] Tabel jurisdicții completat cu toate 8 jurisdicțiile și datat
- [ ] Lista documente acceptate per tip publicată intern cu exemple vizuale
- [ ] Portal upload securizat funcțional (TLS 1.3, AES-256, RBAC)
- [ ] Liveness check integrat și funcțional
- [ ] Integrare tool verificare automată: Sumsub / Onfido / Jumio configurată
- [ ] Screening liste sancțiuni + PEP configurat (ComplyAdvantage / World-Check / Dow Jones)
- [ ] Protocol verificare 7 pași documentat și distribuit echipei KYC
- [ ] SLA verificare documente definit și comunicat (target: 4h business hours)
- [ ] Procedură cazuri speciale documentată (7 scenarii)
- [ ] Re-KYC schedule configurat per nivel de risc
- [ ] Document expiry monitoring activ (alertă la 30 zile)
- [ ] Retenție date configurată per jurisdicție (5-7 ani, encrypted)
- [ ] Audit trail imutabil implementat cu access logging
- [ ] GDPR reconciliation documentată
- [ ] Licențe operatori parteneri verificate (pentru afiliați)

## Instrumente
- **Sumsub** (sumsub.com) — KYC automation, liveness check, document fraud detection, 220+ țări, 6,500+ document types
- **Onfido** (onfido.com) — document verification + biometric + fraud signals
- **Jumio** (jumio.com) — KYC/AML suite + iProov liveness
- **FaceTec** (facetec.com) — 3D liveness + face match
- **ComplyAdvantage** (complyadvantage.com) — AML screening real-time
- **World-Check / Refinitiv** (lseg.com) — PEP + Sanctions database
- **Dow Jones Risk & Compliance** (dowjones.com) — PEP + Sanctions + Adverse Media
- **OFAC SDN List**: sanctionssearch.ofac.treas.gov
- **EU Consolidated Sanctions**: data.europa.eu
- **HM Treasury / OFSI Sanctions**: gov.uk/government/publications/financial-sanctions-consolidated-list-of-targets

## Note
- **Verificarea automată (AI-based)** reduce timpul de KYC la <60 secunde pentru 80%+ din cazuri — investiția în tool-uri automate se amortizează rapid la >1,000 verificări/lună.
- **Deepfake-urile** sunt o amenințare reală și crescândă: în 2024, 7% din tentativele de fraudă KYC în iGaming au folosit documente generate AI (sursa: Sumsub Identity Fraud Report 2024). Alegeți furnizori cu liveness detection de ultimă generație.
- **GDPR restricționează** stocarea documentelor biometrice (selfie/liveness) — obțineți consimțământ explicit la onboarding (GDPR Art. 9.2.a) sau documentați baza legală de "substantial public interest" (Art. 9.2.g).
- **Pentru afiliați**: Nu aveți acces la documentele KYC — dar sunteți responsabili să verificați că operatorul partener este licențiat și compliant. Un operator fără KYC adecvat = risc reputațional și legal pentru afiliat.
- **Multi-jurisdicție**: Dacă un jucător are cont în mai multe jurisdicții, fiecare jurisdicție poate avea cerințe KYC diferite — nu presupuneți că KYC-ul dintr-o jurisdicție este suficient pentru alta.
