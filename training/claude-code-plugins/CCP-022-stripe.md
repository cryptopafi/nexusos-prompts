---
type: procedure
created: 2026-03-20
status: active
slug: ccp-022-stripe
tags: [procedure, nexus]
---

# CCP-022 — Stripe Payments with Best Practices Enforcement

## Problema
Implementing Stripe integrations incorrectly leads to security vulnerabilities, PCI compliance issues, and deprecated API usage. The Stripe plugin provides both MCP API access and a curated best-practices skill that guides Claude toward modern, correct Stripe integration patterns — preventing the use of deprecated APIs like Charges or Card Element.

## Procedura

### Prerequisites
- Stripe account with API access
- Install: `/plugin install stripe@claude-plugins-official`
- MCP type: HTTP to `https://mcp.stripe.com`
- Authentication handled via Stripe's MCP server (OAuth or API key)

### Steps

1. **Install the plugin**:
   ```
   /plugin install stripe@claude-plugins-official
   ```

2. **MCP configuration** (HTTP, no local server):
   ```json
   {
     "stripe": {
       "type": "http",
       "url": "https://mcp.stripe.com"
     }
   }
   ```

3. **Best practices skill activates automatically** for payment implementation tasks. Key enforced rules:

   - **Preferred API hierarchy**: CheckoutSessions > PaymentIntents > Invoicing/Payment Links/Subscription APIs
   - **NEVER use**: Charges API (advise migration to CheckoutSessions/PaymentIntents), Card Element (advise migration to Payment Element), Sources API (use SetupIntents instead for saving cards)
   - **Frontend**: Prioritize Stripe-hosted Checkout or embedded checkout. Payment Element is acceptable for advanced customization.
   - **Dynamic payment methods**: Use dashboard settings instead of passing `payment_method_types` — Stripe auto-selects based on location/wallets/preferences
   - **Subscriptions**: Combine Billing APIs + Stripe Checkout for recurring revenue. Use Subscription Use Cases docs.
   - **Connect**: Prefer direct charges (Stripe takes risk) or destination charges (platform accepts liability). Use `on_behalf_of`. Never mix charge types. Use controller properties (not deprecated Standard/Express/Custom terms).

4. **Two included slash commands**:
   - `/stripe:explain-error [error_code]`: Explains Stripe error codes in plain English, lists common causes, provides code samples for error handling, mentions related error codes
   - `/stripe:test-cards [scenario]`: Shows test card numbers for a scenario (declined, 3DS, fraud, etc.) with expected behavior and links to testing docs

5. **PCI compliance guidance**: If a user needs to send raw PAN data server-side, the skill advises proving PCI compliance first and points to the data migration process for importing PAN data from other processors.

6. **Use the Go Live Checklist** before deploying: `https://docs.stripe.com/get-started/checklist/go-live.md`

7. **For Stripe Confirmation Tokens**: Use when rendering Payment Element before creating a PaymentIntent/SetupIntent (e.g., for surcharging). Do not use `createPaymentMethod` or `createToken` in this case.

### Verification
- [ ] Plugin installs without error
- [ ] Best practices skill activates when implementing Stripe checkout code
- [ ] Claude recommends CheckoutSessions, not Charges API
- [ ] `/stripe:explain-error card_declined` returns actionable explanation
- [ ] `/stripe:test-cards 3dsecure` returns correct test card numbers

## Cortex Logging
- collection: procedures
- tags: claude-code-plugins, training, stripe, payments, mcp, best-practices, v2

## Enforcement Loop
- WHERE: Any project implementing payment processing with Stripe
- WHEN: Building checkout flows, subscriptions, webhooks, Connect platforms
- HOW: Install plugin; best practices activate automatically; use `/stripe:test-cards` during development
- CONNECT: CCP-025 (supabase for backend storing payment data)
