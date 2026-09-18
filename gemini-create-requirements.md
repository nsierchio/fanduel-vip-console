INSTRUCTIONS FOR GEMINI: Create a Google Doc in my Drive titled "VIP KAM Console — Requirements & Roadmap". Apply proper heading styles (Heading 1 for document title, Heading 2 for section headings, Heading 3 for module names). Add a table of contents after the title. Section 1 should be formatted as a table with columns: ID | Feature | Module | Priority (MoSCoW) | Effort | Personas | LWC Component | Data Source | Dependencies | Notes. Color-code the Priority column: Must Have = green background, Should Have = yellow, Could Have = blue, Won't Have = gray. Section 2 as a numbered list with bold question headings. Section 3 as a table with columns: Phase | Feature IDs | Theme | Target | Notes. Add a document header with: "FanDuel VIP KAM Console | Salesforce Sales Console | Version 1.0 | Aug 2026". Add a "Document History" table at the end: Version | Date | Author | Changes. Copy every requirement, scoping question, and roadmap phase below verbatim — do not skip or summarize any content. Return the Google Doc link when done.

===========================================================
VIP KAM Console — Requirements & Roadmap
FanDuel · Salesforce Sales Console Custom Home Page · v1.0 · Aug 2026
===========================================================

PROJECT CONTEXT

Product: VIP KAM Console
Platform: Salesforce Sales Console — Custom Home Page (Lightning App Builder)
Component type: Lightning Web Components (LWC)
AI layer: Einstein Next Best Action (Agentforce)
Real-time signals: Salesforce Platform Events / Change Data Capture
Data: FanDuel VIP data warehouse → Salesforce custom objects (VIP_Account__c, VIP_Bonus__c, VIP_Analytics__c, NBA_Action__c)

Personas: Taylor (SBK KAM), Morgan (CAS KAM), Casey (DFS KAM), Reese (Racing KAM), Avery (KAM Manager), Pat (KAM Director)

Priority Framework (MoSCoW):
• Must Have — core to the product, no MVP without it
• Should Have — high value, ship in Phase 1 if feasible
• Could Have — nice to have, Phase 2
• Won't Have (now) — out of scope for current build

Effort Scale:
• XS = less than 1 day | S = 1–3 days | M = 3–7 days | L = 1–3 weeks | XL = 3+ weeks

===========================================================
SECTION 1: FEATURE REQUIREMENTS
===========================================================

MODULE: MY VIPs TABLE

R-01 | Book of Business Table | My VIPs | Must Have | L
Personas: Taylor, Morgan, Casey, Reese
LWC Component: vipDataTable
Data Source: VIP_Account__c filtered by KAM Territory__c
Dependencies: Apex controller getBookOfBusiness(), territory assignment setup
Notes: Core feature. Columns: Name, Segment, Tier, State, GGR L30, Last Bonus, Last Contact, Status, Actions. 10 rows/page, server-side or client-side pagination.

R-02 | Sortable Columns | My VIPs | Must Have | S
Personas: Taylor, Morgan
LWC Component: vipDataTable (column header click handler)
Data Source: Client-side sort on loaded dataset
Dependencies: R-01
Notes: Sort by Name, Tier, GGR L30, Last Contact, Last Bonus. No re-query needed if client-side.

R-03 | Search & Filter Bar | My VIPs | Must Have | S
Personas: All KAMs
LWC Component: vipTableFilters
Data Source: Client-side name filter; Salesforce OOTB list view for advanced filters
Dependencies: R-01
Notes: Search input (name filter) + Filter button (opens Salesforce List View filters). Refresh button re-calls Apex.

R-04 | VIP Contact Card Modal | My VIPs | Must Have | L
Personas: All KAMs
LWC Component: vipContactCard (lightning-modal)
Data Source: getRecord() on VIP_Account__c + ActivityHistory + VIP_Bonus__c + NBA_Action__c
Dependencies: R-01, R-11 (bonus data), custom objects
Notes: Opens on name click. Shows tier, GGR, last bonus, birthday, activity snapshot. Actions: Contact, Issue Bonus, View Full Record.

R-05 | Status Pills (Active / Deactivated / Suspended) | My VIPs | Must Have | XS
Personas: All KAMs
LWC Component: vipStatusBadge (child of vipTableRow)
Data Source: VIP_Account__c.Status__c picklist
Dependencies: R-01
Notes: Color-coded pill. Pure display component.

R-06 | Tier Badge (including Shield Select) | My VIPs | Must Have | XS
Personas: All KAMs
LWC Component: vipTierBadge
Data Source: VIP_Account__c.Tier__c
Dependencies: R-01
Notes: Shield Select renders with icon. Tier numbering differs by segment (SBK: 1=entry; CAS: 5=entry).

R-07 | NBA Alert Icon on VIP Name | My VIPs | Should Have | S
Personas: Taylor, Morgan
LWC Component: Inline in vipTableRow
Data Source: Boolean hasNBA — queried from NBA_Action__c linked to VIP
Dependencies: R-01, Einstein NBA license
Notes: Amber triangle icon + hover popover "NBA Action pending". Loaded lazily after table renders.

R-08 | Bulk Select + Bulk Actions Bar | My VIPs | Should Have | M
Personas: Morgan, Taylor
LWC Component: vipDataTable + vipBulkActionBar
Data Source: Selected VIP IDs array; Apex for batch operations
Dependencies: R-01, R-11, R-09
Notes: Checkbox per row + select all. Bulk actions: Contact Log, Bonus Issue, Status Update. Disable until 1 or more rows selected.

MODULE: AGENTFORCE NEXT BEST ACTIONS

R-09 | NBA Priority Queue | NBA | Must Have | XL
Personas: Taylor, Morgan, Avery
LWC Component: agentforceNBAQueue
Data Source: Einstein Next Best Action — Recommendation object + Strategy
Dependencies: Einstein NBA license, NBA Strategy configuration, NBA_Action__c custom object
Notes: Displays AI-prioritized list of VIPs to contact. Each card shows VIP name, action type, AI rationale, priority rank. Highest complexity item — requires Einstein NBA setup.

R-10 | Contact Log Quick Action (from NBA card) | NBA | Must Have | M
Personas: All KAMs
LWC Component: vipContactCard + NavigationMixin
Data Source: Task object — creates ActivityHistory record on VIP
Dependencies: R-09, R-04, Log a Call quick action configuration
Notes: 2-click contact log. Must update Last Contact date in table immediately after logging.

R-11 | Bonus Issuance Flow | NBA / My VIPs | Must Have | L
Personas: Taylor, Morgan
LWC Component: Screen Flow invoked via lightning-flow
Data Source: VIP_Bonus__c — creates bonus record; compliance check via Flow decision element
Dependencies: VIP_Bonus__c custom object, compliance rules as Flow decisions, bonus budget fields
Notes: Selects type (Bonus Bet, Postal Gift, Event), amount, runs compliance check. Overbonused flag set by compliance. Connects to external bonus fulfillment system (scope TBD with tech architect).

R-12 | NBA Dismiss Action | NBA | Should Have | S
Personas: All KAMs
LWC Component: agentforceNBAQueue
Data Source: Apex updates Recommendation record (IsActionTaken=true, dismissed)
Dependencies: R-09
Notes: Removes card from queue. Dismissed recommendation should not re-surface for configurable cooldown period. Default cooldown: 7 days, set via Custom Metadata record.

MODULE: AWARENESS / ALERTS

R-13 | Big Swings Alert Widget | Awareness | Should Have | XL
Personas: Taylor
LWC Component: bigSwingsAlert
Data Source: Platform Event or Change Data Capture on wager records; requires real-time integration from betting platform
Dependencies: Platform Events infrastructure, wager data pipeline, threshold configuration
Notes: Threshold for "big swing" must be configurable (e.g., single wager over $1,000). Highest data complexity item — confirm integration feasibility with tech architect before scheduling.

R-14 | Payment Alerts Widget | Awareness | Should Have | XL
Personas: Taylor, Morgan
LWC Component: paymentAlerts
Data Source: Platform Event from payment processor webhook → Apex trigger → Alert__c custom object
Dependencies: Payment processor API, Platform Events, Alert__c object
Notes: Covers withdrawals, failed deposits, disputes. 1-hour SLA on alert delivery. KAM can act directly from alert card.

R-15 | Losing Streak Widget | Awareness | Should Have | L
Personas: Morgan (CAS KAM primary)
LWC Component: losingStreakAlert
Data Source: VIP_Analytics__c — consecutive_losing_sessions__c field (batch or real-time)
Dependencies: Session/wager data pipeline, RG threshold configuration, Responsible Gambling policy sign-off
Notes: Requires RG team input on threshold (e.g., 5 consecutive losing sessions). Display must be RG-compliant — welfare framing, not purely commercial.

MODULE: REPORTING

R-16 | Tableau Embedded Reports | Reports | Should Have | M
Personas: Taylor, Avery, Pat
LWC Component: tableauEmbed (or CRM Analytics iframe)
Data Source: Tableau Cloud or CRM Analytics dashboard
Dependencies: Tableau license or CRM Analytics license; Salesforce Connected App for embed auth
Notes: 4 views: Overview, Spend Trends, Segments, Retention. Integration method TBD (direct embed vs. middleware). Confirm with tech architect.

R-17 | Last Contact Distribution Summary | Reports | Could Have | S
Personas: Avery, Pat
LWC Component: contactDistribution (child of Reports page)
Data Source: Apex aggregation of ActivityHistory — computed from ALL_VIPS dataset, no additional query needed
Dependencies: R-01 (reuses loaded VIP data)
Notes: 3-bucket summary: 20 days or fewer (on track), 21–35 days (needs attention), more than 35 days (overdue). Simple bar chart.

R-18 | Manager/Director Team View | Reports | Could Have | XL
Personas: Avery, Pat
LWC Component: kamTeamDashboard (role-gated via custom permission)
Data Source: VIP_Account__c aggregated by Territory__c.KAM__c; ActivityHistory rollups; VIP_Bonus__c spend
Dependencies: Custom permission set for manager role, multi-KAM data access (sharing rules)
Notes: Shows per-KAM GGR, contact compliance, bonus spend. Requires record sharing design that allows managers to view all subordinate KAM records.

MODULE: ONBOARDING & LIFECYCLE

R-19 | New VIP Assignment Notification | Lifecycle | Should Have | M
Personas: All KAMs
LWC Component: In-app notification via Bell icon / Custom Notification Type
Data Source: Flow trigger on VIP_Account__c Owner change — sends Custom Notification to new KAM
Dependencies: Custom Notification Type setup, territory assignment automation
Notes: Notification opens a "New VIP Welcome" Screen Flow with pre-populated VIP context and a first-contact checklist.

R-20 | Churn Risk Cohort | Lifecycle | Could Have | XL
Personas: Taylor, Morgan, Avery
LWC Component: churnRiskWidget (Today dashboard + My VIPs filter)
Data Source: Einstein Prediction Builder or batch Apex scoring on VIP_Analytics__c trends
Dependencies: Predictive scoring model, sufficient historical data, Einstein license or custom Apex
Notes: Risk score + primary signal (e.g., "GGR down 60% over 60d"). Configurable threshold. Phase 2 candidate — requires data science involvement.

MODULE: GLOBAL / INFRASTRUCTURE

R-21 | Lightning App + Custom Home Page | Infrastructure | Must Have | M
Personas: All
LWC Component: Lightning App Builder page composition
Data Source: N/A — configuration
Dependencies: All LWC components
Notes: Configure the VIP KAM Console as a Lightning App with Custom Home Page. Assign to KAM profile/permission set. Tab structure: Today | My VIPs | Actions | Reports (ALT LO proposed structure) — pending stakeholder sign-off.

R-22 | Role-Based View Defaults | Infrastructure | Should Have | S
Personas: Avery (Manager), Pat (Director) vs. KAMs
LWC Component: Conditional rendering via @wire getPermissionSet or custom permission
Data Source: Custom Permission Set
Dependencies: R-21, permission set design
Notes: KAMs see their book scoped to their territory. Managers/Directors see team-wide data. Same LWC, different data scope based on role.

R-23 | WPC Tracker | Actions | Should Have | M
Personas: All KAMs, Avery
LWC Component: weeklyPriorityContacts
Data Source: Activity records with WPC_Flag__c = true, created this week, by KAM
Dependencies: WPC_Flag__c custom field on Task, WPC target configuration
Notes: Progress bar per KAM (manager view) or single bar (KAM view). Resets weekly. Target configurable as Custom Metadata Type record.

===========================================================
SECTION 2: OPEN SCOPING QUESTIONS
===========================================================

1. Bonus Fulfillment Integration — Does the bonus issuance flow connect to an external fulfillment system (FanDuel promo engine) or does it just create a record in Salesforce for ops to process? This affects R-11 complexity significantly. If external, we need an Apex callout to the fulfillment API and error handling for failures. If internal only, it's a VIP_Bonus__c record creation with no outbound integration.

2. Tableau vs. CRM Analytics — Which reporting embed technology will be used? Direct Tableau Cloud iframe (requires Connected App OAuth) or CRM Analytics (native Salesforce, requires additional license)? Integration method affects R-16 build complexity and license costs. Confirm with tech architect before scheduling Phase 1 reporting work.

3. Real-Time Signals Infrastructure — Do Platform Events or Change Data Capture already exist for wager and payment data? If not, R-13 and R-14 require a new integration layer from the FanDuel betting and payments platforms into Salesforce. This is the highest-risk dependency in the roadmap. Needs confirmation from a data/platform architect before committing R-13 and R-14 to any phase.

4. Einstein NBA License — Is Einstein Next Best Action included in the current Salesforce license tier? R-09 (NBA Queue) depends entirely on this. If not licensed, a rules-based NBA mock (based on days since last contact + GGR threshold) is needed for Phase 1. Confirm with Salesforce AE or license admin.

5. Racing KAM (Reese) — TVG and FanDuel Racing are separate systems. How will Reese's VIP data be surfaced in the Salesforce console? Is there an existing data pipeline from TVG to Salesforce, or will this require a new integration? Racing KAMs are a smaller cohort — confirm whether this is in scope for Phase 1 or deferred.

6. RG Threshold Ownership — Who owns the responsible gambling thresholds for losing streak alerts (R-15)? This requires explicit sign-off from the RG/compliance team before implementation. The feature should not ship without approved threshold values and approved display language.

7. Multi-Segment VIP Data Model — A VIP can belong to multiple segments (SBK + CAS, DFS + SBK, etc.). How is this currently modeled in the Salesforce org? Options: junction object (VIP_Segment__c), multi-select picklist on VIP_Account__c, or separate VIP records per segment. This affects how segment pills and segment-specific filters are implemented in the table (R-01, R-03).

===========================================================
SECTION 3: PHASED ROADMAP
===========================================================

Phase 1 — Foundation (8–12 weeks)
Feature IDs: R-01, R-02, R-03, R-04, R-05, R-06, R-09 (mock), R-10, R-11, R-21, R-22
Theme: Core book of business view + basic NBA queue (mock data) + contact/bonus actions
Target: Internal demo + stakeholder review
Notes: R-09 uses static/mock NBA data if Einstein license not confirmed. Focus on KAM daily workflow (Taylor, Morgan). Establish custom objects (VIP_Account__c, VIP_Bonus__c) and Apex data layer. Lightning App scaffolding.

Phase 2 — Intelligence (6–8 weeks after Phase 1)
Feature IDs: R-07, R-08, R-09 (live), R-12, R-13, R-14, R-15, R-16, R-19, R-23
Theme: Real-time signals + live NBA + bulk actions + reporting embed + notifications
Target: Pilot with 2–3 KAMs
Notes: Requires Platform Events infrastructure confirmation (R-13, R-14) before scheduling. Einstein NBA license required for R-09 live. R-15 requires RG team sign-off before implementation begins.

Phase 3 — Scale (Post-pilot, timeline TBD)
Feature IDs: R-17, R-18, R-20
Theme: Manager/Director views + churn prediction + advanced reporting distribution
Target: Full KAM team rollout
Notes: R-20 (churn risk) requires data science involvement and sufficient historical GGR/activity data. R-18 (team view) requires Salesforce sharing rules architecture for cross-KAM data access. Confirm data readiness before committing Phase 3 timelines.

Out of Scope (current build): Racing KAM TVG integration, mobile native app, VIP self-service portal, SMS/push notification channels.

===========================================================
DOCUMENT HISTORY (add to end of document)
===========================================================

Version 1.0 | Aug 2026 | FanDuel VIP KAM Team | Initial draft — 23 requirements, 7 scoping questions, 3-phase roadmap
