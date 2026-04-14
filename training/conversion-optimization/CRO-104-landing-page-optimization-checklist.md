---
id: CRO-104
title: Landing Page Optimization Checklist
domain: conversion-optimization
source: CRO Landing Page Course / Complete Conversion Optimization Course
version: 2.0
forge_score: "estimated L/3.0"
business_mapping: ["ai-agency"]
---

# Landing Page Optimization Checklist — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Checklist complet de optimizare landing pages cu conversion benchmarks per industrie (SaaS 3-5%, ecom 2-3%, lead gen 5-15%), 5-second test methodology, message match verification, friction/anxiety elimination, și mobile-first audit.

---

## 1. Problema

Landing page-urile suboptime pierd potențialul traficului plătit și organic. Benchmark CR per industrie: SaaS trial/demo 3-5%, e-commerce 2-3%, lead generation 5-15%, B2B services 2-5%. Un landing page sub benchmark-ul industrie pierde revenue cu fiecare vizitator. Acest checklist asigură că fiecare element al paginii este optimizat pentru conversie maximă.

Situații acoperite:
- Audit landing page existent cu conversie sub benchmark
- Crearea landing page nou optimizat de la început
- Pre-launch review înainte de campanie PPC

---

## 2. Procedura

### Pas 1: Above the Fold — Testul de 5 secunde (obligatoriu)
Deschide landing page-ul. Arată-l 5 secunde unei persoane care NU cunoaște business-ul. Întreabă:
1. "Ce face acest business/produs?"
2. "Pentru cine este?"
3. "Ce acțiune trebuie să faci?"

Dacă NU poate răspunde la TOATE 3 → above the fold trebuie refăcut.

**Tool**: UsabilityHub/Lyssna ($79/mo) — panel extern, 20 participanți, rezultate în 24h. Alternativa free: arată colegilor/prietenilor din outside industry.

**Elementele obligatorii above the fold**:
- [ ] Headline cu propunere de valoare CLARĂ și SPECIFICĂ (nu vag)
- [ ] Subheadline care extinde/clarifică headline-ul
- [ ] Hero image/video relevant (nu stock generic)
- [ ] CTA primar VIZIBIL fără scroll
- [ ] Minim 1 trust signal (nr. clienți, rating, logo client recunoscut)

### Pas 2: Message match — alinierea cu sursa traficului
Textul din ad/email → headline landing page trebuie să fie CONSISTENT:

| Sursa | Ad text | LP headline | Match? |
|-------|---------|-------------|--------|
| Google Ad | "Crește vânzările cu AI în 30 zile" | "Crește vânzările cu AI în 30 zile" | ✅ PERFECT |
| Google Ad | "Crește vânzările cu AI în 30 zile" | "Soluții avansate de business" | ❌ MISMATCH → bounce |

Verifică message match pentru FIECARE sursă majoră de trafic:
- Google Ads (fiecare ad group poate avea headline diferit → landing page diferit sau dynamic text)
- Facebook/Instagram Ads (copy-ul ad-ului → reflectat în LP)
- Email campaigns (subject + email body → LP headline)
- Organic search (meta title/description → LP content alignment)

Mismatch = bounce instant. Benchmark: message match corect reduce bounce rate cu 30-50%.

### Pas 3: Clarity checklist — eliminarea confuziei
Parcurge pagina secțiune cu secțiune:
- [ ] Propunerea de valoare este SPECIFICĂ (nu vagă): "Crește reply rate-ul cu 47% în 14 zile" ✅ vs. "Soluții avansate de growth" ❌
- [ ] Headline-urile descriu BENEFICII (nu features): "Economisești 10h/săptămână" ✅ vs. "Dashboard cu 50 metrici" ❌
- [ ] Fiecare secțiune are un scop clar (contribuie la decizia de conversie)
- [ ] Jargon tehnic explicat sau eliminat
- [ ] Bullet points cu beneficii concrete (nu funcționalități abstracte)
- [ ] Imagini/video relevante (nu stock photos generice — studii arată că stock photos REDUC conversia)
- [ ] Prețul vizibil și contextul valorii clar (dacă e relevant pentru pagină)
- [ ] O singură direcție clară pe pagină (nu 5 opțiuni egale)

### Pas 4: Friction reduction — simplificarea acțiunii de conversie
Friction = orice lucru care face conversia mai GREA. Fiecare punct de friction redus = uplift imediat:

| Friction point | Impact | Fix |
|---------------|--------|-----|
| Formular >4 câmpuri (lead gen) | -10-15% CR per câmp extra | Reduce la 3-4 câmpuri esențiale |
| Multiple CTA-uri egale | Confusion → paralysis | 1 CTA primar + 1 secundar max |
| Pași invizibili | Anxiety | Arată progress bar (Step 1 of 3) |
| Plată fără guest checkout | -35% abandonare | Guest checkout obligatoriu |
| Lipsa metode de plată | Variabilă | Adaugă card + PayPal + Apple Pay |
| Loading slow (>3s) | -53% vizitatori pleacă | Optimizează sub 2.5s LCP |
| Captcha vizibil | -3-10% | reCAPTCHA v3 (invisible) |

**Regula câmpurilor formular per tip de ofertă**:
- Newsletter / free resource: Email only (1 câmp)
- Free trial / demo request: Name + Email + Company (3 câmpuri)
- Contact / quote request: Name + Email + Company + Phone (4 câmpuri max)

### Pas 5: Anxiety elimination — trust signals obligatorii
Fiecare obiecție anticipată trebuie adresată PE PAGINĂ:

| Anxiety | Trust signal |
|---------|-------------|
| "E sigur să-mi dau datele?" | SSL badge, privacy policy link, "No spam, ever" lângă formular |
| "Merită banii?" | Money-back guarantee vizibilă (30-60 zile), ROI calculator |
| "Sunt destul de buni?" | Credentiale, certifications, awards, years in business |
| "Funcționează cu adevărat?" | Case studies cu CIFRE reale, video testimonials |
| "Ce dacă nu-mi place?" | Free trial, free demo, no credit card required |
| "Sunt reali?" | Photos echipă reale (nu stock), adresă fizică, telefon |

Poziționare: Trust signals lângă CTA (nu buried în footer). Garantia money-back direct sub butonul de plată.

### Pas 6: Mobile optimization audit (device real)
Deschide pe device real (NU doar Chrome DevTools):
- [ ] CTA vizibil fără scroll pe mobile (above fold diferit de desktop!)
- [ ] Buton CTA ≥44x44px (Apple guideline touch target)
- [ ] Text lizibil fără zoom (≥16px body, ≥24px headlines)
- [ ] Imagini optimizate (WebP, lazy loading, nu 5MB hero image)
- [ ] Formularul ușor de completat pe tastatura mobilă (input types: tel, email)
- [ ] Pagina încarcă sub 2.5s pe 4G (test cu PageSpeed Insights pe mobile)
- [ ] Sticky CTA button pe scroll (pe pagini lungi mobile)
- [ ] Nu pop-ups care blochează conținutul pe mobile (Google penalizează)

**Benchmark**: Dacă mobile CR este >30% mai mic decât desktop CR → problemă de UX mobilă. Prioritizează fixul.

### Pas 7: CTA optimization — text, design, poziție
CTA-ul este cel mai testat element pe landing pages:

**Text button**:
- SPECIFIC > GENERIC: "Descarcă ghidul gratuit" > "Submit". "Începe free trial 14 zile" > "Get Started"
- First person: "Start My Free Trial" outperforms "Start Your Free Trial" cu 25-90% (ContentVerve study)
- Include beneficiu: "Get My Free Audit" > "Request Audit"

**Design button**:
- Culoare care CONTRASTEAZĂ cu background-ul (nu care se asortează)
- Dimensiune suficientă: minim 44px height, full-width pe mobile
- Whitespace în jurul butonului (nu blocat de alte elemente)

**Poziție**:
- Above fold (prima vizualizare) — OBLIGATORIU
- Repetat după fiecare secțiune majoră pe pagini lungi
- Sticky header sau footer CTA pe mobile

**Micro-copy sub buton**: "No credit card required." / "Cancel anytime." / "Takes 30 seconds." — reduce anxiety la momentul deciziei.

---

## 3. Cortex Logging

```json
{
  "text": "Landing page optimization checklist completat: 5-second test executat, message match verificat per sursă trafic, friction redusă (≤4 form fields), anxiety adresată cu trust signals, mobile audit pe device real, CTA optimizat (specific, contrast, repeated).",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "CRO-104",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["landing-page", "cro", "optimization-checklist", "friction-reduction", "message-match", "mobile-first"],
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
- LP lansat fără 5-second test → violation
- Message match neverificat pentru surse de trafic → violation
- Formular >5 câmpuri fără justificare → violation
- Mobile audit neglijat → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] 5-second test executat cu persoană externă?
- [ ] Message match verificat per sursă trafic?
- [ ] Mobile audit pe device real?

**VK-uri obligatorii:**
1. `✅ [PROC] CRO-104 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Landing Page Optimization Checklist" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| UsabilityHub / Lyssna ($79/mo) | 5-second test cu panel extern |
| PageSpeed Insights | Mobile performance audit |
| Hotjar / Clarity | Validare post-launch (heatmap) |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| 5-second test: propunere de valoare înțeleasă | ≥80% participanți |
| Câmpuri formular (lead gen) | ≤4 |
| Mobile PageSpeed Score | ≥80 |
| Mobile CR vs. Desktop CR gap | <30% |
| CTA text specific (nu generic) | 100% |
| Message match per traffic source | 100% |
| Uplift conversie post-optimizare | ≥15% vs. baseline |
