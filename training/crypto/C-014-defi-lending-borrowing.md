---
type: procedure
created: 2026-03-22
status: active
slug: c-014-defi-lending-borrowing
tags: [procedure, nexus]
---

# Use DeFi Lending and Borrowing — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Utilizarea corectă a protocoalelor DeFi de lending și borrowing (Aave, Compound) cu gestionarea riscului de lichidare.

---

## 1. Problema

DeFi lending și borrowing oferă oportunități de yield și leverage, dar riscul de lichidare este real și rapid în piețe volatile. Un collateral ratio insuficient poate duce la pierderea parțială sau totală a colateralului în câteva ore de la o mișcare de piață adversă.

Situații acoperite:
- Depunerea de colateral și împrumutarea de stablecoins pe Aave/Compound
- Monitorizarea health factor și gestionarea riscului de lichidare
- Utilizarea strategiei de lending simplu (fără borrowing) pentru randament

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

---

## 2. Procedura

### Pas 1: Înțelege mecanismul și termenii cheie
Înainte de orice acțiune, înțelege: Collateral = activul depus ca garanție, Borrow = suma împrumutată, Health Factor = raportul colateral/datorie (Aave: sub 1 = lichidare), Liquidation Threshold = procentul la care lichidarea se declanșează. Pe Aave, menținerea Health Factor >2 este ținta minimă sigură.

### Pas 2: Evaluează protocolul și auditurile de securitate
Folosește EXCLUSIV protocoale majore, auditate și cu track record: Aave v3, Compound v3, MakerDAO. Verifică pe DeFiLlama că TVL-ul este consistent și nu a scăzut brusc. Nu experimenta cu protocoale noi de lending fără minimum 6 luni de existență și audit Certik/Trail of Bits.

### Pas 3: Calculează collateral ratio țintă minim 200%
Collateral ratio = (valoare colateral / valoare împrumut) × 100. Ținta minimă: 200% (dai în împrumut maxim 50% din valoarea colateralului). Recomandat conservator: 300% (dai în împrumut maxim 33% din valoare). La volatilitate mare a colateralului, folosește 400%+.

### Pas 4: Depune colateral și împrumută cu limita calculată
Conectează wallet-ul la Aave (app.aave.com — URL direct, verificat). Aprobă contractul pentru tokenul colateral, depune suma. Calculează suma maximă de împrumutat bazat pe collateral ratio țintă și împrumută mai puțin decât maximul permis de protocol. Verifică Health Factor afișat după tranzacție.

### Pas 5: Configurează alerte de Health Factor
Setează alerte pentru Health Factor pe DeFiSaver, Instadapp sau direct prin DeFi Saver Automation. Alertă la HF < 1.5 → adaugă colateral sau rambursează parțial. Alertă la HF < 1.2 → acțiune imediată obligatorie. Niciodată nu lăsa o poziție de borrowing nemonitorizată.

### Pas 6: Monitorizează zilnic în piețe volatile
Verifică Health Factor zilnic când piața este volatilă. Pregătește-te să adaugi colateral rapid (ai fonduri disponibile în wallet). Dacă nu poți monitoriza activ o poziție leveraged, folosește DeFi Saver Automation pentru protecție automată sau închide poziția.

### Pas 7: Rambursare și management al poziției
Rambursează împrumutul înainte de expirarea oricărei date sau când collateral ratio scade sub 200%. Calculează că ai plătit dobânda acumulată (APY variabil pe Aave). La închiderea poziției, retrage colateralul și verifică că wallet-ul reflectă balanța corectă.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-014: DeFi Lending și Borrowing — collateral ratio 200%+ menținut, Health Factor monitorizat, alerte configurate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-014",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "defi", "lending", "borrowing", "aave", "collateral", "liquidation-risk"],
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
- Collateral ratio sub 150% la DeFi lending → violation critică
- Poziție de borrowing fără alerte de Health Factor configurate → violation critică
- Utilizarea unui protocol de lending neauditat sau cu TVL sub $100M → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto
- C-012 → prerequisit pentru wallet DeFi configurat
- C-007 → selectarea stablecoin-ului de împrumutat

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Collateral ratio minim 200% menținut la intrarea în poziție?
- [ ] Alerte Health Factor configurate înainte de a lăsa poziția nesupravegheată?

**VK-uri obligatorii:**
1. `✅ [PROC] C-014 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Use DeFi Lending and Borrowing" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Aave v3 / Compound v3 | Protocol DeFi lending/borrowing |
| DeFi Saver | Automation și alerte Health Factor |
| DeFiLlama | Verificare TVL și sănătate protocol |
| C-012 | Wallet DeFi configurat (prerequisit) |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Collateral ratio minim | 200% (recomandat 300%) |
| Health Factor minim la orice moment | >1.5 (țintă >2.0) |
