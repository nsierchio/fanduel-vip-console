# Cursor → Figma Prompt — VIP KAM Console Components + Screens

Paste this entire prompt into Cursor chat. Cursor will use Figma MCP to build editable components and screen frames.

---

```
You are working in Figma using the Figma MCP. Build a component library and screen frames for the FanDuel VIP KAM Console. Follow all specifications exactly. Create all items on a page named "KAM Console – Components & Screens".

---

DESIGN TOKENS

Colors:
- primary: #0060a3
- primary-dark: #004a80
- primary-light: #eff6ff
- text-primary: #1e1e1e
- text-secondary: #475569
- text-muted: #94a3b8
- bg-page: #f3f3f3
- bg-card: #ffffff
- bg-header: #f8fafc
- border: #e5e5e5
- border-input: #d0d5dd
- green-bg: #d1fae5 | green-text: #065f46
- yellow-bg: #fef3c7 | yellow-text: #92400e
- red-bg: #fee2e2 | red-text: #991b1b
- blue-link: #0b5fc5

Typography (font: Salesforce Sans, fallback Arial):
- label-xs:  10px / SemiBold / uppercase / letter-spacing 0.06em
- body-sm:   12px / Regular
- body-base: 13px / Regular
- body-md:   14px / Regular
- label-sm:  11px / Bold / uppercase / letter-spacing 0.05em
- value-sm:  11px / Bold
- value-md:  12px / Bold
- name-link: 12px / SemiBold / underline / color blue-link

Spacing unit: 4px base. Common: 4, 8, 12, 16, 24, 32px.
Border radius: pill=999px, sm=4px, md=6px, lg=8px.

---

PHASE 1 — COMPONENTS

Create a component section at the top of the page with a label "Components" (Inter Bold 20px, #1e1e1e).
Lay components out in a horizontal auto-layout row with 48px gap, wrapping.

─────────────────────────────────────
COMPONENT 1: Status Pill
─────────────────────────────────────
Name: "Status Pill"
Three variants: Active | Deactivated | Suspended

Each variant:
- Auto-layout horizontal, padding 2px 8px, border-radius 999px
- Font: 10px Bold uppercase, letter-spacing 0.05em
- Active:      bg #d1fae5, text #065f46, label "ACTIVE"
- Deactivated: bg #f1f5f9, text #64748b, label "DEACTIVATED"
- Suspended:   bg #fee2e2, text #991b1b, label "SUSPENDED"

─────────────────────────────────────
COMPONENT 2: Tier Badge
─────────────────────────────────────
Name: "Tier Badge"
Two variants: Numbered | Shield Select

Numbered variant:
- Text only: "Tier 2" — font 12px SemiBold, color #334155
- No background or border

Shield Select variant:
- Horizontal auto-layout, gap 4px, align center
- Shield icon: 11×13px, fill #0060a3 (opacity 15% fill, stroke #0060a3 1.3px)
  Shape: pentagon/shield — path M7 1 L1.5 3.5 V8 C1.5 11.3 3.8 14.2 7 15 C10.2 14.2 12.5 11.3 12.5 8 V3.5 Z
  Checkmark inside: stroke #0060a3, stroke-width 1.3, linecap round
- Label: "Shield Select" — 11px Bold, color #0060a3

─────────────────────────────────────
COMPONENT 3: NBA Alert Icon
─────────────────────────────────────
Name: "NBA Alert"
- Triangle SVG: 13×13px
  Path: M8 1.5 L1.5 13 H14.5 Z — fill #fef3c7, stroke #d97706, stroke-width 1.3
  Dot at bottom center: circle r=0.75, fill #d97706
  Vertical bar: M8 5.5 V9 — stroke #d97706 stroke-width 1.5 linecap round
- Tooltip label (shown as a separate frame below for reference): bg #1e293b, text #fff, 10px SemiBold, border-radius 4px, padding 4px 8px, "NBA Action pending"

─────────────────────────────────────
COMPONENT 4: GGR Value
─────────────────────────────────────
Name: "GGR Value"
Two variants: Positive | Negative

- Positive: "+$4,820" — 12px Bold, color #065f46
- Negative: "-$820"  — 12px Bold, color #991b1b

─────────────────────────────────────
COMPONENT 5: Last Contact Badge
─────────────────────────────────────
Name: "Last Contact Badge"
Three variants: Recent (≤20d) | Aging (21–35d) | Overdue (>35d)

Each: auto-layout, padding 2px 6px, border-radius 4px, font 10px Bold
- Recent:  bg #f0fdf4, text #166534, example "3d ago"
- Aging:   bg #fef3c7, text #92400e, example "28d ago"
- Overdue: bg #fee2e2, text #991b1b, example "43d ago"

─────────────────────────────────────
COMPONENT 6: Bonus Cell
─────────────────────────────────────
Name: "Bonus Cell"
Three variants: Standard | Overbonused | None

Standard:
- Vertical auto-layout, gap 2px
- Top row: "$500" — 12px Bold, color #334155
- Bottom row: "Bonus Bet · Aug 14" — 10px Regular, color #64748b

Overbonused:
- Vertical auto-layout, gap 2px
- Top row: horizontal layout — "$0" (12px Bold #334155) + pill badge "Overbonused" (10px Bold, bg #fee2e2, color #991b1b, border-radius 999px, padding 2px 6px)
- Bottom row: "Postal Gift · Jul 22" — 10px Regular, color #64748b

None:
- "—" — 12px Regular, color #d1d5db

─────────────────────────────────────
COMPONENT 7: Segment Text
─────────────────────────────────────
Name: "Segment Text"
Two variants: Single | Multi

- Single: "SBK" — 12px Medium, color #475569
- Multi:  "SBK, CAS" — 12px Medium, color #475569

─────────────────────────────────────
COMPONENT 8: Action Buttons
─────────────────────────────────────
Name: "VIP Action Buttons"
Two buttons in a horizontal auto-layout, gap 4px:

Contact button:
- bg #0060a3, text #fff, border #0060a3
- 10px Bold, border-radius 4px, padding 3px 8px
- Label: "Contact"

Bonus button:
- bg #10b981, text #fff, border #10b981
- 10px Bold, border-radius 4px, padding 3px 8px
- Label: "Bonus"

─────────────────────────────────────
COMPONENT 9: Table Row
─────────────────────────────────────
Name: "VIP Table Row"
Three variants: Default | Suspended | Selected

Layout:
- Horizontal auto-layout, height 44px, padding 6px 12px, gap 8px, align center
- Full-width: 1040px
- Border-bottom: 1px solid #f1f5f9
- Left border: 3px solid transparent (Default), 3px solid #0060a3 (Selected)

Column widths (10 columns, matching grid):
1. Checkbox:         24px  — checkbox input, accent #0060a3
2. Name + NBA:      220px  — name link (12px SemiBold, underline, #0b5fc5) + optional NBA Alert icon
3. Segment:          80px  — Segment Text component
4. Tier:             90px  — Tier Badge component
5. State:            60px  — "NJ" 11px Medium #475569
6. GGR L30:          90px  — GGR Value component
7. Last Bonus:      180px  — Bonus Cell component
8. Last Contact:     80px  — Last Contact Badge component
9. Status:           80px  — Status Pill component
10. Actions:         90px  — VIP Action Buttons component

Variant backgrounds:
- Default:    #ffffff
- Suspended:  #fff8f8 (very subtle red tint)
- Selected:   #f0f7ff, left-border color #0060a3

Use real data for the default row:
- Name: Jordan Williams | NBA alert: yes | Segment: SBK | Tier: Shield Select | State: NJ | GGR: +$4,820 | Bonus: $500 / Bonus Bet · Aug 14 | Last Contact: 3d ago | Status: Active

─────────────────────────────────────
COMPONENT 10: Table Header
─────────────────────────────────────
Name: "Table Header"
- Horizontal auto-layout, height 32px, padding 6px 12px, gap 8px, align center
- Width: 1040px
- Background: #f3f3f3, border-bottom: 1px solid #e5e5e5
- Font: 10px Bold uppercase, letter-spacing 0.04em, color #3e3e3c

Columns (same widths as Table Row):
1. Checkbox: 24px — checkbox
2. "Name ⇅" — sort arrow ⇅ in #94a3b8, 9px
3. "Segment"
4. "Tier ⇅"
5. "State"
6. "GGR L30 ⇅"
7. "Last Bonus Issued ⇅"
8. "Last Contact ⇅"
9. "Status"
10. "Actions"

─────────────────────────────────────
COMPONENT 11: Filter Bar
─────────────────────────────────────
Name: "Filter Bar"
- Horizontal auto-layout, height 40px, padding 6px 12px, gap 8px, align center
- Background: #f8fafc, border-bottom: 1px solid #e2e8f0
- Width: 1040px

Children left to right:
a. Search input wrapper (auto-layout, relative):
   - Input field: width 200px, height 28px, border 1.5px solid #d0d5dd, border-radius 4px, padding-left 24px
   - Placeholder text: "Search by name…" 12px Regular #94a3b8
   - Search icon (left): magnifier, 13×13px, color #94a3b8, positioned 8px from left

b. Filter button:
   - Border 1.5px solid #d0d5dd, bg #fff, border-radius 4px, padding 4px 10px
   - Funnel icon (12×12 lines) + "Filter" label — 11px Bold, color #475569
   - Hover state: border #0060a3, text #0060a3

c. Refresh button:
   - Border 1.5px solid #d0d5dd, bg #fff, border-radius 4px, padding 4px 8px
   - Circular arrow icon, 13×13px, color #475569

d. "Refreshed 9:41 AM" — 11px Regular, color #94a3b8, margin-left auto

─────────────────────────────────────
COMPONENT 12: Pagination Footer
─────────────────────────────────────
Name: "Pagination Footer"
- Horizontal auto-layout, height 36px, padding 6px 12px
- Background: #f8fafc, border-top: 1px solid #e2e8f0
- Width: 1040px
- Justify: space-between

Left: "1–10 of 353" — 11px Regular, color #64748b
Right: two buttons in horizontal layout, gap 6px
  - "‹ Prev" — 11px SemiBold, border 1.5px solid #d0d5dd, bg #fff, border-radius 4px, padding 4px 10px, disabled state = opacity 40%
  - "Next ›" — same style, enabled

─────────────────────────────────────
COMPONENT 13: Global Nav Bar
─────────────────────────────────────
Name: "Global Nav Bar"
- Width: 1280px, height 52px
- Background: #032d60 (Salesforce dark navy)
- Horizontal auto-layout, padding 0 24px, align center, gap 0

Children:
a. App launcher icon: 9-dot grid, 18×18px, color #fff, opacity 70%, margin-right 16px
b. Logo area: "FD" text badge (bold, #fff, bg #0060a3, 28×28px border-radius 4px) + "VIP Console" (14px SemiBold, #fff)
c. Spacer (flex grow)
d. Nav items horizontal, gap 4px:
   - "VIP Playbook" — 13px Medium, #fff, bg rgba(255,255,255,0.15) active pill, padding 6px 14px, border-radius 4px
   - "Agentforce NBAs" — same style, inactive
   - "Awareness" — same style, inactive
   - "WPC Tracker" — same style, inactive
e. Spacer
f. Notification bell icon: 18×18px, #fff, opacity 70%
g. User avatar: circle 32px, bg #0060a3, initials "TK" 12px Bold #fff

─────────────────────────────────────
COMPONENT 14: VIP Contact Card Modal
─────────────────────────────────────
Name: "VIP Contact Card Modal"
- Width: 520px, auto height
- Background: #fff, border-radius 8px, box-shadow 0 8px 32px rgba(0,0,0,0.18)
- Vertical auto-layout, no padding (sections handle their own)

Header section (bg #0060a3, padding 20px 24px, horizontal auto-layout, gap 16px, align center):
- Avatar circle: 48px, bg #004a80, initials "JW" 16px Bold #fff
- Name + meta column:
  - "Jordan Williams" — 16px Bold #fff
  - "Sportsbook · Shield Select · NJ" — 12px Regular rgba(255,255,255,0.8)
- Close button (×): top-right, 20×20px, color rgba(255,255,255,0.7)

Stats row (bg #f8fafc, border-bottom 1px solid #e5e5e5, padding 12px 24px):
- 4 stat cards in horizontal layout, equal width, gap 0, dividers between
- Each card: vertical auto-layout, align center
  - Label: 10px uppercase SemiBold, #94a3b8
  - Value: 15px Bold, #1e1e1e
- Cards: Est. LTV $8,240 | 30d Spend $5,730 | Sessions/Wk 3.0 | Avg Stake $1,450

Profile section (padding 16px 24px, vertical auto-layout, gap 12px):
- Section title "VIP Profile" — 11px Bold uppercase, #94a3b8, letter-spacing 0.06em
- 2×3 grid of field/value pairs (auto-layout grid, 2 columns, gap 8px 16px):
  - Segment: SBK | Tier: Shield Select | State: NJ | Birthday: 03/15 | Last Contact: 3d ago | Status: ACTIVE pill

Actions row (border-top 1px solid #e5e5e5, padding 12px 24px, horizontal auto-layout, gap 8px):
- "Contact" — primary button: bg #0060a3, text #fff, 12px Bold, border-radius 4px, padding 8px 16px
- "Issue Bonus" — secondary button: bg #fff, text #0060a3, border 1.5px solid #0060a3, same sizing
- "Invite to Event" — same secondary style
- "View Full Record" — text-only: 12px SemiBold, color #475569

---

PHASE 2 — SCREEN FRAMES

Create a section below the components with label "Screens" (Inter Bold 20px, #1e1e1e).
Place screens in a horizontal row, 80px gap between them.

─────────────────────────────────────
SCREEN 1: My VIPs — Default State
─────────────────────────────────────
Frame name: "Screen / My VIPs — Default"
Width: 1280px, Height: 900px, bg: #f3f3f3

Layers top to bottom:
1. Global Nav Bar component — full width, y=0
2. Page content area — padding 24px, vertical auto-layout, gap 16px

   a. Section header row (horizontal auto-layout, space-between):
      - Left: "VIP Playbook" — 18px Bold, #1e1e1e
      - Right: metrics summary chips (small pills): "353 VIPs" | "81% Active" | "Avg GGR $2,140" — 11px SemiBold, bg #e0f0ff, color #0060a3, border-radius 999px, padding 3px 10px

   b. Tab bar (horizontal auto-layout, border-bottom 1px solid #e5e5e5, gap 0):
      - "Overview" tab: 13px SemiBold, color #514f4d, padding 10px 16px, border-bottom 2px solid transparent
      - "My VIPs" tab: 13px SemiBold, color #0060a3, padding 10px 16px, border-bottom 2px solid #0060a3 (active)

   c. Table card (bg #fff, border-radius 8px, box-shadow 0 1px 4px rgba(0,0,0,0.08), overflow hidden):
      - Filter Bar component
      - Table Header component
      - 10 Table Row components stacked (use these rows in order):
        Row 1:  Jordan Williams | NBA✓ | SBK           | Shield Select | NJ | +$4,820 | $500 · Bonus Bet · Aug 14      | 3d ago  | Active
        Row 2:  Alex Johnson    |      | SBK, CAS      | Tier 2        | NY | +$1,230 | $0 Overbonused · Bonus Bet · Aug 3 | 8d ago  | Active
        Row 3:  Morgan Smith    | NBA✓ | CAS           | Tier 3        | CT | -$340   | $126 · Postal Gift · Jul 30     | 22d ago | Active
        Row 4:  Taylor Brown    |      | DFS, SBK      | Tier 1        | PA | +$980   | $47 · Bonus Bet · Aug 10        | 14d ago | Suspended
        Row 5:  Casey Davis     |      | FDR           | Tier 2        | NJ | +$210   | —                               | 38d ago | Deactivated
        Row 6:  Reese Wilson    | NBA✓ | SBK, CAS, DFS | Shield Select | NY | +$7,650 | $715 · Event · Aug 1            | 5d ago  | Active
        Row 7:  Sam Moore       |      | CAS           | Tier 1        | CT | -$820   | $0 Overbonused · Postal Gift · Jul 22 | 29d ago | Active
        Row 8:  Jamie Taylor    | NBA✓ | SBK           | Tier 4        | NJ | +$2,100 | $293 · Bonus Bet · Aug 18       | 11d ago | Active
        Row 9:  Riley Anderson  |      | DFS           | Tier 3        | PA | +$180   | —                               | 43d ago | Suspended
        Row 10: Chris Thomas    | NBA✓ | SBK, CAS      | Tier 2        | NY | +$3,400 | $200 · Postal Gift · Aug 5      | 17d ago | Active
      - Pagination Footer component

─────────────────────────────────────
SCREEN 2: My VIPs — Contact Card Open
─────────────────────────────────────
Frame name: "Screen / My VIPs — Contact Card"
Width: 1280px, Height: 900px, bg: #f3f3f3

Duplicate Screen 1.
Add:
- Overlay layer: full-frame rectangle, bg rgba(0,0,0,0.45), z-index above table
- VIP Contact Card Modal component — centered horizontally, y=160px (vertically centered-ish)
- Row 1 (Jordan Williams) should appear selected (blue left border, bg #f0f7ff) behind the overlay

─────────────────────────────────────
SCREEN 3: Next Best Action Queue
─────────────────────────────────────
Frame name: "Screen / Next Best Action"
Width: 1280px, Height: 900px, bg: #f3f3f3

Layers top to bottom:
1. Global Nav Bar — "Agentforce NBAs" tab active in nav
2. Page content (padding 24px, vertical auto-layout, gap 16px):

   a. Section header: "Agentforce Next Best Actions" — 18px Bold #1e1e1e
      Subtitle: "AI-prioritized outreach recommendations for your book of business" — 13px Regular #64748b

   b. Priority queue card (bg #fff, border-radius 8px, box-shadow 0 1px 4px rgba(0,0,0,0.08)):
      - Card header (bg #032d60, padding 12px 16px, border-radius 8px 8px 0 0):
        "🤖 Agentforce Priority Queue" — 13px Bold #fff
        Badge: "4 Actions" — 10px Bold bg rgba(255,255,255,0.2) color #fff border-radius 999px padding 2px 8px

      - 4 NBA action cards stacked (each card: padding 14px 16px, border-bottom 1px solid #f1f5f9):
        Each card horizontal auto-layout, gap 12px:
        - Priority badge (circle 28px): #1 bg #dc2626 text #fff, #2 bg #ea580c, #3 bg #ca8a04, #4 bg #0060a3 — 11px Bold
        - Middle column (flex grow, vertical auto-layout gap 4px):
          - VIP name: 13px SemiBold, color #0060a3, underline
          - Action label: 12px Regular #1e1e1e (e.g. "Retention Outreach — 38d since last contact")
          - AI reasoning chip: bg #f0f9ff, color #0369a1, 10px Regular, border-radius 4px, padding 2px 6px ("Churn risk: GGR down 40% vs prior 30d")
        - Right column (vertical auto-layout, gap 4px, align end):
          - "Contact" button — compact, bg #0060a3, text #fff, 10px Bold, border-radius 4px, padding 4px 10px
          - "Dismiss" text link — 10px Regular, color #94a3b8

      Cards data:
      1: Casey Davis    | Retention Outreach — 38d since last contact       | Churn risk: no bonus issued, longest lapse in book
      2: Riley Anderson | Re-engagement — Account suspended, check status   | Suspended 43d ago, prior GGR positive
      3: Morgan Smith   | Bonus Consideration — GGR negative past 30d       | -$340 L30, last bonus Jul 30 ($126 Postal Gift)
      4: Jordan Williams| Loyalty Milestone — Shield Select, high GGR       | +$4,820 L30, no milestone recognition this quarter

─────────────────────────────────────
SCREEN 4: Awareness Command Center
─────────────────────────────────────
Frame name: "Screen / Awareness Command Center"
Width: 1280px, Height: 900px, bg: #f3f3f3

Layers:
1. Global Nav Bar — "Awareness" tab active
2. Page content (padding 24px, vertical auto-layout, gap 16px):

   a. Header: "Real-time Awareness" — 18px Bold #1e1e1e
      Subtitle: "Live signals across your book of business" — 13px Regular #64748b

   b. 3-column grid (horizontal auto-layout, gap 16px, equal width columns):

      Column 1 — "Big Swings" card (bg #fff, border-radius 8px, box-shadow):
        Header: amber left-border 3px, "🎯 Big Swings" 13px Bold, "#94a3b8 2 active" badge
        3 rows: VIP name + "Placed $2,400 parlay · NFL Sunday" type entries, each with a "Review" link

      Column 2 — "Payment Alerts" card (bg #fff, border-radius 8px, same shadow):
        Header: red left-border 3px, "⚠ Payment Alerts" 13px Bold, "1 alert" badge bg #fee2e2 text #991b1b
        1 row: "Alex Johnson · Withdrawal pending $8,500 · 2h ago" with "View" button

      Column 3 — "Losing Streaks" card (bg #fff, border-radius 8px, same shadow):
        Header: orange left-border 3px, "📉 Losing Streaks" 13px Bold, "2 flagged" badge
        2 rows: VIP name + "Down $820 past 7 sessions" type entries, each with "Contact" link

---

EXECUTION NOTES
- Name all layers descriptively: component name / variant / element (e.g. "Status Pill / Active", "VIP Table Row / Suspended / Name Cell")
- All components must be proper Figma components (use makeComponent), not just groups
- Table rows in screens should be instances of the Table Row component
- Use auto-layout throughout — no absolute positioning except the modal overlay in Screen 2
- Maintain 8px grid alignment
- After building all components and screens, execute PHASE 3 below
- After building, zoom to fit all frames in the viewport
- Return the Figma node IDs of all created components and screen frames

---

PHASE 3 — ANNOTATIONS

CONTEXT: This prototype is being implemented as a Salesforce Sales Console Custom Home Page called "VIP KAM Console". Annotations must reflect this — every annotation should help a Salesforce developer or Ren (who is learning to build this) understand what each piece is and how to build it.

─────────────────────────────────────
ANNOTATION STYLE
─────────────────────────────────────
Annotation frame style:
- bg: #fffbeb, border-left: 3px solid #f59e0b, border-radius: 4px, padding: 10px 12px
- Title: 10px Bold uppercase, color #92400e, letter-spacing 0.06em
- Body: 11px Regular, color #374151, line-height 1.5
- Place annotation frame to the RIGHT of (or below, if right is crowded) each component/screen
- Connect with a dashed line: stroke #f59e0b, dash pattern [4,4], stroke-width 1

─────────────────────────────────────
COMPONENT ANNOTATIONS
─────────────────────────────────────
Add one annotation frame next to each component:

Component 1 — Status Pill
  Title: STATUS PILL
  Body: "Displays VIP account status. In Salesforce: formula field or picklist on VIP_Account__c.Status__c.
  LWC: vipStatusBadge.js — accepts status prop, returns styled badge.
  Complexity: Easy — pure display, no Apex needed."

Component 2 — Tier Badge
  Title: TIER BADGE
  Body: "Displays VIP tier within their primary segment. In Salesforce: VIP_Account__c.Tier__c picklist.
  Shield Select variant = highest tier — use conditional rendering in LWC template.
  LWC: vipTierBadge.js. Complexity: Easy."

Component 3 — NBA Alert Icon
  Title: NBA ALERT (INLINE)
  Body: "Amber triangle shown when a VIP has a pending Next Best Action. Data from: Einstein NBA recommendation linked to the VIP record.
  In LWC: boolean prop hasNBA passed from parent query. Hover tooltip uses CSS ::after pseudo-element or a custom popover LWC.
  Complexity: Medium — requires NBA object query."

Component 4 — GGR Value
  Title: GGR L30 VALUE
  Body: "Gross Gaming Revenue last 30 days — how much FanDuel made from this VIP this month. Positive = house up (green), Negative = VIP winning (red).
  Data source: VIP_Analytics__c.GGR_L30__c — likely a rollup or nightly batch from the data warehouse.
  LWC: inline formatted value. Complexity: Easy display / Medium data pipeline."

Component 5 — Last Contact Badge
  Title: LAST CONTACT BADGE
  Body: "Days since last KAM-to-VIP contact. Calculated from ActivityHistory (Task/Event) on the VIP record.
  Logic: today - MAX(ActivityDate) where OwnerId = current KAM.
  LWC: computed in Apex, passed as integer days. Color thresholds: ≤20d green, 21–35d amber, >35d red.
  Complexity: Easy–Medium."

Component 6 — Bonus Cell
  Title: LAST BONUS ISSUED
  Body: "Shows most recent bonus issued to the VIP. Data from: VIP_Bonus__c custom object (Amount__c, Type__c, IssuedDate__c, IsOverbonused__c).
  Overbonused flag is set by compliance rules — display as red badge, not editable.
  LWC: vipBonusCell.js — queries last bonus record via @wire. Complexity: Medium."

Component 7 — Segment Text
  Title: SEGMENT
  Body: "VIP's product segment(s): SBK (Sportsbook), CAS (Casino), DFS (Fantasy), FDR (Racing).
  A VIP can span multiple segments — stored as multi-select picklist or junction object.
  Display as comma-separated text. Complexity: Easy."

Component 8 — Action Buttons
  Title: ACTION BUTTONS
  Body: "Contact: opens a Salesforce Task quick-action pre-populated with VIP context.
  Bonus: opens a Flow screen to issue a bonus — triggers VIP_Bonus__c record creation + compliance check.
  LWC: vipActionBar.js, calls NavigationMixin for Contact; Salesforce Flow for Bonus.
  Complexity: Medium (requires Quick Actions + Flow)."

Component 9 — Table Row
  Title: VIP TABLE ROW
  Body: "One row in the KAM's book of business. Composes all other components.
  Parent LWC: vipDataTable.js — iterates over VIP records returned by Apex controller.
  Apex: getBookOfBusiness(kamId, filters, page, pageSize) — queries VIP_Account__c with related Bonus, Activity, NBA.
  Row variants (Default/Suspended/Selected) controlled via CSS class binding in LWC template.
  Complexity: Medium–Hard (multi-object query + pagination)."

Component 10 — Table Header
  Title: TABLE HEADER
  Body: "Sortable column headers. Sort state managed as component property (sortField, sortDir).
  Clicking a header dispatches a custom 'sort' event caught by parent vipDataTable.
  No Apex re-query on sort if all records are client-side; re-query if server-side pagination is used.
  Complexity: Easy."

Component 11 — Filter Bar
  Title: FILTER BAR
  Body: "Search input filters by VIP name (client-side). Filter button opens Salesforce List View filter panel (OOTB behavior) — no custom filter logic needed for MVP.
  Refresh button re-calls Apex getBookOfBusiness() with current params and shows a spinner.
  LWC: vipTableFilters.js. Complexity: Easy–Medium."

Component 12 — Pagination Footer
  Title: PAGINATION FOOTER
  Body: "Client-side pagination — Apex returns full record set on load, JS slices by page.
  Alternative: server-side pagination using OFFSET in SOQL (better for large books >500 VIPs).
  LWC: vipPaginationFooter.js — receives totalCount, currentPage, pageSize as props, emits 'pagechange' events.
  Complexity: Easy."

Component 13 — Global Nav Bar
  Title: GLOBAL NAV BAR
  Body: "In Salesforce: this is the App Navigation Bar configured in Lightning App Builder for the 'VIP KAM Console' custom app.
  Each nav item = a Lightning App Page (Custom Home Page component) assigned to the app.
  The nav bar itself is an OOTB Salesforce component — tabs are defined in Setup > App Manager.
  Active tab highlighting is automatic. Complexity: Easy (config, not code)."

Component 14 — VIP Contact Card Modal
  Title: VIP CONTACT CARD MODAL
  Body: "Detailed VIP profile drawer/modal. Opens on name click within vipDataTable.
  In Salesforce: implement as a Quick Action panel or a custom LWC modal using lightning-modal (available SLDS v2).
  Data: single VIP_Account__c record query + related Activity, VIP_Bonus__c, NBA records.
  LWC: vipContactCard.js — accepts vipId prop, runs @wire getRecord or imperative Apex.
  Stats (LTV, Sessions, Avg Stake) may come from VIP_Analytics__c rollup object.
  Complexity: Hard — multi-object, modal lifecycle, action integrations."

─────────────────────────────────────
SCREEN ANNOTATIONS
─────────────────────────────────────
For each screen frame, add a callout legend below the frame (not overlapping the screen).
Legend style: bg #f8fafc, border 1px solid #e2e8f0, border-radius 6px, padding 16px, vertical auto-layout gap 8px.
Legend title: "Screen Notes" — 11px Bold uppercase, color #475569.
Each callout: numbered circle (bg #0060a3, text #fff, 16px diameter, 10px Bold) + text (11px Regular #374151).

Screen 1 — My VIPs Default
  Callout 1: "Custom Home Page — This entire view is one Lightning App Page in Salesforce. Each section is a separate LWC region."
  Callout 2: "Section Header + Tab Bar — Static HTML/LWC wrapper. Tab switching shows/hides child components; no page navigation."
  Callout 3: "Table Card — Core component: vipDataTable LWC. Loads on connectedCallback() with KAM's userId to scope the book of business."
  Callout 4: "Pagination — 10 rows per page default. Recommended: server-side pagination for books >200 VIPs to stay within governor limits."
  Callout 5: "NBA Alert icons — pulled from a separate @wire query for NBAs linked to visible VIPs. Lazy-loaded after table renders."

Screen 2 — Contact Card Open
  Callout 1: "Modal implementation — Use lightning-modal (SLDS v2) for accessibility compliance. Triggered by handleNameClick() in vipDataTable."
  Callout 2: "Overlay — Handled automatically by lightning-modal backdrop. Do not build a custom overlay div."
  Callout 3: "VIP data in modal — Single getRecord() call using the row's VIP Id. Related Activity and Bonus loaded in parallel via Promise.all."
  Callout 4: "Action buttons — 'Contact' uses NavigationMixin to open a Log a Call quick action. 'Issue Bonus' invokes a Screen Flow via lightning-flow."

Screen 3 — Next Best Action Queue
  Callout 1: "NBA data source — Einstein Next Best Action (Recommendation + Strategy objects). Requires Einstein NBA license + configured strategy."
  Callout 2: "Priority ordering — Set in the NBA Strategy (Salesforce AI decision engine). The queue LWC just displays what the strategy returns."
  Callout 3: "Contact action — Same NavigationMixin pattern as Contact Card. Dismiss action calls an Apex method to record the dismissal on the Recommendation record."
  Callout 4: "AI reasoning chip — Custom field Recommendation.Rationale__c or a formula summarizing the trigger conditions."

Screen 4 — Awareness Command Center
  Callout 1: "Real-time signals — Big Swings and Losing Streaks likely come from a nightly batch or streaming via Salesforce Platform Events / Change Data Capture."
  Callout 2: "Payment Alerts — May originate from an external payment processor webhook feeding into a Salesforce Platform Event, which triggers an Apex trigger to create an alert record."
  Callout 3: "Three-column layout — Each column is an independent LWC. Load them in parallel; if one fails, the others still render (defensive loading)."
  Callout 4: "This section is the highest complexity area — most data is external and requires a real-time integration layer. Scope carefully with the tech architect."
```
