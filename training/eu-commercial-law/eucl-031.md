---
type: procedure
created: 2026-03-17
status: active
slug: eucl-031
tags: [procedure, nexus]
---

# EUCL-031 — EU AI Act (Regulation 2024/1689)

## Problema
Developers and deployers of AI systems face a complex risk-based regulatory framework and fail to correctly classify their systems, risking non-compliance penalties of up to EUR 35 million or 7% turnover.

## Procedura
### Prerequisites
- EUCL-001 (legal system), EUCL-037 (GDPR — AI often processes personal data)

### Steps
1. Determine if the system is an **AI system** (Art. 3(1)): machine-based system designed to operate with varying levels of autonomy, that may exhibit adaptiveness, and that infers from input how to generate outputs (predictions, content, recommendations, decisions) that can influence environments
2. Apply **risk classification**:
   - **Prohibited practices** (Art. 5): subliminal manipulation, exploitation of vulnerabilities, social scoring by public authorities, real-time remote biometric identification in public spaces by law enforcement (limited exceptions), emotion recognition in workplace/education, untargeted scraping for facial recognition databases
   - **High-risk** (Art. 6 + Annex III): biometric identification, critical infrastructure, education/vocational training, employment/worker management, essential services access (credit, insurance), law enforcement, migration/border control, justice/democratic processes. Also: AI used as safety component of product under EU harmonized legislation (Annex I)
   - **Limited risk** (Art. 50): transparency obligations — chatbots must disclose AI interaction, deepfakes must be labelled, emotion recognition/biometric categorization must inform subjects
   - **Minimal risk**: voluntary codes of conduct
3. For **high-risk AI** — comply with Chapter III requirements: risk management system (Art. 9), data governance (Art. 10), technical documentation (Art. 11), record-keeping/logging (Art. 12), transparency and information to deployers (Art. 13), human oversight (Art. 14), accuracy/robustness/cybersecurity (Art. 15)
4. **Conformity assessment** (Art. 43): self-assessment for most high-risk AI (internal control, Annex VI). Third-party assessment (notified body) required for biometric identification and critical infrastructure AI
5. **Provider obligations** (Art. 16): quality management system, CE marking, EU declaration of conformity, post-market monitoring, serious incident reporting to authorities within 15 days
6. **Deployer obligations** (Art. 26): use in accordance with instructions, ensure human oversight, monitor operation, inform individuals when subject to high-risk AI decision, DPIA under GDPR where applicable
7. **General-purpose AI models (GPAI)** (Art. 51-56): transparency obligations (technical documentation, copyright compliance, content summary). Systemic risk GPAI (>10^25 FLOPs or Commission designation): model evaluation, adversarial testing, serious incident reporting, cybersecurity
8. **Governance and enforcement**: AI Office (Commission), national competent authorities, AI Board. Fines: prohibited practices EUR 35M or 7% turnover; high-risk non-compliance EUR 15M or 3%; incorrect information EUR 7.5M or 1%

### Verification
- Can classify an AI system into the correct risk category
- Can list the compliance requirements for a high-risk AI system
- Can identify prohibited AI practices

## Cortex Logging
- collection: procedures
- tags: eu-commercial-law, training, ai-act, risk-classification, high-risk-ai, conformity-assessment, gpai

## Enforcement Loop
- WHERE: Training and skill development
- WHEN: When practicing eu-commercial-law skills
- HOW: Follow steps sequentially, verify at end
- CONNECT: EUCL-037 (GDPR), EUCL-017 (product liability), EUCL-015 (CE marking)

## Dependente
- EUCL-001

## Metrics
- Completion: all steps executed
- Quality: verification checks pass
