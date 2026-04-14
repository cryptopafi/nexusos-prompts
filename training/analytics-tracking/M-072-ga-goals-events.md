---
type: procedure
created: 2026-03-22
status: active
slug: m-072-ga-goals-events
tags: [procedure, nexus]
---

# Google Analytics Goal and Event Configuration — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Configurare completă a goal-urilor și event-urilor în GA4 — de la definirea conversiilor până la testarea cu DebugView și monitorizarea săptămânală.

---

## 1. Problema

GA4 fără goals și events configurate corect este o platformă care trackuiește trafic dar nu conversii. Fără events personalizate, nu există date despre comportamentul utilizatorilor dincolo de page views. Fără conversii marcate, optimizarea campaniilor este imposibilă.

Situații acoperite:
- Setup inițial GA4 care necesită configurare events și goals
- Site existent cu GA4 dar fără conversii definite
- Audit analytics pentru a verifica integritatea tracking-ului

---

## 2. Procedura

### Pas 1: Definește goal-urile de conversie
Inventariază toate acțiunile valoroase pe care utilizatorii le pot face:
- **Tranzacție** — finalizare cumpărătură (`purchase`)
- **Lead** — completare formular contact (`generate_lead`)
- **Signup** — înregistrare cont/newsletter (`sign_up`)
- **Engagement** — vizualizare video complet, download PDF, scroll 90% pe pagina cheie

Documentează fiecare goal cu: nume, valoare estimată, volum așteptat per lună.

### Pas 2: Creează events custom în GA4 pentru fiecare goal
GA4 → Configure → Events → Create Event:
- Creează event pe baza unui event existent sau de la zero
- Adaugă condiții de declanșare (ex: `page_location contains /multumim`)
- Sau configurează via GTM pentru mai multă flexibilitate

Exemplu event formular: declanșat pe `form_submit` cu condiția că `page_path` conține `/contact`.

### Pas 3: Marchează events cheie ca conversii
GA4 → Configure → Events:
- Găsește event-ul în listă (apare după prima declanșare)
- Toggle "Mark as conversion" = ON
- Sau mergi la Configure → Conversions → New conversion event și introdu numele

Conversiile apar în raportul Conversions după 24-48h de la marcare.

### Pas 4: Configurează trigger-ele în GTM
Pentru fiecare event custom, creează trigger în GTM:
- **Form Submit trigger** — pentru formulare (verifică că Form ID sau Page Path este corect)
- **Click trigger** — pentru butoane CTA specifice
- **Custom event trigger** — pentru events transmise din codul site-ului

Publicează containerul GTM după fiecare modificare. Testează în GTM Preview Mode înainte de publicare.

### Pas 5: Testează events cu GA4 DebugView
Activează debug mode:
- Extensie Chrome **Tag Assistant** de la Google → toggle debug
- Sau adaugă `?_gl=1` și activează din GTM Preview

Accesează GA4 → Configure → DebugView:
- Execută manual fiecare acțiune trackuită (completează formular, apasă buton)
- Verifică că events apar în real-time în DebugView cu parametrii corecți
- Confirmă că conversiile sunt înregistrate corect

### Pas 6: Creează rapoarte de goal completion
GA4 → Reports → Engagement → Conversions:
- Verifică că toate conversiile apar și au volume realiste
- Adaugă dimensiune secundară "Session source/medium" pentru attribution
- Creează raport personalizat în Library cu: Conversions per channel / Conversion rate per landing page

Salvează raportul în Reports Library pentru acces rapid.

### Pas 7: Monitorizează ratele de conversie săptămânal
Setează reminder săptămânal (luni dimineața):
- Deschide raportul Conversions
- Compară săptămâna curentă vs precedentă
- Identifică scăderi neașteptate (posibil indiciu de tracking broken)
- Documentează anomaliile și investighează cauza

O scădere bruscă de peste 50% a conversiilor poate indica: tracking broken, site down, schimbare UX sau problemă tehnică.

---

## 3. Cortex Logging

```json
{
  "text": "GA4 Goals & Events configurate: conversii definite per tip (purchase/lead/signup/engagement), events create și marcate ca conversii, triggers GTM configurați, DebugView validat, rapoarte create, monitorizare săptămânală activată.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-072",
    "domain": "analytics-tracking",
    "rule_id": "META-H-002",
    "tags": ["ga4", "goals", "events", "conversii", "gtm", "debugview", "tracking"],
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
1. `✅ [PROC] M-072 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Google Analytics Goal and Event Configuration" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Google Analytics 4 | Platformă analytics și conversii |
| Google Tag Manager | Implementare triggers și events |
| Tag Assistant (Chrome Extension) | Activare DebugView |
| GA4 DebugView | Testare events în real-time |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Conversii marcate în GA4 | Minimum 1 per obiectiv business |
| DebugView validare | Toate events confirmate |
| Monitorizare săptămânală | Activă |
