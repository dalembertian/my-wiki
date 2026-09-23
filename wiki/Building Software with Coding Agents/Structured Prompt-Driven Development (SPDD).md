# Structured Prompt-Driven Development (SPDD)

A method from Thoughtworks' internal IT teams (Global IT Services), published on martinfowler.com, for making LLM-assisted code changes **governable, reviewable, and reusable**. Its starting observation: AI assistants speed up the individual, but delivery throughput is set by the whole lifecycle — ambiguous requirements turn into code faster, reviews have more change to absorb, "generated" doesn't mean "aligned." *"It's like buying a Ferrari and driving it on muddy roads."*

The move SPDD makes is to treat **prompts as first-class delivery artifacts**: version-controlled, reviewed, reused and improved, rather than ad hoc chat. Birgitta Böckeler's taxonomy classes it as a *spec-anchored* approach — the same starting point as Spec-Driven Development, but with the spec living on as a governed team asset that evolves alongside the code.

## The REASONS Canvas

A fixed seven-part template that carries a prompt from intent → design → execution → governance:

- **R — Requirements**: what problem, and what is "done"?
- **E — Entities**: domain entities and relationships.
- **A — Approach**: the strategy for meeting the requirements.
- **S — Structure**: where the change fits; components and dependencies.
- **O — Operations**: the abstract strategy broken into concrete, testable implementation steps — precise down to method signatures and execution order.
- **N — Norms**: cross-cutting engineering conventions (naming, observability, defensive coding).
- **S — Safeguards**: non-negotiable boundaries (invariants, performance limits, security rules).

The first four are abstract (intent and design), `O` is execution, and `N`/`S` are governance. Because every prompt has the same shape, reviewers reason about one artifact instead of scattered chat logs and partial diffs, and domain knowledge accumulates in a reviewable place.

## The workflow, and its one hard rule

**When reality diverges, fix the prompt first — then update the code.** The loop closes on two scales: within an iteration (logic corrections flow requirements → prompt → code; refactorings flow code → prompt), and across iterations (last cycle's canvas is next cycle's starting context).

The steps are implemented as commands in `openspdd`, a CLI: `/spdd-story` (split a requirement into INVEST stories), `/spdd-analysis` (extract domain keywords, scan only the relevant code, produce concepts/risks/direction), `/spdd-reasons-canvas` (generate the canvas), `/spdd-generate` (code, task by task, no improvisation), `/spdd-api-test` (a cURL script covering normal/boundary/error cases), `/spdd-prompt-update` (requirements → canvas) and `/spdd-sync` (code → canvas). Most of the article is a worked end-to-end example: adding model-aware pricing and a new Premium plan to a token-billing service.

Two sequencing choices are argued for explicitly:

- **Six checkpoints instead of one plan-then-code review**, because a single post-plan review asks more sustained attention than reviewers actually have — they skim, defer, or approve by default. Narrower decisions per step is the point, not more steps.
- **API tests before code review, unit tests after** — almost the inverse of TDD. Generated code is cheap, so validate the behaviour at the system boundary before spending human review on it; then review what only humans can judge; then write unit tests last, once the implementation has stopped moving.

## Three skills, and where the method doesn't pay

The skills the authors say developers now need are **abstraction first** (know the objects, collaborations and boundaries before generating), **alignment** (make "what we will / won't do" explicit up front), and **iterative review** (a controlled loop rather than patching a drifting one-shot draft).

The fitness table is unusually candid about the method's limits. Five stars for scaled standardized delivery and for high-compliance/hard-constraint environments; two stars for firefighting hotfixes, exploratory spikes and one-off scripts, where the governance overhead can't pay back; one star for "context black holes" (domains whose rules are unclear — *"a more powerful AI does not fix this; it just fails more confidently"*) and for taste-driven creative work. The upfront investment is a design-first mindset shift, senior expertise per feature, and automation tooling without which SPDD hits a throughput ceiling.

The post-publication Q&A concedes two open weaknesses rather than defending them. Two developers writing the same canvas still produce different specs and there is no objective standard for a "good" one — the canvas narrows the variance band rather than eliminating it, so *"human judgement is still load-bearing"* until automated verification exists at the asset layer. And SPDD does not claim determinism: it keeps the model's non-determinism "within controllable bounds." Hotfixes, rated a poor fit, are handled by deferring governance one step — fix first, then fold the failure mode back into the canvas in a post-mortem, which is also how SPDD coverage grows over legacy code.

## Cross-reference

The clearest way to place SPDD is against [[Harness Engineering - Guides, Sensors, and Regulation Categories]], published on the same site: the REASONS Canvas is an unusually formal *guide*, and `/spdd-api-test` plus the final unit tests are *sensors*. Read together, SPDD is a concrete answer to that essay's "elephant in the room" — the behaviour harness — by making intent explicit enough to check against, while inheriting the same unresolved problem it flags, since nothing mechanically verifies that the canvas itself matches the business need.

[[Loop Engineering]] sits at the opposite end of the automation dial: there the human designs a loop that prompts the agent unattended, here the human is deliberately kept as the gatekeeper at six checkpoints. Both close a loop with persistent state — a canvas versus a memory file — and both land on the same caveat, that verification and comprehension stay with the human; SPDD's `/spdd-code-review` command exists but is explicitly built to handle the mechanical half of review, not to take it over.

Pattern 1 of [[Nine Emerging Developer Patterns for the AI Era]] — "AI-native Git," where the source of truth shifts upstream from the diff to the prompt plus tests — is the prediction SPDD implements in detail, down to committing the prompt beside the code and syncing it back after every refactor.

[[Karpathy's 'Think Before Coding' Skills File]] is the same instinct at the opposite scale: 65 lines of personal convention versus a seven-part template plus a CLI and a review workflow. The contrast is informative about evidence, too — that file's author couldn't tell whether it helped, while SPDD's claims rest on a team's internal practice and an acknowledged absence of objective canvas quality measures.

[[The Last Solo Programmers]] gives the skill-erosion argument SPDD answers directly: the method's stated third benefit is that it *forces* developers to keep modelling and abstracting alongside the tool, so judgement compounds instead of atrophying — the "craftsman" mode of AI use, turned into an enforced workflow rather than left to personal discipline.

## Sources
- [Structured-Prompt-Driven Development (SPDD)](<../../source/Structured-Prompt-Driven Development (SPDD).md>)

#developer-tools #harness-engineering #prompt-engineering #agents #spec-driven
