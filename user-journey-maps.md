# FanDuel VIP KAM Console — User Journey Maps

**Primary personas:** Taylor (Sportsbook KAM) · Morgan (Casino KAM)  
**Staged personas:** Casey (DFS KAM) · Reese (Racing KAM)  
**Leadership:** Avery (KAM Manager, hybrid default) · Pat (KAM Director — division ops, not Andy Geissler)  
**Stakeholder (not a user persona):** Andy Geissler, Director VIP Technology  

Full cards: research repo `deliverables/personas.md`. Visual board: Cursor canvas `vip-kam-personas-journeys.canvas.tsx`.

These maps are **future-state on this console**, written for Figma (prompt at the bottom). Each journey is a module in `index.html`. Segment nuances are not extra personas — they are how Taylor vs Morgan vs Casey vs Reese walk the same stages.

**Journey set (console modules):**
1. VIP Playbook — Managing My Book
2. NBA Recommendations — Acting on AI Guidance
3. VIP Onboarding — Setting Up a New VIP *(includes demotion / seasonal bench)*
4. Awareness Center — Staying Ahead of Risk
5. Events & Engagement — Four tools, synced in none *(added from the external readout)*
6. Manager Cockpit — Exceptions, not more records *(gap in the current prototype; Director is the last stage)*

---

## Journey 1: VIP Playbook — "Managing My Book"
**Primary Actor:** VIP KAM | **Secondary:** KAM Manager, Director  
**Goal:** The KAM logs in, orients to their full book of business, prioritizes outreach using filters and signals, drills into individual VIP profiles, identifies the right action, executes it, and closes the loop with a logged activity.

### Stages

#### Stage 1: Login
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM authenticates via SSO into the Salesforce Lightning shell and lands on the console home |
| System Response | Console loads the VIP Playbook as the default view; last-session state is restored (filters, sort order, pinned VIPs) |
| Touchpoint | Salesforce SSO → VIP Playbook table |
| Emotion | Neutral — routine start to the workday |
| Pain Point | ACE homepage is a dead end (external readout). Real morning ritual is HVP Excel via Slack (~2 hrs) because ACE refreshes ~11am–2pm. If session state is not persisted on top of that, KAM re-applies filters and starts even further behind. |
| Opportunity | Agentforce generates a "Good morning" digest: top 3 priority actions since last login, flagged VIPs, and one sentence on overnight activity (big wagers, alerts triggered) |

#### Stage 2: Orient
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM scans the Playbook table to get a sense of overall book health — total active VIPs, overdue contacts, open alerts |
| System Response | Table renders with sortable columns: VIP name, segment, tier, last contact date, open NBA count, alert flags, lifetime value, current-period GGR |
| Touchpoint | VIP Playbook — summary row / KPI header strip |
| Emotion | Scanning, slightly anxious if the book is large and alerts are high |
| Pain Point | No rolled-up health score means the KAM mentally synthesizes dozens of rows to understand book health; alert counts are not visually weighted by urgency |
| Opportunity | Agentforce surfaces a book-level health summary card: "7 VIPs overdue for contact, 2 payment alerts active, 1 high-value VIP in a losing streak" — single glanceable status |

#### Stage 3: Filter & Prioritize
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM applies filters (segment, tier, last contact window, alert type) and sorts by priority signal to surface who needs attention most urgently |
| System Response | Table dynamically re-renders; filtered row count updates; KAM can save filter presets; NBA badge count appears inline per VIP |
| Touchpoint | VIP Playbook — filter bar, column sort, saved views |
| Emotion | Focused — building the day's work queue |
| Pain Point | Filter options may not map to KAM mental models (e.g., "who hasn't heard from me in 7 days AND has an open alert" requires combining multiple filters manually); no AI-assisted prioritization |
| Opportunity | Agentforce proposes a smart "Today's Priority List" — a pre-ranked queue based on recency, alert severity, upcoming events, and engagement trend — one click to apply |

#### Stage 4: Drill Into VIP
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM clicks a VIP row to open the detail panel — reviews activity history, open bets, recent deposits/withdrawals, segment behavior, and prior contact notes |
| System Response | Side panel or full record loads with unified VIP profile: financial summary, engagement timeline, open NBAs, linked campaigns, onboarding status, and last Agentforce analysis |
| Touchpoint | VIP Playbook row → VIP Detail Panel / Salesforce Record |
| Emotion | Curious, building context — wants to walk into a conversation informed |
| Pain Point | Data is fragmented across multiple Salesforce objects and external systems (sportsbook platform, Casino CRM, Tableau); KAM must open multiple tabs and mentally stitch together a picture |
| Opportunity | Agentforce pre-compiles a "VIP Briefing Card" — a synthesized paragraph summarizing recent behavior, key financials, last contact outcome, and a recommended talking point |

#### Stage 5: Identify Action
| Dimension | Detail |
|-----------|--------|
| KAM Action | Having reviewed the profile, KAM decides the right next move — a call, an offer, an escalation, a campaign enrollment, or a check-in |
| System Response | NBA card for this VIP is visible in the panel; Agentforce recommendation is displayed with confidence score and rationale; KAM can accept, modify, or dismiss |
| Touchpoint | VIP Detail Panel → NBA Card |
| Emotion | Decisive but sometimes uncertain — the "right" action is not always obvious without full context |
| Pain Point | Without AI guidance, KAM relies on intuition and experience which varies across the team; junior KAMs may miss signals that senior ones catch |
| Opportunity | Agentforce NBA provides ranked action options with "why now" context (e.g., "This VIP just wagered 3x their average — now is the right time to offer a VIP experience upgrade") |

#### Stage 6: Take Action
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM executes the action — places a call, sends a personalized message, enrolls VIP in a campaign, or creates a task for a future touchpoint |
| System Response | Outbound call/message tools are accessible in-console; campaign enrollment is one-click from the profile; task creation auto-populates VIP context and due date |
| Touchpoint | VIP Detail Panel → Campaign Manager / Task Creator / Outreach tool |
| Emotion | Productive — in motion, executing the plan |
| Pain Point | Context-switching required if outreach tools live outside the console; copy-pasting VIP details into external tools breaks flow and introduces errors |
| Opportunity | Agentforce drafts a personalized outreach message based on VIP profile, prior conversations, and the specific NBA trigger — KAM edits and sends without leaving the console |

#### Stage 7: Log & Close
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM logs the activity outcome, updates the last contact date, and marks the NBA as acted upon |
| System Response | Activity is logged to the VIP timeline; Last Contact Tracker updates automatically; NBA card moves to "Completed" state; book health metrics recalculate |
| Touchpoint | Activity Log → Last Contact Tracker → NBA card status |
| Emotion | Satisfied — closure, progress visible |
| Pain Point | Manual note entry is time-consuming; KAMs often defer logging until end of day and lose nuance; Last Contact Tracker may not auto-update unless the KAM explicitly logs |
| Opportunity | Agentforce generates a call summary from brief notes and proposes the log entry for one-click confirmation; Last Contact Tracker updates automatically on any logged activity |

### Segment Nuances
- **Sportsbook / Taylor (Primary):** Prioritization is time-sensitive and event-driven — filters must surface VIPs active around upcoming slates. Bulk Bonus Tool exists but still needs Claude CSV prep. "Big Swings" in Awareness are a primary daily trigger into Playbook.
- **Casino / Morgan (Primary):** Same Playbook, 24/7 clock. Book health is session frequency and average session value; CPP token status belongs on the briefing card. No Bulk Bonus Tool — leaving ACE for CPP is the generosity path today.
- **Racing / Reese (2 systems, sparse data):** Book is smaller and more relationship-intensive. TVG + FDR double rows mean Agentforce must surface what is available **and flag missing identity**. Do not rank a duplicated human.
- **DFS / Casey (Small, strategic):** Contest entry and prize outcomes are the columns that matter, not live handle. Playbook is a deep-dive on ~180–200 names, plus nightly voucher work (see Campaign Manager). A DFS chip on SBK KPIs is not enough.

### Key Moments
The highest-stakes moment is **Stage 5: Identify Action** — a misread of VIP intent here can damage the relationship rather than strengthen it. Agentforce's "why now" context is the difference between a KAM who feels like a partner and one who feels like a sales rep.

---

## Journey 2: NBA Recommendations — "Acting on AI Guidance"
**Primary Actor:** VIP KAM | **Secondary:** KAM Manager (Manager Review stage)  
**Goal:** The KAM spots an Agentforce-generated Next Best Action alert, evaluates the AI analysis, makes a human judgment call, executes, completes the loop, and surfaces outcomes for manager visibility.

### Stages

#### Stage 1: Spot Alert
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM notices an NBA badge on a VIP row or receives a push notification that a new recommendation has been generated |
| System Response | NBA badge count increments on the VIP row; top-of-page notification strip highlights new high-priority NBAs; card is collapsed by default |
| Touchpoint | VIP Playbook — NBA badge / notification strip |
| Emotion | Alert, attentive — a signal has arrived |
| Pain Point | If many NBAs are generated simultaneously, badge fatigue sets in and KAMs begin ignoring notifications; no prioritization between "urgent" and "nice-to-do" |
| Opportunity | Agentforce assigns urgency tiers (P1/P2/P3) with plain-language rationale — "Act within 24h: this VIP hit a significant loss threshold and has not been contacted in 5 days" |

#### Stage 2: Open Card
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM clicks to expand the NBA card, revealing the full recommendation detail |
| System Response | Card expands inline; content loads: recommendation type, triggering signal, AI analysis summary, confidence score, proposed action options |
| Touchpoint | NBA Expandable Card |
| Emotion | Curious, open — ready to evaluate |
| Pain Point | Cards that are too dense with data cause cognitive overload rather than clarity; slow load breaks momentum |
| Opportunity | Agentforce renders the card in a scannable three-part layout: (1) What happened, (2) Why it matters, (3) What you can do |

#### Stage 3: Read AI Analysis
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM reads the AI-generated analysis — triggering behavioral signals, recommended action, rationale, and relevant VIP history |
| System Response | Analysis presented in plain language with linked supporting data; confidence score displayed with brief explanation of what drove it |
| Touchpoint | NBA Card — analysis body |
| Emotion | Evaluative, occasionally skeptical — "Do I agree with this?" |
| Pain Point | Black-box recommendations without clear rationale erode KAM trust quickly; tenured KAMs may dismiss AI guidance that contradicts their gut without evidence to weigh against it |
| Opportunity | Agentforce surfaces the exact data points that drove the recommendation ("Triggered by: 3 deposits in 72h + no KAM contact in 10 days + upcoming high-value event") |

#### Stage 4: Decide
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM makes a judgment call: accept, modify (different action type or timing), snooze, or dismiss with a reason code |
| System Response | Accept / Modify / Snooze / Dismiss buttons are prominent; Dismiss triggers a lightweight reason picker; all decisions logged for Agentforce model feedback |
| Touchpoint | NBA Card — action buttons |
| Emotion | Empowered — human in the loop, not replaced by AI |
| Pain Point | If dismiss/snooze requires a lengthy form, KAMs skip it and close the card — losing valuable feedback data that would improve future recommendations |
| Opportunity | Agentforce learns from accept/dismiss patterns per KAM and per segment, progressively improving recommendation relevance |

#### Stage 5: Execute
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM executes the accepted action — sends AI-drafted message, places call, enrolls VIP in recommended campaign, or creates a task |
| System Response | Agentforce pre-populates the outreach draft, campaign enrollment form, or task; KAM reviews, edits if needed, and confirms |
| Touchpoint | NBA Card → Outreach tool / Campaign Manager / Task Creator |
| Emotion | Efficient, confident — the AI has done the heavy lifting |
| Pain Point | If execution requires leaving the NBA card to navigate to a separate tool, context is lost |
| Opportunity | "Execute in context" — outreach draft, campaign link, or task form is embedded directly in the expanded NBA card, no navigation required |

#### Stage 6: Complete
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM marks the NBA as completed after execution; adds qualitative notes on the VIP's response or outcome |
| System Response | NBA card moves to "Completed" state; VIP timeline updated; Last Contact Tracker updates; NBA completion feeds into KAM performance metrics |
| Touchpoint | NBA Card → VIP Timeline → Last Contact Tracker |
| Emotion | Satisfied, accomplished — a clean close |
| Pain Point | KAM performance metrics around completion rate may feel punitive if evaluated regardless of recommendation quality — creates pressure to "complete" without genuine engagement |
| Opportunity | Agentforce tracks outcome quality, not just completion — "VIP responded positively / No response / VIP churned post-action" — so managers see effectiveness, not just activity |

#### Stage 7: Manager Review
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM Manager reviews team-level NBA completion rates, dismissal patterns, and outcome quality; Director reviews aggregate trend data |
| System Response | Manager view surfaces KAM-level NBA adoption rate, most-dismissed recommendation types, and outcome correlation data |
| Touchpoint | Manager Dashboard / Reporting layer |
| Emotion | Oversight — analytical, strategic |
| Pain Point | If managers only see completion counts without outcome quality, they cannot distinguish high-performing KAMs from those who complete NBAs perfunctorily |
| Opportunity | Agentforce Manager Insights: "Your team dismissed 34% of VIP retention NBAs last week — top reason: 'Already handled.' Consider reviewing the triggering threshold." |

### Segment Nuances
- **Sportsbook (Primary):** NBAs are event-triggered with short expiry windows — a recommendation to contact a VIP before a major game becomes worthless 2 hours later. Urgency tiering and time-to-expiry indicators are critical.
- **Casino (Primary):** NBAs often center on session re-engagement or responsible gambling signals. Dismissal patterns from Casino KAMs are particularly important to monitor as they indicate where AI thresholds may be miscalibrated.
- **Racing (2 systems, sparse data):** NBAs are less frequent and higher-touch — a recommendation to offer hospitality at an upcoming race meeting requires lead time. Data sparsity means Agentforce confidence scores may be lower; KAMs should be coached on how to interpret low-confidence NBAs.
- **DFS (Small, strategic):** NBAs align tightly with contest cycles — a recommendation before a major NFL contest deadline is highly time-sensitive. The small book size means each NBA carries more individual weight.

### Key Moments
The highest-stakes moment is **Stage 3: Read AI Analysis** — if the analysis fails to earn the KAM's trust, the entire NBA system will be dismissed as noise within weeks of launch. The transparency of the triggering signals, not the sophistication of the model, determines adoption.

---

## Journey 3: VIP Onboarding — "Setting Up a New VIP for Success"
**Primary Actor:** VIP KAM | **Secondary:** KAM Manager (assignment, first contact, graduation)  
**Goal:** A newly identified high-value player is structured into the VIP program — profiled, assigned to the right KAM, given a tailored first-contact experience, and monitored through early engagement until confidently active in the book.

### Stages

#### Stage 1: Identify New VIP
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM or Manager identifies a new VIP candidate — from an automated platform alert, a referral from the platform team, or a manual nomination |
| System Response | Onboarding tracker displays a "New Candidate" queue; candidate record pre-populated with platform data; Agentforce runs an initial profile analysis |
| Touchpoint | Onboarding & Offboarding Tracker — Candidate Queue |
| Emotion | Curious, optimistic — a new relationship with potential |
| Pain Point | Candidate identification is often reactive and ad hoc; the threshold for nomination is inconsistent across segments; KAMs may receive a name with minimal context |
| Opportunity | Agentforce proactively flags candidates who cross value thresholds and surfaces a "Readiness Score" — likelihood this VIP will respond well to outreach based on behavioral profile |

#### Stage 2: Review Profile
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM reviews the candidate's profile — financial history, preferred product, activity cadence, geographic market, and any existing support interactions |
| System Response | Unified VIP profile panel loads from the onboarding record; Agentforce "VIP Briefing Card" generated — a synthesized paragraph summarizing who this person is and what they care about |
| Touchpoint | Onboarding Tracker → VIP Profile Panel |
| Emotion | Research mode — building empathy and context before reaching out |
| Pain Point | Data completeness varies significantly; KAMs have no standard for what "enough information to make first contact" looks like |
| Opportunity | Agentforce generates a "Profile Confidence Indicator" — flags which data dimensions are strong vs. thin, and suggests data-gathering questions for the first contact to fill gaps |

#### Stage 3: Assign & Configure
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM Manager assigns the VIP to the right KAM based on segment expertise, book capacity, and relationship fit; KAM accepts and configures the VIP record |
| System Response | Assignment tracked in Onboarding Tracker with timestamp; KAM receives in-console notification; VIP record linked to KAM's Playbook; tier and channel preferences are editable fields |
| Touchpoint | Onboarding Tracker — Assignment step / Manager Dashboard |
| Emotion | KAM: ready, taking ownership. Manager: confident in the match |
| Pain Point | Assignment rationale is rarely documented — if the KAM moves or the VIP needs re-assignment, the logic behind the original match is lost |
| Opportunity | Agentforce suggests the optimal KAM assignment based on book capacity, segment specialty, and historical outcome data |

#### Stage 4: Plan First Contact
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM plans the first outreach — choosing channel, timing, and message framing; may set up a task with a specific due date and talking points |
| System Response | First Contact task auto-created with a suggested SLA (e.g., "First contact within 48h of assignment"); Agentforce drafts a personalized first-contact talking point guide |
| Touchpoint | Onboarding Tracker → Task Creator → Agentforce Companion |
| Emotion | Thoughtful, slightly pressured — the first impression matters |
| Pain Point | No standard first-contact playbook by segment; junior KAMs are unsure what to say; inconsistency in the program's opening impression |
| Opportunity | Agentforce generates a segment-specific first-contact guide: channel recommendation, three personalized talking points, and a suggested tone drawn from the VIP's profile |

#### Stage 5: Execute First Contact
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM makes first contact — call, message, or email — introduces the VIP program, establishes the relationship, and gathers preference data |
| System Response | Outreach logged in VIP timeline; Last Contact Tracker updates; Onboarding Tracker milestone "First Contact Made" checked off |
| Touchpoint | Last Contact Tracker → VIP Timeline → Onboarding Tracker milestone |
| Emotion | Energized, relationship-building — this is the KAM's strength |
| Pain Point | If the call is not logged promptly, the milestone is missed and the VIP may appear "not yet contacted" in manager reporting |
| Opportunity | Agentforce captures the outreach log automatically from CRM activity (call via integrated dialer, message sent in-console) and marks the milestone without KAM manual input |

#### Stage 6: Monitor Early Engagement
| Dimension | Detail |
|-----------|--------|
| KAM Action | Over first 30 days, KAM monitors engagement — wagering, product usage, response to outreach, offer redemption |
| System Response | Onboarding Tracker shows 30/60/90-day engagement timeline with milestone markers; Awareness Center flags early alerts; Agentforce generates weekly engagement health updates |
| Touchpoint | Onboarding Tracker — Engagement Timeline / Real-Time Awareness Center |
| Emotion | Watchful, invested — wants the relationship to take hold |
| Pain Point | Engagement monitoring requires checking multiple places — wagering platform, CRM, Campaign Manager; no unified early-engagement view |
| Opportunity | Agentforce delivers a weekly "Early Engagement Pulse": behavioral signals, engagement score trend, recommended next action, and a flag if the VIP appears at risk of disengaging before 30 days |

#### Stage 7: Graduate to Active Book
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM and Manager agree the VIP has passed the onboarding threshold and move them to the active Playbook |
| System Response | Onboarding Tracker status changes to "Active"; VIP record promoted to full Playbook visibility; Agentforce generates a "Graduation Summary" with recommended ongoing cadence and first post-onboarding NBA |
| Touchpoint | Onboarding Tracker → VIP Playbook |
| Emotion | Proud, confident — a relationship established |
| Pain Point | Graduation criteria are often informal and subjective; KAMs may graduate VIPs too early or miss the window when the VIP is most receptive |
| Opportunity | Agentforce defines a data-driven graduation threshold per segment: "This VIP meets 4 of 5 criteria — one more successful interaction recommended before promotion" |

#### Stage 8: Manager Oversight
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM Manager reviews onboarding pipeline: stage distribution, SLA compliance, and early engagement quality across the team |
| System Response | Manager view shows stage distribution, SLA compliance, and Agentforce engagement health scores per KAM's onboarding book; Director sees segment-level pipeline health |
| Touchpoint | Onboarding Tracker — Manager View / Director Dashboard |
| Emotion | Strategic oversight — accountability without micromanagement |
| Pain Point | Without standardized onboarding SLAs, managers rely on anecdotal check-ins rather than data |
| Opportunity | Agentforce sends managers a weekly "Onboarding Health Digest" flagging SLA breaches, VIPs at risk, and outlier success stories |

### Segment Nuances
- **Sportsbook (Primary):** First contact timing should align with upcoming event calendars — a VIP onboarded during a dead sports period will be harder to engage. Agentforce should factor the sports calendar into first-contact recommendations.
- **Casino (Primary):** RG awareness is heightened during onboarding — KAMs must establish a tone that is welcoming but not aggressive about play frequency. First-contact guide should include RG-aware language frameworks.
- **Racing (2 systems, sparse data):** Onboarding is often relationship-led rather than digital-first; many Racing VIPs come through hospitality events or referrals. Profile data may be thin at outset — the "Profile Confidence Indicator" is especially important here.
- **DFS (Small, strategic):** New VIPs often need education on specific DFS VIP benefits (priority entry access, dedicated support for large slates). First contact should include a product walkthrough element.

### Key Moments
The highest-stakes moment is **Stage 5: Execute First Contact** — the opening interaction sets the relational tone for the entire VIP lifecycle. A first contact that feels generic or uninformed will create skepticism that is difficult to reverse regardless of how good subsequent interactions are.

---

## Journey 4: Awareness Center — "Staying Ahead of Risk"
**Primary Actor:** VIP KAM | **Secondary:** KAM Manager, Director (escalation)  
**Goal:** The KAM opens the Real-Time Awareness Center, processes risk signals across tabs (Big Swings, Payment Alerts, Losing Streaks), contextualizes each alert, decides on the appropriate response, executes, monitors resolution, and escalates where needed.

### Stages

#### Stage 1: Open Center
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM navigates to the Real-Time Awareness Center — either proactively as a daily check or in response to a notification badge |
| System Response | Center loads with three tabs: Big Swings, Payment Alerts, Losing Streaks; each tab shows alert count badge; most recent/urgent alerts surface at top; "Unread" indicator highlights new alerts since last visit |
| Touchpoint | Real-Time Awareness Center — tab navigation |
| Emotion | Alert, focused — this part of the day requires the most judgment |
| Pain Point | If all three tabs have high alert counts simultaneously, KAM must triage across tabs without a unified priority view — risk of missing a critical alert buried beneath lower-priority noise |
| Opportunity | Agentforce generates a cross-tab "Alert Triage Summary" at the top: ranked list of top 3 alerts across all tabs with one-line severity rationale |

#### Stage 2: Scan Big Swings
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM opens the Big Swings tab and scans for VIPs who have placed unusually large wagers relative to their historical baseline |
| System Response | Tab renders alerts sorted by wager size relative to baseline; each row shows VIP name, wager amount, event/game, deviation from average, timestamp; color-coded severity (amber/red) |
| Touchpoint | Awareness Center — Big Swings tab |
| Emotion | Engaged, analytical — reading the signals |
| Pain Point | A large wager is not inherently a risk or opportunity without context — raw data without baseline context creates noise, not signal |
| Opportunity | Agentforce annotates each Big Swing with behavioral context: "This is 4.2x this VIP's average wager — their 3rd such event this month" plus a recommended action type (monitor / reach out / flag) |

#### Stage 3: Review Payment Alert
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM opens Payment Alerts tab and reviews flagged payment events — failed deposits, reversed withdrawals, chargeback indicators, or unusual deposit velocity |
| System Response | Tab shows each alert with type, VIP, amount, timestamp, and status (new / in review / resolved); links to full payment record; Agentforce has run an initial severity assessment |
| Touchpoint | Awareness Center — Payment Alerts tab |
| Emotion | Cautious, responsible — payment issues require delicacy and compliance awareness |
| Pain Point | KAMs do not always know when it is appropriate for them to reach out to the VIP vs. when they need to wait for compliance clearance |
| Opportunity | Agentforce classifies each payment alert by recommended KAM action: "Do not contact — compliance review in progress," "Safe to contact," or "Escalate to manager before acting" |

#### Stage 4: Review Losing Streak
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM opens Losing Streaks tab and reviews VIPs with sustained losses — potential responsible gambling signals alongside retention risk |
| System Response | Tab shows VIPs in a streak with cumulative loss, streak duration, segment, and RG flag if thresholds have been met; links to full profile and RG interaction history |
| Touchpoint | Awareness Center — Losing Streaks tab |
| Emotion | Empathetic, careful — this tab carries the heaviest human weight |
| Pain Point | The line between retaining a losing VIP and facilitating problem gambling is critically important and not always clearly defined |
| Opportunity | Agentforce enforces RG-aware protocols automatically: if a VIP has crossed an RG threshold, the "Contact" action is replaced by an "RG Referral" action with the approved communication pathway — removing discretion from a high-risk decision |

#### Stage 5: Contextualize
| Dimension | Detail |
|-----------|--------|
| KAM Action | For each high-priority alert, KAM opens the linked VIP profile to contextualize against full relationship history — is this a pattern? Has there been recent contact? |
| System Response | VIP profile panel opens from within the alert row without losing alert context; Agentforce Briefing Card includes alert-specific context ("This alert is the second in 14 days — prior alert resolved with no contact") |
| Touchpoint | Awareness Center alert row → VIP Profile Panel |
| Emotion | Investigative, building a complete picture before acting |
| Pain Point | Navigating from an alert to the full VIP profile and back is a multi-step flow that breaks concentration; losing alert context on return means re-orienting |
| Opportunity | Agentforce maintains "alert context persistence" — the Center remembers where the KAM was in their review and allows a quick return to the alert queue after profile review |

#### Stage 6: Decide
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM decides how to respond: reach out now, monitor and review tomorrow, escalate to manager, or close as not actionable |
| System Response | Each alert has action buttons: Respond Now, Monitor (snooze with reminder), Escalate, Close; decision logged with reason; Agentforce surfaces recommended action with confidence and rationale |
| Touchpoint | Awareness Center alert — action buttons |
| Emotion | Decisive but considered — aware that the wrong move has real consequences |
| Pain Point | Without documented decision rationale, escalation reviews by managers lack context — accountability gaps emerge |
| Opportunity | Agentforce auto-populates the decision log with alert data, contextual signals, and the KAM's chosen action — creating an auditable record without manual entry |

#### Stage 7: Execute
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM executes the response — personalized outreach, VIP experience offer, responsible gambling communication, or internal escalation |
| System Response | Outreach dispatched in-console; Agentforce has drafted the message based on alert type and VIP profile; RG alerts use pre-approved communication templates; escalation generates a structured alert record for the manager |
| Touchpoint | Awareness Center → Outreach tool / Escalation form / RG communication template |
| Emotion | In motion — the weight of the decision lifts when action is taken |
| Pain Point | RG communications must follow specific approved language; KAMs using freestyle messaging introduce compliance risk |
| Opportunity | Agentforce locks RG communication into approved templates while allowing KAMs to personalize only the approved variable fields — compliance by design |

#### Stage 8: Monitor Resolution
| Dimension | Detail |
|-----------|--------|
| KAM Action | After acting, KAM monitors whether the alert has resolved — did the VIP respond? Did the losing streak continue? Was the payment issue cleared? |
| System Response | Alert status updates to "In Resolution"; if the triggering signal persists after 24–48h with no change, the alert re-flags automatically; resolution confirmation closes the alert and logs the outcome |
| Touchpoint | Awareness Center — alert status / VIP Timeline |
| Emotion | Patient but watchful — resolution is not always immediate |
| Pain Point | Alerts with no time-bound follow-up can sit in "In Resolution" indefinitely, giving a false sense of being handled |
| Opportunity | Agentforce sets auto-escalation timers based on severity — "If no resolution signal in 48h for a P1 Losing Streak alert, escalate automatically to KAM Manager" |

#### Stage 9: Escalate to Manager
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM escalates a high-severity or unresolved alert to KAM Manager with full context; Manager reviews and either takes over or provides guidance; Director looped in for threshold-level events |
| System Response | Escalation record created with full alert history, KAM actions taken, and Agentforce analysis; Manager receives in-console notification and structured briefing; Director dashboard shows aggregate escalation trends |
| Touchpoint | Escalation form → Manager Dashboard → Director Reporting |
| Emotion | KAM: relieved to have support. Manager: accountable, taking ownership. Director: concerned with pattern-level risk |
| Pain Point | Escalation context is often conveyed verbally or via Slack, losing the structured data trail; managers must reconstruct history rather than seeing it pre-assembled |
| Opportunity | Agentforce generates an "Escalation Briefing" automatically — alert history, actions taken, VIP profile highlights, and recommended next step — so the manager walks in fully briefed |

### Segment Nuances
- **Sportsbook (Primary):** Big Swings dominate the Awareness Center — pre-game wager spikes are frequent around major events. Console must distinguish event-driven expected spikes from genuine anomalies using event-specific baselines, not just overall averages.
- **Casino (Primary):** Losing Streak signals carry the highest sensitivity given regulatory context around responsible gambling. The RG-aware response protocol is most critical here; this tab should have the highest visual prominence in the Casino KAM's view.
- **Racing (2 systems, sparse data):** Awareness Center usage is lower frequency given a smaller, more relationship-known book. When alerts do fire around major race meetings, they are often engagement signals first and risk signals second. Data sparsity means some signals may not fire at all — KAMs rely more on personal knowledge of their book.
- **DFS (Small, strategic):** Payment alerts are less common. Big Swings are recontextualized — a large DFS contest entry in season-opening week may be completely normal. Agentforce baselines for DFS must be contest-type-aware, not just dollar-amount-aware.

### Key Moments
The highest-stakes moment is **Stage 4: Review Losing Streak** — this is where the business's commercial interests and its duty of care to customers are in direct tension. The quality of Agentforce's RG classification determines whether this tool enables compliant, empathetic care or inadvertently encourages harmful contact with vulnerable players.

---

## Journey 5: Events & Engagement — "Four Tools, Synced in None"
**Primary Actor:** Taylor / Morgan (VIP KAM) | **Secondary:** Avery (allocation / ROI), Pat (rare)  
**Goal:** Run the hospitality lifecycle — request, invite, ticket, attend, follow up, ROI — on one ACE surface so two KAMs cannot issue the same seat and post-event reporting is not a reconciliation project.

*External readout Finding 03. Ticket Manager ↔ ACE is the one integrated step today. Asana, Splash, attendance, and follow-up are not.*

### Stages

#### Stage 1: See Inventory
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM opens Upcoming Events to see what they can offer this week — seats, suite, trip, leftover inventory |
| System Response | Events rail lists date, remaining inventory, geo/venue, and conflict lock if another KAM has pulled from the pool |
| Touchpoint | Upcoming Events |
| Emotion | Neutral, scanning |
| Pain Point | Inventory lives in Ticket Manager plus Excel plus Asana. "Two KAMs both gave out the same ticket. There's no lock." — Stockton, Sportsbook |
| Opportunity | Agentforce flags double-pull in real time and proposes the next-best unused seat |

#### Stage 2: Pick Names
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM builds a 40-name long list, needs the right 20 |
| System Response | Ranking by 90-day NGR, KYC fit, last attendance, no-show pattern; KAM can override |
| Touchpoint | Events → Campaign Manager · Agentforce rank |
| Emotion | Focused — fairness and ROI in tension |
| Pain Point | Greg (Casino manager) sorts 40 names by 90-day NGR by hand. KYC is incomplete so "fit" is memory |
| Opportunity | Agentforce returns ranked 20 with "why this person" and a seasonal/RG exclusion list |

#### Stage 3: Invite & RSVP
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM sends invites and tracks who is in |
| System Response | Splash (or ACE-native RSVP) writes status back to the contact; Ticket Manager path for seats |
| Touchpoint | Events · Ticket Manager (already liked) |
| Emotion | Productive when it works; anxious when tools diverge |
| Pain Point | Invite lives in Splash; ACE never learns who RSVP'd. "After an event I go to Splash for attendance, Asana to close the request, then manually update ACE notes." — Jake, Casino |
| Opportunity | RSVP writes to the contact timeline. Asana approval status visible without leaving ACE |

#### Stage 4: Attend
| Dimension | Detail |
|-----------|--------|
| KAM Action | Night of / day after, KAM marks who showed |
| System Response | Contact gets "attended Mets 8-16"; no-show pattern increments |
| Touchpoint | Events → contact timeline |
| Emotion | Satisfied if logged; resigned if they skip it |
| Pain Point | No events tab on the profile. Attendance is personal notes |
| Opportunity | Check-in or Splash attendance auto-writes the tag; no-show pattern feeds the next rank |

#### Stage 5: Follow up & ROI
| Dimension | Detail |
|-----------|--------|
| KAM Action | KAM thanks attendees, redistributes unused tickets, later asks "was it worth it" |
| System Response | Follow-up task auto-created; unused-ticket alert; NGR vs invitees on a card Avery can see |
| Touchpoint | Events · NBA · Playbook |
| Emotion | Relieved when the loop closes |
| Pain Point | Post-event reporting is a manual reconciliation project every time. Follow-up is untracked personal reminders |
| Opportunity | One ROI card per event. Unused tickets re-enter the pool with a lock |

### Segment Nuances
- **Sportsbook / Taylor:** Regional tentpoles + box seats. Conflict lock on the ticket pool is the Sportsbook-specific scar.
- **Casino / Morgan:** Higher volume of smaller events + F&B. Same module, more operational traffic. Host team may execute while Morgan is off.
- **Racing / Reese:** 8–10 states. Keeneland (and similar) refuse Ticket Manager — email/manual must still write attendance. 30-mile geo-radius fails this book.
- **DFS / Casey:** 4–5 blowout trips/year for top 12–20; hotel + flight credit, player books travel. Not a box-seat object. Travel-credit tracking belongs here.

### Key Moments
**Stage 3: Invite & RSVP** — this is where ACE loses the plot today. If RSVP never writes back, Stages 4–5 stay spreadsheets forever, no matter how pretty the Events rail looks.

---

## Journey 6: Manager Cockpit — "Exceptions, Not More Records"
**Primary Actor:** Avery (KAM Manager) | **Secondary:** Pat (Director queue) · Taylor/Morgan as the team being coached  
**Goal:** Avery sees team health without clicking into every KAM, toggles to a personal book if hybrid, and packages the rare item Pat must approve. Not in the current prototype.

*External readout: configurable by role. Research T-03: all 6 managers built a different DIY. Zach spends 90% of time on the homepage.*

### Stages

#### Stage 1: Land in Team Mode
| Dimension | Detail |
|-----------|--------|
| KAM Action | Avery authenticates and lands in **My team**, not Jordan's KAM Playbook |
| System Response | Header toggle: My book / My team. Team top-20 + scorecards load |
| Touchpoint | Manager homepage (to design) |
| Emotion | Expectant — this is where they already live |
| Pain Point | Current prototype is KAM-only. Today: "click, click, click." SharePoint 1:1 sheets. Outlook cadence. Databricks |
| Opportunity | "A high-level dashboard to see top 20 players and team scorecards in one view." — Lisa, Sportsbook Manager |

#### Stage 2: Scan Per-KAM Exceptions
| Dimension | Detail |
|-----------|--------|
| KAM Action | Avery reads one strip per KAM: stale priority contacts, winners, losers, bonus over/under, bulk vs 1:1 mix |
| System Response | Stacked KAM cards; drill into a KAM without losing the team view |
| Touchpoint | Manager per-KAM strip |
| Emotion | Coaching mode |
| Pain Point | Last-activity is gamed by bulk email. No bulk-vs-1:1 mix. WPC completion is a scorecard stick, not a coaching signal |
| Opportunity | Zach's spec is the UI: "Andrew's top 10 priority contacts he hasn't reached… top 5 winners… losers… too much bonus… not enough" |

#### Stage 3: Coverage
| Dimension | Detail |
|-----------|--------|
| KAM Action | Avery checks who is OOO and whether a buddy/host team is covering |
| System Response | Team calendar; Casino host-team load; WPC does not punish a covered KAM |
| Touchpoint | Team OOO (#10) |
| Emotion | Protective |
| Pain Point | Buddy system. Shared inbox denied. Outlook invites |
| Opportunity | Coverage is a first-class state on the VIP, not a Slack message |

#### Stage 4: Assign & Coach
| Dimension | Detail |
|-----------|--------|
| KAM Action | Avery drops a task onto a KAM's NBA/WPC or flags a pattern for 1:1 |
| System Response | Task appears on the KAM homepage; outcome later visible to Avery |
| Touchpoint | Manager action → KAM NBA/WPC |
| Emotion | Empowered |
| Pain Point | "At Caesars we could assign tasks as managers." — Lisa. ACE cannot |
| Opportunity | Assignment with a why; not a shadow Excel |

#### Stage 5: Hybrid Flip
| Dimension | Detail |
|-----------|--------|
| KAM Action | Five of six managers still have 20–170 personal VIPs. Avery flips to My book and runs Journey 1 for two names |
| System Response | Same console, personal Playbook. No second login |
| Touchpoint | Header toggle |
| Emotion | Relieved if instant; furious if they must alt-tab |
| Pain Point | Hybrid is the default and has no toggle today |
| Opportunity | Mode persists per session; team badge remains visible so they can jump back |

#### Stage 6: Package for Director
| Dimension | Detail |
|-----------|--------|
| KAM Action | Avery builds the $50K+ bonus or ownership-transfer or demotion-appeal package |
| System Response | RTC 30/60/180/365 already calculated; RG flags; KAM notes; one submit to Pat |
| Touchpoint | Approval queue |
| Emotion | Anxious — 15 minutes of math in email today |
| Pain Point | Manual RTC windows + email to director |
| Opportunity | The package *is* the Director experience. Do not make Pat open a KAM homepage |

#### Stage 7: Director Decide (Pat)
| Dimension | Detail |
|-----------|--------|
| KAM Action | Pat opens a short queue, not 353 NBA cards. Approves, bounces, or asks for more. Leaves |
| System Response | Decision + audit trail. VIP never knows Pat exists |
| Touchpoint | Director queue (slice of this cockpit) |
| Emotion | Decisive, brief |
| Pain Point | Policy lives in a director's head (Alex NY: demote them all). No trail. Andy Geissler is the tech stakeholder — he is not this user |
| Opportunity | Queue + policy rollup only. If we only fund one leadership surface, this journey is it; Pat borrows Stage 6–7 |

### Segment Nuances
- **Sportsbook Avery:** WPC completion, Tableau scorecards, bulk-vs-1:1 bonus mix (Ben).
- **Casino Avery:** 24/7 freshness, host-team coverage, F&B / ticket assets (Greg). Zach is the rare pure-oversight variant — 90% homepage.
- **Racing Avery:** Geo-dispersed KAMs, TVG/FDR dedupe health, Outlook rotation DIY (Dom Reggio).
- **DFS Avery:** Tiny team (2 KAMs), Databricks-native exceptions, manual onboard from queries (Dom Sindoni).

### Key Moments
**Stage 1: Land in Team Mode** — if Avery lands on Jordan's KAM Playbook, they bounce as fast as KAMs bounce from today's ACE. This journey is a different information architecture, not extra list views.

---

## Cursor → Figma Prompt Template

Use this prompt in Cursor (with Figma MCP active) for each journey. Paste the journey stage data where indicated.

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

[PASTE JOURNEY DATA HERE]

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
