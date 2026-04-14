---
type: procedure
created: 2026-03-22
status: active
slug: m-073-chatgpt-prompt-engineering-marketing
tags: [procedure, nexus]
---

# ChatGPT Prompt Engineering for Marketing Content — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea de prompt-uri ChatGPT eficiente pentru generarea de conținut de marketing de calitate, conform brand voice-ului și obiectivelor de conversie.

---

## 1. Problema

Marketerii care folosesc ChatGPT fără o metodologie de prompt engineering obțin conținut generic, nealiniat brandului și cu conversion rate slab. Această procedură standardizează procesul de la obiectiv la output final, asigurând conținut relevant, ton corect și conformitate.

Situații acoperite:
- Generarea de blog posts, articole sau conținut long-form
- Creare de email marketing (subject lines, body, CTA)
- Ad copy pentru Google Ads, Meta, LinkedIn
- Conținut pentru social media (captions, threads, posts)
- Landing page copy și headlines

---

## 2. Procedura

### Pas 1: Define — Definește obiectivul de conținut
Stabilește clar: ce etapă de funnel vizezi (Awareness / Consideration / Conversion), ce acțiune dorești de la cititor, care este metrica de succes (CTR, conversii, engagement). Nu scrie prompt-ul înainte de a avea răspuns clar la aceste 3 întrebări.

### Pas 2: Select — Alege tipul de conținut și formatul
Specifică exact: blog post (lungime + nr. secțiuni), email (subject + body + CTA), ad copy (platformă + limite de caractere), social media (platformă + ton + hashtag strategy). Fiecare format are cerințe tehnice diferite care trebuie incluse în prompt.

### Pas 3: Craft — Construiește prompt-ul cu structura role-based
Folosește structura: **[ROLĂ]** „Ești un copywriter expert în [industrie] care scrie pentru [audiență]." + **[CONTEXT]** produsul/serviciul + USP-ul principal + **[SARCINA]** ce trebuie generat + **[FORMAT]** structura exactă + **[TON]** 3 adjective de ton + **[CONSTRANGERI]** ce să evite (jargon, tone of voice greșit, claim-uri false).

### Pas 4: Specify Format — Adaugă cerințele de format explicit
Include în prompt: lungimea dorită (cuvinte sau paragrafe), structura de headings dacă e cazul, numărul de variante solicitate (A/B testing), formatul output-ului (markdown / plain text / HTML). ChatGPT respectă specificațiile de format când sunt explicite.

### Pas 5: Generate and Iterate — Generează și iterează
Rulează prompt-ul, evaluează output-ul față de obiectivul din Pasul 1. Dacă nu e satisfăcător, iterează: (a) adaugă exemple de conținut de referință în prompt, (b) ajustează tonul cu descrieri mai specifice, (c) reduce sau mărește constrangerile, (d) folosește „Rescrie varianta X cu [schimbare specifică]."

### Pas 6: Brand Voice — Aplică ghidul de brand voice
Verifică output-ul față de brand guidelines: tonul este consistent cu comunicarea existentă? Terminologia specifică brandului este folosită corect? Există claim-uri care necesită verificare de conformitate? Editează manual unde AI-ul deviază de la vocea brandului.

### Pas 7: Quality Check — Verifică conformitatea și acuratețea
Check final: toate faptele/cifrele menționate sunt verificate? Nu există claims false sau exagerate? Conținutul este conform reglementărilor sectoriale (GDPR pentru email, Ad Standards pentru publicitate)? CTA-ul este clar și funcțional?

---

## 3. Cortex Logging

```json
{
  "text": "Prompt Engineering Marketing executat pentru [tip conținut]. Obiectiv: [awareness/consideration/conversion]. Format: [blog/email/ad/social]. Iterații necesare: [X]. Output aprobat: [da/nu].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-073",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["prompt-engineering", "chatgpt", "marketing-content", "copywriting", "brand-voice"],
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
La fiecare creare de conținut de marketing cu asistență AI

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Conținut publicat fără Brand Voice check (Pasul 6) → violation
- Quality Check (Pasul 7) sărit pentru conținut publicabil → violation critică
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-marketing

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-073 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "ChatGPT Prompt Engineering for Marketing Content" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT (GPT-4o sau echivalent) | Generare conținut |
| Brand Voice Guidelines | Referință Pasul 6 |
| Compliance checklist sectorial | Referință Pasul 7 |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Conținut aprobat fără re-lucru major | ≥ 70% din generări |
| Iterații medii per output | ≤ 3 |
