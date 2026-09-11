# Six Python Agent Frameworks Compared

A controlled-ish bake-off: one developer built **the same agent six times** over a weekend — a research agent with three tools (web search, SQLite lookup, structured summary) producing a validated JSON output — on the same model (GPT-4o), across LangGraph, CrewAI, PydanticAI, the OpenAI Agents SDK, Smolagents, and Google ADK. Measured on lines of code, time-to-first-working-agent, average tokens per run (over 10 runs), and a subjective "2 AM debug score" for how quickly failures could be diagnosed.

The headline finding is not which framework won. It is that **output quality was near-identical across all six** once the system prompt and tool descriptions were good — what actually differed was developer experience, token cost, and debuggability.

## The comparison

| Metric | LangGraph | CrewAI | PydanticAI | OpenAI SDK | Smolagents | Google ADK |
|---|---|---|---|---|---|---|
| Lines of code | ~210 | ~340 | ~130 | ~150 | ~95 | ~180 |
| Time to prototype | 3 hrs | 45 min | 1.5 hrs | 1 hr | 30 min | 2 hrs |
| Avg tokens/run | 2,847 | 4,216 | 2,912 | 2,791 | 3,340 | 3,102 |
| Multi-agent | Yes (graph) | Yes (teams) | Manual | Yes (handoffs) | Yes (hierarchy) | Yes (AgentTeam) |
| Type safety | TypedDict | Pydantic config | Full generics | Generic context | Minimal | Standard |
| MCP support | Yes | Limited | Native + A2A | Native | Yes | Yes |
| Model-agnostic | Yes | Yes | Yes (20+) | Yes (100+) | Yes (LiteLLM) | Gemini-first |
| Best debugger | LangSmith | Logs | IDE/types | Built-in tracing | Code output | ADK Web UI |
| 2 AM debug | 9/10 | 5/10 | 8/10 | 7/10 | 8/10 | 6/10 |

## The abstraction determines the token bill

The most transferable result, because it generalizes past these six libraries: **token cost is a property of the framework's abstraction, not of the model.**

CrewAI burned roughly **48% more tokens than LangGraph** for a job a single agent could do. Its abstraction is a *team* — each agent gets a role, a goal, and a backstory, and tasks are assigned to the crew rather than to an agent — so even a one-agent problem is modelled as a researcher delegating to a writer, and every delegation round-trip costs tokens.

The two cheapest, LangGraph (2,847) and the OpenAI Agents SDK (2,791), are cheap for the same underlying reason from opposite directions: when control flow is stated explicitly — as a compiled `StateGraph` of nodes and conditional edges, or as a declared handoff between named agents — the model spends fewer tokens *deciding what to do next*, because it has already been told. Ceremony up front buys tokens back on every run.

This is the same trade as [[Function Calling and Tool Use in LLMs]]: a fixed, explicit action space is cheaper and safer than a dynamically negotiated one.

## Smolagents writes code, not JSON

The genuine outlier. Five of the six use JSON-based tool calling — the agent emits a blob naming a function and its arguments. Smolagents' `CodeAgent` **writes actual Python** to accomplish the task.

That difference is larger than it sounds. A code agent can write a `for` loop, branch on a condition, and chain tool outputs into one another — compositions that JSON tool-calling cannot express at all, and that would otherwise have to be modelled as extra graph nodes or extra agents. The author reports this is also the easiest framework to debug despite being the least structured, because a failure produces an ordinary Python traceback rather than a tool-call chain to reconstruct.

Two costs. Code is verbose, so token use is higher (3,340) than the JSON-emitting frameworks. And the security surface is real: the default `LocalPythonExecutor` is *explicitly documented as not being a security boundary*, with sandboxing via E2B, Docker, or Pyodide left to the developer. Smolagents' whole core is ~1,000 lines of Python, readable in an afternoon — minimalism that extends to what it will and won't protect you from.

## What each one is actually for

- **LangGraph** — explicit state machines, branching logic, human-in-the-loop, and **durable execution**: a crashed run resumes from its last checkpoint, which is the difference between a retry and a re-run for an overnight pipeline. LangSmith tracing is the best debugger in the set (9/10). Cost: three hours to first working agent, and you must know your workflow up front.
- **CrewAI** — fastest to a prototype (45 minutes). The role/goal/backstory framing measurably improved output detail, gimmicky as it looks. Cost: the token overhead above, a synchronous core that needs `run_in_executor` to sit behind async web frameworks, and a delegation chain that becomes a black box when it misbehaves (5/10).
- **PydanticAI** — fewest lines among the conventional frameworks (~130), typed end to end, with `result_type` validating every LLM response against a Pydantic model and retrying on malformed output, so no hand-written validation. Cost: single-agent by design; multi-agent coordination is yours to write.
- **OpenAI Agents SDK** — four primitives (Agents, Handoffs, Tools, Guardrails), lowest tokens in the set, and — despite the name — provider-agnostic across 100+ models; the author ran the test on Claude unchanged. Cost: a young ecosystem and documentation gaps that send you into the source.
- **Smolagents** — as above.
- **Google ADK** — the only one shipping mature **evaluation tooling** out of the box: test cases as JSON, `adk eval`, structured reports on whether behavior matches expectations. Plus workflow agents (Sequential, Parallel, Loop) and smooth Vertex AI deployment. Cost: Gemini-first in practice — running GPT-4o surfaced undocumented edge cases.

## Caveats

Worth holding the numbers loosely. This is n=1: one developer, one weekend, one task shape, and a "2 AM debug score" that is explicitly subjective. Several of the sharpest figures are *quoted from other people's projects rather than measured here* — the $1,088-vs-$390 CrewAI/PydanticAI cost comparison and the 160/280/420-line implementation comparison both come from external sources. And the author flags his own shelf life: all six frameworks shipped significant updates during the writing.

The durable parts are the structural claims — abstraction drives token cost, code-generation composes where JSON cannot, evaluation and durable execution are differentiators — not the specific integers.

## Cross-reference

These six are what [[Agent Harnesses]] describes as the valuable layer, in `pip install` form: each framework is a pre-built harness making a different bet about which parts you should be able to customize. [[LangChain's Middleware Model for Custom Agent Harnesses]] is the inside view of one of them — middleware is LangChain's answer to the customization question this comparison scores from the outside — and this page is the closest thing the wiki has to a price list for those bets. At the other end of the same spectrum sits writing the loop yourself, which is the baseline these ~95-to-340-line implementations are measured against.

[[Function Calling and Tool Use in LLMs]] is the mechanism five of the six implement, and Smolagents is its sharpest counterexample: generating executable Python inverts that page's security argument, since a fixed action space is defensible in a way arbitrary generated code is not.

The "framework mattered less than prompt and tool design" finding is this page's contribution to [[The Harness Is the Product]] — a rare piece of evidence about *which* parts of the harness layer carry the weight.

## Sources
- [6 Python AI Agent Frameworks Compared](<../../source/6 Python AI Agent Frameworks Compared.md>)

#agents #harness-engineering #python #langgraph #function-calling
