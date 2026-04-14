---
id: CRO-202
title: "ResearchXL Deep Implementation (CXL Methodology)"
domain: conversion-optimization
source: "CXL ResearchXL Framework, Contentsquare, HubSpot, LIFT Model"
version: "2.0"
forge_score: null
business_mapping: "Revenue optimization — evidence-based CRO research methodology"
type: SOP
complexity: HIGH
created: 2026-03-20
---

# CRO-202 — ResearchXL Deep Implementation (CXL Methodology)

## Problema
Most CRO programs fail because they skip the research phase and jump straight to testing random ideas. The ResearchXL framework (developed by CXL/Peep Laja) provides a structured 6-pillar research methodology that ensures every A/B test is backed by converging evidence from multiple data sources. Without this rigor, teams waste testing cycles on low-impact changes and misattribute results. The framework turns CRO from guesswork into a systematic evidence-gathering machine.

## Obiectiv
Implement the full ResearchXL 6-pillar research methodology to ensure every A/B test is backed by converging evidence from multiple data sources, achieving a test win rate above 33% through systematic evidence triangulation.

## Procedura

### Prerequisites
- CRO-201 completed (basic CRO audit done)
- GA4 with enhanced e-commerce or goal tracking
- Heatmap + session recording tool (Hotjar, Clarity, or Contentsquare)
- Qualitative survey tool (Hotjar Surveys, Qualaroo, or Typeform)
- A/B testing platform (VWO, AB Tasty, Convert, or Kameleoon)
- Access to 5-10 users for usability testing (UserTesting.com, Maze, or recruited customers)

### Step 1: TECHNICAL ANALYSIS — Fix Before You Test
1. **Performance audit**:
   - Run Google PageSpeed Insights on all priority pages — target Core Web Vitals:
     - LCP (Largest Contentful Paint) < 2.5s
     - FID (First Input Delay) < 100ms
     - CLS (Cumulative Layout Shift) < 0.1
   - Each 1-second delay in load time reduces conversions by ~7%
2. **Cross-browser/device testing**:
   - Test on Chrome, Safari, Firefox, Edge (desktop)
   - Test on iOS Safari, Chrome Android (mobile)
   - Use BrowserStack or LambdaTest for systematic coverage
   - Flag rendering issues, broken elements, JS errors
3. **Form validation audit**:
   - Test all forms for error handling, autofill compatibility, mobile keyboard types
   - Check payment flow on all supported methods
4. **Broken element scan**:
   - 404 pages, broken images, dead links, missing assets
   - Console errors (JS failures that silently break functionality)

**Deliverable**: Technical issues spreadsheet with severity (Critical/High/Medium/Low) and fix timeline. Fix all Critical/High BEFORE running any A/B tests.

### Step 2: HEURISTIC ANALYSIS — Expert Walkthrough
1. Conduct a systematic expert review of each priority page using the LIFT model:
   - **Value Proposition**: Is the unique value clear and compelling?
   - **Relevance**: Does the page match visitor expectations from the traffic source?
   - **Clarity**: Is the layout, copy, and CTA immediately understandable?
   - **Urgency**: Is there a reason to act now (without being manipulative)?
   - **Anxiety**: Are there trust signals to reduce purchase risk? (guarantees, reviews, security)
   - **Distraction**: Are there elements pulling attention from the primary conversion action?
2. Score each factor 1-5 for every priority page
3. Use the "Cognitive Walkthrough" method:
   - Walk through the entire conversion flow as a first-time visitor
   - Document every point of confusion, hesitation, or friction
   - Rate friction severity: Blocker (prevents conversion) / Friction (slows) / Annoyance (damages experience)
4. Have 2-3 team members independently conduct the walkthrough, then compare findings

**Deliverable**: Heuristic analysis matrix with page-by-page scores and annotated screenshots

### Step 3: DIGITAL ANALYTICS DEEP DIVE
1. **Funnel analysis** (GA4 Explorations > Funnel):
   - Map full conversion funnel with step-by-step drop-off rates
   - Segment by: device, traffic source, new vs returning, geography, day of week
   - Identify the BIGGEST single drop-off — this is your #1 optimization target
2. **Landing page analysis**:
   - Sort by sessions, then by conversion rate
   - Find "high traffic + low CR" pages — highest ROI optimization targets
   - Find "low traffic + high CR" pages — candidates for traffic investment
3. **Segment comparison**:
   - Converters vs non-converters: which pages did converters visit that non-converters didn't?
   - Cart abandoners: at which checkout step do they leave? What did they do before abandoning?
4. **Internal search analysis**:
   - What terms do visitors search for? (navigation failure signals)
   - Post-search conversion rate vs non-search (if lower, search UX is broken)
5. **Cohort analysis**:
   - First visit to conversion time lag — how many sessions before purchase?
   - If multi-session, what content do they consume between visits?

**Deliverable**: Analytics findings doc with top 10 data-backed insights, each tied to a specific page and metric

### Step 4: MOUSE TRACKING & BEHAVIORAL ANALYTICS
1. **Heatmap analysis** for each priority page:
   - **Click heatmap**: Are visitors clicking where you want? Are they clicking non-clickable elements? (false affordance)
   - **Scroll heatmap**: What % reaches the CTA? Content below the fold seen by <30% of visitors = wasted
   - **Move/attention heatmap**: Where does visual attention concentrate?
2. **Session replay analysis** (minimum 50 sessions per priority page):
   - Tag patterns: hesitation behaviors, form field re-entry, scroll yo-yos, rage clicks
   - Segment replays: converters vs abandoners — what's different in their behavior?
   - Time-to-action: how long before first meaningful interaction?
3. **Frustration detection**:
   - Rage clicks: >3 rapid clicks on same element = broken expectation
   - Dead clicks: clicks on non-interactive elements = unclear affordance
   - Cursor thrashing: rapid erratic movement = confusion
   - Error clicks: clicking disabled buttons or form errors

**Tools**: Hotjar (free tier: 35 daily sessions), Microsoft Clarity (free, unlimited), Contentsquare (enterprise)

**Deliverable**: Behavioral patterns report with annotated heatmap screenshots and replay clips tagged by frustration type

### Step 5: QUALITATIVE RESEARCH — Voice of Customer
1. **On-site surveys** (3 types, deploy simultaneously):
   - **Exit-intent survey** (abandoners): "What stopped you from completing your purchase today?"
   - **Post-purchase survey** (buyers): "What almost stopped you from buying?" + "What convinced you?"
   - **On-page survey** (key pages, triggered at 60% scroll): "Is there anything missing on this page?"
2. **Survey design rules**:
   - Max 3 questions per survey
   - First question always open-ended (qualitative gold)
   - Collect minimum 100 responses before analyzing patterns
   - Use AI-assisted categorization to group themes
3. **Customer interviews** (5-8 interviews):
   - Recruit recent buyers AND recent abandoners
   - Ask: "Walk me through your entire decision process"
   - Probe: "What were you worried about?", "What alternatives did you consider?", "What would have made this easier?"
   - Record, transcribe, extract verbatim quotes for hypothesis backing
4. **Support ticket / chat log mining**:
   - Pull last 200 support tickets mentioning product/checkout/shipping
   - Categorize by theme: pricing confusion, shipping concerns, product info gaps, trust issues
   - Frequency = priority signal

**Deliverable**: VoC synthesis with top 10 customer insights, verbatim quote evidence, and theme frequency ranking

### Step 6: USER TESTING — Watch Real People
1. **Task-based usability tests** (5-8 participants per round):
   - Define 3-5 core tasks: "Find product X and add to cart", "Complete checkout", "Find return policy"
   - Measure: task completion rate, time-on-task, error count, satisfaction score (SUS scale)
   - Use think-aloud protocol: participants narrate their thought process
2. **5-second tests** (for landing pages):
   - Show page for 5 seconds, then ask: "What does this company do?", "What would you do next?"
   - If >40% can't answer correctly, value proposition needs rework
3. **First-click tests**:
   - Show page, give task, record where they click first
   - If first click is wrong, task completion drops to ~30%
4. **Preference tests** (for A/B test design):
   - Show 2-3 design variations, ask preference and WHY
   - Useful for generating variation ideas, NOT for predicting winners

**Tools**: UserTesting.com, Maze.co, Lyssna (formerly UsabilityHub), or recruit via social media/email list

**Deliverable**: Usability findings report with task success rates, critical friction points, and video clip evidence

### Step 7: EVIDENCE TRIANGULATION & HYPOTHESIS PRIORITIZATION
1. Create a master findings matrix:
   | Finding | Technical | Heuristic | Analytics | Behavioral | VoC | Usability | Evidence Score |
   |---|---|---|---|---|---|---|---|
   | Checkout form too long | | X | X | X | X | X | 5/6 |
   | Mobile CTA below fold | | X | X | X | | | 3/6 |
2. **Evidence scoring**: Count how many pillars (Steps 1-6) support each finding
   - 4-6 pillars = HIGH confidence — test immediately
   - 2-3 pillars = MEDIUM confidence — test in next cycle
   - 1 pillar = LOW confidence — gather more evidence before testing
3. **PXL prioritization framework** (CXL's evolution of PIE/ICE):
   - Is it above the fold? (+1)
   - Is it noticeable within 5 seconds? (+1)
   - Does it add/remove elements? (+1 for add, +2 for remove)
   - Does it run on high-traffic pages? (+1)
   - Is the evidence from user data (not best practices)? (+2)
   - Is the test addressing a known conversion barrier? (+1)
   - Sum scores, rank, run highest PXL first
4. **Hypothesis format**:
   ```
   Because we observed [evidence from Steps 1-6],
   we hypothesize that [change] on [page/element]
   will improve [primary metric] by [estimated %]
   measured over [duration] with [sample size].
   ```

### Step 8: TEST DESIGN & STATISTICAL RIGOR
1. **Sample size calculation** (before launching ANY test):
   - Inputs: baseline conversion rate, minimum detectable effect (MDE), significance level, power
   - For 3% baseline CR, 20% relative MDE, 95% significance, 80% power: ~21,000 visitors per variation
   - Use calculator: evanmiller.org/ab-testing/sample-size.html
2. **Test duration rules**:
   - Minimum 14 days (capture weekly patterns)
   - Maximum 60 days (longer = external validity threats)
   - Never peek and stop early — pre-commit to duration
3. **Statistical significance requirements**:
   - Primary metric: 95% confidence (p < 0.05) MINIMUM
   - Revenue/transaction metrics: 99% confidence recommended
   - Use Sequential Testing or Bayesian methods if traffic is limited
   - Check for Sample Ratio Mismatch (SRM) — if traffic split deviates >1% from planned, test is invalid
4. **Guardrail metrics**: Define metrics that must NOT degrade:
   - Bounce rate increase < 5%
   - Page load time increase < 200ms
   - Revenue per visitor must not decrease

### Step 9: POST-TEST ANALYSIS & LEARNING LOOP
1. **Win analysis**: Document what worked and WHY (connect back to research evidence)
2. **Loss analysis**: Often more valuable — what does this tell us about user behavior?
3. **Inconclusive analysis**: Was sample size sufficient? Was the change too small? Was there SRM?
4. **Update research repository**: Every test result enriches your understanding for future tests
5. **Cascade learnings**: If a finding works on one page, create hypotheses for similar pages
6. **Quarterly research refresh**: Re-run Steps 1-6 every quarter — user behavior evolves

## Verificare
- [ ] All 6 research pillars completed with documented deliverables before first test launches
- [ ] Evidence triangulation matrix created with multi-pillar scoring
- [ ] Minimum 3 hypotheses have 3+ pillar evidence support
- [ ] Sample size pre-calculated for every test
- [ ] No test declared winner below 95% statistical significance
- [ ] SRM check performed on every completed test
- [ ] Post-test learning documented and fed back into research repository
- [ ] Quarterly research refresh scheduled

## Instrumente
- Google Analytics 4 (funnel, segments, cohort analysis)
- Google PageSpeed Insights (Core Web Vitals)
- BrowserStack or LambdaTest (cross-browser testing)
- Hotjar (heatmaps, session replays, surveys)
- Microsoft Clarity (free behavioral analytics)
- Contentsquare (enterprise behavioral analytics)
- UserTesting.com or Maze.co (usability testing)
- Lyssna (5-second tests, first-click tests)
- VWO, AB Tasty, or Convert (A/B testing platform)
- Evan Miller Sample Size Calculator

## Note
- The 6-pillar research phase is the differentiator — skipping it reduces test win rate from >33% to ~20%.
- Evidence triangulation (Step 7) is the most critical step — findings supported by 4+ pillars almost always produce winning tests.
- PXL framework is superior to ICE/PIE because it penalizes "best practice" tests without user data backing.
- Never peek at test results early — pre-commit to duration and sample size before launching.
- SRM (Sample Ratio Mismatch) invalidates test results even if significance is reached — always check.
- Quarterly research refresh is non-negotiable because user behavior evolves with seasons, competitors, and market changes.
- The LIFT model (Value, Relevance, Clarity, Urgency, Anxiety, Distraction) is the best heuristic framework for page-level analysis.

## Cortex Logging
- collection: procedures
- tags: conversion-optimization, training, cro, researchxl, cxl, evidence-based-testing, statistical-significance

## Enforcement Loop
- WHERE: Any website or app optimization program
- WHEN: Setting up or scaling a CRO program beyond basic testing
- HOW: Steps 1-6 (research pillars) run in parallel; Steps 7-9 are sequential after research completes
- CONNECT: CRO-201 (prerequisite launch playbook), CRO-203 (creative testing for ads), PAS-201 (Motion framework)

## Dependente
- CRO-201 completed (basic CRO audit)
- Full analytics stack (GA4 + heatmaps + surveys + user testing tool)
- A/B testing platform with statistical engine (VWO, AB Tasty, Convert)
- Minimum 10,000 monthly sessions for reliable testing

## Metrics
- Completion: all 6 research pillars executed with deliverables, minimum 3 tests completed
- Quality: all verification checks pass, evidence triangulation score > 3 for tested hypotheses
- Impact: test win rate > 33% (industry average is ~20-25%, evidence-backed should beat this)

## Sources
- CXL ResearchXL Framework (Peep Laja, cxl.com)
- Contentsquare CRO Guide (contentsquare.com/guides/conversion-rate-optimization)
- HubSpot CRO Guide (blog.hubspot.com/marketing/conversion-rate-optimization-guide)
- LIFT Model (WiderFunnel / Go Group Digital)
- Evan Miller Sample Size Calculator (evanmiller.org/ab-testing/sample-size.html)
