---
type: procedure
created: 2026-03-22
status: active
slug: c-025-mint-solana-token
tags: [procedure, nexus]
---

# Mint Your Token on Solana — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pas cu pas pentru mintarea unui token SPL pe Solana folosind CLI sau Metaplex Sugar, cu testare obligatorie pe devnet înainte de execuție pe mainnet.

---

## 1. Problema

Mintarea unui token pe Solana este o operațiune ireversibilă în mai multe aspecte critice: adresa tokenului (mint address) este permanentă, supply-ul inițial minted nu poate fi redus fără burn mechanism, și erorile în decimals sau supply nu pot fi corectate fără a crea un token nou. Fiecare eroare pe mainnet consumă SOL real și poate compromite lansarea proiectului.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

> **Safety Note**: Verificați de 3 ori adresa walletului și network-ul înainte de mint. Tranzacțiile blockchain sunt ireversibile.

Situații acoperite:
- Crearea unui Mint Account SPL Token pe Solana devnet pentru testare
- Mintarea supply-ului inițial în walletul de distribuție
- Executarea acelorași pași pe mainnet după validare completă pe devnet

---

## 2. Procedura

### Pas 1: Verificați prerequisite-urile înainte de mint
Confirmați că aveți: Solana CLI instalat și configurat (`solana --version`), wallet configurat și finanțat (`solana balance` — minimum 0.1 SOL), mint keypair generat și securizat (C-024), SPL Token CLI instalat (`spl-token --version`). Verificați că sunteți pe network-ul corect: `solana config get` — URL trebuie să fie devnet pentru teste. Dacă oricare dintre prerequisite lipsește, opriți și completați pașii anteriori.

### Pas 2: Stabiliți parametrii tokenului înainte de creare
Documentați și confirmați pentru ultima oară: Decimals (standard: 9 pentru tokeni fungibili, compatibil cu ecosistemul Solana; 6 pentru stable-coins), Total Supply planificat, Mint Authority (adresa walletului care poate minta tokeni suplimentari), Freeze Authority (None dacă nu este necesar). Acești parametri nu pot fi schimbați după creare — verificați de 3 ori.

### Pas 3: Creați Mint Account pe devnet
Rulați comanda de creare a tokenului pe DEVNET: `spl-token create-token --decimals 9 ~/token-keypairs/mint-keypair.json`. Output-ul va fi: "Creating token [ADRESA_TOKEN]" urmată de TX signature. Copiați adresa tokenului. Verificați pe Solana Explorer (explorer.solana.com, cluster: Devnet) că Mint Account a fost creat cu parametrii corecți: decimals, mint authority, supply = 0. Confirmați că este devnet.

### Pas 4: Creați un Token Account pentru recepționarea supply-ului
Înainte de a minta tokeni, trebuie creat un Associated Token Account (ATA) în walletul destinație. Rulați: `spl-token create-account [ADRESA_TOKEN]`. Aceasta creează un ATA în walletul CLI curent (cel din `solana config`). Alternativ, creați pentru o altă adresă: `spl-token create-account [ADRESA_TOKEN] --owner [ALTA_ADRESA]`. Notați adresa Token Account creată. Verificați pe Explorer că contul există.

### Pas 5: Mintați supply-ul inițial pe devnet
Rulați comanda de mint: `spl-token mint [ADRESA_TOKEN] [CANTITATE] [ADRESA_TOKEN_ACCOUNT]`. Exemplu pentru 1 miliard de tokeni cu 9 decimals: `spl-token mint TokenAdresaAici 1000000000000000000 TokenAccountAdresa`. Verificați supply-ul: `spl-token supply [ADRESA_TOKEN]` — trebuie să returneze cantitatea mintată. Verificați balanța în wallet: `spl-token balance [ADRESA_TOKEN]`.

### Pas 6: Validați complet pe devnet înainte de mainnet
Efectuați teste complete pe devnet: transferați tokeni la o altă adresă de test (`spl-token transfer [TOKEN] [SUMA] [DESTINATIE] --fund-recipient`), verificați că transferul apare pe Explorer, verificați că balanțele sunt corecte. Dacă planificați revocare mint authority: `spl-token authorize [TOKEN] mint --disable` (IREVERSIBIL — testați mai întâi pe devnet). Documentați toate TX hash-urile de test.

### Pas 7: Executați pe mainnet după validare completă
Comutați la mainnet: `solana config set --url https://api.mainnet-beta.solana.com`. Verificați balanța SOL pentru mainnet: `solana balance`. Repetați exact Pașii 3-5 pe mainnet cu parametrii identici validați pe devnet. Verificați pe Solana Explorer (mainnet) că toate operațiunile sunt corecte. Stocați permanent: adresa mint tokenului, TX hash-ul de creare, adresa Token Account, supply total. Salvați în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Token Solana mintat cu succes: Mint Account creat, supply inițial mintat, validat pe devnet, executat pe mainnet, toate adresele documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-025",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "solana", "token-launch", "mint", "spl-token", "devnet", "mainnet"],
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
- Mint realizat fără test prealabil pe devnet → violation critică
- Decimals sau supply greșit nemontorizat înainte de mainnet → violation
- Mint authority revocată pe devnet fără testare prealabilă a acestui pas → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Devnet test complet (mint + transfer + verificare) înainte de mainnet?
- [ ] Adresa mint tokenului și TX hash salvate permanent?

**VK-uri obligatorii:**
1. `✅ [PROC] C-025 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Mint Your Token on Solana" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Solana CLI | Configurare network și wallet |
| SPL Token CLI (`spl-token`) | Creare Mint Account și mintare supply |
| Mint Keypair (C-024) | Identificatorul permanent al tokenului |
| Solana Explorer | Verificare operațiuni pe devnet și mainnet |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Devnet validation | Obligatoriu (mint + transfer + verificare) |
| Supply accuracy | 100% corespunzător cu Token Allocation Document |
