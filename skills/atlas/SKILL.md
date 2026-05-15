---
name: atlas
description: Routes a product management situation to the right LLM reasoning technique and assembles a copy-ready structured prompt. Use this skill when the user describes a PM problem, decision, analysis, or challenge — "what should I build", "I need to prioritize", "how do I validate this", "our activation is dropping", "I need to position against X", "help me build an OST", "which LLM pattern should I use". Also activates when the user invokes /atlas explicitly.
---

This skill maps a PM situation to the right reasoning technique(s) from the LLM Reasoning Atlas and produces a fully structured, copy-ready prompt.

## What you receive

The user describes their PM situation. It may be a decision, a problem, a deliverable, or a question. It may be brief ("I need to prioritize my roadmap") or detailed (full context).

## Step 1 — Classify the primary situation

Identify the dominant situation type from the user's input:

| Situation ID | Key signals |
|---|---|
| `discover` | "what to build next", "opportunity space", "user research", "what problem", "backlog ideas", "feature request noise" |
| `prioritize` | "what should we do first", "roadmap", "trade-offs", "capacity", "rank these", "RICE", "what deserves investment" |
| `validate` | "should we build this", "is there demand", "assumption", "test before", "MVP", "before we commit", "de-risk" |
| `differentiate` | "why choose us", "against competitors", "positioning", "market", "differentiation angle", "category" |
| `improve` | "metric dropped", "churn", "activation down", "retention", "why is X moving", "diagnose", "root cause" |
| `launch` | "go-to-market", "launch", "adoption", "users not using it", "enablement", "message", "drive usage" |
| `strategy` | "where to play", "long-term direction", "Wardley", "platform vs. point solution", "moat", "expansion" |
| `decision` | "irreversible", "make or break", "high-stakes", "bet", "board decision", "cut or double down" |
| `measure` | "HEART", "KPIs", "product health", "metrics framework", "NSM", "what to track", "UX measurement" |
| `llm-tool` | "how to prompt", "LLM reasoning", "agent pattern", "better output", "Tree of Thoughts", "ReAct" |

If the situation spans two types (e.g., prioritize + validate), route to a **chain** rather than a single technique.

---

## Step 2 — Route to technique(s)

### Primary routing table

**discover →**
- Primary: `Opportunity Solution Tree` + `JTBD decomposition`
- Supporting: `Customer struggle mining`, `Weak signal synthesis`, `Riskiest assumption test`
- Boost: evidence-grounded synthesis — quote → inferred need → opportunity → risk → test

**prioritize →**
- Primary: `Cost of Delay` or `Kano model` (use Kano if feature type is unclear; Cost of Delay if business urgency is the driver)
- Supporting: `RICE critique`, `Bet sizing`, `WSJF` (if SAFe context)
- Boost: rubric calibration — define scoring anchors before ranking, then challenge low-confidence scores

**validate →**
- Primary: `Riskiest assumption test` + `Assumption mapping`
- Supporting: `MVP laddering`, `Smoke test design`, `Kill criteria definition`
- Boost: hypothesis-first — falsifiable assumption → threshold → minimum test → decision rule

**differentiate →**
- Primary: `Contrarian positioning` + `Competitive teardown`
- Supporting: `Underserved segment mining`, `Substitute analysis`, `Strategic narrative building`
- Boost: adversarial positioning — compare direct, substitutes, non-consumption before writing claims

**improve →**
- Primary: `Metric tree decomposition` + `Funnel diagnosis`
- Supporting: `Segmented diagnosis`, `Cohort reasoning`, `Difference-in-differences`
- Boost: causal decomposition — drivers, confounders, counterfactuals, segment effects, leading indicators

**launch →**
- Primary: `Message-market fit` + `Adoption path design`
- Supporting: `Launch tiering`, `Enablement generation`, `Activation campaign reasoning`
- Boost: objection-first — list adoption blockers before writing narrative or enablement

**strategy →**
- Primary: `Wardley Mapping` + `Opportunity Solution Tree`
- Supporting: `First-principles reasoning`, `Moat reasoning`, `Cynefin framing`, `Strategic option set`
- Boost: multi-lens — customer, market, moat, channel, trade-off lenses before recommending

**decision →**
- Primary: `Pre-mortem` + `Reversibility analysis`
- Supporting: `Red team`, `Pre-parade`, `Bet sizing`, `Cynefin framing`, `Decision memo`
- Boost: adversarial pre-mortem — failure modes, disconfirming evidence, second-order effects, stop conditions

**measure →**
- Primary: `HEART framework` + `North Star input tree`
- Supporting: `HEART-GSM metric design`, `Leading vs lagging indicators`, `Proxy metric validation`
- Boost: causal decomposition — connect UX dimensions to NSM input metrics with behavioral signals

**llm-tool →**
- Primary: route by sub-need (see LLM tool selection below)
- Supporting: combine with the domain technique the user is working on

### LLM tool sub-routing

| Need | Recommended tool |
|---|---|
| Exploring multiple angles before deciding | `Tree of Thoughts` |
| Step-by-step multi-stage analysis | `ReAct (Reason + Act)` |
| First draft is shallow, needs depth | `Reflexion loop` |
| Output must feed into a system or tracking tool | `Structured generation` |
| Preventing hallucination, anchoring on docs | `RAG grounding` |
| Challenging a decision with opposing views | `Multi-agent debate` |
| Constraining the reasoning to principles | `Constitutional prompting` |

---

## Step 3 — Assemble the structured prompt

Build the output in this exact order:

### Output structure

```
## Technique: [Technique Name]
**Category:** [Family]
**Situation:** [One-line description of what this solves]
**Origin:** [Lineage / where this comes from]
**When to use:** [One-line benefit statement]

---

## Recommended chain (if complex situation)
[Only if routing to a chain: list technique order with one-line purpose for each step]

---

## Copy-ready prompt

[Full assembled prompt — see template below]

---

## Also consider
- [Technique 2] — [one line why]
- [Technique 3] — [one line why]

## Skip if
- [Condition where this technique is the wrong choice]
```

### Full prompt template

Assemble the copy-ready prompt using this structure:

```
Role:
You are a senior product strategist. You reason rigorously, separate evidence from assumptions, and optimize for actionable product decisions.

Product objective:
Use the "[TECHNIQUE]" technique to help with: [USE CASE from technique].

Input context (fill before pasting):
- Product / feature:
- Target users / segment:
- Current situation or workflow:
- Available metrics or data:
- User evidence / verbatims:
- Business constraints:
- Decision deadline:

Technique to apply:
[TECHNIQUE NAME]

Principle:
[PRINCIPLE]

Prompt kernel:
[PROMPT KERNEL from technique]

Context-quality-gate:
Before answering, run a context-quality-gate:
1. List the context I provided that is sufficient for the objective.
2. List the missing or weak context that would materially improve the answer.
3. Separate facts, assumptions, and inferences.
4. If critical context is missing, ask up to 5 targeted clarification questions.
5. If the task can proceed, state your working assumptions and mark confidence High / Medium / Low.

Prompt engineering boost:
[BOOST for the technique's category]

Reasoning protocol:
1. Restate the objective in PM language.
2. Identify the decision this analysis must enable.
3. Apply the technique step by step with visible intermediate reasoning.
4. Distinguish user value, business value, feasibility, and risk.
5. Challenge your recommendation with a brief red-team pass.

Output requirements:
- Use a structured table where options are compared.
- Separate facts, assumptions, inferences, and recommendations.
- Rank options by impact, confidence, effort, risk, and evidence strength.
- Include the context that would change the recommendation.
- End with: next action, fastest validation test, success metric, kill criteria.

Quality bar:
- Do not invent evidence. If missing, say so.
- Prefer precise trade-offs over generic best practices.
- Make the answer directly usable in a roadmap, discovery, or decision memo.
```

---

## Technique reference (inline — do not read from external files)

### Prompt engineering boosts by category

| Category | Boost |
|---|---|
| ideation | Divergent-convergent: generate many options first, then force ranking with explicit constraints and rejection criteria. |
| discovery | Evidence-grounded synthesis: separate raw quotes, inferred needs, confidence level, and follow-up questions. |
| strategy | Multi-lens reasoning: customer, market, moat, channel, and trade-off lenses before recommending. |
| prioritization | Rubric calibration: define scoring anchors before ranking, then challenge low-confidence scores. |
| experimentation | Hypothesis-first: falsifiable assumption → decision threshold → minimum test → kill criteria. |
| metrics | Causal decomposition: drivers, confounders, counterfactuals, leading indicators instead of metric commentary. |
| ux | Cognitive walkthrough: simulate user intent, attention, decisions, errors, and recovery at each step. |
| competition | Adversarial positioning: direct competitors, substitutes, non-consumption, then force a contrarian angle. |
| delivery | Specification stress testing: inspect as engineering, design, support, sales, and security. |
| monetization | Segment simulation: evaluate pricing separately for buyer, user, admin, and economic sponsor. |
| gtm | Objection-first messaging: list adoption blockers before writing narrative or enablement assets. |
| risk | Adversarial pre-mortem: failure modes, disconfirming evidence, second-order effects, stop conditions. |

### Key technique prompt kernels (most-used)

**Opportunity Solution Tree:**
Build an Opportunity Solution Tree: start from this desired outcome, list customer opportunities underneath, generate solution options per opportunity, then map the smallest experiment for each solution branch.

**JTBD decomposition:**
Analyze this segment with Jobs-to-be-Done: functional job, emotional job, social job, triggers, anxieties, and alternatives considered.

**Cost of Delay:**
Estimate the cost of delay for each initiative: lost revenue, churn, strategic risk, delayed learning. Rank by urgency-weighted value.

**Kano model:**
Classify these features into Must-be, Performance, and Delight categories. Explain the implications for roadmap sequencing and investment depth.

**Wardley Mapping:**
Draw a Wardley Map: user need, value chain components, evolution stage (genesis → commodity), strategic implications for build, buy, or partner.

**Riskiest assumption test:**
Which single assumption would invalidate this entire initiative? Propose the fastest possible test. Define the success threshold and kill criteria.

**HEART framework:**
Apply HEART to this product surface: propose 1-2 metrics per dimension (Happiness, Engagement, Adoption, Retention, Task success) and identify the leading indicator for each.

**Pre-mortem:**
We launched this initiative and it failed. Give the 10 most likely root causes, ranked by probability. For each, identify the earliest warning signal and the preventive action.

**Cynefin framing:**
Classify this situation (Clear, Complicated, Complex, Chaotic). What does the classification imply for the decision method, acceptable risk, and required evidence threshold?

**Tree of Thoughts:**
Generate 3 distinct reasoning paths. Evaluate each path's logic, evidence quality, and risks. Converge on the strongest and explain why the others were rejected.

**Multi-agent debate:**
Agent A argues FOR this strategy (3 arguments). Agent B argues AGAINST (3 counter-arguments). Each responds to the other's best point. Synthesis agent writes a final judgment: what is defensible, what is not, what evidence resolves the disagreement.

**Reflexion loop:**
Generate a first answer. Critique it: list 3-5 specific weaknesses. Produce an improved version that explicitly addresses each critique.

---

## Tone and format rules

- **Be directive**: name the technique, explain the routing decision in one sentence, then give the prompt.
- **Never list all 148 techniques**: route to 1-3 and explain why.
- **If the situation is ambiguous**: ask one clarifying question before routing — "Is the primary challenge [X] or [Y]?" — then route.
- **If the user provides rich context**: skip the question and route directly with the assembled prompt.
- **Chain vs. single technique**: use a chain only if the situation genuinely requires multiple reasoning passes (e.g., OST → assumption mapping → MVP laddering is a natural sequence for complex discovery). Do not chain for simple, focused asks.
- **LLM tool pairing**: when a domain technique is identified AND the user's context suggests a reasoning challenge (complex, multi-path, needs rigor), pair with the appropriate LLM tool and explain the combination.
