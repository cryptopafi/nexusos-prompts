---
type: procedure
created: 2026-03-20
status: active
slug: aim-103-ai-content-marketing-pipeline
tags: [procedure, nexus]
---

# AI Content Marketing Pipeline (Brief to Publish) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Execuția end-to-end a unui pipeline de content marketing cu AI, de la brief strategic la publicare și distribuție multicanal, acoperind research, scriere, SEO, editare, design, QA și repurposing. Include prompt templates per etapă, workflow automation cu Make.com/n8n, și quality gates anti-hallucination.

---

## 1. Problema

Content marketingul fără AI produce 2-4 piese de conținut/săptămână per persoană, cu 60% din timp pe research și scriere, 20% pe editare, și doar 20% pe strategie și distribuție. Cu un pipeline AI bine configurat, același output crește la 10-20 piese/săptămână menținând calitatea, iar mixul de timp se inversează: 20% producție, 30% editare/QA, 50% strategie și distribuție. Fără proces, AI produce conținut generic, neoptimizat SEO, fără brand voice, cu claim-uri hallucinate.

Situații acoperite:
- Blog posts și articole long-form (1,500-3,000 cuvinte)
- Pillar pages și content clusters SEO
- Conținut educational (ghiduri, how-to, liste, comparații)
- Repurposing articol existent în 5-8 formate diferite
- Content as a Service pentru clienți de agenție

---

## 2. Procedura

### Pas 1: Brief — Construiește brieful de conținut structurat (template obligatoriu)

Completează acest template înainte de orice generare AI — fără brief complet, pipeline-ul nu pornește:

```markdown
# Content Brief — [Titlu working]

## SEO
- Keyword principal: [exact match]
- Secondary keywords (3-5): [lista]
- Intenție de căutare: [informațională / navigațională / comercială / tranzacțională]
- Search volume estimat: [luna, sursa]
- Difficulty score: [0-100, sursa]

## Audiență
- Persona: [nume persona — ex: „Marketing Manager Maria, 32 ani, SaaS B2B"]
- Nivel cunoaștere subiect: [începător / intermediar / avansat]
- Pain point principal: [1 propoziție]
- Ce caută concret: [ce întrebare vrea să rezolve]

## Obiectiv & Format
- Obiectivul piesei: [awareness / lead gen / SEO ranking / nurture / thought leadership]
- Format: [listicle / how-to / ghid complet / comparație / opinie / pillar page]
- Lungime target: [cuvinte]
- CTA principal: [ce acțiune vrei de la cititor]

## Brand Voice
- Ton: [3 adjective — ex: „autoritar dar accesibil, data-driven"]
- Stil: [persoana a 2-a / a 3-a, formal / informal]
- Evită: [cuvinte sau fraze specifice de evitat]

## Delivery
- Deadline: [data]
- Owner: [cine editează și aprobă]
- Canale distribuție: [blog, email, social, syndication]
```

### Pas 2: Research — Cercetează topicul cu AI multi-source (nu doar SERP)

**Pasul 2A — SERP Analysis** (5-10 minute):
Analizează primele 5-10 rezultate Google pentru keyword-ul principal.

Prompt pentru **Gemini Pro** (web access nativ):
```
Caută pe Google „[keyword principal]" și analizează primele 7 rezultate organice.
Pentru fiecare rezultat, extrage:
1. Titlul și lungimea estimată (cuvinte)
2. Subtopicele acoperite (H2-uri principale)
3. Format (listicle/how-to/ghid/comparație)
4. Statistici sau date citate
5. Ce face bine acest articol
6. Ce lipsește (ce nu acoperă)

La final, sintetizează:
- Subtopice acoperite de toți (must-have)
- Subtopice acoperite de mai puțin de 3 (content gap = oportunitate)
- Întrebări din "People Also Ask" pe acest keyword
- Lungimea medie a articolelor care rankează
```

**Pasul 2B — Expert Research** (10-15 minute):
Prompt pentru **Claude Sonnet**:
```
Ești un expert în [domeniu]. Topicul: [subiect articol].

Oferă:
1. 5 statistici recente (2025-2026) relevante pe acest subiect, cu sursa exactă
2. 3 studii de caz sau exemple reale care ilustrează impactul [subiectului]
3. 2 perspective contrarian sau nuanțe pe care majoritatea articolelor le omit
4. 3 citate de la experți recunoscuți pe acest subiect
5. Ce greșeală frecventă fac oamenii pe acest subiect (pentru secțiunea „de evitat")

IMPORTANT: Dacă nu ai date concrete recente, specifică explicit „necesită verificare".
Nu inventa statistici sau studii de caz.
```

**Pasul 2C — Hallucination Check** (5 minute):
Orice statistică, studiu sau citat generat de AI → verifică sursa manual (Google, site sursa). Budget de 5 minute per articol pentru verificare. Statistici neverificate se marchează cu [NEEDS SOURCE] în draft.

### Pas 3: Outline — Generează structura optimizată SEO + engagement

Prompt pentru **Claude Sonnet** (superior la structurare logică):
```
Pe baza acestui research, creează un outline detaliat:

BRIEF: [copy-paste din Pasul 1]
RESEARCH SERP: [insights cheie din Pasul 2A]
RESEARCH EXPERT: [insights cheie din Pasul 2B]

Generează:
1. Titlu H1 optimizat (include keyword „[KW]" natural, 55-65 caractere)
2. Meta description (150-160 caractere, include keyword, cu CTA implicit)
3. Intro hook (2-3 propoziții care captează atenția + promit valoare)
4. 6-8 secțiuni H2, fiecare cu:
   - Titlul H2 (include secondary keyword unde natural)
   - 3-5 bullet points cu ce se acoperă
   - Date/exemple de inclus din research
   - Sugestie de element vizual (dacă relevant)
5. FAQ section (3-5 întrebări din PAA)
6. Concluzie cu CTA specific: [CTA din brief]

CONTENT GAPS de acoperit: [din Pasul 2A — ce lipsește competitorilor]
Lungime per secțiune: distribui [lungime target] cuvinte proporțional.
```

Revizuiește outline-ul înainte de a trece la scriere. Adaugă, elimină sau reordonează secțiuni.

### Pas 4: Draft — Generează draft-ul secțiune cu secțiune (nu integral)

**Regulă critică**: Nu genera articolul întreg dintr-un singur prompt. Calitatea scade dramatic după 500 cuvinte la orice model. Generează secțiune cu secțiune.

Prompt pentru **Claude Sonnet** (secțiune cu secțiune):
```
Scrie secțiunea „[H2 title]" din articolul nostru despre [topic].

CONTEXT ARTICOL: [rezumat articol 2-3 propoziții]
SECȚIUNEA ANTERIOARĂ s-a încheiat cu: „[ultima propoziție]"
KEYWORD-URI de inclus natural: [1-2 secondary keywords]
TONE OF VOICE: [din brief]
AUDIENȚA: [din brief]

CERINȚE SECȚIUNE:
- [X] cuvinte (±50)
- Include: [date/statistici/exemplu specific din research]
- Format: [paragraf / bullet list / numbered steps / tabel]
- Încheie cu tranziție naturală către „[subiect secțiune următoare]"

EVITĂ: „în lumea de azi", „este important de menționat", „de fapt",
„în concluzie", fraze de umplutură, superlative goale.
```

Repetă pentru fiecare secțiune H2. Concatenează într-un Google Doc / Notion page.

### Pas 5: SEO — Optimizează cu Surfer SEO sau checklist manual

**Cu Surfer SEO ($89/mo Essential)**:
1. Creează Content Editor cu keyword-ul principal
2. Copy-paste draft-ul generat
3. Verifică scorul (target: ≥70/100)
4. Ajustează pe baza recomandărilor Surfer: adaugă NLP terms lipsă, extinde secțiuni sub word count target, verifică heading hierarchy

**Fără Surfer (checklist manual)**:
- [ ] Keyword principal în: H1, primul paragraf, 2-3 H2, meta title, meta description, alt text featured image
- [ ] Keyword density: 1-1.5% (natural, nu forțat)
- [ ] Secondary keywords distribuite în body (fiecare apare cel puțin 1 dată)
- [ ] Internal links: 3-5 spre pagini relevante de pe site
- [ ] External links: 2-3 spre surse autoritare (studii, publicații recunoscute)
- [ ] Meta title: 50-60 caractere, include keyword
- [ ] Meta description: 150-160 caractere, include keyword, CTA implicit
- [ ] URL slug: scurt, include keyword, fără stop words
- [ ] Featured snippet optimization: paragraf concis 40-60 cuvinte imediat după un H2 formulat ca întrebare
- [ ] FAQ schema markup (dacă CMS suportă)

### Pas 6: Edit — Aplică brand voice și editare umană (quality gate obligatoriu)

**AI-Assisted Editing** — Prompt pentru **Claude Sonnet**:
```
Editează acest text aplicând următoarele standarde de calitate:

BRAND VOICE: [3 adjective + exemplu propoziție din brand voice doc]
AUDIENȚA: [persona din brief]

Verifică și corectează:
1. AI-ISMS: elimină frazele generice tipice AI (ex: „o gamă largă de", „nu este un secret că",
   „în peisajul dinamic de astăzi", „utilizând", „leveraging")
2. REDUNDANȚĂ: comprimă propoziții care spun același lucru în moduri diferite
3. SUPERLATIVE GOALE: elimină „cel mai bun", „incredibil", „amazing" fără dovezi
4. FLOW: asigură tranziții naturale între paragrafe
5. ACTIVE VOICE: rescrie pasivul în activ unde posibil
6. READABILITY: propoziții max 20-25 cuvinte, paragrafe max 3-4 propoziții
7. SPECIFICITY: înlocuiește „mulți", „semnificativ", „recent" cu cifre concrete

Marchează cu [HUMAN REVIEW] orice claim factual care necesită verificare.
```

**Editare umană obligatorie** (15-30 minute per articol):
1. Citește articolul integral — sună ca un om sau ca un robot?
2. Adaugă experiență personală, anecdote, opinii proprii — AI nu le poate genera
3. Verifică fiecare statistică și citat marcat [NEEDS SOURCE]
4. Verifică CTA — este clar, specific, și contextual?
5. Citește intro separat — în 30 secunde, știi de ce să citești tot articolul?

**Nicio piesă de conținut nu merge la publicare fără editare umană.** Aceasta este regula ne-negociabilă a pipeline-ului.

### Pas 7: Design — Creează elemente vizuale cu AI

**Featured Image:**
- **Midjourney** ($30/mo Standard) pentru stil artistic/editorial: `/imagine [describe scene] --ar 16:9 --style raw --v 6`
- **DALL-E 3** (via ChatGPT Plus, inclus) pentru iterații rapide: describe scena, iterează în chat
- **Canva AI** ($13/mo) pentru templates de brand consistente: Magic Design → brand kit → generate

**In-Article Visuals:**
- Infografice: **Napkin.ai** (free) — paste text, generează diagram automat
- Diagrame și flowcharts: **Whimsical AI** sau **Mermaid** (code-to-diagram)
- Screenshots annotate: **CleanShot** sau **Markup Hero**
- Custom illustrations: **Midjourney** cu stil consistent (seed + style reference)

**Standarde tehnice:**
- Format: WebP sau compressed JPEG (nu PNG pentru fotografii)
- Dimensiune: <200KB per imagine (TinyPNG/Squoosh)
- Alt text: descriptiv, include keyword unde natural (nu keyword stuffing)
- Naming: `keyword-descriptive-name.webp` (nu `image1.jpg`)

### Pas 8: Publish — Configurează în CMS cu SEO tehnic complet

Checklist de publicare:
- [ ] Title tag setat (diferit de H1 dacă necesar)
- [ ] Meta description setată
- [ ] URL slug clean
- [ ] Featured image + alt text
- [ ] Heading hierarchy corectă (1x H1, H2 pentru secțiuni, H3 pentru subsecțiuni)
- [ ] Internal links funcționale (nu broken)
- [ ] External links cu `target="_blank"` și `rel="noopener"`
- [ ] Schema markup (Article, FAQ, HowTo — ce e relevant)
- [ ] Open Graph tags (og:title, og:description, og:image) pentru social sharing
- [ ] Canonical URL setat (evită duplicate content)
- [ ] Mobile preview verificat
- [ ] Page speed check (imagini compresate, nu lazy load above-the-fold)

### Pas 9: Distribute — Repurposing și distribuție multicanal (24-72 ore window)

Din fiecare articol, extrage minim 5 piese de conținut derivat:

Prompt pentru **ChatGPT GPT-4o** (repurposing):
```
Din acest articol, creează:

1. LINKEDIN POST (200-300 cuvinte):
   - Hook pe prima linie (controversat sau insightful)
   - 3-5 key takeaways ca bullet points
   - Ending cu întrebare de engagement
   - Fără hashtags excesive (max 3)

2. TWITTER/X THREAD (5-7 tweets):
   - Tweet 1: hook + promisiune de valoare
   - Tweets 2-6: câte 1 insight per tweet, standalone
   - Tweet 7: CTA + link articol

3. INSTAGRAM CAROUSEL COPY (8-10 slides):
   - Slide 1: titlu hook (text over image)
   - Slides 2-9: 1 idee per slide, text concis
   - Slide 10: CTA (save, share, follow)
   - Caption: 150-200 cuvinte + 10 hashtags relevante

4. EMAIL NEWSLETTER TEASER (100-150 cuvinte):
   - Subject line (3 variante A/B)
   - Preview text (40-90 caractere)
   - Teaser body cu hook + 1 insight + CTA la articol

5. SHORT VIDEO SCRIPT (30-60 secunde):
   - Hook (3 secunde)
   - Top 3 takeaways condensate
   - CTA: „articolul complet în link-ul din bio"

Ton consistent cu articolul sursă: [tone from brief].
Brand: [brand name].
```

**Scheduling timeline:**
- Ziua 0: Publică articolul pe blog
- Ziua 0 (+2h): LinkedIn post
- Ziua 1: Email newsletter cu teaser
- Ziua 1-2: Twitter thread
- Ziua 2-3: Instagram carousel
- Ziua 3-5: Short video (filmează sau AI avatar)
- Ziua 7: Reshare pe LinkedIn cu nou unghi

### Pas 10: Automate — Workflow automation cu Make.com/n8n

**Make.com Content Pipeline Automation:**
```
Scenario: AI Content Pipeline

Trigger: Google Sheet new row (content brief completed)
→ Module 1: Gemini API → research SERP (Pasul 2A)
→ Module 2: Claude API → generate outline (Pasul 3)
→ Module 3: Claude API → generate draft section by section (Pasul 4) [iterator]
→ Module 4: Claude API → AI editing pass (Pasul 6)
→ Module 5: Google Docs → create document with full draft
→ Module 6: DeepSeek API → generate meta tags + social posts (Pasul 9)
→ Module 7: Google Sheet → update row status to „Draft Ready"
→ Module 8: Slack → notify editor with Google Doc link

Estimated cost per article: $0.20-0.50 API + $9/mo Make.com
Manual time reduced from 6h to 1.5h (editing + publishing only)
```

**n8n alternative** (self-hosted, gratuit):
Același flow, dar cu HTTP Request nodes + Code nodes pentru transformări.

---

## 3. Cortex Logging

```json
{
  "text": "Content Marketing Pipeline executat. Topic: [X]. Keyword: [KW]. Format: [blog/pillar/how-to]. SEO score: [X/100]. Canale distribuție: [lista]. Piese derivate: [Y]. Timp total: [ore]. Cost AI: [$X.XX].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-103",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["content-marketing", "blog", "seo", "pipeline", "publishing", "repurposing", "automation"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + orice producție de conținut long-form

### WHEN
La fiecare piesă de conținut publicabilă generată cu AI

### HOW (violation detection)
- Brief incomplet înainte de generare → violation
- Draft generat integral (nu secțiune cu secțiune) → violation (quality risk)
- Nicio editare umană înainte de publicare → violation critică
- SEO check sărit (Surfer sau checklist manual) → violation
- Statistici necitate sau neverificate → violation (hallucination risk)
- Distribuție singlecanal (doar blog, fără repurposing) → violation
- Zero piese derivate din articol → violation

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-102 → multi-model routing (care model pentru ce pas)
- AIM-108 → SEO optimization detailed (complement Pasul 5)
- AIM-105 → social posts din content repurposing (Pasul 9)

### VERIFY
- [ ] Brief template completat integral?
- [ ] Research multi-source realizat (SERP + expert)?
- [ ] Hallucination check pe statistici/citate?
- [ ] Outline revizuit înainte de generare draft?
- [ ] Draft generat secțiune cu secțiune?
- [ ] SEO score ≥70 sau checklist manual completat?
- [ ] Editare umană aplicată (minim 15 min)?
- [ ] Vizualuri compresate (<200KB) cu alt text?
- [ ] Minim 5 piese derivate pentru distribuție?
- [ ] Scheduled pe calendar editorial cu timeline?

**VK-uri:**
1. `✅ [PROC] AIM-103 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Content Marketing Pipeline" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motiv |
|-----------|-------|-------|
| Research SERP live | Gemini Pro | Web access nativ |
| Research expert depth | Claude Sonnet | Nuanță, mai puțin hallucination |
| Outline structurare | Claude Sonnet | Logică și coerență |
| Scriere draft secțiuni | Claude Sonnet | Long-form quality, brand voice |
| AI editing pass | Claude Sonnet | Detectare AI-isms |
| Repurposing social | ChatGPT GPT-4o | Creativitate, hooks, adaptare ton |
| Bulk meta tags | DeepSeek | Cost-efficient, task simplu |
| Featured image | Midjourney / DALL-E 3 | Calitate vizuală |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Claude Sonnet (claude.ai / API) | Draft, outline, editing |
| ChatGPT GPT-4o (chat.openai.com / API) | Repurposing, creative |
| Gemini Pro (gemini.google.com / API) | Research live |
| Surfer SEO ($89/mo) sau checklist manual | SEO optimization |
| Midjourney ($30/mo) / DALL-E 3 (inclus ChatGPT Plus) | Featured image |
| CMS (WordPress/Webflow/Ghost) | Publicare |
| Make.com ($9/mo) / n8n (free) | Workflow automation |
| Google Sheets / Notion | Content calendar și briefs |

---

## 6. Instrumente

- Claude Pro ($20/mo) — draft, outline, editing, QA
- ChatGPT Plus ($20/mo) — repurposing, hooks, creative ideation
- Gemini Pro (free/$20/mo) — research SERP live
- Surfer SEO ($89/mo) — content optimization SERP-based
- Midjourney ($30/mo) — featured images, editorial visuals
- Canva Pro ($13/mo) — templates, social media visuals
- Make.com ($9/mo) — pipeline automation
- Buffer / Hootsuite — scheduling distribuție multicanal
- Napkin.ai (free) — infografice din text

---

## 7. Metrics

| Metrică | Target |
|---------|--------|
| Timp brief → draft ready | ≤ 2 ore (AI) + 1 oră (editare umană) |
| SEO content score | ≥ 70/100 (Surfer) |
| Canale de distribuție per articol | ≥ 4 |
| Piese derivate per articol | ≥ 5 |
| Articole publicate fără editare umană | 0 |
| Cost AI per articol (API) | ≤ $0.50 |
| Hallucination rate (statistici false) | 0% (post-verificare) |
| Organic traffic per articol (90 zile) | ≥ 100 visits |
