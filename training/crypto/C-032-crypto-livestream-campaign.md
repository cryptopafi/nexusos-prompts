---
type: procedure
created: 2026-03-22
status: active
slug: c-032-crypto-livestream-campaign
tags: [procedure, nexus]
---

# Run a Crypto Livestream Campaign — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Planificarea și execuția unui livestream crypto pentru promovarea unui token sau proiect, de la setup tehnic până la follow-up post-eveniment.

---

## 1. Problema

Livestream-urile sunt unul dintre cele mai eficiente formate de engagement în crypto — permit interacțiune directă, demonstrații live și construirea încrederii cu potențialii investitori. Fără o planificare adecvată, livestream-urile eșuează din cauza problemelor tehnice, lipsei de audiență sau conținutului slab.

> **Disclaimer**: Această procedură are scop exclusiv educativ. Nu constituie sfat de investiții. Tokenii crypto sunt active speculative cu risc ridicat.

Situații acoperite:
- AMA (Ask Me Anything) cu echipa fondatoare pentru comunitatea tokenului
- Livestream de lansare a tokenului cu demonstrație live de cumpărare
- Colaborare cu un influencer crypto pentru co-hosted stream

---

## 2. Procedura

### Pas 1: Definește obiectivul și formatul livestream-ului
Stabilește: scopul principal (lansare, AMA, tutorial, collab), platforma (YouTube Live, Twitter/X Spaces, Twitch, Telegram Live), durata estimată (60-90 minute optim), și guest-urile invitate. Documentează agenda și flow-ul evenimentului.

### Pas 2: Promovează evenimentul cu minim 48h înainte
Creează announcement post cu: data, ora (în UTC + timezone local), link de accesare, agenda, și CTA (set reminder). Distribuie pe toate canalele comunității. Creează un event pe Twitter/X. Trimite reminder cu 1h înainte de start.

### Pas 3: Pregătește setup-ul tehnic
Verifică: conexiune internet stabilă (recomandă ethernet, nu WiFi), OBS sau software de streaming configurat, microfon de calitate, backup plan (mobile hotspot dacă cade internetul). Testează streaming-ul cu 30 minute înainte cu un test privat.

### Pas 4: Pregătește agenda și materialele vizuale
Creează slide-deck sau screen-share pregătit: intro proiect, tokenomics vizual, roadmap, demonstrație live (cum cumperi tokenul), Q&A. Pregătește răspunsuri la întrebările frecvente anticipate. Asigură-te că ai adresa contractului și link-ul pool-ului la îndemână.

### Pas 5: Execută livestream-ul conform agendei
Începe cu 5 minute înainte de ora anunțată pentru a acumula audiență. Urmează agenda dar rămâi flexibil pentru întrebări relevante. Menționează explicit că nu oferi sfaturi de investiții. Pin-uiește adresa contractului verificat în chat. Interacționează activ cu chat-ul.

### Pas 6: Exportă și redistribuie înregistrarea
Imediat după stream, publică înregistrarea pe YouTube (dacă nu a fost acolo). Creează un clip highlight (3-5 minute) cu momentele cheie. Distribuie în comunitate cu un rezumat text al punctelor principale. Răspunde la comentariile din primele 24h.

### Pas 7: Compilează metrics și raportul post-stream
Documentează: peak viewers, total unique viewers, durata medie vizionare, număr întrebări/comments, creștere comunitate post-stream (followeri noi, membri Discord/Telegram). Evaluează ce a funcționat și ce de îmbunătățit. Salvează în Cortex.

---

## 3. Cortex Logging

```json
{
  "text": "Livestream crypto executat. Setup tehnic verificat, agenda respectată, înregistrare publicată, metrics documentate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-032",
    "domain": "crypto",
    "rule_id": "META-H-002",
    "tags": ["crypto", "livestream", "community", "ama", "token-launch", "marketing"],
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
- Livestream fără test tehnic pre-eveniment → violation
- Promovare eveniment cu sub 24h înainte → violation (audiență insuficientă)
- Nicio înregistrare publicată post-stream → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum crypto

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Test tehnic executat înainte de stream?
- [ ] Înregistrare publicată și distribuită?

**VK-uri obligatorii:**
1. `✅ [PROC] C-032 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Run a Crypto Livestream Campaign" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| OBS Studio | Software streaming |
| YouTube Live / Twitter Spaces | Platformă livestream |
| Canva / Google Slides | Slide-deck vizual |
| Discord / Telegram | Distribuție anunț și comunitate |
| StreamYard | Alternativă browser-based pentru co-hosts |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Peak viewers | Documentat |
| Promovare pre-eveniment | Minim 48h înainte |
| Durata stream | 60-90 minute |
| Înregistrare publicată | < 2h post-stream |
| Creștere comunitate post-stream | Documentată și comparată cu baseline |
