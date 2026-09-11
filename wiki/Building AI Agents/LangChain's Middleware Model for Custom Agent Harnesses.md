# LangChain's Middleware Model for Custom Agent Harnesses

Restates "agent = model + harness" and narrows it to a concrete implementation: LangChain's `create_agent` primitive builds a minimal base harness (model + tools + system prompt), deliberately leaving customization to **middleware** — hooks that fire before/after model calls, before/after tool calls, and at agent startup/teardown.

## Why minimal-by-default

Pre-assembled, opinionated harnesses (Deep Agents, the Claude Agent SDK) bundle memory, context management, and sandboxing out of the box and get you to production fast — but leave less room for bespoke business logic. `create_agent`'s bet, similar in spirit to the highly configurable [Pi](https://pi.dev/) coding-agent harness, is that most production agents eventually need finer-grained customization than a bundled harness supports, so it implements only the core loop and exposes middleware as the extension point.

## What middleware can touch

- **Deterministic logic** — business rules, policy enforcement, runtime control (swapping models by task complexity, adjusting prompts, editing message history during compaction). "The right place for anything that can't or shouldn't live in a prompt."
- **Tools** — full lifecycle (setup, teardown, registration) handled by middleware rather than scattered across the agent definition, useful when tools have dependencies or need clean teardown.
- **Custom state** — middleware can extend the agent's state to track counters, flags, or other values across hooks and share data between them.
- **Stream handlers** — intercept and route the agent's output stream to different consumers (a UI reading token deltas, an audit log capturing tool calls, a monitoring system tracking latency).

## Capability → middleware mapping (selected)

| Capability | Middleware |
|---|---|
| Prevent context overflow | SummarizationMiddleware, ContextEditingMiddleware |
| Access/update memory | FilesystemMiddleware, MemoryMiddleware, SkillsMiddleware |
| Take actions in an environment | ShellToolMiddleware, FilesystemMiddleware, CodeInterpreterMiddleware |
| Delegate tasks | SubAgentMiddleware, AsyncSubAgentMiddleware, TodoListMiddleware |
| Handle transient failures | ToolRetryMiddleware, ModelRetryMiddleware, ModelFallbackMiddleware |
| Enforce policies | PIIMiddleware, HumanInTheLoopMiddleware |
| Steer the agent | HumanInTheLoopMiddleware |
| Control costs | ModelCallLimitMiddleware, ToolCallLimitMiddleware, PromptCachingMiddleware |

Because each piece of middleware is isolated and composable, the same middleware can be reused across every agent in an organization — new agents inherit battle-tested behavior instead of rebuilding it.

## Task-harness fit

The core design concept: how well a harness's context, failure handling, and policies actually match what a specific task demands. A customer-service agent's harness looks nothing like a long-running coding agent's harness, even when both are built on the same `create_agent` primitive — LangChain reports building its own GTM agent, an asynchronous coding agent, and a no-code agent builder this way, each with a middleware stack tailored to its own mission.

## Cross-reference

A concrete, framework-level answer to the open question [[Harness Engineering - Guides, Sensors, and Regulation Categories]] raises about keeping a growing harness internally coherent — middleware composability is one proposed answer. FilesystemMiddleware/MemoryMiddleware/SkillsMiddleware map directly onto the taxonomy in [[Agent Memory - Semantic, Episodic, and Procedural]], and SubAgentMiddleware/TodoListMiddleware are direct implementations of two of the five primitives in [[Loop Engineering]].


[[NVIDIA's Agent Infrastructure Bet]] shows the same decomposition arriving from the hardware side — memory, tools/skills, and governance as separable blocks around a cognitive loop, which is what this middleware model makes composable in code.


[[Six Python Agent Frameworks Compared]] scores LangGraph from the outside against five rivals, and supplies the number this page's design argument implies but doesn't state: explicit graph-defined control flow was among the cheapest per run (2,847 tokens), because a model told what to do next spends nothing deciding it.


[[The Harness Is the Product]] treats this as the framework vantage point — what the abstract discipline looks like once someone has to ship it.

## Sources
- [How to Build a Custom Agent Harness](<../../source/How to Build a Custom Agent Harness.md>)

#ai-agents #harness-engineering #langchain
