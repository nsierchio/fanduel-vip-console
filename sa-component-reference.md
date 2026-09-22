# FanDuel VIP KAM Console — SA Component Reference

**Purpose:** Source-of-truth reference for the Solutions Architect generating Intent Requirement Docs (IRDs). Synthesises decisions from the prototype, user stories, Figma spec, and design sessions that do not fit neatly into a user story card. Read alongside `all-components-user-stories.csv` and the Figma spec.

**Prototype:** `console-v2.html` — single-page HTML/JS mock, SLDS v1 shell, all data seeded.
**Target platform:** Salesforce Sales Cloud, Lightning Experience, SLDS v1 (client is on v1 — LWCs must render correctly in this environment).
**Build model:** Two-track. Track A = Experience POC (LWC, seeded data, Friday demos). Track B = Integration readiness (DataPoints, Agentforce, Tableau). Phases 1–4 defined in `build-roadmap.md`.

---

## Personas

| Alias | Role | Primary use cases |
|---|---|---|
| Taylor | Sportsbook KAM | UF1–UF8 full suite, WPC, event-driven clock |
| Morgan | Casino KAM | UF3/UF9 SLAs in hours not days, CPP bonusing |
| Casey | DFS KAM | Voucher flow, contest-shaped UF4, rarely demotes |
| Reese | Racing KAM | TVG + FanDuel Racing dual systems, sparse data |
| Avery | KAM Manager | UF10 team exceptions, hybrid default |
| Pat | KAM Director | Division ops, not a daily console user |
| Jordan Davis (JD) | Demo persona | Logged-in KAM in all prototype screens |

---

## Cross-Component Business Rules

These rules apply to multiple components. Define once, reference everywhere.

### Last Contact Definition
**Locked.** Last Contact = the most recent activity satisfying ALL of:
1. Activity type: Email (logged or synced) · Call (logged) · SMS (logged or synced)
2. Activity associated to the VIP's **Contact record** or **Asset record** — not Opportunity or other related objects
3. Activity is **NOT** linked to a Salesforce Campaign or Marketing Cloud mass send
4. **Campaign emails are explicitly excluded** — the console tracks 1-to-1 communication only

SF objects: `Task` (for calls/SMS) · `EmailMessage` (for emails). Filter on `WhoId` = Contact Id. Exclude where `CampaignId` is populated or activity source = Marketing Cloud.

Phase 1: mock data on Contact records. Phase 2: live SOQL via `ActivityDate` on Task/EmailMessage.

### Priority Book Definition
**Locked.** Priority Book = top 10% of a KAM's book by revenue rank.
- SBK: `Book_GGR_Rank_Pct__c ≤ 10` on the Contact object
- CAS: `Book_NGR_Rank_Pct__c ≤ 10` on the Contact object
- DFS / Racing: not yet defined — Phase 1 SBK/CAS only

This single field drives both the **NBA No Contact alert trigger** and the **LCT Priority Book section**. It is the only Phase 1 data dependency shared across two components. Source: nightly ETL or Data Cloud sync to Contact.

### RG Gate (Welfare Check)
Any outreach initiated from a Big Loss or Losing Streak signal must check the RG welfare flag before surfacing a commercial action. If RG threshold is met, the Welfare Check path (locked template) takes priority. Agentforce must never suggest a bonus on a card that has triggered an RG flag. This gate is a hard requirement — not configurable by the KAM.

---

## Component 1 — VIP Playbook (My VIPs Table)

**Purpose:** The KAM's primary book of business view. Every VIP assigned to Jordan Davis (JD) appears here. Entry point for all downstream actions.

**Locked decisions:**
- Column order: Name · State · Segment · Tier · GGR L30 · Last Bonus Issued · KAM · Last Contact · Status · Actions
- Segment abbreviations: SBK · CAS · DFS · FDR (no full names)
- Shield Select renders as a small shield SVG + label (not a colored pill)
- Segment displayed as comma-separated plain text ("SBK, CAS") — no chips
- GGR L30: green if positive (house won), red if negative (player winning)
- Last Bonus Issued: Amount on top, Type · Date below; if Overbonused: "$0 [Overbonused badge] / Type · Date"; if no bonus: em dash
- Status: Active · Deactivated · Suspended — small tight uppercase pill
- NBA pending: amber triangle icon on VIP name + hover popover "NBA Action pending" — no full NBA column at this stage
- Filter: search input + Filter button (OOTB Salesforce list filters — no custom pill filters)
- Sortable: Name, Tier, GGR L30, Last Contact, Last Bonus Issued

**SF objects:** Contact (VIP master) · Task/Activity (Last Contact) · Bonus object TBC (Last Bonus Issued)

**Phase 1:** Mock rows seeded in sandbox. No live GGR. No live bonus data.
**Phase 2:** Live Contact + Activity reads. GGR L30 and bonus data blocked on DataPoints/Track B.

**Open questions:**
- Bonus Bet / Postal Gift / Event — are these picklist values on an existing SF object, or a new custom object?
- Overbonused flag: is this a Contact field, or derived from a bonus limit object?

---

## Component 2 — Next Best Action (NBA) Queue

**Purpose:** AI-prioritised outreach prompts surfaced at the top of the console. Four flat cards for Phase 1 MVP — no expand/collapse, actions always visible inline.

**Locked decisions:**
- 4 MVP scenarios only: No Contact (red) · CS Alert (red) · Big Loss (red) · Bonus Opp (blue)
- Cards are flat — no accordion/expand pattern. All metadata and actions visible on the card face.
- Colored left border (4px): red = urgent (No Contact, CS Alert, Big Loss) · blue = opportunity (Bonus Opp)
- **No Contact fires ONLY for Priority Book VIPs** (top 10% by GGR/NGR). Rest of book managed via Last Contact Tracker.
- Tag: "MVP Static NBA Recco" — static hardcoded for Phase 1. Dynamic Agentforce NBA in Phase 2.
- Actions per card:
  - No Contact: [Log Contact] [Defer]
  - CS Alert: [Contact VIP] [View Case] [Defer]
  - Big Loss: [Plan Outreach] [View Wagers] [Defer 24h]
  - Bonus Opp: [Issue Bonus] [Defer]

**SF objects:** No Contact → `Book_GGR_Rank_Pct__c` on Contact · CS Alert → Case object · Big Loss → Player Intelligence Engine / AWS data (Phase 3 dependency) · Bonus Opp → bonus cadence fields TBC

**NBA No Contact trigger rule:**
- Fires when: `Book_GGR_Rank_Pct__c ≤ 10` AND days since last qualifying 1:1 contact exceeds cadence threshold
- Configured via Salesforce NBA Strategy Builder (OOTB config — not custom code)

**Phase 1:** All 4 cards hardcoded with named mock VIPs (Ethan Parker, Sophia Novak, Noah Coleman, Luca Romano).
**Phase 2:** Agentforce NBA use cases — requires Andy Geissler / FanDuel VIP Technology sign-off.
**Phase 3:** Big Loss card requires Player Intelligence Engine data feed from FanDuel AWS layer.

**Open questions:**
- CS Alert: does the CS/HVP team currently create Salesforce Cases for every VIP interaction? Card cannot fire without this process in place.
- Big Loss: what is the exact threshold (e.g. $100K+, or 5× avg)? Who owns the definition?
- Defer duration: is 24h a platform setting or hard-coded per scenario?

---

## Component 3 — Real-Time Awareness Command Center

**Purpose:** Intra-shift signal center for big financial moves, payment issues, and losing streaks. Morgan's primary morning view.

**Tabs:** Big Swings · Payment Alerts · Losing Streaks

**Locked decisions:**
- **Payment Alerts tab** is MVP scope — tag reads "Pending Data Source" (data source needs to be confirmed/connected before build). It is NOT excluded from Phase 1 — it is in scope but blocked on data.
- Losing Streaks tab: Welfare Check button uses locked/approved language template. This is a hard gate — no commercial messaging on welfare-flagged players.
- Big Swings feeds into NBA Big Loss card. Same data dependency (Player Intelligence Engine).

**SF objects / data sources:**
- Big Swings: Player Intelligence Engine / AWS Intelligence Engine — Phase 3 dependency
- Payment Alerts: withdrawals data — source TBC (DataPoints? Payments platform?)
- Losing Streaks: session/loss data — DataPoints or AWS

**Phase 1:** All three tabs mock. Big Swings and Losing Streaks seeded with Ethan Parker data.
**Phase 3:** Live data requires Track B DataPoints/AWS access.

**Open question:** Payment Alerts data source — confirm whether withdrawals data sits in DataPoints, a payments microservice, or a separate platform.

---

## Component 4 — Last Contact Tracker (LCT)

**Purpose:** Full-book cadence management. KAMs self-manage contact frequency for their entire book. Priority Book VIPs surface at the top.

**Locked decisions:**
- Two-section layout: (1) Priority Book — Top 10% GGR/NGR · (2) Full Book
- Section headers labeled, color-coded: Priority Book = `#eef4ff` (blue tint) · Full Book = `#f3f2f2` (neutral)
- Each section sorts independently by active sort (default: most overdue first)
- Empty sections hidden when filter produces no matches
- Cadence thresholds: ≤14d = In Cadence (green) · 15–29d = Due Soon (amber) · 30d+ = Overdue (red)
- **Contact Source column:** Email · Call · SMS — colored text pill. Derived from most recent qualifying 1:1 activity. Blank if none.
  - Email = blue (`#0b5cab` on `#e8f4fd`) · Call = green (`#1a6632` on `#e8f5ec`) · SMS = purple (`#6b21a8` on `#f3e8fd`)
- **Contact Note column:** Truncated note (45 chars) from last activity's Description/Notes field. "View" link opens the full activity record in Salesforce. Blank if no note — no placeholder dash.
- Contact Source and Contact Note derive from the **same activity record** as the Last Contact date.
- **Last Contact definition:** see Cross-Component Business Rules above — 1:1 only, campaigns excluded.
- Actions per row: [Email] [SMS] [⋮ Log Contact / View Contact Record]
- Pagination: 10 rows per page (default), spans both sections combined

**SF objects:** Contact · Task · EmailMessage · Asset (if activity logged on Asset)
**Key field:** `Book_GGR_Rank_Pct__c` (SBK) / `Book_NGR_Rank_Pct__c` (CAS) on Contact — shared with NBA No Contact

**Phase 1:** Seeded mock data on Contact records. Priority flags, source, and notes hardcoded.
**Phase 2:** Live read from Task/EmailMessage. Cadence calculated dynamically. View link resolves to real Task/EmailMessage record URL.

**Open questions:**
- Cadence thresholds (14d/30d): confirm with ops team — may vary by segment (e.g. CAS may require tighter cadence).
- If a VIP has both SBK and CAS segments, which rank field determines Priority Book membership?
- Asset record: confirm whether KAMs ever log activities against Asset objects specifically, or exclusively against Contact.

---

## Component 5 — Tasks & Weekly Priority Contacts (WPC)

**Purpose:** Structured contact obligation tracker. WPC = a weekly list of VIPs the KAM is required to contact. Tasks are ad hoc items.

**Locked decisions:**
- Tag reads "Use Existing Rules" — the WPC rules/cadence logic is pre-defined (existing business rules apply, not custom-built in the console).
- Filter tabs: All · Tasks · WPC · Pending · Done
- Contact action available from the WPC row inline (no navigation away required)
- Do not extend WPC to Casino / Racing / DFS until statuses and in-row outreach are fully functional for SBK

**SF objects:** Task (standard) · WPC likely a custom or configured list view on Task/Activity

**Phase 1:** Mock WPC list seeded. Existing Salesforce task object — low Phase 1 build risk.
**Phase 2:** Live Task reads/writes. WPC status update writes back to Task.

**Open question:** How is the WPC list generated today — Agentforce, manager-assigned, or rule-based? This determines Phase 2 complexity.

---

## Component 6 — Upcoming Events

**Purpose:** VIP event inventory and invite management. Replaces the current Asana + Splash + Ticket Manager + Excel workflow.

**Locked decisions:**
- Actions via ⋮ contextual menu per event row: Manage Event · View RSVPs · Send Reminder (for hosted events) · Invite VIP · View Event · Remove (for standard events)
- Ticket Manager integration is the one existing ACE integration point — build on top of it, don't replace it.
- Attendance and RSVP tracking are Phase 2+ — not in Phase 1 mock scope.

**SF objects:** Custom Event object (or Campaign used for events — confirm with client). EventRelation or CampaignMember for RSVPs.

**Phase 1:** Mock event rows. No live Ticket Manager integration.
**Phase 2:** Requires event catalog source confirmation (Track B item).

**Open questions:**
- Where does the event catalog live today — Asana, Splash, manual spreadsheet, or is there already a Salesforce Event object?
- Ticket inventory: is this Ticket Manager API, or manually entered?

---

## Component 7 — Campaign Manager

**Purpose:** KAM-initiated bonus and promotion campaigns. Reduces the Excel-to-ACE manual workflow.

**Locked decisions:**
- Actions via ⋮ contextual menu per campaign: View Campaign · Add VIP · Duplicate · Edit Campaign · Preview · Delete Draft
- Campaign Manager handles KAM-issued bonuses. System-generated promotions are explicitly separate — they must NOT satisfy the KAM bonus cadence threshold.
- Bulk Bonus exists on SBK (Claude CSV process today). Casino has no current bulk path. DFS uses AdminWeb CSV. Racing has no ACE path. Phase 1 covers SBK shape only.

**SF objects:** Campaign (standard) · CampaignMember · Bonus object TBC

**Phase 1:** Mock campaign rows with status badges and ⋮ menus.
**Phase 2:** Live Campaign reads. Bonus issuance blocked on ACE integration (Track B).

**Open question:** Variable bulk bonus (Agentforce customises amounts per VIP before send) — confirm if this is Phase 2 or Phase 3.

---

## Component 8 — Birthdays This Week

**Purpose:** Surfaces VIPs with a birthday in the current calendar week so the KAM can send a personalised offer or message.

**Locked decisions:**
- Actions via ⋮ contextual menu: Send Celebration · View Profile · Log Note
- Sent state tracked per VIP (celebration sent / not yet sent)
- Source field (DOB) is on the Contact record — high confidence, standard SF field.

**SF objects:** Contact (Date of Birth field). Activity (for logging birthday outreach).

**Phase 1:** Mock birthday rows seeded with this-week dates.
**Phase 2:** Live DOB read from Contact. Low risk — standard field.

---

## Component 9 — VIP Feedback Pulse

**Purpose:** Surfaces unreviewed VIP feedback for the KAM to review and respond to. Prevents the 18k feedback corpus from sitting unactioned.

**Locked decisions:**
- Sentiment classification (Positive / Neutral / Negative) is Agentforce / Einstein — Phase 3.
- MVP card shows feedback text + review status only. No AI sentiment in Phase 1.
- Feedback Review Status (Unreviewed / Reviewed) is a KAM-set flag — simple field write.

**SF objects:** Custom Feedback object (or Survey object). Likely requires new data capture mechanism — red-rated in `data-points-validation.md`.

**Phase 1:** Mock feedback rows.
**Phase 2:** Requires a confirmed feedback capture platform (Salesforce Surveys, in-app, or third-party). This is a Track B item.

---

## Integration Dependencies by Phase

| Dependency | Component(s) affected | Phase | Track |
|---|---|---|---|
| Contact + Activity (Task/EmailMessage) read | LCT, WPC, My VIPs, Birthdays | 2 | A |
| `Book_GGR_Rank_Pct__c` on Contact | LCT Priority Book, NBA No Contact | 1→2 | A (mock Phase 1, live Phase 2) |
| Campaign exclusion flag on Activity | LCT (Last Contact definition) | 2 | A |
| Salesforce Case object (CS team process) | NBA CS Alert | 2 | B (process dependency) |
| DataPoints / Player Intelligence Engine | NBA Big Loss, RTA Big Swings, RTA Losing Streaks | 3 | B |
| Payment platform (withdrawals data) | RTA Payment Alerts | 2–3 | B |
| ACE Bonus issuance API | Campaign Manager, NBA Bonus Opp | 2 | B |
| Event catalog source (Ticket Manager / custom) | Upcoming Events | 2 | B |
| Feedback capture platform | VIP Feedback Pulse | 2 | B |
| Agentforce NBA use cases | All NBA dynamic scoring | 2–3 | B (Andy Geissler sign-off) |

---

## Phase 1 Scope (Committed — Weeks 1–4)

**What Phase 1 is:** A click-through sandbox demo with seeded mock data. No live org data. No Agentforce. No DataPoints. The gate is: a KAM can click the console and say the workflow is right.

**In Phase 1:**
- All 9 components render with mock/seeded data
- LCT Priority Book / Full Book two-section layout with mock Contact Source + Contact Note
- NBA 4 flat cards (hardcoded VIPs)
- My VIPs table (mock rows)
- All ⋮ contextual menus functional
- Log Contact modal
- Toast notifications

**Not in Phase 1:**
- Any live data reads/writes
- Agentforce NBA scoring
- DataPoints signals
- Ticket Manager integration
- Bonus issuance

---

## Open Questions — Consolidated

| # | Question | Owner | Blocks |
|---|---|---|---|
| 1 | Does the CS/HVP team create Salesforce Cases for every VIP interaction today? | FanDuel Ops | NBA CS Alert |
| 2 | Big Loss threshold — what is the exact dollar / multiplier rule? Who owns the definition? | FanDuel Product | NBA Big Loss, RTA Big Swings |
| 3 | Payment Alerts data source — withdrawals in DataPoints, payments platform, or elsewhere? | FanDuel Platform | RTA Payment Alerts |
| 4 | WPC list generation — Agentforce, manager-assigned, or rule-based? | FanDuel Ops | Tasks & WPC Phase 2 |
| 5 | Event catalog source — Asana, Splash, manual, or existing SF object? | FanDuel Events team | Upcoming Events Phase 2 |
| 6 | Feedback capture platform — does one exist, or does it need to be built? | FanDuel Product | VIP Feedback Pulse Phase 2 |
| 7 | Cadence thresholds (14d/30d) — do these vary by segment? | FanDuel Ops | LCT status logic |
| 8 | Multi-segment VIPs — if a VIP has SBK + CAS, which rank field (GGR vs NGR) determines Priority Book membership? | FanDuel Data | LCT Priority Book, NBA No Contact |
| 9 | Bonus object in Salesforce — custom object or standard Campaign/CampaignMember? | FanDuel CRM | Campaign Manager, My VIPs Last Bonus |
| 10 | DataPoints API access — confirmed, in progress, or not yet started? | FanDuel Technology / Andy Geissler | All Phase 3 components |

---

*Generated from: `console-v2.html` prototype · `all-components-user-stories.csv` · `cursor-figma-sales-console-spec.md` · design sessions Aug–Sep 2026*
*For questions on prototype decisions, contact the AI Experience Architect.*
