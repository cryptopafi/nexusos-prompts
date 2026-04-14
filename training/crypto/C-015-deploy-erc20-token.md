---
type: procedure
created: 2026-03-22
status: active
slug: c-015-deploy-erc20-token
tags: [procedure, nexus]
---

# Deploy an ERC-20 Token on Ethereum — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid complet pentru scrierea, testarea și deployarea unui contract ERC-20 pe rețeaua Ethereum, inclusiv verificare pe Etherscan.

---

## 1. Problema

Deployarea unui token ERC-20 pe Ethereum implică scrierea unui smart contract, compilarea, testarea riguroasă pe testnet și abia apoi deployarea pe mainnet. Fără o procedură clară, există riscuri majore de pierdere permanentă a fondurilor prin buguri de contract sau configurații greșite. Orice eroare în codul contractului este ireversibilă după deploy pe mainnet.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

> **Safety Note**: Contractele smart deployate pe mainnet sunt imuabile. Testați întotdeauna pe testnet (Sepolia/Goerli) înainte de mainnet. Un bug în contract poate cauza pierderea permanentă a fondurilor.

Situații acoperite:
- Crearea unui token ERC-20 standard cu funcționalități de bază (transfer, approve, allowance)
- Deploy pe testnet Sepolia pentru validare înainte de mainnet
- Deploy pe mainnet Ethereum cu verificare publică pe Etherscan

---

## 2. Procedura

### Pas 1: Pregătire environment de dezvoltare
Instalați Hardhat sau Foundry ca framework de development. Rulați `npm init -y && npm install --save-dev hardhat @nomicfoundation/hardhat-toolbox` pentru Hardhat. Creați structura de proiect cu `npx hardhat init` și selectați TypeScript project. Configurați `.env` cu private key și RPC URL (Infura/Alchemy) — niciodată hardcodate în cod.

### Pas 2: Scrieți contractul ERC-20
Creați fișierul `contracts/MyToken.sol` folosind OpenZeppelin ca bază: `import "@openzeppelin/contracts/token/ERC20/ERC20.sol"`. Definiți constructor cu `name`, `symbol` și `initialSupply`. Adăugați funcționalități custom doar dacă sunt necesare (mint, burn, pause). Auditați manual codul pentru vulnerabilități comune: reentrancy, overflow, access control.

### Pas 3: Compilați și inspectați ABI-ul
Rulați `npx hardhat compile` și verificați că nu există erori sau warning-uri critice. Inspectați ABI-ul generat în `artifacts/contracts/MyToken.sol/MyToken.json`. Confirmați că toate funcțiile ERC-20 standard sunt prezente: `transfer`, `transferFrom`, `approve`, `allowance`, `balanceOf`, `totalSupply`. Documentați ABI-ul pentru integrări viitoare.

### Pas 4: Scrieți și rulați testele
Creați `test/MyToken.test.ts` cu teste pentru toate funcțiile critice: deploy corect, transfer de tokeni, approve/allowance, edge cases (transfer zero, transfer peste balance). Rulați `npx hardhat test` și asigurați-vă că 100% din teste trec. Adăugați teste de coverage: `npx hardhat coverage` — target minim 95%.

### Pas 5: Deploy pe testnet Sepolia
Configurați `hardhat.config.ts` cu network Sepolia (RPC URL din Infura/Alchemy, private key din `.env`). Obțineți ETH de test de la faucet (sepoliafaucet.com). Rulați `npx hardhat run scripts/deploy.ts --network sepolia`. Notați adresa contractului deployat și verificați pe Sepolia Etherscan că tranzacția a reușit.

### Pas 6: Verificați contractul pe Etherscan
Rulați `npx hardhat verify --network sepolia ADRESA_CONTRACT "NumeToken" "SIMBOL" INITIAL_SUPPLY` pentru a verifica și publica codul sursă. Accesați Sepolia Etherscan și confirmați că tab-ul "Contract" arată codul sursă verificat (verde). Interacționați cu contractul prin Etherscan Read/Write pentru a valida funcțiile. Repetați procesul pe mainnet doar după validare completă pe testnet.

### Pas 7: Deploy pe mainnet și documentare finală
Asigurați-vă că aveți suficient ETH real pentru gas (estimați cu `npx hardhat run scripts/estimate-gas.ts`). Rulați deploy pe mainnet: `npx hardhat run scripts/deploy.ts --network mainnet`. Verificați contractul pe Etherscan mainnet. Documentați adresa contractului, ABI-ul, data deploy-ului și configurațiile în fișier de proiect. Salvați toate detaliile în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "ERC-20 token deployat pe Ethereum: contract compilat, testat pe Sepolia, verificat pe Etherscan, deploiat pe mainnet.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-015",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "ethereum", "erc20", "smart-contract", "blockchain-dev", "hardhat", "solidity"],
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
- Contract deployat pe mainnet fără audit de securitate → violation critică
- Deploy realizat fără teste prealabile pe testnet → violation critică
- Private key expus în cod sau commit Git → violation de securitate maximă
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Teste rulate cu coverage >95% înainte de deploy?
- [ ] Contract verificat public pe Etherscan?

**VK-uri obligatorii:**
1. `✅ [PROC] C-015 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Deploy an ERC-20 Token on Ethereum" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Hardhat / Foundry | Framework de development și testing |
| OpenZeppelin Contracts | Implementare ERC-20 auditată și sigură |
| Infura / Alchemy | RPC provider pentru Ethereum mainnet și testnet |
| Etherscan | Verificare și publicare cod sursă contract |
| Sepolia Faucet | ETH de test pentru testnet |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Test coverage | >95% |
| Testnet validation | Obligatoriu înainte de mainnet |
