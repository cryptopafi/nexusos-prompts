---
id: SEO-102
title: Google Penalty Recovery SOP
domain: seo
source: Advanced SEO 2025 — Penalty Recovery
version: 2.0
forge_score: "estimated M/3.5"
---

# Google Penalty Recovery SOP

## Obiectiv
Protocol complet de diagnosticare și recovery pentru penalizări Google manuale și algoritmice (Penguin, Panda, Core Updates, Helpful Content), cu disavow file format, reconsideration request template, content audit framework și monitoring post-recovery.

## Pași

### 1. Diagnosticarea tipului de penalizare
**Manuală vs Algoritmică**:
- **Manuală**: GSC → Security & Manual Actions → Manual Actions → message explicit
  - Tipuri: Unnatural links, Thin content, Cloaking, User-generated spam
- **Algoritmică**: NU apare în GSC. Diagnosticare prin corelație:
  - Verifică Google Core Update dates (searchengineland.com/google-algorithm-updates)
  - Drop trafic la 3 zile de update = probabil algoritmic
  - Verifică Semrush/Ahrefs Organic Research → Traffic history per date

### 2. Recovery penalizare manuală — Unnatural Links
**Audit link profile**:
- Export toate linkurile: Ahrefs + Semrush + GSC → Links
- Identifică toxic: link farms, directories ieftine, PBN-uri compromise, footer links plătite
- Score toxicitate: Semrush Backlink Audit Tool — toxic score per link

**Contactare proprietari site-uri** (2-3 săptămâni):
- Email formal scurt: "Am observat un link de pe site-ul dvs. spre al meu. Vă rog să îl eliminați."
- Documentează TOATE încercările în spreadsheet (domeniu, email, data, status)

**Google Disavow file** (dacă proprietarii nu răspund):
```
# Toxic domains identified via Ahrefs/Semrush audit
# Date: 2026-03-20
# Reason: Unnatural links penalty recovery
domain:badsite1.com
domain:badsite2.com
# Specific toxic pages
https://specificbadpage.com/toxic-link-page
```
Upload: GSC → Legacy Tools → Disavow Links
**ATENȚIE**: disavow greșit face MAI MULT rău — disavow DOAR linkuri clar toxice

**Reconsideration Request** (GSC → Manual Actions → Request Review):
- Rezumat: ce am găsit, ce am făcut, dovezi (email-uri, spreadsheet)
- Fii transparent, asumă-ți responsabilitatea
- Review: 2-4 săptămâni

### 3. Recovery penalizare conținut (Panda/Helpful Content)
**Content Audit complet**:
- Screaming Frog crawl → export toate URL-urile
- Identifică: <300 cuvinte / duplicate intern / scraped / AI fără valoare

**Decizie per pagină (4 opțiuni)**:
1. **Îmbunătățește**: adaugă valoare reală, extinde, actualizează
2. **Consolidează**: merge pagini similare cu 301 redirect
3. **Noindex**: pagini tehnice fără valoare SEO (tags, search results)
4. **Șterge + 301**: pagini complet inutile → redirect pagina cea mai relevantă

### 4. Recovery după Core Update
**Checklist post-Core Update**:
- [ ] Audit EEAT complet (SEO-101)
- [ ] Conținut mai bun decât top 3 competitori per keyword
- [ ] Conținut low-quality eliminat sau îmbunătățit
- [ ] Satisfacție utilizatori: bounce rate, dwell time verificate
- [ ] Date vechi actualizate (statistici, prețuri, informații)
- [ ] Expertise signals adăugate (author bio, surse, date reale)

**Timeline realiste**: Core Update recovery durează 3-6 luni. Adesea se rezolvă la URMĂTORUL Core Update.

### 5. Penalty avoidance — link profile health
**Anchor text safety**: exact match <20% din total
**Link velocity**: spike brusc (500 linkuri/7 zile) = suspect
**TF/CF ratio**: Trust Flow trebuie minim 40% din Citation Flow
**Monitoring**: Ahrefs Alerts → noi linkuri zilnic + keyword drops săptămânal

### 6. Disavow file management ongoing
- Review disavow file trimestrial
- Nu "over-disavow": linkuri bune disavowed = pierdere PageRank
- Regula: disavow DOAR linkuri clar toxice (spam score >60%, TF <5)
- Păstrează disavow file versionat (date + reason per entry)

### 7. Monitoring post-recovery (early warning)
**GSC alerts**: Manual Actions, Security Issues, Coverage Errors
**Ahrefs Alerts**: pierderi linkuri importante, keyword drops >5 poziții
**GA4 Intelligence**: alertă dacă organic traffic scade >20% vs previous period
**Calendar audit**: zilnic GSC (5 min) → săptămânal keywords → lunar full link profile → trimestrial tech audit

### 8. Gambling SEO penalty specifics
- Gambling sites = high risk PBN/link scheme penalties
- Google monitorizează agresiv nișa gambling/casino
- Tier 2 link building (linkuri la PBN-uri, nu direct la money site) = detectabil dar mai greu
- Parasite SEO (guest posts pe domenii autoritative) = risc crescut post-2024 updates
- Recovery gambling site: 6-12 luni (mai lung decât alte nișe)

## Verificare
- [ ] Tip penalizare diagnosticat (manuală vs algoritmică + care update)
- [ ] Disavow file creat și uploadat (dacă penalizare linkuri)
- [ ] Reconsideration Request trimis cu documentație (penalizare manuală)
- [ ] Content Audit complet cu acțiuni per pagină (îmbunătățire/consolidare/noindex/delete)
- [ ] Monitoring alerts configurate (GSC + Ahrefs + GA4)
- [ ] Timeline recovery realistă documentată (3-6 luni)
- [ ] Disavow file versionat cu date și motivare per entry
- [ ] Post-recovery audit calendarizat (trimestrial)

## Instrumente
- Google Search Console — manual actions, performance, coverage
- Ahrefs Site Audit + Backlink Checker — link profile audit
- Semrush Backlink Audit Tool — toxic score per link
- Screaming Frog — content audit tehnic
- Google Disavow Tool (GSC Legacy Tools)
- Copyscape — verificare plagiat extern

## Note
- Nu "over-disavow": discreditare linkuri bune = pierdere PageRank
- Helpful Content Update: site-ul rămâne "tainted" până la next update
- Cea mai bună recovery = prevenirea: build corect de la început
- Gambling: recovery 6-12 luni, PBN detection risk crescut
