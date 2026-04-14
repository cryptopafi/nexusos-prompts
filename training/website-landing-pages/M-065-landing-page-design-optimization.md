---
type: procedure
created: 2026-03-22
status: active
slug: m-065-landing-page-design-optimization
tags: [procedure, nexus]
---

# Landing Page Design and Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Design și optimizare landing page cu un singur obiectiv de conversie, de la structură până la testare cu heatmap.

---

## 1. Problema

O landing page eficientă trebuie să aibă un singur obiectiv clar, mesaj consistent cu sursa de trafic și elemente de conversie poziționate strategic. Fără un proces standard, landing page-urile sunt dezorganizate, au mesaje nealiniate cu ad-urile și rate de conversie slabe.

Situații acoperite:
- Creare landing page pentru campanie PPC (Google Ads / Meta Ads)
- Optimizare landing page existent cu CR scăzut
- Launch produs nou sau ofertă specială

---

## 2. Procedura

### Pas 1: Definește un singur obiectiv de conversie
Stabilește clar CE vrei să facă vizitatorul (1 acțiune per pagină):
- Completare formular lead
- Achiziție produs
- Download resource
- Înregistrare webinar

Documentează: obiectiv + metric primar de succes (CR target).

### Pas 2: Creează headline-ul aliniat cu sursa de trafic
Headline-ul trebuie să fie continuarea mesajului din ad sau link-ul de intrare (message match). Formulă recomandată: `[Beneficiu principal] + [Timp/Ușurință] + [Differentiator]`. Testează minimum 2 variante de headline înainte de lansare.

### Pas 3: Construiește secțiunea above-the-fold
Elementele obligatorii vizibile fără scroll:
- **Headline** — beneficiu principal clar
- **Subheading** — clarificare sau suport pentru headline
- **Hero image/video** — relevant pentru produsul/serviciul oferit
- **CTA primar** — buton cu text acțional (nu "Submit", ci "Obține acces gratuit")

Verifică că secțiunea above-fold este completă și convingătoare independent.

### Pas 4: Construiește secțiunea de trust
Adaugă dovezi sociale pentru a reduce fricțiunea:
- Testimoniale (cu foto + nume real + rezultat specific)
- Logo-uri clienți sau media (dacă există)
- Statistici și numere concrete ("10.000+ clienți mulțumiți")
- Certificate, premii, acreditări relevante

Poziționează trust section imediat sub above-fold sau lângă CTA principal.

### Pas 5: Creează urgență și secțiunea de beneficii
Listează 3-5 beneficii cheie (nu features) în format bullet. Adaugă element de urgență dacă este autentic:
- Timer countdown pentru ofertă limitată
- Număr limitat de locuri disponibile
- Bonus exclusiv pentru primii X cumpărători

Urgența falsă distruge credibilitatea — folosește doar dacă este reală.

### Pas 6: Plasează CTA strategic în 3 puncte
Repetă CTA-ul de minimum 3 ori pe pagină:
1. Above-fold (primul CTA)
2. Mijlocul paginii (după secțiunea de beneficii)
3. Sfârșitul paginii (după ultimul element de trust)

Fiecare CTA poate avea formulare ușor diferite dar același obiectiv.

### Pas 7: Elimină navigarea pentru a reduce distracțiile
Șterge sau ascunde meniu-ul de navigare standard de pe landing page. Păstrează doar logo (link opțional către home) și eventual link Privacy Policy în footer. Scopul: 0 ieșiri din pagină altele decât prin CTA.

### Pas 8: Testează cu heatmap tool
Instalează Hotjar sau Microsoft Clarity (gratuit) pe pagină. Colectează minimum 500 sesiuni. Analizează:
- Click heatmap — apasă lumea unde trebuie?
- Scroll heatmap — ajunge lumea la CTA-urile de jos?
- Session recordings — există friction points vizibile?

Aplică ajustări bazate pe date și repetă.

---

## 3. Cortex Logging

```json
{
  "text": "Landing Page Design & Optimization executat: obiectiv definit, headline creat cu message match, above-fold complet, trust section + urgency adăugate, CTA plasat de 3x, navigare eliminată, heatmap instalat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-065",
    "domain": "website-landing-pages",
    "rule_id": "META-H-002",
    "tags": ["landing-page", "cro", "design", "conversion", "heatmap", "cta"],
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

### HOW
- Procedura executată fără toți pașii → violation
- Output fără VK → violation
- Runner: Opus audit + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum website-landing-pages

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-065 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Landing Page Design and Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Hotjar / Microsoft Clarity | Heatmap și session recording |
| Google Optimize / Optimizely | A/B testing |
| Unbounce / Leadpages / WordPress | Platformă landing page |
| Google Analytics 4 | Tracking conversii |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CTA appearances | Minimum 3 pe pagină |
| Heatmap sessions | Minimum 500 |
| Message match score | Headline = Ad message |
