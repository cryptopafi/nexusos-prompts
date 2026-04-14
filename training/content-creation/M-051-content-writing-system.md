---
type: procedure
created: 2026-03-22
status: active
slug: m-051-content-writing-system
tags: [procedure, nexus]
---

# Content Writing System (Articles/Blog Posts) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Sistem standardizat pentru producerea de articole și blog posts optimizate SEO, cu structură clară, CTA integrat și calitate editorială consistentă.

---

## 1. Problema

Articolele produse fără un sistem de scriere structurat sunt inconsistente ca lungime, calitate și optimizare SEO. Lipsa unui proces reproductibil duce la timpi de producție variabili, conținut fără CTA clar și rate de conversie slabe din trafic organic.

Situații acoperite:
- Producerea unui articol de blog nou de la zero
- Rescrierea sau actualizarea unui articol existent
- Crearea de pillar content sau cluster content pentru strategie topică

---

## 2. Procedura

### Pas 1: Brief de conținut și research pre-scriere
Înainte de a scrie un singur cuvânt, completează brief-ul de conținut: keyword principal, keyword-uri secundare (3-5), intenția de căutare, audiența țintă, obiectivul articolului și CTA-ul final. Analizează primele 3 articole de pe SERP pentru keyword-ul principal și notează lungimea medie, subtopicurile acoperite și formatele utilizate. Brief-ul completat este condiție obligatorie pentru pornirea scrierii.

### Pas 2: Structurarea outlineului
Creează un outline detaliat cu H1, H2-uri și H3-uri înainte de redactare. H1-ul trebuie să conțină keyword-ul principal. Planifică minimum 5 secțiuni H2 cu subtopicuri relevante. Include în outline: introducere cu hook, body cu subtopicuri, secțiune FAQ bazată pe "People Also Ask", și concluzie cu CTA. Outline-ul aprobat previne rescrierea structurală ulterioară.

### Pas 3: Redactarea introducerii cu hook
Scrie o introducere de 100-150 cuvinte care: (1) agăță cititorul cu o statistică, întrebare sau declarație provocatoare, (2) identifică problema pe care articolul o rezolvă, (3) promite soluția și beneficiul concret. Evită introducerile generice — fiecare introducere trebuie să fie specifică pentru subiect și audiență. Keyword-ul principal trebuie să apară în primele 100 de cuvinte.

### Pas 4: Redactarea body-ului cu optimizare on-page
Scrie fiecare secțiune H2 cu minimum 150-200 cuvinte. Distribuie keyword-ul principal cu densitate de 1-2% și keyword-urile secundare natural în text. Include statistici sau date cu surse credibile pentru fiecare claim major. Adaugă elemente de formatare: bullet points, tabele, bold pe concepte cheie. Scrie pentru cititorul uman, nu pentru algoritmul SEO — claritate și valoare mai presus de keyword stuffing.

### Pas 5: Optimizare SEO on-page completă
Completează toate elementele SEO: meta title (50-60 caractere cu keyword principal), meta description (150-160 caractere cu CTA inclus), alt text pentru toate imaginile, internal links (minimum 3) și external links către surse autoritare (minimum 2). Verifică că URL slug-ul conține keyword-ul principal și este scurt. Adaugă schema markup relevant (Article, FAQ, HowTo).

### Pas 6: Integrarea CTA și elementelor de conversie
Fiecare articol trebuie să conțină minimum un CTA principal clar, plasat în concluzie, și opțional un CTA secundar în body (content upgrade, lead magnet, link către pagină produs). CTA-ul trebuie să fie specific și relevant pentru subiectul articolului — nu generic. Adaugă social proof lângă CTA-ul principal dacă este disponibil (testimonial, număr clienți).

### Pas 7: Revizuire editorială și publicare
Rulează articolul prin checklist editorial: claritate, flow, erori gramaticale (Grammarly sau similar), plagiat (Copyscape), și respectarea brand voice. Verifică că toate imaginile sunt comprimate și au alt text. Asigură-te că articolul are minimum lungimea competitivă identificată în Pas 1. Publică cu data corectă, autorul corect și categoria/tag-urile corecte. Trimite URL-ul pentru indexare în Google Search Console.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-051 executată: Articol/blog post produs cu sistem complet — brief, outline, scriere, SEO on-page, CTA și revizuire editorială.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-051",
    "domain": "content-creation",
    "rule_id": "META-H-002",
    "tags": ["content-writing", "blog-posts", "SEO-on-page", "editorial", "CTA"],
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
- Articol publicat fără keyword research și brief completat → violation
- Articol fără CTA integrat → violation
- Meta title sau meta description lipsă la publicare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-creation

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Brief de conținut completat înainte de scriere?
- [ ] Toate elementele SEO on-page completate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-051 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Content Writing System (Articles/Blog Posts)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Ahrefs / SEMrush | Keyword data și analiza SERP |
| Grammarly / Hemingway | Revizuire editorială calitate |
| Copyscape | Verificare plagiat |
| Google Search Console | Indexare și monitorizare performanță |
| CMS (WordPress/Webflow) | Publicare și configurare SEO on-page |
| Content Ideation (M-050) | Sursa brief-ului de conținut |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Timp mediu producție articol | < 4 ore (standard) |
| SEO on-page completeness | 100% elemente completate |
| CTA inclusion rate | 100% articole |
| Organic ranking (90 zile) | Top 20 pentru keyword principal |
