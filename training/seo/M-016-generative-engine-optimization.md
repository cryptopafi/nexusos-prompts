---
type: procedure
created: 2026-03-22
status: active
slug: m-016-generative-engine-optimization
tags: [procedure, nexus]
---

# Generative Engine Optimization (GEO) Content Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Structured GEO strategy for optimizing content to be cited, referenced, and surfaced by AI-powered search engines (Google SGE/AI Overviews, Perplexity, ChatGPT search), covering content structure, E-E-A-T signals, structured data, and AI visibility monitoring.

---

## 1. Problema

Traditional SEO optimizes for 10 blue links. Generative AI search engines synthesize answers from multiple sources, citing only the most authoritative, structured, and trustworthy content. Without a GEO strategy, content is invisible in AI-generated overviews even when it ranks on page 1 of traditional SERPs.

Types of situations covered:
- New content creation — building AI-citation-ready content from the start
- Existing content upgrade — retrofitting traditional SEO content for GEO
- E-E-A-T authority building — establishing brand as an AI-cited source
- Google SGE appearance — optimizing for AI Overview inclusion
- Perplexity/ChatGPT visibility — expanding presence beyond Google

---

## 2. Procedura

### Pas 1: Identify AI-Cited Content in Target Niche
Query Google (with AI Overviews enabled), Perplexity, and ChatGPT with 10-20 target search queries. Document which sites and URLs are cited in AI-generated responses for your target topics. Identify patterns: what content types are cited (listicles, guides, research, news), what domain types (authoritative publishers, official sources, expert blogs), and what content structures appear most (structured Q&A, numbered lists, tables with data). This is the citation benchmark.

### Pas 2: Analyze What Makes Content AI-Friendly
From the citation benchmark, extract the structural and content characteristics of cited pages:
- Authoritative sources cited inline (statistics with links, expert quotes)
- Clear, direct answers in the first 50-100 words of each section
- Well-structured headings that match query phrasing
- Factual density — specific numbers, dates, named entities
- Author credentials and publication date visible
- No ambiguity in claims — every assertion is supported
Document these as GEO content principles for your niche.

### Pas 3: Optimize for Google AI Overviews (SGE)
For each target keyword where AI Overviews appear: restructure the content to lead with a concise, direct answer (2-3 sentences) immediately following the H2 that matches the query. Use the "inverted pyramid" approach — most important information first. Include statistics, definitions, and procedural steps where relevant. Ensure the page clearly establishes topical authority through comprehensive coverage and internal linking to supporting content.

### Pas 4: Build E-E-A-T Signals
E-E-A-T (Experience, Expertise, Authoritativeness, Trustworthiness) is the primary GEO ranking factor. Implement:
- Named author bylines with bio, credentials, and LinkedIn/profile links
- "Last updated" dates on all content
- Primary research, original data, or case studies where possible
- Expert quotes or co-authorship with credentialed specialists
- Third-party citations: link to and be linked from authoritative sources
- About page with team credentials, company background, and contact information

### Pas 5: Implement Structured Data for Rich Results and AI Parsing
Add schema markup that AI systems parse to understand content structure:
- **FAQPage**: For any content with Q&A format
- **HowTo**: For process and step-by-step content
- **Article** with datePublished, dateModified, author, and publisher
- **Speakable**: Mark the most AI-citation-worthy sections for voice and AI
- **Dataset** or **Table**: For data-heavy content AI systems frequently cite
Validate all schema. Monitor Rich Results in Search Console for schema-driven features.

### Pas 6: Monitor AI Visibility
Set up tracking for GEO performance:
- Semrush AI Overview tracker — monitor which of your pages appear in Google AI Overviews
- Manual weekly queries: search 20 target keywords in Perplexity and ChatGPT, record citations
- Track "AI Overview impressions" in Google Search Console (available under Search Type filter)
- Monitor brand mentions in AI responses using brand monitoring tools
Record baseline metrics and track weekly changes. Correlate AI visibility gains with content update dates.

---

## 3. Cortex Logging

```json
{
  "text": "GEO Content Strategy executed for [domain/topic]. AI citation benchmarks: [n sites]. E-E-A-T signals implemented: [list]. Schema types added: [list]. AI Overview appearances: [n]. GEO monitoring active.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-016",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["geo", "generative-engine-optimization", "ai-overview", "sge", "e-e-a-t", "ai-search"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execution

### WHEN
La fiecare execuție a procedurii în context real sau training

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- E-E-A-T signals implementate fără author bylines → violation
- AI visibility monitoring nesetat → violation
- Conținut optimizat pentru GEO fără validare în AI search engines → violation
- Structuri de răspuns GEO create fără testare în Perplexity/ChatGPT → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum SEO & Content
- Procedura se conectează cu M-093 (GEO Metrics Tracking)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] AI citation benchmark documentat?
- [ ] E-E-A-T signals implementate pe paginile target?
- [ ] GEO monitoring tools configurate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-016 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Generative Engine Optimization (GEO) Content Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Search (AI Overviews enabled) | Benchmark AI citation patterns |
| Perplexity / ChatGPT | Cross-platform AI visibility tracking |
| Semrush AI Overview tracker | Automated GEO monitoring |
| Google Search Console | AI Overview impression data |
| Google Rich Results Test | Schema validation for AI parsing |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| AI Overview appearances (tracked queries) | Increasing trend month-over-month |
| E-E-A-T compliance (pages) | 100% of target pages with author + date |
| Schema types implemented | Minimum 3 relevant types per content template |
| Citation benchmark documented | Minimum 10 AI citations analyzed |
