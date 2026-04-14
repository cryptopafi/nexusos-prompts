---
type: procedure
created: 2026-03-22
status: active
slug: m-095-multi-platform-social-strategy
tags: [procedure, nexus]
---

# Multi-Platform Social Media Strategy Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definește cadrul strategic pentru gestionarea coerentă a prezenței pe multiple platforme sociale cu o identitate de brand unitară și mesaj adaptat per canal.

---

## 1. Problema

Prezența pe multiple platforme fără un framework unificator duce la mesaje inconsistente, efort duplicat și burnout creativ. Fiecare platformă are cultura, algoritmul și publicul său distinct, iar copierea identică de conținut reduce eficiența și poate dăuna algoritmilor. Un framework multi-platform bine definit maximizează reach-ul fără a multiplica efortul.

Situații acoperite:
- Brand prezent pe 4+ platforme fără strategie coerentă și mesaj unificat
- Conținut identic postat pe toate platformele cu rezultate slabe
- Lipsă de prioritizare a platformelor în funcție de ROI și audiență

---

## 2. Procedura

### Pas 1: Audit platforme existente și selecție platforme prioritare
Listează toate platformele actuale și evaluează fiecare pe: rata de engagement, calitatea audienzei (demografic + intenție), timp investit vs. rezultate, potențial de creștere. Clasifică platformele în: Tier 1 (platforme principale, 60% din efort), Tier 2 (platforme secundare, 30% din efort), Tier 3 (prezență minimală, 10%). Maximum 2-3 platforme Tier 1 pentru a evita fragmentarea resurselor.

### Pas 2: Definire identitate de brand cross-platform
Creează un Brand Bible minimal cu: paleta de culori (hex codes), fonturi principale (1-2), tonul vocii (3-5 adjective: ex: "direct, educativ, accesibil"), template de bio adaptat per platformă (același mesaj, lungimi diferite: 160 char Instagram, 300 char LinkedIn, 160 char Twitter). Salvează toate asset-urile în folder partajat (Drive/Dropbox) accesibil echipei sau ție în orice moment.

### Pas 3: Definire strategie de conținut per platformă
Fiecare platformă Tier 1 primește o strategie specifică: Instagram — vizual + Reels + Stories (edu + lifestyle), LinkedIn — thought leadership + cazuri de studiu + postări text lungi, YouTube — video long-form tutorial + SEO, TikTok — trending + entertainent + edutainment scurt, Pinterest — SEO vizual + evergreen. Frecvența: Tier 1 zilnic sau 5x/săpt, Tier 2 de 2-3x/săpt, Tier 3 1x/săpt.

### Pas 4: Creare hub de conținut și model hub-and-spoke
Desemnează un canal principal (hub) unde publici conținut long-form: blog, YouTube sau podcast. Toate platformele sociale (spokes) distribuie sau adaptează conținut din hub. Exemplu: articol blog 2.000 cuvinte → LinkedIn post (rezumat + perspectivă personală) → Twitter thread (10 puncte cheie) → Instagram carousel (5 slide-uri) → Pinterest infografic. Un singur efort de creație → 5 piese de conținut.

### Pas 5: Setare cross-posting inteligent (nu identic)
Folosește un tool de scheduling (Buffer, Publer, Hootsuite) care permite editarea conținutului per platformă din același draft. Regulă de adaptare: păstrează ideea centrală identică, schimbă formatul, lungimea și tonul. LinkedIn → mai lung, profesional, date; Instagram → mai scurt, vizual, emoji; Twitter/X → esențializat, conversațional; Facebook → middle ground, mai narativ.

### Pas 6: Monitorizare și raportare centralizată
Setează un dashboard simplu (Google Data Studio / Notion) cu metricile cheie per platformă: reach/impresii, engagement rate, urmăritori (net growth), clicks spre site. Revizuiește săptămânal un singur indicator per platformă (nu toate deodată) și lunar imaginea de ansamblu. Compară performanța lunii curente vs. luna anterioară și vs. aceeași lună a anului trecut.

### Pas 7: Testare și ajustare trimestrială a mixului de platforme
La fiecare trimestru: re-evaluează Tier-urile platformelor bazat pe date reale. Dacă o platformă Tier 2 depășește performanța unui Tier 1 timp de 2 trimestre consecutive, promovează-o. Dacă o platformă Tier 1 nu îndeplinește targeturile 2 trimestre consecutive, retrogradează-o sau abandonează prezența. Fii dispus să renunți la platforme care nu performează — focusul bate prezența.

---

## 3. Cortex Logging

```json
{
  "text": "Multi-Platform Social Media Strategy Framework executat: audit platforme, Brand Bible creat, strategie hub-and-spoke implementată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-095",
    "domain": "social-media-marketing",
    "rule_id": "META-H-002",
    "tags": ["multi-platform", "social-strategy", "brand-consistency", "hub-and-spoke", "content-framework"],
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
- Conținut identic postat fără adaptare pe toate platformele → violation
- Mai mult de 3 platforme Tier 1 alocate simultan → violation
- Lipsă Brand Bible sau identitate vizuală consistentă → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum social-media-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Platformele clasificate în Tier 1/2/3 cu criterii clare?
- [ ] Hub de conținut principal desemnat și model hub-and-spoke definit?

**VK-uri obligatorii:**
1. `✅ [PROC] M-095 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Multi-Platform Social Media Strategy Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Buffer / Hootsuite / Publer | Scheduling și adaptare conținut per platformă |
| Google Data Studio / Looker Studio | Dashboard centralizat metrici |
| Canva Brand Kit | Consistență vizuală cross-platform |
| Notion / Google Sheets | Brand Bible + calendar editorial |
| Google Analytics | Trafic inbound per sursă socială |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Engagement rate cross-platform mediu | > 3% |
| Net follower growth lunar per platformă Tier 1 | > 5% |
| Trafic organic din social spre site | Creștere lunară > 10% |
