---
type: procedure
created: 2026-03-24
status: active
slug: price-intelligence
tags: [procedure, nexus]
---

# PRICE-INTELLIGENCE — Universal Comparative Shopping Procedure
# Version: 1.1 | Created: 2026-03-24 | Last tested: 2026-03-24 | Author: Genie (Opus 4.6)
# FORGE Standard v1.4 | Type: PIPELINE | Trigger: /price-intel OR "find best price"
# Test runs: 2 (blood tests, peptides) | Status: VALIDATED

## Purpose
Find the best-priced provider for ANY goods or services by running a multi-channel
discovery → scrape → cross-reference → rank → verify pipeline. Reuses Arbitrage Pro
infrastructure (scout→analyze→report→notify).

## Scope
- Medical labs, insurance, electronics, services, any comparative shopping
- Geographic: any city (seed lists per domain)
- Payment: out-of-pocket, insurance, mixed

## Pipeline Architecture

```
SCOUT (3 channels) → SCRAPE (3 methods) → FILTER (80% gate) → RANK (score) → VERIFY (browser) → REPORT (Delphi Tier 2) → NOTIFY (Telegram)
```

Maps to Arbitrage Pro:
| Arbitrage | Price Intelligence |
|-----------|-------------------|
| scout-source | Phase 1: SCOUT |
| scout-dest | Phase 2: SCRAPE |
| quality-gate | Phase 3: FILTER |
| analyzer | Phase 4: RANK |
| agent-browser | Phase 5: VERIFY |
| reporter | Phase 6: REPORT |
| telegram-alert | Phase 7: NOTIFY |

---

## Input Contract

```yaml
shopping_request:
  what: ""                # what you're buying (e.g. "blood tests", "car insurance")
  items: []               # exact items/tests/services needed
  location: ""            # geographic constraint (e.g. "Bucharest, Romania")
  budget: "best price"    # budget ceiling or "best price"
  timeline: "ASAP"        # when needed
  payment: "out-of-pocket"  # insurance/cash/card
  constraints: []         # e.g. "walk-in only", "near Unirii", "open weekends"
```

---

## Phase 1: SCOUT — Build Provider List

Run 3 channels in parallel, merge and deduplicate.

### Channel A — Brave Local Search
PromptForge: C-050 Recipe + C-067 RAR

```
Query 1: "{what} private {location}"
Query 2: "{what} fara trimitere {location} pret"
Query 3: "{what} {location} sector 1 2 3 4 5 6"
```

Rationale: Q1 = broad, Q2 = intent-specific (paid, no referral), Q3 = geographic spread.

### Channel B — Delphi scout-web
PromptForge: C-062 Step-Back + C-058 Few-Shot CoT

```
Primary:   "comparatie preturi {what} {location} {year}"
Secondary: "cel mai ieftin {what} {location} recenzii"
Tertiary:  "lista completa {what} {location} contact preturi"
Exa neural: "{what} price comparison {location_en}"
Exa blog:   category:"blog post" "{what} {location}" "preturi"
```

### Channel C — Seed List (hardcoded per domain)

```yaml
# Medical Labs (Bucharest)
tier_1_national:
  - {name: Synevo, domain: synevo.ro, price_page: "/analize-medicale/"}
  - {name: MedLife, domain: medlife.ro, price_page: "/laboratoare/"}
  - {name: Regina Maria, domain: reginamaria.ro, price_page: "/servicii/analize/"}
  - {name: Sanador, domain: sanador.ro, price_page: "/servicii/laborator/"}
tier_2_specialist:
  - {name: Gral Medical, domain: gralmedical.ro}
  - {name: Bioclinica, domain: bioclinica.ro}
  - {name: CDT, domain: cdtbabes.ro}
  - {name: Genesys, domain: genesys.ro}
  - {name: Affidea, domain: affidea.ro}
  - {name: Personal Genetics, domain: personalgenetics.ro}
  - {name: Medicover, domain: medicover.ro}
tier_3_discount:
  - {name: Clinica Sante, domain: clinicasante.ro}
  - {name: Biostandard, domain: biostandard.ro}
  - {name: Polisano, domain: polisano.ro}
aggregators:
  - {name: DOC.ro, domain: doc.ro}
  - {name: iMed24, domain: imed24.ro}

# Insurance (future): add seed list here
# Electronics (future): use arbitrage scout-source + apify-ecommerce
# Services (future): add seed list here
```

### Merge Logic
1. Union all providers from A + B + C
2. Deduplicate by domain (keep richest record)
3. Output: `providers[]` with name, domain, price_page, address, phone, rating

---

## Phase 2: SCRAPE — Extract Per-Item Prices

For each provider × each item: run 3 methods in fallback chain.

### Method 1 (primary) — Tavily Extract
PromptForge: C-050 Recipe

```python
# First discover price page if unknown
sitemap = tavily_map(url="{provider.domain}")
price_url = find_price_page(sitemap)  # match "preturi", "tarife", "analize"

# Then extract
result = tavily_extract(
    urls=[price_url],
    extract_depth="deep",
    include_raw_content=True
)
# Parse: test_name (RO + EN), test_code, price_ron, turnaround_days
```

### Method 2 (secondary) — Exa Livecrawl
PromptForge: C-067 RAR (3 query variants)

```
# Per test, Romanian
exa.search("{test_name_ro} pret RON", includeDomains=["{provider.domain}"], livecrawl="preferred")

# Per test, English (catches bilingual pages)
exa.search("{test_name_en} price Romania", includeDomains=["{provider.domain}"], livecrawl="preferred")

# Full price list
exa.search("lista preturi analize laborator complet", includeDomains=["{provider.domain}"], livecrawl="preferred")
```

### Method 3 (fallback) — Agent-Browser
PromptForge: C-059 ReAct

```
Step 1: Navigate to {provider.domain}
Step 2: Find "Analize medicale" / "Preturi" / "Tarife" link
Step 3: If search box → type "{test_name}" → read results
Step 4: If category nav → browse to category → find test
Step 5: Extract price, turnaround, availability
Step 6: Screenshot as evidence
Step 7: Repeat for each test
```

### Output
```yaml
price_matrix[provider][item]:
  price_ron: 45.0
  source_method: "tavily" | "exa" | "browser"
  confidence: 1.0  # 2+ methods agree | 0.8 single method | 0.6 browser-only
  url: "https://synevo.ro/analize/hemoglobina-glicozilata/"
  turnaround_days: 2
  available: true
```

---

## Phase 3: FILTER — Coverage Gate

```
For each provider:
  coverage_pct = items_with_price / total_items × 100

HARD GATE: Drop if coverage < 80%
SOFT FLAG: Items covered by 0-1 providers → alert user
```

---

## Phase 4: RANK — Compare & Score

### Scoring Formula
Adapted from Arbitrage Pro analyzer deal scoring:

```
total_cost = SUM(item_prices) for all covered items
discount = best applicable promotion
effective_total = total_cost × (1 - discount_pct / 100)
convenience_score = (collection_points × 0.3) + (walk_in × 0.3) + (turnaround_speed × 0.4)

final_score = (1 / effective_total) × coverage_pct × convenience_score
```

Sort by: (1) coverage DESC, (2) effective_total ASC, (3) convenience DESC

### Discount Discovery
PromptForge: C-044 Cognitive Verifier

```
# Per provider
Exa: "promotie OR reducere OR oferta OR pachet {what} {current_month} {year} site:{provider.domain}"

# Cross-provider
Tavily: "promotii {what} {location} {current_month} {year} reduceri"

# Coupon check
Brave: "cod reducere {provider.name} {what} {year}"
Brave: "cupon {provider.name}"
```

Verify each discount: expiry date, conditions, min spend, combinable?
Flag "permanent discounts" (marketing, not real promotions).

### Bundle Detection
PromptForge: C-058 Few-Shot CoT

```
For each provider with bundled packages:
  1. Extract: bundle_name, included_items, bundle_price
  2. overlap_pct = user_items_in_bundle / bundle_total_items
  3. bundle_savings = sum(individual_prices) - bundle_price
  4. If overlap_pct > 60% AND bundle_savings > 0 → FLAG
  5. effective_cost = bundle_price + sum(remaining_individual_items)
  6. Compare effective_cost vs all-individual cost
```

Common Romanian lab bundles: "Profil hepatic", "Profil tiroidian", "Profil lipidic",
"Hemoleucograma completa", "Profil hormonal", "Screening complet"

### Split Strategy
If cheapest-per-test across 2 providers < cheapest single provider by >15%:
→ Recommend split strategy with specific tests per provider

---

## Phase 5: VERIFY — Browser Agent Confirmation

Top 3 providers only.

### Method J — Agent-Browser (primary)
- Open provider website
- Navigate to price list / calculator
- Verify ≥3 test prices match scraped data
- Screenshot each verification

### Method K — Claude in Chrome (fallback)
- Direct Chrome control for anti-bot sites
- Same verification flow

### Acceptance Criteria
- Price match ±10% → CONFIRMED
- Mismatch >10% → FLAG, use browser-verified price
- Not found → UNVERIFIED (lower confidence)

---

## Phase 6: REPORT — Premium HTML via Delphi Reporter

Invoke: Delphi `/reporter` skill (Tier 2 — Full Report)
Template: `~/.claude/plugins/delphi/resources/templates/tier2-full-report.html`
Design: Inter + JetBrains Mono, glassmorphism, #0A0A0F dark, #58a6ff accent

### Report Sections
1. **Executive Summary** — winner, savings vs average, coverage score
2. **KPI Hero Bar** — cheapest total, best coverage %, biggest discount, providers compared
3. **Full Comparison Table** — sortable, color-coded (green=cheapest, red=most expensive per row)
4. **Price Heatmap** — Chart.js matrix: price distribution per item across providers
5. **Bundle Analysis** — packages vs individual pricing
6. **Active Promotions** — discount cards with validity dates
7. **Coverage Gaps** — items hard to find, alternative sources
8. **Provider Contact Details** — top 3 providers with: name, website, phone, address, hours, walk-in/appointment, online booking, online results, promo codes
9. **Recommendation** — where to go, split strategy, estimated total, who to call first

### Self-Audit (from reporter SKILL.md)
- [ ] Content traces to source URL
- [ ] Chart.js loads and displays
- [ ] Dark/light toggle works
- [ ] Responsive 375px + 1280px
- [ ] Print produces clean layout

Deploy: VPS `/nexus/price-intel-{date}.html`

---

## Phase 7: NOTIFY — Telegram Alert

```
🏥 Price Intelligence Alert

Best: {provider} — {total} RON for {X}/{Y} items
Savings: {savings} RON vs average ({pct}% cheaper)
Discount: {promo_name} (-{discount_pct}%, valid until {date})
Verification: {confirmed_count}/{total} prices browser-verified

[Full Report] | [Book Now: {phone}]
```

Bot: @claudemacm4_bot (Lis)

---

## Output Contract

```yaml
result:
  winner: {provider, total_ron, coverage_pct, discount_applied, phone, address}
  runner_up: {provider, total_ron, coverage_pct}
  split_strategy: {provider_a: [tests], provider_b: [tests], combined_total}
  coverage_matrix: [{item, provider_prices...}]
  promotions: [{provider, name, discount_pct, valid_until}]
  bundles: [{provider, bundle_name, tests_included, bundle_price, vs_individual}]
  verification: {confirmed_count, flagged_count, unverified_count}
  report_url: "http://89.116.229.189/nexus/price-intel-{date}.html"
  items_not_covered: [items no provider offers]
  provider_contacts:  # MANDATORY — top 3 providers
    - name: ""
      website: ""
      phone: ""
      address: ""
      hours: ""
      walk_in: true/false
      online_booking: true/false
      online_results: true/false
      promo_codes: []
      notes: ""  # e.g. "call to confirm prices — website says updating"
```

---

## Tools Used

| Tool | Phase | Purpose |
|------|-------|---------|
| brave_local_search | 1 | Local business discovery |
| brave_web_search | 1, 4 | Web search, coupon discovery |
| exa (web_search_advanced_exa) | 1, 2, 4 | Neural search, livecrawl, blog discovery |
| tavily_search | 1, 4 | Cross-provider promotions |
| tavily_extract | 2 | Deep page content extraction |
| tavily_map | 2 | Sitemap discovery for price pages |
| tavily_crawl | 2 | Full site crawl for hidden prices |
| agent-browser | 2, 5 | JS-heavy sites, verification screenshots |
| Claude_in_Chrome | 5 | Fallback browser verification |
| Delphi reporter | 6 | Premium HTML report |
| telegram-alert | 7 | Winner notification |

## Quality Gates
- Phase 1: ≥8 providers found (before dedup)
- Phase 2: ≥60% of provider×item cells filled
- Phase 3: ≥5 providers pass 80% coverage gate
- Phase 4: Price cross-reference: 2+ methods agree within ±10% for ≥50% of items
- Phase 5: ≥3 prices verified per top-3 provider
- Phase 6: Report renders, charts load, responsive works
- Phase 7: Telegram alert received

## Error Handling
- Tavily quota exhausted → skip to Exa
- Exa quota exhausted → skip to browser
- Browser blocked → flag provider as UNVERIFIED
- All methods fail for a provider → drop from ranking with note
- <5 providers pass filter → lower gate to 60%, warn user

---

## Lessons Learned (Blood Test Run — 2026-03-24)

### Phase 1 SCOUT
- Brave Local Search: QUOTA EXHAUSTED (2000/2000 free tier). Not reliable as primary channel.
- DuckDuckGo: RATE LIMITED after 1 failed query. Not reliable for Romanian queries.
- Exa Neural: ONLY WORKING CHANNEL. Returned 15 high-quality sources. MUST be primary, not secondary.
- FIX: Swap channel priority — Exa FIRST (primary), Brave/DDG as optional enrichment only.

### Phase 2 SCRAPE
- Exa livecrawl with includeDomains worked perfectly for MedicTest (full price list extracted).
- Synevo.ro is JS-heavy — livecrawl only got individual shop pages, not the full catalog. Browser agent needed for Synevo full list.
- infobase.ro comparison article was goldmine — 6 providers × 18 tests in one page. Always search for comparison/aggregator articles first.
- FIX: Add "comparison article" as Method 0 before per-provider scraping.

### Phase 5 VERIFY
- Chrome browser permission was denied for subagent. Agent-browser skill also needs explicit permission.
- Verification fell back to re-reading the webpage content (still valid, but no screenshot evidence).
- FIX: Document that browser verification requires user to grant Chrome MCP permission beforehand.

### Phase 7 NOTIFY
- lis-notification-token keychain entry was EMPTY. Used telegram-bot-token-claudemacm4 instead.
- FIX: Standardize on telegram-bot-token-claudemacm4 as the canonical token name.

### General
- MedicTest (small lab) was 45-71% cheaper than major chains. Small/specialist providers often beat national networks.
- Specialty tests (hs-CRP, IGF-1, Homocisteina, heavy metals) are only available at MedicTest and Synevo among checked providers. Major chains don't list them online.
- Bundle detection saved ~30 RON (Pachet General Barbati + Pachet Hormonal 1).
- Price discrepancy between aggregator (infobase) and direct provider (Synevo shop) was 82% for Hemoleucograma. Always verify with direct provider source.

### Test Run Stats
- Providers discovered: 15 (Exa) + 16 (seed) = 31 → 8 unique after dedup
- Tests priced: 33/33 (100% coverage via split strategy)
- Winner: MedicTest (~1,254 RON) + Synevo Zinc (119 RON) = 1,373 RON total
- Savings vs Synevo retail: 909 RON (40%)
- Pipeline execution time: ~15 minutes (7 phases)
- API costs: ~$0.03 (Exa only)

---

## Lessons Learned — Peptide Run (2026-03-24)

### Phase 1 SCOUT:
- 25 vendor URLs provided by user. After dedup: 25 unique.
- 2 vendors were wrong category (buymesotherapy.eu = cosmetic mesotherapy, not peptides)
- 1 vendor offline (pharmalabglobal.eu)
- 1 vendor all out of stock (peptidesireland.com)
- Net active vendors: 13/25 (52% yield)
- FIX: Add vendor pre-screening step — check if vendor actually sells the product category before scraping

### Phase 2 SCRAPE:
- Exa livecrawl worked for most vendors
- JS-heavy sites (CertaPeptides with React) returned no prices via Exa — need browser fallback
- PenPeptide.ro partially scraped (some products visible, many OOS)
- PLN/RON/GBP/EUR/USD currencies all present — normalization critical
- FIX: Should have used Tavily extract + Apify ecommerce + agent-browser in addition to Exa (not just Exa alone)
- FIX: Add "Method 0 — comparison article search" as discovered in blood test run (infobase.ro goldmine pattern)

### Phase 4 RANK:
- price_per_mg is THE metric for peptides — total price is misleading (2mg vs 50mg vials)
- Vial size dramatically affects per-mg cost: BioBoostX 100mg GHK-Cu at $0.65/mg vs 10mg at $4.00/mg
- Blends (BPC-157+TB-500) should be tracked separately from pure peptides
- CJC-1295 with DAC ≠ without DAC — correctly split as separate entries

### General:
- Polish (PLN) and Slovak (EUR) vendors consistently cheapest due to lower operating costs
- UK vendors (GBP) mid-range but reliable stock
- Romanian vendor (CellPeptides) widest selection but most expensive per mg
- German vendor (BioBoostX) best for large vial sizes
- Prescription peptides (Semaglutide, Tirzepatide) have highest price variance (3x-25x spread)
- 5 parallel scraping agents completed in ~6 minutes — good parallelization

### Test Run Stats:
- Vendors attempted: 25
- Vendors successfully scraped: 13
- Products cataloged: 300+
- Unique peptides: ~40
- Currencies: 5 (EUR, GBP, USD, PLN, RON)
- Cheapest vendor overall: Jacked.sk (7 peptide wins)
- Pipeline execution time: ~6 minutes (5 parallel batches)
- API costs: ~$0.06 (Exa only)

---

## Domain-Specific Notes

### Medical Labs
Reference: Blood test run (2026-03-24). Key findings:
- Small specialist labs (MedicTest) often 45-71% cheaper than major national chains (Synevo, MedLife, Regina Maria)
- Specialty tests (hs-CRP, IGF-1, Homocisteina, heavy metals) only available at specialist labs
- Comparison/aggregator articles (e.g. infobase.ro) are goldmines — check these FIRST (Method 0)
- Bundle detection critical — "Pachet General" + "Pachet Hormonal" saved ~30 RON
- JS-heavy sites (Synevo) need browser agent for full catalog — livecrawl only hits individual pages
- Seed list Tier 1 additions from live run: MedicTest (consistently beats national chains)

### Peptide Vendors
Reference: Peptide run (2026-03-24). Key findings:
- price_per_mg is THE metric — never compare by total price alone (vial sizes vary 2mg to 100mg)
- Pre-screen vendor URLs for correct product category before scraping (saves ~30% wasted calls)
- Polish (PLN) and Slovak (EUR) vendors consistently cheapest (lower operating costs)
- UK vendors reliable stock, mid-range pricing
- Blends (BPC-157+TB-500) track separately from pure compounds
- CJC-1295 with DAC vs without DAC = different products, different entries
- Prescription peptides (Semaglutide, Tirzepatide) show 3x-25x price spread — verify legality per country
- Cheapest benchmark: Jacked.sk (Slovak, 7 wins across peptide categories)
- Currency normalization mandatory: EUR/GBP/USD/PLN/RON all appear in same run

### E-commerce
- Shopify/WooCommerce stores: use `apify-ecommerce` scraper — handles pagination, variants, OOS filtering
- React/JS-heavy stores (e.g. CertaPeptides): Exa livecrawl returns no prices — use agent-browser fallback
- Comparison/aggregator articles frequently more data-dense than per-vendor scraping — always search Method 0 first

### Services
- TBD — add findings here after first service category run (insurance, repairs, etc.)
