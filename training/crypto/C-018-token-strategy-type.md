---
type: procedure
created: 2026-03-22
status: active
slug: c-018-token-strategy-type
tags: [procedure, nexus]
---

# Define Your Token Strategy and Type — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Cadru metodic pentru definirea strategiei unui token crypto, alegerea tipului potrivit și alinierea cu obiectivele proiectului și cerințele legale.

---

## 1. Problema

Lansarea unui token fără o strategie clară este una dintre principalele cauze de eșec în proiecte crypto — tokenul nu are utilitate reală, nu este aliniat cu modelul de business sau ridică probleme legale (clasificare ca security). Definirea corectă a tipului de token și a strategiei înainte de orice implementare tehnică previne reluarea completă a arhitecturii și potențiale probleme cu autorități de reglementare.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Alegerea între utility token, governance token, security token sau token hibrid
- Definirea value proposition-ului tokenului în ecosistemul proiectului
- Evaluarea implicațiilor legale și de conformitate ale tipului ales

---

## 2. Procedura

### Pas 1: Definiți problema pe care o rezolvă tokenul
Articulați în maximum 2 propoziții de ce proiectul are nevoie de un token — dacă nu puteți răspunde clar, tokenul nu este necesar. Răspundeți la: Ce face tokenul imposibil de replcat fără blockchain? Cine sunt utilizatorii și de ce vor deține tokenul? Documentați răspunsurile în Product Requirements Document (PRD) de token.

### Pas 2: Alegeți tipul de token potrivit
Evaluați cele 4 categorii principale: (1) Utility Token — acces la servicii/funcționalități ale platformei; (2) Governance Token — drept de vot în decizii de protocol (DAO); (3) Payment Token — mediu de schimb în ecosistem; (4) Security Token — reprezentare de active reale cu drepturi financiare. Analizați combinații hibride (ex: governance + utility). Documentați alegerea cu justificare.

### Pas 3: Analizați implicațiile legale (Howey Test)
Consultați Howey Test pentru a evalua dacă tokenul ar putea fi clasificat ca security în SUA: (1) Investiție de bani; (2) Într-o întreprindere comună; (3) Cu așteptarea de profit; (4) Din eforturile altora. Dacă tokenul trece Howey Test, consultați un avocat specializat în crypto înainte de lansare. Analizați și reglementările din jurisdicțiile principale unde vor fi utilizatori (UE MiCA, SUA SEC, Singapore MAS).

### Pas 4: Definiți token utility și use cases concrete
Listați minimum 3 use cases concrete în care tokenul este utilizat în ecosistem: acces la features premium, staking pentru rewards, governance voting, fee discounts, etc. Fiecare use case trebuie să răspundă la "De ce utilizatorul nu ar putea face asta fără token?" Eliminați use cases artificiale sau care pot fi înlocuite cu monedă fiat. Validați use cases cu potențiali utilizatori.

### Pas 5: Definiți relația token-protocol (token economics basics)
Stabiliți cum tokenul capturează valoare din activitatea protocolului: fee sharing, buyback & burn, staking rewards, sau accces exclusiv. Definiți mecanismele de cerere (demand drivers) și ofertă (supply mechanics). Analizați sustenabilitatea pe termen lung — sursa de venituri pentru rewards trebuie să fie activitate reală, nu inflație perpetuă. Documentați în Token Economics Framework v0.1.

### Pas 6: Evaluați competiția și poziționarea tokenului
Identificați minimum 5 tokenuri similare pe piață și analizați: market cap, utilitate, tokenomics, traction. Definiți diferențiatorii cheie ai tokenului vostru. Analizați dacă piața are nevoie de încă un token similar sau dacă există un gap neacoperit. Documentați competitive analysis într-un tabel comparativ: Token Name, Type, Utility, Market Cap, Key Differentiator.

### Pas 7: Documentați Token Strategy Document v1.0
Compilați toate outputs-urile din pașii anteriori în Token Strategy Document: Token Name & Symbol, Token Type, Problem Solved, Utility Use Cases (minimum 3), Legal Classification Assessment, Value Capture Mechanism, Competitive Differentiation, Next Steps (Tokenomics Design → C-021). Revizuiți documentul cu echipa și obțineți sign-off înainte de a proceda la design tehnic.

---

## 3. Cortex Logging

```json
{
  "text": "Token strategy definită: tip token ales, use cases documentate, implicații legale evaluate, Token Strategy Document v1.0 completat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-018",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "token-launch", "token-strategy", "tokenomics", "legal", "utility-token", "governance"],
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
- Token strategy definită fără evaluare legală (Howey Test) → violation
- Use cases documentate fără validare cu utilizatori reali → violation
- Token Strategy Document necompilat înainte de trecerea la design tehnic → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Token Strategy Document v1.0 completat și revizuit?
- [ ] Minimum 3 use cases concrete documentate?

**VK-uri obligatorii:**
1. `✅ [PROC] C-018 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Define Your Token Strategy and Type" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Token Strategy Document | Output principal al procedurii |
| Howey Test Framework | Evaluare clasificare legală token |
| Competitive Analysis Template | Comparație cu tokenuri similare |
| PRD Token | Requirements document pentru token |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Use cases documentate | Minimum 3 concrete și validate |
| Legal assessment | Obligatoriu înainte de lansare |
