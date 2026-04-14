---
type: procedure
created: 2026-03-22
status: active
slug: notion-formula-creation
tags: [procedure, nexus]
---

# NOTION-FORMULA-CREATION — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea proprietăților de tip Formula în Notion pentru coloane calculate din alte proprietăți, inclusiv formule aritmetice, logice, de text și de dată.

---

## §1 Problema

Fără formule, datele din Notion trebuie calculate manual sau duplicate în foi de calcul externe. Utilizatorii noi confundă sintaxa Notion cu cea din Google Sheets (referințe de celule vs. referințe de proprietăți), rezultând erori și frustrare. Formulele complexe sunt evitate, pierzând din valoarea automatizării.

Situații acoperite:
- Ai nevoie de o coloană al cărei valoare derivă din alte coloane (ex: profit = venit - cheltuieli)
- Vrei logică condiționată vizuală (ex: "Urgentă" dacă deadline < 3 zile)
- Trebuie să calculezi durate, diferențe de date, sau status compus
- Ai un calcul complex pe care nu știi cum să îl scrii în sintaxa Notion

---

## §2 Procedura

### Pas 1: Identifică proprietățile de input
Înainte de a scrie formula:
- Listează proprietățile din baza de date pe care formula le va folosi ca input
- Notează tipul fiecărei proprietăți (Number, Date, Select, Checkbox, Text, Rollup)
- Verifică că proprietățile există și sunt populate cu date de test

**CRITIC**: în Notion, formulele referențiază proprietăți după NUME, nu după poziție. Sintaxa: `prop("NumeProprietate")`. Nu există referințe de tip A1, B2.

### Pas 2: Determină tipul operației
Alege categoria de formulă necesară:

| Categorie | Când o folosești | Operatori/funcții cheie |
|-----------|-----------------|------------------------|
| Aritmetică | Calcule numerice | `+`, `-`, `*`, `/` |
| Comparație | Condiții true/false | `==`, `!=`, `>`, `<`, `>=`, `<=` |
| Logică condiționată | Valori diferite pe baza unor condiții | `if()`, `and()`, `or()`, `not()` |
| Text | Manipulare șiruri | `format()`, `contains()`, `replace()`, `length()`, `slice()` |
| Dată | Calcule temporale | `now()`, `dateBetween()`, `dateAdd()`, `formatDate()` |

### Pas 3: Construiește formula — template-uri esențiale

**Aritmetică simplă:**
```
prop("Income") - prop("Expenses")
```

**Condiție simplă (if):**
```
if(prop("Status") == "Done", "Completed", "Pending")
```

**Condiție cu comparație numerică:**
```
if(prop("Score") >= 80, "Pass", "Fail")
```

**Zile rămase până la deadline:**
```
dateBetween(prop("Deadline"), now(), "days")
```

**Urgență bazată pe deadline:**
```
if(dateBetween(prop("Deadline"), now(), "days") < 3, "URGENT", "OK")
```

**Procent de finalizare (din rollup):**
```
prop("Tasks Completed") / prop("Total Tasks") * 100
```

**Concatenare text:**
```
prop("First Name") + " " + prop("Last Name")
```

**Condiție multiplă (and/or):**
```
if(and(prop("Priority") == "High", prop("Status") != "Done"), "Action Required", "OK")
```

**Verificare dacă un câmp text conține un string:**
```
if(contains(prop("Tags"), "urgent"), "Flag", "Normal")
```

**Zilele scurse de la creare:**
```
dateBetween(now(), prop("Created"), "days")
```

**Format dată pentru afișare:**
```
formatDate(prop("Deadline"), "MMM DD, YYYY")
```

**Condiție cu valoare lipsă (empty check):**
```
if(empty(prop("Assignee")), "Unassigned", prop("Assignee"))
```

### Pas 4: Scrie formula în Notion
1. Click "+" pentru proprietate nouă → selectează tipul "Formula"
2. Dă un nume descriptiv formulei (ex: "Days Until Deadline", "Profit", "Completion %")
3. Click pe câmpul formulei pentru a deschide editorul
4. Tastează formula — editorul oferă autocomplete pentru funcții și proprietăți
5. Click "Done" sau Enter pentru a confirma

### Pas 5: Folosește ChatGPT pentru formule complexe
Când formula depășește 2-3 operații, delegă scrierea:

Prompt exact pentru ChatGPT:
```
Write a Notion formula that [descrie ce vrei să calculeze în română sau engleză].
The available properties and their types are:
- "[Proprietate1]" (Number)
- "[Proprietate2]" (Date)
- "[Proprietate3]" (Select with options: A, B, C)
- "[Proprietate4]" (Checkbox)
Return only the formula, no explanation.
```

Copiază rezultatul direct în editorul de formule din Notion.

### Pas 6: Formatează outputul
Editează proprietatea Formula după ce funcționează:
- **Number format**: alege între Number, Percent (%), Currency ($, €), Number with commas
- **Date format**: pentru formule care returnează date, configurează formatul de afișare
- **Color-coding**: nu e disponibil direct pe Formula, dar folosește formula ca input pentru o proprietate Select cu culori

### Pas 7: Testează edge-case-uri
Verifică obligatoriu:
- [ ] Rând cu valori null/empty pentru proprietățile de input → formula nu returnează eroare
- [ ] Rând cu valori extreme (0, număr negativ, dată în trecut) → comportament corect
- [ ] Rând complet gol → formula afișează o valoare default sau string gol, nu "NaN"

Pentru a preveni erori cu valori null: învelește proprietățile de input în `toNumber()` sau verifică cu `empty()` înainte de calcul.

---

## §3 Cortex Logging

Ce se salvează în Cortex după execuție:

```json
{
  "text": "Formula Notion creată în [DB-NAME]: '[Nume formulă]' calculează [ce face]. Formula: [textul formulei]. Testat cu edge-cases: null, zero, date extreme — toate OK.",
  "collection": "notion-operations",
  "metadata": {
    "type": "procedure-execution",
    "procedure": "NOTION-FORMULA-CREATION",
    "rule_id": "META-H-002",
    "tags": ["notion", "formula", "automation", "database", "calculation"]
  }
}
```

---

## §4 Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — la orice moment în care Genie sau Codex adaugă o proprietate Formula în Notion
- Post-H Skill extraction — când o formulă Notion reusabilă este documentată
- NOTION-DATABASE-DESIGN-PATTERN Pas 2 — formulele sunt identificate la design

### WHEN
- La FIECARE creare de proprietate Formula în Notion
- Când o formulă existentă returnează erori sau valori incorecte
- La audit periodic al bazelor de date (formule neactualizate la schimbarea numelor proprietăților)

### HOW (violation detection)
- Formulă creată fără testare edge-case (Pas 7) → violation
- Referință la proprietate cu un nume care nu există exact în baza de date → violation (formula returnează eroare)
- Formulă inline cu >3 operații scrisă fără ajutorul ChatGPT Pas 5 și care conține erori → violation
- Runner: audit manual la AUDIT-PRO periodic; verificare vizuală a erorilor în Notion

### CONNECT
- META-H-002 → enforcement obligatoriu
- NOTION-RELATION-ROLLUP-SETUP → rollup-urile furnizează valori numerice pe care formulele le consumă
- NOTION-DATABASE-DESIGN-PATTERN → formulele sunt identificate la etapa de design
- `procedure-health.json` → entry: `NOTION-FORMULA-CREATION`

### VERIFY (procedural checkpoint)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii §1-§7 urmați)
- [ ] Formula returnează valori corecte inclusiv pentru edge-case-uri null și zero?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per execuție** (per VK-H-001):
1. **Execution VK**: `✅ [PROC] NOTION-FORMULA-CREATION | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK**: `✅ [CORTEX] "NOTION-FORMULA-CREATION" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Formule simple (1-2 operații) | Sonnet (agent activ) | Pattern direct din template |
| Formule complexe (3+ operații, logică imbricată) | Opus sau ChatGPT via Zapier | Reasoning logic profund |
| Audit formule existente la redenumire proprietăți | Opus single agent | Scanare sistematică |
| Generare batch formule similare | Codex batch | Repetitive, pattern fix |

---

## §5 Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| NOTION-RELATION-ROLLUP-SETUP | Furnizează valori agregate ca input pentru formule | `~/.nexus/procedures/training/notion/NOTION-RELATION-ROLLUP-SETUP.md` |
| NOTION-DATABASE-DESIGN-PATTERN | Stabilește proprietățile disponibile ca input | `~/.nexus/procedures/training/notion/NOTION-DATABASE-DESIGN-PATTERN.md` |
| ChatGPT (extern) | Generare formule complexe la Pas 5 | `https://chat.openai.com` |
| procedure-health.json | Registry de sănătate proceduri | `~/.nexus/procedures/openclaw/procedure-health.json` |
| Notion Web App | Editorul de formule | `https://notion.so` |
