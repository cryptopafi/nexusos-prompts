---
type: procedure
created: 2026-03-22
status: active
slug: m-086-website-heatmap-analysis
tags: [procedure, nexus]
---

# Website Heatmap Analysis for Conversion Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Analiză sistematică a heatmap-urilor și session recordings pentru identificarea problemelor UX și formularea ipotezelor de optimizare a conversiei.

---

## 1. Problema

Datele cantitative din GA4 spun ce se întâmplă (bounce rate ridicat, CR scăzut) dar nu de ce. Heatmap-urile și session recordings oferă date calitative care explică comportamentul utilizatorilor și identifică friction points invizibile în date numerice.

Situații acoperite:
- Landing page cu CR sub benchmark fără cauze clare din GA4
- Pagina de produs cu add-to-cart rate scăzut
- Checkout cu drop-off ridicat la un pas specific
- Audit UX complet al unui site

---

## 2. Procedura

### Pas 1: Instalează tool-ul de heatmap
Alege și instalează tool gratuit sau plătit:

**Microsoft Clarity (gratuit, fără limite de sesiuni)**:
- clarity.microsoft.com → Create project → copiază script
- Adaugă în `<head>` sau via GTM (Custom HTML tag)
- Nu necesită configurare adițională

**Hotjar (gratuit până la 35 sesiuni/zi)**:
- hotjar.com → adaugă site → instalează snippet
- Configurează heatmaps și recordings separat

Verifică că tracking-ul este activ (sesiuni apar în dashboard în primele minute).

### Pas 2: Configurează tracking pe paginile cheie
Selectează paginile cu prioritate înaltă:
- Landing pages principale (cele cu cel mai mult trafic plătit)
- Pagina de produs (dacă e-commerce)
- Pagina de checkout sau formular
- Homepage

Activează heatmaps și recordings specific pentru aceste pagini. Exclude paginile admin sau interne.

### Pas 3: Colectează minimum 500 sesiuni
Parametrul minim pentru date statistice semnificative:
- 500 sesiuni per pagină pentru heatmap click
- 500 sesiuni pentru scroll heatmap
- 50-100 recordings vizionate manual

Nu analiza date sub 500 sesiuni — pattern-urile vor fi înșelătoare. Dacă traficul este mic, extinde perioada de colectare (2-4 săptămâni).

### Pas 4: Analizează click heatmaps
Deschide click heatmap pentru fiecare pagină prioritară:
- **Ce apasă lumea?** — identifică elementele cu cele mai multe click-uri
- **Apasă pe elemente non-clickable?** — indică confuzie UX sau expectație nesatisfăcută
- **Ignoră CTA-ul principal?** — indică placement greșit sau vizibilitate slabă
- **Click-uri "furioase" (rage clicks)?** — elemente care par clickable dar nu sunt

Notează fiecare finding cu screenshot.

### Pas 5: Analizează scroll heatmaps
Deschide scroll heatmap pentru aceleași pagini:
- **Unde se opresc din scrollat?** — conținut important sub această linie e invizibil pentru majoritate
- **Câți ajung la CTA-ul de jos?** — dacă sub 30%, mută CTA mai sus
- **Există conținut vital la care ajunge sub 50% din vizitatori?** — restructurează pagina

Regula generală: orice element important trebuie văzut de minim 60% din vizitatori.

### Pas 6: Vizionează session recordings (3-5 per pagină)
Alege recordings de la utilizatori care au bounce-uit fără conversie:
- Observă **unde ezită** (mouse hover lung fără click)
- Observă **unde se întorc** în pagină (posibil confuzie)
- Observă **dacă completează formularul corect** sau abandonează
- Notează orice comportament neașteptat sau frustrant

Microsoft Clarity marchează automat sesiunile cu rage clicks și quick backs.

### Pas 7: Identifică problemele UX
Consolidează toate finding-urile într-o listă de UX issues:
- Problemă observată → impactul estimat → urgența
- Exemplu: "CTA-ul 'Cumpără acum' nu este vizibil above-fold pe mobile → impact HIGH → urgență IMEDIATĂ"

Prioritizează: probleme care afectează direct CR sunt urgente, cele estetice sunt opționale.

### Pas 8: Formulează ipoteze de îmbunătățire
Pentru fiecare UX issue identificat, scrie ipoteză structurată:
`Dacă [modificare specifică], atunci [metric] va crește cu [X%], deoarece [evidența din heatmap/recording].`

Transmite ipotezele echipei CRO sau procedura M-066 (Website CRO) pentru A/B testing.

---

## 3. Cortex Logging

```json
{
  "text": "Heatmap Analysis executat: Clarity/Hotjar instalat, pagini cheie configurate, 500+ sesiuni colectate, click heatmaps analizate, scroll heatmaps verificate, 3-5 recordings vizionate per pagină, UX issues identificate, ipoteze de optimizare formulate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-086",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["heatmap", "session-recording", "cro", "ux", "clarity", "hotjar", "optimizare"],
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
- Training Mode → curriculum analytics-tracking
- `procedure-health.json` → adaugă entry la fiecare execuție

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-086 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Website Heatmap Analysis for Conversion Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Microsoft Clarity (gratuit) | Heatmap + session recording fără limite |
| Hotjar | Heatmap + recording + polls |
| Google Tag Manager | Instalare snippet via tag |
| M-066 Website CRO | Procedura upstream pentru A/B testing ipoteze |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Sesiuni colectate per pagină | Minimum 500 |
| Recordings vizionate | Minimum 3-5 per pagină |
| UX issues documentate | Minimum 3 |
| Ipoteze formulate | 1 per UX issue major |
