---
id: LSEO-006
title: Local Schema Markup Implementation
domain: local-seo
source: AI Local SEO Pro Google My Business SEO Marketing
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Local Schema Markup Implementation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Implementarea corectă a structured data Schema.org JSON-LD pentru business-uri locale, cu cod exemplu complet, validare, și integrare cu GBP pentru Rich Results și Knowledge Panel.

---

## 1. Problema

Fără Schema markup, Google "ghicește" informațiile despre business din text — cu erori. Schema corect implementat crește acuratețea Knowledge Panel, activează Rich Results, și trimite semnale de structură care pot boost-a Local Pack ranking. 36% din search results conțin Rich Results — business-urile fără schema pierd vizibilitate.

Situații acoperite:
- Website business local fără niciun structured data
- Schema incompletă care nu generează Rich Results
- Adăugarea de noi tipuri de Schema pentru servicii sau produse
- **Albastru**: Schema pentru LodgingBusiness + Restaurant combinat

---

## 2. Procedura

### Pas 1: Alegerea tipului Schema LocalBusiness specific
Schema.org/LocalBusiness = tipul de bază. Folosește SUBTIPUL cel mai precis:
- Restaurant → schema.org/Restaurant
- Cazare → schema.org/LodgingBusiness (sau BedAndBreakfast)
- Dentist → schema.org/Dentist
- Avocat → schema.org/LegalService
- Construcții → schema.org/HomeAndConstructionBusiness
- Auto → schema.org/AutomotiveBusiness
- Agency → schema.org/ProfessionalService

**Albastru**: Folosește BedAndBreakfast pentru homepage + Restaurant pentru pagina restaurant. Sau LodgingBusiness ca primary cu Restaurant ca secondary (via additionalType).

Verifică lista completă la schema.org/LocalBusiness.

### Pas 2: Proprietățile obligatorii și recomandate

**Obligatorii** (fără ele, schema nu este valid):
- @context, @type, name, address (PostalAddress), telephone, url

**Recomandate** (cresc semnificativ impactul):
- image, priceRange, openingHours, geo (latitude/longitude), aggregateRating (DOAR reviews proprii, NU Google reviews), sameAs (social media URLs), hasMap, areaServed

### Pas 3: Codul JSON-LD complet (exemplu Albastru)

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BedAndBreakfast",
  "name": "Albastru & Origini",
  "image": "https://www.albastruorigini.ro/images/exterior.jpg",
  "url": "https://www.albastruorigini.ro",
  "telephone": "+40XXXXXXXXX",
  "priceRange": "€€",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "Sat [X], Comuna [Y]",
    "addressLocality": "Comuna [Y]",
    "addressRegion": "Argeș",
    "postalCode": "XXXXXX",
    "addressCountry": "RO"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": "XX.XXXXXX",
    "longitude": "XX.XXXXXX"
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"],
      "opens": "08:00",
      "closes": "22:00"
    }
  ],
  "sameAs": [
    "https://www.facebook.com/albastruorigini",
    "https://www.instagram.com/albastruorigini"
  ],
  "department": {
    "@type": "Restaurant",
    "name": "Șapte Focuri",
    "servesCuisine": "Romanian Traditional",
    "menu": "https://www.albastruorigini.ro/meniu"
  }
}
</script>
```

**NAP din Schema TREBUIE identic cu GBP și NAP master canonical** (LSEO-003). Orice discrepanță anulează beneficiul.

### Pas 4: Implementarea pe paginile cheie
Schema JSON-LD se adaugă în `<head>` sau `<body>` (Google acceptă ambele):

| Pagină | Schema type | Note |
|--------|------------|------|
| Homepage | LocalBusiness/BedAndBreakfast | OBLIGATORIU — schema principală |
| Contact | LocalBusiness (same as homepage) | Poate include hasMap |
| Pagini servicii | Service (serviceType, provider → link la main schema) | Per serviciu: cazare, restaurant, events |
| FAQ | FAQPage | Generează Rich Results FAQ în SERP |
| Blog posts | Article + BreadcrumbList | Crește CTR pe organic results |

**WordPress**: Rank Math SEO ($59/an) sau Yoast SEO Premium ($99/an) — ambele au Local SEO schema built-in. Completează câmpurile, plugin-ul generează JSON-LD automat.

**Custom site**: Adaugă manual codul JSON-LD în template-ul paginii.

### Pas 5: Schema FAQPage (bonus — Featured Snippets)
FAQ schema generează expandable Q&A direct în SERP — ocupă mai mult spațiu vizual:

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "Cât costă cazarea la Albastru & Origini?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Prețurile pornesc de la X RON/noapte pentru camera double. Include mic dejun tradițional și acces la grădină."
      }
    },
    {
      "@type": "Question",
      "name": "Cum ajung la Albastru & Origini din București?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Drumul de la București durează aproximativ Xh pe DN/A1. Adresa: [adresă completă]. Coordonate GPS: [lat, long]."
      }
    }
  ]
}
</script>
```

Adaugă 5-10 FAQ-uri relevante pe paginile de servicii și pe pagina FAQ dedicată.

### Pas 6: Validarea implementării (3 tool-uri obligatorii)
1. **Google Rich Results Test** (search.google.com/test/rich-results): Introdu URL-ul fiecărei pagini → verifică: 0 erori, 0 warnings critice. Dacă apar Rich Results: testul e PASS
2. **Schema Markup Validator** (validator.schema.org): Verifică structura JSON-LD — detectează erori de syntaxă și proprietăți invalide
3. **Google Search Console** → Enhancements: După 1-7 zile de la indexare, verifică secțiunea de structured data. Erori apar aici — corectează TOATE

**ATENȚIE**: NU adăuga aggregateRating care să afișeze reviews de pe Google în Schema — Google INTERZICE explicit asta. Folosește DOAR reviews colectate pe site-ul propriu. Violation = penalizare.

### Pas 7: Mentenanță și actualizare la schimbări de business
- **La orice schimbare** (adresă, ore, telefon, servicii noi): Actualizează Schema în max 48h
- **Audit trimestrial**: Rulează Rich Results Test pe toate paginile cu schema. Verifică că plugin updates (WordPress) nu au corupt schema
- **Event schema**: La evenimente speciale (festival, workshop, degustare): adaugă schema Event cu dates, location, offers — temporar, per eveniment
- **Review schema update**: Dacă implementezi review system pe site: actualizează aggregateRating cu date reale

---

## 3. Cortex Logging

```json
{
  "text": "Local Schema markup implementat: LocalBusiness/BedAndBreakfast JSON-LD pe homepage + paginile cheie, FAQPage schema pe FAQ, validat 0 erori Rich Results Test + Schema Validator + GSC, NAP identic cu GBP canonical.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "LSEO-006",
    "domain": "local-seo",
    "rule_id": "META-H-002",
    "tags": ["schema-markup", "structured-data", "local-seo", "json-ld", "rich-results", "faq-schema"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Schema cu erori nerezolvate în Rich Results Test → violation
- NAP în Schema diferit față de GBP canonical → violation
- aggregateRating cu reviews Google → violation policy
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum local-seo

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Schema validată 0 erori în Rich Results Test?
- [ ] NAP identic cu GBP canonical?
- [ ] FAQPage schema pe cel puțin o pagină?

**VK-uri obligatorii:**
1. `✅ [PROC] LSEO-006 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Local Schema Markup Implementation" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Rich Results Test | Validare implementare |
| Schema Markup Validator | Verificare structură JSON-LD |
| Google Search Console | Monitorizare Rich Results |
| Rank Math / Yoast SEO (WordPress) | Implementare Schema via plugin |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Erori Schema în Rich Results Test | 0 |
| Pagini cu Schema implementată | 100% din paginile cheie |
| Rich Results apărute în SERP | verificat în GSC |
| FAQ Rich Results active | ≥1 pagină |
| Audit trimestrial Schema executat | 100% |
| Timp de actualizare la schimbări | <48h |
