---
type: procedure
created: 2026-03-22
status: active
slug: m-102-ai-marketing-funnel-chatgpt
tags: [procedure, nexus]
---

# AI Marketing Funnel Building with ChatGPT — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea pas cu pas a unui marketing funnel operațional folosind ChatGPT ca motor de generare pentru toate componentele de copy, secvențe și strategie de conținut.

---

## 1. Problema

Construirea unui funnel de marketing complet (landing page, email sequence, ad copy, content calendar) necesită în mod tradițional ore de copywriting și strategie. ChatGPT poate comprima acest proces la ore, dar fără o metodologie structurată produce output fragmentat și neintegrat. Această procedură creează un workflow end-to-end cu ChatGPT ca co-pilot de funnel building.

Situații acoperite:
- Lansarea rapidă a unui funnel pentru un produs nou
- Construirea primului funnel pentru un business nou
- Generarea de variante A/B pentru un funnel existent
- Crearea de funneluri pentru campanii sezoniere sau promoții

---

## 2. Procedura

### Pas 1: Foundation — Definește business goal și ICP
Înainte de orice prompt ChatGPT, documentează: obiectivul de business (vânzare produs X / capturare lead / înregistrare webinar), ICP-ul (vârstă, profesie, pain points primare, obiecții principale, canale frecventate), și propunerea de valoare unică (de ce tu și nu altcineva). Aceste 3 elemente vor fi incluse în fiecare prompt ca context mandatory.

### Pas 2: Strategy — Folosește ChatGPT pentru strategia completă de funnel
Prompt: „Ești un expert în marketing funnel pentru [industrie]. Clientul ideal este [ICP]. Obiectivul este [goal]. Propunerea de valoare este [USP]. Construiește o strategie completă de funnel: etapele, canalele de trafic recomandate, tipul de lead magnet, structura email sequence și KPI-urile de urmărit." Iterează până obții o strategie coerentă și specifică.

### Pas 3: Landing Page — Generează copy-ul de landing page
Cu contextul complet din Pasul 1, prompt ChatGPT: „Scrie copy complet pentru landing page-ul de capturare a lead-ului. Include: headline principal (beneficiu clar), subheadline (clarificare + credibilitate), 3 bullet points beneficii cheie, social proof placeholder, form copy (titlu + CTA button text), și o secțiune de FAQ cu 3 obiecții principale ale ICP-ului." Testează cel puțin 2 variante de headline.

### Pas 4: Email Sequences — Generează secvența completă de emailuri
Solicită ChatGPT să scrie email-ul complet pentru fiecare etapă (din M-076 structura): Welcome + livrare lead magnet, Valoare 1 (educațional), Valoare 2 (story/case study), Social proof email, Soft sell (prezentare soluție), Obiecții FAQ, Hard sell cu urgency/scarcity. Pentru fiecare: subject line, preview text (max 90 caractere), body complet, CTA. Specifică tonul: [profesional/prietenos/direct].

### Pas 5: Ad Copy — Generează copy pentru reclamele de trafic
Pentru fiecare canal de trafic din strategie (Meta/Google/LinkedIn), solicită ChatGPT: headline-uri (5 variante pentru testare), descriere (Primary text pentru Meta, Description pentru Google), CTA options (3-5 variante). Specifică limitele de caractere per platformă. Asigură-te că ad copy-ul este aliniat cu mesajul de pe landing page (message match).

### Pas 6: Content Calendar — Construiește calendarul de conținut cu AI
Solicită ChatGPT un calendar de conținut pe 30 zile care alimentează funnelul: tipul de conținut (blog/social/video), topic-ul specific aliniat cu etapa AIDA, platforma de distribuție, frecvența. Conținutul trebuie să creeze awareness pentru problema pe care o rezolvi și să direcționeze trafic spre landing page.

### Pas 7: Tracking — Definește framework-ul de tracking și optimizare
Listează toate punctele de tracking de setat: pixel Meta/Google pe landing page, UTM parameters pentru fiecare canal de trafic, evenimente de conversie (form submit, email click, purchase). Stabilește cadența de review: zilnic (ad performance), săptămânal (email stats), lunar (funnel conversion rate per etapă). Definește acțiunile de optimizare pentru fiecare scenariu de underperformance.

---

## 3. Cortex Logging

```json
{
  "text": "AI Marketing Funnel building cu ChatGPT executat pentru [produs/business]. Componente generate: landing page copy, [X] emailuri, ad copy [Y canale], content calendar 30 zile. Funnel live: [da/nu/în progres].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-102",
    "domain": "ai-marketing",
    "rule_id": "META-H-002",
    "tags": ["marketing-funnel", "chatgpt", "landing-page", "email-sequence", "ad-copy", "content-calendar"],
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
La construirea oricărui funnel de marketing cu ChatGPT

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Landing page generat fără cel puțin 2 variante de headline (A/B) → violation
- Funnel construit fără tracking framework (Pasul 7) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- Training Mode → curriculum ai-marketing
- M-076 AI Marketing Funnel Design → framework strategic complementar

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-102 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "AI Marketing Funnel Building with ChatGPT" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ChatGPT (GPT-4o) | Generare toate componentele de copy |
| Landing page builder (Webflow/Unbounce) | Implementare landing page |
| Email marketing platform (Mailchimp/Klaviyo) | Implementare email sequence |
| Ads Manager (Meta/Google) | Implementare ad campaigns |
| Analytics (GA4 + Pixel) | Tracking Pasul 7 |
| Cortex | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Componente funnel generate | 100% (landing + emails + ads + calendar) |
| Variante A/B headline | Min. 2 |
