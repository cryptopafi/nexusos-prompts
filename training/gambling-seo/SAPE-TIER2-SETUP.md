---
type: procedure
created: 2026-03-20
status: active
slug: sape-tier2-setup
tags: [procedure, nexus]
---

# TRAIN-SEO-019: SAPE-TIER2-SETUP — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate Blackhat SEO + SAPE Marketplace Documentation -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Link Building / Tier 2 Automation / Budget Link Acquisition -->

---

## §1 Problema

Tier 1 assets (PBN sites, parasite pages) stagnate without incoming link equity. Manual T2 link building costs $500-2,000/month at scale. GSA SER provides volume but low quality. SAPE (sape.ru) fills the gap: contextual links on pages with real traffic at $2-4/month per link — a fraction of PBN or niche edit costs. Confirmed functional at T2 by Charles Floate (2024+). However, without proper implementation — wrong tier targeting (money site instead of T1), excessive exact-match anchors (>20% = footprint), or selecting pages without organic traffic (dead pages = zero link equity transfer) — SAPE links become a detectable footprint that risks the entire T1 layer. This procedure covers SAPE acquisition, quality filtering, anchor distribution, campaign management, and integration with the T0→T1→T2→T3 architecture.

**Critical rule**: SAPE links target T1 EXCLUSIVELY (PBN sites and parasite pages). NEVER point SAPE links directly at the money site (T0). SAPE = rented links (monthly payment), not permanent — if you stop paying, links disappear. This makes SAPE ideal for T2 (disposable tier) but dangerous for T1 (where link permanence matters).

---

## §2 Procedura

### Pas 1: UNDERSTAND SAPE LINK ECONOMICS & RISK PROFILE

**1.1 — What SAPE is**:
- Russian link marketplace (sape.ru) — webmasters sell contextual link placements on existing pages
- Model: monthly rent per link ($2-4/link/month), not permanent purchase
- Link type: contextual (within body content on real pages with organic traffic)
- Language: Interface in Russian — use Google Translate or English broker

**1.2 — SAPE vs alternatives comparison**:

| Method | Cost/link/month | Permanence | Quality | Footprint Risk | Best Tier |
|--------|----------------|-----------|---------|---------------|-----------|
| SAPE | $2-4 | Rented (stop paying = gone) | Medium (real pages) | Medium | T2 |
| GSA SER | $0.01-0.05 | Volatile (30-50% lost) | Low (spam platforms) | High | T2-T3 |
| PBN links | $50-100 one-time | Permanent (you control) | High | Low (if built properly) | T1 |
| Niche edits | $50-80 one-time | Semi-permanent | High | Low | T1-T2 |
| Guest posts | $100-300 | Permanent | High | Very Low | T1 |

**1.3 — When to use SAPE**:
- Budget constrained but need T2 link velocity
- Testing whether T2 links move T1 rankings before investing in permanent links
- Markets where GSA SER platforms are mostly in English but you need Spanish links (SAPE has Spanish-language inventory)
- Supplementing GSA SER with higher-quality contextual links

---

### Pas 2: SAPE ACCESS — Direct vs Broker

**Option A — Direct access (lowest cost, language barrier)**:
1. Create account at `sape.ru` (interface in Russian — use Chrome auto-translate)
2. Navigate: Registration → Advertiser account (рекламодатель)
3. Deposit minimum ~$20 USD equivalent in rubles (payment: WebMoney, PayPal, crypto accepted)
4. Dashboard: "Купить ссылки" (Buy Links) → filter by parameters
5. Verify account email and complete any identity verification

**Option B — English broker (recommended for non-Russian speakers)**:
1. Search BlackHatWorld: "SAPE links broker" or "SAPE marketplace English"
2. Vetted broker characteristics:
   - Active BHW account with 50+ positive reviews
   - Provides SAPE dashboard access or detailed reporting
   - Markup: +10-20% over direct SAPE prices (acceptable for convenience)
   - Accepts PayPal or crypto
3. Request sample report before bulk order: show page metrics, traffic, anchor placement

**Option C — SAPE alternatives with similar model**:
- Miralinks.com (Russian, higher quality, higher cost ~$10-20/link)
- GoGetLinks.net (Russian/English interface, similar pricing to SAPE)
- MainLink.ru (Russian, cheaper but lower quality)

---

### Pas 3: LINK QUALITY FILTERING — Page Selection Criteria

**3.1 — Mandatory quality thresholds** (ALL must pass):

| Criterion | Minimum Value | Why |
|----------|--------------|-----|
| Page organic traffic | ≥100 visits/month | Zero traffic = dead page = zero link equity |
| Page Authority (PA) | ≥20 (Moz) or URL Rating ≥15 (Ahrefs) | Below threshold = negligible equity |
| Content relevance | Gambling/finance/sports/entertainment | Irrelevant pages (e.g., gardening) = low topical equity |
| Content language | Spanish (Peru/Chile) or English (Kenya) | Must match target market language |
| Link type | Contextual (in-body) | Sidebar/footer/widget links = low value + footprint |
| Page age | ≥6 months indexed | New pages may be de-indexed |
| No outbound link farms | <10 outbound links on page | Pages with 50+ outbound links = link farms = zero value |

**3.2 — Red flags that disqualify a SAPE page**:
- Page has 0 organic traffic (Ahrefs/SEMrush check)
- Page is on a site with >50 SAPE links already selling (over-saturated)
- Page content is in a completely unrelated language
- Page has been deindexed (check `site:url` in Google)
- Same page already links to a competitor in your niche

---

### Pas 4: CAMPAIGN STRUCTURE — T2 Link Architecture

**4.1 — Campaign setup per T1 asset**:

```
T1 Target: https://medium.com/@user/casino-mpesa-kenya-2026
Campaign: SAPE_T2_Kenya_MPesa
Links: 10-15 SAPE links pointing to this T1 URL
Budget: $20-45/month
Duration: 3-6 months (evaluate at 90 days)
```

**4.2 — Anchor text distribution (CRITICAL)**:

| Anchor Type | Percentage | Examples |
|------------|-----------|---------|
| Naked URL | 30% | `https://medium.com/@user/casino-mpesa-kenya` |
| Generic | 25% | "click here", "read more", "this article", "learn more" |
| Branded partial | 25% | "casino Kenya guide", "M-Pesa betting article", "Kenya casino review" |
| Exact match keyword | 20% MAX | "mejores casinos online Peru", "casino M-Pesa Kenya" |

**HARD RULE**: Exact match anchors NEVER exceed 20% of total SAPE links to any single T1 URL. Above 20% = detectable pattern → risk to T1 asset.

**4.3 — Link velocity (drip-feed)**:
- Week 1: Activate 3-4 links (test batch)
- Week 2: Verify links are live and indexed → add 3-4 more
- Week 3-4: Reach target of 10-15 links total
- Do NOT activate all 15 links simultaneously (velocity spike = footprint)

---

### Pas 5: BUDGET PLANNING — Per Market

| Market | Links/campaign | Cost/link/mo | Monthly cost | Annual cost |
|--------|---------------|-------------|-------------|-------------|
| Peru (Spanish) | 10-12 | $2-3 | $20-36 | $240-432 |
| Kenya (English) | 10-12 | $3-4 | $30-48 | $360-576 |
| Chile (Spanish) | 12-15 | $2-3 | $24-45 | $288-540 |
| **Total (all 3 markets)** | **32-39** | — | **$74-129** | **$888-1,548** |

**ROI calculation**: If SAPE T2 links push a T1 parasite from position 15 to position 5 for a keyword with 500 monthly searches, and 3% click through to affiliate links with $50 CPA, that's: 500 × (CTR@5 - CTR@15) × 3% × $50 = significant ROI on $30/month investment.

---

### Pas 6: MONITORING & MAINTENANCE

**6.1 — Weekly monitoring (10 minutes)**:
- SAPE dashboard: verify all links are active (not removed by webmaster)
- Check: no links were moved from in-content to sidebar/footer
- Note: if >20% of links drop in a week → investigate (webmaster cleanup or site penalty)

**6.2 — Monthly monitoring (30 minutes)**:
- Ahrefs: check T1 referring domains — should be increasing
- Ahrefs: check T1 rankings for target keywords — should be improving or stable
- SAPE dashboard: review page traffic for SAPE pages — replace any that dropped below 100/month
- Budget reconciliation: actual spend vs planned

**6.3 — 90-day evaluation (campaign decision)**:

| T1 Ranking Change | Action |
|-------------------|--------|
| Improved ≥5 positions | Continue campaign, consider adding 5 more links |
| Improved 1-4 positions | Continue at current level |
| No change | Evaluate link quality — replace low-quality links, test different anchors |
| Declined | STOP campaign immediately, audit for footprint issues |

**6.4 — Link rotation strategy**:
- Every 3 months: replace bottom 20% of links (lowest traffic pages) with fresh ones
- This prevents stale link profiles and introduces natural variation
- SAPE allows easy replacement — cancel old link, add new one

---

### Pas 7: INTEGRATION WITH OTHER T2 METHODS

**7.1 — SAPE + GSA SER combination** (recommended):

```
T1 Target (parasite page or PBN)
    ↑
    ├── SAPE links (10-15) → Higher quality, contextual, real traffic
    ├── GSA SER links (20-50/day) → Volume, Web 2.0s, forum profiles
    └── Both feed link equity to T1 simultaneously
```

- SAPE provides the quality contextual signal
- GSA SER provides the volume/velocity signal
- Combined: faster T1 ranking movement than either alone

**7.2 — SAPE replacing PBN links at T2**:
- For operators who don't want to build/maintain PBN: SAPE can partially replace T1 PBN links
- CAVEAT: SAPE links are rented (disappear if you stop paying), PBN links are permanent
- Recommendation: use SAPE for testing keywords, then invest in permanent PBN/niche edits for confirmed winners

---

## §3 Cortex Logging

```json
{
  "text": "SAPE T2 campaign for {market}. Target T1: {url}. Links active: {N}. Monthly cost: ${X}. Anchor distribution: {naked}%/{generic}%/{branded}%/{exact}%. T1 ranking before: {X}, after: {Y}. Link survival rate: {X}%. Page traffic average: {X}/mo. Next evaluation: {date}.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SAPE-TIER2-SETUP",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-019",
    "version": "2.0",
    "tags": ["sape", "tier2", "link-building", "rented-links", "budget-seo", "casino", "gambling-seo", "peru", "kenya", "chile"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

**WHERE**: T2 link building for parasite pages and PBN sites. NEVER directly toward money site (T0).

**WHEN**: After T1 (parasite page) has been published and indexed (minimum 7 days post force-index). Campaign evaluation at 90 days. Monthly monitoring.

**HOW**:
1. Identify T1 target URL (parasite or PBN)
2. Choose SAPE access method (direct or broker)
3. Select 10-15 links per quality criteria (Pas 3)
4. Set anchor mix per distribution table (Pas 4.2)
5. Activate campaign in drip-feed mode (Pas 4.3)
6. Monitor weekly/monthly per schedule (Pas 6)
7. Evaluate at 90 days — continue/adjust/stop

**Violations**:
- SAPE links pointing to T0 (money site) → **violation CRITICAL**
- Exact match anchors >20% → violation (footprint risk)
- Links on pages with 0 organic traffic → violation (wasted budget)
- All links activated same day (no drip-feed) → violation
- No monitoring for >30 days → violation
- Campaign continued despite T1 ranking decline → violation

**CONNECT**:
- → `PARASITE-PAGE-BUILD.md` (TRAIN-SEO-004) — T1 target for SAPE
- → `PARASITE-LINK-BUILDING.md` (TRAIN-SEO-006) — SAPE = T2 alternative to standard link building
- → `GSA-SER-TIER2-SETUP.md` (TRAIN-SEO-007) — GSA = complement to SAPE for T2-T3 volume
- → `TIERED-LINK-ARCHITECTURE.md` (TRAIN-SEO-008) — architectural context T0-T1-T2-T3
- → `BACKLINK-QUALITY-AUDIT.md` (TRAIN-SEO-020) — evaluate SAPE links by real traffic, not DA

**VERIFY**:
`[VK-019-A] SAPE links active: verified in dashboard — 0 links dropped in first 7 days`
`[VK-019-B] Target T1: CONFIRMED not money site — T1 URL verified before activation`
`[VK-019-C] Page traffic: ≥100/month organic on every SAPE page — verified in Ahrefs`
`[VK-019-D] Anchor distribution: ≤20% exact match — calculated and documented`
`[VK-019-E] Drip-feed: links activated over 2-3 weeks, not all at once`

---

## §5 Dependențe

| Tool | Purpose | Cost |
|------|---------|------|
| **sape.ru** (or English broker) | Link marketplace — primary platform | $20-50/mo per campaign |
| **Ahrefs** | Verify SAPE page traffic + monitor T1 ranking changes | $99+/mo |
| **BHW Marketplace** | Find vetted SAPE brokers with English interface | Free (BHW membership) |
| **Google Chrome** (with translate) | Navigate sape.ru Russian interface | Free |

**DISCLAIMER**: Link buying/renting contravenes Google Webmaster Guidelines. This procedure describes techniques used in the SEO industry. Application is the user's sole responsibility. Verify legality of casino operations in each target jurisdiction.
