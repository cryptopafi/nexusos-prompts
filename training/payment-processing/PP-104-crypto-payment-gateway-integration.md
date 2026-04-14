---
id: PP-104
title: "Crypto Payment Gateway Integration"
domain: payment-processing
source: "Industry best practices, gateway documentation"
version: "2.0"
forge_score: null
business_mapping: "Payment operations — cryptocurrency deposits/withdrawals, AML compliance"
type: SOP
complexity: HIGH
created: 2026-03-20
---

# PP-104 — Crypto Payment Gateway Integration

## Problema
Crypto payments for gambling offer zero chargebacks, lower fees (0.5-1% vs 2.9%), and global reach without banking restrictions. However, integration requires navigating gateway selection (most restrict gambling), KYB compliance, hot/cold wallet security, blockchain analytics for AML, and settlement reconciliation. Without a systematic approach, merchants face wallet compromises, compliance violations, and reconciliation failures.

## Obiectiv
Integrate a crypto payment gateway that accepts BTC, ETH, USDT, and USDC for betting/gaming deposits with instant settlement, automatic fiat conversion, and compliance with AML/KYC requirements across target jurisdictions.

## Procedura

### Prerequisites
- Business entity with gambling license (Curacao, Malta MGA, UK GC, or equivalent)
- KYB documentation prepared (incorporation docs, UBO info, AML policy)
- Technical team for API integration
- Treasury/finance team for settlement management
- Blockchain analytics provider account (Chainalysis, Elliptic, or CipherTrace)

### Step 1: Select & Evaluate Crypto Payment Gateway

**Action:** Choose a gateway that supports gambling verticals and target jurisdictions.

Gateway comparison:

| Gateway | Gambling OK? | Coins | Fiat Settlement | Fees | API Quality |
|---------|-------------|-------|-----------------|------|-------------|
| **NOWPayments** | Yes (licensed operators) | 200+ | EUR/USD/GBP | 0.5-1% | REST, good |
| **CoinsPaid** | Yes (gambling focus) | 50+ | EUR/USD | 0.8% | REST + webhooks |
| **Alphapo** | Yes (gambling/gaming) | 30+ | EUR/USD/GBP | 0.5% | REST, enterprise |
| **BitPay** | No (restricted) | BTC/ETH/stables | USD/EUR | 1% | REST, mature |
| **Coinbase Commerce** | No (restricted) | Limited | USD | 1% | REST, simple |
| **B2BinPay** | Yes | 80+ | EUR/USD/GBP | 0.5% | REST + WS |

Recommended: **CoinsPaid** or **Alphapo** for gambling vertical (purpose-built, compliance-ready).

Evaluation criteria:
- [ ] Supports gambling/betting merchants explicitly
- [ ] Instant fiat conversion (no holding crypto exposure)
- [ ] Multi-currency settlement (EUR, USD, GBP minimum)
- [ ] Webhook reliability (99.9%+ delivery rate)
- [ ] Sandbox/testnet environment available
- [ ] Compliance documentation provided (SAR filing, transaction monitoring)
- [ ] Volume capacity (>10K transactions/day)
- [ ] Cold storage option for retained crypto

**Validation:** Gateway selected, NDA signed, sandbox credentials received.

### Step 2: Complete KYB & Compliance Onboarding

**Action:** Pass Know-Your-Business verification and configure compliance settings.

Required documentation:
1. **Certificate of incorporation** + articles of association
2. **Gambling license** (Curacao, Malta MGA, UK GC, or equivalent)
3. **UBO (Ultimate Beneficial Owner) documentation** — passport + proof of address for all >25% shareholders
4. **AML/KYC policy document** — your internal compliance procedures
5. **Bank statements** (last 3 months) — proves fiat settlement capability
6. **Website/app** — live product with T&C, privacy policy, responsible gambling page
7. **Source of funds declaration** — explains expected transaction volumes and origins

AML configuration on gateway:
```json
{
  "compliance_settings": {
    "aml_screening": true,
    "transaction_monitoring": true,
    "risk_scoring": true,
    "pep_screening": true,
    "sanctions_screening": true,
    "travel_rule_compliance": true,
    "suspicious_activity_threshold_usd": 3000,
    "auto_flag_mixers": true,
    "auto_flag_darknet": true,
    "chainalysis_integration": true
  }
}
```

**Validation:** KYB approved, merchant account active, compliance settings confirmed by gateway compliance team.

### Step 3: Implement Payment Flow — Deposits

**Action:** Build the crypto deposit flow with address generation, monitoring, and crediting.

```python
# Generate unique deposit address per transaction
def create_crypto_deposit(user_id: str, amount_usd: float, currency: str = 'BTC'):
    """
    Supported currencies: BTC, ETH, USDT_ERC20, USDT_TRC20, USDC, LTC, DOGE
    """
    response = gateway_api.post('/v1/addresses', {
        'currency': currency,
        'foreign_id': f'{user_id}_{int(time.time())}',  # Unique reference
        'convert_to': 'USD',  # Instant conversion to fiat
        'callback_url': 'https://api.example.com/crypto/callback'
    })

    return {
        'address': response['address'],
        'amount_crypto': response['expected_amount'],
        'amount_usd': amount_usd,
        'exchange_rate': response['rate'],
        'rate_lock_seconds': response['rate_lock'],  # Typically 15-30 min
        'qr_code_url': response['qr_code'],
        'expires_at': response['expiry']
    }

# Handle payment callback
@app.route('/crypto/callback', methods=['POST'])
def crypto_callback():
    data = verify_hmac_signature(request)

    status = data['status']
    # Statuses: pending, confirming, confirmed, failed, expired

    if status == 'confirming':
        # Transaction seen on blockchain, awaiting confirmations
        # Optional: credit immediately for small amounts (< $50)
        confirmations = data['confirmations']
        required = CONFIRMATION_REQUIREMENTS[data['currency']]
        notify_user(data['foreign_id'], f'Deposit detected. {confirmations}/{required} confirmations.')

    elif status == 'confirmed':
        # Fully confirmed, safe to credit
        user_id = data['foreign_id'].split('_')[0]
        amount_usd = data['amount_usd']  # After conversion
        credit_user_balance(user_id, amount_usd)
        send_deposit_confirmation(user_id, amount_usd, data['txid'])

    elif status == 'failed':
        log_failed_deposit(data)
        alert_support(data)

    return '', 200
```

Confirmation requirements:

| Currency | Confirmations | Typical Time |
|----------|--------------|--------------|
| BTC | 3 | 30 min |
| ETH | 12 | 3 min |
| USDT (ERC-20) | 12 | 3 min |
| USDT (TRC-20) | 20 | 1 min |
| USDC | 12 | 3 min |
| LTC | 6 | 15 min |

Optional: "0-conf" instant credit for amounts <$50 (accept blockchain risk for better UX).

**Validation:** Test deposit on testnet for each supported currency. Callback received. Balance credited correctly.

### Step 4: Implement Payment Flow — Withdrawals

**Action:** Build the crypto withdrawal flow with security controls and hot wallet management.

```python
# Process withdrawal request
def request_withdrawal(user_id: str, amount_usd: float, currency: str, wallet_address: str):
    # Security checks
    if not validate_wallet_address(wallet_address, currency):
        raise InvalidAddressError(f'Invalid {currency} address')

    if is_sanctioned_address(wallet_address):  # Chainalysis/Elliptic check
        flag_suspicious_activity(user_id, wallet_address)
        raise ComplianceBlockError('Address flagged by compliance screening')

    # Withdrawal limits
    limits = get_user_withdrawal_limits(user_id)
    if amount_usd > limits['remaining_daily']:
        raise LimitExceededError('Daily withdrawal limit exceeded')

    # KYC tier check
    kyc_tier = get_user_kyc_tier(user_id)
    if amount_usd > KYC_TIER_LIMITS[kyc_tier]:
        raise KYCRequiredError(f'KYC tier {kyc_tier + 1} required for this amount')

    # 2FA verification
    if not verify_2fa(user_id):
        raise AuthenticationError('2FA verification required')

    # Queue withdrawal (manual approval for large amounts)
    if amount_usd > 5000:
        queue_for_manual_review(user_id, amount_usd, currency, wallet_address)
        return {'status': 'pending_review', 'eta': '2 hours'}

    # Auto-process
    response = gateway_api.post('/v1/withdrawals', {
        'currency': currency,
        'amount': convert_usd_to_crypto(amount_usd, currency),
        'address': wallet_address,
        'foreign_id': f'wd_{user_id}_{int(time.time())}'
    })

    return {'status': 'processing', 'txid': response.get('txid')}
```

Withdrawal security controls:

| Control | Threshold | Action |
|---------|-----------|--------|
| Address whitelist | All withdrawals | User must pre-register + 24h cooling period |
| Daily limit (unverified) | $1,000/day | Block |
| Daily limit (KYC tier 1) | $10,000/day | Block |
| Daily limit (KYC tier 2) | $50,000/day | Manual review |
| Single withdrawal review | >$5,000 | Manual approval |
| New address first use | Any amount | Delay 1h + email confirmation |
| Velocity check | >3 withdrawals/hour | Block + alert |

**Validation:** Test withdrawal on testnet. Limits enforced. Sanctioned address blocked. 2FA required.

### Step 5: Configure Hot/Cold Wallet Strategy

**Action:** Set up wallet infrastructure for security and liquidity.

Architecture:
```
[User Deposits] -> [Deposit Addresses] -> [Hot Wallet] -> [Cold Wallet]
                                              |
                                              v
[User Withdrawals] <-- [Hot Wallet] <-- [Cold Wallet (manual top-up)]
```

Hot wallet configuration:
- Maximum balance: 10-20% of daily withdrawal volume
- Auto-sweep to cold wallet when hot wallet exceeds threshold
- Multi-signature: 2-of-3 for transactions >$10K
- Key management: HSM (Hardware Security Module) or cloud KMS
- Monitoring: real-time balance alerts at 20%, 50%, 80% of max

Cold wallet configuration:
- 95%+ of crypto reserves in cold storage
- Multi-signature: 3-of-5 with geographically distributed signers
- Hardware wallets: Ledger Enterprise or equivalent
- Top-up schedule: daily review, manual transfer to hot wallet
- Air-gapped signing for transfers >$100K

```python
# Hot wallet monitoring
def check_hot_wallet_health():
    for currency in SUPPORTED_CURRENCIES:
        balance = get_hot_wallet_balance(currency)
        max_balance = HOT_WALLET_LIMITS[currency]
        daily_withdrawal_avg = get_avg_daily_withdrawals(currency, days=7)

        # Sweep excess to cold
        if balance > max_balance:
            sweep_amount = balance - (max_balance * 0.7)  # Sweep to 70% capacity
            initiate_cold_transfer(currency, sweep_amount)

        # Alert if running low
        if balance < daily_withdrawal_avg * 0.5:
            alert_treasury(f'{currency} hot wallet low: {balance}. '
                          f'Need top-up. Avg daily: {daily_withdrawal_avg}')
```

**Validation:** Hot wallet balance within limits. Cold wallet audit monthly. Sweep automation tested.

### Step 6: Implement Blockchain Analytics & Transaction Monitoring

**Action:** Integrate blockchain analytics for AML compliance and fraud detection.

```python
# Pre-deposit screening (check source of funds)
def screen_incoming_transaction(txid: str, currency: str):
    # Chainalysis KYT (Know Your Transaction) or Elliptic
    result = chainalysis_api.post('/v2/transfers', {
        'network': currency_to_network(currency),
        'asset': currency,
        'transferReference': txid,
        'direction': 'received'
    })

    risk_score = result['risk']['score']  # 0-10
    categories = result['risk']['categories']  # ['gambling', 'exchange', 'mixer', etc.]

    if 'sanctions' in categories or 'darknet' in categories:
        freeze_deposit(txid)
        file_sar(txid, result)  # Suspicious Activity Report
        return 'BLOCKED'

    if 'mixer' in categories or 'tumbler' in categories:
        flag_for_review(txid, 'Mixer/tumbler detected')
        return 'REVIEW'

    if risk_score > 7:
        flag_for_review(txid, f'High risk score: {risk_score}')
        return 'REVIEW'

    return 'CLEAN'

# Continuous monitoring
def daily_compliance_scan():
    """Run daily on all active wallets and recent transactions"""
    # Re-screen last 30 days of deposits (risk profiles update retroactively)
    recent_deposits = get_deposits(days=30)
    for deposit in recent_deposits:
        updated_risk = screen_incoming_transaction(deposit['txid'], deposit['currency'])
        if updated_risk != deposit['last_risk_status']:
            escalate_risk_change(deposit, updated_risk)
```

Travel Rule compliance (transfers >$1,000):
- Collect originator info: name, account number, address
- Collect beneficiary info: name, account number
- Transmit via TRISA or OpenVASP protocol
- Store for 5 years minimum

**Validation:** All deposits screened before crediting. SAR filing workflow tested. Travel Rule data collected for qualifying transfers.

### Step 7: Build Settlement & Reconciliation Pipeline

**Action:** Automate fiat settlement and daily reconciliation.

```python
# Daily settlement reconciliation
def daily_reconciliation():
    # 1. Gateway transaction report
    gateway_txns = gateway_api.get('/v1/reports/daily', {
        'date': yesterday(),
        'type': 'all'  # deposits + withdrawals
    })

    # 2. Internal ledger
    internal_txns = db.query("""
        SELECT * FROM crypto_transactions
        WHERE DATE(created_at) = %s
    """, [yesterday()])

    # 3. Blockchain verification (sample 10% or all >$1000)
    large_txns = [t for t in gateway_txns if t['amount_usd'] > 1000]
    for txn in large_txns:
        blockchain_status = verify_on_chain(txn['txid'], txn['currency'])
        if blockchain_status != 'confirmed':
            flag_reconciliation_issue(txn)

    # 4. Compare and flag discrepancies
    discrepancies = compare_ledgers(gateway_txns, internal_txns)
    if discrepancies:
        alert_finance(f'{len(discrepancies)} reconciliation discrepancies found')

    # 5. Fiat settlement verification
    expected_settlement = sum(t['settled_amount'] for t in gateway_txns if t['type'] == 'deposit')
    actual_settlement = get_bank_credit(yesterday())
    if abs(expected_settlement - actual_settlement) > 1.00:  # $1 tolerance
        alert_finance(f'Settlement mismatch: expected ${expected_settlement}, received ${actual_settlement}')

    return generate_reconciliation_report(gateway_txns, internal_txns, discrepancies)
```

Settlement schedule:
- **Instant conversion:** Deposits converted to fiat within minutes (gateway handles)
- **Fiat settlement:** T+1 (next business day) via wire/SEPA to merchant bank
- **Reconciliation:** Daily automated + weekly manual audit
- **Reserve requirement:** Maintain 2x average daily withdrawal volume in fiat

**Validation:** Daily reconciliation runs without discrepancies for 30 consecutive days. Settlement matches within $1.

### Step 8: Monitoring, Alerts & Incident Response

**Action:** Set up comprehensive monitoring and incident response for crypto operations.

Monitoring dashboard metrics:
- [ ] Deposit volume (count + USD value) — real-time
- [ ] Withdrawal volume (count + USD value) — real-time
- [ ] Hot wallet balances per currency — real-time
- [ ] Gateway uptime — real-time (target 99.9%)
- [ ] Average confirmation time per currency — hourly
- [ ] Failed transactions rate — hourly (alert if >5%)
- [ ] Compliance flags (SAR, sanctions hits) — real-time
- [ ] Exchange rate deviation from market — per-transaction (alert if >2%)
- [ ] Settlement status — daily

Incident response playbook:

| Incident | Severity | Response |
|----------|----------|----------|
| Gateway API down | P1 | Switch to backup gateway. Disable crypto deposits. Notify users. |
| Hot wallet compromised | P0 | Freeze all wallets immediately. Disable withdrawals. Transfer cold to new infrastructure. Incident report within 1h. |
| Blockchain network congested | P2 | Increase fee estimates. Extend confirmation windows. Notify users of delays. |
| Sanctions hit on deposit | P1 | Freeze funds. Do NOT return to sender. File SAR. Contact compliance officer. |
| Exchange rate anomaly | P2 | Pause instant conversion. Switch to manual rate approval. |
| Reconciliation mismatch >$100 | P2 | Pause settlements. Manual audit. Resolve within 24h. |

**Validation:** All alerts tested (fire drill). Incident response rehearsed quarterly. Mean time to detect <5 min, mean time to respond <15 min.

## Fee Comparison Summary

| Method | Merchant Fee | Chargebacks? | Settlement | Best For |
|--------|-------------|-------------|------------|----------|
| Card (Stripe/Adyen) | 2.9% + $0.30 | Yes (Visa 10.4, MC 4837) | T+2 | Mainstream users |
| Carrier billing (DCB) | 30-50% to carrier | Limited (carrier disputes) | T+30-60 | Unbanked, mobile-first |
| Crypto (gateway) | 0.5-1% | No (irreversible) | T+1 | Privacy-focused, global |
| Crypto (direct) | Network fee only | No | Instant | Advanced users, lowest cost |

## Verificare
- [ ] Gateway selected and KYB approved
- [ ] Deposit flow tested on testnet for all supported currencies
- [ ] Withdrawal flow tested with all security controls (limits, 2FA, address whitelist)
- [ ] Hot/cold wallet strategy implemented with auto-sweep
- [ ] Blockchain analytics screening all deposits before crediting
- [ ] SAR filing workflow tested
- [ ] Travel Rule data collected for transfers >$1,000
- [ ] Daily reconciliation running with <$1 variance
- [ ] Monitoring dashboard live with all alert thresholds
- [ ] Incident response playbook documented and rehearsed

## Instrumente
- Crypto payment gateway (CoinsPaid, Alphapo, or NOWPayments)
- Blockchain analytics (Chainalysis KYT, Elliptic, or CipherTrace)
- Hardware wallets (Ledger Enterprise or equivalent)
- HSM or cloud KMS for key management
- Monitoring/alerting system (Datadog, Grafana, or equivalent)
- Reconciliation pipeline (custom or gateway-provided)

## Note
- Most major gateways (BitPay, Coinbase Commerce) restrict gambling — use gambling-focused providers.
- Instant fiat conversion eliminates crypto price volatility risk — always enable unless deliberately holding crypto.
- Hot wallet should hold maximum 10-20% of daily withdrawal volume — everything else in cold storage.
- Sanctioned address deposits must be frozen, NOT returned — returning violates sanctions law.
- 0-conf instant credit for small amounts (<$50) significantly improves UX but carries blockchain risk.
- Travel Rule compliance is mandatory for transfers >$1,000 — implement from day one.
- Crypto's zero-chargeback nature is its biggest advantage over card and carrier billing.

## Cortex Logging
- collection: procedures
- tags: payment-processing, training, crypto, bitcoin, ethereum, stablecoin, gateway, aml, kyc, chainalysis, hot-wallet, cold-wallet, settlement

## Enforcement Loop
- WHERE: Cryptocurrency payment integration for betting/gambling services
- WHEN: Adding crypto as a payment method or optimizing existing crypto operations
- HOW: Follow Steps 1-8 sequentially; Steps 1-2 (selection/compliance) gate all subsequent steps
- CONNECT: PP-101 (Stripe — card comparison), PP-102 (Adyen — risk scoring), PP-103 (carrier billing — alternative)

## Dependente
- Gambling license (Curacao, Malta MGA, UK GC, or equivalent)
- KYB documentation complete
- Blockchain analytics provider account
- Treasury team for settlement management
- Incident response team

## Metrics
- Completion: all 8 steps executed with testnet validation passed
- Quality: zero compliance violations, reconciliation variance <$1/day, MTTD <5 min
- Impact: crypto payment channel live and processing deposits/withdrawals

## Sources
- CoinsPaid documentation (coinspaid.com/docs)
- Alphapo documentation (alphapo.com)
- Chainalysis KYT documentation
- FATF Travel Rule guidance
- Gambling jurisdiction licensing requirements
