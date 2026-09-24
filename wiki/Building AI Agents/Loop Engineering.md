# Loop Engineering: Designing Systems That Prompt Your Agents

The claimed shift, per Peter Steinberger and Boris Cherny (Anthropic, Claude Code lead): stop prompting the agent yourself — design the system ("loop") that prompts it instead, on a schedule, indefinitely. Framed as "one floor above" harness engineering: the harness shapes what a single agent run can do; the loop decides when runs happen and feeds itself the next one.

## Five primitives, plus memory

Both the Codex app and Claude Code now ship roughly matching implementations of all five — this is no longer a bespoke pile of bash you maintain alone:

1. **Automations** — scheduled discovery/triage runs (Codex's Automations tab; Claude Code's `/loop`, cron, hooks, GitHub Actions). The `/goal` primitive is the sharpest piece: it keeps working across turns until a *separate* small model verifies a stated condition is actually true — the agent that wrote the code isn't the one grading it.
2. **Worktrees** — isolate parallel agent runs on separate git checkouts so they can't collide on the same files.
3. **Skills** — `SKILL.md` files encoding project-specific conventions once, so the loop doesn't "re-derive your whole project from zero every cycle." Framed as intent written down on the outside — conventions, build steps, "we don't do it like this because of that one incident" — read by the agent every run instead of re-explained each session.
4. **Plugins/connectors** — MCP-based access to real tools (issue trackers, databases, Slack) so the loop can *act*, not just report.
5. **Sub-agents** — splitting the agent that writes from the agent that checks, since "the model that wrote the code is way too nice grading its own homework." This maker/checker split is what `/goal`'s verification step does under the hood too.
6. **Memory/state** — a markdown file or external board (e.g. Linear) persisting across runs, since the model forgets everything between invocations. "The agent forgets, the repo doesn't."

## A worked example

A daily automation triages CI failures/issues/commits via a skill and writes findings to a state file. For each finding worth acting on, it opens an isolated worktree, has one sub-agent draft a fix and a second review it against project skills and tests, then uses connectors to open the PR and update the ticket. The state file carries context from today's run into tomorrow's — you designed this once; you didn't prompt any individual step.

## What the loop still doesn't do for you

Three things the author insists get *harder*, not easier, as the loop improves:

- **Verification is still on you.** A loop running unattended is a loop making mistakes unattended — "done" from a verifier sub-agent is a claim, not a proof.
- **Comprehension debt compounds.** The faster a loop ships code you didn't write, the bigger the gap between what exists and what you actually understand grows, unless you deliberately read what it produced.
- **"Cognitive surrender"** — the comfortable failure mode is letting the loop run because stopping to check feels like friction, not because you've actually verified it's right. "Designing the loop is the cure when you do it with judgement and the accelerant when you do it to avoid thinking — same action, opposite result."

## Cross-reference

Builds directly on [[Agent Harnesses]] and [[Harness Engineering - Guides, Sensors, and Regulation Categories]] (a loop is scheduling + feedback applied across many runs, not just within one). Pattern 7 (asynchronous agent work) in [[Nine Emerging Developer Patterns for the AI Era]] is an early version of the same idea. [[LangChain's Middleware Model for Custom Agent Harnesses]]'s SubAgentMiddleware and TodoListMiddleware are direct implementations of two of the primitives here.


Loops at other scales: [[Why Most Enterprise Agentic Projects Are Doomed]] applies the same mindset to organizations rather than individual engineers, [[Markdown as the New Agent Memory Moat]] covers what accumulates across iterations, and [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is a loop run to an extreme — dozens of agents iterating for eleven days against a compiler that adjudicates every step.


[[Structured Prompt-Driven Development (SPDD)]] is the same closed-loop instinct dialled the other way: state persists in a version-controlled spec rather than a scratch memory file, and the human is deliberately re-inserted at six checkpoints instead of designed out of the loop. It is the sharpest available counterweight to the "cognitive surrender" risk named here.


[[The Harness Is the Product]] collects this page's insistence that verification stays with the human alongside the same conclusion reached independently by Fowler, by the OpenAI collusion incident, and by the Fermat proof.


[[Workflows vs. Agents - LangGraph's Pattern Catalog]] has this page's loop in miniature. Its evaluator-optimizer pattern is a generate-grade-retry cycle inside one run, and its example has no iteration cap, which is a small-scale reminder of why a loop needs a stopping rule.

## Sources
- [Loop Engineering](<../../source/Loop Engineering.md>)

#ai-agents #harness-engineering #agents #automation
