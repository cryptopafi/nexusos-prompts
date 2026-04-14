---
type: procedure
created: 2026-03-22
status: active
slug: m-077-ai-search-optimization
tags: [procedure, nexus]
---

# AI Search Optimization and AI Visibility Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Auditarea și optimizarea vizibilității unui brand sau produs în răspunsurile AI (ChatGPT, Claude, Gemini, Perplexity), prin strategii GEO (Generative Engine Optimization) și AIO (AI Optimization).

---

## 1. Problema

Motoarele de căutare AI (ChatGPT, Claude, Perplexity, Google AI Overviews) devin canale primare de descoperire pentru branduri și produse. Companiile care nu apar în răspunsurile AI pierd vizibilitate față de competitorii citați. SEO tradițional nu este suficient — este nevoie de o strategie specifică de AI Search Optimization (GEO/AIO).

Situații acoperite:
- Auditul vizibilității actuale a brandului în răspunsurile AI
- Strategia de conținut pentru a fi citat de motoarele AI
- Optimizarea conținutului existent pentru citabilitate AI
- Monitorizarea și raportarea AI visibility metrics

---

## 2. Procedura

### Pas 1: Audit — Auditează vizibilitatea AI actuală
Testează manual: interoghează ChatGPT, Claude și Gemini cu 10-15 query-uri relevante pentru nișa ta (ex: „Care sunt cele mai bune [categorie produs] pentru [ICP]?", „Cum să [problemă pe care o rezolvi]?", „Compară [tu] vs [competitor]"). Documentează: ești menționat? Cum ești descris? Care competitori sunt citați mai frecvent? Creează un baseline de vizibilitate AI.

### Pas 2: Benchmark — Identifică conținutul citat de AI în nișa ta
Analizează ce tip de conținut AI-urile citează pentru query-urile din Pasul 1: articole de tip „ghid complet", studii cu date originale, comparatoare structurate, conținut de pe site-uri cu autoritate (Wikipedia, publicații majore, site-uri .edu/.gov). Notează formatele, structura și sursele dominante.

### Pas 3: Optimize — Optimizează conținutul existent pentru citabilitate AI
Aplică GEO best practices pe conținutul existent cu cel mai mare potențial: (a) adaugă date originale și statistici verificabile, (b) structurează cu headings clare și răspunsuri directe la întrebări (question-answer format), (c) include definiții clare ale termenilor cheie, (d) adaugă surse și referințe citate, (e) îmbunătățește E-E-A-T signals (Experience, Expertise, Authoritativeness, Trustworthiness).

### Pas 4: E-E-A-T — Construiește semnalele de autoritate
Acțiuni concrete pentru E-E-A-T: (a) **Experience** — adaugă studii de caz proprii, date din proiecte reale; (b) **Expertise** — bio-uri de autori cu credențiale, publicații în media de specialitate; (c) **Authoritativeness** — link-uri de la site-uri autoritare, mențiuni în publicații majore; (d) **Trustworthiness** — politici clare, date de contact, certificări, recenzii verificate.

### Pas 5: Content — Creează conținut nou optimizat pentru AI citation
Planifică 3-5 piese de conținut „AI-citation-ready": studii de industrie cu date proprii, ghiduri comprehensive care răspund la întrebări frecvente în nișă, comparatoare obiective (incluzând competitori), glossare de termeni de specialitate, white papers sau rapoarte originale. Fiecare piesă trebuie să aibă date unice și o structură clară citabilă.

### Pas 6: Monitor — Monitorizează AI visibility săptămânal
Setează un proces de monitorizare: (a) testare manuală săptămânală cu query-urile de referință din Pasul 1, (b) monitorizarea mențiunilor brandului în răspunsuri AI (manual sau tool specializat), (c) tracking al pozițiilor pentru query-urile țintă, (d) raportare lunară de AI Visibility Score (% query-uri în care ești menționat din totalul testate).

---

## 3. Cortex Logging

```json
{
  "text": "AI Search Optimization executat pentru [brand/domeniu]. Audit baseline: [% vizibilitate]. Platforme testate: ChatGPT, Claude, Gemini. Acțiuni GEO aplicate: [X]. AI Visibility Score post-optimizare: [%].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-077",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["geo", "aio", "ai-search-optimization", "ai-visibility", "eeat", "generative-engine"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
La fiecare audit de strategie SEO/marketing digital sau la lansarea unei campanii de conținut

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Audit (Pasul 1) executat pe mai puțin de 3 platforme AI → violation
- Monitorizare (Pasul 6) săptămânală omisă după implementare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-marketing

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-077 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Search Optimization and AI Visibility Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT / Claude / Gemini / Perplexity | Testare manuală vizibilitate |
| Ahrefs / SEMrush | Analiza conținutului existent |
| CMS (WordPress/Webflow) | Implementare optimizări conținut |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| AI Visibility Score (baseline → target) | +20% în 90 zile |
| Query-uri testate în audit | Min. 10 |
