---
type: procedure
created: 2026-03-22
status: active
slug: c-034-advertise-crypto-platforms
tags: [procedure, nexus]
---

# Advertise on Crypto Platforms (DexScreener, DexTools) — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pentru crearea și rularea campaniilor publicitare plătite pe platformele DexScreener și DexTools pentru promovarea unui token crypto.

---

## 1. Problema

DexScreener și DexTools sunt platformele principale pe care traderii de retail le folosesc pentru a descoperi tokeni noi. Publicitatea plătită pe aceste platforme oferă vizibilitate directă unui public deja interesat de trading, dar fără o strategie clară și targeting corect, bugetul poate fi risipit.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- Token nou lansat care dorește vizibilitate pe platformele de tracking DEX
- Campanie de boost pentru un token existent care a pierdut tracțiune
- Promovare pentru un eveniment specific (listing CEX, major update, parteneriat)

---

## 2. Procedura

### Pas 1: Verifică eligibilitatea și cerințele platformelor
DexScreener Ads necesită: token listat activ cu volum real, website funcțional, contract verificat. DexTools Ads are cerințe similare. Verifică paginile de advertising ale fiecărei platforme pentru cerințele curente și prețurile pachetelor disponibile. Documentează opțiunile.

### Pas 2: Definește obiectivul campaniei și bugetul
Stabilește: scopul (awareness, holderi noi, volum, listing), durata campaniei, bugetul total (USD sau crypto acceptat). Calculează CPM estimat față de vizibilitatea așteptată. DexScreener oferă pachete de banner ads și "Promoted" placement — alege în funcție de buget.

### Pas 3: Creează materialele publicitare
Pregătește: banner grafic conform dimensiunilor specificate de fiecare platformă (verifică specs pe site-ul lor), copy scurt (tagline, CTA), și landing page (direct la pool sau la website proiect). Asigură-te că materialele nu conțin promisiuni de randamente sau claims false.

### Pas 4: Configurează campania pe DexScreener
Accesează dexscreener.com și caută opțiunea de advertising/boost. Completează formularul cu: adresa token, link campanie, materiale grafice, buget, durată. Efectuează plata (de obicei în crypto). Salvează confirmarea și dashboard link-ul campaniei.

### Pas 5: Configurează campania pe DexTools
Accesează dextools.io și secțiunea de advertising. Procesul este similar cu DexScreener. Urmărește specificațiile lor proprii pentru banner size și format. Documentează configurarea și plata.

### Pas 6: Monitorizează performanța campaniilor
Urmărește zilnic: impressii, click-uri, CTR (click-through rate), corelarea cu volumul de tranzacționare și holderi noi. Compară performanța celor două platforme. Ajustează dacă o platformă livrează semnificativ mai bine decât cealaltă.

### Pas 7: Compilează raportul de campanie și ROI
La finalul campaniei documentează: total impressii, total click-uri, CTR mediu, cost per click, creștere volum/holderi corelată cu perioada campaniei, ROI estimat. Evaluează dacă merită repetarea. Salvează raportul în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Campanii publicitare pe DexScreener și DexTools configurate și monitorizate. Materiale conforme, buget documentat, raport ROI completat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-034",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "advertising", "dexscreener", "dextools", "token-launch", "marketing"],
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
- Materiale publicitare cu claims false sau promisiuni de randamente → violation critică
- Campanie lansată fără monitorizare zilnică → violation
- Niciun raport de ROI la finalul campaniei → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Materialele publicitare verificate (fără claims false)?
- [ ] Raport ROI documentat la finalizare?

**VK-uri obligatorii:**
1. `✅ [PROC] C-034 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Advertise on Crypto Platforms (DexScreener, DexTools)" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| DexScreener Ads | Platformă publicitate principală |
| DexTools Ads | Platformă publicitate secundară |
| Canva / Figma | Creare materiale grafice |
| Analytics Dashboard | Monitorizare CTR și performanță |
| Spreadsheet | Tracking buget și ROI |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Platforme activate | 2/2 (DexScreener + DexTools) |
| CTR minim acceptabil | 0.5% |
| Monitorizare zilnică campanie | 100% zile active |
| Raport ROI la finalizare | Obligatoriu |
| Cost per click documentat | Da, pentru comparație viitoare |
