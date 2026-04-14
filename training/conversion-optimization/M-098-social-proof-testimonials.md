---
type: procedure
created: 2026-03-22
status: active
slug: m-098-social-proof-testimonials
tags: [procedure, nexus]
---

# Social Proof and Testimonial Collection System — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui sistem sistematic de colectare, validare, formatare și distribuție a social proof pentru creșterea conversiei.

---

## 1. Problema

Lipsa unui sistem structurat de colectare a testimonialelor duce la social proof slab, generic sau inexistent pe materialele de marketing, reducând credibilitatea și conversia. Testimonialele colectate ad-hoc sunt rareori specifice, cuantificabile sau strategic poziționate pentru a elimina obiecțiile cheie.

Situații acoperite:
- Lansare produs/serviciu nou fără social proof existent — construire sistem de la zero
- Optimizarea social proof existent: testimoniale vagi, fără rezultate specifice sau fără credibilitate
- Scalarea colectării de dovezi sociale pentru a acoperi toate etapele funnel-ului și toate obiecțiile identificate

---

## 2. Procedura

### Pas 1: Maparea obiecțiilor și tipurilor de social proof necesare
Identifică top 5-7 obiecții ale prospectilor pe baza datelor (survey-uri, call recordings, chat support). Pentru fiecare obiecție, determină tipul de social proof care o neutralizează cel mai eficient: testimonial text, video testimonial, case study, date agregate, endorsement, user-generated content. Creează o matrice obiecție → tip de social proof necesar.

### Pas 2: Segmentarea clienților sursă
Identifică clienții cei mai potriviți pentru colectarea de testimoniale: cei cu rezultate măsurabile, reprezentativi pentru buyer persona target, cu experiență recentă (sub 90 zile). Prioritizează clienții care pot oferi testimoniale specifice cu cifre concrete (ROI, timp economisit, venituri generate). Creează lista segmentată cu 20-30 de candidați.

### Pas 3: Construirea procesului de outreach
Creează secvența de outreach personalizată: email inițial cu context clar + request specific, follow-up la 3 zile, opțiune alternativă (formular scurt vs. interviu video). Redactează întrebările ghid care generează răspunsuri specifice: "Care era situația înainte?", "Ce rezultat concret ai obținut?", "Cui ai recomanda și de ce?". Automatizează declanșatoarele (post-onboarding, post-milestone, post-renewal).

### Pas 4: Colectarea și formatarea testimonialelor
Colectează răspunsurile și editează minimal pentru claritate (fără distorsionarea sensului — obține aprobare pentru orice editare). Formatează conform structurii: situație înainte → soluția → rezultat specific cuantificat → recomandare. Adaugă context credibil: nume, titlu, companie, fotografie, dacă posibil link verificabil. Respinge testimonialele vagi fără rezultate specifice.

### Pas 5: Validarea și obținerea aprobărilor
Trimite versiunea finală clientului pentru aprobare explicită (email cu versiunea exactă ce va fi publicată). Obține acordul pentru toate canalele de utilizare: website, ads, email, print. Documentează aprobarea cu timestamp. Nu publica niciodată fără aprobare explicită.

### Pas 6: Distribuția strategică în funnel
Poziționează fiecare testimonial la punctul din funnel unde neutralizează obiecția specifică: testimoniale despre rezultate → lângă pricing, testimoniale despre ușurința adoptării → lângă formular de sign-up, case studies detaliate → pagini de comparație. Testează A/B poziționarea și formatele (carusel vs. static, video vs. text).

### Pas 7: Sistemul de actualizare continuă
Setează remindere trimestriale pentru actualizarea testimonialelor. Creează un pipeline automat de solicitare post-milestone (la 30, 60, 90 zile post-achiziție). Arhivează testimonialele mai vechi de 24 luni sau înlocuiește cu unele mai recente. Actualizează procedure-health.json și Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Sistem social proof construit: obiecții mapate, testimoniale colectate cu rezultate specifice, aprobări obținute, distribuite strategic în funnel.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-098",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["social-proof", "testimonials", "cro", "credibility", "user-generated-content"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Testimonial publicat fără aprobare explicită a clientului → violation
- Testimonial generic fără rezultate specifice acceptat ca valid → violation
- Social proof distribuit fără mapare pe obiecții specifice → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Fiecare testimonial are aprobare documentată?
- [ ] Toate testimonialele conțin rezultate specifice cuantificabile?

**VK-uri obligatorii:**
1. `✅ [PROC] M-098 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Social Proof and Testimonial Collection System" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CRM (HubSpot/Pipedrive) | Identificarea clienților candidați și declanșarea outreach |
| Typeform / Google Forms | Colectarea structurată a testimonialelor |
| Email automation (Klaviyo/ActiveCampaign) | Secvența automată de solicitare |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Rată răspuns outreach testimoniale | ≥25% |
| Testimoniale cu rezultate cuantificate | ≥80% din total colectat |
| Acoperire obiecții principale | 100% din top 5 obiecții |
