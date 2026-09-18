INSTRUCTIONS FOR GEMINI: Create a Google Sheet in my Drive titled "VIP KAM Console — User Stories". Sheet 1 named "User Stories" with columns: Story ID | Module | Persona | Title | As a... | I want to... | So that... | Acceptance Criteria | Priority (Must/Should/Could/Won't) | Effort (XS/S/M/L/XL) | Dependencies | Notes. Sheet 2 named "Priority Matrix" showing a 2x2 Impact vs Effort grid with story IDs placed in quadrants. Format: freeze row 1, bold headers, alternate row colors by module, color-code Priority column (Must=green, Should=yellow, Could=blue, Won't=gray). Populate Sheet 1 with ALL 25 stories below — do not skip any. Return the Sheet URL when done.

===========================================================
USER STORIES — VIP KAM CONSOLE
FanDuel · Salesforce Sales Console Custom Home Page · v1.0 · Aug 2026
===========================================================

MODULE: My VIPs Table

Story ID: US-01
Module: My VIPs Table
Persona: Taylor (SBK KAM)
Title: Book of Business Table
As a: Sportsbook KAM
I want to: See all VIPs in my book in a single table showing Name, Segment, Tier, State, GGR L30, Last Bonus Issued, Last Contact, Status, and Actions
So that: I can orient myself at the start of each day without switching between multiple Salesforce views
Acceptance Criteria:
  • Given I open the console, when the My VIPs tab loads, then all VIPs assigned to my territory appear within 3 seconds
  • Each row shows all 9 columns with correct data from the VIP's Salesforce record
  • Table defaults to insertion order (no sort applied) on first load
Priority: Must Have
Effort: L
Dependencies: VIP_Account__c object, Territory__c assignment, Apex getBookOfBusiness()
Notes: Core feature — no MVP without it. 10 rows per page default. Columns: Name, Segment, Tier, State, GGR L30, Last Bonus Issued, Last Contact, Status, Actions.

---

Story ID: US-02
Module: My VIPs Table
Persona: Taylor (SBK KAM)
Title: Sortable Columns
As a: KAM
I want to: Sort the VIP table by Name, Tier, GGR L30, Last Contact, or Last Bonus Issued
So that: I can quickly find the VIPs most relevant to my current task
Acceptance Criteria:
  • Clicking a column header sorts ascending; clicking again sorts descending
  • Active sort column shows a directional arrow indicator (▲/▼)
  • Sort state persists during the session but resets on page reload
Priority: Must Have
Effort: S
Dependencies: US-01
Notes: Client-side sort on loaded dataset. No Apex re-query needed if all records loaded at once.

---

Story ID: US-03
Module: My VIPs Table
Persona: Morgan (CAS KAM)
Title: Search and Filter Bar
As a: Casino KAM
I want to: Search by VIP name and access Salesforce list view filters for segment, tier, and status
So that: I can focus on a specific cohort without rebuilding list views from scratch
Acceptance Criteria:
  • Typing in the search input filters the visible table rows in real-time by VIP name
  • Filter button opens Salesforce OOTB list view filter panel (no custom pill filters)
  • Refresh button re-queries the server and updates the "Last refreshed" timestamp
Priority: Must Have
Effort: S
Dependencies: US-01
Notes: Leverages Salesforce OOTB filtering — no custom filter logic for MVP. Reduces customization scope significantly.

---

Story ID: US-04
Module: My VIPs Table
Persona: Taylor (SBK KAM)
Title: VIP Contact Card Modal
As a: KAM
I want to: Click a VIP's name and see their full profile in a modal — segment, tier, GGR, last bonus, birthday, last contact, LTV — without leaving the console
So that: I can prepare for outreach in under 30 seconds without navigating away to the VIP's full Salesforce record
Acceptance Criteria:
  • Clicking a VIP name opens a lightning-modal in under 1 second
  • Modal shows all profile fields plus an activity snapshot (Est. LTV, 30d Spend, Sessions/Wk, Avg Stake)
  • Modal includes action buttons: Contact, Issue Bonus, View Full Record, Invite to Event
Priority: Must Have
Effort: L
Dependencies: US-01, VIP_Account__c getRecord(), ActivityHistory, VIP_Bonus__c, VIP_Analytics__c
Notes: Implemented as lightning-modal (SLDS v2) for accessibility compliance. Multi-object query: VIP record + related Activity + Bonus + NBA. Highest UI complexity in this module.

---

Story ID: US-05
Module: My VIPs Table
Persona: All KAMs
Title: Status Pills
As a: KAM
I want to: See a color-coded status badge (Active / Deactivated / Suspended) on each VIP row
So that: Account status is immediately visible without clicking into any records
Acceptance Criteria:
  • Active = green pill | Deactivated = gray pill | Suspended = red pill
  • Status reflects VIP_Account__c.Status__c in real time
  • Pill text is uppercase and legible at small font sizes
Priority: Must Have
Effort: XS
Dependencies: US-01
Notes: Pure display component. No Apex needed beyond what US-01 already queries.

---

Story ID: US-06
Module: My VIPs Table
Persona: All KAMs
Title: Tier Badge with Shield Select
As a: KAM
I want to: See VIP tiers displayed with a clear visual hierarchy — including a special Shield Select icon for the highest tier
So that: I can immediately identify my most elite VIPs at a glance
Acceptance Criteria:
  • Shield Select renders as a shield SVG icon + "Shield Select" label in FanDuel blue (#0060a3)
  • Other tiers display as plain "Tier N" text (e.g., Tier 2, Tier 4)
  • Tier logic respects segment convention: SBK Tier 1=entry scaling up; CAS Tier 5=entry scaling down to Tier 1
Priority: Must Have
Effort: XS
Dependencies: US-01
Notes: VIP_Account__c.Tier__c picklist. Conditional rendering in LWC template. No Apex needed beyond US-01 query.

---

Story ID: US-07
Module: My VIPs Table
Persona: Taylor (SBK KAM)
Title: NBA Alert Icon on VIP Name
As a: KAM
I want to: See an amber alert icon next to a VIP's name when they have a pending Next Best Action
So that: I can prioritize those VIPs for contact without having to open the NBA queue separately
Acceptance Criteria:
  • NBA alert icon appears only when a linked NBA_Action__c record exists for the VIP and is not yet actioned
  • Hovering over the icon shows a tooltip: "NBA Action pending"
  • Icon loads after the table renders (lazy load) to avoid blocking the main table
Priority: Should Have
Effort: S
Dependencies: US-01, Einstein NBA license, NBA_Action__c custom object
Notes: Boolean hasNBA prop passed from parent query. Lazy-loaded after table renders to protect initial load performance.

---

Story ID: US-08
Module: My VIPs Table
Persona: Morgan (CAS KAM)
Title: Bulk Select and Bulk Actions
As a: Casino KAM
I want to: Select multiple VIPs with checkboxes and perform a bulk action (Contact Log, Bonus Issue, Status Update)
So that: Post-event follow-up for groups of VIPs doesn't require opening each record individually
Acceptance Criteria:
  • Checkbox per row plus select-all in table header
  • Bulk Action bar appears when one or more rows are selected, showing selected count and action buttons
  • Bulk Contact Log creates a Task record for each selected VIP | Bulk Bonus Issue opens a Screen Flow for batch issuance
Priority: Should Have
Effort: M
Dependencies: US-01, US-10, US-11
Notes: Requires shared state between vipDataTable and bulk action bar. Apex batch method for bulk Task creation. Governor limit risk at high VIP counts — cap bulk operations at 50 VIPs per batch.

---

MODULE: Agentforce Next Best Actions

Story ID: US-09
Module: Agentforce NBA
Persona: Taylor (SBK KAM)
Title: NBA Priority Queue
As a: KAM
I want to: See an AI-generated, prioritized list of VIPs I should contact today — with the reason for each recommendation
So that: I can focus my outreach on the highest-impact actions without spending time on manual analysis
Acceptance Criteria:
  • Queue displays up to 10 NBA cards ranked by AI priority score
  • Each card shows: VIP name, recommended action type (Retention, Upsell, Reactivation, etc.), and a one-line AI rationale
  • Cards are sourced from Einstein Next Best Action Recommendation records scoped to the KAM's territory
Priority: Must Have
Effort: XL
Dependencies: Einstein NBA license, NBA Strategy configuration in Salesforce, NBA_Action__c custom object
Notes: Highest implementation complexity. Requires Einstein NBA license + Strategy configuration. Use static/mock data for Phase 1 demo if license not confirmed. Full live version in Phase 2.

---

Story ID: US-10
Module: Agentforce NBA
Persona: Taylor (SBK KAM)
Title: Contact Log Quick Action
As a: KAM
I want to: Log a contact directly from the NBA card or VIP Contact Card in 2 clicks
So that: Activity logging happens immediately after each interaction and Last Contact data stays accurate
Acceptance Criteria:
  • "Contact" button opens a Log a Call quick action pre-populated with VIP name and KAM context
  • On save, a Task is created on the VIP record and Last Contact date updates in the table within 5 seconds
  • KAM can add a free-text note before saving
Priority: Must Have
Effort: M
Dependencies: US-09, US-04, Log a Call Quick Action configuration in Salesforce Setup
Notes: Uses NavigationMixin to invoke the quick action. The 5-second refresh requirement means the table must re-query or receive a wire refresh event after task creation.

---

Story ID: US-11
Module: Agentforce NBA
Persona: Taylor (SBK KAM)
Title: Bonus Issuance Flow
As a: KAM
I want to: Issue a bonus to a VIP from the console — selecting type (Bonus Bet, Postal Gift, Event), amount, with an automated compliance check — all in one flow
So that: Bonus issuance is fast, auditable, and doesn't require switching to a separate system or emailing ops
Acceptance Criteria:
  • "Bonus" button opens a Salesforce Screen Flow with steps: select type → enter amount → compliance check → confirm
  • If VIP is flagged Overbonused, the flow blocks issuance and shows the reason
  • On successful completion, a VIP_Bonus__c record is created and Last Bonus Issued updates in the table
Priority: Must Have
Effort: L
Dependencies: VIP_Bonus__c custom object, compliance rules modeled as Flow decision elements, bonus budget fields on VIP record, external bonus fulfillment integration (scope TBD)
Notes: Compliance check logic must be confirmed with the FanDuel compliance team. External fulfillment system connection is an open scoping question — may be Salesforce record creation only for Phase 1.

---

Story ID: US-12
Module: Agentforce NBA
Persona: Taylor (SBK KAM)
Title: NBA Dismiss Action
As a: KAM
I want to: Dismiss an NBA recommendation I've decided not to act on
So that: My queue stays clean and only shows actionable items
Acceptance Criteria:
  • Clicking "Dismiss" removes the card from the visible queue immediately
  • Apex method updates the Recommendation record (IsActionTaken = true, dismissal reason recorded)
  • Dismissed recommendation does not resurface for a configurable cooldown period (default: 7 days, set via Custom Metadata)
Priority: Should Have
Effort: S
Dependencies: US-09
Notes: Cooldown period configurable as Custom Metadata Type record. Dismiss reason dropdown (optional) helps refine the NBA strategy over time.

---

MODULE: Awareness / Alerts

Story ID: US-13
Module: Awareness
Persona: Taylor (SBK KAM)
Title: Big Swings Alert
As a: Sportsbook KAM
I want to: See an alert when a VIP in my book places a bet above a configurable threshold
So that: I can reach out proactively while the experience is top of mind and reinforce the relationship at a high-emotion moment
Acceptance Criteria:
  • Alert appears in the Awareness panel within 30 minutes of the wager being placed
  • Alert card shows: VIP name, bet amount, bet type (e.g., NFL parlay), and time of bet
  • KAM can tap "Contact" directly from the alert card to open the Log a Call quick action
  • Alert threshold is configurable as a Custom Metadata record (default: single wager > $1,000)
Priority: Should Have
Effort: XL
Dependencies: Platform Events infrastructure, wager data pipeline from FanDuel betting platform, Alert__c custom object
Notes: Highest data complexity item. Requires real-time integration from the betting platform via Platform Events or Change Data Capture. Confirm infrastructure feasibility with tech architect before scheduling in Phase 2.

---

Story ID: US-14
Module: Awareness
Persona: Taylor (SBK KAM)
Title: Payment Alerts
As a: KAM
I want to: See payment alerts — pending withdrawals, failed deposits, disputed charges — for VIPs in my book
So that: I can proactively reach out before the VIP contacts support and deliver white-glove service at a friction moment
Acceptance Criteria:
  • Alert appears within 1 hour of the payment event occurring
  • Alert card shows: VIP name, alert type (Withdrawal/Failed Deposit/Dispute), amount, and timestamp
  • KAM can take action (Contact or View Full Record) directly from the alert card
Priority: Should Have
Effort: XL
Dependencies: Payment processor webhook → Apex trigger → Platform Event → Alert__c record creation
Notes: Requires payment processor API integration. Alert__c is a custom object storing alert type, VIP link, amount, timestamp, and status. Confirm webhook availability with FanDuel payments team.

---

Story ID: US-15
Module: Awareness
Persona: Morgan (CAS KAM)
Title: Losing Streak Welfare Check Alert
As a: Casino KAM
I want to: See which VIPs are on a significant losing streak so I can perform a welfare check and offer appropriate support
So that: FanDuel meets its responsible gambling obligations while maintaining the VIP relationship through a difficult moment
Acceptance Criteria:
  • VIPs with 5 or more consecutive losing sessions surface as a Losing Streak alert
  • Alert shows streak length and GGR context (e.g., "-$820 across last 7 sessions")
  • Threshold is configurable via Custom Metadata
  • Alert framing uses welfare-first language — not commercial messaging
Priority: Should Have
Effort: L
Dependencies: VIP_Analytics__c with consecutive_losing_sessions__c field, RG team threshold sign-off, responsible gambling policy compliance review
Notes: Requires explicit sign-off from the RG team before implementation. Display language must be reviewed by compliance. Do not frame as a bonus opportunity.

---

MODULE: Reporting

Story ID: US-16
Module: Reporting
Persona: Taylor (SBK KAM)
Title: Self-Serve GGR Trend Charts
As a: KAM
I want to: See GGR trend charts for my book — week-over-week and month-over-month — without waiting for a monthly analytics report
So that: I can proactively identify performance trends and adjust my outreach strategy in real time
Acceptance Criteria:
  • Charts update daily (not real-time)
  • Shows book-level total GGR vs. prior period with delta percentage
  • Filterable by segment (SBK, CAS, DFS, FDR)
Priority: Should Have
Effort: M
Dependencies: Tableau Cloud or CRM Analytics (Einstein Analytics) license, VIP_Analytics__c rollup data
Notes: Integration method TBD — Tableau Cloud iframe embed via Connected App vs. CRM Analytics native dashboard. Confirm with tech architect. This is an open scoping question.

---

Story ID: US-17
Module: Reporting
Persona: Pat (KAM Director)
Title: Director Team Performance View
As a: KAM Director
I want to: Compare GGR contribution, contact frequency, bonus spend, and churn rate across all KAMs on my team
So that: I can make data-driven resource allocation, target-setting, and coaching decisions
Acceptance Criteria:
  • View is role-gated via Custom Permission Set (Director/Manager only)
  • Displays one row per KAM with all four metric columns
  • Filterable by segment and date range
Priority: Could Have
Effort: XL
Dependencies: kamTeamDashboard LWC, Salesforce sharing rules for cross-KAM data access, Custom Permission Set design, VIP_Account__c territory rollup
Notes: Requires record sharing design that grants managers read access to subordinate KAM territory records. Significant data access architecture work — Phase 3 candidate.

---

Story ID: US-18
Module: Reporting
Persona: Avery (KAM Manager)
Title: Live WPC Tracking
As a: KAM Manager
I want to: See each KAM's Weekly Priority Contact (WPC) progress updated daily
So that: I can intervene mid-week if a KAM is behind on their contact target before the deadline
Acceptance Criteria:
  • Per-KAM progress bar showing contacts made vs. weekly target
  • Color coding: green ≥80% of target, amber 50–79%, red below 50%
  • Progress resets weekly (Monday midnight)
  • Target configurable per KAM as a Custom Metadata record
Priority: Should Have
Effort: M
Dependencies: WPC_Flag__c custom field on Task object, weeklyPriorityContacts LWC, Custom Metadata Type for targets
Notes: Progress derived from Activity records (Tasks) created this week where WPC_Flag__c = true and OwnerId = KAM user. KAM view shows only their own bar; Manager view shows all KAMs on team.

---

MODULE: Onboarding & Lifecycle

Story ID: US-19
Module: Lifecycle
Persona: Taylor (SBK KAM)
Title: New VIP Assignment Notification
As a: KAM
I want to: Receive an in-console notification when a new VIP is assigned to my book — with their profile and a suggested first contact
So that: New VIPs receive a warm welcome within 24 hours of being tiered, setting the tone for the relationship
Acceptance Criteria:
  • Notification delivered to the KAM's bell icon within 1 hour of VIP_Account__c Owner or Territory change
  • Clicking the notification opens a New VIP Welcome Screen Flow pre-populated with VIP context (name, segment, tier, state)
  • Flow includes a first-contact checklist (e.g., review VIP profile, send welcome message, schedule first call)
Priority: Should Have
Effort: M
Dependencies: Custom Notification Type in Salesforce Setup, Flow trigger on VIP_Account__c Owner change, Screen Flow for welcome checklist
Notes: Custom Notification Type is a declarative Salesforce feature — no Apex needed for delivery. Flow trigger fires on record Owner or Territory__c field change.

---

Story ID: US-20
Module: Lifecycle
Persona: Taylor (SBK KAM)
Title: Churn Risk Cohort
As a: KAM
I want to: See which VIPs are at risk of churning or being demoted — before it happens — with a risk score and the primary signal driving the risk
So that: I can take targeted retention action while there is still time to change the trajectory
Acceptance Criteria:
  • Churn Risk cohort visible on Today dashboard as a widget and as a filterable cohort in My VIPs
  • Shows risk level (High/Medium) and primary driving signal (e.g., "GGR down 60% over 60d", "No response to last 3 contacts")
  • Risk threshold configurable via Custom Metadata
Priority: Could Have
Effort: XL
Dependencies: Predictive scoring model (Einstein Prediction Builder or custom Apex batch job), sufficient historical GGR and activity data, VIP_Analytics__c
Notes: Requires data science involvement and meaningful historical data volume. Phase 3 candidate. For Phase 1/2, a rules-based approximation (e.g., GGR down >50% + last contact >30d) can serve as a proxy.

---

MODULE: Infrastructure

Story ID: US-21
Module: Infrastructure
Persona: All
Title: Lightning App and Custom Home Page
As a: KAM
I want to: Have a dedicated Salesforce app (VIP KAM Console) with a custom home page set as my default landing experience
So that: I open Salesforce and immediately see my KAM console — no extra navigation required
Acceptance Criteria:
  • VIP KAM Console app is available in the App Launcher for KAM profile users
  • Custom Home Page loads as the default landing page when the app opens
  • Tab navigation structure configured per stakeholder sign-off (proposed: Today | My VIPs | Actions | Reports)
Priority: Must Have
Effort: M
Dependencies: Lightning App Builder, all LWC components built and deployed, App assignment to KAM permission set/profile
Notes: Declarative configuration in Lightning App Builder once LWC components are ready. Tab structure is a stakeholder decision — confirm before finalizing App config.

---

Story ID: US-22
Module: Infrastructure
Persona: Avery (Manager), Pat (Director)
Title: Role-Based View Defaults
As a: KAM Manager or Director
I want to: Have my console default to team-level views rather than a single KAM's book
So that: I have the correct data scope without needing to manually configure anything each session
Acceptance Criteria:
  • Custom Permission Set gates access to manager/director views
  • KAM users see only VIPs in their assigned territory
  • Manager/Director users see aggregated data across all subordinate KAM territories
Priority: Should Have
Effort: S
Dependencies: US-21, Custom Permission Set design, Salesforce sharing rules or territory hierarchy for cross-KAM data access
Notes: LWC checks for custom permission at runtime using @wire getPermissionSet or FeatureManagement.checkPermission(). Same LWC components, different data scope based on role.

---

Story ID: US-23
Module: Infrastructure
Persona: Taylor, Avery
Title: WPC Tracker Widget
As a: KAM
I want to: See my Weekly Priority Contact (WPC) progress for the current week
So that: I always know whether I am on track to hit my contact target without asking my manager
Acceptance Criteria:
  • Progress bar shows contacts made vs. weekly target for the current KAM
  • Updates daily (not real-time)
  • Manager view shows per-KAM bars for all team members
Priority: Should Have
Effort: M
Dependencies: WPC_Flag__c custom field on Task object, weeklyPriorityContacts LWC, Custom Metadata Type for target configuration
Notes: Distinct from US-18 (manager perspective) — this is the KAM's own view. Same LWC, different rendering based on user role.

---

Story ID: US-24
Module: Infrastructure
Persona: All KAMs
Title: Agentforce Companion Chat
As a: KAM
I want to: Access an AI chat assistant from anywhere in the console to ask natural language questions about my book
So that: I can get quick answers (e.g., "Who hasn't been contacted in 30 days?", "Which Casino VIPs are Shield Select?") without building reports
Acceptance Criteria:
  • Floating action button (FAB) opens a chat panel from any tab in the console
  • Chat supports natural language queries scoped to the KAM's book of business
  • Results include links to relevant VIP records
Priority: Could Have
Effort: L
Dependencies: Agentforce / Einstein Copilot license, custom topics and actions configuration for KAM domain, VIP data accessible to the AI agent
Notes: Requires Agentforce license and custom topic/action configuration to scope responses to VIP data. High value but license-dependent. Phase 2/3 candidate.

---

Story ID: US-25
Module: Infrastructure
Persona: All
Title: Toast Notifications and Error States
As a: KAM
I want to: See clear success and error feedback for every action I take in the console
So that: I always know whether my action succeeded, failed, or needs my attention — without ambiguity
Acceptance Criteria:
  • Every action (contact logged, bonus issued, filter applied, refresh triggered) shows a toast notification within 1 second
  • Success toasts are green with a confirmation message | Error toasts are red with a specific reason
  • Table and UI state reflect successful changes immediately — no manual refresh required
Priority: Must Have
Effort: S
Dependencies: All action-triggering LWC components (US-10, US-11, US-12, US-08)
Notes: Uses SLDS v2 toast notification pattern. Implement as a shared LWC utility imported by all action components. Consistent messaging vocabulary should be defined in a separate content spec.

===========================================================
PRIORITY MATRIX GUIDE (for Sheet 2)
===========================================================

High Impact + Low Effort (Do First): US-01, US-02, US-03, US-05, US-06, US-10, US-21, US-25
High Impact + High Effort (Plan Carefully): US-04, US-09, US-11, US-13, US-14
Low Impact + Low Effort (Fill In): US-07, US-12, US-19, US-22, US-23
Low Impact + High Effort (Reconsider): US-17, US-20, US-24
