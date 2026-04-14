---
type: procedure
created: 2026-03-22
status: active
slug: c-026-token-metadata-metaplex
tags: [procedure, nexus]
---

# Create and Upload Token Metadata (Metaplex/MetaBoss) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru crearea, structurarea și upload-ul metadatelor unui token Solana conform standardului Metaplex Token Metadata, cu hosting pe Arweave sau IPFS pentru persistență permanentă.

---

## 1. Problema

Metadata unui token (nume, simbol, logo, descriere) este ceea ce face tokenul recognoscibil în wallet-uri, exchange-uri și aplicații DeFi. Metadata incorectă sau stocată pe servere centralizate poate dispărea, schimbată ulterior, sau poate face tokenul să nu fie recunoscut de ecosistemul Solana. Standardul Metaplex Token Metadata este obligatoriu pentru integrarea în wallet-uri (Phantom, Solflare) și marketplace-uri.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Crearea fișierului JSON de metadata conform standardului Metaplex
- Upload-ul imaginii (logo) și al metadatelor pe Arweave sau IPFS
- Atașarea URI-ului de metadata la Mint Account-ul tokenului Solana

---

## 2. Procedura

### Pas 1: Pregătiți assets-urile vizuale ale tokenului
Creați logo-ul tokenului: format PNG cu fundal transparent, dimensiuni recomandate 512×512px sau 1024×1024px, sub 1MB pentru performanță. Asigurați-vă că logo-ul respectă identitatea vizuală definită în planul de marketing (C-022). Pregătiți și o versiune thumbnail (200×200px). Nu utilizați imagini cu copyright sau asset-uri din proiecte existente. Salvați local în folderul `/token-assets/`.

### Pas 2: Structurați fișierul metadata.json conform standardului Metaplex
Creați fișierul `metadata.json` cu structura exactă Metaplex Fungible Token Standard:
```json
{
  "name": "Nume Token",
  "symbol": "SIMBOL",
  "description": "Descriere completă a tokenului și proiectului.",
  "image": "URI_IMAGINE_ARWEAVE_SAU_IPFS",
  "external_url": "https://website-proiect.com",
  "attributes": [],
  "properties": {
    "files": [{"uri": "URI_IMAGINE", "type": "image/png"}],
    "category": "fungible"
  }
}
```
Verificați că toate câmpurile obligatorii sunt completate: `name`, `symbol`, `description`, `image`.

### Pas 3: Alegeți și configurați soluția de stocare permanentă
Opțiunile recomandate: (1) Arweave — stocare permanentă plătită o singură dată, cel mai utilizat în ecosistemul Solana; integrare directă prin Metaplex Sugar sau Bundlr; (2) IPFS cu Pinata sau NFT.Storage — stocare distribuită, mai ieftin dar necesită pinning continuu; (3) Shadow Drive (Solana-native). Recomandare: Arweave pentru permanență garantată. Configurați contul și obțineți credite/AR tokens pentru upload.

### Pas 4: Upload-ați imaginea pe Arweave/IPFS
Pentru Arweave via Bundlr CLI: `npm install -g @bundlr-network/client`, finanțați Bundlr: `bundlr fund 1000000 -h https://node1.bundlr.network -c solana -w ~/token-keypairs/mint-keypair.json`, upload imagine: `bundlr upload logo.png -h https://node1.bundlr.network -c solana -w ~/token-keypairs/mint-keypair.json`. Notați URI-ul Arweave returnat (format: `https://arweave.net/[TX_ID]`). Verificați că imaginea este accesibilă la URI.

### Pas 5: Upload-ați fișierul metadata.json
Actualizați câmpul `image` în `metadata.json` cu URI-ul Arweave al imaginii obținut în Pasul 4. Upload-ați metadata.json pe Arweave: `bundlr upload metadata.json -h https://node1.bundlr.network -c solana -w ~/token-keypairs/mint-keypair.json`. Notați URI-ul metadata (format: `https://arweave.net/[TX_ID]`). Accesați URI-ul în browser și verificați că JSON-ul este servit corect cu toate câmpurile.

### Pas 6: Atașați metadata URI la Mint Account cu MetaBoss sau Metaplex Sugar
Instalați MetaBoss: `cargo install metaboss`. Utilizați comanda de update metadata: `metaboss update uri --keypair ~/token-keypairs/mint-keypair.json --account [ADRESA_MINT_TOKEN] --new-uri https://arweave.net/[METADATA_TX_ID] --rpc-url https://api.devnet.solana.com`. Verificați pe Solana Explorer că Metadata Account a fost creat și URI-ul este corect. Testați în Phantom wallet că tokenul afișează corect numele, simbolul și logo-ul.

### Pas 7: Verificați integrarea în ecosistem și documentați
Verificați că tokenul apare corect în: Phantom Wallet (import token by address), Solflare Wallet, Solana Explorer (metadata tab), Jupiter Aggregator (pentru swap). Dacă tokenul nu apare automat în wallet-uri, poate fi necesară înregistrarea în Solana Token List (github.com/solana-labs/token-list) sau Jupiter Strict List. Documentați toate URI-urile (imagine + metadata + TX hash-uri) și salvați în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Token metadata creat și uploadat: JSON metadata structurat conform Metaplex standard, imagine uploadată pe Arweave, metadata URI atașat la Mint Account, verificat în Phantom.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-026",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "solana", "token-launch", "metaplex", "metadata", "arweave", "nft-standard"],
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
- Metadata stocată pe server centralizat (AWS, GCP) în loc de Arweave/IPFS → violation
- Fișier metadata.json fără câmpurile obligatorii Metaplex → violation
- URI metadata neaccesat și neverificat înainte de atașare la Mint Account → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Metadata URI accesibil și returnează JSON valid?
- [ ] Token afișează corect în Phantom Wallet?

**VK-uri obligatorii:**
1. `✅ [PROC] C-026 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Create and Upload Token Metadata (Metaplex/MetaBoss)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Metaplex Token Metadata Standard | Standard pentru structura metadatelor |
| Bundlr Network / Arweave | Upload și stocare permanentă metadata și imagini |
| MetaBoss CLI | Atașarea URI metadatei la Mint Account |
| Phantom Wallet | Verificare afișare token în ecosistem |
| Solana Explorer | Verificare Metadata Account on-chain |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Metadata storage | Arweave sau IPFS (nu servere centralizate) |
| Wallet display verification | Testat în Phantom și Solflare |
