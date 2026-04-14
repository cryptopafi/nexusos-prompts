---
type: procedure
created: 2026-03-22
status: active
slug: m-074-ai-competitor-analysis
tags: [procedure, nexus]
---

# AI-Powered Competitor Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Analiza sistematică a peisajului competitiv folosind AI pentru a extrage positioning, gaps de conținut și oportunități strategice de diferențiere.

---

## 1. Problema

Analiza competitivă realizată manual este lentă, incompletă și subiectivă. AI-ul poate accelera dramatic procesul de colectare și sinteză, dar fără o metodologie structurată produce output superficial și neacționabil. Această procedură combină rigoarea analizei strategice cu eficiența AI pentru competitive intelligence de calitate.

Situații acoperite:
- Intrarea pe o nouă piață sau lansarea unui produs nou
- Revizuirea periodică a poziționării (trimestrial/semestrial)
- Răspuns la un concurent nou intrat pe piață
- Fundamentarea strategiei de conținut și SEO pe baza gap-urilor identificate

---

## 2. Procedura

### Pas 1: Define — Mapează peisajul competitiv
Identifică 3-5 competitori direcți (același produs/piață) și 2-3 competitori indirecți (soluții alternative). Pentru fiecare colectează: URL, categorie de produs, piața țintă estimată. Justifică includerea fiecărui competitor în analiză.

### Pas 2: Collect — Colectează date despre fiecare competitor
Pentru fiecare competitor, extrage: homepage copy + tagline, paginile de pricing (structura planurilor), pagina About (story, valori, echipă), top 5 pagini după trafic (folosind SimilarWeb/Ahrefs), recenzii recente (G2/Capterra/Trustpilot), conținut recent de blog sau social media.

### Pas 3: Analyze — Aplică AI pe datele colectate
Folosește AI (ChatGPT/Claude) cu prompt structurat pentru fiecare competitor: „Analizează acest conținut și extrage: (1) Propunerea de valoare principală, (2) ICP-ul țintit explicit sau implicit, (3) Mesajele cheie de marketing, (4) Punctele de preț și structura de pricing, (5) Punctele forte și slabe aparente din positioning." Rulează pentru fiecare competitor separat.

### Pas 4: Gaps — Identifică gap-urile de conținut și positioning
Compară cu ajutorul AI output-urile din Pasul 3: ce probleme ale clienților nu sunt adresate de niciunul? Ce segmente de audiență sunt ignorate? Ce formate de conținut lipsesc (cazuri de studiu, ghiduri, comparatoare)? Ce unghiuri de messaging nu sunt folosite?

### Pas 5: SWOT — Construiește SWOT-ul competitiv cu AI
Cu toate datele colectate, solicită AI să genereze un SWOT comparativ: Strengths ale competitorilor (ce fac bine), Weaknesses (ce le lipsește sau fac prost), Opportunities pentru tine (gap-uri pe care le poți acoperi), Threats (ce risc reprezintă pentru tine). Validează fiecare punct față de datele din Pasul 2.

### Pas 6: Recommendations — Generează recomandări strategice de diferențiere
Pe baza SWOT, solicită AI 5-7 recomandări specifice și acționabile: schimbări de positioning, oportunități de conținut, ajustări de pricing sau packaging, mesaje de diferențiere de testat. Fiecare recomandare trebuie să specifice: ce să faci, de ce (legat de un gap/weakness competitor), și metrica de succes.

---

## 3. Cortex Logging

```json
{
  "text": "Competitor Analysis AI executat pentru [industrie/produs]. Competitori analizați: [X]. Gap-uri identificate: [Y]. Top recomandare: [descriere scurtă].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-074",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["competitor-analysis", "competitive-intelligence", "swot", "positioning", "content-gaps"],
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
La fiecare analiză competitivă sau revizuire de strategie de marketing

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- SWOT generat fără validarea față de date reale (Pasul 5) → violation
- Recomandări fără legătură explicită cu gap-urile identificate (Pasul 6) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-marketing

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-074 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI-Powered Competitor Analysis" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| SimilarWeb / Ahrefs / SEMrush | Date trafic și top pagini competitor |
| G2 / Capterra / Trustpilot | Recenzii pentru sentiment analysis |
| ChatGPT / Claude | Analiză și sinteză SWOT |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Competitori analizați | Min. 3 direcți |
| Recomandări acționabile generate | Min. 5 |
