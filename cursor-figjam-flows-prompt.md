# Cursor → FigJam: FanDuel VIP KAM — System Process Flows

## OBJECTIVE

Using the Figma MCP, create a new FigJam file named **"FanDuel VIP KAM — System Process Flows"** containing **5 frames**, one per system process flow. Each frame has two side-by-side views separated by a vertical divider: a Stakeholder view on the left and a Technical view on the right.

---

## GLOBAL STYLE TOKENS

Apply these consistently across all frames and all elements:

| Token | Value |
|---|---|
| Primary accent | `#0060a3` (FanDuel blue) |
| Frame background | `#f8f9fb` |
| Font family | Inter |
| Frame title font size | 28px, bold, color `#0060a3` |
| Subtitle font size | 14px, color `#6b7280` |
| Swimlane label background | `#0060a3` |
| Swimlane label text | white, 12px, bold, uppercase |
| Arrow / connector color | `#0060a3` |
| Arrow label font size | 11px, color `#374151` |
| Step shape (rounded rect) fill | `#e8f0fe` |
| Decision shape (diamond) fill | `#fef3c7` |
| Blocked / RG flag shape fill | `#fee2e2` |
| Open scoping shape fill | `#fff7ed` |
| Data / IO shape (parallelogram) fill | `#f0fdf4` |
| LOE badge — HIGH | background `#fee2e2`, text `#991b1b` |
| LOE badge — MEDIUM | background `#fef3c7`, text `#92400e` |
| LOE badge — LOW | background `#f0fdf4`, text `#065f46` |
| Open question flag | `⚠` prefix, fill `#fff7ed`, border `#f59e0b` |
| Divider line | `#d1d5db`, 2px, dashed |
| Inter-frame gap (vertical stack) | 80px |

---

## FRAME STRUCTURE (apply to every frame)

Each frame is **2400px wide × auto height** (minimum 1400px tall). Internal layout:

```
[ Frame Title — Flow N: Name                                    ]
[ Subtitle: one-line description                                ]
─────────────────────────────────────────────────────────────────
[ LEFT HALF: Stakeholder View (1100px) ] [ DIVIDER ] [ RIGHT HALF: Technical View (1100px) ]
─────────────────────────────────────────────────────────────────
[ Legend (full width, bottom)                                   ]
```

### Divider
- A vertical line at x=1200, spanning from just below the subtitle to just above the legend
- Above the line on the left: label "Stakeholder View" — Inter 13px bold, color `#0060a3`
- Above the line on the right: label "Technical View" — Inter 13px bold, color `#0060a3`

### Swimlane structure (both halves)
Each half contains horizontal swimlane bands stacked top to bottom. Each swimlane band:
- Full width of its half (1100px)
- Height: minimum 160px, expand to fit content
- Swimlane label: left-aligned pill (140px wide, 32px tall), background `#0060a3`, text white — placed at the vertical center of the band, flush left
- Content area: shapes and arrows placed inside the band to the right of the label

### Shape dimensions
- Rounded rectangle (step): 180px × 60px, corner radius 10px
- Diamond (decision): 120px × 80px
- Parallelogram (data/IO): 180px × 60px, skew 15deg
- LOE badge: 64px × 24px, corner radius 6px, 10px font, placed in the upper-right corner of its parent shape
- ⚠ flag sticky note: 120px × 48px

### Arrows
- Style: directed arrow, elbow routing
- Color: `#0060a3`
- Label: short text on the connector, 11px Inter, `#374151`

### Legend (each frame, full width, 100px tall)
Four legend items in a horizontal row, centered:
- Rounded rect `#e8f0fe` — "Step / Action"
- Diamond `#fef3c7` — "Decision"
- Parallelogram `#f0fdf4` — "Data / IO"
- Rounded rect `#fee2e2` — "Blocked / RG Flag"
- Rounded rect `#fff7ed` — "Open Scoping"

---

## FRAME 1: NBA Recommendation Engine

**Frame title:** `Flow 1: NBA Recommendation Engine`
**Subtitle:** `Agentforce surfaces next-best-action recommendations to KAMs based on multi-platform VIP activity`

### Left Half — Stakeholder View

**Swimlane 1 — Data Sources**
Shapes (parallelograms, left to right):
1. `Sportsbook Platform` — fill `#f0fdf4`
2. `Casino CRM` — fill `#f0fdf4`
3. `DFS Platform` — fill `#f0fdf4`
4. `TVG + FD Racing` — fill `#fff7ed`, ⚠ flag: "sparse data coverage"

**Swimlane 2 — System / Agentforce**
Shapes (left to right):
1. Rounded rect: `Collect Activity`
2. → arrow labeled "activity received" →
3. Diamond: `RG Flag?` — fill `#fef3c7`
4. Arrow from diamond "Yes" → Rounded rect `#fee2e2`: `Flag for Compliance — pause NBA`
5. Arrow from diamond "No" →
6. Rounded rect: `Score VIPs`
7. → arrow →
8. Rounded rect: `Generate Recommendation`
9. → arrow →
10. Rounded rect: `Set Priority` (label inside: "P1 / P2 / P3")

**Swimlane 3 — KAM**
Shapes (left to right):
1. Rounded rect: `NBA Alert Badge appears`
2. → arrow →
3. Rounded rect: `Review Recommendation Card`
4. → arrow →
5. Diamond: `Accept?` — fill `#fef3c7`
6. Arrow "Yes" → Rounded rect: `Action Taken`
7. Arrow "No" → Rounded rect: `Dismiss with Reason`
8. Both → merge → Rounded rect: `Outcome Logged`
9. → arrow labeled "feedback loop" →
10. Back to Swimlane 2 "Score VIPs" (curved return arrow)

### Right Half — Technical View

**Swimlane 1 — Data Sources**
Shapes (parallelograms):
1. `Sportsbook Platform` — label below: "Scheduled Batch every 4h · REST API"
2. `Casino CRM` — label below: "Scheduled Batch every 4h · REST API"
3. `DFS Platform` — label below: "Contest-trigger Batch · REST API"
4. `TVG + FD Racing` — fill `#fff7ed`, label below: "Nightly Batch · ⚠ TBD: sparse <70% coverage"

LOE badge on each: Sportsbook = MEDIUM, Casino = MEDIUM, DFS = MEDIUM, TVG/Racing = LOW

**Swimlane 2 — Feature Engineering / Agentforce**
Shapes:
1. Rounded rect: `Feature Engineering` — sub-label: "recency · frequency · GGR delta · contact gap"
2. → arrow →
3. Rounded rect: `Einstein Prediction Service` — sub-label: "REST API callout · sync"  — LOE badge: HIGH
4. → arrow →
5. Rounded rect: `NBA_Recommendation__c created` — LOE badge: LOW
6. → arrow labeled "Platform Event fired" →
7. Rounded rect: `Platform Event: NBA_New__e`
8. → arrow → (connects to Swimlane 3)

**Swimlane 3 — KAM Console / Apex**
Shapes:
1. Rounded rect: `Console receives Platform Event`
2. → arrow →
3. Rounded rect: `KAM reviews NBA_Recommendation__c`
4. → arrow →
5. Diamond: `accepted__c = true?`
6. Arrow "true" → Rounded rect: `NBA_Feedback__c written · action_taken__c stamped`
7. Arrow "false" → Rounded rect: `dismissed_reason__c stamped on NBA_Recommendation__c`
8. Both → Rounded rect: `Outcome updates Einstein scoring loop`

⚠ sticky note in this half: "⚠ Open: Einstein model training cadence not yet defined — weekly retrain assumed"

---

## FRAME 2: Awareness Center Alert Flow

**Frame title:** `Flow 2: Awareness Center Alert Flow`
**Subtitle:** `Real-time and near-real-time detection of high-risk VIP events surfaces actionable alerts to KAMs`

### Left Half — Stakeholder View

**Swimlane 1 — Platform**
Shapes (parallelograms):
1. `Large Wager Detected` — fill `#fee2e2`
2. `Payment Issue Flagged` — fill `#fee2e2`
3. `Sustained Loss Detected` — fill `#fee2e2`

**Swimlane 2 — Detection / System**
Shapes:
1. Diamond: `Alert Type?` — fill `#fef3c7`
2. Three arrows out: "Big Swing" → Rounded rect `#e8f0fe`: `Big Swing Alert`, "Payment" → Rounded rect: `Payment Alert`, "Loss Pattern" → Rounded rect: `Losing Streak Alert`
3. All three → Diamond: `RG Threshold breached?` — fill `#fee2e2`, amber
4. Arrow "Yes" → Rounded rect `#fee2e2`: `RG Referral — Compliance Notified`
5. Arrow "No" → Diamond: `Contact allowed?`
6. Arrow "Yes" → Swimlane 3
7. Arrow "No" → Rounded rect `#fee2e2`: `Alert suppressed — VIP on exclusion`

**Swimlane 3 — KAM**
Shapes:
1. Rounded rect: `Alert appears in Awareness Center tab`
2. → arrow →
3. Diamond: `KAM action?` — fill `#fef3c7`
4. Arrow "Respond Now" → Rounded rect: `Contact VIP — log outreach`
5. Arrow "Monitor + Snooze" → Rounded rect: `Snoozed with review date`
6. Arrow "Escalate" → Rounded rect `#fee2e2`: `Escalate to Manager`
7. All three → Rounded rect: `Resolution Logged`
8. Dashed arrow from "Snoozed": "if 48h unresolved → auto-escalate"

### Right Half — Technical View

**Swimlane 1 — Source Platforms**
Shapes:
1. `Sportsbook` — sub-label: "Platform Events · near-real-time"
2. `Casino CRM` — sub-label: "Platform Events · near-real-time"
3. `Racing` — sub-label: "Scheduled Batch T+1 · ⚠ latency gap"

**Swimlane 2 — Apex / Detection**
Shapes:
1. Rounded rect: `Apex Trigger on Wagering_Event__c`
2. → arrow →
3. Rounded rect: `VIP_Baseline__c threshold compare`
4. → arrow →
5. Rounded rect: `RG_Classification__c lookup`
6. → arrow →
7. Rounded rect: `Alert_Record__c created` — LOE badge: MEDIUM
8. → arrow labeled "Platform Event" →
9. Rounded rect: `Alert_New__e Platform Event fired`

**Swimlane 3 — Console / Scheduled Apex**
Shapes:
1. Rounded rect: `Awareness Center tab — Alert_Record__c list view`
2. → arrow →
3. Rounded rect: `KAM updates status__c field`
4. → arrow →
5. Rounded rect: `Scheduled Apex — hourly escalation check` — LOE badge: MEDIUM
6. Diamond: `open > 48h?`
7. Arrow "Yes" → Rounded rect `#fee2e2`: `auto_escalated__c = true · Manager notified`
8. Arrow "No" → Rounded rect: `remains in queue`

⚠ sticky notes:
- "⚠ Open: Racing T+1 latency — alert may lag 24h for racing wagers"
- "⚠ Open: RG suppression rules — confirm with Compliance before build"

---

## FRAME 3: VIP Feedback Pulse — Survey Data Flow

**Frame title:** `Flow 3: VIP Feedback Pulse — Survey Data Flow`
**Subtitle:** `Survey responses captured outside Salesforce are integrated, scored, and surfaced to KAMs as actionable feedback cards`

### Left Half — Stakeholder View

**Swimlane 1 — Survey**
Shapes:
1. Rounded rect: `Survey sent to VIP`
2. → arrow →
3. Rounded rect: `VIP responds`
4. → arrow →
5. Parallelogram: `Response data in Tableau`

**Swimlane 2 — Integration (⚠ route TBD)**
Shapes:
1. Diamond: `Integration path?` — fill `#fff7ed`, ⚠ label
2. Arrow "Option A: Tableau Connector (24h lag)" → Rounded rect `#fff7ed`: `Tableau Connector sync — daily batch`
3. Arrow "Option B: MuleSoft (near-real-time)" → Rounded rect `#fff7ed`: `MuleSoft API event — near-real-time`
4. Both arrows → Rounded rect: `Data lands in Salesforce`

**Swimlane 3 — System / Agentforce**
Shapes:
1. Rounded rect: `Einstein NLP sentiment scoring`
2. → arrow →
3. Diamond: `Sentiment?` — fill `#fef3c7`
4. Arrow "Positive" → Rounded rect `#e8f0fe`: `Positive badge applied`
5. Arrow "Neutral" → Rounded rect `#e8f0fe`: `Neutral — no action`
6. Arrow "Negative" → Rounded rect `#fee2e2`: `Negative badge — surface in NBA queue`
7. All → Rounded rect: `Star rating captured · VIP record updated`

**Swimlane 4 — KAM**
Shapes:
1. Rounded rect: `Open Feedback Pulse card in console`
2. → arrow →
3. Rounded rect: `Review VIP response`
4. → arrow →
5. Diamond: `Negative sentiment?`
6. Arrow "Yes" → Rounded rect `#fee2e2`: `Prioritise outreach — NBA surfaced`
7. Arrow "No" → Rounded rect: `Log review · mark complete`

### Right Half — Technical View

**Swimlane 1 — Survey Tool**
Shapes:
1. Parallelogram: `Survey platform (tool TBD)` — fill `#fff7ed`, ⚠ "survey tool not yet confirmed"
2. → arrow →
3. Parallelogram: `Tableau data store`

**Swimlane 2 — Integration Layer**
Shapes:
1. Diamond: `Connector type?` — fill `#fff7ed`
2. Arrow "A" → Rounded rect `#fff7ed`: `Tableau Salesforce Connector · 24h schedule · Survey_Response__c upsert` — LOE badge: MEDIUM
3. Arrow "B" → Rounded rect `#fff7ed`: `MuleSoft Anypoint · near-RT · Survey_Response__c upsert` — LOE badge: HIGH
4. ⚠ sticky: "⚠ Open: integration method not scoped — Decision required before sprint planning"

**Swimlane 3 — Salesforce / Einstein**
Shapes:
1. Rounded rect: `Survey_Response__c record created / upserted`
2. → arrow →
3. Rounded rect: `Einstein NLP — sentiment_score__c field updated` — LOE badge: MEDIUM
4. → arrow →
5. Rounded rect: `feedback_unreviewed_count__c incremented on VIP_Account__c`
6. → arrow →
7. Rounded rect: `Platform Event: Feedback_New__e fired`

**Swimlane 4 — KAM Console**
Shapes:
1. Rounded rect: `Feedback Pulse card rendered in console`
2. → arrow →
3. Rounded rect: `KAM marks reviewed — reviewed_by__c · reviewed_date__c stamped`
4. → arrow →
5. Rounded rect: `NBA_Recommendation__c created if sentiment = Negative`

---

## FRAME 4: VIP Onboarding Automation

**Frame title:** `Flow 4: VIP Onboarding Automation`
**Subtitle:** `Candidate identification through KAM assignment, first contact SLA, 90-day engagement tracking, and graduation`

### Left Half — Stakeholder View

**Swimlane 1 — Identification**
Shapes:
1. Parallelogram: `Platform threshold alert triggered`
2. → arrow "OR" →
3. Rounded rect: `Manual nomination by KAM / Manager`
4. → arrow →
5. Rounded rect: `VIP enters Candidate Queue`

**Swimlane 2 — Assignment**
Shapes:
1. Rounded rect: `Agentforce calculates Readiness Score`
2. → arrow →
3. Rounded rect: `Match to KAM — capacity + segment fit`
4. → arrow →
5. Diamond: `Manager approves?` — fill `#fef3c7`
6. Arrow "Yes" → Rounded rect: `KAM notified — assignment confirmed`
7. Arrow "No" → Rounded rect: `Re-evaluate or re-assign`

**Swimlane 3 — First Contact**
Shapes:
1. Rounded rect: `48h SLA timer starts`
2. → arrow →
3. Rounded rect: `Task auto-created for KAM`
4. → arrow →
5. Rounded rect: `KAM contacts VIP`
6. → arrow →
7. Rounded rect: `First contact milestone logged`

**Swimlane 4 — Monitoring (30/60/90d)**
Shapes:
1. Rounded rect: `30d engagement check`
2. → arrow →
3. Rounded rect: `60d engagement check`
4. → arrow →
5. Rounded rect: `90d engagement check`
6. → arrow →
7. Diamond: `Meets graduation criteria?` — fill `#fef3c7`
8. Arrow "Yes" → Rounded rect `#e8f0fe`: `Graduate to Active VIP`
9. Arrow "No" → Rounded rect `#fff7ed`: `Re-engage — NBA surfaced`

**Swimlane 5 — Manager**
Shapes:
1. Rounded rect `#fee2e2`: `SLA breach alert (48h missed)`
2. Rounded rect: `Weekly digest — pipeline view`
3. Rounded rect: `Graduation approval`

### Right Half — Technical View

**Swimlane 1 — Identification**
Shapes:
1. Parallelogram: `Platform threshold triggers VIP_Candidate__c creation` — LOE badge: MEDIUM
2. → arrow →
3. Rounded rect: `Manual nomination — KAM creates VIP_Candidate__c record`
4. → arrow →
5. Rounded rect: `status__c = "Pending Review"`

**Swimlane 2 — Assignment**
Shapes:
1. Rounded rect: `Einstein Readiness Score model — readiness_score__c` — LOE badge: HIGH
2. → arrow →
3. Rounded rect: `Query KAM_Capacity__c + Segment_Specialty__c — best-fit KAM selected`
4. → arrow →
5. Diamond: `manager_approved__c?`
6. Arrow "true" → Rounded rect: `assigned_kam__c populated · status__c = "Assigned"`
7. Arrow "false" → Rounded rect: `status__c = "Needs Review"`

**Swimlane 3 — First Contact / Milestones**
Shapes:
1. Rounded rect: `Onboarding_Milestone__c: type = "First Contact" · due_date__c = now+48h`
2. → arrow →
3. Rounded rect: `Scheduled Apex SLA monitor — hourly check` — LOE badge: MEDIUM
4. Diamond: `due_date__c breached?`
5. Arrow "Yes" → Rounded rect `#fee2e2`: `Manager notification · SLA_breach__c = true`
6. Arrow "No" →
7. Rounded rect: `Task completed → milestone completed_date__c stamped`

**Swimlane 4 — 30/60/90d Monitoring**
Shapes:
1. Rounded rect: `Scheduled Apex — 30d · 60d · 90d jobs`
2. → arrow →
3. Rounded rect: `Engagement metrics evaluated vs Graduation_Criteria__c thresholds`
4. Diamond: `criteria met?`
5. Arrow "Yes" → Rounded rect: `VIP_Account__c status__c = "Active" · Graduation flow triggered` — LOE badge: MEDIUM
6. Arrow "No" → Rounded rect: `NBA_Recommendation__c created: type = "Re-engage"`

**Swimlane 5 — Manager / Reports**
Shapes:
1. Rounded rect: `SLA_breach__c = true → Platform Event → Manager alert`
2. Rounded rect: `Scheduled report — weekly pipeline digest`
3. Rounded rect: `Graduation approval — custom approval process on VIP_Account__c` — LOE badge: LOW

⚠ sticky notes:
- "⚠ Open: Readiness Score model training data not yet defined"
- "⚠ Open: Graduation_Criteria__c thresholds need segment-by-segment sign-off"

---

## FRAME 5: Data Integration Architecture

**Frame title:** `Flow 5: Data Integration Architecture`
**Subtitle:** `How data flows from all source platforms into Salesforce, is unified, and powers Agentforce and the KAM Console`

### Left Half — Stakeholder View

**Swimlane 1 — Source Systems**
Shapes (parallelograms, left to right):
1. `Sportsbook` — fill `#e8f0fe`, label: "⚡ near real-time"
2. `Casino CRM` — fill `#e8f0fe`, label: "⚡ near real-time"
3. `DFS Platform` — fill `#e8f0fe`, label: "🔄 contest batch"
4. `TVG` — fill `#fff7ed`, label: "⚠ TBD — T+1 est."
5. `FD Racing` — fill `#fff7ed`, label: "⚠ TBD — T+1 est."
6. `Tableau / Surveys` — fill `#fff7ed`, label: "⚠ TBD — 24h+"

**Swimlane 2 — Salesforce**
Shapes (left to right):
1. Rounded rect: `Data Ingest Layer` — sub-label: "deduplicate · validate · normalise"
2. → arrow →
3. Rounded rect: `Unified VIP Data Layer` — sub-label: "canonical VIP record"
4. → arrow →
5. Rounded rect: `Agentforce + Einstein`

**Swimlane 3 — Console / Users**
Shapes:
1. Rounded rect: `KAM Console`
2. Sub-shapes (smaller, stacked):
   - `Taylor` (P2 Sportsbook VIPs)
   - `Morgan` (P1 Casino VIPs)
   - `Casey` (Mixed portfolio)
   - `Reese` (DFS + Racing)
3. Rounded rect: `Avery — Manager`
4. Rounded rect: `Pat — Director`

### Right Half — Technical View

**Swimlane 1 — Source Systems + Connectors**
Shapes:
1. `Sportsbook` → connector label: "REST API + Platform Events (near-RT)"
2. `Casino CRM` → connector label: "REST API + Platform Events (near-RT)"
3. `DFS Platform` → connector label: "Scheduled Batch — contest-end trigger"
4. `TVG` → fill `#fff7ed`, connector label: "⚠ TBD: file extract OR REST API"
5. `FD Racing` → fill `#fff7ed`, connector label: "⚠ TBD: internal API TBD"
6. `Tableau` → fill `#fff7ed`, connector label: "⚠ TBD: Tableau Connector OR MuleSoft"

LOE badges: Sportsbook = MEDIUM, Casino = MEDIUM, DFS = MEDIUM, TVG = LOW, Racing = LOW, Tableau = HIGH

**Swimlane 2 — Salesforce Ingest + Data Layer**
Shapes:
1. Rounded rect: `Ingest: canonical VIP id dedup · upsert logic · data quality validation` — LOE badge: HIGH
2. → arrow →
3. Rounded rect: `Wagering_Event__c · VIP_Account__c · VIP_Baseline__c unified` — LOE badge: MEDIUM
4. → arrow →
5. Diamond: `Data Cloud in scope?` — fill `#fff7ed`
6. Arrow "Yes" → Rounded rect `#fff7ed`: `Data Cloud unification layer` — LOE badge: HIGH
7. Arrow "No" → Rounded rect: `Standard SOQL aggregation queries`
8. Both → Rounded rect: `Einstein models consume unified layer`

**Swimlane 3 — Consumers**
Shapes:
1. Rounded rect: `NBA Recommendation Engine (Flow 1)`
2. Rounded rect: `Awareness Center (Flow 2)`
3. Rounded rect: `Feedback Pulse (Flow 3)`
4. Rounded rect: `Onboarding Automation (Flow 4)`
5. Rounded rect: `KAM Console UI — SLDS components`

⚠ sticky notes:
- "⚠ Open: Data Cloud licensing — confirm with Salesforce AE before architecture decision"
- "⚠ Open: Racing data confidence dampener — <70% coverage requires display flag in console"
- "⚠ Open: TVG + FD Racing integration method not yet scoped — blockers for Flows 1, 2"

---

## EXECUTION INSTRUCTIONS

After creating all 5 frames, carry out the following:

**Layer naming convention:**
- Frames: `flow-01-nba`, `flow-02-awareness`, `flow-03-feedback`, `flow-04-onboarding`, `flow-05-integration`
- Within each frame, name layers: `title`, `subtitle`, `divider`, `stakeholder-view`, `technical-view`, `legend`
- Within each swimlane group: `swimlane-[name]-label`, `swimlane-[name]-content`
- Each shape: `shape-[descriptive-action-slug]`, e.g. `shape-rg-flag-check`, `shape-einstein-prediction`
- Each connector: `arrow-[from-slug]-to-[to-slug]`

**Layout:**
- Stack all 5 frames vertically on the FigJam canvas
- Gap between frames: exactly 80px
- First frame positioned at canvas origin (0, 0)
- Use auto-layout groups within swimlane content areas where the Figma MCP supports it

**On completion:**
- Return the FigJam file URL
- Confirm the count of frames created (should be 5)
- List any shapes or connectors that could not be created and why

**Quality checks before returning:**
- Every frame has both a Stakeholder and Technical half
- Every frame has a legend
- The divider line is present and labeled on all 5 frames
- All ⚠ open question sticky notes are placed
- All LOE badges are placed on the correct shapes in the Technical views
- All arrow labels are present

---

## [PASTE FLOW DATA HERE] — PER-FRAME REFERENCE

The complete step-by-step content for each frame is embedded in the FRAME sections above. When building each frame, use the corresponding section as the authoritative source of truth for shape labels, swimlane assignments, fill colors, LOE badge levels, and ⚠ notes. Do not infer or invent content — if a detail is not listed above, leave a `[TBD]` placeholder label on that shape.
