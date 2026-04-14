---
type: procedure
created: 2026-03-22
status: active
slug: m-031-facebook-business-manager-setup
tags: [procedure, nexus]
---

# Facebook Business Manager and Ad Account Setup — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurarea completă a Facebook Business Manager și a contului de reclame pentru operațiuni publicitare profesionale.

---

## 1. Problema

Conturile de reclame Facebook configurate incorect duc la suspendări, limitări de cheltuieli și performanță slabă. Fără o structură corectă în Business Manager, echipele pierd accesul la active critice și nu pot scala campaniile eficient.

Situații acoperite:
- Configurare inițială Business Manager pentru client nou sau business propriu
- Migrare cont personal de reclame în Business Manager
- Restructurare cont existent cu permisiuni și active dezordonate

---

## 2. Procedura

### Pas 1: Creare sau verificare Business Manager
Accesează business.facebook.com și creează un Business Manager nou sau verifică că cel existent are un cont verificat, număr de telefon și adresă de business completate. Verifică că Business Manager nu este restricționat sau în stare de review. Documentează Business Manager ID-ul din Settings > Business Info.

### Pas 2: Configurare profil business și verificare domeniu
Completează toate câmpurile din Business Settings: nume legal, adresă, site web și categorie de business. Adaugă domeniul principal în Brand Safety > Domains și finalizează verificarea prin meta-tag HTML sau DNS TXT record. Verificarea domeniului este obligatorie pentru campaniile cu pixel și retargeting.

### Pas 3: Creare Ad Account cu setări corecte
În Business Settings > Ad Accounts, creează un Ad Account nou cu moneda corectă, fusul orar al pieței țintă și numele descriptiv (ex: "ClientNume - RO - Main"). Setează limita de cheltuieli a contului la un nivel rezonabil pentru prima lună. Niciodată nu folosi Ad Account personal pentru campanii de business.

### Pas 4: Configurare metode de plată
Adaugă un card de business sau PayPal verificat ca metodă de plată principală. Setează o metodă de plată secundară pentru continuitate. Verifică că facturarea este configurată pe entitatea juridică corectă și că threshold-urile de facturare sunt setate la valori gestionabile (ex: 50 RON, 100 RON, 250 RON).

### Pas 5: Alocare roluri și permisiuni
Adaugă utilizatorii din echipă cu roluri minime necesare: Admin pentru lead-ul de cont, Advertiser pentru managerii de campanii, Analyst pentru raportare. Evită acordarea rolului de Admin multiplu. Adaugă agenția sau freelancer-ul ca Business Partner, nu ca utilizator direct, pentru a păstra controlul activelor.

### Pas 6: Conectare active — Pagina Facebook și Instagram
Conectează Pagina de Facebook a business-ului la Ad Account în Business Settings > Pages. Adaugă profilul de Instagram în Instagram Accounts. Verifică că ambele sunt conectate și că Ad Account are permisiune de a rula reclame în numele lor. Testează prin crearea unui draft de reclamă.

### Pas 7: Verificare finală și documentare
Rulează Business Manager Checklist: Business verificat, domeniu verificat, Ad Account activ, plată configurată, active conectate (Pagină + IG), pixel creat (chiar fără instalare încă), roluri alocate corect. Salvează un screenshot cu Business Settings Overview și documentează toate ID-urile (BM ID, Ad Account ID, Page ID) în fișierul de onboarding al clientului.

---

## 3. Cortex Logging

```json
{
  "text": "Facebook Business Manager și Ad Account configurat complet — BM verificat, domeniu verificat, Ad Account activ cu plată, active conectate, roluri alocate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-031",
    "domain": "paid-advertising-social",
    "rule_id": "META-H-002",
    "tags": ["facebook", "business-manager", "ad-account", "setup", "onboarding"],
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
- Ad Account creat fără verificarea domeniului → violation
- Roluri Admin acordate multiplu fără justificare → violation
- Active (Pagină, IG) neconectate la Ad Account → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-social

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Business Manager verificat și domeniu adăugat?
- [ ] Ad Account are metodă de plată activă și limite setate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-031 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Facebook Business Manager and Ad Account Setup" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Business Manager | Platformă centrală de management active |
| Facebook Ad Account | Cont de facturare și rulare campanii |
| Pagina Facebook | Identitate publică pentru reclame |
| Instagram Account | Placement suplimentar pentru reclame |
| Meta Business Verification | Validare entitate juridică |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Business Manager verificat | Da (în 48h) |
| Domeniu verificat | Da |
| Active conectate | Minim Pagină FB + IG |
| Timp setup complet | < 2 ore |
