---
id: PP-103
title: "Carrier Billing (DCB) & Premium SMS Setup SOP"
domain: payment-processing
source: "Industry standards, carrier billing best practices, SMSads context"
version: "2.0"
forge_score: null
business_mapping: "Payment operations — mobile billing, carrier integration, gambling compliance"
type: SOP
complexity: HIGH
created: 2026-03-20
---

# PP-103 — Carrier Billing (DCB) & Premium SMS Setup SOP

## Problema
DCB allows users to charge purchases to their mobile phone bill, which is critical for betting/gambling verticals reaching unbanked users in emerging markets. However, carrier approval cycles are 6-14 weeks, chargeback limits are strict (1-3%), and regulatory requirements vary drastically per market. Without a systematic setup process, merchants face carrier rejection, compliance violations, and service suspension.

## Obiectiv
Establish a fully compliant Direct Carrier Billing (DCB) and Premium SMS payment pipeline for betting/gaming services, including carrier integration, PIN-flow authentication, billing API setup, dispute handling, and regulatory compliance across target markets.

## Procedura

### Prerequisites
- Gambling license for target jurisdiction(s)
- Business entity with content provider capability
- Aggregator relationship (Boku, Fortumo, Centili, or Infobip)
- Technical team for API integration
- Compliance officer designated for mobile payments

### Step 1: Market & Carrier Assessment

**Action:** Evaluate target markets for DCB feasibility and carrier coverage.

Market assessment matrix:

| Market | Major MNOs | DCB for Gambling? | Regulatory Body | Notes |
|--------|-----------|-------------------|-----------------|-------|
| UK | EE, Vodafone, Three, O2 | Yes (licensed operators) | Ofcom + PSA (Phone-paid Services Authority) | PSA Code 15 compliance required |
| Nigeria | MTN, Airtel, Glo, 9mobile | Yes (with NITDA approval) | NCC + NITDA | High volume, low ARPU |
| Kenya | Safaricom (M-Pesa), Airtel | Limited (betting tax) | CAK + BCLB | M-Pesa dominant, DCB secondary |
| South Africa | Vodacom, MTN, Cell C | Yes (WASPA registered) | WASPA + NGB | Self-regulatory via WASPA |
| Peru | Movistar, Claro, Entel | Restricted | OSIPTEL + MINCETUR | Gambling license required first |
| Brazil | Vivo, Claro, TIM, Oi | Yes (sports betting legal 2025+) | Anatel + SPA/MF | New regulatory framework |

- Identify aggregators with carrier relationships in target markets
- Top aggregators: Boku, Fortumo (Boku), Digital Turbine, Centili, Infobip
- Evaluate: revenue share (typically 30-50% to carriers), payout terms, API quality

**Validation:** Market shortlist with confirmed DCB gambling legality. Aggregator selected with signed NDA.

### Step 2: Obtain Licenses & Carrier Approvals

**Action:** Secure required licenses and MNO approvals for DCB gambling content.

Required documentation per carrier:
1. **Gambling license** for target jurisdiction (prerequisite)
2. **Content provider application** to aggregator
3. **Service description** — detailed flow, pricing, subscription terms
4. **Marketing samples** — all ad creatives, landing pages, opt-in flows
5. **Terms & conditions** — user-facing, carrier-compliant
6. **Technical integration spec** — API endpoints, callback URLs
7. **Compliance officer contact** — designated responsible person
8. **Data processing agreement** — GDPR/local data protection

Approval timeline:
- Aggregator onboarding: 1-2 weeks
- Carrier technical review: 2-4 weeks
- Carrier compliance review: 2-6 weeks (gambling = extra scrutiny)
- Live testing: 1-2 weeks
- Total: 6-14 weeks per carrier per market

**Validation:** Written approval from each carrier. Test credentials issued. Service ID assigned.

**Failure mode:** Gambling content rejection is common. Mitigation: work with aggregator experienced in gambling vertical; pre-align marketing materials with carrier guidelines before submission.

### Step 3: Implement PIN-Flow Authentication (Double Opt-In)

**Action:** Build the mandatory two-step user consent flow for DCB transactions.

```
Flow: User clicks "Pay via Phone Bill"
  1. User enters MSISDN (phone number)
  2. System sends PIN via SMS to MSISDN
  3. User enters PIN on payment page (consent step 1)
  4. System displays: "You will be charged [AMOUNT] on your [CARRIER] bill
     for [SERVICE]. Reply YES to confirm." (consent step 2)
  5. User confirms -> charge initiated
  6. Success/failure callback received
  7. Confirmation SMS sent to user with receipt + opt-out instructions
```

Implementation:

```python
# Step 1: Initiate PIN flow
def initiate_dcb_payment(msisdn: str, amount: float, service_id: str):
    response = aggregator_api.post('/v1/payment/initiate', {
        'msisdn': msisdn,
        'amount': amount,
        'currency': 'GBP',
        'service_id': service_id,
        'description': 'BetService deposit',
        'callback_url': 'https://api.example.com/dcb/callback',
        'pin_flow': True,  # Mandatory for gambling
        'reference': generate_unique_ref()
    })
    return response['transaction_id'], response['pin_sent']

# Step 2: Verify PIN and charge
def confirm_dcb_payment(transaction_id: str, pin: str):
    response = aggregator_api.post('/v1/payment/confirm', {
        'transaction_id': transaction_id,
        'pin': pin
    })
    # States: SUCCESS, FAILED, INSUFFICIENT_FUNDS, BLOCKED, TIMEOUT
    return response['status'], response['charge_id']

# Step 3: Handle callback
@app.route('/dcb/callback', methods=['POST'])
def dcb_callback():
    data = verify_signature(request)
    if data['status'] == 'CHARGED':
        credit_user_account(data['reference'], data['amount'])
        send_confirmation_sms(data['msisdn'], data['amount'])
    elif data['status'] == 'REFUNDED':
        debit_user_account(data['reference'], data['amount'])
    log_transaction(data)
    return '', 200
```

Mandatory elements in confirmation SMS:
- Service name and provider
- Amount charged
- How to stop/unsubscribe (e.g., "Text STOP to 12345")
- Customer service number
- Frequency if recurring

**Validation:** End-to-end test on each carrier. PIN delivery <30s. Charge confirmation <5s.

### Step 4: Configure Billing Limits & Subscription Management

**Action:** Set appropriate billing caps and manage recurring billing.

Carrier-imposed limits (typical):

| Type | UK (PSA) | Nigeria | South Africa |
|------|----------|---------|--------------|
| Single transaction max | GBP 40 | NGN 5,000 | ZAR 500 |
| Daily cap per user | GBP 80 | NGN 10,000 | ZAR 1,000 |
| Monthly cap per user | GBP 240 | NGN 50,000 | ZAR 5,000 |
| Free trial required? | No (gambling exempt) | Varies | Yes (non-gambling) |

Implementation:
```python
# Billing limit enforcement (server-side, don't rely on carrier alone)
def check_billing_limits(msisdn: str, amount: float, market: str):
    limits = MARKET_LIMITS[market]
    user_history = get_user_billing_history(msisdn, period='month')

    if amount > limits['single_max']:
        return False, 'EXCEEDS_SINGLE_LIMIT'
    if user_history['daily_total'] + amount > limits['daily_max']:
        return False, 'EXCEEDS_DAILY_LIMIT'
    if user_history['monthly_total'] + amount > limits['monthly_max']:
        return False, 'EXCEEDS_MONTHLY_LIMIT'

    return True, 'OK'
```

Subscription management:
- Implement immediate cancellation endpoint (STOP keyword via SMS + web portal)
- Send renewal reminder 24h before recurring charge
- Grace period: 7 days after failed charge before service suspension
- Maintain opt-out log with timestamps (regulatory requirement)

**Validation:** Limits enforced correctly. STOP keyword processes within 60 seconds. Audit trail complete.

### Step 5: Implement Premium SMS (MO/MT) Billing

**Action:** Set up Mobile Originated (MO) and Mobile Terminated (MT) SMS billing as alternative/complement to DCB API.

```
MO Flow (user-initiated):
  User sends "BET 50" to shortcode 12345
  -> System receives MO SMS
  -> Validates keyword + amount
  -> Charges user's phone bill (MT charge)
  -> Sends confirmation MT SMS
  -> Credits betting account

MT Flow (merchant-initiated):
  System sends premium MT SMS to user
  -> Carrier charges user on delivery
  -> Callback confirms charge
  -> Credits betting account
```

Shortcode setup:
- Dedicated shortcode per market (shared shortcodes = lower trust)
- Keyword registration: BET, DEPOSIT, PAY, FUND, STOP, HELP, INFO
- Configure routing: keyword -> service handler mapping

```python
# MO handler
@app.route('/sms/mo', methods=['POST'])
def handle_mo_sms():
    msisdn = request.form['sender']
    keyword = request.form['keyword'].upper()
    shortcode = request.form['shortcode']

    if keyword == 'STOP':
        unsubscribe_user(msisdn)
        send_mt(msisdn, 'You have been unsubscribed. No further charges.')
        return '', 200

    if keyword == 'BET':
        amount = parse_amount(request.form['text'])
        if check_billing_limits(msisdn, amount, get_market(msisdn)):
            charge_id = initiate_mt_charge(msisdn, amount)
            send_mt(msisdn, f'Depositing {amount}. Ref: {charge_id}')
        else:
            send_mt(msisdn, 'Limit exceeded. Try a smaller amount.')

    return '', 200
```

**Validation:** MO receipt <2s, MT delivery <5s, charge callback <10s. STOP processing <60s.

### Step 6: Build Dispute & Refund Management

**Action:** Handle carrier billing disputes and maintain chargeback rate below carrier thresholds.

DCB dispute types:
1. **Unauthorized charge** — User claims they didn't consent -> Check PIN-flow logs
2. **Service not delivered** — Charge succeeded but service failed -> Auto-refund
3. **Unrecognized charge** — User doesn't recognize on phone bill -> Descriptor issue
4. **Minor's purchase** — Parental complaint -> Refund + block MSISDN
5. **Carrier-initiated refund** — Carrier refunds without merchant input

Dispute handling flow:
```python
def handle_dcb_dispute(dispute):
    dispute_type = classify_dispute(dispute)

    if dispute_type == 'service_not_delivered':
        # Auto-refund, no contest
        process_refund(dispute['charge_id'])
        return 'REFUNDED'

    if dispute_type == 'unauthorized':
        evidence = {
            'pin_flow_log': get_pin_verification_log(dispute['transaction_id']),
            'ip_address': get_transaction_ip(dispute['transaction_id']),
            'device_fingerprint': get_device_data(dispute['transaction_id']),
            'consent_timestamp': get_consent_time(dispute['transaction_id'])
        }
        if evidence['pin_flow_log']['pin_verified']:
            submit_representment(dispute, evidence)
            return 'CONTESTED'
        else:
            process_refund(dispute['charge_id'])
            return 'REFUNDED'

    if dispute_type == 'minor':
        process_refund(dispute['charge_id'])
        block_msisdn(dispute['msisdn'])
        return 'REFUNDED_AND_BLOCKED'
```

Carrier chargeback thresholds:
- **UK PSA:** >3% complaint rate = service suspension investigation
- **Most MNOs:** >2% refund rate = warning, >5% = suspension
- **Aggregator-level:** Varies, typically 1-3% before escalation

**Validation:** Refund rate <1.5%. All disputes responded to within 24h. PIN-flow evidence retained 180 days.

### Step 7: Regulatory Compliance & Audit Trail

**Action:** Ensure full regulatory compliance across all markets.

Compliance checklist by market:

**UK (PSA Code 15):**
- [ ] PSA registration as Level 2 provider
- [ ] Age verification (18+) before gambling service access
- [ ] Spend reminders at GBP 20 and GBP 40 thresholds
- [ ] Monthly spending cap per MSISDN
- [ ] STOP keyword functional 24/7
- [ ] Complaints handling procedure documented
- [ ] Quarterly compliance reports to PSA
- [ ] All marketing materials pre-approved

**Nigeria (NCC):**
- [ ] Content provider license from NCC
- [ ] NITDA data protection compliance
- [ ] DND (Do Not Disturb) registry check before marketing
- [ ] Pricing displayed in NGN with tax
- [ ] Opt-in evidence retained 12 months

**South Africa (WASPA):**
- [ ] WASPA membership active
- [ ] Code of Conduct compliance
- [ ] POPIA (data protection) compliance
- [ ] Pricing in ZAR inclusive of VAT
- [ ] Monthly WASPA compliance report

Audit trail requirements (all markets):
```json
{
  "transaction_id": "txn_abc123",
  "msisdn_hash": "sha256:...",
  "timestamp": "2026-03-20T14:30:00Z",
  "consent_type": "pin_flow",
  "pin_verified": true,
  "pin_timestamp": "2026-03-20T14:29:55Z",
  "ip_address": "1.2.3.4",
  "amount": 500,
  "currency": "GBP",
  "carrier": "vodafone_uk",
  "service_id": "SVC-BET-001",
  "keyword": "BET",
  "shortcode": "12345",
  "confirmation_sms_sent": true,
  "confirmation_sms_timestamp": "2026-03-20T14:30:02Z"
}
```

Retention periods:
- Transaction logs: 7 years (financial regulation)
- Consent evidence: minimum 2 years (carrier requirement)
- Marketing opt-in/out: 3 years after last interaction
- Dispute records: 5 years

**Validation:** Quarterly internal audit passes. Zero regulatory findings. Audit trail queryable within 24h of request.

## Revenue Share Model (Typical)

```
User pays: $10.00 via phone bill
  Carrier takes: 30-50% ($3.00-5.00)
  Aggregator takes: 5-10% ($0.50-1.00)
  Merchant receives: 40-65% ($4.00-6.50)

Premium SMS (higher carrier share):
  Carrier takes: 40-60%
  Aggregator takes: 5-15%
  Merchant receives: 25-55%
```

Note: Revenue share is negotiable with volume. >100K transactions/month can push merchant share to 60-70%.

## Verificare
- [ ] Market assessment completed with DCB gambling legality confirmed
- [ ] Gambling license obtained for target jurisdictions
- [ ] Carrier approvals received with test credentials
- [ ] PIN-flow double opt-in implemented and tested on each carrier
- [ ] Billing limits enforced (server-side, per-market)
- [ ] STOP keyword processing <60 seconds
- [ ] Premium SMS shortcode configured with keyword routing
- [ ] Dispute handling flow implemented with auto-refund for service failures
- [ ] Refund rate <1.5%
- [ ] Regulatory compliance checklist passed for each market
- [ ] Audit trail retaining all required fields per transaction

## Instrumente
- Carrier aggregator API (Boku, Fortumo, Centili, or Infobip)
- SMS gateway (for MO/MT handling)
- Shortcode provider (per-market)
- PIN-flow authentication system
- Compliance management platform
- Audit trail database (7-year retention)

## Note
- Carrier approval for gambling content takes 6-14 weeks per market — start early.
- Never store MSISDN in plain text — always hash (sha256).
- Revenue share is 30-50% to carriers — factor this into unit economics before committing.
- PIN-flow logs are your strongest evidence in unauthorized charge disputes.
- STOP keyword must work 24/7 without exception — regulators test this.
- Server-side billing limits are mandatory even if carriers enforce their own — defense in depth.

## Cortex Logging
- collection: procedures
- tags: payment-processing, training, carrier-billing, dcb, premium-sms, mobile-payments, gambling, smsads, kyc, compliance, pin-flow

## Enforcement Loop
- WHERE: Mobile payment integration for betting/gambling services
- WHEN: Setting up DCB or premium SMS billing in new markets
- HOW: Follow Steps 1-7 sequentially; Step 1-2 (approvals) gate all subsequent steps
- CONNECT: PP-101 (Stripe disputes — comparison), PP-102 (Adyen risk profiles for DCB), PP-104 (crypto — alternative payment)

## Dependente
- Gambling license for target jurisdiction
- Aggregator account with carrier relationships
- Shortcode allocation per market
- Compliance officer for mobile payments
- Legal review of per-market regulations

## Metrics
- Completion: all 7 steps executed with carrier approval obtained
- Quality: refund rate <1.5%, STOP processing <60s, zero regulatory findings
- Impact: DCB payment channel live and processing in target market(s)

## Sources
- PSA Code of Practice (UK Phone-paid Services Authority)
- WASPA Code of Conduct (South Africa)
- NCC/NITDA regulations (Nigeria)
- Boku/Fortumo aggregator documentation
- Industry carrier billing best practices
