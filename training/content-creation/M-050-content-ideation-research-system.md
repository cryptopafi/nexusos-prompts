---
type: procedure
created: 2026-03-22
status: active
slug: m-050-content-ideation-research-system
tags: [procedure, nexus]
---

# Content Ideation and Research System — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Sistem structurat pentru generarea și validarea ideilor de conținut bazat pe keyword research, analiza competiției și nevoile audienței.

---

## 1. Problema

Fără un sistem de ideație structurat, conținutul creat nu răspunde nevoilor reale ale audienței și nu are potențial de ranking organic. Echipele produc conținut aleatoriu fără validare SEO sau de piață, risipind resurse pe subiecte cu impact redus.

Situații acoperite:
- Planificare calendar editorial lunar sau trimestrial
- Identificare oportunități de conținut neexploatate în nișă
- Validarea relevanței unui subiect înainte de producție

---

## 2. Procedura

### Pas 1: Definirea obiectivelor de conținut
Stabilește obiectivul principal al campaniei de conținut: trafic organic, generare leads, awareness sau educație. Aliniază obiectivul cu stadiul de funnel (TOFU, MOFU, BOFU) și cu persona țintă definită. Documentează obiectivul și KPI-urile asociate înainte de orice research.

### Pas 2: Keyword Research primar
Utilizează tool-uri SEO (Ahrefs, SEMrush, Google Keyword Planner) pentru identificarea keyword-urilor cu volum relevant și dificultate accesibilă. Extrage minimum 30 de keyword-uri seed din nișă și grupează-le pe teme principale. Prioritizează keyword-urile cu intenție de căutare aliniată obiectivului de conținut.

### Pas 3: Analiza competiției și gap analysis
Analizează primele 10 rezultate SERP pentru keyword-urile principale. Identifică topicurile acoperite de competitori și găsește gap-urile — subiecte neabordate sau acoperite superficial. Notează formate de conținut dominante (articole long-form, listicle, how-to, video embeds).

### Pas 4: Analiza intenției de căutare
Clasifică fiecare keyword după intenție: informațională, navigațională, comercială sau tranzacțională. Asigură-te că tipul de conținut planificat se potrivește intenției dominante. Conținut informațional pentru TOFU, conținut de comparație pentru MOFU, landing pages pentru BOFU.

### Pas 5: Ideație structurată și clustering
Generează minimum 20 de idei de articole bazate pe keyword clusters. Grupează ideile în pillar content (articole comprehensive 2000+ cuvinte) și cluster content (articole suport 800-1200 cuvinte). Creează o hartă vizuală a relațiilor dintre pillar și cluster pentru internal linking strategy.

### Pas 6: Validarea ideilor cu audiența
Verifică ideile prin sondaje rapide (Typeform, Google Forms), analiza comentariilor și întrebărilor din comunități relevante (Reddit, Quora, grupuri Facebook). Folosește AnswerThePublic sau AlsoAsked pentru variante de întrebări reale. Prioritizează ideile care rezolvă probleme reale exprimate de audiență.

### Pas 7: Construirea și prioritizarea backlogului editorial
Organizează toate ideile validate într-un backlog editorial cu scoring: volum keyword + dificultate SEO + relevanță pentru obiectiv + urgență sezonieră. Asignează prioritate (High/Medium/Low) și estimare de efort (ore de producție). Populează calendarul editorial pentru următoarele 4-8 săptămâni din top-priority items.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-050 executată: Content ideation și research sistem complet — backlog editorial populat cu idei validate SEO și de audiență.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-050",
    "domain": "content-creation",
    "rule_id": "META-H-002",
    "tags": ["content-ideation", "keyword-research", "editorial-calendar", "SEO", "content-strategy"],
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
- Conținut planificat fără keyword research validat → violation
- Idei adăugate în calendar fără scoring de prioritate → violation
- Gap analysis omis — conținut duplicat față de competitori → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-creation

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 20 idei de conținut generate și validate?
- [ ] Backlog editorial populat cu scoring de prioritate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-050 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Content Ideation and Research System" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Ahrefs / SEMrush | Keyword research și analiza competiției |
| Google Search Console | Date de performanță existing content |
| AnswerThePublic / AlsoAsked | Întrebări reale ale audienței |
| Google Sheets / Notion | Backlog editorial și tracking |
| Buyer Persona (M-053) | Context audiență pentru validare idei |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Idei generate per sesiune | Minimum 20 |
| Keyword coverage | Minimum 30 keyword-uri seed |
| Backlog fill rate | 4-8 săptămâni calendar populat |
| Gap opportunities identificate | Minimum 5 per nișă |
