---
type: procedure
created: 2026-03-17
status: active
slug: enrich-procedure
tags: [procedure, nexus]
---

# ENRICH — Procedură de Enrichment Intelligence (Cross-Channel)

> **Notă de naming**: Această procedură se numea anterior "INSIGHT". Redenumită "ENRICH" (2026-02-27) pentru a elimina coliziunea cu "INSIGHT REFINE" (REFINE Phase 3, Opus). ECHELON enrichment per-item = **ENRICH**. REFINE Phase 3 synthesis = **INSIGHT REFINE**.

**Status**: ACTIVE
**Creat**: 2026-02-27
**Versiune**: 1.3
**Regulă asociată**: ECHELON-S-001
**Scope**: Orice conținut din orice canal ECHELON care trece de filtrare → enrichment standardizat

---

## 1. Problema

Conținutul extras din canale (X, YouTube, Reddit, LinkedIn, Newsletter, etc.) vine ca raw data — text + metrici. Fără enrichment, e doar noise. INSIGHT transformă raw data în intelligence actionabilă prin:
- Structured analysis (9 secțiuni standard)
- Scoring (relevance, sentiment, urgency)
- Entity extraction (people, companies, products, technologies)
- **Reference extraction** (books, papers, courses, tools → search free sources → ingest or propose purchase)
- Action items generation
- Cross-reference cu existing knowledge (Cortex)

### De ce e procedură separată (nu embedded în fiecare canal)
- **Reusable**: Aceeași logică se aplică pe X, YouTube, Reddit, orice canal
- **Updatable**: Schimbarea modelului/prompt-ului se face într-un singur loc
- **Testable**: Se poate benchmarka independent de sursă
- **Cost-tracked**: Consumul Gemini e vizibil per canal

---

## 2. Input Contract

Orice canal care apelează INSIGHT trebuie să furnizeze:

```json
{
  "content": "string — textul complet (obligatoriu)",
  "channel": "string — x-twitter|youtube|reddit|linkedin|newsletter|skool|github|bluesky|instagram|threads|tiktok|podcast|coursera|scribd",
  "author": "string — numele/handle-ul autorului",
  "url": "string — URL original",
  "engagement": {
    "views": "number (opțional)",
    "likes": "number (opțional)",
    "bookmarks": "number (opțional)",
    "comments": "number (opțional)"
  },
  "engagement_score": "number — calculat de canalul sursă",
  "dedup_checked": "boolean — true dacă URL a trecut dedup gate (opțional)",
  "content_type": "string — tweet|article|video_transcript|thread|post|comment",
  "extracted_at": "ISO timestamp",
  "media": ["array of media URLs — opțional"]
}
```

### Engagement Score Normalization

- `engagement_score` este calculat de fiecare canal conform propriei formule.
- INSIGHT rutează pe baza valorii primite, nu pe formula.
- Threshold-ul Mode A (`>2000`) se aplică uniform pe scorul normalizat.
- Fiecare canal TREBUIE să documenteze formula în propria procedură de extracție.

---

## 3. Routing (Dual-Layer: Gemini Default + Opus High-Value)

### Arhitectura Dual-Layer

```
Post trece filter → Calculează engagement_score →
  IF engagement_score > 5000 OR manual_flag="opus":
    → Mode A-Deep (Opus) — verificare claims, entity discovery, bias detection
  ELIF engagement_score > 2000 OR content_type in [article, video_transcript]:
    → Mode A-Standard (Gemini Flash) — analiză rapidă, 10 secțiuni
  ELIF batch scan (top N):
    → Mode B (Gemini Flash) — batch overview
  ELSE:
    → Mode C — metrics-only store
```

**De ce dual-layer**: Gemini Flash e rapid ($0.00) dar superficial — acceptă toate claims-urile fără verificare, nu detectează sensaționalism, nu caută surse originale. Opus e lent (~90s) și costă (~$0.15-0.20/post) dar verifică claims real (caută paper-ul pe ArXiv), identifică autorii, detectează bias, și produce action items specifice cu IDs/DOIs.

**Buget Opus estimat**: ~3-5 posturi/săptămână × $0.20 = $0.60-$1.00/săptămână.

---

### Mode A-Standard: Gemini Flash (Default)
**Când**: `engagement_score > 2000` SAU `content_type in [article, video_transcript]`
**Tool**: Gemini 2.5 Flash (OAuth free) — `/opt/homebrew/bin/gemini -m gemini-2.5-flash`
**Cost**: $0.00 (1000 calls/day)
**Timp**: ~8s/post
**Calitate**: 6/10 — complet dar superficial, claims "verified within context" only

**Prompt Gemini (10 secțiuni)**:
```
You are an ECHELON intelligence analyst. Your job is to extract actionable competitive intelligence from social/media content.
Be precise and factual — cite names, numbers, dates, and versions. Do not pad with generic observations.

Analyze this {content_type} from {channel} by @{author}.

Content:
---
{content}
---

Engagement: {views} views, {likes} likes, {bookmarks} bookmarks

Respond using these exact 10 numbered sections. Budget: ~30 words per section, ~50 for KEY CLAIMS and ACTION ITEMS.

1. SUMMARY — 2-3 sentences: what is this about, why it matters.
2. KEY CLAIMS — Bullet points of factual claims made. Be literal — quote or paraphrase, don't interpret.
3. COMPETITIVE INTELLIGENCE — Companies, products, market moves mentioned. Include specifics (funding amounts, release dates, partnerships).
4. MARKET SIGNALS — Trends, shifts, opportunities, threats. State the "so what" for each.
5. ENTITIES — People, companies, products, technologies. Format: "Name (type) — context".
6. SENTIMENT — One of: positive/negative/neutral/mixed. Confidence 0.0-1.0.
7. RELEVANCE SCORE — Integer 1-10 for AI/SaaS/market intelligence/automation. Deduct for vagueness or off-topic content.
8. ACTION ITEMS — Specific next steps. Not "monitor the space" but "check {tool} pricing page" or "track {company} Q2 earnings".
9. FOLLOW-UP — Questions to investigate, accounts to monitor, topics to track. Max 3 items.
10. REFERENCED MATERIALS — Books, papers, courses, frameworks, tools, datasets explicitly mentioned. For each: title, author if known, type (book|paper|course|framework|tool|dataset|report), URL/identifier if mentioned. If none: "None detected."

CONSTRAINTS:
- Total output: 250-400 words. Skip sections only if truly not applicable (write "N/A").
- Do NOT inflate relevance scores — a 7 means genuinely relevant, not "somewhat interesting."
```

**Post-Gemini upgrade check**:
```python
# După Gemini Mode A, verifică dacă merită upgrade la Opus
gemini_result = parse_gemini_output(raw_output)
relevance = gemini_result.get("relevance_score", 0)

if relevance >= 8 and engagement_score >= 5000:
    # Upgrade to Opus for deep verification
    opus_result = run_opus_insight(content, engagement)
    final_result = opus_result  # Opus replaces Gemini
    final_result["upgraded_from"] = "gemini"
```

---

### Mode A-Deep: Opus (High-Value Only)
**Când**: `engagement_score > 5000` SAU `manual_flag="opus"` SAU upgrade din Gemini (relevance ≥8 + engagement ≥5000)
**Tool**: Claude Opus 4.6 (via Genie subagent)
**Cost**: ~$0.15-$0.20/post
**Timp**: ~60-90s/post
**Calitate**: 9/10 — claims verification reală, entity discovery, bias detection, specific action items

**Prompt Opus (10 secțiuni + verificare)**:
```
You are running ECHELON ENRICH Mode A-Deep analysis. Be rigorous, specific, and actionable.
This is for competitive intelligence purposes.

Analyze this {content_type} from {channel} by @{author}.

Content:
---
{content}
---

Engagement: {views} views, {likes} likes, {bookmarks} bookmarks

Provide analysis in these 10 sections:

1. SUMMARY (2-3 sentences)
2. KEY CLAIMS (bullet points, factual claims made)
3. KEY CLAIMS VERIFICATION (for each claim: verified/unverified/misleading, with brief reasoning.
   IMPORTANT: Do NOT just verify "within context of the article." Actually search for the original
   sources — papers on ArXiv, official announcements, author profiles. Flag sensationalized or
   embellished claims explicitly.)
4. COMPETITIVE INTELLIGENCE (companies, products, market moves — with specific implications)
5. MARKET SIGNALS (trends, shifts, opportunities, threats — be specific about why each matters)
6. ENTITIES (people, companies, products, technologies — discover real identities, not just what
   the post mentions. Find paper authors, company affiliations, DOIs.)
7. SENTIMENT (positive/negative/neutral, confidence 0-1. Note if the tone is sensationalized
   vs. the underlying source material's actual tone.)
8. RELEVANCE SCORE (1-10 for AI/SaaS/market intelligence/automation. Deduct points for
   missing actionability or excessive sensationalism.)
9. ACTION ITEMS (specific and actionable — include paper IDs, URLs, metrics to track.
   Not generic "monitor the space" but concrete next steps.)
10. REFERENCED MATERIALS (books, papers, courses, tools, datasets. For each: title, author,
    type, and identifier (ArXiv ID, DOI, ISBN, URL). Search for the actual source if the post
    only mentions it by name.)

Return structured analysis only.
```

**Diferențe cheie Opus vs Gemini**:
| Capabilitate | Gemini | Opus |
|-------------|--------|------|
| Claims verification | "within context" (lazy) | **Caută sursa originală** (ArXiv, DOI) |
| Entity discovery | Doar ce e în text | **Găsește autori reali, afilieri** |
| Bias/sensationalism | Nu detectează | **Flagează explicit** |
| Action items | Generice | **Specifice cu IDs, URLs, metrici** |
| Relevance scoring | Inflat (always 9-10) | **Realist, deduce puncte motivat** |
| Referenced materials | Titlu doar | **ArXiv ID, DOI, PMC links** |

---

### Mode B: Batch Overview (Gemini Only)
**Când**: Light scan results — top 10 posts per engagement
**Tool**: Gemini 2.5 Flash
**Cost**: $0.00
**Notă**: Opus nu se justifică pe batch (cost/valoare slab pe overview)

**Prompt batch**:
```
You are an ECHELON intelligence analyst performing batch content analysis for competitive intelligence monitoring.
Your goal is to evaluate whether an author is worth tracking long-term based on their content patterns.

Analyze these {N} posts from @{author} on {channel}.

Posts:
---
{post_1_text} [likes:{l1} bookmarks:{b1}]
{post_2_text} [likes:{l2} bookmarks:{b2}]
...
---

Respond using these exact sections with markdown ## headers:

## Batch Summary
1 sentence: who is this person and what do they post about.

## Author Profile
Expertise areas, focus topics, communication style, audience level (beginner/intermediate/expert). Cite specific posts as evidence.

## Top Signals
Top 5 themes across all posts — ranked by frequency. For each: theme name + 1-line description + which post(s).

## Unique Insights
What does this author know that most accounts in this space don't? Be specific — name frameworks, data points, or contrarian takes. If nothing unique: say "No differentiated signal detected."

## Monitoring Recommendation
YES or NO + 1 sentence justification. Consider: signal-to-noise ratio, posting frequency, overlap with existing monitored accounts.

## Relevance Score
Single number 1-10 for AI/SaaS/market intelligence/automation relevance. Brief justification.

CONSTRAINTS:
- Max 300 words total. Be specific — cite post numbers, names, numbers, dates.
- Do not pad sections with filler. If a section has nothing substantive, write "N/A" and move on.
- Score 7+ only if the author provides actionable intelligence, not just commentary.
```

---

### Mode C: Metrics-Only Store (skip AI enrichment)
**Când**: Valuable dar sub threshold-ul de deep enrichment
**Ce salvezi**: Raw text + engagement metrics + channel + author + timestamp
**Cost**: $0.00 (doar Cortex store)

---

## 4. Output Contract

### Mode A output (ambele layere — Gemini + Opus):
```json
{
  "type": "insight_individual",
  "source": {"channel": "x-twitter", "author": "@handle", "url": "..."},
  "analysis": {
    "summary": "...",
    "key_claims": ["..."],
    "claims_verification": [
      {"claim": "...", "status": "verified|unverified|misleading", "reasoning": "..."}
    ],
    "competitive_intel": ["..."],
    "market_signals": ["..."],
    "entities": [{"name": "...", "type": "person|company|product|tech", "context": "..."}],
    "sentiment": {"label": "positive", "confidence": 0.85, "tone_note": "sensationalized vs source"},
    "relevance_score": 9.5,
    "action_items": ["..."],
    "follow_up": ["..."],
    "referenced_materials": [
      {
        "title": "...",
        "author": "... (if known)",
        "type": "book|paper|course|framework|tool|dataset|report",
        "identifier": "ArXiv ID / DOI / ISBN / URL",
        "context": "how it was referenced (recommended, cited, criticized, etc.)"
      }
    ]
  },
  "enriched_at": "ISO timestamp",
  "model": "gemini-2.5-flash | claude-opus-4-6",
  "insight_layer": "standard | deep",
  "upgraded_from": "null | gemini",
  "cost": 0.00
}
```

### Mode B output:
```json
{
  "type": "insight_batch",
  "source": {"channel": "x-twitter", "author": "@handle", "post_count": 10},
  "analysis": {
    "author_profile": "...",
    "main_topics": ["..."],
    "unique_insights": ["..."],
    "monitoring_value": "...",
    "relevance_score": 8.0
  },
  "enriched_at": "ISO timestamp"
}
```

### Mode C output:
```json
{
  "type": "insight_metrics_only",
  "source": {"channel": "...", "author": "...", "url": "..."},
  "content_preview": "primele 280 chars din content",
  "engagement": {"views": "N", "likes": "N", "bookmarks": "N"},
  "engagement_score": "N",
  "content_type": "tweet|article|...",
  "stored_at": "ISO timestamp",
  "insight_mode": "C"
}
```

---

## 5. Store (Cortex)

Toate rezultatele INSIGHT → Cortex `intelligence` collection:

```bash
ssh pafi@100.81.233.9 "curl -s -X POST http://localhost:6400/api/store \
  -H 'Content-Type: application/json' \
  -d '{
    \"text\": \"[SUMMARY]\\n\\n[KEY CLAIMS]\\n\\n[FULL ANALYSIS]\",
    \"collection\": \"intelligence\",
    \"metadata\": {
      \"type\": \"insight\",
      \"channel\": \"{channel}\",
      \"author\": \"{author}\",
      \"url\": \"{url}\",
      \"content_type\": \"{content_type}\",
      \"engagement_score\": {score},
      \"relevance_score\": {relevance},
      \"sentiment\": \"{sentiment}\",
      \"insight_mode\": \"A|B|C\",
      \"entities\": [\"{entity1}\", \"{entity2}\"],
      \"tags\": [\"{tag1}\", \"{tag2}\"],
      \"enriched_at\": \"{timestamp}\"
    }
  }'"
```

Local backup: `~/.nexus/echelon/{channel}/output/YYYY-MM-DD/{id}.json`

---

## 5.1 Display & Notify (Post-Store, Obligatoriu)

**Scope**: După fiecare INSIGHT Mode A sau B, rezultatele se afișează operatorului și se trimit pe Telegram. Fără display/notify = intelligence pierdută (nu se validează, nu se acționează).

### 5.1.1 Display în Terminal (Genie → Pafi)

După store, Genie TREBUIE să afișeze un **sumar structurat** al insight-ului:

**Format Mode A:**
```
═══════════════════════════════════════════════════
📊 ECHELON ENRICH — {channel} | Mode A
═══════════════════════════════════════════════════
Source: @{author} — {url}
Engagement: {views} views, {likes} likes, {bookmarks} bookmarks
Score: {engagement_score} | Relevance: {relevance}/10 | Sentiment: {sentiment}
───────────────────────────────────────────────────
SUMMARY: {summary — 2-3 sentences}

KEY CLAIMS:
• {claim_1}
• {claim_2}
• ...

COMPETITIVE INTEL: {top 2-3 bullet points}

MARKET SIGNALS: {top 2-3 bullet points}

ENTITIES: {entity list with types}

ACTION ITEMS:
→ {action_1}
→ {action_2}

REFERENCED MATERIALS: {materials or "None detected"}

Cortex ID: {cortex_id}
═══════════════════════════════════════════════════
```

**Format Mode B:**
```
═══════════════════════════════════════════════════
📊 ECHELON ENRICH — {channel} | Mode B (Batch: {N} posts)
═══════════════════════════════════════════════════
Author: @{author}
Topics: {main_topics}
Monitoring Value: {recommendation}
Relevance: {score}/10
Cortex ID: {cortex_id}
═══════════════════════════════════════════════════
```

**Mode C**: Nu se afișează (metrics-only, silent store).

### 5.1.2 Telegram Notification

După display, trimite notificare pe Telegram **EXCLUSIV prin VPS bot** (@Caludeintelbot).

**REGULA**: Toate notificările ECHELON → VPS Telegram bot. Niciodată MacIntel bot.

**Telegram Format Mode A** (mesaj concis, max 4096 chars):
```
📊 ECHELON ENRICH [{channel}]

@{author}: {summary_1_sentence}

📈 Engagement: {engagement_score} | Relevance: {relevance}/10
💬 Sentiment: {sentiment} ({confidence})

🎯 Key Signals:
• {market_signal_1}
• {market_signal_2}

⚡ Action Items:
→ {action_1}
→ {action_2}

📚 Materials: {count} referenced ({ingested}/{proposed})

🔗 {url}
📦 Cortex: {cortex_id}
```

**Telegram Format Mode B:**
```
📊 ECHELON BATCH [{channel}]

@{author}: {N} posts analyzed
Topics: {top_3_topics}
Monitoring: {yes/no + reason}
Relevance: {score}/10

📦 Cortex: {cortex_id}
```

**Implementare Telegram send (ÎNTOTDEAUNA via VPS):**
```bash
# VPS bot (@Caludeintelbot) — via SSH din orice mașină
ssh pafi@100.81.233.9 'curl -s "https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage" \
  -d chat_id="${TELEGRAM_CHAT_ID}" \
  -d parse_mode="Markdown" \
  -d text="'"${ESCAPED_MESSAGE}"'"'
```

**VPS env vars** (deja configurate în `.env` sau systemd):
- `TELEGRAM_BOT_TOKEN` — token-ul @Caludeintelbot
- `TELEGRAM_CHAT_ID` — chat ID Pafi

### Enforcement

- **WHO**: Genie (la execuție manuală) sau pipeline script (la execuție automată)
- **WHEN**: Imediat după Cortex store (§5) și ÎNAINTE de Reference Extraction (§5.2)
- **VIOLATION**: Insight stocat dar neafișat/netrimis = invisible intelligence = pipeline broken
- **CHECK**: Cortex entries fără corresponding Telegram message → gap

---

## 5.2 Reference Extraction & Ingestion (Post-Analysis)

**Scope**: Când INSIGHT Mode A detectează materiale referite (cărți, papers, cursuri, etc.), acest pas le caută în surse gratuite și le ingestează automat, sau le propune către Pafi pentru achiziție.

**Trigger**: `referenced_materials` array din Mode A output conține cel puțin 1 entry.

### Pipeline

```
INSIGHT Mode A output → Parse referenced_materials →
  FOR EACH material:
    1. CLASSIFY type (book|paper|course|framework|tool|dataset|report)
    2. SEARCH free sources (by type)
    3. DECIDE: found_free → queue ingestion | paid_only → notify Pafi
```

### Search Strategy per Type

| Type | Surse gratuite (în ordine) | Surse plătite (propune) |
|------|---------------------------|------------------------|
| **paper** | ArXiv, Semantic Scholar, Google Scholar, Sci-Hub | Journal paywall → Pafi |
| **book** | YouTube (reviews/summaries), Scribd (free trial), LibGen, author's blog | Amazon, O'Reilly → Pafi |
| **course** | YouTube (full lectures), MIT OCW, Coursera audit, author talks | Udemy, Coursera paid → Pafi |
| **framework/tool** | GitHub, official docs, npm/PyPI | Enterprise license → Pafi |
| **dataset** | Kaggle, HuggingFace, GitHub, official releases | Licensed datasets → Pafi |
| **report** | Company blog, press release, SlideShare, author tweets | Gartner, McKinsey → Pafi |

### Search Implementation

```python
def search_reference(material):
    """Search free sources for a referenced material."""
    title = material["title"]
    mat_type = material["type"]

    results = []

    # 1. Always try Delphi first (cross-engine search)
    delphi_result = delphi_search(f'"{title}" free {mat_type} full text')
    if delphi_result:
        results.append({"source": "delphi", "url": delphi_result.url, "access": "free"})

    # 2. Type-specific searches
    if mat_type == "paper":
        # ArXiv search
        arxiv = arxiv_search(title)  # MCP tool: mcp__arxiv__search_papers
        if arxiv:
            results.append({"source": "arxiv", "url": arxiv.url, "access": "free"})
        # Semantic Scholar
        scholar = web_search(f'"{title}" site:semanticscholar.org')
        if scholar:
            results.append({"source": "semantic_scholar", "url": scholar.url, "access": "free"})

    elif mat_type == "book":
        # YouTube summary/review
        yt = web_search(f'"{title}" book summary OR review site:youtube.com')
        if yt:
            results.append({"source": "youtube", "url": yt.url, "access": "free", "note": "summary/review, not full"})
        # Author talks
        author_talk = web_search(f'{material.get("author", "")} "{title}" talk OR lecture site:youtube.com')
        if author_talk:
            results.append({"source": "youtube_talk", "url": author_talk.url, "access": "free"})

    elif mat_type == "course":
        yt_course = web_search(f'"{title}" full course site:youtube.com')
        if yt_course:
            results.append({"source": "youtube_course", "url": yt_course.url, "access": "free"})

    elif mat_type in ["framework", "tool"]:
        gh = web_search(f'"{title}" site:github.com')
        if gh:
            results.append({"source": "github", "url": gh.url, "access": "free"})

    elif mat_type == "dataset":
        hf = web_search(f'"{title}" site:huggingface.co OR site:kaggle.com')
        if hf:
            results.append({"source": "huggingface_or_kaggle", "url": hf.url, "access": "free"})

    return results
```

### Decision Logic

```python
def process_references(insight_output):
    """Post-INSIGHT: process all referenced materials."""
    materials = insight_output.get("referenced_materials", [])
    if not materials or materials == "None detected.":
        return  # Nothing to process

    ingestion_queue = []
    purchase_proposals = []

    for material in materials:
        results = search_reference(material)
        free_sources = [r for r in results if r["access"] == "free"]

        if free_sources:
            # Queue for ingestion via appropriate ECHELON channel
            best = free_sources[0]  # Prioritize first match
            ingestion_queue.append({
                "material": material,
                "source": best,
                "ingest_via": map_source_to_channel(best["source"]),
                "priority": "normal"
            })
        else:
            # No free source found → propose to Pafi
            purchase_proposals.append({
                "material": material,
                "search_attempted": [r["source"] for r in results] if results else ["delphi"],
                "suggested_source": suggest_purchase_source(material)
            })

    # Action 1: Queue free materials for ingestion
    for item in ingestion_queue:
        queue_echelon_ingestion(item)
        # Store reference link in Cortex
        cortex_store_reference(item)

    # Action 2: Notify Pafi about paid materials
    if purchase_proposals:
        notify_pafi_purchase(purchase_proposals)

def map_source_to_channel(source):
    """Map found source to ECHELON channel for ingestion."""
    mapping = {
        "youtube": "youtube",
        "youtube_talk": "youtube",
        "youtube_course": "youtube",
        "arxiv": "arxiv",  # Direct ArXiv MCP download
        "github": "github",
        "semantic_scholar": "web",
        "huggingface_or_kaggle": "web",
        "delphi": "web"
    }
    return mapping.get(source, "web")

def suggest_purchase_source(material):
    """Suggest where to buy based on type."""
    suggestions = {
        "book": "Amazon / O'Reilly Safari",
        "paper": "Journal direct / ResearchGate request",
        "course": "Udemy / Coursera / official site",
        "report": "Direct from publisher",
        "dataset": "Provider licensing page"
    }
    return suggestions.get(material["type"], "Web search for purchase")
```

### Notification Format (Telegram → Pafi)

**For free ingestion** (automated, logged):
```
📥 ECHELON Reference Ingested
Material: "{title}" by {author}
Type: {type}
Source: {source_url}
Found via: {search_source}
Ingested to: {echelon_channel}
Original context: {insight_url}
```

**For purchase proposal**:
```
📚 ECHELON Material Proposal
Material: "{title}" by {author}
Type: {type}
Referenced in: {insight_url}
Context: "{context}" (how it was mentioned)
Searched: {sources_tried}
Suggested purchase: {suggested_source}
Action: Reply /buy or /skip
```

### Cortex Store for References

```json
{
  "text": "Reference: {title} by {author}. Type: {type}. Found in: {original_insight_url}. Context: {context}",
  "collection": "references",
  "metadata": {
    "type": "echelon_reference",
    "material_title": "{title}",
    "material_type": "{type}",
    "material_author": "{author}",
    "source_insight_id": "{cortex_id_of_parent_insight}",
    "ingestion_status": "queued|ingested|proposed|skipped",
    "ingestion_channel": "{channel}",
    "free_source_url": "{url}",
    "detected_at": "{timestamp}"
  }
}
```

### Guardrails

- **Rate limit**: Max 5 reference searches per INSIGHT run (evită explosion pe content cu multe cărți)
- **Dedup**: Cortex search pe `collection=references` + `material_title` înainte de search (nu recaută ce am găsit deja)
- **Quality gate**: Doar materiale cu `relevance_score >= 7` din INSIGHT trigger reference search
- **Cooldown**: Același material nu se re-caută mai des de 30 zile

---

## 6. Fallback Chain

### Mode A-Standard (Gemini default):
| Primară | Fallback 1 | Fallback 2 |
|---------|-----------|-----------|
| Gemini 2.5 Flash (OAuth free) | Ollama qwen2.5:7b (local, $0.00) | Grok via OpenRouter ($0.05/query) |

**Triggerul fallback**: Gemini 429 (rate limit) sau timeout > 30s.
**Ollama limitations**: Mode A doar (no batch), output mai scurt, quality ~70% vs Gemini.

### Mode A-Deep (Opus high-value):
| Primară | Fallback 1 | Fallback 2 |
|---------|-----------|-----------|
| Claude Opus 4.6 (via Genie subagent) | Gemini 2.5 Flash (downgrade to standard) | Ollama qwen2.5:7b |

**Triggerul fallback**: Opus token budget exhausted sau Genie la >85% usage.
**Downgrade note**: Dacă Opus fallback → Gemini, tag `"downgraded_from": "opus"` în output.

---

## 7. Enforcement Loop (META-H-002)

### WHERE
- **WISH Step H** — Orice canal ECHELON care a trecut de filter stage → MUST call INSIGHT
- **Pipeline scripts** — `x-enrich.js`, `yt-enrich.js`, etc. → hardcoded INSIGHT call

### WHEN
- **Trigger**: Conținut trece de filtrul canalului (engagement threshold + topic relevance)
- **Frequency**: Per post (Mode A), per scan batch (Mode B), continuous

### HOW (violation detection)
- **Cortex audit**: Entries cu `collection=intelligence` dar fără `insight_mode` metadata → gap
- **Pipeline log check**: Posts care au trecut filter dar nu au enrichment timestamp → missed
- **Cost tracking**: Dacă Gemini calls/day < expected volume → pipeline broken
- **Runner**: Verificările HOW sunt executate de:
  - `test-all-procedures.js` (daily 02:00) — enforcement loop check automat
  - `procedure-health-check.sh` (weekly) — staleness + component health
  - Manual: Genie verifică la fiecare utilizare a procedurii (PROC-H-001)
  - Niciun script custom nou necesar — sistemele existente acoperă

### CONNECT
- **X-EXTRACTION-PROCEDURE.md** Pas 6 → `CALL ENRICH Mode A/B/C`
- **YOUTUBE-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/B`
- **REDDIT-EXTRACTION-PROCEDURE.md** Pas 6 → `CALL ENRICH Mode A/B/C`
- **LINKEDIN-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/B`
- **NEWSLETTER-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/B`
- **PODCAST-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/C`
- **BLUESKY-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/B/C`
- **INSTAGRAM-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/C`
- **THREADS-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/C`
- **TIKTOK-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/C` (post-transcription)
- **SKOOL-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/B`
- **GITHUB-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A/C`
- **COURSERA-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A` (post-extraction)
- **SCRIBD-EXTRACTION-PROCEDURE.md** → `CALL ENRICH Mode A` (post-extraction)
- **Cortex store** → Always through INSIGHT output contract
- **Display & Notify (§5.1)** → Terminal display + Telegram notification
- **Reference Extraction (§5.2)** → Post-analysis material search + ingestion/proposal
- **Radar** → INSIGHT follow-up items → new monitoring targets

### Dependencies
- **Requires**: Gemini OAuth active (`~/.gemini/settings.json`)
- **Requires**: Cortex VPS reachable (`100.81.233.9:6400`)
- **Required by**: All ECHELON channel procedures, Daily Digest, Telegram alerts

---

## 8. Metrics

| Metric | Target | Cum măsori |
|--------|--------|-----------|
| Enrichment rate | >90% of filtered posts | Cortex count(insight_mode=A or B) / total filtered |
| Avg relevance score | >6.0 | Cortex avg(relevance_score) per channel |
| Gemini latency | <10s/call | Pipeline timing logs |
| Fallback rate | <5% | Count(ollama or grok calls) / total calls |
| Action items generated | >2 per Mode A post | Cortex count(action_items) per post |
