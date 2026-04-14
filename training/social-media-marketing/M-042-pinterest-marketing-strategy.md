---
type: procedure
created: 2026-03-22
status: active
slug: m-042-pinterest-marketing-strategy
tags: [procedure, nexus]
---

# Pinterest Marketing Strategy — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definește procesul complet de creare, optimizare și scalare a prezenței pe Pinterest pentru trafic organic și conversii.

---

## 1. Problema

Pinterest este un motor de căutare vizual cu ciclu de viață al conținutului mult mai lung decât alte platforme sociale, dar necesită o strategie specifică de board-uri, keyword-uri și design de Pin-uri pentru a genera trafic consistent. Fără o structură clară, efortul de creare conținut nu se convertește în reach sau click-uri.

Situații acoperite:
- Cont Pinterest nou sau subdezvoltat care nu generează trafic organic
- Strategie de Pin-uri fără optimizare SEO internă Pinterest
- Lipsă de consistență în publicare și structura board-urilor

---

## 2. Procedura

### Pas 1: Audit cont și configurare profil business
Comută la cont Pinterest Business dacă nu este deja. Completează bio-ul cu keyword-ul principal al nișei (max 160 caractere), adaugă URL site verificat și activează Rich Pins (Article sau Product). Verifică că Pinterest Tag este instalat pe site pentru tracking conversii.

### Pas 2: Cercetare keyword-uri și nișă Pinterest
Folosește bara de search Pinterest pentru autocomplete — notează 10-15 sugestii long-tail pentru nișa ta. Verifică secțiunea "Related searches" și "Trending" pentru sezonalitate. Construiește o listă de keyword-uri primare (3-5) și secundare (10-15) care vor fi folosite în titluri, descrieri și board-uri.

### Pas 3: Structurare board-uri optimizate SEO
Creează minimum 8-12 board-uri tematice relevante pentru nișă. Fiecare board primește: titlu cu keyword principal (ex: "Email Marketing Tips for Beginners"), descriere de 200-500 caractere cu keyword-uri naturale, cover image reprezentativ. Primul board trebuie să fie cel mai relevant pentru audiența țintă.

### Pas 4: Design Pin-uri cu template-uri Canva/Adobe
Creează 3-5 template-uri de Pin cu dimensiunea optimă 1000×1500px (2:3 ratio). Fiecare template include: titlu text overlay bold lizibil, branding subtil (logo + culori), imagine de fundal de calitate înaltă. Variază stilurile: infografic, quote, tutorial step-by-step, produs, before/after.

### Pas 5: Optimizare titlu și descriere Pin
Scrie titluri de Pin cu keyword principal în primele 40 de caractere (afișate în feed). Descrierile trebuie să aibă 150-300 caractere, să includă 3-5 keyword-uri relevante natural integrate și un CTA clar (ex: "Salvează pentru mai târziu" sau "Click pentru ghid complet"). Adaugă link direct către landing page sau articol.

### Pas 6: Calendar de publicare și scheduling
Programează 5-15 Pin-uri pe zi folosind Tailwind sau Pinterest Scheduler nativ. Distribuie publicarea în intervalele de vârf: 8-11 PM, 2-4 AM (pentru audiență US), 14-16 (pentru audiență EU). Repinează conținut de calitate din nișă (20-30% din total) pentru a menține activitate și a construi relații cu creatori relevanți.

### Pas 7: Analiză metrici și iterație lunară
Revizuiește lunar în Pinterest Analytics: impresii, saves, outbound clicks și click-through rate per Pin. Identifică top 5 Pin-uri ca outbound clicks și recrează variante similare. Elimină board-urile cu engagement sub 1% și consolidează conținut. Target: CTR > 0.5%, saves rate > 2%, outbound clicks creștere lunară 15%.

---

## 3. Cortex Logging

```json
{
  "text": "Pinterest Marketing Strategy executată: board-uri structurate, Pin-uri optimizate SEO, calendar publicare activ.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-042",
    "domain": "social-media-marketing",
    "rule_id": "META-H-002",
    "tags": ["pinterest", "visual-search", "seo", "pin-design", "trafic-organic"],
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
- Pin-uri publicate fără keyword în titlu sau descriere → violation
- Board-uri create fără descriere optimizată SEO → violation
- Lipsă Pinterest Tag pe site la momentul lansării campaniei → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum social-media-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Pinterest Business cont activ cu Rich Pins activate?
- [ ] Minimum 8 board-uri create cu descrieri SEO?

**VK-uri obligatorii:**
1. `✅ [PROC] M-042 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Pinterest Marketing Strategy" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Pinterest Business Account | Platformă principală + analytics |
| Canva / Adobe Express | Design template-uri Pin-uri |
| Tailwind / Pinterest Scheduler | Scheduling și optimizare timing |
| Pinterest Tag (pixel) | Tracking conversii pe site |
| Google Analytics | Verificare trafic inbound din Pinterest |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Outbound CTR | > 0.5% per Pin |
| Saves rate | > 2% din impresii |
| Creștere lunară trafic Pinterest | +15% outbound clicks |
