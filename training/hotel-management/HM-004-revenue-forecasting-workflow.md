---
id: HM-004
title: Hotel Revenue Forecasting Workflow
domain: hotel-management
source: Hotel Management 360 (Tourism & Hotel Academy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# Hotel Revenue Forecasting Workflow

## Obiectiv
Produce a reliable 30/60/90-day revenue forecast that enables proactive pricing decisions, staffing adjustments, and cost controls — replacing reactive gut-feel management. The forecast should achieve a variance of less than 10% vs. actuals on a monthly basis within 6 months of implementation, and less than 5% after 12 months of calibration.

## Pași

### Pas 1: Collect and Structure Historical Data (done once, updated monthly)
- Pull the last 24 months of daily data from PMS. If the property is new or has less than 24 months of history, use 12 months minimum — supplement gaps with local market data (Booking.com Demand Calendar, STR report, or tourism board statistics).
- Export to a structured spreadsheet with these exact columns: Date | Day of Week | Season Type (Peak/High/Shoulder/Low) | Occupancy% | Rooms Sold | Rooms Available | ADR (RON) | RevPAR (RON) | Room Revenue | F&B Revenue | Other Revenue | Total Revenue | Channel Mix (Direct%/OTA%/Corporate%/Group%) | Notes (events, holidays, closures, weather events, competitor openings).
- Tag each date with:
  - **Season type**: classified per HM-020 seasonal calendar.
  - **Day-of-week pattern**: weekday (Mon–Thu) vs. weekend (Fri–Sun). For rural/leisure properties, weekend occupancy is typically 20–40 percentage points higher than weekday.
  - **Local events**: festivals within 30km, school holidays (national and regional), national public holidays, religious holidays (Easter, Christmas — Orthodox calendar dates for Romania), major sports events, conferences.
  - **Competitor events**: new property openings or closures within your competitive set, major renovations that took competitor inventory offline.
  - **Weather anomalies**: extreme heat, flooding, or storms that materially impacted demand.
- Calculate baseline indices:
  - Monthly average occupancy, ADR, RevPAR for each of the last 24 months.
  - Day-of-week occupancy profile: average Friday occupancy vs. average Tuesday occupancy across all months.
  - Seasonal averages: peak/high/shoulder/low occupancy and ADR aggregated.
  - Booking window distribution: what % of bookings were made 0–7 days, 8–30 days, 31–90 days, and 90+ days before arrival? This tells you when demand materializes for each season.

### Pas 2: Build the Forecast Model
- Use a **rolling 12-month same-period-last-year (STLY)** as the primary baseline. For each forecast date, start with last year's actual occupancy and ADR on the same day-of-week in the same week (not the same calendar date — a Friday's best comparison is last year's corresponding Friday, not last year's June 15 if that was a Tuesday).
- Apply four adjustment factors to the STLY baseline:
  - **Market trend factor** (+/- %): is the overall market growing or declining? Sources: Booking.com Insights (free in extranet — shows market demand trends), STR reports (subscription, provides competitive set benchmarking), local/national tourism statistics (INSSE for Romania, WTTC for global trends). If the regional market grew 8% last year, apply +8% unless there is a specific reason your property will deviate from the market trend.
  - **Booking pace factor** (+/- %): compare current on-the-books (OTB) reservations for each future date against the same OTB count at the same point in time last year. Example: for June 15, if on May 1 last year you had 5 rooms booked, and on May 1 this year you have 7 rooms booked, your pace is +40%. Apply a dampened uplift: if pace is +40%, apply +20–25% uplift (pace advantage does not fully convert to occupancy because late bookers may compensate in the trailing year). Rule of thumb: apply 50–60% of the observed pace difference as the adjustment factor.
  - **Event factor** (+/- %): confirmed demand generators add 10–40% depending on proximity and scale. Local village festival within 5km: +15–20%. Major national event (Untold Festival, Neversea) within 50km: +25–40%. New Year's Eve: typically +50–80% above a normal winter Saturday. For demand destructors (major road closure, competitor promotional campaign), apply -5 to -15%.
  - **External risk factor** (+/- %): economic slowdowns (consumer confidence index dropping), negative press or a recent bad review trend (Booking.com score dropped 0.3+ in the last quarter), competitive new openings within 10km, inflation impact on disposable travel budget. Conservative dampener: -3% to -15%.
- **Formula per day**: Forecast Occupancy = STLY Occupancy x Market Factor x Pace Factor x Event Factor x Risk Factor. Cap at 100% (cannot exceed full house). Floor at 0%.
- **ADR forecast**: similar structure. STLY ADR adjusted for: inflation (+3–5% per year in Romania), rate strategy changes (if you raised BAR by 10%, apply that), demand-driven premium (if occupancy forecast exceeds 80%, ADR should reflect the higher pricing tier from HM-005).
- **RevPAR forecast** = Forecast Occupancy x Forecast ADR. This is the single most important output number.

### Pas 3: Segmentation Breakdown
- Split the total forecast into segments. Each segment has different booking behavior, ADR, cost-to-acquire, and ancillary revenue:
  - **Direct (website + phone + email)**: typically 15–30% of bookings for independent properties. Highest net ADR (no commission, only card processing fee of 1.5–2.5%). Booking window: 2–8 weeks. Encourage and grow this segment relentlessly.
  - **OTA (Booking.com, Airbnb, Expedia)**: typically 50–70% of bookings for independent properties. Commission: 15–18% (Booking.com), 3% host fee + guest fee (Airbnb), 15–25% (Expedia). Net ADR = Gross ADR minus commission. Booking window: 2–6 weeks for leisure.
  - **Corporate Individual**: business travelers at negotiated rates. Typically BAR-10 to BAR-20%. Booking window: 1–2 weeks. Midweek dominant. Low F&B ancillary (often expense-budgeted).
  - **Corporate Group/MICE**: meetings, events, retreats. 10+ rooms. Negotiated package rate (room + F&B + AV). Booking window: 2–6 months. Highest total revenue per booking when F&B is included.
  - **Walk-in**: 5–10% in urban, 2–5% in rural. Full BAR or above. No acquisition cost. Unpredictable — do not forecast more than last year's average.
  - **Tour Operator / Wholesale**: bulk rates 20–30% below BAR. Guaranteed volume but lowest net ADR. Only pursue if occupancy is consistently below 50% in specific periods.
- For each segment: Segment Revenue = Forecast Segment Rooms x Segment ADR. Segment Net Revenue = Segment Revenue minus Commission/Discount costs.
- **Total Net Revenue Forecast** = sum of all Segment Net Revenues + F&B Revenue Forecast + Other Revenue Forecast.
- F&B Revenue Forecast: estimate based on Forecast Occupied Rooms x F&B capture rate x average F&B spend per captured room. For a rural property with on-site restaurant: dinner capture rate 50–70%, average dinner spend per room 150–250 RON. Breakfast capture rate near 100% if included in rate.

### Pas 4: Build the Rolling Forecast Calendar
- Create a 90-day rolling forecast calendar in Google Sheets:
  - Row per day, columns: Date | Day | Season | Forecast Occ% | Forecast ADR | Forecast RevPAR | OTB Rooms | Remaining to Sell | Active Rate Tier (from HM-005) | Notes.
  - Color-code by confidence level: green = high confidence (>70% of forecast rooms already OTB), yellow = moderate (40–70% OTB), red = low confidence (<40% OTB, heavy reliance on forecast assumptions).
- Update the calendar every Monday morning with the latest OTB data. This is the basis for the weekly revenue review meeting.
- For dates in the next 7 days: the forecast should be near-final (most bookings already OTB). Focus on last-minute rate optimization.
- For dates 8–30 days out: the forecast is actionable — this is where pricing and marketing decisions have the most impact.
- For dates 31–90 days out: the forecast is directional — used for staffing, purchasing, and marketing planning rather than tactical pricing.

### Pas 5: Weekly Forecast Review Meeting
- **Frequency**: every Monday at 09:00, 30 minutes maximum.
- **Attendees**: GM, Revenue Manager (or the person managing rates), Front Office Manager, Sales/Marketing Manager (if the role exists), F&B Manager.
- **Agenda** (structured, no tangents):
  1. **Actuals vs. Forecast for last week** (5 min): occupancy, ADR, RevPAR, total revenue. Highlight any day with variance >5% and explain why (event stronger/weaker than expected, weather impact, cancellation spike, competitor rate change).
  2. **OTB review for next 30 days** (10 min): for each week in the next 30 days, compare current OTB vs. STLY OTB at the same booking window position. Flag dates that are significantly ahead (rate increase opportunity) or behind (promotion or rate decrease needed).
  3. **Pricing action decisions** (10 min): based on the OTB review, decide specific rate changes for the next 14 days and rate strategy for days 15–30. Log every decision: date range, old rate tier, new rate tier, rationale. Cross-reference with HM-005 pricing triggers.
  4. **60/90-day outlook** (5 min): any major upcoming events, group inquiries in the pipeline, seasonal transitions? Are staffing and purchasing aligned with the forecast?
  5. **Action items** (2 min): assign specific follow-ups with deadlines.
- **Meeting output**: updated rolling forecast calendar, rate change instructions to whoever manages the channel manager/PMS rates, and meeting minutes in the Forecast Log.

### Pas 6: Monthly Forecast Summary and Distribution
- By the 3rd of each month: produce the Monthly Forecast Summary for the upcoming month.
- Format: one page per month, rows = days, columns: Forecasted Occ% | Forecasted ADR | Forecasted RevPAR | Forecasted Total Revenue | Notes.
- Summary row: monthly totals and averages, plus comparison to budget and STLY month.
- Color coding: green = above budget, yellow = within 5% of budget, red = below budget by more than 5%.
- Distribute to: GM, F&B Manager (for staffing and prep planning), Housekeeping Manager (for staffing), Finance/Accounting, Owner/Investors (monthly investor report format).
- **F&B Manager use case**: if the forecast shows 85% occupancy on Saturday, the kitchen preps for 85% of capacity dinner covers. If the forecast shows 35% occupancy on Tuesday, the kitchen reduces prep and may close a la carte in favor of a simpler menu. Forecast accuracy directly reduces food waste and labor cost.

### Pas 7: Variance Analysis (monthly, after month closes)
- Pull actual results for the closed month by the 5th of the following month.
- Calculate variance for every line: Forecast vs. Actual for Occupancy, ADR, RevPAR, Room Revenue, F&B Revenue, Total Revenue. Express as both absolute (RON) and percentage.
- Identify the **top 3 drivers of positive variance** and **top 3 drivers of negative variance**. Be specific: "Thursday June 8 was forecasted at 45% occupancy but achieved 72% due to an unforecast local wedding group of 14 rooms that booked 10 days before arrival" — this tells you to improve your short-window group detection.
- **Update adjustment factors**: if your event factor for local festivals was consistently underestimated by 8–12%, revise the factor upward for similar events in future months. If your STLY baseline is consistently 5% too optimistic (suggesting the market has softened), adjust the market trend factor downward.
- **Forecast accuracy tracking**: calculate MAPE (Mean Absolute Percentage Error) for each month. MAPE = average of |Actual - Forecast| / Actual x 100 across all days. Target: MAPE below 10% after 6 months of operation, below 5% after 12 months.
- Archive the variance report in the Revenue Management folder (digital: /Revenue/[Year]/[Month]/Variance-Analysis-[Month].xlsx). This builds the institutional memory that makes next year's forecast significantly better.

### Pas 8: Scenario Planning for Uncertainty
- For periods with high uncertainty (new property with no history, post-pandemic recovery, major competitor opening), build three forecast scenarios:
  - **Optimistic** (+15% above base case): market grows faster, events are stronger, direct bookings exceed expectations.
  - **Base case**: the standard forecast.
  - **Conservative** (-15% below base case): market softens, weather disrupts a shoulder season, competitor launches an aggressive promotion.
- For each scenario, calculate the revenue impact and identify the breakeven point (minimum occupancy at which the property covers all fixed costs). For most independent hotels in Romania, the breakeven occupancy is 35–50% depending on the cost structure.
- Prepare contingency actions for the conservative scenario: which variable costs can be reduced within 2 weeks if demand drops (casual staff hours, marketing spend, F&B prep levels)? Having pre-planned contingency responses prevents panic decisions.

### Pas 9: Technology and Automation Opportunities
- **Revenue Management Systems (RMS)**: for properties with 30+ rooms and established history, an RMS (e.g., IDeaS, Duetto, Atomize, RoomPriceGenie) automates the forecast and pricing recommendation process using machine learning. Cost: 200–800 EUR/month depending on property size. ROI threshold: if the RMS improves RevPAR by 3–5%, it pays for itself.
- **RoomPriceGenie**: specifically designed for small independent properties (10–50 rooms). Connects to PMS and channel manager, provides automated dynamic pricing recommendations based on demand signals, competitor rates, and booking pace. Cost: approximately 100–300 EUR/month. Recommended for properties that do not have a dedicated Revenue Manager.
- **For properties without an RMS**: the spreadsheet-based forecast model described in this procedure is fully functional. The key is discipline — updating weekly, reviewing variance, and calibrating factors monthly. A well-maintained spreadsheet outperforms an expensive RMS that is not configured correctly.
- **Booking.com Extranet tools** (free): the Demand Calendar shows expected demand in your market for the next 12 months based on search volume. The Rate Intelligence tool shows competitor pricing (available in the Analytics section). These are the minimum data sources for any property on Booking.com.

## Verificare
- [ ] 12–24 months of historical data imported, cleaned, and tagged with season/event flags
- [ ] STLY baseline calculated for all 365 days with day-of-week matching
- [ ] All four adjustment factors (market, pace, event, risk) documented with sources — no arbitrary numbers
- [ ] 90-day rolling forecast calendar live in Google Sheets and accessible to all stakeholders
- [ ] Weekly forecast review meeting scheduled as a standing calendar event (every Monday 09:00)
- [ ] Monthly forecast summary distributed to all stakeholders before the 3rd of each month
- [ ] Variance analysis completed within 5 days of month close with specific driver identification
- [ ] MAPE tracked monthly — trending toward <10% within 6 months
- [ ] Breakeven occupancy calculated and known by GM and owner

## Instrumente
- PMS export (CSV/Excel — daily revenue, occupancy, ADR data)
- Google Sheets (forecast model, rolling calendar, variance analysis)
- Booking.com Extranet Analytics (Demand Calendar, Rate Intelligence — free)
- STR Report (subscription — competitive set benchmarking, optional but highly recommended)
- RoomPriceGenie or equivalent RMS (for properties ready to automate, 100–300 EUR/month)
- Airbnb Host Analytics (market insights, free in hosting dashboard)
- INSSE tourism statistics (free, published monthly by Romania's National Institute of Statistics)

## Note
- **Most common mistake**: anchoring the forecast entirely on last year without adjusting for booking pace shifts. A hotel that redesigned its website and launched Google Hotel Ads may see direct booking pace 30% ahead of STLY — without adjusting, you will underforecast and underprice, leaving significant revenue on the table.
- **Second most common mistake**: producing a forecast but not acting on it. A forecast that sits in a spreadsheet and is not discussed in a weekly meeting and used to make pricing and staffing decisions is wasted effort. The meeting cadence (Pas 5) is the mechanism that converts data into revenue.
- **Edge case — property opening year**: if you have no historical data, build the forecast using: (1) local market data from Booking.com Demand Calendar, (2) competitor occupancy estimates (review volume on Booking.com can approximate occupancy), (3) industry benchmarks for your property class and location (boutique rural Romania: year 1 expect 40–50% annual occupancy, ADR 250–400 RON). Conservative assumptions in year 1 protect cash flow; the variance analysis from year 1 will make year 2's forecast far more accurate.
- **Edge case — sudden demand shock** (pandemic, natural disaster, major negative press): switch from the standard forecast to a weekly re-forecast cycle using real-time OTB data only (ignore STLY, it is irrelevant). Focus on breakeven analysis and cost containment.
- **Albastru context**: start with a 12-month model since the property may have limited operating history. Use Booking.com Extranet's Demand Calendar (free) as a proxy for market demand signals. For a rural property in Arges/Valcea region, key demand drivers are: (1) Summer (July–August), (2) Easter and Christmas/New Year, (3) May 1st and December 1st long weekends, (4) Local festivals and events. Weekday demand will be the biggest challenge — the corporate retreat segment (HM-006) is the primary strategy for Monday–Thursday occupancy. Start forecasting from month 1 of operations, even if the data feels thin — the discipline of weekly review is more valuable than the accuracy of the initial numbers. Link this procedure to HM-005 (Dynamic Pricing), HM-010 (KPI Dashboard), and HM-020 (Seasonal Pricing) — all four should be reviewed in the same weekly meeting.
