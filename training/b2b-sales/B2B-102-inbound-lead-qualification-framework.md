---
type: procedure
created: 2026-03-22
status: active
slug: b2b-102-inbound-lead-qualification-framework
tags: [procedure, nexus]
---

# Inbound Lead Qualification Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implementarea unui framework sistematic de calificare a lead-urilor inbound, de la first touch la sales-ready handoff, folosind BANT + GPCT combinat cu lead scoring automatizat în HubSpot CRM.
**Sursă**: HubSpot Academy — Inbound Sales Certification + Inbound Marketing Certification

---

## 1. Problema

Echipele de sales pierd 50-67% din timp pe lead-uri necalificate. Marketing generează volume mari de lead-uri dar fără calificare structurată, sales primește un mix de tire-kickers, researchers, și genuine buyers. Rezultat: cycle time lung, frustrare bidirecțională marketing↔sales, și conversion rate sub 5%. Un framework de calificare structurat separă signal de noise și focalizează resursele pe lead-urile cu cea mai mare probabilitate de conversie.

Situații acoperite:
- Definirea criteriilor MQL (Marketing Qualified Lead) și SQL (Sales Qualified Lead)
- Implementarea lead scoring (demographic + behavioral)
- Setup automated workflows de calificare în HubSpot
- Handoff protocol marketing → sales
- Disqualification framework (când și cum să spui nu)
- Re-engagement campaigns pentru lead-uri necalificate temporar

---

## 2. Procedura

### Pas 1: Define ICP — Creează Ideal Customer Profile
Documentează profilul ideal pe 3 dimensiuni: (a) **Firmographic** — industrie, company size (employees), revenue range, geografie, tech stack, (b) **Buyer Persona** — rol/title, seniority, departament, pain points top 3, goals top 3, canale preferate de comunicare, (c) **Negative Persona** — cine NU este clientul tău: studenți, competitori, companii prea mici/mari, industrii incompatibile. Creează proprietăți custom în HubSpot pentru fiecare criteriu ICP.

### Pas 2: Build Scoring Model — Construiește modelul de lead scoring
Creează un scoring model pe 2 axe: **Fit Score** (cât de bine se potrivește cu ICP) și **Engagement Score** (cât de activ interacționează). Fit criteria (demographic): Industry match +20, Company size match +15, Title/Seniority match +15, Geography match +10. Engagement criteria (behavioral): Website visit +5, Blog read +3, Content download +15, Pricing page visit +25, Demo request +50, Email open +2, Email click +5, Webinar attendance +20, Return visit (within 7 days) +10. Negative scoring: Competitor domain -50, Student email -30, Unsubscribe -20, No engagement 30 days -15.

### Pas 3: Define Lifecycle Stages — Configurează etapele în CRM
Setup în HubSpot Lifecycle Stage progression: (a) **Subscriber** — s-a abonat la newsletter/blog (Engagement Score 1-20), (b) **Lead** — a descărcat conținut gated (Score 21-40), (c) **MQL** — a atins threshold-ul de calificare marketing (Score 41-70, Fit Score minim 30), (d) **SQL** — sales a validat manual și confirmat interes real (Score 71+), (e) **Opportunity** — deal creat în pipeline, (f) **Customer** — closed won. Configurează workflow automat: contact atinge MQL threshold → lifecycle stage update + notificare sales rep.

### Pas 4: Setup Qualification Workflow — Automatizează calificarea
În HubSpot Workflows: (a) **MQL Trigger** — when contact score ≥ 41 AND fit score ≥ 30 → set lifecycle = MQL → assign owner → create task „Review and qualify within 4 hours" → send internal notification, (b) **SQL Promotion** — sales rep marks contact as SQL → create deal → move to pipeline stage 1 → log activity, (c) **Recycle Flow** — if sales disqualifies → set lifecycle back to Lead → enroll in nurturing workflow → re-evaluate at 30/60/90 days, (d) **Dead Lead** — no engagement for 90 days → set lifecycle = Other → exclude from active campaigns.

### Pas 5: BANT+GPCT Qualification Call — Execută calificarea live
Când MQL devine assignment: (a) Review all HubSpot intelligence (pages viewed, content downloaded, email engagement, source), (b) Outreach cu context: „Am observat că ai explorat [topic] pe site-ul nostru...", (c) Pe call, folosește dual framework: BANT rapid (**B**udget: au fonduri? **A**uthority: este decision maker? **N**eed: au nevoie reală? **T**imeline: au urgență?) + GPCT profund (**G**oals, **P**lans, **C**hallenges, **T**imeline). Documentează în HubSpot notes cu format structurat.

### Pas 6: Scoring Calibration — Calibrează modelul lunar
Rulează analiza retroactivă: (a) Din MQLs-urile ultimei luni, câte au devenit SQL? Ce scor aveau? (b) Din SQL-uri, câte au convertit? (c) Există lead-uri cu scor mare care nu convertesc? (scor inflat) (d) Există lead-uri cu scor mic care au convertit? (criteriu lipsă). Ajustează weights-urile scoring model-ului bazat pe date reale. Target: MQL-to-SQL conversion ≥ 30%, SQL-to-Opportunity ≥ 50%.

### Pas 7: SLA Marketing-Sales — Definește Service Level Agreement
Documentează SLA bidirecțional: **Marketing promite**: minimum X MQLs/lună, lead data complete (email, company, title), lead scoring transparent. **Sales promite**: contact MQL în max 4 ore de la assignment, feedback pe fiecare MQL (accepted/rejected + reason), update deal stage weekly. Setup raport automat: SLA compliance dashboard în HubSpot, review săptămânal în meeting marketing-sales.

### Pas 8: Nurture Non-Ready — Re-engagement pentru lead-uri necalificate
Lead-urile care nu trec de calificare nu sunt pierdute — sunt „not yet ready." Configurează nurturing workflows: (a) **Educational track** — send conținut relevant buyer's journey stage-ului lor la 7/14/21 zile, (b) **Re-engagement trigger** — if lead revisits site after 30+ days dormant → re-score și re-evaluate, (c) **Seasonal campaign** — re-engage dormant leads cu oferte/conținut nou trimestrial. Monitorizează re-activation rate.

### Pas 9: Report + Optimize — Dashboard și optimizare continuă
Creează dashboard de calificare: (a) MQL volume by source, (b) MQL-to-SQL conversion rate, (c) Time-to-contact (SLA compliance), (d) SQL-to-Opportunity rate, (e) Disqualification reasons breakdown, (f) Scoring model accuracy. Prezintă metrics în meeting-ul săptămânal marketing-sales. Optimizează: dacă o sursă generează MQLs care nu convertesc → reduce investment; dacă un content piece generează SQL-uri → double down.

---

## 3. Cortex Logging

```json
{
  "text": "Lead Qualification Framework executat. MQLs generated: [X]. SQL conversion rate: [Y]%. SLA compliance: [Z]%. Scoring model accuracy: [W]%. Top source: [source].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "B2B-102",
    "domain": "b2b-sales",
    "rule_id": "META-H-002",
    "tags": ["hubspot", "lead-qualification", "BANT", "GPCT", "lead-scoring", "MQL", "SQL", "inbound-sales"],
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
La setup inițial lead qualification + calibrare lunară

### HOW (violation detection)
- ICP nedefinit sau incomplet → violation critică
- Lead scoring fără negative signals → violation
- MQL threshold fără fit score minim → violation
- SLA marketing-sales nesettat → violation
- Scoring model necalibrat > 60 zile → violation
- MQL contact time > 24 ore → violation critică
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- B2B-101 → Inbound Sales Methodology
- B2B-007 → Lead Scoring Qualification Matrix
- AIM-202 → HubSpot AI Analytics & Reporting

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] ICP definit pe 3 dimensiuni (Pasul 1)?
- [ ] Scoring model cu Fit + Engagement axe (Pasul 2)?
- [ ] Lifecycle stages configurate cu threshold-uri (Pasul 3)?
- [ ] BANT+GPCT qualification call executat (Pasul 5)?
- [ ] SLA marketing-sales documentat bidirecțional (Pasul 7)?
- [ ] Scoring model calibrat pe date reale (Pasul 6)?

**VK-uri obligatorii:**
1. `✅ [PROC] B2B-102 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Inbound Lead Qualification Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| HubSpot CRM (Pro/Enterprise) | Lead scoring, workflows, lifecycle stages |
| HubSpot Marketing Hub | Forms, landing pages, email nurturing |
| ICP Document | Referință calificare |
| BANT+GPCT Template | Framework qualification call |
| Marketing-Sales SLA Document | Referință compliance |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| MQL-to-SQL conversion rate | ≥ 30% |
| SQL-to-Opportunity conversion | ≥ 50% |
| Time-to-contact MQL | ≤ 4 ore |
| Scoring model calibration frequency | Monthly |
| Disqualification with documented reason | 100% |
| Lead nurture re-activation rate | ≥ 10% |
