---
type: procedure
created: 2026-03-20
status: active
slug: training-mode
tags: [procedure, nexus]
---

# TRAINING-MODE v2.1 — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-02-28 | **Rescris**: 2026-02-28 | **Optimized**: 2026-02-28
**Versiune**: 2.1
**Regulă asociată**: COM-H-001 (Session management — extinsă pentru learning sessions)
**Scope**: Mod de lucru structurat pentru sesiuni de învățare — cursuri, skill building, business education, document review.
**Research basis**: Bloom's Taxonomy Revised (Anderson & Krathwohl 2001), SM-2 Spaced Repetition (Wozniak), Mastery Learning (Bloom/Guskey), Testing Effect (Roediger & Karpicke 2006), Adaptive Learning / ZPD (Vygotsky/Bjork), Self-Regulated Learning / SRL (Zimmerman 2002), Metacognitive Prompting (Wang et al. 2023), Webb's Depth of Knowledge (1997), Protege Effect / Learning by Teaching (Chase et al. Stanford 2009), FSRS state model (Open Spaced Repetition), xAPI/SCORM state persistence (ADL/IEEE), Affective Scaffolding in AI Tutors (2025).

---

## Ce este Training Mode — ghid pentru utilizator

Training Mode transformă Genie dintr-un executor de taskuri într-un **facilitator de învățare structurată**. Activează-l când vrei să **înveți** ceva, nu doar să **faci** ceva.

### Când îl activezi
- Parcurgi un curs (ECHELON, Coursera, Udemy, YouTube)
- Vrei să înțelegi un subiect nou ("învață-mă despre X")
- Ai un document sau procedură nouă de asimilat
- Precision Mode a detectat un skill gap pe care vrei să-l acoperi
- Business training cu Leo

### Ce face diferit față de normal mode
- **Stabilește obiectiv clar** înainte de a începe (nu "citim și vedem")
- **Alege metoda** de predare potrivită conținutului (nu one-size-fits-all)
- **Verifică** ce ai reținut, nu doar ce ai citit (active recall, nu passive reading)
- **Adaptează** dificultatea și ritmul la tine (nu merge mai departe dacă n-ai înțeles)
- **Salvează** ce ai învățat în Cortex (nu pierzi knowledge între sesiuni)
- **Programează review** spaced repetition (nu uiți peste 3 zile)
- **Se poate activa și mini** în taskuri normale când detectează un knowledge gap

### Cum îl activezi
- Scrie: `training`, `learning mode`, sau `training mode`
- Dezactivare: `training off` sau `normal`
- Genie poate sugera activare, dar NU activează singur

### Ce vei vedea
- `📚 TRAINING` la începutul fiecărui răspuns
- Checkpoint-uri cu scor de confidence (1-5) după fiecare concept
- Întrebări de verificare (active recall) — Genie nu acceptă "da, am înțeles"
- La final: raport cu ce ai învățat, ce trebuie repetat, ce lipsește încă

---

## 1. Problema

### Ce nu funcționează fără Training Mode

Sesiunile de learning (Leo business training, Pafi technical courses, ECHELON content) au 6 probleme:

1. **Fără obiectiv** — se începe cu "citim un articol" fără a defini ce se vrea: înțelegere conceptuală? skill practic? evaluare pentru decizie?
2. **Consum ≠ absorbție** — se parcurge material fără a verifica ce s-a reținut. Studiile (Roediger & Karpicke 2006) arată: retrieval practice reține 50% mai mult decât re-reading.
3. **Sursele neevaluate** — un YouTube transcript (T5) primește aceeași greutate ca documentația oficială (T1).
4. **Metodă unică** — totul e "citește și rezumă". Dar conținut factual, conceptual și procedural necesită metode diferite.
5. **Fără retention** — se învață azi, se uită mâine. Fără spaced repetition, 80% din informație se pierde în 7 zile (Ebbinghaus).
6. **Skills nesalvate** — knowledge rămâne în sesiune, nu se transferă în Cortex.

### Ce rezolvă Training Mode

- Entry point clasificat → obiectiv clar
- Metoda adaptată la tip de conținut → absorbție maximă
- Source Selection Framework → surse evaluate
- Active recall + mastery gates → verificare reală
- SM-2 spaced repetition → retention pe termen lung
- Cortex save → zero skills pierdute

---

## 2. Procedura

### 2.1 Activare / Dezactivare

**Activare**: `training`, `learning mode`, `training mode`
**Dezactivare**: `training off`, `normal`
**Indicator**: `📚 TRAINING` la începutul fiecărui răspuns
**Auto-suggest** (NU auto-activa): ECHELON course, Leo business session, skill gap detectat, document nou de asimilat
**Embedded mode**: Se poate activa ca micro-training (2-3 min) în taskuri normale — vezi §2.7.
**Persistence (compaction-safe)**:
- La activare → write `~/.claude/active-mode.md`:
  ```
  TRAINING
  activated: {date}
  topic: {subiect învățare}
  entry_type: {COURSE/TOPIC/GAP/DOCUMENT}
  ```
- La dezactivare (`training off` / `normal`) → clear `~/.claude/active-mode.md` (write empty string)
- Restaurare automată: Session Start Checklist Step 1.5 citește fișierul și reactivează modul.

### 2.2 Entry Classification — GATE (primul pas)

Clasifică entry type-ul ÎNAINTE de orice:

| Entry Type | Trigger | Ce se schimbă |
|-----------|---------|---------------|
| **COURSE** | Link curs, "parcurge cursul X", ECHELON content | Materialul există. Source Plan = assess quality. Structura vine din curs. |
| **TOPIC** | "Învață-mă despre X", "ce e Y" (>lookup simplu) | Genie caută materiale. Source Plan = find + evaluate. Structura se creează. |
| **GAP** | Precision checkpoint, research gap, skill lipsă | Gap-ul definește obiectivul. Source Plan minimal, focusat. |
| **DOCUMENT** | PDF, articol, procedură nouă, spec tehnic | Documentul e materialul. Source Plan = assess doc quality. Extrage concepte cheie. |

Emit VK: `📚 [ENTRY] type: COURSE/TOPIC/GAP/DOCUMENT | source: "titlu/link"`

### 2.3 OBJECTIVE — ACTION step 0

Definește ce se învață și de ce:

- **Topic**: Ce anume? (specific, nu vag)
- **Goal type** — clasifică folosind Bloom's Taxonomy:
  - **REMEMBER** — reținere factuală (definiții, date, formule)
  - **UNDERSTAND** — înțelegere conceptuală (pot explica altcuiva)
  - **APPLY** — aplicare practică (pot folosi în context nou)
  - **ANALYZE** — analiză (pot compara, diferenția, identifica patterns)
  - **EVALUATE** — evaluare (pot judeca calitate, pot lua decizie informată)
  - **CREATE** — creare (pot produce ceva nou pe baza cunoștințelor)
- **Prior knowledge**: Cortex search pe topic + self-assessment (none/basic/intermediate/advanced)
- **Scope boundary**: Ce NU învățăm acum (evită scope creep)
- **Success criterion**: Cum știm că am reușit? (ex: "pot configura X independent", "pot explica Y lui Leo")

Emit VK: `📚 [OBJECTIVE] "topic" | bloom: LEVEL | prior: LEVEL | success: "criterion"`

**Bloom escalation rule**: NU evalua la nivel mai înalt fără a confirma mastery la nivel inferior. Remember → Understand → Apply → ... secvențial.

### 2.4 SOURCE PLAN — ACTION step 1

Depinde de entry type:

| Entry Type | Ce faci |
|-----------|---------|
| **COURSE** | Evaluează cursul pe Source Selection Framework (tier?). Extrage structura (module/lecții). Identifică materiale suplimentare dacă cursul e T4-T5. |
| **TOPIC** | Cortex search → external search. Aplică Source Selection Framework tiers. Budget surse per depth (D1=1-2, D2=3-5). Structurează conținut în unități logice. |
| **GAP** | Gap-ul e deja definit. Caută 1-2 surse targetate (preferabil T1-T2). |
| **DOCUMENT** | Evaluează documentul (tier, quality checks). Identifică concepte cheie. Structurează în unități de learning. |

Emit VK: `📚 [SOURCES] topic | T1:{n} T2:{n} T3:{n} T4:{n} T5:{n} | units: {n} planned`

### 2.5 LEARNING EXECUTION — ACTION step 2

**Pasul central** — aici se face învățarea efectivă. Structurat în 3 faze SRL per learning unit (Zimmerman 2002):

#### SRL Phase 1: FORETHOUGHT (înainte de fiecare unit)
- "Ce știi deja despre acest topic?" (prior knowledge activation)
- "Ce vrei să poți face după ce termini?" (goal setting)
- "Cum prefer să înveți asta?" (strategy selection — oferă opțiuni)

#### SRL Phase 2: PERFORMANCE (durante — metoda adaptivă)

| Tip Conținut | Metodă | Flow |
|-------------|--------|------|
| **Factual** (definiții, date, formule) | Summary → Key Points → Verify | Rezumat concis → 3-5 puncte cheie → testează cu recall |
| **Conceptual** (principii, teorii, "de ce") | Socratic Q&A → Examples → Counter-examples | Întrebări ghidate → user descoperă → consolidează |
| **Procedural** (cum se face X, step-by-step) | Explain → Demo → Exercise → Feedback | Explică → demonstrează → user execută → feedback |
| **Analytical** (comparații, evaluări, decizii) | Case Study → Framework → Apply | Caz real → framework decizie → aplică la contextul propriu |

**Metacognitive prompting** (Wang et al. 2023) — NU accepta "da, am înțeles". Prompt through:
1. "Care e răspunsul tău inițial?" (preliminary judgment)
2. "De ce crezi asta?" (reasoning)
3. "Ce ar putea fi greșit?" (critical evaluation)
4. "Care e răspunsul final?" (decision)
5. "Cât de sigur ești?" (confidence)

**Frustration detection** (affective scaffolding): Dacă user dă 2+ răspunsuri greșite consecutive, răspunsuri scurte/terse, sau exprimă frustrare explicit → switch de la assessment la encouragement: "Conceptul ăsta e genuine dificil. Hai să încercăm altă abordare." Schimbă modalitatea (narativ → exemplu → exercițiu ghidat).

#### SRL Phase 3: SELF-REFLECTION (după fiecare unit)
- "Ce ai învățat?" (self-evaluation — user articulează, nu Genie)
- "Ce a fost greu?" (causal attribution)
- "Ce ai face diferit data viitoare?" (adaptive reaction)

#### Assessment Diversity (Webb DOK + Bloom)

| DOK Level | Assessment Type | Când se folosește | Bloom echivalent |
|-----------|----------------|-------------------|-----------------|
| **DOK 1** Recall | Free recall, cued recall, MCQ | Early mastery check, vocabular | Remember |
| **DOK 2** Skill/Concept | Compare, classify, explain | Skill building, aplicare | Understand, Apply |
| **DOK 3** Strategic | Design solution, justify with evidence | Transfer, intermediate mastery | Analyze, Evaluate |
| **DOK 4** Extended | Teach-back, project, multi-session investigation | Deep mastery, capstone | Create |

**Teach-back assessment** (Protege Effect, Chase et al. Stanford 2009): La DOK 3-4, user "predă" conceptul → Genie joacă rolul unui learner confuz care pune întrebări de clarificare. User nu poate preda ce nu înțelege profund. Impact: +50% retention vs re-reading.

**Adaptive difficulty** (ZPD research):
- Target accuracy band: **70-85%** — zona optimă de învățare
- Sub 70% după 2 attempts → **simplify**: switch modality, reduce complexity, verifică prerequisites
- Peste 85% pe 3 consecutive → **advance**: crește DOK level, avansează la Bloom level superior
- 2 cicluri corrective eșuate → **probe prerequisites**: verifică conceptele de bază

**User adaptation** (MODIFIER activ pe tot parcursul):
- **Leo** → limbaj simplu, analogii business, fără jargon tehnic, vizual când posibil
- **Pafi** → shorthand tehnic, comenzi concrete, cod exemplu, link direct la docs

**Per learning unit** (concept/capitol/exercițiu):
1. SRL Forethought (prior knowledge + goal)
2. Prezintă conținut folosind metoda adaptivă
3. Assessment la DOK level potrivit (nu doar recall — variază)
4. SRL Self-Reflection ("Ce ai învățat? Ce a fost greu?")
5. Evaluează → confidence score → decide: avansează / reinforce / simplify
6. Emit learning checkpoint (step 3)

### 2.6 LEARNING CHECKPOINT — ACTION step 3

Se emite **după fiecare unitate de learning** (1 concept / 1 capitol / 1 exercițiu).

**Verificare**:
- Ce s-a învățat? (1-3 takeaways concrete — articulate de user, nu de Genie)
- Confidence level (1-5, din Bloom + mastery research):

| Score | Nivel | Bloom echivalent | Mastery | Spaced Rep interval |
|-------|-------|------------------|---------|---------------------|
| 1 | Am auzit de asta | Remember (parțial) | Nu | Review mâine (1d) |
| 2 | Înțeleg ideea generală | Understand (parțial) | Nu | Review în 3 zile |
| 3 | Pot explica altcuiva | Understand (complet) | Threshold (≈80%) | Review în 7 zile |
| 4 | Pot aplica independent | Apply / Analyze | Da (85%+) | Review în 14 zile |
| 5 | Pot preda / adapta creativ | Evaluate / Create | Mastered | Review în 30 zile |

- Tip: **applied** (am practicat) sau **theoretical** (am citit/ascultat)
- Gaps: întrebări rămase, concepte neclare

**Mastery gate** (din Bloom/Guskey research):
- Confidence ≥ 3 (mastery threshold 80%+) → avansează la unitatea următoare
- Confidence 2 → reinforce: re-explain cu altă metodă, apoi re-test
- Confidence 1 → simplify: verifică prerequisites, descompune conceptul, re-teach
- Maxim 2 cicluri corrective pe concept. Dacă tot sub 3 → flag prerequisite gap.
- Re-assessment-ul trebuie să folosească **întrebări diferite** (nu aceleași — per Guskey)

Emit VK: `📚 [LEARN] "concept" | confidence: N/5 | type: applied/theoretical | gaps: N | mastery: pass/reinforce/prerequisite`

### 2.7 Embedded Mode — MODIFIER

Training Mode poate rula **mini** în taskuri normale, fără a activa modul complet.

**Trigger**: Genie detectează knowledge gap în timpul unui task normal.
**Flow**:
1. Emit: `📚 [GAP] "concept" detected | suggest: micro-training ~2-3 min | skip?`
2. Dacă user acceptă → micro-training: 1-2 concepte, 1 active recall question
3. Dacă user skip → loghează gap în Cortex cu `type: pending_training`, propune la next session
4. **Self-audit loop**: După 3 sesiuni cu embedded training, Genie evaluează:
   - Cât a durat efectiv? (target: 2-3 min, max 5)
   - A fost util? (user a aplicat knowledge-ul mai târziu în sesiune?)
   - Adaptează: dacă > 5 min constant → reduce scope; dacă user skip > 50% → reduce frequency

**Nu întrerupe niciodată**: task-uri urgente, debugging activ, Codex queue processing, producție.

### 2.8 SKILL EXTRACTION — ACTION step 4

Dacă learning-ul a produs knowledge actionable:

1. Aplică FORGE template (META-H-002) dacă e procedură
2. Save Cortex cu metadata learning:
   ```json
   {
     "type": "learned_skill",
     "confidence": N,
     "source_tier": "T{N}",
     "bloom_level": "remember|understand|apply|analyze|evaluate|create",
     "learning_method": "factual|conceptual|procedural|analytical",
     "next_review": "YYYY-MM-DD",
     "interval_days": N,
     "ease_factor": 2.5,
     "review_count": 0,
     "learning_session": true
   }
   ```
3. **Spaced repetition scheduling** (SM-2 simplificat):
   - Confidence 1 → next review: +1 zi
   - Confidence 2 → next review: +3 zile
   - Confidence 3 → next review: +7 zile
   - Confidence 4 → next review: +14 zile
   - Confidence 5 → next review: +30 zile (sau graduate)
   - La fiecare review: dacă recall corect → interval * ease_factor. Dacă incorect → reset la +1 zi.

Emit VK: `📚 [SKILL] "title" | confidence: N/5 | bloom: LEVEL | review: YYYY-MM-DD | source: T{N}`

### 2.9 ACTIVE RECALL — Review la Session Start

La începutul fiecărei sesiuni (indiferent de mod — normal, precision, training):

1. Check Cortex: skills cu `next_review ≤ today`
2. Dacă există → prezintă 2-3 recall questions (free recall prioritar):
   - "Explică-mi X"
   - "Cum ai aplica Y în context Z?"
3. Evaluează răspunsul:
   - Corect → advance interval (interval * ease_factor)
   - Parțial → menține interval
   - Incorect → reset la 1 zi
4. Update Cortex metadata: `review_count++`, `next_review`, `interval_days`, `ease_factor`
5. Emit VK: `📚 [RECALL] {N} due | {correct}/{total} correct | next batch: {date}`

**Nu depășește 3 min** la session start. Dacă > 5 skills due → prioritizează cele cu cel mai vechi review.

### 2.10 SESSION REVIEW — ACTION step 5

La `training off`, `/sync`, sau exit:

- **Topics covered**: N concepts, N units
- **Avg confidence**: X/5 (target: ≥3.5)
- **Applied ratio**: Y% (target: ≥40% — nu doar teoretic)
- **DOK distribution**: câte assessments per DOK level (target: nu toate DOK 1)
- **Bloom distribution**: câte concepts per level (target: nu toate la Remember)
- **Mastery**: N passed / M total (target: ≥80%)
- **Key takeaways**: top 3 (salvate în Cortex)
- **Gaps remaining**: ce trebuie învățat next (salvate ca next-session TODO)
- **Knowledge delta**: ce știam înainte vs acum
- **Review scheduled**: N skills cu next_review setat
- **SRL reflection** (Zimmerman self-reflection phase):
  - Ce a funcționat bine? Ce metodă a produs cel mai mare confidence lift?
  - Ce nu a funcționat? Unde s-a blocat learning-ul?
  - Ajustare pentru next session: metodă default, pacing, scope per unit

Emit VK: `📚 [TRAINING-REVIEW] session | topics: N | avg_conf: X/5 | applied: Y% | DOK: 1:{n} 2:{n} 3:{n} 4:{n} | mastery: N/M | gaps: N | review_scheduled: N`

### 2.11 MULTI-SESSION CONTINUITY — State Persistence

**Problema**: Un curs ECHELON de 20 lecții se parcurge pe 5+ sesiuni. Fără state persistence, fiecare sesiune începe de la zero.

**Session state model** (inspirat din FSRS + xAPI + SCORM):

La `training off` sau session exit, salvează în Cortex:
```json
{
  "text": "Training State: {course_title} | position: {unit_N}/{total} | mastery: {passed}/{total} | next: {unit_title}",
  "collection": "procedures",
  "metadata": {
    "type": "training_state",
    "course_id": "unique_identifier",
    "course_title": "",
    "position": 0,
    "total_units": 0,
    "completed_units": [],
    "current_unit": "",
    "mastery_map": {},
    "misconceptions": [],
    "preferred_method": "conceptual",
    "avg_confidence": 0,
    "gaps_remaining": [],
    "last_session": "YYYY-MM-DD",
    "forge_bypass": true,
    "forge_bypass_reason": "training_state_not_procedure"
  }
}
```

**Ce se persistă** (per SCORM minimum + ITS extensions):
1. **Position** — unde a rămas (unitate curentă, progres %)
2. **Mastery map** — per concept: confidence, DOK level atins, review status
3. **Misconceptions** — concepte confundate, erori recurente (din ITS research)
4. **Preferred method** — ce modalitate a funcționat mai bine pentru acest user pe acest curs
5. **Gaps remaining** — ce trebuie acoperit la next session

**La session resume**:
1. Check Cortex: `training_state` cu `course_id` matching
2. Dacă există → afișează: `📚 [RESUME] "{course_title}" | position: {N}/{total} | last: {date} | gaps: {N}`
3. Propune: "Continuăm de la unitatea {N} ({title}), sau preferă recap?"
4. Dacă last_session > 7 zile → micro-recap (2-3 recall questions din unitățile anterioare) ÎNAINTE de a continua

---

## 2.12 Enforcement Chain Map

**ACTION steps** (emit VK propriu):
| Step | Chain | Rule | VK |
|------|-------|------|----|
| Entry classification | TRAINING.ENTRY | COM-H-001 | `📚 [ENTRY]` |
| Objective | TRAINING.0 | COM-H-001 + Bloom | `📚 [OBJECTIVE]` |
| Source plan | TRAINING.1 | REASON-H-001 + SOURCE-SELECTION | `📚 [SOURCES]` |
| Learning execution | TRAINING.2 | TRAINING MODE + SRL + Webb DOK | (adaptive method — output vizibil) |
| Learning checkpoint | TRAINING.3 | TRAINING MODE + Mastery Learning | `📚 [LEARN]` |
| Skill extraction | TRAINING.4 | MEM-H-002 + META-H-002 | `📚 [SKILL]` |
| Active Recall review | TRAINING.RECALL | TRAINING MODE + SM-2/FSRS | `📚 [RECALL]` |
| Session review | TRAINING.5 | TRAINING MODE + SRL Self-Reflection | `📚 [TRAINING-REVIEW]` |
| Multi-session state | TRAINING.STATE | TRAINING MODE + xAPI/SCORM | `📚 [RESUME]` |

**MODIFIER steps** (schimbă comportament, nu emit VK separat):
| Step | Chain | Ce modifică |
|------|-------|-------------|
| SRL 3-phase | TRAINING.SRL | Forethought→Performance→Self-Reflection per unit (Zimmerman 2002) |
| Adaptive difficulty | TRAINING.D | DOK + accuracy 70-85%=menține, <70%=simplify, >85%x3=advance |
| User adaptation | TRAINING.U | Leo=simplu+analogii, Pafi=tehnic+comenzi |
| Frustration detection | TRAINING.F | 2+ wrong → encouragement mode, switch modality (affective scaffolding) |
| Source tagging | TRAINING.S | Fiecare fapt tagat cu sursă + tier |
| Embedded mode | TRAINING.E | Mini-training 2-3 min în taskuri normale, cu self-audit loop |

### Entry/Exit VKs

```
📚 [TRAINING] ON | entry: COURSE/TOPIC/GAP/DOCUMENT | topic: "descriere" | user: pafi/leo
📚 [ENTRY] type: COURSE | source: "titlu"
📚 [OBJECTIVE] "topic" | bloom: UNDERSTAND | prior: basic | success: "criterion"
📚 [SOURCES] topic | T1:2 T2:1 T3:0 T4:0 T5:0 | units: 4 planned
... (learning units + checkpoints) ...
📚 [LEARN] "concept A" | confidence: 4/5 | type: applied | gaps: 0 | mastery: pass
📚 [LEARN] "concept B" | confidence: 2/5 | type: theoretical | gaps: 1 | mastery: reinforce
📚 [SKILL] "title" | confidence: 4/5 | bloom: APPLY | review: 2026-03-14 | source: T1
📚 [TRAINING-REVIEW] session | topics: 4 | avg_conf: 3.5/5 | applied: 50% | mastery: 3/4 | gaps: 1
📚 [TRAINING] OFF
```

---

## 3. Cortex Logging

### Session log (la finalul fiecărei training session):
```json
{
  "text": "Training Session: {topic} | {N} concepts | avg_confidence: {X}/5 | applied: {Y}% | bloom: R:{n} U:{n} Ap:{n} | mastery: {passed}/{total} | gaps: {Z}. Key: {takeaway1}, {takeaway2}",
  "collection": "procedures",
  "metadata": {
    "type": "training_session",
    "procedure": "TRAINING-MODE",
    "rule_id": "COM-H-001",
    "tags": ["training", "learning"],
    "topic": "",
    "entry_type": "COURSE|TOPIC|GAP|DOCUMENT",
    "goal_bloom": "remember|understand|apply|analyze|evaluate|create",
    "user": "pafi|leo",
    "concepts_count": 0,
    "avg_confidence": 0,
    "applied_ratio": 0,
    "mastery_ratio": 0,
    "gaps_count": 0,
    "skills_saved": 0,
    "reviews_scheduled": 0,
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

### Skill-uri individuale (per concept learned):
```json
{
  "text": "LEARNED: {concept} | {explanation} | source: {source_name} (T{N})",
  "collection": "procedures",
  "metadata": {
    "type": "learned_skill",
    "confidence": 0,
    "bloom_level": "",
    "source_tier": "T1",
    "learning_method": "factual|conceptual|procedural|analytical",
    "next_review": "YYYY-MM-DD",
    "interval_days": 0,
    "ease_factor": 2.5,
    "review_count": 0,
    "mastery_achieved": false,
    "learning_session": true,
    "forge_bypass": true,
    "forge_bypass_reason": "learned_skill_not_procedure"
  }
}
```

### Recall review log (per review session):
```json
{
  "text": "Recall Review: {N} skills tested | {correct}/{total} correct | avg_ease: {X}",
  "collection": "procedures",
  "metadata": {
    "type": "recall_review",
    "procedure": "TRAINING-MODE",
    "skills_tested": 0,
    "correct_count": 0,
    "total_count": 0,
    "forge_bypass": true,
    "forge_bypass_reason": "recall_log_not_procedure"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- La activarea `training` / `learning mode` / `training mode`
- La orice sesiune ECHELON cu user Leo
- La research D2+ care se transformă în structured learning
- Embedded: în taskuri normale când knowledge gap detectat
- Active Recall: la session start (indiferent de mod) dacă skills due for review

### WHEN
- **Manual**: Pafi/Leo activează explicit
- **Auto-suggest**: Genie sugerează la trigger-uri (ECHELON, skill gaps, curs nou, document review)
- **Embedded**: Genie propune micro-training în task normal (user decide)
- **Session start**: Active Recall check automat (≤3 min)
- **Session end**: Review obligatoriu la dezactivare sau exit

### HOW (violation detection)
- Training session fără `📚 [ENTRY]` → violation (entry type neclasificat)
- Training session fără `📚 [OBJECTIVE]` la start → violation (obiectiv lipsă)
- Learning units fără `📚 [LEARN]` checkpoint → violation (min 1 per sesiune)
- Checkpoint fără confidence score → violation (mastery negătat)
- Session exit fără `📚 [TRAINING-REVIEW]` → violation
- Applied ratio < 20% pe 3+ sesiuni consecutive → warning (prea mult teoretic)
- Skills learned dar nesalvate în Cortex → violation MEM-H-002
- Active Recall skills due but skipped 3+ sesiuni → warning (spaced rep abandonat)
- Embedded micro-training > 5 min → warning (self-audit trebuie să reducă scope)

### CONNECT
- **COM-H-001** → Training Mode e extensie a session management
- **MEM-H-002** → Skills learned → Cortex save obligatoriu
- **META-H-002** → FORGE template pe skills procedurale (learned_skills get bypass)
- **SOURCE-SELECTION-FRAMEWORK** → Source planning la TRAINING.1
- **REASON-H-001** → DARE framework pentru TOPIC entry type (source discovery)
- **ECHELON** → Course tracking integration
- **Precision Mode** → Skill gaps din checkpoints pot triggera Training Mode
- **Session Start Checklist** → Active Recall se integrează la Step 6 (briefing)

### VERIFY (procedural checkpoint)
- [ ] Entry type clasificat? (COURSE/TOPIC/GAP/DOCUMENT)
- [ ] Obiectiv setat cu Bloom level?
- [ ] Surse evaluate cu tier-uri?
- [ ] Cel puțin 1 learning checkpoint cu confidence score?
- [ ] Skills salvate în Cortex cu spaced rep metadata?
- [ ] Session review emis cu statistici complete?

**Două VK-uri obligatorii per procedură FORGE** (per VK-H-001):
1. **Execution VK**: `✅ [PROC] TRAINING-MODE | ENTRY✓ OBJ✓ SRC✓ LEARN✓(N) SKILL✓(N) REVIEW✓ | complete`
2. **Save VK**: `✅ [CORTEX] "Training Session {topic}" | FORGE ✓ | rule: COM-H-001 | v2.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Training session execution | Genie direct (Sonnet) | Session context required |
| Complex concept explanation | Genie + Opus subagent | Deep synthesis for difficult topics |
| Leo business training | Genie direct | User Adaptation modifier handles simplification |
| Active Recall questions | Genie direct | Low complexity, needs session context |
| Socratic Q&A facilitation | Genie direct | Needs real-time interaction |
| Source discovery (TOPIC entry) | Genie + subagents | Parallel source search |

---

## 5. Dependențe

| Componentă | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Source Selection Framework | Source planning | `~/.nexus/procedures/SOURCE-SELECTION-FRAMEWORK.md` |
| ECHELON | Course tracking | `~/.nexus/echelon/` |
| Cortex KB | Skill + recall storage | `http://localhost:6400/api/store` |
| User profiles | Adaptation per user | `memory/users/{user}.md` |
| Precision Mode | Skill gap detection → training trigger | CLAUDE.md §PRECISION MODE |
| Session Start Checklist | Active Recall at step 6 | CLAUDE.md §Session Start |
| FORGE | Template for procedural skills | `~/.nexus/procedures/FORGEBUILD.md` |

---

## 6. Metrics

| Metrică | Ce măsoară | Target |
|---------|-----------|--------|
| Avg confidence per session | Calitatea absorbției | ≥ 3.5/5 |
| Applied ratio | Practică vs teorie | ≥ 40% applied |
| Skills saved | Knowledge captured | ≥ 1 per session |
| Mastery rate | Concepts mastered / total | ≥ 80% |
| Bloom distribution | Nu toate la Remember | ≥ 30% la Apply+ |
| Gap closure rate | Gaps closed vs opened | Trend descrescător |
| Active Recall accuracy | Retention la review | ≥ 75% correct |
| Review compliance | Skills due → actually reviewed | ≥ 90% |
| Embedded duration | Mini-training time | ≤ 3 min avg |
| Ease factor trend | Learning efficiency | Stable or rising |

---

## Checklist Pre-Publicare

- [x] Regulă asociată există (COM-H-001)
- [x] Enforcement loop complet: WHERE + WHEN + HOW + CONNECT + VERIFY
- [x] VERIFY checkpoint prezent
- [x] Procesul din WHERE referențiază această procedură
- [ ] Entry adăugat în `procedure-health.json`
- [ ] Salvat în Cortex cu metadata
- [x] Descrie CE nu CUM (zero cod inline >10 linii)
- [x] VK format specificat (per VK-H-001)
- [x] ACTION/MODIFIER classification pe fiecare step
- [x] Chain Map cu Type + VK columns
- [x] Entry/Exit VKs definite
- [x] Onboarding description pentru first-time users
- [x] Research basis documented (Bloom, SM-2, Mastery Learning, Testing Effect, ZPD)
- [x] Adaptive learning thresholds cu valori numerice concrete
- [x] Spaced repetition intervals definite
- [x] Embedded mode cu self-audit loop
