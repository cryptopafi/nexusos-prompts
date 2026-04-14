---
type: procedure
created: 2026-03-22
status: active
slug: m-052-content-repurposing-system
tags: [procedure, nexus]
---

# Content Repurposing System — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Sistem pentru transformarea unui piece de conținut existent în multiple formate distribuite pe canale diferite, maximizând ROI-ul de producție.

---

## 1. Problema

Echipele de marketing produc conținut nou constant, ignorând potențialul conținutului existent de a fi redistribuit în formate adaptate pentru canale diferite. Fiecare articol, video sau podcast poate genera 5-10 piese derivate de conținut, multiplicând reach-ul fără cost de producție proporțional.

Situații acoperite:
- Repurposing unui articol de blog în conținut social media
- Transformarea unui webinar sau video în articol, podcast și infografice
- Actualizarea și redistribuirea conținutului evergreen existent

---

## 2. Procedura

### Pas 1: Auditul și selecția conținutului sursă
Identifică conținutul cu cel mai bun potențial de repurposing: articole evergreen cu trafic organic bun, webinare sau prezentări comprehensive, studii de caz sau date propriii. Verifică că conținutul sursă este actualizat și acurat înainte de repurposing. Evită repurposing-ul conținutului time-sensitive sau cu date expirate.

### Pas 2: Mapping formate și canale țintă
Pentru fiecare piesă sursă, creează un map al formatelor derivate posibile: articol lung → thread Twitter/X, carousel LinkedIn, newsletter snippet, infografic, clip video scurt, episod podcast, prezentare SlideShare. Selectează 3-5 formate bazate pe canalele active și audiența disponibilă. Documentează mapping-ul într-un tabel cu canal, format și audiență țintă.

### Pas 3: Adaptarea mesajului pentru fiecare canal
Nu copia-paste conținut între canale. Adaptează tonul, lungimea și formatul specific platformei: LinkedIn preferă insights profesionale și date; Instagram/TikTok preferă hooks vizuale și conținut scurt; Twitter/X preferă opinii concise și listicle; email preferă conținut personal și exclusiv. Păstrează mesajul central consistent, dar adaptează prezentarea.

### Pas 4: Producția formatelor derivate
Execută producția fiecărui format derivat conform standardelor canalului: aspect ratio corect pentru imagini video, lungime adecvată pentru text, elemente vizuale de brand consistente. Utilizează templates predefinite pentru eficiență. Prioritizează formatele high-impact (video scurt, carousel) față de cele low-impact (text simplu).

### Pas 5: Optimizarea pentru fiecare platformă
Aplică optimizări specifice per canal: hashtag-uri relevante pentru Instagram/LinkedIn/TikTok, timing optim de postare per platformă, thumbnail optimizat pentru video, caption cu hook în primele 2 linii vizibile. Adaptează CTA-ul pentru comportamentul specific al fiecărei platforme (swipe up, link in bio, comentariu).

### Pas 6: Scheduling și distribuție coordonată
Planifică distribuția în mod coordonat — nu publica toate formatele simultan. Staggerează publicarea pe 2-4 săptămâni pentru a maximiza reach organic total. Folosește un tool de scheduling (Buffer, Hootsuite, Later) pentru automatizarea distribuției. Asigură-te că există link-uri cross-platform acolo unde este relevant (ex: Instagram story → articol complet).

### Pas 7: Tracking și analiza performanței per format
Monitorizează performanța fiecărui format derivat cu metrici specifice platformei: reach, engagement rate, click-through, shares. Compară performanța formatelor pentru a identifica care funcționează cel mai bine pentru audiența ta. Documentează learnings pentru optimizarea viitoarelor sesiuni de repurposing.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-052 executată: Sistem repurposing conținut aplicat — piesă sursă transformată în formate multiple distribuite pe canale targetate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-052",
    "domain": "content-creation",
    "rule_id": "META-H-002",
    "tags": ["content-repurposing", "multi-channel", "distribution", "content-strategy", "ROI"],
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
- Conținut copiat verbatim între canale fără adaptare → violation
- Formate distribuite fără tracking setup → violation
- Repurposing de conținut outdated sau cu date expirate → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-creation

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Minimum 3 formate derivate produse per piesă sursă?
- [ ] Tracking configurat pentru toate formatele distribuite?

**VK-uri obligatorii:**
1. `✅ [PROC] M-052 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Content Repurposing System" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Buffer / Hootsuite / Later | Scheduling și distribuție automatizată |
| Canva / Adobe Express | Producție formate vizuale |
| Descript / CapCut | Editare video și audio clipuri |
| Google Analytics / Platform Analytics | Tracking performanță per format |
| Content Writing System (M-051) | Sursă primară de conținut pentru repurposing |
| One-to-Many Framework (M-083) | Extensie avansată a acestui sistem |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Formate derivate per piesă sursă | Minimum 3 |
| Reach total vs. conținut original | +300% |
| Engagement rate per format | Benchmark specific platformă |
| Cost per reach (repurposed vs. nou) | -60% față de producție nouă |
