# Team Structures for an Agentic World - Pyramid to Hourglass

An AWS Events keynote by Stephen Brozovich ("A Leader's Guide to Advanced Team Structures in an Agentic World", 2026) on how leaders should reorganize engineering teams around AI agents. The clipped note holds only the talk's blurb; the substance comes from the 44-slide deck attached to it. The talk is built as four questions to be answered **in order: Economics → Talent → Structure → Governance** — the team you need depends on an economic question, and the answer to that question differs per workflow.

A caveat on reading it: this is a vendor keynote. The governance section is in effect a pitch for Amazon Bedrock AgentCore, and most figures come from third-party secondary sources quoted on slides (Ravio, SignalFire, CodeRabbit, Stanford HAI, MIT NANDA and others). The numbers below are the deck's claims, not independently checked.

## 1. Economics — decide per workflow

- **The only moats that matter are "hard to get," not "hard to do."** AI is making workflow embeddedness, software scale, integration lock-in and engineering complexity worthless as moats. What survives is what's bottlenecked by time rather than effort: compounding proprietary data, network effects, regulatory permission, capital at scale and physical infrastructure. *"AI compresses the time it takes to do things. It does not compress the time it takes for things to happen."* The same applies to a team: senior judgment, verification discipline and the apprenticeship pipeline all take years, and AI makes them *more* valuable.
- **The pricing scissors.** Frontier training costs rise about 2.4× a year while inference prices fall about 10× a year, opening a 12–24× gap annually.
- **Three worlds: Use, Compose, Build.** *Use* is packaged SaaS and orchestration tooling; *Compose* is frontier APIs plus custom agents; *Build* is fine-tuned models, local inference and scale economics. Invest in your actual differentiator and "shift undifferentiated work left." The decision is not which world a company lives in but which world *each part of a workflow* lives in: start in one world with a frontier model doing everything, let the economics and data show where to split, and end with one workflow routing across all three.

## 2. Talent — from builders to orchestrators

- **The convergence.** Domain experts get pushed to broaden (AI handles their narrow execution tasks); generalists deepen (AI gives them specialist-grade tools). They meet in what Werner Vogels called the "renaissance developer": deep domain judgment, full-workflow orchestration, AI as force multiplier. The deck lines up supporting voices — Martin Fowler's "expert generalist" and "the job shifts from writing to steering", Jurgen Appelo's M-skilled orchestrators, antirez's "if you were a 10x developer with architectural clarity, that's a brutal advantage when using AI."
- **Domain expertise + AI tools > coding skills alone.** The example is Anthropic's Build with Claude hackathon (February 2026, 13,000 applicants): first place went to a lawyer (a California permitting tool), third to an interventional cardiologist who built a patient follow-up platform in seven days. No top winner was a professional developer.
- **Hyper-convergence.** A specialist squad of 6–8 people, each owning a lane with handoffs between them, gives way to 2–3 expert generalists plus agents, each orchestrating a workflow end to end with agents raising the floor on the skills they don't hold deeply.
- **Four forces, all true at once** — "the leader's job is to hold the tension":
  - *The expert multiplier* — AWS's Project Mantle compressed nine months to 76 days with 5× fewer people, but only for people who understand every line.
  - *The bottleneck shifts* — building becomes instant; deciding what to build, and whether the data exists, becomes the constraint.
  - *The verification tax* — AI generates 10× faster, reviewing it is 3× harder, 2.74× more vulnerabilities.
  - *The deskilling trap* — 17% lower comprehension; juniors ship faster but understand less. "If they never learn without AI, who verifies the AI in 5 years?"
- **The labor market is bifurcating, not shrinking.** Entry-level hiring collapsed 73% year on year in European tech; new-grad hiring is down 50%+ against pre-pandemic; junior headcount fell 7.7% at AI-adopting firms. At the same time, average AI-engineer pay reached $206K, demand for AI-agent skills grew 1,587%, and there were 67K open software-engineering roles against 52K tech layoffs. AWS CEO Matt Garman: *"Replacing juniors with AI is the dumbest thing I've ever heard"* — if you stop training juniors, where do your seniors come from in 5–10 years?

## 3. Structure — four shapes

The centrepiece slide, from today to the target:

| Shape | Layers | Verdict |
|---|---|---|
| **Pyramid** (today) | Leaders / mid-level / juniors | Traditional apprenticeship; juniors learn by doing. Slow but sustainable. |
| **Diamond** (the trap) | Leaders / bloated layer of "agent managers" / few juniors | Cuts juniors, bloats the middle, starves the pipeline. |
| **Inverted pyramid** (the pod) | Seniors / AI agents / 1–2 juniors | What actually works for delivery (Project Mantle) — but there's no learning path. |
| **Hourglass** (the target) | Expanded seniors / lean middle / AI-literate juniors | Sustainable at scale; keeps the pipeline alive; pods run inverted inside it. |

**"The hourglass is the org. The inverted pyramid is the pod."** Both are true at different levels of the organization: delivery pods are senior-heavy with AI doing execution, while the organization housing them deliberately keeps a junior base.

The talk then maps this onto operating models, arguing that traditional organizations optimize for determinism while agents make non-determinism "a feature, not a bug":

- **Model A — traditional IT ops (the anti-pattern).** Engineering "changes" and builds agents; a separate operations team "runs" them. But debugging an agent means reading the system prompt, context window, retrieval quality, tool results and reasoning chain — traditional ops sees "HTTP 500." The test: can your ops team debug agent behaviour, or do they restart services and file tickets? If the latter, you have Model A whatever the team is called. Cited alongside: 97% of large enterprises have budget for agentic AI but only 18% have deployed it — "the barrier isn't technology, it's operational model."
- **Model B — embedded pods (the default, strengthened).** You build it, you run it: 3–5 senior engineers per pod, each pod owning one workflow end to end, including its own on-call rotation. The inverted pyramid makes this *more* natural — seniors already understand production, agents absorb the L4/L5 execution work (boilerplate, bug triage, first-pass code), and the verification tax stays inside the team, since the people who write the prompts review the output. Dan Shipper's framing: two-pizza team → one-pizza team → "two-slice team."
- **Model C — platform (the essential complement).** Pods can't run agents end to end without a shared platform supplying agent runtime, guardrails and policy-as-code, observability, and RBAC/quotas — agents as "first-class platform citizens." The platform "provides the road, not the destination": pods still choose what to build, which models, how to structure agents and how to use their data.

Verdict: Model A is dead — "the build/run split was already the anti-pattern; AI widens the gap."

## 4. Governance — governance as infrastructure

The anchor is **Singapore's Model AI Governance Framework for Agentic AI** (IMDA, January 2026, launched at Davos), described as the first state-backed governance framework for autonomous agents. Its four dimensions are to assess and bound risks upfront, keep humans accountable, apply technical controls across the lifecycle, and be transparent to end users. What the talk says sets it apart: every agent gets a unique identity linked to a human supervisor, and its permissions can never exceed that human's; five risk categories (erroneous, unauthorised and biased actions, data breaches, disruption to connected systems); explicit treatment of multi-agent risk, where errors cascade; not legally binding yet; and an explicit requirement that organisations prevent **deskilling** as agents take over routine work — the same worry as the talent section, now written into governance.

The talk turns this into four questions every agent must answer before it acts — *who is it, what is it allowed to do, is it working correctly, can we prove it* — and maps each to an AgentCore service (identity and gateway, Cedar policy, evaluations and observability, CloudWatch audit trails). The general point underneath the product pitch: **policy-as-code enforced outside the LLM loop**, intercepting every tool request at a gateway, is deterministic and can't be circumvented by prompt injection — "the layer most frameworks miss."

## What to do Monday morning

1. **Economics** — map every AI initiative to Use, Compose or Build; most will be Use or Compose. If you're building, re-check the economics quarterly.
2. **Talent** — assemble pods of three to five senior engineers at most. If you can't staff a senior-only pod, you're not ready to Build.
3. **Structure** — escape Model A: form one embedded pod with end-to-end ownership, no handoffs, no separate ops.
4. **Governance** — pipeline guardrails, policy-as-code, embedded security specialists, and senior sign-off on AI-assisted production changes. All four.
5. **People** — grow senior domain experts: the person who understands your customer, regulation and product nuance is what AI can't compress.
6. **Pipeline** — don't stop hiring juniors. The pyramid inverts but keeps its base; redesign what juniors learn on, because the need for judgment built through experience doesn't change.

## Cross-reference

[[Why Most Enterprise Agentic Projects Are Doomed]] reaches the same diagnosis from the delivery side — the barrier is the operating model, not the technology — and prescribes the same cure of turning approval into executable code. This page supplies the org chart that goes with it: Accenture's two-weeks-to-build, twelve-months-to-ship project is what Model A looks like in practice, and Model B's embedded pod with a governance platform underneath is the structural answer.

[[The Last Solo Programmers]] is the individual-scale version of this talk's deskilling trap. Baquero worries about the prompt-only programmer who can't catch the AI's mistakes; this talk asks where the organization's verifiers come from once nobody hires juniors, and answers with the hourglass — a structural fix for what that essay treats as a matter of personal craft.

[[Cory Doctorow - The Reverse-Centaur Critique of AI]] describes exactly the move this talk calls the trap: fire the skilled workforce, keep a survivor as an accountability sink. A hyperscaler's CEO calling junior replacement "the dumbest thing I've ever heard" is a notable counterpoint from the vendor side — though the talk's answer is still smaller, senior-heavy teams, which is not the arrangement Doctorow wants either.

[[Defending Against Destructive AI Agents]] makes the same rule at developer scale that this talk's governance layer makes at enterprise scale: don't rely on the model behaving, enforce limits outside it — a sandbox and no production credentials there, a policy gateway and human-bounded agent identities here.

[[Who Will Check the AI]] builds on this talk's deskilling trap and junior-pipeline argument, setting the hourglass beside the personal, process and labor-side remedies other pages propose. It also names the trade-off the talk only implies: the inverted-pyramid pod that verifies best today is the one with no learning path for tomorrow's verifiers.

## Sources
- [AWS - Advanced team structures in an agentic world](<../../source/AWS - Advanced team structures in an agentic world.md>)

#ai-insights #future-of-work #ai-labor #enterprise-ai #agents #developer-skills
