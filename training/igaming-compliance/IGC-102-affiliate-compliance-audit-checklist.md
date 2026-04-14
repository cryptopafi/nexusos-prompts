---
id: IGC-102
title: Affiliate Compliance Audit Checklist
domain: igaming-compliance
source: UKGC LCCP / MGA Advertising Code / GGL Compliance / GDPR / PECR
version: 1.0
forge_score: —
business_mapping: ["smsads", "gambling-seo", "affiliate-ops"]
status: ACTIVE
created: 2026-03-20
rule_id: META-H-002
---

# Affiliate Compliance Audit Checklist — Procedura Standard de Operare

## Obiectiv

Audit sistematic al compliance-ului afiliaților iGaming pe cerințe de advertising, AML/KYC, responsible gambling și protecția datelor, adaptat per jurisdicție. Aplicabil atât la auto-audit (ca afiliat) cât și la auditarea afiliaților (ca operator).

## Prerequisites

- Access to public licence registers (MGA, UKGC, GGL, ONJN, BCLB)
- GDPR compliance assessment toolkit
- SMS/email campaign audit trail access
- Advertising content inventory across all channels

## 1. Problema

Afiliații iGaming sunt expuși riscurilor de compliance semnificative: promovarea operatorilor fără licență validă, încălcarea regulilor de advertising per jurisdicție, lipsa disclosures obligatorii și non-conformitate cu GDPR/data protection. UKGC și MGA aplică enforcement activ și pe afiliați, nu doar pe operatori. Amenzi UKGC pot ajunge la milioane GBP. GGL a blocat site-uri affiliate fără conformitate. Fără un audit checklist structurat, riscul de sancțiuni este ridicat, iar reputația afiliatului se deteriorează ireversibil.

**Business Application**: SMSads (audit compliance pentru campanii SMS gambling — UKGC/MGA/ONJN impun reguli stricte pe SMS marketing), Gambling SEO (Peru/Kenya/Chile — audit affiliate content per jurisdicție), Affiliate operations (due diligence pe operatorii promovați, verificarea licențelor, conformitate advertising).

---

## 2. Procedura

### Pas 1: INVENTAR OPERATORI PROMOVAȚI
Listează toți operatorii iGaming promovați activ, cu: nume brand, entitate juridică, licențe deținute (tip + număr), jurisdicții în care operează, URL-uri licență (verificabile pe registrele publice). Verifică fiecare licență pe registrul autorității:
- **MGA**: mga.org.mt → Licensee Register (caută după nume/URL)
- **UKGC**: gamblingcommission.gov.uk → Public Register
- **GGL**: ggl-behoerde.de → Whitelist
- **Curaçao**: verificare pe site-ul master licensee (ex: Antillephone, Gaming Curaçao)
- **ONJN**: onjn.gov.ro → Lista operatorilor licențiați
- **BCLB**: bclb.go.ke → Licensed Operators list
Flag imediat orice operator fără licență validă sau cu licență expirată.

### Pas 2: AUDIT ADVERTISING COMPLIANCE
Pentru fiecare canal de promovare (site, email, SMS, social media), verifică:
- **UKGC/ASA requirements**: 18+ badge vizibil, BeGambleAware.org link, T&C clear pentru bonusuri, no misleading claims, no targeting under-25s demographics, no urgency tactics ("limited time" pe bonusuri), no guaranteed winnings language
- **MGA Advertising Code**: mesaj responsible gambling, no targeting minori, T&C bonusuri clare, no false/misleading claims despre șanse de câștig
- **GGL requirements**: disclaimer GGL licence number, restricted advertising hours compliance (dacă aplicabil), no bonus advertising (restricții severe Germania), Spielerschutz messaging
- **ONJN requirements**: mesaj "Jocurile de noroc implică riscuri financiare și dependență", no targeting minori, T&C în română
- **SMS-specific (SMSads)**: opt-in explicit, unsubscribe mechanism funcțional, compliance cu e-Privacy Directive/PECR (UK), GDPR consent, frequency caps, nu trimite în time-restricted windows per jurisdicție
- **SEO-specific**: no cloaking/doorway pages targeting regulated jurisdictions without compliance, affiliate disclosure vizibilă

### Pas 3: AUDIT RESPONSIBLE GAMBLING MESSAGING
Verifică prezența și conformitatea mesajelor de responsible gambling:
- **Site/Landing pages**: link către organizație de suport (GamCare UK, Spielen mit Verantwortung DE, jocresponsabil.ro RO), age verification gate sau statement, self-exclusion information sau link
- **Email/SMS**: responsible gambling footer text, opt-out funcțional
- **Social media**: responsible gambling hashtag/mention în posts promotionale
- **Per jurisdicție**: verifică cerințele specifice (UKGC impune cele mai stricte — customer interaction obligation include și afiliații indirect)
- Scorează: FULL compliance / PARTIAL / NON-COMPLIANT per canal

### Pas 4: AUDIT DATA PROTECTION/GDPR
Verifică conformitatea cu protecția datelor:
- **Cookie consent**: banner GDPR compliant cu opt-in granular (nu pre-ticked boxes)
- **Privacy policy**: menționează procesarea datelor pentru affiliate tracking, data controller identification, legal basis pentru procesare, drepturile utilizatorilor
- **Tracking pixels/cookies**: audit tehnic — listează toate third-party cookies, verifică data sharing agreements cu operatorii
- **Data retention**: politica de retenție documentată și conformă (GDPR: max necesar, justificat)
- **Cross-border data transfers**: dacă datele pleacă din EU/UK → verifică adequacy decisions sau SCCs
- **Kenya-specific**: Data Protection Act 2019 compliance (similar GDPR dar cu particularități locale)

### Pas 5: AUDIT AML/KYC ROLE AFILIAT
Deși afiliații nu sunt direct obligați la CDD, au responsabilități indirecte:
- **No facilitare money laundering**: nu promova metode de depozit anonime, nu încuraja bypass KYC
- **UKGC**: afiliații inclusi în supply chain — operatorii trebuie să auditeze afiliații, deci afiliatul trebuie să poată demonstra compliance
- **MGA**: operatorii MGA obligați să auditeze afiliații (Art. 7 MGA Directive) — pregătește documentație de audit
- **Due diligence pe jucători referrați**: nu genera trafic din surse suspecte (bot traffic, click farms, incentivized registrations cu funding suspicios)
- Documentează: source of traffic, acquisition methods, quality metrics (deposit rate, chargeback rate)

### Pas 6: AUDIT CONȚINUT ȘI CLAIMS
Verifică tot conținutul publicat (articole, reviews, comparații):
- **Accuracy**: informații despre bonusuri actualizate și corecte, T&C accurate
- **Fairness**: reviews nu sunt misleading, pro/contra balanced
- **Disclosures**: affiliate disclosure vizibilă ("We may earn commissions"), conform FTC Guidelines (US) și echivalentele locale
- **No fabricated testimonials**: testimoniale reale, documentabile
- **No predatory targeting**: nu targetezi vulnerable populations (problem gamblers, indebted individuals)
- **GGL-specific**: nu promova wild/scatter/jackpot features izolat ca reason to play

### Pas 7: GENERARE RAPORT ȘI REMEDIERE
Compilează findings într-un raport structurat:
- **Critical findings** (risc imediat de sancțiune): licențe invalide, advertising non-compliant în jurisdicții stricte (UKGC/GGL), lipsa totală a responsible gambling messaging → remediere < 24h
- **High findings** (risc pe termen scurt): GDPR gaps, disclosure lipsă, AML facilitation risk → remediere < 7 zile
- **Medium findings** (risc pe termen mediu): conținut outdated, T&C incomplete → remediere < 30 zile
- **Low findings** (best practice): optimizări responsible gambling, îmbunătățiri UX compliance → remediere la next review
Setează owner + deadline per finding. Re-audit la 30 zile post-remediere.

---

## 3. Cortex Logging

```json
{
  "text": "Affiliate compliance audit completed: [N] operators audited, [N] channels checked. Findings: [CRITICAL] critical, [HIGH] high, [MEDIUM] medium, [LOW] low. Jurisdictions: [LIST]. Overall: [PASS/CONDITIONAL/FAIL].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "IGC-102",
    "domain": "igaming_compliance",
    "rule_id": "META-H-002",
    "tags": ["igaming", "compliance", "affiliate", "audit", "advertising", "GDPR", "responsible_gambling"],
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
La fiecare execuție a procedurii în context real sau training. Audit complet trimestrial obligatoriu. Quick check la fiecare nou operator adăugat.

### HOW (violation detection)
- Audit executat fără verificarea licenței pe registrul autorității --> violation
- Operator promovat cu licență expirată/invalidă fără flag --> violation
- Advertising audit fără verificare per jurisdicție --> violation
- Raport fără categorisire findings (critical/high/medium/low) --> violation
- Remediere critical findings > 24h fără escalare --> violation
- SMS campaign lansată fără audit PECR/GDPR --> violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:** Promovarea unui operator fără licență validă verificată, lansarea campaniilor SMS fără audit GDPR/PECR, omiterea responsible gambling messaging din orice canal, ignorarea critical findings peste deadline.

### CONNECT
- META-H-002 --> enforcement FORGE standard
- `procedure-health.json` --> adaugă entry
- Legătură cu IGC-101 (license comparison — sursă pentru verificare licențe), IGC-104 (responsible gambling messaging guidelines)

---

## 5. Validation Checklist

- [ ] All actively promoted operators inventoried with licence numbers verified on authority registers
- [ ] Advertising compliance audited per channel (site, email, SMS, social) per jurisdiction
- [ ] Responsible gambling messaging verified per channel with compliance score (FULL/PARTIAL/NON-COMPLIANT)
- [ ] GDPR/data protection audit completed (cookies, privacy policy, tracking, data retention)
- [ ] AML/KYC affiliate role obligations documented and evidenced
- [ ] Content accuracy verified (bonus T&C, reviews, disclosures)
- [ ] Findings categorized as Critical/High/Medium/Low with owners and deadlines
- [ ] Remediation timeline set per severity (Critical <24h, High <7d, Medium <30d)
- [ ] Re-audit scheduled 30 days post-remediation

## Anti-Patterns

- Promoting an operator without verifying licence status on the official authority register
- Launching SMS campaigns without prior PECR/GDPR audit
- Omitting affiliate disclosure on review/comparison content
- Using pre-ticked GDPR consent checkboxes
- Auditing advertising compliance generically instead of per jurisdiction
- Skipping AML role documentation because affiliates are "not directly obligated"
- Ignoring critical findings past the 24-hour remediation deadline

## References

- UKGC Licence Conditions and Codes of Practice (LCCP)
- MGA Directive on Advertising (Art. 7 — affiliate audit obligations)
- GGL Glücksspielstaatsvertrag advertising rules
- GDPR (EU 2016/679) — Data Protection
- PECR (UK) — Privacy and Electronic Communications Regulations
- FTC Endorsement Guidelines (affiliate disclosures)
- ONJN OUG 77/2009 advertising provisions
