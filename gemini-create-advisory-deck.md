INSTRUCTIONS FOR GEMINI: Create a **Google Slides** presentation in my Drive titled "FanDuel Help Portal — Digital Care Assessment". Widescreen 16:9. Copy every slide below verbatim — do not skip, summarize, merge, or invent slides. Do not add a roadmap, timeline, cost, savings, or "we recommend Casey" slide. Return the Google Slides link when done.

BRAND STYLING (apply to every slide):
• This is a FanDuel **client experience assessment**, not a FanDuel consumer marketing deck and not a Salesforce product demo.
• Palette (use these hex values exactly):
  – Navy #0B1F45 — cover, section breakers, footer bar
  – FanDuel Blue #0060A3 — titles, key numbers, links, accent rule
  – Ink #1A1A1A — body text
  – Slate #5C6B73 — captions, labels, kicker
  – Paper #F4F6F9 — card / table header fill
  – White #FFFFFF — content slide background
  – Performing #2E844A — Location / "keep" only
  – Caution #8C4B02 — self-service bleed (not alarm red)
  – Neutral #5C6B73 — Necessary Transfer (do not paint these as failures)
• Type: Inter if available, otherwise Google Sans. Cover title 36–40pt. Slide titles 24–28pt. Body 16–18pt. Captions 12–14pt. Never use Comic Sans, Impact, or script fonts.
• Layout: generous whitespace; one idea per slide; left-aligned titles; 0.7" margins. 8px FanDuel Blue bar along the top of content slides. Page number + "FanDuel Help Portal · Experience Assessment" in slate at the bottom.
• Cover / section slides: full navy, white type, blue kicker in small caps.
• Tables: navy or paper header, no rainbow row colors. Use the Class column to distinguish Performing / Self-service / Necessary Transfer — color the Class *label* only, not the whole row.
• Do **not** use sports photography, betting imagery, confetti, gradients, drop shadows, 3D, or Salesforce cloud illustrations.
• Wireframe slides: simple phone frames (rounded rect, 390-style, white fill, 1px slate stroke). No emoji if you can use labels instead. Do not invent "avg wait < 2 min" or any SLA.
• Charts: if you visualize chat rate, one color (#0060A3) for all bars. Label the 31.3% site average as a reference line. Caption that Account Locked and Contact Us are Necessary Transfer.

LOCKED (do not violate):
• No cost, savings, 520k chats, 31%→22%, cost-per-chat.
• No delivery plan, phases, or dates as a roadmap.
• Do not recommend Casey / Approach B. The strategic call is: chatbot becomes an Agent. A vs B is hosting. In-app is a third use case pending Agent-team validation (same agent? subagents?).
• Necessary Transfer ≠ failed self-service.

===========================================================
SLIDE CONTENT (verbatim)
===========================================================

SLIDE 1 — Cover
Kicker: EXPERIENCE ASSESSMENT · SEPTEMBER 2026
Title: FanDuel Help Portal
Subtitle: Digital Care
Footer: Prepared for FanDuel · Advisory + Design
Notes: This is assessment and design direction, not a build plan.

---

SLIDE 2 — What this is
Title: What this is
Left column heading: This assessment
• Current-state diagnosis of the help experience
• Design direction: homepage, articles, IA, agent
• How we should choose Agentforce hosting
Right column heading: This is not
• A cost or savings case
• A delivery roadmap
• A recommendation to migrate to Casey
Bottom line: The chatbot should become an Agent. Where that agent lives depends on use cases we are still validating.

---

SLIDE 3 — The audience
Title: The help site is an extension of the app
Four stats, large:
• 5.76M site visitors
• 1.80M chats
• 31.3% overall chat rate
• 82% of visits from a FanDuel app
Supporting: 91% mobile or tablet · ~275k external / organic (2025)
Takeaway: Four in five users are already in the product. The site treats every visitor the same — no app context, no personalisation, no continuity.

---

SLIDE 4 — Five findings
Title: Five findings
1. Search is buried — five category tiles above the fold; search is a header icon.
2. No app context is passed — Casino and DFS see the same homepage; chat starts at zero.
3. High-volume articles bleed to agents — but that is two different jobs (next slide).
4. Taxonomy is product-led — 16 categories follow FanDuel’s org chart, not user intent.
5. Chat handoff has no continuity — customers repeat themselves; agents start cold.

---

SLIDE 5 — Two jobs
Title: Chat rate is two jobs — do not read this as one deflection problem
Table:

Article | Visitors | Chat rate | Class
Location Troubleshooting Tips | 673,000 | 5.5% | Performing
Why Am I Unable to Verify | 108,000 | 22.6% | Self-service
Password Reset | 44,000 | 43.4% | Self-service
Change My Password | — | 39.1% | Self-service
2FA | — | 23.1% | Self-service
How to Contact FanDuel Support | — | 48.5% | Used as a contact path
Account Locked | — | 62.6% | Necessary Transfer
Contact Us | — | 61.8% | Necessary Transfer

Caption: Location works because of an app deeplink — replicate that pattern. Password / verify / 2FA need steps and actions. Account Locked and Contact Us need routing and expectation-setting, not a how-to rewrite.

---

SLIDE 6 — Current homepage
Title: Current homepage — category first, search last
Left: simple wireframe of current mobile homepage
• Header: FanDuel Support + search icon
• Five tiles: My Account · Manage My Money · How to Play · Rewards & Promos · State Rules
• All-Star FAQs below the fold
• Chat as a small floating button
Right: Problems
• Search buried
• Users must pick a product category before they can express intent
• No trending or personalised content
• Chat is an afterthought, not a service path

---

SLIDE 7 — Opportunity
Title: The opportunity is continuity
Body: 82% arrive from a FanDuel app with a product, a session, and often a failed action. Today they leave that context, land on a generic homepage, and repeat themselves to a chatbot that starts at zero.
Three gaps:
1. Self-service does not match how people arrive.
2. Necessary Transfer is mixed with avoidable contact.
3. The agent surface is disconnected from the product.
Bottom: Same intents, same content, and — if architecture allows — the same agent from app and Help Portal, with context passed in.

---

SLIDE 8 — Do this either way
Title: Do this either way — it does not wait on Aura vs Casey
Table:

Work | Why it is independent of platform
Search as the homepage hero | Fixes buried search on any Experience Cloud surface
Top-10 article rewrites | Format: steps, trees, inline actions
Location-style deeplinks | 5.5% chat article already proves the pattern
Intent-led IA (16 → 5) | Taxonomy and redirects; still authored either way
App → help context contract | Continuity is a parameter contract, not a Casey feature

Footer: An agent cannot resolve what the content cannot express in steps.

---

SLIDE 9 — Strategic call
Title: The chatbot should become an Agent
Subtitle: That is the strategic call. Hosting is still open.
Two equal cards (neither is “recommended”):
Card A — Independent Agentforce on current EC
• Stay on Experience Cloud (Aura)
• Agentforce on the help site that exists today
• Can still do: search hero, step articles, deeplinks, intent IA, URL context
Card B — Casey Help Portal
• LWR Help Portal: site + agent as one product
• Dedicated content layer (addresses Salesforce-as-CMS friction)
• In-app is an advantage only if the portal agent is the same agent the apps would open
Footer: We are not recommending a platform in this assessment.

---

SLIDE 10 — The deciding question
Title: The question we are validating with Agent teams
Quote, large:
If the FanDuel app opens an agent, is that the same agent as the Help Portal agent — and can that agent orchestrate subagents (SBK, Casino, DFS, RG)?
If yes: Casey is the stronger hosting fit — one agent definition, one content layer, multiple surfaces.
If no: Independent Agentforce on the current site may be cleaner; in-app is a separate agent.
If still unknown: Do not bind the experience work to Casey. Keep A and B open.

---

SLIDE 11 — Experience recommendations
Title: Experience recommendations (independent of A vs B)
1. Make search the homepage. Category tiles become secondary to intent.
2. Rewrite high-bleed self-service articles into steps, trees, and deep links. Necessary Transfer is routing, not a how-to.
3. Lead IA with user intent, not product lines. Replicate the Location deeplink across Login, Deposit, Withdrawal, Verify.
4. Upgrade the chatbot to an Agent (tiers, RG gate, context in later slides). Choose independent vs Casey once the same-agent question is answered.
5. Pass app context (product, user_id, account_status, last_page) into whatever help or agent surface opens.

---

SLIDE 12 — Proposed homepage
Title: Proposed homepage — search first (either path)
Phone wireframe:
• Clean header: FanDuel Support
• Search as hero
• Quick actions (app-aware): Location · Deposit · Withdrawal · Login
• Trending (Amplitude): reset password · withdrawal timelines · verify account
• Chat with us — a clear CTA, not a floating button. No wait-time number.
Caption: This is the experience recommendation on Aura or Casey. Not a Casey-only design.

---

SLIDE 13 — Article format
Title: Articles as steps — then chat
Phone wireframe: How to Reset Your Password
• Step 1 Open the FanDuel app and tap Log In
• Step 2 Tap Forgot Password
• Step 3 Enter email and tap Send Reset Link
• Action: Reset password now (deep link)
• Still stuck? Chat with us — after the self-service path
Caption: Numbered steps replace narrative. Chat does not float over the content.

---

SLIDE 14 — App-opened agent
Title: In-app agent is an experience target — not a Casey feature
Two phones:
1. Inside FanDuel Sportsbook: Jordan · SBK · Gold. “Need help? Ask me anything about your account.”
2. After tap: Logged in as Jordan · SBK. “Hi Jordan. I can see you’re on Sportsbook. What’s going on?” Chips: Issue with a bet · My account · Deposit / withdrawal
Caption: User never leaves the app. Requires the context contract — and, if validated, the same agent definition as the Help Portal. Pending Agent-team confirmation.

---

SLIDE 15 — Proposed IA
Title: Intent-led IA — 16 product categories → 5 user intents + Get Help
Six groups:
• Log In & Access
• Money In & Out
• Bets, Games & Results
• Responsible Gaming
• State Rules & Location
• Get Help (Agentforce · Talk to a person · Complaint / Regulatory)
Caption: Users think “I can’t withdraw,” not “I have a Casino question.”

---

SLIDE 16 — Agent tiers
Title: What the agent handles
Three tiers:
Tier 1 — Full self-service: Password reset, 2FA, deposit status, location check, verify account status. Agent gives steps + deep links.
Tier 2 — Guided + hand-off: Withdrawal delays, failed transactions, bet dispute (info), promo eligibility. Info first; escalate if unresolved.
Tier 3 — Human required: Account locked/suspended, identity fraud, regulatory complaint, RG escalation. Acknowledge, set expectations, route immediately.
Caption: Tier 3 is a routing problem. Tier 1 is a content and action problem. Do not design them as the same article pattern.

---

SLIDE 17 — RG gate
Title: Responsible Gaming gate — hard rule
If a session or account has any active RG flag (cooling off, self-exclusion, limit alert), the agent must:
1. Acknowledge the user warmly and without judgement
2. Surface only RG support resources and state-mandated contacts
3. Not offer or discuss bonuses, promotions, or incentives in the same session
Footer: Non-configurable. If subagents are supported, RG is a hard-gated subagent (or pre-agent policy), not a topic the general agent improvises.

---

SLIDE 18 — Context contract
Title: Continuity lives in this contract
Table:

Parameter | Source | Use
product | App identifier | Product-relevant replies; route to subagent if the model allows
user_id | Authenticated session | Greet by name; skip account lookup
account_status | API on launch | Proactive alert if the account has an issue
last_page | App navigation | “I can see you were looking at withdrawals…”

Caption: Applies whether the surface is the current site, Casey, or an agent opened from the app.

---

SLIDE 19 — Validating next
Title: What we are validating next
1. Same agent in the app and on the Help Portal — and can it support subagents? (Agent teams) — decides A vs B
2. Current chatbot platform / contract (Digital Care)
3. Which app teams own support entry in SBK / Casino / DFS — the context contract needs owners
4. Amplitude / search data — trending and search-gap feedback
5. Content owner for 16 → 5 taxonomy — recommended either way

---

SLIDE 20 — Close
Navy section slide.
Title: Experience first. Agent next. Hosting when the architecture is clear.
Three lines:
• Do the experience work either way.
• Upgrade the chatbot to an Agent.
• Choose independent vs Casey when we know whether the app opens the same agent — with subagents.
Footer: FanDuel Help Portal · Digital Care Assessment · September 2026
