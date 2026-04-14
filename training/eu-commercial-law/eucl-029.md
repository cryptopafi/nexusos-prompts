---
type: procedure
created: 2026-03-17
status: active
slug: eucl-029
tags: [procedure, nexus]
---

# EUCL-029 — Digital Services Act (Regulation 2022/2065)

## Problema
Online intermediaries fail to comply with tiered DSA obligations on content moderation, transparency, and illegal content handling, risking fines up to 6% of global turnover.

## Procedura
### Prerequisites
- EUCL-001 (legal system), understanding of platform business models

### Steps
1. Determine **service category** and applicable obligation tier:
   - **All intermediary services** (mere conduit, caching, hosting): transparency reporting, point of contact, legal representative in EU, terms of service clarity
   - **Hosting services** (including platforms): notice-and-action mechanisms for illegal content (Art. 16), statement of reasons for content moderation decisions (Art. 17)
   - **Online platforms** (hosting + dissemination to public): internal complaint handling (Art. 20), out-of-court dispute settlement (Art. 21), trusted flaggers (Art. 22), measures against misuse (Art. 23), transparency in advertising (Art. 26), recommender system transparency (Art. 27)
   - **Very Large Online Platforms/Search Engines (VLOPs/VLOSEs)** (>45M monthly active users in EU): systemic risk assessment (Art. 34), mitigation measures (Art. 35), independent audit (Art. 37), data access for researchers (Art. 40), additional advertising transparency (Art. 39), crisis response (Art. 36)
2. Implement **notice-and-action** (Art. 16): accept notices of illegal content, process expeditiously, inform notifier of decision, provide statement of reasons. Electronic submission system required
3. Apply **liability exemptions** (Art. 4-6, successor to E-Commerce Directive Art. 12-14): mere conduit, caching, hosting — exempted if no actual knowledge of illegal content and act expeditiously upon obtaining knowledge. No general monitoring obligation (Art. 8)
4. For **VLOPs/VLOSEs**: conduct annual **systemic risk assessment** covering dissemination of illegal content, negative effects on fundamental rights, public health, minors, democratic processes, gender-based violence, public security
5. Implement **advertising transparency**: online platforms must label ads clearly, identify advertiser, disclose main targeting parameters. VLOPs: maintain ad repository accessible via API. No targeting based on profiling using special categories (Art. 9 GDPR data) or targeting minors
6. Appoint **compliance officer** (VLOPs/VLOSEs, Art. 41) and **legal representative** in EU if not established there (Art. 13)
7. Supervision: **Digital Services Coordinator (DSC)** in each Member State. Commission directly supervises VLOPs/VLOSEs. Fines: up to **6% global annual turnover** (Art. 52). Periodic penalty payments: up to 5% average daily turnover
8. Interact with **other frameworks**: GDPR (data processing for content moderation), AVMSD (audiovisual content), CSAM regulation (proposal), Terrorist Content Regulation (2021/784)

### Verification
- Can classify an intermediary service into the correct DSA tier
- Can design a compliant notice-and-action mechanism
- Can outline VLOP systemic risk assessment requirements

## Cortex Logging
- collection: procedures
- tags: eu-commercial-law, training, dsa, digital-services, content-moderation, vlop, platform-regulation

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing eu-commercial-law skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: EUCL-030 (DMA), EUCL-037 (GDPR), EUCL-034 (P2B)

## Dependente
- EUCL-001

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
