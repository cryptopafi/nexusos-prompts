---
type: procedure
created: 2026-03-22
status: active
slug: c-083-learn-earn-return-career
tags: [procedure, nexus]
---

# Apply the "Learn, Earn, Return" Career Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea cadrului de carieră în trei faze (Learn → Earn → Return) pentru navigarea deliberată a traiectoriei profesionale și de impact.

---

## 1. Problema

Carierele fără un cadru deliberat tind să stagneze, să se fragmenteze sau să urmeze inerția în loc de alegere. Cadrul Learn-Earn-Return oferă o structură temporală clară pentru a maximiza achiziția de competențe, crearea de bogăție și contribuția la comunitate — în ordine, nu simultan.

Situații acoperite:
- Profesionist la începutul carierei care nu știe ce să prioritizeze: bani, experiență sau impact
- Persoană în faza de câștig care se simte blocată sau nealiniată cu scopul mai larg
- Necesitatea de a face tranziția deliberată între faze fără a arde etape

---

## 2. Procedura

### Pas 1: Identifică faza curentă și validează alinierea
Determină în care fază ești acum: Learn (primii 5–10 ani, accent pe competențe și experiență diversă), Earn (maximizare venituri și acumulare de capital), sau Return (contribuție, mentorat, filantropie, moștenire). Faza greșită = obiective greșite. Validează cu cineva care te cunoaște bine.

### Pas 2: Definește criteriile de tranziție la faza următoare
Faza Learn → Earn: ai un set de competențe distinctiv și cerere demonstrată pe piață. Faza Earn → Return: ai atins libertatea financiară (definiți-o numeric) și un surplus de capital sau timp. Nu treci la faza următoare fără să bifezi criteriile — fiecare fază are o contribuție specifică la cea care urmează.

### Pas 3: Optimizează deciziile majore pentru faza curentă
In Learn: alege roluri care maximizează expunerea la competențe valoroase, nu salariul. In Earn: maximizează rata de acumulare, alege piețe și roluri cu leverage financiar ridicat, minimizează lifestyle inflation. In Return: alege contribuții cu impact multiplicat (educație, mentorat de mulți, sisteme).

### Pas 4: Construiește active în faza Learn care plătesc dividende în Earn
Rețeaua profesională, reputația, expertiza specifică și obiceiurile de muncă construite în Learn sunt capitalul invizibil al fazei Earn. Investește deliberat în reputație, relații și competențe cu shelf-life lungă (nu tool-uri perisabile). Acestea compoundează.

### Pas 5: Gestionează tranziția Learn → Earn fără a abandona learning-ul
Faza Earn nu înseamnă stop learning — înseamnă că learning-ul devine mai selectiv și orientat spre leverage mai mare. Continuă să investești 5–10% din timp în competențe noi relevante pentru nivelul următor. Stagnarea în Earn produce obsolescență prematură.

### Pas 6: Pregătește Return-ul cu 3–5 ani înainte de tranziție
Return-ul nu se improvizează. Identifică din timp: ce cauze sau domenii îți pasă profund? Ce tip de contribuție are impact multiplicat prin skillset-ul tău specific (nu oricine poate face)? Construiește relații cu organizații sau comunități cu 3–5 ani înainte de a face tranziția activă.

### Pas 7: Documentează progresul și revizuiește anual
Scrie o evaluare anuală de 1 pagină: în ce fază ești, ce ai realizat în acel an față de obiectivele fazei, dacă criteriile de tranziție se apropie. Share-uiește cu un mentor care înțelege cadrul. Cadrul fără documentare devine abstractie neaplicată.

---

## 3. Cortex Logging

```json
{
  "text": "C-083 Learn-Earn-Return Career Framework executat: faza curentă identificată, obiective aliniate, evaluare anuală documentată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-083",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["career", "framework", "learn-earn-return", "personal-development", "wealth-building", "giving-back"],
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
- Tranziție de fază fără criterii definite și bifate → violation
- Decizii de carieră luate fără aliniere la faza curentă → violation metodologică
- Evaluare anuală omisă → violation cadență
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Faza curentă identificată și validată cu criteriile de tranziție?
- [ ] Evaluare anuală scrisă și salvată?

**VK-uri obligatorii:**
1. `✅ [PROC] C-083 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Learn, Earn, Return Career Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Evaluare anuală de carieră (document) | Tracking progres și aliniere la fază |
| Mentor familiar cu cadrul | Validare externă a fazei și tranziției |
| Plan financiar (pentru criteriile Earn → Return) | Definirea libertății financiare numeric |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Evaluare anuală completată | 1x/an |
| Criteriile de tranziție definite numeric | 100% (nu vague) |
| Mentor check-in despre aliniere la fază | Minim 2x/an |
