---
type: procedure
created: 2026-03-22
status: active
slug: m-034-facebook-instagram-ad-campaign
tags: [procedure, nexus]
---

# Facebook/Instagram Ad Campaign Creation and Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Crearea, lansarea și optimizarea sistematică a campaniilor de reclame Facebook și Instagram pentru maximizarea ROAS și scalare profitabilă.

---

## 1. Problema

Campaniile create fără o structură clară și fără procese de optimizare definite risipesc bugetul în faza de learning și nu scalează profitabil. Deciziile de optimizare luate prea devreme sau pe baza datelor insuficiente duc la întreruperea learning phase-ului și la performanță degradată.

Situații acoperite:
- Lansare campanie nouă pentru produs sau serviciu
- Restructurare campanie existentă cu performanță în scădere
- Scalare campanie profitabilă fără a destabiliza ROAS

---

## 2. Procedura

### Pas 1: Definire obiectiv și KPI-uri înainte de creare
Stabilește obiectivul campaniei înainte de a deschide Ads Manager: ROAS target, CPA maxim acceptabil, buget zilnic și total, fereastra de testare (minim 7 zile, ideal 14 zile). Calculează break-even ROAS din marja brută a produsului. Documentează aceste cifre — ele vor fi standardul de optimizare, nu intuiția.

### Pas 2: Structurare campanie (Campaign → Ad Set → Ad)
Creează campania cu obiectivul corect: Sales (conversii site), Leads (formulare), Traffic (trafic site), Awareness (reach). Setează Campaign Budget Optimization (CBO) pentru campanii cu multiple ad sets de testare. Creează ad set-uri separate pentru audiențe distincte (nu amesteca audiențe reci cu audiențe calde în același ad set). Regula: 1 audiență per ad set.

### Pas 3: Configurare ad set — targeting, plasamente și buget
Setează targeting-ul la nivel de ad set: audiența definită în M-033, locație (țară/regiune/raza km), vârstă și gen dacă sunt relevante, limba. Folosește Advantage+ Placements pentru campaniile de conversii (permite algoritmului să optimizeze). Evită manual placements în faza de testare. Setează bugetul zilnic la minim 5x CPA target pentru a permite learning phase-ul.

### Pas 4: Creare reclame — formate și copywriting
Creează minim 3-5 variante de reclamă per ad set: minim 1 video (Reels-format 9:16 pentru Instagram, 1:1 pentru Feed), 1-2 imagini statice, 1 carousel dacă e relevant. Scrie headline cu beneficiul principal (nu funcționalitate). Primary text: problem → solution → CTA. Adaugă minim 3 variante de headline și 3 de descriere pentru Dynamic Creative. Verifică că toate reclamele au CTA button setat corect.

### Pas 5: Lansare și monitorizare learning phase
Publică campania și monitorizează zilnic fără a face modificări în primele 7 zile (learning phase necesită minim 50 de conversii per ad set per săptămână). Verifică doar că reclamele sunt aprobate și că pixelul trimite date. Documentează CPM, CTR și Cost per Result zilnic fără a acționa pe date incomplete. Modificările în learning phase resetează algoritmul.

### Pas 6: Optimizare post-learning (după 7-14 zile)
Analizează performanța la nivel de ad set și reclamă după minim 50 de conversii. Oprește reclamele cu CTR sub 1% (Feed) sau sub 1.5% (Stories/Reels) și costul per conversie de 2x peste CPA target. Scalează ad set-urile profitabile cu maxim 20% buget la fiecare 3 zile. Testează audiențe noi în ad set-uri noi, nu modifica ad set-urile performante.

### Pas 7: Raportare și optimizare continuă
Setează rapoarte personalizate în Ads Manager cu coloanele: Reach, Impressions, CPM, CTR (Link), CPC, Cost per Result, ROAS, Spend, Revenue. Creează un raport săptămânal cu deciziile luate și justificarea lor. Verifică frecvența reclamelor (dacă depășește 3.0 pentru audiențe mici, reîmprospătează creativele). Rulează un test A/B formal lunar pentru o variabilă (audiență sau creativ).

---

## 3. Cortex Logging

```json
{
  "text": "Campanie Facebook/Instagram creată și optimizată — structură CBO, audiențe distincte per ad set, minim 3 variante creative, learning phase respectat, optimizare post-50 conversii aplicată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-034",
    "domain": "paid-advertising-social",
    "rule_id": "META-H-002",
    "tags": ["facebook", "instagram", "ads", "campaign", "optimization", "ROAS", "CBO"],
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
- Modificări făcute în campanie în primele 7 zile (learning phase) fără justificare critică → violation
- Buget ad set setat sub 5x CPA target → violation
- Audiențe calde și reci mixate în același ad set → violation
- Scalare buget cu peste 20% fără perioada de stabilizare de 3 zile → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum paid-advertising-social

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] KPI-uri și CPA target documentate înainte de lansare?
- [ ] Minim 3 variante creative per ad set?

**VK-uri obligatorii:**
1. `✅ [PROC] M-034 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Facebook/Instagram Ad Campaign Creation and Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Facebook Ads Manager | Creare și management campanii |
| Facebook Pixel + CAPI | Tracking conversii și semnale algoritm |
| Audiences Manager (M-033) | Sursă audiențe pentru targeting |
| Business Manager (M-031) | Container cont și active |
| Creative assets | Imagini și video-uri pentru reclame |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CTR (Link Click) Feed | > 1.0% |
| CTR (Link Click) Stories/Reels | > 1.5% |
| ROAS | > break-even ROAS calculat |
| Frecvență reclame | < 3.0 per audiență |
| Cost per Result | < CPA target setat |
