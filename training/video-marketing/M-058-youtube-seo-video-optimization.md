---
type: procedure
created: 2026-03-22
status: active
slug: m-058-youtube-seo-video-optimization
tags: [procedure, nexus]
---

# YouTube SEO and Video Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Optimizare completă SEO pentru fiecare video publicat pe YouTube — titlu, descriere, tags, thumbnail și semnale de engagement — pentru a maximiza rankingul în search și recomandări.

---

## 1. Problema

Videoclipurile publicate fără optimizare SEO au visibility redusă în YouTube Search și recomandări, indiferent de calitatea conținutului. YouTube este al doilea motor de căutare din lume, iar algoritmul promovează video-uri cu metadate relevante, thumbnails cu CTR ridicat și semnale puternice de engagement în primele 24-48 ore de la publicare.

Situații acoperite:
- Optimizarea SEO completă a unui video nou înainte de publicare
- Re-optimizarea video-urilor existente cu performanță slabă
- Cercetarea keyword-urilor pentru planificarea conținutului viitor

---

## 2. Procedura

### Pas 1: Keyword research specific YouTube
Utilizează TubeBuddy, VidIQ sau YouTube Suggest pentru identificarea keyword-urilor cu volum de căutare relevant pe YouTube. Caută keyword-uri cu: volum de căutare >1000/lună, competiție medie-redusă, și potrivire cu intenția de vizionare a audienței. Analizează video-urile existente din top 10 rezultate pentru keyword-ul target. Selectează un keyword principal și 3-5 keyword-uri secundare.

### Pas 2: Optimizarea titlului video
Scrie titlul cu keyword-ul principal în primele 60 de caractere (maximum vizibil în search). Titlul trebuie să fie compelling pentru click și clar pentru algoritmul de indexare. Utilizează formule dovedite: "Cum să...", "X Moduri de a...", "[Număr] [Beneficiu]". Titlul video fără keyword principal → violation. Evită click-bait care nu reflectă conținutul — crește bounce rate și dăunează rankingului.

### Pas 3: Scrierea descrierii optimizate
Scrie o descriere de minimum 250 cuvinte cu keyword-ul principal în primele 2-3 propoziții (primele 125 caractere sunt vizibile fără "Show more"). Structura recomandată: (1) primele 2 linii cu hook și keyword, (2) summary al conținutului video cu keyword-uri secundare natural distribuite, (3) timestamp-uri pentru navigare în video lung, (4) CTA și linkuri relevante, (5) link-uri social media și website. Descrierea comprehensivă ajută indexarea și crește discoverability.

### Pas 4: Tags, categoria și setările avansate
Adaugă 10-15 tags relevanți: keyword principal, variante ale keyword-ului, keyword-uri secundare, branded terms, și termeni generali de nișă. Nu adăuga tags irelevante — YouTube penalizează tag spam. Selectează categoria corectă (influențează recomandările). Adaugă Chapter markers (timestamps) în descriere pentru video-uri >5 minute — cresc CTR-ul în search cu chapter preview.

### Pas 5: Crearea thumbnailului optimizat
Designează un thumbnail custom cu: față umană expresivă (crește CTR cu 20-30%), text mare și contrastant (maximum 6 cuvinte vizibile pe mobile), culori de brand consistente și contrast ridicat față de thumbnails competitorilor. Dimensiune: 1280x720px, format JPG/PNG, max 2MB. Testează thumbnail-ul pe fundal gri pentru a simula experiența grid YouTube. Titlul video fără thumbnail optimizat → violation.

### Pas 6: Strategia de engagement în primele 48 ore
Primele 48 ore de la publicare sunt critice pentru algoritmul YouTube. Activează notificarea abonaților existenți. Distribuie video-ul pe toate canalele proprii (email newsletter, social media, comunități). Răspunde la toate comentariile din primele 24 ore — engagement-ul timpuriu semnalează relevanța algoritmului. Adaugă video-ul în playlist-urile relevante existente imediat la publicare.

### Pas 7: Monitorizarea și optimizarea performanței
Monitorizează metrici cheie în YouTube Analytics în primele 7 zile: Click-Through Rate (CTR) din impressions (>4% = bun, <2% = revizuieste thumbnail/titlu), Average View Duration (>40% = bun), și Audience Retention (identifică momentele de drop-off). Re-optimizează titlu sau thumbnail dacă CTR-ul este sub 2% după 500 impressions. Documentează learnings pentru video-urile viitoare.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-058 executată: YouTube SEO și optimizare video completă — keyword research, titlu, descriere, tags, thumbnail și strategie engagement configurate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-058",
    "domain": "video-marketing",
    "rule_id": "META-H-002",
    "tags": ["youtube-seo", "video-optimization", "CTR", "thumbnail", "keyword-research"],
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
- Titlu video fără keyword principal → violation
- Descriere video sub 150 cuvinte sau fără keyword în primele 2 linii → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum video-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] CTR monitorizat la 7 zile post-publicare?
- [ ] Video adăugat în playlist-urile relevante imediat la publicare?

**VK-uri obligatorii:**
1. `✅ [PROC] M-058 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "YouTube SEO and Video Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| TubeBuddy / VidIQ | Keyword research și scoring SEO YouTube |
| YouTube Analytics | Monitorizare CTR, retention, watch time |
| Canva / Photoshop | Design thumbnails optimizate |
| YouTube Studio | Configurare metadata și setări video |
| Channel Setup (M-057) | Context structură canal și playlist-uri |
| Video Production (M-059) | Calitatea video-ului care trebuie optimizat |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| CTR din impressions | > 4% |
| Average View Duration | > 40% din durata totală |
| Watch time per video | Creștere lunară +10% |
| Ranking keyword principal | Top 10 YouTube Search în 30 zile |
