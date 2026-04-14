---
type: procedure
created: 2026-03-22
status: active
slug: c-019-crypto-market-research
tags: [procedure, nexus]
---

# Conduct Cryptocurrency Market Research — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Metodologie sistematică pentru cercetarea pieței crypto în contextul lansării unui token, incluzând analiza competiției, timing-ului de piață și identificarea oportunităților de nișă.

---

## 1. Problema

Lansarea unui token fără cercetare de piață riguroasă duce la intrare în segmente saturate, timing greșit (bull vs. bear market), sau ignorarea unor competitori puternici deja existenți. Datele de piață în crypto sunt volatile și fragmentate, necesitând o abordare multi-sursă pentru a obține o imagine corectă. Deciziile de lansare bazate pe FOMO sau informații incomplete au rate de eșec ridicate.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Research pentru definirea nișei și segmentului de piață al unui token nou
- Analiza competitivă a tokenurilor existente în același spațiu
- Evaluarea timing-ului de lansare în contextul ciclului de piață

---

## 2. Procedura

### Pas 1: Definiți scopul și întrebările de research
Articulați 5-7 întrebări specifice la care research-ul trebuie să răspundă: Cât de mare este piața adresabilă? Cine sunt top 5 competitori? Care este nivelul de saturație al nișei? Există demand organic dovedit? Care este sentiment-ul curent față de categoria de token? Documentați aceste întrebări înainte de a începe colectarea de date — previne research paralel și fără focus.

### Pas 2: Analizați datele macro ale pieței crypto
Accesați CoinMarketCap și CoinGecko pentru overview-ul pieței: total market cap, dominanță BTC, Fear & Greed Index. Verificați ciclul de piață curent folosind indicatori on-chain (Glassnode): MVRV Z-Score, Net Unrealized Profit/Loss (NUPL), Bitcoin Halving cycle position. Analizați trend-ul ultimelor 6-12 luni pentru categoria voastră de token pe DeFiLlama (TVL trend pentru DeFi) sau similare.

### Pas 3: Mapați competiția — top 10 proiecte similare
Identificați pe CoinGecko categoria relevantă și filtrați top proiecte by market cap. Pentru fiecare proiect din top 10, documentați: Market Cap, Fully Diluted Valuation (FDV), preț actual vs. ATH (% down from ATH), circulant supply / total supply, TVL (dacă DeFi), launch date, tokenomics summary. Creați un tabel comparativ în spreadsheet.

### Pas 4: Analizați community și traction-ul competitorilor
Verificați pentru fiecare competitor: urmăritori Twitter/X, membri Discord/Telegram, activitate GitHub (commits/lună), număr de wallet-uri unice (Etherscan/Dune Analytics). Analizați calitatea comunității — nu doar cantitatea: raport engaged/total followers, frecvența postărilor, sentiment în comunitate. Identificați punctele slabe ale competiției unde proiectul vostru poate câștiga.

### Pas 5: Identificați oportunități de nișă și timing
Pe baza datelor colectate, identificați gap-uri: funcționalitate lipsă în soluțiile existente, segment de utilizatori neservit, combinație de features inexistentă pe piață. Evaluați timing-ul: lansările în bear market au distribuție mai echitabilă dar traction mai greu; lansările în bull market au mai multă atenție dar mai multă competiție. Documentați 3 oportunități identificate cu justificare data-driven.

### Pas 6: Analizați sentiment și narativele dominante
Monitorizați Twitter/X Crypto Twitter, Reddit (r/CryptoCurrency, r/DeFi), și Discord-uri relevante pentru sentiment față de categoria voastră. Identificați narativele dominante în spațiu (ex: RWA, Layer 2 scaling, AI + crypto, etc.) și evaluați cum proiectul vostru se aliniază sau contrazice aceste narrative. Analizați search trends pe Google Trends și Twitter pentru keywords relevante.

### Pas 7: Compilați Market Research Report
Documentați findings într-un raport structurat: Executive Summary (1 pagină), Market Size & Opportunity, Competitive Landscape (tabelul din Pasul 3), Community Analysis, Timing Assessment, Identified Opportunities, Risk Factors, Recomandare Go/No-Go cu justificare. Revizuiți raportul cu echipa. Actualizați raportul la fiecare 30 de zile dacă lansarea este planificată în viitor.

---

## 3. Cortex Logging

```json
{
  "text": "Market research crypto completat: piață macro analizată, top 10 competitori mapați, oportunități de nișă identificate, Market Research Report compilat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-019",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "token-launch", "market-research", "competitive-analysis", "defi", "tokenomics"],
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
- Research realizat fără date on-chain (doar date de preț) → violation de metodologie
- Competitive analysis cu mai puțin de 5 proiecte analizate → violation
- Market Research Report neactualizat la >30 zile de la compilare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 10 competitori analizați cu date complete?
- [ ] Market Research Report conține recomandare Go/No-Go?

**VK-uri obligatorii:**
1. `✅ [PROC] C-019 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Conduct Cryptocurrency Market Research" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CoinGecko / CoinMarketCap | Date de piață, market cap, categorii |
| DeFiLlama | TVL și date DeFi pentru protocoale |
| Glassnode | Indicatori on-chain și cicluri de piață |
| Dune Analytics | Query-uri on-chain custom și wallet analytics |
| Google Trends | Trend-uri de căutare pentru keywords crypto |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Competitori analizați | Minimum 10 cu date complete |
| Report refresh cycle | La 30 zile dacă lansarea este pending |
