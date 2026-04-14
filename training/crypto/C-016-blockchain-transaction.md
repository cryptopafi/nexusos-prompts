---
type: procedure
created: 2026-03-22
status: active
slug: c-016-blockchain-transaction
tags: [procedure, nexus]
---

# Transact on a Blockchain (End-to-End) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid end-to-end pentru inițierea, semnarea, trimiterea și confirmarea unei tranzacții blockchain, cu înțelegerea completă a mecanismelor gas, nonce și finalizare.

---

## 1. Problema

Tranzacțiile blockchain sunt ireversibile și implică riscuri financiare reale: gas insuficient poate face tranzacția să eșueze (pierzând gas-ul plătit), adrese greșite duc la pierdere permanentă de fonduri, iar nonce-ul incorect poate bloca toate tranzacțiile ulterioare. Înțelegerea end-to-end a mecanismului de tranzacționare este esențială pentru orice activitate DeFi sau on-chain.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Transfer simplu de ETH sau tokeni ERC-20 între adrese
- Tranzacție cu interacțiune cu smart contract (swap, stake, etc.)
- Rezolvarea tranzacțiilor blocate sau pending pe termen lung

---

## 2. Procedura

### Pas 1: Verificați starea walletului înainte de tranzacție
Deschideți MetaMask sau walletul preferat și confirmați că sunteți pe network-ul corect (Ethereum mainnet, Polygon, etc.). Verificați balanța de ETH/token nativ pentru acoperirea gas-ului — minimum recomandat: valoarea tranzacției + 20% buffer pentru gas. Notați nonce-ul curent dacă aveți tranzacții pending existente. Verificați că adresa destinație este corectă folosind ENS sau verificând primele și ultimele 4 caractere.

### Pas 2: Estimați gas-ul și setați parametrii
Consultați gas tracker (ethgasstation.info, blocknative.com) pentru prețul actual al gas-ului în Gwei. Estimați gas limit-ul: 21.000 pentru transfer simplu ETH, 65.000-100.000 pentru transfer ERC-20, 150.000+ pentru interacțiuni DeFi complexe. Calculați costul total: `Gas Limit × Gas Price (Gwei) / 1e9 = ETH cost`. Setați gas price cu 10-15% peste "Standard" pentru confirmare în 1-3 minute.

### Pas 3: Construiți și semnați tranzacția
În MetaMask, inițiați tranzacția introdusând adresa destinație, suma și asigurați-vă că token-ul selectat este corect. Verificați summary-ul complet în popup-ul de confirmare: adresă sursă, adresă destinație, sumă, gas fee estimat, total în USD. Dacă folosiți ethers.js/web3.js programatic, construiți obiectul transaction cu `to`, `value`, `gasLimit`, `maxFeePerGas`, `maxPriorityFeePerGas`. Semnați cu private key sau hardware wallet.

### Pas 4: Transmiteți tranzacția și obțineți hash-ul
Apăsați "Confirm" în MetaMask sau executați `provider.sendTransaction(signedTx)` programatic. Copiați imediat transaction hash-ul (TX hash) — acesta este identificatorul unic al tranzacției. Stocați hash-ul în clipboard sau notițe pentru tracking. Tranzacția este acum în mempool și așteaptă să fie inclusă într-un bloc.

### Pas 5: Monitorizați tranzacția pe block explorer
Accesați Etherscan.io (sau block explorer-ul network-ului relevant) și căutați TX hash-ul. Urmăriți statusul: Pending (în mempool), Success (inclus în bloc), sau Failed (eșuat). Verificați numărul de confirmări — pentru tranzacții mari, așteptați minimum 12 confirmări (≈3 minute pe Ethereum). Dacă statusul rămâne Pending >10 minute, evaluați dacă gas price-ul este prea mic.

### Pas 6: Rezolvați tranzacțiile pending sau eșuate
Dacă tranzacția este blocată în mempool (gas price prea mic), initiați o tranzacție de înlocuire cu același nonce dar gas price mai mare (+10-30%). În MetaMask: Settings → Advanced → Reset Account (doar dacă nonce-ul este blocat local). Dacă tranzacția a eșuat cu "Out of Gas", repetați cu gas limit mai mare (2× față de estimare). Documentați cauza eșecului pentru evitarea în viitor.

### Pas 7: Confirmați finalizarea și actualizați înregistrările
Verificați că balanța walletului destinație a fost actualizată corect. Accesați tab-ul "Internal Transactions" pe Etherscan pentru tranzacții care implică contracte (pot genera sub-transferuri). Exportați detaliile tranzacției (hash, bloc, timestamp, fee plătit) pentru înregistrări fiscale/contabile. Salvați summary-ul în Cortex cu toate detaliile relevante.

---

## 3. Cortex Logging

```json
{
  "text": "Tranzacție blockchain executată end-to-end: gas estimat, tranzacție semnată și transmisă, confirmată pe block explorer, finalizare documentată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-016",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "blockchain", "transaction", "ethereum", "gas", "metamask", "defi-operations"],
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
- Tranzacție inițiată fără verificarea adresei destinație → violation de securitate
- Gas limit setat sub minimul necesar fără estimare prealabilă → violation
- TX hash nestocată imediat după transmitere → violation de tracking
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] TX hash documentat și accesibil pe block explorer?
- [ ] Tranzacția confirmată cu minimum 6 confirmări?

**VK-uri obligatorii:**
1. `✅ [PROC] C-016 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Transact on a Blockchain (End-to-End)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| MetaMask / Hardware Wallet | Semnare și transmitere tranzacții |
| Etherscan / Block Explorer | Monitorizare status și confirmare |
| Gas Tracker (ETH Gas Station) | Estimare prețuri gas în timp real |
| Infura / Alchemy | RPC provider pentru trimiterea tranzacțiilor |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Gas estimation accuracy | ±15% față de gas real consumat |
| Transaction success rate | 100% (fără tranzacții eșuate din lipsă gas) |
