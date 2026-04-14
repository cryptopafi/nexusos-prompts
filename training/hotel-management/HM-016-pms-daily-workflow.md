---
id: HM-016
title: PMS (Property Management System) Daily Workflow
domain: hotel-management
source: Hotel Management 360 (Tourism & Hotel Academy)
version: 2.0
forge_score: "estimated H/3.5"
business_mapping: ["albastru"]
---

# PMS (Property Management System) Daily Workflow

## Obiectiv
Standardize the daily use of the Property Management System so that all departments use it as the single source of truth — room status is always current (within 15 minutes of any change), revenue is accurately captured on the correct folio within 2 hours of the charge, and the system generates reliable reporting for pricing, forecasting, and financial management decisions.

## Pași

### Pas 1: Morning Start-of-Day Routine (07:00–08:00)
- **Arrivals Dashboard**: open the arrivals view for today.
  - Filter and prioritize: unassigned rooms (assign before 12:00), pre-paid vs. pay-on-arrival (ensures correct billing workflow at check-in), VIP flags and special requests (coordinate with housekeeping by 08:30), repeat guests (review stay history — note preferences, last room type, any complaints, preferred language).
  - For each unassigned arrival, pre-assign a room following the ABCD rule (HM-001 Pas 2). Update the assignment in PMS.
  - Cross-reference arrivals with the forecast (HM-004): is the actual arrival count aligned with the forecast? If significantly more arrivals than expected, alert housekeeping to accelerate room preparation. If significantly fewer, investigate (cancellations not processed? OTA sync failure?).
- **Departures Dashboard**: check all expected checkouts for today and tomorrow.
  - Flag folios with a balance exceeding the pre-authorized amount — resolve before the guest approaches the desk.
  - Identify late checkout requests — confirm room availability (a late checkout on a room needed for a same-day arrival creates a conflict).
  - Prepare express checkout folios: print and slip under the door for guests who requested express checkout, or send by email if the PMS supports it.
- **Room Status Board**: open the housekeeping module and sync with the housekeeping supervisor.
  - Verify: rooms marked "Vacant Clean" (VC) are truly ready for assignment. If a VC room was not verified by supervisor inspection (HM-002), it should not be assigned to an arrival.
  - OOO rooms: review reason and expected return date. If any OOO room has been out for more than 7 days without a clear maintenance timeline, escalate to the maintenance lead.
  - Do not assign a room until it shows VC status confirmed by the housekeeping supervisor. Assigning a Vacant Dirty room to an arrival is a recipe for the worst possible first impression.
- **OTA Reservation Sync**: verify all new OTA reservations received overnight have been imported correctly into PMS.
  - Check: rate matches OTA booking confirmation, room type is correct (standard vs. superior vs. deluxe), guest name and contact details are present, special requests are noted, channel source is tagged correctly (for segment reporting).
  - Common sync issues: room type mapping error (OTA "Standard Double" mapped to wrong PMS room type), rate plan mismatch (OTA special promotion rate not configured in PMS), duplicate reservation (guest booked twice on different platforms). Resolve immediately — unresolved sync issues lead to overbooking, wrong billing, or missed availability.
  - If using a channel manager (Cloudbeds, SiteMinder, Beds24): the sync should be automatic. Run a daily spot check on 3 random OTA reservations to verify accuracy. If errors are frequent, contact the channel manager support to debug the mapping.

### Pas 2: Reservation Management — The Revenue Pipeline
- **New direct reservations**: create in PMS with complete information:
  - Guest details: full name, phone, email, nationality, ID/passport number (required by Romanian law for check-in registration).
  - Stay details: room type, dates, rate plan, rate amount (confirm against the current rate tier per HM-005).
  - Payment: credit card for guarantee, pre-authorization if applicable, payment terms for corporate (invoiced, net 30).
  - Channel source: tag as "Direct-Website," "Direct-Phone," "Direct-Email," or "Direct-Walk-in" for accurate channel reporting (HM-007).
  - Special requests: early check-in, late checkout, dietary needs, occasion (birthday, anniversary), bed configuration, floor preference.
  - Confirm the reservation by email immediately: send a booking confirmation with all details, cancellation policy, check-in time, directions, and a personal touch ("We're looking forward to welcoming you!").
- **Modifications**: any change to a reservation (dates, room type, rate, guest count) must be reflected in PMS immediately. If a modification is requested by phone, confirm the change in writing (email) before processing. Log the modification with the original booking details for audit trail.
- **Cancellations**: process in PMS with the correct cancellation reason code (guest request, no availability, duplicate booking, medical/emergency). Apply the cancellation fee per the booking policy. Send a cancellation confirmation email. Release the room back into available inventory immediately — delayed release means the room sits unsold while demand may exist.
- **Group bookings**: create a group master reservation in PMS. Allocate the room block. Track group pickup daily for the 14 days before arrival:
  - Pickup = rooms booked from the block / total rooms allocated. If pickup is below 70% at 14 days before arrival, contact the group organizer to confirm the final count. Release unbooked rooms from the block by the agreed release date (typically 7–14 days before arrival) so they can be sold individually.
  - Group billing: set up the billing structure upfront — master folio for room charges, individual folios for incidentals, or split as agreed with the group organizer.
- **Waitlist management**: if a requested room type is sold out, offer alternatives first. If the guest wants only the specific room type, place them on a waitlist in PMS. Set up a notification trigger: when a cancellation creates availability, the PMS should flag waitlisted guests for that room type. Contact the waitlisted guest within 1 hour of availability.

### Pas 3: Housekeeping Module — Real-Time Room Status
- The PMS housekeeping module is the bridge between front office and housekeeping. Both departments must update it in real time.
- **Status transitions and who updates**:
  - Occupied → Vacant Dirty (VD): front desk updates at checkout (within 2 minutes of the guest departing).
  - VD → Vacant Clean (VC): housekeeping supervisor updates after room is cleaned AND inspected. The supervisor — not the cleaning attendant — changes the status. This enforces the inspection step.
  - VC → Occupied: front desk updates at check-in (within 2 minutes of issuing the key).
  - Any room → Out of Order (OOO): maintenance or supervisor updates. Must include: reason for OOO, expected return-to-service date, and who authorized. OOO rooms are excluded from available inventory in real-time, which directly impacts availability on OTA channels.
  - Room moves: if a guest changes rooms, update immediately. Old room reverts to VD. New room transitions to Occupied. Update the folio to reflect the new room number — charges must post to the correct room.
- **Critical timing**: the delay between a physical status change and the PMS update should never exceed 15 minutes. A room that checks out at 11:00 but is not changed to VD until 11:30 means housekeeping lost 30 minutes of turnaround time. In a property with same-day arrivals, every minute of delay compounds.
- **Mobile access**: if the PMS supports mobile access (Cloudbeds and Mews do), equip the housekeeping supervisor with a tablet or phone to update room status from the floor in real time. This eliminates the "walk to the front desk to report status changes" bottleneck.
- **Housekeeping report**: print from PMS at 14:00 to confirm all departures have been cleaned and all arrivals are in VC status. Any room still showing VD at 14:30 with a same-day arrival is an escalation — supervisor personally verifies status.

### Pas 4: Folio and Charge Management — Zero Revenue Leakage
- **Same-day posting rule**: all charges to guest folios must be posted in PMS on the same day they are incurred. This is the most critical revenue integrity rule.
  - Room charges: automated via night audit (HM-011). Verify nightly.
  - F&B charges: transferred from POS at end of each meal service. If POS and PMS are integrated, this happens automatically. If not, the F&B team manually posts room charges using signed charge slips. Deadline: within 1 hour of service closing.
  - Minibar: housekeeping submits consumption slips (HM-002 Pas 5). Front desk posts to folio by 15:00 daily.
  - Incidentals: telephone charges (if billed), laundry, parking, spa — posted same day by the relevant department.
  - Late charges: any charge discovered after the guest has checked out (most commonly a minibar charge discovered during housekeeping after checkout). Post to the guest's closed folio and either charge the card on file or send an invoice. Every late charge over 50 RON should trigger a process review — why was this charge not posted before checkout?
- **Folio review protocol**: front desk agents review all open folios once per shift:
  - Look for: charges on the wrong folio (room number transposition — room 12 charge on room 21), duplicate charges, missing expected charges (guest dined in the restaurant but no F&B charge on folio), unauthorized discounts or adjustments.
  - For stays of 3+ nights: print a mid-stay folio and review with the guest proactively ("Mr. Ionescu, here's a summary of your charges so far — would you like to review them?"). This prevents checkout surprises and disputes.
- **Corrections and adjustments**: all folio corrections require:
  - Supervisor authorization (signature or digital approval in PMS).
  - A documented reason (in the PMS correction log or a separate register).
  - The original charge and the correction visible in the folio history (never delete — always void and re-post).
  - The FOM reviews the daily correction log every morning. An abnormal correction rate (>2% of transactions) signals either operational errors or potential fraud.

### Pas 5: Revenue Reports and Analytics — Decision Support
- Daily reports to generate from PMS:
  - **Daily Revenue Summary**: occupancy%, rooms sold, ADR, RevPAR, TRevPAR, total revenue by department. Distribution: GM, Revenue Manager, KPI Dashboard Tab 1 (HM-010). Generated as part of the night audit (HM-011).
  - **Pickup Report**: reservations made today for future dates — shows booking momentum. Key metric: net pickup = new bookings minus cancellations for each future date. Positive net pickup = demand is materializing. Negative net pickup (more cancellations than new bookings) = demand concern.
  - **On-the-Books (OTB) Report**: total rooms booked for each date in the next 90 days. Cross-reference with the forecast (HM-004) and pricing triggers (HM-005).
- Weekly reports:
  - **Channel Production Report**: reservations by source (Booking.com, Airbnb, Direct-Website, Direct-Phone, Walk-in, Corporate, Group). Use this for the weekly channel review (HM-007).
  - **Rate Plan Performance**: revenue and volume by rate plan. Identifies which rate plans are selling and which are dormant (dormant rate plans should be deactivated to reduce complexity).
  - **Guest Profile Report**: new profiles created, duplicates detected, profiles with missing information (email, nationality). Data quality maintenance.
- Monthly reports:
  - **Segment Report**: revenue and volume by market segment (FIT, Corporate, Group, Retreat, Local — per HM-006 tags). Feeds the segmentation review.
  - **Revenue by Room Type**: ADR and occupancy by room type. Identifies which room types are most/least popular and at which prices. Informs room type pricing spread decisions (HM-005 Pas 6).
  - **Length of Stay Distribution**: average LOS by segment, by season. Informs MinLOS policy decisions (HM-020).
  - **Cancellation Report**: cancellation rate by channel, by booking window. High cancellation channels (>25%) need attention — either the policy is too lenient or the demand is not genuine.
- **Report filing**: save all reports in a consistent folder structure: /Reports/[Year]/[Month]/[Report Type]. This creates an accessible historical archive for forecasting and year-over-year comparison.

### Pas 6: Guest Profile Management — The Memory of the Property
- The PMS guest profile is the institutional memory of every guest interaction. A well-maintained profile enables personalization that creates loyalty.
- **Profile creation**: always search for an existing profile before creating a new one. Search by: email (most reliable unique identifier), phone number, and name. Creating duplicate profiles is the #1 data quality issue in PMS systems.
- **Profile enrichment**: after every stay, enrich the profile with:
  - Room preference (room type, floor, bed configuration, view preference).
  - Dietary needs and allergies.
  - Occasions (anniversary date, birthday — for future stay personalization).
  - F&B preferences (favorite wine, preferred table, cooking class participant).
  - Complaint history (critical — the next check-in agent must know if this guest had a problem last time).
  - Communication preference (email, phone, WhatsApp).
  - Channel history (how did they book each stay?).
- **Duplicate profile merge**: run a duplicate search quarterly. Most PMS systems have a merge function. When merging, ensure stay history, preferences, and notes from both profiles are preserved in the surviving profile.
- **Profile privacy**: guest profiles contain personal data subject to GDPR. Access should be restricted to staff who need it (front desk, reservations, GM). Do not share guest contact information with third parties without explicit consent. Respond to data access/deletion requests within 30 days per GDPR requirements.

### Pas 7: PMS Data Hygiene and System Maintenance
- **Rate plan audit** (monthly): verify all active rate plans in PMS match the current pricing strategy (HM-005). Deactivate obsolete rate plans. A rate plan that was created for a promotion 6 months ago and never deactivated can accidentally be selected by a new agent, selling rooms at the wrong price.
- **Room type audit** (quarterly): verify room types, names, descriptions, and photos in PMS match the OTA listings and the direct website. A mismatch creates guest expectations that do not match reality.
- **OTA channel mapping verification** (monthly): confirm that PMS room types are correctly mapped to OTA room categories in the channel manager. Mapping errors cause: wrong room type sold on OTA, inventory shown as unavailable when rooms exist, or rate displayed incorrectly.
- **PMS backup and recovery**: for cloud-based PMS (Cloudbeds, Mews, Opera Cloud): verify the provider's backup policy (should be daily, with point-in-time recovery). For on-premise PMS: set up daily local backup to an external drive, verify the backup monthly by test-restoring one day's data.
- **User access management**:
  - Quarterly review: list all PMS user accounts. Deactivate accounts for departed staff on their last working day — same day, no delay. An active account for a departed employee is both a security risk and a compliance issue.
  - Role-based access: front desk agents get reservation and check-in/out access. Housekeeping gets room status access only. F&B gets folio posting access for their outlets only. Revenue reporting access: GM and Revenue Manager only. Rate management access: GM and Revenue Manager only.
  - Password policy: change passwords every 90 days. No shared accounts — each person has their own login for audit trail purposes.
- **PMS updates and training**: when the PMS provider releases updates, review the change log. Test new features in a test environment if available. If the update changes a core workflow (check-in process, rate management), schedule a 15-minute training refresher for affected staff before the update goes live.

### Pas 8: PMS Selection Guide (for properties choosing a system)
- If no PMS is currently in use, select one based on these criteria:
  - **Cloud-based**: mandatory. Cloud PMS provides access from any device, automatic updates, daily backup, and remote access (critical for properties where the owner is not always on-site).
  - **Channel manager integration**: must connect directly to Booking.com and Airbnb at minimum, with real-time 2-way sync (rate and availability updates within 5 minutes).
  - **Booking engine**: should include or integrate with a direct booking engine for the property website.
  - **Mobile access**: supervisors and managers should be able to check status and reports from a phone.
  - **Reporting**: must support daily revenue reports, OTB reports, channel production reports, and guest profile management.
  - **Ease of use**: the system will be used by front desk agents, housekeeping, and the GM. Complex systems that require weeks of training are not suitable for small properties.
  - **Cost**: for a 6–15 room property, budget 50–200 EUR/month. More than 200 EUR/month is not justified at this property size.
- **Recommended PMS for boutique properties in Romania**:
  - **Cloudbeds**: integrated PMS + booking engine + channel manager. Strong for 10–50 room properties. Good Booking.com and Airbnb integration. Cloud-based. Cost: 100–200 EUR/month.
  - **Little Hotelier** (by SiteMinder): designed specifically for small properties (<30 rooms). Simple interface. Built-in channel manager and booking engine. Cost: 80–150 EUR/month.
  - **Mews**: modern, automation-focused. Strong API integrations. Better for tech-forward properties with 20+ rooms. Cost: 150–300 EUR/month.
  - **Beds24**: budget option with good channel manager integration. Less polished interface but functional for very small properties. Cost: 30–80 EUR/month.
- **Migration from spreadsheets to PMS**: allocate 2 weeks for setup (room configuration, rate plans, channel mapping) and 1 week for staff training. Run the spreadsheet and PMS in parallel for the first month to catch any discrepancies.

## Verificare
- [ ] Arrivals pre-assigned and reviewed in PMS every morning before 10:00
- [ ] All OTA reservations syncing correctly into PMS — spot-check 3 reservations weekly
- [ ] Room status in PMS matches physical room status — verified minimum twice daily (11:00 and 15:00)
- [ ] All F&B and incidental charges posted to correct folios by end of each service period — zero late charges above 50 RON
- [ ] Daily Revenue Summary generated and distributed (part of night audit HM-011)
- [ ] Pickup Report reviewed daily as part of pricing review (HM-005)
- [ ] Guest profiles searched before creation — monthly duplicate merge
- [ ] PMS user access list reviewed quarterly — zero departed staff with active accounts
- [ ] Rate plan audit completed monthly — zero obsolete active plans
- [ ] Channel mapping verified monthly — zero mismatch incidents

## Instrumente
- PMS (Cloudbeds, Mews, Little Hotelier, Beds24, or Opera Cloud)
- Channel Manager (integrated with PMS or standalone: SiteMinder, eZee Centrix)
- POS system (integrated with PMS for automatic F&B charge posting, or manual posting workflow)
- PMS reporting module (or export to Google Sheets for custom analysis)
- Tablet/phone for housekeeping mobile access to PMS housekeeping module
- PMS Workflow Checklist (1-page laminated — morning routine, shift-change routine, end-of-day routine)

## Note
- **The PMS is only as accurate as the data entered.** One missed room status update leads to a dirty room assigned to a VIP. One unposted F&B charge leads to a billing dispute at checkout. One unmapped rate plan leads to rooms sold at the wrong price on Booking.com. Enforce real-time data discipline as a non-negotiable standard.
- **Common mistake — using PMS as a typewriter**: entering data but never using the reports. A PMS that only records check-ins and check-outs is a glorified notebook. The value is in the reporting: OTB reports drive pricing, channel reports drive distribution strategy, guest profiles drive personalization. Use the data.
- **Albastru / Sapte Focuri context**: if no dedicated PMS is in use yet, a structured Google Sheets room management system is a viable interim solution. Columns: Room Number | Status (VC/VD/OCC/OOO) | Guest Name | Check-in Date | Check-out Date | Room Type | Rate | Channel | Folio Balance | Special Requests | Notes. Add a second sheet for the reservation calendar (rows = rooms, columns = dates, cells = guest names). This manual system works for up to 8–10 rooms at up to 60% occupancy. Above that, the error rate and management time make a PMS transition essential. Plan the PMS transition for the pre-season period (February–March) to be fully operational before the high-demand season begins. Budget 2 weeks for setup, 1 week for training, and 1 month of parallel running.
