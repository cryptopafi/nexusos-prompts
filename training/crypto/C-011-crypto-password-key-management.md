---
type: procedure
created: 2026-03-22
status: active
slug: c-011-crypto-password-key-management
tags: [procedure, nexus]
---

# Implement Crypto Password and Key Management — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea unui sistem robust de management al parolelor și cheilor private/seed phrases pentru active crypto.

---

## 1. Problema

Pierderea accesului la active crypto din cauza managementului defectuos al cheilor/parolelor este ireversibilă — nu există „forgot password" în blockchain. Simultan, stocarea necorespunzătoare a seed phrase-urilor (digital, cloud, email) reprezintă cel mai frecvent vector de furt.

Situații acoperite:
- Configurarea inițială a unui sistem de stocare sigură a seed phrases și parole crypto
- Backup și recovery plan pentru acces la active crypto în caz de urgență
- Audit periodic al securității acceselor crypto existente

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

> **Safety Note**: Nu distribuiți niciodată cheile private sau seed phrase-ul cu nimeni. Nicio platformă legitimă nu le solicită.

---

## 2. Procedura

### Pas 1: Generează și notează seed phrase offline, imediat
La crearea oricărui wallet nou, notează cele 12/24 cuvinte seed phrase pe hârtie fizică, manual, cu pixul. Nu face screenshot, nu tipă, nu salva în notes/cloud/email/Telegram/WhatsApp. Verifică de două ori că ai notat corect citind înapoi.

### Pas 2: Creează backup-uri fizice multiple ale seed phrase-ului
Fă minim 2 copii fizice ale seed phrase-ului pe hârtie. Stochează-le în locuri SEPARATE (ex: acasă + la o persoană de încredere sau în seif bancar). Consideră gravarea pe metal inoxidabil pentru rezistență la foc/apă (produse: Cryptosteel, Bilodal).

### Pas 3: Nu stoca niciodată seed phrase digital
Regula absolută: seed phrase-ul nu există în nicio formă digitală. Nu în Google Drive, Dropbox, iCloud, email, note, fotografii, Word doc, 1Password sau orice aplicație. Singurul loc: fizic, offline, securizat. Orice stocare digitală = risc de hack.

### Pas 4: Folosește un password manager pentru parole exchange/conturi
Instalează Bitwarden (open source) sau 1Password pentru gestionarea parolelor conturilor exchange și email. Generează parole unice de minim 20 caractere pentru fiecare cont. Master password-ul managerului trebuie memorat — nu scris nicăieri digital.

### Pas 5: Configurează 2FA cu aplicație dedicată (nu SMS)
Activează 2FA pe toate exchange-urile și conturile crypto cu Google Authenticator sau Authy. Nu folosi 2FA prin SMS — SIM swapping este un atac frecvent. Exportă și stochează offline codurile de backup 2FA (printează și securizează fizic).

### Pas 6: Documentează structura acceselor fără a expune secretele
Creează un document fizic (nu digital) care descrie ce wallets ai, ce exchange-uri, unde sunt backup-urile fizice — fără a include seed phrases sau parole. Acesta ajută în caz de urgență (ex: moștenitori sau tu după o perioadă lungă). Stochează și el în loc sigur.

### Pas 7: Auditează securitatea crypto semestrial
La fiecare 6 luni: verifică că seed phrases sunt încă lizibile și intacte, că 2FA funcționează, că parolele nu au fost compromise (verifică pe haveibeenpwned.com). Actualizează parolele compromise imediat. Revizuiește cine știe unde sunt backup-urile fizice.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-011: Crypto Password și Key Management — seed phrase backup fizic creat, 2FA configurat, password manager setat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-011",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "security", "key-management", "seed-phrase", "2fa", "opsec"],
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
- Seed phrase stocat digital (foto/cloud/email) → violation critică
- 2FA prin SMS în loc de aplicație → violation
- Minim 2 backup-uri fizice în locuri separate neconfirmate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Seed phrase stocat EXCLUSIV fizic, minim 2 copii în locuri separate?
- [ ] 2FA configurat cu aplicație (nu SMS) pe toate conturile crypto?

**VK-uri obligatorii:**
1. `✅ [PROC] C-011 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Implement Crypto Password and Key Management" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Bitwarden / 1Password | Password manager pentru conturi exchange |
| Google Authenticator / Authy | 2FA fără SMS |
| Hardware wallet (Ledger/Trezor) | Stocare hardware pentru sume mari |
| Cryptosteel / Bilodal (opțional) | Backup metal rezistent pentru seed phrase |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Backup fizic seed phrase (min 2 locații) | 100% wallets |
| Audit securitate semestrial | 2x pe an |
