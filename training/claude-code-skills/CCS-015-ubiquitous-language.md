---
type: procedure
created: 2026-03-20
status: active
slug: ccs-015-ubiquitous-language
tags: [procedure, nexus]
---

# CCS-015 — Ubiquitous Language (DDD Glossary Extraction)

## Problema
Teams working on complex domains accumulate terminology drift: the same word used for different concepts (ambiguity), different words used for the same concept (synonyms), and vague overloaded terms that mean different things to different stakeholders. This drift causes miscommunication in code reviews, requirements discussions, and documentation. Domain-Driven Design's ubiquitous language practice formalizes terminology into a shared glossary that everyone — developers, product managers, domain experts — uses consistently.

## Procedura

### Prerequisites
- Conversation context with domain discussion (requirements, design, code review, or planning)
- Working directory accessible for writing `UBIQUITOUS_LANGUAGE.md`
- User available to validate canonical term choices

### Steps

1. **Scan the conversation for domain-relevant terms.** Read the full conversation history and extract: domain nouns (Order, Customer, Invoice, Shipment); domain verbs (fulfill, dispatch, confirm, cancel); domain states and lifecycle stages; actors and roles; system boundaries and subdomains. Skip generic programming concepts (array, function, endpoint) unless they have domain-specific meaning.

2. **Identify problems before proposing solutions.** Look for: same word used for different concepts (e.g., "account" meaning both a customer record and an authentication identity); different words used for the same concept (e.g., "customer", "client", "buyer", "account" all referring to the same entity); vague or overloaded terms (e.g., "process" used for 5 different operations); terms implied but never named.

3. **Be opinionated — pick the best term.** When multiple words exist for the same concept, choose the best one and list the others as "Aliases to avoid." The glossary is not a list of synonyms — it is a canonical vocabulary. Justify your choice (e.g., "Customer is preferred over Client because it reflects the transactional relationship — a Client implies a longer advisory relationship").

4. **Write definitions in one sentence.** Define what a term IS, not what it does. "An Order is a customer's request to purchase one or more items" (what it is). Not: "An Order handles the purchase flow and coordinates payment and fulfillment" (what it does).

5. **Group terms into natural clusters.** When multiple terms belong to the same subdomain or lifecycle, group them under a heading with their own table. Examples: Order lifecycle (Order, Invoice, Fulfillment, Shipment); People (Customer, User, Admin); Financial (Payment, Refund, Credit). If all terms belong to one cohesive domain, one table is fine — do not force groupings.

6. **Document relationships explicitly.** After the term tables, add a Relationships section using bold term names and expressing cardinality: "An Invoice belongs to exactly one Customer"; "An Order produces one or more Invoices"; "A Shipment is associated with exactly one Fulfillment." Relationships catch modeling errors that term definitions alone miss.

7. **Write an example dialogue.** Create a short conversation (3-5 exchanges) between a developer and a domain expert that uses the canonical terms precisely. The dialogue should: demonstrate how terms interact naturally; clarify boundaries between related concepts; show terms being used to resolve a real ambiguity. This is the most valuable section for onboarding new team members.

8. **Flag ambiguities explicitly.** In the "Flagged ambiguities" section, call out every case where a term was used ambiguously in the conversation with a clear recommendation. Example: "'account' was used to mean both Customer and User — these are distinct: a Customer places orders, a User is an authentication identity. Use Customer for order context, User for auth context."

9. **Write to `UBIQUITOUS_LANGUAGE.md`.** Create or overwrite the file in the working directory. Use the standard format: section heading per cluster with a term/definition/aliases-to-avoid table; Relationships section; Example dialogue section; Flagged ambiguities section.

10. **Output an inline summary.** After writing the file, output a brief summary in the conversation: what terms were captured, what ambiguities were flagged, what the main clusters are. This lets the user validate without opening the file immediately.

11. **State the forward commitment.** After output: "I've written/updated `UBIQUITOUS_LANGUAGE.md`. From this point forward I will use these terms consistently. If I drift from this language or you notice a term that should be added, let me know."

12. **Handle re-invocation correctly.** When invoked again in the same conversation: read the existing `UBIQUITOUS_LANGUAGE.md`; incorporate any new terms from subsequent discussion; update definitions if understanding has evolved; mark changed entries with "(updated)" and new entries with "(new)"; re-flag any new ambiguities; rewrite the example dialogue to incorporate new terms.

### Verification
- [ ] All major domain nouns, verbs, and states captured
- [ ] Every ambiguity explicitly flagged with a recommendation
- [ ] One canonical term chosen per concept (aliases-to-avoid listed)
- [ ] Relationships section documents cardinality between entities
- [ ] Example dialogue demonstrates terms interacting naturally
- [ ] Forward commitment stated after writing the file

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, ddd, ubiquitous-language, domain-modeling, glossary, v2

## Enforcement Loop
- WHERE: Any project with a non-trivial domain model
- WHEN: User says "define domain terms", "build a glossary", "create ubiquitous language", "domain model", or "DDD"
- HOW: Be opinionated (pick best terms); flag every ambiguity; write dialogue (most valuable section)
- CONNECT: CCS-001 (PRD terminology should align with ubiquitous language), CCS-014 (article editing for domain docs)
