---
id: B2B-004
title: CRM Pipeline Setup and Stage Definitions
domain: b2b-sales
source: AI Cold Emails Lead Generation Sales Masterclass 2024
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# CRM Pipeline Setup and Stage Definitions — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea unui pipeline CRM B2B cu stagii MEDDIC-aligned, criterii de advancement definite, automatizări de follow-up și forecasting bazat pe conversion rates reale. Include comparația CRM-urilor specifice: HubSpot Free vs. Paid, Pipedrive, Close.com.

---

## 1. Problema

CRM-urile neconfigurate sau configurate greșit duc la leads pierdute, follow-up inconsistent și forecasting inexact. Fără stage definitions clare cu criterii MEDDIC de advancement, sales reps mută oportunități subiectiv și datele din pipeline nu reflectă realitatea. Rezultatul: forecasting cu acuratețe sub 50% și deals pierdute prin neglijare.

Situații acoperite:
- Setup CRM nou pentru o echipă de sales B2B / agency (de la zero)
- Refactoring pipeline existent cu stagii neclare sau prea multe
- Standardizarea procesului de sales pentru onboarding reps noi
- Agency selling retainers ($2K-25K/mo) with 2-6 week sales cycle

---

## 2. Procedura

### Pas 1: Alegerea CRM-ului potrivit (comparație realistă)
Selectează CRM-ul bazat pe dimensiunea echipei și buget:
- **HubSpot Free CRM**: Ideal pentru 1-3 persoane, buget $0. Include: contacts, deals, pipeline view, email tracking, meeting scheduler. Limitări: 1 pipeline, fără automatizări, fără sequences, rapoarte limitate. Upgrade la Starter ($20/mo) pentru email sequences. Professional ($500/mo) pentru lead scoring, automatizări, multiple pipelines
- **Pipedrive ($14-49/user/mo)**: Cel mai intuitiv UI pentru sales reps. Excelent pentru vizualizare pipeline. Power plan ($49) include automatizări și email sync. Cel mai bun raport preț/funcționalitate pentru echipe de 3-15
- **Close.com ($59-149/user/mo)**: Built for outbound — calling, emailing, SMS integrat direct în CRM. Cel mai bun pentru high-volume outreach. Power Dialer inclus. Ideal pentru SDR teams
- **Salesforce**: NUMAI pentru echipe >15 reps cu buget enterprise. Overhead de admin enorm pentru echipe mici
- **Recomandare agency**: Start cu HubSpot Free → upgrade la Pipedrive Power ($49/user) când ai 3+ deals active

### Pas 2: Maparea procesului de vânzare actual pe MEDDIC
Înainte de configurare: documentează fluxul actual prin interviuri cu echipa de sales. Mapează pe MEDDIC framework:
- **Metrics**: Ce definește succes pentru prospect? (la ce pas aflii asta?)
- **Economic Buyer**: Cine are puterea de decizie finală? (la ce pas îl identifici?)
- **Decision Criteria**: Ce criterii folosesc în evaluare? (la ce pas le afli?)
- **Decision Process**: Care sunt pașii lor interni de aprobare? (la ce pas afli timeline-ul?)
- **Identify Pain**: Care e durerea reală? (la ce pas o confirmi cu cuvintele lor?)
- **Champion**: Cine te susține intern? (la ce pas îl identifici?)

Creează o diagramă vizuală a procesului actual cu aceste checkpoints MEDDIC suprapuse.

### Pas 3: Definirea stagiilor pipeline (MEDDIC-aligned)
Stagii recomandate pentru B2B agency sales (adaptează la business):

| Stagiu | Definiție | Criterii MEDDIC | Probabilitate |
|--------|-----------|-----------------|---------------|
| 1. Lead/Prospect | Contactat, fără răspuns | - | 5% |
| 2. Engaged | A răspuns pozitiv, interes confirmat | Pain identificat | 10% |
| 3. Discovery | Call de discovery completat | Pain + Metrics + Champion | 25% |
| 4. Qualified | MEDDIC complet | Pain + Metrics + Economic Buyer + Decision Criteria + Decision Process + Champion | 40% |
| 5. Proposal | Ofertă trimisă | Toate + budget confirmat | 60% |
| 6. Negotiation | Discuții active pe ofertă | Toate + feedback pe ofertă | 75% |
| 7. Verbal Commit | Acord verbal, contract în lucru | Toate + verbal yes | 90% |
| 8. Closed Won | Semnat | - | 100% |
| 9. Closed Lost | Pierdut (cu motiv documentat) | - | 0% |

**Regula de aur**: Fiecare stagiu trebuie să aibă un VERB care descrie acțiunea completată (nu un status vag). "Discovery Completed" > "In Discussion".

### Pas 4: Configurarea câmpurilor obligatorii per stagiu
Definește câmpurile gate care BLOCHEAZĂ avansarea dacă nu sunt completate:

| Tranziție | Câmpuri obligatorii |
|-----------|-------------------|
| Lead → Engaged | Sursa lead, canal de contact |
| Engaged → Discovery | Pain point notat, call scheduled |
| Discovery → Qualified | BANT complet: Budget (confirmat/estimat), Authority (decision maker identificat), Need (confirmat cu cuvintele lor), Timeline (deadline sau urgency) |
| Qualified → Proposal | Deal value, propunere de valoare agreată verbal, stakeholders listați |
| Proposal → Negotiation | Propunerea a fost deschisă/citită, feedback primit |
| Negotiation → Verbal Commit | Obiecții rezolvate, verbal agreement pe termeni |

**CRM-specific**: HubSpot: Required Fields per deal stage (Settings → Deals → Pipelines). Pipedrive: Required Fields la deal stage transition. Close.com: Custom required fields per status.

### Pas 5: Custom fields și deal properties esențiale
Câmpuri custom obligatorii (beyond standard):
- **Lead Source** (dropdown): Cold Email, LinkedIn, Referral, Inbound (Website), Inbound (Content), Partner, Event. Critică pentru ROI per channel
- **ICP Fit Score** (1-5): Cât de bine matchează ICP-ul definit
- **Champion Name** (text): Cine este champion-ul intern
- **Economic Buyer** (text): Cine ia decizia finală
- **Competitor Mentioned** (dropdown multi): Ce alternative evaluează
- **Next Action** (text): CE faci NEXT — obligatoriu mereu completat
- **Next Action Date** (date): CÂND — obligatoriu mereu completat
- **Lost Reason** (dropdown — NU text liber): vezi Pas 7
- **Deal Source Channel** (dropdown): care canal a generat prima interacțiune
- **Monthly Retainer Value** (number): pentru agentii — valoarea lunară, nu one-time

**Regula critică**: Un deal fără Next Action + Next Action Date completate este un deal mort. CRM-ul trebuie să alarmeze la deals fără next action.

### Pas 6: Automatizări de follow-up și alerte
Configurează automatizări CRM (HubSpot Workflows, Pipedrive Automations, Close.com Smart Views):
- **Deal stagnant 7 zile** în orice stagiu fără activitate → notificare rep + task automat: "Follow up on [Deal Name]"
- **Deal trece în Proposal** → reminder automat la 3 zile: "Follow up on proposal sent to [Company]"
- **Deal fără activitate 14 zile** → alertă manager + deal marcat "At Risk"
- **Deal fără activitate 21 zile** → escalare automată + email template pre-built pentru breakup
- **Deal Closed Won** → trigger onboarding sequence + notificare delivery team
- **Deal Closed Lost** → trigger feedback survey email (auto, în 48h)
- **Weekly pipeline digest** (luni dimineață): email automat cu summary deals active, la risc, și won/lost din săptămâna precedentă

### Pas 7: Closed Lost reasons (dropdown fix, NU text liber)
Creează lista fixă de motive de pierdere — analiza lunară este imposibilă pe text liber:

| Reason | Definiție |
|--------|-----------|
| No Budget | Budget insuficient sau inexistent |
| Timing Not Right | Interesat dar nu acum (6+ luni) |
| Chose Competitor | A ales alt provider (notează care) |
| No Decision Made | Ghost / au decis să nu facă nimic |
| Not a Fit | Serviciul nu rezolvă problema lor |
| Internal Solution | Au decis să facă intern |
| Champion Left | Champion-ul a plecat din companie |
| Price Too High | Budget existent dar prețul nostru e prea mare |

Analiza closed lost reasons LUNAR este obligatorie. Dacă >30% pierderi sunt "Price Too High": problema e de positioning, nu de preț. Dacă >30% sunt "No Decision Made": problema e de urgency creation în discovery.

### Pas 8: Training echipă, audit și hygiene protocol
- **CRM Playbook**: Documentează un ghid de 2-3 pagini cu: definiția fiecărui stagiu, câmpurile obligatorii, regulile de avansare, procesul de logging activities. Distribuie la onboarding
- **Training session** (30 minute): Walkthrough practic cu echipa pe CRM-ul configurat
- **Weekly pipeline hygiene** (luni, 20 minute): Verifică: deals fără next action, deals cu close date trecut, deals stagnante >21 zile, deals fără deal value. Curăță sau actualizează fiecare
- **Monthly data quality audit**: % deals cu toate câmpurile completate (target ≥90%), % deals cu next action (target ≥95%), % closed lost cu reason completat (target 100%)
- **Quarterly pipeline review**: Recalculează probabilitățile per stagiu bazat pe date reale (nu pe estimările inițiale)

---

## 3. Cortex Logging

```json
{
  "text": "CRM pipeline configurat MEDDIC-aligned: 9 stagii cu criterii clare, câmpuri obligatorii per tranziție, automatizări follow-up + alerte stagnare, closed lost reasons standardizate, echipă traininată, hygiene protocol activ.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "B2B-004",
    "domain": "b2b-sales",
    "rule_id": "META-H-002",
    "tags": ["crm", "pipeline", "b2b-sales", "hubspot", "pipedrive", "close-com", "meddic", "sales-process"],
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
- Stagii definite fără criterii MEDDIC de advancement → violation
- Closed lost reasons ca text liber (nu dropdown fix) → violation
- Deals fără next action date acceptate ca valide → violation
- Weekly hygiene omisă >2 săptămâni consecutiv → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum b2b-sales

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Fiecare stagiu are criteriu MEDDIC de advancement documentat?
- [ ] Automatizări de follow-up configurate și testate?
- [ ] Closed lost reasons = dropdown fix?
- [ ] Weekly hygiene protocol documentat și scheduled?

**VK-uri obligatorii:**
1. `✅ [PROC] B2B-004 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "CRM Pipeline Setup and Stage Definitions" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| HubSpot Free/Starter ($0-20/mo) | CRM entry-level |
| Pipedrive Power ($49/user/mo) | CRM recomandat 3-15 reps |
| Close.com ($59-149/user/mo) | CRM outbound-focused |
| Slack / Teams | Notificări stagnare deals |
| Calendly / SavvyCal | Scheduling integrat cu CRM |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| % deals cu next action completată | ≥95% |
| % câmpuri obligatorii completate | ≥90% |
| Deals stagnate >21 zile | <10% din pipeline |
| Closed lost reasons completate | 100% |
| Weekly hygiene executată | 100% săptămâni |
| Audit lunar complianță CRM executat | 100% |
| Forecasting accuracy (predict vs. actual) | ≥70% |
