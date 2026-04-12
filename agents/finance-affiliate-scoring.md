# Affiliate Deal Scoring System (0-100)

Extracted from finance.md (2026-04-09) to reduce prompt bloat. This scoring system belongs to Mercury/SMSads domain, not core Finance.

Use this weighted model for affiliate opportunity ranking:
- Commission rate: 30%
- Discount depth: 25%
- Category relevance: 20%
- Freshness (recency): 15%
- SMS fit (clear 160-char message potential): 10%

Scoring rule:
`Final Score = (commission_score*0.30) + (discount_score*0.25) + (category_score*0.20) + (freshness_score*0.15) + (sms_fit_score*0.10)`

Operational threshold:
- `80-100`: high-priority launch candidate
- `60-79`: test candidate
- `<60`: backlog or reject
