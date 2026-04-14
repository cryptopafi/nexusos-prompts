---
type: procedure
created: 2026-03-22
status: active
slug: m-070-facebook-insights-analytics
tags: [procedure, nexus]
---

# Facebook Insights and Social Media Analytics — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Analiză completă a performanței social media prin Facebook Insights — de la metrici cheie până la optimizarea calendarului de conținut bazată pe date.

---

## 1. Problema

Fără analiza sistematică a Facebook Insights, conținutul este publicat fără date despre ce funcționează, când funcționează și pentru cine. Echipele de social media postează la ore arbitrare și nu înțeleg ce tipuri de conținut generează engagement real versus reach artificial.

Situații acoperite:
- Revizuire lunară performanță pagină Facebook
- Optimizarea strategiei de conținut bazată pe date
- Raportare performanță social media pentru client sau management

---

## 2. Procedura

### Pas 1: Accesează dashboard-ul Facebook Insights
Mergi la pagina ta de Facebook → Professional Dashboard sau Facebook Business Suite → Insights. Setează intervalul de date la "Last 28 days" pentru comparabilitate standard. Verifică că ai acces de Admin sau Analyst la pagină.

### Pas 2: Analizează metricile cheie ale paginii
Revizuiește metricile principale:
- **Reach** — numărul unic de persoane care au văzut conținut
- **Engagement** (likes + comments + shares + clicks) — interacțiune totală
- **Follower growth** — schimbare netă în numărul de urmăritori
- **Page views** — vizite la pagina de Facebook

Identifică trendul față de perioada precedentă (in creștere / stagnare / scădere).

### Pas 3: Analizează performanța postărilor per tip
Filtrează postările pe tipuri și compară engagement rate:
- **Video** — reach și watch time (>3 secunde)
- **Image** — engagement rate (likes + comments + shares / reach)
- **Text/Link** — click-through rate

Identifică tipul de conținut cu cel mai mare engagement rate mediu. Acesta ar trebui să domine calendarul editorial.

### Pas 4: Identifică orele optime de postare
Mergi la Insights → Your Audience → When your fans are online:
- Identifică zilele cu activitate maximă
- Identifică intervalele orare cu cel mai mult engagement (de obicei 8-10, 12-14, 19-21)
- Documentează top 3 intervale orare per zi a săptămânii

Ajustează programul de publicare conform acestor date.

### Pas 5: Monitorizează performanța paginilor competitoare
Accesează Pages to Watch (Insights → Overview → Pages to Watch):
- Adaugă 5 competitori relevanți
- Compară: număr de postări / săptămână, engagement total, creare de conținut
- Identifică ce tipuri de conținut funcționează la competitori

Nu copia — adaptează insights-urile la vocea brandului tău.

### Pas 6: Exportă date pentru raportare lunară
Descarcă raportul complet din Insights → Export Data:
- Selectează "Page Level" și "Post Level" data
- Alege format `.xlsx`
- Exportă pentru intervalul lunar complet

Creează tab sumar în Excel/Google Sheets cu KPIs principale și trend față de luna anterioară.

### Pas 7: Optimizează calendarul de conținut
Bazat pe analiza pașilor 3-5, actualizează calendarul editorial pentru luna viitoare:
- Crește frecvența tipului de conținut cu cel mai bun engagement rate
- Aliniază orele de publicare cu datele de activitate audiență
- Reduce sau elimină formatele cu performanță slabă
- Planifică minimum 1 conținut viral-potential per săptămână (video / concurs / user-generated content)

---

## 3. Cortex Logging

```json
{
  "text": "Facebook Insights Analytics executat: metrici cheie analizate, performanță per tip conținut identificată, ore optime de postare determinate, competitori monitorizați, date exportate, calendar de conținut optimizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-070",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["facebook", "social-media", "insights", "engagement", "continut", "analytics"],
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
1. `✅ [PROC] M-070 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Facebook Insights and Social Media Analytics" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Insights / Business Suite | Sursă date analytics |
| Google Sheets / Excel | Raportare lunară |
| Calendar editorial | Planificare conținut |
| Pages to Watch | Monitorizare competitori |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Raport lunar exportat | Da |
| Calendar actualizat bazat pe date | Da |
| Competitori monitorizați | Minimum 5 |
