---
id: AGO-009
title: White-Label SaaS Setup (GoHighLevel)
domain: agency-operations
source: GoHighLevel Mastery - Automate & Scale Your Agency Fast (Udemy)
version: 2.0
forge_score: "estimated H/3.0"
business_mapping: ["ai-agency"]
---

# White-Label SaaS Setup (GoHighLevel)

## Obiectiv
Configurează GoHighLevel (GHL) ca platformă white-label a agenției, construiește snapshot-uri per nișă, lansează modelul de SaaS reselling care adaugă $5K-$30K/mo MRR pur cu marjă 70-85%, și creează maximum sticky factor prin integrarea GHL în operațiunile zilnice ale clientului — transformând un tool de $297/mo în revenue stream de $10K-$50K/mo.

## Pași

### Pas 1: Setup cont Agency Pro cu white-label complet
- **Alege planul corect**:
  - Agency Starter ($97/mo): basic features, fără white-label, fără SaaS mode. Folosit doar pentru teste.
  - Agency Unlimited ($297/mo): unlimited sub-accounts, white-label partial (domain + logo), fără SaaS billing. Minim pentru agenții care white-label.
  - Agency Pro ($497/mo): everything in Unlimited + SaaS Mode (auto-billing clienți) + AI capabilities. **RECOMANDAT** — ROI pozitiv de la 3 clienți SaaS.
- **White-label configuration** (Agency Settings → White Label):
  - Upload logo agenție (transparent PNG, min 500x500px)
  - Set brand colors (primary, secondary, accent — consistent cu brand book-ul agenției)
  - Custom domain: `app.youragency.com` sau `crm.youragency.com`
    - DNS: CNAME record `app` → `app.msgsndr.com` (GHL provide instrucțiuni per registrar)
    - SSL se generează automat după DNS propagation (2-24h)
  - Custom favicon
  - Custom loading screen
  - Custom email domain: `notifications@youragency.com` (SPF + DKIM setup)
  - Custom support email: `support@youragency.com` (sau helpdesk integration)
- **SaaS Mode activation** (Agency Pro only):
  - Agency Settings → SaaS Configurator
  - Connect Stripe account (live mode, not test)
  - Configure rebilling markups per feature (SMS, email, AI, phone)
  - Set default plan pricing (see Pas 5)
- **Test EVERYTHING**: login pe custom domain, verify branding, send test email, test SMS, verify no "GoHighLevel" branding visible anywhere clientul poate vedea. Un singur "Powered by GoHighLevel" vizibil = white-label compromis.

### Pas 2: Creează structura de Sub-Accounts optimizată
- **Per client = 1 Sub-Account**. Nu partaja sub-accounts între clienți.
- **Sub-Account setup checklist**:
  1. Agency Dashboard → Sub-Accounts → "+" New Sub-Account
  2. Business info: Legal name, address, timezone (critic pentru automation timing), business type
  3. Phone numbers: purchase Twilio number via GHL ($1.15/mo per number). Register A2P 10DLC (US/Canada — $15 registration, $0.003/SMS vs $0.02 without). EU: verify local SMS regulations.
  4. Email sending: connect Mailgun or GHL built-in ($0.675/1000 emails). Configure DKIM/SPF for client domain. Set sending limits (start at 100/day, ramp up).
  5. Integrations: Google My Business, Facebook Page, Instagram, Google Ads, Meta Ads (pixel), Stripe (for client's payments if applicable)
  6. User setup: add client as User with "Account" role (limited — cannot access Agency settings, other sub-accounts, or billing). Never give client "Agency Admin".
  7. Custom values: populate business-specific merge fields (business name, phone, address, website, operating hours)
- **Sub-account naming convention**: `[Client Name] — [Industry] — [Start Date]` (ex: "Smith Dental — Dental — 2026-03")
- **Security**: enable 2FA for all users, set IP restrictions if client is enterprise

### Pas 3: Construiește Snapshots reutilizabile per nișă (cel mai valoros asset)
Un Snapshot = configurare completă (pipelines, workflows, templates, funnels, forms) exportabilă și importabilă instant pe orice sub-account nou. Cu un snapshot bun, onboarding-ul scade de la 8-10 ore la 1-2 ore.

- **Snapshot #1 — Dental/Medical** (highest demand):
  - Pipeline: New Lead → Contacted → Appointment Booked → Showed Up → Treatment Accepted → Closed
  - Workflows:
    - New lead auto-response: SMS in 5 min + email in 15 min ("Hi {first_name}, thanks for your interest! Would you like to schedule a consultation?")
    - Appointment reminder: 48h email + 24h SMS + 2h SMS
    - No-show follow-up: 30 min after missed appointment SMS + 24h email
    - Review request: 24h after appointment SMS ("How was your visit? If you had a great experience, we'd love a review: {google_review_link}")
    - Reactivation: 90-day no-visit trigger → SMS + email ("We miss you! Book your next appointment: {booking_link}")
  - Funnel: 2-page (landing + thank you) for lead capture (free consultation offer)
  - Email templates: 5 (welcome, appointment confirmation, reminder, review request, reactivation)
  - Forms: new patient intake form, consultation request form
  - Calendar: appointment booking with buffer times

- **Snapshot #2 — Home Services** (HVAC, plumbing, roofing, cleaning):
  - Pipeline: Estimate Request → Quote Sent → Follow-up → Job Booked → In Progress → Completed → Review
  - Workflows: instant lead response, quote follow-up (24h/72h/7d), job completion + review request, seasonal maintenance reminder
  - Funnel: landing page for seasonal offer (AC tune-up, roof inspection)
  - SMS templates: quote sent, job scheduled, tech en route, job complete

- **Snapshot #3 — Real Estate**:
  - Pipeline: New Lead → Qualified → Property Viewing → Offer Made → Negotiation → Closed
  - Workflows: new listing alert, open house follow-up, drip campaign for long-term buyers
  - Funnel: home valuation page, neighborhood guide download

- **Building a snapshot**:
  1. Create a test sub-account for the niche
  2. Build everything inside (pipeline, workflows, funnels, templates)
  3. TEST every automation thoroughly (send test leads through entire flow)
  4. Agency → Snapshots → Create Snapshot from sub-account
  5. Verify: apply snapshot to a new empty sub-account → everything imports correctly
  6. Document: what the snapshot includes, how to customize per client, known limitations

- **Snapshot economics**: 10-15 hours to build first snapshot. Saves 5-8 hours per client onboarding. At 10 clients in same niche = 50-80 hours saved = $3,500-$5,600 in labor costs.

### Pas 4: Configurează 5 automations obligatorii per client (non-negotiable)
Aceste 5 automations sunt "must-have" care demonstrează valoare imediată:

1. **Speed-to-Lead (Instant Response)**:
   - Trigger: new contact/lead from any source (form, FB ad, GMB, manual)
   - Action: SMS + email in <5 minutes
   - Why: businesses that respond in <5 min are 100x more likely to convert than those responding in 30 min
   - Template SMS: "Hi {first_name}, this is {business_name}. Thanks for reaching out! How can we help you today?"

2. **Missed Call Text-Back**:
   - Trigger: incoming call → no answer after X rings
   - Action: immediate SMS to caller
   - Template: "Sorry we missed your call! How can we help? Reply to this text or book a time here: {booking_link}"
   - **This is THE killer feature for first-day quick win** — every missed call now gets a follow-up

3. **Appointment Reminder Sequence**:
   - 48h before: email confirmation with details
   - 24h before: SMS reminder with confirm/reschedule option
   - 2h before: final SMS reminder
   - Reduces no-shows by 30-50%

4. **Review Generation Machine**:
   - Trigger: 24h after appointment/service (status changed to "Completed")
   - Action: SMS with Google Review link
   - Follow-up: if no review in 7 days, email reminder
   - Typical result: 5-15 new Google reviews per month per client

5. **Lead Nurture / Long-Term Follow-Up**:
   - Trigger: lead enters pipeline but doesn't book within 7 days
   - Sequence: day 7 SMS → day 14 email → day 30 SMS → day 60 email → day 90 SMS (final)
   - Content: value-based, not salesy. Tips, testimonials, limited-time offers.
   - Reactivates 10-20% of cold leads over 90 days

### Pas 5: Lansează modelul de SaaS Reselling — unit economics detaliată
- **Costul tău** (Agency Pro plan):
  - Platform: $497/mo fixed (unlimited sub-accounts)
  - Per sub-account variable: ~$1.15/mo (phone number) + SMS costs ($0.0079/segment) + email ($0.675/1000) + AI costs (if enabled)
  - Estimated variable cost per average client: $15-$50/mo depending on usage
  - **Total cost per client**: $15-$50/mo (marginal cost approaches zero la scale)
- **Pricing tiers pentru clienții SaaS**:
  | Tier | Monthly Price | Included | Your Margin | Best For |
  |------|-------------|----------|-------------|---------|
  | Essential | $197/mo | CRM + pipeline + calendar + 500 contacts + missed call text-back + 200 SMS | $147-$182 (75-92%) | Solo businesses, tradespeople |
  | Professional | $397/mo | Everything Essential + automations + review management + email marketing + 2,000 contacts + 500 SMS + 1 funnel | $347-$382 (87-96%) | Small businesses 2-10 employees |
  | Premium | $697/mo | Everything Pro + AI chatbot + multiple funnels + unlimited contacts + 1,000 SMS + advanced reporting | $647-$682 (93-98%) | Growing businesses, multi-location |
  | Enterprise | $997-$1,497/mo | Full platform + custom integrations + dedicated onboarding + priority support | $897-$1,447 (90-97%) | Established businesses, franchises |

- **Revenue model at scale**:
  | # SaaS Clients | Avg Revenue/Client | Monthly SaaS MRR | Agency Platform Cost | Net SaaS Profit | Margin |
  |----------------|-------------------|-----------------|--------------------|-----------------| -------|
  | 5 | $397 | $1,985 | $497 + ~$150 variable | $1,338 | 67% |
  | 15 | $397 | $5,955 | $497 + ~$450 | $5,008 | 84% |
  | 30 | $397 | $11,910 | $497 + ~$900 | $10,513 | 88% |
  | 50 | $397 | $19,850 | $497 + ~$1,500 | $17,853 | 90% |
  | 100 | $397 | $39,700 | $497 + ~$3,000 | $36,203 | 91% |

- **Cum prezinți clienților** (nu menționezi GHL niciodată):
  "Our proprietary [Agency Name] Business Platform includes everything you need to manage leads, automate follow-ups, book appointments, collect reviews, and send marketing campaigns — all from one dashboard, branded with your logo."

### Pas 6: Configurează SaaS billing automat și onboarding
- **Stripe setup în GHL SaaS Mode**:
  - Connect Stripe live account (not test mode)
  - Configure plans matching your tiers (Essential, Professional, Premium)
  - Set trial period: 14 days free trial (reduces friction, increases conversion 30-40%)
  - Enable auto-billing: charges on signup + recurring monthly
  - Set dunning (failed payment retry): attempt 1 on failure, attempt 2 at day 3, attempt 3 at day 7, cancel at day 14
- **SaaS signup flow**:
  1. Prospect visits SaaS landing page (your branded site)
  2. Selects plan, enters payment info (Stripe checkout)
  3. GHL auto-creates sub-account, applies snapshot, sends welcome email
  4. Client receives login credentials + booking link for onboarding call
- **Rebilling configuration** (markup on usage):
  - SMS: your cost $0.0079/segment → rebill at $0.015-$0.025 (100-200% markup)
  - Email: your cost $0.675/1000 → rebill at $1.50-$3.00/1000 (120-350% markup)
  - AI (Conversation AI): your cost ~$0.03-$0.10/message → rebill at $0.10-$0.20
  - Phone: your cost $0.013/min → rebill at $0.025-$0.05/min
  - These usage markups add $50-$200/mo additional revenue per active client

### Pas 7: Client onboarding și training pentru maximum adoption
- **Day 1 — Automated welcome**:
  - Email: login credentials, quick start guide (PDF 1-page), booking link for training call
  - SMS: "Welcome to [Platform Name]! Your account is ready. Check your email for login details."
- **Day 2-3 — Training call (45-60 min via Zoom)**:
  - Agenda: dashboard overview, how to view pipeline, how to respond to leads (from app!), calendar setup, missed call text-back demo (live — call their business, don't answer, show SMS appears)
  - Record and send Loom link for reference
  - Critical: get them to DOWNLOAD THE MOBILE APP. Adoption rate for clients using mobile app: 85%. Without app: 40%.
- **Day 7 — Check-in call (15 min)**:
  - "How's it going? Any questions? Have you responded to any leads through the platform?"
  - Address confusion, fix any setup issues
  - If they haven't used it: walk through one real scenario together
- **Day 14 — Trial ending / conversion**:
  - If on trial: "Your trial ends in 2 days. Here's what you've achieved: [X leads captured, Y appointments booked, Z reviews generated]. Ready to continue?"
  - Conversion rate target from trial: 40-60%
- **Ongoing support**:
  - Knowledge base (Notion or GHL's built-in): 10-15 how-to articles + video tutorials
  - Monthly "office hours" group call (all SaaS clients together) — 30 min Q&A, saves individual support time
  - Email support SLA: respond within 24h business hours
  - Urgent issues: dedicated Slack channel or WhatsApp group

### Pas 8: Reduce SaaS churn și maximize LTV
- **SaaS churn benchmarks**: industry average 5-8% monthly. Target: <3% monthly. At 3% monthly churn, average client lifespan = 33 months. At 8% = 12.5 months.
- **Churn prevention tactics**:
  - **Sticky factor**: the more the client uses the platform daily (responds to leads, checks pipeline, sends SMS), the harder it is to leave. Mobile app adoption is THE #1 predictor of retention.
  - **Usage monitoring**: track weekly active users. If a client hasn't logged in for 7 days → automated re-engagement email. 14 days → account manager call. 30 days → high churn risk, personal outreach.
  - **Value reporting**: monthly automated email: "This month, [Business Name] captured [X] new leads, booked [Y] appointments, and received [Z] new reviews through your platform." Quantified value = retention.
  - **Feature drip**: don't enable all features at once. Month 1: CRM + calendar + missed call. Month 2: add review generation. Month 3: add email marketing. Each new feature is a "surprise upgrade" that re-engages.
  - **Annual commitment discount**: offer 2 months free for annual payment ($397/mo × 10 = $3,970/year vs $4,764). Locks in revenue + reduces monthly churn to near zero.
- **Win-back**: client cancels → wait 30 days → send "We miss you" email with special offer (1 month free to return). 10-15% win-back rate typical.
- **LTV calculation**: Average LTV = ARPU × Average Lifespan. At $397/mo × 33 months = $13,101 LTV. At $200 CAC (referral-based) = 65x LTV/CAC ratio.

### Pas 9: Scale SaaS beyond your agency clients
- **Horizontal expansion**: once you have a proven snapshot for 1 niche → replicate for adjacent niches. Dental snapshot → Medical → Chiropractic → Veterinary (similar workflows, different branding)
- **Partner channel**: recruit other agencies, consultants, business coaches to white-label YOUR platform. They sell to their clients → you handle the platform → revenue share 60/40 or 70/30 (you 60-70%).
- **Content marketing for SaaS**: YouTube tutorials, LinkedIn posts, Facebook groups for your niche. "How [Niche] businesses use CRM to get more patients/clients." Attract SaaS clients who don't need full agency services.
- **Milestone targets**:
  - 10 SaaS clients ($4K/mo MRR) = covers agency platform costs + generates $3K+ profit
  - 25 SaaS clients ($10K/mo MRR) = funds 1 FTE support person
  - 50 SaaS clients ($20K/mo MRR) = SaaS becomes standalone business unit
  - 100 SaaS clients ($40K/mo MRR) = SaaS revenue > agency services revenue for many agencies

## Verificare
- [ ] GHL Agency Pro account created and white-label fully configured
- [ ] Custom domain live with SSL, no GHL branding visible to clients
- [ ] First sub-account created and configured with all integrations
- [ ] Minimum 1 snapshot built, tested on clean sub-account, and documented
- [ ] 5 core automations active per client (speed-to-lead, missed call, reminders, reviews, nurture)
- [ ] SaaS pricing tiers defined with margin calculations
- [ ] Stripe connected in SaaS Mode with plans, trials, and dunning configured
- [ ] SaaS signup flow tested end-to-end (signup → sub-account → snapshot → welcome)
- [ ] Client training materials created (quick start guide, video tutorials, knowledge base)
- [ ] Usage monitoring + re-engagement automations active
- [ ] SaaS churn tracked monthly (target <3%)

## Instrumente
- GoHighLevel Agency Pro ($497/mo) — platform principal
- Stripe — SaaS billing, auto-charge, dunning
- Twilio (integrat în GHL) — SMS, voice, phone numbers
- Mailgun (integrat în GHL) — email sending la scale
- Loom ($15/mo) — training videos
- Notion — knowledge base pentru clienți SaaS
- Zapier / Make — integrări cu tooluri externe (dacă nativele GHL nu sunt suficiente)
- Google Sheets — SaaS MRR tracker, churn dashboard, LTV calculator

## Note
- **Cel mai mare avantaj al GHL SaaS**: sticky factor. Clientul care își gestionează leads, appointments și reviews din platformă zilnic NU va pleca pentru $50/mo mai ieftin. Costul de migrare e prea mare.
- Nu tenta să migrezi toți clienții existenți pe GHL în prima lună. Începe cu clienți noi, validează procesul, apoi migrează clienți vechi gradual.
- **Snapshot-urile sunt cel mai valoros asset al tău în GHL** — cu cât ai mai multe snapshots per nișă, cu atât onboarding-ul e mai rapid și mai profitabil. Vinde snapshot-urile separat pe marketplace GHL ($500-$2,000 fiecare) pentru revenue adițional.
- GHL are curbă de învățare de 3-4 săptămâni pentru echipă — investește ÎNAINTE de a promite clienților funcționalități avansate. Common mistake: sell first, learn later → client churn.
- **Death trap — Over-customization**: dacă fiecare client cere customizare extensivă, margins collapse. Standardizează cu snapshots, allow minor customization, charge for major changes.
- **The SaaS tipping point**: most agencies hit it at 25-30 SaaS clients. At that point, SaaS MRR covers agency overhead, making the services business pure profit. This is how you build a $2M+ agency.
