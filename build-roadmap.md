# FanDuel VIP KAM Console — Build Roadmap

**Purpose:** Define what we build, in what order, and on what clock. Used for sprint planning, resourcing, and stakeholder alignment.

**Operating model (v1.1):** 4-week increments with a **Friday POC demo every week**. Capability phases (1–4) are the product backlog — not the calendar. Only Increment 1 is committed to a 4-week clock.

**Demo labels — use on every screen, every Friday:**
- **Mock** — static/seeded UI, no live org data
- **Seeded CRM** — sandbox records that look real, not production
- **Live** — reading/writing real Salesforce objects
- **Blocked** — cannot go live until a named Track B item lands

**LOE Key:**
- S = Small (1–2 sprints / ~1–2 weeks)
- M = Medium (2–4 sprints / ~2–4 weeks)
- L = Large (4–6 sprints / ~4–6 weeks)
- XL = Extra Large (6+ sprints / 6+ weeks, requires dedicated track)

---

## Two tracks (run in parallel)

Weekly POCs only work if integration unblocking is a separate track. Mixing them produces prettier mocks and no live data.

| Track | Cadence | What it is | Success |
|---|---|---|---|
| **A — Experience POC** | Friday demo. 4-week packages. | LWC, SLDS, flows, seeded data. Owned by the console build pair. | A KAM can click it. |
| **B — Integration readiness** | Not weekly-demoable. Decision and access work. | DataPoints API, Agentforce NBA use cases with Andy, event catalog source, Tableau method. | A signed contract, not a screenshot. |

---

## Committed calendar

A 2–3 engineer Salesforce team typically lands ~15–20 story points per week. Hitting Phase 1 in 4 weeks (~31 pts/week) is a **demo-complete sandbox**, not production-hardened LWC. It assumes the existing prototype, mock data only, AI-paired build, and almost no design churn.

### Increment 1 — weeks 1–4 (committed)

**Scope:** Phase 1 mock console. No DataPoints. No Agentforce. Demo is the product.

**Gate:** A KAM can click the console in sandbox and say the workflow is right.

| Week | Friday POC | Demo label |
|---|---|---|
| 1 | SLDS shell, nav, My VIPs table with mock rows | Mock |
| 2 | Search, filters, row actions, Log Contact modal | Mock |
| 3 | Last Contact, Birthdays, WPC — still mock | Mock |
| 4 | Contact card + visual indicators. Stakeholder gate. | Mock |

**If this is still open at kickoff:** Salesforce sandbox + SLDS/LWC access not day 1 → Week 1 is a Figma click-through, not a POC.

### Increment 2 — weeks 5–8 (conditional)

**Only if** Salesforce CRM objects (Contact, Activity, Task) are mapped and writable in the sandbox.

**Ship live (~46 pts, Low-risk Phase 2):**
- VIP Master Record (name, segment, tier, state, DOB, KAM)
- Last Contact from real Activity dates
- Log Contact writes Activity
- Birthdays from real DOB
- Onboarding tracker from real VIP records *(if objects exist)*

**Keep mock (High-risk Phase 2 — Track B):**
- GGR L30 coloring
- Overbonused / bonus cap
- NBA amber triangle
- Overview GGR by segment

**Do not call Increment 2 “live core data” if DataPoints access is still unconfirmed.**

### After week 8

Phases 3–4 stay off the 4-week calendar until Track B has **named owners and dates** for:
- DataPoints API access + field validation
- Agentforce NBA use cases (Andy Geissler / FanDuel VIP Technology)
- Event catalog source
- Tableau connection method

Until then, Friday demos can still show Phase 3/4 **UI** — labeled Mock/Blocked. Einstein is not in the loop. Static amber triangles are not NBA.

---

## Capability map (backlog, not calendar)

Four capability phases. Business value still stacks in this order. Duration below is production-hardening effort, not the Increment 1 demo clock.

| Capability | Theme | Story Points | LOE | Production effort | Lands on calendar when |
|---|---|---|---|---|---|
| **Phase 1** | Foundation (Mock-Ready) | ~126 | L | 6–8 weeks if hardened | **Increment 1** (4 weeks as demo-complete) |
| **Phase 2** | Live Core Data | ~103 | L–XL | 6–10 weeks | Increment 2 = CRM-only; DataPoints after Track B |
| **Phase 3** | Agentforce Intelligence | ~115 | XL | 8–12 weeks | Track B: Agentforce config + real-time feeds |
| **Phase 4** | Full Operations | ~151 | XL | 8–12 weeks | Track B: event catalog + marketing + Tableau |
| **Total** | | **~495 story points** | | **~28–42 weeks to full production** | Not 16 weeks of 4-week boxes |

---

## Phase 1 — Foundation (Mock-Ready)

**Goal:** Working Salesforce component in sandbox, all UI states implemented, seeded with mock data. Sufficient for stakeholder demos and UX validation.
**Dependency:** Salesforce org access + SLDS/LWC setup. No external data integrations required.
**Calendar:** Increment 1 (4 weeks, demo-complete). Production-hardened: 6–8 weeks.

| Component | Key Features | LOE | Story Points | Data Source | Blocker Risk |
|---|---|---|---|---|---|
| **SLDS Shell + Nav** | Global nav bar, app launcher, notification panel, settings panel | S | 13 | Static | None |
| **VIP Playbook — Table** | My VIPs table, columns, sorting, pagination, loading/empty states | M | 29 | Mock VIP records | None |
| **VIP Playbook — Search & Filter** | Search input, SLDS list filters | S | 10 | Mock | None |
| **VIP Playbook — Row Actions** | Hover actions, Log Contact modal, single/multi-row select, bulk bar | M | 21 | Mock | None |
| **VIP Playbook — Status & Visual Indicators** | GGR coloring, Last Contact urgency, status pills, NBA amber triangle (static) | S | 8 | Mock | None |
| **Last Contact Tracker** | Cadence list, urgency coloring, Log Contact modal | S | 13 | Mock | None |
| **Birthdays This Week** | Birthday list, Send Celebration flow | S | 9 | Mock (DOB from record) | None |
| **WPC Tracker — Basic** | WPC list, mark complete, filter tabs | S | 15 | Mock | None |
| **VIP Contact Card Modal** | Full VIP detail modal, stats, history | S | 8 | Mock | None |

**Phase 1 Total: ~126 story points | LOE: L | Increment 1 = 4 weeks demo-complete | 6–8 weeks if production-hardened**

---

## Phase 2 — Live Core Data

**Goal:** Replace mock data with live Salesforce CRM + DataPoints feeds. KAMs can use the console for real work.
**Dependency:** DataPoints API access confirmed + field mapping validated (see `data-points-validation.md`). Salesforce CRM objects mapped (Contact, Activity, Task, Opportunity).
**Calendar:** Increment 2 (weeks 5–8) ships **Low-risk CRM only**. High-risk DataPoints rows stay Mock/Blocked until Track B confirms access.

| Component | Key Features | LOE | Story Points | Data Source | Blocker Risk | Increment |
|---|---|---|---|---|---|---|
| **VIP Master Record integration** | Live Contact data: name, segment, tier, state, DOB, KAM assignment | S | 8 | Salesforce CRM | Low — standard objects | 2 — Live |
| **Last Contact — Live Activities** | Cadence status from real Activity object dates | S | 8 | Salesforce CRM | Low | 2 — Live |
| **Contact Logging — Live Write** | Log Contact writes real Activity records to Salesforce | M | 13 | Salesforce CRM | Low | 2 — Live |
| **Onboarding Tracker — Live** | Real new VIP records, KYC status, onboarding start date | S | 13 | Salesforce CRM | Low | 2 — Live if objects exist |
| **Birthdays — Live DOB** | Birthday detection from real Contact DOB field | S | 4 | Salesforce CRM | Low | 2 — Live |
| **Bonus Data** | Last Bonus Issued: amount, type, date from real records | M | 13 | Salesforce CRM / DataPoints | Medium — bonus object mapping TBD | 2 — Live if CRM; else Mock |
| **WPC — Live Task Integration** | WPC list driven by real Tasks, completion writes back | M | 15 | Salesforce CRM | Medium — WPC generation logic TBD | 2 — Live if logic defined |
| **Overview Metrics — Live** | Total VIPs, Active count, promotions/demotions from real data | S | 8 | Salesforce CRM + DataPoints | Medium | 2 — CRM counts Live; GGR Mock |
| **GGR L30 + Session Data** | Live GGR coloring, sortable, accurate values | M | 13 | DataPoints | **High — DataPoints access must be confirmed** | Blocked — Track B |
| **Overbonused Flag** | Live bonus cap + overbonused detection | M | 8 | DataPoints | **High — cap data source unconfirmed** | Blocked — Track B |

**Phase 2 Total: ~103 story points | LOE: L–XL | Increment 2 ≈ 46 pts CRM-live | remainder waits on DataPoints**

**Critical path: DataPoints API access + field validation is Track B. It does not start Increment 2. Do not wait on it to ship CRM-live rows.**

---

## Phase 3 — Agentforce Intelligence

**Goal:** Surface AI-generated recommendations, risk signals, and sentiment analysis throughout the console. Transforms it from a data view into an intelligent action engine.
**Dependency:** Agentforce org configuration, Einstein AI features enabled, DataPoints real-time signals available.
**Calendar:** Not on a 4-week clock until Track B has named owners and dates. Friday demos of this UI stay **Mock/Blocked**.

| Component | Key Features | LOE | Story Points | Data Source | Blocker Risk |
|---|---|---|---|---|---|
| **NBA Queue — Live Recommendations** | Real Agentforce NBA actions: priority, type, recommended message, contact window | XL | 29 | Agentforce NBA | **High — requires Agentforce config + use case setup** |
| **NBA — RG Signal Actions** | Live RG flag surfacing, welfare check workflow | L | 13 | Agentforce + Compliance rules | **High — RG threshold ownership TBD** |
| **Real-Time Awareness — Big Swings** | Live large win/loss detection from DataPoints session data | L | 16 | DataPoints (real-time) | **High — real-time feed vs. batch TBD** |
| **Real-Time Awareness — Payment Alerts** | Live withdrawal status, decline detection, escalation workflow | M | 13 | Payments platform | **High — payments data source TBD** |
| **Real-Time Awareness — Losing Streaks** | Live streak detection from session data | M | 8 | DataPoints | Medium — threshold config needed |
| **Churn Risk Scoring** | Agentforce-powered churn risk on Onboarding/Offboarding tracker | L | 18 | Agentforce Einstein | **High — model training required** |
| **VIP Feedback Pulse — Sentiment** | Einstein NLP sentiment classification on feedback items | M | 13 | Agentforce Einstein | Medium — feedback capture mechanism TBD |
| **NBA Amber Triangle — Live** | Real NBA pending flag on My VIPs table rows | S | 5 | Agentforce NBA | Dependent on NBA Queue being live |

**Phase 3 Total: ~115 story points | LOE: XL | Est. 8–12 weeks after Track B unblocks**
**Critical path: Agentforce use case configuration is the longest lead item. Scope it on Track B during Increment 1 — not after Increment 2.**

---

## Phase 4 — Full Operations

**Goal:** Manager views, approval workflows, campaign management, event coordination, advanced analytics. Full production-ready console for all personas.
**Dependency:** Phases 1–3 stable. Campaign tooling and event catalog integrations confirmed.
**Calendar:** Not on a 4-week clock until Track B confirms event catalog, marketing platform, and Tableau method.

| Component | Key Features | LOE | Story Points | Data Source | Blocker Risk |
|---|---|---|---|---|---|
| **Campaign Manager — Full** | Create/activate/approve campaigns, target VIP selector, performance metrics | L | 26 | Salesforce CRM + Marketing platform | Medium — marketing platform integration TBD |
| **Upcoming Events — Full** | Event catalog, invite management, ticket assignment, RSVP tracking | L | 21 | Event catalog (source TBD) | **High — event catalog source unconfirmed** |
| **Manager Cockpit** | Team WPC progress, contact health, alert summaries, churn rates | M | 18 | Aggregated from Phases 1–3 | Low — derived from existing data |
| **KAM Director Views** | Team performance dashboards, demotion approval queue, export tools | M | 15 | Salesforce CRM + Tableau | Medium — Tableau integration method TBD |
| **Tableau Integration** | Embedded analytics: Overview, Spend Trends, Segments, Retention | L | 13 | Tableau + DataPoints | Medium — direct vs. middleware TBD |
| **VIP Feedback Pulse — Respond Flow** | KAM response channel, full feedback history, team sentiment view | M | 19 | Salesforce CRM + feedback platform | Medium — feedback platform TBD |
| **Demotion Approval Workflow** | Manager review queue, approve/reject, compliance notification | S | 13 | Salesforce Flows | Low |
| **Bulk Actions — Full** | Bulk log contact, send bonus, export | S | 13 | Salesforce CRM | Low |
| **Mobile — VIP Table** | Mobile card view, urgency signals on mobile | M | 13 | Same as desktop | Low |

**Phase 4 Total: ~151 story points | LOE: XL | Est. 8–12 weeks after Track B unblocks**

---

## Readiness Assessment — Build Blockers by Component

| Component | Earliest viable increment | Key Blocker Before Build Can Start |
|---|---|---|
| VIP Playbook (My VIPs table) | Increment 1 | None — start now |
| Last Contact Tracker | Increment 1 | None — start now |
| Birthdays This Week | Increment 1 | None — start now |
| WPC Tracker | Increment 1 (mock) / 2 (live) | WPC list generation logic needs definition |
| Onboarding & Offboarding | Increment 1 (mock) / 2 (live) | None for mock; KYC + survey platform for live |
| VIP Contact Card Modal | Increment 1 | None — start now |
| GGR / DataPoints fields | After Track B | DataPoints access confirmed + API scoped |
| Overbonused / Bonus Cap | After Track B | Bonus cap data source confirmed |
| NBA Queue | After Track B | Agentforce NBA use cases configured |
| Real-Time Awareness | After Track B | Real-time DataPoints feed vs. batch confirmed |
| Churn Risk | After Track B | Einstein churn model scoped or existing model identified |
| Campaign Manager | After Track B | Marketing/campaign platform integration defined |
| Upcoming Events | After Track B | Event catalog source defined |
| Tableau Integration | After Track B | Tableau connection method (direct vs. middleware) decided |
| VIP Feedback Pulse | After Track B | Feedback capture mechanism built or sourced |

---

## Recommended Immediate Actions

1. **Kick Increment 1 on Monday** — Salesforce sandbox + SLDS shell + LWC scaffolding. Friday 1 demo is the table. No data dependencies.
2. **Label every Friday screen** Mock / Seeded CRM / Live / Blocked. Do not let polish read as production.
3. **Share `data-points-validation.md` with FanDuel data/platform team (Track B)** — validate field sources. This unblocks GGR/overbonused, not Increment 1.
4. **Scope Agentforce NBA use cases with Andy Geissler / FanDuel VIP Technology (Track B)** — longest lead item. Start during Increment 1, not after Increment 2.
5. **Confirm event catalog source and Tableau connection method (Track B)** — required before Phase 4 gets a calendar date.

---

*Roadmap version: 1.1 — September 2026*
*Change from 1.0: recut from 6–12 week capability phases-as-calendar to 4-week increments + weekly Friday POCs. Phases 3–4 stay off the 4-week clock until Track B has owners and dates.*
*Based on: Figma component screens, prototype (index.html), user stories CSV, data-points-validation.md*
*To be reviewed by: Ren (AI Experience Architect) + FanDuel VIP Technology team*
