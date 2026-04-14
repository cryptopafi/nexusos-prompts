---
type: procedure
created: 2026-03-22
status: active
slug: c-006-buy-memecoin-dexscreener
tags: [procedure, nexus]
---

# Buy a Memecoin via DexScreener — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Procesul de identificare, verificare și achiziție a unui memecoin pe DEX folosind DexScreener ca instrument principal de analiză.

---

## 1. Problema

Memecoins au risc extrem de ridicat și o rată de eșec de >95%. Fără un proces structurat de verificare și fără limite stricte de expunere, cumpărarea de memecoins devine gambling pur cu pierdere aproape sigură pe termen lung.

Situații acoperite:
- Descoperirea și verificarea unui memecoin trending pe DexScreener
- Achiziția printr-un DEX (Uniswap, Raydium, Jupiter etc.) cu wallet self-custody
- Monitorizarea și exit-ul dintr-o poziție memecoin

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Găsește token-ul pe DexScreener și obține adresa contractului
Accesează dexscreener.com și caută token-ul după nume sau ticker. Copiază adresa contractului NUMAI din DexScreener sau site-ul oficial al proiectului. Nu copia niciodată adrese din comentarii Telegram/Twitter/Discord — aceasta este cea mai comună metodă de scam.

### Pas 2: Aplică verificările C-005 (rug pull detection)
Rulează tokenul prin procesul din procedura C-005: contract verificat, lichiditate locked, TokenSniffer check. Dacă orice verificare critică eșuează → stop, nu cumpăra. Memecoins nu justifică skiparea verificărilor de securitate.

### Pas 3: Verifică metricele cheie pe DexScreener
Verifică: vârsta token-ului (sub 1h = risc maxim), volumul 24h vs market cap, numărul de tranzacții și deținători unici. Caută: raport volume/mcap >10%, minim 100+ holders, tranzacții atât BUY cât și SELL (nu doar buy = bot activity). Lichiditate minimă recomandată: >$20,000.

### Pas 4: Configurează wallet-ul și verifică slippage
Asigură-te că wallet-ul (MetaMask, Phantom etc.) este pe rețeaua corectă și are suficient gas. Pe DEX, setează slippage: 5-10% pentru tokene normale, 15-25% pentru tokene cu tax on transfer. Slippage prea mic = tranzacție failed; prea mare = sandwich attack posibil.

### Pas 5: Execută cumpărarea cu sumă strict limitată
Aloca MAXIM 1-2% din portofoliu total pentru orice memecoin individual. Execută swap-ul și confirmă tranzacția în wallet. Verifică pe DexScreener că tokenul a apărut în wallet și că prețul de execuție este rezonabil față de afișat.

### Pas 6: Setează target de exit și stop mental
Stabilește înainte de cumpărare: la ce multiplicator vinzi (ex: 2x, 5x, 10x). Stabilește și pierderea maximă acceptată (ex: -50% = exit). Memecoins necesită exit rapid — nu există „hold for fundamentals" în memecoin trading.

### Pas 7: Monitorizează și execută exit conform planului
Urmărește price action pe DexScreener. Vinde conform planului stabilit la Pas 6 — nu schimba planul din lăcomie sau frică fără motiv obiectiv. Documentează rezultatul (profit/loss) pentru tracking portofoliu.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-006: Buy Memecoin via DexScreener — adresă verificată, rug check completat, achiziție cu sumă limitată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-006",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "trading", "memecoin", "dexscreener", "defi", "dex"],
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
- Adresă contract copiată din comentarii social media (nu DexScreener/oficial) → violation critică
- Verificări C-005 skipped → violation critică
- Alocare >5% din portofoliu într-un singur memecoin → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto
- C-005 → prerequisit pentru orice token nou

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Adresă contract obținută din sursă oficială (nu social media)?
- [ ] C-005 rug check completat înainte de cumpărare?

**VK-uri obligatorii:**
1. `✅ [PROC] C-006 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Buy a Memecoin via DexScreener" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| DexScreener | Descoperire tokene și metrici de piață |
| Uniswap / Raydium / Jupiter | DEX pentru execuție swap |
| MetaMask / Phantom | Self-custody wallet |
| C-005 (Rug Pull Detection) | Prerequisit de securitate |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Alocare max per memecoin | 1-2% din portofoliu |
| Exit plan definit înainte de cumpărare | 100% cazuri |
