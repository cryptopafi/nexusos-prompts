---
type: procedure
created: 2026-03-20
status: active
slug: aim-102-multi-model-marketing-stack-setup
tags: [procedure, nexus]
---

# Multi-Model Marketing Stack Setup (ChatGPT + Claude + Gemini + DeepSeek) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea și orchestrarea unui stack multi-model AI care combină ChatGPT, Claude, Gemini și DeepSeek pentru diferite funcții de marketing, maximizând calitatea output-ului per cost. Include routing table complet, prompt templates per model, cost benchmarks și workflow automation.

---

## 1. Problema

Fiecare model AI are puncte forte diferite: ChatGPT exceleaza la copy persuasiv și ideation rapid, Claude la nuanță strategică și long-form, Gemini la research live și integrare Google, DeepSeek la cost-efficiency și task-uri analitice de volum. Marketerii care folosesc un singur model pierd 30-50% din potențialul AI și plătesc de 2-3x prea mult pentru task-uri simple care nu necesită GPT-4o. Agențiile care gestionează 5-10 clienți simultan fără routing inteligent pot ajunge la costuri API de $500-2,000/lună care pot fi reduse la $150-400 prin routing corect.

Situații acoperite:
- Configurarea unui workflow hibrid AI pentru o echipă de marketing (1-20 persoane)
- Reducerea costurilor API cu 40-60% menținând calitatea output-ului
- Paralelizarea task-urilor de marketing pe modele specializate
- Onboarding echipă nouă pe stack multi-model cu training structurat
- Arbitraj calitate/cost pentru deliverables de agenție

---

## 2. Procedura

### Pas 1: Audit — Inventariază task-urile de marketing recurente cu volumetrie

Listează toate task-urile de marketing recurente. Pentru fiecare, estimează volumul și complexitatea:

| Task | Frecvență | Volume/exec | Complexitate | Ore manuale |
|------|-----------|-------------|-------------|-------------|
| Ad copy creation | Zilnic | 10-20 variante | Mediu | 2h |
| Email drafting | 2x/săpt | 2-3 emailuri | Mediu | 3h |
| Blog posts | Săptămânal | 1-2 articole | Complex | 6h |
| Social media posts | Zilnic | 5-10 posturi | Simplu | 1.5h |
| Keyword research | Săptămânal | 20-50 keywords | Simplu | 2h |
| Competitor analysis | Lunar | 1 raport | Complex | 4h |
| Reporting summaries | Săptămânal | 1 raport | Mediu | 2h |
| Creative briefs | 2x/săpt | 1-2 briefs | Complex | 1.5h |
| Product descriptions | Lunar | 10-50 produse | Simplu | 3h |
| SEO meta tags | Săptămânal | 5-20 pagini | Simplu | 1h |
| Landing page copy | Lunar | 2-3 pagini | Complex | 4h |
| Presentation decks | Lunar | 1-2 prezentări | Mediu | 3h |

Totalul orelor economisite cu AI devine baza calculului ROI.

### Pas 2: Map — Alocă fiecare task pe modelul optim cu routing table detaliat

**ROUTING TABLE COMPLET — Marketing AI Tasks:**

| Task | Model primar | Motiv | Model fallback |
|------|-------------|-------|----------------|
| Ad copy persuasiv (Meta/TikTok) | **ChatGPT GPT-4o** | Superior la persuasiune, hooks, emotional triggers | Claude Sonnet |
| Email sales sequences | **ChatGPT GPT-4o** | Ton conversațional natural, AIDA/PAS fluent | Claude Sonnet |
| Landing page headlines | **ChatGPT GPT-4o** | Creativity și punch pe text scurt | Claude Sonnet |
| Brand storytelling | **ChatGPT GPT-4o** | Narativ emoțional, voce distinctă | Claude Sonnet |
| Creative brainstorming | **ChatGPT GPT-4o** | Divergent thinking, volum mare de idei | Gemini Pro |
| Blog posts long-form (1500+) | **Claude Sonnet** | Coerență pe texte lungi, nuanță, mai puțin generic | ChatGPT GPT-4o |
| Brand voice documentation | **Claude Sonnet** | Precizie în capturing tone, consistency checks | - |
| Competitor analysis reports | **Claude Sonnet** | Analiză structurată, reasoning step-by-step | ChatGPT GPT-4o |
| Strategy documents | **Claude Opus** | Gândire strategică profundă, trade-offs | Claude Sonnet |
| QA și editing AI content | **Claude Sonnet** | Detectare AI-isms, hallucination check | - |
| Research cu surse web live | **Gemini Pro** | Acces web nativ, integrare Google Search | Perplexity |
| Google Ads integration | **Gemini Pro** | Nativ Google ecosystem, Ads data access | ChatGPT |
| Prezentări Google Slides | **Gemini Pro** | Integrare Workspace, slide generation | - |
| Summarizare documente lungi | **Gemini Flash** | Context window mare, cost mic, viteză | DeepSeek |
| YouTube SEO (titles, descriptions) | **Gemini Pro** | Înțelegere ecosistem YouTube/Google | ChatGPT |
| Bulk product descriptions | **DeepSeek** | Cost 95% mai mic decât GPT-4o, calitate suficientă | Claude Haiku |
| SEO meta tags (title + desc) | **DeepSeek** | Volume mari, task simplu, cost minimal | ChatGPT |
| Social post variations (20+) | **DeepSeek** | Bulk generation cost-efficient | ChatGPT |
| Template filling | **DeepSeek** | Repetitiv, nu necesită creativitate | - |
| Data analysis (CSV/metrics) | **DeepSeek** | Calcule precise, cost mic, Code Interpreter | ChatGPT |
| Image generation (ad creatives) | **Midjourney** | Calitate vizuală superioară | DALL-E 3 |
| Quick image iterations | **DALL-E 3** (via ChatGPT) | Iterații rapide, integrare chat | Midjourney |
| Voiceover narration | **ElevenLabs** | Calitate vocală superioară, voice cloning | - |
| AI avatar videos | **HeyGen/Synthesia** | Lip sync, multilingv, scalabil | - |

### Pas 3: Configure — Setează accesul și credentials cu security best practices

**Setup per model:**

**ChatGPT GPT-4o:**
- UI: chat.openai.com (Plus $20/mo — include GPT-4o, DALL-E 3, Code Interpreter)
- API: platform.openai.com → API Keys → Create new key
- Pricing API: $2.50/1M input tokens, $10/1M output tokens
- Rate limit (Tier 1): 500 RPM, $100/mo spend cap
- Best practice: Setează spend alert la 80% din buget lunar

**Claude Sonnet / Opus:**
- UI: claude.ai (Pro $20/mo — include toate modelele)
- API: console.anthropic.com → API Keys
- Pricing API Sonnet: $3/1M input, $15/1M output; Opus: $15/1M input, $75/1M output
- Rate limit: 4,000 RPM (Sonnet), 1,000 RPM (Opus)
- Best practice: Sonnet pentru 90% tasks, Opus doar pentru strategie

**Gemini Pro / Flash:**
- UI: gemini.google.com (free / Advanced $20/mo cu Google One)
- API: aistudio.google.com → Get API Key
- Pricing API: Pro $1.25/1M input, $5/1M output; Flash $0.075/1M input, $0.30/1M output
- Best practice: Flash pentru summarizare, Pro pentru research

**DeepSeek:**
- UI: chat.deepseek.com (free)
- API: platform.deepseek.com → API Keys
- Pricing API: $0.14/1M input, $0.28/1M output (95% mai ieftin decât GPT-4o)
- Best practice: Ideal pentru volume >100 outputs/zi

**Security obligatoriu:**
- Toate API keys în password manager (1Password/Bitwarden) sau `.env` files (niciodată hardcodate)
- Separate API keys per project/client (tracking cost granular)
- Monthly spending alerts pe toate platformele
- Niciodată date confidențiale client (PII, financiare) în modele publice fără agreement DPA

### Pas 4: Interface — Configurează interfețele de lucru per use case

**Trei moduri de acces, choose per context:**

**1. UI Direct (ad-hoc, 1-off tasks):**
- chat.openai.com → task-uri creative individuale
- claude.ai → analiză, long-form, editing
- gemini.google.com → research cu surse live
- Avantaj: zero setup, ideal pentru echipă non-tehnică
- Dezavantaj: no automation, no tracking, manual copy-paste

**2. Agregator Unificat (echipă, multiple modele zilnic):**
- **Poe Pro** ($20/mo) — acces la GPT-4o, Claude, Gemini, DeepSeek din aceeași interfață
- **OpenRouter** (pay-per-use) — API gateway unificat, ideal pentru developers
- **TypingMind** ($39 one-time) — custom UI pentru API keys proprii, prompt library, multi-model
- Avantaj: switch rapid între modele, o singură interfață
- Dezavantaj: cost adițional, depinde de disponibilitatea provider-ului

**3. API via Automation (procese recurente, scale):**
- **Make.com** ($9/mo) — workflow visual, HTTP modules pentru orice API
- **Zapier** ($20/mo) — triggers + actions, built-in ChatGPT module
- **n8n** (self-hosted gratuit) — open-source, cel mai flexibil, necesită setup tehnic
- Avantaj: fully automated, scalabil, cost-per-output tracking
- Dezavantaj: necesită setup tehnic inițial

**Exemplu Make.com workflow — Multi-model content pipeline:**
```
Trigger: New row in Google Sheet (content brief)
→ Module 1: HTTP Request → Gemini API (research topic, extract key facts)
→ Module 2: HTTP Request → ChatGPT API (generate outline from research)
→ Module 3: HTTP Request → Claude API (write full draft section by section)
→ Module 4: HTTP Request → DeepSeek API (generate meta tags + social posts)
→ Module 5: Google Docs → Create document with all outputs
→ Module 6: Slack → Notify team with doc link
```
Cost per article: ~$0.15-0.30 API vs. $20+/articol cu un singur model premium.

### Pas 5: Template — Creează prompt templates optimizate per model

Fiecare model răspunde diferit la aceeași instrucțiune. Template-uri optimizate per model:

**ChatGPT GPT-4o — Ad Copy (persuasion-optimized):**
```
Acționezi ca un direct response copywriter cu 15 ani experiență în [industrie].

PRODUS: [nume + descriere 1 paragraf]
AUDIENȚĂ: [ICP detaliat — demographics, psychographics, pain points]
OFERTĂ: [ce primesc, preț, bonus-uri]
PLATFORMĂ: [Meta/TikTok/Google]
OBIECTIV: [conversii/lead gen/awareness]

Generează 5 variante de ad copy, fiecare cu:
- Hook (prima linie, max 10 cuvinte, scroll-stopping)
- Body (50-120 cuvinte, framework PAS)
- CTA (direct, specific, urgent)

Variația 1: Pain-point hook
Variația 2: Curiosity hook
Variația 3: Social proof hook
Variația 4: Contrarian hook
Variația 5: Benefit-first hook

Nu folosi: superlative goale, cliché-uri de marketing, claim-uri neverificabile.
Ton: [conversațional/autoritar/empatic] — ca și cum vorbești cu un prieten care are [problema].
```

**Claude Sonnet — Long-form Blog Post (depth-optimized):**
```
Ești un content strategist specializat în [industrie]. Scrii pentru [publicație/brand] care are vocea: [3 adjective + exemplu de propoziție în vocea brandului].

TASK: Scrie secțiunea „[H2 title]" dintr-un articol despre [topic general].
KEYWORD principal: [KW] (include natural, 1-2 ori)
CONTEXT ANTERIOR: [rezumat secțiune precedentă, 2-3 propoziții]

Cerințe:
- 300-500 cuvinte pentru această secțiune
- Include cel puțin 1 exemplu concret sau case study (real sau realistic)
- Include 1 statistică relevantă cu sursa citată
- Evită: „în lumea de azi", „este important de menționat", „în concluzie"
- Scrie la persoana a 2-a (tu/dumneavoastră) când te adresezi cititorului
- Tonul: [educational dar accesibil / autoritar dar empatic / casual dar informativ]
- Încheie secțiunea cu o tranziție naturală către [subiect secțiune următoare]
```

**DeepSeek — Bulk Social Posts (volume-optimized):**
```
Generează 10 postări pentru [platformă] pe tema [pilon de conținut].

Brand: [nume + domeniu]
Ton: [3 adjective]
Lungime: [50-100 cuvinte per post]

Variază structura: 3 text pur, 2 întrebări de engagement, 2 liste (3-5 puncte), 2 citate/statistici, 1 behind-the-scenes.

Fiecare post include:
- Prima linie = hook de atenție
- CTA: [save/comment/share/click link]
- Hashtags: [5 relevante, nu generice]

Format output: Numerotează 1-10, separator clar între posturi.
```

**Gemini Pro — Research Brief (web-access-optimized):**
```
Caută informații recente (ultimele 6 luni) despre [subiect].

Structurează rezultatele astfel:
1. Top 5 statistici relevante (cu sursă și dată)
2. Tendințe principale identificate (3-5)
3. Competitori/jucători cheie și ce fac diferit
4. Întrebări frecvente ale audienței pe acest subiect (din Google PAA)
5. Surse de autoritate recomandate pentru citare

Nu inventa date. Dacă nu găsești informație recentă, specifică explicit.
Format: bullet points, fiecare cu sursă [URL sau publicație + dată].
```

### Pas 6: Workflow — Integrează în procese de marketing existente cu reguli clare

Creează un ghid de 1 pagină pentru echipă:

```markdown
# Multi-Model Routing Guide — [Organizație]

## Regula de bază
- Scriu copy scurt persuasiv → ChatGPT
- Scriu text lung analitic → Claude
- Am nevoie de date actuale → Gemini
- Am volume mari de texte simple → DeepSeek
- Nu sunt sigur → începe cu Claude (cel mai versatil)

## Reguli de calitate cross-model
1. ORICE output AI trece prin review uman înainte de publicare
2. Brand voice doc [link] = referința de ton — verifică orice text generat
3. Statistici/cifre din AI = NEVERIFICATE până la confirmare manuală
4. Niciodată date client (PII) în AI fără aprobare compliance
5. Salvează prompturile care funcționează în [Notion/doc partajat] — nu reinventa

## Cost tracking
- Fiecare API call loggat automat (via Make.com webhook)
- Budget alert: [email/Slack] la 80% din cap lunar
- Review cost lunar în prima zi a lunii — ajustează routing dacă necesar
```

### Pas 7: Monitor — Dashboard de cost și calitate per model

Setează monitorizare lunară:

**Cost tracking:**
- OpenAI: platform.openai.com → Usage → export CSV
- Anthropic: console.anthropic.com → Usage
- Google: aistudio.google.com → Usage
- DeepSeek: platform.deepseek.com → Usage

**Dashboard simplu (Google Sheet):**
| Luna | Model | Tasks | Tokens utilizați | Cost | Cost/output | Calitate medie |
|------|-------|-------|-------------------|------|-------------|----------------|
| Mar 2026 | GPT-4o | 150 | 2.5M | $27 | $0.18 | 4.2/5 |
| Mar 2026 | Claude | 80 | 3.1M | $52 | $0.65 | 4.5/5 |
| Mar 2026 | Gemini | 40 | 1.2M | $8 | $0.20 | 3.8/5 |
| Mar 2026 | DeepSeek | 300 | 5.0M | $2 | $0.01 | 3.5/5 |

**Alertele de acțiune:**
- Cost/output crește >20% MoM → investigă (modele mai mari decât necesarul?)
- Calitate medie scade sub 3.5 → re-evaluează prompt template sau model
- Un model acoperă >60% din tasks → verifică dacă routing-ul funcționează

### Pas 8: Optimize — A/B test modele pe aceleași task-uri trimestrial

La fiecare 3 luni, rulează un mini-benchmark intern:
1. Selectează 3 task-uri frecvente (ex: ad copy, blog intro, product description)
2. Rulează exact același prompt pe toate modelele active
3. Evaluează output-urile blind (fără a ști ce model a generat ce)
4. Scorează pe: calitate text, acuratețe, brand voice match, creativitate
5. Actualizează routing table cu rezultatele

Modelele evoluează rapid (GPT-5 poate depăși Claude pe long-form, DeepSeek v3 poate egal GPT-4o pe persuasiune). Routing-ul static devine suboptimal în 3-6 luni.

### Pas 9: Agency Scale — Routing multi-client cu cost separation

Pentru agenții cu 5+ clienți:
- **Separate API keys per client** → cost tracking granular
- **Client-specific prompt templates** cu brand voice embedded
- **Tiered routing per client budget**:
  - Client premium ($2,000+/mo retainer) → ChatGPT + Claude primary
  - Client mid ($1,000-2,000/mo) → Claude + DeepSeek primary
  - Client basic (<$1,000/mo) → DeepSeek primary + ChatGPT pentru copy critic
- **Margin calculation**: costul AI per client = 3-8% din retainer (target max 10%)
- **Monthly client report** include: „AI-assisted deliverables: [X] piese, cost eficiency: [Y]% sub manual equivalent"

---

## 3. Cortex Logging

```json
{
  "text": "Multi-Model Marketing Stack configurat pentru [organizație]. Modele active: ChatGPT/Claude/Gemini/DeepSeek. Task-uri routed: [X]. Cost estimat lunar: [€]. Cost per output mediu: [$X.XX]. Interfață: [UI/API/Agregator].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-102",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["multi-model", "chatgpt", "claude", "gemini", "deepseek", "ai-stack", "routing", "cost-optimization"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + setup inițial stack AI marketing

### WHEN
La configurarea oricărui workflow AI marketing multi-model

### HOW (violation detection)
- Toate task-urile routed pe un singur model → violation (mono-model anti-pattern)
- API keys stocate în plain text sau hardcodate → violation critică
- Nicio monitorizare de cost setată → violation
- Templates lipsă pentru top 10 task-uri recurente → violation
- Cost per output netracked → violation
- Routing table neactualizat >3 luni → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-101 → tool selection (prerequisite)
- AIM-103 → content pipeline (downstream, folosește routing-ul)

### VERIFY
- [ ] Routing table documentat per task cu model primar + fallback?
- [ ] Toate API keys stocate securizat (password manager / env)?
- [ ] Templates create pentru top 10 task-uri recurente?
- [ ] Cost monitoring activ cu alerts?
- [ ] A/B test trimestrial programat?
- [ ] Ghid routing partajat echipei?

**VK-uri:**
1. `✅ [PROC] AIM-102 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Multi-Model Marketing Stack Setup" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Setup și configurare | Sonnet | Precizie, step-by-step |
| Decizie routing arhitectură | Opus | Trade-offs cost/calitate complexe |
| Verificare prețuri API | Gemini Pro | Web access, date actuale |
| Generare templates bulk | DeepSeek | Cost-efficient, volume |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| OpenAI API (platform.openai.com) | ChatGPT GPT-4o access |
| Anthropic API (console.anthropic.com) | Claude Sonnet/Opus access |
| Google AI Studio (aistudio.google.com) | Gemini Pro/Flash access |
| DeepSeek API (platform.deepseek.com) | DeepSeek access |
| Make.com / n8n / Zapier | Automatizare workflow multi-model |
| Notion / Google Docs | Stocare templates și routing guide |
| 1Password / Bitwarden | Stocare securizată API keys |
| Google Sheets | Cost tracking dashboard |

---

## 6. Instrumente

- ChatGPT Plus ($20/mo) — persuasion, copy, ideation rapid
- Claude Pro ($20/mo) — long-form, analiză, QA, brand voice
- Gemini Advanced ($20/mo) — research live, Google ecosystem
- DeepSeek (free/API $0.14-0.28/1M) — volume tasks, cost optimization
- OpenRouter / Poe Pro ($20/mo) — unified access opțional
- Make.com ($9/mo) — workflow automation multi-model
- TypingMind ($39 one-time) — custom interface cu API keys proprii

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Cost per output AI (medie) | -40% vs. single-model GPT-4o |
| Tasks cu routing documentat | 100% |
| Templates disponibile top tasks | ≥ 10 |
| Monthly cost overrun | 0 |
| A/B test models frequency | Trimestrial |
| Satisfacție echipă cu routing | ≥ 4.0/5 |
| Cost AI ca % din retainer (agenție) | ≤ 10% |
