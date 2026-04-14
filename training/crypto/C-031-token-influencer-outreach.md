---
type: procedure
created: 2026-03-22
status: active
slug: c-031-token-influencer-outreach
tags: [procedure, nexus]
---

# Influencer Outreach for Token Promotion — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Proces structurat pentru identificarea, evaluarea și contactarea influencerilor crypto pentru promovarea unui token, cu respectarea cerințelor legale de disclosure.

---

## 1. Problema

Promovarea unui token prin influenceri fără due diligence corespunzătoare poate duce la risipirea bugetului pe audiențe false, la asocierea brandului cu influenceri cu reputație negativă, sau la probleme legale din cauza lipsei de disclosure. O abordare structurată maximizează ROI-ul și protejează proiectul.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

> **Legal Note**: Plata influencerilor pentru promovarea unui token fără disclosure poate constitui manipulare de piață în unele jurisdicții. Asigurați-vă că influencerii declară relația comercială.

Situații acoperite:
- Campanie de outreach pre-lansare pentru generarea de buzz
- Parteneriate cu KOL-uri (Key Opinion Leaders) pentru lansarea tokenului
- Campanie post-lansare pentru creșterea awareness și holderi noi

---

## 2. Procedura

### Pas 1: Definește criteriile de selecție a influencerilor
Stabilește: nișa relevantă (DeFi, meme coins, altcoins, tech crypto), range de followeri (nano: 1K-10K, micro: 10K-100K, macro: 100K+), rata minimă de engagement acceptată, jurisdicțiile permise. Documentează criteriile înainte de a începe căutarea.

### Pas 2: Identifică și listează candidații
Folosește Twitter/X search, LunarCrush, sau agenții de influencer marketing crypto pentru a găsi candidați. Creează o spreadsheet cu: nume, platformă, followeri, engagement rate, nișa principală, contact. Listează minim 20 de candidați pentru a putea selecta 5-10 după filtrare.

### Pas 3: Execută due diligence pe audiență și engagement
Pentru fiecare candidat verifică: rata de engagement reală (likes+comments/followeri × 100 — minim 2% acceptabil), calitatea comentariilor (nu bot-like), istoricul promovărilor (au promovat scam-uri?), și reputația în comunitate. Folosește SparkToro sau HypeAuditor pentru analiză audiență.

### Pas 4: Verifică istoricul legal și reputațional
Caută pe Google: "[Influencer Name] scam", "[Influencer Name] rug pull", "[Influencer Name] lawsuit". Verifică dacă au disclosure-uri pe postările sponsorizate anterioare. Elimină din lista orice influencer cu red flags de manipulare de piață sau promovare de scam-uri.

### Pas 5: Pregătește pitch-ul și pachetul de colaborare
Creează un media kit al tokenului: whitepaper rezumat, tokenomics, roadmap, USP. Definește pachetele de colaborare: ce oferi (tokens/cash), ce aștepți (nr. postări, format, timing). Redactează emailul/DM de outreach personalizat — un template generic are rată de răspuns scăzută.

### Pas 6: Execută outreach și negocierea
Contactează primii 10 candidați calificați. Trackuiește în spreadsheet: data contact, răspuns (da/nu/negociere), termeni propuși, status deal. Negociază termenii cu atenție la: obligativitatea disclosure-ului ("#ad", "#sponsored", "#partnership"), livrabilele exacte, și clauze de exclusivitate.

### Pas 7: Formalizează acordul cu disclosure obligatoriu
Pentru fiecare influencer confirmat, semnează un acord scris (chiar și un email confirmat) care specifică explicit că toate postările trebuie să conțină disclosure clar al relației comerciale. Documentează acordurile și archivează. Salvează în Cortex lista finală de parteneri și termeni.

---

## 3. Cortex Logging

```json
{
  "text": "Outreach influenceri completat. Due diligence pe audiență/engagement executat, acorduri cu disclosure obligatoriu formalizate, parteneri documentați.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-031",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "influencer", "marketing", "kol", "token-launch", "compliance"],
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
- Outreach influencer fără due diligence pe audiență/engagement rate → violation
- Acord de promovare fără clauza de disclosure obligatoriu → violation critică
- Influencer selectat cu red flags de scam/rug pull nedocumentate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Due diligence pe engagement rate documentat pentru fiecare influencer?
- [ ] Clauza de disclosure în toate acordurile?

**VK-uri obligatorii:**
1. `✅ [PROC] C-031 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Influencer Outreach for Token Promotion" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| LunarCrush / HypeAuditor | Analiză audiență și engagement real |
| SparkToro | Due diligence audiență influencer |
| Google Search | Verificare reputație și istoric legal |
| Spreadsheet | Tracking outreach și negociere |
| Email / Twitter DM | Canal de contact |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Influenceri evaluați (due diligence) | Minim 20 |
| Rată de engagement minimă acceptată | 2% |
| Influenceri contactați | Minim 10 |
| Rată de conversie outreach | Minim 20% |
| Acorduri cu disclosure explicit | 100% din parteneri |
