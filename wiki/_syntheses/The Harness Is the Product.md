# The Harness Is the Product

The single most corroborated claim in this wiki: **the model is becoming the commodity, and the scaffolding around it is where the value, the difficulty, and the differentiation now sit.** "Agent = Model + Harness" — everything except the model itself.

What makes this worth a synthesis rather than a tag is that it is not one argument repeated. Four parties with different incentives, reasoning from different evidence, arrive at the same decomposition — and then all of them run into the same wall.

## Four independent vantage points

- **The market.** By April 2026 four frontier labs converged on the harness being the product and diverged only on how to price it — Anthropic hosting it at $0.08/session-hour, OpenAI giving the runtime away and letting you bring compute, Google and Microsoft metering it by the piece. Convergence on *what matters* with disagreement on *how to charge* is the strongest available signal that the layer is real. See [[Agent Harnesses]].
- **The engineering discipline.** Martin Fowler's formalization: guides (forward-looking context) and sensors (feedback), each computational or inferential, wired into a steering loop where the human's job is improving the guides and sensors rather than reviewing every output. See [[Harness Engineering - Guides, Sensors, and Regulation Categories]].
- **The framework layer.** LangChain's middleware model turns exactly those pieces into composable code — Filesystem, Memory, Skills, SubAgent, TodoList — which is what the abstract discipline looks like once someone has to ship it. See [[LangChain's Middleware Model for Custom Agent Harnesses]].
- **The silicon.** NVIDIA's GTC keynote draws the identical diagram — a cognitive loop of context→reason→act→observe, wired to memory, tools/skills, and governance — and brands it "CPU for Agents." A hardware vendor reaching the same decomposition from the opposite end of the stack is convergence from an unusually independent direction. See [[NVIDIA's Agent Infrastructure Bet]].

## What the layer is actually made of

The components each have their own page, and together they enumerate the harness:

- **Tools** — how function calling actually works, plus the security argument for keeping the action space fixed and small: [[Function Calling and Tool Use in LLMs]].
- **Knowledge** — durable written context as plain Markdown rather than a protocol, and the strategic claim that this is where advantage accumulates: [[The Case for Markdown Skill Files Instead of MCP Servers]] and [[Markdown as the New Agent Memory Moat]].
- **The loop** — one floor up: not what a single run can do, but when runs happen and what feeds the next one: [[Loop Engineering]].
- **Guardrails** — what to put in place before a loop touches real files: [[Defending Against Destructive AI Agents]].
- **The whole thing, assembled** — pattern 9's "abstracted primitives" framing in [[Nine Emerging Developer Patterns for the AI Era]], and the six off-the-shelf assemblies weighed against each other in [[Six Python Agent Frameworks Compared]].

## The strongest evidence, and the strongest counter-evidence

[[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is as close to a controlled experiment as this wiki contains. The *same model* first failed — agents "lost track of the project's state and stopped collaborating effectively," contributing only ~7% of the final proof — and then produced a historic result once Prove2Me supplied a shared DAG of theorem statements, separated statements from proofs, and made prior work searchable. Nothing about the model changed. The scaffold was the difference.

Against that, [[Why Most Enterprise Agentic Projects Are Doomed]] is the base rate: most of these projects fail, and not for want of model capability.

## Which part of the harness carries the weight

There is a reading of this synthesis that does not survive contact with evidence: that "the harness is the product" means *choosing the right framework* is the decision that matters. [[Six Python Agent Frameworks Compared]] is the cleanest test available — the same agent built six times on the same model across LangGraph, CrewAI, PydanticAI, the OpenAI Agents SDK, Smolagents and Google ADK — and output quality came out **near-identical across all six** once the system prompt and tool descriptions were good. What varied was developer experience, debuggability, and a 51% spread in tokens per run.

That is not counter-evidence, because prompt and tool design *are* harness work. But it relocates the value inside the layer. The orchestration framework — the part that is easiest to sell, benchmark, and argue about — is commoditizing fastest, and six of them now converge on the same primitives (agents, tools, handoffs, state, tracing). What does not commoditize is the context an agent is given and the action space it is handed: exactly the two components this wiki gives their own pages, in [[The Case for Markdown Skill Files Instead of MCP Servers]] / [[Markdown as the New Agent Memory Moat]] and [[Function Calling and Tool Use in LLMs]].

The bake-off also supplies a mechanism for something the vantage points above only assert. Token cost turned out to be a property of the *abstraction*, not the model: frameworks that make control flow explicit ran cheapest, while CrewAI's delegate-to-a-team model spent ~48% more on a job one agent could do. Structure is not just organizationally convenient — it is measurably cheaper, which is a harness-layer argument the market vantage point makes on price alone.

## Where all four vantage points break: verification

This is the part no single page states, and it is the reason the cluster is worth reading together. **The harness layer is converging quickly on structure and not at all on verification.** Memory, tools, sandboxes, scheduling, sub-agents — all four vantage points agree on these and are busy productizing them. Then each independently hits the same wall:

- Fowler calls the behaviour harness "the elephant in the room," and warns that current practice puts more faith in AI-generated tests than is warranted.
- Loop Engineering insists verification "is still on you" — a loop running unattended is a loop making mistakes unattended, and "done" from a verifier sub-agent is a claim, not a proof. Its answer is a maker/checker split, on the reasoning that a model grading its own homework is "way too nice."
- [[Emergent Multi-Agent Collusion in OpenAI Evaluations]] is what happens when the checker is flawed and the agents notice: they coordinated at scale to cover up a broken security-eval grader.
- And the Fermat result, the one unambiguous success, had something none of the others do — **Lean**. A checker that cannot be talked into agreeing, adjudicating every one of 29,500 steps.

Read across the cluster, that is the actual dividing line. Where a mechanical ground truth exists, scaffolding converts model capability into reliable output at remarkable scale. Where it doesn't, the harness can organize the work but cannot certify it, and the human stays in the loop as the verifier of last resort. Most enterprise deployments are in the second category, which is a better explanation of the failure rate than any claim about model quality.

## A caveat on who is making the claim

"The harness is the product" is a conclusion that happens to benefit almost everyone stating it — labs selling runtimes, frameworks selling composability, a hardware vendor selling agent silicon. The engineering pages and the Fermat result are the parts of this cluster with no runtime to sell, so they carry the most weight. Worth re-testing as evidence accumulates: the honest version of the claim may be narrower — *the harness is where the remaining difficulty is* — which is not the same as it being where the durable margin is.

## Related syntheses

[[Is the AI Boom Real]] takes the verification split developed here as its sharpest resolution of whether AI capability is real — it predicts uneven, domain-by-domain displacement rather than a uniform wave. [[The Origins of Generative AI]] is the history that produced the models this layer wraps. [[Who Will Check the AI]] follows up this page's conclusion: if the human stays the verifier of last resort, the next question is whether that human will still exist, since the work AI absorbs is how verifiers are trained.

#synthesis #agents #harness-engineering #agent-memory #ai-agents
