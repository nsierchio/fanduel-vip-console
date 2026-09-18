# Cursor → Figma Prompt — VIP KAM Console Screen Flow

Paste this entire prompt into Cursor chat. Cursor will use Figma MCP to build a screen flow diagram for the FanDuel VIP KAM Console.

---

```
You are working in Figma using the Figma MCP. Build a screen flow diagram for the FanDuel VIP KAM Console — a Salesforce Sales Console Custom Home Page. Create all content on a Figma page named "KAM Console – Screen Flow".

---

DESIGN TOKENS

Colors:
- bg-page: #f3f3f3
- bg-card: #ffffff
- primary: #0060a3
- text-primary: #1e1e1e
- text-muted: #94a3b8
- border: #e2e8f0
- arrow-nav: #0060a3       (primary navigation)
- arrow-action: #10b981    (action / success path)
- arrow-conditional: #f59e0b  (conditional / decision)
- arrow-error: #991b1b     (error / blocked)
- annotation-bg: #fffbeb
- annotation-border: #f59e0b
- annotation-title: #92400e
- annotation-body: #374151

Typography: Salesforce Sans / Arial fallback.

---

CANVAS SETUP

- Canvas background: #f0f4f8
- Page title frame (top of canvas): "VIP KAM Console — Screen Flow" — 24px Bold #1e1e1e, subtitle "Current build · Salesforce Sales Console Custom Home Page" — 13px Regular #64748b
- Organize all content below the title

---

SWIM LANES

Create 4 horizontal swim lanes, stacked vertically with 40px gap between them.
Each swim lane:
- Label column on the left: 160px wide, bg #e2e8f0, border-radius 6px, centered label text — 12px Bold uppercase, color #475569, letter-spacing 0.06em
- Content area to the right: auto-width, holds screen thumbnails and arrows for that lane

Swim lane names (top to bottom):
1. AUTHENTICATION
2. VIP PLAYBOOK
3. NEXT BEST ACTIONS
4. AWARENESS

---

SCREEN THUMBNAILS

Each screen thumbnail:
- Size: 400×280px
- Background: #ffffff
- Border: 1.5px solid #e2e8f0
- Border-radius: 8px
- Drop shadow: 0 4px 12px rgba(0,0,0,0.10)
- Label below thumbnail (outside the frame): Screen name (12px Bold #1e1e1e) + LWC component (10px Regular #94a3b8)
- Thumbnail content: simplified layout — just enough structure to identify the screen (nav bar strip at top, section title, key UI element placeholder blocks using light gray fills)

Create the following 12 screen thumbnails, placed in their respective swim lanes:

── AUTHENTICATION lane ──

S1: Password Gate
  LWC: passwordGate / standalone entry
  Thumbnail content: centered card (200×140px, white, shadow), "FD" logo badge (40×40 blue square), "VIP Console" label, password input field placeholder, "Enter" button (blue)
  Place at x=0 in the lane.

── VIP PLAYBOOK lane ──

S2: VIP Playbook — Overview Tab
  LWC: vipPlaybookOverview
  Thumbnail content: nav bar strip (#032d60, full width, 28px tall), "VIP Playbook" section title, 4 metric card placeholders in a row (light gray fills, 70×50px each), small chart placeholder below
  Place at x=0 in the lane.

S3: My VIPs Table
  LWC: vipDataTable
  Thumbnail content: nav strip, "My VIPs" tab underlined in blue, filter bar strip (light gray, full width, 24px tall), 6 table row placeholders (alternating white/#f8fafc, full width, 22px tall each), pagination strip at bottom
  Place at x=500 in the lane.

S4: My VIPs — Bulk Select Active
  LWC: vipDataTable (bulk state)
  Thumbnail content: same as S3 but top row has blue checkbox fills, amber bulk action bar below filter bar ("3 VIPs selected · Contact All · Issue Bonus")
  Place at x=1000 in the lane.

S5: VIP Contact Card Modal
  LWC: vipContactCard (lightning-modal)
  Thumbnail content: S3 blurred behind a dark overlay (rectangle #000 opacity 40%), centered modal (220×180px, white, shadow): blue header strip with circle avatar + name placeholder, 2×3 grid of small field placeholders, 3 action button placeholders at bottom
  Place at x=1500 in the lane.

S6: VIP Full Record Page
  LWC: standard Salesforce record page (NavigationMixin)
  Thumbnail content: standard SF record page chrome — nav strip, breadcrumb bar (#f3f3f3), record header with avatar + name + field row, related lists placeholder below. Add a "↗ External" badge (blue, 10px) top-right of thumbnail.
  Place at x=2000 in the lane.

── NEXT BEST ACTIONS lane ──

S7: NBA Queue
  LWC: agentforceNBAQueue
  Thumbnail content: nav strip ("Agentforce NBAs" tab active, white underline), dark navy header card (#032d60, 40px tall), 4 NBA action card placeholders (white, 180px wide, 50px tall each with priority circle + text lines + button), stacked vertically
  Place at x=0 in the lane.

S8: NBA Queue — Card Dismissed
  LWC: agentforceNBAQueue (post-dismiss state)
  Thumbnail content: same as S7 but first card has a strikethrough overlay and fades (opacity 30%), remaining 3 cards shift up
  Place at x=500 in the lane.

── AWARENESS lane ──

S9: Awareness Command Center
  LWC: vipAwarenessCenter
  Thumbnail content: nav strip ("Awareness" tab active), section title, 3-column card layout — left card (amber accent, "Big Swings"), center card (red accent, "Payment Alerts"), right card (orange accent, "Losing Streaks"). Each column shows 2–3 row placeholders.
  Place at x=0 in the lane.

S10: Awareness — VIP Contact Card (from Big Swings)
  LWC: vipContactCard (triggered from Awareness context)
  Thumbnail content: identical to S5 (Contact Card modal) but the overlay sits over the Awareness background instead of the My VIPs background. Visually distinguishable by the 3-column layout visible behind the overlay.
  Place at x=500 in the lane.

---

FLOW ARROWS

Draw directional arrows between screen thumbnails. Each arrow:
- Stroke width: 2px
- Style: solid with arrowhead at destination end
- Label: short action text, 10px SemiBold, bg #fff, border 1px solid current arrow color, border-radius 4px, padding 2px 6px, placed at midpoint of the arrow

Arrow color = arrow type per legend (see LEGEND section).

Draw the following arrows:

1.  S1 → S2          color: arrow-nav       label: "Enter password"
2.  S2 → S3          color: arrow-nav       label: "Click My VIPs tab"
3.  S3 → S2          color: arrow-nav       label: "Click Overview tab"
4.  S3 → S4          color: arrow-conditional  label: "Select checkbox(es)"
5.  S4 → S3          color: arrow-nav       label: "Deselect all"
6.  S3 → S5          color: arrow-action    label: "Click VIP name"
7.  S5 → S3          color: arrow-nav       label: "Close modal"
8.  S5 → S3          color: arrow-action    label: "Contact → toast shown"
9.  S5 → S3          color: arrow-action    label: "Issue Bonus → Flow opens"
10. S5 → S6          color: arrow-action    label: "View Full Record"
11. S3 → S7          color: arrow-nav       label: "Nav: Agentforce NBAs"
12. S7 → S8          color: arrow-action    label: "Dismiss action"
13. S7 → S5          color: arrow-action    label: "Contact action"
14. S3 → S9          color: arrow-nav       label: "Nav: Awareness"
15. S9 → S10         color: arrow-action    label: "Review (Big Swings)"
16. S10 → S9         color: arrow-nav       label: "Close modal"
17. S7 → S9          color: arrow-nav       label: "Nav: Awareness"
18. S9 → S7          color: arrow-nav       label: "Nav: NBAs"

For cross-lane arrows (e.g. S3 → S7, S7 → S5), route them as curved paths that travel between swim lanes. Keep labels readable — offset from the arrow midpoint if needed.

---

LEGEND

Create a legend frame to the right of the swim lanes (or below if space is tight):
- Frame: 280px wide, auto height, bg #ffffff, border 1px solid #e2e8f0, border-radius 8px, padding 16px
- Title: "Arrow Legend" — 11px Bold uppercase, color #475569, margin-bottom 12px
- 4 legend rows (vertical auto-layout, gap 8px):
  Each row: horizontal auto-layout, gap 8px, align center
  - Color swatch: 32×4px rectangle, border-radius 2px, fill = arrow color
  - Label: 11px Regular #374155

  Row 1: swatch #0060a3 — "Primary Navigation (tab switch, modal close)"
  Row 2: swatch #10b981 — "Action / Success Path (contact, bonus, dismiss)"
  Row 3: swatch #f59e0b — "Conditional (requires user selection)"
  Row 4: swatch #991b1b — "Error / Blocked (reserved for error states)"

---

IMPLEMENTATION ANNOTATION

Below the entire swim lane diagram, add a full-width annotation frame:
- Style: bg #fffbeb, border-left 3px solid #f59e0b, border-radius 4px, padding 14px 16px
- Title: "SALESFORCE IMPLEMENTATION NOTE" — 10px Bold uppercase, color #92400e
- Body (11px Regular, color #374151, line-height 1.6):
  "This flow maps the current prototype build. In Salesforce, the entire console is a single Lightning App Page (Custom Home Page) assigned to the 'VIP KAM Console' app.
  Tab switches between Overview / My VIPs are component visibility toggles (conditional rendering in LWC) — they do NOT trigger page navigations or new page loads.
  Nav bar tab switches (Playbook → NBAs → Awareness) navigate between separate Lightning App Pages within the same app.
  Modals use lightning-modal (SLDS v2) for accessibility compliance. Avoid custom overlay divs.
  The VIP Contact Card (S5, S10) is reused across contexts — pass the triggering context as a prop to adjust the modal's action set if needed."

---

EXECUTION NOTES
- Name all layers clearly: "Swim Lane / VIP Playbook", "Screen / S3 My VIPs Table", "Arrow / S3→S5 Click VIP name", "Legend Frame"
- Swim lane labels must use auto-layout (vertical centering)
- All arrows must be vector paths with proper arrowheads (not shapes)
- Cross-lane arrows should be curved (cubic bezier), not straight, to avoid overlapping with thumbnails
- Zoom to fit entire diagram in viewport on completion
- Return Figma node IDs for all swim lane frames, screen thumbnails, and the legend frame
```
