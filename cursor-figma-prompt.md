# Cursor → Figma Prompt — VIP KAM User Journey Maps

Copy this entire prompt into Cursor chat. Paste the journey stage data where indicated at the bottom.

---

```
You are working in Figma using the Figma MCP. Create a user journey map diagram frame for the following journey. Follow all layout, color, typography, and structure specifications exactly.

---

FRAME SETUP
Create a new Figma frame named "[JOURNEY TITLE]" with:
- Width: auto-width (fit content), minimum 1600px
- Height: auto-height (fit content)
- Background: #F8F9FA
- Corner radius: 8px
- Padding: 48px on all sides
- Auto-layout: vertical, gap 32px

---

TITLE BLOCK
- Journey title: Inter Bold, 28px, color #16213E
- Tagline: Inter Regular, 16px, color #5C6B8A
- "Primary Actor:" and "Secondary:" labels: Inter SemiBold 13px, color #5C6B8A, on one row
- Horizontal rule below: #E2E8F0, 1px, full width

---

JOURNEY TABLE
Auto-layout frame, horizontal, gap 2px.
Each STAGE = one COLUMN. Each DIMENSION = one ROW within each column.
Row order: Stage Name | KAM Action | System Response | Touchpoint | Emotion | Pain Point | Opportunity

LEGEND COLUMN (far left, fixed, 160px wide):
- Each cell: dimension name, Inter SemiBold 12px, #5C6B8A, uppercase, vertically centered
- Cell background: #EEF1F6
- Top cell (Stage Name header): blank, background #FFFFFF

STAGE COLUMNS (200px min width, expand to fit):
- 7 cells stacked vertically in auto-layout
- Cell padding: 12px horizontal, 10px vertical
- Cell border: 1px solid #E2E8F0

Row styling:
- STAGE NAME: background #16213E, Inter Bold 13px, #FFFFFF, centered
- KAM ACTION: background #FFFFFF, Inter Regular 12px, #1A202C
- SYSTEM RESPONSE: background #F7F9FC, Inter Regular 12px, #1A202C
- TOUCHPOINT: background #EEF4FF, Inter SemiBold 12px, #2563EB
- EMOTION — color by tone:
  - Positive (confident, satisfied, proud, energized, optimistic, relieved): background #D1FAE5, text #065F46
  - Neutral (focused, analytical, curious, evaluative, watchful, thoughtful): background #FEF9C3, text #92400E
  - Friction (anxious, cautious, pressured, skeptical, uncertain, alert): background #FEE2E2, text #991B1B
- PAIN POINT: background #FFF5F5, left border 3px solid #F87171, Inter Regular 12px, #7F1D1D
- OPPORTUNITY: background #EFF6FF, left border 3px solid #60A5FA, Inter Regular 12px, #1E3A5F

---

SEGMENT NUANCES BLOCK
Four pill-style cards in a horizontal auto-layout row, gap 12px, equal width:
- Background: #F0F4FF, border-radius 8px, padding 16px
- Segment label: Inter Bold 12px, #2563EB
- Content: Inter Regular 12px, #374151
- Order: Sportsbook | Casino | Racing | DFS

---

KEY MOMENTS BLOCK
Full-width block, background #FFFBEB, border-radius 8px, padding 20px, left border 4px solid #F59E0B:
- Label: Inter Bold 13px, #92400E, uppercase
- Content: Inter Regular 13px, #1A202C, max-width 900px

---

TYPOGRAPHY
- All fonts: Inter (fallback: SF Pro Display, system-ui)
- No text below 11px

---

JOURNEY DATA

[PASTE JOURNEY STAGES HERE — copy from user-journey-maps.md]

Format each stage as:

STAGE N: [Stage Name]
KAM Action: [text]
System Response: [text]
Touchpoint: [text]
Emotion: [text] | TONE: [positive / neutral / friction]
Pain Point: [text]
Opportunity: [text]

Then append:

SEGMENT NUANCES:
Sportsbook: [text]
Casino: [text]
Racing: [text]
DFS: [text]

KEY MOMENTS:
[text]

---

EXECUTION NOTES
- No decorative elements, icons, or illustrations not specified above
- Maintain consistent row heights within each dimension row across all stage columns
- Name all layers descriptively: e.g., "Stage 3 - Emotion Cell", "Segment Nuances - Casino Card"
- After creating the frame, zoom Figma to fit the frame in the viewport
- Return the Figma node ID of the completed frame
```
