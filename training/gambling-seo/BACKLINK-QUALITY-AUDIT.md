---
type: procedure
created: 2026-03-20
status: active
slug: backlink-quality-audit
tags: [procedure, nexus]
---

# TRAIN-SEO-020: BACKLINK-QUALITY-AUDIT
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate Blackhat SEO -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Link Building / Quality Evaluation -->

---

## §1 Problema

Standard metrics (DA/DR/PA) are third-party calculations that do NOT reflect real SEO value. A DR70 domain with zero organic traffic is worthless — potentially harmful — compared to a DR35 domain with 15,000 real monthly visitors. In the gambling/casino niche, this problem is amplified because:

- Link sellers inflate DA/DR through circular PBN networks (detectable by Google, invisible to buyers)
- Gambling-adjacent sites attract spam links that artificially inflate authority metrics
- Regional markets (Peru/Kenya/Chile) have different quality baselines than US/UK
- SAPE, GSA SER, and niche edit marketplaces sell links by DA — creating a perverse incentive to inflate metrics
- A single toxic T1 link can propagate penalty risk to your money site through the tiered architecture

Philosophy from Floate: **"Metrics are bullshit — traffic and link profiles matter."**

Without this audit framework, operators spend $500-2,000/month on links that deliver zero ranking improvement or — worse — trigger manual penalties.

---

## §2 Procedura

### Pas 1: Primary Evaluation Framework — Traffic Over Metrics

**The only metrics that matter for gambling SEO link evaluation**:

| Metric | How to Verify | Minimum Threshold (T1) | Minimum Threshold (T2) | Tool |
|--------|---------------|------------------------|------------------------|------|
| Page organic traffic | Ahrefs → Site Explorer → specific URL | ≥ 200 visits/month | ≥ 50 visits/month | Ahrefs |
| Domain organic traffic | Ahrefs → Site Explorer → domain | ≥ 1,000 visits/month | ≥ 200 visits/month | Ahrefs |
| Traffic trend (12mo) | Ahrefs → Traffic History graph | Stable or growing | Not declining >30% | Ahrefs |
| Link profile natural | Ahrefs → Backlinks → diversity check | Mix of sources, not mono-source | Basic diversity | Ahrefs |
| Topical relevance | Manual review of page content | Gambling/finance/sports adjacent | Any non-spam niche | Manual |
| Page indexation | Google: `site:domain.com/url-specific` | Page must be indexed | Must be indexed | Google |
| Referring domain diversity | Ahrefs → Referring Domains → unique count | ≥ 30 unique RDs | ≥ 10 unique RDs | Ahrefs |
| Anchor text profile | Ahrefs → Anchors → distribution | No single anchor >30% | No single anchor >50% | Ahrefs |

### Pas 2: Red Flags — Immediate Rejection Criteria

| Red Flag | How to Detect | Risk Level |
|----------|---------------|------------|
| High DA/DR, zero traffic | Ahrefs: Organic Traffic = 0 on domain/page | CRITICAL — inflated metrics |
| Link farm (excessive OBL) | Ahrefs: Outbound Links on page > 50 | HIGH — link equity diluted to zero |
| Page not indexed | `site:domain.com/url` = 0 results in Google | CRITICAL — zero value |
| Manual penalty history | Traffic cliff >80% in Ahrefs history | CRITICAL — penalty contagion risk |
| Spam anchor ratio | Ahrefs: Backlinks anchors = 90%+ spam anchors | HIGH — toxic profile |
| Same C-class IP as your PBN | Check via ipinfo.io | CRITICAL — footprint exposure |
| Recent ownership change + content pivot | WHOIS history + Wayback Machine | HIGH — possible previous penalty |
| Foreign language mismatch | Page in Russian/Chinese targeting Peru/Kenya/Chile | MEDIUM — relevance dilution |
| Site is on known "SEO hosting" | Check nameservers against known SEO hosting providers | HIGH — Google monitors these |
| Content auto-generated/spun | Manual review — gibberish, repetitive, thin | HIGH — quality signal contamination |

### Pas 3: Rapid Evaluation Checklist (5 Minutes Per Link)

Execute this checklist for EVERY proposed link before purchase:

```
RAPID LINK EVALUATION — 5 MINUTE PROTOCOL
==========================================

[ ] 1. Ahrefs Site Explorer → specific PAGE → Organic Traffic ≥ 200/month (T1) or ≥ 50 (T2)
[ ] 2. Ahrefs → DOMAIN → Organic Traffic ≥ 1,000/month (T1) or ≥ 200 (T2)
[ ] 3. Ahrefs → Traffic History → No cliff drops >40% in last 12 months
[ ] 4. Google: site:domain.com/url → page is indexed (returns results)
[ ] 5. Manual review: real content (not auto-generated), topically relevant
[ ] 6. Ahrefs → Backlinks domain → diversity (not 95% from single domain)
[ ] 7. Outbound links on page: < 25 (strict) or < 40 (acceptable for news sites)
[ ] 8. Anchor text profile: no single anchor text > 30% of total
[ ] 9. IP check: not same C-class as any of your PBN/money sites
[ ] 10. Language match: content language matches your target market

SCORING:
≥ 9/10 = STRONG ACCEPT → pay asking price
7-8/10 = ACCEPT → negotiate 15-25% discount
5-6/10 = WEAK → negotiate 40-50% discount or reject
< 5/10 = REJECT → do not purchase at any price
```

### Pas 4: Value Assessment — Real Price vs. Quoted Price

**Price calibration framework for gambling niche links**:

| Link Profile | Typical Quoted Price | Fair Market Value | Why |
|-------------|---------------------|-------------------|-----|
| DR70, 0 traffic, 200+ OBL | $200-400 | $0 (reject) | Zero value — inflated metrics |
| DR45, 12K traffic, 15 OBL | $100-150 | $100-150 (fair) | Real traffic = real value |
| DR55, 5K traffic, 25 OBL | $120-200 | $120-180 (fair) | Moderate value |
| DR30, 2K traffic, gambling niche | $60-100 | $80-120 (premium) | Topical relevance premium |
| DR80+, 100K+ traffic, news outlet | $500-2,000 | $500-1,500 (varies) | Authority + trust signal |
| Regional news (Peru/Kenya/Chile) DR50+ | $300-800 | $300-600 (fair) | Local relevance premium |

**Negotiation rule**: Never pay full quoted price on first offer. Counter at 60-70% of quoted price. Most sellers have 30-40% margin built in. Volume discount: 2+ links = 15-25% off.

### Pas 5: Market-Specific Quality Benchmarks

**Peru link evaluation adjustments**:
- Spanish language content required for T1 links
- DR thresholds 15-20% lower than US/UK (DR25+ = good for Peru)
- Local news outlets (.pe domains) carry geographic relevance premium
- MINCETUR-related pages = high trust signal for gambling content
- Liga 1 / sports pages = excellent topical adjacency

**Kenya link evaluation adjustments**:
- English content acceptable (Kenya English standard)
- DR thresholds lowest of three markets (DR15+ = acceptable for T1)
- .co.ke domains carry local relevance signal
- BCLB-related pages = high trust signal
- Sports/betting pages abundant and affordable ($30-60/link)
- M-Pesa/Safaricom-related content = strong topical bridge

**Chile link evaluation adjustments**:
- Spanish content required (Chilean or neutral LATAM acceptable)
- DR thresholds higher than Peru/Kenya (DR25+ for T1)
- .cl domains carry geographic relevance
- Ley 21.456 / SCJ-related content = premium trust signal
- Casino de Vina del Mar mentions = cultural relevance bonus

### Pas 6: SAPE Link Evaluation Protocol

For SAPE links (procedure TRAIN-SEO-019), apply additional filters:

1. Filter in sape.ru: minimum traffic set to 100 visits/month
2. Verify in Ahrefs that page is indexed (SAPE pages can be deindexed)
3. Check topical relevance: at minimum adjacent to niche (sport, entertainment, finance acceptable)
4. Reject: purely Russian pages if targeting Peru/Kenya/Chile (linguistic relevance matters)
5. Verify: contextual placement (not sidebar/footer — must be in-content)
6. Check: page doesn't have >30 outbound links (SAPE pages can accumulate spam OBL)
7. Monitor monthly: SAPE links can be replaced by site owners without notice

### Pas 7: Own Link Profile Audit (Quarterly)

Run on money site or parasite pages quarterly:

1. Ahrefs → Site Explorer → Backlinks → filter "dofollow"
2. Sort by: Referring Page Traffic (ascending) → identify links with zero traffic
3. Classify each link: KEEP / MONITOR / DISAVOW
4. **KEEP**: Real traffic, relevant niche, natural anchor
5. **MONITOR**: Low traffic but non-toxic, recently acquired
6. **DISAVOW**: Explicit spam (porn, pharma, auto-generated), penalty-causing anchors
7. Disavow ONLY links with: explicit spam anchors AND page penalty indicators AND zero traffic
8. Do NOT mass-disavow — aggressive disavowal can harm more than help

**Disavow thresholds for gambling sites**:
- Only disavow if link meets ALL three criteria: spam anchor + zero traffic + site penalized/deindexed
- Never disavow links from domains with real traffic, even if anchor text looks unnatural
- Submit disavow file quarterly, not more frequently (Google processes slowly)

### Pas 8: Competitor Backlink Intelligence

When evaluating a market, reverse-engineer competitor link profiles:

1. Identify top 3 ranking competitors for your primary keyword
2. Ahrefs → Site Explorer → each competitor → Backlinks → filter: dofollow, DR > 20
3. Export and analyze: which link sources do 2+ competitors share?
4. Shared sources = validated quality (multiple competitors found value)
5. Attempt to acquire from same sources (outreach or marketplace)
6. Unique sources of strongest competitor = priority targets for gap closure

### Pas 9: Link Portfolio Health Dashboard

Maintain a master spreadsheet with monthly snapshots:

| Column | Description |
|--------|-------------|
| Domain | Linking domain |
| URL | Specific linking page |
| DR/DA | At time of acquisition (reference only) |
| Page Traffic | Monthly organic traffic (Ahrefs) |
| Domain Traffic | Monthly organic traffic (Ahrefs) |
| Tier | T1 / T2 / T3 |
| Anchor Text | Exact anchor used |
| Link Type | PBN / Niche Edit / SAPE / News Outlet / Guest Post |
| Date Acquired | YYYY-MM-DD |
| Cost | USD |
| Status | Active / Lost / Disavowed |
| Last Verified | YYYY-MM-DD |
| Traffic Trend | Growing / Stable / Declining / Dead |

Update monthly. Flag any link where traffic drops >50% for investigation.

---

## §3 Cortex Logging

```json
{
  "text": "Backlink Quality Audit executed. Links evaluated: [N]. Accepted: [N] (avg traffic [X]/mo). Rejected: [N] (top reasons: [list]). Cost saved by rejection: $[X]. Portfolio health: [N] active, [N] monitored, [N] disavowed. Market: [Peru/Kenya/Chile].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "BACKLINK-QUALITY-AUDIT",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-020",
    "tags": ["backlink-audit", "link-quality", "gambling-seo", "traffic-based-evaluation"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

**WHERE**: At every link acquisition (PBN, niche edit, SAPE, news outlet, guest post)

**WHEN**: BEFORE payment. No link purchased without rapid evaluation (5 minutes). Quarterly for portfolio audit.

**HOW**:
1. Receive link proposal / find link in marketplace
2. Run rapid evaluation checklist (Pas 3) — 5 minutes
3. Score: ≥7/10 = pay; 5-6 = negotiate discount; <5 = reject
4. Save decision in Cortex
5. Quarterly: run portfolio audit (Pas 7) on all active links

**CONNECT**:
- → `SAPE-TIER2-SETUP.md` (TRAIN-SEO-019) — evaluate each SAPE link with this framework
- → `PARASITE-LINK-BUILDING.md` (TRAIN-SEO-006) — PBN + niche edits evaluated via traffic
- → `NEWS-OUTLET-OUTREACH.md` (TRAIN-SEO-011) — news outlets evaluated via traffic (not DA)
- → `TIERED-LINK-ARCHITECTURE.md` (TRAIN-SEO-008) — context of where links are used
- → `EXPIRED-DOMAIN-ACQUISITION.md` (TRAIN-SEO-002) — domain quality pre-purchase

**VERIFY**:
`[VK-020-A] Zero links purchased without traffic verification in Ahrefs`
`[VK-020-B] Rapid checklist (10 points) documented for each acquired link`
`[VK-020-C] No T1 link with domain traffic < 1,000/month`
`[VK-020-D] Quarterly portfolio audit completed with status updates`

---

## §5 Dependente

- **Ahrefs** (or Semrush) — traffic verification for domain + page
- **Google Search** — indexation verification: `site:domain.com/url`
- **ipinfo.io** — IP/C-class check for footprint avoidance
- **Google Sheets** — link portfolio health dashboard
- 5 minutes per link evaluation, 2 hours per quarterly audit

*Procedure v2.0 | Updated: 2026-03-20*
