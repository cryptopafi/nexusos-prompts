---
type: procedure
created: 2026-03-22
status: active
slug: m-087-neuromarketing-pricing-strategy
tags: [procedure, nexus]
---

# Neuromarketing Pricing Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea principiilor de neuromarketing și psihologie cognitivă în structurarea prețurilor pentru maximizarea conversiei și a perceived value.

---

## 1. Problema

Prețurile setate fără înțelegerea psihologiei deciziei de cumpărare lasă bani pe masă și reduc conversia. Creierul uman procesează prețurile irațional, iar ignorarea acestor mecanisme cognitive — anchoring, decoy effect, charm pricing, framing — înseamnă dezavantaj competitiv măsurabil.

Situații acoperite:
- Structurarea inițială a prețurilor pentru un produs/serviciu nou
- Redesignul paginii de pricing pentru îmbunătățirea conversiei
- Optimizarea structurii de pachete pentru creșterea average order value

---

## 2. Procedura

### Pas 1: Anchoring — stabilirea prețului de referință
Identifică sau creează un preț ancoră vizibil înainte de prezentarea prețului țintă. Anchora poate fi: prețul original barato (înainte de discount), prețul concurentului premium, valoarea totală a componentelor individuale. Afișează anchora proeminent și taie-o vizual (strikethrough). Regula: anchora trebuie să fie credibilă și justificabilă, nu inventată.

### Pas 2: Decoy pricing — direcționarea spre opțiunea dorită
Dacă oferi 2+ pachete, adaugă un al treilea pachet "decoy" poziționat strategic pentru a face pachetul țintă să pară cea mai bună valoare. Decoy-ul clasic: pachet mediu la preț aproape de premium dar cu mult mai puțin decât premium → face premiumul să pară rezonabil. Testează configurația: Basic → Decoy → Premium cu prețul Premium ≤150% față de Decoy.

### Pas 3: Charm pricing și reducerea durerii de plată
Aplică charm pricing acolo unde este relevant: prețuri terminate în 7 sau 9 (497, 997, 29.99). Reduce "durerea" plății prin: plăți lunare afișate proeminent ("doar 49$/lună"), framing per zi ("mai puțin de o cafea pe zi"), echivalare cu alternativă mai costisitoare. Nu aplica charm pricing pentru produse premium/luxury unde prețurile rotunde comunică calitate.

### Pas 4: Framing și contextul comparativ
Prezintă prețul în cel mai avantajos cadru: față de costul problemei nerezolvate, față de costul alternativei (angajat intern, competitor), față de valoarea rezultatului obținut (ROI). Utilizează framing de câștig ("economisești 200$/lună") nu framing de pierdere pentru prețuri mid-range. Afișează prețul anual și lunar simultan pentru a permite comparația favorabilă.

### Pas 5: Reducerea fricțiunii cognitive la prețuri complexe
Simplifică structura de prețuri: maximum 3 opțiuni vizibile simultan (paradoxul alegerii). Evidențiază vizual opțiunea recomandată ("Most popular", "Best value"). Elimină comparațiile de features inutile care creează confuzie. Fiecare opțiune trebuie să aibă un avatar clar ("pentru freelanceri", "pentru echipe").

### Pas 6: Validarea prin testare
Testează A/B cel puțin 2 structuri de prețuri diferite (de ex. cu și fără decoy, charm vs. preț rotund). Măsoară: rata de conversie per pachet, distribuția alegerii între pachete, AOV total. Documentează rezultatele și aplică structura câștigătoare.

### Pas 7: Documentare și actualizare periodică
Documentează structura de prețuri finală cu raționalul psihologic pentru fiecare decizie. Programează review trimestrial: recalibrează anchore față de piață, verifică relevanța decoy-ului, actualizează framingul. Actualizează procedure-health.json și Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Strategie de pricing neuromarketing implementată: anchoring, decoy, charm pricing și framing aplicate cu validare A/B documentată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-087",
    "domain": "marketing-strategy",
    "rule_id": "META-H-002",
    "tags": ["neuromarketing", "pricing", "anchoring", "decoy-pricing", "charm-pricing", "psychology"],
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
- Ancoră de preț inventată fără bază credibilă → violation
- Structura de prețuri lansată fără validare A/B → violation
- Charm pricing aplicat pe produse luxury/premium → violation (erodare brand)
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum marketing-strategy

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Fiecare principiu psihologic aplicat are justificare documentată?
- [ ] Testare A/B planificată sau executată pentru structura finală?

**VK-uri obligatorii:**
1. `✅ [PROC] M-087 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Neuromarketing Pricing Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| A/B testing platform | Validarea structurii de prețuri |
| Analytics (GA4 / Mixpanel) | Tracking distribuție alegeri și conversie per pachet |
| Competitor research tools | Calibrarea ancorelor față de piață |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Uplift conversie față de pricing anterior | ≥10% |
| % clienți care aleg opțiunea target (middle/premium) | ≥50% |
| Review periodic executat | Trimestrial |
