# FanDuel External Agent — Discovery Brief
**Prepared by: Ren (AI Experience Architect) | Oct 2026 | Sessions: Oct 14 (in person) + Oct 16 (virtual)**

---

## What we bring to these sessions

We've completed a full design and experience spec for this agent (Sep 2026) based on FanDuel's traffic report, chat rate analysis, article categories, and the Help Portal advisory assessment. We are not starting from zero — several of the discovery questions are already answered.

This brief organizes what we know, what we need from FanDuel, and where the gaps are.

---

## Top 3 gaps to resolve before building anything

| # | Gap | Why it blocks |
|---|---|---|
| 1 | **Same-agent or subagent?** If the FanDuel app opens a support agent, is it the same agent definition as the Help Portal, or a separate deployment? | This is the single biggest architectural decision. It determines hosting, training scope, and content strategy. Nothing can be scoped accurately until this is answered. |
| 2 | **Article rewrites for Tier 1 intents** | The agent's answer quality depends directly on the article format. Narrative paragraphs produce imprecise answers. Rewrites are a prerequisite — not a parallel workstream. If this starts late, the go-live date slips. |
| 3 | **Compliance/RG session not scheduled** | For a sports betting operator, compliance guardrails gate what the agent can do. This session needs to happen before or alongside the Actions & Integrations session — not after. |

---

## Session-by-session: What we know vs. what we need

### Session 1 — Scope & Business Goals
**Our answers (bring these in):**
- Agent handles 3 tiers: Full self-service (Tier 1) · Guided + possible handoff (Tier 2) · Necessary Transfer to human (Tier 3)
- Tier 1 confirmed: password reset, 2FA, ID verification, deposit status, location check
- Tier 2 confirmed: withdrawal status, failed transactions, bet disputes, promo eligibility
- Tier 3 (not deflection targets — route only): account locked, fraud, RG escalation
- Right metric is **containment rate**, not raw deflection. Tier 3 contacts handled by humans are not failures.

**Need from FanDuel:**
- Operating hours — is after-hours window 3–7am?
- Peak volume windows (game days, Super Bowl, major events)?
- Containment rate target (need baseline first — what's current chatbot containment?)
- Phase 1 priority ranking within Tier 1 intents

---

### Session 2 — Current-State Support Model
**Our answers:**
- 48.5% chat rate confirmed from article analysis — this is the primary driver
- Top contact reasons: password/login, ID verification, 2FA, withdrawal, location
- Location Troubleshooting is the model to replicate — step-based with deeplink, it works
- Necessary Transfer articles (Account Locked, Contact Us) need expectation-setting format, not resolution attempts

**Need from FanDuel:**
- After-hours staffing model and current vendor coverage
- Can the current chatbot pass conversation context to a human on handoff? (Critical — if not, it's a technical prerequisite before launch)
- Which flows to keep vs. redesign?

---

### Session 3 — Channels & Customer Experience
**Our answers:**
- Primary deployment: EC Help Portal (homepage + article pages)
- 82% of users arrive from a FanDuel app · 91% mobile — mobile-first design is required, not optional
- Context at launch is defined: from app = personalised + product pre-loaded; authenticated only = name; unauthenticated = generic
- Tone: direct, plain English, warm but efficient. Users in support are often frustrated — brevity over polish.
- When the agent can't help: acknowledge cleanly, offer escalation before ending session — never loop the user

**Need from FanDuel:**
- **Same-agent or subagent?** (See Gap #1 above — this is the biggest open question)
- Web widget — same agent or lighter variant?
- Agent name — branded name or "FanDuel Help Agent"?
- Languages beyond English US?

---

### Session 4 — Escalation & Human Handoff
**Our answers:**
- 5 escalation triggers defined: account locked/suspended · fraud/unauthorized access · active RG flag · 3 failed resolution attempts on same intent · user explicitly requests human
- Escalation experience: acknowledge without blame → realistic wait expectation → full context passed to human
- Context must pass to the human agent — customer never repeats themselves
- For Tier 3: agent sets expectations upfront before routing ("Have your ID ready")

**Need from FanDuel:**
- Who handles after-hours escalations, and for which case types?
- Omni-Channel handoff or callback/follow-up flow?
- SLA expectations by case type?

---

### Session 5 — Data & Knowledge
**Our answers:**
- EC Knowledge articles are the single source of truth — no parallel knowledge base assumed
- Format requirements for agent consumption: numbered steps, decision branches, inline deeplinks, H2/H3 headings. Narrative paragraphs = imprecise agent answers.
- Monthly SME review cycle confirmed (2026 waves documented)
- Customer data needed at session start: user_id · account_status · product · last_page

**Need from FanDuel:**
- Who owns each article category and approves rewrites?
- How frequently do promo/bonus T&C articles change? (These need a faster review trigger)
- Is Amplitude integrated? Trending topics (Phase 2) and intent frequency analysis depend on it.
- Where does account/wallet data live — Salesforce, billing system, or both?

---

### Session 6 — Actions & Integrations
**Our answers:**
- Context parameters at launch defined: product · user_id · account_status · last_page
- RG flag lookup is required at session start — hard gate before any intent resolution
- Deeplinks into app flows (reset, verify, etc.) are required for Tier 1 — not optional
- Phase 1 = read-only actions. Transactional (case create, account update) = Phase 2+

**Need from FanDuel:**
- Who owns the support entry point in each app (SBK / Casino / DFS / Racing)? (No owner = context parameters never get built)
- Which APIs are available and owned?
- Authentication approach — session token passthrough or re-auth?
- Integration team constraints and availability?

---

### Session 7 — Compliance, Risk & Guardrails
> ⚠️ **No date set. Needs to be scheduled before Session 6 — compliance guardrails gate what actions are even possible.**

**Our answers:**
- RG gate is a hard rule — non-configurable, no commercial override
- When RG flag active: RG resources only · no bonuses/promos · no retention attempt · route and close
- Immediate escalation for: account locked · fraud/unauthorized access · regulatory complaints

**Need from FanDuel:**
- **State-specific RG routing:** which states require specific helpline numbers? Which have different self-exclusion, cooling-off, or deposit limit rules the agent must encode per state?
- Full approved list of topics agent must never discuss or always escalate
- Legal/compliance review and approval chain
- Audit, logging, and data retention requirements — what gets stored, for how long, per which state rules?
- PII handling for conversation transcripts

---

### Sessions 8–11 — Platform · Testing · Rollout · Reporting
**Mostly open — key input needed:**
- Agentforce org status, licensing, sandbox strategy
- Same-agent/subagent decision (gates hosting path — see Gap #1)
- Test approach: real transcripts, red-teaming, UAT owners
- Go-live criteria

> **Code freeze windows — factor into rollout:**
> - Thanksgiving: Nov 23–27, 2026
> - Holiday: Dec 18, 2026 – Jan 4, 2027
> - Super Bowl: Feb 9–15, 2027

---

## Gap: Conversation Design session (not on the current schedule)

**This is the most common point of failure for agent projects.** Brand voice and tone are addressed briefly in Session 3, but conversation design is a separate discipline — it determines what the customer actually experiences when the agent works, when it struggles, and when it fails. It needs its own session.

### Proposed session outline

**Attendees:** CX · Brand · Support Ops · Content

**Questions to answer:**

#### Agent identity
- What is the agent's name?
- Does it have a visual identity (avatar, icon) or is it text-only?
- Is the persona consistent across Help Portal and in-app, or does it adapt?

#### Conversation patterns
- What does the agent say in the zero state (first message before user types)?
- What are the 5–6 conversation starter labels? (We have a draft — validate against live chat data)
- How does the agent confirm it understood the user's intent before answering?

#### Failure states
- What does the agent say when it genuinely can't help? (Must be specific — "I can't help with that" + clear next step)
- What does it say when a knowledge lookup fails or returns no result?
- What does it say when the user's question is ambiguous?
- What happens after 3 failed attempts on the same intent?

#### Emotionally charged contacts
- How does the agent respond to a user who is clearly frustrated or upset?
- How does the agent handle loss-chasing signals or distressed language — before the RG gate fires?
- What is the approved language for the escalation handoff message? (Must feel human, not robotic)

#### Promo and bonus edge cases
- What does the agent say when a user asks about a promo the agent has no data on?
- What does it say when a promo T&C article is outdated or being rewritten?

#### Ending the conversation
- How does the agent close a resolved session?
- How does it close an unresolved session where escalation was declined?

---

## Recommended additions to the discovery plan

| Action | Why |
|---|---|
| Schedule Session 7 (Compliance/RG) before Session 6 (Actions) | Compliance guardrails must be defined before actions scope is finalized |
| Add Conversation Design session (CX / Brand / Support Ops) | Not currently on the plan — failure mode for most agent launches |
| Confirm same-agent/subagent in Session 8 before any other scoping | Every other scope decision depends on this answer |
| Map code freeze windows against proposed go-live | Super Bowl freeze (Feb 9–15) likely conflicts with any Jan build completion |

---

*Derived from: FAQ External Agent Spec (Sep 2026) · faq-external-agent-spec.md*
*See also: external-agent-discovery-prefill.html for session-by-session reference grid*
