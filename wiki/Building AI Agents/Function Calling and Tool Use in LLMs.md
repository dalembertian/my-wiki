# Function Calling and Tool Use in LLMs

A Martin Fowler walkthrough of **function calling**: the LLM never executes anything itself — it analyzes natural-language input and emits a structured (JSON) description of which function to call and with what arguments; the calling program deserializes that and executes it in its own runtime.

## Building it: a Shopping Agent, test-first

The example builds a `ShoppingAgent` with three possible actions — `Search`, `GetProductDetails`, `Clarify` — starting from unit tests asserting which action a given user message should map to, then implementing `decide_next_action` against OpenAI's `tools` parameter: each function gets a JSON-schema description (name, parameters, required fields, a `description` field that helps the model pick the right one), plus a system prompt establishing the agent's role, expected output format, and constraints (e.g. "ask for clarification when unclear"). A later refactor swaps hand-written JSON schemas for `instructor`-based Pydantic models, cutting most of the schema-definition boilerplate.

## Security: this is a new attack surface

Two concrete guardrails:

1. **Restrict the action space with explicit conditional dispatch, not dynamic `eval`.** Letting the model's output directly drive `eval`-based function invocation is a real code-execution risk — always enumerate the allowed actions explicitly.
2. **Guard against prompt injection.** A user-facing, function-calling agent can be manipulated via natural language the way traditional apps are manipulated via SQL injection — e.g. tricking the agent into revealing its system prompt (which then informs further attacks like unauthorized refunds or data exposure). Mitigations: a denylist of suspicious phrases ("ignore previous instructions," "system prompt," "act as," "new role") as a first pass, combined with LLM-based input screening for more nuanced attempts. Neither is sufficient alone.

## Can this replace rules engines?

Fowler revisits his own 15-year-old skepticism of rules engines: the pitch that non-programmers can safely author business rules "rarely works out in practice," because the *combination* of many individually-simple rules explodes in complexity and becomes hard to test or predict. His read: LLM-based systems are a genuinely different alternative — reasoning about intent and context in natural language rather than chaining static rules — at the cost of full transparency and determinism. Practical middle ground suggested: combine LLM-driven reasoning for flexibility with **explicit manual gates** on the decisions that matter most.

## Terminology and MCP

"Tool calling" is the more general, current term — covering not just custom functions but built-in capabilities like code interpreters and file/database retrieval. On **MCP** specifically: in this article's `ShoppingAgent`, the available tools are hardcoded to three functions — which limits flexibility but makes the action space easy to secure. MCP instead lets the agent query a server at runtime to discover available tools dynamically (decoupling the agent from any fixed tool set), which is valuable for agents that need to adapt to a wide or evolving tool surface (e.g. IDEs) — but the article is explicit that this flexibility is added complexity, and a fixed, hardcoded, easily-secured action space is often the *better* choice for a narrow, well-scoped agent like a shopping assistant.

## Cross-reference

Directly complements the MCP-vs-plain-tools tradeoff in [[Nine Emerging Developer Patterns for the AI Era]] (pattern 8) and [[The Case for Markdown Skill Files Instead of MCP Servers]] — this piece adds the security-motivated case for *not* reaching for MCP by default: a fixed action space is easier to secure than a dynamically-discovered one.


[[A Document-Grounded Rules Referee via Prompt Engineering]] is the limiting case of the argument here: a task where the guardrail is built entirely at the prompt level and the action space is empty, showing what is achievable before any tool layer is introduced.


Smolagents, in [[Six Python Agent Frameworks Compared]], is the live counterexample to the mechanism described here: its `CodeAgent` emits executable Python instead of a JSON tool call, which buys loops and conditionals that JSON cannot express — and gives up exactly the security property this page argues for, its default executor being documented as not a sandbox.


The tool layer as one enumerated component of [[The Harness Is the Product]].


[[Workflows vs. Agents - LangGraph's Pattern Catalog]] builds its whole agent tier on this mechanism. Its `ToolNode` example is a neat illustration of this page's warning: a calculator tool that `eval()`s whatever expression the model sends, in official framework docs.

## Sources
- [Function calling using LLMs](<../../source/Function calling using LLMs.md>)

#ai-techniques #function-calling #agents #mcp #security
