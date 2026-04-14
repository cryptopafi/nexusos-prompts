---
id: HM-011
title: Night Audit Procedure
domain: hotel-management
source: Hotel Management 360 (Tourism & Hotel Academy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# Night Audit Procedure

## Obiectiv
Close the business day cleanly by ensuring all revenue is posted, all folios are balanced, all room statuses are verified, and all management reports are generated — before the new business day begins at midnight. The night audit is the hotel's most important daily financial control process, preventing revenue leakage that typically runs 1–3% of total revenue in hotels without a rigorous audit process.

## Pași

### Pas 1: Pre-Audit Preparation (21:00–22:00)
- Print the **Outstanding Balance Report**: identify all guest folios where the current balance exceeds the pre-authorized credit card amount. Contact guests with expired or insufficient pre-authorizations before midnight — once room charges post, an unauthorized folio becomes a collection risk.
- Print the **No-Show Report**: identify all reservations for today where the guest has not arrived by 21:00. Cross-reference with any late arrival notifications (some guests inform via Booking.com messaging or phone). For confirmed no-shows:
  - Apply the no-show charge per the booking policy (typically 1 night's room charge). Verify the booking terms clearly stated the no-show policy — if not, you may not be able to charge.
  - Release the room back into inventory for potential walk-in or last-minute OTA sale.
  - Send a no-show notification email to the guest (automated via PMS if available).
  - For OTA bookings: mark as no-show in the OTA extranet. Booking.com requires no-show reporting within 48 hours.
- Print the **Expected Departures Report**: identify guests expected to check out tomorrow. Flag any guest with an outstanding balance that exceeds the pre-authorization. Contact these guests before midnight if possible ("Mr. Ionescu, I noticed your incidental charges have exceeded the initial authorization — may I run an additional authorization on your card?").
- Verify all POS systems are closed and totals transferred to PMS:
  - Restaurant POS: close each meal service (lunch, dinner). Run the settlement report. Verify that room charges posted from the restaurant match the POS records.
  - Bar POS: close the bar tab. Transfer any room charges.
  - Room service POS (if separate): close and reconcile.
  - Spa/retail POS: close and reconcile.
  - Every charge from every revenue center must be posted to the correct guest folio in PMS before the night audit runs. An unposted charge is lost revenue.

### Pas 2: Housekeeping Discrepancy Resolution (22:00–22:30)
- Print the **Housekeeping Discrepancy Report**: this compares PMS room status (what the system thinks) against housekeeping's physical report (what is actually true).
- Common discrepancies:
  - **"Occupied" in PMS but "Vacant" per housekeeping** (sleeper): the guest may have checked out without notifying front desk, or the room may have been assigned incorrectly. Physically verify the room — check for luggage, personal items. If empty, correct the status and investigate the checkout gap.
  - **"Vacant" in PMS but "Occupied" per housekeeping** (skipper): someone may be in the room without a registered reservation. Physically verify. If a person is present, the Duty Manager must handle — this may be an unregistered guest of an existing guest, a lost key entry into the wrong room, or an unauthorized occupant.
  - **"Vacant Dirty" that should be "Vacant Clean"**: housekeeping cleaned but did not update the system. Update status and remind housekeeping of the procedure.
- **Every discrepancy must be resolved before the night audit runs.** Zero unverified rooms is the standard. Running the audit with discrepancies means posting room charges to potentially incorrect folios.
- After resolution, the final room count should match: Occupied Rooms (per PMS) + Vacant Rooms + OOO Rooms = Total Rooms in Property.

### Pas 3: Post Room Revenue — The Core Audit Transaction (23:00–23:30)
- This is the central purpose of the night audit: posting one night's room charge to every occupied room folio.
- In PMS, run the **Night Audit / Auto Post** function. This function:
  - Posts one night's room revenue to each occupied room at the rate assigned to that reservation.
  - Posts applicable taxes: VAT (TVA in Romania, currently 9% for accommodation services), local tourism tax (taxa de sejur — amount varies by locality, typically 1–2% of room rate or a fixed amount per person per night).
  - Posts package inclusions as credits (e.g., if breakfast is included in the rate, a breakfast credit is posted to offset the F&B charge).
  - Advances the PMS business date from today to tomorrow (the "day close"). After this step, the system operates on the new business date.
- **Pre-posting verification**: before pressing the audit button, run a Pre-Audit Summary report that shows what will be posted. Check:
  - Total room revenue to be posted = sum of all occupied room rates. Calculate manually: count occupied rooms by rate and multiply. The sum should match the Pre-Audit Summary.
  - Total tax to be posted = Total Room Revenue x Tax Rate. Verify.
  - Any rooms with zero rate (complimentary rooms, house use) — are they authorized? Complimentary rooms must have written authorization from the GM.
  - Any rooms with an unusually high or low rate — rate entry errors are caught here.
- **Post-posting verification**: after the audit runs, review the posting confirmation:
  - Total room revenue posted vs. expected total. Variance = 0 RON is the target. Any variance requires investigation.
  - Total tax posted vs. expected. If tax is wrong, the PMS tax configuration may be incorrect.
  - Count of rooms posted vs. occupied room count. If 32 rooms are occupied but only 31 charges posted, one room was missed — identify and manually post.
- This step is irreversible in most PMS systems. Once the night audit runs, the business date advances. Errors discovered after the audit require manual correction entries in the new business day.

### Pas 4: Balance All Revenue Centers (23:30–00:30)
- Run the **Revenue Center Summary**: a report showing total revenue by department for the day.
- For each revenue center, reconcile three things:
  - **POS revenue total vs. PMS postings**: every charge that went through POS should have a corresponding posting on a guest folio (room charge) or been settled at the POS (cash or card payment by non-guest). The two totals must match. Common discrepancy: a restaurant server rang a charge in POS but forgot to post it to the guest folio. The POS shows revenue; the PMS folio does not.
  - **Cash received vs. physical cash count**: count all cash on hand from each revenue center. Compare against the POS cash settlement report. Variance of more than 10 RON requires investigation. Count every denomination — do not estimate.
  - **Card transactions vs. payment gateway totals**: the number and total of card transactions in POS should match the card terminal batch settlement report. If the terminal is configured for auto-batch settlement (typically at midnight or end of shift), verify before the batch closes.
  - **Vouchers and complimentary items**: every void, comp, or discount must have a documented authorization. List all voids and comps in the Night Audit Discrepancy Report with the authorizing person's name.
- **Variance thresholds**:
  - Variance below 10 RON: note in the report but do not escalate. Rounding and coin-counting discrepancies are normal.
  - Variance 10–50 RON: investigate immediately. Check for unposted charges, miscounted cash, or voided transactions. If unresolvable, document the amount and escalate to the Front Office Manager at 07:00.
  - Variance above 50 RON: do NOT close the audit without attempting resolution. Re-count cash, re-run POS reports, check all void/comp records. If still unresolvable, document with full detail and notify the FOM and GM. A persistent pattern of variances above 50 RON signals potential fraud.

### Pas 5: Generate End-of-Day Reports (00:30–01:00)
- **Daily Revenue Report (DBR)**: the most critical daily management report.
  - Content: total room revenue, F&B revenue (by outlet), other revenue, total revenue, occupancy%, ADR, RevPAR, TRevPAR. Comparison: vs. budget, vs. forecast, vs. STLY.
  - Distribution: email to GM and Revenue Manager by 07:00. Update the KPI Dashboard Tab 1 (HM-010).
  - Format: use a standardized template so that the report looks identical every day — managers should be able to scan it in 60 seconds.
- **Arrivals Report (tomorrow)**: all expected arrivals for the next day. Columns: guest name, arrival time (if known), room type, room assigned (if pre-assigned), rate, channel, payment status, special requests, VIP flag. Print and place at the front desk for the morning shift.
- **Departures Report (today/tomorrow)**: all expected checkouts. Highlight: folios with outstanding balances, late checkout requests, express checkout instructions.
- **In-House Guest Report**: full list of currently occupied rooms. Columns: room number, guest name, arrival date, departure date, rate, folio balance. Used for: housekeeping assignment, emergency contact, and occupancy verification.
- **No-Show Report**: finalized list of no-shows with charges applied/not applied and reasons.
- **Complimentary / House Use Report**: all rooms used by staff, management, owners, or provided free of charge. Each must have documented authorization. This report is reviewed monthly to prevent unauthorized use.
- **High Balance Report**: folios where the current balance exceeds a defined threshold (e.g., 2,000 RON). These require attention from the morning shift to obtain additional authorization or arrange interim payment.

### Pas 6: Cash Drop and Safe Procedure (01:00–01:15)
- **Cash count protocol**: count all cash from all departments (front desk, restaurant, bar). Use a Cash Balance Sheet:
  - Opening float amount (set at shift start).
  - Plus: cash received during the shift (per POS and manual receipt records).
  - Minus: cash paid out during the shift (petty cash disbursements, cash refunds).
  - Equals: expected closing cash on hand.
  - Actual cash count: count every denomination and record on the sheet.
  - Variance: expected minus actual. If variance exceeds 10 RON, investigate before completing the drop.
- **Two-person witness rule**: the cash count and safe deposit must be witnessed by a second staff member. Both sign the Cash Balance Sheet. This is a non-negotiable financial control — solo cash handling creates both fraud risk and false accusation risk.
- **Prepare the deposit envelope**: all cash above the designated float amount goes into the deposit envelope. Seal the envelope, write the amount on the outside, date it, and sign it.
- **Safe deposit**: place the sealed envelope in the hotel safe. Record in the Safe Log: date, time, amount, deposited by (name + signature), witnessed by (name + signature).
- **Float reset**: the float for the next shift should be exactly the designated float amount. If the float is short (less than the designated amount because cash receipts were lower than expected), note the shortfall and arrange replenishment from the safe the next morning.

### Pas 7: Security and Property Check (01:30–03:00)
- The night auditor is often the senior (or only) staff member on the property overnight. Security responsibilities:
  - **Property walkthrough**: walk the property perimeter. Check that all exterior doors and gates are locked. Verify emergency exits are unobstructed (fire safety requirement). Check the pool area is secured (if applicable — drowning risk). Check parking area for any unusual activity.
  - **Fire safety check**: verify that fire alarm panel shows no faults. Confirm fire extinguishers are in their designated positions and not expired. If the property has a sprinkler system, verify the system status indicator is normal.
  - **Guest safety**: check corridors for propped-open doors, unusual noise, or any signs of disturbance. If a noise complaint was logged earlier in the evening, verify the situation is resolved.
  - **IT and systems check**: verify PMS is operational, internet connectivity is stable, CCTV (if installed) is recording, and the booking engine is online (check the property website on your phone).
- **Log the walkthrough time and any observations** in the Security Log. A documented walkthrough protects the property legally if any incident occurs overnight.

### Pas 8: Handover Preparation (05:00–06:30)
- Complete the **Night Audit Handover Log** — the critical communication link between night and morning operations:
  - Summary of the night: occupancy, revenue highlights, any unusual events.
  - Unresolved discrepancies: amounts, references, and suggested resolution.
  - Outstanding maintenance requests logged overnight.
  - Late arrivals processed during the night (guest name, room, any special circumstances).
  - Guests expecting very early checkout (before 07:00) — folios should be pre-prepared.
  - VIP arrivals or special requests for the arriving day.
  - Weather conditions or local events relevant to the morning team.
  - Any security observations from the property walkthrough.
- Sign the handover log. Leave it at the front desk for the morning supervisor.
- **File all physical reports** in the Night Audit Binder:
  - Daily Revenue Report
  - Cash Balance Sheet (with both signatures)
  - Revenue Center Summary
  - Discrepancy Report (if any)
  - All POS settlement reports
  - No-Show Report
  - Maintain a 30-day rolling archive minimum at the front desk. Archive older reports in the administrative office for 12 months minimum (financial audit trail requirement).

### Pas 9: Month-End Night Audit Procedures (last day of each month)
- On the last night of each calendar month, additional procedures:
  - Run the **Monthly Revenue Summary**: total revenue by department, by channel, by segment for the full month. Compare to budget.
  - Run the **Monthly Occupancy Report**: daily occupancy for every day of the month, with ADR and RevPAR. This feeds the KPI dashboard (HM-010) and variance analysis (HM-004).
  - Run the **Accounts Receivable Aging Report**: all outstanding invoices (corporate accounts, group deposits) with aging buckets (0–30 days, 31–60 days, 61–90 days, 90+ days). Flag accounts in the 60+ day bucket for collection follow-up.
  - Run the **Commission Report**: total OTA commissions owed for the month. Verify against OTA extranet invoices when received.
  - Verify **room inventory**: confirm total room count in PMS matches physical rooms. Check that OOO rooms have valid reasons and expected return dates.
- Forward the month-end reports to the accountant/finance team and GM by 08:00 on the 1st.

### Pas 10: Night Audit Quality Assurance
- **Common night audit errors and how to prevent them**:
  - **Posting to wrong folio**: occurs when room numbers are transposed (room 12 charge posted to room 21). Prevention: verify room number on POS docket matches PMS folio before posting. Run the "folio review" report after posting to catch mismatches.
  - **Missing revenue posts**: a POS charge exists but was never posted to a folio. Prevention: reconcile POS totals against PMS postings per revenue center (Pas 4).
  - **Incorrect rate posting**: a room was booked at 300 RON but the PMS has 350 RON loaded. Prevention: pre-audit check of room rates against reservations. Flag any rate that differs from the booking confirmation.
  - **No-show charge applied to wrong booking**: if two guests with similar names had reservations, the wrong one may be charged. Prevention: verify booking ID, not just guest name, before applying no-show charges.
  - **Cash variance ignored**: a 25 RON variance "because it's late and I'm tired" is how cash handling discipline erodes. Prevention: enforce the investigation threshold regardless of time.
- **Night audit training**: every night auditor should complete a supervised night audit for a minimum of 5 shifts before performing solo. Provide a printed Night Audit Checklist (this procedure condensed to a single-page checklist) that is followed step-by-step every night.
- **Spot audit**: the FOM should randomly attend 1 night audit per month (unannounced) to verify procedure compliance. Review the Night Audit Binder weekly for completeness and accuracy.

## Verificare
- [ ] All POS revenue centers closed and totals transferred to PMS before audit run
- [ ] Housekeeping discrepancy report resolved — zero unverified rooms before posting
- [ ] Room revenue posting completed with zero variance between expected and actual
- [ ] All revenue centers balanced — variances above 50 RON documented and escalated
- [ ] No-show charges applied correctly to all confirmed no-shows
- [ ] Daily Revenue Report generated, formatted, and distributed to GM/Revenue Manager by 07:00
- [ ] Cash drop completed: counted, balanced, witnessed, sealed, deposited, logged in Safe Log
- [ ] Night Audit Handover Log completed, signed, and placed at front desk
- [ ] Security walkthrough completed and logged with time and observations
- [ ] All physical reports filed in Night Audit Binder (30-day rolling minimum)
- [ ] Month-end reports generated and forwarded on the last night of each month

## Instrumente
- PMS Night Audit module (Cloudbeds, Mews, Opera Cloud, or equivalent)
- Cash Balance Sheet template (printed, with denomination count grid)
- Safe Log (bound logbook — not loose sheets, to prevent page removal)
- Night Audit Binder (physical, sequential filing, 30-day minimum at front desk)
- Night Audit Discrepancy Report template (for variances above threshold)
- Security Log (walkthrough record with time, observations, and signature)
- Night Audit Checklist (single-page condensed version of this procedure — laminated, at the audit station)
- POS settlement reports (from each revenue center terminal)
- Calculator and coin tray (for cash counting accuracy)

## Note
- **The night audit is the hotel's most important daily financial control process.** A poorly run night audit creates: revenue leakage (unposted charges), incorrect guest folios (billing disputes at checkout), financial reporting errors (budget vs. actual inaccuracies), compliance risks (tax posting errors), and security gaps (no overnight monitoring). Invest in training the night auditor properly — it pays for itself every night.
- **If no PMS**: run a manual night audit using a spreadsheet. Post room charges manually per occupied room (rate x 1 night), add taxes, balance totals against the booking list, file all records. The manual process is slower (add 60 minutes) but the financial control is equally important.
- **Edge case — PMS crashes during night audit**: most cloud-based PMS systems have 99.5%+ uptime, but outages occur. If the PMS is unavailable: (1) do NOT wait — perform the manual components (cash count, POS reconciliation, security walkthrough, handover log). (2) Run the system audit as soon as the PMS comes back online — most systems can process a delayed audit. (3) Note the outage in the handover log with the time and duration.
- **Edge case — fire alarm during night audit**: the night auditor is the fire safety coordinator overnight. Follow the fire evacuation procedure: (1) verify if the alarm is genuine (check the fire panel for zone), (2) if genuine, evacuate all guests per the evacuation plan, (3) call 112, (4) account for all guests using the In-House Guest Report, (5) do not re-enter until fire service gives clearance. The night audit can wait — guest safety cannot.
- **Albastru / Sapte Focuri context**: at a small property, the night audit may be performed by the same person who manages the late shift or even by the property owner. The full PMS audit may not be available — but the Cash Balance Sheet, Safe Log, and Handover Log are the minimum required financial controls. These three documents, completed consistently every night, prevent the two most common financial problems in small hotels: cash shrinkage and unposted revenue. If the property operates without an overnight staff member, the night audit can be performed at the end of the evening shift (22:00–23:00) with the PMS day close set to auto-post at midnight. The morning team then verifies the posting results at shift start.
