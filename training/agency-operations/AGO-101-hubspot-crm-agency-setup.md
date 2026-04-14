---
type: procedure
created: 2026-03-22
status: active
slug: ago-101-hubspot-crm-agency-setup
tags: [procedure, nexus]
---

# HubSpot CRM Agency Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Setup complet HubSpot CRM pentru o agenție de marketing/sales, incluzând multi-client architecture, pipeline management, client reporting, și team permissions — de la zero la operational.
**Sursă**: HubSpot Academy — Inbound Marketing Certification + Inbound Sales Certification + HubSpot Solutions Partner best practices

---

## 1. Problema

Agențiile care implementează HubSpot CRM fără o arhitectură clară sfârșesc cu: date mixate între clienți, lipsa granularității pe client reporting, permisiuni greșite (toți văd totul), pipeline-uri generice care nu reflectă procesul specific fiecărui client, și imposibilitatea de a demonstra ROI per client. Un setup corect din start evită refactoring costisitor și construiește fundația pentru scaling.

Situații acoperite:
- Setup inițial HubSpot CRM pentru agenție (propriul portal)
- Client portal management (dacă clienții au propriile portale)
- Multi-pipeline architecture pentru clienți diferiți
- Team permissions și data partitioning
- Client-facing dashboards și automated reporting
- Onboarding nou client în CRM
- Template-uri reutilizabile (emails, workflows, reports)

---

## 2. Procedura

### Pas 1: Portal Architecture — Decide modelul de portal
Alege între: (a) **Single Portal** — agenția folosește un singur HubSpot portal, cu partitionare pe Teams/Properties pentru fiecare client (mai simplu, mai ieftin, dar limitări pe permisiuni), (b) **Client Portals** — fiecare client are propriul portal HubSpot, agenția are acces ca Partner (mai bine izolat, client owns data, dar management overhead), (c) **Hybrid** — agenția are portal propriu pentru internal CRM + clienți mari au portale proprii. Recomandare: Hybrid. Documentează decizia și rațiunea.

### Pas 2: Configure Properties — Creează proprietăți custom
Properties standard pentru agenție: (a) **Contact level**: Client (dropdown — care client al agenției), Lead Source (Organic/Paid/Referral/Event/Direct), Buyer Persona, Journey Stage, Lead Score, (b) **Company level**: Client Association, Industry, Company Size, Contract Value, (c) **Deal level**: Client, Service Type (SEO/PPC/Content/Social/Web Dev), Monthly Retainer Value, Campaign Association, Contract Start/End Date. Setup property groups: „Agency Internal" (hidden from clients) și „Client Visible."

### Pas 3: Build Pipelines — Creează pipeline-uri per serviciu
Pipeline 1 — **New Business**: Inquiry Received → Discovery Call Scheduled → Proposal Sent → Negotiation → Contract Signed → Onboarding → Active Client. Pipeline 2 — **Client Services**: Campaign Brief → In Production → Review/Approval → Live → Monitoring → Completed → Archived. Pipeline 3 — **Upsell/Renewal**: Renewal Due (90 days) → Renewal Discussion → Proposal Sent → Renewed/Churned. Configurează deal stage probabilities, required fields per stage, și automation (email triggers pe stage change).

### Pas 4: Teams + Permissions — Configurează accesul
Creează Teams în HubSpot: (a) **Account Managers** — acces la toate contacts/deals/reports per client assignment, (b) **Specialists** (SEO, PPC, Content) — acces la deals/tasks, nu la billing/contracts, (c) **Management** — full access, (d) **Client Users** (dacă au access) — view-only pe rapoartele lor. Configurează: team-based access la contacts (fiecare echipă vede doar clienții săi), ownership rules (auto-assign based on client property), restrict delete permissions.

### Pas 5: Email Templates + Sequences — Creează template-uri reutilizabile
Templates pentru agenție: (a) **Client Onboarding Welcome** — introduce echipa, next steps, expectations, (b) **Monthly Report Delivery** — „Iată raportul lunar pentru [Client]...", (c) **Meeting Follow-up** — summary + action items, (d) **Proposal Send** — personalizat cu servicii recomandate, (e) **Renewal Reminder** — 90/60/30 zile înainte de contract end. Sequences: New Lead follow-up (5 emails, 14 zile), Client Re-engagement (3 emails, 21 zile), Post-Churn feedback (2 emails).

### Pas 6: Workflows — Automatizează procesele repetitive
Workflow-uri esențiale: (a) **New Lead Assignment** — lead vine prin formular → assign account manager bazat pe serviciu solicitat → task creat → notification, (b) **Client Onboarding** — contract signed → create tasks checklist (kickoff call, access setup, strategy doc, first deliverable) → assign team → email welcome, (c) **Renewal Pipeline** — contract expiry - 90 zile → create deal în Renewal pipeline → assign AM → email reminder, (d) **Reporting Automation** — primul luni din lună → task „Pregătește raport" pentru fiecare client activ.

### Pas 7: Client Dashboards — Creează rapoarte per client
Dashboard per client: (a) **Traffic Overview** — sessions, sources, top pages (dacă agenția gestionează site-ul), (b) **Lead Generation** — contacts new, MQLs, SQLs, conversion rate, (c) **Campaign Performance** — email metrics, social metrics, ad spend vs. results, (d) **Deal Pipeline** — deals in progress, revenue closed, (e) **ROI Summary** — investment vs. returns. Configurează: shareable dashboard link (view-only) sau automated PDF email monthly. Fiecare dashboard trebuie să aibă annotations cu context.

### Pas 8: Onboard New Client — Checklist de onboarding
Checklist standard per client nou: (a) Create client as Company în CRM + tag cu Agency Client property, (b) Add contact persons (primary contact, billing, decision maker), (c) Create deal în New Business pipeline → move to Active, (d) Assign Team (AM + specialists), (e) Create client-specific dashboard, (f) Setup tracking code pe site-ul clientului (dacă relevant), (g) Import historical data (contacts, deals), (h) Schedule kickoff call, (i) Create shared Google Drive/Notion folder, (j) Add to monthly reporting workflow. Timpul target: ≤ 2 ore per client.

### Pas 9: Internal Agency CRM — Gestionează propriul pipeline
Nu uita de CRM-ul intern al agenției: (a) Track new business pipeline (prospects, proposals, win rate), (b) Monitor client health score: NPS, responsiveness, contract value trend, expansion signals, (c) Churn risk tracking: declining engagement, missed payments, competitor mentions, (d) Revenue dashboard: MRR, client lifetime value, churn rate, expansion revenue. Rulează review săptămânal pe internal pipeline.

### Pas 10: Audit + Optimize — Audit trimestrial al CRM-ului
Checklist trimestrial: (a) Data hygiene — contacts without company, deals cu close dates in past, inactive contacts, (b) Workflow audit — sunt toate workflow-urile active și funcționale? Error logs? (c) Dashboard relevance — sunt dashboards-urile încă utile? Trebuie actualizate? (d) Permission audit — au toți userii accesul corect? Au plecat oameni care încă au acces? (e) Template performance — care template-uri au cele mai bune open/click rates? Actualizează-le pe celelalte. Documentează findings și action items.

---

## 3. Cortex Logging

```json
{
  "text": "HubSpot CRM Agency Setup executat. Portal model: [single/client/hybrid]. Clients onboarded: [X]. Pipelines: [Y]. Workflows active: [Z]. Client dashboards: [W]. Data quality: [V]%.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AGO-101",
    "domain": "agency-operations",
    "rule_id": "META-H-002",
    "tags": ["hubspot", "crm-setup", "agency", "multi-client", "pipeline", "reporting", "onboarding"],
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
La setup inițial CRM agenție + onboarding client + audit trimestrial

### HOW (violation detection)
- Portal fără architecture decision documentată → violation
- Client fără custom properties setate → violation
- Pipeline fără required fields per stage → violation critică
- Client onboarded fără checklist completat → violation
- Client dashboard lipsă → violation
- Permissions neauditate > 90 zile → violation
- Workflows fără error monitoring → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- AGO-003 → Client Onboarding Process
- AGO-007 → Client Reporting Cadence
- B2B-103 → Sales CRM Reporting Template

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Portal architecture decision documentată (Pasul 1)?
- [ ] Custom properties per level create (Pasul 2)?
- [ ] Pipeline-uri per serviciu configurate (Pasul 3)?
- [ ] Teams + permissions setate cu access rules (Pasul 4)?
- [ ] Client onboarding checklist completat (Pasul 8)?
- [ ] Audit trimestrial scheduled (Pasul 10)?

**VK-uri obligatorii:**
1. `✅ [PROC] AGO-101 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "HubSpot CRM Agency Setup" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| HubSpot CRM (Pro minimum, Enterprise recommended) | Core CRM platform |
| HubSpot Marketing Hub | Email, workflows, forms, landing pages |
| HubSpot Sales Hub | Pipeline, sequences, meeting scheduling |
| HubSpot Solutions Partner Account (opțional) | Client portal management |
| Google Drive / Notion | Client documentation |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Client onboarding time | ≤ 2 ore |
| Data quality (deals compliant) | ≥ 95% |
| Client dashboards configured | 100% clienți activi |
| Workflow error rate | ≤ 1% |
| Permission audit cadence | Trimestrial |
| Client reporting automation | ≥ 80% automated |
