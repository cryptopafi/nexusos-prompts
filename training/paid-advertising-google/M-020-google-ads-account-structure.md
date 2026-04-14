---
type: procedure
created: 2026-03-22
status: active
slug: m-020-google-ads-account-structure
tags: [procedure, nexus]
---

# Google Ads Account Structure Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea corectă a structurii de cont Google Ads (campanii, ad groups, keywords) pentru maximum de relevanță și control.

---

## 1. Problema

O structură de cont Google Ads dezorganizată duce la Quality Score scăzut, costuri ridicate și imposibilitatea de a optimiza eficient. Fără o ierarhie clară — cont → campanie → ad group → keyword → anunț — managementul devine haotic și performanța suferă. Această procedură standardizează procesul de setup pentru orice cont nou sau restructurare.

Situații acoperite:
- Cont nou Google Ads fără structură definită
- Restructurare cont existent cu campanii dezorganizate
- Separare campanii pe obiective diferite (brand vs. non-brand, produse vs. servicii)

---

## 2. Procedura

### Pas 1: Audit obiective business și buget disponibil
Identifică obiectivele principale (lead generation, e-commerce, brand awareness) și bugetul lunar total disponibil. Documentează în scris înainte de orice configurare. Această etapă determină câte campanii vei crea și cum le vei împărți bugetul.

### Pas 2: Definește structura de campanii pe obiectiv
Creează câte o campanie separată pentru fiecare obiectiv major: Brand, Non-Brand, Competitor, Remarketing. Separă campaniile și pe rețea (Search vs. Display vs. Shopping) — nu le mixa niciodată în aceeași campanie. Setează buget zilnic individual per campanie.

### Pas 3: Creează Ad Groups tematice (SKAG sau tematic)
Pentru Search, organizează Ad Groups fie pe Single Keyword Ad Groups (SKAG) pentru control maxim, fie pe teme strânse de 3-5 keyword-uri înrudite. Fiecare Ad Group trebuie să aibă un singur intent clar. Limitează-te la maximum 20 Ad Groups per campanie la setup inițial.

### Pas 4: Populează keyword lists cu match types corecte
Adaugă keywords în fiecare Ad Group folosind cel puțin Exact Match și Phrase Match. Evită Broad Match fără Smart Bidding activ. Adaugă imediat o listă de negative keywords la nivel de campanie (minimum 20 termeni irelevanti cunoscuți).

### Pas 5: Configurează setările de campanie (locație, limbă, scheduling)
Setează localizarea geografică strict la piața țintă — dezactivează "Presence or interest" și activează doar "Presence". Setează limba corespunzătoare. Configurează ad scheduling dacă ai date despre orele cu conversii (altfel lasă all day și optimizează ulterior).

### Pas 6: Creează anunțuri (minimum 2 RSA per Ad Group)
Fiecare Ad Group trebuie să conțină minimum 2 Responsive Search Ads cu headlines și descriptions diferite. Asigură-te că keyword-ul principal apare în cel puțin 1 headline pinned la poziția 1. Activează Ad Rotation pe "Optimize" pentru a favoriza anunțul cu performanță mai bună.

### Pas 7: Validează structura și setează tracking înainte de activare
Verifică că fiecare campanie are: buget setat, locație corectă, bid strategy definită, minimum 1 Ad Group cu minimum 2 anunțuri și minimum 5 keywords. Confirmă că conversion tracking este activ (vezi M-022) înainte de a porni campaniile. Documentează structura finală în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "M-020 executat: Structură cont Google Ads configurată. Campanii create: [N]. Ad Groups: [N]. Keywords totale: [N]. Negative keywords adăugate: [N]. Conversion tracking activ: DA/NU.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-020",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "account-structure", "campaign-setup", "ad-groups", "keywords", "skag"],
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
- Campanii Search și Display mixate în aceeași campanie → violation
- Ad Group creat cu mai mult de un intent distinct de keyword → violation
- Campanie activată fără conversion tracking configurat → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Fiecare campanie are rețea separată (Search/Display/Shopping)?
- [ ] Minimum 2 RSA per Ad Group create?

**VK-uri obligatorii:**
1. `✅ [PROC] M-020 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Ads Account Structure Setup" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Ads Account | Platformă principală |
| Google Ads Editor | Bulk upload și management offline |
| Conversion Tracking (M-022) | Prerequisit pentru activarea campaniilor |
| Keyword Planner | Research volum și CPC estimat |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Quality Score mediu la 30 zile | Minim 7/10 per keyword |
| Ad Groups per campanie | Maximum 20 la setup |
| Negative keywords la lansare | Minimum 20 per campanie |
