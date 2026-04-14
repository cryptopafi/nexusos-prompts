---
type: procedure
created: 2026-03-22
status: active
slug: c-007-stablecoin-risk-selection
tags: [procedure, nexus]
---

# Evaluate Stablecoin Risk and Select a Stablecoin — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea riscurilor asociate diferitelor tipuri de stablecoins și selectarea celei mai potrivite pentru scopul utilizatorului.

---

## 1. Problema

Nu toate stablecoin-urile sunt egale — diferă fundamental prin mecanism de backing, risc de depegging, contrapartidă centralizată și utilitate DeFi. Selectarea greșită poate duce la pierderi majore (ex: colapsul UST/LUNA în 2022).

Situații acoperite:
- Selectarea unui stablecoin pentru holding pe termen mediu/lung
- Compararea stablecoins pentru utilizare în DeFi lending sau yield farming
- Evaluarea riscului unui stablecoin existent în portofoliu

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Identifică tipul de stablecoin
Clasifică stablecoin-ul în una din categoriile: Fiat-backed centralizat (USDC, USDT, BUSD), Crypto-collateralized descentralizat (DAI, crvUSD), Algorithmic (fostul UST — risc extrem, de evitat), Commodity-backed (XAUT — gold). Fiecare categorie are profil de risc distinct.

### Pas 2: Evaluează backing-ul și rezervele
Pentru fiat-backed: caută rapoartele de audit ale rezervelor (Circle pentru USDC publică lunar, Tether — USDT publică trimestrial). Verifică că rezervele acoperă 100%+ din supply circulant. Lipsa auditurilor transparente = risc ridicat (ex: USDT history).

### Pas 3: Verifică riscul de contrapartidă și reglementare
USDC și USDT sunt emise de companii centralizate care pot îngheța wallets la cerere legală. Evaluează: ești în jurisdicție cu restricții crypto? Ai nevoie de stablecoin uncensorable (DAI)? Scopul influențează alegerea.

### Pas 4: Analizează istoricul de depeg
Verifică pe CoinGecko sau TradingView istoricul prețului stablecoin-ului față de $1. Un depeg temporar minor (<1%) în momente de stres este normal. Depeg-uri >5% sau recurente indică probleme structurale — evită sau reduce expunerea.

### Pas 5: Evaluează lichiditatea și utilizarea DeFi
Verifică volumul zilnic de tranzacționare și TVL în protocoale DeFi majore (Aave, Compound, Curve). Stablecoins cu lichiditate mare = exit rapid în criză. USDC și USDT au cea mai mare lichiditate; DAI are utilizare DeFi largă.

### Pas 6: Calculează concentrarea maximă acceptabilă
Nu concentra >50% din cash-ul crypto într-un singur stablecoin. O strategie sănătoasă: 50% USDC (transparent, reglementat), 30% USDT (lichiditate maximă), 20% DAI (descentralizat, DeFi). Ajustează în funcție de nevoi.

### Pas 7: Selectează și documentează alegerea
Documentează alegerea finală cu rațiunea: scop (holding, DeFi, trading buffer), risc acceptat, rețeaua preferată (ETH vs Polygon vs Arbitrum pentru costuri mici). Revizuiește alegerea trimestrial sau la schimbări majore de piață.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-007: Stablecoin Risk Evaluation — tipuri identificate, backing verificat, depeg history analizat, selecție documentată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-007",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "stablecoin", "risk-management", "defi", "personal-finance"],
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
- Selectarea unui stablecoin algorithmic fără avertizare despre risc extrem → violation critică
- Concentrare >80% din cash-ul crypto într-un singur stablecoin → violation
- Ignorarea istoricului de depeg în procesul de selecție → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Tipul de stablecoin și mecanismul de backing înțeles?
- [ ] Istoric de depeg verificat pe CoinGecko/TradingView?

**VK-uri obligatorii:**
1. `✅ [PROC] C-007 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Evaluate Stablecoin Risk and Select a Stablecoin" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CoinGecko / CoinMarketCap | Istoric preț și depeg monitoring |
| Circle (centre.io) | Rapoarte rezerve USDC |
| Tether (tether.to) | Rapoarte rezerve USDT |
| DeFiLlama | TVL și utilizare DeFi per stablecoin |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Concentrare max per stablecoin | 50% din cash-ul crypto |
| Raport backing/supply verificat | 100% pentru fiat-backed selectat |
