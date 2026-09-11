# The Case for Markdown Skill Files Instead of MCP Servers

An architectural argument, illustrated by Brad Feld's "CompanyOS" (a company run substantially on 12 Markdown files plus 8 MCP servers): many MCP servers are solving the wrong kind of problem, and a Markdown "skill" file is often better architecture.

## Know vs. do

Every agent task is either a **knowledge problem** (stable, expressible in natural language, fits in-context — coding standards, triage workflows, tone guidelines) or an **execution problem** (needs a live runtime — calling an API, querying a database, sending an email). MCP was built for execution and handles it well. The failure mode: teams install a full MCP server (e.g. the GitHub MCP server, exposing dozens of tools) just to teach an agent institutional conventions — "use squash merges, run tests first, format commits as conventional commits" — when a **SKILL.md** file saying exactly that would work as well or better, at a fraction of the token cost.

A third, most-common-in-production case is hybrid: a skill that *references* MCP tools, encoding the workflow/judgment layer in Markdown while MCP supplies execution underneath. Feld's support-triage skill is the example given: it fully defines severity categorization, tone, and escalation logic, and works in a "standalone mode" even with the Help Scout MCP server disconnected — it just degrades to producing a draft reply instead of auto-sending one. The thinking survives; only the plumbing disappears.

## The token cost is concrete

The GitHub MCP server's tool schema alone consumes ~23,000-50,000 tokens of context, before the agent processes any actual task. A SKILL.md encoding the same team-specific GitHub conventions runs 200-500 tokens — roughly a **100x** reduction — leaving far more context budget for actual reasoning, and producing more relevant decisions since the skill encodes *your team's* conventions rather than the full universe of API possibilities. At enterprise scale, a dozen connected MCP servers can burn 200,000-400,000 tokens in schemas alone, more than half of many models' context windows, before a user request is even read.

## Production examples cited

- **Feld's CompanyOS**: 12 skill files (~2,000 lines total), 8 MCP servers used strictly for execution.
- **Supabase's open-source agent-skills repo**: stable development practices (migration patterns, deployment conventions) as skills; MCP reserved for dynamic API/schema introspection.
- **Microsoft's .NET Skills Executor**: SKILL.md files define workflow; MCP tool calls are resolved as a subordinate execution layer — described as the clearest industry signal of convergence on this two-layer model.
- **Claude Code's own skills system**: Markdown files encode best practices, referencing MCP tools only when execution is actually needed.

## The git advantage

Skill files are plain text, version-controlled, reviewed via pull request, with diff/blame history — changing agent behavior means editing a paragraph and committing, not redeploying a server. The article's suggested audit heuristic: for each MCP tool your system exposes, ask whether it primarily solves a knowledge problem (candidate for extraction into a skill) or a genuine execution need (stays in MCP); and apply a "standalone test" — if disconnecting the MCP server makes a skill produce nothing useful, it was actually an execution concern misfiled as knowledge.

## Cross-reference

This wiki's own `settings/schema.md` is functionally exactly the pattern described here — a Markdown knowledge file that encodes workflow, conventions, and judgment calls for an LLM operating over a fixed set of files, with no runtime/execution layer needed at all. Directly complements [[Nine Emerging Developer Patterns for the AI Era]] (pattern 8, MCP) and [[Agent Harnesses]] (the broader infrastructure-layer land grab this sits inside). See also [[The LLM Wiki Pattern]] for a lightweight, non-technical version of the same "give the agent durable written knowledge instead of just raw access" idea.


The same argument from other angles: [[Karpathy's 'Think Before Coding' Skills File]] is a worked example of one such file, [[Markdown as the New Agent Memory Moat]] makes the strategic case that this format is where durable advantage accumulates, and [[Function Calling and Tool Use in LLMs]] supplies the security reason to keep the execution surface small. [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is the large-scale version of the underlying claim — durable external state, not a richer protocol, is what let dozens of agents cooperate.


The knowledge component of the layer mapped in [[The Harness Is the Product]].

## Sources
- [The case for running AI agents on Markdown files instead of MCP servers](<../../source/The case for running AI agents on Markdown files instead of MCP servers.md>)

#ai-techniques #mcp #agents #skills
