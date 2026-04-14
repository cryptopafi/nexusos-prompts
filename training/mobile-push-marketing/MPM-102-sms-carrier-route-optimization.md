---
id: MPM-102
title: SMS Carrier Route Optimization
domain: mobile-push-marketing
source: SMS-Magic Whitepapers + Great Learning SMS Marketing Course + Industry Knowledge
version: 1.0
forge_score: —
business_mapping: ["smsads", "betting-ops", "fintech"]
type: implementation
complexity: high
status: ACTIVE
created: 2026-03-20
rule_id: META-H-002
---

# MPM-102: SMS Carrier Route Optimization

## Purpose

Optimize SMS delivery routes for maximum deliverability, cost efficiency, and compliance across betting/fintech verticals in LATAM and Africa geos. Covers carrier selection, route testing, failover strategies, sender ID management, and regulatory compliance per market.

## Prerequisites

- SMS aggregator account(s) (Twilio, Sinch, Infobip, MessageBird, or Africa-specific: Africa's Talking, Beem)
- Business registration docs for sender ID approval per country
- Compliance review for gambling/fintech SMS in target markets
- Analytics platform for delivery tracking (aggregator dashboards or custom)
- Budget allocation for route testing ($200-500 per geo for initial test batches)

## Procedure

### Step 1: Map Target Markets and Carrier Landscape

**Goal**: Understand carrier ecosystem and regulatory environment per geo.

1. **LATAM priority markets**:
   - **Brazil**: Claro, Vivo, TIM, Oi. Regulator: Anatel. Gambling SMS requires CONAR compliance. Use local long codes or short codes.
   - **Peru**: Claro, Movistar, Entel, Bitel. Regulator: OSIPTEL. Short codes preferred for bulk.
   - **Chile**: Entel, Movistar, Claro, WOM. Regulator: Subtel. Opt-in mandatory (Ley 19.496).
   - **Mexico**: Telcel (70% market), AT&T, Movistar. Short codes via aggregator required for scale.
   - **Colombia**: Claro, Movistar, Tigo. CRC regulates. Two-way short codes available.
2. **Africa priority markets**:
   - **Kenya**: Safaricom (65%+ market share), Airtel, Telkom. Regulator: CA. Betting SMS restricted by BCLB — requires licensed operator sender ID.
   - **Nigeria**: MTN, Airtel, Glo, 9mobile. Regulator: NCC. DND (Do Not Disturb) registry active — must filter.
   - **South Africa**: Vodacom, MTN, Cell C, Telkom. WASPA code of conduct applies. Gambling SMS requires NGB license reference.
   - **Ghana**: MTN, Vodafone, AirtelTigo. NCA regulates. Sender ID pre-registration mandatory.
   - **Tanzania**: Vodacom, Airtel, Tigo, Halotel. TCRA regulates. Bulk SMS requires license.
3. **Document per market**: dominant carrier, market share, sender ID rules, gambling-specific restrictions, average SMS cost.

### Step 2: Select and Configure SMS Aggregator Routes

**Goal**: Establish primary and backup routes per market with optimal cost/quality balance.

1. **Route types**:
   - **Direct carrier connections**: Highest quality, best deliverability (98%+), highest cost. Use for Tier 1 markets (Brazil, Kenya, Nigeria).
   - **Premium routes**: Near-direct, 95-98% delivery. Good balance for Tier 1-2 markets.
   - **Standard/wholesale routes**: 85-95% delivery. Use for Tier 3 or low-priority sends.
   - **Grey routes** (SIM farms): AVOID for regulated verticals. High filtering risk, regulatory penalties.
2. **Multi-aggregator strategy**:
   - Primary aggregator: Choose based on strongest carrier relationships in your top 3 geos.
   - Secondary aggregator: Different carrier partnerships for redundancy.
   - Recommended combos: Infobip (strong LATAM) + Africa's Talking (strong East Africa) + Twilio (global fallback).
3. **Configuration per route**:
   - Set throughput limits (messages per second) per carrier agreement
   - Configure DLR (Delivery Report) callbacks for real-time tracking
   - Set retry logic: max 3 retries, exponential backoff (30s → 60s → 120s)
   - Configure failover: if primary route fails >5% in 15-min window, auto-switch to backup

### Step 3: Sender ID Strategy and Registration

**Goal**: Establish trusted sender identities per market for maximum deliverability.

1. **Sender ID types**:
   - **Alphanumeric**: Brand name (e.g., "BetKing", "SportsBet"). Professional, but no reply possible. Required registration in most markets.
   - **Short code**: 4-6 digit number. Highest deliverability, supports two-way. Expensive ($500-2000/month per market).
   - **Long code**: Local phone number. Cheaper, supports two-way. Lower throughput (1-3 SMS/sec).
   - **Toll-free**: Available in select LATAM markets. Mid-range throughput.
2. **Registration process per market** (timeline: 2-8 weeks):
   - Submit business docs (incorporation, gambling license)
   - Provide sample message content for approval
   - Some markets (Nigeria, Ghana, Kenya) require manual carrier-by-carrier approval
   - Brazil: Short code via aggregator, Anatel approval needed
3. **Sender ID rotation for promotions**:
   - Use consistent branded sender ID for transactional (OTP, balance updates)
   - Rotate promotional sender IDs if carrier filtering detected (but stay compliant)
   - NEVER spoof sender IDs — regulatory penalties in all target markets
4. **Whitelisting**: Request carrier whitelisting for your sender IDs, especially for high-volume sends.

### Step 4: Message Content Optimization for Carrier Filters

**Goal**: Craft messages that pass carrier spam filters while driving conversions.

1. **Content filtering triggers to AVOID**:
   - ALL CAPS text
   - Excessive exclamation marks (!!!)
   - URL shorteners (bit.ly, tinyurl) — flagged as spam. Use branded short domains.
   - "Free" / "Winner" / "Congratulations" — high spam score in most carrier filters
   - Multiple links in one message
   - Non-standard characters or Unicode (increases segment count and may trigger filters)
2. **Optimized message structure** (160 chars = 1 SMS segment):
   ```
   [Brand]: [Personalized offer]. [Clear CTA]. [Branded short URL]. Reply STOP to opt out.
   ```
   Example: `SportsBet: James, 100% deposit match today. Bet on EPL now: sb.co/james. Reply STOP to unsubscribe`
3. **Segment management**:
   - Standard SMS: 160 chars (GSM-7 encoding)
   - Unicode SMS: 70 chars per segment (needed for Portuguese accents, Swahili)
   - Concatenated: up to 3 segments max (higher cost, lower delivery for >2 segments)
4. **Dynamic content insertion**: First name, last bet amount, favorite sport/team, bonus amount.
5. **Compliance footer**: Always include opt-out instruction. Required by law in all target markets.

### Step 5: Implement Delivery Monitoring and Route Scoring

**Goal**: Real-time visibility into route performance with automated quality controls.

1. **KPIs per route** (track hourly):
   - **Delivery rate**: Accepted by carrier / total sent. Target: >95% for direct routes.
   - **DLR success rate**: Confirmed delivered to handset / accepted. Target: >90%.
   - **Latency**: Time from submit to DLR received. Target: <10s for transactional, <60s for promotional.
   - **Rejection rate**: Track rejection codes (invalid number, blacklisted, content filtered).
   - **Cost per delivered SMS**: Total cost / confirmed deliveries.
2. **Route scoring algorithm**:
   ```
   Route Score = (Delivery Rate × 0.4) + (DLR Rate × 0.3) + (1/Latency × 0.15) + (1/Cost × 0.15)
   ```
   Score routes monthly. Replace any route scoring <0.7 on normalized scale.
3. **Alerting thresholds**:
   - Delivery rate drops below 90% for 15 min → ALERT + auto-failover
   - DLR timeout >5 min on >10% of messages → ALERT
   - Rejection rate >10% for a carrier → pause sends, investigate
4. **A/B route testing**: Split 10% of traffic to test route to compare against primary. Promote if outperforms for 7 consecutive days.

### Step 6: Build Compliance and Consent Management

**Goal**: Full regulatory compliance across all target markets.

1. **Consent collection** (MANDATORY before any SMS):
   - Double opt-in preferred: user enters phone → receives confirmation SMS → must reply YES
   - Single opt-in minimum: checkbox during registration (pre-checked NOT allowed in EU-influenced markets)
   - Store consent proof: timestamp, source (web form, app, IVR), IP address, exact text agreed to
2. **DND/suppression list management**:
   - Nigeria: Check NCC DND registry before every send. Fine for sending to DND numbers: NGN 500,000.
   - India (if expanding): TRAI DND check mandatory, 7-day scrub cycle.
   - Maintain internal suppression list: opt-outs, complaints, bounces, inactive >90 days.
3. **Opt-out processing**:
   - Process STOP/UNSUBSCRIBE/QUIT within 24h (many markets require instant)
   - Confirm opt-out with final message: "You've been unsubscribed. Reply START to re-subscribe."
   - Sync opt-out list across ALL sending platforms (SMS, push, email)
4. **Gambling-specific compliance**:
   - Brazil: Lei 14.790/2023 — licensed operators only, responsible gambling message required
   - Kenya: BCLB requires license number in all gambling communications
   - South Africa: NGB license reference mandatory, no messages to minors
   - Include responsible gambling helpline where required

### Step 7: Optimize Cost with Intelligent Routing Logic

**Goal**: Minimize SMS cost without sacrificing deliverability.

1. **Time-based routing**:
   - Send promotional SMS during off-peak carrier hours (lower cost in some markets)
   - Avoid sending 10pm-8am local time (regulatory restriction in many markets + low engagement)
2. **Priority-based routing**:
   - OTP/transactional: ALWAYS use premium/direct route (cost secondary to speed)
   - Promotional: Use standard route unless delivery rate drops below threshold
   - Re-engagement: Use cheapest compliant route (lower urgency)
3. **Fallback cascade**:
   ```
   Primary route (direct) → Secondary route (premium) → Tertiary (standard) → WhatsApp fallback
   ```
4. **WhatsApp Business API as SMS fallback**:
   - LATAM: WhatsApp penetration 80-95%. Often cheaper than SMS for promotional.
   - Africa: Growing but SMS still dominant in rural areas.
   - Use WhatsApp for rich content (images, buttons) and SMS for time-critical/universal reach.
5. **Volume commitments**: Negotiate volume discounts with aggregators. Typical: 10-30% discount for 100K+/month commitment per market.
6. **Monthly cost review**: Compare actual CpD (Cost per Delivered) across routes. Renegotiate or switch quarterly.

## Validation Checklist

- [ ] Carrier landscape documented for all target markets
- [ ] Primary + backup routes configured per geo
- [ ] Sender IDs registered and approved (or application submitted) per market
- [ ] Content templates pass carrier filter testing (test batch 100 numbers per carrier)
- [ ] DLR callbacks configured and receiving data
- [ ] Route scoring dashboard live
- [ ] Consent management system operational with DND checks
- [ ] Opt-out processing tested end-to-end
- [ ] Failover logic tested (simulate primary route failure)
- [ ] Cost per delivered SMS tracked and compared monthly

## Anti-Patterns

- Using grey routes (SIM farms) for regulated verticals — regulatory risk, brand damage
- Sending without proper consent proof — fines up to $10K+ per violation in some markets
- Single aggregator dependency — one outage kills all SMS delivery
- Ignoring DND/suppression lists — Nigeria NCC fines, Kenya BCLB license risk
- URL shorteners (bit.ly) in SMS — carrier spam filter trigger
- Sending >2 SMS segments — cost doubles, delivery drops
- No failover logic — primary route failure = zero delivery until manual intervention
- Same sender ID for transactional + promotional — deliverability contamination

## References

- SMS-Magic: Best Practices in Conversational Text Messaging (9 whitepapers)
- Great Learning: Introduction to SMS Marketing Course (6 modules)
- Twilio: Global Carrier Regulations Guide
- GSMA: Mobile Messaging Best Practice Guidelines
- Nigeria NCC DND Registry Guidelines
- Brazil Anatel SMS Regulations / Lei 14.790/2023
- Kenya BCLB Advertising Guidelines for Betting Companies

---

## Cortex Logging

```json
{
  "text": "SMS carrier route optimization completed: [N] markets configured, primary+backup routes per geo. Delivery rate: [X]%, DLR rate: [Y]%. Sender IDs registered: [N]. Compliance: [PASS/PARTIAL]. Cost per delivered: [Z].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "MPM-102",
    "domain": "mobile_push_marketing",
    "rule_id": "META-H-002",
    "tags": ["sms", "carrier-routing", "deliverability", "compliance", "LATAM", "Africa", "sender-id"],
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
At each procedure execution in real or training context. Monthly route scoring review. Quarterly cost optimization.

### HOW (violation detection)
- Grey routes (SIM farms) used for regulated verticals --> violation (critical)
- SMS sent without consent proof (opt-in record) --> violation (critical)
- DND/suppression list not checked before send (Nigeria NCC, Kenya) --> violation
- Single aggregator dependency without backup route --> violation
- Sender ID spoofing --> violation (regulatory penalty in all target markets)
- Delivery rate drops below 90% for >15 min without failover trigger --> violation
- Opt-out not processed within 24h --> violation
- No DLR callback monitoring active --> violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Forbidden actions:** Using grey routes for regulated verticals, sending without consent proof, spoofing sender IDs, ignoring DND registries, sending >2 SMS segments, using URL shorteners (bit.ly/tinyurl).

### CONNECT
- META-H-002 --> enforcement FORGE standard
- `procedure-health.json` --> add entry
- Links to MPM-101 (push notifications — WhatsApp fallback strategy), MPM-103 (A/B testing for SMS content), IGC-104 (responsible gambling messaging in SMS)
