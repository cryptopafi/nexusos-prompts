---
id: PP-003
title: PCI-DSS Compliance Checklist
domain: payment-processing
source: Fraud Prevention — PCI-DSS (Udemy)
version: 2.0
forge_score: "3.75"
business_mapping: []
---

# PP-003: PCI-DSS Compliance Checklist

## Obiectiv
Implementa si mentine conformitatea cu standardul PCI-DSS (Payment Card Industry Data Security Standard) v4.0 pentru un merchant care proceseaza plati cu cardul, cu determinarea corecta a tipului de SAQ aplicabil (A, A-EP, C-VT, D), reducerea scope-ului la minimum prin hosted payment pages si tokenizare, evitand fine-urile procesatorului (5K-100K USD/luna), breaches de date si pierderea dreptului de a procesa carduri.

## Pasi

### Pasul 1 — Determinarea nivelului PCI-DSS si a SAQ-ului aplicabil
**PCI-DSS are 4 niveluri bazate pe volumul de tranzactii anuale:**
| Nivel | Volume Visa/MC/an | Cerinta de validare | Cost estimat |
|-------|-------------------|---------------------|-------------|
| Level 1 | > 6M tranzactii | Audit anual de QSA + ASV scan trimestrial + pentest | 50K-200K EUR/an |
| Level 2 | 1M-6M tranzactii | SAQ anual + ASV scan trimestrial | 5K-20K EUR/an |
| Level 3 | 20K-1M tranzactii e-commerce | SAQ anual + ASV scan trimestrial | 2K-10K EUR/an |
| Level 4 | < 20K tranzactii e-commerce | SAQ anual recomandat + ASV scan recomandat | 500-2K EUR/an |

**SAQ (Self-Assessment Questionnaire) — CARE se aplica:**

| SAQ | Cand se aplica | Nr. cerinte | Complexitate | Cand sa-l alegeti |
|-----|---------------|-------------|-------------|-------------------|
| **SAQ A** | Merchantul a externalizat COMPLET procesarea (hosted page sau iframe de la procesator). NU stocheaza, proceseaza sau transmite date de card. | 22 | **MINIM** | **RECOMANDARE #1 — folositi MEREU daca posibil** |
| **SAQ A-EP** | E-commerce cu script/pagina proprie care redirecteaza la procesator (ex: Stripe.js embedded). Serverul propriu NU primeste date de card dar controleaza pagina. | 139 | MEDIU | Cand folositi Stripe Elements, Adyen Drop-in, sau similar embedded |
| **SAQ C-VT** | Terminal virtual (plata prin browser pe site-ul procesatorului, manual per tranzactie — call center). | 73 | MEDIU-MIC | Call center / MOTO processing |
| **SAQ D** | Merchant care stocheaza date de card PE PROPRIUL server sau procesator propriu custom. | 329 | **MAXIM** | EVITATI daca puteti — cel mai complex si scump |

**Decizie practica — Flowchart:**
```
Stocati date de card pe servere proprii?
  → DA → SAQ D (329 cerinte, complex, scump)
  → NU ↓

Aveti pagina de plata pe propriul server (chiar daca redirecteaza)?
  → DA → SAQ A-EP (139 cerinte)
  → NU ↓

Folositi hosted payment page (redirect complet la procesator) sau iframe?
  → DA → SAQ A (22 cerinte — IDEAL)
  → NU ↓

Folositi terminal virtual (browser procesator, manual)?
  → DA → SAQ C-VT (73 cerinte)
```

**Recomandare pentru orice business nou sau mic**: folositi integrare **SAQ A** (hosted payment page sau iframe) — externalizati complet procesarea si reduceti scope-ul PCI la minimum absolut. Stripe Checkout (hosted), Nuvei hosted page, Worldpay hosted page = SAQ A.

### Pasul 2 — Reducerea PCI Scope (principiul CHEIE al intregii proceduri)
Cel mai simplu si ieftin mod de a fi compliant: **NU ATINGETI datele de card**.

**Metode de reducere scope (in ordinea recomandata):**

1. **Hosted Payment Page** (SAQ A — cel mai simplu):
   - Cardholder-ul este redirectat la pagina de plata a procesatorului (ex: Stripe Checkout page).
   - Datele de card nu ajung NICIODATA pe serverele merchantului.
   - Merchantul primeste doar un token si o confirmare.
   - **Trade-off**: mai putin control asupra UX-ului de plata, dar PCI minim.

2. **iFrame / Embedded Form** (SAQ A-EP):
   - Formularul de card este embedded dintr-un iframe al procesatorului pe pagina merchantului.
   - Datele se tokenizeaza INAINTE sa ajunga la merchant.
   - **Trade-off**: control mai mare asupra UX, dar SAQ A-EP (139 cerinte vs. 22).
   - Exemple: Stripe Elements, Adyen Drop-in, Checkout.com Frames.

3. **Tokenizarea** (complementar oricrei metode):
   - Procesatorul inlocuieste datele de card cu un token (ex: `tok_4242xxxx`).
   - Stocati token-ul, NU cardul — tokenul este inutil fara cheia procesatorului.
   - Tokenizarea permite recurring billing fara a stoca PAN.

4. **Regula absoluta**: **NICIODATA nu stocati PAN** (Primary Account Number = numarul de 16 cifre) daca nu este absolut necesar. Daca stocati PAN = SAQ D = 329 cerinte = audit costisitor.

### Pasul 3 — Cele 12 cerinte PCI-DSS v4.0 (checklist complet)
Chiar si cu SAQ A, trebuie sa respectati cerintele aplicabile. Mai jos: checklist-ul COMPLET (cele relevante pentru SAQ A sunt marcate cu *).

**Domeniul 1 — Securitate retea *:**
- [ ] * Firewall configurat cu reguli documentate
- [ ] Retelele de card procesare separate de retelele interne (segmentare)
- [ ] Nicio conexiune directa Internet ↔ sisteme card fara firewall
- [ ] Reguli firewall revizuite la 6 luni

**Domeniul 2 — Fara defaults vendor *:**
- [ ] * Schimbate TOATE parolele default pe orice echipament (router, server, terminal POS, CMS)
- [ ] Dezactivate serviciile si porturile neutilizate
- [ ] * Inventar documentat al componentelor de sistem

**Domeniul 3 — Protejarea datelor stocate:**
- [ ] NU stocati NICIODATA CVV2/CVC2 dupa autorizare
- [ ] NU stocati NICIODATA PIN sau PIN block
- [ ] Daca stocati PAN (SAQ D): criptati cu AES-256 sau hash one-way
- [ ] Politica de retentie si stergere date de card definita (stergeti ce nu mai e necesar)
- [ ] * Daca folositi tokenizare: verificati ca token-ul nu contine PAN reversibil

**Domeniul 4 — Protejarea datelor in transmisie *:**
- [ ] * HTTPS (TLS 1.2 sau 1.3) pe TOATE paginile cu date de card sau redirect
- [ ] NU trimiteti NICIODATA date de card prin email, SMS, chat, sau mesagerie
- [ ] * Certificat SSL valid si actualizat (nu expired, nu self-signed in productie)
- [ ] HSTS header activat

**Domeniul 5 — Protectie malware:**
- [ ] Antivirus/antimalware instalat pe toate sistemele relevante si actualizat automat
- [ ] Scanare regulata (cel putin saptamanal)
- [ ] Protectie anti-phishing pe email-urile corporate

**Domeniul 6 — Software securizat *:**
- [ ] * Aplicati patch-uri de securitate: critice in 1-7 zile, altele in 30 zile
- [ ] * Aplicatii web: protectie WAF (Web Application Firewall) — Cloudflare WAF gratuit disponibil
- [ ] OWASP Top 10 adresat in aplicatiile de plata (XSS, SQL injection, CSRF minimum)
- [ ] Testare de penetrare anuala (obligatoriu Level 1-2, recomandat Level 3-4)
- [ ] Code review sau DAST pentru schimbari in pagina de plata

**Domeniul 7 — Acces restrictionat *:**
- [ ] * Principiul least privilege: fiecare utilizator/sistem are accesul MINIM necesar
- [ ] * Role-based access control implementat
- [ ] * Documentat cine are acces la ce sisteme

**Domeniul 8 — Autentificare *:**
- [ ] * Conturi individuale (NU conturi shared) pentru fiecare administrator
- [ ] * Parole complexe: minimum 12 caractere, complexitate, rotatie la 90 zile (PCI v4.0: sau MFA)
- [ ] * MFA OBLIGATORIU pentru accesul remote la sistemele din scope PCI
- [ ] * Conturi inactive dezactivate in 90 zile
- [ ] Lockout dupa 6-10 incercari esuate

**Domeniul 9 — Acces fizic:**
- [ ] Accesul fizic la servere si terminale POS documentat si restrictionat
- [ ] Camerele de supraveghere la zonele cu echipamente de plata (daca aplicabil)
- [ ] Distrugerea securizata a mediilor de stocare cu date de card (shredding, degaussing)

**Domeniul 10 — Logging *:**
- [ ] * Logging activat pe toate sistemele: cine, ce, cand (audit trail)
- [ ] Log-urile protejate impotriva modificarii (write-once, SIEM, sau append-only)
- [ ] Review periodic al log-urilor (cel putin saptamanal)
- [ ] Retentie log-uri minimum 12 luni (3 luni online + 9 luni arhiva)
- [ ] Alerta automata la evenimente suspecte (login esuat repetat, acces neautorizat)

**Domeniul 11 — Testare securitate *:**
- [ ] * Scanare vulnerabilitati interne trimestrial
- [ ] * ASV (Approved Scanning Vendor) scan extern trimestrial
- [ ] Penetration testing anual (sau dupa schimbari majore de infrastructura)
- [ ] IDS/IPS active (intrusion detection/prevention)

**Domeniul 12 — Politici organizationale *:**
- [ ] * Politica de securitate a informatiilor documentata si distribuita
- [ ] * Training PCI-DSS pentru toti angajatii cu acces la date de card (anual)
- [ ] * Incident Response Plan documentat (ce faceti daca se intampla un breach)
- [ ] * Risk assessment anual
- [ ] * Inventar servicii si provideri terti cu acces la date de card

### Pasul 4 — Completarea SAQ si submission
**Proces de completare SAQ (pentru Level 3-4, SAQ A):**
1. Descarcati SAQ-ul corespunzator de la `pcisecuritystandards.org/assessors_and_solutions/self_assessment_questionnaires`
2. Asigurati-va ca descarcati versiunea PCI-DSS **v4.0** (nu v3.2.1 — expirat)
3. Raspundeti la fiecare cerinta: **Yes / No / N/A / Compensating Control** cu dovezi documentate
4. Semnati Attestation of Compliance (AoC) — declaratie ca informatiile sunt corecte
5. Trimiteti SAQ + AoC la procesatorul dvs. (unii cer anual, altii la activarea contului)

**Daca un raspuns este "No"**: aceasta este o non-conformitate. Documentati un plan de remediere cu data tinta si actiuni specifice. Procesatorul poate accepta temporar cu plan de remediere.

**ASV Scan (Approved Scanning Vendor):**
- Scanare externa trimestriala a IP-urilor/domeniilor publice
- Vendori acreditati: Trustwave, Qualys, Rapid7, Tenable
- Cost: 100-500 EUR/scan (trimestrial = 400-2000 EUR/an)
- Scan-ul trebuie sa fie **PASS** (fara vulnerabilitati critice sau high)
- Daca FAIL: remediati vulnerabilitatile si re-scanati

### Pasul 5 — Incident Response Plan pentru breach de date de card
**Daca suspectati sau detectati un breach:**

**Primele 24 ore (CRITICAL):**
1. Izolati sistemele compromise (disconnect de la retea, NU opriti — pastrati evidențele pentru forensics)
2. Documentati tot ce se intampla (log cronologic cu timestampuri)
3. Notificati procesatorul dvs. (obligatoriu contractual, de obicei 24-72 ore maxim)
4. NU stergeti nimic, NU reinstalati — pastrati evidenta intacta

**In 5 zile:**
- Angajati un PFI (PCI Forensic Investigator) acreditat — lista la pcisecuritystandards.org
- Notificati autoritatile: ANSPDCP (Romania) pentru GDPR breach + date personale
- Notificati bancile emitente afectate (prin procesator)
- Identificati scope-ul: cate carduri afectate, ce date expuse (PAN, CVV, expiry)

**Sanctiuni pentru breach fara compliance:**
| Sanctiune | Valoare | Cine aplica |
|-----------|---------|------------|
| Fine Visa/MC | 5,000-100,000 USD/luna | Card networks |
| Pierdere drept procesare | Permanent posibil | Acquirer + networks |
| Liability re-emitere carduri | 3-10 USD/card x nr. carduri | Issuing banks |
| GDPR fine (Romania) | Pana la 4% din cifra anuala | ANSPDCP |
| Reputational damage | Incalculabil | Piata |

### Pasul 6 — Mentinerea conformitatii ongoing (nu este one-time)
**Calendar anual PCI-DSS:**
| Frecventa | Actiune |
|-----------|--------|
| Zilnic | Review log-uri pentru anomalii (automat cu SIEM sau manual) |
| Saptamanal | Scan malware, review acces conturi |
| Lunar | Patch management review, update inventar sisteme |
| Trimestrial | ASV scan extern, scanare vulnerabilitati interne |
| Semestrial | Review reguli firewall, review acces utilizatori |
| Anual | Completare SAQ, risk assessment, pentest (Level 1-2), training angajati, review IRP |

**Schimbari care declanseaza re-evaluare PCI:**
- Schimbarea procesatorului sau gateway-ului
- Migrarea la alt server/cloud provider
- Modificarea paginii de plata (chiar si UI changes)
- Adaugarea unui nou canal de plata (mobil, telefon)
- Breach sau incident de securitate

### Pasul 7 — PCI-DSS v4.0 cerinte NOI (vs. v3.2.1)
PCI-DSS v4.0 a intrat in vigoare complet in **aprilie 2025**. Cerinte noi cheie:

| Cerinta noua | Descriere | Impact |
|-------------|----------|--------|
| Customized Approach | Alternativa la "Defined Approach" — permite implementari custom daca obiectivul de securitate e atins | Flexibilitate mai mare |
| Multi-Factor Auth everywhere | MFA obligatoriu pentru TOTI utilizatorii cu acces la CDE (nu doar remote) | Setup MFA intern |
| Targeted Risk Analysis | Risk analysis per cerinta, nu generic | Documentatie mai granulara |
| E-commerce skimming prevention | Protectie impotriva scripturilor malitioase pe paginile de plata (Magecart) | CSP headers, SRI, monitoring scripturi |
| Automated log review | Review-ul log-urilor trebuie automatizat (nu doar manual) | SIEM sau echivalent |
| Password 12 chars | Minimum 12 caractere (era 8 in v3.2.1) | Update password policies |

## Verificare
- [ ] Nivelul PCI si tipul de SAQ determinat corect (flowchart completat)
- [ ] Scope redus la minimum (SAQ A daca posibil — hosted payment page)
- [ ] SAQ v4.0 completat si trimis la procesator
- [ ] ASV scan extern realizat si PASS (trimestrial)
- [ ] NU se stocheaza CVV2, PIN sau PAN neencriptat — nicaieri
- [ ] TLS 1.2+ pe toate paginile cu date de card sau redirect
- [ ] MFA activat pentru toti utilizatorii cu acces la sisteme de plata
- [ ] Incident Response Plan documentat si testat (tabletop exercise anual)
- [ ] Training PCI anual completat de toti angajatii relevanti
- [ ] Calendar anual PCI configurat cu remindere

## Instrumente
- PCI SSC portal: `pcisecuritystandards.org` (SAQ-uri, ghiduri oficiale, lista QSA si ASV)
- Trustwave / Qualys / Rapid7 (ASV scanning)
- OWASP ZAP (vulnerability scanning gratuit pentru aplicatii web)
- Cloudflare WAF (Web Application Firewall — plan gratuit disponibil)
- MXToolbox (verificare SSL/TLS configuration)
- Vanta / Drata / Secureframe (platforme automatizare compliance — optional, 5K-15K EUR/an)

## Note
- **PCI-DSS nu este optional** — este o cerinta contractuala cu Visa si Mastercard prin intermediul procesatorului. Non-compliance = riscul de a pierde contul + fine.
- **Cel mai ieftin si eficient mod de a fi compliant**: NU atingeti datele de card (hosted page + tokenizare = SAQ A = 22 cerinte).
- **SAQ D (329 cerinte) vs. SAQ A (22 cerinte)**: diferenta de cost si efort este 10-50x. Faceti ORICE pentru a ramane pe SAQ A.
- PCI-DSS v4.0 este obligatoriu din aprilie 2025 — verificati ca SAQ-ul completat este versiunea 4.0, NU 3.2.1.
- **E-commerce skimming (Magecart attacks)** este noua cerinta v4.0 — implementati Content Security Policy (CSP) headers si Subresource Integrity (SRI) pe paginile cu payment forms.

## Cortex Logging
```json
{
  "type": "procedure_execution",
  "collection": "payment-processing",
  "tags": ["pci-dss", "compliance", "security", "SAQ", "PP-003"],
  "entry": {
    "procedure_id": "PP-003",
    "timestamp": "{{ISO_8601}}",
    "executor": "{{agent_id}}",
    "pci_level": "{{1|2|3|4}}",
    "saq_type": "{{A|A-EP|C-VT|D}}",
    "saq_status": "completed | in-progress | overdue",
    "asv_scan_result": "PASS | FAIL | pending",
    "asv_scan_date": "{{ISO_8601}}",
    "non_conformities": "{{nr}}",
    "remediation_items": "{{nr_open}}",
    "pentest_date": "{{ISO_8601 or N/A}}",
    "outcome": "compliant | non-compliant | remediation",
    "notes": "{{free_text}}"
  }
}
```

## Enforcement Loop

### WHERE
- Activat pe orice business care proceseaza, stocheaza sau transmite date de card.
- Se aplica inclusiv pentru SAQ A (hosted payment page) — 22 cerinte ramase.
- VK: `PP-003-WHERE-ACTIVE`

### WHEN
- La onboarding-ul initial la un procesator (SAQ trebuie completat inainte de go-live).
- Trimestrial: ASV scan extern + scanare vulnerabilitati interne.
- Semestrial: review reguli firewall + review acces utilizatori.
- Anual: completare SAQ, risk assessment, pentest (Level 1-2), training angajati, review IRP.
- La orice schimbare: procesator, server, pagina de plata, canal de plata nou.
- La orice incident de securitate: re-evaluare PCI imediata.
- VK: `PP-003-WHEN-TRIGGERED`

### HOW
- Determinati SAQ-ul aplicabil cu flowchart-ul din Pasul 1 — NU ghiciti.
- Reduceti scope-ul la SAQ A daca posibil (Pasul 2) — diferenta 22 vs 329 cerinte.
- Completati checklist-ul din Pasul 3 integral — marcati N/A unde nu se aplica.
- Calendarul anual PCI (Pasul 6) se configureaza cu remindere automate.
- VK: `PP-003-HOW-EXECUTED`

### CONNECT
- PP-001 (Gateway Selection) — alegerea gateway-ului determina SAQ-ul aplicabil (hosted page = SAQ A).
- PP-002 (Chargeback Management) — 3DS2 si fraud prevention sunt cerinte PCI care reduc si CB-urile.
- PP-005 (High-Risk Onboarding) — website compliance checklist contine elemente PCI.
- VK: `PP-003-CONNECT-LINKED`

### VERIFY
- [ ] SAQ tip determinat corect cu flowchart (Pasul 1).
- [ ] Scope redus la SAQ A daca posibil (Pasul 2).
- [ ] SAQ v4.0 completat si trimis la procesator (Pasul 4).
- [ ] ASV scan PASS trimestrial (Pasul 4).
- [ ] TLS 1.2+ pe toate paginile relevante (Domeniul 4).
- [ ] MFA activat pentru toti utilizatorii cu acces la CDE (Domeniul 8 + v4.0).
- [ ] Incident Response Plan documentat si testat anual (Pasul 5).
- [ ] Calendar anual PCI configurat cu remindere (Pasul 6).
- VK: `PP-003-VERIFY-COMPLETE`

## Model Routing
| Faza | Model | Justificare |
|------|-------|-------------|
| Determinare SAQ tip + nivel PCI | **Sonnet** | Flowchart decision tree — logica clara |
| Completare checklist 12 domenii | **Sonnet** | Matching cerinte cu stare actuala |
| Incident Response (breach activ) | **Opus** | Situatie critica, decizii rapide cu impact mare |
| ASV scan review + remediere | **Sonnet** | Analiza vulnerabilitati si prioritizare fix |
| Training PCI annual prep | **Haiku** | Generare materiale de training standard |

## Metrics
| Metrica | Formula | Target | Red Flag | Frecventa |
|---------|---------|--------|----------|-----------|
| SAQ completion status | Completat DA/NU | DA (anual) | NU sau expirat | Anual |
| ASV scan result | PASS/FAIL | PASS | FAIL | Trimestrial |
| Non-conformitati deschise | Nr. cerinte cu raspuns "No" | 0 | > 3 | Continuu |
| Timp remediere vulnerabilitati critice | Zile de la detectie la fix | < 7 zile | > 14 zile | Per incident |
| MFA coverage | Nr. utilizatori CDE cu MFA / Total utilizatori CDE x 100 | 100% | < 100% | Lunar |
| Training PCI completion | Nr. angajati trainuiti / Nr. angajati relevanti x 100 | 100% | < 90% | Anual |
