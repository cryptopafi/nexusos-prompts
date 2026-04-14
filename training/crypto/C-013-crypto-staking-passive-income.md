---
type: procedure
created: 2026-03-22
status: active
slug: c-013-crypto-staking-passive-income
tags: [procedure, nexus]
---

# Stake Cryptocurrency for Passive Income — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Evaluarea și utilizarea staking-ului crypto ca metodă de generare de venit pasiv, cu înțelegerea riscurilor asociate.

---

## 1. Problema

Staking-ul poate genera randamente atractive, dar implică riscuri specifice: perioadele de lock-up, slashing pentru validatori, riscul de smart contract și impozitarea recompenselor. Fără înțelegerea acestor riscuri, utilizatorii pot pierde capitalul sau pot avea surprize fiscale neplăcute.

Situații acoperite:
- Evaluarea opțiunilor de staking pentru un asset crypto specific (ETH, SOL, ATOM etc.)
- Alegerea între staking nativ, liquid staking și staking pe exchange
- Calcularea randamentului real net după taxe și riscuri

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Identifică tipul de staking disponibil pentru asset
Clasifică opțiunile: Staking nativ (ex: ETH solo validator — 32 ETH minim, SOL via Phantom), Liquid staking (stETH prin Lido, rETH prin RocketPool), Staking pe CEX (Binance Earn, Coinbase), Delegated staking (ATOM, DOT prin wallet). Fiecare tip are profil diferit de risc și randament.

### Pas 2: Compară APY-ul real față de APY-ul afișat
APY-ul afișat este nominal — APY-ul real este mai mic după: inflația tokenului (dacă toată lumea stake, APY-ul real scade), penalitățile de slashing (riscul validatorilor), fee-urile platformei, impozitul pe recompense. Caută date pe StakingRewards.com pentru comparații reale.

### Pas 3: Evaluează perioada de lock-up și lichiditate
Verifică: există perioadă de unbonding (ex: ATOM 21 zile, DOT 28 zile)? Poți accesa fondurile în urgență? Liquid staking (stETH) oferă lichiditate imediată dar introduce risc de smart contract. CEX staking oferă flexibilitate dar contrapartidă centralizată.

### Pas 4: Evaluează riscul de slashing (pentru validatori)
Slashing elimină o parte din stake dacă validatorul se comportă incorect (double signing, downtime excesiv). La liquid staking prin Lido sau RocketPool, riscul este distribuit și acoperit parțial de fonduri de asigurare. Verifică istoricul de slashing al validatorului/protocolului ales.

### Pas 5: Calculează randamentul net și implicațiile fiscale
Recompensele de staking sunt venit impozabil la momentul primirii în România și majoritatea UE. Calculează: APY brut - fee platformă - impozit estimat = randament net real. Sub 3% net real după impozit și risc = randament slab pentru riscul asumat.

### Pas 6: Execută staking-ul pe platforma aleasă
Urmează procesul specific platformei (ex: conectează wallet pe Lido.fi, aprobă tranzacția, primești stETH). Verifică că ai primit token-ul de staking (receipt token) sau că stake-ul apare confirmat. Notează data, suma și APY-ul promis la momentul intrării.

### Pas 7: Monitorizează și documentează recompensele
Urmărește recompensele primite lunar — exportă din platformă sau tool de tracking (DeFiLlama, Zapper). Documentează fiecare recompensă cu valoarea în EUR la data primirii pentru declararea fiscală. Revizuiește periodic dacă APY-ul rămâne competitiv față de alternative.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-013: Crypto Staking Passive Income — tip staking ales, APY real calculat, lock-up evaluat, staking executat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-013",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "defi", "staking", "passive-income", "yield", "liquid-staking"],
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
- APY prezentat fără calcularea randamentului real net după taxe → violation
- Staking fără verificarea perioadei de lock-up și implicațiile de lichiditate → violation
- Recompense staking nedocumentate pentru declarare fiscală → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto
- C-008 → implicații fiscale ale recompenselor staking

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] APY real net calculat (nu doar APY nominal afișat)?
- [ ] Perioada de lock-up și implicațiile de lichiditate înțelese?

**VK-uri obligatorii:**
1. `✅ [PROC] C-013 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Stake Cryptocurrency for Passive Income" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| StakingRewards.com | Comparare APY real între platforme |
| Lido / RocketPool | Liquid staking ETH |
| DeFiLlama / Zapper | Tracking recompense staking |
| C-008 | Tratament fiscal al recompenselor |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| APY real net calculat înainte de intrare | 100% cazuri |
| Recompense documentate pentru fiscal | Lunar |
