# FanDuel VIP KAM Console — Lucidchart Import Guide
## For Technical Architect — System Process Flows

Each section below contains Mermaid syntax. In Lucidchart:
1. Create a new document
2. Click **Insert → Import → Mermaid** (or use the Mermaid shape library)
3. Paste the code block for each flow
4. Lucidchart converts it to an editable diagram
5. Repeat for each flow — one diagram per page recommended

Color key after import:
- 🟡 Amber nodes = RG decision gates or compliance checks
- 🟠 Orange/light nodes = open scoping questions (⚠ TBD)
- 🔴 Red nodes = blocked paths / error states
- 🔵 Blue = standard flow steps

---

## Flow 1: NBA Recommendation Engine
*How Agentforce generates, scores, and delivers a Next Best Action to the KAM.*

### Stakeholder View
```mermaid
flowchart LR
  subgraph DS["Data Sources"]
    SB["Sportsbook Platform"]
    CS["Casino CRM"]
    DFS["DFS Platform"]
    RC["TVG + FD Racing\n⚠ Sparse Data — Lower Confidence"]
  end
  subgraph AF["Agentforce"]
    COL["Collect Activity & Signals"]
    RG{"RG Flag Active?"}
    SCORE["Score & Rank VIPs"]
    GEN["Generate Recommendation"]
    PRI["Set Priority: P1 / P2 / P3"]
    BLOCK["BLOCKED — Route to RG Pathway"]
  end
  subgraph KAM["KAM Console"]
    ALERT["New NBA Alert Badge"]
    REVIEW["KAM Reviews Card"]
    DEC{"Accept or Dismiss?"}
    ACT["Action Taken in Console"]
    LOG["Outcome Logged"]
    FEED["Feedback Loop — Model Improves"]
  end
  SB & CS & DFS & RC --> COL
  COL --> RG
  RG -->|No| SCORE --> GEN --> PRI --> ALERT
  RG -->|Yes| BLOCK
  ALERT --> REVIEW --> DEC
  DEC -->|Accept| ACT --> LOG --> FEED
  DEC -->|Dismiss + Reason| LOG --> FEED
```

### Technical View
```mermaid
flowchart TD
  T1["TRIGGER\nSportsbook/Casino: Scheduled Batch every 4h\nDFS: Platform Event on contest completion\nRacing: Nightly batch — T+1 minimum"]
  FE["FEATURE ENGINEERING — Apex\nRecency score · Frequency score · GGR delta\nContact gap days · RG flag status\nProduct affinity index · Engagement trend 3/7/30d\nRacing: confidence dampener if data coverage under 70%"]
  ESP["EINSTEIN PREDICTION SERVICE\nREST API callout\nReturns: recommendation_score 0-100\nrecommended_action · confidence_level\nLOE: HIGH"]
  RGCHECK{"RG_Classification__c\n= Flagged?"}
  BLOCK2["Create RG_Referral__c record\nDo NOT create NBA_Recommendation__c\nLOE: MEDIUM"]
  CREATE["Create NBA_Recommendation__c\nvip_id · action_type · score · priority_tier\nexpiry_timestamp · segment\nLOE: LOW"]
  PE["Platform Event: NBA_Notification__e fired\nKAM console NBA badge increments"]
  FB{"KAM Decision"}
  ACCEPT["NBA_Feedback__c created\noutcome = Accepted\naction_taken · response_quality logged"]
  DISMISS["NBA_Feedback__c created\noutcome = Dismissed\ndismiss_reason captured — feeds retraining"]
  RETRAIN["Weekly batch: Feedback → Einstein\nretraining pipeline\nLOE: HIGH — ongoing model ops"]
  ERR["⚠ Einstein timeout / unavailable\nFallback: rule-based score\nAlert ops team via Platform Event\nLOE to build fallback: MEDIUM"]

  T1 --> FE --> ESP
  ESP -->|Success| RGCHECK
  ESP -->|Timeout| ERR
  RGCHECK -->|Yes| BLOCK2
  RGCHECK -->|No| CREATE --> PE --> FB
  FB -->|Accept| ACCEPT --> RETRAIN
  FB -->|Dismiss| DISMISS --> RETRAIN
```

### LOE Summary — Flow 1
| Component | Complexity | Notes |
|-----------|-----------|-------|
| Data ingest from SB/Casino | MEDIUM | Platform Events to be confirmed with FanDuel platform team |
| Data ingest from Racing (TVG + FD Racing) | HIGH | ⚠ Integration pattern TBD — two separate systems |
| Feature engineering (Apex) | MEDIUM | Segment-specific weighting logic needed |
| Einstein Prediction Service | HIGH | Model training data, scoring cadence, retraining loop |
| NBA_Recommendation__c object + logic | MEDIUM | Custom object + Flow automation |
| KAM feedback capture + retraining loop | HIGH | Ongoing model ops commitment |

---

## Flow 2: Awareness Center Alert Flow
*How Big Swings, Payment Alerts, and Losing Streak signals are detected, RG-gated, and surfaced to the KAM.*

### Stakeholder View
```mermaid
flowchart LR
  subgraph PLAT["Platform Events"]
    WE["Large Wager Detected"]
    PAY["Payment Issue Flagged"]
    LS["Sustained Loss Detected"]
  end
  subgraph DET["Detection & Classification"]
    TYPE{"Alert Type?"}
    BS["Big Swing"]
    PA["Payment Alert"]
    LST["Losing Streak"]
    RGGATE{"RG Threshold Crossed?"}
    RGREF["RG Referral Path Only\nContact action blocked"]
    COMP{"Compliance Clearance?"}
    NOCON["Do Not Contact VIP\nRead-only alert"]
  end
  subgraph KAM2["KAM — Awareness Center"]
    SHOW["Alert appears in correct tab"]
    KAMACT{"KAM Decision"}
    RESPOND["Respond Now"]
    MONITOR["Monitor and Snooze"]
    ESCALATE["Escalate to Manager"]
  end
  subgraph RES["Resolution"]
    RESOLVE["Alert Resolved and Logged"]
    AUTOESC["Auto-Escalate if unresolved 48h"]
  end

  WE --> TYPE
  PAY --> TYPE
  LS --> TYPE
  TYPE -->|Big Swing| BS --> SHOW
  TYPE -->|Payment| PA --> COMP
  TYPE -->|Losing Streak| LST --> RGGATE
  RGGATE -->|Yes| RGREF
  RGGATE -->|No| SHOW
  COMP -->|Pending| NOCON
  COMP -->|Cleared| SHOW
  SHOW --> KAMACT
  KAMACT -->|Respond| RESPOND --> RESOLVE
  KAMACT -->|Monitor| MONITOR --> AUTOESC
  KAMACT -->|Escalate| ESCALATE --> RESOLVE
```

### Technical View
```mermaid
flowchart TD
  TRIG["TRIGGER\nSportsbook/Casino: Platform Event near-real-time\nRacing: Scheduled Batch T+1 minimum\nPayment: Payment_Event__c Apex trigger"]
  THRESH["Apex: Threshold Check\nCompare vs VIP_Baseline__c segment-weighted averages\nBig Swing: wager greater than Nx rolling avg\nLosing Streak: cumulative loss over threshold over N days\nPayment: event_type in blocked list\nLOE: MEDIUM"]
  RGCHECK2{"RG_Classification__c\nflag = true?"}
  RGPATH["Create RG_Referral__c\nLock Contact action on Alert_Record__c\nShow KAM approved RG template ONLY\nLOE: HIGH — compliance sign-off required"]
  COMPCHECK{"Payment_Compliance\n_Status = Pending?"}
  NOACT["Alert_Record__c created\ncontact_allowed = false\nKAM sees read-only view\nLOE: LOW"]
  CREATE2["Alert_Record__c created\nalert_type · severity P1/P2/P3\nvip_id · expiry_timestamp\ncontact_allowed = true\nLOE: MEDIUM"]
  NOTIFY["Platform Event: Alert_Created__e\nKAM console tab badge increments"]
  KAMRES{"KAM sets status"}
  RESPONDED["status = Responded\nActivity logged to VIP timeline\nLast_Contact__c updated"]
  SNOOZED["status = Monitoring\nReminder Task created\ncreated_date + snooze_hours"]
  ESC["Escalation_Record__c created\nManager notification via Platform Event\nStructured briefing auto-generated"]
  AUTOBATCH["Scheduled Apex hourly\nIF status not Resolved\nAND created_date older than 48h\nAND severity = P1\nTHEN auto-escalate\nLOE: MEDIUM"]
  CLOSED["Alert_Record__c status = Resolved\nresolution_notes · resolved_by · resolved_date"]

  TRIG --> THRESH --> RGCHECK2
  RGCHECK2 -->|Yes| RGPATH
  RGCHECK2 -->|No| COMPCHECK
  COMPCHECK -->|Yes| NOACT
  COMPCHECK -->|No or NA| CREATE2 --> NOTIFY --> KAMRES
  KAMRES -->|Responded| RESPONDED --> CLOSED
  KAMRES -->|Snooze| SNOOZED --> AUTOBATCH
  KAMRES -->|Escalate| ESC --> CLOSED
  AUTOBATCH -->|Triggered| ESC
```

### LOE Summary — Flow 2
| Component | Complexity | Notes |
|-----------|-----------|-------|
| Platform Event integration SB/Casino | MEDIUM | Confirm event schema with platform team |
| VIP_Baseline__c — baseline calculation | HIGH | Segment-weighted, rolling window logic |
| RG_Classification__c + gating logic | VERY HIGH | ⚠ Compliance team must own threshold rules |
| Alert_Record__c object + UI | MEDIUM | Custom object + Awareness Center UI |
| Auto-escalation Scheduled Apex | MEDIUM | Straightforward batch logic |

---

## Flow 3: VIP Feedback Pulse — Survey Data Flow
*How survey responses travel from Tableau into the Feedback Pulse card, with sentiment scoring.*

### Stakeholder View
```mermaid
flowchart LR
  subgraph SURVEY["Survey"]
    SEND["Survey sent to VIP"]
    VIP_R["VIP responds"]
    TAB["Response lands in Tableau"]
  end
  subgraph INT["Integration — Method TBD"]
    OPT{"Which method?"}
    DIRECT["Option A: Tableau Connector\n24h latency · Lower LOE"]
    MIDDLE["Option B: MuleSoft Middleware\nNear real-time · Higher LOE"]
    SF["Salesforce — Survey_Response__c"]
  end
  subgraph PROC["Processing"]
    SENT["Einstein Sentiment\nPositive / Neutral / Negative"]
    STARS["Star Rating Captured"]
    VIP_UPD["VIP Record Updated\nUnreviewed count +1"]
  end
  subgraph KAM3["KAM Console"]
    BADGE["Feedback Pulse Badge Updates"]
    OPEN["KAM Opens Card"]
    DEC3{"Action?"}
    REV["Mark Reviewed"]
    RESP["Respond to VIP"]
    NEG{"Negative Sentiment?"}
    NBA_Q["Surface in NBA Queue as P1"]
    DONE["Logged and Closed"]
  end

  SEND --> VIP_R --> TAB --> OPT
  OPT -->|Connector| DIRECT --> SF
  OPT -->|Middleware| MIDDLE --> SF
  SF --> SENT & STARS --> VIP_UPD --> BADGE
  BADGE --> OPEN --> DEC3
  DEC3 -->|Review| REV --> NEG
  DEC3 -->|Respond| RESP --> NEG
  NEG -->|Yes| NBA_Q --> DONE
  NEG -->|No| DONE
```

### Technical View
```mermaid
flowchart TD
  SRC["SURVEY DISPATCH\nSource TBD: Tableau native survey OR 3rd party tool via Tableau\n⚠ Confirm with FanDuel data team"]
  TAB2["Tableau Dataset\nResponse data structured and available"]
  INT2{"Integration\nMethod — TBD"}
  CONN["Option A: Salesforce Tableau CRM Connector\nScheduled sync approximately 24h latency\nLOE: LOW — connector config only"]
  MUL["Option B: MuleSoft or custom middleware\nNear real-time REST API push\nLOE: HIGH — MuleSoft license + config required"]
  CREATE3["Survey_Response__c created\nvip_id · response_text · star_rating\nsurvey_type · response_date · source_system"]
  EINST["Einstein NLP: Sentiment Analysis\nEinstein Language API callout\nReturns: sentiment_label · sentiment_score 0 to 1\nLOE: MEDIUM"]
  ERR2["⚠ Sentiment service unavailable\nsentiment_label = Unscored\nSurface for manual KAM review\nAlert logged"]
  WRITE["Write to Survey_Response__c\nsentiment_label · sentiment_score\nreviewed = false · star_rating"]
  COUNT["Increment VIP_Account__c\nfeedback_unreviewed_count field\nLOE: LOW"]
  PE2["Platform Event: Feedback_New__e\nKAM console Feedback Pulse badge updates"]
  KAMREV{"KAM marks reviewed"}
  STAMP["Stamp reviewed_by · reviewed_date\nDecrement feedback_unreviewed_count\nLOE: LOW"]
  NEGNBA{"sentiment_label = Negative?"}
  CRTNBA["Create NBA_Recommendation__c\naction_type = Feedback Response\npriority_tier = P1"]
  DONE2["Record closed — audit trail complete"]

  SRC --> TAB2 --> INT2
  INT2 -->|Connector| CONN --> CREATE3
  INT2 -->|Middleware| MUL --> CREATE3
  CREATE3 --> EINST
  EINST -->|Success| WRITE --> COUNT --> PE2 --> KAMREV
  EINST -->|Fail| ERR2 --> PE2
  KAMREV -->|Yes| STAMP --> NEGNBA
  NEGNBA -->|Yes| CRTNBA --> DONE2
  NEGNBA -->|No| DONE2
```

### LOE Summary — Flow 3
| Component | Complexity | Notes |
|-----------|-----------|-------|
| Tableau integration — Option A (Connector) | LOW | Simplest path, 24h latency acceptable? |
| Tableau integration — Option B (MuleSoft) | HIGH | ⚠ Requires MuleSoft license — confirm with arch |
| Survey_Response__c custom object | LOW | Simple custom object |
| Einstein Sentiment scoring | MEDIUM | Standard Einstein Language API |
| Feedback Pulse UI + badge logic | MEDIUM | Already prototyped in console |

---

## Flow 4: VIP Onboarding Automation
*From candidate identification through first contact to active book graduation.*

### Stakeholder View
```mermaid
flowchart LR
  subgraph ID["Identification"]
    THRESH2["Platform threshold alert"]
    MAN["Manual nomination"]
    CAND["Candidate Queue"]
  end
  subgraph ASSIGN["Assignment"]
    SCORE2["Agentforce Readiness Score"]
    KAPFIT["KAM capacity + segment match"]
    MGR["Manager approves"]
    NOTIFY2["KAM notified"]
  end
  subgraph FC["First Contact"]
    SLA["48h SLA timer starts"]
    TASK["Task auto-created with talking points"]
    CONTACT["KAM makes first contact"]
    MILE["Milestone logged"]
  end
  subgraph MON["Monitoring — 30/60/90 days"]
    D30["30d engagement check"]
    D60["60d engagement check"]
    D90["90d engagement check"]
    CRIT{"Meeting graduation criteria?"}
    REENGAGE["Re-engage NBA created"]
    GRADAPP["Manager graduation approval"]
    GRAD["Graduate to Active Playbook"]
  end
  subgraph MGRL["Manager Layer"]
    BREACH["SLA breach alert"]
    DIGEST["Weekly health digest"]
  end

  THRESH2 & MAN --> CAND --> SCORE2 --> KAPFIT --> MGR --> NOTIFY2
  NOTIFY2 --> SLA --> TASK --> CONTACT --> MILE
  MILE --> D30 --> D60 --> D90 --> CRIT
  CRIT -->|No| REENGAGE --> CONTACT
  CRIT -->|Yes| GRADAPP --> GRAD
  SLA -.->|Breached| BREACH --> DIGEST
```

### Technical View
```mermaid
flowchart TD
  CAND2["VIP_Candidate__c created\nSource: platform webhook OR manual console entry\nRacing: may be retroactive entry from in-person event\nLOE: MEDIUM"]
  READY["AGENTFORCE READINESS SCORE\nEinstein model inputs: wagering history\nsegment · account age · deposit pattern\nOutputs: readiness_score · recommended_segment\nLOE: HIGH"]
  ASSIGN2["SALESFORCE FLOW: Assignment Logic\nQuery KAM_Capacity__c + Segment_Specialty__c\nRank available KAMs by fit score\nCreate Manager_Approval_Task__c\nLOE: MEDIUM"]
  ERR3["⚠ KAM assignment conflict\nEscalate to Manager queue\nHold SLA timer pending resolution"]
  MGRAPPR["Manager approves in console\nKAM_Assignment__c record created\nKAM notified via Platform Event\nLOE: LOW"]
  MILECREATE["Create Onboarding_Milestone__c series\nFirst Contact — SLA 48h\n30d Engagement Check\n60d Engagement Check\n90d Engagement Check\nGraduation\nLOE: MEDIUM"]
  SLAMON["SCHEDULED APEX hourly\nMonitor milestone due dates\nIF approaching SLA: create Task on KAM\nIF 24h past due: alert Manager\nLOE: MEDIUM"]
  FIRSTCON["KAM logs first contact\nActivity_History__c auto-created\nMilestone status = Complete\nLast_Contact__c updated on VIP_Account__c\nCasino: RG-aware template used for first contact"]
  ENGBATCH["30/60/90d Scheduled Batch\nEvaluate Engagement_Score__c\nvs Graduation_Criteria__c segment thresholds\nCasino criteria includes RG check\nLOE: HIGH — criteria definition requires stakeholder input"]
  GRADCHECK{"All graduation\ncriteria met?"}
  REENGAGE2["Create NBA_Recommendation__c\naction_type = Re-engage\nLinked to Onboarding_Milestone__c"]
  PROMOTE["VIP_Account__c status = Active\nOnboarding_Tracker__c closed\nAgentforce generates first post-onboarding NBA\nManager notified of graduation\nLOE: LOW"]

  CAND2 --> READY --> ASSIGN2
  ASSIGN2 -->|Conflict| ERR3
  ASSIGN2 -->|Assigned| MGRAPPR --> MILECREATE --> SLAMON
  SLAMON --> FIRSTCON --> ENGBATCH --> GRADCHECK
  GRADCHECK -->|No| REENGAGE2 --> FIRSTCON
  GRADCHECK -->|Yes| PROMOTE
```

### LOE Summary — Flow 4
| Component | Complexity | Notes |
|-----------|-----------|-------|
| VIP_Candidate__c + nomination flow | LOW | Simple object + screen flow |
| Readiness Score Einstein model | HIGH | Training data, model build, ongoing tuning |
| KAM assignment logic (Flow) | MEDIUM | Capacity + specialty matching |
| Onboarding_Milestone__c + SLA tracking | MEDIUM | Scheduled Apex + task automation |
| Graduation criteria definition | HIGH | ⚠ Requires business input per segment |

---

## Flow 5: Data Integration Architecture
*How all external systems feed into Salesforce to power the console.*

### Stakeholder View
```mermaid
flowchart LR
  subgraph SRC2["Source Systems"]
    SBP["Sportsbook Platform\nNear real-time"]
    CCRM["Casino CRM\nNear real-time"]
    DFSP["DFS Platform\nContest batch"]
    TVG2["TVG — Racing\nTBD · T+1 minimum"]
    FDR["FanDuel Racing\nTBD · T+1 minimum"]
    TABL["Tableau — Survey\nTBD · 24h or near RT"]
  end
  subgraph SF2["Salesforce"]
    INGEST["Data Ingest Layer\nDeduplicate · Validate · Normalise"]
    UNIFIED["Unified VIP Data Layer"]
    AGENT["Agentforce Einstein\nNBA · Sentiment · RG · Readiness"]
    CONS["KAM Console\nAll modules"]
  end
  subgraph USERS["Users"]
    KAM4["VIP KAMs\nTaylor · Morgan · Casey · Reese"]
    AVY["Avery — KAM Manager"]
    PAT["Pat — Director"]
  end

  SBP & CCRM & DFSP --> INGEST
  TVG2 & FDR --> INGEST
  TABL --> INGEST
  INGEST --> UNIFIED --> AGENT --> CONS
  CONS --> KAM4 & AVY & PAT
```

### Technical View
```mermaid
flowchart TD
  subgraph INGEST2["Ingest Patterns per Source"]
    SBP2["SPORTSBOOK PLATFORM\nREST API + Platform Events\nObjects: Wagering_Event__c · GGR_Record__c\nRefresh: near real-time\nLOE: HIGH — event schema definition needed"]
    CCRM2["CASINO CRM\nREST API + Platform Events\nObjects: Casino_Session__c · Game_Affinity__c · Bonus_History__c\nRefresh: near real-time\nLOE: HIGH"]
    DFSP2["DFS PLATFORM\nScheduled Batch post-contest\nObjects: DFS_Contest_Entry__c · DFS_Prize__c\nRefresh: contest completion trigger\nLOE: MEDIUM"]
    TVG3["TVG — RACING\nIntegration pattern TBD\nLikely: file-based extract or REST API\nObject: TVG_Activity__c\nRefresh: T+1 minimum\nLOE: VERY HIGH — external system, unknown API"]
    FDR2["FANDUEL RACING\nIntegration pattern TBD\nLikely: internal REST API\nObject: FD_Racing_Activity__c\nRefresh: T+1 minimum\nLOE: HIGH — separate system to SB platform"]
    TABL2["TABLEAU — SURVEY\nIntegration TBD\nOption A: Tableau CRM Connector — 24h\nOption B: MuleSoft — near real-time\nObject: Survey_Response__c\nLOE: LOW to HIGH depending on method"]
  end
  subgraph QUAL["Data Quality Layer — Apex on Ingest"]
    DEDUP["Deduplication\nMatch on canonical vip_id\nMerge into VIP_Account__c master\nLOE: MEDIUM"]
    NULL["Null-fill and confidence flags\nRacing records flagged if coverage under 70%\nConfidence dampener written to VIP_Account__c\nLOE: MEDIUM"]
    VAL["Validation on ingest\nReject malformed records\nAlert ops team via Platform Event\nLOE: LOW"]
  end
  subgraph UNIFIED2["Unified Data Layer — Core Objects"]
    VIP2["VIP_Account__c — master record\nAll segments · all sources merged"]
    OBJS["CHILD OBJECTS\nWagering_Event__c · Casino_Session__c\nDFS_Contest_Entry__c · TVG_Activity__c\nFD_Racing_Activity__c · Survey_Response__c\nAlert_Record__c · NBA_Recommendation__c\nOnboarding_Milestone__c · KAM_Assignment__c"]
  end
  subgraph AI2["Agentforce Einstein Layer"]
    DC["Data Cloud — if in scope\nOR standard SOQL queries on unified objects\n⚠ Data Cloud scope TBD — significant LOE delta"]
    MODELS["Einstein Models\nNBA Scoring · Sentiment Analysis\nRG Classification · Readiness Score\nLOE: VERY HIGH — full model suite"]
  end
  CONS3["KAM CONSOLE — All Modules Read From Unified Layer\nVIP Playbook · NBA Queue · Awareness Center\nFeedback Pulse · Onboarding Tracker\nCampaign Manager · Last Contact Tracker"]

  SBP2 & CCRM2 & DFSP2 & TVG3 & FDR2 & TABL2 --> DEDUP
  DEDUP --> NULL --> VAL --> VIP2 --> OBJS
  OBJS --> DC --> MODELS --> CONS3
```

### LOE Summary — Flow 5
| Component | Complexity | Notes |
|-----------|-----------|-------|
| Sportsbook Platform integration | HIGH | Platform event schema + API design with FD platform team |
| Casino CRM integration | HIGH | Session and game affinity data model |
| DFS Platform integration | MEDIUM | Batch — simpler pattern |
| TVG integration | VERY HIGH | ⚠ External system — unknown API, likely file extract |
| FanDuel Racing integration | HIGH | ⚠ Separate system from Sportsbook — confirm team ownership |
| Tableau integration | LOW–HIGH | ⚠ Depends on method chosen (Connector vs MuleSoft) |
| Data dedup + quality layer | MEDIUM | Canonical ID strategy needed upfront |
| Data Cloud (if in scope) | VERY HIGH | Significant additional LOE — evaluate against SOQL approach |
| Full Einstein model suite | VERY HIGH | Multi-model build + training data + ops |

---

## Open Scoping Questions — For Stakeholder Session

| # | Question | Owner | Impact |
|---|----------|-------|--------|
| 1 | TVG integration pattern — REST API or file extract? | Tech Arch + TVG team | Flow 1, 4, 5 LOE |
| 2 | FanDuel Racing — which team owns the API? | FD Platform team | Flow 1, 4, 5 LOE |
| 3 | Tableau → Salesforce — Connector or MuleSoft? | Data team + Arch | Flow 3, 5 LOE |
| 4 | Data Cloud in scope or standard SOQL? | Solution Architect | Flow 5 — very high LOE delta |
| 5 | Who owns RG classification thresholds — Compliance or Product? | Compliance + Product | Flow 2 — gating logic |
| 6 | Graduation criteria per segment — who defines? | VIP Business team | Flow 4 — cannot build without this |
| 7 | Survey format confirmed? Star ratings staying or removed? | FanDuel VIP team | Flow 3 — data model |
