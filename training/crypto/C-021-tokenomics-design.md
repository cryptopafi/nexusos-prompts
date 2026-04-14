---
type: procedure
created: 2026-03-22
status: active
slug: c-021-tokenomics-design
tags: [procedure, nexus]
---

# Understand and Design Tokenomics (Price Mechanics) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Cadru pentru proiectarea mecanicilor economice ale unui token, incluzând demand drivers, supply mechanics, price dynamics și sustenabilitate pe termen lung.

---

## 1. Problema

Tokenomics-ul prost proiectat este cauza principală de colaps al prețului post-lansare: modele Ponzi deghizate în "staking rewards", emisie inflaționistă fără demand real, sau mecanisme de burn fără impact real pe prețul tokenului. Un tokenomics sustenabil trebuie să echilibreze stimulentele pentru toate categoriile de participanți pe termen lung, nu doar în primele luni de la lansare.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

> **Safety Note**: Tokenomics-ul influențează direct valoarea tokenului. Distribuții inechitabile sau supply excesiv sunt red flags pentru investitori și pot fi considerate manipulare de piață.

Situații acoperite:
- Design complet al mecanicilor de demand și supply pentru un token nou
- Modelarea dinamicilor de preț și scenarii de sustenabilitate
- Proiectarea mecanismelor de incentivare pentru toate categoriile de stakeholderi

---

## 2. Procedura

### Pas 1: Analizați fundamentele cererii (demand drivers)
Identificați și documentați toate sursele de cerere pentru token. Demand drivers primari: utilitate în protocol (staking, acces features, governance voting), cerere speculativă (listing pe exchange-uri, narrativ), și cerere instituțională. Demand drivers secundari: fee distribution, buyback, parteneriate. Cuantificați importanța relativă a fiecărui driver. Eliminați fake demand (ex: "cumpără tokenul ca să câștigi mai mulți tokeni" — model circular).

### Pas 2: Proiectați mecanismele de supply și controlul inflației
Definiți rata de emisie a tokenurilor noi: pentru staking rewards, ecosystem grants, team vesting. Calculați rata anuală de inflație pentru primii 5 ani. Proiectați mecanisme de reducere a supply: burn din taxe de protocol (ex: % din fee-uri), buyback & burn cu venituri din protocol, token-uri blocate în smart contracts (staking, LP). Calculați supply net (emisii - burns) pentru fiecare an.

### Pas 3: Modelați relația preț-activitate
Construiți un model simplu preț-activitate: la ce nivel de activitate a protocolului (TVL, volume, utilizatori) tokenul susține o anumită valoare de market cap? Folosiți P/E ratio analog: Market Cap / Protocol Revenue (P/R ratio). Comparați P/R ratio al tokenului vostru la diferite scenarii de creștere cu protocoale similare. Identificați "fair value" range bazat pe fundamentale, nu speculație.

### Pas 4: Proiectați mecanismul de staking și reward-uri
Dacă tokenul include staking: definiți APY țintă, sursele de reward (inflație sau fee-uri reale), lock-up periods și penalități de unlock anticipat. Analizați impactul pe circulant supply: dacă X% din supply este staked, circulant supply scade, reducând presiunea de vânzare. Modelați echilibrul optim: APY suficient de atractiv pentru a menține 30-50% din supply staked, susținut de venituri reale nu inflație pură.

### Pas 5: Analizați scenarii de stress test și sustenabilitate
Simulați 3 scenarii: Bull (10x creștere activitate), Base (creștere moderată), Bear (activitate scăzută sau stagflație). Pentru fiecare scenariu calculați: Revenue, Token Emissions, Net Supply Change, Implied Market Cap. Identificați scenariile în care tokenomics-ul devine nesustenabil (inflație > demand, APY colapsează). Adăugați mecanisme de protecție pentru scenariul Bear (reducere automată a emisiilor, ajustare APY).

### Pas 6: Proiectați launch mechanics și price discovery
Definiți mecanismul de lansare: IDO (Initial DEX Offering), Liquidity Bootstrapping Pool (LBP), Fair Launch, sau Private + Public Sale. Calculați initial liquidity necesară pentru un order book sănătos (minim recomandat: $500K-$1M pentru lansare fără slippage masiv). Definiți prețul inițial bazat pe FDV comparabil cu competiția. Proiectați bonding curve dacă este relevant.

### Pas 7: Documentați Tokenomics Design Document complet
Compilați toate elementele într-un document structurat: Token Overview, Supply Schedule (5 ani), Demand Drivers, Staking Mechanism, Price Discovery Mechanism, Stress Test Scenarios, Sustainability Analysis. Adăugați vizualizări: grafic supply/emissions/burns pe 5 ani, pie chart alocare, grafic APY vs. % staked. Publicați în whitepaper. Salvați în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Tokenomics design completat: demand drivers analizați, supply mechanics proiectate, staking model configurat, stress tests efectuate, Tokenomics Design Document finalizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-021",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "token-launch", "tokenomics", "price-mechanics", "staking", "defi", "sustainability"],
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
- Tokenomics proiectat fără model de sustenabilitate pe termen lung → violation
- Reward-uri de staking bazate exclusiv pe inflație fără venituri reale → violation
- Stress test omis → violation de metodologie
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Stress test pentru scenariu Bear completat?
- [ ] Sursa de venituri pentru staking rewards documentată (nu inflație pură)?

**VK-uri obligatorii:**
1. `✅ [PROC] C-021 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Understand and Design Tokenomics (Price Mechanics)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Token Terminal / Messari | Date fundamentale pentru P/R ratio comparativ |
| Excel / Google Sheets | Modelare scenarii de sustenabilitate |
| Tokenomics Design Document | Output principal al procedurii |
| Whitepaper | Publicare tokenomics complet |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Scenarii stress test | Minimum 3 (Bull/Base/Bear) |
| Sustenabilitate | Tokenomics pozitiv net în scenariul Base la 24 luni |
