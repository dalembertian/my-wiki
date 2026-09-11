# Agent Memory: Semantic, Episodic, and Procedural

A reusable taxonomy for agent memory (from a LangChain/DeepLearning.AI short course, taught via building a personal email-triage agent that ignores, responds to, or notifies about incoming mail).

- **Semantic memory** — facts about the user or world, written to a searchable long-term store and retrieved in future interactions. E.g. the agent learns and remembers "this user prefers concise replies."
- **Episodic memory** — concrete past examples used as few-shot signal, not abstracted into a general fact. E.g. specific past instances of correctly-triaged emails that steer future triage decisions toward the same pattern.
- **Procedural memory** — the agent's own instructions (system prompt), treated as a mutable, optimizable artifact rather than a fixed constant — evolved over time based on feedback about what worked.

A second, orthogonal axis: memory can be updated via the **hot path** (inline, during the interaction itself) or in the **background** (asynchronously, after the fact) — a design choice independent of which memory type is being updated.

## Why this is worth a dedicated page

"Does this agent need semantic, episodic, or procedural memory — or some mix?" is a reusable design question independent of any specific framework (this course uses LangGraph, but the taxonomy itself isn't framework-specific). It's a more durable concept than the course/tutorial it came from, which is why it gets its own page rather than being folded into [[AI Agent Learning Roadmap and Resources]].

## Cross-reference

Memory is one of the specific capabilities [[Agent Harnesses]] and [[Nine Emerging Developer Patterns for the AI Era]] both list as part of what a harness/runtime layer needs to provide (Anthropic's Managed Agents, for instance, gates "long-term memory" behind a separate research-preview request).


[[LangChain's Middleware Model for Custom Agent Harnesses]] implements this taxonomy directly as FilesystemMiddleware/MemoryMiddleware/SkillsMiddleware. [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is a large-scale illustration of what happens without it: agents "lost track of the project's state and stopped collaborating effectively" until a shared DAG of theorem statements supplied the missing memory externally.


Memory as one enumerated component of the layer described in [[The Harness Is the Product]].

## Sources
- [Long-Term Agentic Memory with LangGraph](<../../source/Long-Term Agentic Memory with LangGraph.md>)

#ai-techniques #agents #agent-memory #langgraph
