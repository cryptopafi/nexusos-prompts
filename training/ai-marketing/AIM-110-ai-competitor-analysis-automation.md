---
type: procedure
created: 2026-03-20
status: active
slug: aim-110-ai-competitor-analysis-automation
tags: [procedure, nexus]
---

# AI Competitor Analysis Automation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Automatizarea monitorizării continue a competitorilor folosind AI și tools specializate — de la tracking ads, conținut, SEO și prețuri la generarea de intelligence reports automatizate. Include workflow Make.com/n8n pentru colectare automată, prompt templates pentru synthesis, triage framework pentru acțiuni, și delivery ca serviciu de agenție.

---

## 1. Problema

Analiza competitivă realizată o dată la 6 luni este depășită în momentul în care e prezentată. Un competitor lansează 3-5 campanii noi pe lună, publică 10-20 articole, testează 50+ ad creatives, și ajustează prețuri trimestrial. Fără monitoring continuu, reacționezi la semnale cu 30-60 zile întârziere — o eternitate în marketing digital. Această procedură configurează un sistem de intelligence automatizat care: colectează automat (sau semi-automat) semnale din 5+ surse, sintetizează cu AI, livrează raport acționabil săptămânal, și transformă intelligence în decizii concrete cu SLA de răspuns.

Situații acoperite:
- Setup inițial al sistemului de competitor monitoring (2-3 ore one-time)
- Tracking ads active ale competitorilor (Meta/Google/TikTok)
- Monitoring conținut organic și SEO ranking
- Tracking prețuri, oferte, lansări de produse
- Generarea de intelligence reports automate cu AI (săptămânal/lunar)
- Competitive intelligence ca serviciu de agenție

---

## 2. Procedura

### Pas 1: Define — Competitor Set și Signal Map

**Pasul 1A — Competitor Selection:**

| # | Competitor | Tip | Website | Prioritate | Monitoring depth |
|---|-----------|-----|---------|------------|-----------------|
| 1 | [Competitor 1] | Direct | [URL] | P1 | Full (ads + content + SEO + pricing) |
| 2 | [Competitor 2] | Direct | [URL] | P1 | Full |
| 3 | [Competitor 3] | Direct | [URL] | P1 | Full |
| 4 | [Competitor 4] | Indirect | [URL] | P2 | Partial (content + pricing) |
| 5 | [Competitor 5] | Indirect | [URL] | P2 | Partial |
| 6 | [Aspirational] | Category leader | [URL] | P3 | Light (strategy signals only) |

**Regula**: 3-5 direcți (P1) + 2-3 indirecți (P2) + 1-2 aspirational (P3). Nu monitorizezi 15 competitori — concentrează-te pe cei care îți iau clienți azi.

**Pasul 1B — Signal Map (ce urmărești per competitor):**

| Signal | Sursă | Frecvență check | Impact |
|--------|-------|-----------------|--------|
| Ads active (Meta) | Meta Ad Library | Săptămânal | Ce mesaje testează, ce oferte promovează |
| Ads active (Google) | Google Ads Transparency | Săptămânal | Keywords pe care biddează, ad copy |
| Ads active (TikTok) | TikTok Creative Center | Săptămânal | Formate creative, hooks |
| Conținut blog nou | RSS / Feedly | Săptămânal | Strategie de conținut, keywords atacate |
| Social media posts | Manual / Social Blade | Bi-săptămânal | Engagement, frecvență, ton |
| SEO rankings | Semrush / Ahrefs | Lunar | Keywords câștigate/pierdute |
| Prețuri și oferte | Website / cachare | Săptămânal | Ajustări pricing, promoții |
| Produse noi | Website + PR | Lunar | Features noi, lansări |
| Press / mențiuni | Google Alerts | Continuu (automat) | Funding, parteneriate, HR signals |
| Reviews (G2/Trustpilot) | G2 / Trustpilot | Lunar | Sentiment clienți, pain points |
| Job postings | LinkedIn Jobs | Lunar | Direcție strategică (hiring = investiție) |
| Tech stack | BuiltWith / Wappalyzer | Trimestrial | Schimbări de tooling |

### Pas 2: Setup — Configurează sursele de monitoring (one-time, 2-3 ore)

**Ads Monitoring:**

1. **Meta Ad Library** (ads.meta.com/ad-library) — GRATUIT:
   - Caută fiecare competitor P1 → bookmark → verificare săptămânală
   - Filtrează: Active ads → Country → Platform (FB/IG)
   - Ce notezi: mesajul principal, oferta, formatul (image/video/carousel), landing page

2. **Google Ads Transparency Center** (adstransparency.google.com) — GRATUIT:
   - Caută fiecare competitor → verifică ads active
   - Ce notezi: headlines, descriptions, keywords implicate, extensions

3. **TikTok Creative Center** (ads.tiktok.com/business/creativecenter) — GRATUIT:
   - Top ads, trending formats, competitor ad search

4. **Tools automatizate** (opțional, paid):
   - **Foreplay.co** ($49/mo) — salvare ads, organizare boards, share cu echipa
   - **AdSpy** ($149/mo) — search engine de ads FB/IG, filtre avansate
   - **BigSpy** ($99/mo) — multi-platform ad spy
   - **SocialPeta** ($69/mo) — TikTok + Meta ad intelligence

**Content & SEO Monitoring:**

5. **Feedly** (free/Pro $8/mo) — RSS reader:
   - Abonează-te la blog-urile tuturor competitorilor P1+P2
   - Creează folder „Competitors" — scan săptămânal

6. **Google Alerts** (google.com/alerts) — GRATUIT:
   - Creează alertă per competitor: `"[Competitor Name]"` (exact match)
   - Frecvență: As-it-happens (email digest zilnic)
   - Include variante de nume (Competitor Inc, @competitor, etc.)

7. **Semrush Position Tracking** ($130/mo Pro):
   - Setup: keywords comune (50-100) → track competitor + own rankings
   - Alert: notificări pe schimbări de >5 poziții
   - Organic traffic estimate per competitor

8. **Ahrefs** ($99/mo Lite) alternativ:
   - Competitor backlink monitoring (new backlinks = new partnerships/PR)
   - Content Explorer: ce articole noi au publicat

**Pricing & Product Monitoring:**

9. **Manual price tracking** (Google Sheet):
   - Creează tab per competitor: produs, preț curent, preț anterior, data schimbării
   - Check săptămânal pe pricing pages

10. **Visualping** (free 5 pages / $14/mo) sau **Distill.io** — website change detection:
    - Monitorizează pricing page per competitor
    - Alert pe email la orice schimbare

**Reputation Monitoring:**

11. **G2.com** / **Trustpilot** / **Capterra** — review tracking:
    - Bookmark profile competitor
    - Check lunar: nr. reviews noi, scor mediu, recenzii negative (pain points)

12. **LinkedIn Jobs** — hiring signals:
    - Check lunar: ce roluri deschid (hiring AI team = investing in AI, hiring sales = expanding)

### Pas 3: Collect — Rutina săptămânală structurată (30-45 minute)

**Friday Intelligence Routine (30-45 min):**

| # | Task | Timp | Tool | Output |
|---|------|------|------|--------|
| 1 | Check Meta Ad Library top 3 competitors | 10 min | Meta Ad Library | Screenshots + note ads noi |
| 2 | Scan Feedly competitor blog posts | 5 min | Feedly | Titluri + linkuri articole noi |
| 3 | Review Google Alerts digest | 5 min | Gmail | Note mențiuni relevante |
| 4 | Check pricing pages (Visualping alerts) | 5 min | Visualping / manual | Schimbări de preț notate |
| 5 | Log toate datele în Competitor Intel Sheet | 10 min | Google Sheet | Date brute structurate |
| 6 | Run AI synthesis (Pasul 4) | 10 min | Claude/ChatGPT | Intelligence report |

**Google Sheet — Competitor Intel Database:**
```
Tab per competitor:
| Data | Signal type | Detail | Source | Impact (1-5) | Action needed? |
| 2026-03-20 | New ad | Video ad: hook „Stop wasting time on..." | Meta Ad Library | 3 | Monitor CTR |
| 2026-03-20 | Blog post | „10 Ways to..." | Feedly RSS | 2 | No action |
| 2026-03-18 | Price change | Pro plan: $99 → $79/mo | Visualping | 5 | Evaluate response |
```

### Pas 4: Synthesize — AI Intelligence Analysis (săptămânal)

Prompt pentru **Claude Sonnet** (structured analysis):
```
Ești un competitive intelligence analyst. Analizează aceste date despre competitorii noștri
din ultimele 7 zile:

BUSINESS NOSTRU: [ce faci, pentru cine, diferențiator]

COMPETITOR 1 — [Nume]:
Ads noi: [descriere]
Conținut nou: [titluri articole]
Prețuri: [schimbări sau status quo]
Alte semnale: [PR, job postings, etc.]

COMPETITOR 2 — [Nume]:
[same structure]

COMPETITOR 3 — [Nume]:
[same structure]

ANALIZEAZĂ:

1. SEMNALE CRITICE (necesită reacție în 48h):
   Ce a făcut un competitor care ne amenință direct? (preț mai mic, campanie agresivă,
   feature nou care acoperă un diferențiator al nostru)

2. TRENDS (observabile pe 2-3 săptămâni consecutive):
   Ce direcție iau competitorii colectiv? (toți investesc în video? migrează pe TikTok?
   reduc prețurile? se concentrează pe un segment nou?)

3. OPORTUNITĂȚI (ce NU fac ei):
   Ce keywords nu acoperă? Ce segment ignoră? Ce format de conținut lipsește din strategia lor?
   Ce obiecție a clienților lor apare în reviews dar nu o adresează?

4. MESSAGING INTELLIGENCE:
   Ce hooks și mesaje testează în ads? Ce limbaj folosesc? Ce oferte promovează?
   Cum ne diferențiem prin messaging?

5. RECOMANDĂRI ACȚIONABILE (top 3):
   Ce facem concret în săptămâna viitoare bazat pe aceste semnale?
   Format: [Acțiune] — [De ce] — [Owner] — [Deadline]
```

### Pas 5: Report — Generează intelligence report structurat

**Report template (1-2 pagini, weekly):**

```markdown
# Competitive Intelligence Report — Week [X], [Luna] 2026

## Executive Summary (3 propoziții)
[Ce s-a întâmplat important, ce înseamnă pentru noi, ce acțiune luăm]

## 🔴 Semnale Critice (acțiune în 48h)
| Signal | Competitor | Impact | Acțiune recomandată |
|--------|-----------|--------|---------------------|
| [ex: Preț redus cu 20%] | [Competitor X] | HIGH | [Evaluăm răspuns pricing] |

## 🟡 Trends (watch list)
- [Trend 1]: [descriere + ce înseamnă]
- [Trend 2]: [descriere + ce înseamnă]

## 🟢 Oportunități
- [Oportunitate 1]: [ce putem face + effort estimat]
- [Oportunitate 2]: [ce putem face + effort estimat]

## Ads Intelligence
| Competitor | Ads noi | Hook dominant | Ofertă | Format |
|-----------|---------|--------------|--------|--------|
| [C1] | [nr] | [tip hook] | [ofertă] | [video/image] |

## Content Intelligence
| Competitor | Articole noi | Topic dominant | Keyword gap |
|-----------|-------------|---------------|-------------|
| [C1] | [nr] | [topic] | [ce lipsește] |

## Next Week Focus
1. [Acțiune 1] — Owner: [nume] — Deadline: [data]
2. [Acțiune 2] — Owner: [nume] — Deadline: [data]
3. [Acțiune 3] — Owner: [nume] — Deadline: [data]
```

### Pas 6: React — Triage framework cu SLA-uri

| Signal type | Severity | SLA răspuns | Acțiune tip |
|------------|----------|-------------|-------------|
| Competitor preț agresiv (<-20%) | CRITICAL | 48 ore | Evaluare contra-ofertă, messaging update |
| Competitor campanie virală | HIGH | 72 ore | Analiză mesaj, consider response content |
| Competitor feature nou major | HIGH | 1 săptămână | Product eval, messaging diferențiere |
| Competitor articol pe keyword-ul nostru | MEDIUM | 2 săptămâni | Content improvement sau new article |
| Competitor angajează pe roluri noi | LOW | Notare | Track trend, fără acțiune imediată |
| Competitor PR/funding | LOW | Notare | Recalibrare competitive landscape |

**Regula de aur**: Intelligence fără acțiune = waste. Fiecare raport TREBUIE să aibă minim 2 acțiuni concrete cu owner și deadline.

### Pas 7: Automate — Make.com Intelligence Pipeline

**Make.com Competitor Monitoring (semi-automated):**
```
Scenario 1: Google Alerts Collector (daily)
Trigger: Gmail — new email from googlealerts-noreply@google.com
→ Module 1: Parse email body → extract competitor name + mention
→ Module 2: Google Sheets → append to „Competitor Intel" tab
→ Module 3: IF mention contains [crisis keywords: „funding", „acquisition", „shutdown"]
  → Slack urgent alert

Scenario 2: RSS Feed Collector (daily)
Trigger: RSS → competitor blog feed
→ Module 1: Parse title + URL + date
→ Module 2: Google Sheets → append to „Competitor Content" tab
→ Module 3: IF title contains [target keywords: competitor enters our niche]
  → Slack alert

Scenario 3: Weekly Intelligence Report (Friday 9 AM)
Trigger: Schedule (every Friday, 9:00)
→ Module 1: Google Sheets → read all new entries from this week
→ Module 2: HTTP → Claude API → synthesis prompt (Pasul 4)
→ Module 3: Google Docs → create weekly report from Claude output
→ Module 4: Slack → post report link with summary
→ Module 5: Email → send to stakeholders

Cost: $9/mo Make.com + $1-2/mo Claude API = $10-11/mo total
Time saved: 3-4 ore/săptămână → 30-45 minute/săptămână (only manual ad library check)
```

**n8n Alternative (self-hosted, free):**
Same flow with HTTP Request nodes, Gmail trigger, Google Sheets nodes.

### Pas 8: Deep Dive — Quarterly competitive audit complet

La fiecare trimestru, rulează un audit profund (dincolo de weekly monitoring):

Prompt pentru **Claude Opus** (strategic analysis):
```
Realizează un competitive audit complet bazat pe 3 luni de date colectate:

BUSINESS NOSTRU: [poziționare, USP, target market, pricing]

[Paste 3 luni de intelligence data per competitor]

AUDIT STRATEGIC:

1. MARKET POSITIONING MAP:
   Mapează toți competitorii pe axe: [Preț Low ←→ High] x [Feature simplicity ←→ Complexity]
   Unde suntem noi? Unde e spațiu neocupat?

2. STRENGTHS & VULNERABILITIES per competitor:
   Ce fac mai bine ca noi? Ce facem noi mai bine?
   Ce vulnerabilitate exploatabilă are fiecare?

3. STRATEGY PREDICTION:
   Bazat pe semnalele din ultimele 3 luni (hiring, ads, content, pricing),
   ce strategie urmează fiecare competitor în Q[X+1]?

4. THREAT ASSESSMENT:
   Care competitor ne amenință cel mai mult și pe ce dimensiune?
   Care competitor e cel mai probabil să intre pe segmentul nostru?

5. OPPORTUNITY MATRIX:
   Ce segmente / keywords / mesaje / canale sunt sub-exploatate de competiție?
   Ce „blue ocean" gap există?

6. STRATEGIC RECOMMENDATIONS (5):
   Ce schimbări strategice recomandăm bazat pe acest audit?
   Prioritizare: Impact (1-5) × Effort (1-5 invers) = Scor prioritate
```

### Pas 9: Competitor Ad Swipe File — Biblioteca de inspirație

Construiește și menține o bibliotecă organizată de ads competitive:

**Structure Notion/Google Drive:**
```
📁 Competitor Ad Swipe File
├── 📁 [Competitor 1]
│   ├── 📁 Meta Ads
│   │   ├── [Screenshot] — Hook: [text] — Format: [video] — Date: [data]
│   │   └── ...
│   ├── 📁 Google Ads
│   └── 📁 TikTok Ads
├── 📁 [Competitor 2]
│   └── ...
├── 📁 Best-of (cross-competitor)
│   ├── 📁 Best Hooks
│   ├── 📁 Best Offers
│   ├── 📁 Best Formats
│   └── 📁 Best Landing Pages
└── 📄 Patterns & Insights (updated monthly)
```

**Alternative tool**: Foreplay.co ($49/mo) — save ads direct din Meta Ad Library, organizare boards, colaborare echipă.

### Pas 10: Agency Delivery — Competitive Intelligence as a Service

**Pachet „Competitive Intelligence" (preț sugerat):**
- **Basic** ($300/mo): Weekly report (3 competitori, ads + content monitoring)
- **Pro** ($800/mo): Weekly report (5 competitori, full signal monitoring) + quarterly audit
- **Enterprise** ($1,500/mo): Daily alerts + weekly report + quarterly audit + ad swipe file + strategy recommendations

**Deliverables:**
1. Weekly intelligence report (1-2 pagini, template din Pasul 5)
2. Quarterly competitive audit (5-10 pagini, template din Pasul 8)
3. Ad swipe file menținut și actualizat (Notion/Foreplay)
4. Alert system (Slack/email) pe semnale critice
5. Recommendations deck trimestrial (prezentare pentru management)

**Cost intern cu AI:**
- Weekly: 45 min muncă + $1-2 AI = ~$40-50/săptămână (la $50/h)
- Monthly cost: ~$170-210
- Margin pe Basic: 40-45%. Margin pe Pro/Enterprise: 60-75%.

---

## 3. Cortex Logging

```json
{
  "text": "Competitor Analysis executat. Competitori monitorizați: [X]. Semnale noi: [Y]. Semnale critice: [Z]. Intelligence report generat: [da/nu]. Acțiuni identificate: [nr]. Quarterly audit: [da/nu].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-110",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["competitor-analysis", "market-intelligence", "ad-monitoring", "seo-tracking", "automation", "competitive-audit"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + setup inițial monitoring și execuție săptămânală

### WHEN
La fiecare sesiune săptămânală de competitive intelligence

### HOW (violation detection)
- Competitor analysis mai rar de lunar → violation
- Intelligence report fără acțiuni concrete (owner + deadline) → violation
- Ad Library neverificat >2 săptămâni → violation
- Semnale critice fără răspuns în SLA (48h critical, 72h high) → violation
- Quarterly audit sărit → violation
- Intelligence report distribuit doar marketerilor (nu management) → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-106 → ad copy (competitor hooks ca inspirație, nu copiere)
- AIM-108 → SEO (competitor keywords ca input pentru content strategy)
- AIM-111 → reporting (competitor metrics în dashboard)

### VERIFY
- [ ] Competitor set definit (3-5 P1 + 2-3 P2)?
- [ ] Signal map completat per competitor?
- [ ] Toate sursele de monitoring configurate (one-time setup)?
- [ ] Google Alerts active per competitor?
- [ ] RSS feeds configurate în Feedly?
- [ ] Rutina săptămânală calendarizată (30-45 min)?
- [ ] AI synthesis rulată pe datele colectate?
- [ ] Report distribuit echipei cu acțiuni concrete?
- [ ] Quarterly audit programat?
- [ ] Ad swipe file menținut?

**VK-uri:**
1. `✅ [PROC] AIM-110 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Competitor Analysis Automation" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Weekly synthesis | Claude Sonnet | Structured analysis, patterns |
| Quarterly strategic audit | Claude Opus | Deep strategic thinking |
| Quick ad analysis | ChatGPT GPT-4o | Rapid pattern recognition |
| Trend research live | Gemini Pro | Web access, date actuale |
| Bulk data processing | DeepSeek | Cost-efficient, large datasets |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Meta Ad Library (free) | Ads competitor monitoring |
| Google Ads Transparency (free) | Google ads monitoring |
| Google Alerts (free) | PR și mențiuni automate |
| Feedly (free/$8/mo) | RSS content monitoring |
| Semrush ($130/mo) / Ahrefs ($99/mo) | SEO rank tracking |
| Visualping (free/$14/mo) | Website change detection |
| Claude Sonnet / ChatGPT | AI synthesis și analysis |
| Make.com ($9/mo) / n8n (free) | Workflow automation |
| Google Sheets / Notion | Data storage și reporting |

---

## 6. Instrumente

- Meta Ad Library (free) — ads transparence, mandatory
- Google Alerts (free) — automated PR monitoring
- Feedly ($8/mo Pro) — RSS content aggregation
- Foreplay.co ($49/mo) — ad swipe file, competitor creative library
- Semrush ($130/mo) — keyword rank tracking, competitor organic traffic
- Visualping ($14/mo) — website change monitoring (pricing pages)
- Claude Pro ($20/mo) — synthesis, strategic analysis
- ChatGPT Plus ($20/mo) — quick analysis, ad copy breakdown
- Make.com ($9/mo) — automation pipeline
- SimilarWeb (free/paid) — traffic estimates per competitor

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Frecvență monitoring | Săptămânal |
| Timp per sesiune colectare | ≤ 45 min |
| Intelligence report distribuit | 100% săptămânal |
| Acțiuni generate per raport | ≥ 2 |
| SLA semnale critice (48h) | 100% |
| Quarterly audit completat | 4/an |
| Ad swipe file size | Growing (20+ ads/lună) |
| Cost total monitoring (tools + time) | ≤ $300/mo |
