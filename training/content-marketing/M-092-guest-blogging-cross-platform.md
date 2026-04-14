---
type: procedure
created: 2026-03-22
status: active
slug: m-092-guest-blogging-cross-platform
tags: [procedure, nexus]
---

# Guest Blogging and Cross-Platform Collaboration — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Atragerea de trafic și autoritate prin guest blogging strategic și colaborări cross-platform cu creatori din nișe complementare.

---

## 1. Problema

Creșterea organică exclusiv pe propriile canale este lentă și plafonate de dimensiunea audienței existente. Guest blogging și colaborările cross-platform permit accesul la audiențe noi calificate, construirea de backlink-uri și creșterea autorității de brand în nișă. Fără o strategie structurată, aceste oportunități sunt valorificate aleatoriu sau deloc.

Situații acoperite:
- Identificarea și outreach-ul pentru oportunități de guest post pe site-uri din nișă
- Planificarea și executarea unei colaborări cross-platform cu un creator complementar
- Crearea unui sistem repetabil de partnership-uri pentru creștere organică continuă

---

## 2. Procedura

### Pas 1: Identificarea țintelor potrivite pentru guest blogging
Criteriile de selecție pentru site-uri de guest post: Domain Authority (DA) ≥ 40 (verificat cu Moz/Ahrefs), audiență similară sau complementară (nu competitoare directe), publică conținut recent (ultimele 30 zile), acceptă explicit guest posts (pagina "Write for us" sau precedente evidente). Creează o listă de 20-30 de site-uri țintă, ordonate după DA și relevanță.

### Pas 2: Research-ul aprofundat al publicației țintă
Înainte de outreach, studiază: top 5 articole publicate (după share-uri și comentarii), tonul și stilul editorial, audienița lor (categoriile, comentariile, rețelele lor sociale), subiectele care nu au fost acoperite dar ar interesa audiența lor. Research-ul este vizibil în pitch și diferențiază un pitch acceptat de unul ignorat.

### Pas 3: Crearea pitch-ului personalizat
Structura pitch-ului eficient: (1) Deschidere personalizată care arată că ai citit publicația; (2) Propunere de 2-3 titluri de articole cu unghiuri unice pentru audiența lor; (3) Social proof scurt: link spre un guest post publicat anterior sau cel mai performant articol propriu; (4) Bio scurt (2 propoziții); (5) Oferta de a trimite draft sau outline la cerere. Lungime totală: max 150 cuvinte. Personalizarea este non-negociabilă.

### Pas 4: Trimiterea și urmărirea outreach-ului
Trimite pitch-ul prin formularul oficial de contact sau emailul editorial specificat. Dacă nu ai răspuns în 7 zile: un singur follow-up. Dacă nu ai răspuns după follow-up: marchează ca inactiv și treci la următorul target. Nu trimite mai mult de 2 mesaje. Menține un tracker (spreadsheet/Notion) cu: site, data pitch, status, follow-up, răspuns.

### Pas 5: Crearea conținutului de guest post de calitate excepțională
Articolul de guest post trebuie să fie cel puțin la fel de bun ca cel mai bun conținut de pe site-ul tău propriu — dacă nu mai bun. Include: research original sau statistici actualizate, exemple specifice pentru audiența publicației, min. 1 link intern spre conținut existent al lor (demonstrezi că ai citit site-ul), bio de autor cu link natural spre resursa ta relevantă (nu homepage generic).

### Pas 6: Planificarea colaborărilor cross-platform cu creatori
Identifică creatori din nișe complementare (nu competitoare) cu audiențe similare ca dimensiune (±30%). Tipuri de colaborare: Instagram/TikTok takeover reciproc, episod de podcast cross-guest, webinar co-hosted, newsletter swap, challenge comun. Contactează cu propunere specifică de valoare pentru ambele audiențe. Documentează termenii: ce oferă fiecare, datele, ce se creează.

### Pas 7: Amplificarea și măsurarea rezultatelor
Promovează activ fiecare guest post sau colaborare pe toate canalele tale. Monitorizează: trafic de referral din fiecare guest post (UTM tracking), noi backlink-uri obținute, creșterea DA propriu, abonați noi atrași din fiecare sursă. Calculează ROI per colaborare și prioritizează tipurile cu cel mai mare return pentru iterare.

---

## 3. Cortex Logging

```json
{
  "text": "Guest blogging și cross-platform collaboration executate conform M-092: site-uri identificate, pitch-uri personalizate trimise, conținut de calitate creat, colaborare planificată, rezultate măsurate.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-092",
    "domain": "content-marketing",
    "rule_id": "META-H-002",
    "tags": ["guest-blogging", "collaboration", "backlinks", "content-marketing", "outreach"],
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
- Conținut distribuit fără adaptare per canal (guest post trimis neadaptat stilului publicației) → violation
- Pitch trimis fără personalizare specifică pentru publicația țintă → violation
- Mai mult de 2 follow-up-uri trimise aceleiași publicații → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Pitch personalizat cu research vizibil al publicației?
- [ ] Tracker de outreach actualizat?

**VK-uri obligatorii:**
1. `✅ [PROC] M-092 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Guest Blogging and Cross-Platform Collaboration" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Ahrefs / Moz | Verificarea DA și research backlink-uri |
| Outreach tracker (Notion/spreadsheet) | Managementul pipeline-ului de guest posts |
| UTM builder + Analytics | Tracking trafic din fiecare sursă |
| Email outreach tool (Hunter.io) | Găsirea emailurilor editoriale |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Rata de acceptare pitch-uri | ≥ 20% |
| Backlink-uri noi per guest post | ≥ 1 dofollow DA 40+ |
| Trafic referral per guest post | ≥ 200 vizitatori unici |
