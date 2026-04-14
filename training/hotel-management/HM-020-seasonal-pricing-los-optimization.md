---
id: HM-020
title: Seasonal Pricing and Length-of-Stay Optimization
domain: hotel-management
source: Hotel Management 360 (Tourism & Hotel Academy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# Seasonal Pricing and Length-of-Stay Optimization

## Obiectiv
Design and execute a full-year pricing and demand management calendar that maximizes RevPAR across all seasons — using season-specific pricing tiers, length-of-stay controls, booking window management, and shoulder/low-season demand stimulation strategies to smooth revenue volatility and achieve a RevPAR variance of less than 25% between the best and worst months (excluding deliberate closure periods).

## Pași

### Pas 1: Define Your Seasonal Calendar — The Foundation of All Pricing Decisions
- Analyze 12–24 months of occupancy data (or local market data if the property is new) and map every calendar date to one of four season types:
  - **Peak Season** (highest demand, >80% expected occupancy): periods where demand exceeds supply and you have pricing power. For a rural/nature property in Romania:
    - July 1 – August 31 (summer holidays, domestic and international leisure)
    - Easter weekend (Orthodox calendar: variable, typically April — check each year)
    - December 27 – January 3 (Christmas/New Year holidays)
    - May 1 long weekend (if combined with bridge days)
    - December 1 long weekend (National Day)
  - **High Season** (strong demand, 65–80% expected occupancy):
    - June (early summer, school not yet out but weather is excellent)
    - September (post-summer, still warm, fewer crowds — increasingly popular with experienced travelers)
    - December 20–26 (Christmas lead-up)
    - Long national holiday weekends (Rusalii/Pentecost, August 15 — Assumption of Mary)
    - Weekends May–October (Friday–Sunday)
  - **Shoulder Season** (moderate demand, 40–65% expected occupancy):
    - April (spring opening — wildflowers, pleasant weather, Easter shoulder weeks)
    - October (autumn colors, harvest season, wine-related experiences)
    - November 1–15 and December 1–19 (pre-holiday periods)
    - January 4–31 (post-New Year)
    - Weekdays (Mon–Thu) during High Season months
  - **Low Season** (below-average demand, <40% expected occupancy):
    - November 16–30
    - February (typically the weakest month for rural properties)
    - March (early spring, unpredictable weather)
    - Weekdays (Mon–Thu) during Shoulder Season months
- **Create the Season Map**: a 12-month color-coded calendar (Google Sheets or printed poster). Each day is colored: red = Peak, orange = High, yellow = Shoulder, blue = Low. This visual tool makes rate decisions intuitive. Post it in the revenue manager's office and at the front desk.
- **Review and adjust annually** (October): some dates may shift between seasons based on changing demand patterns. If September has been consistently performing at High-season levels, reclassify it upward.

### Pas 2: Set Seasonal Rate Levels — The Price Architecture
- Build rate tiers with seasonal multipliers applied to your BAR (Best Available Rate — see HM-005):
  - **Peak rates**: BAR x 1.5–2.0
    - Example: BAR = 350 RON → Peak rate = 525–700 RON
    - New Year's Eve and Easter can justify BAR x 2.0–2.5 if demand supports it
    - Peak rates should feel premium but not gouging — the guest must perceive value. The fire-kitchen dinner, the nature setting, and the personal service justify the premium.
  - **High rates**: BAR x 1.2–1.4
    - Example: BAR = 350 RON → High rate = 420–490 RON
    - Weekend surcharge: Fri–Sat should be priced at the high end of this range or at Peak level during summer
  - **Shoulder rates**: BAR x 1.0–1.1
    - Example: BAR = 350 RON → Shoulder rate = 350–385 RON
    - This is the BAR or just above. The "normal" rate for periods without strong demand drivers.
  - **Low rates**: BAR x 0.75–0.90
    - Example: BAR = 350 RON → Low rate = 262–315 RON
    - Never below your calculated floor rate (breakeven + 10% minimum margin). Going below the floor creates a price anchor that makes it hard to charge full rates when demand returns.
    - Last-minute distress rate (within 5 days, occupancy below 40%): BAR x 0.70–0.80
  - **Special event override rates**: for exceptional demand dates (major local festival, once-per-year event), override the seasonal rate with a specific rate that may exceed the Peak formula. These are set manually per event, not formulaically.
- **Rate loading**: load all seasonal rates into PMS and channel manager for the full 12 months ahead at the beginning of each year. Every future date should have a rate loaded. An unrated date either shows as "unavailable" on OTAs (lost revenue) or defaults to a system rate that may be wrong. Set a reminder to load rates by December 15 for the following year.
- **Room type spread by season**: adjust the price difference between room types based on seasonal demand (per HM-005 Pas 6):
  - Peak/High: wider spread (guests will pay the premium for the best room because they planned and saved for this trip).
  - Shoulder/Low: narrower spread (encourage self-upgrading to fill higher-category rooms that would otherwise sit empty).

### Pas 3: Length-of-Stay (LOS) Controls — Protecting Peak Revenue
- LOS controls prevent high-demand nights from being "wasted" by short stays that block more valuable multi-night bookings. They are the complement to pricing — pricing captures more revenue per night; LOS controls capture more nights.
- **Minimum Length of Stay (MinLOS)**:
  - **Peak weekends** (year-round): MinLOS = 2 nights (Friday arrival requires Saturday stay). This is the most basic LOS control and should be active every weekend during High and Peak seasons.
  - **Holiday long weekends** (May 1, June 1, December 1, Rusalii): MinLOS = 2 or 3 nights depending on the holiday structure.
  - **School holiday peak weeks** (July–August, Easter week, Christmas/New Year): MinLOS = 3 nights. During the absolute peak (Christmas week, New Year's Eve period), MinLOS = 4–5 nights is common and accepted.
  - **Event-driven peaks**: if a major local event spans 3 days, apply MinLOS = 3 for the event dates.
  - Set MinLOS in PMS/channel manager per date. It applies automatically across all channels (Booking.com, Airbnb, direct). Verify after setting — a MinLOS that is not active on one channel creates an arbitrage opportunity where that channel sells 1-night stays at the expense of multi-night bookings on other channels.
- **Closed to Arrival (CTA)**:
  - Close arrivals on dates that are sold out or nearly sold out. Purpose: prevent 1-night arrivals on a peak night that check out the next morning, blocking a multi-night booking that would have included that night.
  - Example: Saturday is sold out. Close Saturday to arrivals. Guests can still depart on Saturday (existing bookings that include Saturday night are unaffected), but no new 1-night Saturday arrivals that would displace multi-night opportunities.
  - Also use CTA to manage the "island" problem: if Friday and Sunday are high-demand but Saturday is even higher, close Saturday arrivals to force bookings that include Saturday to arrive on Friday (2+ night stay).
- **Closed to Departure (CTD)**:
  - Rarely used but powerful in specific situations. CTD on a specific date prevents guests from checking out on that date, forcing them to stay through it.
  - Use case: Thursday night is very high demand (local event). Apply CTD on Thursday. A guest arriving Wednesday for 1 night would normally check out Thursday morning — CTD forces a 2-night minimum (Wed+Thu).
- **When to REMOVE all LOS restrictions**: during low-demand periods, remove every restriction. Open 1-night stays. Remove MinLOS. Open all arrival and departure dates. Any booking is better than an empty room. Keeping a MinLOS=2 during a week with 30% occupancy actively prevents bookings — you are rejecting revenue you need.
- **LOS discount strategy** (shoulder and low season only):
  - "Stay 2 nights = 5% off per night"
  - "Stay 3 nights = 10% off per night"
  - "Stay 5+ nights = 15% off per night" (digital nomad / long-stay rate)
  - "Stay 7 nights = pay for 6" (the classic weekly rate)
  - Progressive LOS discounts increase average LOS, reduce per-room acquisition cost, reduce housekeeping load (less frequent full cleaning), and improve occupancy stability. Do NOT offer LOS discounts during Peak or High demand — full-rate demand exists.

### Pas 4: Shoulder and Low Season Demand Stimulation — Creating Demand Where None Exists
- Low season does not mean accepting empty rooms and waiting for summer. Every empty room night is lost revenue forever — it cannot be "saved" for later. Active demand creation is essential.
- **Strategy 1 — Weekend Escape Packages** (target: Local/Staycation segment):
  - "Fire Kitchen Weekend Escape": 2 nights (Fri–Sun), breakfast included, dinner at Sapte Focuri on Saturday night with a complimentary aperitif, guided morning walk. Package price: 15% below sum of individual components. Example: if 2 nights at BAR + dinner + breakfast individually = 1,100 RON, package price = 935 RON.
  - Promote: Instagram stories (targeted to 100km radius), Facebook local groups, email to guest database, partnerships with Bucharest-based wellness and lifestyle brands.
  - Target audience: couples, friend groups, and families within 100km who want a short, hassle-free escape.
  - Conversion benchmark: well-promoted weekend packages fill 5–15 room nights per month during shoulder season.
- **Strategy 2 — Themed Events** (creates demand generators where none naturally exist):
  - "Autumn Harvest Dinner": multi-course meal featuring seasonal ingredients, paired with local wines. One evening event, priced at 250–400 RON per person including accommodation discount for overnight stay. Creates a single-night demand spike that can fill the property.
  - "Cooking Class Weekend": Saturday morning cooking class with the chef (open-fire cooking techniques), followed by lunch. Package: class + 1 night + dinner + breakfast. Unique, Instagram-worthy, and a powerful differentiator amplifier.
  - "Wine Tasting and Fire Dinner": partnership with a local winery for a vineyard visit + tasting followed by a fire-kitchen dinner with wine pairing. Full-day experience + overnight.
  - "Digital Detox Retreat": 3-night package (Mon–Thu). No TV in rooms, Wi-Fi restricted to common areas, guided nature activities, yoga session (partner with a local instructor), fire-kitchen meals. Targets burnt-out professionals.
  - "Christmas Market Dinner": a special December event — traditional Romanian Christmas feast cooked over the fire. Invitation-only, 30 guests maximum. Creates scarcity and exclusivity.
  - Target: 1–2 themed events per month during shoulder/low season. Each event should be designed to fill at least 50% of available rooms.
- **Strategy 3 — Corporate Mid-Week** (target: the Monday–Thursday occupancy desert):
  - Corporate retreats, team offsites, planning meetings. Package: meeting space + lunch + coffee breaks + team activity (cooking class, foraging walk, wine tasting) + accommodation + dinner. Price per person per night: 400–700 RON all-inclusive.
  - Outreach: direct contact with HR directors and office managers at companies within 100km. Use the existing Albastru corporate outreach list (285+ companies).
  - Positioning: "The most unique team retreat venue in Romania — your team cooks together over an open fire." This is a corporate differentiator that standard conference hotels cannot match.
  - Target: 2–4 corporate bookings per month during shoulder/low season. Even small corporate groups (8–15 people for 2 nights) transform a dead Tuesday/Wednesday into a fully booked, high-revenue period.
- **Strategy 4 — Extended Stay Discounts** (target: digital nomads, remote workers, retirees):
  - "Stay 5+ nights" rate: 15% below BAR. Includes: high-speed Wi-Fi, workspace in room or common area, daily breakfast, weekly housekeeping (instead of daily — reduces cost).
  - Promote on Airbnb (which has a strong long-stay search filter), on remote work platforms (Anyplace, Flatio, NomadList), and through direct outreach to companies with remote work policies.
  - Benefits: fills low-demand weekdays, low housekeeping cost per night, and these guests often become advocates (digital nomad communities are tight-knit).
- **Strategy 5 — Flash Sales** (tactical, time-limited):
  - 48-hour direct booking offer for a specific slow week: "Book by Friday for 25% off stays in November." Sent to the email list (direct channel only — no OTA flash sales unless parity is managed).
  - Effective for driving bookings for specific slow periods that you can see 2–3 weeks in advance.
  - Frequency: maximum 1 per month. More frequent flash sales train guests to wait for discounts.
- **Track which strategy generates the most bookings and revenue** in each low-demand period. Double down on the top performers. Drop or redesign strategies with low uptake.

### Pas 5: Booking Window Management — When Guests Book Matters
- Each season has a different natural booking window — understanding this determines when your pricing and marketing actions need to happen:
  - **Peak**: guests book 2–6 months ahead for school holidays and major holidays. By 60 days before a peak date, 60–80% of rooms should be OTB. If not, your visibility or pricing may be off.
  - **High**: guests book 1–3 months ahead. By 30 days before, 50–70% OTB.
  - **Shoulder**: guests book 2–6 weeks ahead. Demand materializes late. Do not panic at 60 days out with low OTB — shoulder demand comes in the final 2 weeks.
  - **Low**: often booked within 2 weeks or triggered by a promotion. Some low-season bookings are same-week impulse decisions.
- **Pricing strategy by booking window position**:
  - **Far-out bookings** (90+ days before arrival): set at mid-range seasonal rate. Reward early planners with a fair price — they are the foundation of your occupancy.
  - **30–90 days out**: monitor OTB pace vs. STLY. If pace is 15%+ ahead → raise rate one tier. If pace is 10%+ behind → consider a promotional package (not a straight discount).
  - **14–30 days out**: this is the critical pricing window. Most rate adjustments happen here. Match the rate to the demand signal.
  - **0–14 days out**: if occupancy is below target and rooms remain unsold, activate the last-minute fill rate (BAR -15% to BAR -20%). The rationale: a room sold at 280 RON generates 280 RON in revenue + dinner capture (100–200 RON) + breakfast capture (if not included). An empty room generates 0 RON. The marginal cost of selling one more room (housekeeping, amenities, laundry) is 30–50 RON — any rate above marginal cost is profitable.
  - **Day of arrival**: if rooms remain at 18:00, consider a deeply discounted walk-in or same-day OTA rate. At this point, any revenue is pure contribution margin.
- **Never discount below the floor rate** regardless of last-minute urgency. Selling below breakeven is mathematically worse than leaving the room empty (it costs more to service the room than you earn). And sub-floor rates create price anchors that damage future pricing.

### Pas 6: Day-of-Week Revenue Optimization
- Treating every day of the week identically is one of the biggest revenue management mistakes. Each day has a fundamentally different demand profile:
  - **Friday and Saturday**: highest demand for leisure properties. These are your premium nights. Price at the top of the seasonal range. Apply MinLOS=2 (require both nights) during High and Peak seasons.
  - **Sunday**: moderate to low — guests departing from the weekend, few new arrivals. Price at the bottom of the seasonal range for Sunday arrival. Remove MinLOS restrictions.
  - **Monday–Thursday**: lowest demand for rural leisure properties. This is where corporate, long-stay, retreat, and event demand must fill the gap. Price at BAR or below for these nights during shoulder/low season.
  - **Special day-of-week patterns**: some properties see increased demand on specific weekdays due to local factors (a weekly market day, a regional event series, a recurring corporate client). Identify and price accordingly.
- **Revenue optimization tactic — the "anchor night"**: every week, identify the highest-demand night (typically Saturday). Ensure that pricing and LOS restrictions maximize revenue capture for that night. Then use adjacent nights (Friday, Sunday) as "shoulder nights" to the peak — moderately priced, with incentives to extend (extend-your-stay discount: "Add Sunday night for 50% off").

### Pas 7: Package Design for Revenue Optimization
- Packages are a pricing tool, not just a marketing tool. A well-designed package increases total per-stay revenue while making the price feel like a value proposition.
- **Package pricing formula**: price the package 10–15% below the sum of individual components. The guest perceives savings. You capture revenue that would otherwise be lost to separate purchasing decisions (or not purchased at all).
  - Example: 2 nights BAR (350 x 2 = 700) + dinner (200/person x 2 people = 400) + cooking class (150/person x 2 = 300) = 1,400 RON individual total. Package price: 1,190 RON (15% discount). Guest saves 210 RON. You capture 1,190 RON vs. potentially 700 RON (if they only booked the room and nothing else).
- **Package design principles**:
  - Every package includes a room + at least one ancillary element (F&B, experience, service). The ancillary element has low marginal cost but high perceived value.
  - Name packages emotionally: "Romantic Escape," "Fire Kitchen Immersion," "Forest & Feast," "Digital Detox Retreat." Not "2-Night Package with Dinner."
  - Set a clear booking window: "Available for stays April 1–June 15." Urgency increases conversion.
  - Display the individual component prices alongside the package price so the savings are visible.
- **Seasonal package calendar**: design and promote specific packages for each season:
  - Spring: "Awakening of the Senses" — forest walk + seasonal tasting menu + overnight
  - Summer: "Summer at Sapte Focuri" — full-board package with pool access (if available) + outdoor activity
  - Autumn: "Harvest Table" — mushroom foraging + harvest dinner + wine tasting + overnight
  - Winter: "Fire & Snow" — fireside dinner + winter hike + mulled wine + 2 nights
  - Year-round: "Team Fire" (corporate cooking experience), "Romantic Retreat" (couples), "Disconnect" (digital detox)

### Pas 8: Annual Pricing Calendar Review — The October Reset
- Every October, conduct the full-year pricing review for the coming year. This is a 2–3 hour strategic session with the GM and Revenue Manager (or the person managing rates):
- **Data inputs for the review**:
  - Last year's RevPAR by month and by season type. Where did you outperform vs. forecast? Where did you underperform? What drove the variance?
  - OTB pace for the coming year: any dates already significantly booked (early group bookings, corporate contracts, event-related pre-bookings)?
  - Confirmed events for the coming year: festivals, conferences, national holidays, school holiday dates (published by the Ministry of Education by October).
  - Competitor rate monitoring: what are competitors already charging for next summer/holidays?
  - Market signals: new tourism infrastructure (road improvements, new attractions), economic indicators (GDP growth, consumer confidence, exchange rates), tourism policy changes.
- **Decisions to make**:
  - Review and adjust seasonal calendar boundaries. If data shows September performing at High-season levels for the last 2 years, reclassify it.
  - Update BAR for the coming year: apply inflation adjustment + any strategic rate increase. If market conditions support a 5% rate increase above inflation, implement it now.
  - Update seasonal multipliers: if peak rates sold out too early last year (indicating underpricing), increase the peak multiplier. If shoulder rates had low take-up, consider reducing the shoulder multiplier or creating more compelling packages.
  - Update MinLOS rules: confirm which dates get MinLOS 2, which get MinLOS 3, and which are open.
  - Confirm event dates and set event-specific rates.
  - Design the shoulder/low-season promotional calendar: which themed events, which packages, which flash sale windows.
- **Output**: the approved Pricing Calendar for the coming year. Load all rates, restrictions, and packages into PMS and channel manager by December 15.
- **Publish and communicate**: share the Pricing Calendar with all relevant teams (front desk, reservations, marketing). Everyone who takes a booking should know the current rate strategy for any future date.

### Pas 9: Revenue Displacement Analysis — The Advanced Lever
- Displacement analysis answers the question: "By accepting this booking, am I preventing a more profitable booking from being made?"
- **When to perform displacement analysis**: whenever you receive a group booking request or a discounted rate request for dates that have moderate-to-high demand. Especially: corporate groups requesting BAR-20% on a weekend, tour operators requesting allotment at wholesale rates during high season, or long-stay requests at reduced rates during mixed-demand periods.
- **Displacement calculation**:
  - Scenario A (accept the booking): guaranteed revenue from the group at the negotiated rate + estimated F&B revenue + probability of ancillary revenue.
  - Scenario B (decline the booking): expected revenue from selling the same rooms individually at the prevailing rate tier x probability of selling (based on OTB pace and historical sell-through for that date).
  - If Scenario A > Scenario B: accept the booking. The group guarantees revenue that might not materialize otherwise.
  - If Scenario B > Scenario A: decline the booking (or counteroffer at a higher rate). The rooms will likely sell at higher individual rates.
- **Example**: a corporate group requests 8 rooms for a Friday–Saturday at BAR-15% (297.5 RON/night x 8 rooms x 2 nights = 4,760 RON). OTB pace for that weekend shows 70% already booked at 30 days out. Historical data shows 95%+ sell-through at BAR+20% for similar weekends. Expected individual revenue: 8 rooms x 420 RON x 2 nights = 6,720 RON x 0.95 probability = 6,384 RON. The individual scenario generates 1,624 RON more. Decline the group or counteroffer at BAR (no discount).
- Displacement analysis prevents the common mistake of filling the hotel with low-rate bookings when high-rate demand is coming.

## Verificare
- [ ] 12-month Season Map completed with every date classified (Peak/High/Shoulder/Low) — posted visually
- [ ] Seasonal rates loaded in PMS and channel manager for full 12 months ahead — verified on all active OTA channels
- [ ] BAR and all seasonal multipliers documented in the Rate Matrix (HM-005)
- [ ] MinLOS restrictions active for all identified High and Peak season dates — minimum 90 days ahead
- [ ] CTA rules configured for expected sell-out dates
- [ ] At least 4 shoulder/low season demand stimulation strategies planned and budgeted for the year
- [ ] Seasonal package calendar designed with at least 1 package per season — loaded in PMS and promoted
- [ ] Last-minute fill rate rules configured in channel manager (auto-trigger or standing instruction per date threshold)
- [ ] Day-of-week pricing differential active (Fri–Sat premium vs. Mon–Thu standard)
- [ ] Annual pricing calendar review scheduled for October — all rate decisions finalized by December 15

## Instrumente
- PMS rate plan management module (seasonal rates, MinLOS, CTA/CTD per date)
- Channel Manager (rate and restriction loading to all OTA channels — real-time sync)
- Revenue Calendar spreadsheet (Google Sheets: Season Map + rate levels + LOS rules + event dates + package dates)
- Booking.com Extranet (rate, restriction, and package management + Demand Calendar for market signals)
- Email marketing tool (for flash sale campaigns and seasonal newsletter — Mailchimp/Brevo)
- Instagram and Facebook (for promotional package content — targeting local audience within 100km)
- Corporate outreach list (for mid-week retreat packages)
- Displacement Analysis template (Google Sheets: Scenario A vs. Scenario B calculation)
- RoomPriceGenie (automated pricing tool for small properties — optional, 100–300 EUR/month)

## Note
- **The biggest seasonal pricing mistake**: discounting during peak demand because of anxiety about filling rooms. If your historical data and OTB pace both indicate 90%+ sell-through, trust the data and hold premium rates. You will fill at the higher rate. Every time you drop the rate early during a peak period, you leave irretrievable revenue on the table.
- **The second biggest mistake**: passive management during low season. An owner who closes the property for 3 months in winter because "nobody comes" is choosing 0% occupancy when 25–35% occupancy at shoulder rates is achievable with active demand creation (corporate retreats, weekend packages, themed events, long-stay offers). 25% occupancy at 280 RON ADR generates meaningful contribution toward fixed costs that run year-round regardless.
- **The length-of-stay trap**: applying MinLOS=2 during a period with 30% occupancy rejects the 1-night bookings that are the only demand available. LOS restrictions are for protecting peak nights from being wasted — never for restricting already-weak demand.
- **Edge case — extreme weather disrupts peak season**: an unusually rainy July or an unexpected heatwave in September disrupts the normal seasonal pattern. Have a contingency plan: if occupancy drops more than 15 percentage points below forecast during what should be a peak period, immediately activate shoulder/low season pricing and promotional packages. Do not wait and hope — the demand will not materialize if conditions are adverse.
- **Albastru / Sapte Focuri context**: the property likely has a strong summer peak (outdoor setting, nature, pleasant weather) and a sharp drop in November–February. The two biggest RevPAR growth opportunities:
  1. **Shoulder season extension** (April–May, September–October): "spring opening" and "autumn harvest" packages can add 8–15 percentage points of occupancy to months that would otherwise sit at 30%. The natural beauty of spring wildflowers and autumn foliage provides compelling visual content for marketing.
  2. **Mid-week corporate demand** (year-round): the cooking experience is a uniquely compelling corporate team-building activity. Even 2 corporate bookings per month (averaging 10 rooms each for 2 nights) transforms the mid-week revenue landscape. At 400 RON per person per night all-inclusive, a 10-person group generates 8,000 RON in 2 nights — revenue that simply does not exist without proactive corporate outreach.
  The fire kitchen operates year-round (unlike outdoor activities that are weather-dependent), making it the anchor experience that supports shoulder and low season demand. Build the year-round pricing strategy around the kitchen: summer guests come for nature AND the kitchen; winter guests come primarily for the kitchen AND the cozy fireplace atmosphere. The kitchen is the all-season differentiator.
