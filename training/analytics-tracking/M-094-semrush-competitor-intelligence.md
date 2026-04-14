---
type: procedure
created: 2026-03-22
status: active
slug: m-094-semrush-competitor-intelligence
tags: [procedure, nexus]
---

# Semrush Competitor Intelligence System — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Analiză competitivă completă în Semrush — de la trafic organic și backlink-uri până la content audit, advertising intelligence și construirea unui plan de acțiune bazat pe date.

---

## 1. Problema

Fără intelligence competitiv sistematic, strategiile de SEO și content sunt create în vid, fără a înțelege ce funcționează în industrie. Concurenții care rankuiesc bine au investit în cuvinte cheie și content care poate fi identificat și depășit printr-o strategie mai bună, nu printr-un buget mai mare.

Situații acoperite:
- Audit SEO competitiv trimestrial
- Intrarea pe o nișă nouă și înțelegerea landscape-ului
- Identificarea oportunităților de keyword gap față de competitori
- Analiza strategiei PPC a competitorilor

---

## 2. Procedura

### Pas 1: Introduce domeniul competitorului în Semrush
Accesează semrush.com → Domain Overview:
- Introdu URL-ul competitorului (ex: `competitor.ro`)
- Selectează țara corectă din dropdown
- Revizuiește overview rapid: Authority Score, Organic Traffic estimat, Organic Keywords, Backlinks

Identifică top 5 competitori dacă nu îi știi: Semrush → Competitors report → Organic Research → Competitors tab.

### Pas 2: Analizează traficul organic
Mergi la Organic Research → Positions:
- **Total keywords** — câte keyword-uri rankuiesc
- **Traffic estimat** — trafic organic lunar estimat
- **Top pages** — paginile cu cel mai mult trafic organic

Filtrează: Keywords în top 3 / top 10. Exportă lista în CSV.

Identifică: ce subiecte / categorii domină traficul competitorului?

### Pas 3: Analizează profilul de backlink-uri
Mergi la Backlink Analytics → Overview:
- **Referring domains** — număr de domenii unice care linkuiesc
- **Authority Score** — calitatea profilului de backlink
- **Anchor text distribution** — diversitate vs over-optimization

Mergi la Backlinks → filtrează după Authority Score > 30:
- Identifică top 20 site-uri care linkuiesc la competitor
- Notează sursele pentru outreach propriu (guest posting, parteneriate, PR)

### Pas 4: Auditează conținutul competitorului
Mergi la Organic Research → Pages → Top Pages:
- Identifică top 10 pagini după trafic estimat
- Analizează: tip de conținut (articol / landing page / tool / calculator)
- Identifică formatul câștigător în nișă

Verifică manual top 3 pagini: lungime, structură, user value, actualitate. Notează ce fac mai bine decât tine.

### Pas 5: Analizează advertising-ul (PPC)
Mergi la Advertising Research → Positions:
- Identifică keyword-urile pe care competitorul rulează ads
- Verifică Ad Copies (creatives) folosite
- Estimează bugetul lunar (Semrush Traffic Cost)

Mergi la Display Advertising pentru bannere și reclame display. Identifică unghiurile de mesaj și ofertele promovate.

### Pas 6: Exportă Keyword Gap Report
Mergi la Keyword Gap (Competitive Research → Keyword Gap):
- Adaugă domeniul tău + 3-5 competitori
- Filtrează: Missing keywords (rankuiesc competitorii, tu nu) și Weak (rankuiești mai prost)
- Sortează după Volume descrescător

Exportă top 50 keyword-uri Missing cu potențial cel mai mare. Acestea sunt prioritatea ta de content.

### Pas 7: Construiește planul de acțiune
Bazat pe toată analiza, creează plan structurat pe 3 coloane:

| Oportunitate | Acțiune | Prioritate |
|---|---|---|
| Keyword gap X | Creează articol / landing page | HIGH |
| Backlink de la site Y | Outreach guest post | MEDIUM |
| Ad copy superior la competitor Z | Testează angle nou în ads | HIGH |

Limitează la maximum 10 acțiuni cu responsabili și deadline. Calitatea > cantitate.

---

## 3. Cortex Logging

```json
{
  "text": "Semrush Competitor Intelligence executat: domeniu competitor analizat, trafic organic + backlinks auditate, top content identificat, PPC intelligence colectat, Keyword Gap raportat, plan de acțiune cu priorități creat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-094",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["semrush", "competitor-intelligence", "seo", "keyword-gap", "backlinks", "ppc", "content"],
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

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum analytics-tracking
- `procedure-health.json` → adaugă entry la fiecare execuție

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-094 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Semrush Competitor Intelligence System" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Semrush (cont plătit) | Platformă principală competitor intelligence |
| Google Sheets / Excel | Export și organizare date |
| Semrush Keyword Gap Tool | Identificare missing keywords |
| Semrush Backlink Analytics | Analiza profilului de backlink-uri |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Competitori analizați | Minimum 3 |
| Keyword Gap exportat | Top 50 missing keywords |
| Plan de acțiune | Maximum 10 acțiuni prioritizate |
| Frecvență analiză | Trimestrial |
