---
type: procedure
created: 2026-03-22
status: active
slug: pbn-construction-gambling
tags: [procedure, nexus]
---

# TRAIN-SEO-003: PBN-CONSTRUCTION-GAMBLING — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Charles Floate PBN Guidelines + Anti-Footprint Construction -->
<!-- Domeniu: Gambling SEO | Subdomeniu: PBN / Network Construction / Grey-Hat Link Building -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-003
**Scope**: End-to-end construction and management of a Private Blog Network for gambling/casino money sites, covering domain acquisition, hosting setup, site configuration, content strategy, anti-footprint protocol, and link deployment.
**Training Context**: This procedure is part of the Gambling SEO grey-hat curriculum. PBN construction is a grey-hat technique documented here for educational and operational training purposes. All execution must comply with applicable local laws and platform ToS.

---

## 1. Problema

Gambling and casino niches are among the most competitive in SEO. Editorial link acquisition is nearly impossible — licensed operators reject outreach, and authoritative publishers avoid casino content. PBNs remain the primary lever for injecting controlled link equity into money sites in this vertical.

The core challenge is not building the PBN — it is building it without a detectable footprint. Google's detection mechanism combines ML pattern recognition with a 12,000+ person manual review team. Any shared signal across PBN sites (same host, same registrar, same theme, same analytics) collapses the entire network simultaneously.

This procedure covers:
- Domain acquisition with quality gatekeeping
- Hosting setup that prevents IP-level footprinting
- WordPress configuration with full site isolation
- Content strategy that mimics real editorial sites
- Link deployment patterns that avoid anchor and structural footprints
- Ongoing network maintenance and isolation protocols

---

## 2. Procedura

### Pas 1: Domain Acquisition

**Source channels:**
- Expired domain auctions: GoDaddy Auctions, NameJet, Flippa, Domain Hunter Gatherer
- Dropped domains: Spamzilla (bulk filter), Moonsy
- Manual Majestic/Ahrefs scraping of expired domains in gambling-adjacent niches

**Quality gatekeeping — EVERY domain must pass ALL filters:**

- [ ] TF (Trust Flow, Majestic) ≥ 15
- [ ] CF (Citation Flow, Majestic) ≥ 15
- [ ] DA (Domain Authority, MOZ) ≥ 20
- [ ] Topically relevant to gambling OR general authority (news, tech, lifestyle)
- [ ] Clean anchor text history — no symbols, no foreign spam, no adult/pharma/casino spam
- [ ] Real backlink profile — NOT bulk directory links, NOT link farm links
- [ ] Archive.org history — real website existed, not a parked page
- [ ] NOT in bad IP neighborhood (check: ViewDNS.info, isbadip.com)
- [ ] No manual action history (check: Google Search Console if you can verify)
- [ ] No Wayback Machine evidence of prior PBN use (all pages 301'd to homepage = red flag)

**Reject immediately if:**
- [ ] Previously used as a PBN (thin content, all pages redirected, only homepage links)
- [ ] All backlinks from same IP block
- [ ] Anchor text is 80%+ exact match / money keywords
- [ ] Domain registered and immediately expired (no real use)
- [ ] Any prior association with gambling spam

---

### Pas 2: Hosting Setup — Anti-Footprint Protocol

**Core rule**: Each PBN site must have a DIFFERENT IP address from a DIFFERENT IP range (Class C or higher isolation). Use shared hosting ONLY — your own dedicated server means all sites share your IP.

**Approved hosting providers** (rotate across, never double up):
- BigScoots (shared plans)
- SiteGround (shared plans)
- A2 Hosting (shared plans)
- DreamHost (shared plans)
- HostGator (shared plans)
- Bluehost, InMotion, Namecheap hosting (as rotation options)

**Hosting checklist — per site:**
- [ ] Different hosting company than any other PBN site (strict rotation)
- [ ] Verified: IP is in different Class C range than all other PBN sites (check: whatismyip, ipinfo.io)
- [ ] Purchased with separate payment method or privacy card
- [ ] Account registered with unique email address (not shared across PBN sites)
- [ ] WHOIS privacy enabled on every domain
- [ ] Domain registered via different registrar than other PBN domains (rotate: Namecheap, GoDaddy, Porkbun, Dynadot, Google Domains, Name.com)
- [ ] Domains NOT registered on the same day
- [ ] Custom nameservers NOT used (use default hosting nameservers)
- [ ] Cloudflare NOT enabled on all sites — if used, limit to ≤ 20% of network
- [ ] NO common email across domains
- [ ] NO common WHOIS contact across domains

---

### Pas 3: WordPress Installation and Site Setup

**Base setup checklist — per site:**
- [ ] Install WordPress (latest version)
- [ ] Select a UNIQUE theme — no two PBN sites share the same theme
  - Paid/premium themes: GeneratePress, Astra, Kadence (but different licenses per site)
  - Free themes from .org: vary selection, never repeat
  - Custom child themes where possible for uniqueness
- [ ] Plugin selection: VARY across sites — no two sites should have identical plugin stacks
  - Core plugins acceptable network-wide: Yoast/RankMath (ONE or the OTHER, not mix), Wordfence
  - Content/editor: alternate between Gutenberg, Classic Editor, Elementor (not all the same)
  - Contact forms, caching, image optimization: rotate providers
- [ ] Create static pages:
  - [ ] About page (unique content, fake or real persona)
  - [ ] Contact page (form or unique email, NOT linked to any real identity)
  - [ ] Privacy Policy (generate with unique policy generator — not the same boilerplate)
  - [ ] Disclaimer page
- [ ] Set unique author/user profile — different name, bio, avatar on each site
- [ ] Google Analytics: DO NOT install same GA property on multiple sites (= immediate footprint)
  - Option A: No analytics at all
  - Option B: Separate GA4 property per site, different Google account per 3-4 sites
- [ ] NO shared affiliate codes across sites
- [ ] robots.txt: DO NOT use identical robots.txt across sites. Use .htaccess for blocking crawlers where needed instead
- [ ] SSL certificate installed (Let's Encrypt via hosting panel)

**Site type classification** — decide per domain before setup:

| Type | Use | Link Placement |
|------|-----|----------------|
| Homepage linker | High-authority root domain | Link from homepage blogroll or featured post |
| Inner page linker | Strong topical domain | Link from specific inner page (category or post) |
| Contextual linker | General authority | Link embedded in body of contextual article |

---

### Pas 4: Content Strategy

**Minimum content requirements:**
- [ ] Minimum 2,000 words of total content before first PBN link goes live
- [ ] At least 3 published articles before linking to money site
- [ ] Static pages (About/Contact/Privacy/Disclaimer) all populated with unique content

**Content types to use** (rotate to vary appearance):
- [ ] Long-form editorial (1,500-2,500 words) — primary
- [ ] News curation + original commentary (500-800 words)
- [ ] Infographic post with 200-400 words of descriptive text
- [ ] Video embed + transcript (300-500 words)
- [ ] Roundup/listicle (700-1,000 words)
- [ ] Image gallery post with captions (200-400 words)

**Content length variation — MANDATORY:**
- [ ] Articles vary in length across: 200w / 500w / 700w / 1,000w / 1,500w / 2,000w+
- [ ] NO uniform length across all posts on a single site
- [ ] NO uniform length across all PBN sites

**Publishing cadence:**
- [ ] Standard sites: ~1 article per month
- [ ] "Magazine" persona sites: ~2 articles per month
- [ ] Posts NOT published on the same day across multiple PBN sites
- [ ] Stagger publication dates: randomize day and time of publication

**Outbound link (OBL) strategy — CRITICAL for footprint avoidance:**
- [ ] EVERY article links to at least 1-2 authority sites (Wikipedia, CNN, BBC, Reuters, ESPN, industry news sites)
- [ ] Articles include some links to non-authority but relevant sites (OBL padding)
- [ ] NOT all articles link only to money site
- [ ] Money site link present in maximum 1 of every 3-4 published articles
- [ ] Internal linking structure exists (articles link to each other naturally)
- [ ] NO cross-linking between different PBN sites

**Content quality:**
- [ ] NOT spun content
- [ ] NOT duplicate content from other PBN sites
- [ ] NOT word-for-word scraped (paraphrase + add original commentary at minimum)
- [ ] Different images on every site — no shared media library
- [ ] Images renamed with unique filenames before upload (not IMG_001.jpg)
- [ ] IF content references gambling/casino: include responsible gambling disclaimer and 18+ notice in article footer

---

### Pas 5: Link Deployment to Money Site

**Anchor text strategy:**

| Anchor Type | Recommended % |
|-------------|---------------|
| Branded (casino name, URL) | 40-50% |
| Generic (click here, visit, learn more, this site) | 20-30% |
| Partial match (online casino Peru, best slots) | 10-15% |
| Exact match (casino en línea Peru) | 5-10% MAX |
| Naked URL (https://...) | 5-10% |

**Link placement checklist — per link:**
- [ ] Link placed naturally within body content (contextual)
- [ ] NOT always in the first paragraph of the article
- [ ] NOT always in the same position (vary: intro, middle, conclusion)
- [ ] Surrounding text is topically relevant to the money site page being linked
- [ ] Anchor text matches the content context naturally
- [ ] 1 money site link per PBN post maximum (avoid 2+ money links per article)
- [ ] Do-follow link (verify with browser inspector or Screaming Frog)

**Network-level link discipline:**
- [ ] NEVER link between PBN sites (biggest footprint signal)
- [ ] NEVER link from same IP range to same money site
- [ ] Vary number of PBN links deployed per money site per month (not always 5, not always 10)
- [ ] Money site receives links from different PBN sites each month (rotate)
- [ ] NOT all PBN links pointing to same money site page — diversify to inner pages, categories, and homepage

**Money site distribution:**
- [ ] General authority PBN cluster: distribute links across multiple money sites
- [ ] Niche-specific PBN cluster: focus on one money site (topical trust flow)
- [ ] Isolate clusters: if one cluster gets flagged, others remain intact

---

### Pas 6: Network Architecture and Isolation

**Cluster design:**
- Small niche target (e.g., Peru casino): 8-10 PBN sites with TF 15+ = sufficient
- 1 domain TF 35+ = equivalent link power of 5 smaller TF 15 domains
- Recommended: Mix of niche-specific + general authority clusters
- Cluster size per money site: max 15-20 PBN sites to reduce blast radius if detected

**Isolation rules:**
- [ ] PBN sites for different money sites hosted on different accounts
- [ ] If one PBN site gets flagged → immediately stop linking from it, do NOT disavow (draws attention)
- [ ] No money site IP/host used for PBN hosting
- [ ] No shared credentials between PBN management accounts and money site accounts
- [ ] PBN site administration access via VPN or separate IP — never your home/office IP
- [ ] PBN login management via separate browser profile per cluster (no cookie sharing)

**Detection risk model:**

| Google Detection Path | Defense |
|-----------------------|---------|
| ML algo flags suspicious pattern | Zero common footprint across all signals |
| Manual review team inspects on-page | Real content, static pages, author info, OBL to authority sites |
| Algo traverses backlink network | NEVER cross-link PBN sites; OBL padding breaks traversal |
| IP block investigation | Different hosting providers, different IP ranges |

---

### Pas 7: Ongoing Maintenance

**Monthly checklist:**
- [ ] Publish 1 new article on each active PBN site
- [ ] Check all PBN sites are live (uptime monitor: UptimeRobot — separate free account)
- [ ] Verify money site is still indexing new pages receiving links
- [ ] Check Ahrefs/Semrush: are PBN links showing as active?
- [ ] Monitor money site rankings correlated with new PBN links
- [ ] Check Google Search Console on money site for any manual action notices
- [ ] Rotate which PBN sites deliver links this month (don't always use the same 5)

**Annual checklist:**
- [ ] Renew domain registrations (check WHOIS expiry for all PBN domains)
- [ ] Review content freshness — update any 404s, dead external links
- [ ] Audit for newly introduced footprints (plugin updates may introduce version meta tags)
- [ ] Evaluate network health: TF/CF changes? Domain deindexed?
- [ ] Sunset any PBN domain that has been deindexed — stop linking from it immediately

**Deindex detection:**
- Check: `site:pbndomain.com` in Google — if no results, domain is deindexed
- [ ] Deindexed PBN domain → stop all links from it immediately
- [ ] Do NOT delete the site (a sudden 404 from a linking domain raises flags)
- [ ] Leave the site up, stop adding content, stop linking — let it die silently

---

## 3. Cortex Logging

```json
{
  "text": "PBN Construction executed for gambling money site [domain]. Domains acquired: [n]. Hosting providers used: [list]. Cluster type: [niche/general/mixed]. PBN links deployed: [n]. Anchor distribution: branded [x]% / generic [x]% / partial [x]% / exact [x]%.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PBN-CONSTRUCTION-GAMBLING",
    "domain": "Gambling SEO",
    "rule_id": "TRAIN-SEO-003",
    "tags": ["pbn", "link-building", "gambling-seo", "casino-seo", "anti-footprint", "grey-hat"],
    "has_enforcement_loop": true,
    "forge_version": "1.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
Executat în context de research activ (Leo gambling SEO project) + Training Mode SEO curriculum

### WHEN
La fiecare execuție reală a procedurii: achiziție domeniu nou, setup site PBN nou, sau deployment de link spre money site

### HOW (violation detection)
- Două PBN site-uri cu același hosting provider → FOOTPRINT VIOLATION
- Conținut duplicat sau spun detectat pe oricare site din rețea → CONTENT FOOTPRINT VIOLATION
- Cross-link între site-uri PBN → NETWORK FOOTPRINT VIOLATION (cel mai grav)
- Același Google Analytics pe 2+ site-uri → IMMEDIATE DEINDEX RISK
- Anchor text exact match > 10% din total linkuri → OVER-OPTIMIZATION VIOLATION
- Toate articolele leagă doar spre money site (fără OBL diversificare) → PATTERN FOOTPRINT
- Runner: Opus audit lunar al rețelei + AUDIT-PRO batch

**Acțiuni interzise:**
- Cross-linking între site-uri PBN (cel mai grav footprint signal — deindex instant)
- Folosirea aceluiași hosting provider pentru 2+ site-uri PBN
- Instalarea aceluiași Google Analytics property pe 2+ site-uri PBN
- Publicarea de conținut duplicat sau spun pe oricare site din rețea
- Lansarea link building fără minimum 3 articole publicate pe PBN site
- Administrarea PBN site-urilor de pe IP personal fără VPN

### CONNECT
- TRAIN-SEO-003 → enforcement FORGE standard pentru gambling SEO
- `memory/research/seo-2026-full-spectrum.md` → research base pentru procedură
- `memory/tasks/leo-tasks.md` → Leo's gambling SEO project Peru/Kenya/Chile
- Training Mode → procedura face parte din curriculum SEO grey-hat

### VERIFY
La finalul fiecărei execuții, verifică:
- [ ] Toate domeniile achiziționate au trecut TOATE filtrele din Pas 1?
- [ ] Fiecare site PBN are IP în range diferit (Class C isolation verificat)?
- [ ] Nicio pereche de site-uri PBN nu împarte: hosting / registrar / theme / analytics / autor?
- [ ] Conținutul de pe fiecare site este unic (nu spun, nu duplicat)?
- [ ] OBL include linkuri spre authority sites pe fiecare articol?
- [ ] Cross-linking între PBN sites = ZERO?
- [ ] Anchor text distribution respectă tabelul din Pas 5?
- [ ] Static pages (About/Contact/Privacy/Disclaimer) populate pe fiecare site?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PBN-CONSTRUCTION-GAMBLING | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "PBN Construction for Gambling/Casino SEO" | FORGE ✓ | rule: TRAIN-SEO-003 | v1.0`

### MODEL ROUTING

| Activitate | Model |
|-----------|-------|
| Execuție procedură (research/planning) | Sonnet (Genie) |
| Audit rețea PBN existentă | Opus |
| Validare structură FORGE | AUDIT-PRO |
| Clasificare domenii achiziționate | Haiku |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Majestic (TF/CF metrics) | Domain quality gatekeeping — Trust Flow + Citation Flow |
| MOZ (DA metric) | Domain Authority verification |
| Ahrefs / Semrush | Backlink profile audit pe domeniile achiziționate + link monitoring post-deployment |
| Spamzilla / Domain Hunter Gatherer | Expired domain discovery și bulk filtering |
| GoDaddy Auctions / NameJet / Flippa | Domain auction platforms |
| Archive.org (Wayback Machine) | Domain history verification — real site existed? |
| ViewDNS.info / ipinfo.io | IP neighborhood check per domeniu |
| BigScoots / SiteGround / A2 / DreamHost / HostGator | Shared hosting providers (rotați) |
| WordPress.org | CMS obligatoriu pentru toate PBN site-urile |
| UptimeRobot | Network uptime monitoring (conturi separate) |
| VPN | Acces admin la PBN sites (nu din IP personal) |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate checklist Pas 1 | 100% — zero domenii acceptate fără toți filtrele |
| IP isolation rate | 100% — zero site-uri PBN pe același Class C range |
| Shared hosting provider rate | 0% repetiții — fiecare site pe provider diferit |
| Cross-links between PBN sites | ZERO |
| Exact match anchor text % | ≤ 10% din totalul linkurilor spre money site |
| OBL authority links per article | Minim 1-2 per articol |
| Content duplication rate | 0% — verificat cu Copyscape sau manual |
| Monthly content cadence | 1 articol/lună/site (2 pentru magazine-style) |
| Deindex detection response time | ≤ 24h de la detectare → oprire linkuri imediat |
| Network blast radius (max sites per cluster) | ≤ 20 site-uri per money site |
