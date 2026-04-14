---
type: procedure
created: 2026-03-22
status: active
slug: b2b-101-inbound-sales-methodology
tags: [procedure, nexus]
---

# Inbound Sales Methodology (Identify-Connect-Explore-Advise) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implementarea completă a metodologiei HubSpot Inbound Sales cu cele 4 faze (Identify → Connect → Explore → Advise), aliniată cu buyer's journey modern (Awareness → Consideration → Decision).
**Sursă**: HubSpot Academy — Inbound Sales Certification + Inbound Sales Fundamentals

---

## 1. Problema

Salespeople tradiționale (legacy) folosesc aceleași tactici pentru toți prospecții: cold calls generice, prezentări identice, pitch-uri agresive. Buyer-ii moderni au acces la informație și sunt deja la 60-70% din decizie înainte de a vorbi cu un sales rep. Metodologia inbound aliniază procesul de vânzare cu buyer's journey: ajută în loc să vândă, personalizează în loc să generalizeze, și consiliază în loc să preseze.

Situații acoperite:
- Tranziția de la outbound/legacy sales la inbound methodology
- Prioritizarea lead-urilor bazat pe buyer's journey stage
- Personalizarea outreach-ului per prospect context
- Conducerea discovery calls eficiente
- Crearea de prezentări consultative personalizate
- Implementarea framework-ului GPCT (Goals, Plans, Challenges, Timeline)

---

## 2. Procedura

### Pas 1: IDENTIFY — Identifică buyerii activi
Diferența între legacy și inbound: legacy lucrează cu orice lead, inbound prioritizează buyerii care deja au intrat în buyer's journey (au vizitat site-ul, au descărcat conținut, au cerut informații). Setup: (a) configurează lead scoring în CRM (demographic fit + behavioral signals), (b) monitorizează „trigger events" — schimbări de job pe LinkedIn, funding rounds, company news, technology adoption signals, (c) creează Ideal Customer Profile (ICP) cu minimum 5 criterii (industry, company size, revenue, tech stack, pain point), (d) filtrează pipeline-ul: lucrează doar cu leads care au scor ≥ threshold-ul stabilit.

### Pas 2: CONNECT — Conectează-te în contextul buyerului
Nu începe cu pitch-ul tău. Începe cu contextul lor. Framework-ul de Connect: (a) **Research** — 5 minute pre-call: verifică LinkedIn profile, company website, recent content consumed pe site-ul tău, social media activity, (b) **Personalize** — menționează ceva specific din research: „Am observat că ai descărcat ghidul nostru despre [topic]" sau „Am văzut postarea ta despre [challenge]", (c) **Offer Value** — nu cere meeting, oferă insight: „Am un framework care rezolvă exact problema asta — pot să ți-l trimit?" (d) **Multi-channel** — combină email + LinkedIn + eventual telefon, nu te baza pe un singur canal.

### Pas 3: QUALIFY — Aplică framework-ul GPCT+CI
În prima conversație, descoperă: **G**oals (ce obiective are buyerul?), **P**lans (ce plan are deja pentru a atinge obiectivele?), **C**hallenges (ce obstacole întâmpină?), **T**imeline (când trebuie rezolvat?), plus **C**onsequences (ce se întâmplă dacă nu rezolvă?) și **I**mplications (ce câștigă dacă rezolvă?). Documentează răspunsurile în CRM. Dacă GPCT nu se potrivește cu oferta ta → disqualify elegant și oferă o resursă utilă.

### Pas 4: EXPLORE — Explorează nevoile în profunzime
Tranziția de la Identify/Connect la Explore se face când buyerul exprimă interes explicit. Reguli: (a) pune mai multe întrebări decât afirmații (ratio 60/40 listening vs. talking), (b) folosește „tell me more about..." și „help me understand..." nu „let me tell you about...", (c) explorează impact-ul business al problemei (nu doar problema tehnică), (d) identifică decision-making unit (DMU): cine decide, cine influențează, cine blochează, (e) documentează tot în CRM cu note structurate.

### Pas 5: ASSESS FIT — Evaluează potrivirea bidirecțional
Inbound sales = relație, nu tranzacție. Evaluează: (a) poate oferta ta rezolva problema identificată la Explore? (b) are buyerul bugetul necesar? (c) are autoritatea de decizie sau trebuie escalatat? (d) timeline-ul lor se potrivește cu delivery-ul tău? Dacă nu e fit → spune clar „Nu suntem cea mai bună soluție pentru asta, dar recomand [alternativă]." Acest lucru construiește încredere pe termen lung.

### Pas 6: ADVISE — Prezintă soluția personalizat
Diferența critică: legacy salesperson prezintă aceeași demonstrație tuturor. Inbound salesperson creează prezentare custom. Framework: (a) **Recap** — „Din ce am discutat, obiectivul tău e X, provocarea e Y, și timeline-ul e Z. Corect?", (b) **Bridge** — „Iată cum abordăm exact această situație..." (nu generic, ci specific pe problema lor), (c) **Proof** — case study de la un client similar (aceeași industrie, aceeași provocare), (d) **Proposal** — structurat pe GPCT: cum fiecare element al ofertei adresează un Goal/Challenge specific.

### Pas 7: CLOSE — Închide cu tehnica 1-to-10
După prezentare, întreabă: „Pe o scară de la 1 la 10, cât de pregătit te simți să mergi mai departe?" Dacă 8-10 → discută next steps concret (contract, timeline, onboarding). Dacă 6-7 → „Ce ar trebui să se întâmple pentru a ajunge la 10?" și adresează obiecțiile specific. Dacă 1-5 → re-evaluează dacă e fit real sau trebuie retras. Nu forța niciodată o vânzare pe un scor sub 6 — oferă mai multă valoare și revino.

### Pas 8: CRM — Documentează întregul proces
Fiecare interacțiune trebuie documentată în HubSpot CRM: (a) timeline activity log (emails, calls, meetings), (b) deal stage progression (Appointment Scheduled → Qualified → Presentation → Decision Maker Brought In → Contract Sent → Closed Won/Lost), (c) GPCT notes pe deal record, (d) lost deal reason analysis (de ce s-a pierdut?). Rulează raport săptămânal: conversion rate per stage, average deal cycle, win rate.

---

## 3. Cortex Logging

```json
{
  "text": "Inbound Sales Methodology executat. Lead: [company]. Stage progression: [identify→connect→explore→advise]. GPCT completed: [da/nu]. Fit assessment: [fit/no-fit]. Deal outcome: [won/lost/pending]. Close score: [X]/10.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "B2B-101",
    "domain": "b2b-sales",
    "rule_id": "META-H-002",
    "tags": ["hubspot", "inbound-sales", "methodology", "GPCT", "buyer-journey", "identify-connect-explore-advise"],
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
La fiecare interacțiune de sales cu un prospect/lead

### HOW (violation detection)
- Contact fără research prealabil (Pasul 2) → violation
- Discovery call fără GPCT framework → violation critică
- Prezentare non-personalizată (generic deck) → violation
- Deal fără fit assessment documentat → violation
- CRM nedocumentat per interacțiune → violation
- Close attempt fără 1-to-10 technique → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- B2B-005 → Discovery Call Framework
- B2B-007 → Lead Scoring Qualification Matrix

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Lead scoring configurat și ICP definit (Pasul 1)?
- [ ] Research pre-call efectuat (Pasul 2)?
- [ ] GPCT+CI framework completat (Pasul 3)?
- [ ] Fit assessment documentat bidirecțional (Pasul 5)?
- [ ] Prezentare personalizată cu case study (Pasul 6)?
- [ ] CRM documentat cu timeline + GPCT notes (Pasul 8)?

**VK-uri obligatorii:**
1. `✅ [PROC] B2B-101 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Inbound Sales Methodology" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| HubSpot CRM (Free sau Pro) | Pipeline, contacts, deals, activity log |
| LinkedIn Sales Navigator (opțional) | Research prospects, trigger events |
| Email tool (HubSpot / Gmail) | Outreach sequences |
| GPCT Template | Framework discovery call |
| Buyer Persona Documents | ICP definition |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Lead-to-Connect rate | ≥ 25% |
| Connect-to-Explore rate | ≥ 40% |
| Explore-to-Advise rate | ≥ 60% |
| Win rate (Advise to Close) | ≥ 30% |
| Average deal cycle | ≤ 45 zile |
| CRM documentation compliance | 100% |
