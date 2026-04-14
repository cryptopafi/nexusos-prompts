---
type: procedure
created: 2026-03-22
status: active
slug: m-060-sales-video-script-writing
tags: [procedure, nexus]
---

# Sales Video Script Writing — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Scrierea de scripturi pentru video-uri de vânzare — VSL, video explainer, video de produs, sau video ad — care ghidează spectatorul prin procesul de decizie și generează conversii.

---

## 1. Problema

Video-urile de vânzare fără o structură narativă persuasivă dovedită au rate de conversie scăzute, indiferent de calitatea producției vizuale. Scriptul este fundația unui video de vânzare — un script slab nu poate fi salvat de editare bună, dar un script puternic poate converti chiar și cu producție modestă.

Situații acoperite:
- Scrierea unui Video Sales Letter (VSL) pentru landing page
- Scrierea scriptului pentru un video ad (YouTube, Facebook, TikTok)
- Scrierea unui video explainer pentru un produs SaaS sau serviciu complex

---

## 2. Procedura

### Pas 1: Definirea obiectivelor și contextului de vizionare
Stabilește cu precizie: obiectivul video-ului (click pe CTA, completare formular, achiziție directă), platforma și contextul de vizionare (cold traffic pe YouTube ad vs. warm traffic pe landing page), durata optimă pentru context (6-15 sec pentru pre-roll ad, 60-90 sec pentru explainer, 10-30 min pentru VSL), și buyer persona cu awareness level (problem-aware, solution-aware, product-aware). Obiectivele clare determină structura narativă corectă.

### Pas 2: Construirea hook-ului — primele 5-10 secunde
Hook-ul determină dacă spectatorul continuă sau pleacă. Scrie minimum 3 variante de hook și testează-le mental: (1) hook cu problemă dureroasă ("Dacă și tu..."), (2) hook cu promisiune de beneficiu concret ("Descoperă cum să..."), (3) hook cu curiozitate sau paradox ("Ce fac top 1% din antreprenori diferit față de restul"). Alege hook-ul cel mai relevant pentru awareness level-ul audienței. Hook-ul slab = videoclip ignorat înainte de a transmite mesajul.

### Pas 3: Structurarea narativă cu framework persuasiv
Aplică unul din framework-urile dovedite în funcție de tipul video-ului. Pentru VSL: AIDA (Atenție → Interes → Dorință → Acțiune) sau PAS (Problemă → Agitare → Soluție). Pentru explainer: "Lumea actuală → Problema → Soluția ta → Cum funcționează → CTA". Pentru ad: Hook → Conflict → Rezoluție → Offer. Structura narativă ghidează spectatorul psihologic spre decizia de cumpărare.

### Pas 4: Scrierea body-ului cu elemente de persuasiune
Construiește body-ul scriptului cu: (1) amplificarea problemei în limbajul clientului (pain points identificate în M-053), (2) prezentarea soluției ca transformare (before/after), (3) credibilitate și dovezi (testimoniale, statistici, demo), (4) obiecții frecvente anticipate și neutralizate, (5) urgență sau scarcity legitimă (dacă există ofertă limitată). Fiecare paragraph trebuie să avanseze argumentul spre decizia de cumpărare.

### Pas 5: Scrierea ofertei și CTA-ului
Formulează oferta clar și specific: ce primesc exact, la ce preț, ce garanții există, ce bonusuri sunt incluse, și până când este valabilă. CTA-ul trebuie să fie un singur pas clar: "Click pe butonul de mai jos", "Sună acum la...", "Completează formularul". CTA-ul ambiguu sau multiplu diluează conversia. Repetă CTA de minimum 2-3 ori în VSL-urile lungi (la jumătate și la final).

### Pas 6: Optimizarea scriptului pentru video
Rescrie scriptul pentru voce: propoziții scurte (maximum 15 cuvinte), limbaj conversațional (nu literar), tranziții naturale, și variație de ritm. Citește scriptul cu voce tare și marchează locurile care sună forțat sau neclar. Adaugă notații de regie: [pauză], [arată produsul], [insert testimonial], [B-roll demonstrație] pentru a ghida producția video. Durata estimată: ~150 cuvinte = ~1 minut.

### Pas 7: Review, validare și versioning
Revizuiește scriptul față de buyer persona și obiectivul stabilit în Pas 1 — fiecare propoziție trebuie să avanseze decizia de cumpărare. Trimite la review stakeholder cu checklist: hook clar? ofertă specifică? CTA unic? obiecții neutralizate? tonul corect pentru audiență? Salvează versiunile de script cu numerotare clară (v1.0, v1.1) pentru A/B testing viitor. Documentează scriptul câștigător cu metricile de conversie asociate.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-060 executată: Script video de vânzare complet — hook, structură narativă persuasivă, ofertă clară și CTA specific scrise și validate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-060",
    "domain": "video-marketing",
    "rule_id": "META-H-002",
    "tags": ["sales-video", "VSL", "script-writing", "copywriting", "conversion"],
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
- Script video de vânzare fără CTA explicit → violation
- Video publicat fără thumbnail optimizat → violation
- Hook-ul nu este testat în minimum 3 variante → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum video-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Scriptul citit cu voce tare și optimizat pentru audio?
- [ ] Versioning corect al scriptului (v1.0+) pentru tracking A/B?

**VK-uri obligatorii:**
1. `✅ [PROC] M-060 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Sales Video Script Writing" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Buyer Persona (M-053) | Pain points și limbajul clientului pentru script |
| Brand Story Framework (M-054) | Narrative și messaging aliniate |
| Video Production Workflow (M-059) | Producția video-ului bazat pe script |
| Google Docs / Notion | Editare și versioning scripturi |
| Loom / Descript | Preview rapid și editare script-based |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| View-through rate (VTR) | > 30% pentru video ad |
| Conversion rate VSL | > 2% (cold traffic), > 5% (warm traffic) |
| A/B test hook variante | Minimum 2 variante testate |
| Timp scriere script standard | < 3 ore (1000-1500 cuvinte VSL) |
