---
id: MPM-103
title: Push Notification A/B Testing Framework
domain: mobile-push-marketing
source: MoEngage Best Practices + SMS-Magic Conversational Messaging + Industry Testing Methodology
version: 1.0
forge_score: —
business_mapping: ["smsads", "betting-ops", "fintech"]
type: framework
complexity: medium
status: ACTIVE
created: 2026-03-20
rule_id: META-H-002
---

# MPM-103: Push Notification A/B Testing Framework

## Purpose

Establish a systematic A/B (and multivariate) testing framework for push notifications and SMS campaigns. Covers hypothesis formulation, test design, statistical rigor, winner selection, and continuous optimization loops. Applied to betting/fintech verticals targeting LATAM and Africa geos.

## Prerequisites

- Push/SMS platform with native A/B testing (MoEngage, OneSignal, Braze, Airship) OR custom split logic
- Minimum audience size: 10,000 per segment for statistically valid tests
- Analytics integration for downstream metrics (CTR, conversion, revenue)
- Test log document/spreadsheet for institutional knowledge
- Stakeholder alignment on testing cadence (minimum 2 tests/week)

## Procedure

### Step 1: Build the Test Hypothesis Backlog

**Goal**: Create a prioritized queue of test ideas grounded in data.

1. **Audit current performance baseline**:
   - Pull last 30 days of push/SMS campaign data
   - Calculate baseline: delivery rate, open rate, CTR, conversion rate, opt-out rate
   - Segment baseline by geo, device OS, user lifecycle stage, and notification category
2. **Identify optimization opportunities**:
   - Lowest CTR segments → test copy/creative
   - Highest opt-out segments → test frequency/timing
   - Biggest CTR-to-conversion drop → test CTA/landing page
3. **Hypothesis format** (mandatory for every test):
   ```
   IF we [change X] for [segment Y],
   THEN [metric Z] will [increase/decrease] by [estimated %],
   BECAUSE [reasoning based on data or best practice].
   ```
   Example: "IF we add team logo images to pre-match push for LATAM football bettors, THEN CTR will increase by 15%, BECAUSE rich media pushes increase open rates by 25% (MoEngage benchmark)."
4. **Prioritize using ICE scoring**:
   - **I**mpact (1-10): How much will the metric move?
   - **C**onfidence (1-10): How sure are we this will work?
   - **E**ase (1-10): How easy to implement?
   - ICE Score = (I + C + E) / 3. Run highest scores first.
5. **Maintain backlog**: Minimum 15 hypotheses queued at all times. Review and replenish weekly.

### Step 2: Define Test Variables and Control Groups

**Goal**: Isolate exactly one variable per test for clean results.

1. **Testable elements for push notifications**:
   | Element | Example Variants |
   |---------|-----------------|
   | Title/headline | Urgency vs. benefit vs. personalized vs. curiosity |
   | Body copy | Short (50 chars) vs. long (150 chars) |
   | Rich media | No image vs. team logo vs. promo banner vs. GIF |
   | Action buttons | 1 button vs. 2 buttons / "Bet Now" vs. "See Odds" |
   | Emoji | With emoji vs. without |
   | Personalization | Generic vs. first name vs. name + last bet |
   | Send time | Morning vs. afternoon vs. event-triggered |
   | Urgency framing | "Last 2h" vs. "Today only" vs. no urgency |

2. **Testable elements for SMS**:
   | Element | Example Variants |
   |---------|-----------------|
   | Message length | 1 segment (160 chars) vs. 2 segments |
   | CTA placement | Beginning vs. end of message |
   | URL type | Branded short URL vs. full URL |
   | Offer framing | Percentage ("100% match") vs. absolute ("$50 free bet") |
   | Sender ID | Brand name vs. short code number |
   | Language tone | Formal vs. casual (test per geo) |

3. **Control group rules**:
   - Always include a control (current best-performing variant)
   - Control receives minimum 30% of test traffic
   - Hold-out group (5% receives NO message) to measure incremental lift

### Step 3: Calculate Sample Size and Test Duration

**Goal**: Ensure statistical validity before launching any test.

1. **Minimum sample size formula**:
   - For 95% confidence level, 80% statistical power:
   - If baseline CTR = 5%, minimum detectable effect = 1 percentage point (20% relative lift)
   - Required sample: ~3,800 per variant
   - Use online calculator: Evan Miller's A/B Test Sample Size Calculator
2. **Practical minimums for betting vertical**:
   - Push notification test: 5,000 users per variant minimum
   - SMS test: 3,000 users per variant minimum (higher cost, so smaller but still valid)
   - For LATAM/Africa geos with smaller user bases: accept 90% confidence if 95% not achievable
3. **Test duration rules**:
   - Minimum: 24 hours (to capture full day cycle)
   - Recommended: 48-72 hours for push, 24-48 hours for SMS
   - Maximum: 7 days (diminishing returns, opportunity cost)
   - Must span at least one full weekday + one weekend day for non-event-triggered tests
4. **Do NOT peek at results early**. Set a hard end date. Early stopping inflates false positive rate.
   - Exception: If one variant shows >30% higher opt-out rate, kill it immediately (safety stop).

### Step 4: Execute Tests with Platform Controls

**Goal**: Launch tests with proper randomization and tracking.

1. **Randomization**:
   - Use platform's built-in random split (NOT manual list splitting)
   - Ensure even distribution across key dimensions: geo, device OS, user tier
   - Verify with pre-test balance check: compare segment sizes and baseline metrics
2. **Tagging and tracking**:
   - Unique campaign ID per test: `TEST-{domain}-{date}-{variable}` (e.g., `TEST-PUSH-20260320-RICHMEDIA`)
   - UTM parameters on all links: `utm_campaign=test_id&utm_content=variant_a`
   - Track full funnel: send → deliver → open/click → app open → conversion (deposit/bet) → revenue
3. **Platform-specific setup**:
   - **MoEngage**: Use Smart A/B testing with automatic winner selection
   - **OneSignal**: Create A/B test campaign with up to 10 variants
   - **Braze**: Use Canvas for multi-step test flows
   - **Custom**: Split user IDs with hash function for deterministic assignment
4. **QA before launch**:
   - Send test variants to internal devices (iOS + Android + web)
   - Verify rich media rendering, deep link functionality, action buttons
   - Confirm DLR/analytics tracking fires for all variants
5. **Document launch**: Record in test log — date, hypothesis, variants, sample sizes, expected end date.

### Step 5: Analyze Results and Declare Winners

**Goal**: Rigorous statistical analysis with business context.

1. **Primary metric**: Choose ONE primary metric per test (usually CTR or conversion rate).
2. **Statistical significance test**:
   - Use chi-squared test for proportions (CTR, conversion rate)
   - p-value threshold: <0.05 for 95% confidence
   - Also calculate confidence interval for the lift percentage
3. **Practical significance**:
   - Statistical significance alone is not enough
   - Minimum meaningful lift: 5% relative improvement for push CTR, 10% for SMS (higher cost)
   - Consider secondary metrics: if CTR improves but opt-out also increases, net effect may be negative
4. **Segment analysis**:
   - Break results by geo (Brazil vs. Kenya may respond differently)
   - Break by device OS (iOS vs. Android rendering differences)
   - Break by user lifecycle stage (new vs. retained may prefer different messaging)
   - Winner may vary by segment — implement segment-specific winners
5. **Revenue attribution**:
   - Calculate revenue per push/SMS for each variant
   - Factor in cost differences (e.g., rich media = same push cost, but 2-segment SMS = 2x cost)
   - True winner = highest (Revenue - Cost) per send

### Step 6: Implement Winners and Feed the Learning Loop

**Goal**: Scale winning variants and compound learnings over time.

1. **Winner rollout**:
   - Deploy winning variant to 100% of the relevant segment
   - Set calendar reminder to re-test in 30 days (performance decays — creative fatigue)
2. **Test log update** (mandatory fields):
   | Field | Example |
   |-------|---------|
   | Test ID | TEST-PUSH-20260320-RICHMEDIA |
   | Hypothesis | Rich media +15% CTR for LATAM football |
   | Variants | A: text-only (control), B: team logo image |
   | Sample | A: 6,200, B: 6,150 |
   | Duration | 72h (Mar 20-23) |
   | Primary metric | CTR |
   | Result | A: 4.8%, B: 6.1% (+27% lift) |
   | p-value | 0.003 |
   | Winner | B (rich media) |
   | Secondary effects | Opt-out: no significant difference |
   | Action | Rolled out B to all LATAM pre-match pushes |
   | Next test | Test GIF vs. static image (ICE: 7.3) |
3. **Knowledge base patterns** (update quarterly):
   - "Personalized copy with first name lifts CTR 12-18% across all geos"
   - "Urgency framing works better for live betting, benefit framing for pre-match"
   - "Rich media GIFs outperform static images for casino, but not for sports"
4. **Compounding strategy**:
   - Month 1: Test copy (headline variants)
   - Month 2: Test rich media (add to winning copy)
   - Month 3: Test timing (apply to winning copy + media)
   - Month 4: Test segmentation depth (layer all learnings)
   - Expected cumulative lift: 30-60% CTR improvement over 4-month cycle

### Step 7: Advanced Testing — Multi-Armed Bandits and AI Optimization

**Goal**: Graduate from static A/B to dynamic optimization.

1. **Multi-armed bandit (MAB)**:
   - Automatically shifts traffic to winning variant during the test
   - Reduces opportunity cost of sending suboptimal variant
   - Best for: high-volume campaigns where speed matters more than academic rigor
   - Available in: MoEngage (Smart Optimization), Braze (Intelligent Selection)
2. **AI-generated variants**:
   - Use platform AI to generate 5-10 copy variants from a brief
   - Let MAB algorithm find winner without manual hypothesis
   - Best for: promotional pushes with clear offer (bonus amounts, odds boosts)
3. **Personalization engine integration**:
   - Move beyond A/B to 1:1 personalization
   - Platform selects best variant per user based on historical engagement patterns
   - Requires: >50K user base with 90+ days behavioral data
4. **Holdout measurement**:
   - Maintain permanent 5% holdout group receiving NO push/SMS
   - Measure incremental lift of entire push program quarterly
   - Justifies budget: "Push program drives X% incremental deposits and Y% retention lift"

## Validation Checklist

- [ ] Hypothesis backlog contains 15+ prioritized test ideas
- [ ] ICE scoring applied to all hypotheses
- [ ] Sample size calculator used for every test before launch
- [ ] Test log spreadsheet/doc created and accessible to team
- [ ] Platform A/B test functionality verified with dry run
- [ ] UTM parameter scheme defined and tracking confirmed
- [ ] QA checklist for pre-launch variant verification complete
- [ ] Statistical analysis process documented (chi-squared, p-value threshold)
- [ ] First 4-week test calendar planned
- [ ] Holdout group defined (5% permanent no-send)

## Anti-Patterns

- Testing multiple variables simultaneously ("multivariate" without proper sample size — need 4x the users)
- Peeking at results before test duration completes (inflates false positives to ~25%)
- Declaring winners without statistical significance ("Variant B looks better" with p=0.3)
- Never re-testing winners (creative fatigue degrades performance in 3-6 weeks)
- Ignoring secondary metrics (CTR up but opt-out also up = net negative)
- Testing trivial changes (blue vs. slightly-different-blue button) — low impact, wastes test slots
- No test log — team repeats failed experiments or forgets what worked
- Running tests on too-small segments (<1,000 per variant) — results are noise

## References

- MoEngage: A/B Testing and AI Optimization for Push Notifications
- SMS-Magic: Convert 40% More Sales with Conversational Messaging
- Evan Miller: A/B Test Sample Size Calculator
- Ron Kohavi: "Trustworthy Online Controlled Experiments" (A/B testing methodology)
- Google: Multi-Armed Bandit Testing for Marketing
- VWO: Statistical Significance in A/B Testing Guide

---

## Cortex Logging

```json
{
  "text": "A/B testing framework executed: [N] tests completed, [N] winners deployed. Top lift: [X]% on [METRIC]. Hypothesis backlog: [N] items. Cumulative CTR improvement: [Y]%.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "MPM-103",
    "domain": "mobile_push_marketing",
    "rule_id": "META-H-002",
    "tags": ["ab-testing", "push-notifications", "sms", "statistical-significance", "optimization", "MAB"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execution

### WHEN
At each procedure execution in real or training context. Weekly test cadence minimum (2 tests/week). Monthly knowledge base update.

### HOW (violation detection)
- Test launched without hypothesis documented (IF/THEN/BECAUSE format) --> violation
- Multiple variables tested simultaneously without proper multivariate sample size --> violation
- Results peeked before test duration completes (early stopping without safety reason) --> violation
- Winner declared without statistical significance (p-value > 0.05) --> violation
- Test log not updated after test completion --> violation
- Hypothesis backlog below 15 items --> violation
- No holdout group maintained (5% permanent no-send) --> violation
- Sample size below minimum (5,000/variant push, 3,000/variant SMS) without documented justification --> violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Forbidden actions:** Peeking at results early (inflates false positive to ~25%), declaring winners without statistical significance, testing trivial changes, running tests on <1,000 per variant, never re-testing winners (creative fatigue).

### CONNECT
- META-H-002 --> enforcement FORGE standard
- `procedure-health.json` --> add entry
- Links to MPM-101 (push best practices — test elements), MPM-102 (SMS content optimization — SMS-specific testing)
