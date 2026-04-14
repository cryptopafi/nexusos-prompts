---
type: procedure
created: 2026-03-22
status: active
slug: c-012-defi-wallet-dex-setup
tags: [procedure, nexus]
---

# Set Up a DeFi Wallet and Connect to a DEX — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea unui wallet self-custody pentru DeFi și conectarea sigură la un DEX pentru prima tranzacție.

---

## 1. Problema

Utilizatorii care trec de la CEX la DeFi fără pregătire adecvată sunt vulnerabili la pierderea fondurilor prin: seed phrase expusă, conectare la site-uri DeFi false (phishing), aprobare de contracte malițioase sau greșeli de tranzacție ireversibile.

Situații acoperite:
- Configurarea primului wallet DeFi self-custody (MetaMask, Phantom etc.)
- Conectarea wallet-ului la un DEX legitim și execuția primului swap
- Gestionarea aprobărilor de contracte și a riscurilor asociate

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Instalează wallet-ul din sursa oficială
Descarcă MetaMask EXCLUSIV de pe metamask.io sau din Chrome/Firefox Extension Store oficial. Pentru Solana, Phantom se descarcă de pe phantom.app. Verifică că extensia are milioane de utilizatori și reviews pozitive — există extensii fake MetaMask care fură seed phrases.

### Pas 2: Creează wallet-ul și securizează seed phrase-ul
Urmează procesul din C-011 pentru stocarea seed phrase: 12/24 cuvinte notate fizic, offline, 2 copii în locuri separate. Niciodată nu continua fără a fi completat backup-ul fizic. Verifică seed phrase-ul prin procesul de confirmare cerut de wallet.

### Pas 3: Adaugă rețeaua și token-urile necesare
Adaugă rețelele dorite (Arbitrum, Polygon, Base etc.) din chainlist.org — site oficial pentru adăugare sigură de rețele. Adaugă token-urile prin adresa contractului verificată pe Etherscan, nu prin search în wallet (există tokene fake cu același nume).

### Pas 4: Finanțează wallet-ul și verifică adresa
Copiază adresa wallet-ului și trimite o sumă mică de test din CEX înainte de orice transfer mare. Confirmă că suma a ajuns vizibilă în wallet. Verifică de 3 ori adresa destinatarului la orice transfer — tranzacțiile crypto sunt ireversibile.

### Pas 5: Conectează-te la DEX prin URL oficial (bookmark imediat)
Accesează Uniswap (app.uniswap.org), Curve (curve.fi) sau alt DEX prin URL direct tastat sau bookmark. Nu accesa prin Google Ads sau linkuri din Discord/Telegram. Bookmark-uiește imediat URL-ul verificat. Click „Connect Wallet" și aprobă conectarea în extensia wallet.

### Pas 6: Execută primul swap cu sumă minimă de test
Selectează perechea (ex: ETH → USDC), introdu o sumă minimă pentru test. Verifică: prețul, slippage setat (0.5% default ok pentru tokene mari), fee-ul de gas estimat. Confirmă tranzacția în wallet și urmărește statusul pe Etherscan (link direct din wallet).

### Pas 7: Gestionează și revocă aprobările de contract
Periodic (lunar), verifică aprobările active pe revoke.cash sau etherscan.io/tokenapproval. Revocă aprobările neutilizate sau pentru sume nelimitate. Fiecare aprobare de contract este o suprafață de atac — minimizează-le.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-012: DeFi Wallet și DEX Setup — wallet configurat, seed phrase securizat, primul swap executat cu succes.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-012",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "defi", "wallet", "dex", "metamask", "self-custody"],
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
- Wallet instalat din sursă neverificată → violation critică
- Conectare la DEX prin link din social media (nu URL direct/bookmark) → violation critică
- Seed phrase nesecurizat conform C-011 înainte de finanțare wallet → violation critică
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto
- C-011 → prerequisit pentru securizare seed phrase

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Wallet instalat din metamask.io / phantom.app oficial?
- [ ] URL DEX verificat și bookmark-uit înainte de conectare?

**VK-uri obligatorii:**
1. `✅ [PROC] C-012 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Set Up a DeFi Wallet and Connect to a DEX" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| MetaMask / Phantom | Self-custody wallet pentru DeFi |
| Chainlist.org | Adăugare sigură de rețele EVM |
| Uniswap / Curve / DEX ales | Platformă de swap descentralizat |
| Revoke.cash | Audit și revocare aprobări contracte |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Transfer test mic înainte de sumă mare | 100% cazuri noi |
| Audit aprobări contracte | Lunar |
