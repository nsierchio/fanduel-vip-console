# Gemini Prompt — Agentforce FAQ Agent Build Document

You are a senior Salesforce Agentforce solution architect, UX strategist, and technical writer. Using the attached source specification, create an internal review and decision document for a proposed FanDuel customer-facing FAQ Agentforce build.

The audience is:

- FanDuel Digital Care and Customer Experience leadership
- Salesforce / Agentforce architects
- Salesforce Build and Engineering teams
- Knowledge management and content owners
- Compliance, Responsible Gaming, and Operations stakeholders

The document must be useful for internal review and shareable with stakeholders who need to understand what should be built, what decisions are still open, and what FanDuel must validate before implementation.

## Source material

Use the attached file:

`faq-external-agent-spec.md`

Treat that file as the primary source of truth. Do not silently change its locked decisions, intent taxonomy, compliance requirements, or channel assumptions.

## Objective

Produce a polished document titled:

**FanDuel Agentforce FAQ Agent — Build Definition and Readiness Assessment**

The document should translate the experience brief into a practical Agentforce build definition without pretending that unresolved platform or business questions have already been answered.

## Required content

### 1. Executive summary

Explain:

- What the FAQ Agent is
- Who it serves
- Why FanDuel should build it
- What the first release should and should not attempt
- The most important decisions required before build begins

Keep this section concise and suitable for leadership review.

### 2. Proposed solution overview

Describe the target Agentforce experience across:

- EC Help Portal
- Web widget, if validated
- In-app support entry points, if validated

Explain how the conversational agent works on top of EC Knowledge rather than replacing the knowledge content layer.

Clearly distinguish:

- Customer-facing FAQ Agent
- Internal KAM Console Agentforce workstream
- Human support agents
- Any potential subagents or specialized routing agents

### 3. Scope and release boundaries

Create a clear in-scope / out-of-scope section.

Include the three intent tiers:

- Tier 1 — Full self-service
- Tier 2 — Guided plus hand-off
- Tier 3 — Necessary Transfer

For each intent in the source specification, describe the expected Agentforce behavior and whether it is appropriate for an initial release.

Do not treat Tier 3 intents as failed self-service. Explain that routing to a human is the intended outcome.

### 4. Agentforce solution architecture

Propose a platform-neutral Agentforce architecture, clearly labeling any items that require Salesforce validation.

Discuss, as applicable:

- Agentforce agent and topic structure
- Intent/topic boundaries
- Actions and flow orchestration
- Apex or API integrations
- Salesforce Knowledge / EC Knowledge relationship
- Authentication and user context
- Session context
- Deep links into FanDuel product flows
- Human handoff and transcript/context transfer
- Analytics and reporting
- Guardrails and policy enforcement

Do not invent object names, API names, existing integrations, licenses, or Salesforce features. Mark these as **TBC** or **Requires Salesforce validation** where necessary.

Include a conceptual architecture diagram using Mermaid if the document format supports Mermaid. If Mermaid is not appropriate, provide a clearly labeled text-based architecture diagram.

### 5. Topic and intent design

Create a table mapping each source intent to:

- Tier
- Agent topic or topic family
- User goal
- Agent behavior
- Required action or integration
- Escalation condition
- Knowledge/content dependency
- Open question or dependency

Use the source taxonomy as the baseline. You may recommend better Agentforce topic groupings, but explain any proposed changes.

### 6. Conversation experience

Define:

- Zero-state experience
- Recommended conversation starters
- Greeting variants for app context, authenticated Help Portal users, and unauthenticated visitors
- How the agent confirms intent
- How it answers procedural questions
- How it provides deep links
- How it confirms resolution
- How it offers escalation
- How it handles ambiguity
- How it handles repeated failed attempts
- How it handles unsupported requests

Provide short example conversations for:

1. Password reset — successful Tier 1 resolution
2. Withdrawal delay — Tier 2 guidance followed by escalation
3. Account locked — immediate Tier 3 transfer
4. Fraud or unauthorized access — immediate protected route
5. Active Responsible Gaming flag — RG-only experience

Keep examples realistic, concise, and consistent with the source specification.

### 7. Responsible Gaming and safety controls

Treat the Responsible Gaming gate as a non-negotiable policy requirement.

Describe:

- When the gate fires
- What the general FAQ Agent must not do
- What content and contacts may be shown
- How bonuses, promotions, and incentives are suppressed
- Whether a pre-agent policy, hard-gated topic, or specialized subagent is preferable
- State-specific compliance questions that FanDuel must resolve
- Auditability and testing expectations

Do not provide legal advice. Identify compliance review requirements.

### 8. Context-passing contract

Document the proposed handling of:

- `product`
- `user_id`
- `account_status`
- `last_page`

For each parameter, explain:

- Source system
- Expected format or semantic meaning
- How Agentforce uses it
- Privacy/security concern
- Validation owner
- Failure behavior when missing or invalid

Do not invent a token format or claim that URL parameters are secure for sensitive data. Call out the need for secure session or server-side validation.

### 9. Knowledge and content readiness

Explain the content requirements for Agentforce-quality answers:

- Numbered steps
- One action per step
- Explicit decision branches
- Deep links
- Short headings and scannable sections
- Product and jurisdiction variations
- Necessary Transfer content

Create a content readiness checklist and identify the highest-priority article families for rewrite.

Also describe:

- Knowledge ownership
- SME review cadence
- Change management
- Article performance signals
- How poor content quality affects containment and escalation

### 10. Human handoff and operating model

Define:

- Immediate escalation triggers
- Escalation after failed resolution attempts
- Customer expectation-setting
- Information/document preparation
- Context and transcript transfer
- Queue or routing questions
- What happens if live human support is unavailable
- Ownership after handoff

Never invent wait times or service-level commitments.

### 11. Measurement and analytics

Define the recommended measurement framework, including:

- Containment rate
- Tier 1 resolution rate
- Escalation rate by intent
- Session repeat rate
- RG gate firing rate
- Fallback / no-answer rate
- Knowledge article citation or usage signals
- Customer satisfaction, if available

Explain why raw chat deflection is not sufficient and why Tier 3 outcomes should not be counted as agent failures.

Identify required Salesforce, Agentforce, Help Portal, and analytics instrumentation. Mark integration availability as TBC where appropriate.

### 12. Build readiness assessment

Create a readiness assessment grouped into:

- Business decisions
- Salesforce/platform decisions
- Integration dependencies
- Knowledge/content dependencies
- Compliance and Responsible Gaming
- UX and channel dependencies
- Analytics and operations

For each item, include:

- Status: Ready, Partially ready, Blocked, or TBC
- Why it matters
- Owner or stakeholder group
- Evidence needed to close it

Base statuses only on the source material. Do not infer that a dependency is complete.

### 13. Recommended build sequence

Provide a high-level sequence of work, without detailed project plans, dates, story points, or cost estimates.

The sequence should cover:

1. Validate channel and same-agent/subagent decisions
2. Confirm scope and escalation policy
3. Prepare priority knowledge content
4. Define secure context and integration contracts
5. Configure topics, instructions, actions, and guardrails
6. Implement human handoff
7. Test functional, safety, compliance, and accessibility scenarios
8. Instrument measurement
9. Pilot and review outcomes

This is a capability/build sequence, not a delivery schedule.

### 14. Risks and open decisions

Create a prioritized risk and decision register.

At minimum, include:

- Same-agent versus separate in-app agent
- Existing chatbot platform and handoff capability
- Secure context passing
- Knowledge article readiness
- State-specific Responsible Gaming requirements
- Human queue and transcript ownership
- Authentication and account-status lookup
- Analytics availability
- Agent persona/name
- Localization requirements

For each item, state the consequence of leaving it unresolved.

### 15. Final recommendation

End with:

- A concise recommended target state
- A proposed initial release boundary
- The top five decisions FanDuel should make next
- A go/no-go readiness conclusion using only the evidence in the source specification

## Writing and formatting rules

- Write in clear, direct business English.
- Make the document polished enough to circulate internally.
- Use headings, concise paragraphs, and tables where they improve scanability.
- Avoid hype and unsupported claims.
- Do not use “AI will” language without identifying the required data, content, integration, or control.
- Distinguish clearly between:
  - Confirmed
  - Assumed
  - TBC
  - Requires Salesforce validation
  - Requires FanDuel compliance validation
- Preserve the source specification’s use of `[ASSUMED]` and `[TBC]`, while improving readability.
- Do not include pricing, staffing estimates, timelines, story points, or a final platform recommendation unless directly supported by the source.
- Do not claim that any Agentforce capability exists in the current FanDuel environment without evidence.
- Do not expose sensitive customer data in examples.
- Do not provide legal or regulatory conclusions; identify the review required.

## Quality check before delivering

Before presenting the document, verify that:

1. Every source intent appears in the scope or intent design.
2. Tier 3 intents are routed rather than unnecessarily resolved by the agent.
3. Responsible Gaming controls are explicit and enforceable.
4. Context passing does not rely on insecure assumptions.
5. Knowledge readiness is treated as a build dependency.
6. Human handoff includes context transfer and ownership questions.
7. Open questions are visible rather than silently resolved.
8. The document is useful to both leadership and the Salesforce Build team.

Return only the completed internal document, with no discussion of these instructions.
