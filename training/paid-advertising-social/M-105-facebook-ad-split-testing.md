---
type: procedure
created: 2026-03-22
status: active
slug: m-105-facebook-ad-split-testing
tags: [procedure, nexus]
---

# Facebook Ad Split Testing Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implementarea unui framework sistematic de split testing pentru reclame Facebook care produce rezultate statistice valide și decizii de optimizare bazate pe date reale.

---

## 1. Problema

Testarea ad-hoc și schimbările frecvente fără metodologie clară produc date confuze și decizii de optimizare greșite. Fără un framework de split testing structurat, nu poți diferenția între variabilele care influențează cu adevărat performanța și zgomotul statistic al campaniei.

Situații acoperite:
- Testare sistematică pentru identificarea celui mai bun creativ, audiență sau placement
- Validarea unui nou format de reclamă sau mesaj de marketing înainte de scalare
- Compararea a două strategii de targeting pentru a decide direcția campaniei

---

## 2. Procedura

### Pas 1: Definirea ipotezei și a unei singure variabile de testat
Formulează ipoteza testului în format clar: "Credem că [Variabila A] va performa mai bine decât [Variabila B] deoarece [raționament bazat pe date sau observații anterioare]." Testează O SINGURĂ variabilă per test: fie creative (imagine vs. video), fie audience (LAL 1% vs. LAL 3%), fie placement, fie copy, fie CTA. Nu testa multiple variabile simultan — invalidează rezultatele.

### Pas 2: Configurare test cu Facebook's A/B Test tool
Folosește funcția nativă A/B Test din Ads Manager (nu ad sets duplicate manual) pentru a asigura împărțirea echitabilă a traficului. Selectează metrica de victorie: Cost per Result (conversie) pentru campanii de performanță, CPM sau Reach per dollar pentru awareness, CTR pentru testarea creativelor izolat. Setează durata testului: minim 7 zile, maxim 30 zile, cu un buget suficient pentru a atinge semnificație statistică.

### Pas 3: Calculare buget și dimensiune sample pentru validitate statistică
Calculează bugetul necesar pentru semnificație statistică: ai nevoie de minim 100 de conversii per varianta testată (200 total pentru un test 2-way). Dacă CPA target este 50 RON, ai nevoie de minim 5.000 RON buget total per varianta. Folosește Facebook's Test and Learn tool pentru estimarea bugetului minim recomandat. Nu lansa un test cu buget insuficient — rezultatele vor fi neinformative.

### Pas 4: Creare variante de test cu diferențe clare și măsurabile
Creează variantele astfel încât diferența să fie clară și semnificativă: nu testa același copy cu culori ușor diferite. Exemple de teste cu impact real: imagine statică vs. video UGC, headline benefit-focused vs. headline curiosity-gap, audiență rece LAL vs. audiență de engagement, CTA "Shop Now" vs. "Learn More". Asigură-te că toate celelalte elemente sunt identice între variante.

### Pas 5: Lansare și monitorizare fără intervenție
Lansează testul și rezistă tentației de a face modificări sau de a opri varianta care pare a pierde înainte de finalizarea perioadei. Verifica zilnic că ambele variante rulează și cheltuie budget (dacă una nu primește livrare, există o problemă tehnică). Notează orice factori externi care ar putea influența rezultatele în perioada testului (reduceri, știri virale, sezonalitate).

### Pas 6: Analiza rezultatelor cu semnificație statistică
La finalul testului, verifică semnificația statistică indicată de Facebook (target: 95% confidence). Dacă testul nu a atins semnificație statistică, extinde durata sau crește bugetul — nu declara un câștigător prematur. Analizează nu doar metrica principală, ci și metrici secundare: frecvență, reach, engagement rate, pentru a înțelege context complet. Documentează câștigătorul, marja de diferență și intervalul de încredere.

### Pas 7: Implementare câștigător și planificare test următor
Implementează varianta câștigătoare în campaniile active. Documentează insight-ul obținut în Insight Log: ce ai testat, ce a câștigat, cu cât și de ce (ipoteza confirmată sau infirmată). Planifică imediat testul următor pe baza insight-ului obținut — testarea este un proces continuu, nu un eveniment singular. Target: minim un test finalizat pe lună per campanie majoră.

---

## 3. Cortex Logging

```json
{
  "text": "Framework split testing Facebook implementat — ipoteză clară cu variabilă unică, A/B Test nativ configurat, buget calculat pentru semnificație statistică, câștigător validat la 95% confidence, insight documentat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-105",
    "domain": "paid-advertising-social",
    "rule_id": "META-H-002",
    "tags": ["facebook", "split-testing", "A/B-test", "optimization", "statistical-significance", "creative-testing"],
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
- Multiple variabile testate simultan în același test → violation
- Câștigător declarat fără atingerea semnificației statistice de 95% → violation
- Test oprit prematur (sub 7 zile) fără motiv tehnic → violation
- Insight-ul testului nedocumentat → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-social

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] O singură variabilă testată per experiment?
- [ ] Semnificație statistică de 95% atinsă înainte de declararea câștigătorului?

**VK-uri obligatorii:**
1. `✅ [PROC] M-105 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Facebook Ad Split Testing Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Ads Manager A/B Test | Tool nativ pentru split testing echitabil |
| Facebook Test and Learn | Estimare buget și analiză semnificație |
| Insight Log (spreadsheet sau Notion) | Documentare teste și rezultate |
| Facebook Pixel | Tracking conversii pentru metrici de test |
| Campanie activă profitabilă | Baza pentru testare și iterare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Semnificație statistică | Minim 95% confidence |
| Conversii per varianta | Minim 100 |
| Frecvență teste per lună | Minim 1 per campanie majoră |
| Rata implementare câștigători | 100% (câștigători implementați în 48h) |
