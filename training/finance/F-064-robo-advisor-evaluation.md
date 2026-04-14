---
type: procedure
created: 2026-03-22
status: active
slug: f-064-robo-advisor-evaluation
tags: [procedure, nexus]
---

# Robo-Advisor Evaluation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea și selecția unui robo-advisor potrivit profilului individual al investitorului, comparând caracteristici cheie și cost efectiv pe termen lung.

---

## 1. Problema

Piața de robo-advisors oferă zeci de soluții cu structuri de fee, strategii de portofoliu și funcționalități diferite. Alegerea unui robo-advisor incompatibil cu profilul investitorului poate duce la subalimentarea obiectivelor financiare sau la costuri excesive care erodează randamentele. Această procedură acoperă: definirea profilului investitorului, compararea platformelor majore, evaluarea criteriilor cheie, și procesul de onboarding.


> **Disclaimer**: Această procedură are scop exclusiv educativ și de training. Nu constituie sfat de investiții. Performanțele trecute nu garantează rezultate viitoare. Consultați un profesionist licențiat înainte de a lua decizii de investiție.

---

## 2. Procedura

### Pas 1: Definirea profilului investitorului
Documentează parametrii personali: (1) Vârsta și orizontul de timp (până la pensionare sau obiectiv specific); (2) Obiectivul financiar (pensionare/cumpărare casă/fond educație/wealth building general); (3) Toleranța la risc (conservator/moderat/agresiv — testează cu simulatoare de pierdere: "Ești ok cu -30% pe piața bear?"); (4) Venitul lunar disponibil pentru investiție și suma inițială; (5) Nevoia de funcționalități specifice (tax-loss harvesting, SRI/ESG, acces la consilier uman).

### Pas 2: Compararea robo-advisors majori
Construiește tabelul comparativ pentru principalele platforme: **Betterment** (SUA: 0.25% AUM/an, min $0, tax-loss harvesting zilnic, portofoliu ETF diversificat global); **Wealthfront** (SUA: 0.25% AUM/an, min $500, tax-loss harvesting zilnic, direct indexing la >$100k); **Schwab Intelligent Portfolios** (SUA: 0% management fee, min $5000, dar cash drag ~6-10%); **Alternative europene**: Scalable Capital, Nutmeg, Moneyfarm (în funcție de jurisdicție). Adaugă alternative locale dacă sunt disponibile.

### Pas 3: Evaluarea structurii de fee-uri
Calculează costul anual total: `Fee total = % AUM fee + fund expense ratios (ETF-uri din portofoliu) + costuri ascunse`. Exemplu: Betterment 0.25% + ETF fees ~0.08% = ~0.33% total. Calculează impactul pe termen lung: `Cost 20 ani = Portofoliu final × (1 - (1-fee)^20)`. Compară robo vs DIY ETF portfolio: diferența de 0.2% pe an înseamnă mii de dolari pe 20 ani. Verifică dacă există fee-uri pentru retragere, transfer sau consiliere umană.

### Pas 4: Evaluarea calității portofoliului și rebalansării
Analizează: tipurile de ETF-uri folosite (iShares/Vanguard/Schwab = calitate), diversificarea geografică (doar SUA sau global?), frecvența rebalansării automată (zilnică, lunară, threshold-based), strategia de alocare (MPT standard, factor investing, smart beta), disponibilitatea portfoliilor personalizate sau ESG/SRI. Compară alocarea propusă cu un benchmark similar (ex: 60/40 global).

### Pas 5: Evaluarea tax-loss harvesting
Pentru conturi impozabile, tax-loss harvesting (TLH) poate adăuga 0.5-1.5% randament anual după taxe. Verifică: frecvența TLH (zilnică = mai bună), dacă folosesc "direct indexing" pentru TLH mai granular, regulile wash sale compliance, raportarea fiscală automatizată (Form 8949 în SUA, declarație anuală în România). Calculează valoarea estimată TLH pe baza portofoliului tău.

### Pas 6: Compararea randamentelor proiectate după fee-uri
Folosește calculatorul de pe fiecare platformă sau un spreadsheet propriu: suma inițială × (1 + randament net anual)^ani. Ipoteze standard: randament brut piață 7%, inflație 2.5%, fee variabil per platformă. Compară rezultatele proiectate pentru 10/20/30 ani. Identifică platforma cu cel mai bun randament net ajustat la profilul de risc ales.

### Pas 7: Selecție și onboarding
Selectează platforma câștigătoare și parcurge onboarding-ul: completează chestionarul de risc (răspunde sincer!), conectează contul bancar, transferă suma inițială, configurează investiția automată lunară (direct debit), revizuiește alocarea propusă și confirmă. Setează alerte pentru: rapoarte trimestriale, modificări majore ale portofoliului, și newsletter-ul de market updates al platformei.

---

## 3. Cortex Logging

```json
{
  "text": "Robo-Advisor Evaluation F-064 executat: profil investitor definit, platforme comparate (Betterment/Wealthfront/Schwab), fee-uri calculate, TLH evaluat, selectie si onboarding completat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-064",
    "domain": "finance",
    "rule_id": "META-H-002",
    "tags": ["robo-advisor", "automated-investing", "Betterment", "Wealthfront", "tax-loss-harvesting", "passive-investing"],
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

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum Finance

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] F-064 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Robo-Advisor Evaluation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Betterment / Wealthfront / Schwab websites | Informații oficiale fee-uri și funcționalități |
| Compound interest calculator | Proiecții randamente comparative |
| Chestionar profil de risc | Definirea toleranței la risc |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Fee total anual | < 0.5% AUM total (management + ETF fees) |
| Timp până la onboarding complet | < 1 săptămână de la decizie |
