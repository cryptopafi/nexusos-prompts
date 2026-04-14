---
type: procedure
created: 2026-03-22
status: active
slug: m-100-scarcity-urgency-campaign
tags: [procedure, nexus]
---

# Scarcity and Urgency Campaign Execution — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Planificarea și executarea campaniilor bazate pe scarcity și urgency cu deadline-uri și limitări reale, pentru creșterea conversiei fără erodarea credibilității brandului.

---

## 1. Problema

Campaniile de scarcity și urgency false (countdown timers resetate, stocuri inventate, oferte "limitate" permanent disponibile) erodează ireversibil credibilitatea brandului și reduc conversia pe termen lung. Lipsa unui proces structurat duce la urgency artificială detectabilă de consumatori, cu efect contrar celui dorit.

Situații acoperite:
- Lansare produs/ofertă cu fereastră temporală reală sau cu stoc limitat efectiv
- Campanie promoțională sezonieră cu deadline autentic (Black Friday, end-of-quarter, aniversare)
- Ofertă cu capacitate limitată reală (locuri în program, ore de consultanță disponibile, producție limitată)

---

## 2. Procedura

### Pas 1: Validarea legitimității scarcity/urgency
Înainte de orice altceva, documentează în scris baza reală pentru scarcity sau urgency: data exactă de expirare a ofertei, numărul real de unități/locuri disponibile, costul real care crește după deadline, capacitatea efectivă limitată. Dacă nu există o bază reală și verificabilă, procedura se oprește — nu se continuă cu urgency artificială.

### Pas 2: Definirea structurii campaniei
Stabilește tipul de limitare: time-based (deadline fix), quantity-based (stoc/locuri limitate), sau access-based (early bird / founding members). Definește toate detaliile campaniei: data start, data end (fixă și irevocabilă), ce se întâmplă exact după deadline (prețul crește cu X%, oferta dispare, locurile se ocupă). Documentează toate acestea în brief-ul de campanie.

### Pas 3: Crearea mesajelor de urgency autentică
Redactează copy-ul pentru toate touchpoint-urile (email, landing page, ads, pop-up) cu focus pe consecința reală a inacțiunii, nu pe presiune fabricată. Utilizează cifre exacte: "47 locuri rămase" (dacă real) în loc de "stoc limitat". Formulează beneficiul pierdut concret: "Prețul revine la 497$ pe 15 martie la ora 23:59". Evită formulările vagi sau hiperbolicele.

### Pas 4: Implementarea mecanismelor tehnice de urgency
Setează countdown timer legat de un timestamp fix (nu cookie-based resetabil). Configurează actualizarea dinamică a stocului (dacă quantity-based) sincronizată cu inventarul real. Testează că la expirarea deadline-ului oferta dispare sau prețul se modifică automat conform promisiunii. Verifică pe toate device-urile și browserele.

### Pas 5: Secvența de comunicare pre-deadline
Planifică secvența de email/notificări: anunț ofertă → reminder la 48h înainte de deadline → reminder la 24h → reminder la 2h (opțional pentru oferte mari) → confirmare expirare. Fiecare mesaj trebuie să reitereze deadline-ul exact și consecința reală. Nu trimite mai mult de 4 emails per campanie de urgency.

### Pas 6: Executarea și monitorizarea campaniei live
Lansează campania conform planului. Monitorizează în timp real: funcționarea countdown-ului, actualizarea stocului, livrabilitatea emailurilor, conversia. La epuizarea stocului real sau expirarea deadline-ului, execută imediat modificările promise (preț nou, ofertă inactivată). Nu extinde deadline-ul sau stocul post-factum — aceasta este o violation critică de credibilitate.

### Pas 7: Post-mortem și documentare
La finalizarea campaniei, documentează: total conversii, revenue generat, uplift față de campanie fără urgency, feedback clienți. Verifică că toate promisiunile de scarcity au fost respectate integral. Actualizează procedure-health.json și Cortex cu learningurile pentru optimizarea campaniilor viitoare.

---

## 3. Cortex Logging

```json
{
  "text": "Campanie scarcity/urgency executată cu bază reală validată, mecanisme tehnice funcționale, toate promisiunile respectate la deadline.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-100",
    "domain": "conversion-optimization",
    "rule_id": "META-H-002",
    "tags": ["scarcity", "urgency", "campaign", "cro", "deadline", "credibility"],
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
- Urgency creată fără deadline real documentat → violation (credibilitate)
- Deadline extins sau stoc "reapărut" după expirare → violation critică de credibilitate
- Countdown timer resetabil per cookie în loc de timestamp fix → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum conversion-optimization

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Baza reală pentru scarcity/urgency documentată în Pas 1?
- [ ] La expirarea deadline-ului, toate promisiunile respectate fără excepții?

**VK-uri obligatorii:**
1. `✅ [PROC] M-100 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Scarcity and Urgency Campaign Execution" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Email platform (Klaviyo/ActiveCampaign) | Secvența automată de comunicare pre-deadline |
| Countdown timer tool (Deadline Funnel / server-side) | Urgency tehnică autentică, non-resetabilă |
| CMS / E-commerce platform | Actualizarea automată preț/disponibilitate la deadline |
| procedure-health.json | Tracking execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Promisiuni scarcity respectate | 100% fără excepție |
| Uplift conversie față de campanie fără urgency | ≥15% |
| Deadline-uri extinse post-factum | 0 |
