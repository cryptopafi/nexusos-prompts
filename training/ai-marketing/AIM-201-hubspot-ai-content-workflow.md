---
type: procedure
created: 2026-03-22
status: active
slug: aim-201-hubspot-ai-content-workflow
tags: [procedure, nexus]
---

# HubSpot AI Content Creation Workflow — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-20
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Implementarea unui workflow end-to-end de creare conținut marketing folosind AI (HubSpot AI tools, ChatGPT, Claude), de la ideație la publicare, aliniat cu metodologia inbound și buyer's journey.
**Sursă**: HubSpot Academy — AI for Marketers course + best practices 2026

---

## 1. Problema

Marketerii adoptă AI tools fără un workflow structurat, rezultând conținut inconsistent, off-brand, și fără aliniere la etapele buyer's journey. Conținutul generat de AI fără proces produce: volume mari de conținut mediocru, lipsă de personalizare per buyer persona, risipă de timp pe editare retroactivă, și risc de publicare de conținut inexact sau non-compliant.

Situații acoperite:
- Generare blog posts aliniate buyer's journey (Awareness / Consideration / Decision)
- Creare email sequences personalizate cu AI
- Producție social media content at scale (LinkedIn, Instagram, X)
- Landing page copy optimizat pentru conversie
- Content repurposing: un asset → multiple formate
- AI-assisted A/B testing copy variants

---

## 2. Procedura

### Pas 1: Audit Content Gaps — Identifică golurile de conținut
Mapează conținutul existent pe buyer's journey (Awareness → Consideration → Decision) și pe fiecare buyer persona. Identifică unde lipsește conținut: ce întrebări au buyerii la care nu ai răspuns? Folosește HubSpot Content Strategy tool sau SEMrush Topic Research pentru gap analysis. Documentează minimum 10 topic gaps cu prioritate HIGH/MEDIUM/LOW.

### Pas 2: Define Brief — Creează content brief structurat per asset
Pentru fiecare piesă de conținut, completează un brief standard: (a) Buyer Persona target, (b) Journey Stage, (c) Search intent (informational/navigational/transactional), (d) Primary keyword + 3-5 secondary keywords, (e) Competing content URLs (top 3 SERP), (f) Desired CTA și next-step în funnel, (g) Word count target, (h) Tone of voice (3 adjective).

### Pas 3: Prompt Engineering — Construiește prompt-ul AI stratificat
Folosește framework-ul RCSTFC: **Role** (expert content marketer în [industrie]), **Context** (buyer persona X, în etapa Y, cu pain point Z), **Sarcină** (scrie [tip conținut] de [lungime]), **Ton** (3 adjective de brand voice), **Format** (heading structure, bullet points, paragraph style), **Constrângeri** (evită jargon tehnic / nu folosi superlative neargumentate / include date verificabile). Generează minim 2 variante.

### Pas 4: AI Generate + Human Layer — Generează cu AI, editează uman
Rulează prompt-ul în tool-ul ales (HubSpot AI Content Writer, ChatGPT-4o, Claude). Aplică imediat „human layer": (a) verifică acuratețea faptelor și cifrelor, (b) adaugă experiențe reale și exemple specifice brandului, (c) înlocuiește formulări generice cu detalii concrete, (d) asigură-te că vocea e distinctivă, nu generic-AI. Regula 80/20: AI generează 80% structură, umanul adaugă 20% diferențiere.

### Pas 5: SEO Integration — Optimizare pentru search
Integrează keyword-urile din brief-ul din Pasul 2: title tag cu primary keyword, meta description cu CTA, H2/H3 headings cu secondary keywords, internal links către 2-3 piese existente, alt text pentru imagini. Folosește HubSpot SEO Recommendations sau SurferSEO pentru scor de optimizare. Target: scor SEO ≥ 75/100.

### Pas 6: CTA + Conversion Path — Configurează calea de conversie
Fiecare piesă de conținut trebuie să aibă: (a) CTA primar aliniat cu journey stage (Awareness → descarcă ghid, Consideration → request demo, Decision → start trial), (b) smart CTA diferențiat per buyer persona în HubSpot, (c) landing page cu form optimizat (progressive profiling), (d) thank-you page cu next-step. Configurează în HubSpot CMS.

### Pas 7: Repurpose — Multiplică conținutul în formate multiple
Din fiecare asset principal, extrage: (a) 3-5 social media posts (LinkedIn carousel, X thread, Instagram caption), (b) 1 email newsletter section, (c) 1 video script (60-90 secunde), (d) pull quotes pentru graphics. Folosește AI pentru adaptare per platformă dar păstrează mesajul core consistent. Documentează repurposing map.

### Pas 8: Publish + Distribute — Publică și distribuie strategic
Programează în HubSpot: blog publish la ora optimă (verifică analytics), social distribution pe toate canalele, email blast către segmentul relevant. Configurează workflow de nurturing: dacă lead-ul descarcă asset → email follow-up la 3 zile → next content piece. Activează tracking UTM pe toate link-urile.

### Pas 9: Measure + Iterate — Măsoară și iterează
La 7 zile post-publicare, verifică: page views, time on page, bounce rate, CTA click rate, conversion rate. La 30 zile: organic traffic growth, keyword ranking movement, lead generation. Compară performanța AI-generated vs. manual content. Documentează learnings și actualizează prompt templates pentru conținut viitor.

---

## 3. Cortex Logging

```json
{
  "text": "HubSpot AI Content Workflow executat. Topic: [topic]. Persona: [persona]. Stage: [awareness/consideration/decision]. Assets create: [X]. Repurposed în: [Y] formate. SEO Score: [Z]/100. Conversion rate: [W]%.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "AIM-201",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["hubspot", "ai-content", "content-workflow", "inbound-marketing", "buyer-journey", "seo"],
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
La fiecare creare de conținut de marketing cu AI assistance

### HOW (violation detection)
- Conținut publicat fără content brief (Pasul 2) → violation
- Prompt fără RCSTFC framework → violation
- Conținut publicat fără human layer edit (Pasul 4) → violation
- Lipsă CTA sau conversion path (Pasul 6) → violation critică
- Fără repurposing map (Pasul 7) → violation
- Fără measurement la 7 zile (Pasul 9) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- AIM-103 → AI Content Marketing Pipeline
- M-073 → ChatGPT Prompt Engineering for Marketing Content

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Content brief completat cu toate câmpurile (Pasul 2)?
- [ ] Prompt folosește framework RCSTFC (Pasul 3)?
- [ ] Human layer edit aplicat (Pasul 4)?
- [ ] SEO Score ≥ 75/100 (Pasul 5)?
- [ ] CTA + conversion path configurate (Pasul 6)?
- [ ] Repurposing map documentat (Pasul 7)?

**VK-uri obligatorii:**
1. `✅ [PROC] AIM-201 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "HubSpot AI Content Workflow" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |
| Validare structură | AUDIT-PRO |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| HubSpot CMS + Marketing Hub | Publicare, CTA, workflows, analytics |
| AI Tool (ChatGPT / Claude / HubSpot AI) | Generare conținut |
| SEO Tool (HubSpot SEO / SurferSEO) | Optimizare search |
| Brand Voice Guidelines | Referință Pasul 4 |
| Buyer Persona Documents | Referință Pasul 1-2 |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| SEO Score per asset | ≥ 75/100 |
| Content publicat cu CTA funcțional | 100% |
| Repurposing ratio | ≥ 4 formate / asset |
| Time to publish (brief → live) | ≤ 4 ore |
| Organic traffic growth la 30 zile | ≥ 15% vs. baseline |
