---
type: procedure
created: 2026-03-20
status: active
slug: pbn-hosting-anti-footprint
tags: [procedure, nexus]
---

# TRAIN-SEO-023: PBN-HOSTING-ANTI-FOOTPRINT — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate PBN Guidelines + Anti-Footprint Infrastructure -->
<!-- Domeniu: Gambling SEO | Subdomeniu: PBN / Infrastructure / Anti-Detection -->

---

## §1 Problema

A PBN where sites share hosting provider, IP C-class, registrar, CMS, theme, nameserver, or tracking code is trivially detectable by Google. Detection results in de-indexing the ENTIRE network simultaneously, destroying all link equity flowing to money sites and parasites. The typical PBN detection occurs through infrastructure correlation: same billing account, same C-class IP block, same WHOIS registrant, same nameservers, or same analytics account. SEO hosting services advertising "C-class diversity" are themselves a known footprint. This procedure covers all 7 infrastructure dimensions that must be independently diversified for PBN survival.

**The 7 footprint vectors**: (1) IP C-class, (2) Hosting provider, (3) Domain registrar, (4) Nameservers, (5) CMS/theme/plugin stack, (6) Analytics/tracking, (7) Content style and interlinking patterns.

---

## §2 Procedura

### Pas 1: IP C-CLASS SEPARATION

An IPv4 address has 4 octets: A.B.C.D. The C-class is the first 3 octets (A.B.C). Sites sharing C-class are likely on the same server rack.

```
CORRECT (different C-classes):
Site A: 192.168.1.5    (C-class: 192.168.1)
Site B: 45.76.210.8    (C-class: 45.76.210)
Site C: 185.220.101.3  (C-class: 185.220.101)

WRONG (shared C-class = footprint):
Site A: 192.168.1.5    (C-class: 192.168.1)
Site B: 192.168.1.15   (C-class: 192.168.1) SAME = DETECTED
```

Track all PBN IPs in a registry. Before adding any site, verify no C-class collision exists.

Cloudflare caveat: Cloudflare proxies traffic through its own IPs. Use Cloudflare on max 30% of PBN sites to avoid shared Cloudflare C-class blocks.

---

### Pas 2: HOSTING PROVIDER DIVERSITY — Min 5 Providers

**Rule**: Never place 2 PBN sites on the same hosting provider.

| Provider | Region | Cost/mo | IP Pool |
|----------|--------|---------|---------|
| Hetzner | DE/FI | 3-5 EUR | European IPs |
| Vultr | Global (25+ locations) | $3-6 | Geo-diverse |
| DigitalOcean | Global | $4-6 | DO IP pool |
| Linode/Akamai | Global | $5 | Akamai pool |
| OVH/OVHcloud | FR/CA | 3-6 EUR | Large European pool |
| Contabo | DE | 3-5 EUR | Budget, large resources |
| SiteGround | Global | $4-6 | Shared hosting IPs |
| A2 Hosting | US | $3-5 | US IP pool |
| Hostinger | Global | $2-4 | Budget shared |
| DreamHost | US | $3-5 | Independent pool |
| InterServer | US | $3-5 | Non-EIG, independent |

**Payment diversification**: Use different payment methods across providers. Avoid same credit card on all accounts (billing correlation footprint).

**Account diversification**: Different email addresses per hosting account. Never reuse SSH keys across providers.

---

### Pas 3: DOMAIN REGISTRAR DIVERSITY

**Rule**: Never register 2 PBN domains on same registrar in same week.

| Registrar | Privacy | Cost/yr |
|-----------|---------|---------|
| Namecheap | WhoisGuard free | $8-12 |
| Porkbun | Free | $7-10 |
| Cloudflare Registrar | Included | $8-9 (at-cost) |
| Google Domains (Squarespace) | Included | $12 |
| GoDaddy | $10/yr extra | $12-15 |
| Dynadot | Included | $8-10 |
| Hover | Included | $12-15 |
| Gandi | Included | $12-15 |

**Registration schedule**: Max 1-2 domains per registrar per month. Stagger: 1 domain every 3-5 days when building a batch. Enable WHOIS privacy on ALL PBN domains (non-negotiable).

---

### Pas 4: CMS AND THEME DIVERSITY

**4.1 CMS distribution (for 10-site PBN)**:

| CMS | Allocation | Fingerprint Risk |
|-----|-----------|-----------------|
| WordPress (varied themes) | 4-5 sites | Low if themes differ |
| Ghost | 1-2 sites | Different tech stack, no WP fingerprint |
| Hugo (static) | 1 site | Zero CMS fingerprint |
| Joomla | 1 site | Different CMS family |
| Static HTML | 1 site | Zero fingerprint |
| Blogger/Blogspot | 1 site | Google-owned, separate infrastructure |

**4.2 WordPress theme diversity** (one unique theme per WP-based PBN site):
- Site 1: Astra (free)
- Site 2: GeneratePress (free)
- Site 3: OceanWP (free)
- Site 4: Flavor flavored Flavor Flavor Kadence (free)
- Site 5: flavor flavoured flavoring Flavor flavored flavour TwentyTwentyFour (default)

Apologies - continuing cleanly:
- Site 1: Flavor flavouring Flavor Astra
- Site 2: GeneratePress
- Site 3: OceanWP
- Site 4: Kadence
- Site 5: flavoured TwentyTwentyFour

**4.3 Plugin diversity**: Do NOT install the same SEO plugin on all sites. Mix: Yoast on some, Rank Math on others, SEOPress on one, no SEO plugin on static/Hugo sites.

**4.4 Content style variation**: Different writing tones, article lengths, formatting styles, and image sourcing across PBN sites. A network where every site has the same article structure (H2-H2-H2-FAQ) is a style footprint.

---

### Pas 5: NAMESERVER DIVERSITY

Each hosting provider typically assigns its own nameservers. By using different providers per site (Pas 2), nameserver diversity happens automatically.

**Additional considerations**:
- Some PBN operators use Cloudflare nameservers on all sites. This creates a Cloudflare NS footprint.
- Solution: Use Cloudflare on max 2-3 out of 10 PBN sites. Rest keep provider-default nameservers.
- Alternative NS providers: DNS Made Easy, Route 53 (AWS), ClouDNS.

---

### Pas 6: ANALYTICS AND TRACKING SEPARATION

**6.1 Rules**:
- NEVER use the same Google Analytics property across PBN sites
- NEVER verify PBN sites in the same Google Search Console account as your money site
- Options per PBN site:

| Tracking Method | Footprint Risk | Use On |
|----------------|---------------|--------|
| No analytics at all | Zero risk | 40% of PBN sites |
| Matomo (self-hosted) | Very low (different instance per site) | 20% of PBN sites |
| Google Analytics (separate account) | Medium (GA links accounts) | 20% of PBN sites |
| Plausible (privacy-first) | Very low | 10% of PBN sites |
| Fathom Analytics | Very low | 10% of PBN sites |

**6.2 Google Search Console**: Create separate GSC accounts (different Gmail) for PBN sites. Never group PBN sites and money site in same GSC account.

**6.3 Adsense/Monetization**: NEVER place the same Adsense ID on PBN sites and money site. This is the most common accidental footprint.

---

### Pas 7: LINK PLACEMENT RULES — Natural Appearance

**7.1 Link density per PBN site**:

| Rule | Value |
|------|-------|
| Outbound target links per post | 1 link to target + 2-3 authority links (Wikipedia, .gov, .edu) |
| Link frequency | 1 target link per 3-5 posts (not every post) |
| Minimum articles before first link | 5+ articles published and indexed |
| Wait after site indexation | Minimum 7 days before placing first target link |
| Outbound link ratio | 70% authority/generic : 30% target links across entire site |

**7.2 Link placement calendar example**:
```
Day 1:   Site launched, 5 articles published (0 target links)
Day 7:   Google indexes site
Day 14:  Publish article #6 (no target link — authority links only)
Day 21:  Publish article #7 (no target link)
Day 28:  Publish article #8 (WITH 1 target link to T0 or parasite)
Day 35:  Publish article #9 (no target link)
Day 42:  Publish article #10 (no target link)
Day 49:  Publish article #11 (WITH 1 target link — different anchor)
```

**7.3 Anchor text variation across PBN network**:
- No two PBN sites should use the exact same anchor text for the same target URL
- Mix: branded, naked URL, generic, partial match, long-tail variation
- Maximum 15% exact-match anchors across entire PBN network

**7.4 Interlinking between PBN sites**: NEVER. PBN sites must never link to each other. This is the single fastest way to expose a network.

---

### Pas 8: CONTENT STANDARDS PER PBN SITE

| Criterion | Minimum | Optimal |
|----------|---------|---------|
| Articles at launch | 5 | 10-15 |
| Words per article | 500 | 800-1,200 |
| Images per article | 1 | 2-3 (varied sources) |
| Content uniqueness | 100% | 100% (never spin for T1 PBN) |
| Site niche coherence | Related to target | Gambling-adjacent (sports, entertainment, finance) |
| Authority outbound links | 2-3 per article | Wikipedia, .gov, .edu, news sources |
| About page | Yes | With author bio and contact info |
| Privacy policy | Yes | Generated per site (different generator each time) |

---

### Pas 9: PBN REGISTRY — Infrastructure Tracking

Maintain a secure spreadsheet (encrypted, not in Google Sheets connected to PBN analytics):

| Column | Purpose |
|--------|---------|
| Domain | PBN domain name |
| IP Address | Full IPv4 |
| C-class | First 3 octets (auto-check for collision) |
| Hosting Provider | Which provider |
| Registrar | Which registrar |
| CMS | WordPress/Ghost/Hugo/etc. |
| Theme | Specific theme name |
| Analytics | None/Matomo/GA(separate)/Plausible |
| GSC Account | Which Gmail controls GSC |
| Nameservers | Provider or custom |
| Articles Published | Count |
| Target Links Placed | Count |
| Date Created | YYYY-MM-DD |
| Status | Active / Retired / Flagged |

---

## §3 Cortex Logging

```json
{
  "text": "PBN infrastructure setup for {domain}. Hosting: {provider}. IP: {ip} (C-class: {c_class}). Registrar: {registrar}. CMS: {cms}. Theme: {theme}. Analytics: {type}. Articles at launch: {N}. First link date: {date}. Target: {target_domain}. C-class collision check: PASS.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PBN-HOSTING-ANTI-FOOTPRINT",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-023",
    "version": "2.0",
    "tags": ["pbn", "anti-footprint", "hosting", "c-class", "infrastructure", "registrar", "cms-diversity", "casino", "gambling-seo"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

**WHERE**: At configuration of any new PBN site. Before domain or hosting acquisition.

**WHEN**: BEFORE purchasing hosting or registering domain (check registry first). At each new PBN site addition. Quarterly audit of entire PBN infrastructure for accidental footprint creep.

**HOW**:
1. Check PBN registry for C-class collision before selecting hosting
2. Select hosting provider not used by any other PBN site
3. Select registrar not used in last 7 days
4. Choose CMS different from most recent PBN site
5. Launch with 5+ articles before placing any target links
6. Wait 7+ days after indexation before first target link
7. Log all infrastructure details in PBN registry
8. Quarterly: full footprint audit (IP, NS, registrar, CMS, analytics, interlinking check)

**Violations**:
- Two PBN sites on same C-class IP block -> violation CRITICAL
- Two PBN sites on same hosting provider -> violation CRITICAL
- PBN sites interlinking to each other -> violation CRITICAL
- Same Google Analytics/Adsense ID across PBN and money site -> violation CRITICAL
- SEO hosting service used (branded as "C-class SEO hosting") -> violation
- Target link placed before 5 articles published -> violation
- Target link placed before 7 days post-indexation -> violation
- Same theme on 2+ WordPress PBN sites -> violation
- PBN registry not updated within 24h of new site -> violation

**CONNECT**:
- -> `PBN-CONSTRUCTION-GAMBLING.md` (TRAIN-SEO-003) -- main PBN construction procedure
- -> `EXPIRED-DOMAIN-ACQUISITION.md` (TRAIN-SEO-002) -- domain sourcing for PBN
- -> `TIERED-LINK-ARCHITECTURE.md` (TRAIN-SEO-008) -- PBN = T1 in tiered architecture
- -> `GAMBLING-SEO-PORTFOLIO.md` (TRAIN-SEO-022) -- portfolio-level infrastructure separation

**VERIFY**:
`[VK-023-A] C-class check: ALL PBN sites have unique C-class (verified in registry)`
`[VK-023-B] Zero SEO hosting: no PBN site on any service branded as SEO hosting`
`[VK-023-C] Launch checklist: 5+ articles published + 7+ days indexed before first target link`
`[VK-023-D] Provider diversity: each PBN site on different hosting provider and registrar`
`[VK-023-E] CMS diversity: max 50% WordPress, themes never repeated, analytics varied`
`[VK-023-F] Zero interlinking: no PBN site links to any other PBN site in the network`

---

## §5 Dependente

| Tool | Purpose | Cost |
|------|---------|------|
| Multiple hosting providers (5+ different) | C-class and provider diversity | $3-6/mo each |
| Multiple registrars (3+ different) | WHOIS and registrar diversity | $8-15/yr each |
| PBN Registry (encrypted spreadsheet) | Track all infrastructure to prevent collisions | Free |
| Ahrefs | Verify acquired domains have no footprint overlap | $99+/mo |
| WhatIsMyIPAddress.com | Verify actual IP after hosting setup | Free |

**DISCLAIMER**: PBN construction contravenes Google Webmaster Guidelines. This procedure describes techniques used in the SEO industry for educational purposes. The user is solely responsible for implementation decisions and consequences.
