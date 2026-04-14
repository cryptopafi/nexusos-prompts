---
type: procedure
created: 2026-03-22
status: active
slug: m-021-google-search-ads-campaign
tags: [procedure, nexus]
---

# Google Search Ads Campaign Creation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea și lansarea unei campanii Google Search Ads de la zero, de la obiectiv la primul anunț activ.

---

## 1. Problema

Campaniile Search Ads create fără un proces structurat ajung să consume buget fără să genereze conversii din cauza keyword targeting incorect, anunțuri irelevante sau lipsa negative keywords. Un setup greșit de la început este costisitor de corectat ulterior. Această procedură garantează că orice campanie Search este configurată corect din prima zi.

Situații acoperite:
- Lansarea primei campanii Search pentru un cont nou
- Adăugarea unei campanii noi pentru un produs sau serviciu suplimentar
- Recrearea unei campanii existente cu performanță slabă

---

## 2. Procedura

### Pas 1: Definește obiectivul campaniei și KPI-ul principal
Alege obiectivul clar: Sales, Leads sau Website Traffic. KPI-ul principal trebuie definit numeric înainte de lansare (ex: CPA target sub 50 RON, ROAS target 400%). Fără obiectiv numeric, nu poți evalua succesul campaniei. Documentează obiectivul în Cortex.

### Pas 2: Efectuează keyword research și segmentare
Folosește Google Keyword Planner pentru a identifica minimum 50 de keyword-uri relevante. Segmentează-le în grupuri tematice de maximum 5-7 termeni înrudiți. Separă brand keywords de non-brand și competitor terms în Ad Groups distincte. Notează volumul lunar și CPC estimat pentru fiecare grup.

### Pas 3: Construiește lista de negative keywords
Creează o listă de negative keywords cu minimum 30 de termeni irelevanti (ex: "gratuit", "job", "cursuri", termeni din alte industrii). Adaugă negative keywords la nivel de campanie, nu doar Ad Group. Creează o Shared Negative Keywords List dacă rulezi multiple campanii pe același cont.

### Pas 4: Configurează setările campaniei
Setează: Campaign type = Search, Network = dezactivează Display Network, Locations = exact (nu "people interested in"), Language, Budget zilnic, Start date. Activează Conversion Goals corespunzătoare. Dezactivează Search Partners dacă ești la setup inițial și nu ai date de performanță.

### Pas 5: Creează Ad Groups și scrie Responsive Search Ads
Pentru fiecare grup de keywords creat la Pas 2, creează un Ad Group separat. Scrie minimum 2 RSA per Ad Group: 15 headlines (minimum 3 cu keyword inclus), 4 descriptions. Folosește pin la Headline 1 cu keyword-ul principal. Ad Strength trebuie să fie "Good" sau "Excellent" înainte de salvare.

### Pas 6: Adaugă Assets (Extensions) obligatorii
Adaugă minimum 4 tipuri de assets: Sitelinks (minimum 4), Callouts (minimum 4), Structured Snippets, Call Extension (dacă ai număr de telefon). Assets cresc CTR-ul cu 10-20% fără cost suplimentar. Completează toate câmpurile disponibile pentru fiecare asset.

### Pas 7: Setează bid strategy și lansează cu monitoring plan
Alege bid strategy corectă pentru faza de setup: Manual CPC sau Maximize Clicks pentru primele 2 săptămâni (fără date de conversie suficiente). Trece la Smart Bidding (Target CPA / Target ROAS) după minimum 30 de conversii. Setează un reminder de review la 48h și 7 zile după lansare. Documentează toate setările în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "M-021 executat: Campanie Google Search Ads creată. Campanie: [Nume]. Ad Groups: [N]. Keywords: [N]. Negative keywords: [N]. Assets adăugate: [tipuri]. Bid strategy: [tip]. CPA/ROAS target: [valoare]. Status: ACTIV.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-021",
    "domain": "paid-advertising-google",
    "rule_id": "META-H-002",
    "tags": ["google-ads", "search-ads", "rsa", "keyword-research", "negative-keywords", "bid-strategy"],
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
- Campanie lansată cu Display Network activat → violation
- RSA creat cu Ad Strength sub "Good" → violation
- Campanie lansată fără minimum 30 de negative keywords → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-google

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Display Network dezactivat în setările campaniei?
- [ ] Minimum 4 tipuri de assets adăugate?

**VK-uri obligatorii:**
1. `✅ [PROC] M-021 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Search Ads Campaign Creation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Ads Account | Platformă principală |
| Google Keyword Planner | Research volum și CPC |
| Conversion Tracking (M-022) | Prerequisit pentru Smart Bidding |
| Google Ads Editor | Bulk management ad groups și keywords |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CTR minim la 30 zile | Minim 3% pentru Search |
| Ad Strength RSA | "Good" sau "Excellent" 100% |
| Quality Score mediu | Minim 7/10 per keyword activ |
