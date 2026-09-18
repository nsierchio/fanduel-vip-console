---
# Flow 1: NBA Recommendation Engine

## Version 1 — Technical (Solution Architects / LOE Scoping)

**Trigger:** Scheduled batch (every 4 hours for Sportsbook/Casino), platform event on contest completion (DFS), nightly batch (Racing)
**Primary Salesforce Objects:** `VIP_Account__c`, `NBA_Recommendation__c`, `NBA_Feedback__c`, `VIP_Segment__c`, `Wagering_Event__c`, `Casino_Session__c`, `DFS_Contest_Entry__c`, `Racing_Activity__c`
**Integration Pattern:** Scheduled Batch (primary scoring), Platform Event (Sportsbook/Casino event-triggered NBA expiry), REST API callout to Einstein Prediction Service

---

### Steps

1. **Data Ingestion — Cross-Platform Activity Pull**
   - Salesforce Scheduled Apex batch queries `Wagering_Event__c`, `Casino_Session__c`, `DFS_Contest_Entry__c`, `Racing_Activity__c` records updated since last run window
   - GGR delta, deposit frequency, session recency, loss/win streaks, product mix, and engagement gap days are pulled per VIP
   - For Racing: separate queries against `TVG_Activity__c` and `FanDuel_Racing_Activity__c` objects; data sparsity flag applied if record count below threshold
   - LOE: **MEDIUM**
   - ⚠️ Racing activity objects depend on integration patterns being confirmed (see Flow 5). If TVG data arrives via scheduled batch rather than near-real-time, Racing NBAs will always be T+1 minimum.

2. **Feature Engineering — Scoring Input Construction**
   - Apex or Einstein Discovery prep transforms raw activity into feature vectors: recency score, frequency score, monetary value delta, product affinity index, engagement trend (3/7/30d rolling), contact gap in days, RG flag status
   - Segment-specific feature weights applied: Sportsbook weights wagering event recency heavily; Casino weights session depth and game category; DFS weights contest entry rate relative to contest calendar; Racing applies confidence dampener when data coverage < 70%
   - Feature vector written to `NBA_Feature_Snapshot__c` (custom object, one record per VIP per scoring run)
   - LOE: **HIGH**
   - ⚠️ Feature weight configuration — is this managed as Einstein Discovery recipe parameters, custom metadata, or hardcoded Apex? Decision affects maintainability and KAM manager tuning capability.

3. **Einstein Model Scoring**
   - Callout from Apex to Einstein Prediction Service (REST API) passing feature vector
   - Model returns: recommendation category (Retention Outreach, Upsell, Reactivation, Loyalty Milestone, Check-In), confidence score (0–1.0), predicted lifetime value impact, recommended action payload
   - Separate model instances per segment (Sportsbook, Casino, DFS); Racing shares Sportsbook model with confidence dampener applied post-scoring
   - Callout timeout: 10s; retry logic: 2 attempts with exponential backoff; on failure, fall back to rules-based NBA (see Error Handling)
   - LOE: **HIGH**
   - ⚠️ Model training cadence TBD — weekly retrain? Monthly? Who owns the Einstein Discovery dataset refresh?

4. **Recommendation Generation and Deduplication**
   - Scored output written to `NBA_Recommendation__c` with fields: `Segment__c`, `Category__c`, `Confidence_Score__c`, `Expiry_DateTime__c`, `Status__c` (New), `Recommendation_Text__c` (Einstein-generated natural language summary), `Supporting_Data_JSON__c`
   - Deduplication check: if open NBA of same category exists for same VIP with confidence score within 0.1, suppress new record and update existing record's `Refreshed_DateTime__c`
   - Expiry windows set by segment: Sportsbook = 48h (event-driven), Casino = 72h, DFS = until next contest cycle start, Racing = 7 days
   - LOE: **MEDIUM**

5. **Priority Tiering**
   - Apex post-processing applies priority tier: Tier 1 (confidence ≥ 0.80 + VIP tier Platinum/Diamond), Tier 2 (confidence 0.60–0.79 or Gold tier), Tier 3 (< 0.60 or lower VIP tier)
   - Tier 1 NBAs trigger a Salesforce Platform Event (`NBA_Priority_Event__e`) consumed by the console UI for real-time badge/notification
   - Tier 3 NBAs with confidence < 0.40 are stored but not surfaced unless KAM explicitly requests "show all"
   - LOE: **LOW**

6. **Delivery to KAM Console**
   - Console Lightning Web Component queries `NBA_Recommendation__c` via SOQL filtered to KAM's assigned VIP book, Status = New or In-Review, non-expired
   - LWC subscribes to `NBA_Priority_Event__e` Platform Event channel for real-time Tier 1 badge updates without page refresh
   - Recommendation card displays: VIP name, segment badge, category, AI-generated rationale, confidence indicator (visual), expiry countdown, Accept / Dismiss / Snooze actions
   - LOE: **MEDIUM**

7. **KAM Accept / Dismiss / Snooze**
   - Accept: `Status__c` → In-Progress; linked `Task__c` auto-created with subject, due date, and activity type pre-populated; KAM can edit before saving
   - Dismiss: `Status__c` → Dismissed; `Dismiss_Reason__c` picklist required (Not Relevant, Already Contacted, Customer Request, Other)
   - Snooze: `Status__c` → Snoozed; `Snooze_Until__c` date required; reactivates via scheduled batch
   - All transitions logged with timestamp and KAM user ID
   - LOE: **LOW**

8. **Feedback Loop — Model Retraining Input**
   - `NBA_Feedback__c` record created on Accept or Dismiss, capturing: recommendation category, confidence at delivery, VIP segment, KAM action, dismiss reason (if applicable), and — if accepted — subsequent activity outcome (wagering event within 14d? Y/N, populated by scheduled batch at T+14)
   - Feedback dataset exported to Einstein Discovery training pipeline on weekly scheduled batch
   - LOE: **MEDIUM**
   - ⚠️ Outcome attribution window — 14 days appropriate for Sportsbook; Casino may need 7 days; DFS is contest-cycle-specific. Confirm with data science team.

---

### Error Handling

- Einstein Prediction Service callout timeout / 5xx → Fall back to rules-based NBA engine (Apex, custom metadata rules by segment); NBA flagged with `Confidence_Source__c` = Rules-Based; KAM sees visual indicator that AI scoring was unavailable
- Duplicate Platform Event delivery (idempotency) → `NBA_Recommendation__c` deduplication check on external ID before insert
- Feature vector missing required fields (sparse data — Racing) → NBA still generated with `Low_Confidence_Flag__c` = true; confidence dampener applied; KAM console shows "Limited data available" label on card
- Batch job failure mid-run → Standard Salesforce async error email + custom `Batch_Run_Log__c` record with failure state; next scheduled run picks up from last successful watermark

---

### LOE Summary

| Component | Complexity | Notes |
|---|---|---|
| Multi-platform data ingestion (Sportsbook, Casino, DFS) | HIGH | Depends on real-time vs. batch feeds confirmed in Flow 5 |
| Racing dual-system ingestion + sparsity handling | VERY HIGH | TVG + FanDuel Racing integration patterns unconfirmed; data normalization required |
| Feature engineering + segment-specific weighting | HIGH | Custom Apex + possible Einstein Discovery recipe; tuning requires data science input |
| Einstein model scoring (REST callout + retry) | HIGH | Separate model instances per segment; training pipeline ownership TBD |
| Recommendation deduplication + expiry logic | MEDIUM | Custom Apex; straightforward but requires careful segment-specific config |
| Priority tiering + Platform Event delivery | LOW | Apex post-processing + standard PE pattern |
| Console LWC (card rendering + real-time badge) | MEDIUM | LWC + PE subscription; design already partially prototyped |
| KAM action tracking (Accept/Dismiss/Snooze) | LOW | Standard Flow / Apex trigger on status change |
| Feedback loop + outcome attribution batch | MEDIUM | Delayed attribution requires two-pass batch logic |

---

## Version 2 — Stakeholder (Business + SA Review)

**What triggers this:** Every few hours, the system checks each VIP's recent activity across all platforms. When enough signals accumulate — a big bet placed, a casino session ending, a DFS contest completed, or a player going quiet — Agentforce calculates whether the KAM should reach out, and how urgently.

**What the KAM sees:** A recommendation card on their console showing the VIP's name, what action to take (e.g., "Retention Outreach"), why (a plain-English summary), a confidence indicator, and how long the recommendation is valid before it expires.

---

### Swimlane Flow

**DATA LAYER**
Sportsbook / Casino / DFS / Racing platforms → activity events and GGR data → Salesforce data layer → feature snapshot per VIP

**SALESFORCE / AGENTFORCE**
Agentforce scores each VIP → assigns recommendation category + confidence → applies priority tier → delivers to console → tracks KAM response → feeds outcome back to model

**KAM**
Sees recommendation card → reviews AI rationale → accepts (creates a task) / dismisses (logs reason) / snoozes → follows up → outcome tracked automatically at T+14

---

### ASCII Flow Diagram

```
[Platform Activity: Sportsbook / Casino / DFS / Racing]
          ↓
[Agentforce pulls activity signals every 4h (SB/Casino), on contest close (DFS), nightly (Racing)]
          ↓
[Feature snapshot built per VIP — recency, GGR delta, engagement trend, contact gap]
          ↓
[Einstein AI scores VIP → recommendation category + confidence score]
          ↓
[Confidence ≥ 0.80 + Platinum/Diamond tier?]
     ↓ YES                        ↓ NO
[TIER 1 — real-time         [TIER 2/3 — queued in
 badge on console]           next console refresh]
          ↓                              ↓
         [KAM sees recommendation card]
                      ↓
          [Accept? / Dismiss? / Snooze?]
     ↓ Accept         ↓ Dismiss         ↓ Snooze
[Task auto-       [Log reason —      [Reactivates
 created for       not surfaced       on chosen
 KAM]              again]             date]
          ↓
[At T+14 days: did VIP activity respond? → AI model learns]
```

---

### What Could Go Wrong

- The AI model is only as good as the data feeding it. If a platform's data feed is delayed or incomplete — especially Racing, which pulls from two separate systems — recommendations may be stale or missing entirely for those VIPs.
- A KAM dismissing recommendations without logging reasons quietly degrades the model over time. If dismiss reasons aren't enforced, the feedback loop breaks and recommendations get less relevant.
- Sportsbook recommendations tied to live events expire fast (48 hours). If a KAM's book is large and they're out of office, high-priority cards can expire unactioned with no escalation path.

---

### Business Value

- KAMs spend less time deciding who to call next — Agentforce surfaces the highest-impact VIPs automatically, so attention goes to execution rather than prioritization.
- Recommendations improve over time as KAMs accept and dismiss — every action is a training signal, making the model smarter for the specific FanDuel VIP population.
- Racing KAMs, who lack the data richness of Sportsbook or Casino, still receive guidance — lower confidence but still better than no signal, with the system being transparent about its uncertainty.

---
---
# Flow 2: Awareness Center Alert Flow

## Version 1 — Technical (Solution Architects / LOE Scoping)

**Trigger:** Platform event from Sportsbook/Casino on threshold breach (near-real-time), OR Scheduled Apex batch (every 1–4h) for pattern-based signals (Losing Streak, Payment Alert)
**Primary Salesforce Objects:** `VIP_Account__c`, `Awareness_Alert__c`, `RG_Classification__c`, `Compliance_Routing__c`, `Alert_Action__c`, `Wagering_Event__c`, `Casino_Session__c`
**Integration Pattern:** Platform Event (threshold breach), Scheduled Batch (streak/pattern detection), Salesforce Flow (compliance routing), Apex trigger (escalation timer)

---

### Steps

1. **Signal Detection — Threshold Breach or Pattern Recognition**
   - **Big Swing (Sportsbook):** Sportsbook platform publishes `Wagering_Threshold_Event__e` when a single wager exceeds configured threshold (e.g., $10K+ single bet or $50K+ in 24h rolling window). Platform Event consumed by Salesforce via streaming API subscriber.
   - **Payment Alert:** Scheduled Apex batch (every 1h) queries `Payment_Transaction__c` for declined deposits, chargeback flags, or rapid deposit-withdrawal patterns within 72h window.
   - **Losing Streak (Casino):** Scheduled Apex batch (every 4h) queries `Casino_Session__c` for net loss exceeding configurable threshold over rolling 7-day window. Casino Losing Streak carries highest RG sensitivity — flagged as `High_RG_Risk__c` = true automatically.
   - Thresholds stored in Custom Metadata (`Alert_Threshold__mdt`) for no-code configurability by operations team.
   - LOE: **HIGH**
   - ⚠️ Big Swing threshold values: who owns threshold configuration? VIP Technology (Andy Geissler) or Compliance? Confirm governance model.
   - ⚠️ Sportsbook Platform Event publishing: does the Sportsbook platform support outbound platform events to Salesforce today, or does this require a middleware polling layer?

2. **RG Classification Check**
   - On alert creation candidate, Apex immediately queries `RG_Classification__c` for the VIP account
   - `RG_Status__c` field evaluated: Clean, Monitor, Restricted, Excluded
   - If `RG_Status__c` = Restricted or Excluded: alert is created but `Contact_Permitted__c` = false; compliance routing flag set; direct KAM contact action blocked at UI level
   - If `RG_Status__c` = Monitor: alert created with `Contact_Permitted__c` = true but `RG_Caution_Flag__c` = true; KAM contact action available but advisory message displayed
   - Casino Losing Streak alerts: regardless of RG status, a secondary `RG_Review_Required__c` flag = true is set, triggering compliance notification (see Step 4)
   - LOE: **HIGH**
   - ⚠️ RG classification data source: is `RG_Classification__c` populated in real-time from a compliance system, or is it a manually maintained field? Latency here directly affects whether the contact gate is reliable. Critical dependency to confirm.

3. **Alert Record Creation in Salesforce**
   - `Awareness_Alert__c` record created with fields: `Alert_Type__c` (Big Swing / Payment Alert / Losing Streak), `VIP_Account__c`, `Detected_DateTime__c`, `Severity__c` (Low / Medium / High / Critical), `RG_Contact_Permitted__c`, `Status__c` (Open), `Threshold_Value__c`, `Actual_Value__c`, `Expiry_DateTime__c`
   - Severity calculated by Apex: Losing Streak + High RG Risk → Critical; Big Swing over 2x threshold → High; standard threshold breach → Medium; Payment Alert first occurrence → Low
   - Duplicate suppression: if open alert of same type for same VIP exists within 24h, update existing record rather than creating new
   - LOE: **MEDIUM**

4. **Compliance Routing Decision**
   - Salesforce Flow (Record-Triggered) fires on `Awareness_Alert__c` insert
   - Branch 1: `RG_Review_Required__c` = true OR `Contact_Permitted__c` = false → Create `Compliance_Routing__c` task assigned to Compliance team queue; set `Compliance_Notified_DateTime__c`; KAM's Contact action in UI disabled until `Compliance_Cleared__c` = true
   - Branch 2: `RG_Caution_Flag__c` = true but contact permitted → Compliance notification sent (email/Salesforce notification) but no blocking; KAM sees advisory banner
   - Branch 3: Clean RG status → No compliance routing; alert surfaced directly to KAM
   - LOE: **HIGH**
   - ⚠️ Compliance team routing: is there a defined Compliance user group/queue in the Salesforce org today? Who clears the `Compliance_Cleared__c` flag and within what SLA?

5. **Delivery to Awareness Center UI**
   - Console Awareness Center LWC queries `Awareness_Alert__c` records for KAM's VIP book, Status = Open, ordered by Severity desc, Detected_DateTime desc
   - LWC subscribes to `Awareness_Alert__e` Platform Event (published on insert of Critical/High severity alerts) for real-time badge update without refresh
   - Alert card displays: VIP name, alert type, severity badge, triggered value vs. threshold, detection time, RG advisory (if applicable), Contact / Log Note / Escalate actions (Contact disabled if `RG_Contact_Permitted__c` = false)
   - LOE: **MEDIUM**

6. **KAM Action — Contact / Log Note / Escalate**
   - **Contact:** `Alert_Action__c` record created (type = Outreach); linked `Task__c` auto-created; `Status__c` → In Progress; contact timestamp logged
   - **Log Note:** Free-text note captured in `Alert_Action__c`; status → Acknowledged; alert remains open
   - **Escalate:** `Status__c` → Escalated; assigned to KAM Manager queue; `Escalated_DateTime__c` set; manager notified via Salesforce notification
   - All actions require KAM to be logged in user; action captured with user ID and timestamp
   - LOE: **LOW**

7. **Resolution Tracking**
   - KAM marks alert Resolved via console UI → `Status__c` → Resolved; `Resolved_DateTime__c` set; `Resolution_Note__c` required field (minimum 20 chars)
   - Resolution triggers Flow that checks linked Compliance routing (if any) — marks compliance task complete if not already cleared
   - LOE: **LOW**

8. **Auto-Escalation — SLA Breach**
   - Scheduled Apex batch (every 2h) queries open `Awareness_Alert__c` where `Status__c` = Open AND `Detected_DateTime__c` < NOW() minus SLA window
   - SLA windows by severity: Critical = 2h, High = 8h, Medium = 24h, Low = 48h
   - On SLA breach: `Status__c` → Auto-Escalated; `Escalated_DateTime__c` set; KAM Manager assigned; notification sent; `Escalation_Source__c` = System-Auto
   - LOE: **MEDIUM**
   - ⚠️ SLA window values above are placeholder — confirm with Avery (KAM Manager) and Pat (Director) during scoping.

---

### Error Handling

- Platform Event delivery failure (Sportsbook Big Swing) → Sportsbook-side retry required; Salesforce durable subscription stores up to 72h of replayed events. If gap exceeds 72h, batch reconciliation job runs against Sportsbook API to backfill missed threshold events.
- RG Classification field null or stale → Alert created with `Contact_Permitted__c` = false (fail-safe default); compliance routing triggered; KAM sees "RG status unavailable — escalated to compliance" message. Never default to contact-permitted when RG status unknown.
- Compliance routing queue empty (no active compliance users) → Alert auto-escalated to KAM Director; `Escalation_Reason__c` = Compliance Queue Unavailable; admin notification generated.
- Duplicate alert suppression failure (race condition on concurrent inserts) → Unique external ID on `Awareness_Alert__c` (composite of VIP ID + Alert Type + 24h bucket) enforces deduplication at database level.

---

### LOE Summary

| Component | Complexity | Notes |
|---|---|---|
| Big Swing Platform Event subscription (Sportsbook) | HIGH | Requires Sportsbook platform to support outbound events; may need middleware if not |
| Payment Alert batch detection | MEDIUM | Standard Apex batch; logic is straightforward |
| Casino Losing Streak batch detection | HIGH | Rolling window calculation + RG flag logic; Casino session data model must be confirmed |
| RG Classification check + contact gate | HIGH | Reliability depends on RG data source latency — this is a compliance-critical path |
| Compliance routing Flow + blocking UI gate | HIGH | Legal/compliance sign-off required on routing logic; queue setup needed |
| Alert record creation + deduplication | MEDIUM | Custom object + composite unique ID; standard pattern |
| Awareness Center LWC + real-time badge | MEDIUM | LWC + PE subscription; severity-based UI state management |
| KAM action tracking (Contact/Log/Escalate) | LOW | Standard record actions + Task creation |
| Auto-escalation SLA batch | MEDIUM | Scheduled Apex; SLA values need business confirmation |

---

## Version 2 — Stakeholder (Business + SA Review)

**What triggers this:** Three types of signals trigger alerts — a VIP places an unusually large bet (Big Swing), a payment flag appears such as a declined deposit or chargeback pattern (Payment Alert), or a VIP has sustained significant losses over the past week (Losing Streak). Each type is detected differently, but all follow the same routing path before reaching the KAM.

**What the KAM sees:** An Awareness Center section on their console with alert cards ranked by severity. Each card shows what happened, how significant it is, and what actions are available. For some alerts, the Contact action will be locked — that means compliance has been notified and the KAM must wait for clearance before reaching out.

---

### Swimlane Flow

**DATA LAYER**
Sportsbook platform → wager threshold crossed → real-time signal to Salesforce
Casino CRM → session loss data → batch detection every 4 hours
Payment system → transaction anomalies → batch detection every 1 hour

**SALESFORCE / AGENTFORCE**
Signal detected → RG classification check → compliance routing decision → alert record created → delivered to console → SLA timer starts

**KAM**
Sees alert card with severity badge → Contact (if permitted) / Log Note / Escalate → resolves with required note → auto-escalated if ignored past SLA

---

### ASCII Flow Diagram

```
[Sportsbook: Big Swing threshold crossed]
[Casino: 7-day loss threshold crossed    ] → [Salesforce detects signal]
[Payment: Declined / chargeback pattern  ]
                    ↓
          [RG Status Check for this VIP]
                    ↓
     [Restricted or Excluded?]
     ↓ YES                        ↓ NO
[Contact BLOCKED —          [Casino Losing Streak?]
 Compliance notified]         ↓ YES          ↓ NO
     ↓                  [Compliance      [Alert live
     ↓                   notified,        on console,
     ↓                   KAM sees         contact
     ↓                   advisory]        permitted]
          [Alert card appears in Awareness Center]
                    ↓
     [Critical/High → real-time badge on console]
                    ↓
          [KAM Action Required]
     ↓ Contact      ↓ Log Note     ↓ Escalate
[Task created]  [Note logged]  [Manager notified]
                    ↓
          [SLA Timer Running]
                    ↓
     [SLA breached with no action?]
     ↓ YES
[Auto-escalated to KAM Manager — system-flagged]
```

---

### What Could Go Wrong

- If the system that tracks RG classifications is not connected in real time, a Losing Streak alert could reach a KAM for a player who should not be contacted. The system is designed to default to blocking contact when RG status is unavailable — but this needs to be confirmed as a hard rule, not a soft advisory.
- Casino Losing Streak is the most sensitive alert type. If compliance routing is slow or the compliance queue is unmanned, alerts sit open with no action while SLA timers run. A clear compliance SLA and on-call ownership must be defined.
- Sportsbook Big Swings are the highest-volume alert type. If threshold values are set too low, KAMs are flooded with low-value alerts and start ignoring the queue — defeating the purpose. Threshold calibration is a business configuration decision, not a technical one.

---

### Business Value

- KAMs are proactively alerted to high-risk or high-opportunity moments rather than discovering them days later during a routine check-in — reducing both financial risk and missed retention windows.
- The compliance gate on Losing Streak alerts means regulatory obligations are built into the workflow, not left to individual KAM judgment. This reduces compliance exposure at scale.
- Auto-escalation ensures nothing falls through the cracks on a busy day — if an alert isn't actioned within the SLA, the manager is notified automatically, creating accountability without requiring manual oversight.

---
---
# Flow 3: VIP Feedback Pulse — Survey Data Flow

## Version 1 — Technical (Solution Architects / LOE Scoping)

**Trigger:** Survey response recorded in source system (Tableau or 3rd-party survey tool connected to Tableau); Salesforce ingests on scheduled batch or webhook depending on integration method
**Primary Salesforce Objects:** `VIP_Account__c`, `Survey_Response__c`, `Feedback_Pulse__c`, `Sentiment_Score__c`, `KAM_Feedback_Action__c`
**Integration Pattern:** ⚠️ TBD — Option A: Tableau CRM connector direct to Salesforce; Option B: Middleware (MuleSoft or custom ETL) polling Tableau → REST API push to Salesforce; Option C: Tableau webhook → Salesforce Experience Cloud or API endpoint

---

### Steps

1. **Survey Distribution**
   - Survey sent to VIP via email or in-app (source platform TBD — could be Tableau-adjacent survey tool, Qualtrics, or SurveyMonkey integrated with Tableau as the data aggregation layer)
   - Survey payload includes: VIP external ID, KAM ID, survey date, question set version
   - LOE: **MEDIUM** (survey tooling selection adds scope uncertainty)
   - ⚠️ Survey tool selection is unconfirmed. If Qualtrics or similar is used as the collection tool with Tableau as the analytics layer, the integration path to Salesforce changes. This must be confirmed before architecture is finalized.

2. **Response Collection and Landing in Tableau**
   - VIP submits responses: overall satisfaction (1–5 star rating ⚠️ format TBD — see note), NPS-style recommendation likelihood, open-text comment, and up to 3 topic ratings (e.g., Responsiveness, Offers Quality, Event Experience)
   - Responses land in Tableau data source (either native if Tableau is the collection tool, or via connector from 3rd-party survey tool)
   - Tableau workbook/view updated; underlying data source reflects new row per response
   - LOE: **LOW** (data already in Tableau; no Salesforce involvement at this stage)
   - ⚠️ Star rating fields: business stakeholders have flagged that star ratings may be removed pending survey format confirmation. Build `Survey_Response__c` object with star rating fields nullable and behind a feature flag to allow toggle without schema change.

3. **Tableau → Salesforce Integration**
   - ⚠️ Integration method is the highest open scoping question for this flow. Three options:
   - **Option A — Tableau CRM / Einstein Analytics Connector (LOW additional LOE if org already licensed):** Native connector syncs Tableau dataset to Salesforce CRM Analytics dataset; Apex reads dataset via SOQL-equivalent. Latency: up to 24h depending on sync schedule. Best if Tableau CRM is already in the Salesforce org license.
   - **Option B — Middleware (MuleSoft or custom ETL) (HIGH additional LOE):** Polling job queries Tableau REST API for new/updated survey rows → transforms to Salesforce object schema → REST API upsert to `Survey_Response__c`. Latency: configurable (1h–24h). Most flexible but highest build cost.
   - **Option C — Tableau Webhook / Tableau Prep output → Salesforce API (MEDIUM additional LOE):** Tableau Prep flow exports processed CSV to SFTP or S3 → Salesforce scheduled Apex imports file. Lower latency than Option A but requires file landing zone infrastructure.
   - Recommended: evaluate Option A first given Salesforce org licensing; escalate to Option B only if licensing or latency requirements demand it.
   - LOE: **HIGH to VERY HIGH** depending on method selected
   - ⚠️ Andy Geissler (Director VIP Technology) to confirm Tableau CRM licensing status in target Salesforce org.

4. **Sentiment Scoring**
   - On `Survey_Response__c` insert, Apex trigger fires
   - Composite sentiment score calculated: weighted average of star ratings (if present), NPS bucket (Detractor 0–6, Passive 7–8, Promoter 9–10), and Einstein Natural Language Processing analysis of open-text comment (Einstein Language API or Einstein for Service sentiment classification)
   - `Sentiment_Score__c` written to `Survey_Response__c` with fields: `Composite_Score__c` (0–100), `NPS_Category__c`, `Text_Sentiment__c` (Positive/Neutral/Negative/Mixed), `Confidence__c`
   - If star ratings are removed from survey format, composite score recalculated from NPS + text sentiment only — formula stored in Custom Metadata to allow no-code adjustment
   - LOE: **HIGH**
   - ⚠️ Einstein Language API licensing and text sentiment accuracy on gambling/VIP domain language needs validation. Consider fine-tuning or custom keyword library for domain-specific terms (e.g., "comp," "limit," "reload" carry sentiment context specific to VIP program).

5. **VIP Record Update**
   - `Feedback_Pulse__c` custom object upserted on `VIP_Account__c` lookup (one active pulse record per VIP; historical records retained)
   - Fields updated: `Latest_Survey_Date__c`, `Current_Composite_Score__c`, `Score_Trend__c` (Improving / Stable / Declining — compared to prior 2 responses), `NPS_Category__c`, `Open_Text_Excerpt__c` (first 500 chars for console display), `Response_Count__c`
   - LOE: **MEDIUM**

6. **KAM Alert — Negative Feedback Detection**
   - Salesforce Flow (Record-Triggered on `Feedback_Pulse__c` update) evaluates: `Composite_Score__c` < 40 OR `NPS_Category__c` = Detractor OR `Text_Sentiment__c` = Negative
   - If condition met: `KAM_Alert_Required__c` = true; Salesforce notification sent to assigned KAM; `Awareness_Alert__c` record created with `Alert_Type__c` = Negative Feedback
   - Score trend: if `Score_Trend__c` = Declining for 2+ consecutive surveys, escalation notification sent to KAM Manager regardless of absolute score value
   - LOE: **MEDIUM**

7. **KAM Review and Response**
   - VIP Feedback Pulse card in console shows: latest score, trend sparkline, NPS category, sentiment badge, open-text excerpt, survey date, KAM response status
   - KAM actions: Log Outreach (linked `Task__c` created), Leave Internal Note, Mark Reviewed
   - `Reviewed_Status__c` field on `Feedback_Pulse__c`: Unreviewed → Reviewed by KAM; `Reviewed_DateTime__c` and `Reviewed_By__c` set
   - LOE: **LOW**

8. **Historical Trend and Reporting**
   - `Survey_Response__c` records retained indefinitely (soft-delete only)
   - `Feedback_Pulse__c` trend calculation batch runs weekly, updating `Score_Trend__c` and `Rolling_Average_Score__c` (12-month) on all active VIP records
   - Tableau (or CRM Analytics) dashboard for KAM Manager/Director: response rate by segment, NPS distribution, score trends by KAM book, alert response rate
   - LOE: **MEDIUM**

---

### Error Handling

- Tableau integration job fails (Options B/C) → `Integration_Run_Log__c` record created with failure state; retry next scheduled window; alert to integration admin if 3 consecutive failures; survey data not lost as it remains in Tableau
- Einstein Language API unavailable → Sentiment scored as Neutral with `Sentiment_Source__c` = Fallback; Composite Score calculated from NPS + star rating only; KAM sees "Text sentiment temporarily unavailable" label
- Survey response arrives with unmatched VIP external ID → Record quarantined in `Survey_Response_Queue__c` staging object; admin notified for manual ID reconciliation; not discarded
- Duplicate response for same VIP + survey date → Upsert on composite external ID (`VIP_ID__c` + `Survey_Date__c` + `Question_Set_Version__c`); latest submission wins

---

### LOE Summary

| Component | Complexity | Notes |
|---|---|---|
| Survey tooling + distribution (selection TBD) | MEDIUM | Scope uncertainty from unconfirmed tool selection |
| Tableau → Salesforce integration | HIGH to VERY HIGH | Method TBD; highest risk item in this flow; must be resolved in scoping |
| Sentiment scoring (NPS + Einstein NLP) | HIGH | Einstein Language API licensing + domain fine-tuning needed |
| VIP record update + trend calculation | MEDIUM | Standard Apex + batch; trend logic is straightforward |
| KAM alert on negative feedback (Flow) | MEDIUM | Record-triggered Flow; alert creation reuses Awareness Alert object |
| Console Feedback Pulse card (LWC) | MEDIUM | Scorecard + sparkline trend; depends on data model finalization |
| KAM review actions + status tracking | LOW | Standard action buttons + record update |
| Reporting / historical trend batch | MEDIUM | Batch job + CRM Analytics or Tableau reporting layer |

---

## Version 2 — Stakeholder (Business + SA Review)

**What triggers this:** A VIP completes a satisfaction survey. Their responses — scores, likelihood to recommend, and any comments they leave — flow into the system, get analyzed for sentiment, and appear as an updated Feedback Pulse card on the KAM's console. If the response is negative, the KAM is alerted immediately.

**What the KAM sees:** A Feedback Pulse card for each VIP showing their latest satisfaction score, whether the trend is improving or declining, their NPS category (Promoter, Passive, or Detractor), a snippet of their open-text comments, and when the survey was submitted. If the score is low or trending down, the card is flagged and an alert appears in the Awareness Center.

---

### Swimlane Flow

**DATA LAYER**
Survey tool collects VIP response → data lands in Tableau → Salesforce integration job picks up new responses → stored per VIP

**SALESFORCE / AGENTFORCE**
Sentiment scored automatically → VIP feedback record updated → trend calculated → if negative: alert created and KAM notified

**KAM**
Alert or routine review → opens Feedback Pulse card → reads score, trend, and comment → logs outreach or internal note → marks reviewed

---

### ASCII Flow Diagram

```
[VIP completes survey — satisfaction score + NPS + open comment]
                    ↓
[Response collected in survey tool → lands in Tableau]
                    ↓
[⚠️ Integration method TBD]
[Tableau → Salesforce (direct connector / middleware / file export)]
                    ↓
[Salesforce receives response → scores sentiment automatically]
[NPS category + text analysis + star ratings (if kept)]
                    ↓
     [Score below threshold or trending negative?]
     ↓ YES                              ↓ NO
[Alert created →              [Feedback Pulse card
 KAM notified via              updated silently —
 Awareness Center]             visible on next review]
          ↓
[KAM opens Feedback Pulse card]
[Sees: score / trend sparkline / NPS / comment excerpt]
          ↓
[Log Outreach / Leave Note / Mark Reviewed]
          ↓
[Reviewed status logged with timestamp]
          ↓
[Trend batch runs weekly — updates rolling average for reporting]
```

---

### What Could Go Wrong

- The connection between Tableau and Salesforce is the biggest unknown in this flow. Until the integration method is confirmed, there is no reliable estimate for how quickly survey data appears in the console — it could range from near-real-time to 24+ hours depending on the approach chosen.
- If a VIP's survey response can't be matched to their Salesforce record (because IDs don't align between systems), that response sits in a holding area and requires manual reconciliation. This is especially likely early after launch when ID mapping hasn't been fully validated.
- Open-text sentiment analysis works well in general language but gambling-specific terms can be misread. A comment like "my limits are too low" might read as neutral or positive to a general AI model but carries a very specific meaning in this context. Domain tuning or a keyword library is needed to handle this accurately.

---

### Business Value

- KAMs stop finding out a VIP is unhappy during an awkward call — they know before they pick up the phone, giving them the context to have a better conversation.
- Declining trend detection catches slow-burn dissatisfaction before it becomes churn. A VIP who rates 3 stars three surveys in a row is at risk even if 3 stars doesn't look like a crisis on its own.
- Survey data becomes part of the KAM's VIP record permanently, giving new KAMs (during book transitions) instant history on relationship health rather than starting cold.

---
---
# Flow 4: VIP Onboarding Automation

## Version 1 — Technical (Solution Architects / LOE Scoping)

**Trigger:** Platform threshold event (automated candidate identification) OR manual nomination submitted by KAM/Manager via console UI
**Primary Salesforce Objects:** `VIP_Candidate__c`, `VIP_Account__c`, `Readiness_Score__c`, `Onboarding_Milestone__c`, `KAM_Assignment__c`, `Task__c`, `Playbook__c`
**Integration Pattern:** Platform Event (automated identification from Sportsbook/Casino threshold), Salesforce Flow (SLA timer + milestone tracking), Apex (Readiness Score calculation, assignment logic), Scheduled Batch (30/60/90d milestone checks)

---

### Steps

1. **Candidate Identification**
   - **Automated path:** Sportsbook or Casino platform publishes `VIP_Candidate_Event__e` when a player crosses configurable thresholds (e.g., 90-day GGR > $X, deposit frequency > Y, active days > Z). Platform Event consumed by Salesforce; `VIP_Candidate__c` record created with `Identification_Source__c` = System-Automated.
   - **Manual nomination path:** KAM or KAM Manager submits nomination via console UI form; `VIP_Candidate__c` record created with `Identification_Source__c` = Manual-Nomination; `Nominated_By__c` = submitting user.
   - **Racing path:** Racing candidates are frequently identified at in-person events (e.g., at-track promotions, ADW high-value player events). KAM logs retroactive nomination; `Event_Context__c` field captures event name and date; `Identification_Source__c` = In-Person-Event.
   - All paths converge on `VIP_Candidate__c` with `Status__c` = Pending Review.
   - LOE: **MEDIUM**
   - ⚠️ Automated threshold values for candidate identification — are these the same thresholds used by the Sportsbook/Casino platforms today, or are they net-new logic built in Salesforce? If the platform already flags candidates, a simpler inbound event is sufficient. If Salesforce needs to run the detection, additional data pipeline work is required.

2. **Readiness Score Calculation**
   - On `VIP_Candidate__c` creation, Apex calculates `Readiness_Score__c` (0–100) from: GGR over 90 days, deposit frequency, product breadth (number of products used), account age, engagement consistency score, absence of RG flags
   - Segment-specific scoring models: Sportsbook weights wagering GGR and recency; Casino weights session value and visit frequency; DFS weights contest entry rate relative to prize pool; Racing applies a lower confidence floor due to data sparsity across TVG + FanDuel Racing
   - Score written to `Readiness_Score__c` lookup object with full breakdown by dimension (for transparency in Manager review)
   - Readiness Score threshold for auto-approval to onboarding: ≥ 75; score 50–74 routes to manager review; score < 50 → hold with `Status__c` = Below Threshold
   - LOE: **HIGH**
   - ⚠️ Readiness Score model: is this a rules-based calculation or an Einstein-powered model? Rules-based is lower LOE but less adaptive. Einstein model would share training infrastructure with NBA flow but requires labeled historical data on successful vs. unsuccessful VIP conversions.

3. **KAM Assignment Logic**
   - Apex assignment logic evaluates: segment match (Sportsbook candidate → Sportsbook KAM pool), KAM current book size vs. capacity cap (stored in `KAM_Capacity__c`), KAM current active onboarding count (≤ 3 simultaneous onboardings per KAM as default cap), geographic/timezone alignment (if applicable), manager override capability
   - Assignment written to `KAM_Assignment__c` with `Assigned_DateTime__c`
   - If no KAM available within capacity: candidate placed in `Assignment_Queue__c`; KAM Manager notified; `Status__c` = Awaiting Assignment
   - Racing: assignment must account for KAM familiarity with TVG vs. FanDuel Racing platform — `KAM_Racing_Platform_Expertise__c` field on KAM user record drives preference logic
   - LOE: **HIGH**
   - ⚠️ Capacity caps are placeholder values. Avery (KAM Manager) to confirm max concurrent onboardings per KAM and book size caps per segment.

4. **SLA Timer Start and First Contact Task Auto-Creation**
   - On `KAM_Assignment__c` insert, Salesforce Flow starts SLA timer: `SLA_First_Contact_Due__c` = Assignment DateTime + segment-specific window (Sportsbook: 48h, Casino: 48h, DFS: 72h, Racing: 7 days — Racing window longer due to in-person event context)
   - `Task__c` auto-created: Subject = "VIP Onboarding — First Contact: [VIP Name]", Due Date = `SLA_First_Contact_Due__c`, assigned to KAM, with pre-populated activity template by segment
   - Casino first contact task includes RG-aware framing advisory in task description: "Reminder: first contact framing should follow Casino VIP RG guidelines — welcome-focused, no bonus-first language."
   - LOE: **MEDIUM**
   - ⚠️ Casino first contact RG framing — is this an advisory note only, or does it require compliance sign-off before the task is released to the KAM? Confirm with Compliance team.

5. **Milestone Tracking — 30 / 60 / 90 Day**
   - Scheduled Apex batch (daily) evaluates all active onboarding records for milestone windows
   - `Onboarding_Milestone__c` records created at: Day 30 (check: first contact completed? Engagement metric baseline established?), Day 60 (check: second contact completed? Product adoption tracked?), Day 90 (graduation criteria check — see Step 6)
   - Each milestone creates a KAM task with completion checklist fields; KAM marks milestone complete via console UI
   - Overdue milestones (> 7 days past due): `Milestone_Status__c` = Overdue; KAM Manager notified via Salesforce notification
   - LOE: **MEDIUM**

6. **Graduation Criteria Check**
   - At Day 90 milestone, Apex runs graduation criteria evaluation: GGR in onboarding period meets segment minimum, at least 2 contacts logged, engagement score above threshold, no open critical RG flags, RG classification clean
   - Pass: `VIP_Candidate__c` `Status__c` → Graduation Approved; promotion to `VIP_Account__c` active book triggered; `Playbook__c` assignment based on segment and tier
   - Fail: `Status__c` → Extended Review; 30-day extension created; KAM Manager notified; extension limit = 2 (maximum 150-day onboarding); after 2 extensions → Archive with reason
   - LOE: **HIGH**

7. **Active Playbook Promotion**
   - On graduation approval: `VIP_Account__c` `Status__c` → Active; `Segment__c` confirmed; `KAM_Assigned__c` confirmed; `Active_Since__c` = graduation date
   - Playbook record assigned: segment-specific Playbook template (Sportsbook Standard, Casino Standard, DFS Lite, Racing Standard) with pre-populated 12-month engagement cadence targets
   - NBA recommendation engine (Flow 1) activated for this VIP — first NBA scoring run queued within 24h
   - Awareness Alert monitoring (Flow 2) activated for this VIP
   - LOE: **MEDIUM**

8. **Manager Oversight Notifications Throughout**
   - Key notification events pushed to KAM Manager (Avery) and KAM Director (Pat) throughout:
     - Candidate identified (automated) — summary digest, not per-record
     - High Readiness Score candidates (≥ 85) — individual notification
     - SLA breach on first contact
     - Overdue milestones
     - Graduation approval or extended review decision
     - Graduation failures (archive)
   - All notifications configurable via `Notification_Preference__c` custom metadata — avoids hardcoded notification logic
   - LOE: **LOW**

---

### Error Handling

- Automated candidate identification Platform Event arrives for player with existing `VIP_Account__c` (already active VIP) → deduplication check on account external ID; event discarded; `Duplicate_Candidate_Log__c` record created for audit
- Readiness Score calculation fails (missing segment data — Racing) → Score calculated with available data; `Score_Confidence__c` = Low; candidate routed to Manager Review regardless of score value
- KAM assignment no capacity available → Candidate queued; if queue age exceeds 72h with no assignment, KAM Director notified; candidate not lost
- Graduation criteria check fails due to missing milestone completion data (KAM did not log milestones) → Graduation placed on hold; `Graduation_Block_Reason__c` = Incomplete Milestone Data; KAM and Manager notified; 7-day resolution window before auto-escalation

---

### LOE Summary

| Component | Complexity | Notes |
|---|---|---|
| Automated candidate identification (Platform Event) | HIGH | Platform-side threshold publishing must be confirmed (same dependency as Flow 2 Big Swing) |
| Manual + retroactive nomination UI | MEDIUM | Console form; Racing retroactive event logging adds edge case handling |
| Readiness Score calculation (Apex) | HIGH | Segment-specific scoring models; Racing low-confidence handling; rules vs. Einstein TBD |
| KAM assignment logic (capacity + segment + expertise) | HIGH | Multi-variable assignment; capacity caps require business input |
| SLA timer + first contact task auto-creation (Flow) | MEDIUM | Record-triggered Flow; segment-specific SLA windows; Casino RG framing advisory |
| Milestone tracking batch (30/60/90d) | MEDIUM | Scheduled Apex; checklist fields on milestone records |
| Graduation criteria check + Playbook promotion | HIGH | Multi-condition evaluation; Playbook template library must be built |
| Manager oversight notifications | LOW | Custom metadata-driven notification config; reuses standard Salesforce notification framework |

---

## Version 2 — Stakeholder (Business + SA Review)

**What triggers this:** Either the Sportsbook or Casino platform flags a player who has crossed the threshold to be considered for VIP status, or a KAM manually nominates a player they've identified. For Racing, the trigger is often an in-person event — a KAM logs the nomination after meeting the player at a track.

**What the KAM sees:** A new onboarding record in their console with the candidate's Readiness Score, a first-contact task already created with a due date, a recommended first-contact approach by segment, and a milestone timeline showing the 30/60/90-day checkpoints ahead.

---

### Swimlane Flow

**DATA LAYER**
Platform threshold alert → candidate flagged → Readiness Score calculated from activity data across all platforms

**SALESFORCE / AGENTFORCE**
Assignment logic finds best-fit KAM → SLA timer starts → task auto-created → milestones tracked → graduation criteria evaluated → Playbook assigned

**KAM**
Receives first-contact task → completes outreach → logs milestones → system evaluates graduation → active book + Playbook activated

---

### ASCII Flow Diagram

```
[Platform threshold alert] OR [KAM manual nomination] OR [Racing: in-person event log]
                    ↓
[VIP Candidate record created → Readiness Score calculated]
                    ↓
     [Readiness Score ≥ 75?]
     ↓ YES                        ↓ NO (50–74)           ↓ Below 50
[Auto-approved →         [Manager Review            [Held — below
 proceed to               required]                  threshold]
 assignment]
          ↓
[KAM Assignment: segment match + capacity check]
          ↓
     [KAM available within capacity?]
     ↓ YES                              ↓ NO
[Assign KAM →                  [Queue candidate →
 SLA timer starts]               Manager notified]
          ↓
[First contact Task auto-created → due within SLA window]
[Casino: RG-aware framing advisory included]
          ↓
[KAM makes first contact → logs activity]
          ↓
[30-day milestone check] → [60-day milestone check] → [90-day graduation check]
          ↓
     [Graduation criteria met?]
     ↓ YES                              ↓ NO
[Promoted to Active VIP book       [30-day extension (max 2)
 Playbook assigned                  Manager notified
 NBA + Alerts activated]            2nd failure → Archive]
```

---

### What Could Go Wrong

- If a KAM doesn't log their milestone activities in the system, the graduation check at Day 90 will fail even if the KAM has done the work. Milestone logging compliance is a people-and-process problem as much as a technical one — it needs to be reinforced during rollout training.
- Casino onboarding requires a specific first-contact framing that avoids leading with bonuses. If this is only an advisory note and not an enforced gate, KAMs may skip it — particularly during high-volume periods. Consider whether this needs to be a compliance checkpoint, not just a reminder.
- Racing onboarding frequently starts with an in-person event that happens before the KAM has time to log anything. Retroactive logging after the fact means the system's SLA timer and milestone dates may not reflect reality. The onboarding record needs to support a "start date override" for Racing to prevent false SLA breach flags.

---

### Business Value

- Onboarding SLAs are enforced automatically — the system creates the task, sets the due date, and escalates if it's missed. No candidate slips through because a KAM was busy.
- Readiness Scoring gives KAMs and managers a shared, objective view of why a candidate was selected, replacing gut-feel nominations with data-backed decisions that can be reviewed and improved over time.
- The 30/60/90 milestone structure creates a consistent onboarding experience across all KAMs and segments — new VIPs receive the same quality of welcome regardless of which KAM they're assigned to or how experienced that KAM is.

---
---
# Flow 5: Data Integration Architecture

## Version 1 — Technical (Solution Architects / LOE Scoping)

**Trigger:** Continuous / scheduled — various cadences per source system
**Primary Salesforce Objects:** `VIP_Account__c`, `Wagering_Event__c`, `Casino_Session__c`, `DFS_Contest_Entry__c`, `TVG_Activity__c`, `FanDuel_Racing_Activity__c`, `Payment_Transaction__c`, `GGR_Snapshot__c`, `Data_Integration_Log__c`
**Integration Pattern:** REST API (Sportsbook/Casino real-time events), Scheduled Batch (GGR snapshots), Platform Event (threshold triggers), ⚠️ TBD (TVG, FanDuel Racing, Tableau)

---

### Steps

1. **Sportsbook Platform → Salesforce**
   - **Wagering Events (near-real-time):** Sportsbook platform posts `Wagering_Event__c` records via outbound REST API call to Salesforce Connected App endpoint (or Salesforce MuleSoft inbound) on bet settlement. Payload: account external ID, wager amount, outcome, market type, timestamp.
   - **GGR Snapshots (daily batch):** Scheduled job on Sportsbook side pushes daily GGR aggregate per account to `GGR_Snapshot__c`. Salesforce receives via REST API upsert. Alternatively, Salesforce pulls via callout if push is not supported.
   - **Account data (weekly sync):** Sportsbook player profile fields (account status, limit settings, product eligibility) synced to `VIP_Account__c` external ID match.
   - Idempotency: external ID on `Wagering_Event__c` = Sportsbook transaction ID; upsert prevents duplicates on retry.
   - LOE: **HIGH**
   - ⚠️ Does the Sportsbook platform have an existing outbound event/webhook capability, or does Salesforce need to poll? Push architecture is strongly preferred for near-real-time use cases. Confirm with Andy Geissler.
   - ⚠️ Volume estimate needed: how many wagering events per day across the VIP book? This drives governor limit risk and batch sizing decisions.

2. **Casino CRM → Salesforce**
   - **Session data (near-real-time or hourly batch):** Casino CRM pushes session records to Salesforce via REST API or MuleSoft. Payload per session: account ID, session start/end, game categories played, net result, deposit/withdrawal during session.
   - **GGR and loss streak data (daily batch):** Casino CRM pushes daily aggregate GGR and rolling loss metrics per account. These feed the Losing Streak detection in Flow 2.
   - **RG Classification sync:** ⚠️ Critical dependency. Casino CRM is assumed to be the source of truth for RG classification status. Sync cadence must be near-real-time or at minimum hourly — stale RG data creates compliance risk. Integration method to be confirmed.
   - LOE: **HIGH**
   - ⚠️ Casino CRM integration method and RG sync cadence are blocking dependencies for Flow 2 (Awareness Center Losing Streak + compliance gate). Must be resolved in Phase 1 scoping.

3. **DFS Platform → Salesforce**
   - **Contest entries and results (event-triggered):** DFS platform posts contest entry and result records to Salesforce on contest completion. Payload: account ID, contest ID, entry fee, prize won, net result, contest type (GPP/H2H/50-50).
   - **Account data (daily sync):** DFS player profile and account status synced daily.
   - DFS is the smallest segment and lowest data volume; REST API push on contest completion is feasible without batch infrastructure.
   - LOE: **MEDIUM** (lower volume, simpler event structure than Sportsbook)

4. **TVG → Salesforce**
   - ⚠️ Integration pattern is a critical open scoping item. TVG (tvg.com) is a legacy platform with an independent tech stack. Options:
   - **Option A — TVG REST API (MEDIUM LOE if API exists):** Query TVG's player API for account activity, wagering history, GGR. Requires TVG API documentation review and authentication setup. Scheduled Apex callout from Salesforce on hourly/daily cadence.
   - **Option B — TVG data export → file landing zone (HIGH LOE):** TVG exports CSV/flat file to SFTP or S3 daily; Salesforce scheduled Apex imports and maps to `TVG_Activity__c`. Higher latency (T+1 minimum) but no API dependency.
   - **Option C — Middleware / MuleSoft (VERY HIGH LOE):** Full ETL pipeline from TVG → MuleSoft → Salesforce. Most flexible; highest build cost. Justified only if TVG has no API and file export cadence is insufficient.
   - TVG data written to `TVG_Activity__c` custom object linked to `VIP_Account__c` via TVG account external ID → Salesforce VIP record match (⚠️ ID mapping logic required — TVG player IDs may not match FanDuel VIP IDs).
   - LOE: **HIGH to VERY HIGH**
   - ⚠️ TVG player ID ↔ FanDuel VIP ID mapping: is there an existing identity resolution layer? If not, this is a significant data engineering effort. Confirm with Andy Geissler.
   - ⚠️ TVG API availability: does TVG expose a player activity API? If not, Option B (file export) is the fallback. This must be confirmed before LOE can be finalized.

5. **FanDuel Racing → Salesforce**
   - ⚠️ Integration pattern also TBD. FanDuel Racing (racing.fanduel.com) is a separate platform from TVG, though both serve the Racing segment.
   - Assumed newer platform than TVG; more likely to support REST API outbound events. Same options as TVG apply (Option A preferred).
   - Data model for `FanDuel_Racing_Activity__c` should mirror `TVG_Activity__c` to allow unified Racing view in the console — segment logic merges both into a single Racing activity timeline.
   - LOE: **HIGH**
   - ⚠️ Same ID mapping question as TVG: does FanDuel Racing use FanDuel VIP account IDs natively, or does it have its own player ID system? If native FanDuel IDs, integration simplifies significantly.
   - ⚠️ Data sparsity: even after integration, Racing data may be sparse for many VIPs (low wagering frequency vs. Sportsbook/Casino). The unified data layer must handle null/sparse gracefully — NBA and scoring models must apply Racing confidence dampeners (see Flow 1 Step 3).

6. **Tableau → Salesforce (Survey Data)**
   - Per Flow 3 Step 3 — integration method TBD (Tableau CRM connector / Middleware / File export).
   - In the context of the overall architecture: Tableau is a one-directional data source (survey data flows Tableau → Salesforce only; Salesforce does not write back to Tableau).
   - Tableau may also serve as a reporting consumer of Salesforce data (KAM performance dashboards, VIP analytics) — this is a separate out-of-scope data flow but worth noting for architecture diagram.
   - LOE: **HIGH** (same as Flow 3)
   - ⚠️ Tableau CRM licensing in the Salesforce org — confirm with Andy Geissler before committing to Option A.

7. **Unified Data Layer in Salesforce**
   - All source data lands in segment-specific activity objects (`Wagering_Event__c`, `Casino_Session__c`, `DFS_Contest_Entry__c`, `TVG_Activity__c`, `FanDuel_Racing_Activity__c`) linked to `VIP_Account__c` as the master record.
   - `GGR_Snapshot__c` aggregates cross-platform GGR per VIP per day — calculated by daily batch from activity objects and/or received directly from source platforms.
   - `Data_Integration_Log__c` tracks every integration run: source, run datetime, records processed, records failed, failure reason. Used for monitoring, alerting, and SLA tracking.
   - Data retention: raw activity records retained per compliance/legal requirement (⚠️ confirm retention period with legal); `GGR_Snapshot__c` aggregates retained indefinitely.
   - LOE: **MEDIUM** (data model design) + **HIGH** (data quality and ID resolution across all sources)

8. **Agentforce Einstein — Sitting on Unified Data Layer**
   - Einstein Prediction Service and Einstein Discovery consume `VIP_Account__c` + all activity objects as training and scoring inputs.
   - Einstein operates on the Salesforce data layer only — no direct connection to source platforms. All AI/ML processing is downstream of the integration layer.
   - Model refresh cadence (weekly batch) requires activity data to be current as of previous day; same-day data gaps in TVG/Racing (if T+1 batch) mean Racing model scores may lag by 24–48h relative to Sportsbook/Casino.
   - LOE: **HIGH** (full Einstein configuration across segments) — shared with Flow 1

9. **Integration Monitoring and Alerting**
   - Scheduled Apex (every 30 min) checks `Data_Integration_Log__c` for runs that are overdue or have failed
   - Alert conditions: source has not posted data within expected window (configurable per source in `Integration_Config__mdt`); failure rate > 5% on last run; 3 consecutive failures
   - On alert: Salesforce notification to integration admin user group; optional email/Slack via outbound webhook; `Integration_Alert__c` record created
   - Console data freshness indicator: LWC reads `Last_Successful_Run__c` per source from `Integration_Config__mdt` and displays source status badges (green = fresh, amber = approaching stale, red = stale/failed)
   - LOE: **MEDIUM**

---

### Data Freshness / Latency Table

| Source | Data Type | Expected Refresh Cadence | Latency to Console | Notes |
|---|---|---|---|---|
| Sportsbook Platform | Wagering Events | Near-real-time (on bet settlement) | < 5 min | Requires push architecture from Sportsbook |
| Sportsbook Platform | GGR Snapshot | Daily (T+1) | ~24h | Aggregate; acceptable for scoring use |
| Casino CRM | Session Data | Hourly batch (target) | ~1h | Near-real-time preferred for Losing Streak detection |
| Casino CRM | GGR + Loss Metrics | Daily (T+1) | ~24h | Feeds Awareness Center batch detection |
| Casino CRM | RG Classification | ⚠️ Target: near-real-time | ⚠️ Must confirm | Stale RG data is a compliance risk — cannot be T+1 |
| DFS Platform | Contest Entries/Results | On contest completion (event-triggered) | < 15 min | Low volume; event-driven acceptable |
| DFS Platform | Account Data | Daily | ~24h | Low priority for latency |
| TVG | Wagering Activity | ⚠️ TBD — API vs. file export | ⚠️ 1h (API) or T+1 (file) | Highest uncertainty; Racing sparsity compounds latency impact |
| FanDuel Racing | Wagering Activity | ⚠️ TBD — API preferred | ⚠️ Target < 1h | Newer platform; API more likely than TVG |
| Tableau | Survey Responses | ⚠️ TBD — connector vs. middleware | ⚠️ 1h to 24h | See Flow 3 for options |

---

### Error Handling

- Source system outage (any platform) → `Data_Integration_Log__c` failure recorded; console shows amber/red freshness badge for that source; Agentforce scoring continues with last-known data; NBA confidence score is not degraded unless data gap exceeds 48h (configurable)
- ID mismatch on upsert (TVG player ID not matching VIP Account external ID) → Record written to `ID_Mismatch_Queue__c`; admin notified for manual resolution; not discarded
- Governor limits exceeded on high-volume Sportsbook event ingestion → Batch size and chunking configuration in `Integration_Config__mdt`; async Apex patterns used throughout; Salesforce Bulk API 2.0 used for high-volume upserts
- Duplicate record on race condition (two concurrent integration runs) → Upsert on external ID (source transaction ID) prevents duplicate data at object level

---

### LOE Summary

| Component | Complexity | Notes |
|---|---|---|
| Sportsbook platform integration (events + GGR) | HIGH | Push vs. pull architecture TBD; volume sizing needed |
| Casino CRM integration (sessions + RG sync) | HIGH | RG sync latency is a compliance-critical blocker |
| DFS platform integration | MEDIUM | Lower volume; event-driven pattern is straightforward |
| TVG integration | VERY HIGH | Legacy platform; API availability unconfirmed; ID mapping required |
| FanDuel Racing integration | HIGH | Newer platform; API likely but unconfirmed; ID mapping TBD |
| Tableau integration | HIGH | Method TBD; see Flow 3 |
| Unified data model (custom objects + relationships) | MEDIUM | Design effort; Racing dual-source normalization adds complexity |
| ID resolution across all sources | HIGH | Cross-platform identity matching is a data engineering effort, not just configuration |
| Integration monitoring + console freshness badges | MEDIUM | Standard pattern; custom metadata config |
| Agentforce Einstein on unified layer | HIGH | Shared with Flow 1; training pipeline + model management |

---

## Version 2 — Stakeholder (Business + SA Review)

**What triggers this:** This is not a single flow with one trigger — it is the always-on infrastructure that keeps the entire console fed with current data. Every card on the KAM's screen, every alert, every AI recommendation, and every VIP profile draws from this layer. Without it, nothing else works.

**What the KAM sees:** Data freshness indicators in the console showing when each source last updated. If a source is delayed or failing, the KAM sees an amber or red status badge so they know to interpret any affected VIP data cautiously.

---

### Swimlane Flow

**SOURCE SYSTEMS**
Sportsbook → wagering events, GGR → Salesforce (near-real-time + daily)
Casino CRM → sessions, loss metrics, RG status → Salesforce (hourly + daily)
DFS platform → contest entries and results → Salesforce (on contest close)
TVG → wagering history → Salesforce (⚠️ method TBD)
FanDuel Racing → wagering history → Salesforce (⚠️ method TBD)
Tableau → survey responses → Salesforce (⚠️ method TBD)

**SALESFORCE UNIFIED DATA LAYER**
All source data lands in platform-specific activity objects → linked to the master VIP record → aggregated into GGR snapshots → monitored for freshness

**AGENTFORCE**
Einstein sits on top of the unified data layer → scores VIPs → generates recommendations → powers alerts → all AI output depends on data quality and freshness below it

---

### ASCII Flow Diagram

```
[Sportsbook Platform]  ──→ REST API (near-real-time) ──→ [Wagering_Event__c]
                             daily batch               →  [GGR_Snapshot__c]

[Casino CRM]           ──→ hourly batch ──────────────→ [Casino_Session__c]
                             daily batch               →  [GGR_Snapshot__c]
                             ⚠️ RG sync cadence TBD   →  [RG_Classification__c]

[DFS Platform]         ──→ event-triggered ───────────→ [DFS_Contest_Entry__c]

[TVG]                  ──→ ⚠️ API or file export TBD ─→ [TVG_Activity__c]

[FanDuel Racing]       ──→ ⚠️ API TBD ────────────────→ [FanDuel_Racing_Activity__c]

[Tableau]              ──→ ⚠️ connector / middleware   → [Survey_Response__c]
                              TBD

All objects link to → [VIP_Account__c] (master record)
                              ↓
             [Integration monitoring — freshness checks every 30 min]
                              ↓
         [Console data freshness badges: green / amber / red per source]
                              ↓
                    [Agentforce Einstein]
                    [scores on unified data]
                              ↓
          [NBA Recommendations] [Awareness Alerts] [Feedback Pulse]
```

---

### What Could Go Wrong

- Racing is the highest data risk in the entire architecture. Two separate platforms (TVG and FanDuel Racing) with unconfirmed integration methods and potentially different player ID systems means a Racing KAM could be looking at an incomplete picture of their VIP's activity for an extended period after launch. This must be treated as a critical scoping item, not a Phase 2 nice-to-have.
- RG classification data flowing from the Casino CRM needs to be current. If this sync runs once a day and a player's status changes in the morning, the console could show the wrong contact permission status for up to 24 hours. For a compliance-sensitive system, that gap is unacceptable — near-real-time RG sync must be confirmed and prioritized.
- Data freshness indicators only help if KAMs know how to read them and what to do when a source shows red. A stale data badge is not a blocker — the console still works — but KAMs need to be trained that AI recommendations and alerts sourced from a stale feed should be treated with lower confidence until the feed recovers.

---

### Business Value

- A unified data layer means a KAM looking at any VIP sees everything in one place — Sportsbook bets, Casino sessions, DFS contests, and Racing activity — rather than logging into four separate systems to piece together a complete picture.
- The integration monitoring layer and freshness badges give KAMs and managers real-time confidence in the data they're acting on. When something breaks upstream, the console tells them — instead of KAMs acting on bad data without knowing it.
- Every piece of AI intelligence in the console — recommendations, alerts, sentiment scores, readiness scores — flows from this integration layer. Investment in getting the integration right, especially for the harder platforms like TVG, directly multiplies the value of every feature built on top of it.

---
