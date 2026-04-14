---
type: procedure
created: 2026-03-22
status: active
slug: c-038-bitcoin-lightning-wallet
tags: [procedure, nexus]
---

# Set Up Bitcoin Lightning Wallet for Payments — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pas-cu-pas pentru configurarea și utilizarea unui Bitcoin Lightning Network wallet pentru plăți rapide și cu comisioane mici.

---

## 1. Problema

Bitcoin on-chain este prea lent și costisitor pentru plăți de zi cu zi (confirmări 10-60 minute, fee-uri $1-$50+). Lightning Network rezolvă aceste probleme permițând tranzacții instant și aproape gratuite, dar configurarea corectă necesită pași specifici pe care utilizatorii obișnuiți îi ignoră, ducând la pierderi de fonduri sau utilizare incorectă.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Utilizator care dorește să primească plăți în Bitcoin prin Lightning
- Merchant care vrea să accepte Lightning pentru produse/servicii
- Developer care integrează Lightning în aplicații sau API-uri

---

## 2. Procedura

### Pas 1: Alege wallet-ul Lightning potrivit cazului tău de utilizare
Opțiuni principale: Phoenix Wallet (self-custody, beginner-friendly, iOS/Android), Breez Wallet (self-custody, features avansate), Muun (hibrid on-chain + Lightning), Wallet of Satoshi (custodial, cel mai simplu, ideal pentru începători). Alege Phoenix sau Breez pentru self-custody, WoS dacă prioritizezi simplitatea. Documentează alegerea.

### Pas 2: Instalează și configurează wallet-ul
Descarcă wallet-ul ales din App Store / Google Play oficial. Urmează procesul de onboarding: generare seed phrase (12 sau 24 cuvinte). Scrie seed phrase-ul pe hârtie și stochează în minim 2 locuri fizice sigure — NICIODATĂ digital (screenshot, cloud, email). Verifică backup-ul.

### Pas 3: Înțelege și configurează canalele Lightning (pentru wallet-uri non-custodial)
Pentru Phoenix/Breez: la primul deposit, wallet-ul deschide automat un canal Lightning cu un LSP (Lightning Service Provider). Există o taxă de deschidere canal (de obicei 0.1-1% din suma depusă). Înțelege conceptul de inbound/outbound liquidity — ai nevoie de inbound liquidity pentru a primi plăți.

### Pas 4: Finanțează wallet-ul cu Bitcoin
Generează o adresă on-chain de deposit în wallet. Trimite o sumă mică de test (ex. 0.001 BTC) de pe un exchange sau alt wallet. Verifică că fondurile au sosit (poate dura 1-3 confirmări on-chain). Testează o tranzacție Lightning mică (ex. achită o factură de testare de pe testnet sau un serviciu real de $0.01).

### Pas 5: Generează și testează o invoice Lightning
Crează o invoice (factură Lightning) în wallet: specifică suma, descrie opțional, generează QR code. Testează primind o plată mică de la alt wallet (poți folosi un serviciu de faucet Lightning pentru test gratuit). Verifică că plata a sosit instant.

### Pas 6: Configurează backup-ul și recuperarea
Verifică că wallet-ul are backup automat al stării canalelor (SCB — Static Channel Backup). Phoenix și Breez fac acest backup automat. Înțelege că dacă pierzi telefonul, poți recupera fondurile on-chain cu seed phrase-ul, dar canalele Lightning trebuie redeschise. Documentează procesul de recovery.

### Pas 7: Testează scenariul complet de plată și documentează în Cortex
Execută un ciclu complet: primește o plată → trimite o plată → verifică fee-urile și viteza. Documentează: wallet ales, data setup, suma test, fee-urile plătite, experiența generală. Salvează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Bitcoin Lightning wallet configurat. Self-custody, seed phrase backup fizic, canal deschis, test plată incoming și outgoing executat cu succes.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-038",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "bitcoin", "lightning-network", "wallet", "personal-finance", "payments"],
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
- Seed phrase stocată digital (screenshot, cloud) → violation critică de securitate
- Niciun test de plată executat înainte de utilizare reală → violation
- Configurare fără înțelegerea inbound/outbound liquidity → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Seed phrase backup fizic confirmat (nu digital)?
- [ ] Test plată incoming și outgoing executat?

**VK-uri obligatorii:**
1. `✅ [PROC] C-038 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Set Up Bitcoin Lightning Wallet for Payments" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Phoenix Wallet / Breez / WoS | Wallet Lightning de ales |
| Bitcoin on-chain wallet | Sursă pentru finanțarea inițială |
| Lightning Faucet (testnet) | Test plată gratuită |
| Hârtie + pix | Backup fizic seed phrase |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Seed phrase backup locuri fizice | Minim 2 |
| Test plată incoming | Executat și confirmat |
| Test plată outgoing | Executat și confirmat |
| Fee per tranzacție Lightning | < $0.01 (target Lightning) |
| Timp setup complet | < 30 minute |
