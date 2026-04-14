---
type: procedure
created: 2026-03-22
status: active
slug: c-033-list-token-coingecko-cmc
tags: [procedure, nexus]
---

# List Your Token on CoinGecko and CoinMarketCap — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Ghid pas-cu-pas pentru aplicarea și aprobarea listării unui token pe CoinGecko și CoinMarketCap, incluzând cerințele de eligibilitate și procesul de aplicare.

---

## 1. Problema

Listarea pe CoinGecko și CoinMarketCap crește legitimitatea și descoperibilitatea unui token, dar procesul de aplicare are cerințe stricte pe care mulți fondatori nu le cunosc. Aplicațiile incomplete sau cu informații incorecte sunt respinse automat sau duc la delistare ulterioară.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

> **Note**: CoinGecko și CoinMarketCap nu sunt burse — listarea nu garantează lichiditate sau valoare. Listarea falsă sau cu informații incorecte poate duce la delistare.

Situații acoperite:
- Token nou lansat care aplică pentru prima dată pe ambele platforme
- Token existent care necesită actualizarea informațiilor (contract nou, rebranding)
- Proiect care urmărește verificarea suplimentară (CoinGecko Trust Score, CMC Verified)

---

## 2. Procedura

### Pas 1: Verifică eligibilitatea pre-aplicare
CoinGecko și CMC au cerințe minime: tokenul trebuie să fie listat activ pe un DEX/CEX cu volum real, trebuie să existe un website funcțional, whitepaper (sau lite paper), și contract verificat pe block explorer. Verifică toate acestea înainte de a completa formularul — o aplicație incompletă este respinsă imediat.

### Pas 2: Pregătește informațiile și asset-urile necesare
Colectează: adresa contractului verificat, chain-ul, numele și simbolul token-ului, supply total și circulant, descriere proiect (200-300 cuvinte în engleză), logo PNG (200×200px, fundal transparent), link website, link whitepaper, link-uri sociale (Twitter, Telegram, Discord), și adresa pool-ului principal de lichiditate.

### Pas 3: Aplică pe CoinGecko
Accesează coingecko.com/en/coins/new și completează formularul. Folosește "Request Form" oficial. Completează toate câmpurile obligatorii cu informații exacte și verificabile. Atașează logo-ul conform specificațiilor. Submit și salvează numărul de ticket/confirmare email.

### Pas 4: Aplică pe CoinMarketCap
Accesează coinmarketcap.com/request și completează formularul de listare. CMC are câmpuri suplimentare față de CoinGecko (inclusiv informații despre echipă și audit de securitate dacă există). Completează și submit. Salvează confirmarea.

### Pas 5: Monitorizează statusul aplicațiilor
Ambele platforme au timpi de procesare variabili (CoinGecko: 1-4 săptămâni, CMC: 2-6 săptămâni). Verifică email pentru update-uri. Nu trimite duplicate sau follow-up-uri agresive — pot întârzia procesarea. Notează datele de aplicare și deadlines estimate.

### Pas 6: Răspunde la solicitările suplimentare
Dacă echipa de review solicită informații suplimentare (volum suplimentar, audit raport, clarificări), răspunde prompt și complet. Furnizează documentația cerută în formatul solicitat. Niciodată nu furniza informații false sau exagerate.

### Pas 7: Confirmă listarea și actualizează profilul
La aprobare, verifică că toate informațiile afișate sunt corecte. Dacă există erori, folosește funcția de "Update Info" de pe platformă. Anunță listarea în comunitate. Documentează datele de listare și link-urile paginilor în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Aplicații listare token pe CoinGecko și CoinMarketCap submise. Toate informațiile verificate și corecte, asset-uri conforme specificațiilor.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-033",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "coingecko", "coinmarketcap", "listing", "token-launch"],
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
- Aplicație submisă fără verificarea eligibilității pre-aplicare → violation
- Logo sau informații furnizate în format neconform → violation
- Informații false sau exagerate în aplicație → violation critică
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Eligibilitate verificată pre-aplicare?
- [ ] Confirmare aplicație salvată pentru ambele platforme?

**VK-uri obligatorii:**
1. `✅ [PROC] C-033 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "List Your Token on CoinGecko and CoinMarketCap" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| CoinGecko Request Form | Portal aplicare listare CoinGecko |
| CoinMarketCap Request Form | Portal aplicare listare CMC |
| Block Explorer | Verificare contract pentru aplicație |
| Canva / Figma | Creare logo conform specificațiilor |
| Website și Whitepaper | Cerințe obligatorii de eligibilitate |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Platforme aplicate | 2/2 (CoinGecko + CMC) |
| Acuratețe informații aplicație | 100% verificat |
| Timp de răspuns la solicitări suplimentare | < 48h |
| Status aplicații monitorizat | Săptămânal până la aprobare |
