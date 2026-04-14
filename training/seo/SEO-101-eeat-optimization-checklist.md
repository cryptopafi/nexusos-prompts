---
id: SEO-101
title: EEAT Optimization Checklist
domain: seo
source: Advanced SEO 2025 — Authority & Trust Signals
version: 2.0
forge_score: "estimated M/3.5"
---

# EEAT Optimization Checklist

## Obiectiv
Optimizarea completă a semnalelor EEAT (Experience, Expertise, Authoritativeness, Trustworthiness) pentru îmbunătățirea evaluărilor Quality Raters Google, cu author pages, credentials, citations, bylines, review process, schema markup și prioritizare acțiuni pe timeline realistă.

## Pași

### 1. Experience — demonstrarea experienței directe
- Adaugă foto/video din experiența directă (nu stock photos)
- Include date concrete: "Am testat acest produs 30 zile și am obținut X"
- Review-uri: data cumpărării, nr. comandă, experiență specifică
- Secțiune "Tested by" / "Based on X years experience" în intro
- UGC: reviews cu foto/video de la utilizatori reali (Google le preferă)
- Schema markup Review cu datePublished și author

### 2. Expertise — author pages și credentials
**Author Bio pages (OBLIGATORIU)**:
- Fiecare autor: pagină dedicată cu CV, educație, publicații, certificări
- Link-uri externe: LinkedIn, Google Scholar, publicații specialitate
- Site-uri YMYL (medicale/legale/financiare): autor = specialist certificat obligatoriu
- Actualizare biografie: minim anual
- Byline vizibil pe fiecare articol: "[Numele Autorului] — [Credențiale]"

**Conținut expertise signals**:
- Citații și referințe surse autoritative (studii, publicații, rapoarte)
- Profunzime > competitori (depth > word count)
- FAQ bazat pe date reale de search (People Also Ask, GSC queries)
- Glossary/Dicționar termeni domeniu (semnal autoritate tematică)

### 3. Authoritativeness — on-page și off-page signals
**Off-page**:
- Linkuri editoriale de pe site-uri autoritative din nișă
- Unlinked brand mentions pe site-uri relevante
- Interviuri, podcast-uri, guest articles pe publicații referință
- Press mentions (comunicate, apariții media)
- Wikipedia/Wikidata mention (gold standard autoritate)

**On-page**:
- Internal linking structurat: pillar pages → cluster articles
- Citare surse autoritative cu anchor text natural
- Content freshness: actualizare regulată conținut vechi
- Topical authority: acoperire comprehensivă a nișei (cluster model)

### 4. Trustworthiness — elemente tehnice și legale
**Pagini obligatorii**:
- [ ] HTTPS cu SSL valid și HSTS enabled
- [ ] Contact: adresă fizică, telefon, email, CUI (business)
- [ ] About: echipă reală, misiune, istorie autentică
- [ ] Terms & Conditions + Privacy Policy (GDPR compliant)
- [ ] Return Policy (e-commerce)
- [ ] Recenzii reale integrate (Google Reviews, Trustpilot widget)

**Technical trust signals**:
- Core Web Vitals: LCP <2.5s, FID/INP <100ms, CLS <0.1
- Zero broken links (crawl lunar Screaming Frog)
- Zero redirect chains >2 salturi
- Schema markup Organization cu toate câmpurile
- Certificat SSL valid, HSTS header active

### 5. Schema markup EEAT (JSON-LD)
**Person schema (author page)**:
```json
{
  "@type": "Person",
  "name": "Dr. Maria Ionescu",
  "jobTitle": "Nutriționist Certificat",
  "url": "https://site.ro/author/maria-ionescu",
  "sameAs": ["https://linkedin.com/in/maria-ionescu"],
  "alumniOf": "Universitatea de Medicină București",
  "knowsAbout": ["nutriție", "dietetică", "sănătate"]
}
```

**Organization schema (homepage)**:
```json
{
  "@type": "Organization",
  "name": "Brand Name",
  "url": "https://site.ro",
  "logo": "https://site.ro/logo.png",
  "contactPoint": {"@type": "ContactPoint", "telephone": "+40..."},
  "sameAs": ["https://facebook.com/brand", "https://instagram.com/brand"]
}
```

### 6. Review process documentation
- Publică "Editorial Policy" page: cum sunt create, verificate și actualizate articolele
- "Fact-checking process" vizibil: cine verifică, ce surse sunt folosite
- "Medical/Legal/Financial review" disclaimer: "Articol revizuit de [Expert] pe [Data]"
- Update log vizibil pe articole: "Ultima actualizare: [Data] de [Autor]"

### 7. EEAT audit — cum evaluezi și prioritizezi
**Audit process**:
1. `site:domeniu.com` în Google → cum apar paginile
2. Citește Google Search Quality Evaluator Guidelines (175 pagini, gratuit)
3. Screaming Frog crawl → pagini fără author markup identificate
4. Verifică author pages: complete sau goale?
5. GSC → Manual Actions → penalizări active?

**Prioritizare impact (timeline)**:
- **Săptămâna 1-2**: Trustworthiness (HTTPS, legal pages, contact) — impact rapid
- **Luna 1-2**: Author pages + Expert Bio complete — impact 30-60 zile
- **Luna 2-3**: Content depth și citații — impact 60-90 zile
- **Luna 3-6**: Off-page authority (linkuri, mențiuni) — impact 3-6 luni

### 8. EEAT per nișă — ajustări specifice
**YMYL (Your Money Your Life)** — sănătate, finanțe, juridic:
- Evaluare MULT mai strictă — invest maxim în Expertise + Trust
- Autor OBLIGATORIU specialist certificat
- Referințe surse academice/oficiale obligatorii

**Gambling SEO**: EEAT signals mici dar nu zero — focus pe Trustworthiness (licențe, disclaimer) și Experience (revieweri cu cont real pe platforme)

**E-commerce**: Product schema + AggregateRating + real reviews = EEAT principal

## Verificare
- [ ] Author pages complete pentru toți autorii (bio, credentials, links externe)
- [ ] Schema Person/Organization implementat (validat Rich Results Test)
- [ ] Trust pages complete (About, Contact, T&C, Privacy) — actualizate
- [ ] Core Web Vitals verde (LCP <2.5s, INP <100ms, CLS <0.1)
- [ ] Zero broken links (Screaming Frog crawl lunar)
- [ ] Editorial Policy publicată
- [ ] Bylines vizibile pe fiecare articol
- [ ] EEAT audit completat cu timeline prioritizare

## Instrumente
- Google Search Quality Evaluator Guidelines — referință EEAT
- Screaming Frog SEO Spider — crawl tehnic
- Schema.org markup generator + Rich Results Test validare
- Ahrefs / Semrush — audit autoritate și linkuri
- Google Search Console — Manual Actions check

## Note
- EEAT nu e algoritm separat — e set de criterii pentru Quality Raters umani
- YMYL evaluat mult mai strict — invest mai mult în Expertise/Trust
- "Hidden" EEAT signal: comportamentul user (bounce, dwell time, pogo-sticking)
- Schema corectă poate crește CTR 20-30% fără schimbare de poziție
