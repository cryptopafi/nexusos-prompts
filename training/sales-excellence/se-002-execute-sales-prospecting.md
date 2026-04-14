---
type: procedure
created: 2026-03-20
status: active
slug: se-002-execute-sales-prospecting
tags: [procedure, nexus]
---

# SE-002 — Execute Sales Prospecting

## Problema
Reps spend 65% of their time on non-selling activities (Salesforce State of Sales) and lack a systematic prospecting cadence, resulting in feast-or-famine pipeline. Without a structured prospecting engine, pipeline dries up within 30-60 days.

## Procedura
### Prerequisites
- Ideal Customer Profile (ICP) defined (see SE-020 for B2C, SE-014 for B2B)
- Access to prospecting tools: LinkedIn Sales Navigator, Apollo.io, or ZoomInfo
- CRM configured with lead stages
- Email and phone system ready

### Steps
1. **Define your prospecting math** — Work backwards from quota. If quota = $500K/quarter, average deal = $50K, win rate = 25%, you need 40 qualified opportunities. If prospect-to-opportunity conversion = 10%, you need 400 prospects/quarter = ~7/day. Write your personal prospecting equation and post it visibly.
2. **Build a tiered prospect list** — Create three tiers: Tier 1 (ideal fit, high value — max 50 accounts, fully researched), Tier 2 (good fit — 200 accounts, partially researched), Tier 3 (broad fit — volume play, minimal research). Use Apollo.io or LinkedIn Sales Navigator filters: industry, company size, tech stack, funding stage, hiring signals.
3. **Research Tier 1 accounts deeply** — For each Tier 1 account, spend 15 minutes gathering: recent news/earnings, key decision-makers (titles, LinkedIn activity), tech stack (BuiltWith, Wappalyzer), competitors they're losing to, and a personalized hook. Document in CRM contact notes.
4. **Design a multi-channel cadence** — Build a 21-day, 12-touch sequence: Day 1: personalized email. Day 3: LinkedIn connection + note. Day 5: phone call. Day 7: email #2 (value-add content). Day 10: LinkedIn engage (comment on their post). Day 12: phone + voicemail. Day 14: email #3 (case study). Day 17: LinkedIn InMail. Day 19: phone. Day 21: breakup email. Use Salesloft, Outreach, or Apollo sequences to automate tracking.
5. **Write prospecting messages using the PVC framework** — Personalization (reference their specific situation), Value (state the outcome you deliver), Call-to-action (one clear ask). Example: "Hi [Name], saw [Company] just expanded to EMEA [P]. We helped [Similar Company] reduce onboarding time by 40% when they scaled internationally [V]. Worth a 15-minute call Thursday? [C]." Write 5 email templates and 3 cold call scripts.
6. **Execute daily prospecting blocks** — Block 90 minutes every morning (8:30-10:00 AM) for prospecting only. No email, no Slack. Structure: first 20 min = research new prospects, next 50 min = calls (aim for 25-30 dials), final 20 min = personalized emails. Track: dials, connections, conversations, meetings booked.
7. **Handle the gatekeeper and voicemail** — Gatekeeper script: "Hi, this is [Name] from [Company]. I'm working with [Peer Company] on [specific outcome]. Could you connect me with [Title]?" Voicemail (under 30 seconds): state name, one compelling reason, phone number twice. Never say "just following up" or "checking in."
8. **Qualify during first conversation** — Use the 3x3 qualification: 3 questions about their situation (pain, current solution, timeline) and 3 signals you listen for (urgency, authority, budget). If 2+ signals positive, book next meeting. If 0-1, nurture or disqualify. Log qualification notes in CRM immediately after call.
9. **Track daily metrics in a prospecting dashboard** — Build a simple tracker: Dials | Connections | Conversations | Meetings Booked | Emails Sent | Replies. Calculate ratios weekly: dial-to-connect (benchmark: 15-20%), connect-to-meeting (benchmark: 20-30%), email reply rate (benchmark: 5-15% cold, 15-30% warm).
10. **Review and optimize weekly** — Every Friday, 30-minute review: which messages got replies, which call times yielded most connections, which Tier converted best. A/B test subject lines (aim for 35%+ open rate). Adjust cadence timing, messaging, and prospect criteria based on data. Share top-performing templates with team.

### Verification
- Prospecting math documented with personal equation
- Tiered prospect list built (50 T1, 200 T2, T3 volume)
- Multi-channel cadence designed and loaded into sequencing tool
- Daily prospecting block scheduled and protected on calendar
- Dashboard tracking minimum 5 daily metrics

## Cortex Logging
- collection: procedures
- tags: sales-excellence, training, prospecting, cold-outreach, cadence, pipeline-generation
- version: 2.0

## Enforcement Loop
- WHERE: Training and skill development — daily sales execution
- WHEN: Daily execution; full cadence review weekly
- HOW: Execute daily block, track metrics, optimize weekly
- CONNECT: SE-013 (Prospecting Plan), SE-014 (Qualify Cold Leads), SE-031 (Lead Scoring)

## Dependente
- ICP defined (SE-020 or SE-014)
- Prospecting tools (LinkedIn Sales Navigator, Apollo, Outreach)
- CRM with sequence tracking

## Metrics
- Completion: all 10 steps executed, daily cadence running
- Quality: meeting booking rate >3/week from cold prospecting
- Benchmark: 25+ dials/day, 5-15% email reply rate, 20%+ connect-to-meeting
- Level: ADVANCED
