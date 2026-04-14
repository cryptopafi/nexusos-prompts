---
type: procedure
created: 2026-03-22
status: active
slug: c-004-altcoin-research
tags: [procedure, nexus]
---

# Research an Altcoin Before Buying — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: META-H-002
**Scope**: Framework complet de due diligence pentru evaluarea unui altcoin înainte de achiziție, combinând analiză fundamentală, on-chain metrics, tokenomics, echipă și market microstructure.

---

## 1. Problema

90% din altcoins pierd valoare pe termen lung vs BTC. Fără un proces de research structurat, deciziile de cumpărare se bazează pe hype, FOMO sau recomandări nesolicitate. Un framework sistematic de evaluare reduce semnificativ probabilitatea de a investi în proiecte care eșuează. Chiar și proiecte aparent legitime pot avea tokenomics predatorii (vesting cliffs, inflation ascunsă) care diluează investitorii retail.

Situații acoperite:
- Evaluarea unui altcoin nou sau necunoscut înainte de prima achiziție
- Due diligence aprofundat pe un token cu market cap sub $1B
- Compararea a doi sau mai mulți tokeni din aceeași categorie/narrative
- Re-evaluarea unui activ existent din portofoliu

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Identifică fundamentalele proiectului
Citește whitepaper-ul complet (nu doar summary). Verifică: (a) ce problemă rezolvă concret (nu buzzwords), (b) care este mecanismul tehnic (L1, L2, DeFi, infrastructure), (c) există un produs funcțional sau doar promisiuni? Accesează site-ul oficial, demo-uri, GitHub repo. Verifică activitatea pe GitHub: commits în ultimele 30 zile, număr de contribuitori activi. Un proiect cu <10 commits/lună și 1-2 developeri este un red flag major.

### Pas 2: Analizează echipa și backers
Caută fondatorii pe LinkedIn — verifică experiența anterioară (proiecte crypto anterioare, companii tech). Red flags: echipă anonimă fără track record, fondatori cu proiecte eșuate anterior. Verifică investitorii (VCs): a16z, Paradigm, Polychain, Multicoin = stamp of quality. Pe Crunchbase sau ICODrops verifică rundele de funding, valuation-ul la care au intrat VC-urile. Dacă VC-urile au intrat la 1/10 din prețul curent, riscul de sell pressure la vesting unlock este masiv.

### Pas 3: Evaluează tokenomics în detaliu
Pe CoinGecko/CoinMarketCap verifică: (a) **Supply**: circulating vs total vs max supply — inflația anuală ce este? (b) **Vesting schedule**: cât din supply este locked și când se deblochează? Token Unlocks (token.unlocks.app) arată calendar-ul exact. (c) **Distribution**: cât dețin team + VCs + treasury? Dacă >40% la insiders, dilution risk major. (d) **Utility reală**: tokenul este necesar pentru funcționarea protocolului (fee payment, staking, governance cu impact real) sau este doar speculative wrapper?

### Pas 4: Verifică on-chain metrics cu instrumente specializate
**Glassnode**: Active Addresses (trend crescător = adopție), NVT Ratio (<30 undervalued), Holder Distribution. **Nansen**: Smart Money flows — dacă smart money acumulează, semnal pozitiv; dacă distribuie, red flag. **Dune Analytics**: caută dashboard-uri specifice protocolului — utilizatori activi zilnici, revenue generat, TVL trend. **DefiLlama**: pentru DeFi tokens — TVL, fees generate, revenue vs emissions. Un protocol care plătește mai mult în emissions decât generează în revenue nu este sustenabil.

### Pas 5: Analizează market microstructure
Pe Binance/OKX: verifică (a) **Liquidity depth** — order book bid/ask la ±2% de preț; dacă <$100K, exit va fi dificil la sume mari. (b) **Volume/Market Cap ratio** — volum zilnic <5% din market cap = low liquidity, manipulare ușoară. (c) **Funding rates** pe perpetuals (Coinglass) — pozitiv persistent >0.05% = overlevered longs, risc de cascade liquidation. (d) **Open Interest** — OI crescând fără price increase = shorts acumulează sau longs se construiesc pe leverage fragil.

### Pas 6: Evaluează competitive landscape
Identifică top 3-5 competitori direcți. Compară pe: (a) TVL (DefiLlama), (b) active users (Dune), (c) developer activity (Electric Capital report, GitHub), (d) market cap/TVL ratio (sub 1 = potențial undervalued), (e) technology moat (ce face proiectul diferit). Dacă proiectul nu are nici un avantaj clar față de competitori cu market cap 10x mai mare, probabilitatea de underperformance este ridicată.

### Pas 7: Verifică sentiment-ul și riscurile de governance
Verifică pe: (a) CryptoPanic — news sentiment, (b) LunarCrush — social engagement metrics, (c) Governance forums (Snapshot, Tally) — sunt propunerile controversate? Token holders participă activ? (d) Reddit/Discord — calitatea discuțiilor (tehnice vs pump-and-dump vibes). **Risk checklist**: centralizare excesivă (admin keys, upgrade proxy fără timelock), dependență de un singur founder, controversă juridică activă (verifică pe SEC EDGAR dacă proiectul US-based).

### Pas 8: Construiește thesis-ul de investiție și definește exit
Scrie în 3-5 propoziții DE CE cumperi: care este catalizatorul de preț pe orizontul tău? Definește: (a) **Entry**: preț curent sau order limit la support, (b) **Position size**: conform C-002 position sizing, (c) **TP targets**: 2x, 5x, 10x — la fiecare nivel, cât vinzi? (d) **Stop-loss**: bazat pe ATR sau sub support tehnic, (e) **Invalidation thesis**: ce eveniment ar invalida teza? (ex: „dacă TVL scade sub $500M" sau „dacă team dump-uiește >5% din supply"). Scrie toate acestea ÎNAINTE de a cumpăra.

### Pas 9: Documentează și monitorizează post-achiziție
Salvează research-ul complet: data, thesis, metrics cheie, entry price, stop-loss, TP. Setează alerte pe: (a) Token Unlocks pentru vesting events, (b) DexScreener/CoinGecko pentru price alerts, (c) Nansen Smart Money alerts. Re-evaluează quarterly: thesis-ul este încă valid? Metrics-urile se îmbunătățesc sau se degradează? Dacă thesis-ul este invalidat, ieși indiferent de P&L. **Fiscalitate România**: documentează cost basis complet — preț achiziție + comisioane = bază de calcul pentru impozitul de 10%.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-004: Altcoin Research — fundamentale verificate, tokenomics analizate, on-chain metrics validate, thesis de investiție documentat cu entry/exit definite.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-004",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "research", "due-diligence", "altcoin", "tokenomics", "on-chain", "fundamental-analysis"],
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
- Altcoin cumpărat fără research documentat → violation critică
- Investiție fără thesis scris și exit plan definit → violation critică
- Skip verificare on-chain metrics → violation
- Skip verificare tokenomics/vesting → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto
- C-001 → prerequisit (cont exchange)
- C-002 → stop-loss obligatoriu
- C-003 → portfolio allocation
- C-005 → scam detection complementar

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Whitepaper citit și fundamentale verificate?
- [ ] Tokenomics + vesting analizate?
- [ ] On-chain metrics validate (Glassnode/Nansen/Dune)?
- [ ] Thesis de investiție scris cu exit plan?

**VK-uri obligatorii:**
1. `✅ [PROC] C-004 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Research an Altcoin Before Buying" | FORGE ✓ | rule: META-H-002 | v2.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CoinGecko / CoinMarketCap | Market data, supply info |
| Glassnode | On-chain metrics (NVT, addresses) |
| Nansen | Smart money flows, whale tracking |
| Dune Analytics | Custom dashboards per protocol |
| DefiLlama | TVL, fees, revenue, yields |
| Token Unlocks (token.unlocks.app) | Vesting schedule tracking |
| Coinglass | Funding rates, open interest |
| CryptoPanic / LunarCrush | Sentiment analysis |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Research documentat pre-achiziție | 100% altcoins |
| Thesis scris cu exit plan | 100% |
| On-chain validation completă | 100% |
| Win rate pe altcoins researched | >50% |
