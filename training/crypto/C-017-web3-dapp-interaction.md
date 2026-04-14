---
type: procedure
created: 2026-03-22
status: active
slug: c-017-web3-dapp-interaction
tags: [procedure, nexus]
---

# Interact with a Web3 DApp — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru conectarea în siguranță a unui wallet la un DApp Web3, interacțiunea cu contracte smart prin interfață și gestionarea riscurilor de securitate specifice DeFi.

---

## 1. Problema

Interacțiunea cu DApp-uri Web3 expune utilizatorul la riscuri multiple: site-uri phishing care mimează DApp-uri legitime, aprobare de contracte malițioase cu acces nelimitat la fonduri, sau semnarea de mesaje care transferă control asupra walletului. Fără o procedură de verificare riguroasă, o singură interacțiune greșită poate duce la pierderea completă a tuturor fondurilor din wallet.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Conectarea walletului la un DApp DeFi (Uniswap, Aave, Compound, etc.)
- Interacțiunea cu contracte: swap, provide liquidity, stake, borrow
- Revocare aprobare contracte după utilizare

---

## 2. Procedura

### Pas 1: Verificați autenticitatea DApp-ului înainte de conectare
Accesați DApp-ul numai prin URL-uri salvate în bookmark-uri sau din surse oficiale verificate (Twitter oficial, GitHub, CoinGecko). Verificați că URL-ul este exact corect — phishing-ul folosește variante aproape identice (uni-swap.org vs uniswap.org). Verificați certificatul SSL (lacăt verde) și că domeniul nu este recent înregistrat. Căutați pe DeFiLlama sau CoinGecko adresa oficială a protocolului.

### Pas 2: Pregătiți un wallet dedicat pentru DeFi
Utilizați un wallet separat de cel principal pentru interacțiuni DeFi — transferați doar fondurile necesare pentru tranzacția curentă plus gas. Nu conectați niciodată walletul principal/cold storage la DApp-uri. Verificați că extensia MetaMask este versiunea oficială (chrome.google.com/webstore, nu alte surse). Dezactivați alte extensii browser care ar putea intercepta tranzacții.

### Pas 3: Conectați walletul și analizați permisiunile solicitate
Apăsați "Connect Wallet" și selectați MetaMask sau walletul preferat. Analizați cu atenție permisiunile solicitate în popup-ul de conectare — un DApp legitim solicită doar vizualizarea adresei și balanței, nu semnarea de tranzacții la conectare. Dacă DApp-ul solicită acces la toate rețelele sau permisiuni extinse la conectare, abandonați sesiunea. Confirmați că adresa conectată este cea intenționată.

### Pas 4: Analizați tranzacția înainte de aprobare
Înainte de orice tranzacție, verificați în detaliu popup-ul MetaMask: adresa contractului cu care interacționați (verificați pe Etherscan că este contractul oficial al protocolului), funcția apelată și parametrii acesteia, suma autorizată (pentru approve ERC-20 — setați o limită exactă, nu "unlimited"), gas fee estimat și impactul asupra prețului pentru swap-uri. Folosiți Revoke.cash pentru a verifica aprobrile existente.

### Pas 5: Executați tranzacția și monitorizați execuția
Confirmați tranzacția în MetaMask după analiza completă din Pasul 4. Monitorizați execuția pe block explorer folosind TX hash-ul. Verificați că output-ul tranzacției (tokeni primiți, LP tokens, etc.) corespunde cu previziunea din interfața DApp. Pentru tranzacții DeFi complexe (multi-step), confirmați fiecare pas înainte de a continua.

### Pas 6: Revocați aprobările inutile după utilizare
Accesați revoke.cash sau etherscan.io/tokenapprovalchecker după finalizarea sesiunii DeFi. Identificați toate aprobările ERC-20 cu limită "Unlimited" sau sume mai mari decât necesarul curent. Revocați aprobările pentru contracte cu care nu mai interacționați activ — fiecare revocare costă o mică taxă de gas dar elimină riscul de exploit viitor. Mențineți o listă curată de aprobare active.

### Pas 7: Documentați interacțiunea și actualizați securitatea
Notați protocolul utilizat, adresa contractului, suma și tipul tranzacției pentru înregistrări. Verificați că fondurile au ajuns corect (balanțe post-tranzacție vs. pre-tranzacție). Dacă ați interacționat cu un protocol nou, căutați rapoarte de audit de securitate (Certik, Trail of Bits, OpenZeppelin) și verificați TVL și istoricul de securitate pe DeFiLlama. Salvați detaliile în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Interacțiune Web3 DApp completă: DApp autentificat, wallet conectat securizat, tranzacție analizată și executată, aprobare revocată post-utilizare.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-017",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "defi", "web3", "dapp", "metamask", "security", "defi-operations"],
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
- Wallet principal conectat la DApp fără wallet dedicat DeFi → violation de securitate
- Aprobare ERC-20 "Unlimited" lăsată activă post-tranzacție → violation
- DApp accesat fără verificare URL și autenticitate → violation critică
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Aprobările ERC-20 revocate după utilizare?
- [ ] Adresa contractului verificată pe Etherscan înainte de interacțiune?

**VK-uri obligatorii:**
1. `✅ [PROC] C-017 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Interact with a Web3 DApp" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| MetaMask | Wallet pentru conectare și semnare tranzacții |
| Revoke.cash / Etherscan Token Approvals | Gestionarea și revocarea aprobărilor ERC-20 |
| DeFiLlama | Verificare TVL și autenticitate protocoale DeFi |
| Etherscan | Verificare adrese contracte și monitorizare tranzacții |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Aprobare revocate | 100% aprobare Unlimited revocate post-sesiune |
| Phishing incidents | 0 |
