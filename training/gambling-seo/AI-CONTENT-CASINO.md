---
type: procedure
created: 2026-03-20
status: active
slug: ai-content-casino
tags: [procedure, nexus]
---

# AI Content Creation for Casino/Gambling SEO (Bypass AI Detection) — Procedura Opus v2

**Status**: ACTIVE
**Creat**: 2026-03-04
**Actualizat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-010
**Scope**: End-to-end workflow for producing AI-generated casino/gambling content that passes AI detection tools, optimized per regional market (Peru, Kenya, Chile), using a multi-stage pipeline: ICP creation, RAG research, Claude humanization, regional voice injection, anti-detection structural engineering, and scaled production with quality gates.

---

## §1 Problema

AI content detection tools (GPTZero, Originality.ai, Copyleaks, Turnitin, Winston AI) now achieve 85-95% accuracy on raw LLM output. Google's Helpful Content System, while not explicitly penalizing AI content, deprioritizes content that lacks demonstrable first-hand expertise, regional specificity, and natural language variance.

In the gambling/casino niche, this creates a compounding problem:
- Casino content is a "Your Money or Your Life" (YMYL) category — held to higher scrutiny by Google's quality raters
- Regional markets (Peru, Kenya, Chile) require hyper-local signals that generic AI content lacks entirely
- Scale requirements (10-20 articles/month per market) make 100% human writing economically unviable at $50-150/article
- Raw GPT/Claude output contains statistical fingerprints (high perplexity uniformity, low burstiness, banned vocabulary clusters, predictable sentence-length distributions) that detection tools exploit
- Gambling regulators in all three target markets increasingly scrutinize online content for compliance
- Competitors using raw AI content get deranked within 60-90 days, creating a "quality moat" for those who humanize properly

**Cost of failure**: A single flagged article can trigger a site-wide Helpful Content demotion that takes 3-6 months to recover from. At scale (60 articles across 3 markets), even a 10% detection rate means 6 articles flagging your entire domain.

---

## §2 Procedura

### Pas 1: ICP Creation per Market (Gemini Pro)

**Obiectiv**: Create a detailed Ideal Customer Profile for the target market before writing a single word. Content without ICP grounding reads generic — and generic reads AI. The ICP grounds every content decision: vocabulary, payment references, trust signals, objection handling.

**Tool**: Gemini Pro (Google AI Studio or API) with web browsing enabled

**Prompt (verbatim — do not modify)**:

```
Create a detailed Ideal Customer Profile for [Peru/Kenya/Chile] online casino players including: age range, income level, preferred payment methods, casino game preferences, mobile vs desktop usage, trust factors, main objections to signing up, and language/cultural considerations. Include specific local brands, apps, and cultural references that this person encounters daily.
```

**Output expected — verify ALL present**:
- Age range + income bracket (in local currency: PEN/KES/CLP)
- Payment methods with adoption rates:
  - Peru: Yape (67% adoption), Plin (23%), PagoEfectivo, BCP transfer, Interbank
  - Kenya: M-Pesa (96% of mobile money), Airtel Money (3%), T-Kash, bank transfer
  - Chile: WebPay (dominant), RedCompra, Khipu, Multicaja, MACH, bank transfer
- Top 3 game categories with local preferences (Peru: slots + sports Liga 1; Kenya: sports EPL/KPL + jackpots; Chile: slots + live dealer + sports La Roja)
- Mobile % vs desktop % (Kenya: 95%+ mobile; Peru: 78%; Chile: 72%)
- Trust signals ranked by impact for this specific market
- Top 3 objections with exact phrasing a local user would use
- Language notes: slang, formality level, regional idioms, words to avoid

**Per-market ICP specifics to verify**:

**Peru ICP**:
- Mention Yape and Plin by name (not generic "e-wallets")
- Reference Liga 1, Alianza Lima, Universitario de Deportes
- Currency in soles (PEN/S/.)
- MINCETUR as trust authority
- Tone: informal-friendly, tuteo preferred over usted in gambling context

**Kenya ICP**:
- M-Pesa must appear as primary payment (96%+ adoption)
- Reference EPL teams (Arsenal, Man City, Chelsea popular in Kenya), Harambee Stars, KPL
- Currency in KES (Kenyan Shillings), not USD
- BCLB (Betting Control and Licensing Board) as trust authority
- Terms: "punter" not "gambler", "bet" not "wager", "jackpot" universal
- Mobile-first writing mandatory (95%+ mobile users)

**Chile ICP**:
- WebPay as primary payment gateway
- Reference La Roja, Colo Colo vs Universidad de Chile rivalry, tennis (Jarry, Tabilo)
- Currency in CLP (pesos chilenos)
- Ley 21.456 and SCJ (Superintendencia de Casinos de Juego) as regulatory framework
- Casino de Vina del Mar as cultural casino reference
- Tone: formal-casual, chilenismos with moderation ("al tiro", "bacan")

**Checklist Pas 1**:
- [ ] ICP generated for correct target market
- [ ] Payment methods are LOCAL and SPECIFIC (not generic "e-wallets" or "mobile payments")
- [ ] Trust factors + objections captured in local user language
- [ ] Cultural references verified (sports, apps, brands)
- [ ] Saved as `icp-[country]-[date].md` in `~/.nexus/research/gambling-seo/`

---

### Pas 2: RAG Research Dataset (Gemini Pro with Web Browsing)

**Obiectiv**: Build a factual, verified dataset with current 2026 data about the target market. Injecting this dataset into the writing prompt is what makes content appear to come from a local expert — not from an LLM hallucinating.

**Tool**: Gemini Pro with web browsing enabled (fallback: Perplexity Pro)

**Prompt (verbatim — do not modify)**:

```
Build a factual research dataset about the [country] online gambling market including: legal status and regulatory authority, licensed operators with their current bonus offers (exact amounts in local currency), popular games by category with percentage popularity, payment methods used with adoption rates, average player demographics, competitor bonus offers with wagering requirements, local sports betting culture including popular leagues and teams, and any regulatory changes in 2025-2026. Use web browsing to get current data. No hallucinations — mark uncertain data as [UNVERIFIED].
```

**Output expected — verify ALL sections present**:
- Legal framework: licensing authority, current status (legal/grey/offshore), recent changes
- Top 5 operators with EXACT current bonus amounts in local currency
- Top games by category with % popularity where available
- Payment methods with adoption rates (not just names)
- Competitor bonus benchmark: welcome bonus, free spins, wagering requirements typical in market
- Sports betting culture: top sports, local leagues, peak traffic periods
- Regulatory timeline: any pending legislation or recent enforcement actions

**Critical verification steps**:
- Cross-check 3-5 key facts manually against operator websites
- Verify bonus amounts are CURRENT (operators change these frequently)
- Confirm regulatory status is 2026-current (not outdated 2023-2024 data)
- Flag any data Gemini marks as uncertain with [UNVERIFIED] tag
- If Gemini lacks web access in current session, use Perplexity Pro with identical query

**Market-specific RAG requirements**:

**Peru RAG**:
- MINCETUR licensing list (verify against mincetur.gob.pe)
- Current Inkabet, Betsson Peru, Codere Peru bonus amounts in soles
- Liga 1 2026 season status and major upcoming fixtures
- Yape/Plin transaction limits for gambling deposits

**Kenya RAG**:
- BCLB licensed operators list (verify against bclb.go.ke)
- Current Betway Kenya, Odibets, SportPesa (if returned) bonus amounts in KES
- M-Pesa deposit limits and any Safaricom restrictions on gambling transactions
- EPL 2025-2026 season key matches, KPL status

**Chile RAG**:
- Ley 21.456 current enforcement status
- SCJ licensed operators (verify against scj.gob.cl)
- Current Betsson Chile, Codere Chile bonus amounts in CLP
- WebPay integration status with online casinos
- Any pending regulatory tightening (expected 2026-2027)

**Checklist Pas 2**:
- [ ] Dataset generated with active web browsing (not from LLM memory)
- [ ] Legal status correct and dated 2026
- [ ] Payment methods LOCAL with specificity and adoption rates
- [ ] Competitor bonus data included with EXACT amounts in local currency
- [ ] Hallucinations verified manually on minimum 5 facts
- [ ] Regulatory changes noted with dates
- [ ] Saved as `rag-[country]-[keyword]-[date].md`

---

### Pas 3: Content Draft + Humanization Pipeline (Claude)

**Obiectiv**: Produce article draft using ICP + RAG as context, then apply multi-stage humanization that strips AI markers, injects regional voice, and produces text with natural burstiness.

**3a. Draft Creation**

Write or generate a base draft using ICP + RAG dataset as context. Structure:
- H1: Primary keyword (include year)
- Intro: Pain point or question specific to ICP (NOT "In today's digital landscape")
- H2 sections: 5-8 thematic sections covering the keyword cluster
- Comparison table: operators with bonus, payment methods, rating
- Payment method section: step-by-step local payment guide
- Legal/regulatory section: trust-building
- FAQ: 5-7 questions targeting long-tail queries
- CTA: naturally integrated (not salesy closing)
- Word count target: 1,800-3,500 words depending on intent and SERP average

**3b. Humanization Prompt (verbatim — do not modify any word)**:

```
I have created a piece of content on "[KEYWORD HERE]" for our company, I need you to make this content more engaging for a user, no fluff, remove cringe, increase the humour levels where appropriate but not throughout the content, increase explanations where relevant based on your most up to date understanding of the parent and primary topics.

Focus on the emotion, vocabulary, tone and optimization of the article, break standard English rules via idioms, slang and even a regional tone if necessary, remove overly technical terms or jargon. Improve the article for a user to engage and buy, increase active tone, add personal touch, personal stories, specific use cases and experience whilst still maintaining standards related to the Google algorithm, helpful content system and product reviews system.

Focus on sentence structure that makes it easy to semantically connect entities, then focus on choosing the most human entity and smooth transitions between topics, sentences and paragraphs.

Write for and as a human, use natural, conversational dialogue and use stemming, lemmatization, n-grams and zipf's law.
```

**3c. Second-pass Regional Voice Injection**

After humanization, apply a second pass specifically for regional voice:

```
Adapt this content for a [Peruvian/Kenyan/Chilean] audience. Add 3-5 local cultural references (sports teams, local apps, common expressions). Replace any generic payment references with specific local methods ([Yape/Plin | M-Pesa | WebPay]). Ensure currency amounts are in [soles/KES/pesos chilenos]. Keep the conversational tone but match [informal Peruvian Spanish / Kenyan English / Chilean Spanish with moderate chilenismos].
```

**3d. Verification**:
- Output must sound human, not corporate
- Regional tone present: idioms, local payment methods, cultural references
- AI detection check: Originality.ai or Winston AI — target: sub 15% AI score
- If score > 15%: identify flagged paragraphs, rewrite manually or with targeted re-humanization

**Checklist Pas 3**:
- [ ] Draft created with ICP + RAG as context (not without them)
- [ ] Keyword replaced in humanization prompt `[KEYWORD HERE]`
- [ ] Humanization prompt applied verbatim — zero modifications
- [ ] Regional voice injection applied as second pass
- [ ] Output verified — sounds like a local expert, not a corporate report
- [ ] Regional tone present (idioms, payment methods, cultural references)
- [ ] AI detection score checked and documented

---

### Pas 4: AI Detection Bypass — Complete Banned Words Protocol

**Obiectiv**: Completely eliminate words that trigger detection tools. These are statistically overrepresented in LLM output vs human writing. This is not optional — a single cluster of banned words can flag an entire article.

**ATENȚIE**: Complete elimination — not replacement with similar AI synonyms. Rewrite the entire phrase.

**Banned words list (eliminate completely)**:

```
landscape, realm, navigating, tailored, unveil, transformative, encompass,
dynamic, ecosystem, confluence, facilitate, engaging, quest, solutions,
delving, significant, specific, numerous, craft, glean, harness, intricacies,
deep, grappling, journey, by storm, hype, extensive, drawback, ultimately,
diving, crucial, meticulous, bespoke, ever-changing, robust, seamless,
groundbreaking, paradigm, synergy, holistic, cutting-edge, innovative,
comprehensive, streamline, empower, foster, pivotal, elevate, revolutionize,
multifaceted, nuanced, underscore, paramount, noteworthy, testament
```

**Banned phrases (eliminate completely)**:

```
in today's digital landscape
imagine this
it's about
firstly, secondly, thirdly (and any numbered sequence variant)
it's worth noting that
it's important to note
in the realm of
when it comes to
in today's world
rest assured
at the end of the day
without further ado
let's dive in
buckle up
game-changer
a testament to
the world of
```

**Banned word families (ALL forms)**:

```
dive / diving / dived / deep-dive
delve / delving / delved
leverage / leveraging / leveraged
unlock / unlocking / unlocked
unveil / unveiling / unveiled
navigate / navigating / navigated
craft / crafting / crafted (when used as verb for content/experience)
harness / harnessing / harnessed
streamline / streamlining / streamlined
empower / empowering / empowered
```

**Verification procedure**:
1. Paste article in text editor
2. Ctrl+F / Cmd+F every word from the lists above
3. Zero hits = passed | Any hit = rewrite the entire phrase (not just swap the word)
4. Run through Originality.ai (or Winston AI) — target: sub 15% AI score
5. Run through GPTZero as secondary check — target: "Likely Human" classification
6. Document scores in production tracker

**Checklist Bypass**:
- [ ] All words from banned list verified manually with Ctrl+F
- [ ] Zero occurrences confirmed
- [ ] Banned phrases verified (not just individual words)
- [ ] Word families all variants verified
- [ ] Originality.ai / Winston AI score sub 15%
- [ ] GPTZero secondary check: "Likely Human"

---

### Pas 5: Structural Bypass Engineering

Beyond banned words, apply these structural tactics that reduce AI score by simulating human writing patterns:

**5a. Burstiness (Sentence Length Variation)**
- Humans alternate between short and long sentences irregularly
- LLMs produce sentences with uniform average length (statistical signature detectable at 85%+ accuracy)
- Deliberately add 4-6 sentences of 4-8 words mixed with sentences of 25-40 words
- Pattern: long paragraph → sudden short punch → medium → long → one-word fragment
- Example: "That's it. No tricks." after a long paragraph = strong human signal
- Measurement: standard deviation of sentence lengths should be >8 words (LLMs typically produce SD of 3-5)

**5b. Grammar/Punctuation Micro-Errors (3-4 per article)**
- Missing comma in a place where it is grammatically correct but naturally omittable
- Deliberate sentence fragment used as stylistic effect
- Dash instead of comma in unexpected places
- Parenthetical aside that slightly breaks the flow
- One instance of starting a sentence with "And" or "But"
- **IMPORTANT**: 3-4 micro-errors per article maximum. Not obvious spelling mistakes — subtle structural choices that AI systems never make.

**5c. Structural Variance**
- Eliminate unnecessary hyphens connecting words (ex: "ever-changing" → "always changing")
- Use contractions inconsistently (sometimes "it's", sometimes "it is" — not uniformly one way)
- Add a rhetorical question mid-article that doesn't directly answer itself
- One single-sentence paragraph somewhere in the middle (not at the end)
- Vary paragraph lengths: 1 sentence, 3 sentences, 5 sentences, 2 sentences (not uniform 3-4)
- Include one parenthetical aside or em-dash digression

**5d. Zipf's Law Application**
- High-frequency words (the, a, is, for, to) must dominate word distribution
- LLMs overuse mid-frequency "fancy" words (transformative, seamless, robust, pivotal)
- Post-humanization: verify article doesn't have clustering of mid-frequency words
- Tool: paste into a word frequency counter — top 20 words should be function words, not content words
- If content words appear in top 20, replace some instances with simpler alternatives

**5e. Reading Level Calibration**
- Target: US Grade 8-10 with adult tone (Flesch-Kincaid)
- Tool check: Hemingway App — target: Grade 8-10, zero "very hard to read" sentences
- Sentences over 30 words without punctuation = split or simplify
- For Spanish content: use equivalent readability tools (Legible.es or similar)
- For Kenya content: slightly higher reading level acceptable (Grade 9-11) due to formal English tradition

**5f. Entity Density and Semantic Signals**
- Include at least 5 named entities per 500 words (brand names, place names, regulatory bodies, payment apps)
- Named entities are a strong human-writing signal — AI tends to use generic descriptions
- Example: "Deposit with Yape in under 30 seconds" vs "Use a mobile payment method to fund your account"

**Checklist Structural Bypass**:
- [ ] Sentence length variation verified visually (irregular mix of short/long)
- [ ] Sentence length SD > 8 words (calculated or estimated)
- [ ] 3-4 micro-errors deliberately added
- [ ] Unnecessary hyphens eliminated
- [ ] Contractions inconsistent (not uniform one way)
- [ ] One rhetorical question mid-article
- [ ] Paragraph lengths varied (not uniform)
- [ ] Hemingway App check: Grade 8-10
- [ ] Named entity density: 5+ per 500 words

---

### Pas 6: Regional Tone Adaptation — Deep Localization

**Peru**
- Language: Spanish (not neutral Latin American — Peruvian register)
- Formality: informal-friendly, tuteo in gambling context
- Idioms to integrate: expressions tied to football (Liga 1, Alianza Lima, Universitario)
- Payment methods to mention naturally: Yape (mention in first 200 words), Plin, BCP transfer, Interbank, PagoEfectivo
- Cultural signals: preference for Liga 1 matches + Copa Libertadores, ceviche culture references in food analogies
- Trust factor: MINCETUR license (Ministerio de Comercio Exterior y Turismo)
- Tone: informal-friendly, not corporate
- Currency: always in soles (PEN / S/.) — never USD unless comparing
- Avoid: Mexican slang ("guey", "chido"), Argentine slang ("che", "boludo"), Spain Spanish ("vale", "tio")
- Example of good localization: "Deposita con Yape en menos de 30 segundos — asi de facil, pe. Ni siquiera necesitas salir de la app."

**Kenya**
- Language: English (Kenya English with local variation accepted)
- Formality: direct and clear — Kenyans appreciate directness in commercial content
- Idioms: references to Safaricom, M-Pesa as de facto standard, Harambee Stars, rugby Shujaa, KPL
- Payment methods: M-Pesa (DOMINANT — mention in first 100 words, include paybill/till number format), Airtel Money
- Cultural signals: sports betting > casino slots as primary preference, EPL viewership massive
- Trust factor: BCLB license (Betting Control and Licensing Board)
- Tone: direct, not overly formal, not pidgin/Sheng (urban slang — avoid in editorial content)
- Currency: KES (Kenyan Shillings) — "KES 50" not "$0.38"
- Mobile-first writing: short paragraphs (2-3 lines max), scannable headers, bullet points
- Example of good localization: "Top up your M-Pesa and you're set — KES 50 minimum deposit, and your money hits the account before you can even check the score."

**Chile**
- Language: Spanish with Chilean register (chilenismos)
- Formality: more formal than Peru, but with local expressions that signal authenticity
- Idioms: "al tiro" (immediately), "bacan" (cool/great), references to La Roja, Primera Division, Casino de Vina del Mar
- Payment methods: WebPay (dominant), RedCompra, Khipu, Multicaja, MACH
- Cultural signals: football + tennis (Jarry, Tabilo) + casino culture (Vina del Mar casino is cultural landmark)
- Trust factor: SCJ (Superintendencia de Casinos de Juego) + Ley 21.456
- Tone: formal-casual with chilenismos signaling locality
- Currency: CLP (pesos chilenos) — always in local currency
- Grey market awareness: careful phrasing around unlicensed operators ("disponible para jugadores chilenos" not "legal en Chile")
- Avoid: Mexican expressions, Argentine expressions, over-use of chilenismos that sound forced

**Checklist Regional**:
- [ ] At least 5 local-specific references integrated naturally in text
- [ ] Local payment method mentioned in first 200 words (first 100 for Kenya)
- [ ] Trust factor / local licensing authority mentioned with official name
- [ ] Local sport or cultural reference included
- [ ] Currency in local denomination throughout (zero USD unless comparing)
- [ ] Tone appropriate for target culture (verified against ICP from Step 1)
- [ ] No foreign slang from wrong Spanish dialect / wrong English register

---

### Pas 7: Content Optimization and Scaling System

**7a. Semantic Optimization with Frase AI**

After humanization and regional adaptation:
1. Import article into Frase
2. Target: Frase content score 75+ (optimal: 80-90)
3. Add semantic terms suggested by Frase that are missing (without reintroducing banned words)
4. Verify co-occurring keywords from top 10 SERP results are present
5. Check: keyword density 1-2% for primary keyword (not higher — over-optimization risk)

**7b. Scaling Production System**

Template per article:
```
[KEYWORD] + [ICP output] + [RAG dataset] = article brief → draft → humanization → regional voice → bypass check → semantic optimization → QA gate
```

**Team structure**:
- Genie/AI: ICP + RAG generation, draft creation, humanization prompt application
- Freelance writers ($30-75/article): outline + draft based on AI-generated brief
  - Brief includes: ICP summary, RAG key facts, keyword, tone guide, banned words list, regional examples
  - Writers work from brief → draft → AI humanization → bypass check
- Full AI pipeline (no freelancers): draft generated completely AI → humanization → bypass check (faster, less "human" substrate)

**7c. Production schedule target**:

| Market | Articles/month | Writer budget | Tool budget | Total/month |
|--------|---------------|---------------|-------------|-------------|
| Peru   | 10-15         | $300-1,125    | ~$50        | $350-1,175  |
| Kenya  | 10-15         | $300-1,125    | ~$50        | $350-1,175  |
| Chile  | 10-15         | $300-1,125    | ~$50        | $350-1,175  |

**7d. Quality Assurance Gate (MANDATORY per article)**

Every article must pass ALL of these before publication:
1. AI detection: Originality.ai < 15% AND GPTZero "Likely Human"
2. Frase content score: 75+
3. Reading level: Grade 8-10 (Hemingway)
4. Regional references: 5+ local mentions
5. Payment method: local method in first 200 words
6. Banned words: zero occurrences (Ctrl+F verified)
7. Word count: meets or exceeds SERP average for target keyword
8. Schema markup: FAQPage for FAQ section prepared
9. Affiliate links: all masked via Rebrandly/ThirstyAffiliates
10. Disclaimer: responsible gambling notice + 18+ + local regulatory reference

---

### Pas 8: Post-Publication Monitoring and Iteration

**8a. 30-Day Performance Check per Article**

After publication, monitor:
- Indexation: `site:domain.com/url` confirmed within 7 days
- Initial ranking position for target keyword
- GSC impressions and clicks (even if low initially)
- Any manual actions or flags

**8b. Feedback Loop**

- If article gets flagged by platform moderators → identify which section triggered, rewrite
- If ranking stagnates at position 30-50 after 60 days → audit on-page (keyword density, co-occurring terms)
- If AI score increases on re-check (detection tools update) → rewrite flagged paragraphs
- If conversion rate is low despite traffic → audit CTA placement and affiliate link visibility

**8c. Monthly Batch Review**

At the end of each month:
- Audit 10% of published articles through Originality.ai (re-check for updated detection)
- If more than 2 articles score >25%: update banned words list with new patterns detected
- If rankings stagnate: verify ICP + RAG are current (2026 data, not outdated)
- Refresh bonus amounts and regulatory data in top-performing articles

---

## §3 Cortex Logging

```json
{
  "text": "AI Content Casino executed for market [COUNTRY]. Keyword: [KW]. Article: [TITLE]. ICP: [OK/MISSING]. RAG: [OK/MISSING]. Humanized: [OK/MISSING]. Bypass score: [X%]. Frase score: [X]. Regional refs: [N]. Publish URL: [URL]. Batch total: [N] articles. Avg bypass: [X%].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AI-CONTENT-CASINO",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-010",
    "tags": ["ai-content", "casino", "gambling", "humanization", "ai-detection-bypass", "icp", "rag", "peru", "kenya", "chile", "content-pipeline", "regional-tone"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

### WHERE
At every content creation task for gambling/casino SEO in Peru/Kenya/Chile markets.

### WHEN
At each article produced (pre-publish gate mandatory). At each batch of 10+ articles (batch review). Monthly (audit 10% of published articles).

### HOW (violation detection)
- Article published without ICP created (Step 1) → violation
- RAG dataset generated from LLM memory without web browsing (Step 2) → violation
- Humanization prompt modified (not verbatim, Step 3) → **critical violation**
- Banned words present in published article (Step 4) → **critical violation**
- AI detection score over 15% at publish (Step 4) → **critical violation**
- Frase score below 75 at publish (Step 7) → violation
- Regional references absent (< 5 per article, Step 6) → violation
- Payment method local absent from first 200 words (Step 6) → violation
- Hemingway App grade over 10 → violation
- Batch of 10+ articles without batch logging → violation
- Article published without QA gate completion (Step 7d) → **critical violation**
- Currency in USD instead of local denomination → violation

**Pre-publish gate (MANDATORY — do not publish without ALL checked)**:

- [ ] Step 1: ICP created for target market with local payment methods
- [ ] Step 2: RAG dataset generated with web browsing (not from LLM memory)
- [ ] Step 3: Humanization prompt applied verbatim + regional voice pass
- [ ] Step 4: Banned words list — zero occurrences (Ctrl+F verified manually)
- [ ] Step 4: Banned phrases — zero occurrences
- [ ] Step 4: Banned word families (dive/leverage/unlock etc.) — zero occurrences
- [ ] Step 4: Originality.ai or Winston AI score sub 15%
- [ ] Step 5: Sentence length burstiness present (SD > 8)
- [ ] Step 5: 3-4 micro-errors deliberately added
- [ ] Step 5: Unnecessary hyphens eliminated
- [ ] Step 5: Hemingway App — Grade 8-10
- [ ] Step 5: Named entity density 5+ per 500 words
- [ ] Step 6: Minimum 5 local-specific references
- [ ] Step 6: Payment method local in first 200 words
- [ ] Step 6: Currency in local denomination throughout
- [ ] Step 7: Frase content score 75+
- [ ] Step 7: Schema markup prepared (FAQPage minimum)

If any item is unmet: Stop. Do not publish. Resolve the item. Re-verify the entire list.

### CONNECT
- TRAIN-SEO-010 → enforcement AI content casino gambling SEO
- TRAIN-SEO-001 (CASINO-MARKET-ANALYSIS) → prerequisite: market data for RAG dataset
- TRAIN-SEO-009 (CASINO-AFFILIATE-SETUP) → articles feed conversion structure
- TRAIN-SEO-006 (PARASITE-LINK-BUILDING) → link building for ranking produced content
- TRAIN-SEO-016 (SEMANTIC-TOPICAL-AUTHORITY) → content fits topical map
- `procedure-health.json` → add entry

### VERIFY
At the end of each execution, verify:
- [ ] ICP created and saved as `icp-[country]-[date].md` (Step 1)?
- [ ] RAG dataset generated with web browsing and saved (Step 2)?
- [ ] Humanization prompt applied verbatim, output sounds human (Step 3)?
- [ ] Banned words list — zero occurrences confirmed with Ctrl+F (Step 4)?
- [ ] AI detection score sub 15% (Originality.ai or Winston AI) (Step 4)?
- [ ] Structural bypass applied: burstiness + micro-errors + Grade 8-10 (Step 5)?
- [ ] Regional tone: min 5 local references + payment method in first 200 words (Step 6)?
- [ ] Frase content score 75+ (Step 7)?
- [ ] QA gate fully completed (Step 7d)?

---

## §5 Dependente

**Tools necesare**:
- Gemini Pro (Google AI Studio — free tier or API) — for Step 1 + Step 2
- Claude (Anthropic) — for Step 3 humanization
- Originality.ai or Winston AI — for bypass verification (Step 4)
- GPTZero — secondary AI detection check
- Frase AI — for semantic optimization (Step 7)
- Hemingway App (free, hemingwayapp.com) — for reading level check (Step 5)
- Word frequency counter — for Zipf's law verification

**Proceduri conexe**:
- `CASINO-MARKET-ANALYSIS.md` — market analysis Peru/Kenya/Chile (input for RAG)
- `PBN-CONSTRUCTION-GAMBLING.md` — where produced content may be published
- `PARASITE-PLATFORM-SELECTION.md` — alternatives to PBN for publishing
- `SEMANTIC-TOPICAL-AUTHORITY-CASINO.md` — topical map the content fits into

**Research salvat**:
- `memory/research/gambling-markets-global-2026.md` — market data for RAG
- `memory/research/seo-2026-full-spectrum.md` — complete SEO context
- `memory/research/gambling-seo-resources-2026.md` — additional resources

**Surse procedurale**:
- Charles Floate — Super AI SEO system (3-step workflow + humanization prompt)
- Banned words list: standard compilation from GPTZero/Originality.ai detection research
- Regional data: compiled from `gambling-markets-global-2026.md`

---

*Procedure version 2.0 | Updated: 2026-03-20 | Original: 2026-03-04*
