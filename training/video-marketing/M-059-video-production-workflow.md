---
type: procedure
created: 2026-03-22
status: active
slug: m-059-video-production-workflow
tags: [procedure, nexus]
---

# Video Production Workflow — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Workflow standardizat de producție video de la concept la export final — pre-producție, filmare, post-producție și quality check — pentru consistență și eficiență de producție.

---

## 1. Problema

Fără un workflow standardizat de producție video, fiecare videoclip devine un proiect de la zero cu timpi imprevizibili, calitate inconsistentă și reeditare frecventă. Echipele de producție fără proces clar pierd timp în coordonare și livrează output de calitate variabilă care afectează credibilitatea canalului.

Situații acoperite:
- Producerea unui video YouTube educational sau de marketing
- Producerea de video-uri scurte pentru Reels, TikTok sau YouTube Shorts
- Workflow pentru echipe de 1-3 persoane cu resurse limitate de producție

---

## 2. Procedura

### Pas 1: Concept și aprobarea brief-ului de producție
Documentează brief-ul de producție: titlu video, keyword target, audiența, obiectivul (educație, entertainment, conversie), format (tutorial, talking head, screencast, vlog), durata estimată, și CTA final. Obține aprobare stakeholder pentru brief înainte de alocare resurse de producție. Brief-ul aprobat previne refilmarea din cauza dezalinierii de obiective.

### Pas 2: Scriptul sau outline structurat
Scrie scriptul complet sau un outline detaliat cu: hook (primele 30 secunde), corpul conținutului cu secțiuni clare, tranziții, și outro cu CTA. Primele 30 de secunde sunt critice — hook-ul determină dacă spectatorul rămâne sau pleacă. Pentru talking head, un script verbatim sau prompter; pentru tutorial, un outline cu puncte cheie și demonstrații. Scriptul aprobat = garanția că filmarea merge smooth.

### Pas 3: Pre-producție — setup filmare și pregătire
Pregătește toate elementele înainte de filmare: locație (fundal curat sau branding consistent), iluminare (ring light sau natural light din față), audio (microfon extern preferat față de microfon camera), și echipament verificat (baterie, spațiu pe card). Dacă este screencast, pregătește toate tab-urile, fișierele și demo-urile deschise. 30 minute de setup previn 2 ore de refacere.

### Pas 4: Filmare cu protocol de calitate
Filmează cu minimum 2-3 take-uri pe secțiunile cheie pentru opțiuni de montaj. Aplică regula de filmare: start 3 secunde înainte de vorbire, pauze clare între secțiuni pentru cut points ușoare, nu tăia în mijlocul frazei. Verifică audio pe parcurs (monitorizare cu căști). Pentru tutorial/screencast: asigură-te că acțiunile de pe ecran sunt vizibile și se mișcă lent suficient. Exportă footage brut în folder organizat cu naming convention.

### Pas 5: Post-producție — editare și montaj
Editează video-ul în software dedicat (DaVinci Resolve, Adobe Premiere, CapCut Pro): (1) taie greșelile și pauzele lungi, (2) adaugă intro de brand (5-10 sec), (3) inserează B-roll sau screen recordings unde este relevant, (4) adaugă text overlays, titluri și lower thirds conform brand guidelines, (5) muzică de fundal la volum potrivit (-20 dB față de voce), (6) outro cu end screen (20 sec) și card-uri pentru CTA. Respectă consistența cu video-urile anterioare ale canalului.

### Pas 6: Color grading, audio mastering și export
Aplică color grade de bază pentru consistență vizuală (LUT de brand sau preset consistent). Masterizează audio: normalizare la -14 LUFS pentru YouTube, fără peaking peste -1 dB, și fără zgomot de fundal vizibil. Exportă în setările optime YouTube: H.264 sau H.265, 1080p minimum (4K dacă e posibil), bitrate 15-20 Mbps pentru 1080p, audio AAC 320kbps. Verifică exportul final pe ecran mobil și desktop înainte de upload.

### Pas 7: Quality Check final înainte de upload
Rulează checklist QC înainte de upload: (1) video se redă corect de la început la final fără glitch, (2) audio clar fără distorsiuni, (3) intro și outro prezente, (4) toate titlurile text sunt corecte (fără typo-uri), (5) CTA-ul este clar și functional, (6) durata finală este în parametrii planificați, (7) fișierul exportat are dimensiunile și calitatea corecte. Abia după QC complet se uploadează pe YouTube și se aplică M-058.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-059 executată: Video production workflow complet — brief, script, filmare, editare, export și QC finalizate pentru upload.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-059",
    "domain": "video-marketing",
    "rule_id": "META-H-002",
    "tags": ["video-production", "filming", "editing", "post-production", "workflow"],
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
- Video publicat fără thumbnail optimizat → violation
- Video uploadat fără QC checklist complet → violation
- Audio sub standard acceptabil publicat pe canal (-14 LUFS, fără distorsiuni) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum video-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] QC checklist complet înainte de upload?
- [ ] Export la setările corecte YouTube (1080p+, H.264/H.265)?

**VK-uri obligatorii:**
1. `✅ [PROC] M-059 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Video Production Workflow" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| DaVinci Resolve / Adobe Premiere / CapCut Pro | Software editare video |
| Audacity / Adobe Audition | Audio mastering și cleanup |
| Brand Identity Design (M-055) | Templates intro/outro și overlay-uri |
| Sales Video Script (M-060) | Scripting pentru video-uri de vânzare |
| YouTube SEO (M-058) | Aplicat după finalizarea producției |
| AI Avatar Tools (M-063) | Alternativă de producție cu AI |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Timp producție video standard (5-10 min) | < 6 ore end-to-end |
| QC pass rate | 100% (zero re-upload din erori tehnice) |
| Audio quality standard | -14 LUFS, fără distorsiuni |
| Average View Duration post-publicare | > 40% |
