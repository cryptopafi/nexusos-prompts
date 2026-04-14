---
id: HM-010
title: Hotel KPI Dashboard Setup and Monitoring
domain: hotel-management
source: Hotel Management 360 (Tourism & Hotel Academy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# Hotel KPI Dashboard Setup and Monitoring

## Obiectiv
Build a centralized, always-current KPI dashboard that gives management a real-time view of property performance across revenue, operations, guest satisfaction, and financial health — enabling data-driven decisions within 24 hours of any significant metric shift. Target: GM makes zero decisions based on gut-feel alone within 6 months of dashboard implementation.

## Pași

### Pas 1: Define the Core KPI Set — The 28-Metric Framework
- Agree on the KPIs before building anything. Resist the temptation to track everything — track what you will act on. For a hotel, the core set organized by decision area:

**Revenue KPIs (8 metrics — reviewed daily/weekly):**
1. **Occupancy %** = Rooms Sold / Rooms Available x 100. The most fundamental hotel metric. Benchmark for boutique rural Romania: 55–70% annual average, 85%+ peak season, 35–45% low season.
2. **ADR (Average Daily Rate)** = Total Room Revenue / Rooms Sold. Benchmark: 250–500 RON depending on positioning and season.
3. **RevPAR (Revenue per Available Room)** = Occupancy% x ADR. THE single most important hotel revenue metric. It combines pricing power and demand capture into one number. Benchmark: 140–350 RON.
4. **TRevPAR (Total Revenue per Available Room)** = Total Property Revenue (rooms + F&B + other) / Rooms Available. This captures ancillary revenue performance. For a property with a strong F&B concept, TRevPAR should be 30–60% higher than RevPAR.
5. **GOPPAR (Gross Operating Profit per Available Room)** = GOP / Rooms Available. The ultimate profitability metric — it tells you how much profit each room generates after all operating costs. Benchmark: 80–200 RON for independent hotels.
6. **Direct Booking %** = Direct Room Nights / Total Room Nights x 100. Target: grow 5 percentage points per year. Below 25% = dangerous OTA dependency.
7. **Channel Commission Cost %** = Total OTA Commissions / Total Room Revenue x 100. Target: below 12%. Above 15% = too much revenue going to intermediaries.
8. **Average Lead Time** = average days between booking date and arrival date. Tracks how far in advance guests book. Shorter lead times (below 14 days) mean less pricing optimization time.

**Operations KPIs (6 metrics — reviewed weekly):**
9. **Room Turnaround Time** = minutes from checkout notification to "Vacant Clean" status in PMS. Target: under 45 minutes for checkout rooms with same-day arrivals.
10. **Housekeeping Inspection Score** = average score across all inspected rooms (scale of 0–10). Target: 8.5+ team average.
11. **Maintenance Response Time** = hours from maintenance request submission to resolution confirmed. Target: under 4 hours for guest-impacting issues, under 24 hours for non-urgent.
12. **Check-in Wait Time** = average minutes from guest arrival at desk to key handover. Target: under 5 minutes for returning guests, under 8 minutes for new guests.
13. **Upsell Conversion Rate** = upsell offers accepted / upsell offers made x 100. Target: 20%+ for room upgrades, 30%+ for F&B (see HM-008).
14. **Energy/Water per Occupied Room** = monthly utility cost / occupied room nights. Track trend for eco-efficiency (HM-019).

**Guest Satisfaction KPIs (6 metrics — reviewed weekly):**
15. **Booking.com Review Score** = composite score displayed on listing. Target: 8.5+ (Booking.com considers 8.6+ as "Excellent"). A 0.1-point increase typically improves conversion rate by 3–5%.
16. **Google Review Rating** = average star rating on Google Business Profile. Target: 4.5+ (out of 5.0).
17. **Review Volume** = number of new reviews received per month across all platforms. More reviews = more social proof = higher conversion. Target: at least 1 review per 3 stays.
18. **Review Response Rate** = reviews responded to within 48 hours / total reviews x 100. Target: 100%.
19. **Complaint Rate** = Level 2+ complaints / total stays x 100. Target: below 3%. A complaint rate above 5% indicates a systemic service failure.
20. **NPS (Net Promoter Score)** = % Promoters (9–10 rating) minus % Detractors (0–6 rating) from post-stay survey. Target: 50+ (world-class hospitality).

**Financial KPIs (8 metrics — reviewed monthly):**
21. **GOP% (Gross Operating Profit Margin)** = GOP / Total Revenue x 100. Target: 30–45% for boutique independent hotels. Below 25% = cost structure problem.
22. **Payroll Cost %** = Total Payroll (including social contributions) / Total Revenue x 100. Target: 25–35%. Above 35% = overstaffing or underpricing.
23. **F&B Food Cost %** = Food COGS / Food Revenue x 100. Target: 28–32%. Above 35% = waste, over-portioning, or supplier pricing problem.
24. **F&B Beverage Cost %** = Beverage COGS / Beverage Revenue x 100. Target: 20–28%.
25. **Cost per Occupied Room (CPOR)** = Total Variable Operating Costs / Occupied Room Nights. Normalizes cost regardless of occupancy. Track trend monthly.
26. **EBITDA** = Earnings Before Interest, Taxes, Depreciation, Amortization. Monthly and YTD.
27. **Cash Flow** = monthly cash inflow minus cash outflow. Critical for seasonal businesses where revenue is concentrated in a few months but costs are year-round.
28. **Debtors Days** = average days to collect receivables (relevant for corporate accounts with invoice terms). Target: under 30 days.

### Pas 2: Set Targets and Alert Thresholds for Each KPI
- For every KPI, define three values:
  - **Baseline**: current actual (last 3-month rolling average). This is your starting point.
  - **Target**: where you want to be in 12 months. Set stretch targets that are ambitious but achievable — unrealistic targets demotivate.
  - **Alert threshold (Red Line)**: the level at which you trigger an immediate investigation. This is the "something is wrong" signal.
    - Occupancy drops below 40% for 2+ consecutive weeks outside of planned low season.
    - Review score drops below 8.0 on Booking.com.
    - F&B cost% exceeds 38%.
    - Payroll cost% exceeds 38%.
    - Cash flow negative for 2 consecutive months.
    - Complaint rate exceeds 5%.
- Document all targets in a **KPI Target Sheet** — the reference document for all dashboard color-coding. Agree targets with the ownership/management team before building the dashboard. A dashboard without agreed targets is decoration.

### Pas 3: Build the Dashboard Structure
- **Recommended tool**: Google Sheets (accessible from any device, shareable, free, formula-capable). For properties with dedicated BI needs, Looker Studio (free) or Tableau (paid) can create automated visual dashboards pulling from Google Sheets.
- **Dashboard layout — 5 tabs**:
  - **Tab 1 — Daily Snapshot** (updated by 08:00 every morning): today's occupancy, yesterday's ADR/RevPAR/total revenue, OTB for next 7 days with forecast comparison, any open Level 2+ complaints, today's VIP arrivals. This tab is the GM's morning coffee companion — it should answer "how are we doing right now?" in under 60 seconds.
  - **Tab 2 — Revenue Trends** (updated weekly): line charts for Occupancy%, ADR, RevPAR, TRevPAR over the last 13 weeks, with a comparison line for STLY (same period last year). Bar chart: actual vs. budget vs. forecast for the current month. Channel mix pie chart: direct vs. OTA breakdown.
  - **Tab 3 — Guest Satisfaction** (updated weekly): review scores by platform (current + 3-month trend), complaint log summary (count by category), review response rate, NPS trend if measured, top 3 positive and negative review themes.
  - **Tab 4 — Financial Performance** (updated monthly): P&L summary (actual vs. budget), GOP%, cost ratios (payroll, F&B, CPOR), EBITDA, cash flow position, debtors aging.
  - **Tab 5 — Operations** (updated weekly): room turnaround time average, housekeeping inspection score trend, maintenance backlog count, upsell performance (conversion rate + revenue), energy/water consumption trend.
- **RAG (Red/Amber/Green) indicators on every KPI**:
  - Green = at or above target.
  - Amber = within 10% below target (early warning — monitor closely, may require action).
  - Red = more than 10% below target or below the alert threshold (requires immediate investigation and corrective action).
- **Conditional formatting in Google Sheets**: use conditional formatting to automatically color cells based on the target thresholds. This makes the RAG system automatic — no manual color coding required.

### Pas 4: Data Collection and Automation
- **PMS daily export**: most PMS systems (Cloudbeds, Mews, Opera Cloud) support scheduled daily reports by email or CSV export. Set up automatic daily export of: occupancy, rooms sold, ADR, RevPAR, total revenue by department, channel production. Import into Tab 1 and Tab 2.
- **Review score monitoring**: Booking.com and Google Business Profile can be checked manually daily (2 minutes each). For automation, tools like ReviewPro, TrustYou, or GuestRevu aggregate reviews from all platforms into a single dashboard. Cost: 50–150 EUR/month. Worth it when managing 3+ review platforms.
- **F&B data**: POS daily report should include covers, revenue by meal period, average spend per cover. Import into Tab 2 (revenue) and Tab 4 (cost ratios). F&B cost calculation requires weekly inventory counts (see HM-003).
- **Payroll**: accounting software export (Saga, QuickBooks) or manual entry from payroll records. Update monthly.
- **If full automation is not possible**: designate a responsible person for each data update:
  - Night Auditor: updates Tab 1 (Daily Snapshot) by 07:00 as part of the night audit closing.
  - Revenue Manager/GM: updates Tab 2 (Revenue Trends) every Monday by 10:00.
  - Front Office Manager: updates Tab 3 (Guest Satisfaction) every Monday.
  - Accountant/Finance: updates Tab 4 (Financial) by the 5th of each month.
  - Operations Manager: updates Tab 5 (Operations) every Monday.
- Post the update responsibility assignments at each person's workstation. If the dashboard is not updated on time, the review meeting (Pas 5) will flag the gap immediately.

### Pas 5: Dashboard Review Cadence — The Rhythm of Management
- **Daily (5 minutes — GM only)**: review Tab 1 (Daily Snapshot) every morning with coffee. Look for red flags: unexpected occupancy drop, complaint logged overnight, OTB for next 7 days below forecast by more than 10%. Red flags trigger same-day investigation and action.
- **Weekly (30 minutes — GM + Revenue Manager + FOM)**: review Tabs 1, 2, 3, and 5. Agenda: (1) last week's revenue performance vs. forecast (5 min), (2) OTB pace for next 30 days — pricing decisions needed? (10 min), (3) review score and complaint review (5 min), (4) operations metrics review (5 min), (5) action items with owners and deadlines (5 min). This meeting should be the same meeting as the weekly forecast review (HM-004) and pricing review (HM-005) — one meeting, one dashboard, multiple decision areas.
- **Monthly (60 minutes — full management team)**: review all 5 tabs. Present to department heads. Each head explains their KPI variances and presents corrective actions for the coming month. Financial review: actual vs. budget, cost overruns explained, reforecast if needed. This is the property's strategic management meeting.
- **Quarterly (90 minutes — ownership review)**: present a quarterly business review to ownership/investors. Format: (1) revenue summary vs. budget and forecast, (2) guest satisfaction trend, (3) competitive position (review score vs. competitors), (4) financial performance including EBITDA and cash flow, (5) strategic initiatives status, (6) next quarter outlook and risks.
- **Annual (half-day — strategic planning)**: full-year performance review. Compare all KPIs vs. annual targets. Identify the top 3 achievements and top 3 improvement areas. Set targets for the coming year. Feed into the budget process (HM-013).

### Pas 6: Act on the Data — Standing Decision Rules
- A dashboard that is observed but not acted on is a wall decoration. Define standing decision rules that automatically trigger actions based on KPI readings:
  - "If OTB Occupancy for any date in the next 14 days drops below 40% → activate last-minute rate (BAR-15%) on that date."
  - "If Booking.com Review Score drops below 8.3 → trigger immediate department review; GM inspects 5 rooms personally; analyze last 10 negative reviews for pattern."
  - "If F&B Food Cost% exceeds 35% for 2 consecutive weeks → kitchen waste audit, inventory count, supplier price review."
  - "If Payroll Cost% exceeds 35% for the month → review scheduling for next month; reduce casual labor hours by 10%."
  - "If Direct Booking% drops below 20% → increase Google Hotel Ads budget by 20%; send promotional email to the guest database."
  - "If Upsell Conversion Rate drops below 15% → schedule upsell training refresher; review script and offer pricing."
  - "If Maintenance Response Time exceeds 24 hours → GM meets maintenance lead; review staffing and priority system."
  - "If Cash Flow is projected negative for next month → freeze non-essential purchases; defer CAPEX; review AR collection."
- **Print these decision rules** and post them in the GM's office alongside the dashboard login link. The rules convert data observation into automatic operational response.

### Pas 7: Benchmarking Against Competitors and Industry
- Your KPIs are only meaningful in context. Benchmark against:
  - **Your own history**: STLY (same period last year) is the most relevant comparison. Are you improving or declining?
  - **Competitive set**: if you subscribe to STR (Smith Travel Research), you receive monthly competitive set data showing your property's occupancy, ADR, and RevPAR indexed against your defined competitor group. The key metrics are RGI (Revenue Generation Index — your RevPAR / competitive set average RevPAR x 100), MPI (Market Penetration Index — your occupancy / set average occupancy x 100), and ARI (Average Rate Index — your ADR / set average ADR x 100). An index above 100 means you are outperforming; below 100 means underperforming.
  - **Industry benchmarks**: use published benchmarks from Romanian tourism authorities, Horeca associations, and international hospitality research (Cornell Hotel School publications, STR Global Benchmark reports).
  - **OTA marketplace data**: Booking.com Extranet Analytics provides market demand data, search volume trends, and conversion benchmarks for your property class and location — all free.
- Update competitive benchmarks quarterly. Track your index position over time — the goal is consistent RGI above 100 (outperforming the market).

### Pas 8: Dashboard Evolution and Maturity Model
- Do not try to build a perfect dashboard on day one. Follow a maturity path:
  - **Month 1–2 (Foundation)**: Tab 1 only — Daily Snapshot. Build the habit of daily review. Fix any data collection issues.
  - **Month 3–4 (Expansion)**: add Tab 2 (Revenue Trends) and Tab 3 (Guest Satisfaction). Start weekly review meetings.
  - **Month 5–6 (Integration)**: add Tab 4 (Financial) and Tab 5 (Operations). Start monthly full-team reviews.
  - **Month 7–12 (Optimization)**: add benchmarking, refine decision rules, automate data feeds, train department heads to present their own KPIs.
  - **Year 2+**: consider a dedicated BI tool (Looker Studio, ProfitSword, or OTA Insight) if the property grows beyond 20 rooms.
- **Common dashboard failures to avoid**:
  - Too many KPIs: 28 is already the maximum. If a KPI is not reviewed in 3 consecutive months, remove it. Dashboard bloat leads to dashboard abandonment.
  - Stale data: a dashboard with last week's data is already less useful. A dashboard with last month's data is useless for daily decisions. Enforce update deadlines.
  - No accountability: if nobody is responsible for a specific tab, it will not be updated. Assign names, not roles.
  - Analysis paralysis: the dashboard exists to drive action, not discussion. Each review meeting should produce 2–5 specific action items with deadlines. If a meeting produces only "interesting data" and no actions, the meeting format needs revision.

## Verificare
- [ ] KPI list agreed and documented with baseline, 12-month target, and alert threshold for every metric
- [ ] Dashboard built with all 5 tabs (start with Tab 1, expand per maturity model)
- [ ] RAG conditional formatting applied — green/amber/red automatic
- [ ] Data update responsibility assigned for each tab (specific person + specific deadline)
- [ ] Daily/weekly/monthly review cadence scheduled as standing calendar events
- [ ] Standing decision rules documented and posted (minimum 6 rules)
- [ ] Dashboard access shared with all relevant managers (view link, not edit)
- [ ] Competitive benchmarking data source identified (STR, Booking.com Analytics, or manual competitor tracking)
- [ ] Dashboard reviewed and refined quarterly — remove unused KPIs, add emerging needs

## Instrumente
- Google Sheets (primary dashboard — accessible, free, formula-capable, shareable)
- Google Looker Studio (optional — for visual charts and automated dashboards pulling from Sheets)
- PMS daily export (Cloudbeds, Mews, Opera Cloud — CSV or scheduled email report)
- Booking.com Extranet Analytics (free — review score, market demand, channel performance)
- Google Business Profile (review monitoring and rating)
- POS daily report (covers, revenue, average spend per cover)
- Accounting software (Saga, QuickBooks — monthly financial data export)
- STR Report (subscription, competitive set benchmarking — optional but recommended for properties with 15+ rooms)
- ReviewPro / TrustYou / GuestRevu (review aggregation tools — optional, 50–150 EUR/month)

## Note
- **A dashboard that is not reviewed is worse than no dashboard** — it creates a false sense of management rigor. The cadence (Pas 5) is more important than the data. Start with a simple daily review habit and build from there.
- **RevPAR is the single most important revenue metric.** If you track only one KPI, track RevPAR. If you track only three, track RevPAR, GOP%, and Booking.com Review Score. These three numbers tell you whether you are capturing revenue, converting it to profit, and maintaining the guest experience that sustains both.
- **Common mistake — dashboard as a retrospective tool only**: the dashboard should be 60% forward-looking (OTB, forecast, upcoming events, leading indicators) and 40% retrospective (last week's performance). A dashboard that only shows what happened is a history book, not a management tool.
- **Albastru / Sapte Focuri context**: start with Tab 1 (Daily Snapshot) only. Once the habit of daily review is established (4–6 weeks), add the remaining tabs one at a time. For a small property without a dedicated Revenue Manager, the GM reviews the dashboard personally — this is not delegation work. The daily 5-minute review habit will generate more management value than any single operational improvement. For a property with 6–10 rooms, track TRevPAR alongside RevPAR because the F&B contribution (Sapte Focuri kitchen) is a significant portion of total revenue — RevPAR alone underrepresents the property's earning power. For a property with BookRetreats.com as a channel, track retreat segment KPIs separately (bookings, ADR, LOS, ancillary spend) as this high-value segment deserves dedicated monitoring.
