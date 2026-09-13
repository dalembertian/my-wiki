# Claude's Computer-Checked Proof of Fermat's Last Theorem

Anthropic reports the first complete computer-checked proof of **Fermat's Last Theorem**, produced largely autonomously by Claude in the Lean proof assistant over **11 days**. The work was initiated by Tianyi Peng, an Anthropic researcher whose Columbia group builds AI formalization tooling, and the result went further than anyone expected: the community had assumed formalizing FLT would take years.

## What was actually produced

- **13 million lines of Lean** — over 5x the size of Mathlib, the community's principal proof library that the work builds on.
- **30,300 verified theorems**, 29,500 of them used in the final proof.
- The proof follows the simplified Wiles exposition by **Darmon, Diamond, and Taylor**.
- It relies on **only Lean's three standard axioms**, and a `comparator` tool confirmed the theorem statement matches Mathlib's own statement of FLT (guarding against the classic formalization failure of proving something subtly weaker than intended).
- Roughly **six billion output tokens** consumed, from a general-purpose internal research model comparable to Claude Fable 5.1.
- Human mathematical input was limited to occasional high-level priority nudges from Peng — *"Jacobian as a scheme sounds high priority"*, *"push Mazur to be done soon."*

Kevin Buzzard, who leads the Imperial College FLT formalization project and reviewed the output, called it an "extraordinary autoformalization achievement... with no assumptions other than the axioms of mathematics," noting the artefacts are now "robust enough to be built upon."

## What is novel here: verification, not mathematics

The result is explicitly *not* new mathematics — unlike Anthropic's Riemann-zeta work. It is **verification at scale**. The bottleneck it attacks is a real and chronic one in mathematics: Wiles's 1995 proof ran 129 pages and took months to check, and a reviewer found a critical gap two months into the effort that took Wiles another year to close. The page's footnotes catalog the same pattern elsewhere — Hales's Kepler conjecture spent four years in review before a 12-referee panel settled for "99% certain"; Perelman's Poincaré proof took ~4 years and three 300-page expositions; Helfgott's weak Goldbach proof is still under review; and wrong results have been accepted for years with other work built on top of them.

## The agent-engineering lesson: scaffolding beat raw capability

This is the part with the widest transfer beyond mathematics. **Claude's initial attempts failed.** Agents had early success, then "quickly lost track of the project's state and stopped collaborating effectively" — those failed runs contributed only ~7% of the final proof's non-boilerplate lines.

The effort succeeded only after switching to **Prove2Me**, an open collaborative formalization platform, which supplied three things:

1. **A DAG of theorem statements** that agents read to decide what to attempt next — explicitly credited with *mitigating memory degradation* and enabling many agents to work in parallel without colliding.
2. **Separation of theorem statements from proofs into different files**, with links maintained independently — speeding Lean compilation and cutting resource use.
3. **Natural-language descriptions of each theorem statement**, making prior results searchable and reusable so agents found shorter proof paths instead of re-deriving.

In other words: dozens of agents, a shared externalized state graph as the coordination substrate, and a compiler as ground truth. The scaffold, not the model, was the difference between failure and a historic result — a direct echo of the harness-is-the-product thesis running through this wiki.

## Scale is not the whole story

A follow-on experiment suggests the approach isn't gated on Anthropic-scale compute: researchers using **three personal Claude Max plans**, collaborating entirely through Prove2Me, formalized **Vinogradov's Three Primes Theorem in three days**. The post argues collaborative formalization of major results with consumer subscriptions is achievable with the right scaffold.

## Implications the post claims

- **Rigorous checking of LLM-generated mathematics** — currently an expensive human-led process — becomes tractable, which matters more as AI produces more purported proofs than reviewers can absorb.
- Expect formalized proofs to be produced **alongside** human-readable write-ups as standard practice; Anthropic is explicit that formalization should not *replace* human exposition, but may be the only way the community keeps up.
- Writing Lean appears to **help Claude prove novel results** — partial formalizations act as a self-check on hypotheses, much like writing numerical simulations to test whether an approach is on track.
- Anthropic frames formalization as a place where it feels "unambiguously good" about AI's role, and points to expanded researcher support (free/discounted subscriptions, research credits, AI-for-science grants).

## Cross-reference

The multi-agent coordination failure and its fix are a large-scale instance of the control structures catalogued in [[Loop Engineering]]; the "the scaffold is what makes the model useful" conclusion is the central claim of [[Agent Harnesses]] and [[Harness Engineering - Guides, Sensors, and Regulation Categories]]. Prove2Me's DAG-of-statements as durable shared state parallels the file-based memory argument in [[Markdown as the New Agent Memory Moat]] and [[The Case for Markdown Skill Files Instead of MCP Servers]]. As a claimed AI research result it sits alongside [[Inherent Labs and Faraday]], and its "agents lost track and stopped cooperating" phase is the benign counterpart to the coordination pathologies in [[Emergent Multi-Agent Collusion in OpenAI Evaluations]]. It is also a strong data point against the failure patterns described in [[Why Most Enterprise Agentic Projects Are Doomed]] — the difference being an unambiguous ground-truth checker.


For the history that produced the models behind this result, see the cluster hub [[The Origins of Generative AI]].


[[AI Digital Fossils in LLM Training Data]] is the inverse case worth reading alongside this one: a body of knowledge with no mechanical checker, where an error introduced by a 1950s scanning glitch is now permanent.


[[The Harness Is the Product]] treats this as the closest thing to a controlled experiment for the harness claim: same model, failure then success, only the scaffold changed.

## Sources
- [Formalizing Fermat's Last Theorem](<../../source/Formalizing Fermat's Last Theorem.md>)

#anthropic #agents #claude #ai-for-science #harness-engineering #agent-memory
