---
type: procedure
created: 2026-03-20
status: active
slug: aim-101-ai-marketing-tool-selection-matrix
tags: [procedure, nexus]
---

# AI Marketing Tool Selection Matrix — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea și selecția sistematică a instrumentelor AI de marketing dintr-un ecosistem de 100+ opțiuni, aliniind alegerea la obiective de business, buget și maturitate operațională. Include framework de scoring ponderat, cost-benefit analysis, și proces de procurement pentru context de agenție.

---

## 1. Problema

Piața AI marketing tools crește cu sute de produse noi anual. Fără o metodologie de selecție, organizațiile adoptă tools haotic (tool sprawl), plătesc pentru funcționalități duplicate și nu maximizează ROI-ul. Echipele de marketing medii folosesc 12-15 tools, din care 40% au overlap funcțional și 25% sunt subutilizate. Agențiile multiplicau problema per fiecare client onboardat. Această procedură oferă un framework de evaluare structurat care reduce timpul de selecție de la săptămâni la ore și previne tool sprawl-ul din prima iterație.

Situații acoperite:
- Construirea unui stack AI marketing de la zero (startup sau departament nou)
- Auditarea și optimizarea unui stack existent pentru reducerea costurilor
- Evaluarea unui tool specific pentru o funcție de marketing definită
- Prioritizarea investițiilor AI în constrângeri de buget (bootstrapped vs. funded)
- Selecție tool ca serviciu de consultanță pentru clienți de agenție

---

## 2. Procedura

### Pas 1: Map — Cartografiază nevoile de marketing pe categorii funcționale

Listează toate funcțiile de marketing active în organizație. Pentru fiecare funcție, completează un mini-audit de 4 întrebări:

| Funcție | Cine execută | Frecvență | Ore/săpt. | Satisfacție (1-5) |
|---------|-------------|-----------|-----------|-------------------|
| Content creation | [Nume/Rol] | Zilnic | 15 | 2 |
| SEO optimization | ... | Săptămânal | 5 | 3 |
| Paid social ads | ... | ... | ... | ... |
| Email marketing | ... | ... | ... | ... |
| Social media mgmt | ... | ... | ... | ... |
| Analytics/reporting | ... | ... | ... | ... |
| Design/vizual | ... | ... | ... | ... |
| Video production | ... | ... | ... | ... |
| Competitor intel | ... | ... | ... | ... |
| CRM/automation | ... | ... | ... | ... |

Funcțiile cu satisfacție ≤3 și ore/săpt. ≥5 devin candidați prioritari pentru AI tooling. Sortează descrescător pe (ore × inversul satisfacției) pentru a obține ordinea de prioritizare.

### Pas 2: Categorize — Clasifică toolurile disponibile pe 8 categorii cu pricing de referință

Organizează candidații pe categorii cu pricing actualizat 2026:

**(1) AI Writing & Copy**
- ChatGPT Plus — $20/mo (GPT-4o, ideal pentru copy persuasiv, brainstorming)
- Claude Pro — $20/mo (Claude 4.6 Sonnet, superior la nuanță, long-form, brand voice)
- Jasper — $49/mo Creator, $125/mo Pro (templates de marketing, brand voice training, campanii)
- Copy.ai — $49/mo Pro (workflows automatizate, GTM AI, bulk generation)
- Writesonic — $19/mo (entry-level, bun pentru volume mici)

**(2) AI Design & Vizual**
- Midjourney — $10/mo Basic, $30/mo Standard (cel mai bun pentru imagini stilizate, ad creatives)
- DALL-E 3 via ChatGPT Plus — inclus în $20/mo (integrare directă, bun pentru iterații rapide)
- Canva Pro + Magic Studio — $13/mo (templates + AI generativ, cel mai accesibil full-stack)
- Adobe Firefly — $5/mo (integrare Photoshop/Illustrator, ideal echipe creative existente Adobe)
- Ideogram — $8/mo (superior la text în imagini, postere, logo concepts)

**(3) AI Video**
- Runway Gen-3 — $15/mo Standard (video generation, VFX, cel mai avansat)
- Synthesia — $29/mo Starter (AI avatars, training videos, scale presentations)
- HeyGen — $29/mo Creator (AI avatars + lip sync, dubbing multilingv)
- Pictory — $25/mo (text-to-video, blog to video, ideal repurposing)
- ElevenLabs — $5/mo Starter, $22/mo Creator (voice cloning, narration, podcast)
- CapCut Pro — $10/mo (editare AI-powered, auto-captions, trending templates)

**(4) AI SEO**
- Surfer SEO — $89/mo Essential, $179/mo Scale (NLP optimization, content editor SERP-based)
- Clearscope — $170/mo (enterprise-grade content optimization)
- MarketMuse — $149/mo (content planning + briefs + optimization)
- Semrush — $130/mo Pro (all-in-one SEO + AI writing assistant)
- Ahrefs — $99/mo Lite (backlinks + keywords, nu are AI writer nativ)

**(5) AI Social Media**
- Buffer — $6/mo per canal (scheduling + AI assistant pentru captions)
- Hootsuite — $99/mo Professional (scheduling + analytics + AI composer)
- Publer — $12/mo (multi-platform scheduling cu AI assist, cel mai cost-efficient)
- Later — $25/mo (visual-first scheduling, ideal Instagram/TikTok)
- Metricool — $18/mo (scheduling + analytics + competitor tracking)

**(6) AI Email**
- Klaviyo — free up to 250 contacts, poi $20/mo+ (e-commerce personalization leader)
- Mailchimp — free up to 500, poi $13/mo+ (general purpose, AI subject lines)
- ActiveCampaign — $29/mo Lite (automation avansată, CRM integrat)
- Instantly — $30/mo (cold email outbound, warmup inclus)
- Beehiiv — free up to 2,500, poi $49/mo (newsletter-first, monetization built-in)

**(7) AI Analytics & Reporting**
- Looker Studio — gratuit (nativ Google, cel mai bun raport calitate/preț)
- Databox — free up to 3 sources, poi $59/mo (mobile-first, agenție-friendly)
- Polymer AI — $25/mo (AI-powered analytics, no-code dashboards)
- Triple Whale — $100/mo+ (e-commerce attribution, AI insights)
- Supermetrics — $39/mo (data connectors universali, nu dashboard)

**(8) AI Automation & Workflow**
- Zapier — free 100 tasks, poi $20/mo Starter (cel mai mare ecosistem de integrări)
- Make.com — free 1,000 ops, poi $9/mo Core (mai puternic decât Zapier, mai ieftin)
- n8n — self-hosted gratuit, cloud $20/mo (open-source, cel mai flexibil, necesită tehnic)
- Bardeen — $10/mo (browser automation + AI, ideal scraping și data entry)

Marchează overlap-urile: Jasper vs. ChatGPT/Claude (writing), Surfer vs. Clearscope (SEO), Buffer vs. Hootsuite (social). Un stack sănătos are maximum 1 tool per subcategorie.

### Pas 3: Score — Aplică matricea de evaluare pe 6 criterii ponderate

Pentru fiecare tool candidat, scorează 1-5 pe 6 criterii:

| Criteriu | Pondere | Ce evaluezi |
|----------|---------|-------------|
| Fit funcțional | 30% | Acoperă exact nevoia din Pasul 1? Output quality? |
| Cost / ROI | 25% | Preț lunar vs. ore economisite × costul orar al echipei |
| Integrare | 15% | API disponibil? Se conectează nativ cu stack-ul actual? |
| Ease of use | 10% | Learning curve pentru echipa existentă |
| Maturitate | 10% | Tool stabil, updates regulate, 50+ reviews pe G2 ≥4.0? |
| Scalabilitate | 10% | Poate crește cu echipa? Pricing linear sau exponențial? |

**Formula ROI pentru scoring**:
```
ROI mensual = (ore_economiste/lună × cost_orar_echipă) - cost_tool/lună
Payback ratio = ROI mensual / cost_tool/lună
```
Exemplu: Surfer SEO ($89/mo) economisește 8 ore/lună la $35/oră = $280 economisiți → ROI = $191/lună → Payback ratio = 2.1x.

Un tool cu payback ratio sub 1.0x nu se justifică fără beneficii calitative semnificative documentate.

### Pas 4: Compare — Construiește shortlist cu AI-assisted comparison

Selectează top 2-3 tools per categorie cu scor ponderat ≥3.5. Folosește acest prompt pentru a genera tabel comparativ:

```
Ești un Marketing Technology Analyst. Compară aceste [X] tools de [categorie]:
[Tool 1 - preț, features cheie]
[Tool 2 - preț, features cheie]
[Tool 3 - preț, features cheie]

Generează un tabel comparativ cu coloanele:
- Feature | Tool 1 | Tool 2 | Tool 3
Acoperă: preț, free tier limitări, API disponibilitate, integrări native (CRM, CMS, analytics),
nr. reviews G2 și scor mediu, data ultimei actualizări majore, limbă interfață,
support quality, contracte (monthly/annual lock-in).
La final, recomandă cel mai potrivit pentru: (a) solopreneur, (b) echipă 3-5, (c) agenție 10+.
```

**Model routing**: Folosește **Claude** pentru această comparație — superior la analiză structurată și mai puțin predispus la hallucination pe date factuale recente. Verifică manual prețurile pe site-urile oficiale (AI poate avea date vechi).

### Pas 5: Trial — Testează top 2 per categorie pe use case real

Pentru fiecare funcție prioritară (top 3 din Pasul 1), testează cele 2 tool-uri finaliste cu un task real din workflow-ul actual. Protocol de trial:

1. **Definește task-ul de test** — folosește un deliverable real (nu demo data). Ex: scrie un articol de 1500 cuvinte pentru keyword-ul X cu Tool A și Tool B.
2. **Alocă timp egal** — max 2 ore per tool, cronometrat.
3. **Evaluează pe 4 axe**: (a) Calitate output brut (fără editare), (b) Timp efectiv de producție, (c) Nivel de editare umană necesar, (d) Experiența utilizatorului (frustrări, bugs).
4. **Documentează side-by-side** — screenshot-uri, output-uri, timpi.
5. **Cere feedback echipei** — dacă altcineva va folosi tool-ul zilnic, include-l în trial.

**Regula strictă**: Niciun tool cu cost >$50/lună nu se adoptă fără trial pe use case real. Free trials sau money-back guarantee sunt standard — dacă nu oferă, e un red flag.

### Pas 6: Decide — Selectează stackul final și calculează bugetul total

Alege maxim 1 tool per funcție primară. Construiește documentul „AI Marketing Stack":

```markdown
# AI Marketing Stack — [Organizație] — [Data]

## Stack selectat
| Funcție | Tool ales | Plan | Cost/lună | Owner intern | Review date |
|---------|-----------|------|-----------|-------------|-------------|
| Writing | Claude Pro | $20/mo | $20 | [Nume] | [+6 luni] |
| SEO | Surfer SEO | Essential | $89 | [Nume] | [+6 luni] |
| Design | Canva Pro | Team | $13/pers | [Nume] | [+6 luni] |
| ... | ... | ... | ... | ... | ... |

## Cost total lunar: $XXX
## ROI estimat lunar: $XXX (ore economiste × cost orar)
## Payback period: X luni

## Tools evaluate și respinse (cu motivul)
- [Tool X] — respins pentru [motiv]
```

**Pentru agenții**: Construiește 3 tier-uri de stack pe care le oferi clienților:
- **Starter** ($100-200/mo): ChatGPT + Canva + Buffer + Looker Studio
- **Growth** ($300-500/mo): Claude Pro + Surfer SEO + Hootsuite + Klaviyo + Zapier
- **Scale** ($800-1500/mo): Jasper + Surfer + HeyGen + ActiveCampaign + Make + Databox

### Pas 7: Onboard — Creează SOP-uri și prompt libraries per tool

Pentru fiecare tool selectat, creează un SOP de 1 pagină:

```markdown
# SOP: [Nume Tool] — [Funcție]

## Acces
- URL: [link]
- Login: [metoda — SSO/individual]
- Plan: [detalii plan]

## Use cases acoperite intern
1. [Use case 1 — frecvență]
2. [Use case 2 — frecvență]
3. [Use case 3 — frecvență]

## Prompt templates testate
### Template 1: [Nume]
[Prompt complet copiat, gata de paste]

### Template 2: [Nume]
[Prompt complet]

## Ce NU face acest tool
- [Limitare 1]
- [Limitare 2]

## Troubleshooting
- [Problemă frecventă 1] → [Soluție]
```

### Pas 8: Automate Stack Audit — Configurează re-evaluare semestrială

Setează un reminder recurent (Google Calendar, Notion recurring) la fiecare 6 luni:
1. Verifică dacă pricing-ul s-a schimbat pe tools active
2. Verifică dacă features noi fac un tool alternativ mai atractiv
3. Analizează utilizarea reală (logs, nr. de sesiuni) — tool nefolosat 30+ zile = candidat de eliminare
4. Verifică satisfacția echipei (survey rapid, 3 întrebări: folosești zilnic? ești satisfăcut? ce lipsește?)
5. Actualizează documentul AI Stack cu orice schimbare

**Automation cu Make.com** (workflow opțional):
- Trigger: Schedule (1st of month, every 6 months)
- Module 1: Google Forms → trimite survey echipei
- Module 2: Aggregate responses → route to Claude API
- Module 3: Claude generează recomandări de ajustare
- Module 4: Slack notification cu summary-ul

### Pas 9: Agency Delivery — Transformă evaluarea în serviciu client

Dacă oferi tool selection ca serviciu de agenție:

**Pachet „AI Stack Audit"** (preț sugerat: $500-1,500 one-time):
- Deliverable 1: Assessment maturitate curentă (Pasul 1)
- Deliverable 2: Stack recommendation document cu 3 tier-uri
- Deliverable 3: Implementation roadmap pe 30 zile
- Deliverable 4: 3 SOP-uri cu prompt libraries pentru toolurile alese
- Deliverable 5: Training session 2 ore (live sau Loom)

**ROI pitch pentru client**: „Stack-ul recomandat economisește [X ore/lună] la un cost de [Y €/lună], generând un ROI net de [Z €/lună]. Payback period: [N săptămâni]."

### Pas 10: Quality Gate — Hallucination check pe pricing și features

AI-ul poate genera prețuri și features incorecte. Înainte de a finaliza orice document:
1. Verifică **manual** prețurile pe site-urile oficiale ale toolurilor selectate
2. Verifică **manual** dacă feature-urile menționate există cu adevărat pe planul ales
3. Verifică **G2.com** pentru nr. de reviews și scor mediu (nu te baza pe AI pentru cifre exacte)
4. Verifică **data ultimei actualizări** — un tool fără update în 6+ luni poate fi în decline

---

## 3. Cortex Logging

```json
{
  "text": "AI Tool Selection Matrix executat pentru [organizație/proiect]. Funcții evaluate: [X]. Tooluri shortlistate: [Y]. Stack final adoptat: [lista]. Cost lunar total: [€/mo]. ROI estimat: [€/mo]. Payback ratio mediu: [X.Xx].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-101",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["tool-selection", "ai-stack", "martech", "evaluation-framework", "pricing", "roi"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + onboarding nou client/proiect AI marketing

### WHEN
La orice decizie de adoptare tool AI marketing (nou sau înlocuire)

### HOW (violation detection)
- Tool adoptat fără scorare pe 6 criterii din Pasul 3 → violation
- Stack cu overlap funcțional nerezolvat → violation
- Nicio dată de re-evaluare setată → violation
- Trial sărit pentru tool cu cost >$50/lună → violation critică
- Pricing neverificat manual pe site oficial → violation (hallucination risk)
- ROI necalculat → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-102 → multi-model stack setup (complement)
- AIM-112 → adoption plan (stack-ul devine input pentru training)

### VERIFY
- [ ] Toate funcțiile de marketing mapate cu ore și satisfacție?
- [ ] Scoring complet pe 6 criterii ponderate?
- [ ] Trial realizat pe use case real pentru tools >$50/mo?
- [ ] ROI calculat cu payback ratio?
- [ ] Pricing verificat manual pe site-uri oficiale?
- [ ] Stack document creat și partajat echipei?
- [ ] Re-evaluare semestrială calendarizată?

**VK-uri:**
1. `✅ [PROC] AIM-101 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Marketing Tool Selection Matrix" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Comparație tools | Claude Sonnet | Analiză structurată, mai puțin hallucination |
| Scoring și calcul ROI | Claude Sonnet | Calcule precise, format tabelar |
| Decizie arhitectură stack | Claude Opus | Viziune strategică, trade-offs complexe |
| Verificare prețuri live | Gemini Pro | Web access nativ, date recente |
| Bulk research features | DeepSeek | Cost-efficient pentru volume mari |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| G2.com / Capterra | Review data și scoring pentru evaluare |
| Claude / ChatGPT | Generare tabel comparativ și analiză |
| Gemini Pro | Verificare prețuri și features live |
| Site-uri oficiale tools | Validare manuală pricing |
| Notion / Google Sheets | AI Stack document |
| Make.com / Zapier | Automatizare re-evaluare semestrială |
| Cortex | Logging execuție |

---

## 6. Instrumente

- Claude Pro ($20/mo) — comparații, scoring, analiză strategică
- ChatGPT Plus ($20/mo) — brainstorming, quick research
- Gemini Pro (free/paid) — web research live pentru prețuri
- G2.com (free) — review data
- Product Hunt (free) — discovery tools noi
- Notion / Google Sheets — tool comparison matrix și Stack doc
- Make.com ($9/mo) — automatizare audit semestrial

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Timp selecție per funcție | ≤ 4 ore |
| Tools în stack cu overlap 0 | 100% |
| Trial realizat (tools >$50/mo) | 100% |
| Re-evaluare programată | Semestrial |
| Payback ratio mediu stack | ≥ 1.5x |
| Prețuri verificate manual | 100% |
| Satisfacție echipă cu stack (survey) | ≥ 4.0/5 |
