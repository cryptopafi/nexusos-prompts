---
type: procedure
created: 2026-03-22
status: active
slug: c-003-diversified-crypto-portfolio
tags: [procedure, nexus]
---

# Build a Diversified Crypto Portfolio — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui portofoliu crypto diversificat pe baza categoriilor de risc, corelații inter-asset, on-chain metrics și rebalansare sistematică cu implicații fiscale românești.

---

## 1. Problema

Majoritatea investitorilor crypto concentrează 80%+ din capital într-un singur activ sau în active puternic corelate (ex: toate Layer 1s), eliminând beneficiul diversificării. Un portofoliu nestructurat amplifică drawdown-urile: în bear market 2022, portofoliile 100% altcoins au pierdut 85-95%, în timp ce portofoliile diversificate (BTC+stables+DeFi yields) au limitat pierderea la 30-50%. Fără rebalansare sistematică, winner-urile devin overweight și pierderile se cascadează.

Situații acoperite:
- Construirea unui portofoliu de la zero cu alocare pe categorii de risc
- Rebalansarea trimestrială bazată pe corelații și on-chain metrics
- Integrarea yield-urilor DeFi și staking ca sursă de venit pasiv
- Documentare completă pentru fiscalitate România (10% pe câștiguri)

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Definește profilul de risc și obiectivele
Stabilește: (a) orizont de investiție (1 an, 3-5 ani, 10+ ani), (b) toleranță la drawdown maxim acceptabil (20%, 40%, 60%?), (c) obiectiv de return anual realist (20-50% crypto, nu 1000%). Categorii de risc crypto: **Tier 1 (blue chips)**: BTC, ETH — corelație cu piața generală, volatilitate mai mică. **Tier 2 (large caps)**: SOL, AVAX, DOT, LINK — risc mediu, potențial 2-5x. **Tier 3 (mid/small caps)**: tokeni sub $1B market cap — risc înalt, potențial 5-50x dar și -95%.

### Pas 2: Aplică framework-ul de alocare pe categorii
Model conservator: 60% Tier 1 (40% BTC + 20% ETH), 25% Tier 2, 10% Tier 3, 5% stablecoins (cash buffer). Model moderat: 40% Tier 1, 30% Tier 2, 20% Tier 3, 10% stablecoins. Model agresiv: 25% Tier 1, 35% Tier 2, 30% Tier 3, 10% stablecoins. Regula de aur: niciun singur activ >30% din portofoliu (exceptie BTC la profil conservator). Diversifică și pe narratives: DeFi (AAVE, UNI, CRV), Layer 2 (ARB, OP), AI (RNDR, FET), RWA (ONDO).

### Pas 3: Analizează corelațiile inter-asset
Pe IntoTheBlock sau Coin Metrics, verifică corelația Pearson pe 90 de zile între activele selectate. BTC-ETH corelație ~0.85 (diversificare minimală). BTC-LINK ~0.65. BTC-stablecoins ~0 (diversificator perfect). Țintă: cel puțin 30% din portofoliu în active cu corelație <0.5 față de BTC. Verifică pe Dune Analytics: „Crypto Correlations Dashboard" pentru date actualizate. Evită concentrarea în active cu corelație >0.8 între ele.

### Pas 4: Validează fiecare activ cu on-chain metrics
Înainte de includere, verifică pe Glassnode/Nansen: (a) **NVT Ratio** (Network Value to Transactions) — sub 30 = undervalued, peste 100 = overvalued; (b) **Active Addresses** trend — creștere = adopție reală; (c) **Exchange Reserves** — scădere = acumulare (bullish); (d) **Supply Distribution** — verifică concentrarea la balene pe Etherscan holders tab. Pe DefiLlama: TVL trend pentru DeFi tokens — TVL crescând constant = protocol sănătos. Exclud orice token cu TVL în scădere >30% în ultimele 3 luni fără explicație tehnică.

### Pas 5: Implementează DCA (Dollar Cost Averaging) sistematic
Nu investi tot capitalul dintr-o dată (lump sum) — DCA pe 3-6 luni reduce riscul de timing. Configurare: Binance Auto-Invest pentru Tier 1 (săptămânal), ordine manuale pentru Tier 2-3 (la niveluri de suport tehnic). Alternativă: 3Commas sau Shrimpy pentru DCA automatizat cross-exchange. Documentează fiecare achiziție cu preț mediu per activ (cost basis) pentru fiscalitate.

### Pas 6: Integrează yield sources (staking + DeFi)
Alocă o parte din Tier 1 și Tier 2 în yield: (a) ETH staking via Lido (stETH, ~3-4% APY) sau native staking (32 ETH minim); (b) Stablecoins pe Aave v3 (3-8% APY variabil); (c) Liquidity provision pe Curve (stablecoin pools, 5-15% APY cu CRV rewards). Atenție la impermanent loss pe pools volatile (vezi C-028). **Yield farming risk**: verifică pe DefiLlama APY sustainability — yield >50% APY este nesustenabil pe termen lung (emissions dilute). Preferă protocoale cu revenue real (fee-based yield).

### Pas 7: Rebalansează trimestrial cu disciplină
La fiecare 3 luni: (a) compară alocarea actuală vs target, (b) dacă un activ depășește target cu >5 puncte procentuale, vinde excesul, (c) dacă este sub target cu >5pp, cumpără diferența. Rebalansarea forțează „buy low, sell high" sistematic. Folosește un spreadsheet sau Koinly/CoinTracking pentru tracking. **Fiscalitate**: fiecare vânzare la rebalansare este eveniment taxabil în România (10% pe câștigul net). Calculează impactul fiscal ÎNAINTE de rebalansare.

### Pas 8: Monitorizează semnale macro și on-chain
Setează alerte pe Glassnode pentru: (a) Exchange Net Flow (intrări mari = sell pressure), (b) NUPL (Net Unrealized Profit/Loss) — >0.75 = euphoria, consider profit-taking, (c) MVRV Z-Score >7 = top potențial. Pe CryptoQuant: verifică Puell Multiple și Reserve Risk. Macro: urmărește Fed rate decisions, DXY (dolar index), US 10Y Treasury yield. Crypto-ul este corelat invers cu DXY (~-0.6 pe 90 zile).

### Pas 9: Documentează și raportează performanța
Ține un performance tracker cu: (a) valoare totală portofoliu lunar, (b) return vs BTC benchmark (alpha), (c) Sharpe Ratio estimat, (d) max drawdown realizat. Exportă pentru Declarația Unică (formular 212): câștiguri realizate anual, cost basis per activ, comisioane deductibile. Deadline: 25 mai anul următor. Păstrează exporturile de pe exchange minim 5 ani.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-003: Diversified Crypto Portfolio — alocare pe tiers definită, corelații verificate, on-chain metrics validate, DCA activ, yield sources integrate, rebalansare planificată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-003",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "portfolio", "diversification", "rebalancing", "on-chain", "defi-yield", "dca", "fiscalitate-ro"],
    "has_enforcement_loop": true,
    "forge_version": "2.0"
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
- Portofoliu cu >50% într-un singur activ fără justificare → violation critică
- Rebalansare efectuată fără calcul fiscal prealabil → violation
- Active incluse fără verificare on-chain metrics → violation
- Lipsă documentare cost basis → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto
- C-001 → prerequisit (cont exchange)
- C-002 → stop-loss pe fiecare poziție
- C-004 → research altcoins pentru Tier 2-3
- C-013 → staking passive income

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Alocare conformă cu profilul de risc definit?
- [ ] Corelații inter-asset verificate?
- [ ] On-chain metrics validate pentru fiecare activ?
- [ ] Plan de rebalansare documentat?
- [ ] Implicații fiscale calculate?

**VK-uri obligatorii:**
1. `✅ [PROC] C-003 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Build a Diversified Crypto Portfolio" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Binance / Kraken / OKX | Exchange-uri principale |
| Glassnode | On-chain metrics (NVT, NUPL, MVRV) |
| Nansen | Whale tracking, smart money flows |
| DefiLlama | TVL, yields, protocol health |
| Dune Analytics | Correlation dashboards, custom queries |
| Koinly / CoinTracking | Portfolio tracking + fiscalitate |
| Aave / Curve / Lido | Yield sources DeFi |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Concentrare max per activ | ≤30% |
| Corelație medie portofoliu | <0.7 |
| Rebalansare frecvență | Trimestrială |
| Documentare fiscală | 100% tranzacții |
| Alpha vs BTC benchmark | Pozitiv pe 12 luni |
