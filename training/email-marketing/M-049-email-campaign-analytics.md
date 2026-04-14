---
type: procedure
created: 2026-03-22
status: active
slug: m-049-email-campaign-analytics
tags: [procedure, nexus]
---

# Email Campaign Analytics and Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definește procesul de analiză sistematică a metricilor campaniilor de email și implementare de optimizări bazate pe date pentru îmbunătățirea continuă a deliverability, open rate, CTR și conversii.

---

## 1. Problema

Majoritatea echipelor de email marketing urmăresc superficial open rate-ul și ignoră metricile mai profunde care indică sănătatea reală a listei și performanța campaniilor: click-to-open rate, revenue per email, deliverability rate, sender score. Fără un proces structurat de analiză și iterație, campaniile de email stagnează sau se deteriorează în timp pe măsură ce lista îmbătrânește și algoritmii de inbox se schimbă.

Situații acoperite:
- Metrici de email în declin (open rate scăzut, CTR în scădere) fără diagnostic clar
- Campanii A/B testate fără metodologie riguroasă și documentare a learnings
- Deliverability probleme nedetectate până când rata de inbox placement scade semnificativ

---

## 2. Procedura

### Pas 1: Setup dashboard de metrici email și benchmark-uri de industrie
Configurează un dashboard centralizat (Google Sheets, Notion sau ESP nativ) cu metricile principale per campanie: open rate, click-through rate (CTR), click-to-open rate (CTOR), unsubscribe rate, bounce rate (hard vs. soft), spam complaint rate, revenue per email (dacă e-commerce). Stabilește benchmark-urile pentru industria ta: open rate mediu industrie (B2B: 25-35%, B2C: 20-25%, e-commerce: 15-20%). Toate metricile sub benchmark necesită investigare imediată.

### Pas 2: Analiză deliverability și inbox placement rate
Deliverability nu înseamnă doar "emailul a ajuns" — înseamnă că a ajuns în inbox, nu în spam sau tab Promotions. Verifică lunar: Google Postmaster Tools (domain reputation: High/Medium/Low, spam rate, feedback loop), Sender Score (senderscore.org), inbox placement rate cu un tool dedicat (GlockApps, Litmus Email Analytics) care testează livrarea la Gmail, Yahoo, Outlook, Apple Mail. O deliverability sub 90% inbox placement necesită audit tehnic și de conținut imediat.

### Pas 3: Segmentare audiență pentru analiză comparativă
Analizează metricile separat pe segmente cheie: sursa de înrolare (lead magnet vs. optim direct vs. social media), data înrolării (abonați noi < 30 zile vs. vechi > 6 luni), comportamentul de engagement (activi — au deschis în ultimele 90 zile vs. inactivi), tipul de client (potențial vs. existent vs. churned). Segmentele care under-performează față de media necesită strategii diferite de content sau re-engagement activ.

### Pas 4: Analiză subiect line și preheader pentru open rate optimization
Exportă datele istorice ale tuturor subject line-urilor din ultimele 6-12 luni și identifică pattern-urile care generează open rate peste medie: lungimea optimă (benchmark: 6-10 cuvinte), utilizarea numelui (personalizare crește OR cu 10-15%), tipuri de hook (curiozitate, beneficiu direct, urgency, cifre specifice, întrebare). Creează un "swipe file" intern cu top 20 subject lines care au performat și tipologiile de copie asociate.

### Pas 5: Analiză CTR și CTOR pentru optimizare conținut email
Click-through rate scăzut cu open rate ridicat indică o problemă de conținut sau CTA, nu de subject line. Analizează: poziția primului CTA în email (above the fold vs. mijloc vs. final), formatul CTA (hyperlink text vs. buton, culori), numărul de CTA-uri per email (optim: 1 CTA principal, maxim 3 total), congruența între promisiunea subject line-ului și conținutul emailului. CTOR target: > 15% (dacă cineva a deschis, 15% ar trebui să și dea click pe link-ul principal).

### Pas 6: Protocol de re-engagement pentru abonați inactivi
Definește "inactiv" (ex: nu a deschis niciun email în 90 zile). Lansează secvența de re-engagement: Email 1 — "Te-am pierdut? Uite ce ai ratat" (highlight conținut de valoare), Email 2 — "Ești încă interesat de [topic]?" cu un singur CTA clar, Email 3 — "Ultima șansă — rămân sau mă dezabonez?" cu opțiune explicită de preferințe (frecvență, topicuri). Abonații care nu răspund după 3 emailuri de re-engagement sunt eliminați din lista activă — aceasta îmbunătățește deliverability și reduce costurile ESP.

### Pas 7: Raport lunar de performance și planning iterații
Redactează lunar un raport simplu (1 pagină) cu: performanța lunii curente vs. luna anterioară vs. aceeași lună anul trecut, top 3 campanii performante și learnings, bottom 3 campanii și cauze, 3 ipoteze de testat luna viitoare (A/B tests planificate), sănătatea listei (creștere netă abonați activi). Documentează orice schimbare de strategie sau setup tehnic care ar putea explica variații neașteptate. Raportul servește ca memorie instituțională și ghid de optimizare continuă.

---

## 3. Cortex Logging

```json
{
  "text": "Email Campaign Analytics & Optimization executat: dashboard creat, deliverability auditat, re-engagement protocol activ, raport lunar redactat.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-049",
    "domain": "email-marketing",
    "rule_id": "META-H-002",
    "tags": ["email-analytics", "deliverability", "open-rate", "ctr", "ctor", "re-engagement", "a-b-testing"],
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
- Analiză limitată la open rate fără CTR, CTOR, deliverability rate → violation
- Abonați inactivi (+90 zile) menținuți fără protocol de re-engagement → violation
- Raport lunar lipsă sau nedocumentat (learnings pierdute) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum email-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Dashboard cu minimum 7 metrici cheie activ și actualizat?
- [ ] Deliverability verificată cu Google Postmaster Tools și inbox placement tool?

**VK-uri obligatorii:**
1. `✅ [PROC] M-049 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Email Campaign Analytics and Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ESP Analytics (ActiveCampaign / Mailchimp etc.) | Date primare per campanie |
| Google Postmaster Tools | Monitorizare reputație domeniu și spam rate |
| GlockApps / Litmus | Inbox placement rate testing |
| senderscore.org | Sender Score monitorizare |
| Google Sheets / Notion | Dashboard centralizat și rapoarte lunare |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Open rate | > 25% (benchmark general) |
| CTR | > 2.5% |
| CTOR | > 15% |
| Inbox placement rate | > 90% |
| Spam complaint rate | < 0.1% |
