---
type: procedure
created: 2026-03-22
status: active
slug: m-054-brand-story-messaging-framework
tags: [procedure, nexus]
---

# Brand Story and Messaging Framework — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Construirea unui framework de brand story și mesagerie consistentă care comunică valoarea unică a brandului și rezonează cu audiența țintă pe toate canalele.

---

## 1. Problema

Brandurile care nu au un framework de mesagerie clar comunică inconsistent pe diferite canale, diluând identitatea și creând confuzie în mintea publicului. Fără un brand story convingător, companiile se rezumă la a vorbi despre features și prețuri în loc să creeze conexiune emoțională cu audiența.

Situații acoperite:
- Definirea sau redefinirea mesajului central al unui brand
- Crearea de brand guidelines pentru echipe de marketing și content
- Lansarea unui brand nou sau rebranding al unui brand existent

---

## 2. Procedura

### Pas 1: Definirea "De ce"-ului brandului (Purpose)
Articula motivul fundamental pentru care brandul există dincolo de profit — inspirat de Golden Circle al lui Simon Sinek. Răspunde la: Ce problemă din lume vrea brandul să o rezolve? Ce schimbare vrea să o creeze în viețile clienților? Purpose-ul autentic devine ancora tuturor mesajelor și creează loialitate emoțională pe termen lung.

### Pas 2: Identificarea Unique Value Proposition (UVP)
Formulează UVP-ul în maximum 2 propoziții care răspund la: ce faci, pentru cine, și ce beneficiu unic oferi față de alternative. UVP-ul trebuie să fie specific, credibil și relevant pentru buyer persona definită. Testează UVP-ul cu clienți reali — dacă nu îl recunosc ca adevărat, revizuiește.

### Pas 3: Construirea brand story cu structura narativă
Structurează brand story-ul folosind framework-ul StoryBrand de Donald Miller: Personajul (clientul) → Problema → Ghidul (brandul) → Planul → Acțiunea → Evitarea eșecului → Succesul. Brandul nu este eroul — clientul este eroul, brandul este ghidul (Yoda, nu Luke Skywalker). Story-ul trebuie să fie autentic, emoțional și memorabil.

### Pas 4: Definirea vocii și tonului brandului
Stabilește 3-5 atribute de personalitate ale brandului (ex: expert, cald, direct, inovator, empatic). Definește ce face brandul și ce nu face în comunicare (ex: "Suntem directi, nu agresivi"; "Suntem experți, nu aroganți"). Creează exemple concrete de "spunem X, nu spunem Y" pentru situații comune. Voice rămâne constant; tonul variază per context.

### Pas 5: Crearea ierarhiei de mesaje
Construiește o ierarhie de mesaje pe 3 niveluri: (1) Mesajul umbrelă — one-liner pentru brand (tagline), (2) Mesaje de valoare — 3 beneficii principale cu suport narativ, (3) Mesaje specifice per segment/persona/canal. Această ierarhie asigură că toate materialele de marketing sunt aliniate și că nu există mesaje contradictorii.

### Pas 6: Documentarea framework-ului și crearea de brand messaging guide
Compilează toate elementele într-un Brand Messaging Guide: purpose, UVP, brand story rezumat, voice și ton, ierarhia de mesaje, exemple de aplicare per canal, și fraze de evitat. Documentul trebuie să fie practic și acționabil — nu o declarație abstractă de valori. Include exemple de headlines, intro-uri și CTA-uri care respectă framework-ul.

### Pas 7: Implementarea și alinierea echipelor
Distribuie Brand Messaging Guide tuturor celor care creează conținut sau comunică în numele brandului: marketing, sales, customer success, PR. Organizează un workshop de aliniere pentru a clarifica aplicarea practică. Implementează un proces de review al noilor materiale față de framework. Planifică revizuire anuală a brand story-ului cu input de la clienți și echipă.

---

## 3. Cortex Logging

```json
{
  "text": "Procedura M-054 executată: Brand story și messaging framework complet — purpose, UVP, brand story, voice și ierarhia de mesaje documentate și distribuite.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-054",
    "domain": "content-creation",
    "rule_id": "META-H-002",
    "tags": ["brand-story", "messaging-framework", "UVP", "brand-voice", "StoryBrand"],
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
- Conținut publicat care contrazice brand voice definit → violation
- UVP formulat fără validare cu clienți reali → violation
- Brand Messaging Guide creat dar nedistribuit echipei → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum content-creation

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] UVP validat cu minimum 3 clienți reali?
- [ ] Brand Messaging Guide distribuit tuturor echipelor relevante?

**VK-uri obligatorii:**
1. `✅ [PROC] M-054 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Brand Story and Messaging Framework" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Buyer Persona (M-053) | Audiența pentru care este construit brand story-ul |
| StoryBrand Framework | Metodologia de structurare narativă |
| Notion / Google Docs | Documentare și distribuire Brand Messaging Guide |
| Brand Identity Design (M-055) | Expresia vizuală a brand story-ului |
| Content Writing System (M-051) | Aplicare practică a messaging framework-ului |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Consistență mesaj pe canale | > 90% materiale aliniate |
| Adoption rate echipă | 100% membri instruiți |
| Recunoaștere UVP de clienți | > 70% în user testing |
| Revizuire periodică | Anual + la fiecare rebranding major |
