---
type: procedure
created: 2026-03-22
status: active
slug: proc-010-notion-forms-data-collection
tags: [procedure, nexus]
---

# NOTION-FORMS-DATA-COLLECTION — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-05
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea și configurarea Notion Forms ca înlocuitor nativ pentru Typeform/Google Forms, cu submissions care se scriu direct în baze de date Notion pentru procesare, filtrare și raportare imediată.

---

## 1. Problema

Colectarea de date structurate de la persoane externe (clienți, candidați, utilizatori, publicul) necesita în mod tradițional tool-uri terțe (Google Forms, Typeform, JotForm) cu export manual sau Zapier pentru a ajunge în Notion. Aceasta introduce latență, puncte de eșec suplimentare și costuri extra.

Notion Forms permite crearea de formulare publice (fără cont Notion necesar) care scriu submissions direct ca rânduri în baze de date Notion existente — disponibil pe planul gratuit și instant procesabile în orice view (Table, Board, Calendar, Gallery).

**Situații acoperite:**
- Colectare aplicații pentru joburi (candidații completează form, apare în Candidates DB)
- Feedback de la clienți post-livrare (submissions în Feedback DB, filtrate pe proiect)
- Onboarding de noi membri (date colectate în Members DB)
- Cereri de proceduri noi (submissions în Procedure Requests DB)
- Orice colectare de date externe care trebuie procesate în Notion

---

## 2. Procedura

### Pas 1: CREEAZA SAU IDENTIFICA BAZA DE DATE TARGET
Notion Forms se linkează la o bază de date existentă — submissions = rânduri noi în acel DB. Opțiuni:
- Folosești un DB existent: verifică că are proprietățile (coloanele) care corespund câmpurilor din form
- Creezi un DB nou dedicat formularului: creează-l ÎNAINTE de form, cu toate proprietățile necesare
Schema minimă recomandata pentru orice form DB: Title (câmpul principal), Submission Date (Created time), Status (Select: New, In Review, Done), Source (Text sau Select).

### Pas 2: CREEAZA PAGINA FORM
Navighează la locul unde vrei form-ul în sidebar. Click "+ New page". În selectorul de tip pagini, caută și selectează **"Form"** (apare ca opțiune de tip pagină, nu block). Se creează automat un form conectat la o bază de date — Notion te va întreba ce DB să conecteze. Selectează DB-ul de la Pas 1.

### Pas 3: CONFIGUREAZA CAMPURILE FORMULARULUI
Notion generează automat câmpuri din proprietățile DB-ului conectat. Pentru fiecare câmp:
- **Redenumește**: click pe label → editează cu un text uman, clar, fără jargon intern (ex: "Your Name", nu "Title property")
- **Adaugă descriere**: click pe "+" sub câmp → adaugă instrucțiuni scurte pentru completare
- **Marchează Required/Optional**: toggle "Required" pe câmpurile obligatorii
- **Reordonează**: drag-and-drop câmpurile în ordinea logică a completării
- **Ascunde câmpuri interne**: câmpuri ca "Status", "Assignee", "Created date" NU trebuie să apară în form — ascunde-le (toggle "Show in form" off)

### Pas 4: CUSTOMIZEAZA MESAJUL DE CONFIRMARE
Click pe secțiunea "Submit" sau Settings ale form-ului. Editează mesajul post-submit: înlocuiește textul default ("Your response has been submitted") cu ceva personalizat și specific:
- Confirmă ce s-a întâmplat: "Mulțumim! Aplicația ta a fost primită."
- Setează așteptările: "Vei primi un răspuns în 3-5 zile lucrătoare."
- Adaugă context sau next steps dacă e relevant.

### Pas 5: TESTEAZA FORM-UL CU O SUBMISIE REALA
Click "Share" → copiază link-ul public. Deschide link-ul într-un tab incognito (fără cont Notion). Completează form-ul ca și cum ai fi utilizatorul extern. Submit. Verifică imediat în DB-ul conectat că:
- S-a creat un rând nou?
- Toate câmpurile sunt populate corect?
- Câmpurile ascunse nu apar în form dar există în DB?
- Mesajul de confirmare apare corect?
Dacă orice pas din această verificare eșuează → revino la configurare.

### Pas 6: CREEAZA VIEWS PENTRU PROCESAREA SUBMISSIONS
În DB-ul conectat, creează cel puțin două view-uri pentru procesarea eficientă a submissions:
- **"Inbox" view**: filter Status = New, sort Created date ASC (cel mai vechi primul) — queue de procesare
- **"All Submissions" view**: nefiltrat, sort Created date DESC — vizibilitate completă
Opțional:
- **"In Review" view**: filter Status = In Review — ce e în lucru
- **"Done" view**: filter Status = Done — arhivă

### Pas 7: DISTRIBUIE LINK-UL SI DOCUMENTEAZA SURSA
Copiază link-ul public (Share → Copy link). Distribuie pe canalele relevante (email, website, Slack, social). Documentează în DB-ul de form (sau un doc separat): unde e distribuit link-ul, data lansării, cui e adresat. Dacă vrei să embed-uiești form-ul pe un site extern: folosește `/embed` în Notion sau copiază link-ul în orice pagină web care suportă iframe embeds.

---

## 3. Cortex Logging

```json
{
  "text": "Notion Form created: [form name]. Connected to DB: [DB name]. Fields configured: [N fields, list]. Required fields: [list]. Test submission: PASS/FAIL. Share link: [YES - distributed to: channel/platform]. Processing views created: [list view names].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PROC-010",
    "domain": "notion",
    "rule_id": "META-H-002",
    "tags": ["notion", "forms", "data-collection", "submissions", "no-code"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + orice sesiune în care se identifică necesitatea de colectare date externe structurate

### WHEN
- La orice necesitate de colectare date externe (clienți, candidați, publicul)
- Când un tool extern (Google Forms, Typeform) e folosit pentru date care trebuie să ajungă în Notion
- La setup de noi procese de onboarding sau feedback

### HOW (violation detection)
- Form creat fără test de submisie completă (Pas 5) → violation critică (potențial de pierdere de date)
- Câmpuri interne (Status, Assignee) vizibile în form → violation (confuzie pentru utilizatori externi)
- DB conectat fără view "Inbox" pentru procesarea submissions → violation (submissions pierdute nerevizuite)
- Mesajul de confirmare nemodificat din default → violation (experiență slabă pentru utilizatori)
- Form distribuit fără documentare a canalelor de distribuție → violation (pierdere de tracking)
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:** Conectarea unui form la un DB cu date sensibile fără să verifici ce câmpuri sunt expuse, distribuirea link-ului de form fără mesaj de confirmare customizat.

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry pentru PROC-010
- PROC-008 (NOTION-WORKSPACE-ARCHITECTURE) → DB-urile de colectare forms fac parte din master DB architecture
- PROC-009 (NOTION-BUTTON-WORKFLOW-AUTOMATION) → Button "Mark as Done" în processing views pentru eficiență
- Training Mode → face parte din curriculum Notion Level 3 (Power User — Notion Forms)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] DB conectat are schema corectă pentru date colectate?
- [ ] Câmpurile interne ascunse din form?
- [ ] Câmpurile required configurate corect?
- [ ] Mesaj de confirmare customizat?
- [ ] Test submission completat și verificat în DB?
- [ ] View "Inbox" creat pentru procesare?
- [ ] Link distribuit și sursa documentată?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PROC-010 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "NOTION-FORMS-DATA-COLLECTION" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Creare form (Pas 1-7) | Sonnet (Genie) | Pași de UI clari, fără reasoning complex |
| Design schema DB pentru form complex | Opus | Structurarea proprietăților pentru use cases complexe |
| Audit periodic | Opus | Review structural + verificare că forms sunt active și procesate |
| AUDIT-PRO | Opus single agent | Scoring NPLF complet |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Notion (orice plan, inclusiv gratuit) | Forms disponibile pe toate planurile | notion.so |
| Baza de date Notion target | Primește submissions ca rânduri noi | Workspace Notion existent |
| Link public form | Distribuit utilizatorilor externi (nu necesită cont Notion) | Generat la Share → Copy link |
| Browser incognito | Testare form din perspectiva utilizatorului extern | Orice browser |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Test pass rate | % Forms testate complet înainte de distribuire | 100% |
| Inbox review rate | % submissions procesate din view Inbox în <48h | Minim 90% |
| Field completion rate | % câmpuri required completate de utilizatori | Minim 80% (dacă sub 80% → simplifică form) |
| Confirmation message customized | % Forms cu mesaj de confirmare customizat | 100% |
| Distribution documented | % Forms cu canale de distribuire documentate | 100% |
