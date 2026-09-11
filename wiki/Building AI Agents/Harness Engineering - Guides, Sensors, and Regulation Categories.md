# Harness Engineering: Guides, Sensors, and Regulation Categories

Martin Fowler's essay formalizing "harness engineering" — the article that [[Agent Harnesses]] cites as canonizing the term (Agent = Model + Harness). That earlier page covers the *market* for harness infrastructure; this one covers the *engineering discipline* of building a good outer harness as a coding-agent user, on top of whatever harness the agent vendor already ships.

## The building blocks: guides and sensors, computational and inferential

A harness feeds the agent **guides** (forward-looking input: conventions, specs, reference docs) and checks its output with **sensors** (feedback: tests, static analysis, review). Both come in two flavors:

- **Computational** — deterministic, fast, cheap, run by the CPU (linters, type checkers, structural tests). Reliable, but blind to meaning.
- **Inferential** — semantic, slower, non-deterministic, run by a GPU/NPU (AI code review, "LLM as judge"). Can judge things computational tools can't (over-engineering, redundant tests) but expensively and probabilistically — not something you run on every single commit.

## The steering loop

The human's actual job isn't reviewing every agent output line by line — it's **steering**: whenever an issue recurs, improving the guides and sensors so it becomes less likely next time. This loop is itself acceleratable by AI — agents can help write structural tests, draft rules from observed patterns, or scaffold custom linters.

## Keep quality left

Distribute checks across the development timeline by cost and speed: fast, cheap checks (linters, quick tests) run before or at commit time; expensive checks (mutation testing, broad architectural review) run post-integration. Separately, some drift accumulates gradually and needs sensors running continuously outside any single change's lifecycle — dead code, degrading test coverage quality, dependency staleness, runtime SLO drift.

## Three regulation categories

- **Maintainability harness** — the easiest today, since mature tooling already exists. Computational sensors reliably catch duplicate code, complexity, missing coverage, architectural drift. Inferential sensors can partially catch semantic issues (over-engineering, redundant tests) but expensively and non-reliably. Neither reliably catches the highest-impact failures — misdiagnosis, unnecessary features, misunderstood instructions — because correctness ultimately depends on the human having specified intent clearly in the first place.
- **Architecture fitness harness** — guides and sensors that check architectural characteristics (performance, observability) hold — essentially [Fitness Functions](https://www.thoughtworks.com/en-de/radar/techniques/architectural-fitness-function) applied to agent output.
- **Behaviour harness** — "the elephant in the room": does the app actually do what's functionally needed? Current common practice: feed forward a spec, feed back whether the AI-generated test suite passes and has reasonable coverage, plus manual testing — which the author flags as putting more faith in AI-generated tests than is currently warranted. The "approved fixtures" pattern is cited as a partial, selectively-applicable improvement, not a general solution.

## Harnessability

Not every codebase is equally harnessable. Strong typing gives you type-checking as a sensor for free; clear module boundaries enable architectural constraint rules; opinionated frameworks implicitly raise the agent's odds of success by hiding detail it doesn't need to worry about. This creates an uncomfortable asymmetry: **greenfield teams can bake harnessability in from day one** via their tech/architecture choices, while **legacy teams with the most accumulated debt need harnessing most but can build it least easily**.

## Harness templates (a speculative direction)

Most enterprises have a handful of common service topologies (data dashboard, CRUD service, event processor) already codified as templates. These could evolve into **harness templates** — a bundle of guides/sensors pre-matched to a topology's structure and stack — with teams eventually picking tech stacks partly based on which harness templates already exist for them. The article expects this to face the same drift/versioning problems service templates already have, likely worse given non-deterministic components.

## Open questions the author flags

How do you keep a growing harness internally coherent as guides and sensors accumulate? How much should agents be trusted to resolve conflicting instructions and feedback signals? If a sensor never fires, is that high code quality or inadequate detection? The field currently lacks something like code/mutation coverage, but for **harness coverage and quality** itself.

## Cross-reference

See [[Agent Harnesses]] for how the major AI labs are packaging and pricing pieces of this same layer as a product.


Applied instances of this discipline elsewhere in the wiki: [[LangChain's Middleware Model for Custom Agent Harnesses]] implements guides and sensors as composable middleware; [[Loop Engineering]] extends the steering loop across many runs rather than within one; [[Defending Against Destructive AI Agents]] is the safety-critical case, where the sensors exist to catch destructive actions before they land; [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is the largest demonstration available, with a compiler as the sensor and a shared DAG as the guide; and [[AI Agent Learning Roadmap and Resources]] collects the material for learning to build these.


[[NVIDIA's Agent Infrastructure Bet]] is this same decomposition showing up in a hardware vendor's keynote — evidence the discipline described here is converging into an industry-standard mental model.


[[The Harness Is the Product]] places this essay as the engineering vantage point of the wider claim, and picks up the behaviour-harness problem flagged here as the cluster's central unsolved question.

## Sources
- [Harness engineering for coding agent users](<../../source/Harness engineering for coding agent users.md>)

#ai-techniques #harness-engineering #agents #developer-tools
