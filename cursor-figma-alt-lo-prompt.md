# Cursor → Figma Prompt — ALT LO VIP KAM Console (Proposed Nav Structure)

This is the proposed restructured layout of the VIP KAM Console — do NOT overwrite existing screens.
Create all items on a NEW Figma page named "ALT LO – VIP KAM Console".

Paste this entire prompt into Cursor chat with Figma MCP connected.

---

```
You are working in Figma using the Figma MCP. Build a full alternate layout of the FanDuel VIP KAM Console using a proposed 4-tab navigation structure. This is a Salesforce Sales Console Custom Home Page. Create all items on a page named "ALT LO – VIP KAM Console". Do NOT modify any existing pages.

---

CONTEXT

Current structure: VIP Playbook | Agentforce NBAs | Awareness | WPC Tracker (separate destinations, KAM must tab-hop)
Proposed structure: Today | My VIPs | Actions | Reports
Rationale: Aligns nav to KAM daily workflow — check what's urgent → review book → take actions → review performance.
This is for a Salesforce Sales Console Custom Home Page (Lightning App Builder, LWC components).

---

DESIGN TOKENS (same as existing build)

Colors:
- primary: #0060a3 | primary-dark: #004a80 | primary-light: #eff6ff
- text-primary: #1e1e1e | text-secondary: #475569 | text-muted: #94a3b8
- bg-page: #f3f3f3 | bg-card: #ffffff | bg-header: #f8fafc
- border: #e5e5e5 | border-input: #d0d5dd
- green: bg #d1fae5 / text #065f46 | yellow: bg #fef3c7 / text #92400e | red: bg #fee2e2 / text #991b1b
- accent-agentforce: #032d60 (Salesforce dark navy)

Typography: Salesforce Sans / Arial, same scale as existing build.

---

SHARED COMPONENT: ALT NAV BAR

Name: "ALT Nav Bar"
Width: 1280px, height: 52px, bg: #032d60
Horizontal auto-layout, padding: 0 24px, align center, gap: 0

Children:
a. App launcher (9-dot, 18px, #fff opacity 70%), margin-right 16px
b. Logo: "FD" badge (bold #fff, bg #0060a3, 28×28 border-radius 4px) + "VIP Console" (14px SemiBold #fff), gap 8px
c. Spacer (flex grow)
d. 4 nav tabs, horizontal, gap 4px:
   - Each tab: 13px Medium, padding 6px 14px, border-radius 4px
   - Active state:  text #fff, bg rgba(255,255,255,0.15), underline 2px #fff below
   - Inactive state: text rgba(255,255,255,0.65), bg transparent
   - Tab labels: "Today" | "My VIPs" | "Actions" | "Reports"
e. Spacer
f. Bell icon (18px #fff opacity 70%) + Avatar circle (32px bg #0060a3, "TK" 12px Bold #fff)

Create 4 variants of this component, one with each tab active.

---

SCREEN A: Today Dashboard
─────────────────────────────────────
Frame name: "ALT / Today Dashboard"
Width: 1280px, Height: 960px, bg: #f3f3f3

Nav Bar variant: "Today" active

Page content (padding 24px, vertical auto-layout, gap 16px):

  a. Welcome header row (horizontal, space-between):
     Left: "Good morning, Taylor" — 20px Bold #1e1e1e
            "Thursday, Aug 28 · Sportsbook KAM · 353 VIPs" — 13px Regular #64748b
     Right: 3 KPI chips horizontal gap 8px:
       - "12 Contact Due" bg #fee2e2 text #991b1b 11px Bold border-radius 999px padding 4px 12px
       - "4 NBA Actions" bg #fef3c7 text #92400e same style
       - "2 Alerts" bg #fee2e2 text #991b1b same style

  b. 3-column top row (horizontal auto-layout, gap 16px):

     Column 1 (flex 1.2) — "Priority Actions" card (bg #fff, border-radius 8px, shadow 0 1px 4px rgba(0,0,0,0.08)):
       Card header: bg #032d60, border-radius 8px 8px 0 0, padding 10px 16px
         "🤖 Top Agentforce NBAs" — 13px Bold #fff
         "4 actions" badge: bg rgba(255,255,255,0.2) text #fff 10px Bold border-radius 999px padding 2px 8px
       3 NBA action rows (padding 10px 16px, border-bottom 1px solid #f1f5f9):
         Each row horizontal auto-layout gap 10px:
         - Priority circle (22px, bg by priority: #dc2626/#ea580c/#ca8a04), 10px Bold #fff
         - VIP name (12px SemiBold #0b5fc5 underline) + action text (11px Regular #475569) — vertical stack
         - "Go →" link — 11px SemiBold #0060a3
         Rows:
           #1: Casey Davis · Retention — 38d no contact
           #2: Riley Anderson · Re-engagement — account suspended
           #3: Morgan Smith · Bonus consideration — GGR negative
       Footer link: "View all 4 actions →" 11px SemiBold #0060a3, padding 10px 16px, border-top 1px solid #f1f5f9

     Column 2 (flex 1) — "Contact Due Today" card (bg #fff, border-radius 8px, same shadow):
       Card header: border-bottom 1px solid #e5e5e5, padding 10px 16px
         "📞 Contact Due" — 13px Bold #1e1e1e
         "12 VIPs" badge bg #fee2e2 text #991b1b 10px Bold border-radius 999px padding 2px 8px
       5 contact rows (padding 8px 16px, border-bottom 1px solid #f1f5f9):
         Each: VIP name (12px SemiBold #0b5fc5) + "Xd ago" badge (red if >35d, amber if >20d) — horizontal space-between
         Rows: Casey Davis 38d | Riley Anderson 43d | Sam Moore 29d | Morgan Smith 22d | Chris Thomas 17d
       Footer: "View all 12 →" 11px SemiBold #0060a3, padding 10px 16px, border-top 1px solid #f1f5f9

     Column 3 (flex 0.8) — "Live Alerts" card (bg #fff, border-radius 8px, same shadow):
       Card header: border-bottom 1px solid #e5e5e5, padding 10px 16px
         "⚠ Live Alerts" — 13px Bold #1e1e1e
         "2" badge bg #fee2e2 text #991b1b
       2 alert rows (padding 10px 16px, border-bottom 1px solid #f1f5f9):
         Alert 1: amber left-border 3px | "Big Swing" label 10px Bold #92400e | "Jordan Williams · $2,400 parlay · 30min ago" 11px Regular #1e1e1e | "Review" link
         Alert 2: red left-border 3px | "Payment" label 10px Bold #991b1b | "Alex Johnson · Withdrawal $8,500 · 2h ago" 11px Regular #1e1e1e | "View" link

  c. Bottom row — 2 columns (horizontal auto-layout, gap 16px):

     Column 1 (flex 1.5) — "Book Snapshot" card (bg #fff, border-radius 8px, same shadow):
       Card header: border-bottom 1px solid #e5e5e5, padding 10px 16px
         "📊 Book Snapshot" — 13px Bold #1e1e1e
         "Last 30 days" — 11px Regular #94a3b8
       4-column stat row (padding 16px, horizontal auto-layout equal width, dividers):
         Each stat: label (10px uppercase #94a3b8) / value (18px Bold #1e1e1e) / delta (11px SemiBold color by direction)
         Stats: Total GGR L30: $754,200 ▲12% | Active VIPs: 288/353 | Avg Last Contact: 16d | Bonuses Issued: 47 this month
       Mini-table below: top 5 VIPs by GGR L30 — compact 3-col table (Name | GGR | Tier), 10px font, 5 rows

     Column 2 (flex 0.8) — "New This Week" card (bg #fff, border-radius 8px, same shadow):
       Card header: padding 10px 16px, border-bottom 1px solid #e5e5e5
         "🆕 New This Week" — 13px Bold #1e1e1e
       2 sections vertical:
         "Onboarded (2)" — 11px Bold #065f46, then 2 VIP name rows with "New" green badge
         "Churn Risk (3)" — 11px Bold #991b1b, then 3 VIP name rows with "Risk" red badge

ANNOTATION LEGEND for Screen A (below frame, same amber annotation style):
  1. "Today tab = Custom Home Page default landing. All 4 widgets are independent LWCs loaded in parallel — defensive rendering prevents one failure from blocking others."
  2. "Priority Actions widget = agentforceNBAQueue LWC (top 3 only). Full list lives on the Actions tab."
  3. "Contact Due = computed server-side by Apex, ordered by days since last Activity. Threshold configurable as a Custom Metadata record."
  4. "Live Alerts = subscribes to a Salesforce Platform Event channel. Requires an Apex trigger or Flow feeding the event when new alerts are created."
  5. "Book Snapshot stats = rollup fields on KAM_Territory__c or an Apex @AuraEnabled method. Delta % requires prior-period comparison — consider a scheduled batch."

---

SCREEN B: My VIPs (same table, new nav context)
─────────────────────────────────────
Frame name: "ALT / My VIPs"
Width: 1280px, Height: 900px, bg: #f3f3f3

Nav Bar variant: "My VIPs" active (identical table content to existing Screen 1 — reuse component instances)
No sub-tabs needed — My VIPs is now its own top-level destination.

Page content (padding 24px, vertical auto-layout, gap 16px):
  a. Section header (horizontal, space-between):
     Left: "My VIPs" — 18px Bold #1e1e1e · "353 VIPs in your book" — 13px Regular #64748b
     Right: "Last synced: 9:41 AM" — 11px Regular #94a3b8

  b. Table card (bg #fff, border-radius 8px, shadow) — identical to existing Screen 1 table:
     Filter Bar + Table Header + 10 data rows + Pagination Footer

ANNOTATION LEGEND:
  1. "My VIPs is now a top-level nav destination, not a sub-tab. This removes one click from the KAM's most frequent action."
  2. "Table implementation unchanged from existing build — vipDataTable LWC, same Apex controller."
  3. "The Overview metrics sub-tab (from original build) moves to the Today dashboard — no loss of data, just better placement."

---

SCREEN C: Actions
─────────────────────────────────────
Frame name: "ALT / Actions"
Width: 1280px, Height: 960px, bg: #f3f3f3

Nav Bar variant: "Actions" active

Page content (padding 24px, vertical auto-layout, gap 16px):

  a. Section header:
     "Actions" — 18px Bold #1e1e1e
     "AI-prioritized outreach + bulk tools for your book of business" — 13px Regular #64748b

  b. 2-column layout (horizontal auto-layout, gap 16px):

     Left column (flex 1) — Full NBA Queue card (bg #fff, border-radius 8px, same shadow):
       Card header (bg #032d60, border-radius 8px 8px 0 0, padding 12px 16px):
         "🤖 Agentforce Priority Queue" — 13px Bold #fff · "4 Actions" badge
       4 full NBA cards (same as existing Screen 3 layout, all 4 rows)

     Right column (flex 0.7) — Bulk Actions card (bg #fff, border-radius 8px, same shadow):
       Card header (border-bottom 1px solid #e5e5e5, padding 10px 16px):
         "⚡ Bulk Actions" — 13px Bold #1e1e1e
         "Select VIPs from My VIPs to enable" — 11px Regular #94a3b8
       3 bulk action buttons (vertical auto-layout, gap 8px, padding 16px):
         Each button: full-width, horizontal auto-layout, padding 10px 14px, border-radius 6px
         a. "Bulk Contact Log" — bg #eff6ff border 1.5px solid #0060a3 text #0060a3 13px SemiBold icon left
         b. "Bulk Bonus Issue" — bg #f0fdf4 border 1.5px solid #10b981 text #065f46 13px SemiBold icon left
         c. "Bulk Status Update" — bg #f8fafc border 1.5px solid #d0d5dd text #475569 13px SemiBold icon left
       Disabled state overlay note: light gray wash + "Select VIPs from My VIPs tab first" tooltip

       WPC (Weekly Priority Contacts) section below (border-top 1px solid #e5e5e5, padding 16px):
         Header: "WPC This Week" — 12px Bold #1e1e1e · progress "7 / 15 contacted" pill
         Progress bar: full width, height 6px, bg #e2e8f0, fill #0060a3, border-radius 999px, fill 47%
         "8 remaining · Week ends Fri Aug 30" — 11px Regular #94a3b8

ANNOTATION LEGEND:
  1. "Actions tab consolidates NBA Queue + Bulk Tools + WPC Tracker — three previously separate destinations now colocated because they share the same intent: take action on VIPs."
  2. "Bulk Actions panel is disabled until VIPs are selected on the My VIPs tab. State managed via a shared LWC state service or a parent component wrapping both tabs."
  3. "WPC Tracker widget = weeklyPriorityContacts LWC. Progress derived from Activity records created this week where WPC_Flag__c = true."
  4. "NBA Queue implementation: Einstein Next Best Action — requires einstein_analytics license. Mock with static data for prototype/demo phase."

---

SCREEN D: Reports
─────────────────────────────────────
Frame name: "ALT / Reports"
Width: 1280px, Height: 960px, bg: #f3f3f3

Nav Bar variant: "Reports" active

Page content (padding 24px, vertical auto-layout, gap 16px):

  a. Section header:
     "Reports" — 18px Bold #1e1e1e
     "Performance analytics for your book of business" — 13px Regular #64748b

  b. View switcher tabs (horizontal auto-layout, border-bottom 1px solid #e5e5e5, gap 0):
     4 tabs: "Overview" (active: #0060a3 border-bottom 2px) | "Spend Trends" | "Segments" | "Retention"
     Tab style: 13px SemiBold, padding 10px 16px

  c. Tableau embed placeholder (bg #fff, border-radius 8px, border 1px solid #e5e5e5, height 520px):
     Centered content (vertical auto-layout, align center, gap 16px, padding 48px):
       Tableau logo placeholder: rectangle 48×48 bg #e2e8f0 border-radius 8px, "T" 18px Bold #475569
       "Tableau Report: VIP Overview" — 16px Bold #1e1e1e
       "Connected via Salesforce CRM Analytics or Tableau Cloud embed" — 13px Regular #64748b max-width 400px center
       Embed URL input (mock): width 360px, height 32px, border 1.5px solid #d0d5dd, border-radius 4px, placeholder "https://tableau.fanduel.com/views/..."
       "Load Report" button: bg #0060a3 text #fff 12px Bold padding 8px 20px border-radius 4px

  d. Last Contact Tracker summary (bg #fff, border-radius 8px, shadow, padding 16px):
     Header: "Last Contact Distribution" — 13px Bold #1e1e1e
     3 stat blocks horizontal (equal width):
       ≤20 days: "214 VIPs" 18px Bold #065f46 · "On track" 11px #065f46
       21–35 days: "97 VIPs" 18px Bold #92400e · "Needs attention" 11px #92400e
       >35 days: "42 VIPs" 18px Bold #991b1b · "Overdue" 11px #991b1b
     Bar chart (simplified 3 bars, proportional widths, same colors as above)

ANNOTATION LEGEND:
  1. "Reports tab consolidates Tableau + Last Contact Tracker + future pipeline reporting — all read-only, analytical views in one place."
  2. "Tableau embed: two options for Salesforce — (a) Tableau Cloud iframe embed via Connected App, (b) CRM Analytics (formerly Einstein Analytics) native dashboard. Confirm with tech architect."
  3. "Last Contact Distribution = client-side aggregation of the vipDataTable dataset. No additional Apex needed — compute from ALL_VIPS array."
  4. "Onboarding/Offboarding tracker (from original build) would also live here as a 5th report tab — scoped for Phase 2."

---

EXECUTION NOTES
- Create the ALT Nav Bar as a Figma component with 4 variants (one per active tab)
- Reuse existing component instances from "KAM Console – Components & Screens" page where possible (Table Row, Status Pill, etc.)
- All screen frames must be on the page "ALT LO – VIP KAM Console" — do not modify other pages
- Annotation frames: amber left-border style, below each screen frame, not overlapping
- Layer naming: "ALT / [Screen] / [Section] / [Element]"
- After building all 4 screens, create a 5th frame named "ALT / Nav Structure Comparison" (800×300px) showing the old nav vs new nav as a simple before/after table:
  Old nav (4 cols, labels): VIP Playbook | Agentforce NBAs | Awareness | WPC Tracker
  New nav (4 cols, labels): Today | My VIPs | Actions | Reports
  With arrows showing where each old section now lives in the new structure
- Zoom to fit all frames on completion
- Return Figma node IDs of all created frames
```
