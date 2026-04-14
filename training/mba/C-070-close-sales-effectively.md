---
type: procedure
created: 2026-03-22
status: active
slug: c-070-close-sales-effectively
tags: [procedure, nexus]
---

# Close Sales Effectively (Two Techniques) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea sistematică a tehnicilor assumptive close și alternative close pentru creșterea ratei de conversie în vânzări.

---

## 1. Problema

Majoritatea vânzătorilor pierd clienți nu din cauza produsului, ci din cauza eșecului în closing: fie evită cererea directă, fie nu gestionează obiecțiile eficient. Fără un proces structurat de closing, conversiile rămân sub potențial indiferent de calitatea prezentării.

Situații acoperite:
- Prospect interesat care nu ia o decizie la finalul conversației de vânzare
- Client care ridică obiecții de preț sau timing fără un răspuns pregătit
- Vânzător care prezintă bine dar nu solicită explicit angajamentul

---

## 2. Procedura

### Pas 1: Confirmă înțelegerea nevoii înainte de closing
Înainte de orice tehnică de closing, rezumă verbal problema clientului și soluția propusă: "Deci, ceea ce aveți nevoie este X, și ceea ce oferim noi rezolvă asta prin Y — corect?" Obține confirmare explicită. Closing fără aliniere este pierdut.

### Pas 2: Detectează semnalele de cumpărare
Identifică microcomportamente: întrebări despre livrare, preț detaliat, timp de implementare, referințe la situații specifice. Când apar 2+ semnale, inițiază closing. Nu aștepta semnalul perfect.

### Pas 3: Aplică Assumptive Close
Vorbește ca și cum decizia este deja luată. "Când vrem să începem — săptămâna aceasta sau în două săptămâni?" Presupune implicit acordul și mută conversația la detalii logistice. Funcționează cel mai bine cu prospecți calzi care au văzut valoarea.

### Pas 4: Aplică Alternative Close ca variantă
Oferă două opțiuni, ambele reprezentând o decizie pozitivă: "Preferați pachetul standard sau cel premium?" sau "Plătiți integral sau în rate?" Elimini opțiunea "nu" din cadrul mental al clientului. Folosit mai ales când prospectul ezită.

### Pas 5: Gestionează obiecțiile cu Feel-Felt-Found
La orice obiecție: "Înțeleg cum vă simțiți (feel). Alți clienți s-au simțit la fel (felt). Ceea ce au descoperit este că... (found)." Validezi, normalizezi, reframezi. Nu contrazice niciodată direct o obiecție.

### Pas 6: Silence după cererea de angajament
După ultimul close, taci. Prima persoană care vorbește pierde avantajul. Lasă 5–10 secunde de tăcere confortabilă. Completarea tăcerii de către prospect semnalează frecvent decizia sau obiecția reală.

### Pas 7: Documentează obiecțiile și close-urile reușite
După fiecare interacțiune, notează: ce obiecție a apărut, ce tehnică a funcționat, ce ar fi putut fi îmbunătățit. Construiți un playbook de obiecții cu răspunsuri validate. Review lunar al close rate.

---

## 3. Cortex Logging

```json
{
  "text": "C-070 Close Sales Effectively executat: tehnici assumptive/alternative close aplicate, obiecții documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-070",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["sales", "closing", "objection-handling", "assumptive-close", "alternative-close", "conversion"],
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
- Closing tentat fără confirmarea alinierii nevoii (Pas 1) → violation
- Obiecție gestionată prin contrazicere directă în loc de Feel-Felt-Found → violation
- Tăcerea după close spartă de vânzător → violation tactică
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Ambele tehnici de close cunoscute și distinse corect?
- [ ] Playbook de obiecții actualizat după sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] C-070 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Close Sales Effectively (Two Techniques)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Playbook de obiecții | Răspunsuri validate la obiecții frecvente |
| CRM / tracker vânzări | Documentare conversații și rezultate |
| Scorecard sesiune vânzare | Evaluare per interacțiune |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Close rate minim | 30% din prospecți calificați |
| Timp mediu la decizie client | < 48h după ultima interacțiune |
| Rata de follow-up documentat | 100% |
