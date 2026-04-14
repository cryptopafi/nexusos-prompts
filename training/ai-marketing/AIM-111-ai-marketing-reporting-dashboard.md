---
type: procedure
created: 2026-03-20
status: active
slug: aim-111-ai-marketing-reporting-dashboard
tags: [procedure, nexus]
---

# AI Marketing Reporting and Dashboard Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Construirea și automatizarea raportării de marketing cu AI — de la definirea KPI hierarchy și conectarea surselor de date la construirea dashboards live, generarea interpretării automate AI, și transformarea datelor în decizii cu SLA-uri. Include template-uri per tip de dashboard, automation cu Make.com/Looker Studio, AI interpretation prompts, și client reporting framework pentru agenții.

---

## 1. Problema

Raportarea de marketing manuală consumă 4-8 ore pe săptămână: login pe 5-8 platforme, export CSV-uri, copy-paste în PowerPoint, calcule manuale, și scriere de „insights" care de fapt sunt observații descriptive fără recomandări. Rapoartele ajung la management cu 3-7 zile întârziere, sunt retrospective (ce s-a întâmplat) și nu predictive (ce ar trebui să facem). AI + automation transformă reporting-ul din corvoadă administrativă în sursă de decizii rapide: date fresh (<24h lag), interpretare inteligentă, și recomandări acționabile generate automat.

Situații acoperite:
- Construirea unui dashboard de marketing de la zero
- Automatizarea raportului săptămânal / lunar pentru management
- Client reporting pentru agenție (multiple conturi)
- Integrarea datelor din Meta, Google, email, SEO, social
- Interpretarea anomaliilor și generarea de recomandări cu AI
- Executive dashboard (CMO/CEO level — 5-10 metrici max)

---

## 2. Procedura

### Pas 1: KPI Architecture — Definește metricile pe 3 niveluri ierarhice

**Principiul**: Un dashboard cu 50 metrici nu este folosit. Un dashboard cu 10-15 metrici este citit zilnic.

**Nivel 1 — North Star (2-3 metrici, CEO/CMO level):**
| Metrică | Ce arată | Target |
|---------|----------|--------|
| Revenue atribuit marketing | Contribuția marketing la revenue | $X/mo |
| Cost per Acquisition (CPA) | Eficiența globală de achiziție | ≤ $X |
| Marketing ROI | Revenue / Marketing Spend | ≥ X:1 |

**Nivel 2 — Channel KPIs (2-3 per canal, Marketing Manager level):**
| Canal | KPI 1 | KPI 2 | KPI 3 |
|-------|-------|-------|-------|
| Paid (Meta/Google) | ROAS | CPC | Conversions |
| Organic SEO | Organic traffic | Avg position (top 10 KWs) | Organic conversions |
| Email | Open rate | CTR | Revenue per email |
| Social | Engagement rate | Follower growth | Referral traffic |
| Content | Pageviews per article | Avg time on page | Lead conversions |

**Nivel 3 — Operational (execution team, daily monitoring):**
| Metrică | Ce arată |
|---------|----------|
| Ads active | Nr. de creatives live |
| Posts published | Volumul de output |
| Emails sent | Nr. și frecvența |
| Rankings in top 10 | SEO progress |
| Budget utilization | % din buget cheltuit |

**Template de definire per metrică:**
```
Metrică: [Nume]
Definiție: [ce măsoară exact, formula dacă e calculated]
Sursă: [platforma de unde vine data — GA4, Meta, Klaviyo]
Frecvență refresh: [real-time / zilnic / săptămânal]
Target: [valoarea goal]
Benchmark industrie: [dacă disponibil]
Owner: [cine e responsabil de această metrică]
Alert threshold: [la ce valoare se declanșează alertă]
```

### Pas 2: Data Sources — Conectează sursele cu connectors

**Inventar surse standard de marketing:**

| Sursă | Date cheie | Connector recomandat |
|-------|-----------|---------------------|
| Google Analytics 4 | Traffic, conversii, attribution | Nativ Looker Studio |
| Google Ads | Spend, clicks, conversions, ROAS | Nativ Looker Studio |
| Meta Ads Manager | Spend, reach, clicks, conversions | Supermetrics / Funnel.io |
| TikTok Ads | Spend, impressions, conversions | Supermetrics / manual export |
| Google Search Console | Impressions, clicks, position, CTR | Nativ Looker Studio |
| Klaviyo / Mailchimp | Open rate, CTR, revenue, subscribers | Supermetrics / Zapier |
| Instagram Insights | Reach, engagement, followers | Supermetrics / Iconosquare |
| LinkedIn Analytics | Impressions, engagement, followers | Manual export / Supermetrics |
| HubSpot / Pipedrive | Leads, pipeline, deal value | Nativ sau Supermetrics |
| Stripe / Shopify | Revenue, transactions, AOV | Nativ Looker Studio / API |

**Connector tools:**
- **Supermetrics** ($39/mo) — cel mai popular, 100+ integrări, extrage în Google Sheets sau direct în Looker Studio
- **Funnel.io** ($359/mo) — enterprise, data warehouse first, perfect pentru agenții mari
- **Fivetran** ($1/credit) — ETL tool, cel mai robust dar mai complex
- **Stitch Data** (free plan) — open-source ETL alternativă
- **Manual export** (free) — CSV download săptămânal din fiecare platformă → Google Sheet

**Recomandare per buget:**
- **$0/mo**: Looker Studio nativ (GA4 + Google Ads + Search Console) + manual export restul
- **$39/mo**: Looker Studio + Supermetrics (adaugă Meta, email, social)
- **$100+/mo**: Databox sau proprietary dashboard cu toate integrările native

### Pas 3: Structure — Organizează dashboard pe secțiuni logice

**Layout recomandat (Looker Studio):**

```
┌─────────────────────────────────────────────┐
│  EXECUTIVE SUMMARY                          │
│  [Date range picker] [Compare: prev period] │
│                                             │
│  Revenue: $XX,XXX (↑12%)  CPA: $XX (↓8%)   │
│  ROAS: X.X (↑5%)  Leads: XXX (↑15%)        │
│  Marketing Spend: $X,XXX  Budget: 78%       │
├──────────────────┬──────────────────────────┤
│  PAID PERFORMANCE│  ORGANIC PERFORMANCE     │
│  Spend: $X,XXX   │  Organic traffic: XX,XXX │
│  ROAS: X.X       │  Top keyword: [KW] #X    │
│  Top campaign:   │  New pages ranked: X      │
│  [chart: spend   │  [chart: organic trend    │
│   vs conversions]│   over time]             │
├──────────────────┼──────────────────────────┤
│  EMAIL PERF      │  SOCIAL PERFORMANCE      │
│  Open rate: XX%  │  Engagement: X.X%        │
│  CTR: X.X%       │  Follower growth: +XXX   │
│  Revenue: $X,XXX │  Top post: [preview]     │
│  Unsubs: X.X%    │  Referral traffic: XXX   │
├──────────────────┴──────────────────────────┤
│  CAMPAIGN BREAKDOWN (table)                 │
│  Campaign | Spend | Impressions | Clicks |  │
│           | Conv  | CPA    | ROAS           │
│  [sortable, filterable]                     │
├─────────────────────────────────────────────┤
│  AI INSIGHTS & RECOMMENDATIONS              │
│  [Generated weekly — Pasul 6]               │
│  1. [Insight + action]                      │
│  2. [Insight + action]                      │
│  3. [Insight + action]                      │
└─────────────────────────────────────────────┘
```

**Design principles:**
- **Left-to-right**: macro → micro (summary left, details right)
- **Top-to-bottom**: most important → least important
- **Color coding**: Verde = above target, Roșu = below target, Gri = neutral
- **Comparison always visible**: vs. previous period (week/month/year)
- **No vanity metrics**: dacă metrica nu influențează decizii, nu o include

### Pas 4: Build — Implementare în Looker Studio (pas cu pas)

**Looker Studio (free, cel mai bun raport calitate/preț):**

1. **Create report**: lookerstudio.google.com → Create → Report
2. **Add data sources**:
   - Google Analytics 4: Add data → Google Analytics → select property
   - Google Ads: Add data → Google Ads → select account
   - Search Console: Add data → Search Console → select property
   - Google Sheets (pentru date Meta/email/social via Supermetrics): Add data → Google Sheets

3. **Build scorecards** (North Star metrics):
   - Insert → Scorecard → select metric → add comparison date range
   - Conditional formatting: verde dacă ≥target, roșu dacă <target

4. **Build charts**:
   - Time series: trafic, spend, conversii over time
   - Bar chart: campaign comparison
   - Pie chart: traffic sources mix
   - Table: campaign breakdown cu toate metricile

5. **Add filters**:
   - Date range control (global, afectează toate vizualizările)
   - Campaign filter (dropdown)
   - Channel filter (dacă applicable)

6. **Design & format**:
   - Brand colors aplicat consistent
   - Clear labels pe fiecare vizualizare
   - White space suficient (nu overload)
   - Logo + report title la top

**Alternatives:**
- **Databox** (free 3 sources / $59/mo): Mobile-first dashboards, alerts native, goal tracking. Perfect pentru agenții care vor mobile access.
- **Klipfolio** ($49/mo): Custom dashboards, 100+ integrări, TV mode.
- **Geckoboard** ($39/mo): Simple, beautiful, TV dashboards. Best for office display.
- **Google Sheets + Manual** ($0): Cel mai simplu. Tab per canal, formule, conditional formatting.

### Pas 5: Automate — Raportare automată (zero manual effort)

**Looker Studio automated delivery:**
1. Share → Schedule email delivery
2. Recipients: [email addresses stakeholders]
3. Frequency: Weekly (vineri 8:00) sau Monthly (1st of month)
4. Format: PDF attachment sau link
5. Subject line: „Marketing Report — Week [X] — [Date range]"

**Make.com Advanced Automation:**
```
Scenario: Weekly Marketing Report with AI Insights

Trigger: Schedule (every Friday, 7:00 AM)

→ Module 1: Google Sheets → Read „Weekly KPI Data" (auto-populated by Supermetrics)
→ Module 2: Math → Calculate WoW changes (%, absolute)
→ Module 3: HTTP → Claude API → Generate AI interpretation (Pasul 6 prompt)
→ Module 4: Google Docs → Create report from template + AI insights
→ Module 5: Google Drive → Save in „Marketing Reports" folder
→ Module 6: Slack → Post summary + doc link in #marketing channel
→ Module 7: Gmail → Send to stakeholders with PDF attachment
→ Module 8: (Optional) IF any KPI below alert threshold
  → Slack → Urgent alert in #marketing-alerts

Cost: $9/mo Make.com + $1/mo Claude API = $10/mo
Time saved: 4-6 hours/week → 15 min/week (review only)
```

**Supermetrics scheduled refresh:**
- Supermetrics → Schedule → daily/weekly refresh of all queries in Google Sheets
- Data always fresh when Make.com runs Friday morning

### Pas 6: Interpret — AI interpretation automată (killer feature)

**Prompt template pentru interpretare săptămânală (Claude Sonnet):**
```
Ești un Senior Marketing Analyst. Analizează performanța marketing din [perioadă]:

NORTH STAR METRICS:
| Metric | This week | Last week | Change | Target |
| Revenue | $XX,XXX | $XX,XXX | +X% | $XX,XXX |
| CPA | $XX | $XX | -X% | $XX |
| ROAS | X.X | X.X | +X% | X.X |

CHANNEL BREAKDOWN:
| Channel | Spend | Conversions | CPA | ROAS |
| Meta Ads | $X,XXX | XX | $XX | X.X |
| Google Ads | $X,XXX | XX | $XX | X.X |
| Email | $XX | XX | $XX | X.X |
| Organic | $0 | XX | $0 | ∞ |

NOTABLE DATA POINTS:
- Top performing campaign: [name] — ROAS [X.X]
- Worst performing campaign: [name] — ROAS [X.X]
- Email open rate: [X%] (vs benchmark [Y%])
- Organic traffic: [X] visits (vs last week [Y])
- Top SEO keyword: [KW] position [X] (was [Y])

ANALIZĂ (format: executive-ready, actionable):

1. PERFORMANCE SUMMARY (3-4 propoziții):
   Ce s-a întâmplat global? Suntem on-track vs. targets?

2. WHAT WORKED (top 2-3):
   Ce a performat peste expectation? De ce? Ce putem scala?

3. WHAT DIDN'T WORK (top 2-3):
   Ce a fost sub benchmark? Care sunt cauzele probabile?
   (nu „CTR-ul e mic" ci „CTR-ul e mic probabil pentru că hook-ul nu se aliniază cu audiența cold")

4. ANOMALIES:
   Orice schimbare >20% vs. perioadă anterioară care nu are explicație evidentă

5. RECOMMENDATIONS (top 3, prioritizate):
   Format: [Acțiune specifică] — [Impact estimat] — [Effort: low/medium/high]
   Ex: „Scala campania [X] cu +30% budget" — „Estimat +$X,XXX revenue" — „Low effort"

6. NEXT WEEK FOCUS:
   Ce 1-2 lucruri ar trebui să fie prioritatea #1 a echipei?

NU include: observații descriptive fără acțiune („CTR-ul este 2.5%"),
jargon fără explicație, recomandări vagi („improve performance").
```

**Prompt template pentru interpretare lunară (mai strategic):**
```
Ești CMO fractional. Analizează performanța marketing din [luna]:

[Same data structure but monthly + MoM + YoY comparisons]

Analiză suplimentară față de weekly:

7. TREND ANALYSIS (ultimele 3 luni):
   Ce metrici sunt în trend ascendent/descendent consistent?
   Ce inflection points observi?

8. BUDGET REALLOCATION RECOMMENDATION:
   Bazat pe ROAS per canal, cum ar trebui realocat bugetul luna viitoare?
   Format tabel: Canal | Budget curent | Budget recomandat | Motivul

9. QUARTERLY FORECAST:
   Dacă tendințele continuă, unde vom fi la finalul trimestrului?
   Suntem pe drumul spre atingerea obiectivelor?

10. STRATEGIC CONCERNS:
    Ce risc strategic vedem? (dependency pe un canal, declining organic, rising CPA)
```

### Pas 7: Alert System — Anomaly detection cu threshold alerts

**Configurare alerte pe threshold-uri:**

| Metrică | Alert threshold | Tip alert | Canal |
|---------|----------------|-----------|-------|
| ROAS | < 1.5x (sub breakeven) | CRITICAL | Slack + Email |
| CPA | > 2x target | CRITICAL | Slack + Email |
| Ad spend | > 110% daily budget | HIGH | Slack |
| Email unsubscribe rate | > 0.5% | HIGH | Email |
| Organic traffic | Drop > 20% WoW | HIGH | Slack |
| Website downtime | Any | CRITICAL | Slack + SMS |
| CTR | < 50% benchmark | MEDIUM | Slack (weekly digest) |

**Make.com alert workflow:**
```
Scenario: KPI Alert Monitor (runs every 6 hours)

Trigger: Schedule (every 6 hours)
→ Module 1: Google Sheets → Read current KPIs
→ Module 2: Router →
  Path A: IF ROAS < 1.5 → Slack critical alert + Email to manager
  Path B: IF CPA > 2x target → Slack critical alert
  Path C: IF organic traffic drop > 20% → Slack high alert
  Path D: IF all normal → no action
```

### Pas 8: Client Reporting — Dashboard și rapoarte pentru clienți de agenție

**Client dashboard setup (per client):**
1. **Looker Studio separate per client** (nu shared) — white-label cu logo client
2. **Client-specific KPIs** aliniate cu obiectivele lor (nu metrici generice)
3. **Access**: view-only link, scheduled email PDF, sau embed pe client portal
4. **Data freshness**: automated daily refresh via Supermetrics

**Monthly client report template:**
```markdown
# [Client Name] — Marketing Performance Report — [Month] 2026

## Executive Summary
[3-5 propoziții: ce am făcut, ce rezultate, ce urmează]

## KPI Dashboard
| Metric | This Month | Last Month | Change | Target | Status |
|--------|-----------|------------|--------|--------|--------|
| [KPI 1] | ... | ... | +X% | ... | ✅/❌ |

## Campaign Performance
[Breakdown per campanie cu spend, results, learnings]

## Content Performance
[Top performing content, organic traffic growth, SEO progress]

## Key Learnings & Insights
1. [AI-generated insight + what it means for client]
2. [AI-generated insight]
3. [AI-generated insight]

## Next Month Plan
1. [Action 1] — [Expected impact]
2. [Action 2] — [Expected impact]
3. [Action 3] — [Expected impact]

## Budget Utilization
| Channel | Budget | Spent | Remaining | Recommendation |
|---------|--------|-------|-----------|----------------|
```

**Pricing agency reporting:**
- Include în retainer: basic monthly report (no extra charge)
- Premium: weekly reports + live dashboard + alerts = $200-500/mo add-on
- Tools cost per client: $39/mo Supermetrics + $0 Looker Studio = $39/mo

### Pas 9: Act — Transform raportul în decizii cu SLA-uri

**Raportul fără decizie este cost, nu investiție.**

| Data signal | Severity | SLA acțiune | Acțiune tip |
|------------|----------|-------------|-------------|
| ROAS drop >30% | CRITICAL | 24 ore | Pause underperforming ads, investigate |
| Negative trend 3 weeks | HIGH | Current week | Strategy adjustment |
| Performance below target | HIGH | 48 ore | A/B test launch or budget reallocation |
| Anomaly detected | MEDIUM | 72 ore | Investigate root cause |
| Optimization opportunity | LOW | Next sprint | Add to backlog |

**Decision log (accountability):**
```
| Date | Signal | Decision | Owner | Outcome (30 days) |
|------|--------|----------|-------|-------------------|
| 03/20 | Meta ROAS < 1.5 | Paused 3 campaigns, launched 5 new creatives | [Name] | ROAS recovered to 2.8 |
| 03/18 | Email CTR declining | A/B tested new format, personalization added | [Name] | CTR: 2.1% → 4.3% |
```

### Pas 10: ROI of Reporting — Calculează valoarea sistemului

```markdown
# Reporting System ROI Calculator

## Time savings:
| Activity | Manual | Automated | Savings |
|----------|--------|-----------|---------|
| Data collection (5+ platforms) | 3h/week | 0h (auto) | 3h |
| Report creation | 2h/week | 15min (review) | 1.75h |
| Distribution | 30min | 0 (auto) | 30min |
| AI interpretation | 1h (manual analysis) | 5min (auto) | 55min |
| TOTAL | 6.5h/week | 20min/week | 6h 10min |

## Annual savings:
- Time: 6h × 50 weeks = 300 hours/year
- Cost: 300h × $50/h = $15,000/year
- Tool cost: ($39 Supermetrics + $9 Make + $20 Claude) × 12 = $816/year
- NET SAVINGS: $14,184/year

## Decision quality improvement:
- Faster decisions: 3-7 day lag → same day
- Better decisions: AI catches patterns humans miss
- Accountability: decision log tracks outcomes
- Estimated revenue impact: 5-15% improvement in marketing ROI
```

---

## 3. Cortex Logging

```json
{
  "text": "Marketing Reporting Dashboard configurat/executat. Tool: [Looker Studio/Databox/etc.]. Surse conectate: [X]. KPIs tracked: [Y]. Raportare automată: [zilnic/săptămânal/lunar]. AI interpretation: [activ/manual]. Alerts configurate: [X threshold alerts].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-111",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["reporting", "dashboard", "analytics", "kpi", "automation", "looker-studio", "ai-interpretation", "alerts"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + setup inițial reporting și revizuire lunară

### WHEN
La orice setup nou de reporting sau revizuire periodică a dashboard-ului

### HOW (violation detection)
- Dashboard cu >20 metrici fără structurare pe niveluri → violation
- Raport distribuit fără AI interpretation sau analiză umană → violation
- Date neactualizate (>48h lag) în dashboard live → violation
- Raportare fără secțiunea de recomandări acționabile → violation
- Nicio alertă configurată pe metrici critice → violation
- Decizii bazate pe raport neloggate → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-110 → competitor analysis (competitor data în dashboard)
- AIM-107 → email metrics (sursă dashboard)
- AIM-108 → SEO metrics (sursă dashboard)
- AIM-106 → paid metrics (sursă dashboard)

### VERIFY
- [ ] KPIs definite pe 3 niveluri (North Star / Channel / Operational)?
- [ ] Toate sursele conectate și testate?
- [ ] Dashboard structurat pe secțiuni logice?
- [ ] Comparație vs. perioadă anterioară vizibilă?
- [ ] Raportare automată configurată (schedule + distribution)?
- [ ] AI interpretation prompt template-izat?
- [ ] Alert system cu threshold-uri setate?
- [ ] SLA pentru acțiuni definit?
- [ ] Decision log activ?

**VK-uri:**
1. `✅ [PROC] AIM-111 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Marketing Reporting and Dashboard Setup" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| AI interpretation weekly | Claude Sonnet | Structured analysis, pattern recognition |
| AI interpretation monthly (strategic) | Claude Opus | Strategic thinking, forecasting |
| Anomaly analysis | Claude Sonnet | Root cause analysis |
| Quick data questions | ChatGPT GPT-4o | Rapid answers, calculations |
| Report template generation | DeepSeek | Cost-efficient, structured output |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Looker Studio (free) / Databox ($59/mo) | Dashboard și vizualizare |
| Supermetrics ($39/mo) | Data connectors universal |
| Google Analytics 4 (free) | Web + conversion tracking |
| Google Search Console (free) | SEO monitoring |
| Claude Sonnet / ChatGPT | AI interpretation layer |
| Make.com ($9/mo) | Automation pipeline |
| Meta/Google/TikTok Ads | Surse date paid |
| Klaviyo / Mailchimp | Surse date email |

---

## 6. Instrumente

- Looker Studio (free) — dashboarding nativ Google, cel mai bun raport calitate/preț
- Supermetrics ($39/mo) — data connectors (Meta, email, social → Sheets/Looker)
- Databox (free/$59/mo) — mobile dashboards, goal tracking, agenție-friendly
- Claude Pro ($20/mo) — AI interpretation, anomaly analysis
- ChatGPT Plus ($20/mo) — quick data analysis, calculations
- Make.com ($9/mo) — automation: data refresh + AI analysis + distribution
- Google Sheets (free) — data intermediar, simple reports
- Geckoboard ($39/mo) — TV display dashboards

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Timp construire dashboard inițial | ≤ 4 ore |
| Data refresh lag | ≤ 24 ore |
| Timp generare raport săptămânal | ≤ 20 min (automated) |
| AI interpretation inclusă | 100% reports |
| Decizii luate per raport | ≥ 1 |
| Alert response time (critical) | ≤ 24 ore |
| Decision log entries/month | ≥ 4 |
| Client satisfaction cu reporting (agenție) | ≥ 4.5/5 |
