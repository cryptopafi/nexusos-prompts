---
id: HM-012
title: Cost Control and Expense Management
domain: hotel-management
source: Hotel Management 360 (Tourism & Hotel Academy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# Cost Control and Expense Management

## Obiectiv
Implement a systematic cost control framework that monitors departmental expenses against budget, identifies waste and leakage, and maintains a GOP margin of 30–45% — without compromising guest experience. The framework should reduce controllable costs by 8–15% in year one through waste elimination, purchasing discipline, and energy optimization.

## Pași

### Pas 1: Map All Cost Categories Using the USALI Framework
- Use the **Uniform System of Accounts for the Lodging Industry (USALI)** structure to categorize costs. This is the global hotel industry standard and enables benchmarking.
- **Operated departments (revenue-generating + their direct costs)**:
  - **Rooms Department**: housekeeping labor, cleaning supplies, guest amenities, laundry, linen replacement, room maintenance materials. These are variable costs that scale with occupancy.
  - **F&B Department**: food cost (COGS), beverage cost, F&B labor (kitchen + service), kitchen supplies, tableware replacement, menu printing.
  - **Other Operated Departments**: spa, parking, retail, activities — each with its own revenue and direct cost line.
- **Undistributed operating expenses (overhead)**:
  - **Administration & General**: management salaries, accounting, legal, insurance, office supplies, software subscriptions, bank charges, professional fees.
  - **Sales & Marketing**: advertising, OTA commissions, website maintenance, photography, social media management, PR, travel agent commissions.
  - **Property Operations & Maintenance**: maintenance labor, repair materials, contracted services (elevator, HVAC, pest control), groundskeeping.
  - **Utilities**: electricity, water, gas, firewood/biomass, internet/telephone, waste disposal.
- **Fixed charges**: rent/mortgage, property taxes, insurance premiums, depreciation, loan interest. These are largely non-controllable in the short term.
- For each variable cost line, calculate the **Cost per Occupied Room (CPOR)**: Monthly Cost / Occupied Room Nights. CPOR normalizes cost regardless of occupancy and is the primary metric for tracking cost efficiency.

### Pas 2: Set Cost Benchmarks and Targets
- Industry benchmark CPOR for mid-scale independent hotels in Romania (adjust for your specific context):
  - **Housekeeping supplies** (cleaning chemicals, cloths, bags): 8–15 RON per occupied room.
  - **Guest amenities** (toiletries, stationery, welcome items): 5–12 RON per occupied room. Higher for boutique properties with premium amenities.
  - **Laundry** (in-house or outsourced): 12–25 RON per occupied room. In-house laundry is typically 30–40% cheaper than outsourced at volumes above 1,000 kg/month.
  - **Linen replacement**: 3–5 RON per occupied room (amortized annual replacement cost).
  - **Maintenance materials**: 5–10 RON per occupied room.
  - **Room energy**: 15–30 RON per occupied room (electricity + heating/cooling, varies dramatically by season and climate).
- F&B cost benchmarks:
  - **Food cost %**: 28–32% of food revenue (restaurant), 30–35% (buffet breakfast — inherently wastier).
  - **Beverage cost %**: 20–28% of beverage revenue. Spirits and cocktails: 15–22%. Wine: 25–35% (wine has narrower margins).
  - **F&B labor cost %**: 30–38% of F&B revenue.
- Overall property cost benchmarks:
  - **Total payroll** (all departments including social contributions): 25–35% of total revenue.
  - **Total marketing** (including OTA commissions): 8–15% of total revenue. If OTA commission alone exceeds 12%, the distribution channel is too expensive (see HM-007).
  - **Utilities**: 4–8% of total revenue.
  - **GOP margin target**: 30–45% for a well-managed boutique property. Below 25% is unsustainable long-term.
- Set your property-specific targets based on actual year-1 data. Adjust after 3 months of consistent measurement.

### Pas 3: Purchasing Control — The Purchase Order System
- **The single most impactful cost control tool for independent hotels.** Without a PO system, expenses creep silently. With one, every spend above a threshold is deliberate, justified, and authorized.
- **Purchase Order process**:
  1. Requester identifies a need and fills out a PO form: item description, quantity, supplier name, unit price, total price, budget line it charges against, urgency level (routine/urgent/emergency).
  2. PO is submitted to the authorizing manager per the approval tier.
  3. Authorizing manager reviews: is this purchase budgeted? Is the quantity reasonable? Is the price competitive (compare against the preferred supplier contract)? If approved, signs the PO.
  4. Purchase is made. Requester retains the PO copy.
  5. When the invoice arrives, it is matched against the approved PO. Price, quantity, and items must match. Invoice without a matching PO is flagged and investigated.
  6. Payment is processed only for invoices with matched, approved POs.
- **Approval tiers**:
  - Up to 500 RON: Department Head approves.
  - 500–2,000 RON: GM approves.
  - 2,000–10,000 RON: GM + Owner approve.
  - Above 10,000 RON: Owner approval with competitive quotes from 3 suppliers.
  - Emergency purchases (e.g., broken pipe, equipment failure): Duty Manager can authorize up to 1,000 RON without prior approval, but must complete a PO retroactively within 24 hours with a written justification.
- **Preferred supplier contracts**: establish annual agreements with preferred suppliers for high-volume categories:
  - Food supplier (meat, dairy, produce): negotiate volume-based pricing, delivery schedule, and quality standards. Review quarterly.
  - Cleaning supplies: negotiate annual pricing with a single supplier.
  - Linen supplier: annual contract with fixed pricing for replacement orders.
  - Negotiate payment terms: 30-day net is standard. Some suppliers offer 2–3% early payment discount (net 10).
  - Maintain at least 2 approved suppliers per category to prevent dependency and maintain price competition.
- **Track purchasing**: all approved POs are recorded in the Purchasing Log (Google Sheets): date, PO number, supplier, items, amount, budget line, approved by. Monthly: compare total purchasing spend by category against budget.

### Pas 4: Inventory Management and Waste Control
- **Housekeeping inventory**: monthly physical count of all cleaning supplies, guest amenities, and chemical stock. Compare: opening stock + purchases - closing stock = consumed. Compare consumed vs. theoretical consumption (rooms cleaned x par usage per room). If actual consumption exceeds theoretical by >15%, investigate: over-use, spillage, pilferage, or incorrect theoretical calculation.
- **F&B inventory — the highest-value control area**:
  - **Weekly stockcount** for high-value items: protein (meat, fish), alcohol, dairy, premium ingredients. Weigh and count every item. The investment of 1 hour per week typically saves 5–10x the cost in identified waste and theft.
  - **Daily tracking** for fresh produce: order only what is needed for the next 48 hours. Fresh produce waste is the #1 food cost leak. Use a prep list that calculates quantities based on forecasted covers (HM-004).
  - **FIFO (First In, First Out)**: every delivery is labeled with the receipt date. Older stock is always placed in front of newer stock. FIFO violations are the primary cause of spoilage.
  - **Recipe cards with standard portions**: every dish on the menu has a recipe card showing: ingredients, quantities (by weight), preparation steps, plating instructions, and theoretical food cost. If the recipe calls for 200g of lamb per portion, the kitchen cannot serve 280g "because it looked small." Consistent portioning is the foundation of food cost control.
  - **Yield testing**: for high-value ingredients (beef, fish), calculate the usable yield vs. purchased weight. A 10kg beef loin that yields 7.5kg of usable cuts has a 75% yield. Your food cost calculation must use the yield-adjusted cost, not the purchase price.
- **Food waste tracking**: record all waste daily in the Food Waste Log. Categories: prep waste (trimmings, bones — target: under 10% by weight), plate waste (uneaten returns — target: under 5%), spoilage (expired or damaged — target: under 2% of food cost), over-production (prepped but unsold — target: under 3%). Monthly spoilage cost should not exceed 2% of total food purchases. If it does, ordering quantities are too high.
- **Linen inventory**: monthly count. Par stock = 3 sets per room (one in room, one in laundry, one in reserve). Track loss rate. Annual loss above 15% needs investigation. Common causes: staining beyond recovery, damage from incorrect washing, misrouting, and theft.

### Pas 5: Labor Cost Management
- Payroll is the largest operating expense (25–35% of revenue). Managing it requires a staffing model, not guesswork.
- **Staffing model**: build a spreadsheet showing: every position, number of staff, contract type (full-time/part-time/seasonal), monthly cost per person (salary + social contributions + benefits). Total monthly payroll cost. Total payroll as % of forecasted revenue.
- **Seasonal labor flexibility**: for a property with 40% occupancy in winter and 85% in summer, the staffing level cannot be the same year-round. Build a seasonal staffing plan:
  - Core permanent staff: year-round positions (GM, head chef, head housekeeper, 1–2 front desk agents). These are your culture carriers.
  - Seasonal staff: additional housekeeping, kitchen, and service staff during high season. Hire by March for May–September. Use returning seasonal staff whenever possible (faster ramp-up, cultural consistency).
  - Flexible casual labor: for demand spikes (events, holiday weekends). Establish relationships with 3–5 reliable casual workers who can be called in with 48-hour notice.
- **Labor scheduling optimization**: schedule staff based on forecasted occupancy, not historical habit. If Tuesday forecasts 30% occupancy, you need 1 front desk agent and 2 housekeepers, not the full high-season team. Over-scheduling by 1 unnecessary staff member for 1 shift costs 80–120 RON in direct payroll — multiply by 100 unnecessary shifts per year = 8,000–12,000 RON wasted.
- **Overtime control**: overtime costs 25–100% more than regular hours (depending on Romanian labor law thresholds). If overtime consistently exceeds 5% of total labor hours, you are understaffed for the demand level — it is cheaper to hire an additional part-time employee.
- **Labor productivity metrics**: track Revenue per Employee (total revenue / FTE count) and Cost per Occupied Room for labor (total payroll / occupied room nights). Both should improve year-over-year.

### Pas 6: Energy and Utility Cost Control
- Utilities are typically the 2nd or 3rd largest operating cost category after payroll.
- **Monthly utility tracking**: record electricity (kWh), water (m3), gas/firewood (consumption units), and waste disposal cost. Calculate cost per occupied room night for each utility. Track monthly trend.
- **Quick wins — low/no investment**:
  - HVAC timers: program heating/cooling to reduce output in vacant rooms and during overnight hours. A keycard energy cutoff system (power to room only when keycard is inserted) reduces room energy consumption by 20–30%.
  - Hot water temperature: set the main boiler at 60 degrees C (legionella prevention minimum). Every 10 degrees above 60 increases energy cost by approximately 6–8%.
  - LED lighting: replace all incandescent and halogen bulbs with LED. LED uses 80% less electricity and lasts 25x longer. ROI: typically under 12 months for high-use areas.
  - Laundry optimization: run full loads only. Use cold-water wash cycles where linen standards permit. Each partial load wastes 30–40% of the water and energy of a full load.
  - Equipment standby: turn off kitchen equipment, coffee machines, and computers when not in use. Standby power consumption can reach 5–10% of a property's total electricity bill.
- **Medium investment improvements** (ROI 1–3 years):
  - Low-flow showerheads and tap aerators: reduce water consumption 30–40% per fitting. Cost: 20–60 RON per unit.
  - Motion-sensor lighting in corridors and public areas: reduces corridor lighting cost by 50–70%.
  - Programmable thermostats in each room: allows guests to control temperature within limits (20–24 degrees C range).
  - Pool cover (if applicable): reduces water evaporation by 70% and heating cost by 50%.
- **Higher investment improvements** (ROI 3–7 years):
  - Solar water heating: reduces hot water energy cost by 40–60%. Particularly effective in Romania's southern regions with 200+ sunny days per year.
  - Solar PV panels: for electricity generation. Government subsidies may be available through Casa Verde program.
  - Insulation upgrades: for older buildings, improving wall and roof insulation can reduce heating costs by 25–40%.
- **Energy audit**: if utility cost per occupied room exceeds the benchmark by more than 20%, commission a professional energy audit. Cost: 1,000–3,000 RON. The audit identifies specific waste sources and calculates ROI for each improvement. Many energy auditors offer the audit free if you contract them for the implementation.

### Pas 7: Monthly Cost Review and Variance Reporting
- By the 5th of each month, produce the **Monthly Cost Report**:
  - Format per cost category: Budget Amount | Actual Amount | Variance (RON) | Variance (%) | Explanation | Corrective Action.
  - Group by department: Rooms, F&B, Maintenance, Marketing, Administration, Utilities.
  - Include: CPOR trend (this month vs. last month vs. STLY), payroll % of revenue, food cost %, GOP%.
- **Variance investigation thresholds**:
  - Variance within 5%: acceptable, no action required unless part of a multi-month trend.
  - Variance 5–10%: explanation required from department head. May be anomalous (one-time expense) or structural (ongoing).
  - Variance above 10%: written corrective action plan required from department head with deadline. GM reviews and approves the plan.
- **Rolling 3-month trend analysis**: one bad month may be anomalous (equipment breakdown, unusual event). Three consecutive months of over-budget in the same category signifies a structural problem requiring structural correction (staffing change, supplier change, process change).
- Present the monthly cost report in the management meeting alongside the KPI dashboard (HM-010). Link costs to revenue: "F&B food cost was 36% this month, 4 points above target, which reduced F&B GOP by 12,000 RON. The root cause was over-ordering fresh produce during a week when occupancy dropped below forecast."

### Pas 8: Theft and Fraud Prevention
- Small-property theft risk areas and controls:
  - **Cash handling**: two-person witness for all cash counts and deposits (HM-011). Void register with supervisor approval. Spot cash counts (unannounced count during shift by supervisor — compare against POS running total).
  - **Inventory shrinkage**: physical counts compared against book counts. If alcohol, high-value food, or amenities show consistent shrinkage, install security cameras in storage areas (where legal) and restrict storage access to authorized personnel only.
  - **Ghost employees**: verify every person on the payroll works actual shifts. In small properties this is easily verified; in larger ones, it requires payroll-to-attendance reconciliation.
  - **Vendor kickbacks**: if one staff member consistently directs purchases to a specific supplier at above-market prices, investigate. Require competitive quotes for purchases above 2,000 RON.
  - **Complimentary abuse**: track all comps and voids. If one staff member has a significantly higher comp/void rate than peers, investigate. Set a maximum comp/void value per shift per person.
- **The best fraud prevention is visibility**: when staff know that controls are active and audited regularly, the temptation decreases significantly. The PO system, regular inventory counts, and spot cash audits create a control environment that makes theft risky and visible.

### Pas 9: Annual Cost Optimization Review
- Once per year (November, as part of the budgeting cycle per HM-013):
  - Review all supplier contracts. Are current prices competitive? Request quotes from alternative suppliers for every category above 5,000 RON annual spend.
  - Review all fixed costs: insurance (get competitive quotes every 2 years), software subscriptions (are all subscriptions still used?), maintenance contracts (is the contracted service still needed at the contracted level?).
  - Identify the top 5 cost reduction opportunities based on the year's variance analysis. For each: quantify the potential saving, identify the action required, and assign a responsible person with a deadline.
  - Review the CAPEX plan: are there equipment replacements that would reduce operating costs (e.g., replacing an old inefficient boiler, upgrading to energy-efficient appliances)? Calculate the payback period.
  - Benchmark CPOR against industry averages and against your prior year. Are you improving?

## Verificare
- [ ] All cost categories mapped to USALI structure with department allocation
- [ ] CPOR baseline calculated for every variable cost category
- [ ] PO system implemented with defined approval tiers and enforced for all purchases above 200 RON
- [ ] Preferred supplier contracts in place for top 5 categories by spend volume
- [ ] Monthly physical stockcount completed for housekeeping and F&B inventory
- [ ] Food waste tracked daily with monthly cost analysis — spoilage under 2% of food cost
- [ ] Staffing model built with seasonal adjustments — payroll at 25–35% of revenue
- [ ] Utility cost per occupied room tracked monthly — trend improving or stable
- [ ] Monthly cost report vs. budget completed within 5 days of month close — 10%+ variances have corrective action plans
- [ ] Annual supplier review completed by November with competitive quotes obtained

## Instrumente
- Purchase Order Form (Google Docs template — printed or digital with approval signature)
- Purchasing Log (Google Sheets — all POs tracked with date, amount, supplier, budget line, approver)
- Monthly Cost Report template (Google Sheets — budget vs. actual with variance calculation)
- F&B Inventory Count Sheets (separate templates for kitchen, bar, and housekeeping)
- Food Waste Log (daily, by category: prep/plate/spoilage/overproduction)
- Recipe Cards with costed ingredients (per menu item)
- Utility Meter Readings Log (monthly readings for electricity, water, gas)
- Staffing Model spreadsheet (positions x rates x hours x seasons)
- Accounting software (Saga, QuickBooks, or dedicated hotel accounting — for P&L and cost tracking)
- Safe Log and Void Register (for cash and transaction controls)

## Note
- **Cost control is NOT cost cutting.** Cutting the wrong costs (guest amenities, maintenance, training) destroys revenue and creates a downward spiral. Focus on eliminating waste, not reducing quality. A guest who finds cheap toilet paper and no welcome amenity will not return — the 5 RON "saved" costs 10,000 RON in lifetime guest value.
- **The PO system resistance**: staff will resist the PO system initially ("it slows things down"). This is expected. Enforce it consistently for 3 months. After that, it becomes habit and the benefit (budget adherence, fraud prevention, supplier negotiation leverage) is obvious.
- **Common mistake — ignoring small leaks**: a 2 RON overcharge on a daily cleaning product order seems trivial. Over 365 days across 3 products, it is 2,190 RON — enough to fund a meaningful guest experience improvement.
- **Albastru / Sapte Focuri context**: energy costs are particularly relevant for the open-fire kitchen. Wood/biomass fuel is a significant cost (estimate: 1,000–3,000 RON/month during active cooking seasons). Track wood consumption per service session (kg of wood per dinner service). Optimize fire management techniques: use seasoned hardwood (beech, oak — burns hotter and longer, reducing total consumption), manage airflow to control burn rate, batch-cook items that share the same fire temperature, and extinguish fires properly after service to prevent unnecessary burn-through. The fire is the differentiator — the goal is not to reduce its use but to eliminate waste in its use. Compare fuel cost per cover (total wood cost / covers served) monthly. For the rural location, negotiate directly with local forestry suppliers for annual wood supply contracts — buying in bulk during summer/autumn for the full year is significantly cheaper than spot-purchasing during winter.
