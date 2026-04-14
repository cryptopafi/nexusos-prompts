---
type: procedure
created: 2026-03-22
status: active
slug: m-104-unbounce-landing-page
tags: [procedure, nexus]
---

# Unbounce Landing Page Creation System — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Creare completă landing page în Unbounce — de la template până la publicare pe domeniu custom, integrare CRM și monitorizare stats.

---

## 1. Problema

Unbounce permite crearea rapidă de landing pages fără cod, dar fără un proces standard rezultatele sunt inconsistente: pagini fără variantă A/B, copy slab, formulare neconectate la CRM și lipsă de monitorizare post-lansare.

Situații acoperite:
- Campanie nouă PPC care necesită landing page dedicat
- Launch produs sau ofertă cu deadline
- Test rapid de concept înainte de development full

---

## 2. Procedura

### Pas 1: Creează cont Unbounce și alege template
Accesează app.unbounce.com. Creează un nou landing page și alege template din biblioteca Unbounce:
- Filtrează după industrie și obiectiv (lead gen / click-through / sales)
- Alege template cu structură apropiată de ce ai nevoie
- Evită templates complexe dacă obiectivul este simplu

### Pas 2: Personalizează design-ul
Modifică template-ul în Smart Builder sau Classic Builder:
- **Logo** — înlocuiește logo-ul placeholder cu cel real
- **Culori** — aplică brand colors din hex codes
- **Fonturi** — aliniază cu brand font (Google Fonts disponibile)
- **Imagini** — înlocuiește stock photos cu imagini reale ale produsului/serviciului

Verifică că pagina arată bine pe mobil (toggle mobile view în editor).

### Pas 3: Scrie copy focusat pe conversie
Completează toate elementele de text:
- **Headline** — beneficiu principal clar, aliniat cu ad-ul sursă
- **Subheading** — clarificare sau support pentru headline
- **Bullet points** — 3-5 beneficii (nu features) ale ofertei
- **CTA button** — text acțional specific ("Obține demo gratuit" nu "Submit")

Evită text generic. Fiecare cuvânt trebuie să servească obiectivul de conversie.

### Pas 4: Configurează formularul și thank-you redirect
În secțiunea Form:
- Adaugă câmpuri minimale necesare (mai puține câmpuri = CR mai mare)
- Configurează validare pentru fiecare câmp
- Setează thank-you page redirect sau afișează mesaj de confirmare inline
- Testează formularul manual înainte de lansare

### Pas 5: Configurează varianta A/B
Creează o variantă B pentru test:
- Duplică pagina cu butonul "Add Variant"
- Modifică o singură variabilă (headline / CTA / imagine hero)
- Setează traffic split 50/50
- Definește conversion goal în Unbounce

### Pas 6: Publică pe domeniu custom
Configurează publishing:
- Mergi la Settings → Domain
- Conectează domeniu custom (adaugă CNAME în DNS: `unbouncepages.com`)
- Sau folosește subdomeniu dedicat (`lp.domeniu.ro`)
- Publică pagina și verifică că se încarcă corect pe domeniu

### Pas 7: Conectează la CRM sau email
Integrează formularul cu sistemul de captură lead:
- **Mailchimp / ActiveCampaign** — prin integrare nativă Unbounce
- **HubSpot / Salesforce** — prin Zapier sau webhook
- **Email simplu** — notificare email pentru fiecare submit

Testează că lead-urile ajung corect în CRM după submit.

### Pas 8: Monitorizează statisticile
Urmărește în dashboard-ul Unbounce:
- **Conversion rate** — per variantă (A vs B)
- **Bounce rate** — vizitatori care ies fără acțiune
- **Visits** — volum trafic pe pagină

Verifică zilnic în primele 7 zile. Declară câștigătorul A/B după minimum 100 conversii per variantă.

---

## 3. Cortex Logging

```json
{
  "text": "Unbounce Landing Page creat: template selectat, design personalizat, copy conversion-focused scris, formular configurat cu redirect, A/B test activ, publicat pe domeniu custom, integrat cu CRM, stats monitorizate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-104",
    "domain": "website-landing-pages",
    "rule_id": "META-H-002",
    "tags": ["unbounce", "landing-page", "ab-test", "crm", "conversion", "copy"],
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
- Training Mode → curriculum website-landing-pages

### VERIFY
- [ ] Toți pașii executați?
- [ ] Output satisface §1?
- [ ] VK emis?

**VK-uri:**
1. `✅ [PROC] M-104 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Unbounce Landing Page Creation System" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Unbounce | Platform landing page builder |
| CRM (HubSpot / ActiveCampaign / Mailchimp) | Captură și nurturing lead-uri |
| Zapier | Integrare CRM dacă nu există nativă |
| DNS provider | Configurare CNAME pentru domeniu custom |
| Google Analytics 4 | Tracking suplimentar conversii |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| A/B variante active | Minimum 2 |
| CRM integration | Funcțională și testată |
| Conversion rate baseline | Documentat la lansare |
