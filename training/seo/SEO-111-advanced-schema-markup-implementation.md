---
id: SEO-111
title: Advanced Schema Markup Implementation Guide
domain: seo
source: Advanced SEO 2025 — Structured Data
version: 2.0
forge_score: "estimated M/3.5"
---

# Advanced Schema Markup Implementation Guide

## Obiectiv
Implementare comprehensivă Schema.org cu JSON-LD pentru Rich Results eligibility, cu schema types per tip de pagina (FAQ, HowTo, LocalBusiness, Product, Review, Event, Recipe), validare workflow, CTR impact și gambling SEO schema specifics.

## Pași

### 1. Schema types prioritare per tip de site
**Universale (orice site)**:
- `Organization` — homepage: date business
- `WebSite` — homepage: sitelinks searchbox
- `BreadcrumbList` — toate paginile: navigare
- `WebPage` / `Article` — pagini conținut

**Per business type**:
- E-commerce: `Product`, `Offer`, `Review`, `AggregateRating`
- Local Business: `LocalBusiness` (Restaurant, Hotel, etc.)
- Blog/Editorial: `Article`, `BlogPosting`, `NewsArticle`
- Events: `Event`
- Recipes: `Recipe` (RELEVANT pentru Albastru)
- Education: `Course`, `EducationalOrganization`

**Rich Results eligibile**: FAQ, HowTo, Product, Recipe, Event, Job Posting, Review Snippet

### 2. Organization schema (homepage — JSON-LD)
```json
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Albastru & Origini",
  "url": "https://www.albastru-origini.ro",
  "logo": {"@type": "ImageObject", "url": "https://www.albastru-origini.ro/logo.png"},
  "description": "Restaurant cu foc deschis și cazare rurală în Argeș",
  "address": {"@type": "PostalAddress", "addressLocality": "Argeș", "addressCountry": "RO"},
  "telephone": "+40...",
  "sameAs": ["https://facebook.com/albastru", "https://instagram.com/albastru"]
}
```

### 3. Recipe schema (Rich Results spectaculoase — GOLD pentru food sites)
```json
{
  "@context": "https://schema.org/",
  "@type": "Recipe",
  "name": "Fasole cu Ciolan la Tuciul de Fontă",
  "image": ["https://example.com/fasole.jpg"],
  "author": {"@type": "Person", "name": "Chef Albastru"},
  "datePublished": "2026-03-20",
  "description": "Rețetă tradițională de fasole cu ciolan la foc deschis",
  "prepTime": "PT30M", "cookTime": "PT2H", "totalTime": "PT2H30M",
  "recipeYield": "6 porții",
  "recipeCategory": "Fel principal", "recipeCuisine": "Română",
  "nutrition": {"@type": "NutritionInformation", "calories": "450 calories"},
  "recipeIngredient": ["500g fasole uscată", "1 ciolan afumat", "2 morcovi"],
  "recipeInstructions": [
    {"@type": "HowToStep", "name": "Înmoaie", "text": "Pune fasolea la înmuiat peste noapte"}
  ],
  "aggregateRating": {"@type": "AggregateRating", "ratingValue": "4.8", "ratingCount": "127"}
}
```

### 4. FAQ schema (dublează spațiul SERP cu dropdowns)
```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question", "name": "Cum se face rezervarea?",
      "acceptedAnswer": {"@type": "Answer", "text": "Rezervările se fac online sau la +40..."}
    },
    {
      "@type": "Question", "name": "Ce includ pachetele weekend?",
      "acceptedAnswer": {"@type": "Answer", "text": "Cazare + mic dejun + acces bucătărie foc deschis."}
    }
  ]
}
```

### 5. Product schema (e-commerce)
```json
{
  "@context": "https://schema.org/",
  "@type": "Product",
  "name": "Tuciu Fontă 28cm Tradițional",
  "image": "https://example.com/tuciu.jpg",
  "description": "Tuciu de fontă 28cm, gătit pe foc deschis",
  "brand": {"@type": "Brand", "name": "Șapte Focuri"},
  "offers": {
    "@type": "Offer", "price": "180", "priceCurrency": "RON",
    "availability": "https://schema.org/InStock",
    "url": "https://example.com/tuciu-28cm"
  },
  "aggregateRating": {"@type": "AggregateRating", "ratingValue": "4.9", "ratingCount": "85"}
}
```

### 6. LocalBusiness + Event schema
**LocalBusiness** (pentru Albastru):
```json
{
  "@type": "Restaurant",
  "name": "Șapte Focuri",
  "servesCuisine": "Romanian Traditional",
  "address": {"@type": "PostalAddress", "addressLocality": "Argeș", "addressCountry": "RO"},
  "openingHours": "Mo-Su 10:00-22:00",
  "priceRange": "$$"
}
```

**Event** (pentru live cooking events):
```json
{
  "@type": "Event",
  "name": "Live Cooking — Fasole la Foc Deschis",
  "startDate": "2026-04-15T19:00",
  "location": {"@type": "VirtualLocation", "url": "https://tiktok.com/@albastru/live"},
  "offers": {"@type": "Offer", "price": "0", "priceCurrency": "RON"}
}
```

### 7. Implementare tehnică și validare
**JSON-LD** (recomandat Google): `<script type="application/ld+json">` în `<head>` sau `</body>`
**WordPress plugins**: Rank Math (recomandat), Yoast SEO Premium, Schema Pro

**Validare workflow**:
1. Google Rich Results Test (search.google.com/test/rich-results) — eligibilitate
2. Schema.org Validator (validator.schema.org) — conformitate
3. GSC → Enhancements → verifică schema indexate + erori
4. Monitor: check lunar Enhancements pentru noi erori

**Erori comune**:
- AggregateRating cu date false → Manual Action garantat
- FAQ schema fără FAQ vizibil pe pagină → schema removed
- 2x Organization pe aceeași pagină → conflict

### 8. Gambling SEO schema specifics
- **SportsEvent** schema pentru pagini pariuri sportive (meci, dată, echipe)
- **Review** schema pentru review-uri casino (cu disclaimer gambling)
- **FAQ** schema pe pagini "ghid pariuri" — real FAQs from users
- NU: AggregateRating pe pagini casino fără reviews reale
- LocalBusiness NU se aplică la site-uri gambling online (doar landbased casinos)

## Verificare
- [ ] Organization schema pe homepage, validat Rich Results Test
- [ ] BreadcrumbList pe toate paginile (verificat GSC Enhancements)
- [ ] Per tip pagina: Recipe/FAQ/Product/Article implementat corect
- [ ] 0 erori, 0 warnings în Rich Results Test pe paginile principale
- [ ] GSC Enhancements section monitorizat — zero erori
- [ ] LocalBusiness schema (dacă business fizic)
- [ ] WordPress plugin schema configurat (Rank Math/Yoast)
- [ ] Validare workflow documentat (test → deploy → monitor)

## Instrumente
- Google Rich Results Test — validare + preview
- Schema.org — documentație completă
- Rank Math / Yoast Premium — implementare WordPress
- JSON-LD Generator (technicalseo.com/tools/schema-markup-generator)
- GSC → Enhancements — monitorizare continuă

## Note
- Schema corectă crește CTR 20-30% fără schimbare de poziție (Rich Results vizuale)
- Recipe schema pentru Albastru = oportunitate imediată, diferențiatoare vs competitori
- Evită "schema spam": relevanta > cantitate — nu implementa toate tipurile pe toate paginile
- Google nu garantează Rich Results chiar cu schema corectă — eligibilitate ≠ garantat
- Gambling: SportsEvent + FAQ schema = low-hanging fruit pentru pariuri sportive pages
