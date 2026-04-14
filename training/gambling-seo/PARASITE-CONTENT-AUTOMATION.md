---
type: procedure
created: 2026-03-20
status: active
slug: parasite-content-automation
tags: [procedure, nexus]
---

# TRAIN-SEO-025: PARASITE-CONTENT-AUTOMATION — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Julian Goldie Parasite SEO Course + Make.com Automation -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Automation / Content Operations / Scale Management -->

---

## §1 Problema

At scale (20+ active parasite pages across Peru, Kenya, Chile), manual monthly refresh becomes a 10-15h/month time sink: checking rankings, updating bonus offers, refreshing dates, verifying affiliate links, re-indexing updated pages, and monitoring platform ToS compliance. Without automation, refresh cadence slips → Google's freshness signals decay → rankings drop → traffic and revenue decline. At 50+ pages, manual management becomes impossible without a team. Make.com automation reduces monthly maintenance to 2-3h (review-only), with automated content generation, scheduled publishing, force re-indexing, and tracking — freeing operator time for strategy and expansion.

**What CAN be automated**: Content refresh generation (AI + human review gate), re-indexing requests, tracking spreadsheet updates, alert notifications for ranking drops, affiliate link verification.

**What CANNOT be automated** (YMYL gambling requirement): Final content review before publication (mandatory human gate), affiliate link insertion decisions, platform ToS monitoring, response to moderation actions, strategic keyword targeting decisions.

---

## §2 Procedura

### Pas 1: TRACKER SETUP — Google Sheets Master Database

**1.1 — Create master parasite tracker** with these columns:

| Column | Description | Example |
|--------|-------------|---------|
| URL | Full URL of parasite page | `https://medium.com/@user/casino-mpesa-kenya` |
| Platform | Hosting platform | Medium / LinkedIn / Substack / Vocal |
| Market | Target market | Peru / Kenya / Chile |
| Primary Keyword | Main target keyword | "casino M-Pesa Kenya" |
| Current Position | Ahrefs rank (updated weekly) | 7 |
| Last Refresh Date | Date of last content update | 2026-02-15 |
| Next Refresh Date | Scheduled refresh (auto-calculated: +30 days) | 2026-03-17 |
| Affiliate Links | Number of active affiliate links | 3 |
| Affiliate Links Status | All working? | OK / BROKEN |
| Monthly Traffic | Ahrefs estimated organic | 450 |
| Revenue (est.) | Monthly affiliate revenue from this page | $75 |
| Status | Page status | Active / Paused / Removed / Backup |
| Backup URLs | Redundant pages for same keyword | `https://vocal.media/...` |
| Notes | Manual notes | "Update bonus amounts Q2" |

**1.2 — Formulas to add**:
```
Next Refresh Date: =IF(E2="Active", F2+30, "N/A")
Days Until Refresh: =IF(G2<>"N/A", G2-TODAY(), "N/A")
Overdue Flag: =IF(AND(G2<>"N/A", G2<TODAY()), "OVERDUE", "OK")
```

---

### Pas 2: MAKE.COM AUTOMATION — Auto-Refresh Workflow

**2.1 — Architecture overview**:

```
[Google Sheets Trigger] ─── checks daily at 09:00 UTC
    │ Filter: Next_Refresh_Date ≤ TODAY() AND Status = "Active"
    ↓
[Claude API / OpenAI API] ─── generates 200-300 word update section
    ↓
[Google Doc Buffer] ─── stores draft for human review
    ↓
[Slack/Telegram Notification] ─── "3 pages ready for review"
    ↓
[Human Review] ─── approves/edits in Google Doc (MANDATORY)
    ↓
[Platform API] ─── publishes approved update (WordPress REST / manual for Medium)
    ↓
[Indexing API] ─── force re-index updated URL
    ↓
[Google Sheets Update] ─── Last_Refresh = TODAY(), Next_Refresh = TODAY()+30
```

**2.2 — Make.com scenario setup (step-by-step)**:

**Step A: Trigger**
- Module: Google Sheets → Watch Rows
- Schedule: Daily at 09:00 UTC
- Filter condition: Column "Overdue Flag" = "OVERDUE" AND Column "Status" = "Active"

**Step B: AI Content Generation**
- Module: HTTP → Make a Request
- URL: `https://api.anthropic.com/v1/messages` (Claude) or `https://api.openai.com/v1/chat/completions` (OpenAI)
- Method: POST
- Prompt template:

```
Generate a content update section (200-300 words) for a casino review article targeting {market}.

Topic: {primary_keyword}
Platform: {platform}
Language: {Spanish for Peru/Chile, English for Kenya}

Include:
1. A "Update [Month] 2026:" header
2. One new casino operator or bonus offer relevant to {market}
3. Updated statistics or market information
4. 2-3 practical tips for players
5. Maintain {casual/professional} tone appropriate for {market}

DO NOT:
- Repeat information from previous updates
- Include specific affiliate links (those are added manually)
- Make unverified claims about operator licensing
- Use generic filler content

Include local references: {M-Pesa/KES for Kenya | PagoEfectivo/Soles for Peru | WebPay/CLP for Chile}
```

**Step C: Buffer + Notification**
- Module: Google Docs → Create Document (titled: "Review: {keyword} — {date}")
- Module: Slack/Telegram → Send Message: "3 parasite pages ready for review in [Google Docs folder link]"

**Step D: After human approval** (manual trigger or approval webhook):
- Module: WordPress REST API → Update Post (for WP-hosted parasites)
- For Medium/LinkedIn: manual publish (APIs don't support content updates)
- Module: HTTP → POST to indexing API

**Step E: Tracking update**
- Module: Google Sheets → Update Row
- Set: Last_Refresh = TODAY(), Next_Refresh = TODAY()+30, Overdue_Flag recalculates

**2.3 — Make.com pricing**:
- Starter ($10.59/mo): 10,000 operations — sufficient for 20-30 pages
- Core ($18.82/mo): 10,000+ operations — for 30-50 pages
- Pro ($34.12/mo): 10,000+ with priority — for 50+ pages

---

### Pas 3: INDEXING TOOLS API INTEGRATION

**3.1 — Omega Indexer (~$30/mo) — recommended for volume**:
```
POST https://api.omegaindexer.com/submit
Headers: Content-Type: application/json
Body: {
  "urls": ["https://medium.com/@user/article-slug"],
  "api_key": "your_key"
}
Response: {"status": "submitted", "estimated_time": "24-72h"}
```

**3.2 — IndexMeNow (~$20/mo) — budget alternative**:
```
POST https://api.indexmenow.com/v1/index
Headers: Authorization: Bearer your_api_key
Body: {
  "url": "https://medium.com/@user/article-slug"
}
```

**3.3 — Google Indexing API (free, limited)**:
- Works for sites you own (verify in GSC)
- Does NOT work for Medium, LinkedIn, etc. (third-party platforms)
- Use for money site pages and WordPress parasites you control

**3.4 — Social signal indexing (free, supplementary)**:
- Auto-share updated URL on Twitter/X via Make.com
- Auto-post in relevant subreddit (careful with spam rules)
- LinkedIn share for professional parasite pages

---

### Pas 4: AFFILIATE LINK HEALTH MONITORING

**4.1 — Automated link checker** (Make.com scenario):

```
[Weekly Trigger] ─── every Monday 08:00
    ↓
[Google Sheets] ─── get all Active URLs
    ↓
[HTTP Module] ─── GET request to each URL
    ↓
[Parse HTML] ─── extract all affiliate links (pattern: rebrandly.com/*, bit.ly/*, etc.)
    ↓
[HTTP Module] ─── follow redirect chain for each affiliate link
    ↓
[Check] ─── final URL resolves to casino operator? 200 status?
    ↓
[If broken] ─── update Sheets "Affiliate Links Status" = "BROKEN"
    ↓
[Notification] ─── "Broken affiliate links found on {N} pages — review needed"
```

**4.2 — Common affiliate link failures**:
- Casino operator changed tracking URL structure → update Rebrandly destination
- Affiliate program suspended your account → re-apply or switch program
- Platform (Medium) removed your article for ToS violation → activate backup page
- Rebrandly/link masking service expired → renew subscription

---

### Pas 5: RANKING DROP ALERT SYSTEM

**5.1 — Ahrefs Rank Tracker integration** (if API available):

```
[Daily Trigger] ─── 10:00 UTC
    ↓
[Ahrefs API] ─── get current position for each tracked keyword
    ↓
[Compare] ─── vs position from 7 days ago
    ↓
[If drop > 5 positions] ─── ALERT: "Ranking drop detected"
    ↓
[Notification] ─── "{keyword} dropped from #{X} to #{Y} — investigate"
    ↓
[Google Sheets] ─── log drop event with date, positions, suggested actions
```

**5.2 — Manual alternative** (if no Ahrefs API):
- Weekly Google incognito check for top 10 keywords (with VPN to target country)
- Log positions in Sheets manually
- Conditional formatting: highlight cells where position increased by >3

**5.3 — Automated response to ranking drops**:

| Drop Size | Automated Action | Human Action Required |
|-----------|-----------------|----------------------|
| 1-3 positions | Log only | Review at weekly check |
| 4-7 positions | Generate content refresh, notify | Review + approve refresh, check competitor activity |
| 8+ positions | Notify URGENT, pause SAPE links | Full audit: page indexed? Platform ToS? Algorithm update? Competitor surge? |
| Page removed | Notify CRITICAL, activate backup | Publish new version on different platform |

---

### Pas 6: NEW CONTENT SCHEDULING — Publishing Pipeline

**6.1 — Content calendar automation**:

```
[Google Sheets "Content Calendar"]
    ↓
Columns: Keyword | Market | Target Publish Date | Content Status | Platform | Writer
    ↓
[Make.com Weekly Trigger] ─── Monday 08:00
    ↓
[Filter] ─── Target Publish Date = THIS WEEK
    ↓
[Notification] ─── "This week: publish {N} new parasite pages"
    ↓
[AI Brief Generation] ─── generate content brief per keyword (title, structure, LSI terms, word count)
    ↓
[Google Doc] ─── create brief document for writer/AI
```

**6.2 — AI content generation for new pages** (2,000+ words):
- Generate via Claude API with detailed prompt (see AI-CONTENT-CASINO procedure)
- MANDATORY human review before publication (YMYL gambling content)
- Review checklist: factual accuracy, local relevance, affiliate disclosure, responsible gambling notice, no ToS violations

**6.3 — Platform-specific publishing constraints**:

| Platform | API Publishing | Refresh Capability | Automation Level |
|----------|---------------|-------------------|-----------------|
| WordPress.org | Full REST API | Full update API | 100% automatable |
| WordPress.com | REST API (limited) | Update available | 90% automatable |
| Medium | Read-only API | Manual only | Notification only |
| LinkedIn Articles | No API | Manual only | Notification only |
| Substack | No API | Manual via email | Partial (email draft) |
| Vocal Media | No API | Manual only | Notification only |
| Blogger/Blogspot | Blogger API v3 | Full update | 100% automatable |
| Google Sites | No public API | Manual only | Notification only |

---

### Pas 7: SCALE BENCHMARKS & COST OPTIMIZATION

**7.1 — Time savings by scale**:

| Parasite Pages | Manual Time/Month | Automated Time/Month | Savings |
|---------------|-------------------|---------------------|---------|
| 5-10 | 2-5h | 30 min (review only) | 75-90% |
| 10-20 | 5-10h | 1h (review) | 80-90% |
| 20-50 | 10-25h | 2-3h (review) | 80-88% |
| 50-100 | 25-50h | 3-5h (review) | 88-90% |
| 100+ | 50h+ (needs team) | 5-8h (review) | 84-90% |

**7.2 — Monthly automation costs**:

| Component | Cost | Scale |
|-----------|------|-------|
| Make.com | $10-35/mo | Depends on operations volume |
| Claude API (content generation) | $5-20/mo | ~$0.50-1.00 per 300-word update |
| Omega Indexer | $30/mo | Up to 10,000 URLs/mo |
| Google Sheets | Free | Unlimited |
| **Total** | **$45-85/mo** | Covers 20-100 pages |

**7.3 — Break-even calculation**:
- If automation saves 15h/month at operator value of $30/h = $450/month value
- Automation cost: $45-85/month
- Net benefit: $365-405/month (or reinvest saved time into strategy)

---

## §3 Cortex Logging

```json
{
  "text": "Parasite content automation cycle. Pages automated: {N}. Refreshed this month: {N}. Auto-indexed: {N}. Broken affiliate links detected: {N}. Ranking drops detected: {N}. Make.com operations used: {N}. Monthly cost: ${X}. Time saved: {X}h. New pages scheduled: {N}.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PARASITE-CONTENT-AUTOMATION",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-025",
    "version": "2.0",
    "tags": ["automation", "make.com", "parasite-seo", "content-refresh", "indexing", "scaling", "casino", "gambling-seo"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

**WHERE**: At ≥10 active parasite pages — below 10, manual is more efficient.

**WHEN**: Setup once. Monthly monitoring that Make.com scenarios run correctly. Quarterly optimization of prompts and workflows.

**HOW**:
1. Create Google Sheets tracker with all active parasite pages (Pas 1)
2. Setup Make.com auto-refresh scenario (Pas 2)
3. Connect indexing API (Pas 3)
4. Setup affiliate link health monitor (Pas 4)
5. Setup ranking drop alerts (Pas 5)
6. Test with 2-3 URLs before full activation
7. Monthly: verify all scenarios executed, review AI-generated content quality

**Violations**:
- AI-generated gambling content published without human review → **violation CRITICAL** (YMYL risk)
- Parasite page not refreshed for >45 days → violation (freshness signal decay)
- Broken affiliate links not fixed within 7 days of detection → violation (lost revenue)
- Ranking drop >8 positions ignored for >48h → violation
- Make.com scenario broken for >7 days without fix → violation

**CONNECT**:
- → `PARASITE-PAGE-LIFECYCLE.md` (TRAIN-SEO-024) — lifecycle management that automation supports
- → `PARASITE-PAGE-BUILD.md` (TRAIN-SEO-004) — initial build (not automated, manual quality)
- → `AI-CONTENT-CASINO.md` (TRAIN-SEO-010) — AI content generation prompts and quality standards
- → `CASINO-AFFILIATE-SETUP.md` (TRAIN-SEO-009) — affiliate link management integration

**VERIFY**:
`[VK-025-A] Make.com scenario: all parasite pages with next_refresh ≤ TODAY() processed automatically`
`[VK-025-B] Indexing API: every refreshed URL re-indexed within 48h (confirmed in GSC)`
`[VK-025-C] Human review: 100% of AI-generated content reviewed before publication`
`[VK-025-D] Affiliate links: weekly health check active, broken links flagged within 24h`
`[VK-025-E] Ranking alerts: drops >5 positions notified within 24h`

---

## §5 Dependențe

| Tool | Purpose | Cost |
|------|---------|------|
| **Make.com** | Automation platform — scenarios, triggers, integrations | $10-35/mo |
| **Google Sheets** | Master tracker, content calendar | Free |
| **Claude API** or **OpenAI API** | Content refresh generation | $5-20/mo |
| **Omega Indexer** or **IndexMeNow** | Force re-indexing API | $20-30/mo |
| **Ahrefs** (optional API) | Ranking monitoring, traffic verification | $99+/mo |
| **Slack** or **Telegram Bot** | Notifications and alerts | Free |

**DISCLAIMER**: Auto-generated content for gambling (YMYL category) requires mandatory human review before publication. Platforms may penalize auto-published spam content. This procedure describes automation techniques — it does not constitute legal or financial advice.
