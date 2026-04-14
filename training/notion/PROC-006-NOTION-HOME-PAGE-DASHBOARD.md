---
type: procedure
created: 2026-03-22
status: active
slug: proc-006-notion-home-page-dashboard
tags: [procedure, nexus]
---

# NOTION-HOME-PAGE-DASHBOARD — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea paginii Home din Notion ca centru operațional zilnic, cu widget-uri pentru task-uri, calendar și database views prioritare.

---

## 1. Problema

Fără o pagină Home configurată corect, navigarea în Notion devine manuală și fragmentată — deschizi sidebar-ul, cauți prin TeamSpaces, navighezi între baze de date separate pentru a vedea task-urile, deadline-urile și prioritățile zilei. Aceasta pierdere de timp și context se repetă la fiecare sesiune de lucru.

Procedura configurează Home ca un dashboard operațional care agregă task-uri din surse multiple, evenimentele din calendar și view-urile critice ale workspace-ului — toate vizibile dintr-un singur loc, fără navigare suplimentară.

**Situații acoperite:**
- Setup inițial workspace nou (prima configurare Home)
- Restructurare workspace existent pentru eficiență zilnică
- Adăugare surse noi de task-uri (nou proiect, nouă bază de date)
- Conectarea Notion Calendar după instalarea app-ului

---

## 2. Procedura

### Pas 1: DESCHIDE HOME SI EVALUEAZA STAREA CURENTA
Navighează la Home (click pe "Home" în sidebar stânga sau shortcut Cmd+Shift+H). Identifică widget-urile deja prezente. Notează ce lipsește: My Tasks, Upcoming Events, database views customizate. Home este disponibil NUMAI pe desktop — nu mobile/iPad.

### Pas 2: CONFIGUREAZA WIDGET-UL MY TASKS
Click pe widget-ul "My Tasks" (dacă nu există, adaugă-l via butonul "Add widget" din colțul paginii). Intră în Edit al widget-ului:
- Apasă "Add source" → selectează fiecare bază de date care conține task-uri (Tasks DB, Procedure Queue, orice DB de proiect activ)
- Setează filtru: Assignee contains Me OR Assignee is empty
- Alege layout-ul preferat: List (minimal), Board (status vizual), Table (detaliu), Timeline (deadline-uri)
- Verifică că task-urile apar corect din TOATE sursele adăugate

### Pas 3: CONFIGUREAZA WIDGET-UL UPCOMING EVENTS
Adaugă sau configurează widget-ul "Upcoming Events". Acesta necesită Notion Calendar conectat:
- Dacă Notion Calendar nu e instalat: descarcă app-ul (Mac/Windows/iOS/Android) → sign in cu Google → conectează workspace-ul în Settings → Notion → select workspace → Connect
- În widget-ul Upcoming Events de pe Home: toggle-uiește calendarele Google și Notion pe care vrei să le vezi
- Verifică că evenimentele Google Calendar și date din baze de date Notion apar simultan în widget

### Pas 4: ADAUGA DATABASE VIEWS CUSTOMIZATE
Click "Add widget" → selectează "Database view". Alege baza de date și view-ul pe care vrei să-l afișezi pe Home. Exemple recomandate pentru Nexus:
- Procedures DB → filter: Status = Draft OR AUDIT-PRO-Pending, sort: Creat date ASC (vizibilitate backlog)
- Projects DB → Board view, filter: Status = Active (overview proiecte active)
- Tasks DB → filter: Due date = this week, sort: priority DESC (task-uri urgente săptămâna curentă)
Redenumește fiecare widget cu un titlu descriptiv (click pe titlul widget-ului).

### Pas 5: PRIORITIZEAZA BAZE DE DATE CU STAR
În sidebar stânga, navighează la fiecare DB critică și apasă Star (steluța) pentru a o adăuga la Favorites. Menține maxim 5-7 items starred — prea multe anulează avantajul. Starred items apar în secțiunea "Favorites" din sidebar, deasupra TeamSpaces.

### Pas 6: SETEAZA GREETING SI VERIFICA LAYOUT FINAL
Personalizează greeting-ul de pe Home (click pe text). Reordonează widget-urile prin drag-and-drop pentru fluxul tău optim (recomandat: My Tasks top-left, Upcoming Events top-right, database views jos). Fă un test complet: creează un task nou din widget-ul My Tasks → verifică că apare în DB sursă. Marchează un task Done → verifică că dispare din widget.

---

## 3. Cortex Logging

```json
{
  "text": "Notion Home configured as operational dashboard. Sources added to My Tasks: [list DBs]. Calendar connected: [YES/NO]. Custom views added: [list views]. Layout verified: [YES/NO].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PROC-006",
    "domain": "notion",
    "rule_id": "META-H-002",
    "tags": ["notion", "home", "dashboard", "productivity", "workspace"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + orice sesiune de setup/restructurare workspace Notion

### WHEN
- La configurarea unui workspace Notion nou
- La adăugarea unei noi surse de task-uri (nou DB sau proiect)
- La instalarea Notion Calendar
- La restructurarea operațională a workspace-ului

### HOW (violation detection)
- Home configurat fără surse adăugate la My Tasks → violation
- My Tasks widget fără filtru pe Assignee → violation (arată task-uri ale altora)
- Upcoming Events widget prezent fără Notion Calendar conectat → violation (widget vid)
- Mai mult de 10 items în Favorites (sidebar poluat) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:** Configurare Home fără testarea că task-urile apar corect din toate sursele, lăsarea Upcoming Events widget fără calendar conectat.

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry pentru PROC-006
- PROC-008 (NOTION-WORKSPACE-ARCHITECTURE) → Home configurarea vine după architecture setup
- Training Mode → face parte din curriculum Notion Level 4 (Expert/Architect)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] My Tasks widget are cel puțin o sursă configurată?
- [ ] Filtrul pe Assignee setat corect?
- [ ] Upcoming Events widget funcționează (afișează evenimente)?
- [ ] Cel puțin un database view custom adăugat?
- [ ] Test creare/completare task din widget efectuat?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PROC-006 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "NOTION-HOME-PAGE-DASHBOARD" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție procedură | Sonnet (Genie) | Setup ghidat, pași clari fără reasoning complex |
| Audit periodic | Opus | Review structural + verificare enforcement |
| AUDIT-PRO | Opus single agent | Scoring NPLF complet |
| Validare structură | AUDIT-PRO deterministic | Nu e LLM — e checklist |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Notion desktop app | Home disponibil NUMAI pe desktop | notion.so sau app desktop |
| Notion Calendar app | Upcoming Events widget necesită calendar conectat | notional.so/calendar sau app store |
| Google Calendar | Sursă de evenimente pentru Upcoming Events | Conectat via Notion Calendar Settings |
| Task databases (Tasks DB, Projects DB, etc.) | Surse pentru My Tasks widget | Workspace Notion existent |
| Notion AI plan (opțional) | Pentru AI-powered widgets | ~$12/month |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Task sources configured | Număr DB-uri conectate la My Tasks | Minim 2 |
| Upcoming events visible | Events din calendar apar în widget | YES |
| Custom views on Home | Database views adăugate | Minim 2 |
| Starred items in sidebar | Favorites configurate | 3-7 items |
| Setup time | Timp total pentru configurare completă | Sub 20 minute |
