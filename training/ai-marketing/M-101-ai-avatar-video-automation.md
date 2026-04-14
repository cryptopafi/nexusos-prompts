---
type: procedure
created: 2026-03-22
status: active
slug: m-101-ai-avatar-video-automation
tags: [procedure, nexus]
---

# AI Avatar and Video Content Automation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Producerea de conținut video cu avatar AI la scară, de la selectarea platformei și crearea avatarului până la distribuție și optimizare bazată pe engagement.

---

## 1. Problema

Producția de conținut video tradițională este costisitoare și lentă (echipament, filmare, editare). Platformele de AI avatar (HeyGen, Synthesia, D-ID) permit crearea de video-uri profesionale în ore, dar fără un workflow structurat produc conținut inconsistent sau de calitate slabă. Această procedură standardizează producția de video AI pentru scalabilitate.

Situații acoperite:
- Video-uri explicative pentru produse sau servicii
- Conținut pentru social media (LinkedIn, Instagram, TikTok, YouTube)
- Training videos sau onboarding pentru clienți
- Ad video-uri și testimoniale sintetice
- Newslettere video sau update-uri de produs

---

## 2. Procedura

### Pas 1: Platform — Selectează platforma AI avatar potrivită
Evaluează platformele disponibile pe criterii: (a) **HeyGen** — cel mai realist, limbaje multiple, ideal pentru business/LinkedIn; (b) **Synthesia** — cel mai simplu UI, potrivit pentru training videos; (c) **D-ID** — ideal pentru personalizare cu poze proprii, mai accesibil ca preț. Selectează pe baza: buget, calitate necesară, limbă, frecvența de producție. Documentează alegerea și limitele platformei.

### Pas 2: Avatar — Creează sau selectează avatarul
Opțiunea A (avatar template): alege un avatar prestabilit al platformei — ideal pentru conținut generic sau la volum mare. Opțiunea B (avatar custom): uploadează 2-3 minute de video propriu în condiții bune de lumină și audio — pentru contenut branded. Verifică: expresivitatea avatarului, naturalețea mișcărilor, calitatea lip-sync. Testează cu un clip de 30 secunde înainte de producție.

### Pas 3: Script — Scrie script-ul cu asistență AI
Folosind ChatGPT/Claude, generează script-ul conform structurii: Hook (primele 3 secunde — ce câștigă spectatorul), Problema (empatizează cu pain point-ul), Soluția (prezintă produsul/serviciul/informația), Dovadă (beneficiu concret sau social proof), CTA (acțiune clară și specifică). Adaptează lungimea la platforma de distribuție: TikTok/Reels 30-60s, LinkedIn/YouTube 1-3 minute. Citește script-ul cu voce tare pentru a verifica naturalețea înainte de generare.

### Pas 4: Generate — Generează video-ul cu text-to-speech
Uploadează script-ul în platformă. Configurează: vocea (selectează sau customizează), ritmul de vorbire (ușor mai lent decât natural = mai clar), accentul/limba. Generează preview și verifică lip-sync pe primele 30 secunde. Aprobă sau ajustează înainte de renderul final. Timpul de render: 5-20 minute în funcție de lungime și platformă.

### Pas 5: Edit — Adaugă captions, B-roll și branding
Post-producție în CapCut, DaVinci Resolve sau editor nativ al platformei: (a) adaugă captions automate (85% din vizionări sunt fără sunet), (b) inserează B-roll (imagini de produs, grafice, screenshots) pentru a dinamiza, (c) adaugă intro/outro branded cu logo și culori, (d) include lower-thirds (text pe ecran) pentru punctele cheie. Verifică că video-ul are aspect ratio corect pentru fiecare platformă țintă.

### Pas 6: Distribute — Distribuie pe platformele țintă
Postează cu metadata optimizate: titlu cu keyword principal (YouTube/LinkedIn), descriere cu primele 2 rânduri de hook (TikTok/Instagram), hashtag-uri relevante (5-10 pentru Instagram/TikTok, 3-5 pentru LinkedIn), thumbnail custom pentru YouTube. Programează postarea la orele de peak engagement per platformă.

### Pas 7: Optimize — Monitorizează și optimizează pe baza datelor
După 48-72 ore, analizează: rata de vizionare completă (target >40%), engagement rate (likes/comments/shares), CTR pe CTA. Dacă rata de vizionare completă este sub 30%, identifică punctul de drop-off și optimizează hook-ul sau lungimea. Folosește datele pentru a informa script-ul video-ului următor.

---

## 3. Cortex Logging

```json
{
  "text": "AI Avatar Video produs pentru [topic/produs]. Platformă: [HeyGen/Synthesia/D-ID]. Durată: [Xs]. Distribuție: [platforme]. Watch rate: [%]. CTA clicks: [X].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-101",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["ai-avatar", "video-automation", "heygen", "synthesia", "content-production", "social-media"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H + Training Mode execution

### WHEN
La producerea oricărui video cu AI avatar pentru distribuție publică

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Video distribuit fără captions (Pasul 5) → violation
- Script generat fără structura Hook-Problemă-Soluție-Dovadă-CTA → violation
- Optimizare (Pasul 7) omisă după 72h → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-marketing

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-101 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Avatar and Video Content Automation" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| HeyGen / Synthesia / D-ID | Generare video avatar |
| ChatGPT / Claude | Generare script |
| CapCut / DaVinci Resolve | Post-producție și captions |
| Platforme social media | Distribuție |
| Analytics (native per platformă) | Monitorizare Pasul 7 |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Watch rate completă | ≥ 40% |
| Video-uri cu captions | 100% |
