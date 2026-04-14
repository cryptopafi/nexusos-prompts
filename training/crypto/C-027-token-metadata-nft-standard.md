---
type: procedure
created: 2026-03-22
status: active
slug: c-027-token-metadata-nft-standard
tags: [procedure, nexus]
---

# Register Token Metadata as an NFT Standard — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru înregistrarea și conformarea metadatelor unui token fungibil cu standardul NFT Metaplex Token Metadata Program, asigurând compatibilitatea completă cu ecosistemul Solana.

---

## 1. Problema

Standardul NFT Metaplex Token Metadata Program este utilizat nu doar pentru NFT-uri, ci și pentru tokeni fungibili pe Solana — acesta este mecanismul oficial prin care wallet-uri, DEX-uri și aplicații identifică și afișează informațiile unui token. Înregistrarea incorectă sau incompletă duce la tokeni nerecunoscuți în ecosistem, imposibilitate de listare pe Jupiter sau alte DEX-uri, și lipsă de credibilitate față de utilizatori.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Crearea și configurarea Metadata Account on-chain cu Metaplex Token Metadata Program
- Setarea corectă a câmpurilor: name, symbol, uri, creators, seller_fee_basis_points
- Verificarea conformității cu standardul și actualizarea metadatelor dacă este necesar

---

## 2. Procedura

### Pas 1: Înțelegeți structura Metadata Account Metaplex
Metadata Account Metaplex conține: `name` (max 32 caractere), `symbol` (max 10 caractere), `uri` (URI metadata JSON — din C-026, max 200 caractere), `update_authority` (adresa cu drept de actualizare a metadatelor), `mint` (adresa Mint Account-ului tokenului), `is_mutable` (true = metadata poate fi actualizată, false = imutabilă), `creators` array (optional pentru tokeni fungibili). Documentați valorile pentru fiecare câmp.

### Pas 2: Instalați și configurați Metaplex Sugar sau Metaboss
Metaboss este tool-ul principal pentru operațiuni pe Metadata Accounts existente. Instalați: `cargo install metaboss` (necesită Rust instalat — `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`). Verificați instalarea: `metaboss --version`. Alternativ, utilizați JavaScript SDK Metaplex: `npm install @metaplex-foundation/mpl-token-metadata`. Configurați RPC URL preferat (Helius, QuickNode) pentru latență mai mică.

### Pas 3: Creați Metadata Account pentru tokenul existent
Dacă tokenul (C-025) nu are încă un Metadata Account creat, folosiți Metaboss create: `metaboss create metadata --keypair ~/token-keypairs/mint-keypair.json --mint [ADRESA_MINT] --name "Nume Token" --symbol "SIMBOL" --uri "https://arweave.net/[URI_METADATA]" --rpc-url https://api.devnet.solana.com`. Alternativ cu JS SDK: utilizați `createMetadataAccountV3` din `@metaplex-foundation/mpl-token-metadata`. Notați adresa Metadata Account creată.

### Pas 4: Configurați update_authority și is_mutable
Implicit, `update_authority` este wallet-ul care a creat metadata. Evaluați dacă tokenul trebuie să fie mutable sau imutabil: pentru token-uri în dezvoltare activă, mențineți `is_mutable: true`; pentru tokeni lansați cu branding fix, considerați setarea la `false` pentru a preveni modificări ulterioare (semnal de încredere pentru comunitate). Dacă doriți transferul update_authority la un multisig: `metaboss update authority --new-update-authority [ADRESA_MULTISIG]`.

### Pas 5: Verificați Metadata Account pe Solana Explorer
Accesați Solana Explorer (explorer.solana.com) și căutați adresa Mint Account-ului. În secțiunea "Metadata", verificați că toate câmpurile sunt corecte: Name, Symbol, URI (accesibil), Update Authority, Is Mutable. Accesați URI-ul și verificați că JSON returnează datele corecte (din C-026). Importați tokenul în Phantom Wallet by address și verificați că afișează corect: nume, simbol și logo.

### Pas 6: Înregistrați tokenul în registrele ecosistemului Solana
Pentru vizibilitate completă în ecosistem: (1) Solana Token List (legacy, github.com/solana-labs/token-list) — submiteți PR cu tokenul vostru; (2) Jupiter Strict List (verificare calitate) — aplicați la jup.ag/strict; (3) Birdeye Token List — contact support@birdeye.so; (4) CoinGecko/CoinMarketCap — listare gratuită după ce tokenul are lichiditate și volum. Procesul de înregistrare poate dura 1-4 săptămâni.

### Pas 7: Testați compatibilitatea completă în ecosistem și documentați
Testați tokenul în: Jupiter Aggregator (swap disponibil?), Raydium/Orca (add liquidity posibil?), Phantom și Solflare (afișare corectă), Solscan și Birdeye (metadata completă vizibilă). Documentați: adresa Mint Account, adresa Metadata Account, URI metadata (Arweave), Update Authority, Is Mutable status, listele de înregistrare (pending/approved). Compilați "Token Launch Technical Summary" cu toate adresele și TX hash-urile relevante. Salvați în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Token metadata înregistrat ca NFT standard: Metadata Account creat cu Metaplex, câmpuri verificate on-chain, update_authority configurat, token verificat în Phantom și ecosistem Solana.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-027",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "solana", "token-launch", "metaplex", "nft-standard", "metadata-program", "token-registry"],
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
- Metadata Account creat fără verificarea câmpurilor pe Explorer → violation
- Update authority lăsată pe wallet hot fără plan de transfer la multisig → violation
- Token lanssat fără înregistrare în minimum un registru public (Jupiter/TokenList) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Metadata Account verificat pe Solana Explorer cu toate câmpurile corecte?
- [ ] Token Launch Technical Summary compilat cu toate adresele?

**VK-uri obligatorii:**
1. `✅ [PROC] C-027 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Register Token Metadata as an NFT Standard" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Metaboss CLI | Creare și actualizare Metadata Accounts |
| Metaplex Token Metadata Program | Smart contract on-chain pentru metadata standard |
| Solana Explorer / Solscan | Verificare Metadata Account on-chain |
| Jupiter Strict List | Registru de tokeni de calitate în ecosistemul Solana |
| Phantom Wallet | Verificare afișare token în wallet principal |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Ecosystem registries | Aplicat pentru minimum 1 (Jupiter sau TokenList) |
| Wallet display test | Phantom + Solflare ambele verificate |
