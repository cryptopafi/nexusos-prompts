---
type: procedure
created: 2026-03-22
status: active
slug: m-069-marketing-campaign-analytics
tags: [procedure, nexus]
---

# Marketing Campaign Analytics and ROI Measurement — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Măsurare completă a performanței campaniilor de marketing și calculul ROI per canal, de la setup UTM până la revizuire lunară și realocarea bugetului.

---

## 1. Problema

Fără un sistem de tracking al campaniilor, bugetele de marketing sunt alocate pe baza intuiției, nu a datelor. Canale profitabile primesc prea puțin budget, cele neprofitabile consumă resurse nejustificat. Procesul standard asigură vizibilitate completă și decizii bazate pe ROAS real.

Situații acoperite:
- Campanii active pe multiple canale (Google Ads, Meta, Email, SEO)
- Evaluare lunară a performanței campaniilor
- Decizia de alocare buget pentru luna viitoare

---

## 2. Procedura

### Pas 1: Definește KPI-urile campaniei
Înainte de lansare, stabilește metricile de succes:
- **CPA** (Cost per Acquisition) — buget / conversii
- **ROAS** (Return on Ad Spend) — venituri / cheltuieli publicitare
- **CPL** (Cost per Lead) — pentru campanii de generare lead-uri
- **LTV** (Lifetime Value) — pentru evaluare pe termen lung

Documentează target-urile pentru fiecare KPI înainte de lansarea campaniei.

### Pas 2: Configurează parametrii UTM pentru toate sursele de trafic
Adaugă UTM la toate link-urile campaniei cu Google Campaign URL Builder:
- `utm_source` — sursa (google / facebook / email / newsletter)
- `utm_medium` — mediul (cpc / social / email / organic)
- `utm_campaign` — numele campaniei (ex: spring-sale-2026)
- `utm_content` — varianta de ad (opțional, pentru A/B)
- `utm_term` — keyword (pentru Google Ads manual tagging)

Template standard: `?utm_source={source}&utm_medium={medium}&utm_campaign={campaign}`

### Pas 3: Creează dashboard centralizat
Construiește dashboard în Looker Studio (Google Data Studio — gratuit):
- Conectează GA4 ca sursă de date principală
- Adaugă conectori pentru Google Ads, Meta Ads, Search Console
- Include: Sessions / Conversions / Revenue / ROAS per canal
- Setează comparație automată față de perioada precedentă

Dashboard-ul trebuie să fie actualizat în timp real și accesibil echipei.

### Pas 4: Calculează ROI per canal
Formula ROI standard:
`ROI = (Venituri generate - Cost canal) / Cost canal × 100`

Formula ROAS:
`ROAS = Venituri generate / Cost publicitate`

Calculează separat pentru fiecare canal activ: Google Ads, Meta Ads, Email Marketing, SEO (cost intern/agenție).

### Pas 5: Identifică canalele cu cea mai bună performanță
Sortează canalele descrescător după ROAS. Identifică:
- **Top performers** (ROAS ≥ 4x) — scalează bugetul
- **Mid performers** (ROAS 2-4x) — optimizează
- **Under performers** (ROAS sub 2x) — pause sau restructurare

Documentează analiza cu date concrete, nu cu percepții subiective.

### Pas 6: Realocă bugetul bazat pe date ROAS
Aplică regula de realocare:
- Mută 20-30% din bugetul canalelor sub-performante către top performers
- Setează budget cap în platformele de ads la noile valori
- Documentează decizia și justificarea bazată pe date

### Pas 7: Revizuire lunară de performanță
Prima zi a lunii: meeting de revizuire cu raport structurat:
- Performance vs target (KPIs atinse / neatingse)
- Top 3 insights din luna trecută
- Acțiuni pentru luna curentă (cu responsabili și deadline)
- Budget alocat per canal luna viitoare

---

## 3. Cortex Logging

```json
{
  "text": "Marketing Campaign Analytics executat: KPIs definite, UTM parametri configurați, dashboard Looker Studio creat, ROI calculat per canal, top performers identificați, buget realalocat bazat pe ROAS, revizuire lunară completată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-069",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["analytics", "roi", "roas", "utm", "campanii", "budget", "looker-studio"],
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
1. `✅ [PROC] M-069 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Marketing Campaign Analytics and ROI Measurement" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Analytics 4 | Sursă date conversii și sesiuni |
| Looker Studio (Data Studio) | Dashboard centralizat reporting |
| Google Campaign URL Builder | Generator parametri UTM |
| Google Ads / Meta Ads | Platforme advertising cu date cost |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| UTM coverage | 100% linkuri campaniei |
| Dashboard actualizat | Real-time |
| Revizuire lunară | Prima zi a lunii |
