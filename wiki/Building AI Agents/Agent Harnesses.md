# Agent Harnesses

A **harness** is the control layer that surrounds an AI model to make it operate reliably in production as an autonomous agent — everything except the model itself. It typically covers: model invocation and context management, tool orchestration, sandboxed code execution, persistent session/execution state, scoped permissions, error recovery, observability, and tracing. The term entered wide use after an OpenAI engineering post (Feb 2026) describing a production system built with zero human-written code, and was formalized by Martin Fowler's essay defining "harness engineering" as the discipline of building this layer. The analogy used: a harness is to a model what container orchestration infrastructure is to an application — not the core logic, but what makes long-running instances of it safe and dependable.

By April 2026, four frontier labs converged on the same conclusion — the harness, not the raw model, is where the product value now sits — but diverged sharply on how to sell it, all within about two weeks:

- **Anthropic — Managed Agents** (beta, Apr 8): fully hosted on Anthropic's own infrastructure, billed at **$0.08/session-hour** on top of standard token rates. Multi-agent orchestration, self-evaluating outcomes, and long-term memory are gated behind a separate research-preview request. Launch customers: Notion, Rakuten, Sentry, Asana, Atlassian.
- **OpenAI — Agents SDK update** (Apr 15): open source, model-native harness with **no separate first-party runtime fee** — standard token/tool pricing only. Developers bring their own compute via a "Manifest" abstraction spanning 7 sandbox providers (Blaxel, Cloudflare, Daytona, E2B, Modal, Runloop, Vercel) and 4 storage backends. Explicit tradeoff stated by OpenAI: managed APIs simplify deployment but constrain where agents run and how they touch sensitive data.
- **Google — Vertex AI Agent Engine**: managed runtime, but meters sessions, memory, code execution, and observability as **separate consumption lines** rather than one bundled fee.
- **Microsoft — Foundry Agent Service**: consumption-based, metered **per model and per tool** (e.g. specific metering on Code Interpreter) rather than a platform-wide fee.
- **AWS**: co-developing a Stateful Runtime Environment with OpenAI (announced Feb, to ship via Bedrock), alongside its own Bedrock AgentCore runtime primitives.

## The strategic read

The piece draws a direct parallel to earlier infrastructure splits — Terraform vs. AWS CloudFormation, open-source Kubernetes vs. managed container services — where open source and managed offerings coexisted long-term by serving different buyer profiles (control/portability vs. hosted convenience) rather than one absorbing the other.

Who's exposed: **horizontal, model-agnostic orchestration frameworks** (LangChain, CrewAI, VoltAgent) are squeezed hardest, since they now compete against a free, model-native harness from the same lab whose models they wrap — undercutting their core "avoid vendor lock-in" pitch. Vendor-neutral governance/trust-focused startups (e.g. Sycamore, which raised a $65M seed the same month) are less exposed, since that positioning is orthogonal to any single lab's harness. For teams building their own in-house harness, there are now two concrete external benchmarks to build-vs-buy against (Anthropic's per-hour rate, or OpenAI's SDK + your own infra costs) that didn't exist a month earlier.

## Cross-reference

This page covers the market/pricing side of "harness"; see [[Harness Engineering - Guides, Sensors, and Regulation Categories]] for the Martin Fowler essay formalizing the engineering discipline itself (guides, sensors, the steering loop), and [[Six Python Agent Frameworks Compared]] for what that layer costs to buy off the shelf. [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is a large-scale demonstration of the thesis: the same model failed until the scaffold supplied shared state.


Inbound context: the harness-layer land grab here is the current instance of the infrastructure-bet pattern in [[AlexNet and the Three Nonconformists Who Built the Deep Learning Boom]], and [[NVIDIA's Agent Infrastructure Bet]] shows a hardware vendor arriving at the identical "Agent = LLM + Harness" decomposition from the silicon end. [[The Case for Markdown Skill Files Instead of MCP Servers]] and pattern 9 of [[Nine Emerging Developer Patterns for the AI Era]] are arguments about *which* parts of this layer are worth paying for; [[Loop Engineering]] covers one component of it in depth — the scheduling and feedback structure that spans runs.


[[Six Python Agent Frameworks Compared]] is this page's abstract layer made concrete and installable — six open-source harnesses benchmarked head-to-head, which is what the market described here looks like from a developer's side of the purchase decision.


This page is the market vantage point in [[The Harness Is the Product]], which collects the four independent lines of evidence for the harness-as-product claim — and notes that vendors selling runtimes are interested parties.

## Sources
- [Anthropic, OpenAI, Google, and Microsoft agree that the harness is the product. They disagree on the price.](<../../source/Anthropic, OpenAI, Google, and Microsoft agree that the harness is the product. They disagree on the price..md>)

#ai-agents #harness-engineering #pricing #anthropic #openai #google #microsoft
