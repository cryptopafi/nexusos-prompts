---
type: procedure
created: 2026-03-22
status: active
slug: c-005-detect-rug-pull-scam
tags: [procedure, nexus]
---

# Detect a Rug Pull or Scam Token — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Identificarea semnalelor de avertizare ale unui rug pull sau token scam înainte de a investi capital.

---

## 1. Problema

Rug pull-urile și scam token-urile sunt responsabile pentru miliarde de dolari pierdute anual în crypto. Majoritatea pot fi detectate înainte de pierdere dacă se aplică o verificare sistematică a contractului, lichidității și comportamentului echipei.

Situații acoperite:
- Evaluarea unui token nou sau necunoscut înainte de prima cumpărare
- Identificarea semnalelor de alertă pe DexScreener sau Uniswap pentru tokene mici
- Verificarea unui token existent suspect după comportament de preț anormal

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții sau recomandare financiară. Criptomonedele sunt active volatile cu risc ridicat. Investiți doar ce vă puteți permite să pierdeți.

> **Safety Note**: Nu distribuiți niciodată cheile private sau seed phrase-ul cu nimeni. Nicio platformă legitimă nu le solicită.

---

## 2. Procedura

### Pas 1: Verifică contractul pe Etherscan / BSCScan
Caută adresa contractului pe Etherscan (ETH/Base) sau BSCScan (BSC). Verifică: este contractul verificat (cod sursă disponibil)? Dacă nu — stop. Citește funcțiile contractului sau folosește un tool automat (TokenSniffer, GoPlus Security).

### Pas 2: Rulează analiza automată de securitate
Accesează TokenSniffer.com sau GoPlus Security (gopluslabs.io). Introdu adresa contractului și analizează rezultatele. Red flags critice: honeypot detected, mint function necontrolată, owner poate modifica taxe la infinit, funcție de blacklist wallets.

### Pas 3: Verifică lichiditatea și lock-ul acesteia
Pe DexScreener sau Uniswap, verifică TVL (total value locked) în pool. Verifică dacă lichiditatea este locked pe un platform de tip Unicrypt sau Team.Finance — caută dovada lock-ului. Lichiditate unlocked sau în posesia echipei = rug pull posibil în orice moment.

### Pas 4: Analizează distribuția holder-ilor
Pe Etherscan, verifică tab-ul „Holders". Dacă top 10 wallets dețin >50% din supply (excepție wallet-urile de locked liquidity și burn) → semnal roșu major. Verifică dacă wallet-urile mari au activitate recentă de acumulare suspectă.

### Pas 5: Evaluează semnalele sociale și comportamentul echipei
Verifică vârsta conturilor de Twitter/X și Telegram ale proiectului. Echipe create cu 1-2 zile înainte de launch → semnal de alertă. Caută pe Google „[token name] scam / rug pull" — comunitatea raportează de obicei rapid.

### Pas 6: Testează cu sumă minimă (dacă continui)
Dacă ai trecut toate verificările și dorești să investești, începe cu o sumă MINIMĂ (maxim 10-20 USD echivalent). Verifică că poți vinde (swap-ul funcționează în ambele direcții — honeypot-urile permit cumpărare dar nu vânzare). Confirmă că tranzacția de vânzare se execută înainte de a crește poziția.

### Pas 7: Monitorizează și setează exit rapid
Setează un alert de preț pe DexScreener. Dacă lichiditatea scade brusc cu >20% sau echipa face anunțuri vagi de „pause" / „upgrade" neprogramat → ieșire imediată. Nu aștepta „revenirea" după un rug pull confirmat.

---

## 3. Cortex Logging

```json
{
  "text": "Executat C-005: Detect Rug Pull / Scam Token — contract verificat, lichiditate analizată, holder distribution evaluat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-005",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "security", "rug-pull", "scam-detection", "defi", "smart-contract"],
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
- Token cumpărat fără verificare contract pe Etherscan/BSCScan → violation critică
- Investiție fără verificarea lock-ului de lichiditate → violation critică
- Skip al analizei automate TokenSniffer/GoPlus → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Contract verificat pe Etherscan/BSCScan?
- [ ] Lichiditate lock verificată pe Unicrypt/Team.Finance?

**VK-uri obligatorii:**
1. `✅ [PROC] C-005 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Detect a Rug Pull or Scam Token" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Etherscan / BSCScan | Verificare contract și holder distribution |
| TokenSniffer / GoPlus Security | Analiză automată securitate contract |
| DexScreener | Monitorizare lichiditate și price action |
| Unicrypt / Team.Finance | Verificare lock lichiditate |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Verificare contract înainte de orice cumpărare | 100% cazuri |
| Test cu sumă minimă înainte de poziție mare | 100% tokene noi |
