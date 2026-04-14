---
type: procedure
created: 2026-03-22
status: active
slug: m-048-email-nurture-sequence-design
tags: [procedure, nexus]
---

# Email Nurture Sequence Design — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Definește procesul de design și implementare a secvențelor de nurture email care ghidează abonații de la awareness la decizia de cumpărare cu metrici măsurabile per email.

---

## 1. Problema

O secvență de email nurture prost designată livrează conținut neadaptat la stadiul de conștientizare al abonatului, bombardând cu oferte premature sau educând fără niciodată a avansa spre conversie. Lipsa unui framework clar de nurture duce la rate de conversie sub 1% din lista totală, deși potențialul real al email marketingului este de 5-15x ROAS față de publicitate plătită.

Situații acoperite:
- Secvență de welcome care livrează lead magnetul și se oprește fără nurture ulterior
- Secvențe cu conținut identic pentru toți abonații indiferent de comportament
- Open rate-uri în scădere accelerată după primele 3 emailuri din secvență

---

## 2. Procedura

### Pas 1: Maparea Customer Journey și definire etape nurture
Definește cele 3-5 etape ale journey-ului specific nișei tale: Awareness (știe că are problema), Consideration (caută soluții), Evaluation (compară opțiunile), Intent (pregătit să cumpere), Loyalty (client existent). Fiecare etapă necesită conținut diferit — nu pitch vânzare unui abonat în etapa Awareness. Decide structura secvenței: număr emailuri per etapă (recomandat 3-5 emailuri per etapă pentru B2C, 5-7 pentru B2B).

### Pas 2: Design secvență Welcome (primele 7 zile)
Email 1 (imediat): Livrare lead magnet + introducere scurtă + setare așteptări ("Vei primi X emailuri în X zile despre Y"). Email 2 (zi 2): Poveste/origine — de ce ai creat business-ul, ce problemă ai rezolvat pentru tine. Email 3 (zi 4): Valoare pură educativă — cel mai bun sfat pe care îl poți da gratuit. Email 4 (zi 7): Social proof — studii de caz sau testimoniale + primul CTA soft (nu vânzare directă: "citește articolul", "ascultă episodul", "alătură-te grupului"). Obiectiv welcome: construire relație, nu conversie imediată.

### Pas 3: Design secvență de educație și awareness (săptămânile 2-4)
Trimite 2-3 emailuri pe săptămână cu conținut care: demontează mituri din nișă, prezintă framework-ul tău unic de rezolvare a problemei, arată consecințele nerezolvării problemei (urgency fără manipulare), răspunde la obiecții frecvente. La finalul fiecărui email, include un CTA consistent (nu vânzare — resurse gratuite, conținut deep-dive, quiz, comunitate). Open rate target pentru această etapă: > 30%.

### Pas 4: Design secvență de Consideration și prezentare soluție (săptămânile 3-5)
Introduce gradual produsul/serviciul ca soluție la problema documentată în etapele anterioare. Structura recomandată: Email 1 — "Iată cum arată viața după [problema rezolvată]" (aspirational), Email 2 — Studiu de caz detaliat cu cifre reale (social proof), Email 3 — Comparație opțiuni (DIY vs. cu ajutor vs. cu produsul tău), Email 4 — Introducere produs cu focus pe beneficii nu features, Email 5 — FAQ și obiecții principale. CTR target per email: > 3%.

### Pas 5: Design secvență de conversie (launch sau evergreen)
Secvența de conversie (3-5 emailuri în 5-7 zile): Email 1 — Anunț ofertă cu beneficiu principal, Email 2 — Urgency/scarcity legitimă (locuri limitate, bonus care expiră, preț early-bird), Email 3 — Testimoniale și social proof masiv, Email 4 — FAQ obiecții vânzări, Email 5 — Ultimele ore (scarcity reală). Subject lines de conversie: personalizare + urgency. Rată de conversie target din această secvență: 2-5% din lista segmentată (calificați).

### Pas 6: Implementare ramuri condiționale bazate pe comportament
Configurează în ESP automation-uri condiționale: dacă a deschis emailul X → merge în ramura de interes accelerat; dacă nu a deschis emailul X → primește un re-send cu subject line diferit la 48 ore; dacă a dat click pe link de produs → primește secvența de conversie accelerată; dacă nu a deschis 5 emailuri consecutive → intră în secvența de re-engagement. Ramurile condiționale cresc CTR-ul global cu 20-40%.

### Pas 7: Testare A/B sistematică și optimizare secvențe
Testează un singur element per experiment la un moment dat: subject line (personaliza vs. curiozitate vs. beneficiu direct), ziua și ora de trimitere, lungimea emailului (scurt < 200 cuvinte vs. lung > 600 cuvinte), CTA (text link vs. buton, poziție în email, culoare buton), sender name (brand vs. persoană). Minim 30% din listă per varianta de test pentru semnificativitate statistică. Documentează câștigătorii și aplică learnings la toate secvențele noi.

---

## 3. Cortex Logging

```json
{
  "text": "Email Nurture Sequence Design executat: welcome sequence, educație, consideration și conversie design implementate cu ramuri condiționale.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-048",
    "domain": "email-marketing",
    "rule_id": "META-H-002",
    "tags": ["email-nurture", "sequence-design", "automation", "customer-journey", "conversion", "a-b-testing"],
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
- Secvență de vânzare lansată fără etapă de nurture/educație prealabilă → violation
- Automation fără ramuri condiționale bazate pe comportament → violation
- A/B testing absent din secvențele active → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum email-marketing

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Customer Journey mapat cu etape distincte și conținut diferit per etapă?
- [ ] Ramuri condiționale configurate bazat pe comportament (open/click)?

**VK-uri obligatorii:**
1. `✅ [PROC] M-048 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Email Nurture Sequence Design" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| ActiveCampaign / Klaviyo / ConvertKit | ESP cu automation vizual și ramuri condiționale |
| Customer Journey Map | Framework pentru designul secvenței |
| Google Analytics / UTM Builder | Tracking conversii din emailuri pe site |
| Loom / Canva | Creare conținut vizual pentru emailuri |
| Litmus / Email on Acid | Testare rendering email pe clienți de mail diferiți |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Open rate secvență welcome | > 40% |
| CTR secvență educație | > 3% |
| Rata de conversie secvență vânzări | 2-5% din segmentul calificat |
| Unsubscribe rate per email | < 0.3% |
