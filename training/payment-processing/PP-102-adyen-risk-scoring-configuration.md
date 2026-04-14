---
id: PP-102
title: "Adyen Risk Scoring & Fraud Management Configuration"
domain: payment-processing
source: "Adyen Docs (risk-management), industry best practices"
version: "2.0"
forge_score: null
business_mapping: "Payment operations — fraud detection, risk engine configuration"
type: SOP
complexity: HIGH
created: 2026-03-20
---

# PP-102 — Adyen Risk Scoring & Fraud Management Configuration

## Problema
Without properly configured risk scoring, merchants face either excessive fraud losses (rules too lenient) or excessive false positives that decline legitimate customers (rules too strict). Adyen's risk engine requires systematic configuration of risk profiles, custom rules, velocity checks, ML models, and 3DS triggers to find the optimal balance between fraud prevention and conversion.

## Obiectiv
Configure Adyen's risk management engine (Protect/RevenueProtect) with optimized risk profiles, custom rules, velocity checks, and ML-based fraud detection to minimize fraud losses while keeping false positive rates below 2%.

## Procedura

### Prerequisites
- Adyen Customer Area access (admin level)
- RevenueProtect or Protect license active
- Test merchant account for rule validation
- Client-side `adyen.js` integrated for device fingerprinting
- Webhook endpoint for dispute/chargeback notifications

### Step 1: Select Risk Engine & Create Risk Profiles

**Action:** Choose the appropriate risk engine and create segmented risk profiles.

- Log in to Adyen Customer Area > Risk > Settings
- Select engine: **Protect** (recommended for >10K txn/month) or **RevenueProtect** (simpler setup)
- Create risk profiles by business segment:

| Profile | Use Case | Risk Tolerance |
|---------|----------|----------------|
| `default` | Standard transactions | Medium |
| `high_value` | Transactions >$500 | Low (strict) |
| `trusted_customers` | Returning customers with history | High (lenient) |
| `carrier_billing` | DCB/premium SMS (SMSads) | Very Low (strictest) |
| `crypto_payments` | Cryptocurrency gateway txn | Low |

- Assign profiles to payment methods/merchant accounts via API:

```json
{
  "riskData": {
    "profileReference": "carrier_billing"
  }
}
```

**Validation:** Each profile appears in Customer Area > Risk > Profiles. Test transaction routes to correct profile.

### Step 2: Configure Standard Risk Rules

**Action:** Enable and tune built-in risk rules for each profile.

**Protect engine (outcome-based):**

| Rule Category | Configuration |
|---------------|--------------|
| **Issuer country != shopper country** | Block for carrier_billing, Review for default |
| **IP country != card country** | Review if mismatch, Block if high-risk country |
| **AVS mismatch** | Block if no match + risk_score > medium |
| **CVC failed** | Block always |
| **Velocity: same card >3 txn/hour** | Block |
| **Velocity: same email >5 txn/day** | Review |
| **Velocity: same IP >10 txn/hour** | Block (bot detection) |
| **Amount >2x average for merchant** | Review |
| **New card on file** | Review if amount >$200 |

**RevenueProtect engine (score-based):**

| Rule | Score Impact |
|------|-------------|
| IP/Card country mismatch | +25 |
| AVS no match | +20 |
| CVC failed | +40 (auto-block) |
| Anonymous proxy/VPN detected | +30 |
| Card BIN from high-risk country | +15 |
| Velocity >3 txn/card/hour | +35 |
| First transaction on card | +10 |
| Amount >$1000 | +10 |

Block threshold: 100. Review threshold: 60-99.

**Validation:** Process 100 test transactions across profiles. Verify scoring matches expected outcomes.

### Step 3: Implement Block & Trust Lists

**Action:** Configure referral lists for known good/bad entities.

**Block lists (auto-decline):**
- `blocked_cards`: Known fraudulent card hashes
- `blocked_emails`: Disposable email domains + known fraud emails
- `blocked_ips`: IP ranges associated with fraud rings
- `blocked_bins`: High-fraud BIN ranges (update monthly from network reports)
- `blocked_countries`: Sanctioned countries + high-fraud regions for your vertical

**Trust lists (bypass checks):**
- `trusted_customers`: Verified customers with >5 successful transactions, 0 disputes
- `trusted_ips`: Office IPs for B2B customers
- `trusted_bins`: Corporate card BINs from known partners

```json
// API: Add to block list
POST /pal/servlet/RiskManagement/v1/addToBlockList
{
  "merchantAccountCode": "YOUR_MERCHANT",
  "blockListReference": "blocked_emails",
  "entries": [
    { "email": "fraud@disposable.com", "expiryDate": "2026-06-20" }
  ]
}
```

- Set expiry dates on all list entries (90 days default, review before expiry)
- Automate list updates from dispute data: any `reason=fraud` dispute -> add card+email to block list

**Validation:** Test blocked entity -> transaction declined. Test trusted entity -> bypasses review.

### Step 4: Configure Machine Learning Models (Protect Premium)

**Action:** Enable and tune ML-based fraud detection.

- Enable **Bot Detection ML** in Customer Area > Risk > ML Models
  - Protects against card testing attacks (rapid-fire small transactions)
  - Requires client-side `adyen.js` risk data collection (device fingerprint)

- Enable **Fraudulent Dispute Prediction ML** (Premium feature)
  - Predicts likelihood of fraud dispute based on transaction attributes
  - Configure action: Block if prediction confidence >0.85, Review if >0.60

- Configure **Data Collection** for ML accuracy:

```javascript
// Client-side: Collect risk data
const checkout = await AdyenCheckout({
    clientKey: 'YOUR_CLIENT_KEY',
    environment: 'live',
    analytics: { enabled: true },
    risk: { enabled: true }  // Enables device fingerprinting
});
```

Required data fields for optimal ML:
- `shopperIP` — Shopper's IP address
- `shopperEmail` — Email address
- `shopperReference` — Unique customer ID
- `deliveryAddress` — Shipping address
- `browserInfo` — Full browser fingerprint
- `deviceFingerprint` — Adyen device fingerprint token

**Validation:** ML model accuracy visible in Customer Area > Risk > Analytics. Target: precision >90%, recall >70%.

### Step 5: Set Up Dynamic 3D Secure (3DS)

**Action:** Configure risk-based 3D Secure challenges to balance security and conversion.

```json
// Payment request with dynamic 3DS
{
  "amount": { "value": 5000, "currency": "EUR" },
  "paymentMethod": { "type": "scheme", "number": "..." },
  "threeDS2RequestData": {
    "challengeIndicator": "requestChallenge"
  },
  "authenticationData": {
    "threeDSRequestData": {
      "nativeThreeDS": "preferred"
    }
  }
}
```

3DS trigger rules:

| Condition | 3DS Action |
|-----------|------------|
| Risk score > 60 (RevenueProtect) | Force challenge |
| Amount > $250 AND new customer | Force challenge |
| EU/EEA transaction (PSD2) | Always (SCA mandate) |
| Trusted customer + amount < $100 | Frictionless (exemption) |
| Carrier billing / DCB | Force challenge always |
| Recurring subsequent | Exemption (MIT) |

- Configure exemptions via `scaExemption` field: `lowValue`, `trustedBeneficiary`, `transactionRiskAnalysis`
- Track 3DS outcomes in Risk Analytics for rule tuning

**Validation:** 3DS challenge rate 30-50% of total transactions. Conversion drop <5% vs. no-3DS baseline.

### Step 6: Implement Case Management & Review Queue

**Action:** Set up manual review workflow for flagged transactions.

- Configure review queue in Customer Area > Risk > Case Management
- Set SLA: review within 2 hours during business hours, 4 hours off-hours
- Define review actions: Accept, Reject, Request More Info

Review checklist for agents:
1. Check email domain (disposable? matches card country?)
2. Verify IP geolocation vs. billing/shipping address
3. Check customer history (previous orders, disputes)
4. Verify phone number (callable? matches country?)
5. Cross-reference shipping address with fraud databases
6. For high-value: call customer to verify

- Automate review outcomes back into ML training data
- Track reviewer accuracy: false accept rate <3%, false reject rate <5%

**Validation:** Average review time <15 minutes. Review queue backlog <50 items at any time.

### Step 7: A/B Test & Optimize Risk Profiles

**Action:** Use Adyen's A/B testing to continuously optimize rule performance.

- Customer Area > Risk > Experiments
- Create experiment: `strict_vs_balanced` comparing two rule configurations
- Split traffic: 50/50 for 30 days minimum (need statistical significance)
- Metrics to compare:
  - **Authorization rate** (higher = better conversion)
  - **Fraud rate** (lower = better detection)
  - **Review rate** (lower = less operational cost)
  - **False positive rate** (lower = better customer experience)

Optimization cycle:
1. Week 1-4: Run A/B test
2. Week 5: Analyze results, pick winner
3. Week 5: Adjust losing variant rules
4. Week 6+: New A/B test with refined rules
5. Monthly: Review block/trust lists, purge expired entries
6. Quarterly: Full risk profile audit vs. industry benchmarks

**Validation:** Quarter-over-quarter improvement in fraud rate with stable or improving authorization rate.

### Step 8: Configure Dispute & Chargeback Handling

**Action:** Set up dispute management within Adyen's platform.

- Enable automatic dispute notifications via webhook:

```json
// Webhook: CHARGEBACK notification
{
  "eventCode": "CHARGEBACK",
  "reason": "Fraud",
  "additionalData": {
    "disputeStatus": "Undefended",
    "chargebackReasonCode": "4837",  // MC: No Cardholder Authorization
    "chargebackSchemeCode": "Mastercard"
  }
}
```

- Configure auto-defense for disputes with strong evidence
- Track dispute-to-rule correlation: which rules would have caught disputed transactions?
- Feed dispute outcomes back into risk rules (closed loop):
  - Dispute won -> no rule change needed
  - Dispute lost (fraud) -> tighten rules for matching pattern
  - Dispute lost (service) -> operational fix, not risk rule

Chargeback rate monitoring:
- **Visa VDMP threshold:** 0.9% dispute ratio or 100 disputes
- **Mastercard ECP threshold:** 1.0% dispute ratio or 100 disputes
- Alert at 0.65% (warning) and 0.85% (critical)

**Validation:** Dispute rate consistently below 0.5%. Feedback loop operational with monthly rule adjustments.

## Adyen vs. Stripe Risk Comparison

| Feature | Adyen Protect | Stripe Radar |
|---------|--------------|--------------|
| Risk scoring | Outcome-based (block/allow/review) | Score 0-100 |
| ML models | Bot detection + dispute prediction | Fraud score + card testing |
| 3DS | Dynamic, risk-based | Configurable per PaymentIntent |
| Custom rules | Premium feature | Included with Radar for Fraud Teams |
| Review queue | Built-in Case Management | Dashboard manual review |
| A/B testing | Built-in experiments | Not native |
| Best for | Enterprise, multi-acquirer, complex routing | Startups to mid-market, fast setup |

## Verificare
- [ ] Risk engine selected (Protect or RevenueProtect) and profiles created
- [ ] Standard risk rules configured and tested across all profiles
- [ ] Block and trust lists populated with expiry dates
- [ ] ML models enabled with client-side data collection active
- [ ] Dynamic 3DS configured with risk-based triggers
- [ ] Case management queue operational with SLA defined
- [ ] A/B testing experiment running or completed
- [ ] Dispute webhook handler deployed with closed-loop feedback
- [ ] False positive rate <2%, dispute rate <0.5%

## Instrumente
- Adyen Customer Area (admin)
- Adyen Protect or RevenueProtect engine
- Adyen Checkout SDK (adyen.js)
- Adyen Risk Management API
- Adyen Case Management
- Adyen Experiments (A/B testing)
- Webhook infrastructure

## Note
- Protect (outcome-based) is superior to RevenueProtect (score-based) for merchants with >10K txn/month.
- ML models require minimum 30 days of transaction data to reach useful accuracy.
- Block list entries should always have expiry dates — stale lists cause unnecessary declines.
- The closed-loop feedback from disputes back to risk rules is the single most important long-term optimization.
- A/B testing risk profiles is unique to Adyen and a major advantage over Stripe Radar.
- Review queue SLA directly impacts conversion — flagged transactions that sit too long = lost revenue.

## Cortex Logging
- collection: procedures
- tags: payment-processing, training, adyen, risk-scoring, fraud-detection, revenue-protect, machine-learning, 3d-secure, velocity-checks

## Enforcement Loop
- WHERE: Any Adyen-integrated payment processing environment
- WHEN: Setting up or optimizing fraud management configuration
- HOW: Follow Steps 1-8 sequentially; Steps 1-3 (foundation) before Steps 4-6 (advanced), Step 7 ongoing
- CONNECT: PP-101 (Stripe comparison), PP-103 (carrier billing risk profiles), PP-104 (crypto payments)

## Dependente
- Adyen Customer Area admin access
- Protect or RevenueProtect license
- Client-side adyen.js integration
- Webhook endpoint infrastructure
- Test merchant account

## Metrics
- Completion: all 8 steps executed with validation checks passed
- Quality: false positive rate <2%, fraud rate decreasing QoQ, dispute rate <0.5%
- Impact: authorization rate stable or improving while fraud rate decreases

## Sources
- Adyen Docs: risk-management, protect, revenue-protect
- Visa Core Rules (VDMP thresholds)
- Mastercard Chargeback Guide (ECP program)
- PSD2 Strong Customer Authentication requirements
