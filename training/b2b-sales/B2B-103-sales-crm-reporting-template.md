---
type: procedure
created: 2026-03-22
status: active
slug: b2b-103-sales-crm-reporting-template
tags: [procedure, nexus]
---

# Sales CRM Reporting Template — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Setup și operarea unui sistem complet de sales reporting în HubSpot CRM, de la pipeline dashboards la forecasting, activity tracking, și performance reviews structurate.
**Sursă**: HubSpot Academy — Inbound Sales Certification + Sales Software Certification

---

## 1. Problema

Echipele de sales operează fără vizibilitate pe pipeline, forecast inexact, și fără tracking sistematic al activităților. Managerii iau decizii pe feeling, nu pe date. Rep-ii nu știu unde pierd deals, care etapă de pipeline are cea mai mare „leak rate", și cum se compară cu benchmark-urile. Un CRM reporting system structurat oferă vizibilitate, predictibilitate, și accountability.

Situații acoperite:
- Setup pipeline stages cu probabilities în HubSpot
- Configurare sales dashboards (rep, team, management)
- Activity tracking și quota management
- Deal velocity analysis și bottleneck identification
- Win/loss analysis sistematic
- Forecasting bazat pe pipeline weighted value
- Sales performance reviews bazate pe date

---

## 2. Procedura

### Pas 1: Configure Pipeline Stages — Definește etapele cu probabilități
În HubSpot: Settings > Objects > Deals > Pipelines. Creează pipeline cu etape clare și probabilitate de close: (a) Appointment Scheduled — 20%, (b) Qualified to Buy — 30%, (c) Presentation Scheduled — 40%, (d) Decision Maker Brought In — 60%, (e) Contract Sent — 80%, (f) Closed Won — 100%, (g) Closed Lost — 0%. Adaugă deal properties obligatorii per stage: close date, deal amount, deal source, lost reason (required la Closed Lost). Configurează deal stage automation: email notification la stage change.

### Pas 2: Required Properties — Setează câmpurile obligatorii
Configurează mandatory fields pe deal: (a) **Deal Amount** — required la stage ≥ Qualified to Buy, (b) **Close Date** — required la creare, (c) **Deal Source** — required la creare (Inbound Lead, Outbound Prospecting, Referral, Partner, Event), (d) **Contact Association** — minimum 1 contact asociat, (e) **Company Association** — required, (f) **Next Step** — text field, required at every stage change, (g) **Competitor** — dropdown, required la Presentation Scheduled+. Acest lucru asigură data quality pentru reporting.

### Pas 3: Build Dashboards — Creează 3 nivele de dashboards
**Dashboard 1 — Rep View**: Deals by stage (kanban), Monthly quota vs. actual, Activities today/week (calls, emails, meetings), Upcoming tasks, Win rate last 90 days. **Dashboard 2 — Manager View**: Team pipeline by stage, Forecast weighted vs. unweighted, Rep comparison (deals won, revenue, activities), Deal velocity (average days per stage), Bottleneck analysis (where deals stall). **Dashboard 3 — Executive View**: Revenue closed vs. target, Pipeline coverage ratio (3x rule), Quarter forecast, Year-over-year comparison, Win rate trend, Average deal size trend.

### Pas 4: Activity Tracking — Implementează tracking sistematic
Configurează în HubSpot: (a) Log all calls (duration, outcome: connected/voicemail/no answer), (b) Track all emails (opens, clicks, replies via HubSpot Sales Extension), (c) Log meetings (type: discovery/demo/negotiation, notes required), (d) Tasks assigned and completed tracking. Creează Activity Dashboard: activities by type, activities per rep, activity-to-meeting conversion rate. Setează minimum activity targets: X calls/day, Y emails/day, Z meetings/week.

### Pas 5: Deal Velocity Report — Măsoară viteza pipeline-ului
Deal Velocity = (Number of Deals x Average Deal Size x Win Rate) / Average Sales Cycle Length. Configurează în HubSpot: (a) Raport: average time in each pipeline stage, (b) Identifică bottleneck-uri: dacă deals petrec > 14 zile în „Presentation Scheduled" → problema e urmărirea post-demo, (c) Compare velocity by: source, rep, deal size, industry. Setează benchmark-uri: average sales cycle ≤ 45 zile, no deal stagnant > 21 zile fără next step.

### Pas 6: Win/Loss Analysis — Analizează de ce câștigi și pierzi
Configurează: (a) Lost Reason dropdown obligatoriu: Price, Feature Gap, Competitor Won, No Decision/Status Quo, Budget Cut, Timing, Champion Left, (b) Raport monthly: lost deals by reason (pie chart), lost revenue by reason, (c) Win analysis: ce au în comun deals-urile câștigate? (source, deal size, industry, sales cycle length, activities count), (d) Quarterly deep-dive: selectează 5 won + 5 lost deals, fă post-mortem cu note detaliate. Documentează patterns și actualizează playbook-ul.

### Pas 7: Forecasting — Implementează forecast structurat
3 metode de forecast în paralel: (a) **Weighted Pipeline** — sum(deal amount x stage probability) = forecast, (b) **Rep Forecast** — fiecare rep estimează deals care vor închide luna aceasta (Best Case, Commit, Unlikely), (c) **Historical Run Rate** — average monthly closed revenue x remaining months. Compară cele 3 metode. Creează raport: forecast accuracy (forecast vs. actual) pe ultimele 6 luni. Target: forecast accuracy ≥ 80%.

### Pas 8: Performance Review Template — Structurează review-urile
Monthly 1:1 sales performance review cu date din CRM: (a) **Results** — revenue closed vs. quota, deals won/lost, new pipeline created, (b) **Activities** — calls/emails/meetings vs. targets, activity-to-opportunity conversion, (c) **Pipeline Health** — pipeline coverage ratio (should be ≥ 3x quota), deal aging analysis, (d) **Skills** — win rate by stage (unde pierde cel mai mult?), average deal size trend, (e) **Action Items** — 3 concrete improvement actions for next month. Documentează în CRM notes pe contact record al rep-ului.

### Pas 9: Automated Reports — Configurează raportare automată
HubSpot: Reports > Schedule: (a) **Daily** — rep activity summary (automatic email la 8 AM), (b) **Weekly** — pipeline changes, deals won/lost, forecast update (email luni), (c) **Monthly** — full performance report, win/loss analysis, forecast accuracy (email first business day), (d) **Quarterly** — executive summary, YoY comparison, strategic recommendations. Fiecare raport trebuie să aibă: data, insights (not just numbers), recommended actions.

### Pas 10: Hygiene + Audit — Menține calitatea datelor
Rulează CRM hygiene săptămânal: (a) Deals fără next step > 7 zile → flag, (b) Close dates in the past but deal still open → flag, (c) Deals stagnant in same stage > 21 zile → flag, (d) Contacts without company → flag. Creează saved filter/list pentru fiecare categorie. Review în weekly sales meeting. Target: < 5% deals cu data quality issues.

---

## 3. Cortex Logging

```json
{
  "text": "Sales CRM Reporting executat. Pipeline stages: [X]. Dashboards: [Y]. Forecast accuracy: [Z]%. Win rate: [W]%. Hygiene compliance: [V]%.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "B2B-103",
    "domain": "b2b-sales",
    "rule_id": "META-H-002",
    "tags": ["hubspot", "crm-reporting", "pipeline", "forecasting", "sales-dashboard", "win-loss-analysis"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
La setup inițial CRM reporting + audit săptămânal

### HOW (violation detection)
- Pipeline fără probabilități setate → violation critică
- Dashboard fără cele 3 nivele (rep/manager/exec) → violation
- Deals fără required properties → violation
- Forecasting fără minimum 2 metode → violation
- CRM hygiene audit omis > 14 zile → violation
- Win/loss analysis omis > 30 zile → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- B2B-101 → Inbound Sales Methodology
- B2B-009 → Deal Pipeline Forecasting
- AIM-202 → HubSpot AI Analytics & Reporting

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Pipeline stages cu probabilități configurate (Pasul 1)?
- [ ] Required properties setate pe deals (Pasul 2)?
- [ ] 3 nivele dashboards create (rep/manager/exec) (Pasul 3)?
- [ ] Deal velocity benchmark-uri setate (Pasul 5)?
- [ ] Win/loss analysis cu lost reason obligatoriu (Pasul 6)?
- [ ] CRM hygiene audit rulat (Pasul 10)?

**VK-uri obligatorii:**
1. `✅ [PROC] B2B-103 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Sales CRM Reporting Template" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |
| Validare structură | AUDIT-PRO |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| HubSpot CRM (Pro/Enterprise) | Pipeline, deals, reporting, dashboards |
| HubSpot Sales Hub | Activity tracking, sequences, email integration |
| HubSpot Sales Extension (browser) | Email tracking, meeting scheduling |
| Google Sheets (opțional) | Custom analysis, forecast comparison |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Pipeline data quality | ≥ 95% deals compliant |
| Forecast accuracy | ≥ 80% |
| CRM hygiene (deals flagged) | ≤ 5% |
| Win/loss analysis cadence | Monthly |
| Dashboard usage (views/week) | ≥ 3 per manager |
| Activity tracking compliance | ≥ 90% |
