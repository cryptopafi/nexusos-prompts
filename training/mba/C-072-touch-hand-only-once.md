---
type: procedure
created: 2026-03-22
status: active
slug: c-072-touch-hand-only-once
tags: [procedure, nexus]
---

# Apply the "Touch Hand Only Once" Principle — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Aplicarea principiului GTD de single-handling al taskurilor pentru eliminarea re-procesării și menținerea inbox-zero discipline.

---

## 1. Problema

Re-deschiderea aceluiași email, task sau document fără a-l finaliza este una dintre cele mai mari pierderi de timp productive. Fiecare "atingere" suplimentară a unui item consumă energie cognitivă fără output. Principiul "Touch Hand Only Once" (OHIO) forțează o decizie imediată și completă la primul contact cu orice item.

Situații acoperite:
- Email deschis, citit și închis fără acțiune — deschis din nou a doua zi
- Task adăugat pe listă dar amânat repetat fără procesare reală
- Document sau fișier "parcat" temporar ajunge să fie re-găsit și re-evaluat de multiple ori

---

## 2. Procedura

### Pas 1: Adoptă regula celor 4D la primul contact
La primul contact cu orice item aplică imediat una din cele 4 decizii: Do (fă-l acum dacă < 2 min), Delegate (atribuie altcuiva cu deadline clar), Defer (planifică în calendar cu dată concretă), Delete (elimină dacă nu are valoare). Nicio excepție, nicio "parcare" temporară.

### Pas 2: Stabilește sesiuni fixe de procesare inbox
Nu lăsa inbox-ul deschis permanent. Alocă 2–3 ferestre fixe pe zi (ex: 9:00, 13:00, 17:00) pentru procesarea completă a inbox-ului. În afara acestor ferestre, inbox-ul este închis. Fiecare item procesat = ieșit din inbox.

### Pas 3: Creează un sistem de captură unificat
Toate inputurile (email, mesaje, idei, taskuri verbale) intră într-un singur inbox/sistem de captură. Fără liste paralele, fără sticky notes răzlețe, fără "îmi amintesc eu". Unicitatea sistemului elimină re-descoperirea.

### Pas 4: Procesează inbox-ul la zero la fiecare sesiune
Scopul fiecărei sesiuni de procesare este inbox zero — nu "am mai redus". Fiecare item din sesiune primește o decizie 4D și iese din inbox. Un inbox de 500 emails niciodată nu se rezolvă parțial; se rezolvă prin decizie per item.

### Pas 5: Aplică principiul OHIO la documente și fișiere
Când deschizi un document pentru revizuire sau editare, finalizează acțiunea în acea sesiune sau nu-l deschide. "Îl uit deschis" = re-procesare garantată. Dacă nu ai timp complet, nu începe — defer explicit în calendar.

### Pas 6: Elimină re-procesarea prin template-uri și răspunsuri pregătite
Răspunsurile la situații repetitive (cereri comune, follow-up-uri standard) se tratează cu template-uri. Creează 5–10 template-uri pentru cele mai frecvente tipuri de comunicare. Prima atingere = răspuns trimis.

### Pas 7: Review săptămânal al sistemului (GTD Weekly Review)
O dată pe săptămână, verifică: există itemi în inbox > 24h fără decizie? Există taskuri "defer" cu date trecute? Există liste paralele create ad-hoc? Curăță și recentralizează. Sistemul se degradează fără review.

---

## 3. Cortex Logging

```json
{
  "text": "C-072 Touch Hand Only Once executat: principiu OHIO aplicat, inbox processat la zero, sistem de captură unificat menținut.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "C-072",
    "domain": "mba",
    "rule_id": "META-H-002",
    "tags": ["productivity", "GTD", "inbox-zero", "OHIO", "single-handling", "time-management"],
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
- Item procesat fără una din cele 4D aplicate → violation
- Inbox lăsat cu itemi nefinalizați la sfârșitul sesiunii de procesare → violation
- Liste paralele de taskuri detectate → violation sistem
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum mba

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Inbox la zero după sesiunea de procesare?
- [ ] GTD Weekly Review efectuat în ultimele 7 zile?

**VK-uri obligatorii:**
1. `✅ [PROC] C-072 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Apply the Touch Hand Only Once Principle" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Sistem de task management (ex: Things, Todoist) | Inbox unificat + defer scheduling |
| Template-uri de comunicare | Răspunsuri rapide la situații repetitive |
| Calendar blocat pentru sesiuni inbox | Feronerie de timp fixă |
| procedure-health.json | Logging execuție procedură |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Inbox zero atins per sesiune | 100% sesiuni |
| Items re-procesate (atinse de 2+ ori) | < 5% din total |
| GTD Weekly Review cadență | 1x/săptămână |
