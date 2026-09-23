# Nine Emerging Developer Patterns for the AI Era

An a16z survey of early, real-pain-point-driven shifts in how software gets built once AI agents are full participants in the development loop, not just tooling.

1. **AI-native Git.** As agents write more code, the exact diff matters less than whether behavior is correct. The source of truth may shift upstream from the Git SHA to the *prompt + tests* that generated and verify the code — Git becomes an artifact log (tracking why and by whom) rather than a record of hand-authored changes.

2. **Dashboards → synthesis.** Static, overloaded dashboards give way to conversational interfaces that can answer "why did metric X drop?" by correlating data itself. Since agents now consume dashboards too, interfaces may split into human-facing and agent-facing modes sharing common state.

3. **Docs as interactive knowledge bases.** Developers query rather than read top-down; docs (e.g. Mintlify) become semantically searchable and serve as grounding context for coding agents, not just human reference — effectively becoming instructions *for* agents.

4. **Templates → generation.** Text-to-app platforms (Replit, Bolt, Cursor, etc.) replace static scaffolds like `create-react-app` with personalized, on-demand stack generation. Because agents can execute large refactors semi-autonomously, framework choice becomes far more reversible — less lock-in, more experimentation.

5. **Beyond `.env`.** Static secrets files make little sense when an agent is the one deploying and orchestrating. Emerging alternatives: OAuth-scoped, revocable tokens (per the latest MCP spec) instead of raw keys, or local "secret brokers" that grant agents narrowly-scoped, audited, just-in-time capabilities instead of standing credentials.

6. **Accessibility APIs as the universal agent interface.** Apps like Granola and Highlight request macOS accessibility permissions not for their original purpose but to let agents perceive UI semantically (buttons, headings, roles) instead of scraping pixels or the DOM — a fallback "render surface" that makes any app with a screen agent-usable even without a public API.

7. **Asynchronous agent work.** Interaction shifts from synchronous pair-programming to delegated, background task orchestration — via Slack messages, Figma comments, PR annotations, or voice — compressing coordination that used to require meetings and handoffs.

8. **MCP nearing universal-standard status.** MCP solves two problems: giving an LLM the right context for unfamiliar tasks, and replacing N×M bespoke integrations with a standard client/server interface. Because MCP clients and servers are logical (not physical) roles, any agent can be both a consumer and a provider of capabilities — enabling composable agent ecosystems.

9. **Abstracted primitives for agents.** Just as human developers reach for Stripe (payments) or Clerk (auth) rather than rolling their own, agents need equally clean service primitives — and those services may start exposing MCP servers directly, letting an agent say "create a $49/mo Pro plan with usage overages" and have the provider handle validation and orchestration.

## Cross-reference

Pattern 8 (MCP) is in direct tension with [[The Case for Markdown Skill Files Instead of MCP Servers]], which argues many MCP servers are solving *knowledge* problems that a cheap Markdown skill file would handle better — MCP being reserved for genuine *execution*. Pattern 9's "abstracted primitives" thinking is the same instinct behind [[Agent Harnesses]] — infrastructure vendors racing to own the layer agents build on top of.


Several patterns here have dedicated pages: pattern 7's asynchronous agent work is developed fully in [[Loop Engineering]], and pattern 8's tool layer gets a security-motivated treatment in [[Function Calling and Tool Use in LLMs]]. The reason these patterns are emerging at all — software shifting from coded rules to learned behavior — is the subject of [[From Data-Driven Software to Generative AI]].


Pattern 1 (AI-native Git) is no longer only a prediction: [[Structured Prompt-Driven Development (SPDD)]] is a full methodology built on exactly that shift, committing the structured prompt alongside the code and syncing it back whenever the code changes.


Pattern 9's abstracted-primitives thinking is one of the vantage points collected in [[The Harness Is the Product]].

## Sources
- [Nine Emerging Developer Patterns for the AI Era](<../../source/Nine Emerging Developer Patterns for the AI Era.md>)

#ai-techniques #developer-tools #agents #mcp
