---
type: procedure
created: 2026-03-22
status: active
slug: proc-008-notion-workspace-architecture
tags: [procedure, nexus]
---

# NOTION-WORKSPACE-ARCHITECTURE — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proiectarea și inițializarea unui workspace Notion multi-echipă de la zero, cu ierarhie corectă TeamSpaces → Pages → Databases, roluri de acces și convenții de naming standardizate.

---

## 1. Problema

Un workspace Notion construit fără o arhitectură deliberată degenerează rapid în haos: baze de date duplicate pentru același tip de entitate, naming inconsistent care face căutarea inutilizabilă, drepturi de acces prea largi care duc la modificări accidentale ale structurii, și lipsa unui singur punct de referință pentru fiecare tip de informație.

Această procedură definește secvența corectă de decizii arhitecturale pentru orice workspace nou — sau pentru restructurarea unuia existent — astfel încât scalarea la echipe mai mari sau volume mari de date să nu necesite refactoring dureros.

**Situații acoperite:**
- Workspace nou pentru un proiect, echipă sau sistem (ex: Nexus restructurare)
- Onboarding unui nou domeniu în workspace-ul existent (ex: adăugare TeamSpace Crypto)
- Restructurare workspace haotic moștenită (baze de date duplicate, fără ierarhie)
- Audit de arhitectură trimestrial

---

## 2. Procedura

### Pas 1: DEFINESTE DOMENIILE SI ENTITATI-CHEIE
Înainte de a deschide Notion, documentează pe hârtie sau whiteboard:
- **Domenii** (ex: Marketing, Dev, Finance, Research, Operations) — fiecare va deveni un TeamSpace
- **Entități** (ex: Tasks, Projects, Procedures, Contacts, Resources) — fiecare va deveni o master database
Regula: o entitate = o singură bază de date master. Nu duplica Contacts DB pentru Marketing și Sales — creează una singură cu view-uri separate.

### Pas 2: CREEAZA TEAMSPACES PER DOMENIU
În Notion: Settings → TeamSpaces → Create a TeamSpace. Creează câte un TeamSpace pentru fiecare domeniu identificat la Pas 1. Naming convention pentru TeamSpaces: `[Emoji] [Domain Name]` (ex: `📊 Finance`, `🚀 Marketing`, `⚙️ Operations`). Emoji-urile permit scanare vizuală rapidă în sidebar fără citire text.

### Pas 3: CONFIGUREAZA ROLURI DE ACCES
Pentru fiecare TeamSpace, asignează roluri deliberat (Settings → Members → Roles):
- **Admin**: maxim 1-2 persoane per TeamSpace. Poate edita structura, invita membri, schimba setările.
- **Member**: default pentru toți colaboratorii activi. Poate edita conținut, nu structura.
- **Guest**: acces per-pagină doar. Nu poate accesa TeamSpaces. Folosit pentru clienți sau colaboratori externi pe un singur proiect.
Principiu: minimum necessary access. Prea mulți admins = structural drift garantat.

### Pas 4: STABILESTE NAMING CONVENTIONS OBLIGATORII
Documentează și publică convenția de naming ÎNAINTE de a crea orice pagină sau DB. Format recomandat:
- Pagini de proiect: `[DEPT]-[ProjectName]-[Status]` (ex: `MKTG-GamblingSEO-Peru-Active`)
- Task-uri: `[Verb] [Object]` (ex: `Write landing page copy`, `Setup Zapier integration`)
- Baze de date: `[Entity] DB` (ex: `Tasks DB`, `Procedures DB`, `Contacts DB`)
- Views: `[Audience/Context] [Format]` (ex: `My Tasks Board`, `Team Overview Table`, `This Week List`)
Creează o pagină "Workspace Standards" în fiecare TeamSpace care documentează convenția.

### Pas 5: CREEAZA MASTER DATABASES PER TIP DE ENTITATE
Creează câte o singură bază de date master pentru fiecare entitate (Tasks, Projects, Procedures, etc.). Plasează master DB-urile într-un TeamSpace dedicat (ex: `⚙️ Operations` sau `🗄️ Core Systems`). Schema de bază pentru orice master DB:
- Title (obligatoriu, e și numele paginii)
- Status (Select: Draft, Active, Done, Archived)
- Owner/Assignee (Person property)
- Created date (Created time — auto)
- Domain/Category (Select sau Multi-Select)
- Relation la alte master DB-uri (configurat în Pas 6)

### Pas 6: CONFIGUREAZA RELATII INTRE MASTER DATABASES
Linkează master DB-urile cu Relation properties (two-way obligatoriu):
- Tasks DB ↔ Projects DB (fiecare task aparține unui proiect)
- Procedures DB ↔ Projects DB (proceduri folosite în proiecte)
- Contacts DB ↔ Projects DB (persoane implicate în proiecte)
Enable "two-way" la crearea fiecărei relații. Adaugă Rollup properties în Projects DB: count de Tasks, count de Procedures linkate.

### Pas 7: CREEAZA WIKI PAGES SHARED SI LOCK DATABASES CRITICE
Creează pagini wiki partajate în fiecare TeamSpace:
- "Start Here" — navigare, naming conventions, roluri
- "Resources" — link-uri, documente de referință, tool stack
- "Team Contacts" — cu directorul de contacte embedded sau linked
Aplică Lock pe toate master DB-urile stabile (3 dots → Lock page). Lock previne editarea structurii de către Members. Unlock temporar când e necesar să adaugi properties noi.

### Pas 8: VERIFICA ARHITECTURA CU CHECKLIST
Înainte de a invita echipa în workspace:
- [ ] Un TeamSpace per domeniu creat?
- [ ] O singură master DB per tip de entitate?
- [ ] Roluri asignate corect (minim admins)?
- [ ] Naming conventions documentate pe "Workspace Standards" page?
- [ ] Relații two-way configurate între DB-urile principale?
- [ ] Wiki "Start Here" pages create și populate?
- [ ] Master DB-urile stabile locked?

---

## 3. Cortex Logging

```json
{
  "text": "Notion workspace architecture completed. TeamSpaces created: [N, list domains]. Master DBs created: [list]. Relations configured: [list pairs]. Roles setup: [N admins, N members]. Lock applied: [YES/NO]. Naming conventions documented: [YES/NO].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PROC-008",
    "domain": "notion",
    "rule_id": "META-H-002",
    "tags": ["notion", "workspace", "architecture", "teamspaces", "databases", "design"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + orice sesiune de creare sau restructurare workspace Notion

### WHEN
- La crearea oricărui workspace Notion nou
- La adăugarea unui nou domeniu (TeamSpace) în workspace existent
- La audit trimestrial de arhitectură (detectare drift)
- La onboarding de noi membri în echipă (verificare că arhitectura suportă rolurile)

### HOW (violation detection)
- Baze de date duplicate pentru același tip de entitate → violation critică (ex: două Tasks DB)
- TeamSpace fără pagini wiki "Start Here" și "Workspace Standards" → violation
- Master DB fără Lock aplicat odată schema stabilizată → violation
- Naming conventions nerespectate (verificat la audit) → violation
- Relații one-way în loc de two-way între DB-uri principale → violation
- Runner: Opus audit trimestrial + AUDIT-PRO batch

**Acțiuni interzise:** Crearea unui DB nou pentru o entitate care are deja un master DB (duplicare), asignarea rolului Admin fără aprobare explicită.

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry pentru PROC-008
- PROC-006 (NOTION-HOME-PAGE-DASHBOARD) → Home configurabil corect după ce arhitectura e stabilă
- PROC-007 (NOTION-AI-PROPERTY-AUTOFILL) → AI properties adăugate pe master DB-uri text-heavy
- PROC-009 (NOTION-BUTTON-WORKFLOW-AUTOMATION) → Buttons adăugate pe wiki pages după arhitectură
- Training Mode → face parte din curriculum Notion Level 4 (Expert/Architect)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Un master DB per entitate (zero duplicate)?
- [ ] TeamSpaces create per domeniu?
- [ ] Roluri asignate corect?
- [ ] Naming conventions documentate?
- [ ] Relații two-way configurate?
- [ ] Wiki pages create și populate?
- [ ] Lock aplicat pe DB-urile stabile?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PROC-008 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "NOTION-WORKSPACE-ARCHITECTURE" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție procedură (setup inițial) | Sonnet (Genie) | Pași de configurare UI sistematici |
| Design arhitectural complex | Opus | Reasoning asupra trade-off-urilor de structură |
| Audit trimestrial | Opus | Review structural profund + detectare drift |
| AUDIT-PRO | Opus single agent | Scoring NPLF complet |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Notion workspace cu plan Business/Plus | TeamSpaces disponibile pe planuri plătite | notion.so/pricing |
| Documentație entități și domenii (Pas 1) | Prerequisite pentru orice decizie de arhitectură | Whiteboard/doc extern |
| Lista de membri și roluri dorite | Necesară pentru configurare acces (Pas 3) | HR/team roster |
| Naming conventions agreate în echipă | Trebuie decise ÎNAINTE de Pas 4 | Doc de referință intern |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Master DB duplicates | Entități cu mai mult de un DB | 0 |
| TeamSpaces with wiki | % TeamSpaces cu "Start Here" page | 100% |
| Locked master DBs | % DB-uri stabile cu Lock aplicat | 100% |
| Two-way relations | % relații inter-DB configurate two-way | 100% |
| Admin count | Număr de admins per TeamSpace | Maxim 2 |
| Naming compliance | % pagini care respectă convenția | Minim 95% la audit |
