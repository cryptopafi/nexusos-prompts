---
id: HM-005
title: Dynamic Room Pricing Strategy
domain: hotel-management
source: Hotel Management 360 (Tourism & Hotel Academy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# Dynamic Room Pricing Strategy

## Obiectiv
Replace static rack rates with a rule-based dynamic pricing system that adjusts room prices based on demand signals, booking pace, competitor rates, and day-of-week patterns — maximizing RevPAR while maintaining occupancy targets of 55–70% annually and ADR growth of 5–8% year-over-year.

## Pași

### Pas 1: Define Your Rate Architecture
- Establish the **BAR (Best Available Rate)**: the lowest non-discounted public rate. This is the floor for standard demand periods and the anchor from which all other rates derive. Set BAR by calculating your breakeven room cost (fixed + variable cost per room night) plus a minimum margin of 30%. Example: if your total cost per occupied room (including proportional fixed costs, housekeeping, amenities, laundry, and OTA commission) is 180 RON, your minimum BAR should be 234 RON. In practice, BAR for a boutique rural property in Romania ranges from 250 to 500 RON depending on room quality, location, and positioning.
- Create **rate tiers above BAR** for increasing demand levels:
  - **Tier 1 — BAR** (100%): standard demand periods, shoulder season weekdays.
  - **Tier 2 — BAR +10–15%**: above-average demand (forecasted occupancy 60–70%). Shoulder season weekends, early high season.
  - **Tier 3 — BAR +20–25%**: high demand (forecasted occupancy 70–80%). High season weekdays, standard weekends in summer.
  - **Tier 4 — BAR +35–45%**: very high demand (forecasted occupancy 80–90%). High season weekends, minor event weekends.
  - **Tier 5 — BAR +50–80%**: peak/sell-out risk (forecasted occupancy >90%). New Year's Eve, Easter, major local festivals, long weekends with MinLOS.
  - **Tier 6 — BAR +100%+** (ceiling rate): extraordinary demand events. Only triggered when all competitors are sold out and demand is confirmed extreme (once or twice per year at most).
- Create **rate tiers below BAR** for demand stimulation:
  - **BAR -10%**: shoulder/low season incentive rate for 2+ night stays.
  - **BAR -15–20%**: distress rate for last-minute fill (within 5 days of arrival date with occupancy below 40%).
  - **Floor rate**: the absolute minimum below which you will never sell, regardless of demand. Set at breakeven cost + 10%. Selling below the floor damages brand perception and creates price anchoring that undermines future pricing.
- Document every rate tier in a **Rate Matrix spreadsheet**: rows = room types (Standard, Superior, Deluxe, Suite), columns = rate tiers. Every intersection has a specific price. Add a second matrix for net rates (after commission) so you always know your actual margin.

### Pas 2: Define Pricing Triggers — The Rules Engine
- Pricing triggers are the conditions that cause a rate tier change. Making these explicit and documented eliminates subjective pricing decisions and ensures consistency.
- **Occupancy pace trigger (primary)**: compare OTB occupancy for a future date against STLY OTB at the same booking window position.
  - OTB pace >15% ahead of STLY at 30 days out → move up one tier.
  - OTB pace >25% ahead of STLY at 30 days out → move up two tiers.
  - OTB pace >10% behind STLY at 14 days out → move down one tier (but never below BAR unless within 5-day last-minute window).
- **Day-of-week trigger**: weekends (Fri–Sat) start at BAR+15% by default year-round. Sunday–Thursday start at BAR unless pace data suggests otherwise.
- **Event trigger**: confirmed local demand generator within 30km → pre-set BAR+25–40% for those dates from the moment the event is publicly announced. Examples: local wine festival, outdoor concert series, corporate event at a nearby venue that generates room overflow. Add these events to your forecasting calendar (HM-004) and rate calendar simultaneously.
- **Competitor trigger**: monitor the top 3 competitors on Booking.com daily. If all three are priced above your rate by more than 15%, you are likely underpriced — review upward. If all three are below your rate by more than 10% and your OTB pace is behind STLY, consider matching. Never blindly follow competitor prices — your property's differentiation (HM-018) may justify a premium.
- **Pickup velocity trigger**: if you receive 3+ bookings for the same future date within 24 hours, demand is spiking — move up one tier immediately and monitor closely. Conversely, 3+ cancellations for the same date within 24 hours is a warning signal — investigate (competitor promotion? Bad review posted? Event cancelled?).
- **Weather/seasonality trigger**: school holiday periods (Bucharest schools, as they are the primary domestic leisure source), national public holidays, and Orthodox religious holidays → pre-set premium tiers at the beginning of the rate-loading cycle.
- **Last-minute fill trigger**: if 5 days from arrival date and occupancy is below 40%, activate the distress rate (BAR-15 to BAR-20%) with no minimum stay restriction. This captures price-sensitive last-minute demand that is better than an empty room. Close the distress rate immediately when occupancy reaches 60% for that date.
- **Booking channel trigger**: direct bookings should always have a slight perceived advantage. Offer BAR on OTA, but add a value-add for direct (free breakfast, late checkout, welcome drink). This maintains rate parity (OTA contractual requirement) while incentivizing direct booking.

### Pas 3: Set Up Rate Loading in PMS and Channel Manager
- In your PMS or channel manager (Cloudbeds, SiteMinder, Beds24, or direct OTA extranets), create a rate plan for each tier. Name them clearly: "Standard Rate," "Weekend Rate," "High Demand Rate," "Peak Rate," "Last Minute Rate."
- Map each rate plan to all active distribution channels: Booking.com, Airbnb, direct website, phone/email (walk-in rate = BAR or above, never discounted for walk-ins during peak).
- **Rate parity rule**: OTA contracts (Booking.com, Expedia) require that you do not display a lower public rate on other OTAs. You CAN offer a lower effective rate on your own direct channel through value-adds (free breakfast, room upgrade, late checkout) — this is the primary lever for growing direct bookings.
- **Derived rates**: set up derivative rate plans that automatically calculate from BAR changes. Example: "OTA Rate = BAR + 0%" (same as BAR), "Direct Rate = BAR + free breakfast" (same price but better value), "Corporate Rate = BAR - 15%" (negotiated). When you change BAR, all derived rates adjust automatically — this prevents rate inconsistencies across channels.
- **Test the rate loading**: manually trigger a rate change in the channel manager and verify it updates correctly on Booking.com and Airbnb within 15 minutes. Check that the correct room type is displayed at the correct price. A misconfigured channel manager can sell your best room at your lowest rate — test thoroughly before going live.
- **Rate loading frequency**: load rates for the full 12 months ahead at the start of each year (using your seasonal calendar from HM-020). Then adjust individual dates through the daily and weekly review process.

### Pas 4: Daily Rate Review (10 minutes every morning at 08:30)
- This is the most important daily revenue management task. It takes 10 minutes and directly impacts revenue. Non-negotiable, every single morning.
- Open the rate management dashboard (PMS or channel manager):
  - View current rate, OTB occupancy, and remaining inventory for: today, tomorrow, and the next 7 days.
  - Compare today's OTB vs. yesterday's OTB for the same dates (are you gaining or losing bookings overnight?).
  - Check competitor rates: use Booking.com's rate intelligence widget (free in extranet) or manually check the top 3 competitors on Booking.com (search as a guest). Note their price, room type, and any promotions.
- **Decision framework** (use this exact logic):
  - OTB pace ahead of STLY + competitors at same or higher level → hold rate or raise one tier. You have demand momentum and market support.
  - OTB pace ahead of STLY + competitors lower → hold rate. Your demand is strong despite competitor pricing — your differentiation is working.
  - OTB pace behind STLY + competitors lower → lower rate one tier to capture price-sensitive demand. You are losing to competitors on price.
  - OTB pace behind STLY + competitors higher → lower rate to capture demand that is not going to competitors (demand is weak market-wide). Consider a promotional package rather than a straight rate reduction.
  - Date is within 3 days and occupancy below 50% → activate last-minute rate if not already active.
  - Date is within 3 days and occupancy above 85% → raise rate to highest tier. The remaining rooms are the most valuable ones.
- **Log every rate change decision** in the Rate Log: date changed, target dates affected, from rate tier, to rate tier, rationale, who made the decision. This log is reviewed in the weekly meeting and is essential for learning what works.

### Pas 5: Length-of-Stay (LOS) Controls
- LOS controls complement pricing by protecting high-demand nights from being displaced by low-value short stays.
- **Minimum LOS (MinLOS)**:
  - During peak weekends: apply MinLOS=2 on Friday arrival. This forces Friday arrivals to stay through Saturday (the highest-revenue night).
  - During holiday peaks (Easter, Christmas/New Year): apply MinLOS=3. This captures the full holiday revenue window.
  - During event weekends with Saturday sold out but Friday/Sunday available: apply MinLOS=2 on Friday to create Saturday-inclusive stays.
  - Set MinLOS in the PMS/channel manager per date. It should apply automatically on all channels.
- **Closed to Arrival (CTA)**: prevents new arrivals on a specific date.
  - Use CTA on dates that are sold out: there is no point allowing arrivals on a night with no rooms. A CTA prevents 1-night bookings that arrive on your peak night and check out the next morning, which blocks a higher-value multi-night stay.
  - Example: Saturday is sold out, but a 1-night arrival on Saturday would check out Sunday morning — the room cannot be sold Saturday night anyway. Close arrivals on Saturday and keep departures only.
- **Closed to Departure (CTD)**: prevents checkouts on a specific date. Use sparingly.
  - Application: if Thursday night is very high demand and Wednesday night is moderate, close departures on Thursday. This forces Wednesday arrivals to stay through Thursday, capturing the higher-revenue night.
- **When to remove restrictions**: during low demand, remove ALL restrictions. Open 1-night stays, remove MinLOS, open all arrival/departure dates. Any demand is better than no demand when you have excess inventory.
- **LOS discount strategy**: offer a progressive discount for longer stays during shoulder and low seasons. "Stay 2 nights = 5% off, Stay 3 nights = 10% off, Stay 5+ nights = 15% off." This increases average LOS and reduces per-room acquisition cost. Do NOT offer LOS discounts during peak demand.

### Pas 6: Room Type Upsell Pricing
- Dynamic pricing applies not just to the overall rate level but to the spread between room types.
- **High demand periods**: increase the spread between Standard and Superior/Deluxe. If Standard is at BAR+35% (405 RON), Deluxe should be at BAR+50% (450 RON). The guest perceives the upgrade as affordable relative to the already-premium Standard rate.
- **Low demand periods**: narrow the spread. If Standard is at BAR-10% (270 RON), price Deluxe at only +30 RON more (300 RON) rather than the usual +80 RON spread. This encourages natural self-upgrading — guests choose the better room because the premium feels trivial.
- **Last room available pricing**: if only one room type has remaining inventory (e.g., only Deluxe rooms left), do NOT lower the rate to Standard level. Price at or above the Deluxe rate. The last available rooms in a near-sold-out hotel are the most valuable — scarcity justifies premium pricing.

### Pas 7: Weekly Revenue Strategy Review
- Every Monday: part of the weekly forecast review meeting (HM-004, Pas 5).
- Review last week's rate decisions vs. actual outcomes:
  - Did the rate increase on Friday capture demand at the higher rate, or did it suppress bookings (you raised too early or too high)?
  - Did the last-minute rate fill rooms that would have been empty, or did it cannibalize full-rate bookings (you offered the discount too early)?
- Calculate **Rate Achievement**: Actual ADR / BAR x 100%. Targets: >110% rate achievement on peak demand dates (you priced above BAR), >95% on high demand dates, >85% on shoulder dates, >70% on low demand dates (discounting is expected). If rate achievement on peak dates is below 105%, you are underpricing.
- **Revenue Opportunity Cost**: estimate how much revenue was left on the table. If a peak Saturday sold out at BAR+25% by Wednesday, and competitor rates suggest BAR+40% would have sold, the opportunity cost is the difference x rooms sold after you could have raised the rate. This is the most powerful learning metric for improving pricing decisions.
- Adjust rate triggers if patterns show consistent under/over-pricing. Document all adjustments in the Rate Strategy Log.
- Update the 30-day rate calendar with next week's rate decisions and communicate to the person managing the channel manager.

### Pas 8: Competitor Rate Intelligence
- Maintain a Competitor Rate Tracker (Google Sheets): track the top 3–5 competitors' rates daily for the next 14 days, weekly for the next 30 days.
- Columns: Date | Your Rate | Competitor A Rate | Competitor B Rate | Competitor C Rate | Your Position (highest/middle/lowest) | Notes.
- **When you should be the highest-priced**: when your property has a genuine differentiator (HM-018) that competitors cannot match. For a property with a unique culinary experience, premium pricing is justified and expected by the target segment.
- **When you should be competitive**: when you are in a cluster of comparable properties targeting the same segment, and your occupancy pace is behind. Price competitively to capture your fair share of market demand.
- **When competitor intelligence is misleading**: OTA displayed rates may not reflect the actual selling rate (competitors may have promotions, Genius discounts, or mobile-only rates that lower the effective price). Do not chase competitor rates that include hidden discounts — focus on your own OTB pace as the primary signal.
- **Rate shopping tools**: for properties with 20+ rooms, a rate shopping tool (OTA Insight, Lighthouse/formerly Hotel Price Reporter, RateGain) automates competitor rate monitoring across multiple channels. Cost: 50–200 EUR/month. For smaller properties, manual daily checks on Booking.com are sufficient.

### Pas 9: Common Pricing Mistakes to Avoid
- **Dropping rates too early**: if occupancy is below target but the arrival date is still 3+ weeks away, wait. Demand often materializes in the final 2 weeks for leisure properties. Dropping the rate early trains guests to wait for discounts.
- **Not raising rates when demand is strong**: fear of "losing bookings" by raising rates is the most expensive fear in hotel management. If your property will sell out regardless, every room sold at a lower rate is pure revenue loss. Raise early and incrementally.
- **Flat pricing across the week**: a Tuesday night and a Saturday night have fundamentally different demand — they should never be the same price. Day-of-week pricing is the minimum level of dynamic pricing.
- **Discounting instead of adding value**: a 20% discount reduces revenue by 20%. A value-add (free breakfast that costs you 25 RON but is valued at 65 RON by the guest) increases perceived value without reducing revenue proportionally. Always prefer value-adds to straight discounts.
- **Ignoring total guest value**: a guest paying BAR-10% who books dinner (200 RON), buys a wine bottle (80 RON), and extends their stay by 1 night is more valuable than a guest paying BAR+20% who brings their own food and checks out early. Revenue management should consider total guest value, not just room rate.
- **Set-and-forget pricing**: rates loaded in January for July should be reviewed and adjusted as the booking window progresses. A rate set 6 months ago without adjustment is almost certainly wrong.

## Verificare
- [ ] Rate architecture documented with BAR, all premium tiers, discount tiers, and floor rate
- [ ] Rate Matrix spreadsheet complete for all room types x all rate tiers (gross and net)
- [ ] All pricing triggers documented explicitly with thresholds and actions
- [ ] Rate plans loaded and mapped in PMS/channel manager for all active channels
- [ ] Rate parity verified across all OTA channels (manual spot check weekly)
- [ ] Daily rate review completed every morning at 08:30 (Rate Log entry exists for every business day)
- [ ] MinLOS restrictions active for all identified high-demand dates (minimum 60 days ahead)
- [ ] Competitor Rate Tracker maintained (minimum 3 competitors, daily for 14 days)
- [ ] Weekly revenue strategy review completed with Rate Achievement and Revenue Opportunity Cost calculated
- [ ] Rate triggers tested and refined quarterly based on outcome data

## Instrumente
- PMS or Channel Manager (Cloudbeds, SiteMinder, Beds24, or direct OTA extranets)
- Booking.com Rate Intelligence widget (free in extranet Analytics section)
- Rate Matrix spreadsheet (Google Sheets — all room types x all tiers, gross and net rates)
- Rate Log (Google Sheets — date, rate change, dates affected, rationale, approver, outcome)
- Competitor Rate Tracker (Google Sheets — daily rates for top 3–5 competitors)
- RoomPriceGenie (automated pricing recommendation tool for small properties, 100–300 EUR/month)
- Revenue Calendar (linked to HM-004 forecast and HM-020 seasonal calendar)

## Note
- **The fundamental principle**: dynamic pricing is not about being greedy on high-demand nights. It is about optimizing revenue across all demand levels. Smart pricing on low-demand nights (last-minute fill rates, LOS discounts, value-add packages) prevents zero-revenue empty rooms. Smart pricing on high-demand nights (premium tiers, MinLOS, CTA) captures the full value of scarcity.
- **RevPAR, not ADR, is the goal**: a property that achieves 800 RON ADR at 30% occupancy (RevPAR = 240 RON) is performing worse than a property at 400 RON ADR at 70% occupancy (RevPAR = 280 RON). The objective is to find the price point that maximizes the product of occupancy and rate — not to maximize rate alone.
- **Albastru / Șapte Focuri context**: if no channel manager is available yet, manage rates manually in the Booking.com and Airbnb extranets daily. The daily 10-minute review (Pas 4) is non-negotiable even without automation — it is the single highest-ROI activity in hotel revenue management. For a 6–10 room property, the impact of pricing correctly is proportionally even larger than for a 100-room hotel because each room represents 10–17% of total inventory. A single room sold at the wrong rate on a peak night costs 5–15% of the night's total revenue. The fire-kitchen experience and the property's unique positioning (HM-018) justify premium pricing 20–40% above comparable rural accommodation — guests are paying for an experience, not just a bed. Build your BAR around the experience value, not just the room cost. BookRetreats.com, as the property's primary OTA for the wellness/retreat segment, may have different commission structures and pricing flexibility — manage it as a separate channel with its own rate plan.
