INSTRUCTIONS FOR GEMINI:
Create a Google Doc in my Drive titled "VIP KAM Console — User Intent Cards". Format with Heading 1 for the title, Heading 2 for each module, Heading 3 for each card title. Bold each field label (Persona, I want to, So that, Context, Current Pain, Success Signal). Add a table of contents after the title. Insert a horizontal rule between each card. Copy every card below verbatim — do not skip or summarize any content. Return the Google Doc link when done.

===========================================================
VIP KAM Console — User Intent Cards
FanDuel VIP KAM Console · Salesforce Sales Console Custom Home Page
Version 1.0 · Aug 2026
===========================================================

ABOUT THIS DOCUMENT

User Intent Cards capture the "I want to… so that…" behind each feature area. They are lighter than user stories (no acceptance criteria) and are designed to drive alignment in UX workshops, prioritization sessions, and stakeholder reviews.

Personas:
• Taylor — Sportsbook KAM (primary daily user)
• Morgan — Casino KAM (primary daily user)
• Casey — DFS KAM (strategic, smaller book)
• Reese — Racing KAM (sparse data, dual systems: TVG + FanDuel Racing)
• Avery — KAM Manager (oversees team)
• Pat — KAM Director (division ops, reporting-focused)

===========================================================
MODULE 1: VIP PLAYBOOK / MY VIPs
===========================================================

Card 01 — See My Full Book at a Glance

Persona: Taylor (SBK KAM)
I want to: See my entire book of business in one view — names, segments, tiers, GGR, last contact, and status — at a glance.
So that: I can quickly orient myself at the start of each day without opening multiple Salesforce records or spreadsheets.
Context: Taylor checks the console every morning before standup. The book has 353 VIPs across multiple tiers. Today this requires switching between reports, list views, and Excel exports.
Current Pain: No single unified view exists. KAMs piece together data from 3–4 sources. High cognitive load before any action is taken.
Success Signal: Taylor can see the full book, sorted by priority, within 5 seconds of opening the console.

---

Card 02 — Surface Overdue Contacts

Persona: Taylor (SBK KAM)
I want to: Immediately see which VIPs are overdue for contact — with clear visual urgency indicators.
So that: I don't accidentally miss a high-value VIP who hasn't been contacted in 30+ days.
Context: FanDuel has informal SLAs around contact frequency — Shield Select VIPs every 10–14 days, lower tiers every 20–30 days. Not enforced by any system today.
Current Pain: No automated tracking. KAMs rely on memory or personal spreadsheets. High-value VIPs fall through the cracks, leading to churn.
Success Signal: Overdue VIPs (>35 days) surface in red at the top of the list or in a "Contact Due" widget without any filtering required.

---

Card 03 — Filter Book by Cohort

Persona: Morgan (CAS KAM)
I want to: Filter my book by segment, tier, and status so I can focus on a specific cohort.
So that: I can plan batch outreach (e.g., "all Tier 3 Casino VIPs not contacted in 20+ days") efficiently.
Context: Morgan manages a Casino-heavy book spanning Tier 5 through Shield Select. Different tiers require different outreach strategies and bonus budgets.
Current Pain: Filtering requires building a Salesforce list view from scratch each time. Casino tier numbering (5=entry, 1=top) is counterintuitive in standard Salesforce.
Success Signal: Morgan can apply a filter in under 30 seconds and see a scoped list with all relevant columns.

---

Card 04 — Quick VIP Context Before Outreach

Persona: Taylor (SBK KAM)
I want to: Click on a VIP's name and immediately see their profile — segment, tier, GGR, last bonus, birthday, recent activity — without leaving the console.
So that: I can prepare for a call or message in context, in under 30 seconds, without navigating away.
Context: Before reaching out, Taylor wants to know: what did I last say? Have they won or lost recently? Did we already issue a bonus? This requires opening multiple related records today.
Current Pain: Opening a VIP record and navigating related lists takes 3–5 clicks and multiple page loads. KAMs often skip prep because it takes too long.
Success Signal: VIP Contact Card modal opens in under 1 second and surfaces all relevant context in a single panel.

---

Card 05 — Manager Team Overview

Persona: Avery (KAM Manager)
I want to: See a rolled-up view of my team's books — total VIPs, contact compliance, GGR performance, bonus spend — across all KAMs.
So that: I can identify which KAMs are behind on contact frequency and where attention needs to go this week.
Context: Avery manages 4–6 KAMs and holds a weekly team review. Currently pulls data from multiple reports and compiles manually in Excel.
Current Pain: No manager view exists. Avery cannot see contact compliance or bonus spend across KAMs without building custom reports.
Success Signal: Avery has a dedicated role-default view showing team-level metrics with drill-down to individual KAM books.

===========================================================
MODULE 2: AGENTFORCE NEXT BEST ACTIONS
===========================================================

Card 06 — Daily AI-Prioritized Outreach List

Persona: Taylor (SBK KAM)
I want to: Start each day with an AI-generated, prioritized list of the specific VIPs I should contact today and why.
So that: I don't have to manually decide who's most urgent — I can trust the system to surface the right names based on GGR trends, contact history, and lifecycle signals.
Context: Taylor manages 353 VIPs. Without AI prioritization, deciding who to contact is manual, time-consuming, and subjective.
Current Pain: No prioritization system. KAMs default to whoever they remember. High-value at-risk VIPs are often missed.
Success Signal: Taylor works through the NBA queue each morning in under 20 minutes, confident no high-priority VIP was skipped.

---

Card 07 — Understand AI Reasoning

Persona: Taylor (SBK KAM)
I want to: Understand the AI's reasoning for flagging a VIP — not just who, but why.
So that: I can make an informed decision about whether to follow, adjust, or dismiss the recommendation.
Context: Taylor won't blindly follow AI suggestions without understanding the logic. KAMs have relationship context the AI doesn't. They need the signal (e.g., "GGR down 40%, last contact 28d ago").
Current Pain: Tools that use AI don't expose reasoning. KAMs distrust black-box suggestions.
Success Signal: Each NBA card shows a one-line AI rationale. Taylor can scan it in 3 seconds and decide to act or dismiss with confidence.

---

Card 08 — Log Contact Without Friction

Persona: Taylor (SBK KAM)
I want to: Log a contact quickly — right from the NBA card or VIP Contact Card — without switching to a different Salesforce record.
So that: Activity logging doesn't feel like overhead and gets done immediately after each interaction.
Context: Salesforce requires navigating to the VIP record, finding Activities, and manually creating a Task/Event. This friction means many contacts go unlogged, corrupting "Last Contact" data.
Current Pain: Contact logging takes 5+ clicks. Most KAMs batch-log at end of day or skip it entirely.
Success Signal: Contact log is completable in 2 clicks from the console. Last Contact date updates immediately in the table.

---

Card 09 — Issue Bonus In-Flow

Persona: Taylor (SBK KAM)
I want to: Issue a bonus to a VIP from within the console — selecting type (Bonus Bet, Postal Gift, Event), amount, and triggering a compliance check — in one flow.
So that: Bonus issuance is fast, auditable, and doesn't require switching to a separate system or emailing ops.
Context: Currently bonus issuance involves a workflow outside Salesforce. KAMs email a request, ops processes it, no real-time status.
Current Pain: Slow, disconnected process. No visibility into bonus budget or compliance status at point of issuance.
Success Signal: Taylor issues a bonus in under 60 seconds. Compliance check runs automatically and either approves or flags for review.

---

Card 10 — Bulk Post-Event Actions

Persona: Morgan (CAS KAM)
I want to: Log contact or issue a bonus for multiple VIPs at once after a Casino event or promotion.
So that: Post-event follow-up doesn't require individually opening 20 VIP records.
Context: Casino KAMs frequently run group outreach after live events, jackpot hits, or tier upgrades. Batch actions are critical for efficiency.
Current Pain: No bulk action capability in Salesforce for custom VIP records. Morgan spends hours logging contacts individually after events.
Success Signal: Morgan selects 20 VIPs, chooses "Bulk Contact Log" or "Bulk Bonus Issue," and completes the batch in under 5 minutes.

===========================================================
MODULE 3: AWARENESS / ALERTS
===========================================================

Card 11 — Big Swing Alerts

Persona: Taylor (SBK KAM)
I want to: Be notified when a VIP places an unusually large bet or parlay.
So that: I can reach out proactively while the experience is top of mind — reinforcing the relationship at a high-emotion moment.
Context: A VIP placing a $2,400 parlay on NFL Sunday is an engagement opportunity. KAMs who reach out in the moment see higher satisfaction scores.
Current Pain: No real-time signal. Taylor finds out about big bets retroactively, if at all.
Success Signal: Taylor receives an alert within 30 minutes of the bet and can tap "Contact" directly from the alert card.

---

Card 12 — Payment Alerts

Persona: Taylor (SBK KAM)
I want to: See payment alerts — pending withdrawals, failed deposits, disputed charges — for VIPs in my book.
So that: I can proactively reach out before the VIP contacts support, showing white-glove service.
Context: VIPs with payment friction are at high churn risk. A KAM who resolves a $8,500 withdrawal before the VIP has to ask creates a loyalty-defining moment.
Current Pain: Payment alerts go to support ops, not KAMs. By the time a KAM is aware, the VIP is already frustrated.
Success Signal: KAMs see payment alerts in real-time (within 1 hour) with a direct action path.

---

Card 13 — Losing Streak Welfare Checks

Persona: Morgan (CAS KAM)
I want to: Identify Casino VIPs on a significant losing streak so I can do a welfare check and offer support.
So that: FanDuel meets its responsible gambling obligations while maintaining the VIP relationship through a difficult moment.
Context: Casino KAMs have a dual responsibility: commercial (maintain engagement) and welfare (don't over-bonus someone on a bad run). Losing streak signals trigger RG-aware outreach.
Current Pain: No losing streak data in the KAM view. Morgan relies on intuition or hearing from the VIP directly.
Success Signal: VIPs with 5+ consecutive losing sessions surface as a "Losing Streak" alert with streak length and GGR context visible before deciding to contact.

===========================================================
MODULE 4: REPORTING
===========================================================

Card 14 — Self-Serve GGR Trends

Persona: Taylor (SBK KAM)
I want to: See how my book's total GGR is trending week-over-week and month-over-month.
So that: I can self-assess my book's health without waiting for a monthly report from the analytics team.
Context: Taylor's performance is partially evaluated on GGR contribution. Real-time visibility removes surprises at review time.
Current Pain: GGR reports are published monthly by the analytics team. No self-serve access between cycles.
Success Signal: Taylor can see GGR trend charts for their book, updated daily, directly in the console.

---

Card 15 — Director Team Performance View

Persona: Pat (KAM Director)
I want to: Compare KAM-level performance metrics — GGR contribution, contact frequency, bonus spend, churn rate — across the team.
So that: I can allocate resources, set targets, and identify coaching opportunities based on data.
Context: Pat oversees the full KAM division. Currently receives manually compiled Excel reports from Avery monthly. No drill-down capability.
Current Pain: Monthly cadence on data that should be weekly. No ability to slice by segment, region, or tier.
Success Signal: Pat has a director-level Reports view with KAM comparison tables, filterable by segment and date range.

---

Card 16 — Live WPC Tracking

Persona: Avery (KAM Manager)
I want to: Track my team's Weekly Priority Contacts (WPC) completion in real-time.
So that: I can intervene mid-week if a KAM is behind, rather than discovering the miss after the deadline.
Context: WPC is a weekly KPI — each KAM has a contact target (e.g., 15 VIPs per week). Avery currently checks compliance by reviewing Activity logs on Friday.
Current Pain: No live WPC dashboard. Avery only knows about misses after the fact.
Success Signal: Avery sees a per-KAM WPC progress bar updated daily. Red/amber/green status visible by Wednesday.

===========================================================
MODULE 5: ONBOARDING & LIFECYCLE
===========================================================

Card 17 — New VIP Assignment Notification

Persona: Taylor (SBK KAM)
I want to: Be notified when a new VIP is assigned to my book — with their profile, first-contact checklist, and a suggested welcome outreach.
So that: New VIPs receive a warm welcome within 24 hours of being tiered, setting the tone for the relationship.
Context: VIP onboarding is a critical loyalty moment. A KAM who reaches out quickly with context makes a strong first impression.
Current Pain: KAMs often don't know a new VIP was added until they happen to check the list. No onboarding checklist or suggested first message.
Success Signal: Taylor receives an in-console notification within 1 hour of assignment. Clicking it opens a "New VIP Welcome" flow with pre-populated context.

---

Card 18 — Churn Risk Surfacing

Persona: Taylor (SBK KAM)
I want to: See which VIPs are at risk of churning or being demoted — before it happens.
So that: I can take retention action while there's still time to change the trajectory.
Context: A drop from Shield Select to Tier 3 represents significant LTV loss. Predictive signals (declining GGR, reduced sessions, no response to last 3 contacts) should surface proactively.
Current Pain: No churn prediction. KAMs notice the drop after demotion has already happened.
Success Signal: A "Churn Risk" cohort surfaces in the console with a risk score and the primary driving signal (e.g., "GGR down 60% over 60d").

===========================================================
NEXT STEPS
===========================================================

• Review with FanDuel stakeholders for missing intents
• Prioritize using MoSCoW framework (see Requirements doc)
• Map each card to a User Story in the User Stories sheet
• Confirm data availability for real-time signal cards (11, 12, 13) with tech architect
