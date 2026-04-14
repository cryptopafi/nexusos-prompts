---
type: procedure
created: 2026-03-22
status: active
slug: m-033-facebook-custom-lookalike-audiences
tags: [procedure, nexus]
---

# Facebook Custom and Lookalike Audience Creation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea sistematică a audiențelor custom și lookalike în Facebook pentru retargeting eficient și scalare controlată a campaniilor.

---

## 1. Problema

Targetarea fără audiențe custom și lookalike bine construite duce la costuri ridicate și conversii scăzute. Audiențele construite aleatoriu sau fără strategie de funnel generează suprapuneri, epuizare rapidă și imposibilitatea de a scala profitabil campaniile.

Situații acoperite:
- Construire audiențe de la zero pentru cont nou sau campanie nouă
- Restructurare audiențe existente pentru reducerea suprapunerii și a costurilor
- Scalare campanii prin creare lookalike-uri din surse de calitate înaltă

---

## 2. Procedura

### Pas 1: Audit audiențe existente și definire strategie
Deschide Audiences în Business Manager și listează toate audiențele existente cu dimensiunile lor. Identifică audiențele expirate (fără actualizare >180 zile) și suprapunerile majore cu Audience Overlap tool. Definește strategia: TOFU (awareness), MOFU (considerare) și BOFU (conversie) — fiecare nivel necesită audiențe distincte.

### Pas 2: Creare audiențe Website Custom Audiences
Creează audiențe bazate pe pixel pentru fiecare segment cheie al funnelului: Toți vizitatorii (30 zile, 60 zile, 180 zile), vizitatori pagini produs (30 zile), adăugat în coș (14 zile, 30 zile), checkout inițiat (14 zile), cumpărători (30 zile, 90 zile, 180 zile). Setează excluderile corecte (ex: audiența "Cumpărători 30 zile" exclude din audiența "Checkout inițiat 14 zile").

### Pas 3: Creare audiențe Customer List
Importă lista de clienți din CRM cu minim 1.000 de emailuri, preferabil 10.000+. Include câmpurile de enrichment disponibile: email, telefon, first_name, last_name, date_of_birth, city, country pentru a maximiza match rate-ul. Targetul este un match rate >60%. Creează audiențe separate pentru: clienți activi (cumpărători <90 zile), clienți VIP (valoare mare), leads necumpărători, clienți inactivi (>180 zile).

### Pas 4: Creare audiențe de Engagement
Construiește audiențe din interacțiunile cu activele Facebook: persoane care au interacționat cu Pagina (30, 60, 90 zile), persoane care au vizionat minim 50% dintr-un video, persoane care au deschis un Lead Form, persoane care au interacționat cu profilul de Instagram. Aceste audiențe sunt valoroase mai ales când pixelul are date insuficiente (site nou).

### Pas 5: Creare Lookalike Audiences din surse de calitate
Creează Lookalike Audiences din sursele cele mai valoroase, în ordinea priorității: 1) Cumpărători (LTV ridicat) → LAL 1%, 2%, 5%, 2) Customer List VIP → LAL 1%, 3) Checkout inițiat → LAL 1%, 3%, 4) Vizitatori site 30 zile → LAL 1%. Folosește întotdeauna o sursă de minim 1.000 de persoane din aceeași țară cu piața țintă. Nu combina surse din țări diferite.

### Pas 6: Organizare și naming convention
Redenumește toate audiențele cu o convenție clară: `[Tip] | [Sursă] | [Durată] | [Excluderi]` (ex: "WCA | Site - Checkout | 14D | excl. Purchasers"). Această convenție previne confuzia la scalare și auditare. Grupează audiențele pe funnel în interfața Audiences cu prefixe TOFU/MOFU/BOFU.

### Pas 7: Verificare dimensiuni și programare refresh
Verifică că fiecare audiență are o dimensiune suficientă pentru obiectivul său: BOFU minim 1.000 persoane, MOFU minim 10.000, TOFU LAL minim 100.000. Dacă o audiență critică este prea mică, extinde fereastra temporală sau îmbogățește sursa. Programează un audit lunar al audiențelor pentru a elimina cele expirate și a actualiza Customer Lists din CRM.

---

## 3. Cortex Logging

```json
{
  "text": "Audiențe Facebook custom și lookalike construite complet — Website Custom Audiences pe funnel, Customer Lists importate, Engagement Audiences active, Lookalike 1%-5% din surse de calitate, naming convention aplicat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-033",
    "domain": "paid-advertising-social",
    "rule_id": "META-H-002",
    "tags": ["facebook", "audiences", "custom-audiences", "lookalike", "retargeting", "targeting"],
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
- Lookalike creat din sursă cu sub 1.000 persoane → violation
- Customer List importată fără verificarea match rate-ului → violation
- Audiențe fără excluderi definite la nivel BOFU → violation
- Naming convention ignorat → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-social

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Audiențe BOFU au excluderi corecte configurate?
- [ ] Lookalike-urile au surse de minim 1.000 persoane și aceeași țară?

**VK-uri obligatorii:**
1. `✅ [PROC] M-033 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Facebook Custom and Lookalike Audience Creation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Audiences Manager | Creare și management audiențe |
| Facebook Pixel | Sursă date pentru Website Custom Audiences |
| CRM / Export clienți | Sursă pentru Customer List Audiences |
| Audience Overlap Tool | Verificare suprapuneri |
| Conversions API | Semnale suplimentare pentru audiențe |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Customer List match rate | > 60% |
| Dimensiune audiență BOFU | Minim 1.000 persoane |
| Suprapunere audiențe paralele | < 20% |
| Lookalike source size | Minim 1.000 persoane |
