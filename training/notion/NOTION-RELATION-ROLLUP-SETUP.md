---
type: procedure
created: 2026-03-22
status: active
slug: notion-relation-rollup-setup
tags: [procedure, nexus]
---

# NOTION-RELATION-ROLLUP-SETUP — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea sistemului relațional între două sau mai multe baze de date Notion, cu rollup-uri calculate care se actualizează automat.

---

## §1 Problema

Fără relații, bazele de date Notion sunt silozuri izolate: datele sunt duplicate prin copy-paste, calculele agregate sunt manuale, și orice schimbare necesită actualizări în mai multe locuri. Rollup-urile fără relații preexistente sunt imposibil de creat — ordinea operațiunilor este critică.

Situații acoperite:
- Două baze de date împart date și ai nevoie de calcule agregate (ex: total tasks per project)
- Vrei să afișezi date dintr-o bază de date în alta fără duplicare
- Ai o bază de date cu categorii și vrei să numeri/sumezi itemii per categorie
- Construiești un sistem Projects → Tasks → People care trebuie să rămână sincronizat automat

---

## §2 Procedura

### Pas 1: Verifică prerequisite-urile
Înainte de a crea orice relație sau rollup:
- Confirmă că ambele baze de date există și au date de test (minimum 3 rânduri fiecare)
- Identifică direcția relației: DB-A are o relație CU DB-B (DB-A este "sursa", DB-B este "ținta")
- Decide tipul relației: One-to-Many (un proiect are mai multe task-uri) sau Many-to-Many (un task poate aparține mai multor proiecte)

### Pas 2: Creează proprietatea Relation în DB-A
Deschide DB-A (sursa):
1. Click "+" pentru a adăuga o proprietate nouă
2. Selectează tipul "Relation"
3. În dialogul de selecție, alege DB-B (ținta)
4. **OBLIGATORIU**: activează toggle-ul "Show on [DB-B]" pentru relație bidirecțională (two-way)
5. Setează "Limit" la "No limit" dacă un item poate fi legat de mai multe rânduri din DB-B
6. Dă un nume descriptiv proprietății (ex: "Tasks" în Projects DB, "Project" în Tasks DB)
7. Click "Add relation"

### Pas 3: Verifică backlink-ul în DB-B
Deschide DB-B și confirmă că:
- Apare o proprietate nouă cu numele ales la Pas 2
- Proprietatea afișează corect relațiile create din DB-A
- Modificările în DB-A se reflectă imediat în DB-B (testează cu un item de test)

Dacă backlink-ul nu apare: relația two-way nu a fost activată. Șterge relația și recreaz-o cu toggle-ul activat.

### Pas 4: Populează relațiile cu date de test
Înainte de a crea rollup-uri, linkuiește manual câteva rânduri pentru a testa:
1. Deschide un rând din DB-A
2. Click pe câmpul Relation
3. Selectează 2-3 rânduri din DB-B
4. Verifică că rândurile selectate din DB-B afișează corect referința înapoi la DB-A

### Pas 5: Creează proprietatea Rollup în DB-A
Cu relația funcțională și date de test populate:
1. Click "+" pentru o proprietate nouă în DB-A
2. Selectează tipul "Rollup"
3. Configurează:
   - **Relation**: selectează relația creată la Pas 2
   - **Property**: selectează proprietatea din DB-B pe care vrei să o agregezi (ex: "Estimated Hours", "Status", "Cost")
   - **Calculate**: alege funcția de agregare:
     - `Count all` — numărul total de itemi legați
     - `Count checked` — numărul de checkbox-uri bifate (pt task completion)
     - `Sum` — suma valorilor numerice
     - `Average` — media valorilor numerice
     - `Percent checked` — procent de finalizare (ideal pentru progress tracking)
     - `Show original` — afișează valorile brute (nu agregat)

### Pas 6: Configurează afișarea vizuală a rollup-ului (opțional dar recomandat)
Pentru rollup-uri numerice care reprezintă progres:
1. Editează proprietatea Rollup → secțiunea "Number format"
2. Schimbă display la "Bar" sau "Ring"
3. Setează "Divide by" la valoarea maximă (ex: 100 pentru procente, numărul total de task-uri)
4. Salvează — bara/inelul se actualizează live când datele din DB-B se schimbă

### Pas 7: Testează cu scenarii edge-case
Verifică comportamentul rollup-ului în situații limită:
- Rând din DB-A fără niciun item legat în DB-B → rollup trebuie să afișeze 0 sau gol (nu eroare)
- Rând din DB-B șters → relația din DB-A trebuie să dispară automat
- Adăugare de item nou în DB-B și linkuire la DB-A → rollup trebuie să se actualizeze instant

---

## §3 Cortex Logging

Ce se salvează în Cortex după execuție:

```json
{
  "text": "Relație Notion creată: [DB-A] → [DB-B] (two-way). Rollup configurat: [proprietate] calculat cu [funcție]. Pattern: [ex: Projects→Tasks, count checked tasks per project].",
  "collection": "notion-operations",
  "metadata": {
    "type": "procedure-execution",
    "procedure": "NOTION-RELATION-ROLLUP-SETUP",
    "rule_id": "META-H-002",
    "tags": ["notion", "relation", "rollup", "database", "automation"]
  }
}
```

---

## §4 Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — la orice moment în care Genie sau Codex conectează două baze de date Notion
- Post-H Skill extraction — când o lecție despre relații Notion este salvată ca procedură
- NOTION-DATABASE-DESIGN-PATTERN Pas 4 — această procedură este apelată explicit

### WHEN
- La FIECARE creare de relație Notion nouă
- La orice restructurare a unui sistem relațional existent
- Când un rollup returnează eroare sau valori incorecte

### HOW (violation detection)
- Rollup creat înainte de relație → violation (imposibil de creat, dar tentativa = violation)
- Relație creată fără two-way toggle activat când bidirecționalitatea era necesară → violation
- Rollup configurat fără testare cu date edge-case (Pas 7) → violation
- Backlink-ul din DB-B neconfirmat (Pas 3 sărit) → violation
- Runner: audit manual la AUDIT-PRO periodic; verificare vizuală în Notion la sesiunile de workspace review

### CONNECT
- META-H-002 → enforcement obligatoriu
- NOTION-DATABASE-DESIGN-PATTERN → Pas 4 din aceea apelează această procedură
- NOTION-FORMULA-CREATION → formulele pot combina rollup-uri ca input
- `procedure-health.json` → entry: `NOTION-RELATION-ROLLUP-SETUP`

### VERIFY (procedural checkpoint)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii §1-§7 urmați)
- [ ] Rollup-ul se actualizează corect când datele din DB-B se schimbă?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per execuție** (per VK-H-001):
1. **Execution VK**: `✅ [PROC] NOTION-RELATION-ROLLUP-SETUP | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK**: `✅ [CORTEX] "NOTION-RELATION-ROLLUP-SETUP" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție procedură | Sonnet (agent activ) | Pași structurați, configurare UI |
| Decizie arhitecturală (Many-to-Many vs One-to-Many) | Opus | Reasoning despre structura datelor |
| Audit rollup-uri incorecte | Opus single agent | Debugging relațional complex |
| Validare automată relații | Deterministic script | Nu e LLM |

---

## §5 Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| NOTION-DATABASE-DESIGN-PATTERN | Prerequisite — ambele baze de date trebuie să existe | `~/.nexus/procedures/training/notion/NOTION-DATABASE-DESIGN-PATTERN.md` |
| NOTION-FORMULA-CREATION | Formule care consumă valorile rollup | `~/.nexus/procedures/training/notion/NOTION-FORMULA-CREATION.md` |
| procedure-health.json | Registry de sănătate proceduri | `~/.nexus/procedures/openclaw/procedure-health.json` |
| Notion Web App | Interfața de configurare relații și rollup-uri | `https://notion.so` |
