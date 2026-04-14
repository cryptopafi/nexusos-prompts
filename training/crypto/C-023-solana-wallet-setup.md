---
type: procedure
created: 2026-03-22
status: active
slug: c-023-solana-wallet-setup
tags: [procedure, nexus]
---

# Set Up a Solana Wallet and Fund It for Token Minting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru crearea, securizarea și finanțarea unui wallet Solana dedicat operațiunilor de minting de tokeni, cu verificare pe devnet înainte de operațiuni pe mainnet.

---

## 1. Problema

Configurarea incorectă a unui wallet Solana pentru minting poate duce la pierderea permanentă a fondurilor (seed phrase nesalvat), utilizarea walletului principal expunând toate fondurile la riscuri de operare, sau consumul de SOL real în teste care puteau fi efectuate pe devnet. Un wallet dedicat și corect securizat este prima linie de apărare în orice operațiune pe blockchain.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Crearea unui wallet Solana nou cu Phantom, Solflare sau CLI (solana-keygen)
- Securizarea seed phrase-ului și exportul cheii private
- Finanțarea walletului cu SOL pentru fees (devnet cu airdrop, mainnet cu transfer)

---

## 2. Procedura

### Pas 1: Alegeți tipul de wallet și instalați aplicația
Decideți între: Phantom Wallet (browser extension — recomandat pentru interfață grafică), Solflare (alternativă populară cu suport hardware wallet), sau Solana CLI (`solana-keygen new` — recomandat pentru automatizare și minting programatic). Instalați Phantom din phantom.app (NUMAI de pe site-ul oficial, verificați URL-ul). Pentru CLI: instalați Solana Tool Suite de pe docs.solana.com/cli/install.

### Pas 2: Creați un wallet nou dedicat operațiunilor de minting
Deschideți Phantom și alegeți "Create New Wallet" — nu importați un wallet existent cu fonduri. Alternativ în CLI: `solana-keygen new --outfile ~/.config/solana/mint-keypair.json`. Generați și notați în siguranță seed phrase-ul de 24 de cuvinte (Phantom) sau salvați fișierul JSON al keypair-ului (CLI). Acest wallet este DEDICAT minting-ului — nu depozitați alte fonduri sau NFT-uri valoroase în el.

### Pas 3: Securizați seed phrase-ul și backup-ul
Scrieți seed phrase-ul de mână pe hârtie — niciodată digital (screenshot, note app, email). Creați minimum 2 copii fizice stocate în locații diferite. Verificați că seed phrase-ul este corect testând recuperarea walletului într-un browser incognito sau device diferit ÎNAINTE de a adăuga fonduri. Pentru CLI: faceți backup la fișierul JSON și stocați-l criptat (GPG sau password manager). Documentați adresa publică a walletului.

### Pas 4: Configurați Solana CLI pentru devnet
Instalați Solana CLI dacă nu este instalat: `sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"`. Setați cluster-ul pe devnet: `solana config set --url https://api.devnet.solana.com`. Configurați wallet-ul implicit: `solana config set --keypair ~/.config/solana/mint-keypair.json`. Verificați configurația: `solana config get` și `solana address`. Confirmați că adresa afișată corespunde walletului creat.

### Pas 5: Finanțați walletul pe devnet cu SOL de test
Solicitați SOL gratuit pe devnet (faucet): `solana airdrop 2` (2 SOL de test). Dacă airdrop-ul CLI nu funcționează, utilizați faucet.solana.com sau solfaucet.com introducând adresa walletului. Verificați balanța: `solana balance`. Asigurați-vă că aveți minimum 1-2 SOL pe devnet pentru a acoperi costurile de mint și crearea de conturi asociate (fiecare token account costă ~0.002 SOL rent).

### Pas 6: Testați walletul cu o tranzacție simplă pe devnet
Trimiteți o tranzacție de test pe devnet pentru a confirma că totul funcționează: `solana transfer --allow-unfunded-recipient ALTA_ADRESA 0.1 --url devnet`. Verificați tranzacția pe Solana Explorer (explorer.solana.com) selectând cluster Devnet. Confirmați că TX este finalizată cu status "Finalized". Aceasta validează că wallet-ul, CLI-ul și conexiunea la devnet sunt funcționale.

### Pas 7: Pregătiți finanțarea pentru mainnet (dacă este planificat)
Documentați adresa publică a walletului de minting pentru a fi finanțat de pe un exchange sau wallet principal. Calculați SOL necesar pentru operațiuni de mainnet: crearea unui mint account (~0.0015 SOL), metadata (~0.01 SOL), asociate token accounts și fees de tranzacție. Recomandare: finanțați cu minimum 0.1-0.5 SOL pentru primele operațiuni. Transferați SOL pe mainnet NUMAI după ce toate testele pe devnet sunt complete.

---

## 3. Cortex Logging

```json
{
  "text": "Solana wallet configurat pentru minting: wallet dedicat creat, seed phrase securizat, devnet configurat, SOL de test primit, tranzacție de test validată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-023",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "solana", "wallet", "token-launch", "devnet", "phantom", "solana-cli"],
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
- Seed phrase stocat digital (screenshot, note app, cloud) → violation critică de securitate
- Wallet principal utilizat pentru minting în loc de wallet dedicat → violation
- Operațiuni pe mainnet fără testare prealabilă pe devnet → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Seed phrase backup fizic creat și verificat?
- [ ] Tranzacție de test pe devnet finalizată cu succes?

**VK-uri obligatorii:**
1. `✅ [PROC] C-023 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Set Up a Solana Wallet and Fund It for Token Minting" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Phantom / Solflare | Wallet cu interfață grafică pentru Solana |
| Solana CLI (solana-keygen) | Generare și gestionare keypairs programatic |
| Solana Devnet Faucet | SOL de test gratuit pentru devnet |
| Solana Explorer | Verificare tranzacții pe devnet și mainnet |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Backup seed phrase | 2 copii fizice în locații separate |
| Devnet validation | Obligatoriu înainte de orice operațiune mainnet |
