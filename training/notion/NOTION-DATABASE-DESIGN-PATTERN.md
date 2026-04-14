---
type: procedure
created: 2026-03-22
status: active
slug: notion-database-design-pattern
tags: [procedure, nexus]
---

# NOTION-DATABASE-DESIGN-PATTERN — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Design corect al unei noi baze de date Notion de la zero, inclusiv selectarea tipurilor de proprietăți, relații, și configurarea view-urilor inițiale.

---

## §1 Problema

Fără o procedură clară, bazele de date Notion sunt construite ad-hoc: proprietăți greșite, relații lipsă, view-uri duplicate, naming inconsistent. Rezultatul: redundanță, date imposibil de filtrat, workspace care nu scalează.

Situații acoperite:
- Ai nevoie să urmărești un tip nou de entitate (proiect, task, persoană, procedură, resursă)
- Workspace-ul actual are baze de date duplicate care servesc scopuri similare
- Un colaborator a creat o bază de date fără relații cu restul sistemului
- Migrezi date dintr-un tool extern (Trello, Airtable, Excel) în Notion

---

## §2 Procedura

### Pas 1: Definește entitatea și atributele sale
Răspunde la întrebări înainte de a deschide Notion:
- Ce este un "rând" din această bază de date? (un task, un proiect, o persoană?)
- Care sunt atributele obligatorii? (minimum: Title, Status, Date, Owner)
- Ce informații trebuie să fie searchable/filterable?
- Există deja o bază de date în workspace care acoperă acest tip de entitate?

Dacă există deja o bază de date similară: adaugă un view nou pe cea existentă. Nu crea o bază de date nouă.

### Pas 2: Mapează atributele la tipuri de proprietăți Notion
Folosește această matrice de decizie:

| Nevoia ta | Tipul de proprietate |
|-----------|---------------------|
| Câmp de text liber | Text |
| Valoare unică din listă fixă | Select |
| Valori multiple din listă | Multi-Select |
| Număr, sumă, procent | Number |
| Dată sau interval | Date |
| Bifă completat/nu | Checkbox |
| Link extern | URL |
| Fișier, imagine | Files & Media |
| Persoană din workspace | Person |
| Legătură cu altă bază de date | Relation |
| Valoare calculată din relație | Rollup |
| Coloană calculată din alte coloane | Formula |

Reguli de selecție:
- Select (nu Multi-Select) când un item poate aparține unei singure categorii
- Multi-Select când poate aparține mai multor categorii simultan
- Relation în loc de Select când entitatea referențiată are propriile atribute (nu e doar un string)

### Pas 3: Configurează proprietatea Title
Title este obligatoriu și devine numele paginii peste tot în workspace.
- Formatul Title trebuie să fie consistent și searchable
- Convenție recomandată: `[DEPT]-[EntityName]` (ex: `MKTG-GamblingSEO-Peru`)
- Nu lăsa Title să fie "Untitled" în nicio înregistrare

### Pas 4: Identifică relațiile cu bazele de date existente
Listează toate bazele de date din workspace cu care această bază de date ar trebui să fie conectată.
- Activează Two-Way Relation pentru fiecare relație identificată
- Verifică că backlink-ul apare în baza de date țintă după activare
- Ordinea corectă: creează ÎNTÂI relația, APOI rollup-ul (rollup-ul depinde de relație)

### Pas 5: Creează view-urile inițiale
Configurație minimă pentru orice bază de date nouă:
1. **Table view** — suprafața de editare primară, toate proprietățile vizibile
2. **Board view** — Kanban grupat după proprietatea Status (urmărire pipeline)
3. **Calendar view** — necesită o proprietate Date populată (vizualizare deadline-uri)

Numește view-urile descriptiv: "Editor Table", "Status Board", "Deadline Calendar" — nu "View 1".

### Pas 6: Blochează schema după stabilizare
Odată ce schema este stabilă și testată cu date reale:
- Trei puncte (…) → Lock page
- Documentează proprietățile în comentariile bazei de date sau într-o pagină Wiki anexată
- Informează echipa că schema este locked și cum să solicite modificări

---

## §3 Cortex Logging

Ce se salvează în Cortex după execuție:

```json
{
  "text": "Baza de date [NUME-DB] creată în Notion: [N] proprietăți, [M] view-uri, relații cu [DB-A, DB-B]. Schema locked. Convenție naming: [format].",
  "collection": "notion-operations",
  "metadata": {
    "type": "procedure-execution",
    "procedure": "NOTION-DATABASE-DESIGN-PATTERN",
    "rule_id": "META-H-002",
    "tags": ["notion", "database", "design", "workspace"]
  }
}
```

---

## §4 Enforcement Loop (META-H-002)

### WHERE
- WISH Step H — la orice moment în care Genie sau Codex creează o bază de date Notion nouă
- Session Checklist — verificat la fiecare sesiune care include Notion workspace changes

### WHEN
- La FIECARE creare de bază de date nouă în Notion
- La fiecare migrare de date dintr-un tool extern
- La orice restructurare a workspace-ului

### HOW (violation detection)
- Bază de date Notion fără proprietate Status → violation
- Bază de date cu mai puțin de 2 view-uri configurate → violation
- Relație identificată în Pas 4 dar neimplementată → violation
- Title cu format inconsistent față de convenția workspace → violation
- Runner: audit manual la AUDIT-PRO periodic; Opus review la restructurări majore

### CONNECT
- META-H-002 (hard rule) → enforcement obligatoriu la orice procedură nouă
- NOTION-RELATION-ROLLUP-SETUP → Pas 4 din această procedură trimite către aceea
- NOTION-WORKSPACE-ARCHITECTURE → convenția de naming vine din acea procedură
- `procedure-health.json` → entry: `NOTION-DATABASE-DESIGN-PATTERN`

### VERIFY (procedural checkpoint)
La finalul execuției procedurii, agentul care a executat-o verifică:
- [ ] Procedura a fost executată complet? (toți pașii §1-§6 urmați)
- [ ] Baza de date are Title, Status, Date, Owner și cel puțin 2 view-uri?
- [ ] VK emis în sesiune?
- [ ] Dacă oricare = NU → procedura NU e completă, nu marca DONE

**Două VK-uri obligatorii per execuție** (per VK-H-001):
1. **Execution VK**: `✅ [PROC] NOTION-DATABASE-DESIGN-PATTERN | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. **Save VK**: `✅ [CORTEX] "NOTION-DATABASE-DESIGN-PATTERN" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție procedură | Sonnet (agent activ) | Task structurat, pași clari |
| Decizie arhitecturală complexă (relații multiple) | Opus | Reasoning profund necesar |
| Audit periodic schema | Opus single agent | Review structural |
| Validare automată proprietăți | Deterministic script | Nu e LLM |

---

## §5 Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| NOTION-RELATION-ROLLUP-SETUP | Pas 4 — configurarea relațiilor identificate | `~/.nexus/procedures/training/notion/NOTION-RELATION-ROLLUP-SETUP.md` |
| NOTION-WORKSPACE-ARCHITECTURE | Convenție naming și ierarhie workspace | `~/.nexus/procedures/training/notion/` |
| procedure-health.json | Registry de sănătate proceduri | `~/.nexus/procedures/openclaw/procedure-health.json` |
| Notion Web App | Interfața de creare baze de date | `https://notion.so` |
