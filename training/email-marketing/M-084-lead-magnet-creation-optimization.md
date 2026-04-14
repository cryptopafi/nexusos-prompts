---
type: procedure
created: 2026-03-22
status: active
slug: m-084-lead-magnet-creation-optimization
tags: [procedure, nexus]
---

# Lead Magnet Creation and Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea și optimizarea unui lead magnet care convertește vizitatorii în subscriberi calificați.

---

## 1. Problema

Un lead magnet slab sau neoptimizat generează liste de email cu calitate scăzută și rate de conversie minime. Fără o propunere de valoare clară și un format adecvat audienței, efortul de list-building devine ineficient. Procedura asigură că fiecare lead magnet este construit strategic pentru maximum de relevanță și conversie.

Situații acoperite:
- Crearea unui lead magnet nou de la zero pentru o campanie
- Optimizarea unui lead magnet existent cu conversie sub benchmark
- Alegerea formatului potrivit de lead magnet pentru o audiență specifică

---

## 2. Procedura

### Pas 1: Identificarea problemei-pivot a audienței
Definește problema numărul 1 pe care audiența țintă vrea să o rezolve imediat. Folosește research din forumuri, comentarii, review-uri și interviuri cu clienți. Lead magnetul trebuie să promită rezolvarea acestei probleme specifice, nu un subiect general.

### Pas 2: Alegerea formatului optim
Selectează formatul în funcție de complexitatea problemei și comportamentul audienței: checklist (acțiune rapidă), template (economie de timp), ghid PDF (profunzime), mini-curs email (educare progresivă), webinar înregistrat (demonstrație vizuală), calculator/tool (valoare utilitară). Prioritizează formatele cu timp de consum sub 10 minute pentru conversii maxime.

### Pas 3: Redactarea titlului și a promisiunii
Scrie un titlu cu beneficiu specific și cuantificabil. Formula recomandată: "[Număr/Adjectiv] + [Rezultat specific] + [Timp/Condiție]". Testează minimum 3 variante de titlu. Titlul trebuie să comunice valoarea în sub 10 cuvinte.

### Pas 4: Crearea conținutului lead magnetului
Livrează exact ce promite titlul — nu mai puțin, nu semnificativ mai mult. Structurează conținutul în pași acționabili. Include un element de "quick win" în primele 20% din conținut pentru a valida decizia subscriberului. Adaugă un CTA la final către pasul următor în funnel.

### Pas 5: Designul paginii de opt-in
Creează o pagină dedicată (nu popup ca singur element) cu: headline care reia promisiunea lead magnetului, bullet points cu beneficii concrete, imagine preview a materialului, formular minimal (email + prenume max), social proof (număr subscriberi sau testimonial). Elimină toate distragerile și link-urile externe.

### Pas 6: Configurarea secvenței de livrare și urmărire
Setează automația: email de confirmare trimis în max 5 minute cu link direct de download, email de follow-up la 24h pentru a verifica dacă au accesat materialul, email la 3 zile cu un "next step" valoros. Verifică livrabilitatea în inbox înainte de lansare.

### Pas 7: Testarea și optimizarea continuă
Lansează lead magnetul și monitorizează: rata de opt-in (benchmark: 20-40% pentru trafic rece, 40-60% pentru trafic cald), rata de download efectiv, engagement cu secvența de follow-up. Testează A/B titlul paginii după minimum 200 vizitatori. Revizuiește lead magnetul la fiecare 6 luni pentru relevanță.

---

## 3. Cortex Logging

```json
{
  "text": "Lead magnet creat/optimizat conform M-084: titlu testat, format ales, conținut acționabil, pagină opt-in configurată, secvență automată activă.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-084",
    "domain": "email-marketing",
    "rule_id": "META-H-002",
    "tags": ["lead-magnet", "opt-in", "list-building", "conversion", "email-marketing"],
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
- Titlu lead magnet scris fără testarea a min. 3 variante → violation
- Email de livrare trimis fără test în inbox real înainte de lansare → violation
- Lead magnet creat fără identificarea problemei-pivot (Pas 1 sărit) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum email-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 3 variante de titlu testate?
- [ ] Secvența de livrare configurată și testată în inbox?

**VK-uri obligatorii:**
1. `✅ [PROC] M-084 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Lead Magnet Creation and Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Email platform (Mailchimp/ActiveCampaign/etc.) | Automație livrare și follow-up |
| Canva / design tool | Crearea materialului vizual |
| Landing page builder | Pagina de opt-in |
| Analytics (GA4 / pixel) | Tracking conversii opt-in |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Rata de opt-in (trafic rece) | ≥ 20% |
| Rata de opt-in (trafic cald) | ≥ 40% |
| Rata de download efectiv | ≥ 70% din subscriberi |
