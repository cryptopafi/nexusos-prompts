---
type: procedure
created: 2026-03-22
status: active
slug: proc-007-notion-ai-property-autofill
tags: [procedure, nexus]
---

# NOTION-AI-PROPERTY-AUTOFILL — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea și utilizarea AI Autofill properties în Notion pentru generarea automată de date structurate (rezumate, action items, quotes) din conținut text-heavy în baze de date.

---

## 1. Problema

Bazele de date cu conținut text dens (proceduri, notițe de meeting, research docs, articole) sunt dificil de scanat manual — fiecare row trebuie deschis individual pentru a înțelege ce conține. Crearea manuală de rezumate sau extragerea de action items pentru sute de pagini este prohibitiv de lentă.

Notion AI Autofill rezolvă această problemă prin generarea automată a coloanelor derivate din conținut: rezumate scurte, liste de acțiuni, citate cheie, traduceri — calculabile în batch pentru întreaga bază de date și auto-actualizate când conținutul paginilor se schimbă.

**Situații acoperite:**
- Baza de date Procedures (297 proceduri) care necesită coloane AI Summary scanabile
- Baze de date de notițe de meeting care necesită Action Items extrase automat
- Research databases care necesită Key Quotes sau rezumate în altă limbă
- Orice bază de date text-heavy unde scanabilitatea în table view e importantă

---

## 2. Procedura

### Pas 1: VERIFICA PLAN NOTION AI
AI Autofill necesită planul Notion AI (~$12/month, separat de planul workspace). Verifică: Settings → Plans → confirmă că Notion AI e activ. Fără acest plan, opțiunile AI nu apar în property selector. Notează costul: fiecare Autofill pe o pagină consumă credite AI.

### Pas 2: DESCHIDE BAZA DE DATE SI ADAUGA PROPERTY NOU
Navighează la baza de date target (ex: Procedures DB). Click "+" la capătul header-ului de coloane pentru a adăuga o proprietate nouă. În selector-ul de tip proprietate, caută secțiunea "AI" — vei vedea opțiunile AI Autofill disponibile.

### Pas 3: SELECTEAZA MODUL AI AUTOFILL POTRIVIT
Alege modul în funcție de use case:
- **Summary**: Generează un rezumat scurt al conținutului paginii. Ideal pentru: proceduri, notițe, articole. Cel mai frecvent utilizat.
- **Action items**: Extrage lista de acțiuni/task-uri din conținut. Ideal pentru: notițe de meeting, briefing-uri, planuri.
- **Key quotes**: Extrage citate sau fraze cheie verbatim din text. Ideal pentru: research, interviuri, transcrieri.
- **Translate**: Traduce conținutul paginii în limba specificată. Ideal pentru: baze de date multilingve.
- **Custom**: Scrie un prompt specific care descrie exact ce vrei să extragă/genereze AI-ul.

### Pas 4: CONFIGUREAZA AUTO-UPDATE
După selectarea modului, în setările proprietății AI, activează toggle-ul **"Auto-update"**. Când e activ, proprietatea AI se recalculează automat de fiecare dată când conținutul paginii se modifică. Fără auto-update, valorile devin stale și trebuie recalculate manual.

### Pas 5: SCRIE PROMPT CUSTOM (OPTIONAL DAR RECOMANDAT)
Pentru modul Custom sau pentru a rafina Summary/Action items, editează prompt-ul. Format recomandat pentru Summary pe Procedures DB:
```
Summarize this procedure in 2 sentences: what problem it solves and when to use it. Be specific, not generic.
```
Format recomandat pentru Action items pe Meeting Notes:
```
Extract all action items as a numbered list. Include owner name if mentioned. Format: [Owner]: [Action]
```

### Pas 6: RULEAZA AUTOFILL ALL PAGES (BATCH)
După configurarea proprietății, în header-ul coloanei AI nou create, apasă "Autofill all pages". Aceasta rulează AI-ul pe toate paginile din DB în batch. Pentru 297 proceduri: estimează 5-15 minute processing time. Progresul e vizibil în coloană pe măsură ce se completează row-urile.

### Pas 7: REVIZUIESTE SI CORECTEAZA EDGE CASES
După batch completion, sortează DB-ul după coloana AI nouă. Identifică row-urile unde AI-ul a produs output nesatisfăcător (pagini goale, pagini cu conținut minim, erori). Pentru aceste pagini: deschide pagina → adaugă conținut suficient → click "Regenerate" pe proprietatea AI individuală.

---

## 3. Cortex Logging

```json
{
  "text": "Notion AI Autofill configured on [DB name]. Mode: [Summary/Action items/Custom]. Pages processed: [N]. Auto-update: ON. Edge cases requiring manual review: [N]. Prompt used: [brief description].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PROC-007",
    "domain": "notion",
    "rule_id": "META-H-002",
    "tags": ["notion", "ai", "autofill", "automation", "database", "productivity"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + orice sesiune de setup sau expansie database Notion text-heavy

### WHEN
- La crearea oricărei baze de date cu conținut text dens (>50 pagini)
- La adăugarea unui nou tip de database cu notițe/proceduri/research
- La migrarea unui database existent care nu are AI properties configurate

### HOW (violation detection)
- Baza de date cu >50 pagini text și fără AI Summary property → violation (scalabilitate compromisă)
- AI property configurată fără Auto-update activ → violation (stale data)
- Batch autofill rulat fără review al edge cases → violation
- Custom prompt nu specifică formatul output-ului → violation (output inconsistent)
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:** Activarea AI Autofill fără Notion AI plan activ (generează erori), rularea batch pe DB cu mii de pagini fără a estima costul de credite.

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry pentru PROC-007
- PROC-006 (NOTION-HOME-PAGE-DASHBOARD) → AI Summary columns îmbunătățesc scanabilitatea în Home dashboard views
- PROC-008 (NOTION-WORKSPACE-ARCHITECTURE) → AI properties configurate ca standard pentru orice DB text-heavy la setup
- Training Mode → face parte din curriculum Notion Level 2 (Advanced Features — AI Properties)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Notion AI plan activ confirmat?
- [ ] Modul AI potrivit selectat pentru use case?
- [ ] Auto-update activat?
- [ ] Batch autofill rulat pe toate paginile?
- [ ] Edge cases revizuite și corectate?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PROC-007 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "NOTION-AI-PROPERTY-AUTOFILL" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Execuție procedură | Sonnet (Genie) | Pași de configurare UI clari, fără reasoning complex |
| Scriere prompt custom | Opus | Prompt engineering precis pentru output consistent |
| Audit periodic | Opus | Review structural + verificare enforcement |
| AUDIT-PRO | Opus single agent | Scoring NPLF complet |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Notion AI plan | Necesar pentru AI Autofill properties | Settings → Plans → Notion AI (~$12/month) |
| Baza de date Notion cu conținut text | Sursă de date pentru AI processing | Workspace Notion existent |
| Notion desktop sau web app | Interfața pentru configurare și batch run | notion.so |
| AI credits | Consumate la fiecare Autofill pe pagină | Incluse în planul Notion AI |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Pages auto-filled | Număr pagini procesate în batch | 100% din DB |
| Auto-update status | Auto-update activ pe toate AI properties | YES |
| Edge case rate | % pagini cu output nesatisfăcător | Sub 5% |
| Summary quality | Rezumate specifice și scanabile | Testare subiectivă pe sample 10 pagini |
| Processing time | Timp batch completion pentru 100 pagini | Sub 10 minute |
