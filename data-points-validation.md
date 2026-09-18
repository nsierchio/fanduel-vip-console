# FanDuel VIP KAM Console — Data Points Validation

**Purpose:** Identify every data field displayed in the console. FanDuel team to validate: where does this data live today, is it accessible via API, and what effort is required to surface it?

**How to use:**
- Review each field row
- Fill in **Confirmed Source** column (e.g. Salesforce CRM, DataPoints, TVG, FDR, Manual/KAM-entered, Not yet collected)
- Flag **Accessible Today?** (Yes / Needs integration work / Unknown)
- Add notes on any blockers, latency concerns, or data quality issues

---

## Confidence Key
| Symbol | Meaning |
|---|---|
| ✅ | Very likely in Salesforce CRM (standard object/field) |
| 🟡 | Likely DataPoints or FanDuel internal data platform |
| 🔵 | Likely Agentforce / Einstein AI-generated |
| ⚠️ | Unknown source — needs validation |
| 🔴 | Likely requires new data collection or integration |

---

## 1. VIP Master Record (used across all components)

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| VIP Name | Full name of the player | ✅ | | | |
| VIP ID | Internal player identifier | ✅ | | | |
| Segment | Product segment: SBK / CAS / DFS / FDR | ✅ | | | Multiple segments per player |
| Tier | VIP tier level (Tier 1–5 / Shield Select) | ✅ | | | Tier taxonomy differs by segment |
| State | Player's state/jurisdiction | ✅ | | | |
| Account Status | Active / Suspended / Deactivated | ✅ | | | |
| KAM Assignment | Which KAM owns this VIP | ✅ | | | |
| Date of Birth | For birthday detection | ✅ | | | |
| Join Date | When player became a VIP | ✅ | | | |
| LTV (Lifetime Value) | Total lifetime GGR or net revenue | 🟡 | | | |

---

## 2. VIP Playbook — My VIPs Table

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| GGR L30 | Gross Gaming Revenue last 30 days | 🟡 | | | Positive = house won; Negative = player winning |
| Last Contact Date | Most recent KAM contact log date | ✅ | | | Likely Activity object in Salesforce |
| Last Bonus Issued — Amount | Dollar value of most recent bonus | ✅ | | | |
| Last Bonus Issued — Type | Bonus Bet / Postal Gift / Event | ✅ | | | |
| Last Bonus Issued — Date | Date bonus was issued | ✅ | | | |
| Overbonused Flag | Whether player has hit their bonus cap | 🟡 | | | |
| Bonus Cap Amount | Player's total allowed bonus amount | 🟡 | | | |
| Bonus Cap Reset Date | When the cap resets | 🟡 | | | |
| NBA Pending Flag | Whether Agentforce has a pending action for this player | 🔵 | | | Agentforce Next Best Action |
| NBA Recommendation Text | The actual AI-generated recommendation | 🔵 | | | |

---

## 3. Overview Metrics (VIP Playbook — Overview Tab)

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Total VIPs Count | Total VIPs assigned to KAM | ✅ | | | |
| Active VIPs Count | VIPs with Active status | ✅ | | | |
| Promotions (period) | VIPs promoted to higher tier this period | 🟡 | | | |
| Demotions (period) | VIPs demoted or churned this period | 🟡 | | | |
| GGR by Segment | 30/60/90d GGR broken down by SBK/CAS/DFS/FDR | 🟡 | | | Tableau integration |
| Avg Stake | Average wager size | 🟡 | | | |
| Sessions Per Week | Avg sessions per week per player | 🟡 | | | |

---

## 4. Agentforce NBA Queue

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| NBA Action Type | Category of recommended action (Retention / Upsell / Reactivation / Loyalty / Check-In / RG Signal) | 🔵 | | | Agentforce classification |
| NBA Priority Score | AI-assigned urgency/priority | 🔵 | | | |
| NBA Trigger Reason | Why this action was recommended | 🔵 | | | |
| NBA Recommended Message | Draft outreach message from AI | 🔵 | | | |
| NBA Contact Window | Suggested timeframe to act | 🔵 | | | |
| Player GGR 30d | Used as input signal for NBA | 🟡 | | | |
| Player Session Count | Recent session frequency | 🟡 | | | |
| Days Since Last Contact | Used as input signal | ✅ | | | |
| RG Signal Flag | Responsible gambling risk indicator | ⚠️ | | | May sit with compliance/RG team separately |

---

## 5. Real-Time Awareness Command Center

### Big Swings Tab
| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Single-Session Win/Loss Amount | Large win or loss in a single session | 🟡 | | | Real-time or near-real-time |
| Sessions Count (recent) | Number of sessions in recent window | 🟡 | | | |
| % Change vs Avg Stake | Current vs historical stake average | 🟡 | | | |
| Welfare Check Flag | Triggered when loss threshold exceeded | ⚠️ | | | RG policy-dependent threshold |

### Payment Alerts Tab
| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Withdrawal Request Amount | Dollar amount of pending withdrawal | 🟡 | | | |
| Withdrawal Status | Pending / Declined / Approved | 🟡 | | | |
| Withdrawal Decline Count | Number of consecutive declines | 🟡 | | | |
| Escalation SLA | Required response time for escalations | ⚠️ | | | Operational policy, may be manual |

### Losing Streaks Tab
| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Consecutive Loss Sessions | Number of consecutive losing sessions | 🟡 | | | |
| Net Loss (period) | Total loss over defined period | 🟡 | | | |
| Wager History (detail) | Session-by-session breakdown | 🟡 | | | |

---

## 6. WPC (Weekly Priority Contacts) Tracker

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| WPC List | Weekly list of VIPs requiring contact | ⚠️ | | | How is this generated? Agentforce? Manual? |
| WPC Status per VIP | Pending / In Progress / Complete | ✅ | | | Likely Task/Activity in Salesforce |
| WPC Due Date | Deadline for weekly contact | ⚠️ | | | |
| Last Contact Date | Pulls from VIP master record | ✅ | | | |
| Contact Log Entry | Notes from completed contact | ✅ | | | Activity / Task Notes field |

---

## 7. Onboarding & Offboarding Tracker

### New VIPs Tab
| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Onboarding Start Date | When 6-week window began | ✅ | | | |
| Onboarding Week Number | Current week in onboarding (Wk 1–6) | ✅ | | | Derived from start date |
| KYC Status | Know Your Customer verification status | ✅ | | | |
| Welcome Survey Sent Flag | Whether survey has been sent | ⚠️ | | | Surveying platform? |
| Survey Response Data | Player responses | ⚠️ | | | |

### Churn / Demotion Tab
| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Churn Risk Score | High / Medium / Low risk of churning | 🔵 | | | Agentforce or predictive model |
| Churn Risk Signals | Reasons driving the risk flag | 🔵 | | | |
| Last Deposit Date | Most recent deposit | 🟡 | | | |
| Deposit Frequency Change | Change in deposit cadence | 🟡 | | | |
| Demotion Reason (selected) | KAM-selected reason when initiating demotion | ✅ | | | Picklist field |
| Demotion Workflow Status | Whether demotion process has been initiated | ✅ | | | |

---

## 8. Last Contact Tracker (20-Day Cadence)

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Days Since Last Contact | Calculated from last Activity date | ✅ | | | |
| Contact Cadence Status | In Cadence / Due Soon / Overdue | ✅ | | | Derived: ≤14d / 15–19d / 20d+ |
| Contact Type (last) | Call / Email / In-Person / Event | ✅ | | | Activity Type field |
| Contact Log Notes | Summary of what was discussed | ✅ | | | |
| Contact Outcome | Result of contact (e.g. engaged, no response) | ✅ | | | |

---

## 9. Upcoming Events

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Event Name | Name of the event | ⚠️ | | | Where is the event catalog maintained? |
| Event Date | Date of event | ⚠️ | | | |
| Event Venue | Venue name and location | ⚠️ | | | |
| Ticket Inventory | Number of tickets available for VIP allocation | ⚠️ | | | |
| Tickets Allocated (per VIP) | How many tickets assigned to this player | ⚠️ | | | |
| Invite Status per VIP | Not Invited / Invited / Accepted / Declined | ⚠️ | | | |
| Event Type | Sports / Concert / Hospitality / etc. | ⚠️ | | | |

---

## 10. Campaign Manager

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Campaign Name | Name of the promotion/campaign | ✅ | | | |
| Campaign Status | Draft / Active / Completed | ✅ | | | |
| Campaign Type | Bonus Bet / Reload / Event / Loyalty | ✅ | | | |
| Campaign Target (VIP count) | How many VIPs are in the campaign | ✅ | | | |
| Campaign Send Date | When campaign activates | ✅ | | | |
| Campaign Offer Amount | Dollar value of the offer | ✅ | | | |
| Approval Status | Pre-approved / Awaiting sign-off / In commercial review | ✅ | | | Approval workflow |
| Campaign Performance | Open rate / redemption rate / GGR lift | 🟡 | | | Post-send analytics |

---

## 11. Birthdays This Week

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Date of Birth | Player's birthday | ✅ | | | |
| Birthday This Week Flag | Derived: DOB falls in current week | ✅ | | | |
| Birthday Offer Sent Flag | Whether a birthday bonus/message was sent | ✅ | | | |
| Birthday Offer Amount | Dollar value of birthday offer | ✅ | | | |
| Birthday Offer Type | Profit Boost / Bonus Credit / Gift | ✅ | | | |

---

## 12. VIP Feedback Pulse

| Field | Description | Confidence | Confirmed Source | Accessible Today? | Notes |
|---|---|---|---|---|---|
| Feedback Text | Player's free-text feedback | 🔴 | | | Requires survey/feedback capture mechanism |
| Feedback Submitted Date | When feedback was submitted | 🔴 | | | |
| Feedback Sentiment | Positive / Neutral / Negative (AI-classified) | 🔵 | | | Agentforce Einstein sentiment |
| Feedback Review Status | Unreviewed / Reviewed | ✅ | | | Simple flag, KAM-set |
| KAM Response | KAM's written reply to feedback | ✅ | | | |
| Response Sent Date | When KAM responded | ✅ | | | |
| Feedback Channel | In-app / Survey / Direct / etc. | 🔴 | | | |

---

## Open Questions for FanDuel Team

1. **DataPoints scope**: What data does DataPoints actually surface today — is it GGR, sessions, wager history, or something broader?
2. **Real-time vs. batch**: Which fields are available in real-time vs. daily batch updates? (Critical for RTA, NBA signals, and Last Contact Tracker)
3. **Event catalog**: Where does the event inventory live? Is there an API or is this managed manually?
4. **WPC list generation**: Is the Weekly Priority Contact list Agentforce-generated, manager-assigned, or rule-based?
5. **RG thresholds**: Who owns responsible gambling threshold definitions — compliance team, product, or is it configurable by the KAM?
6. **TVG / FDR Racing data**: Is racing player data accessible via the same data platform as SBK/CAS/DFS, or is it a separate integration?
7. **VIP Feedback Pulse mechanism**: Is there an existing feedback capture tool, or does this require building a new survey/feedback channel?
8. **Churn risk model**: Does a predictive churn model already exist, or would this need to be built in Agentforce?

---

*Document version: 1.0 — generated from prototype UI (index.html + Figma component screens)*
*To be reviewed and validated by: FanDuel VIP Technology team + Data/Platform team*
