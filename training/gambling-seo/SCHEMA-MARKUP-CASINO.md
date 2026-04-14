---
type: procedure
created: 2026-03-20
status: active
slug: schema-markup-casino
tags: [procedure, nexus]
---

# TRAIN-SEO-017: SCHEMA-MARKUP-CASINO — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Koray Tuğberk GÜBÜR Semantic SEO + Google Structured Data Documentation -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Technical SEO / Structured Data -->

---

## §1 Problema

Casino affiliate sites without structured data markup lose SERP real estate to competitors who display rich snippets (star ratings, FAQ dropdowns, breadcrumb trails, author info). In the Peru/Kenya/Chile markets where top 10 competitors rarely implement schema beyond basic Organization markup, proper structured data creates immediate visual differentiation in search results — increasing CTR by 15-30% without any additional link building. Casino content falls under YMYL (Your Money Your Life) where Google scrutinizes E-E-A-T signals heavily; structured data provides machine-readable proof of expertise, authorship, and content type that satisfies Google's quality rater guidelines programmatically.

**Casino-specific schema challenges**: Review schema requires genuine review methodology (not fake star ratings), FAQ schema must contain actually useful answers (Google filters thin FAQ markup since 2023 HCU update), gambling content requires explicit disclaimers and responsible gambling signals that structured data can encode.

---

## §2 Procedura

### Pas 1: SCHEMA AUDIT — Current State Assessment

**1.1 — Test existing markup**:
- Google Rich Results Test: `search.google.com/test/rich-results` → test each page type
- Schema Markup Validator: `validator.schema.org` → validate JSON-LD syntax
- Screaming Frog: crawl site → Structured Data tab → identify pages with/without schema

**1.2 — Competitor schema audit**:
- For top 5 competitors in target market: view page source → search for `application/ld+json`
- Document: which schema types they use, which SERP features they win
- Identify gaps: schema types competitors DON'T use = your opportunity

**1.3 — Page type → Schema type mapping**:

| Page Type | Required Schema | Optional Schema | SERP Feature |
|-----------|----------------|-----------------|--------------|
| Casino review page | Review, AggregateRating | Product, Offer | Star rating snippet |
| Comparison / "Top 10" page | ItemList, ListItem | Review per item | Carousel, list snippet |
| FAQ / Guide page | FAQPage | HowTo, Article | FAQ dropdown |
| Bonus page | Offer, AggregateOffer | Product | Price/offer snippet |
| About / Author page | Person, Organization | ProfilePage | Knowledge panel |
| Homepage | Organization, WebSite | SearchAction | Sitelinks searchbox |
| Blog / News article | Article, NewsArticle | Speakable | Article snippet |
| Payment method guide | HowTo | FAQPage | Step-by-step snippet |

---

### Pas 2: IMPLEMENT REVIEW SCHEMA — Casino Review Pages

**2.1 — Single Casino Review Page** (`/casinos/betsson-chile/`):

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Review",
  "itemReviewed": {
    "@type": "Organization",
    "name": "Betsson Chile",
    "url": "https://www.betsson.cl",
    "image": "https://yoursite.com/images/betsson-chile-logo.webp",
    "description": "Casino online licenciado en Chile bajo Ley 21.456"
  },
  "author": {
    "@type": "Person",
    "name": "Juan Pérez",
    "url": "https://yoursite.com/autores/juan-perez/",
    "jobTitle": "Casino Industry Analyst",
    "sameAs": ["https://linkedin.com/in/juanperez"]
  },
  "reviewRating": {
    "@type": "Rating",
    "ratingValue": "4.5",
    "bestRating": "5",
    "worstRating": "1"
  },
  "datePublished": "2026-01-15",
  "dateModified": "2026-03-01",
  "publisher": {
    "@type": "Organization",
    "name": "YourBrand",
    "logo": {
      "@type": "ImageObject",
      "url": "https://yoursite.com/logo.webp"
    }
  },
  "reviewBody": "Betsson Chile ofrece una experiencia de casino online regulada con licencia SCJ, aceptando WebPay y transferencias bancarias en CLP."
}
</script>
```

**2.2 — Comparison Page with ItemList** (`/mejores-casinos-online-chile/`):

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "ItemList",
  "name": "Mejores Casinos Online Chile 2026",
  "description": "Ranking actualizado de los mejores casinos online con licencia en Chile",
  "numberOfItems": 5,
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "item": {
        "@type": "Review",
        "name": "Betsson Chile Review",
        "url": "https://yoursite.com/casinos/betsson-chile/",
        "reviewRating": {"@type": "Rating", "ratingValue": "4.7", "bestRating": "5"},
        "author": {"@type": "Person", "name": "Juan Pérez"}
      }
    },
    {
      "@type": "ListItem",
      "position": 2,
      "item": {
        "@type": "Review",
        "name": "Codere Chile Review",
        "url": "https://yoursite.com/casinos/codere-chile/",
        "reviewRating": {"@type": "Rating", "ratingValue": "4.3", "bestRating": "5"},
        "author": {"@type": "Person", "name": "Juan Pérez"}
      }
    }
  ]
}
</script>
```

**Rating methodology requirement**: Google's Review snippet policy requires genuine editorial assessment. Document your rating criteria (bonus value, payment options, customer support, game variety, license status) and reference it in the review. Fake or inflated ratings trigger manual actions.

---

### Pas 3: IMPLEMENT FAQ SCHEMA — Guide & Informational Pages

**3.1 — FAQPage Schema** (`/guias/casino-legal-chile/`):

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "¿Es legal jugar en casino online en Chile?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Sí, desde la entrada en vigor de la Ley 21.456 en 2022, los casinos online con licencia de la Superintendencia de Casinos de Juego (SCJ) operan legalmente en Chile. Solo juega en sitios con licencia verificable en scj.gob.cl."
      }
    },
    {
      "@type": "Question",
      "name": "¿Cómo depositar con WebPay en un casino online?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Ingresa al cajero del casino, selecciona WebPay como método de pago, ingresa el monto en CLP, confirma con tu banco a través de la pasarela WebPay. El depósito se acredita al instante. Monto mínimo: generalmente CLP 5.000."
      }
    },
    {
      "@type": "Question",
      "name": "Is online gambling legal in Kenya?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes, online gambling is legal in Kenya and regulated by the Betting Control and Licensing Board (BCLB). Only bet on sites that hold a valid BCLB license. You can verify licenses at bclb.go.ke."
      }
    },
    {
      "@type": "Question",
      "name": "How do I deposit with M-Pesa at an online casino?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Go to the casino's deposit page, select M-Pesa, enter the amount in KES (minimum usually KES 50-100), and confirm via your Safaricom M-Pesa PIN. Deposits are instant. You can also withdraw winnings to M-Pesa."
      }
    }
  ]
}
</script>
```

**FAQ schema rules post-2023 HCU**:
- Each answer MUST provide genuine value (not 1-sentence filler)
- Limit to 5-8 questions per page (Google may ignore if >10)
- Questions must match real user queries (check "People Also Ask" in SERPs)
- Do NOT use FAQ schema on pages where FAQs are not the primary content purpose

---

### Pas 4: IMPLEMENT HOWTO SCHEMA — Payment & Tutorial Pages

**4.1 — HowTo Schema for M-Pesa Deposit Tutorial** (`/how-to-deposit-mpesa-casino/`):

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to Deposit at an Online Casino Using M-Pesa in Kenya",
  "description": "Step-by-step guide to depositing KES at a BCLB-licensed online casino using Safaricom M-Pesa",
  "totalTime": "PT3M",
  "estimatedCost": {"@type": "MonetaryAmount", "currency": "KES", "value": "50"},
  "step": [
    {"@type": "HowToStep", "position": 1, "name": "Register at Casino", "text": "Create an account at a BCLB-licensed casino (Betway Kenya, 1xBet, etc.). Verify your phone number."},
    {"@type": "HowToStep", "position": 2, "name": "Navigate to Deposit", "text": "Log in and go to the Cashier or Deposit section of the casino."},
    {"@type": "HowToStep", "position": 3, "name": "Select M-Pesa", "text": "Choose M-Pesa as your payment method from the available options."},
    {"@type": "HowToStep", "position": 4, "name": "Enter Amount", "text": "Enter the deposit amount in KES. Minimum is usually KES 50-100 depending on the casino."},
    {"@type": "HowToStep", "position": 5, "name": "Confirm via M-Pesa PIN", "text": "You will receive an M-Pesa push notification on your phone. Enter your M-Pesa PIN to confirm the transaction."},
    {"@type": "HowToStep", "position": 6, "name": "Funds Available", "text": "Your casino balance updates instantly. You can now play slots, table games, or place sports bets."}
  ],
  "tool": [
    {"@type": "HowToTool", "name": "Safaricom M-Pesa registered phone"},
    {"@type": "HowToTool", "name": "BCLB-licensed casino account"}
  ]
}
</script>
```

---

### Pas 5: IMPLEMENT ORGANIZATION & AUTHOR SCHEMA — E-E-A-T Signals

**5.1 — Organization Schema** (homepage, sitewide via header):

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "YourBrand Casino Reviews",
  "url": "https://yoursite.com",
  "logo": "https://yoursite.com/logo.webp",
  "description": "Independent casino reviews and guides for players in Peru, Kenya, and Chile",
  "foundingDate": "2025",
  "sameAs": [
    "https://twitter.com/yourbrand",
    "https://linkedin.com/company/yourbrand"
  ],
  "contactPoint": {
    "@type": "ContactPoint",
    "email": "info@yourbrand.com",
    "contactType": "editorial"
  }
}
</script>
```

**5.2 — Person Schema for Authors** (`/autores/juan-perez/`):

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "Juan Pérez",
  "jobTitle": "Casino Industry Analyst",
  "description": "8 años de experiencia en análisis de la industria de juegos de azar en Chile y Latinoamérica",
  "url": "https://yoursite.com/autores/juan-perez/",
  "image": "https://yoursite.com/images/juan-perez.webp",
  "sameAs": ["https://linkedin.com/in/juanperez", "https://twitter.com/juanperez"],
  "worksFor": {"@type": "Organization", "name": "YourBrand Casino Reviews"}
}
</script>
```

**E-E-A-T schema impact for YMYL gambling content**: Google's quality raters check for author credentials, editorial oversight, and organizational transparency. Schema makes these signals machine-readable, giving algorithmic advantage in YMYL categories where manual review occurs frequently.

---

### Pas 6: IMPLEMENT BREADCRUMB SCHEMA — Site-Wide Navigation Signal

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {"@type": "ListItem", "position": 1, "name": "Home", "item": "https://yoursite.com/"},
    {"@type": "ListItem", "position": 2, "name": "Casinos Chile", "item": "https://yoursite.com/casinos/"},
    {"@type": "ListItem", "position": 3, "name": "Betsson Chile Review", "item": "https://yoursite.com/casinos/betsson-chile/"}
  ]
}
</script>
```

Breadcrumb schema appears in SERP as clickable navigation path — improves CTR and communicates site architecture to Google.

---

### Pas 7: IMPLEMENT SITELINKS SEARCHBOX — Homepage Authority Signal

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebSite",
  "name": "YourBrand Casino Reviews",
  "url": "https://yoursite.com",
  "potentialAction": {
    "@type": "SearchAction",
    "target": "https://yoursite.com/search?q={search_term_string}",
    "query-input": "required name=search_term_string"
  }
}
</script>
```

---

### Pas 8: IMPLEMENT RESPONSIBLE GAMBLING SCHEMA — Compliance & Trust Signal

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "WebPage",
  "speakable": {
    "@type": "SpeakableSpecification",
    "cssSelector": [".responsible-gambling-notice"]
  },
  "about": {
    "@type": "Thing",
    "name": "Responsible Gambling",
    "description": "Information about responsible gambling practices, self-exclusion, and support resources"
  },
  "mentions": [
    {"@type": "Organization", "name": "Superintendencia de Casinos de Juego", "url": "https://scj.gob.cl"},
    {"@type": "Organization", "name": "Betting Control and Licensing Board", "url": "https://bclb.go.ke"},
    {"@type": "Organization", "name": "MINCETUR", "url": "https://www.mincetur.gob.pe"}
  ]
}
</script>
```

---

### Pas 9: VALIDATION & MONITORING

**9.1 — Pre-launch validation**:
- Test EVERY page type in Google Rich Results Test — zero errors required
- Validate JSON-LD syntax at validator.schema.org
- Cross-check: visible content must match schema data (ratings, names, dates)

**9.2 — Post-launch monitoring**:
- GSC → Enhancements → check each schema type for errors weekly
- Monitor rich snippet appearance: search `site:yoursite.com` in incognito
- Track CTR changes in GSC after schema deployment (expect +15-30% for FAQ/Review snippets)

**9.3 — Schema maintenance cadence**:
- Update `dateModified` on Review schema when content is refreshed
- Update ratings when casino terms change (bonus modifications, license status)
- Add new FAQ questions based on emerging "People Also Ask" queries
- Remove schema from pages where content no longer matches (closed casino, expired bonus)

---

## §3 Cortex Logging

```json
{
  "text": "Schema markup implemented for {domain}. Types: {Review|FAQ|HowTo|Organization|Breadcrumb|etc}. Pages marked up: {N}/{total}. Rich Results Test: {N} pass, {N} errors. CTR impact: {before}→{after}%. SERP features won: {list}.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SCHEMA-MARKUP-CASINO",
    "domain": "gambling_seo",
    "rule_id": "TRAIN-SEO-017",
    "version": "2.0",
    "tags": ["schema", "structured-data", "json-ld", "review", "faq", "rich-snippets", "eeat", "casino", "technical-seo"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
  }
}
```

---

## §4 Enforcement Loop

**WHERE**: Every casino money site. After each new page type is created. At launch and during quarterly technical audits.

**WHEN**: At site launch (full implementation), when new page templates are created, monthly validation check, after Google algorithm updates affecting structured data.

**HOW**:
1. Audit current schema state (Pas 1)
2. Implement per page type (Pas 2-8)
3. Validate all implementations (Pas 9)
4. Monitor GSC Enhancements weekly
5. Track CTR impact monthly

**Violations**:
- Casino review page without Review schema → violation
- FAQ page without FAQPage schema → violation
- Review ratings without documented methodology → violation CRITICAL (manual action risk)
- Schema errors in GSC Enhancements ignored >7 days → violation
- Author pages without Person schema (YMYL E-E-A-T requirement) → violation

**CONNECT**:
- → `CORE-WEB-VITALS-CASINO.md` (TRAIN-SEO-018) — technical SEO companion
- → `SEMANTIC-TOPICAL-AUTHORITY-CASINO.md` (TRAIN-SEO-016) — content architecture feeds schema structure
- → `CASINO-MARKET-ANALYSIS.md` (TRAIN-SEO-001) — competitor schema audit in market analysis
- → `PARASITE-PAGE-BUILD.md` (TRAIN-SEO-004) — parasite pages support limited schema (FAQPage on Medium)

**VERIFY**:
`[VK-017-A] Rich Results Test: ALL page types pass with zero errors`
`[VK-017-B] Review schema: methodology documented, ratings genuine and defensible`
`[VK-017-C] FAQ schema: 5-8 questions per page, answers substantive (>50 words each)`
`[VK-017-D] Organization + Person schema: E-E-A-T signals machine-readable on all review content`
`[VK-017-E] Breadcrumb schema: site-wide, matching actual navigation hierarchy`

---

## §5 Dependențe

| Tool | Purpose | Cost |
|------|---------|------|
| **Google Rich Results Test** | Validate schema markup per page | Free |
| **Schema.org Validator** | Syntax validation for JSON-LD | Free |
| **Google Search Console** | Monitor Enhancements, track schema errors | Free |
| **Screaming Frog** | Bulk schema audit across all pages | $259/yr (free for <500 URLs) |
| **Schema markup generator** | Template generation (technicalseo.com/tools/schema-markup-generator/) | Free |

**DISCLAIMER**: This procedure describes standard web development practices for structured data markup. Schema implementation must accurately represent page content — fake reviews, inflated ratings, or misleading FAQ content violates Google's guidelines and risks manual penalties.
