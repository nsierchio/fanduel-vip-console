# Cursor Instructions — FanDuel VIP KAM Console SLDS v1 Rebuild

## What you are building
Rebuild the FanDuel VIP KAM Console prototype (`index.html`) as a new file (`index-lightning-blue.html`) that accurately matches the **Salesforce Lightning Design System v1 (SLDS v1)** — the design system used in the target Salesforce org.

This is a high-fidelity HTML prototype for stakeholder demos. It is not a Salesforce LWC app yet — it is a static HTML/CSS/JS file that looks and behaves like a real Salesforce Lightning Console App.

---

## Figma files — use your Figma MCP to read these

### 1. KAM Console Design Spec
**File key:** `hheGjrO7wTbtXbZhn0glaR`
Contains: 9 component spec pages (pages 05–13), 14 journey maps.
Components specified: NBA Queue, Tasks & WPC, Onboarding & Offboarding, Last Contact Tracker, Real-Time Awareness, Upcoming Events, Campaign Manager, Birthdays This Week, VIP Feedback Pulse.
Use this to understand: component layouts, data fields, interaction flows, annotation notes.

### 2. SLDS v1 Component Reference
**File key:** `5dgFdCHB6FGjfOPAZEDNVK`
**File name:** SLDS Components — Web v1
This is the official Salesforce SLDS v1 Figma spec. Use it to get exact component anatomy, spacing, color, and typography for every component you build.

Key nodes to reference:
- `1136:3613` — Activity Timeline (use for contact history / activity feed)
- Browse other pages for: Global Nav, Tabs, Cards, Data Tables, Buttons, Badges, Modals, Forms, List Views

**How to use:** Call `get_metadata` on a page or node to find what's there, then call `get_design_context` on specific nodes to get component specs before implementing.

---

## Target Salesforce org context
- **Org:** FanDuel Group sandbox (`fanduelgroup--aceai2.sandbox.lightning.force.com`)
- **Edition:** Unlimited Edition
- **Release:** Summer '26 / API 63.0
- **Active theme:** Lightning Blue (standard Salesforce default — NOT a custom theme)
- **SLDS version:** v1 — Salesforce Cosmos (SLDS v2) is NOT enabled

---

## SLDS v1 Lightning Blue Design Tokens

Use these exact values throughout. Do not invent custom values.

```
/* Colors */
--nav-bar-bg:        #0b5cab;   /* Global header background — Lightning Blue theme */
--brand-primary:     #0176d3;   /* Buttons, links, active states */
--brand-dark:        #0a5d9c;   /* Hover state for brand elements */
--page-bg:           #f3f2f2;   /* App body background */
--card-bg:           #ffffff;
--card-border:       #dddbda;
--table-header-bg:   #f3f2f2;
--text-primary:      #181818;
--text-secondary:    #706e6b;
--text-label:        #3e3e3c;
--text-placeholder:  #9b9b9b;
--border-color:      #dddbda;
--row-hover:         #f3f9ff;

/* Semantic */
--color-success:     #2e844a;
--color-error:       #ba0517;
--color-warning:     #dd7a01;
--color-info:        #0176d3;

/* Spacing (4px base unit) */
--space-xx-small:    0.25rem;   /* 4px */
--space-x-small:     0.5rem;    /* 8px */
--space-small:       0.75rem;   /* 12px */
--space-medium:      1rem;      /* 16px */
--space-large:       1.5rem;    /* 24px */
--space-x-large:     2rem;      /* 32px */

/* Component heights */
--global-nav-height: 3rem;      /* 48px */
--context-bar-height: 3rem;     /* 48px */
--page-header-height: 3.5rem;   /* 56px */
--button-height:     2rem;      /* 32px */
--input-height:      2rem;      /* 32px */
--table-header-row:  2.5rem;    /* 40px */
--table-data-row:    3rem;      /* 48px */
--card-header-height: 3.25rem;  /* 52px */

/* Typography */
--font-family:       'Salesforce Sans', Arial, sans-serif;
--font-size-x-small: 0.6875rem; /* 11px */
--font-size-small:   0.75rem;   /* 12px */
--font-size-body:    0.8125rem; /* 13px */
--font-size-medium:  0.875rem;  /* 14px — base */
--font-size-large:   1rem;      /* 16px */
--font-size-x-large: 1.25rem;   /* 20px */

/* Radius */
--radius-small:      0.25rem;   /* 4px */
--radius-medium:     0.5rem;    /* 8px */
--radius-circle:     50%;

/* Shadows */
--shadow-card:       0 2px 2px rgba(0,0,0,.06);
--shadow-dropdown:   0 8px 32px rgba(0,0,0,0.18);
```

---

## Page Shell — Lightning Console App Structure

The page is a **Lightning Console App** (workspace tab pattern). Structure top to bottom:

```
┌─────────────────────────────────────────────────────────────┐
│  GLOBAL NAV BAR  (48px | #0b5cab | sticky z:8000)           │
│  [waffle] [FanDuel VIP KAM Console] ··· [? ⚙ 🔔 avatar]    │
├─────────────────────────────────────────────────────────────┤
│  WORKSPACE TABS  (48px | white | sticky z:7000)             │
│  [📋 My VIP Playbook ✕] [+ New Tab]                        │
├─────────────────────────────────────────────────────────────┤
│  PAGE HEADER  (56px | white | sticky z:6000)                │
│  [icon] My VIP Playbook  [Persona: Taylor · SBK KAM]  [Filters] [Export] [+ Log Contact] │
├─────────────────────────────────────────────────────────────┤
│  CONTENT BODY  (padding:16px | bg:#f3f2f2)                  │
│                                                             │
│  ┌──────────────────────┐ ┌────────────┐ ┌──────────────┐  │
│  │   MAIN COLUMN        │ │  ACTIVITY  │ │  AGENT       │  │
│  │   (fluid ~44%)       │ │  (~28%)    │ │  (~28%)      │  │
│  │                      │ │            │ │              │  │
│  │  VIP Playbook card   │ │  NBA Queue │ │  Agentforce  │  │
│  │  (Overview | VIPs)   │ │            │ │  Companion   │  │
│  │                      │ │  Tasks &   │ │  Chat        │  │
│  │  Real-Time Awareness │ │  WPC       │ │              │  │
│  │                      │ │            │ │              │  │
│  │  Onboarding &        │ │  Upcoming  │ │              │  │
│  │  Offboarding         │ │  Events    │ │              │  │
│  │                      │ │            │ │              │  │
│  │  Last Contact        │ │  Campaign  │ │              │  │
│  │  Tracker             │ │  Manager   │ │              │  │
│  └──────────────────────┘ └────────────┘ └──────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## Component Specs

### Global Nav Bar
- Height: 48px, background: `#0b5cab`, position: sticky top:0
- Left: 9-dot waffle/app launcher button, then divider, then "FanDuel VIP KAM Console" text (white, 700, 13px)
- Right: Help (?), Setup (⚙), Notifications (🔔 with red badge), user avatar "TK" circle
- All icons: white, 17px

### Workspace Tabs Bar
- Height: 48px, white background, `1px solid #dddbda` bottom border, sticky top:48px
- Active tab: white bg, `#0176d3` text, `3px solid #0176d3` bottom border, icon + label + ✕
- Inactive tabs: `#706e6b` text, transparent bottom border
- Show: "My VIP Playbook" as active open tab

### Page Header (SLDS slds-page-header)
- Height: 56px min, white, `1px solid #dddbda` bottom border, sticky top:96px
- Left: 28px square object icon (blue), title "My VIP Playbook" (20px bold), meta "47 VIPs · Sportsbook KAM · Taylor Kim" (12px #706e6b)
- Right: Filters button (neutral), Export button (neutral), Log Contact button (brand blue)

### VIP Playbook Card (slds-card)
Tabbed card with two panes: **Overview** and **My VIPs**

**Overview pane:**
- 4 metric tiles in a row: Total VIPs (47), Active (44), Shield Select (6), Avg GGR L30 ($2,840)
- Each tile: label (12px #706e6b uppercase), value (24px bold #181818)
- Below: "View Reports" toggle that expands a Tableau placeholder section (4 tabs: Overview, Spend Trends, Segments, Retention — show placeholder iframe with "Tableau Report" label)

**My VIPs pane (slds-table):**
- Toolbar: search input + quick filter tabs (All VIPs / Overdue Contact / Overbonused / Shield Select)
- Table columns: ☐ | Name | Segment | Tier | GGR L30 | Last Contact | Last Bonus Issued | Status | Actions
- Column specs:
  - Name: bold link (`#0176d3`), NBA amber triangle `⚠` with hover tooltip "NBA Action pending" if applicable
  - Segment: plain text — SBK · CAS · DFS · FDR (abbreviations only, comma-separated)
  - Tier: text, Shield Select shows gold shield SVG icon + label
  - GGR L30: green if positive (`#2e844a` bold), red if negative (`#ba0517` bold)
  - Last Contact: red if >20 days, amber if 11–20 days, gray if ≤10 days
  - Last Bonus Issued: amount bold + type · date below; if overbonused: "$0 [Overbonused badge] / type · date"
  - Status: SLDS badge — ACTIVE (green bg), DEACTIVATED (gray), SUSPENDED (amber)
  - Actions: hidden until row hover — Contact button (neutral) + ⋮ more button
- Row height: 48px, hover: `#f3f9ff`
- Multi-select: checkbox col, bulk action bar appears when rows checked
- Pagination: "Showing 1–10 of 47 VIPs" + prev/next/page buttons
- Bonus types: Bonus Bet · Postal Gift · Event

### Real-Time Awareness (slds-card with 3 tabs)
Tabs: Big Swings | Payment Alerts | Losing Streaks
- Each tab shows a list of VIPs with event details
- Big Swings: winner/loser rows with $ amount, color-coded
- Payment Alerts: VIP name, alert type (Withdrawal Hold, Decline), amount, time
- Losing Streaks: VIP name, sessions, total loss, urgency level
- All labeled **[Mock]**

### Onboarding & Offboarding (slds-card with 2 tabs)
Tabs: New VIPs | Churn Risk
- Table: Name | Segment | Start Date | KYC Status | Assigned KAM | Status
- KYC badge: Verified (green), Pending (amber), Incomplete (red)
- Labeled **[Mock]**

### Last Contact Tracker (slds-card)
- Header: "Last Contact Tracker" + "20-Day Cadence"
- Progress summary: "62% in cadence (5/8 VIPs)"
- List items: urgency dot (red/amber/green) + VIP name + days ago + Contact button
- Red = >20 days, Amber = 11–20 days, Green = ≤10 days

### NBA Queue — Next Best Action (slds-card)
- Header: Agentforce icon + "Next Best Action" + red badge "4 Actions" + "AI-prioritized" label
- Each card: expandable (collapsed by default)
  - Collapsed: left color bar (red/amber/green/teal) + action type bold + VIP name link + tier chip + urgency tag + 1-line summary + chevron ›
  - Expanded: meta row (Segment, Tier, GGR 30d, Last Contact) + 3 AI insight bullets (green/amber/blue left border) + action buttons (Contact, Send Bonus, View Profile)
- Action types: Birthday Alert (green), Churn Risk (red), No Contact (amber), New Feedback (teal)

### Tasks & WPC (slds-card with 2 tabs)
Tabs: My Tasks | Weekly Priority Contacts
- My Tasks: checkbox list, task title, due date, VIP link
- WPC: list of VIPs to contact this week, mark complete, segment chip, days since contact
- Progress bar: "X of Y complete"

### Upcoming Events (slds-card)
- Event rows: event name, date, type (Hosted/Invited), VIP count or RSVP status
- Action: Manage / RSVP button per row

### Campaign Manager (slds-card)
- Campaign rows: name, segment target, status badge (Active/Draft/Scheduled), VIP count, action
- Status badges: ACTIVE (green), DRAFT (gray), SCHEDULED (blue)

### Agentforce Companion Chat (slds-card, 3rd column)
- Header: Agentforce icon + "Agentforce Companion" + expand/collapse icon
- Chat interface: message bubbles, input field, send button
- Show pre-seeded conversation showing KAM asking about a VIP and getting an AI response
- FAB (floating action button): when companion is collapsed, show a floating circular button bottom-right with Agentforce icon

### VIP Contact Card Modal (slds-modal)
- Triggered by clicking VIP name
- Full-width modal: VIP name, tier, segment, state
- Sections: Overview stats, Contact History (Activity Timeline — use SLDS Activity Timeline pattern from node `1136:3613`), Bonus History, Notes
- Footer: Close, Log Contact, Send Bonus

### Log Contact Modal (slds-modal)
- Form: Contact Type (Call/Email/In-Person — slds-select), Notes (slds-textarea), Date (slds-input date), Duration
- Footer: Cancel, Save

### Notification Panel (dropdown from bell icon)
- Dropdown from nav bell icon
- List of notifications with type dots (red/amber/green/blue) + title + description + timestamp
- Mark all as read link, View all link

---

## Persona context (use in mock data)
- **Taylor Kim (TK)** — Sportsbook KAM (primary persona, logged-in user)
- **Morgan** — Casino KAM
- **Casey** — DFS KAM
- **Reese** — Racing KAM
- **Avery** — KAM Manager
- **Pat** — KAM Director

---

## Mock data requirements
- At least 8 VIP rows in the My VIPs table with varied: names, segments, tiers (including 2x Shield Select), GGR (+/-), last contact dates (spanning <3 days to 25+ days), bonus data (including 1 overbonused), statuses
- At least 4 NBA action cards (1 Birthday, 1 Churn Risk, 1 No Contact, 1 New Feedback)
- At least 4 Last Contact Tracker entries
- At least 3 Birthdays This Week entries
- At least 3 WPC items (1 completed)
- Demo label `[Mock]` on all data-dependent sections (small gray pill)

---

## Technical requirements
- Single HTML file — no external JS frameworks, no build step
- SLDS CDN: `https://cdnjs.cloudflare.com/ajax/libs/design-system/2.24.2/styles/salesforce-lightning-design-system.min.css`
- Salesforce Sans font (fallback: Arial, sans-serif)
- Sticky nav bar + workspace tabs + page header (three sticky layers)
- Console tabs must be clickable (basic JS toggle) — no routing needed
- VIP name links open the VIP Contact Card Modal
- Log Contact button opens Log Contact Modal
- NBA cards expand/collapse on click
- Notification panel toggles from bell icon
- Tab switching works within each card (Overview/My VIPs, Big Swings/Payments/Streaks, etc.)
- GGR colors, Last Contact urgency, badge colors all applied via JS logic (not hardcoded per row)
- Label every mock data section with `[Mock]` demo badge
- No password gate needed for this file

---

## Files in this project
- `index.html` — original prototype (DO NOT MODIFY — reference only)
- `index-lightning-blue.html` — your output file (overwrite/improve this)
- `build-roadmap.md` — 4-phase build plan
- `data-points-validation.md` — data fields by component
- `all-components-user-stories.csv` — 57 user stories
- `user-journey-maps.md` — 4 journey maps

---

## Definition of done
- [ ] Page opens in browser and looks like a Salesforce Lightning Console App
- [ ] All 3 columns render correctly at 1440px+ width
- [ ] Global nav bar is `#0b5cab` (Lightning Blue) with correct icons
- [ ] Workspace tab is visible and active
- [ ] VIP table shows 8+ rows with all column treatments (GGR color, urgency, badge, Shield icon, NBA flag, overbonused)
- [ ] NBA Queue shows 4 expandable cards
- [ ] Agentforce Companion chat column renders
- [ ] All tabbed cards switch tabs correctly
- [ ] VIP Contact Card modal opens on name click
- [ ] All SLDS v1 design tokens applied — no custom colors outside the token set above
- [ ] [Mock] labels on all data sections
- [ ] No console errors
