---
id: IGC-103
title: Transaction Monitoring Rules Engine Setup
domain: igaming-compliance
source: UKGC LCCP Section 12 / EU 5AMLD-6AMLD / FATF Recommendations / Featurespace TM Best Practices
version: 1.0
forge_score: —
business_mapping: ["smsads", "gambling-seo", "affiliate-ops"]
status: ACTIVE
created: 2026-03-20
rule_id: META-H-002
---

# Transaction Monitoring Rules Engine Setup — Procedura Standard de Operare

## Obiectiv

Configurarea și operarea unui sistem de monitorizare a tranzacțiilor pentru operatori iGaming, conform cerințelor AML per jurisdicție. Include definirea regulilor, thresholds, alerting și raportare SAR/STR.

## Prerequisites

- TM platform (Featurespace, NICE Actimize, Feedzai, ComplyAdvantage, or in-house)
- PEP/sanctions database subscription (World-Check, Dow Jones, ComplyAdvantage)
- MLRO (Money Laundering Reporting Officer) appointed
- Risk Assessment Matrix template
- Access to SAR/STR filing portals per jurisdiction

## 1. Problema

Operatorii iGaming sunt obligați legal să monitorizeze tranzacțiile pentru detectarea money laundering și terrorist financing. UKGC LCCP Section 12 impune risk assessment anual și politici AML proporționale. MGA aliniază la EU 5AMLD/6AMLD cu CDD la €2,000 cumulate. GGL impune deposit limit €1,000/lună cross-operator. Fiecare jurisdicție are thresholds și obligații diferite. Lipsa unui rules engine configurat corect duce la: sancțiuni regulatory (amenzi UKGC de milioane GBP), pierderea licenței, facilitarea money laundering. Această procedură oferă un framework de setup aplicabil multi-jurisdicțional.

**Business Application**: SMSads (transaction monitoring pe platforma de billing — tranzacții SMS gambling sunt financial flows monitorizabile), Gambling SEO (înțelegerea cerințelor TM pentru operatorii parteneri — due diligence), Affiliate operations (awareness despre ce monitorizează operatorii — impactul asupra traffic quality și player referrals).

---

## 2. Procedura

### Pas 1: RISK ASSESSMENT INIȚIAL
Conform UKGC LCCP 12.1.1 și MGA AML Directive, execută risk assessment pe business:
- **Riscuri de produs**: tipul de joc (casino > betting în risc ML), volume tranzacții, metode de plată acceptate (crypto = high risk, cards = medium, bank transfer = lower risk dar higher value)
- **Riscuri de client**: jurisdicții clienților (high-risk countries FATF Grey/Black List), PEPs, tip audiență (VIP/high roller = enhanced monitoring)
- **Riscuri de canal**: online vs retail, mobile, third-party payment processors
- **Riscuri geografice**: jurisdicții de operare (mapează la FATF risk ratings)
Documentează în Risk Assessment Matrix. Review: minim anual (UKGC) sau la schimbare materială (produs nou, piață nouă, metodă de plată nouă).

### Pas 2: DEFINIRE THRESHOLDS PER JURISDICȚIE
Configurează thresholds diferențiate pe jurisdicție:

**CDD Triggers:**
- **MGA/EU**: CDD la depozit cumulat > €2,000, orice tranzacție suspectă indiferent de sumă
- **UKGC**: CDD risk-based (no fixed threshold, dar industry practice €/£2,000), EDD la PEPs/high-risk
- **GGL**: Verificare identitate ÎNAINTE de primul depozit (cel mai strict), deposit limit €1,000/lună, LUFT cross-operator check
- **Curaçao**: CDD la deschidere cont, threshold variabil per master licensee
- **ONJN**: CDD la prima înregistrare (identitate verificată), threshold depozit €2,000 conform L129/2019
- **BCLB**: CDD la înregistrare, conform POCAMLA Kenya

**Transaction Monitoring Thresholds:**
- **Depozite rapide**: > 3 depozite în 1 oră → alert
- **Depozite mari**: > €5,000 single depozit → alert + EDD review
- **Depozite cumulate**: > €10,000 în 30 zile → alert obligatoriu
- **Retrageri fără joc**: depozit + retragere cu minimal gameplay (< 10% wagered) → red flag ML
- **Metode multiple**: > 3 metode de plată diferite în 7 zile → alert
- **Structurare**: depozite repetate sub threshold (€1,900 x5 în loc de €9,500 single) → red flag ML

### Pas 3: CONFIGURARE RULES ENGINE
Implementează regulile în sistemul TM (intern sau third-party: Featurespace, NICE Actimize, Feedzai, ComplyAdvantage):

**Rule categories:**
1. **Velocity rules**: frecvența tranzacțiilor per time window (depozite/retrageri/logins)
2. **Amount rules**: single transaction, cumulative (daily/weekly/monthly), spike detection
3. **Pattern rules**: structurare (smurfing), round-tripping (deposit-withdraw-no play), third-party payments
4. **Behavioral rules**: schimbare bruscă pattern joc (stakes x10), game switching rapid, device/IP changes
5. **Geographic rules**: VPN detection, multi-jurisdicție login, high-risk country access
6. **Payment method rules**: crypto conversions, prepaid cards multiple, e-wallet layering
7. **Cross-reference rules**: PEP/sanctions list matching (EU Sanctions List, OFAC, UN), adverse media screening

**Per fiecare regulă, definește:**
- Trigger condition (specific, testabil)
- Alert severity (Low/Medium/High/Critical)
- Auto-action (none/flag/freeze/block)
- Escalation path (L1 analyst → L2 MLRO → L3 SAR filing)
- SLA response time (Critical: 1h, High: 4h, Medium: 24h, Low: 72h)

### Pas 4: CONFIGURARE ALERTING ȘI WORKFLOW
Setează workflow-ul de procesare alerte:
- **L1 Triage**: Analyst primește alert, verifică date tranzacție, player history, risk profile. Decizie: close (false positive), escalate (genuine concern), sau request more info (EDD)
- **L2 Investigation**: MLRO (Money Laundering Reporting Officer) review. Deep dive: source of funds, player communication, pattern analysis pe 90 zile
- **L3 SAR/STR Filing**: Dacă suspiciunea se menține post-investigare:
  - **UKGC**: SAR la NCA (National Crime Agency) via UKFIU
  - **MGA**: STR la FIAU Malta (Financial Intelligence Analysis Unit)
  - **GGL**: Meldung la FIU Deutschland
  - **Curaçao**: Report la FIU Curaçao (MOT)
  - **ONJN**: Raport la ONPCSB (Oficiul Național de Prevenire și Combatere a Spălării Banilor)
  - **BCLB**: Report la Financial Reporting Centre (FRC) Kenya
- **Tipping off prohibition**: NU informa player-ul despre SAR/STR (infracțiune în toate jurisdicțiile)
- **Record keeping**: toate alertele + deciziile documentate, retenție minim 5 ani (UKGC/MGA), 7 ani (GGL)

### Pas 5: IMPLEMENTARE SCREENING LISTS
Integrează verificări automate contra listelor de sancțiuni și PEP:
- **EU Consolidated Sanctions List**: actualizare zilnică
- **OFAC SDN List**: actualizare zilnică
- **UN Security Council Sanctions**: actualizare la publicare
- **PEP databases**: World-Check (Refinitiv), Dow Jones Risk & Compliance, ComplyAdvantage
- **Adverse media screening**: monitoring continuu pe key players/VIPs
- **FATF Grey/Black Lists**: actualizare la fiecare FATF plenary (3x/an)
Configurează fuzzy matching (threshold 85%+) pentru variații de nume. Review false positives săptămânal.

### Pas 6: TESTING ȘI CALIBRARE
Înainte de go-live și apoi trimestrial:
- **Back-testing**: rulează regulile pe date istorice 6 luni → verifică false positive rate (target < 40%) și false negative rate (target < 5%)
- **Scenario testing**: testează scenarii ML cunoscute (structurare, round-tripping, 3rd party funding) → verifică că alertele se generează corect
- **Threshold tuning**: ajustează thresholds bazat pe rezultate — prea multe false positives = threshold prea low, prea puține alerte = threshold prea high
- **Red team testing**: simulează tranzacții suspecte pe cont test → verifică end-to-end (generare alert → triage → investigare → raportare)
- Documentează rezultatele calibrării și justificarea oricărei modificări de threshold

### Pas 7: RAPORTARE ȘI COMPLIANCE REPORTING
Setează raportare periodică:
- **Internă (lunar)**: total alerte generate, breakdown per severity, false positive rate, SARs filed, median response time, top triggered rules
- **Către board/senior management (trimestrial)**: risk assessment update, significant findings, regulatory changes, resource needs
- **Către regulator (per cerință)**:
  - **UKGC**: Annual Assurance Statement, AML policies on request
  - **MGA**: Compliance audit report anual, AML officer appointment notification
  - **GGL**: Quarterly compliance reports, incident reporting
  - **ONJN**: Raport anual conformitate AML
  - **BCLB**: Annual compliance certificate
- **KPIs de monitorizat**: alerte/1000 tranzacții, SAR conversion rate, median time-to-resolution, false positive rate trend

---

## 3. Cortex Logging

```json
{
  "text": "TM rules engine setup completed: [N] rules configured across [N] jurisdictions. Thresholds calibrated: false positive rate [X]%, coverage [Y]%. Screening lists: [N] integrated. Testing: [PASS/FAIL].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "IGC-103",
    "domain": "igaming_compliance",
    "rule_id": "META-H-002",
    "tags": ["igaming", "compliance", "AML", "transaction_monitoring", "KYC", "SAR", "rules_engine"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execuție

### WHEN
La fiecare execuție a procedurii în context real sau training. Calibrare trimestrială obligatorie.

### HOW (violation detection)
- Rules engine fără risk assessment documentat --> violation
- Thresholds identice pe toate jurisdicțiile (fără diferențiere) --> violation
- Screening lists neactualizate > 7 zile --> violation
- Alert SLA breach (critical > 1h, high > 4h) fără escalare --> violation
- SAR/STR filing omis când investigarea confirmă suspiciunea --> violation (infracțiune penală)
- Back-testing neexecutat > 90 zile --> violation
- Tipping off (informarea player-ului despre SAR) --> violation (infracțiune penală)
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:** Dezactivarea regulilor fără justificare documentată și aprobare MLRO, ignorarea alertelor critical, modificarea thresholds fără back-testing, operare fără screening lists actualizate.

### CONNECT
- META-H-002 --> enforcement FORGE standard
- `procedure-health.json` --> adaugă entry
- Legătură cu IGC-101 (license comparison — thresholds per jurisdicție), IGC-102 (affiliate audit — TM context)

---

## 5. Validation Checklist

- [ ] Risk assessment documented per UKGC LCCP 12.1.1 requirements
- [ ] CDD thresholds configured per jurisdiction (differentiated, not identical)
- [ ] All 7 rule categories implemented (velocity, amount, pattern, behavioral, geographic, payment method, cross-reference)
- [ ] Each rule has: trigger condition, alert severity, auto-action, escalation path, SLA response time
- [ ] L1/L2/L3 alerting workflow operational with SLA enforcement
- [ ] PEP/sanctions screening lists integrated with fuzzy matching (85%+)
- [ ] Back-testing executed on 6-month historical data (false positive <40%, false negative <5%)
- [ ] Scenario testing passed for known ML typologies (structurare, round-tripping, 3rd party)
- [ ] SAR/STR filing process tested per jurisdiction
- [ ] Internal monthly reporting + quarterly board reporting configured

## Anti-Patterns

- Applying identical thresholds across all jurisdictions without differentiation
- Disabling rules without documented justification and MLRO approval
- Ignoring critical alerts beyond the 1-hour SLA
- Modifying thresholds without back-testing validation
- Operating without updated screening lists (stale >7 days)
- Tipping off — informing the player about a SAR/STR filing (criminal offence)
- Setting false positive thresholds too low, causing alert fatigue and missed genuine cases

## References

- UKGC LCCP Section 12 — AML/CTF Requirements
- EU 5AMLD / 6AMLD Directives
- GGL Geldwäschegesetz (GwG) compliance requirements
- FATF Recommendations (40 + 9 Special)
- Featurespace: Transaction Monitoring Best Practices
- NICE Actimize: AML Rules Engine Configuration Guide
- FIAU Malta — Implementing Procedures (Part I & II)
