---
id: TRAVEL-AGENT
version: "3.0"
created: 2026-03-12
updated: 2026-03-15 (v3.0-fixes — agent prompts, model routing fix, idempotency, regional routing, SOP-4 failure mode, ETIAS, TRAVEL-S-001 created)
author: Genie
status: DEPRECATED
deprecated_at: 2026-03-28
deprecated_reason: "Replaced by Travel Pro plugin (~/.claude/plugins/travel/) and Travel V2 agent (~/.nexus/v2/agents/travel/)"
successor: "~/.claude/plugins/travel/agents/travel.md"
scope: Genie, Lis
regulă_asociată: TRAVEL-S-001
forge_score: 3.15 (pre-fix audit 2026-03-15 — re-audit pending)
tags: [travel, flights, hotels, restaurants, search, procedure, multi-agent, DEPRECATED]
---

# [DEPRECATED] TRAVEL-AGENT v3.0 — End-to-End Travel Search Procedure

> **DEPRECATED 2026-03-28**: This procedure has been replaced by the Travel Pro plugin at `~/.claude/plugins/travel/`. Use `/travel` command or dispatch via GENIE instead. This file is kept for reference only.

---

## §0 Multi-Agent Architecture

### Agent Roles

```
Orchestrator (Sonnet — always fixed model)
├── IntakeAgent    (Haiku)   — Phase 0: constraints intake + memory load
├── FlightAgent    (Sonnet)  — Phase 1: multi-source flights
├── TrainAgent     (Haiku)   — Phase 2: trains/buses
├── HotelAgent     (Sonnet)  — Phase 3: accommodation
├── RestaurantAgent(Haiku)   — Phase 4: restaurants
├── CarAgent       (Haiku)   — Phase 5: car rental
├── ActivityAgent  (Haiku)   — Phase 6: attractions
├── VisaAgent      (Haiku)   — Phase 7: visa/ETIAS check (dispatched by Orchestrator)
├── SynthesisAgent (Opus)    — Phase 8: report + ranking final
└── ChangeMonitor  (Sonnet)  — SOP-4: post-booking PNR monitoring
```

**Orchestrator model is always Sonnet.** It dispatches sub-calls to the appropriate model per §0.5. The Orchestrator itself does not switch models — it delegates. Each agent receives a `phase_input` dict, executes its phase, and returns `phase_output`. The Orchestrator validates `dates_confirmed` and `city_confirmed` before passing downstream (see §4 Enforcement Loop step 6a).

### Agent Communication Protocol

- **Shared scratchpad**: `~/.nexus/state/travel-session-{uuid}.json` — written after each phase
- **Handoff format**: each agent appends to scratchpad under key `phases.{name}`
- **Inter-agent latency target**: <200ms (local; Telegram relay adds ~1s)
- **Error escalation**: agent returns `status: FAIL` → Orchestrator decides skip/retry/abort

---

## §0.5 Model Routing Table

| Phase | Task Complexity | Model | Rationale |
|-------|----------------|-------|-----------|
| P0 — Constraints intake | Simple | Haiku | Extraction only |
| P1 — Flights | High | Sonnet | Multi-source, cross-validation |
| P2 — Trains | Medium | Haiku | Simpler search, fewer sources |
| P3 — Hotels | High | Sonnet | Multi-source, price comparison |
| P4 — Restaurants | Low | Haiku | Local search, no price comparison |
| P5 — Cars | Low | Haiku | Simple price lookup |
| P6 — Activities | Low | Haiku | Discovery only |
| P7 — Visa | Low | Haiku | Fact lookup |
| P8 — Report synthesis | High | Opus | Ranking, HTML generation, final verdict |
| P9 — Delivery | Low | Haiku | Formatting, send |
| SOP-4 — Change monitor | Medium | Sonnet | PNR parse + diff logic |

**Cost impact**: ~55-60% reduction vs running all phases on Sonnet/Opus.

---

## §0.1 NexusOS Agent Mapping

Which real NexusOS agent owns each TRAVEL-AGENT phase. The virtual agent names in §0 are prompt templates — this table maps them to actual deployed agents.

| Phase | Virtual Agent | **NexusOS Agent** | Dispatch Method |
|-------|--------------|-------------------|-----------------|
| P0 Constraints + Profile | IntakeAgent (Haiku) | **LIS** (Telegram) / **GENIE** (CLI) | Lis classifies travel intent; Genie runs `travel-memory.py --load-profile` |
| P0.5 IRIS Pre-Check (STANDARD/COMPLEX only) | — | **DELPHI** | Genie writes `DELPHI-REQUEST.md` type=`travel-feasibility`; DELPHI returns route viability, seasonal intel, known issues |
| P1 Flights | FlightAgent (Sonnet) | **GENIE** (Sonnet subagent) + **DELPHI** (D3/D4 only) | Genie dispatches with FlightAgent prompt; DELPHI escalation if route complexity ≥ D3 |
| P2 Trains | TrainAgent (Haiku) | **GENIE** direct | Haiku-tier call, no delegation |
| P3 Hotels | HotelAgent (Sonnet) | **GENIE** (Sonnet subagent) | Genie dispatches with HotelAgent prompt; DELPHI only if "best neighborhood" D3 query |
| P4 Restaurants | RestaurantAgent (Haiku) | **GENIE** direct | Simple MCP + Brave |
| P5 Cars | CarAgent (Haiku) | **GENIE** direct | Simple Brave search |
| P6 Activities | ActivityAgent (Haiku) | **GENIE** direct | GetYourGuide + Viator Brave search |
| P7 Visa/ETIAS | VisaAgent (Haiku) | **GENIE** direct (or **DELPHI** if multi-country chain) | Haiku fact lookup; DELPHI if >2 transit countries |
| P8 Report Synthesis | SynthesisAgent (Opus) | **GENIE** → spawn **Opus subagent** | Genie passes all phase_outputs + SynthesisAgent prompt from §0.2 |
| P9 Delivery | — | **LIS** (Telegram) / **GENIE** (CLI) | LIS relays Telegram summary; Genie displays in CLI |
| SOP-4 Change Monitor | ChangeMonitorAgent (Sonnet) | **`change-monitor.py` daemon** monitored by **SENTINEL** | Script runs as LaunchAgent; SENTINEL owns health monitoring (5 checks — see §5) |

**Routing rules:**
- SIMPLE trip → Genie executes all phases sequentially, no DELPHI escalation
- STANDARD trip → Genie + optional DELPHI pre-check (P0.5) for feasibility intel
- COMPLEX trip → DELPHI pre-check mandatory; P1/P3 may escalate individual sub-questions to DELPHI
- DELPHI depth gate: D1/D2 → Genie/Lis direct; D3/D4 → write `DELPHI-REQUEST.md`, await `DELPHI-OUTPUT.md`

**OpenClaw**: Deferred. INSIGHTS (GPT-5.2) and FINANCE (GPT-5.2) have potential for competitive intel and trip budgeting, but cross-machine overhead not justified for personal travel. Revisit if commercial travel product is built.

---

## §0.2 Agent Prompt Templates

Minimum required prompts. Include these verbatim when dispatching each agent.

### FlightAgent (Sonnet)

```
You are FlightAgent, a specialized flight search agent. Your task:
1. Search for flights from {origin} to {destination} on {date} for {passengers} in {class}.
2. Query sources in this order: Kiwi MCP → Google Flights → Trip.com → Momondo (stop when scaling limit reached).
3. Cross-validate: NEVER present a price without at least 2 independent sources confirming within 30%.
4. For each result, set source_type (non-affiliate|neutral|affiliate) and record price_scraped_at as UTC ISO8601.
5. Apply the ranking formula from §2 Ranking Formula. Return top 5 ranked options.
6. Flag phantom routes (IndiGo CPH suspended), Kiwi virtual interlining (separate tickets — warn user), and any discrepancies >30%.
7. NEVER include passenger names, passport numbers, or loyalty IDs in output.
8. Return a complete phase_output dict per the Phase Output Schema.
Confidence: HIGH if 3+ sources within 10%. MEDIUM if 2 sources within 30%. LOW if single source.
```

### HotelAgent (Sonnet)

```
You are HotelAgent, a specialized accommodation search agent. Your task:
1. Search for hotels/accommodation in {city} from {check_in} to {check_out} for {guests}.
   Use dates_confirmed from Phase 1 output — do not re-ask.
2. Query sources in order: Expedia Rapid API → Booking.com → Airbnb MCP → Google Hotels.
3. Cross-validate prices across at least 2 sources. Flag discrepancies >30%.
4. Apply ranking formula (substitute Rating for Duration, Distance for Stops).
5. Record price_scraped_at and price_ttl_minutes=1440 for each result.
6. Flag cross-border notes and properties that require advance payment or have strict cancellation.
7. NEVER include guest names or payment details in output.
8. Return a complete phase_output dict.
```

### SynthesisAgent (Opus)

```
You are SynthesisAgent, responsible for producing the final travel report. Your task:
1. Receive all phase_output dicts from completed phases.
2. Validate each phase: status OK or PARTIAL? If FAIL, note in report with reason.
3. Apply the global ranking formula across all phases — surface the best overall trip combination.
4. Check price freshness: flights >30 min old → add stale warning callout. Hotels >24h → mark indicative.
5. Generate a complete HTML report using publish_html() per the Phase 8 template in §2.
6. Include: metric_grid, callout, ranked recommendations, full data table with badges,
   CO2 section, excluded options, search parameters, sources + methodology, agent execution log.
7. Affiliate-sourced prices must be clearly labeled. Non-affiliate prices get precedence in ranking.
8. ZERO PII in the report — no names, no passport numbers, no payment details.
9. Add agent execution log: list each phase, agent, model, duration_s, sources_used.
10. Upload to VPS via publish_html() and return the report URL.
```

### IntakeAgent (Haiku)

```
You are IntakeAgent. Extract travel constraints from the user message and load any existing travel profile.
1. Parse: origin, destination, dates, passengers, class, budget, max duration, avoid regions, baggage.
2. Run: cortex search --tags travel-profile --limit 1 "travel preferences {user}"
   If profile found: pre-fill missing constraints from profile. State all assumptions.
3. Determine complexity: SIMPLE (single route, fixed dates, 1-2 pax) | STANDARD (multiple routes OR flexible dates) | COMPLEX (multi-city, large group, budget-critical).
4. Initialize session scratchpad at ~/.nexus/state/travel-session-{uuid}.json
5. Check for incomplete sessions: if a scratchpad for same route+dates exists, offer to resume or start fresh.
6. Return structured constraints dict + complexity level.
```

### VisaAgent (Haiku)

```
You are VisaAgent. Check visa and entry requirements for the trip.
1. For each destination country, search: "{nationality} visa {destination_country} {year} requirements"
2. Check ETIAS status for EU/Schengen: search "ETIAS enforcement 2026 {destination_country}"
3. Flag transit visa requirements for layover countries.
4. If any visa or ETIAS is required, add a warning callout to the phase_output.
5. Return phase_output with visa_required (bool), etias_required (bool), details, and source URLs.
```

---

## §1 Problema

Without a standardized travel search procedure:
- Results come from a single source (usually Google Flights), missing cheaper options on LCCs, virtual interlining, or regional OTAs
- Prices are presented as verified when they come from one scrape — user books based on stale or wrong data
- No cross-validation between sources means phantom prices (e.g., IndiGo CPH suspended routes) leak into recommendations
- Report format varies per session — sometimes plain text, sometimes partial HTML — making comparisons across trips impossible
- Passenger PII (names, passport numbers, loyalty IDs) can leak into logs, reports, or Cortex entries if not handled carefully
- No structured handoff between phases — flight results don't inform hotel date ranges, restaurant city selection, etc.
- User preferences not retained across sessions — same constraints re-asked every trip

Situations covered:
- Flight search (one-way, round-trip, multi-city)
- Train/bus search (European, Asian networks)
- Hotel, villa, Airbnb accommodation search
- Restaurant discovery and reservation prep
- Car rental search (brief)
- Attractions and activities lookup
- Combined trip planning (all phases)
- Visa/ETIAS eligibility checks (optional)
- Change monitoring for confirmed bookings (SOP-4)

---

## §2 Procedura

### PHASE 0: Constraints Intake + Memory Load

Before any search:

**Step 0a — Load user travel profile from Cortex:**
```bash
# Check for existing user profile
cortex search --tags travel-profile --limit 1 "user preferences travel {user_name}"
# If found: pre-fill defaults (preferred airlines, class, budget range, seat preference, home airport)
# If not found: start fresh, will save after session
```

**Step 0b — Extract or ask for constraints:**

| Parameter | Required | Example |
|-----------|----------|---------|
| Origin(s) | YES | BKK, HKT, KBV |
| Destination(s) | YES | ARN, CPH, GOT, OSL |
| Date(s) | YES | Mar 13-15, flexible +-3d |
| Type | YES | one-way / round-trip / multi-city |
| Passengers | YES | 1 adult, 2 adults + 1 child |
| Class | YES | economy / business |
| Budget cap | RECOMMENDED | <EUR 1,201 |
| Max duration | RECOMMENDED | <30h |
| Avoid regions | OPTIONAL | no Middle East transit |
| Preferred airlines | OPTIONAL | Norse Atlantic, SAS |
| Baggage needs | OPTIONAL | carry-on only / checked |

If user provides incomplete constraints, infer reasonable defaults from travel profile (if exists) and state assumptions.

**Step 0b.5 — DELPHI Pre-Check (STANDARD/COMPLEX trips only):**

Before launching search phases, Genie writes an IRIS feasibility request:
```bash
# Write IRIS-REQUEST.md for route feasibility intel
cat > ~/.nexus/workspace/IRIS-REQUEST.md << EOF
type: travel-feasibility
priority: D2
route: {ORIGIN} → {DEST}
dates: {DATE_RANGE}
questions:
  - Are there known route suspensions or reliability issues on this route?
  - What is the typical price range and best booking window for this route/month?
  - Any regional entry requirements, strikes, or disruptions to flag?
  - Which source (Kiwi/GF/Trip.com) has best coverage for this route?
requested_by: TRAVEL-AGENT P0.5
max_wait: 5min
EOF
```
IRIS returns `IRIS-OUTPUT.md` with feasibility intel → Genie injects into FlightAgent context before P1.
Skip this step for SIMPLE trips or when user uses keyword `acum`/`urgent`.

**Step 0c — Initialize session scratchpad (with dedup check):**
```bash
# Check for incomplete sessions for same route+dates
EXISTING=$(ls ~/.nexus/state/travel-session-*.json 2>/dev/null | xargs grep -l '"route":"[ORIGIN]-[DEST]"' 2>/dev/null | head -1)
if [ -n "$EXISTING" ]; then
  # Offer to resume or start fresh
  echo "Incomplete session found: $EXISTING — resume or start fresh?"
fi

# Initialize new scratchpad (atomic write: write to .tmp then mv)
TMP=$(mktemp ~/.nexus/state/travel-session-XXXXXX.tmp)
cat > "$TMP" << 'EOF'
{
  "session_id": "{uuid}",
  "user": "{name}",
  "route": "{ORIGIN}-{DEST}",
  "created_at": "YYYY-MM-DDTHH:MM:SSZ",
  "complexity": "SIMPLE|STANDARD|COMPLEX",
  "phases": {}
}
EOF
mv "$TMP" ~/.nexus/state/travel-session-{uuid}.json
```

**Resource Scaling** (determines how many sources to query per phase):

| Complexity | Criteria | Sources per phase |
|------------|----------|-------------------|
| SIMPLE | Single route, fixed dates, 1-2 passengers | 2-3 sources |
| STANDARD | Multiple routes OR flexible dates OR families | 4-5 sources |
| COMPLEX | Multi-city, large group, special requirements, budget-critical | All available sources (6-7) |

**Phase Timeout** (prevents runaway searches):

| Complexity | Per-phase timeout | Total search cap |
|------------|-------------------|------------------|
| SIMPLE | 2 min | 10 min |
| STANDARD | 5 min | 20 min |
| COMPLEX | 10 min | 30 min |

If timeout exceeded, deliver partial results with "TIMEOUT — partial results" callout.

**Phase Output Schema** — each phase MUST output a structured dict:

```
phase_output = {
    "phase": "flights|trains|hotels|restaurants|cars|attractions",
    "status": "OK|PARTIAL|FAIL",
    "results_count": int,
    "sources_used": ["Google Flights", "Kiwi.com", ...],
    "sources_failed": ["Amadeus (429)", ...],
    "top_pick": { ... },
    "all_options": [ ... ],
    "dates_confirmed": "YYYY-MM-DD to YYYY-MM-DD",
    "city_confirmed": "Stockholm",
    "warnings": ["IndiGo route suspended", ...],
    "confidence": "HIGH|MEDIUM|LOW",
    "ranking_scores": [ {"option": "...", "score": 0.85, "rank": 1}, ... ],
    "feeds_into": ["hotels", "restaurants"],
    "price_scraped_at": "YYYY-MM-DDTHH:MM:SSZ",
    "price_ttl_minutes": 30,
    "model_used": "haiku|sonnet|opus",
    "agent": "FlightAgent|HotelAgent|..."
}
```

### Ranking Formula (applies to all phases)

Default weighting for sorting recommendations:

| Factor | Weight | Notes |
|--------|--------|-------|
| Price | 0.38 | Normalized to 0-1 scale against cheapest option |
| Duration | 0.24 | Total travel time including layovers |
| Stops | 0.14 | 0 stops = 1.0, 1 stop = 0.6, 2+ stops = 0.3 |
| Departure time | 0.10 | User-preferred window = 1.0, off-peak = 0.5, red-eye = 0.3 |
| Emissions | 0.09 | Lower = better, normalized against route average |
| Source trust | 0.05 | Non-affiliate = 1.0, Neutral = 0.8, Affiliate = 0.5 |

**Source Classification for Trust Factor**:

| Source | Type | Trust Score |
|--------|------|-------------|
| Google Flights (direct scrape) | Non-affiliate | 1.0 |
| Airline website (direct) | Non-affiliate | 1.0 |
| Amadeus API | Non-affiliate | 1.0 |
| Duffel API | Non-affiliate | 1.0 |
| Momondo | Non-affiliate | 1.0 |
| Kiwi.com (organic) | Neutral | 0.8 |
| Trip.com | Neutral | 0.8 |
| Omio / Trainline | Neutral | 0.8 |
| Expedia Rapid | Neutral | 0.8 |
| Kiwi Tequila (affiliate) | Affiliate | 0.5 |
| Travelpayouts | Affiliate | 0.5 |
| SerpAPI | Affiliate | 0.5 |
| Skyscanner Partners | Affiliate | 0.5 |

User can override weights via constraints intake (e.g., "I care most about duration").
Score = sum(factor * weight). Top 3-5 by score = recommendations.
For non-flight phases: replace Duration with Rating, Stops with Distance, apply Source Trust universally.

### Price Freshness (TTL) Rule

Every price in the report carries a `price_scraped_at` timestamp. TTL by category:

| Category | TTL | Action if expired |
|----------|-----|-------------------|
| Flights | 30 min | Re-scrape top 3 before presenting |
| Hotels | 24 hours | Mark as "indicative" if >24h old |
| Activities | 48 hours | Acceptable as-is |
| Car rental | 24 hours | Mark as "indicative" if >24h old |

If report delivery is delayed beyond TTL for flights, add to Phase 8 report:
```
body += callout("FLIGHT PRICES STALE (>30 min) — re-run Phase 1 for fresh data before booking", "warning")
```

**NEVER cache flight prices across sessions. Always re-scrape at booking step.**

---

### PHASE 1: Multi-Source Flight Search

**Agent**: FlightAgent (Sonnet)

#### 1.1 Source Priority (use ALL available based on scaling level)

| Priority | Source | Method | Free? | Best For |
|----------|--------|--------|-------|----------|
| 1 | **Kiwi MCP** | MCP server (preferred) | YES | Virtual interlining, LCCs — replaces manual scraping |
| 2 | **Google Flights** | Playwright scraping (see 1.2) | YES | Broadest airline coverage, price insights |
| 3 | **Trip.com** | Brave Search + fetch | YES | Asian airlines, budget options |
| 4 | **Momondo** | Brave Search extraction | YES | Meta-search, hidden deals |
| 5 | **Skyscanner** | MCP server (if installed) | YES | Calendar view, price trends |
| 6 | **Amadeus API** | REST API (if key available) | FREE tier | GDS fares, seat maps, ancillaries |
| 7 | **Duffel API** | REST API (if key available) | FREE search | NDC fares |

**v3.0 change**: Kiwi MCP is now Priority 1 — use it before Playwright scraping. Playwright is fallback.

#### 1.1b Regional Source Priority Modifier

Override the global priority table based on destination region:

| Region | Bump Up | Bump Down | Rationale |
|--------|---------|-----------|-----------|
| Asia-Pacific (TH, SG, JP, KR, VN, MY) | Trip.com → P2, AirAsia.com → add as P3 | Momondo → P5 | Asian LCCs better covered by Trip.com |
| Europe (EU/Schengen) | Momondo → P2, Skyscanner → P3 | Trip.com → P6 | European meta-search stronger here |
| Middle East / Gulf | Flydubai.com → add P3, Jazeera.com → P4 | Trip.com → P5 | Regional LCCs not on Kiwi/Google |
| Americas | Kayak → add P3 | Trip.com → P6 | US/LATAM routes better on Kayak |
| Africa | Travelstart.co.za → add P3 | Duffel → P4 | African routes limited on GDS |

Apply modifier after determining route origin/destination. Combined with scaling level (SIMPLE/STANDARD/COMPLEX) to set final source list.

#### 1.2 Google Flights Scraping (Playwright — fallback if Kiwi MCP unavailable)

```python
# Standard Google Flights scraping pattern
# IMPORTANT: Always use a CLEAN browser context — no persistent cookies, no geo leak

browser = playwright.chromium.launch(headless=True)
context = browser.new_context(
    locale='en-US',
    timezone_id='UTC',
    geolocation=None,
    permissions=[],
    color_scheme='light',
    # No storage_state — fresh session every run, no cookie accumulation
)
page = context.new_page()

url = f"https://www.google.com/travel/flights?q=Flights+from+{origin}+to+{dest}+on+{date}+one+way&curr=EUR&hl=en&gl=us"

# Handle consent wall → Wait 6-8s → Extract → Parse
# Record price_scraped_at = datetime.utcnow().isoformat() + "Z"
context.close()
browser.close()
```

Script location: `~/.nexus/scripts/travel/gf-search.py`

Key: **Always use `browser.new_context()` with no `storage_state`** — logged-in or cookie-tracked sessions can show inflated prices.

#### 1.3 MCP Servers

| MCP Server | Install | Auth | Status |
|------------|---------|------|--------|
| **Kiwi.com MCP** | HTTP: `https://mcp.kiwi.com` | None | ✅ INSTALLED (2026-03-15) |
| **Airbnb MCP** | HTTP: `https://mcp-server-airbnb.openbnb.com` | None | ✅ INSTALLED (2026-03-15) |
| **Amadeus MCP** | github.com/donghyun-chae/mcp-amadeus | Amadeus API key | ⏳ pending key |
| **Google Flights MCP** | github.com/HaroldLeo/google-flights-mcp | Varies | ⏳ pending |
| **Restaurant MCP** | github.com/jrklein343-svg/restaurant-mcp | None (Resy+OpenTable) | ⏳ optional |
| **Expedia Rapid MCP** | Expedia Rapid API (700K+ properties) | API key | ⏳ pending key |

#### 1.4 Brave/Tavily Search Patterns

**Brave Search** (use IATA codes in quotes for OTA result cards):

| Scenario | Query Template |
|----------|----------------|
| Standard one-way | `"[IATA_ORIGIN]" to "[IATA_DEST]" one way [MONTH] [YEAR] flight price [CURRENCY] economy` |
| Round-trip | `"[IATA_ORIGIN]" to "[IATA_DEST]" round trip [DEPART_MONTH] [RETURN_MONTH] [YEAR] flight [CURRENCY]` |
| Flexible dates | `cheapest flight "[IATA_ORIGIN]" "[IATA_DEST]" [MONTH] [YEAR] flexible dates` |
| Nearby airports | `flight "[ORIGIN_CITY]" to "[DEST_CITY]" [MONTH] [YEAR] [CURRENCY] site:kiwi.com OR site:momondo.com` |
| LCC-specific | `[AIRLINE_NAME] "[IATA_ORIGIN]" "[IATA_DEST]" [DATE] price [CURRENCY]` |

**Tavily Search** (always use `search_depth=advanced`):

| Scenario | Query | Extra params |
|----------|-------|-------------|
| Standard | `[ORIGIN_CITY] to [DEST_CITY] one way flight [YYYY-MM-DD] economy price [CURRENCY]` | `include_domains=["trip.com","kiwi.com","skyscanner.com","momondo.com"]` |
| Price comparison | `cheapest flight [ORIGIN_CITY] to [DEST_CITY] [MONTH] [YEAR] compare prices` | `include_domains=["skyscanner.com","kayak.com","momondo.com"]` |

**Query Rules**:
1. Use IATA codes in quotes for Brave (triggers OTA cards), city names for Tavily (better semantic matching)
2. Always include currency to avoid results in wrong currency
3. For routes with known LCCs (SE Asia: AirAsia, Scoot; Europe: Ryanair, Wizz, Norse), add separate LCC-specific query
4. If first query returns <3 results with prices, broaden: drop "economy", drop date specificity, add "cheapest"
5. Use `site:` operator for Brave when targeting specific OTAs

---

### PHASE 2: Train Search

**Agent**: TrainAgent (Haiku)

#### Sources

| Source | Coverage | Method |
|--------|----------|--------|
| **Omio** | 37 countries, 1000+ operators | Brave Search / Web scraping |
| **Trainline** | 270+ operators, 40 countries | B2B API (if available) |
| **Rome2Rio** | Global multimodal | Brave Search |
| **National rail sites** | Country-specific | Direct fetch (SJ.se, NSB.no, DSB.dk) |

#### Pattern
Search Brave: `"[ORIGIN CITY]" to "[DEST CITY]" train [DATE] price`
Fetch Omio/Rome2Rio results pages for verified times and prices.

---

### PHASE 3: Hotel / Villa / Accommodation Search

**Agent**: HotelAgent (Sonnet)

#### Sources

| Priority | Source | Type | Method |
|----------|--------|------|--------|
| 1 | **Expedia Rapid API** | Hotels/All | REST API (700K+ properties) — v3.0 priority upgrade |
| 2 | **Booking.com** | Hotels | Brave Search + MCP |
| 3 | **Airbnb** | Apartments/Villas | MCP server (openbnb-org) or Apify scraper |
| 4 | **Google Hotels** | Aggregator | Playwright scraping (same consent pattern) |
| 5 | **Hostelworld** | Budget | Brave Search |

#### Search Pattern
```
Expedia Rapid: GET /properties/availability?location={city}&checkIn={date}&checkOut={date}&adults={n}
Brave: "[CITY] hotel [CHECK-IN] to [CHECK-OUT] [BUDGET] per night"
Airbnb MCP: search_listings(location="[CITY]", check_in="YYYY-MM-DD", check_out="YYYY-MM-DD", guests=N)
```

Use `dates_confirmed` from Phase 1 output to set check-in/check-out automatically.

---

### PHASE 4: Restaurant Search

**Agent**: RestaurantAgent (Haiku)

#### Sources

| Source | Method |
|--------|--------|
| **Restaurant MCP** | Resy + OpenTable unified search |
| **Google Maps/Places** | API or Brave Search |
| **TripAdvisor** | Brave Search |
| **TheFork** | Brave Search (EU) |

#### Pattern
```
Brave: "best restaurants [CITY] [CUISINE TYPE] reservation [DATE]"
Restaurant MCP: search(location="[CITY]", cuisine="[TYPE]", date="YYYY-MM-DD", party_size=N)
```

Use `city_confirmed` from Phase 1/3 output for location.

---

### PHASE 5: Car Rental Search (Optional)

**Agent**: CarAgent (Haiku)

#### Sources

| Source | Method |
|--------|--------|
| **Rentalcars.com** | Brave Search |
| **Kayak Cars** | Brave Search |
| **AutoEurope** | Brave Search |
| **Local providers** | Brave Search (e.g., Sixt, Europcar direct) |

#### Pattern
```
Brave: "car rental [CITY] [PICKUP DATE] to [RETURN DATE] economy"
```

Note: Cross-border fees vary significantly in Europe. Always flag if trip crosses borders.

---

### PHASE 6: Attractions & Activities

**Agent**: ActivityAgent (Haiku)

#### Sources

| Source | Method |
|--------|--------|
| **GetYourGuide** | Brave Search |
| **Viator** | Brave Search |
| **Google Things to Do** | Playwright |
| **TripAdvisor** | Brave Search |

---

### PHASE 7: Visa / ETIAS Check (Optional)

**Agent**: VisaAgent (Haiku) — dispatched by Orchestrator

If traveler nationality is known:
1. Check visa requirements for destination country via Brave Search: `"[NATIONALITY] visa [DESTINATION COUNTRY] 2026"`
2. For EU destinations: check ETIAS requirement status — **as of March 2026, ETIAS is actively enforced for non-EU/EEA nationals entering Schengen zone; verify current exemptions and fee (€7) via search before reporting**
3. Flag any transit visa requirements (e.g., UK transit, US ESTA for connections)
4. Add to report as callout if visa/ETIAS is needed

---

### PHASE 8: Report Generation (MANDATORY)

**Agent**: SynthesisAgent (Opus)

Every travel search MUST produce an HTML report via `publish_html()`.

```python
# CORRECT IMPORT — use publish_html from travel scripts, NOT report_template from lib
import sys; sys.path.insert(0, os.path.expanduser("~/.nexus/scripts/travel"))
from publish_html import publish_html

body = ""
body += metric_grid(
    metric_card("Cheapest", "EUR XXX", delta="[route] [airline]"),
    metric_card("Best Value", "EUR XXX", delta="[route] [airline]"),
    metric_card("Nonstop", "EUR XXX", delta="[route] [airline]"),
    metric_card("Sources", "N", delta="[list]"),
)
body += callout("MULTI-SOURCE VERIFIED -- [sources] [date]", "info")
body += section("Top Recommendations", "<ol>...</ol>")
body += section("All Options", data_table(headers, rows, html_cols=[N]))
body += section("CO2 Emissions", "[per-flight kg CO2, comparison to train if applicable]")
body += section("Excluded Options", "...")
body += section("Search Parameters", "...")
body += section("Sources & Methodology", "...")
body += section("Agent Execution Log", "[phases completed, models used, per-phase timing]")

url = publish_html(
    title="[ROUTE] — Verified Travel Prices",
    body_html=body,
    local_path=os.path.expanduser("~/.nexus/reports/travel-[slug]-[date].html"),
    meta={"generator": "Genie/Lis v3.0", "date": "YYYY-MM-DD", "source": "[sources]"}
)
```

#### Report Requirements
- metric_grid with key stats
- callout with verification note
- Ranked recommendations (numbered list)
- Full data table with badges (CHEAPEST, BEST VALUE, NONSTOP, ME TRANSIT)
- CO2 emissions section
- Excluded options section with reasons
- Search parameters section
- Sources with methodology
- **NEW v3.0**: Agent Execution Log (phase timings, models used, sources hit)

---

### PHASE 9: Delivery

#### Telegram (Lis)
Send summary message with:
1. Top 3-5 recommendations (price, route, airline, duration)
2. Key warnings/alerts
3. Report URL

#### Claude Code (Genie)
1. Display summary in chat
2. Confirm report URL uploaded to VPS
3. Note: "Prices valid as of [datetime]. Book soon — may change."

---

### SOP-4: Change Monitoring (NEW v3.0)

**Agent**: ChangeMonitorAgent (Sonnet) | **Trigger**: after confirmed booking

**Purpose**: Monitor confirmed PNRs for price drops, schedule changes, cancellations.

#### Step 1 — Baseline snapshot
After booking confirmed, save to scratchpad:
```json
{
  "pnr": "XXXXXX",
  "route": "BKK-ARN",
  "flight": "TG960",
  "departure": "2026-04-15T08:30:00+07:00",
  "price_paid": 1150.00,
  "currency": "EUR",
  "snapshot_at": "2026-03-15T10:00:00Z"
}
```

#### Step 2 — Polling schedule

| Check | Frequency | Action |
|-------|-----------|--------|
| Price | Daily (T-30d to T-7d) | Alert if drop >10% from paid |
| Schedule | Every 6h (T-7d to day of) | Alert immediately if >30min change |
| Cancellation | Every 6h (T-7d to day of) | Alert immediately + re-search alternatives |

#### Step 3 — Alert format (Telegram)
```
✈️ TRAVEL ALERT: [ROUTE]
Type: PRICE DROP / SCHEDULE CHANGE / CANCELLATION
Details: ...
Action: [rebook link / contact airline]
```

#### Step 4 — Cortex log
Save each monitoring event to Cortex with tag `travel-monitor`.

#### Step 5 — SOP-4 Failure Modes

| Failure | Action |
|---------|--------|
| PNR lookup fails (airline API down) | Log failure, retry in 1h; after 3 consecutive failures → Telegram alert "MONITOR DEGRADED" |
| Script crashes / stops polling | LaunchAgent auto-restarts; if 2 crashes in 1h → send Telegram alert "MONITOR DOWN" + log to `~/.nexus/logs/travel-monitor-errors.log` |
| Stale monitoring (departure passed) | Auto-archive: move `travel-monitor-{pnr}.json` to `~/.nexus/state/archive/`; stop polling |
| Alert send fails (Telegram down) | Fallback: write to `~/.nexus/logs/travel-monitor-pending-alerts.log` for manual review |
| Max consecutive poll failures | After 5 consecutive failures on any PNR → mark monitor as SUSPENDED, alert user, do not delete snapshot |

**Watchdog**: The LaunchAgent (`com.nexus.travel-monitor`) includes a heartbeat check — if the script has not written to its log in >13h, the LaunchAgent restarts it.

**Note**: SOP-4 implementation script: `~/.nexus/scripts/travel/change-monitor.py` (pending — Codex m4-519)

---

### Price Cross-Validation Rule (MANDATORY — ALL PHASES)

**NEVER present a price from a single source as verified.**

- Minimum 2 independent sources per route/date/option
- If prices differ >30%, flag discrepancy and note both
- Mark source for each price
- Convert all prices to user's preferred currency with date of conversion
- Applies to: flights, trains, hotels, car rentals, activities — not just flights

---

### Phase-Level Failure Behavior

| Scenario | Action |
|----------|--------|
| Phase returns 0 results from all sources | Mark phase as FAIL, skip from report, notify user with reason |
| Phase returns results from <2 sources | Mark as PARTIAL, include results but add "UNVERIFIED — single source" warning |
| Phase hits rate limits on all sources | Mark as FAIL, suggest retry in 30 min, proceed with other phases |
| Non-critical phase fails (restaurants, attractions) | Continue with remaining phases, note gap in report |
| Critical phase fails (flights for flight search) | STOP, report failure, suggest alternative approach |
| MCP unavailable | Fall back to Playwright/Brave, log "MCP_FALLBACK" in phase_output |

---

### Error Handling

| Error | Action |
|-------|--------|
| Google Flights consent wall | Playwright force-click Accept all button |
| API rate limit (429) | Wait 30s, retry once. If fails, skip source, note in report |
| No results for route | Expand search: nearby airports, +-3 days, remove constraints one at a time |
| Price mismatch >30% between sources | Flag in report with both prices, recommend user verify on booking site before purchasing |
| Playwright not installed | See §5 Dependencies for correct venv install |
| MCP server not available | Fall back to Brave Search + fetch |
| VPS upload fails | Save locally, provide local path, retry VPS later |
| Kiwi MCP unavailable | Fall back to Kiwi Tequila API or Brave Search |

---

## §3 Cortex Logging

### Travel Search Session Log

Every travel search session MUST log to Cortex. **Dedup check before saving**:

```bash
# Check for duplicate session log before saving
EXISTING=$(cortex search --tags travel,search --limit 1 "route:[ORIGIN]-[DEST] dates:[YYYY-MM-DD]" 2>/dev/null)
if [ -n "$EXISTING" ]; then
  # Update existing entry instead of creating duplicate
  cortex update "$EXISTING" --meta '{"updated_at": "YYYY-MM-DDTHH:MM:SSZ"}'
  exit 0
fi
```

```bash
cortex save --tags travel,search,[phase-tags] \
  --meta '{
    "type": "travel-search",
    "route": "[ORIGIN]-[DEST]",
    "dates": "[YYYY-MM-DD]",
    "phases_completed": ["flights","hotels","restaurants"],
    "phases_failed": [],
    "sources_used": ["Kiwi MCP","Google Flights","Expedia Rapid"],
    "cheapest_found": "EUR XXX [airline/provider]",
    "report_url": "[VPS URL]",
    "report_local": "~/.nexus/reports/travel-[slug].html",
    "complexity": "SIMPLE|STANDARD|COMPLEX",
    "passengers": "N adults, N children",
    "models_used": {"flights": "sonnet", "hotels": "sonnet", "report": "opus"},
    "total_duration_seconds": N
  }' \
  "Travel search: [ORIGIN] to [DEST] [DATE] — [N] options found, cheapest EUR [X]"
```

### User Travel Profile (Persistent Memory — NEW v3.0)

After every session, update or create user travel profile in Cortex:

```bash
cortex save --tags travel-profile,user-prefs \
  --meta '{
    "type": "travel-profile",
    "user": "[name]",
    "home_airport": "[IATA]",
    "preferred_class": "economy|business",
    "preferred_airlines": ["SAS", "Norse Atlantic"],
    "budget_range_eur": {"min": 500, "max": 1500},
    "seat_preference": "aisle|window",
    "baggage": "carry-on|checked",
    "avoid": ["Middle East transit"],
    "loyalty_programs": [],
    "updated_at": "YYYY-MM-DD"
  }' \
  "Travel profile: [user] — updated [date]"
```

**PII/Data Handling (MANDATORY)**:
- NEVER store passenger names, passport numbers, loyalty IDs, or payment details in Cortex, logs, or reports
- Use generic labels: "2 adults + 1 child (age 8)" instead of names
- If user provides PII in chat, acknowledge but do not echo it back in logs or reports
- GDPR awareness: travel data is personal data under GDPR. Reports shared via VPS should not contain PII
- Scraping sessions should not store cookies with user login sessions
- Travel profile stores only preferences, never booking details

---

## §4 Enforcement Loop

**WHERE**: Triggered when user requests any travel-related search (flights, hotels, trains, restaurants, trip planning, or change monitoring).

**WHEN**: At session start when travel intent is detected. Re-check at each phase transition.

**HOW**:
1. Detect travel intent in user message (keywords: flight, hotel, travel, trip, vacation, book, reservation, train, restaurant + destination)
2. Load this procedure (TRAVEL-AGENT v3.0)
3. Execute Phase 0 via IntakeAgent (Haiku) — do not skip; includes session dedup check
4. Determine complexity level (SIMPLE/STANDARD/COMPLEX)
5. Route each phase to correct agent per §0.5 Model Routing Table
6. Execute phases sequentially, writing `phase_output` atomically to scratchpad after each
6a. **Input validation before each phase dispatch**: verify `dates_confirmed` is valid ISO date range, `city_confirmed` is non-empty string. If invalid → re-query upstream phase, do not pass corrupt data downstream.
7. Cross-validate prices (minimum 2 sources) before marking any price as verified
8. Apply regional source priority modifier (§2 Phase 1 §1.1b) based on destination region
9. Generate HTML report (Phase 8, Opus) — mandatory, no exceptions
10. Deliver via appropriate channel (Phase 9)
11. Log session to Cortex (§3)
12. Update user travel profile in Cortex (§3)

**CONNECT**:
- Feeds into: `~/.nexus/scripts/travel/publish_html.py` (MANDATORY — NOT report_template.py from lib), Cortex (logging + profile), VPS upload pipeline, SOP-4 (if booking confirmed)
- Receives from: User constraints (chat/Telegram), MCP servers (data), Brave/Tavily (search), Cortex travel profile
- Related procedures: SOURCE-SELECTION-FRAMEWORK, FORGE-AUDIT (error correction)

**VERIFY**:
- [ ] Phase 0 completed — constraints present + travel profile loaded or created
- [ ] Complexity level determined and source count matches
- [ ] Model routing applied per §0.5 table
- [ ] At least 2 sources queried per active phase
- [ ] Cross-validation applied to all prices
- [ ] Price TTL checked — stale prices marked or re-scraped
- [ ] No PII in Cortex entry, report, or logs
- [ ] HTML report generated and accessible
- [ ] Delivery completed (chat or Telegram)
- [ ] Cortex session log saved
- [ ] User travel profile updated
- [ ] Scratchpad cleared after session

---

## §5 Observability & Debugging (NEW v3.0)

### Agent State Logging

Each agent MUST write to `~/.nexus/logs/travel-agent-{date}.log`.
Note: specific scripts use their own log paths:
- `travel-model-router.py` → `~/.nexus/logs/travel-model-usage-{date}.jsonl`
- `change-monitor.py` → `~/.nexus/logs/travel-monitor-{date}.log`

```
[2026-03-15T10:01:23Z] [FlightAgent/sonnet] phase=1 status=OK sources=3 results=12 duration=18.3s
[2026-03-15T10:01:41Z] [HotelAgent/sonnet] phase=3 status=OK sources=2 results=24 duration=12.1s
[2026-03-15T10:01:53Z] [SynthesisAgent/opus] phase=8 status=OK report_url=http://... duration=8.4s
```

### KPI Targets

| Metric | Target | Alert if |
|--------|--------|---------|
| Phase accuracy | ≥95% | <90% |
| Task completion | ≥90% | <80% |
| Inter-agent handoff | <200ms | >500ms |
| Total session time (STANDARD) | <20 min | >25 min |
| Price freshness compliance | 100% | any stale price delivered without warning |

### Health Check

Run after each session:
```bash
# Verify scratchpad was cleared
ls ~/.nexus/state/travel-session-*.json 2>/dev/null | wc -l  # should be 0 after session ends

# Check log for errors
grep -c "status=FAIL" ~/.nexus/logs/travel-agent-$(date +%Y-%m-%d).log
```

### SENTINEL Integration (SOP-4 Daemon Monitoring)

SENTINEL (`/Users/pafi/.nexus/agents/sentinel/`) owns the following 5 health checks for `change-monitor.py`:

| Check | Method | Trigger | Alert |
|-------|--------|---------|-------|
| Daemon heartbeat | Last write time of `~/.nexus/logs/travel-monitor-{date}.log` | Stale >13h | "TRAVEL MONITOR: heartbeat stale >13h" |
| Consecutive failures | Parse log for `status=FAIL` on same PNR | 5 consecutive | "TRAVEL MONITOR: PNR {X} suspended after 5 failures" |
| State file integrity | JSON parse `~/.nexus/state/travel-monitor-*.json` | Parse error | "TRAVEL MONITOR: corrupt state file {file}" |
| Archive cleanup | Check if any monitor still active for departure date in past | Departure passed | Log warning + auto-archive |
| LaunchAgent health | `launchctl list com.nexus.travel-monitor` | Not loaded | "TRAVEL MONITOR: LaunchAgent not loaded — restart required" |

These checks are added to SENTINEL's existing health loop via Codex m4-520.

---

## §6 Dependencies

### Tool Installation (Correct Method)

```bash
# Playwright — use venv, NOT --break-system-packages
python3 -m venv ~/.nexus/venvs/travel
source ~/.nexus/venvs/travel/bin/activate
pip install playwright
playwright install chromium
deactivate

# To run scripts with this venv:
~/.nexus/venvs/travel/bin/python ~/.nexus/scripts/travel/gf-search.py
```

```bash
# Kiwi MCP (no auth needed)
# Add to Claude MCP config: github.com/alpic-ai/kiwi-mcp-server-public

# Airbnb MCP
# Add to Claude MCP config: github.com/openbnb-org/mcp-server-airbnb

# Restaurant MCP (Resy + OpenTable)
# Add to Claude MCP config: github.com/jrklein343-svg/restaurant-mcp

# Expedia Rapid (hotel data, 700K+ properties)
# Register: developers.expediagroup.com/docs/products/rapid
```

### Script Locations

All travel scripts in `~/.nexus/scripts/travel/`:
- `gf-search.py` — Google Flights Playwright scraper (pending — add to Codex m4-519 if not built)
- `change-monitor.py` — SOP-4 PNR polling + alerts (**BUILT** — 584 lines, 2026-03-15)
- `travel-model-router.py` — model routing dispatcher (**BUILT** — 412 lines, 2026-03-15)
- `travel-memory.py` — Cortex travel profile read/write helper (**BUILT** — 434 lines, 2026-03-15)

### API Registration Links

| API | URL | Free Tier |
|-----|-----|-----------|
| Amadeus Self-Service | developers.amadeus.com | 200-10K calls/mo |
| Duffel | duffel.com | Free search, pay per booking |
| Kiwi Tequila | tequila.kiwi.com | Free (affiliate model) |
| SerpAPI | serpapi.com | 100 searches/mo |
| Skyscanner Partners | partners.skyscanner.net | Apply for access |
| Travelpayouts | travelpayouts.com | Free (affiliate) |
| Expedia Rapid | developers.expediagroup.com | Sandbox free |

---

## Known Limitations (2026)

1. **No Airbnb public API** — rely on MCP server or Apify scrapers
2. **No OpenTable public API** — community MCP or Apify only
3. **Google Flights has no API** — QPX shut down 2018. Must scrape or use SerpAPI ($50/mo). Use Kiwi MCP first.
4. **IndiGo CPH suspended** since Feb 2026 — ignore phantom prices
5. **Kiwi virtual interlining** = separate tickets. Warn user about baggage/connection risk
6. **Prices are volatile** — always note scrape datetime, recommend booking within 24h
7. **Affiliate bias** — Kiwi Tequila, Travelpayouts, and SerpAPI results may be affiliate-influenced. Cross-validate with non-affiliate sources.
8. **Scraping ToS** — Google Flights, Booking.com, and Airbnb ToS prohibit automated scraping. This procedure uses scraping for personal research only. Do not use for commercial redistribution.
9. **Cookie/session price inflation** — OTAs may inflate prices for returning visitors. All Playwright scraping uses `browser.new_context()` with no persistent `storage_state`.
10. **SOP-4 change-monitor.py BUILT** (2026-03-15, 584 lines) — functional, pending smoke test.
11. **Expedia Rapid requires API key** — sandbox free, production needs partner agreement.

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| v2.2 | 2026-03-15 | TTL enforcement, incognito context, affiliate bias ranking |
| v3.0 | 2026-03-15 | Multi-agent orchestration (7 agents), model routing (Haiku/Sonnet/Opus), Kiwi MCP Priority 1, Expedia Rapid Priority 1 hotels, persistent memory (Cortex travel profile), SOP-4 change monitoring, observability §5, scratchpad handoff |
| v3.0-fixes | 2026-03-15 | **Breaking from v2.2**: Phase 0 now loads/saves Cortex profile (requires `cortex` CLI). Agent prompts added (§0.1). Model routing fixed (Orchestrator=Sonnet always). TRAVEL-S-001 rule created. Session dedup + atomic scratchpad writes. Input validation on phase handoff. Regional source routing table. SOP-4 failure modes. ETIAS date updated. |

**Migration from v2.2**: Procedure is backward-compatible for Phases 1-8. New requirements: (1) `~/.nexus/state/` directory must exist; (2) `cortex` CLI must be accessible for persistent memory; (3) Cortex travel profile is optional — if unavailable, Phase 0 proceeds without profile. SOP-4 requires `change-monitor.py` (Codex m4-519).

---

## Checklist Pre-Publicare

Before delivering any travel report:

- [ ] All prices cross-validated (min 2 sources)
- [ ] Currency consistent (user's preferred, with conversion date noted)
- [ ] Scrape datetime noted on all prices (`price_scraped_at` populated)
- [ ] Flight prices within 30-min TTL — if stale, re-scrape or add stale warning callout
- [ ] Source trust classification set per result (`source_type` field populated)
- [ ] No PII in report (passenger names, passport numbers, etc.)
- [ ] Excluded options listed with reasons
- [ ] Sources and methodology section complete
- [ ] CO2 emissions included where data available
- [ ] Report URL tested (accessible from browser)
- [ ] Cortex session log saved
- [ ] User travel profile updated in Cortex
- [ ] Agent execution log present in report
- [ ] Scratchpad cleared
