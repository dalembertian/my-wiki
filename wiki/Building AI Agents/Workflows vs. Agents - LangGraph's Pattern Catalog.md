# Workflows vs. Agents - LangGraph's Pattern Catalog

LangGraph's own documentation page on the recurring shapes of LLM systems. It is useful mainly for one distinction and the ladder of patterns hung on it:

- **Workflows** have predetermined code paths and run in a fixed order.
- **Agents** are dynamic: they decide their own process and which tools to use.

The page's overview diagram splits this into three tiers of autonomy rather than two, and that is the most transferable idea in it:

| Tier | Who decides the control flow | Patterns |
|---|---|---|
| LLM embedded in predefined code paths | The developer, entirely | Prompt chaining, parallelization |
| LLM directs control flow *through* predefined code paths | The model chooses among routes the developer drew | Orchestrator-worker, evaluator-optimizer, routing |
| LLM directs its own actions based on environmental feedback | The model | Agent (LLM ↔ tool loop) |

The pattern names match Anthropic's "Building effective agents" essay (December 2024) one-for-one. The page doesn't cite it, but this is effectively that taxonomy with LangGraph code attached.

## The patterns

- **Augmented LLM** — the building block for all of them: a model plus tool calling, structured output and short-term memory.
- **Prompt chaining** — each call processes the previous call's output, optionally with a programmatic gate between steps. For tasks that break into well-defined, verifiable steps (translate, then check for consistency).
- **Parallelization** — independent subtasks run at the same time and are aggregated (speed), or the same task runs several times and the results are compared (confidence).
- **Routing** — classify the input first, then send it to a specialised path (a product-support bot splitting pricing, refunds and returns). The router is just an LLM call with a structured-output schema listing the allowed routes.
- **Orchestrator-worker** — an orchestrator plans subtasks, delegates them to workers and synthesises the results. It differs from parallelization in that the subtasks *can't be known in advance* (updating installation instructions across an unknown number of files). LangGraph's `Send` API creates worker nodes at runtime, one per planned subtask. Each worker has its own state and writes into a shared key whose reducer (`operator.add`) concatenates their outputs.
- **Evaluator-optimizer** — one call generates, another grades and returns feedback, and the loop repeats until the grade passes. The evaluator can also be a human-in-the-loop. For tasks with clear success criteria that take iteration to meet, like translation.
- **Agent** — an LLM calling tools in a continuous feedback loop until it stops asking for tools. For problems whose solution path can't be predicted. The developer still sets the toolset and guidelines.

## Two APIs for the same graph

Every pattern is shown twice, which makes the page a Rosetta stone for LangGraph's two programming models:

- **Graph API** — explicit `StateGraph`: typed state, nodes as functions, edges and `add_conditional_edges` for branching, then `compile()`. The graph can be rendered as a diagram, and this is what [[Six Python Agent Frameworks Compared]] measured.
- **Functional API** — `@task` and `@entrypoint` decorators over ordinary Python. Branching is an `if`, looping is a `while`, fan-out is calling several tasks and collecting their futures. It's the same runtime without drawing the graph.

The Functional API narrows the gap to code-writing frameworks: control flow is plain Python again. But the developer writes it, not the model, which is the difference from Smolagents' `CodeAgent`.

For the agent tier, `ToolNode` is the prebuilt tool executor, with parallel execution and error handling built in. Tools can read graph state and per-run context the model didn't generate through an injected `ToolRuntime` argument. The example is a user ID from state and an organisation ID from context. That keeps identifiers the model shouldn't be able to forge out of the tool's model-visible arguments.

## Caveats

This is vendor documentation. It walks you towards LangSmith tracing, and nothing on it argues *when not to* use a framework at all. The code is illustrative and not production-safe, in ways worth noticing:

- **The `ToolNode` example runs `eval()` on a model-supplied expression.** In a page about giving models tools, that is exactly the unbounded action space [[Function Calling and Tool Use in LLMs]] warns against.
- **Both evaluator-optimizer loops run until the joke is "funny", with no iteration cap.** A strict evaluator makes that an unbounded token bill. A real version needs a maximum number of rounds.
- **The two prompt-chaining versions disagree.** The Functional API's `check_punchline` returns "Fail" where the Graph API version returns "Pass", so the two don't behave the same.

## Cross-reference

[[Six Python Agent Frameworks Compared]] scored LangGraph from outside: slowest to prototype (three hours), among the cheapest per run, best debugger. This page shows where both numbers come from. Every workflow tier here is control flow the developer writes up front, and every decision moved into the graph is one the model doesn't pay tokens to make.

[[LangChain's Middleware Model for Custom Agent Harnesses]] is the same vendor one layer up. `create_agent` packages the "agent" row of the table above as a ready-made loop with middleware hooks. This page is the graph layer underneath it, for when a task fits one of the workflow tiers better than a free-running agent.

[[Function Calling and Tool Use in LLMs]] explains the mechanism the agent tier rests on. Its security argument is also the right lens for the `eval()` example above.

[[Loop Engineering]] is evaluator-optimizer scaled up and moved outward. Here the generate-grade-retry cycle runs inside a single invocation. There, the loop spans many agent runs on a schedule and feeds itself the next task.

## Sources
- [LangGraph - Workflows and Agents](<../../source/LangGraph - Workflows and Agents.md>)

#agents #langgraph #python #harness-engineering #function-calling
