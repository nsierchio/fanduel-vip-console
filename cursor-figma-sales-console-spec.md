# Cursor Instructions — FanDuel VIP KAM Console Figma Spec File (v2)

## What you are building

Use your Figma MCP to create a **new Figma file** for the FanDuel VIP KAM Console.
This is a Salesforce Sales Console spec file handed to the Salesforce build team as their design reference for Phase 1.

**CRITICAL RENDERING RULES — read before building anything:**
- Every artboard must show REAL content — actual VIP names, real data, real UI components. No empty boxes, no gray placeholder rectangles labeled "table goes here."
- Every tabbed component must show ALL tab content states — one artboard per tab, not just the tab bar.
- Every component must show multiple states — see state list per component below.
- The full console layout must show every component rendered with real content inside it, not a shell.
- Mobile artboards must be included for every component.

---

## Step 1 — Create the new file

Call `create_new_file` with name: `FanDuel VIP KAM Console — Sales Console Spec v2`

Set up these pages:
1. `00 — Full Console Layout (Desktop + Mobile)`
2. `01 — My VIPs (Playbook Table)`
3. `02 — Real-Time Awareness`
4. `03 — Onboarding & Offboarding`
5. `04 — Last Contact Tracker`
6. `05 — Next Best Action Queue`
7. `06 — Tasks & Weekly Priority Contacts`
8. `07 — Upcoming Events`
9. `08 — Campaign Manager`
10. `09 — Birthdays This Week`
11. `10 — VIP Feedback Pulse`
12. `11 — Bonus Bet Credit`
13. `12 — Agentforce KAM Companion`

---

## Design tokens

```
Nav bar bg:        #0b5cab
Brand primary:     #0176d3
Page bg:           #f3f2f2
Card bg:           #ffffff
Card border:       #dddbda
Table header bg:   #f3f2f2
Text primary:      #181818
Text secondary:    #706e6b
Text label:        #3e3e3c
Row hover:         #f3f9ff
Success:           #2e844a
Error:             #ba0517
Warning:           #dd7a01
Info:              #0176d3
Font:              Salesforce Sans (fallback: Inter)
Base size:         13px
Card shadow:       0 2px 2px rgba(0,0,0,0.06)
```

---

## Annotation system

- **Orange numbered circles** (●1 ●2 …) pointing to UI elements
- **Legend box** bottom-left: white card, 1px #dddbda border, each number with label + 1-line note
- **Yellow sticky callouts** (`#fffbe6` bg, `#92400e` text) for open questions
- **Tag pills:**
  - Not MVP: `#e2e8f0` bg / `#64748b` text
  - Use Existing: `#e0f2fe` bg / `#0369a1` text
  - Data: AWS Agent: `#f3e8ff` bg / `#7e22ce` text
  - Post-MVP: `#fef3c7` bg / `#92400e` text

---

## Page 00 — Full Console Layout (Desktop + Mobile)

### Artboard A — Desktop (1440 × 1024px)

Render the FULL 3-column console with every component populated with real content. This is not a wireframe — it must look like an actual Salesforce console screen.

**Global Nav Bar (48px, #0b5cab):**
- Left: 9-dot waffle icon · divider · "FanDuel VIP KAM Console" (white, 13px, 700)
- Right: ? icon · ⚙ icon · 🔔 bell (red badge "3") · "JD" avatar circle

**Workspace Tabs (48px, white):**
- Active tab: "📋 My VIP Playbook" with #0176d3 text and 3px bottom border · ✕
- Inactive tab: "+ New Tab" in gray

**Page Header (56px, white):**
- Left: blue square icon · "My VIP Playbook" (20px bold) · "47 VIPs · Sportsbook KAM · Jordan Davis" (12px #706e6b)
- Right: Filters button · Export button · + Log Contact button (brand blue)

**Main Column (~44%):**

*VIP Playbook card* — show the My VIPs tab active with the SLDS table populated:
- 6 visible VIP rows (truncated for layout): James Harrington · Ethan Parker · Sophia Novak · Isla Hayes · Luca Romano · Noah Coleman
- Columns: Name · State · Segment · Tier · GGR L30 · Last Bonus · KAM · Last Contact · Status · Actions
- GGR colors applied (green positive, red negative)
- One row showing expanded child rows (multi-segment)
- Pagination footer: "Showing 1–10 of 47 VIPs"
- Segment filter pills above table: All (active, blue) · SBK · CAS · DFS · FDR

*Real-Time Awareness card* — show Big Swings tab active:
- 3 win rows (green amounts) + 2 loss rows (red amounts)
- Tab bar: Big Swings (active) · Losing Streaks · Payment Alerts (Not MVP tag)

*Onboarding & Offboarding card* — show New VIPs tab active:
- 4 onboarding rows with day counts color-coded
- Uma Price row showing ⚠ flag

*Last Contact Tracker card:*
- 5 VIP rows with color-coded last contact days
- Filter pills: All · In Cadence · Due Soon · Overdue

*VIP Feedback Pulse card* — SF placeholder empty state

**Activity Column (~28%):**

*Next Best Action Queue card:*
- 4 collapsed NBA cards with color bars
- Card header: Agentforce icon · "Next Best Action" · "4 Actions" badge · "MVP Static NBA Recco" tag

*Tasks & WPC card* — WPC tab showing 4 priority contacts

*Upcoming Events card* — 3 event rows

*Campaign Manager card* — 3 campaign rows

*Birthdays card* — current week showing 2 VIPs

*Bonus Bet Credit card* — upload zone placeholder

**Agent Column (~28%):**
- Agentforce Companion chat panel
- Pre-built prompt buttons row at top of input
- 3-turn conversation sample:
  - KAM: "Prepare me for a call with Ethan Parker"
  - Agent: structured brief with HPV summary + recent bets + promo recommendation
  - KAM: "What events should I invite him to?"
  - Agent: VIP Events Agent response

Add numbered annotations calling out the 3-column structure, sticky header layers, and key interaction zones.

---

### Artboard B — Mobile (390 × 844px, iPhone 14)

Show a single-column stacked layout of the same console on mobile:

**Mobile nav:** hamburger menu · "FanDuel VIP KAM" · bell icon · JD avatar

**Stacked cards (scroll view):**
1. My VIPs table — horizontal scroll, showing Name · Segment · GGR · Status · Actions (5 columns only; other columns accessible via horizontal scroll)
2. Next Best Action — 2 collapsed cards visible, "See all 4" link
3. Real-Time Awareness — Big Swings tab, 3 rows
4. Last Contact Tracker — 3 rows, pagination

**Floating Action Button (FAB):** bottom-right corner, Agentforce icon, #0176d3 bg — tapping opens the Companion chat as a full-screen sheet overlay

Add annotation: "Mobile view: Companion accessible via FAB only. Tables horizontally scrollable. Cards stack in priority order."

---

## Pages 01–12 — Component Spec Pages

**Each page layout:**
- Artboard size: 1440px wide, height as needed
- Left side: component renders (all states and tabs)
- Right side: annotations + legend
- Bottom: user notes and open questions

**State artboards to include per component:**
- **Default/Loaded** — real data populated
- **Empty state** — no records, with SF empty state illustration/text
- **Loading** — skeleton shimmer rows (gray animated bars)
- **Hover** — row highlighted (#f3f9ff), actions revealed
- **Tab states** — one artboard per tab showing that tab's content
- **Expanded state** — where applicable (expandable rows, cards)
- **Error/Validation** — where applicable (forms, uploads)
- **Mobile** — 390px artboard for the component

---

### Page 01 — My VIPs (Playbook Table)

**Artboard 1A — Default loaded state, My VIPs tab (1440px)**

Render the full SLDS table with 8 populated rows:

| Name | State | Segment | Tier | GGR L30 | Last Bonus Issued | KAM | Last Contact | Status | Actions |
|---|---|---|---|---|---|---|---|---|---|
| James Harrington ⚠ | NJ | SBK, CAS | Shield Select | +$8,400 | $500 Bonus Bet · Aug 28 | Jordan Davis | 3d ago | ACTIVE | ⋮ |
| Ethan Parker | NY | CAS | Tier 1 | -$12,400 | $0 [Overbonused] Bonus Bet · Aug 10 | Jordan Davis | 35d ago | ACTIVE | ⋮ |
| Sophia Novak ⚠ | FL | SBK | Tier 2 | +$3,200 | $250 Bonus Bet · Sep 2 | Jordan Davis | 18d ago | ACTIVE | ⋮ |
| Isla Hayes | TX | SBK | Tier 3 | -$4,100 | $150 Postal Gift · Aug 20 | Jordan Davis | 32d ago | SUSPENDED | ⋮ |
| Luca Romano | NJ | CAS | Shield Select | +$22,100 | $1,000 Event · Sep 5 | Jordan Davis | 8d ago | ACTIVE | ⋮ |
| Noah Coleman | NY | SBK | Tier 1 | +$46,200 | $500 Bonus Bet · Sep 1 | Jordan Davis | 12d ago | ACTIVE | ⋮ |
| Marcus Chen ▶ | CA | SBK, DFS | (multi) | — | — | Jordan Davis | 21d ago | ACTIVE | ⋮ |
| Carolina Hayes | IL | CAS | Tier 2 | -$2,800 | $200 Bonus Bet · Aug 15 | Jordan Davis | 7d ago | DEACTIVATED | ⋮ |

Apply GGR colors (green/red), Last Contact colors (≤14d gray, 15–29d amber, 30+ red), status pills.
Show Marcus Chen row with expand chevron ▶. No checkbox column.

**Artboard 1B — Multi-segment row expanded**
Same table, Marcus Chen row expanded showing 2 child rows:
- Child 1: ↳ · SBK · Tier 2 · +$3,100 · Jordan Davis
- Child 2: ↳ · DFS · Tier 5 · -$800 · Casey Park

**Artboard 1C — Overview tab**
Show the Overview tab content with 4 metric tiles:
- Total VIPs: 47 | Active: 44 | Shield Select: 6 | Avg GGR L30: +$2,840
- Tableau "View Reports" toggle below tiles (collapsed state)
- Tableau expanded state: 4 tabs (Overview / Spend Trends / Segments / Retention) with placeholder iframe

**Artboard 1D — Row hover state**
Ethan Parker row hovered: #f3f9ff bg, ⋮ menu visible

**Artboard 1E — ⋮ actions dropdown open**
Dropdown showing: Log Contact · Bonus · View Profile

**Artboard 1F — Empty state**
No VIPs match filter: SF-style empty state illustration + "No VIPs match your current filter. Try adjusting your segment or search."

**Artboard 1G — Loading state**
Table header visible, 5 rows showing gray skeleton shimmer bars

**Artboard 1H — Mobile (390px)**
Table with horizontal scroll: Name · GGR · Status · ⋮ (3 columns pinned left, rest scrollable)
Segment filter pills wrap to 2 lines. Pagination footer.

**Annotations (numbered):**
1. Segment filter pills — All/SBK/CAS/DFS/FDR, active state = #0060a3 bg + white text
2. Table header — #f3f2f2 bg, uppercase 11px, sort arrows on sortable columns
3. NBA alert ⚠ on VIP name — amber, hover tooltip "NBA Action pending"
4. Multi-segment expand ▶ chevron — rotates 90° when expanded
5. Child rows — indented ↳, dashed bottom border, lighter bg (#f8fafc)
6. GGR L30 — green (#2e844a) positive / red (#ba0517) negative, font-weight 700
7. Last Bonus: Overbonused variant — "$0 [red badge] / type · date"
8. Last Contact coloring — ≤14d=default · 15–29d=#dd7a01 · 30d+=#ba0517
9. Status pill — ACTIVE=green bg / SUSPENDED=amber bg / DEACTIVATED=gray bg
10. Row actions — hidden until hover, ⋮ opens dropdown
11. Pagination — "Showing X–Y of Z VIPs" + Prev/Next

**User notes:**
- "No bulk actions in MVP — row-level ⋮ only"
- "Segment filter replaces OOTB Salesforce list view filters — confirm approach in Phase 1"
- "KAM column: multi-seg VIPs may show different KAMs per child row"
- "Overbonused: $0 displayed + red badge — confirm with commercial team threshold logic"

---

### Page 02 — Real-Time Awareness

**Artboard 2A — Big Swings tab (default)**
Tab bar: **Big Swings** (active) · Losing Streaks · Payment Alerts [Not MVP]
Card header: bolt icon · "Real-Time Awareness" · "Data to reflect AWS Agent" tag

Big Swings content — wins section:
- Noah Coleman · Sportsbook · **+$46,200** · [View Wager] button
- Luca Romano · Casino · **+$28,750** · [View Wager] button
- Farah Siddiqui · DFS · **+$17,400** · [View Wager] button

Big Swings — losses section:
- Ethan Parker · Casino · **-$12,400** · [View Wager] button
- Isla Hayes · Sportsbook · **-$9,800** · [View Wager] button

**Artboard 2B — Losing Streaks tab**
Tab bar: Big Swings · **Losing Streaks** (active) · Payment Alerts [Not MVP]

Losing Streaks rows:
- Ethan Parker · 6 losing sessions · -$34,400 total · [Log Contact] [⋮]
- Marcus Thompson · 4 losing sessions · -$18,200 total · [Log Contact] [⋮]
- Riley King · 3 losing sessions · -$9,600 total · [Log Contact] [⋮]

**Artboard 2C — Payment Alerts tab**
Tab bar: Big Swings · Losing Streaks · **Payment Alerts** (active)
"Not MVP" pill prominently shown below tab bar
"Data readiness pending — Payment Alerts will be available once AWS Agent integration is confirmed."
Gray placeholder content showing what it will look like when live.

**Artboard 2D — Wager History modal**
Dark gradient header (#0b1f45 → #0060a3): "Wager History" · VIP meta · "Data to reflect AWS Agent" tag · ✕
Summary bar: 6 Sessions · Net -$12,400 · Avg stake $4,200
Filter pills: All · Wins · Losses
Table: Date · Sport · Bet Type · Stake · Return · Net
5 rows of wager data
Footer: [Apply Bonus] [Contact VIP] [Close]

**Artboard 2E — Mobile (390px)**
Tabs scroll horizontally. Big Swings showing 3 rows. View Wager opens full-screen modal.

**Annotations:**
1. Tab bar — Big Swings default active · Payment Alerts has Not MVP tag
2. "Data to reflect AWS Agent" tag on card header
3. Win/Loss color coding — green (+) / red (-)
4. View Wager button — opens wager history modal
5. Wager modal header — dark gradient, AWS Agent tag, ✕
6. Summary bar in modal — key metrics at a glance
7. Losing Streaks — Log Contact pre-populates with welfare check context

**User notes:**
- "Big Swings + Losing Streaks: data from AWS Agent event stream — confirm field mapping with data team"
- "Payment Alerts: backlog — 3rd-party data source TBD"
- "View Wager modal: will link to actual wager records in Salesforce once integration live"

---

### Page 03 — Onboarding & Offboarding

**Artboard 3A — New VIPs tab (default)**
Tab bar: **New VIPs** (active) · Churn / Demotion

SLDS table with 4 rows:

| Name | Segment | Joined | Stage | KYC | Actions |
|---|---|---|---|---|---|
| Tyler Morgan | SBK | 1d ago (green) | Wk 1 | Pending | [Send Survey] [⋮] |
| Uma Price ⚠ | SBK | 5d ago (amber) | Wk 1 | Pending | [Send Survey] [⋮] |
| Victor Ward ⚠ | CAS | 21d ago (red) | Wk 3 | Complete | [Log Preferences] [⋮] |
| Aaliyah Henderson ⚠ | DFS | 38d ago (red) | Wk 6 | Pending | [Urgent] [⋮] — Urgent button red |

⚠ flag visible on Uma, Victor, Aaliyah rows.
Day count colors: ≤3d=green · 4–7d=amber · >10d=red.

**Artboard 3B — ⋮ dropdown open (New VIPs)**
Victor Ward row with dropdown showing: Follow Up / Contact · Mark Complete (green)

**Artboard 3C — Mark Complete animation state**
Victor Ward row fading out (opacity reduced), toast: "Onboarding complete — Victor Ward removed from queue ✓"

**Artboard 3D — Churn / Demotion tab**
Tab bar: New VIPs · **Churn / Demotion** (active)
"Leverage Current Process" gray pill below tab bar
Subtitle: "VIPs at risk of demotion or churn — action required."

Rows:
- Sophia Novak · [Demotion Risk red badge] · High · [Retain] [Demote]
- Parker Ortiz · [Long Timeout yellow badge] · Med · [Retain] [Demote]
- Riley King · [Churn Signal pink badge] · High · [Retain] [Demote]

**Artboard 3E — Demotion confirmation modal**
Modal: "Initiate Demotion — Sophia Novak"
Fields: Demotion Reason (picklist, required) · Impact Summary (read-only)
Footer: [Cancel] [Confirm Demotion] (red, disabled until reason selected)

**Artboard 3F — Empty state (New VIPs)**
No VIPs in onboarding window: "All new VIPs have completed onboarding. Great work!"

**Artboard 3G — Mobile (390px)**
Stacked card rows. Day count and ⚠ flag visible. Action button full width. ⋮ as overflow.

**Annotations:**
1. ⚠ no-contact flag — amber, appears >3 days after join with no logged contact
2. Joined day count colors — green/amber/red thresholds
3. Stage label — plain week label, no colored badge
4. KYC badge — Pending (amber) / Complete (green)
5. Contextual action button — changes by stage: Send Survey / Log Preferences / Urgent (red)
6. ⋮ dropdown — Follow Up/Contact · Mark Complete
7. "Leverage Current Process" tag on Churn tab
8. Demotion confirmation modal — reason required before confirm enables

**User notes:**
- "3-day contact target is a business rule — confirm if configurable per segment"
- "Send Survey: pending Outlook integration — manual trigger for MVP"
- "Churn/Demotion: leverage existing process per stakeholder direction"

---

### Page 04 — Last Contact Tracker

**Artboard 4A — Default (All filter, default sort: most overdue first)**

Filter pills: **All** (active) · In Cadence · Due Soon · Overdue

SLDS table:

| Name + Segment | Last Contact | Contact Status | Actions |
|---|---|---|---|
| Ethan Parker CAS | 35d ago (red) | [Overdue pill] | [Email] [SMS] [⋮] |
| Isla Hayes SBK | 32d ago (red) | [Overdue pill] | [Email] [SMS] [⋮] |
| Sophia Novak SBK | 18d ago (amber) | [Due Soon pill] | [Email] [SMS] [⋮] |
| Nolan Brooks SBK | 20d ago (amber) | [Due Soon pill] | [Email] [SMS] [⋮] |
| Jordan Williams SBK | 5d ago (green) | [In Cadence pill] | [Email] [SMS] [⋮] |

Pagination: "Showing 1–10 of 47 VIPs" + Prev/Next

**Artboard 4B — Overdue filter active**
Filter pills: All · In Cadence · Due Soon · **Overdue** (active, red)
Showing only overdue rows. Count badge "2 Overdue" in card header.

**Artboard 4C — Row hover + ⋮ dropdown**
Ethan Parker row hovered, ⋮ open showing: View Contact Record · Log Contact

**Artboard 4D — Empty state (In Cadence)**
"All VIPs are in cadence. No contacts overdue."

**Artboard 4E — Loading state**
Skeleton shimmer rows

**Artboard 4F — Mobile (390px)**
2-column layout: Name + Last Contact · Actions. Status pill on second line under name. Email/SMS as icon buttons.

**Annotations:**
1. Filter pills — color-coded: In Cadence=green text · Due Soon=amber · Overdue=red
2. Last Contact color threshold — 0–14d=default · 15–29d=amber · 30d+=red
3. Contact Status pills — In Cadence (green bg) / Due Soon (amber) / Overdue (red)
4. Email + SMS direct action buttons per row
5. ⋮ overflow — View Contact Record · Log Contact
6. Pagination footer — supports 260–500 VIP books
7. Default sort — most overdue first (highest days)

**User notes:**
- "Email/SMS buttons link to Salesforce email/SMS send actions in build"
- "Log Contact in ⋮ updates Last Contact date and recalculates status real-time"
- "Cadence thresholds (14d/30d) to be confirmed with ops team — may vary by segment"

---

### Page 05 — Next Best Action Queue

**Artboard 5A — Flat cards (default)**
Card header: Agentforce icon · "Next Best Action" · "4 Actions" red badge · "AI-prioritized" · "MVP Static NBA Recco" gray tag

4 flat cards (no expand/collapse — actions always visible):
- 🔴 **No Contact** · Ethan Parker · Tier 1 · CAS · "No contact logged in 35 days. Last contact Aug 13. Cadence threshold exceeded." · −$12,400 GGR L30 · 35 days overdue · [Log Contact] [Defer]
- 🔴 **CS Alert** · Sophia Novak · Shield Select · SBK · "Case #00812 opened 4 hours ago. Withdrawal delay — funds not received after 48h." · Case open 4h · Priority: High · [Contact VIP] [View Case] [Defer]
- 🔴 **Big Loss** · Noah Coleman · Tier 2 · CAS · "−$46,200 single-day loss. 5.4× daily average. Empathy outreach — no promo messaging." · −$46,200 today · 5.4× avg · [Plan Outreach] [View Wagers] [Defer 24h]
- 🔵 **Bonus Opp** · Luca Romano · Tier 3 · CAS · "22 days since last bonus. RTC handle below target — Bonus Bet Credit could re-activate play." · Last bonus: 22d ago · RTC below target · [Issue Bonus] [Defer]

**Artboard 5B — Empty state**
Agentforce icon + "You're all caught up. No pending actions." green check

**Artboard 5C — Mobile (390px)**
Cards stack full width. Action buttons wrap below description.

**Annotations:**
1. Colored left border (4px) — red=urgent scenarios (No Contact/CS Alert/Big Loss) · blue=opportunity (Bonus Opp)
2. Category tag — "No Contact" / "CS Alert" / "Big Loss" / "Bonus Opp" uppercase label, color-matched to border
3. "MVP Static NBA Recco" tag — static hardcoded for prototype; dynamic Agentforce NBA in Phase 2
4. No expand/collapse — all card content and actions visible by default (flat card pattern)
5. Action buttons inline — always visible; Log Contact/Plan Outreach/Contact VIP/Issue Bonus as primary; Defer as secondary
6. Issue Bonus → opens Bonus Bet Credit modal (Wallet → Label → Expiration → Value → Description → On Behalf Of)

**User notes:**
- "MVP: static hardcoded recommendations — dynamic Agentforce NBA in Phase 2"
- "4 scenarios selected for MVP: No Contact, CS Alert, Big Loss, Bonus Opp — all feasible without AWS Agent dependency"
- "RG Signal (Big Loss): no promo before empathy outreach confirmed — welfare protocol enforced"
- "Dismiss with reason: needed for Agentforce feedback loop — confirm data model with architect"

---

### Page 06 — Tasks & Weekly Priority Contacts

**Artboard 6A — WPC tab (default)**
Card header: checkmark icon · "Tasks & Weekly Priority Contacts" · "9" blue badge · "Use Existing" teal tag
Tab bar: My Tasks · **Weekly Priority Contacts** (active)
Progress bar: "3 of 7 complete" (filled to ~43%, brand blue)

WPC rows (4 visible):
- Isla Hayes · SBK · 32d since contact · Not Started [Log Contact]
- Nolan Brooks · SBK · 20d since contact · In Progress [Log Contact]
- Sophia Novak · SBK · 18d since contact · Not Started [Log Contact]
- Jordan Williams · SBK · 5d since contact · ✓ Complete (green, strikethrough)

**Artboard 6B — My Tasks tab**
Tab bar: **My Tasks** (active) · Weekly Priority Contacts
Filter: All · Pending · Done

Tasks:
- ☐ Send birthday offer to Olivia Bennett · Due Today · SBK · [VIP link]
- ☐ Review Ethan Parker welfare flag · Due Today · CAS · [VIP link]
- ☐ Log preferences for Victor Ward · Due Sep 20 · CAS · [VIP link]
- ☑ Confirmed event invite — Noah Coleman · Completed Sep 15 · SBK · (green strikethrough)

**Artboard 6C — Log Contact modal**
Modal overlay: "Log Contact — Isla Hayes"
Fields: Contact Type (Call selected) · Date/Time (auto-filled) · Notes (text area) · Outcome (picklist)
Footer: [Cancel] [Save]

**Artboard 6D — Mobile (390px)**
Tabs full width. Progress bar. WPC rows stacked. Log Contact full-width button.

**Annotations:**
1. "Use Existing" tag — leverage native Salesforce Tasks object
2. Progress bar — WPC completion % for current week
3. Filter tabs — All / Pending / Done persist during session
4. Log Contact modal — pre-filled from row context
5. Completed rows — green badge + visual distinction

**User notes:**
- "Tasks: use standard Salesforce Task object — no custom object needed"
- "WPC list: sourced from commercial team's weekly priority export — confirm pipeline"

---

### Page 07 — Upcoming Events

**Artboard 7A — Default (all events)**
Card header: calendar icon · "Upcoming Events" · "3" purple badge · "Ticket Manager / Avail Event" gray tag

Event 1: My Hosted Event [Host orange badge] [Not MVP gray tag]
- Washington Nationals at NY Mets · Aug 15 · Citi Field
- 6 Accepted · 1 Declined · 3 Pending · 10 invites
- ⋮ 3-dot menu → Manage Event / View RSVPs / Send Reminder

Event 2: Invited Event
- NFL Season Opener VIP Suite · Sep 8 · MetLife Stadium
- RSVP: Accepted [green badge] · 4 tickets remaining
- ⋮ 3-dot menu → Invite VIP / View Event / Remove

Event 3: Upcoming Event
- FanDuel Golf Classic · Oct 12 · TPC Sawgrass
- 12 tickets available · Invitations not yet sent
- ⋮ 3-dot menu → Invite VIP / View Event / Remove

**Artboard 7B — Manage Event modal**
Modal: "My Hosted Event — Guest List"
Table: VIP Name · RSVP Status · Ticket Assigned · Notes
5 rows: James Harrington (Accepted) · Ethan Parker (Declined) · Noah Coleman (Pending) · Luca Romano (Accepted) · Sophia Novak (Sent)
Footer: [Add VIP] [Export List] [Close]

**Artboard 7C — Empty state**
"No upcoming events. Check back soon or create a new event."

**Artboard 7D — Mobile (390px)**
Event cards stacked. RSVP chips visible. Action button full width.

**Annotations:**
1. "Ticket Manager / Avail Event" tag — data from ticketing platform (3rd-party)
2. "Not MVP" on My Hosted Event — data source not yet confirmed
3. RSVP status chips — Accepted=green · Declined=red · Pending=amber
4. Manage modal — full guest list with RSVP status per VIP

**User notes:**
- "Event data: ticketing platform API — integration TBD Phase 2"
- "Hosted events: backlog until data source confirmed"

---

### Page 08 — Campaign Manager

**Artboard 8A — Campaign list (default)**
Card header: chart icon · "Campaign Manager" · "3 Active" blue badge · "Use Existing" teal tag

Campaigns:
- NFL Season Opener Promo · SBK · **ACTIVE** green · 24 VIPs · Sent Sep 1 · ⋮ → View Campaign / Add VIP / Duplicate
- Shield Select Loyalty Reward · All · **ACTIVE** green · 6 VIPs · Sends Sep 20 · ⋮ → View Campaign / Add VIP / Duplicate
- DFS Reactivation · DFS · **SCHEDULED** blue · 12 VIPs · Sends Oct 1 · ⋮ → View Campaign / Add VIP / Duplicate
- Q4 Casino Retention · CAS · **DRAFT** gray · 18 VIPs · Not scheduled · ⋮ → Edit Campaign / Preview / Delete Draft

New button top-right.

**Artboard 8B — Campaign Detail modal**
Modal: "NFL Season Opener Promo"
VIP target list (5 rows visible), offer details ($500 bonus), approval history, scheduled send
Footer: [Export List] [Close]

**Artboard 8C — New Campaign modal**
Fields: Campaign Name · Type (picklist) · Offer Amount · Target VIPs (multi-select) · Send Date
Footer: [Cancel] [Save as Draft]

**Artboard 8D — Mobile (390px)**
Campaign rows stacked. Status badges. New button in header.

**Annotations:**
1. "Use Existing" tag — native Salesforce Campaigns object
2. Status badges — ACTIVE=green / DRAFT=gray / SCHEDULED=blue
3. + New button — creates draft, routes to manager for approval
4. Campaign Detail — full target VIP list accessible

**User notes:**
- "Use native Salesforce Campaign object — no custom development needed"
- "Approval: route through standard Salesforce approval process"

---

### Page 09 — Birthdays This Week

**Artboard 9A — Current week (Sep 9–15)**
Card header: barcode-style blue icon · "My Customer Bdays Within 30 Days"
Week navigator: [‹] **Sep 9 – Sep 15** [›]

Birthday list:
- Olivia Bennett · SBK · **Today** · ⋮ 3-dot menu → Send Celebration / View Profile / Log Note
- Luca Romano · CAS · **Tomorrow** · ⋮ 3-dot menu → Send Celebration / View Profile / Log Note

Today row: amber left border highlight.

**Artboard 9B — Next week (Sep 16–22)**
Navigator: [‹] **Sep 16 – Sep 22** [›]
- Marcus Thompson · CAS · Sep 18 · ⋮ 3-dot menu
- Jordan Williams · SBK · Sep 20 · ⋮ 3-dot menu

**Artboard 9C — Sent state**
Olivia Bennett row after send: "Gift sent ✓" green text. 3-dot menu remains — Send Celebration item hidden; View Profile / Log Note remain.

**Artboard 9D — Empty week**
Navigator pointing to a week with no birthdays: "No birthdays this week."

**Artboard 9E — Mobile (390px)**
Navigator full width. Birthday rows stacked. 3-dot menu right-aligned.

**Annotations:**
1. Week navigator — ‹/› steps through 4-week window; Prev disabled at current week
2. "Today" row — amber left border highlight
3. Segment chip — SBK/CAS/DFS/FDR color-coded
4. Sent state — green "Gift sent ✓" text; Send Celebration removed from menu to prevent double-send
5. 3-dot menu actions to be confirmed in refinement — placeholder labels: Send Celebration / View Profile / Log Note

**User notes:**
- "Birthday data from VIP profile DOB — confirm data quality with ops"
- "Send Celebration: links to email/offer flow — Outlook integration or Salesforce email action"

---

### Page 10 — VIP Feedback Pulse

**Artboard 10A — Placeholder empty state (current MVP)**
SF-style placeholder: olive hammer icon in colored tile + "My VIP Customers Feedback" + "0 items, sorted by Incident Number" + chevron →
Empty state: "There's nothing in My VIP Customers Feedback yet. When records are added to this list view, you'll see them here."
More link top-right.

**Artboard 10B — Future loaded state (Post-MVP vision)**
Show what it will look like when live:
Tabs: Unreviewed (3) · All Feedback
3 feedback items: Positive (green) · Neutral (gray) · Negative (red)
Each item: VIP name · feedback excerpt · sentiment badge · date
Actions: [Mark Reviewed] [Respond]

**Artboard 10C — Mobile (390px)**
Placeholder card. Future state preview below.

**Annotations:**
1. Current state: SF Related List placeholder — data source TBD
2. Future state: sentiment badges powered by Agentforce Einstein NLP (Phase 2)
3. "More" link — deep-links to full Salesforce list view

**User notes:**
- "Data source unclear — pending scoping with FanDuel data/platform team"
- "Sentiment analysis (Positive/Neutral/Negative): Agentforce Einstein NLP — Phase 2"

---

### Page 11 — Bonus Bet Credit

**Artboard 11A — Upload zone (default)**
Card header: "Bonus Bet Credit"
Dashed border upload zone: Upload Files button + "Or drop files" text
Info bar (gray): ⓘ "Upload a CSV file with columns: VIP ID · Credit Amount · Expiry Date"

**Artboard 11B — File uploading state**
Progress bar inside upload zone. File name shown. Cancel option.

**Artboard 11C — Validation: errors present**
Preview table with rows:
- Row 1: ✓ valid — Jordan Williams · $500 · Oct 30
- Row 2: ✗ error (red highlight) — "Invalid VIP ID: FD99999 not found"
- Row 3: ✓ valid — Ethan Parker · $250 · Oct 30
Error summary: "1 error found. Fix before uploading or remove invalid rows."
Footer: [Cancel] [Upload Valid Rows Only] [Fix & Re-upload]

**Artboard 11D — Awaiting manager approval**
"Awaiting Approval" banner at top (amber). Shows submitted by, date, credit count.
Manager view: same artboard with [Approve] [Reject] buttons.

**Artboard 11E — Mobile (390px)**
Upload zone responsive. Error state rows stacked.

**Annotations:**
1. Dashed upload zone — drag-and-drop or click to browse
2. CSV format spec — VIP ID · Credit Amount · Expiry Date
3. Validation — invalid rows flagged inline before confirm
4. Manager approval state — Awaiting Approval banner visible to KAM

**User notes:**
- "CSV upload: manual workaround for MVP — longer term triggered by commercial system"
- "Manager approval: standard Salesforce approval process"

---

### Page 12 — Agentforce KAM Companion

**Artboard 12A — Companion chat panel (Desktop, resting state)**
3rd column panel. Companion header: Agentforce icon · "Agentforce Companion" · expand icon
Pre-built prompt buttons row (above input): [Prep for a call ▸] [Who needs contact today? ▸] [Top promo opportunity ▸] [Upcoming events ▸]
Empty chat state: Agentforce icon centered, "Ask me anything about your VIP book."
Input field: "Ask Agentforce…" placeholder · Send button

**Artboard 12B — Active conversation (call prep example)**
Pre-built prompt "[Prep for a call ▸]" tapped, then KAM typed "Ethan Parker"

Chat thread:
- KAM bubble: "Prep me for a call with Ethan Parker"
- Agent bubble (structured response):
  **Ethan Parker — Call Brief**
  📊 HPV: -$12,400 GGR L30 · Tier 1 Casino · 6 losing sessions
  🎲 Last bets: 4 NFL parlays, avg $2,100 stake, all losses
  🎁 Promo Coach: Consider $500 Bonus Bet — retention offer
  📅 Events: No upcoming invitations sent
  [View Full Profile] [Log Contact]
- KAM bubble: "What events should I invite him to?"
- Agent bubble: "Based on Ethan's profile I recommend the **FanDuel Golf Classic Oct 12** (Tier 1 eligible, 4 tickets remaining) and the **NFL Suite Sep 28** (matches betting preferences). Want me to send invitations?"
  [Send Invitations] [View Events]

**Artboard 12C — Intent routing visible**
Show the companion indicating which sub-agent was invoked:
Small label under agent response: "↳ VIP Events Agent · VIP HPV Report Agent · BetExplain Agent · Promo Coach"

**Artboard 12D — Multi-agent response: Promo Coach**
KAM: "What promo should I give Sophia Novak?"
Agent bubble: "Promo Coach recommends a **$250 Bonus Bet** for Sophia Novak.
Reason: Tier 2 SBK · +$3,200 GGR L30 (healthy) · Last contacted 18 days ago — approaching cadence threshold.
Timing: Send before Sunday NFL slate for best uptake.
↳ Promo Coach Agent"
[Apply Bonus] [Log Contact Instead]

**Artboard 12E — Post-MVP: Log Contact from chat**
KAM: "Log that I called Ethan Parker today"
Agent: "Logging a Call for Ethan Parker today (Sep 17). Want to add notes?"
KAM: "Just say welfare check completed"
Agent: "✓ Contact logged — Call · Sep 17 · Welfare check completed. Last Contact updated."
"Post-MVP" tag on this artboard.

**Artboard 12F — Post-MVP: Send Email from chat**
KAM: "Send Ethan a follow-up email about the bonus"
Agent draft shown in chat: subject + body pre-filled
[Edit Draft] [Send Now] [Cancel]
"Post-MVP" tag on this artboard.

**Artboard 12G — Manager/Director persona**
Manager Jordan Davis asks: "Which of my KAMs has the most overdue contacts?"
Agent: "Across your team of 4 KAMs:
1. Casey Park (DFS) — 8 VIPs overdue
2. Reese Torres (Racing) — 5 VIPs overdue
3. Morgan Lee (Casino) — 3 VIPs overdue
4. Jordan Davis (SBK) — 2 VIPs overdue
Want me to surface the most urgent VIPs for each?"
[View Full Team Report]

**Artboard 12H — FAB (Mobile, Companion collapsed)**
Full console visible on mobile. Bottom-right: circular Agentforce FAB (#0176d3). Tap to open chat as full-screen sheet.

**Artboard 12I — Mobile chat sheet**
Full-screen bottom sheet: Companion chat with pre-built prompt buttons. Input at bottom with keyboard.

**Annotations:**
1. Pre-built prompt buttons — one-tap common queries; content TBD in build refinement
2. Intent routing label — shows which sub-agent(s) were invoked
3. Structured response format — call brief uses sections: HPV / Bets / Promo / Events
4. Inline action buttons — CTA within agent response (Post-MVP for write actions)
5. Multi-agent parallel query — single question invokes Events + HPV + BetExplain + Promo Coach
6. Role-scoped responses — Manager sees team-level data
7. Mobile FAB — only access point for Companion on mobile

**Sub-agents legend (include as an info box):**
| Sub-agent | Capability | Status |
|---|---|---|
| VIP Events Agent | Event availability · invites · RSVP status | Built |
| VIP HPV Report Agent | Custom high-player-value metric reporting | Built |
| BetExplain Agent | Bet history breakdown in plain English | Built |
| Promo Coach Agent (x2) | Promotional recommendations — routing TBD | Built |
| KAM Companion (Orchestrator) | Routes to all above · VIP book awareness · team hierarchy | In progress |

**User notes:**
- "Pre-built prompt labels: TBD in build refinement — placeholder labels shown"
- "Promo Coach dual-instance routing logic: confirm with platform team (segment-based vs product-line)"
- "Log Contact + Send Email: post-MVP — ship orchestrator + read-only first, add write actions in next sprint"
- "Team/hierarchy awareness: confirm data model — KAM→Manager→Director relationship in Salesforce org"
- "BetExplain: designed for KAM use; confirm if VIP-facing use case (direct to player) is also in scope"

---

## Final checklist before delivering the file

- [ ] All 13 pages created (00–12)
- [ ] Every component has: Default · Empty · Loading · Hover states as artboards
- [ ] Every tabbed component has one artboard per tab showing that tab's content
- [ ] Full console page (00) shows real component content — not a shell
- [ ] Desktop (1440px) AND mobile (390px) artboards on every page
- [ ] SLDS design tokens applied consistently
- [ ] Annotation system consistent: numbered circles + legend + yellow sticky notes
- [ ] All MVP tags, Not MVP tags, Use Existing tags, and Post-MVP tags applied
- [ ] Sub-agent legend box on Page 12
- [ ] File shared with edit access — return the Figma file URL
