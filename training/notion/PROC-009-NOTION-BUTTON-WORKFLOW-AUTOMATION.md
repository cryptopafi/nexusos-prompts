---
type: procedure
created: 2026-03-22
status: active
slug: proc-009-notion-button-workflow-automation
tags: [procedure, nexus]
---

# NOTION-BUTTON-WORKFLOW-AUTOMATION — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea de Buttons în Notion care automatizează acțiuni repetitive multi-step (creare pagini, editare proprietăți, inserare conținut) fără Zapier sau integrări externe.

---

## 1. Problema

Acțiunile repetitive care implică 3+ pași manuali în Notion (ex: crearea unui nou task cu status default + assignee + data curentă + template de conținut) sunt o sursă constantă de erori și omisiuni. Fiecare execuție manuală riscă să uite un câmp, să seteze un status greșit sau să creeze pagini fără metadata necesară.

Notion Buttons elimină această fricțiune: un singur click execută o secvență completă de acțiuni predefinite, garantând consistența output-ului fără a necesita integrații externe sau cod. Sunt disponibile pe orice plan, inclusiv gratuit.

**Situații acoperite:**
- Crearea unui nou task/procedură cu defaults predefinite (status, dată, tip)
- Pornirea unui "Daily Review" care creează un journal entry cu date pre-completate
- Inițierea unui nou proiect cu structura standard (page + proprietăți + template de conținut)
- Orice acțiune care se repetă identic de mai mult de 3 ori pe săptămână

---

## 2. Procedura

### Pas 1: IDENTIFICA ACTIUNEA REPETITIVA TARGET
Documentează acțiunea pe care vrei să o automatizezi:
- Ce pagini/DB-uri sunt implicate?
- Ce proprietăți trebuie setate și la ce valori default?
- Ce conținut trebuie inserat (text, template)?
- Cine va folosi butonul și pe ce pagină trebuie plasat?
Exemplu Nexus: "New FORGE Procedure" button → creează pagină în Procedures DB cu Status=Draft, Type=FORGE, Creat=today.

### Pas 2: INSEREAZA BUTTON BLOCK PE PAGINA TARGET
Navighează la pagina unde vrei să plasezi butonul (ex: pagina principală a Procedures DB, wiki "Operations Hub"). Tastează `/button` și selectează "Button" din menu. Apare un bloc "Untitled button" cu text editabil.

### Pas 3: DENUMESTE BUTONUL CU INTENTIE
Click pe textul "Untitled button" și înlocuiește cu un text descriptiv care comunică acțiunea: `+ New FORGE Procedure`, `▶ Start Daily Review`, `📋 New Task from Template`. Textul butonului e vizibil pentru toți utilizatorii paginii — face-l self-explanatory.

### Pas 4: CONFIGUREAZA ACTIUNILE IN SECVENTA
Click pe buton → Edit. În panoul de configurare, adaugă acțiunile în ordinea corectă de execuție:
- **"Create page in"**: selectezi DB-ul target → butonul creează un nou row în acel DB
- **"Edit property"**: setezi o proprietate la o valoare fixă (ex: Status = "Draft")
- **"Insert content above/below"**: inserezi un block de text sau template deasupra/sub buton
- **"Open page"**: navighezi automat la o pagină specifică după execuție
- **"Send notification to"**: trimite notificare unui utilizator Notion specificat
Adaugă câte acțiuni ai nevoie — execuția e secvențială în ordinea din lista de configurare.

### Pas 5: SETEAZA VALORI DEFAULT PENTRU PROPRIETATI
Pentru fiecare acțiune "Edit property" adăugată la Pas 4, configurează valoarea:
- Status → "Draft" (text exact cum apare în Select property)
- Assignee → poate fi setat la persoana curentă sau lăsat nesetat (userele aleg singuri)
- Date → "Today" (opțiune built-in pentru Date properties)
- Text fields → valori fixe de text (ex: Type = "FORGE")
Testează că valorile din configurare matchează exact opțiunile din proprietățile DB-ului.

### Pas 6: TESTEAZA BUTONUL SI VERIFICA OUTPUT-UL
Click pe buton o dată. Verifică:
- S-a creat pagina nouă în DB-ul corect?
- Proprietățile sunt setate la valorile configurate?
- Conținutul inserat e corect?
- Dacă există "Open page" action, navighează corect?
Dacă ceva e greșit: Edit buton → ajustează → retestează. Nu considera butonul funcțional până când testul complet e PASS.

### Pas 7: DOCUMENTEAZA BUTONUL PE PAGINA
Sub sau lângă buton, adaugă un text scurt (callout block recomandat) care explică:
- Ce face butonul (una-două propoziții)
- Cine ar trebui să-l folosească
- Orice prerequisite sau context necesar
Exemplu: `ℹ️ Folosit de Genie la fiecare nouă procedură FORGE. Creează automat pagina în Procedures DB cu defaults standard.`

---

## 3. Cortex Logging

```json
{
  "text": "Notion Button created: [button name]. Actions configured: [N actions, list]. Target DB: [DB name]. Properties set by default: [list property=value]. Placed on page: [page name]. Test: PASS/FAIL.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PROC-009",
    "domain": "notion",
    "rule_id": "META-H-002",
    "tags": ["notion", "button", "automation", "workflow", "no-code"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + orice sesiune în care se identifică o acțiune repetitivă 3+ pași în Notion

### WHEN
- La identificarea oricărei acțiuni manuale repetate consistent (minim 3x/săptămână)
- La setup de noi workspace-uri sau TeamSpaces (Buttons standard create din start)
- La onboarding de noi membri (Buttons simplifică acțiunile fără training extensiv)

### HOW (violation detection)
- Acțiune manuală repetată >3x identificată și niciun Button creat → violation
- Button creat fără test (Pas 6) → violation (potențial de output incorect în producție)
- Button creat fără documentație pe pagină (Pas 7) → violation (utilizatorii nu știu cum/când să-l folosească)
- Buton cu nume generic ("Untitled button", "Click here") → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:** Plasarea unui Button pe o pagină locked (utilizatorii nu pot click), crearea de Buttons cu acțiuni care modifică DB-uri la care utilizatorul nu are acces.

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry pentru PROC-009
- PROC-008 (NOTION-WORKSPACE-ARCHITECTURE) → Buttons adăugate pe wiki pages și DB pages post-arhitectură
- PROC-006 (NOTION-HOME-PAGE-DASHBOARD) → Button "New Task" plasat pe Home pentru acces rapid
- Training Mode → face parte din curriculum Notion Level 3 (Power User — Notion Buttons)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Acțiunea repetitivă target identificată și documentată (Pas 1)?
- [ ] Button denumit descriptiv (nu "Untitled")?
- [ ] Toate acțiunile configurate în secvența corectă?
- [ ] Valori default setate și testate?
- [ ] Test complet PASS (Pas 6)?
- [ ] Documentație adăugată lângă buton (Pas 7)?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PROC-009 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "NOTION-BUTTON-WORKFLOW-AUTOMATION" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Identificare acțiuni repetitive (Pas 1) | Sonnet (Genie) | Pattern recognition în workflow curent |
| Configurare Button (Pas 4-5) | Sonnet (Genie) | Pași de UI clari, fără reasoning complex |
| Design Button chains complexe | Opus | Secvențe multi-acțiune cu dependențe logice |
| Audit periodic | Opus | Review structural + verificare că Buttons sunt documentate |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Notion (orice plan, inclusiv gratuit) | Buttons disponibile pe toate planurile | notion.so |
| Baze de date Notion target | DB-urile în care Buttons creează pagini | Workspace Notion existent |
| Pagina host pentru Button | Pagina pe care e plasat butonul (nu locked) | Orice pagină editabilă |
| Member/Admin access | Necesitar pentru a edita pagina host și configura Button | Workspace roles |

---

## 5 Template-uri Button pentru Nexus (High-Value)

| Button Name | Actions | Plasat pe |
|------------|---------|-----------|
| `+ New FORGE Procedure` | Create in Procedures DB + Status=Draft + Type=FORGE + Date=Today | Procedures DB page |
| `▶ Start Daily Review` | Create in Journal DB + Date=Today + insert template | Operations Hub page |
| `📋 New Task` | Create in Tasks DB + Status=To Do + Assignee=Me | Home page + Projects pages |
| `🚀 New Project` | Create in Projects DB + Status=Planning + insert project template | Projects DB page |
| `✅ Mark Sprint Done` | Edit Status=Done on all linked tasks (filtered) | Sprint/Project pages |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Buttons documented | % Buttons cu callout explicativ | 100% |
| Test pass rate | % Buttons testate complet înainte de deployment | 100% |
| Actions per button | Număr mediu de acțiuni per Button | 3-5 (mai mult = complexitate ridicată) |
| Weekly trigger rate | De câte ori e folosit un Button per săptămână | Minim 3x (altfel nu justifică existența) |
