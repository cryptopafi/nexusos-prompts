---
type: procedure
created: 2026-03-22
status: active
slug: m-075-ai-review-analysis
tags: [procedure, nexus]
---

# AI Review Analysis and Product Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Extragerea de insights acționabile din recenziile clienților folosind AI pentru sentiment analysis, identificarea temelor recurente și generarea de recomandări de optimizare a produsului.

---

## 1. Problema

Recenziile clienților conțin informații valoroase despre produse și experiențe, dar volumul mare face imposibilă analiza manuală sistematică. Fără structurare, companiile ratează pattern-uri critice de feedback care ar putea ghida îmbunătățirile de produs. AI poate transforma sute sau mii de recenzii în insights strategice în ore.

Situații acoperite:
- Analiza recenziilor produselor e-commerce (Amazon, Emag, Shopify)
- Evaluarea feedback-ului pentru SaaS (G2, Capterra, Product Hunt)
- Analiza recenziilor locale (Google Maps, Tripadvisor)
- Monitorizarea sentimentului post-lansare produs nou

---

## 2. Procedura

### Pas 1: Collect — Colectează recenzii din toate platformele relevante
Exportă sau scrape recenzii din: Google Business (dacă aplicabil), Amazon/marketplace-uri, G2/Capterra/Trustpilot pentru SaaS, App Store/Google Play pentru aplicații mobile, social media mentions (opțional). Documentează: platform, număr total recenzii, perioada acoperită, rating mediu. Target minim: 50 recenzii pentru analiză semnificativă.

### Pas 2: Prepare — Structurează datele pentru analiză AI
Curăță recenziile: elimină duplicatele, recenziile spam sau prea scurte (sub 10 cuvinte), recenziile irelevante (despre livrare vs. produs dacă analizezi produsul). Organizează în format CSV sau text structurat: [rating | platforma | data | text recenzie]. Separă recenziile pozitive (4-5 stele) de negative (1-2 stele).

### Pas 3: Sentiment — Rulează sentiment analysis cu AI
Trimite recenziile în batch-uri AI cu prompt: „Analizează aceste recenzii și pentru fiecare identifică: sentimentul general (pozitiv/neutru/negativ), emoția primară (mulțumire/frustrare/surpriză/dezamăgire), și motivul principal al sentimentului în max. 5 cuvinte." Agregă rezultatele: distribuția sentimentelor, emoțiile dominante.

### Pas 4: Themes — Extrage temele recurente
Cu toate recenziile, rulează AI pentru identificare teme: „Identifică cele mai frecvente 10 teme pozitive și 10 teme negative din aceste recenzii. Pentru fiecare temă: numele temei, frecvența aproximativă (%), 2-3 citate reprezentative, impactul perceput de clienți (minor/major)." Validează temele identificate prin re-citirea unui eșantion.

### Pas 5: Opportunities — Identifică oportunități de îmbunătățire
Pe baza temelor negative și a frecvenței lor, construiește cu AI o matrice de prioritizare: problemă | frecvență | severitate (impact client) | dificultate estimată de fix | prioritate (Înaltă/Medie/Scăzută). Evidențiază Quick Wins (frecvență mare + dificultate mică).

### Pas 6: Recommendations — Generează recomandări de produs și marketing
Produce două seturi de recomandări: (1) **Produs** — ce să schimbi/adaugi/elimini din produs, cu justificare în datele de recenzii; (2) **Marketing** — cum să amplifici temele pozitive în comunicare, cum să adresezi obiecțiile comune din recenzii negative în sales material.

---

## 3. Cortex Logging

```json
{
  "text": "Review Analysis AI executat pentru [produs/brand]. Recenzii analizate: [X] din [N] platforme. Sentiment: [%+/%0/%-]. Top temă negativă: [temă]. Top oportunitate Quick Win: [descriere].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-075",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["review-analysis", "sentiment-analysis", "product-optimization", "customer-feedback", "voc"],
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
La lansarea unui produs nou (post 30 zile) sau la analiza periodică de feedback (trimestrial)

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Analiza pe sub 50 recenzii fără documentarea limitării → violation
- Recomandări de produs fără legătură cu temele identificate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-marketing

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-075 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Review Analysis and Product Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Platforme de recenzii (Google/G2/Amazon) | Sursă de date |
| Tool de scraping sau export (opțional) | Colectare Pasul 1 |
| ChatGPT / Claude | Sentiment analysis + extragere teme |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Recenzii analizate | Min. 50 |
| Teme identificate | Min. 5 pozitive + 5 negative |
