---
type: procedure
created: 2026-03-22
status: active
slug: c-024-solana-token-keypairs
tags: [procedure, nexus]
---

# Create Solana Token Key Pairs — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru generarea și gestionarea în siguranță a keypair-urilor necesare creării unui token pe Solana, incluzând mint authority, freeze authority și token keypair.

---

## 1. Problema

Pe Solana, fiecare token este reprezentat de un Mint Account cu un keypair asociat — adresa acestui keypair devine adresa permanentă a tokenului. Generarea și securizarea greșită a acestor keypair-uri poate face tokenul inaccesibil (pierderea mint authority = imposibilitatea de a minta tokeni noi) sau vulnerabil (expunerea keypair-ului mint authority permite oricui să minteze tokeni neautorizat).

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat de pierdere totală a investiției.

Situații acoperite:
- Generarea keypair-ului pentru mint account (adresa tokenului)
- Generarea și configurarea mint authority și freeze authority
- Securizarea și backup-ul tuturor keypair-urilor critice

---

## 2. Procedura

### Pas 1: Înțelegeți structura de keypair-uri necesare pentru un token Solana
Pe Solana un token necesită: (1) Mint Keypair — adresa publică devine adresa tokenului, cheia privată este necesară la creare; (2) Mint Authority — walletul cu dreptul de a minta tokeni noi (poate fi setat la un multisig sau revocat după supply fix); (3) Freeze Authority — walletul cu dreptul de a îngheța conturi (opțional — pentru tokeni fără freeze, setați la `None`). Documentați rolul fiecărei chei.

### Pas 2: Generați Mint Keypair cu Solana CLI
Rulați în terminal: `solana-keygen new --outfile ~/token-keypairs/mint-keypair.json --no-bip39-passphrase`. Notați adresa publică afișată — aceasta va fi adresa permanentă a tokenului pe Solana. Alternativ, pentru a genera un "vanity address" (ex: care începe cu un prefix specific): `solana-keygen grind --starts-with TOKEN:1`. Stocați fișierul JSON în locație sigură — NU în directoare sincronizate cu cloud sau git.

### Pas 3: Verificați integritatea keypair-ului generat
Verificați că keypair-ul generat este valid: `solana-keygen verify ADRESA_PUBLICA ~/token-keypairs/mint-keypair.json`. Output-ul trebuie să fie "Verification: succeeded". Afișați adresa publică: `solana-keygen pubkey ~/token-keypairs/mint-keypair.json`. Salvați adresa publică separat (nu necesită securitate specială — este publică). Confirmați că fișierul JSON conține exact 64 de bytes (array de 64 numere).

### Pas 4: Configurați Mint Authority (walletul cu drept de mint)
Implicit, mint authority va fi walletul configurat în `solana config` (cel din C-023). Dacă doriți un multisig ca mint authority (recomandat pentru proiecte serioase): utilizați Squads Protocol (squads.so) pentru a crea un multisig 2/3 sau 3/5. Documentați adresa mint authority. Planificați dacă și când veți revoca mint authority (supply fix) — după revocare nu mai puteți minta tokeni suplimentari.

### Pas 5: Decideți și documentați Freeze Authority
Evaluați dacă tokenul necesită freeze authority: utility tokens și governance tokens tipic NU au nevoie de freeze; payment tokens reglementati pot necesita freeze pentru conformitate (KYC/AML). Dacă nu este necesară: setați `--freeze-authority None` la crearea mint-ului. Documentați decizia în Token Strategy Document (C-018). Freeze authority nerevocată este un red flag pentru comunitate.

### Pas 6: Creați backup securizat al tuturor keypair-urilor
Exportați conținutul fișierelor JSON pentru backup: folosiți `cat ~/token-keypairs/mint-keypair.json` și copiați array-ul de bytes. Stocați în minim 2 locații fizice diferite: encrypted USB drive, hardware security module (HSM), sau password manager cu backup offline (Bitwarden cu backup export). Nu stocați niciodată în: email, Google Drive, Dropbox, GitHub, sau orice cloud neencriptat. Testați recuperarea din backup.

### Pas 7: Documentați registrul de keypair-uri și pregătiți pentru mint
Creați un document securizat (offline sau criptat) cu: Adresa publică a Mint Keypair, Locația fișierului JSON (path local), Data generării, Adresa Mint Authority, Adresa Freeze Authority (sau None), Network de target (devnet/mainnet). Pregătiți comenzile CLI pentru pasul următor (C-025 Mint): `spl-token create-token --mint-authority ADRESA_AUTH ~/token-keypairs/mint-keypair.json`. Salvați documentul în Cortex (FĂRĂ cheile private).

---

## 3. Cortex Logging

```json
{
  "text": "Solana token keypairs generate: mint keypair creat și verificat, mint authority configurată, freeze authority documentată, backup securizat realizat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-024",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "solana", "keypair", "token-launch", "mint-authority", "security", "spl-token"],
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
- Keypair JSON stocat în cloud neencriptat sau repository Git → violation critică de securitate
- Mint authority configurată fără documentarea planului de revocare → violation
- Backup keypair netestată pentru recuperare → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Keypair verificat cu `solana-keygen verify`?
- [ ] Backup în minimum 2 locații fizice offline?

**VK-uri obligatorii:**
1. `✅ [PROC] C-024 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Create Solana Token Key Pairs" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Solana CLI (solana-keygen) | Generare și verificare keypair-uri |
| Squads Protocol | Multisig pentru mint authority (proiecte serioase) |
| Password Manager (Bitwarden) | Backup criptat al keypair-urilor |
| SPL Token CLI | Utilizare keypair la crearea token-ului |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Backup-uri keypair | Minimum 2 locații fizice offline |
| Verificare keypair | 100% keypairs verificate cu `solana-keygen verify` |
