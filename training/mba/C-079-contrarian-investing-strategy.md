---
type: procedure
created: 2026-03-22
status: active
slug: c-079-contrarian-investing-strategy
tags: [procedure, nexus]
---

# Apply Contrarian Investing/Business Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Cadru pentru identificarea și acționarea oportunităților contra-consensus în investiții și strategie de business, cu analiză de sentiment și timing.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Consultați un profesionist licențiat.

---

## 1. Problema

Consensul pieței sau al industriei este adesea greșit la extreme: la euforie maximă activele sunt supraevaluate, la panică maximă sunt subevaluate. Gândirea contrariană nu înseamnă a fi mereu împotriva curentului — înseamnă a evalua independent și a acționa când dovezile sunt clare dar piața nu a ajuns acolo încă.

Situații acoperite:
- Activ sau sector tratat cu dispreț uniform de media și analiști, dar cu fundamentale solide
- Strategie de business contrar tendinței dominante din industrie cu oportunitate de diferențiere
- Moment de piață cu Fear & Greed la extreme (sub 20 sau peste 80)

---

## 2. Procedura

### Pas 1: Identifică consensul dominant și cuantifică-l
Caută: ce crede "toată lumea" despre această oportunitate? Măsoară sentiment prin: Fear & Greed Index, put/call ratio, short interest %, titluri media din ultimele 30 zile, sondaje analiști. Documentează poziția de consens ca baseline.

### Pas 2: Evaluează fundamentalele independent de sentiment
Analizează activul/oportunitatea strict pe baze fundamentale: earnings, cashflow, poziție competitivă, management, bilanț. Separă complet analiza fundamentală de opinia pieței. Pune întrebarea: "Dacă nu știam ce crede piața, care ar fi valoarea justă?"

### Pas 3: Identifică "the edge" — de ce ești mai informat
Gândirea contrariană fără edge este joc de noroc. Definește explicit de ce evaluarea ta diferă de consens: date pe care piața nu le-a procesat, orizont de timp diferit, expertiză sectorială specifică, eroare logică în narativul dominant. Fără edge documentat, nu acționa.

### Pas 4: Evaluează catalizetorii de reversie
Consensul greșit poate rămâne greșit mult timp. Identifică: ce eveniment sau date ar schimba narativul? Când se publică earnings? Ce trigger extern ar forța reevaluarea? Fără un catalyst probabil în 6–18 luni, poziția contrariană este prea devreme — și prea devreme = greșit în piețe.

### Pas 5: Dimensionează poziția la convingere, nu la oportunitate
Pozițiile contrariate au un timing incert. Dimensionează proporțional cu nivelul de convingere și cu toleranța de pierdere: dacă ești 70% convins, maximum 5–10% din portofoliu. Dacă mai devreme decât așteptat, ai capital pentru a adăuga la prețuri mai bune.

### Pas 6: Stabilește exit criteria clare înainte de intrare
Definește dinainte: (a) target dacă ai dreptate și (b) stop-loss dacă fundamentalele se deteriorează (nu dacă prețul scade temporar). Ieșirea din poziție contrariană când consensul s-a mutat în direcția ta = profit realizat, nu grăbeală.

### Pas 7: Documentează teza și revizuiește trimestrial
Scrie teza contrariană în 200–300 cuvinte: consensul actual, de ce crezi că e greșit, ce edge ai, catalyzator, exit criteria. Revizuiește la fiecare 90 zile: teza rămâne valabilă? Consensul s-a schimbat? Actualizează sau închide poziția.

---

## 3. Cortex Logging

```json
{
  "text": "C-079 Contrarian Investing/Business Strategy executat: analiză sentiment + fundamentale + edge documentat, teză contrariană scrisă.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-079",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["contrarian", "investing", "strategy", "sentiment-analysis", "timing", "value-investing"],
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
- Poziție contrariană luată fără edge documentat (Pas 3) → violation critică
- Teză contrariană fără catalyst identificat → violation metodologică
- Exit criteria nedefinite înainte de intrare → violation de risc management
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Edge documentat explicit și distinct de opinia de consens?
- [ ] Teza scrisă în 200–300 cuvinte și salvată?

**VK-uri obligatorii:**
1. `✅ [PROC] C-079 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply Contrarian Investing/Business Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Fear & Greed Index / Sentiment tools | Cuantificarea poziției de consens |
| Jurnal de teze de investiție | Documentare și review trimestrial |
| Date fundamentale (financiare, sectoriale) | Evaluare independentă de sentiment |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Teze documentate cu edge explicit | 100% din poziții |
| Review trimestrial al tezelor active | 4x/an |
| Poziții fără exit criteria predefinite | 0% |
