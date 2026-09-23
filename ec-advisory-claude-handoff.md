# Claude handoff — FanDuel Help Portal experience assessment

Use this file as the **current source of truth**. The assessment below already includes the locked updates. Do not revert to an earlier draft that recommended Casey, quoted chat-volume savings, or included a delivery roadmap.

**Author / owner:** Ren (AI Experience Architect). FanDuel Digital Care advisory + design. Check in before making further edits; offer options on anything visual.

**What this is:** An experience assessment and design/strategic recommendation. Not a commercial case, not a build plan, not a platform upsell.

---

## Locked decisions — do not undo

1. **No money, cost, or unapproved operational numbers in front of the customer.**
   - Do not use ~520k fewer chats, 31% → 22%, cost-per-chat, or any savings model.
   - Do not invent an SLA (e.g. “avg wait < 2 min”) in the UI. Wait time only if FanDuel publishes a real figure.
   - Traffic and chat **rates** from FanDuel-provided docs (5.76M visitors, 1.8M chats, 31.3%, 82% from app, 91% mobile) are experience facts and may stay.

2. **This is not a delivery plan.**
   - No phases, timelines, story points, or “Phase 1 scoped and contracted.”
   - Focus: current-state assessment + strategic and design recommendations.

3. **The strategic call is: upgrade the chatbot to an Agent.** A vs B is *where that agent is hosted*, based on use cases — not a predetermined Casey recommendation.
   - **A** = independent Agentforce on the current Experience Cloud (Aura) help site.
   - **B** = Casey Help Portal (LWR): site + agent as a unified product, dedicated CMS layer.
   - Steelman A: search hero, step articles, deeplinks, intent IA, and URL/session context (`product`, `user_id`, `account_status`, `last_page`) all ship on Aura. Continuity is a **parameter contract**, not a Casey feature.

4. **In-app agent is a third use case, not a Casey feature.**
   - Validating with Salesforce Agent teams: if the FanDuel **app** opens an agent, is it the **same agent** as the Help Portal agent — and can that Help Portal agent **support / orchestrate subagents** (SBK, Casino, DFS, RG)?
   - If yes → Casey is the stronger hosting fit.
   - If no → independent Agentforce on the current site may be cleaner; in-app is a separate agent.
   - Until that validates: do not recommend B; do not label the in-app screen “Approach B only.”

5. **Highest-ROI experience work is independent of A vs B and should start either way:**
   - Search as homepage hero
   - Top-10 self-service article rewrites (steps, decision trees, deep links)
   - Replicate the Location Troubleshooting deeplink pattern
   - Intent-led IA (16 product categories → 5 user intents)
   - App → help context contract

6. **Necessary Transfer ≠ failed self-service.**
   - Account Locked and Contact Us need routing and expectation-setting, not a how-to rewrite.
   - Password, verify, 2FA, “how to contact” used as a contact path are a different class.
   - Bleed tables must keep a **Class** column (Performing / Self-service / contact path / Necessary Transfer).

7. **Section numbering:** 3.1 decision → 3.2 what each path gives → 3.3 experience recs → 3.4 UI → 3.5 IA. Then Part 4 agent parameters (not a roadmap).

---

## What Claude may do next (only if Ren asks)

Typical follow-ons: Figma wireframes from Screens 1–5, client presentation of assessment + design direction, polish copy, or fold in Agent-team answers on same-agent / subagents. Do not add cost, a plan, or a Casey recommendation unless Ren explicitly changes the lock above.

---

## Current assessment (paste / treat as the live draft)

The document that follows is the updated `ec-advisory-draft.md` as of 23 September 2026.

---

# FanDuel Help Portal — Digital Care Assessment
**Experience Assessment | September 2026**
*Prepared for: FanDuel | Engagement: Advisory + Design*

---

## Part 1 — Current State

### 1.1 Traffic & Engagement Snapshot

| Metric | Value |
|---|---|
| Total site visitors | 5,755,615 |
| Total chats | 1,799,644 |
| Overall chat rate | **31.3%** (27.3% adjusted) |
| Mobile / tablet users | **91%** |
| Visits arriving from a FanDuel app | **82%** |
| External / organic visits | ~275,000 (2025) |

**What this tells us:** The support site exists almost entirely as an extension of the FanDuel app experience. 4 in 5 users come from inside the product. Yet the site treats every visitor identically — no app context, no personalisation, no continuity. The chat rate is a deflection signal, but it is also a design signal: the site is not built for the audience it actually serves.

---

### 1.2 Site Diagnosis

**Search is buried.** The current above-fold design presents five navigation tiles (My Account · Manage My Money · How to Play · Rewards & Promos · State Rules & Locations). Search is an icon in the header. Users who know what they want — the majority — are forced into category navigation before they can express intent.

**No app context is passed.** When a user taps Support from the SBK app, the help site has no idea where they came from or what they were doing. A Casino player and a DFS player see the exact same homepage. The same chat bot opens with no prior context. Every handoff starts at zero.

**High-volume articles are bleeding to agents.** Chat rate here is two different jobs. Self-serviceable topics (password, verify, 2FA) are written as narrative, so users who could finish on the page escalate. Necessary Transfer topics (Account Locked, Contact Us) correctly need a person — they need expectation-setting and routing, not a how-to rewrite. Do not read this table as one deflection problem.

| Article | Visitors | Chat Rate | Class | Gap |
|---|---|---|---|---|
| Location Troubleshooting Tips | 673,000 | 5.5% | Performing | Deeplink from app drives volume — replicate this pattern |
| Why Am I Unable to Verify | 108,000 | 22.6% | Self-service | Should resolve; article format likely insufficient |
| Password Reset | 44,000 | 43.4% | Self-service | Critical — nearly half escalate |
| Change My Password | — | 39.1% | Self-service | Same pattern as reset |
| 2FA / Two-Factor Authentication | — | 23.1% | Self-service | Step-based content needed |
| How to Contact FanDuel Support | — | 48.5% | Self-service used as a contact path | People using the article to reach an agent, not to resolve |
| Account Locked | — | 62.6% | Necessary Transfer | Needs better pre-chat expectation-setting, not a how-to |
| Contact Us | — | 61.8% | Necessary Transfer | Appropriate purpose; surface after self-service has been attempted |

**Taxonomy is product-architecture-led.** The 16 article categories (SBK · Casino · DFS · Racing · Predicts · FaceOff · PokerStars · FDTV · TVG) reflect FanDuel's internal product structure. Users don't think "I have a Casino question" — they think "I can't withdraw my winnings." The mismatch creates friction at the navigation layer before users ever reach content.

**Content format is not optimised for self-service or AI.** The majority of articles are written in narrative paragraph format. Users on mobile skim; they need numbered steps, decision trees, and inline action links (e.g. "tap here to reset your password"). The same format issue limits Agentforce's ability to use the content for AI-assisted resolution.

**Chat handoff lacks context continuity.** When a user escalates from self-service to chat, the agent starts a new conversation with no record of what the user was reading, what they tried, or what app/product they were using. Customers repeat themselves. Agents start cold.

---

### 1.3 Current Sitemap

#### Navigation Shell
```
fanduelgroup.my.site.com/s/
├── Home
├── FAQs
└── Contact Us
```

#### Homepage Tiles (above fold)
```
├── My Account
├── Manage My Money
├── How to Play
├── Rewards & Promos
└── State Rules & Locations
```

#### Full Article IA (current — product-led)
```
Account
├── Login & Password
├── Account Verification / Identity
├── Personal Details
├── Two-Factor Authentication
├── Account Status (locked, suspended, closed)
└── Device & App settings

Money
├── Deposits (by payment method)
├── Withdrawals (by payment method)
├── Pending / Processing Times
├── Failed Transactions
└── Limits & Controls

Responsible Gaming (RG)
├── Deposit Limits
├── Session / Reality Checks
├── Self-Exclusion
├── Cooling Off
└── State-specific RG requirements

States & Eligibility
├── Location Troubleshooting
├── Eligible States by Product
└── State-Specific Rules

Bets & Features
├── Placing Bets
├── In-Play / Live Betting
├── Bet Settlement / Disputes
└── Features (SGP, cash out, etc.)

Tentpoles & Sports
├── Promotions by sport / event
└── Bonus terms

Taxes
├── W-2G Forms
├── 1099 Forms
└── Tax FAQs

DFS
├── Contests
├── Scoring
├── Payouts
└── DFS-specific FAQs

SBK (Sportsbook)
├── Bet types
├── Promos
└── SBK-specific FAQs

Casino
├── Games
├── Promos
└── Casino-specific FAQs

Predicts
Racing / TVG
FaceOff
PokerStars
FDTV
```

**Sitemap gaps identified:**
- No unified "I can't log in" landing — scattered across Account subcategories
- No app-specific help content (iOS vs Android troubleshooting)
- No conversational entry points ("something went wrong with my bet") — users must navigate product category first
- Tentpoles & Sports is event-driven content mixed with evergreen FAQs — maintenance-heavy, no clear update owner
- Predicts, FaceOff, FDTV, TVG have thin article coverage vs. their user base
- No dedicated promo/bonus hub across products — bonuses live in each product category separately

---

### 1.4 Content Audit

#### Priority triage by deflection impact

| Priority | Category | Issue | Recommended Action |
|---|---|---|---|
| 🔴 High | Password Reset & Change | 39–43% chat rate on self-serviceable content | Rewrite: numbered steps, inline deep link to reset flow |
| 🔴 High | Account Verification | 22.6% chat, high app-driven traffic | Rewrite: decision tree by ID type, steps with screenshots |
| 🔴 High | Two-Factor Authentication | 23.1% chat | Rewrite: per-device steps, recovery options upfront |
| 🔴 High | "How to Contact Support" | 48.5% chat (people using it as a contact route, not resolution) | Redesign: pre-chat deflection checklist before surfacing agent |
| 🟡 Medium | Withdrawals | Processing timelines unclear; drives avoidable "where is my money" contacts | Add: explicit timelines by method, status check deep link |
| 🟡 Medium | Bonuses & Promos | Scattered across product categories | Consolidate: cross-product promo hub with terms inline |
| 🟡 Medium | Location Troubleshooting | 5.5% chat (performing well) but 673K visits from a single app deeplink | Maintain; replicate the deeplink pattern across other high-volume topics |
| 🟢 Good | Account Locked | 62.6% chat — but correctly classified Necessary Transfer | Add pre-chat message setting expectations: "This requires an agent — have your ID ready" |
| 🟢 Good | Contact Us | 61.8% chat — appropriate given purpose | Maintain; surface only after self-service path has been attempted |

#### Content format scorecard

| Format | Current state | Target |
|---|---|---|
| Paragraph narrative | Dominant | Eliminate for procedural content |
| Numbered steps | Limited | Standard for any "how to" article |
| Decision trees | Absent | Add for verification, payment, and account status topics |
| Inline action links | Absent | Deep links to account settings actions where possible |
| Screenshots / visuals | Inconsistent | Standardise for verification and app navigation articles |
| AI-readable structure | Not optimised | Bullet/step format enables Agentforce to extract and surface answers |

#### SME Review coverage
Articles tracked across 16 categories with monthly review waves (July · May · June · August 2026 completed). Content governance is active. Gap: no clear signal on which articles are *performing* vs. just reviewed — review cycle should incorporate chat rate data as a trigger for rewrites.

---

## Part 2 — Opportunity

### 2.1 What's at Stake

This is an **experience problem**, not a channel-mix problem.

**82% of visitors arrive from a FanDuel app.** They already have a product, a session, and often a specific failed action. The current path asks them to leave that context, land on a generic help homepage, and — if they escalate — repeat themselves to a chatbot that starts at zero.

Three experience gaps compound:

1. **Self-service does not match how people arrive.** Search is buried. Navigation is product-architecture-led. High-intent articles (password, verification, 2FA) are written as narrative, so users who could finish in the app bounce to chat.
2. **Necessary Transfer is mixed in with avoidable contact.** Account Locked and Contact Us are correctly high-chat — those users need a person. Password reset and “how to contact support” are not the same class of problem. The site currently treats them similarly: article, then chat.
3. **The agent surface is disconnected from the product.** Whether help opens in the browser or from the app, the user should meet *an agent that already knows who they are and which product they were in* — not a new conversation.

The opportunity is continuity: the same intents, the same content, and — if architecture allows — the **same agent** from app and Help Portal, with context passed in.

---

### 2.2 Do this either way — starting now

The highest-leverage experience work does **not** depend on Aura vs Casey. It should start regardless of how the agent is hosted.

| Work | Why it is independent of platform |
|---|---|
| Search as the homepage hero | Fixes the buried-search problem on any Experience Cloud surface |
| Top-10 article rewrites (steps, decision trees, inline actions) | Password, verification, 2FA, contact-as-deflection — format, not platform |
| Location-style deeplinks on other high-volume intents | The 5.5% chat article already proves the pattern; replicate it |
| Intent-led IA (16 product categories → 5 user intents) | Taxonomy and redirects; the CMS still has to be authored either way |
| App → help context contract (`product`, `user_id`, `account_status`, `last_page`) | Continuity is a parameter contract, not a Casey feature. URL/session context can land on Aura today |

These are the recommendations we would make even if the chatbot stayed as-is. They also make whatever Agentforce path is chosen actually usable: an agent cannot resolve what the content cannot express in steps.

---

## Part 3 — Recommended Direction

### 3.1 The decision in front of us

**The chatbot should become an Agent.** That is the strategic call. The open implementation question is *where that agent lives*, based on the use cases — not a platform preference in the abstract.

| Path | What it is |
|---|---|
| **A — Independent Agentforce on the current Help site** | Upgrade the existing Experience Cloud (Aura) experience. Stand up Agentforce on that site. Content, IA, search, deeplinks, and URL context all ship here. |
| **B — Casey Help Portal** | Migrate the help *site* to Salesforce’s purpose-built Help Portal (LWR), where the site and the agent are a unified product, with a dedicated content layer (addresses Salesforce-as-CMS friction). |

These are **site + agent hosting** choices. They are not, by themselves, the in-app agent.

**In-app help is a third use case**, and it is the one that matters most to this audience (82% from apps). The question we are validating with Salesforce Agent teams:

> If the FanDuel app opens an agent, is that the **same agent** as the Help Portal agent — and can that Help Portal agent **orchestrate subagents** for product-specific work (SBK, Casino, DFS, RG)?

| If the answer is… | Then… |
|---|---|
| Yes — one agent, portal and app, with subagents | Casey becomes the stronger fit: one agent definition, one content layer, multiple surfaces |
| The app would open a *different* agent, or the portal agent cannot parent subagents | Independent Agentforce on the current site may be the cleaner path; in-app is a separate agent (or a later orchestration) rather than a Casey benefit |
| Partial / still unknown | Do not bind the experience recommendations to Casey. Keep A and B open until this validates |

We are not recommending a platform in this assessment. We are recommending the experience, the agent upgrade, and the questions that should decide A vs B.

---

### 3.2 What each path actually gives you

Continuity (knowing the user, the product, the last screen) requires a **parameter contract** with the app teams. Casey makes a unified agent *cleaner* if the same-agent / subagent model holds. It does not uniquely unlock search, step content, deeplinks, or URL context.

#### Approach A — Independent Agentforce on current EC

Redesign homepage, IA, and article format on the existing Experience Cloud site. Replace the chatbot with Agentforce on that site.

| Dimension | Detail |
|---|---|
| Platform | Existing Experience Cloud (Aura) — no site migration |
| Agent | Agentforce on the current help site (independent of Casey) |
| What A *can* do | Search hero, step articles, deeplinks, intent IA, URL/session context (`product`, `last_page`, etc.) |
| App surface | App can still open help (or an agent) via a contract with app teams — not automatic, not Casey-dependent |
| CMS | Salesforce-as-CMS constraints remain (already a delivery pain point) |
| Tradeoff | Faster on the platform the SF Build team already operates; Agentforce + Aura integration is still non-trivial; CMS friction persists |

**When A is the right agent path:** the Help Portal agent cannot be the same agent the apps open, or cannot support the subagent model the use cases need — and FanDuel still wants Agentforce on the site that exists today.

---

#### Approach B — Casey Help Portal

Migrate to Salesforce’s purpose-built Help Portal, where the **site and the agent are designed as one product**, with a dedicated content layer.

| Dimension | Detail |
|---|---|
| Platform | LWR Help Portal — purpose-built for self-service |
| Agent | Agentforce as a native product of the portal, not a bolt-on |
| What B *adds* vs A | Unified site + agent product; dedicated CMS layer (removes Salesforce-as-CMS friction) |
| App surface | Only an advantage **if** the portal agent is the same agent the apps would open, and it can support subagents — this is the validation in flight |
| CMS | Dedicated content layer — better long-term authoring for Digital Care / SF Build |
| Tradeoff | Content migration and a new platform for the SF Build team; do not treat in-app embed as a Casey feature until Agent teams confirm the model |

**When B is the right agent path:** use cases need one agent across Help Portal and apps, with subagents for product or RG, *and* the Casey architecture supports that. CMS relief is a real secondary reason even if in-app is sequenced later.

---

### 3.3 Experience recommendation (independent of A vs B)

1. **Make search the homepage.** Category tiles become secondary to intent.
2. **Rewrite the high-bleed, self-serviceable articles** into numbered steps, decision trees, and inline deep links. Do not treat Necessary Transfer articles (Account Locked, Contact Us) as the same job — those need expectation-setting and routing, not a how-to rewrite.
3. **Lead IA with user intent**, not product lines. Replicate the Location Troubleshooting deeplink pattern across Login, Deposit, Withdrawal, Verify.
4. **Upgrade the chatbot to an Agent** with the intent tiers, RG gate, and context parameters in Part 4. Choose **independent vs Casey** once the same-agent / subagent question is answered.
5. **Pass context from the app** (`product`, `user_id`, `account_status`, `last_page`) into whatever help or agent surface opens. That contract is the continuity layer. The hosting choice sits on top of it.

---

### 3.4 Proposed UI: Mobile-First Wireframe Concepts

*Mobile-first: 91% of users access via mobile or tablet. All screens designed at 390px width. Desktop adapts from mobile base.*

---

#### Screen 1 — Homepage (Current State)

```
┌─────────────────────────────┐
│ ≡  FanDuel Support    🔍    │  ← Search icon only, buried
├─────────────────────────────┤
│  HOW CAN WE HELP?           │
│                             │
│  [👤 My Account]            │  ← 5 category tiles
│  [💰 Manage My Money]       │     force navigation
│  [🎮 How to Play]           │     before intent
│  [🎁 Rewards & Promos]      │
│  [📍 State Rules]           │
│                             │
│  ALL-STAR FAQs              │  ← Static list below fold
│  › Deposit with FanDuel     │
│  › Taxes - W-2G / 1099      │
│  › ...                      │
│                             │
│              [💬 Chat]      │  ← Floating button, small
└─────────────────────────────┘
```

**Problems:** Search buried. Category-first forces navigation. No trending or personalised content. Chat is a floating button — not promoted as a service path.

---

#### Screen 2 — Homepage (Proposed)

*This homepage is the experience recommendation on either path — not a Casey-only design.*

```
┌─────────────────────────────┐
│  FanDuel Support            │  ← Clean header, no clutter
├─────────────────────────────┤
│                             │
│  How can we help?           │  ← Search as hero (40% screen)
│  ┌─────────────────────┐    │
│  │ 🔍 Search...        │    │
│  └─────────────────────┘    │
│                             │
│  QUICK ACTIONS              │  ← Context-aware (app source)
│  ┌──────────┐ ┌──────────┐  │
│  │ 📍 Track │ │ 💳 Depos-│  │
│  │ Location │ │ it Help  │  │
│  └──────────┘ └──────────┘  │
│  ┌──────────┐ ┌──────────┐  │
│  │ 💸 With- │ │ 🔑 Login │  │
│  │ drawal   │ │ & Access │  │
│  └──────────┘ └──────────┘  │
│                             │
│  TRENDING NOW               │  ← Dynamic, Amplitude-driven
│  › How to reset my password │
│  › Withdrawal timelines     │
│  › Verify my account        │
│                             │
│  ┌─────────────────────────┐│
│  │ Chat with us            ││  ← Chat as a clear CTA
│  │                         ││     not a floating button
│  └─────────────────────────┘│
└─────────────────────────────┘
```

**Key changes:** Search hero. Quick actions surface the 4 highest-volume intents. Trending section (Amplitude-powered) surfaces what users are looking for right now. Chat is a clear CTA — not an afterthought. Live wait time only if FanDuel chooses to publish a real figure; do not invent an SLA in the UI.

---

#### Screen 3 — Article Page (Proposed — Step Format)

```
┌─────────────────────────────┐
│ ← Back    🔍           💬  │
├─────────────────────────────┤
│ How to Reset Your Password  │
│                             │
│ ⏱ 2 min read  📱 Mobile    │
│                             │
│ ─────────────────────────── │
│                             │
│ Step 1                      │
│ Open the FanDuel app and    │
│ tap Log In                  │
│                             │
│ Step 2                      │
│ Tap Forgot Password         │
│                             │
│ Step 3                      │
│ Enter your email and tap    │
│ Send Reset Link             │
│                             │
│ ⚡ [Reset password now →]   │  ← Deep link to app action
│                             │
│ ─────────────────────────── │
│ Still stuck?                │
│ [💬 Chat with us]           │  ← Deflection before agent
│                             │
└─────────────────────────────┘
```

**Key changes:** Numbered steps replace narrative paragraphs. Estimated read time. Inline deep link to the actual app action. Chat appears *after* the self-service path — not floating over the content.

---

#### Screen 4 — Agentforce Chat (In-portal)

```
┌─────────────────────────────┐
│ ← Back    FanDuel Support  │
├─────────────────────────────┤
│                             │
│  ┌──────────────────────┐   │
│  │ 🤖 Hi, I'm here to  │   │
│  │ help. What's going  │   │
│  │ on with your account│   │
│  │ today?              │   │
│  └──────────────────────┘   │
│                             │
│  Quick replies:             │
│  [Can't log in]             │
│  [Withdrawal issue]         │  ← Intent chips (Agentforce)
│  [Verify my account]        │
│  [Something else]           │
│                             │
│                             │
│                             │
│ ─────────────────────────── │
│ ┌─────────────────────┐[→] │
│ │ Type a message...   │    │
│ └─────────────────────┘    │
└─────────────────────────────┘
```

---

#### Screen 5 — App-opened agent (same agent as Help Portal — pending validation)

*Experience target, not a Casey feature. Whether this is the Help Portal agent (with subagents) is the question in validation with Agent teams.*

```
┌─────────────────────────────┐
│  FanDuel Sportsbook    [≡]  │  ← Inside the FD app
│  Jordan · SBK · Gold        │
├─────────────────────────────┤
│                             │
│  [Bet Slip]  [My Bets]      │
│  [Promos]  [Account]        │
│                             │
│  ─────────────────────────  │
│  ┌──────────────────────┐   │
│  │ 💬 Need help?        │   │  ← Agent CTA in-app
│  │ Ask me anything about│   │
│  │ your account.        │   │
│  │ [Get help →]         │   │
│  └──────────────────────┘   │
│                             │
└─────────────────────────────┘

↓ Taps "Get help" →

┌─────────────────────────────┐
│ ←      FanDuel Support      │
│ Logged in as Jordan · SBK   │  ← Context pre-loaded
├─────────────────────────────┤
│                             │
│  ┌──────────────────────┐   │
│  │ 🤖 Hi Jordan! I can  │   │
│  │ see you're on        │   │
│  │ Sportsbook. What's   │   │
│  │ going on?            │   │
│  └──────────────────────┘   │
│                             │
│  [Issue with a bet]         │
│  [My account]               │
│  [Deposit / withdrawal]     │
│                             │
└─────────────────────────────┘
```

**What this screen is asking:** User never leaves the app. The agent knows who they are and which product they are on. No “start from zero” moment. That requires the context contract — and, if validated, the *same* agent definition as the Help Portal rather than a second bot.

---

### 3.5 Proposed Information Architecture (Intent-Led)

#### Proposed sitemap
```
Home
├── Log In & Access
│   ├── Forgot / Reset Password
│   ├── Account Locked or Suspended
│   ├── Two-Factor Authentication
│   ├── Verify My Account (ID, Address, DOB)
│   └── Device & App Troubleshooting
│
├── Money In & Out
│   ├── Deposit (by method: Card, PayPal, Online Banking, etc.)
│   ├── Withdrawal (by method + timelines)
│   ├── Pending or Failed Transactions
│   ├── Bonuses, Promos & Free Bets (cross-product)
│   └── Tax Documents (W-2G, 1099)
│
├── Bets, Games & Results
│   ├── Placed a Bet / Entered a Contest
│   ├── Live / In-Play Features
│   ├── Results & Disputes
│   ├── Rules (Sportsbook · Casino · DFS · Racing · Predicts)
│   └── App & Feature Help (SGP, Cash Out, Boost)
│
├── Responsible Gaming
│   ├── Set a Deposit Limit
│   ├── Take a Break / Self-Exclusion
│   ├── Reality Checks
│   └── State RG Resources
│
├── State Rules & Location
│   ├── Where can I play? (map / state list)
│   ├── Location Check Troubleshooting (← deeplink target from app)
│   └── State-Specific Rules by Product
│
└── Get Help
    ├── Chat with Agentforce (self-service agent)
    ├── Talk to a Person (escalation — clearly labelled)
    └── Submit a Complaint / Regulatory

```

#### Current vs proposed IA comparison

| Dimension | Current | Proposed |
|---|---|---|
| Top-level structure | 5 homepage tiles + 16 product-led categories | 5 intent-led categories + Get Help |
| Entry point | Category navigation (product) | Search hero + intent tiles |
| Bonus/promo content | Scattered across each product | Consolidated cross-product hub |
| Legal / RG content | Buried in States and product categories | Dedicated, prominent RG section |
| App-to-site deeplink | One (Location) | Scalable deeplink target per category |
| Content maintainability | 16 tabs, separate SME per product | 5 intent categories, shared taxonomy |

---

## Part 4 — Agent Design Parameters

### 4.1 Agentforce parameters

*These are an output of the assessment — the brief for whichever Agentforce path is chosen (independent on current EC, or Casey). They are not a build plan.*

#### Intent taxonomy (what the agent handles)

| Tier | Intents | Agent can resolve? |
|---|---|---|
| Tier 1 — Full self-service | Password reset, 2FA setup, deposit status, location check, verify account status | Yes — agent provides steps + deep links |
| Tier 2 — Guided + hand-off | Withdrawal delays, failed transactions, bet dispute (informational), promo eligibility | Agent provides info; escalates if unresolved |
| Tier 3 — Human required | Account locked/suspended, identity fraud, regulatory complaint, responsible gaming escalation | Agent acknowledges, sets expectations, routes immediately |

Necessary Transfer (Tier 3) is a **routing** problem. Tier 1 is a **content and action** problem. Do not design them as the same article pattern.

#### Escalation triggers (always to human agent)
- Account status = Locked or Suspended
- User mentions fraud, stolen account, or unauthorised access
- User has invoked self-exclusion or requests RG support
- Three failed resolution attempts on same intent in session
- User explicitly requests a human

#### Responsible Gaming gate (hard rule)
If a user's session or account has any active RG flag (cooling off, self-exclusion, limit alert), the agent must:
1. Acknowledge the user warmly and without judgement
2. Surface only RG support resources and state-mandated contacts
3. Not offer or discuss bonuses, promotions, or incentives in the same session

This gate is non-configurable — it cannot be overridden by any team.

If subagents are supported, RG should be a **hard-gated subagent** (or a pre-agent policy), not a topic the general help agent improvises.

#### Context passing rules (app → help or agent)
Continuity lives in this contract. It applies whether the surface is the current site, Casey, or an agent opened from the app.

| Parameter | Source | Use |
|---|---|---|
| `product` | App identifier | Pre-load product-relevant quick replies; route to the right subagent if the model allows |
| `user_id` | Authenticated session | Agent greets by name, skips account lookup |
| `account_status` | API on launch | Surface proactive alert if account has an issue |
| `last_page` | App navigation | Inform first agent message ("I can see you were looking at withdrawals…") |

#### Tone and persona
- Name: not defined — FanDuel to confirm whether agent has a name or uses "FanDuel Support"
- Tone: direct, plain English, no jargon. Warm but efficient — users are often frustrated when they contact support
- Never: deflect to irrelevant content, use legalese, or suggest a bonus/promo in a Tier 3 session
- Always: confirm what the user needs before answering; offer to escalate before ending a session

---

### 4.2 What we are validating next

| Question | Who | Why it decides A vs B |
|---|---|---|
| If the app opens an agent, is it the **same agent** as the Help Portal agent? | Salesforce Agent teams + this advisory | One agent definition vs. two products to design and govern |
| Can the Help Portal agent **support subagents** (product, RG, payments)? | Salesforce Agent teams | Casey’s value as an orchestration layer vs. a single-site agent |
| What is the current chatbot platform and any contract / migration dependency? | FanDuel Digital Care | Replacement vs. parallel run when Agentforce stands up |
| Which app teams own the support entry point in SBK / Casino / DFS? | FanDuel app teams | The context contract has owners; without them, neither path has continuity |

---

## Appendix — Open Questions

| # | Question | Why it matters |
|---|---|---|
| 1 | If the app opens an agent, is it the same agent as the Help Portal — and can that agent support subagents? | Decides independent Agentforce vs Casey; validating with Agent teams |
| 2 | What was actually delivered against the May–June 2026 milestones in the recos deck? | Net-new vs. picking up in-progress work |
| 3 | Has Amplitude been integrated? Is search data available? | Trending content and search-gap feedback depend on this |
| 4 | What is the current chatbot platform? Contract or migration dependency? | Agentforce replacement vs. parallel deployment |
| 5 | Which app teams own the support entry point in SBK / Casino / DFS? | Context passing requires a parameter contract with those teams |
| 6 | Does FanDuel have a named agent persona, or is this TBD? | Tone and name |
| 7 | Responsible Gaming: state-level rules that govern agent behaviour? | RG gate may need state-specific routing or a dedicated subagent |
| 8 | Who is the content owner for a 16 → 5 taxonomy restructure? | SME sign-off and redirect management — this work is recommended either way |

---

*Experience assessment. All data sourced from FanDuel-provided documents (Q1 2025 traffic report, chat rate analysis, recos deck, article categories Aug 2026). Site observations from document content — live site requires authenticated browser access for full audit. No cost, volume-savings, or delivery plan is included in this deliverable.*

*Next: same-agent / subagent validation with Agent teams · wireframes to Figma · client presentation of assessment + design direction.*
