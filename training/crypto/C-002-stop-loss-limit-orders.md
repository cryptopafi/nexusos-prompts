---
type: procedure
created: 2026-03-22
status: active
slug: c-002-stop-loss-limit-orders
tags: [procedure, nexus]
---

# Set Up Stop-Loss and Limit Orders — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea și utilizarea avansată a ordinelor stop-loss, limit, OCO și trailing stop pe exchange-uri centralizate, cu strategii de position sizing bazate pe Kelly Criterion și ATR.

---

## 1. Problema

Traderii fără ordine de protecție pierd în medie 2-5x mai mult în drawdown-uri decât cei cu stop-loss-uri sistematice. Fără o strategie clară de exit, deciziile sunt emoționale — „HODL" devine „bag holding" la -80%. Ordinele automate elimină factorul emoțional și protejează capitalul în crash-uri unde lichiditatea dispare în secunde (vezi FTX collapse, LUNA de-peg). Fee-urile de funding rate pe perpetuals pot consuma 0.01-0.03% la fiecare 8h, transformând un trade profitabil într-o pierdere pe termen lung.

Situații acoperite:
- Setarea stop-loss-urilor pe pozițiile spot și futures
- Utilizarea OCO (One-Cancels-Other) pentru profit-taking + stop-loss simultan
- Trailing stop pentru maximizarea profitului în trending markets
- Position sizing bazat pe volatilitate (ATR) și Kelly Criterion
- Gestionarea slippage-ului în piețe cu lichiditate scăzută

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Definește riscul maxim per trade (Position Sizing)
Regula de bază: nu risca mai mult de 1-2% din portofoliu pe un singur trade. Calculul: dacă portofoliul = 10,000 EUR și riscul = 1%, pierderea maximă acceptabilă = 100 EUR. **Kelly Criterion simplificat**: Kelly % = (Win Rate × Avg Win / Avg Loss - Loss Rate) / (Avg Win / Avg Loss). Folosește Half-Kelly (50% din valoarea calculată) pentru conservatorism. Exemplu: win rate 55%, avg win/loss ratio 1.5:1 → Kelly = (0.55 × 1.5 - 0.45) / 1.5 = 0.25 → Half-Kelly = 12.5% din capital per trade.

### Pas 2: Calculează nivelul stop-loss pe baza ATR (Average True Range)
Pe TradingView, aplică indicatorul ATR(14) pe timeframe-ul de trading. Stop-loss recomandat: 1.5-2x ATR sub prețul de intrare (long) sau deasupra (short). Exemplu BTC: ATR zilnic = $1,500, intrarea = $65,000 → stop-loss = $65,000 - (2 × $1,500) = $62,000. Aceasta elimină noise-ul și previne stop-hunting pe mișcări normale. Verifică și Bollinger Bands — stop sub banda inferioară oferă confirmare suplimentară.

### Pas 3: Configurează Stop-Loss Order pe exchange
**Binance Spot**: Orders → Stop-Limit → setează Stop Price (trigger) și Limit Price (0.5-1% sub trigger pentru fill). **Kraken**: Orders → Conditional Close → Stop Loss. **OKX**: Trade → Stop-Limit Order. **Bybit**: Trade → Conditional → Stop Loss. IMPORTANT: Stop-Market garantează execuția dar nu prețul (slippage în flash crash); Stop-Limit garantează prețul minim dar riscă non-fill. Pentru sume >$10K: folosește Stop-Limit cu buffer de 1%.

### Pas 4: Setează Take-Profit cu ratio risc/recompensă minim 1:2
Dacă stop-loss = 5% sub intrare, take-profit minim = 10% deasupra (R:R 1:2). Strategie de scaling out: 50% la TP1 (1:2), 30% la TP2 (1:3), 20% trail cu trailing stop. Pe Binance: folosește **OCO** (One-Cancels-Other) — combină take-profit limit + stop-loss. Pe OKX: TP/SL Order type nativ. Verifică pe order book că există suficientă lichiditate la nivelul TP.

### Pas 5: Configurează Trailing Stop pentru trend-following
**Binance Futures**: Trailing Stop → Callback Rate 3-5% (day trading), 8-15% (swing). **Bybit**: Trailing Stop pe Derivatives. **Kraken**: Trailing Stop Order nativ. Trailing stop-ul se mișcă doar în direcția favorabilă. Combinație avansată: trailing stop activat doar după ce prețul depășește TP1 — astfel asiguri profit minim și lași upside-ul să curgă.

### Pas 6: Gestionează slippage-ul și lichiditatea
Verifică order book depth pe exchange înainte de ordine mari. Pe Binance/OKX: verifică bid/ask spread și depth la nivelul stop-loss. Dacă spread >0.5% sau depth <$50K: (a) fragmentează ordinul, (b) folosește Stop-Limit, (c) alege exchange cu lichiditate mai mare. **On-chain signals**: Nansen pentru whale movements, Glassnode pentru exchange net flows (intrări masive = presiune vânzare), Dune Analytics pentru monitorizare DeFi liquidations aproape de nivelul tău.

### Pas 7: Monitorizează funding rates pe perpetuals
Pe Binance/Bybit/OKX Futures: verifică funding rate la fiecare 8h. Funding pozitiv >0.05% = long-urile plătesc short-urile (piață overheated, consider short). Funding negativ = short-urile plătesc (piață oversold). Coinglass.com pentru heatmap funding rates cross-exchange. Open Interest crescând + funding ridicat = potențial squeeze. Ajustează stop-loss-urile mai strâns când funding rate depășește ±0.1%.

### Pas 8: Monitorizează ordinele și adaptează
Verifică zilnic că ordinele sunt active (exchange-urile le anulează la maintenance/delisting). Nu muta stop-loss-ul în direcția defavorabilă (widening = cauza #1 blow-up). Folosește TradingView price alerts ca backup. După execuție: verifică fill price vs set price, notează slippage. Ajustează stop-ul doar în sus (long) sau jos (short) — trail manual la fiecare higher low/lower high confirmat.

### Pas 9: Analizează execution quality și ține jurnal
Post-trade: (a) verifică slippage real, (b) notează dacă piața a continuat în direcția stop-ului (stop hunt vs mișcare reală), (c) calculează P&L net inclusiv fee-uri + funding. Jurnal: data, pereche, direcție, intrare, stop, TP, rezultat, R multiplu, lecții. **Fiscalitate România**: fiecare execuție = eveniment taxabil. Exportă trade history lunar. Impozit 10% pe câștig net (preț vânzare - preț cumpărare - comisioane). Revizuiește jurnalul lunar pentru optimizare.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-002: Stop-Loss & Limit Orders — position sizing calculat (Kelly/ATR), stop-loss + TP + OCO configurate, trailing stop activ, slippage evaluat, funding rates monitorizate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-002",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "trading", "stop-loss", "risk-management", "position-sizing", "kelly-criterion", "atr", "oco", "trailing-stop", "funding-rates"],
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
- Trade deschis fără stop-loss configurat → violation critică
- Risc per trade >2% din portofoliu → violation critică
- Stop-loss mutat în direcția defavorabilă (widening) → violation critică
- Poziție futures fără stop-loss pre-setat → violation critică
- Lipsă jurnal de trading post-trade → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto
- C-001 → prerequisit (cont exchange activ)
- C-003 → portfolio allocation complementară

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Position sizing calculat ÎNAINTE de plasarea ordinului?
- [ ] Stop-loss bazat pe ATR, nu pe procentaj arbitrar?
- [ ] OCO sau trailing stop configurat?
- [ ] Jurnal de trading actualizat post-execuție?

**VK-uri obligatorii:**
1. `✅ [PROC] C-002 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Set Up Stop-Loss and Limit Orders" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Binance / Kraken / OKX / Bybit | Exchange-uri cu ordine avansate |
| TradingView | Calcul ATR, charting, alerts |
| Nansen | Whale flow monitoring |
| Glassnode | Exchange net flows, on-chain signals |
| Coinglass | Funding rates, open interest, liquidations |
| Dune Analytics | DeFi liquidation monitoring |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Risc per trade | ≤2% din portofoliu |
| R:R ratio minim | 1:2 |
| Stop-loss pe fiecare poziție | 100% |
| Slippage mediu | <0.5% |
| Jurnal de trading completat | 100% trade-uri |
