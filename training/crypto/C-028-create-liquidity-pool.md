---
type: procedure
created: 2026-03-22
status: active
slug: c-028-create-liquidity-pool
tags: [procedure, nexus]
---

# Create a Liquidity Pool for Your Token — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pas-cu-pas pentru crearea și finanțarea unui liquidity pool pe un DEX (ex. Uniswap, Raydium) pentru tokenul propriu.

---

## 1. Problema

Fără lichiditate, tokenul tău nu poate fi tranzacționat pe piața secundară. Crearea unui liquidity pool este pasul fundamental care permite utilizatorilor să cumpere și să vândă tokenul pe un DEX descentralizat. Mulți fondatori eșuează la acest pas din cauza configurărilor greșite sau a neînțelegerii mecanismelor de impermanent loss.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

> **Safety Note**: Furnizarea de lichiditate expune la impermanent loss și risc de rug pull. Nu furnizați lichiditate cu fonduri pe care nu vă puteți permite să le pierdeți. Verificați contractul pool-ului pe Etherscan înainte de deposit.

Situații acoperite:
- Token nou lansat care nu are încă lichiditate pe niciun DEX
- Token existent care migrează lichiditatea pe un DEX diferit
- Fondator care dorește să crească adâncimea pool-ului existent

---

## 2. Procedura

### Pas 1: Selectează DEX-ul și chain-ul potrivit
Identifică ecosistemul chain-ului pe care a fost deployat tokenul (Ethereum → Uniswap v3, Solana → Raydium, BSC → PancakeSwap). Verifică volumul zilnic al DEX-ului ales și taxele de gas estimate. Documentează alegerea și motivarea ei.

### Pas 2: Verifică contractul token-ului pe block explorer
Accesează Etherscan / Solscan / BscScan și verifică adresa contractului tokenului. Confirmă că supply-ul total, funcțiile de mint/burn și permisiunile owner sunt conform whitepaper-ului. Salvează adresa verificată — aceasta va fi folosită la crearea pool-ului.

### Pas 3: Verifică contractul pool-ului pe block explorer
Înainte de orice deposit, caută adresa pool-ului pe block explorer și verifică că nu există funcții ascunse (honeypot, fee-uri ascunse, withdraw owner). Folosește tokensniffer.com sau honeypot.is pentru un scan automat. Aceasta este verificarea de securitate obligatorie.

### Pas 4: Pregătește perechea de lichiditate
Determină cu ce token vei face pereche (ETH, SOL, USDC, BNB). Calculează raportul inițial de preț: dacă tokenul tău valorează $0.001 și ETH = $3000, vei pune 1 ETH la 3,000,000 tokeni. Asigură-te că ai ambele active în wallet.

### Pas 5: Configurează range-ul de preț (pentru Uniswap v3)
Pe Uniswap v3, setează un range de preț realist (ex. ±50% față de prețul inițial). Un range mai îngust oferă mai multă eficiență a capitalului, dar riscă să iasă din range dacă prețul se mișcă brusc. Pentru lansări noi, un range larg (full range) este mai sigur.

### Pas 6: Adaugă lichiditatea și semnează tranzacția
Conectează wallet-ul (MetaMask / Phantom) la interfața DEX-ului. Aprobă token spending (approve transaction), apoi adaugă lichiditatea cu suma decisă. Verifică gas fee-ul înainte de confirmare. Salvează hash-ul tranzacției de creare a pool-ului.

### Pas 7: Monitorizează și documentează pool-ul
Copiază adresa pool-ului și adaug-o în dashboard-ul proiectului. Setează alertă pe DexScreener pentru activitate suspectă. Documentează: adresa pool, DEX, pereche, lichiditate inițială (USD), dată. Salvează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Pool de lichiditate creat pentru token pe DEX. Verificare contract completă, lichiditate inițială adăugată, monitorizare activată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-028",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "defi", "liquidity-pool", "dex", "token-launch"],
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
- Lichiditate adăugată fără verificarea contractului pool → violation critică
- Pool creat fără calcularea raportului de preț inițial → violation
- Nicio documentare a adresei pool-ului și hash-ului tranzacției → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Contractul pool-ului verificat pe block explorer ÎNAINTE de deposit?
- [ ] Hash-ul tranzacției și adresa pool-ului salvate?

**VK-uri obligatorii:**
1. `✅ [PROC] C-028 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Create a Liquidity Pool for Your Token" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| DEX (Uniswap/Raydium/PancakeSwap) | Platforma de creare pool |
| Block Explorer (Etherscan/Solscan) | Verificare contract securitate |
| TokenSniffer / Honeypot.is | Scan automat honeypot |
| MetaMask / Phantom Wallet | Semnare tranzacții |
| DexScreener | Monitorizare post-lansare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Lichiditate inițială (USD) | Minim $1,000 |
| Verificare contract securitate | 100% (obligatoriu pre-deposit) |
| Timp setup pool | < 30 minute |
| Documentare adresă pool | 100% |
