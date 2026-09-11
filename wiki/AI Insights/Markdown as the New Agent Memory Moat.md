# Markdown as the New Agent Memory Moat

Three unrelated bets landed on the same substrate within a single quarter: plain Markdown, versioned in git, as what agents read and write.

## Three bets, three different problems

- **Andrej Karpathy's "LLM Wiki" gist** (April) — a personal-knowledge-base pattern: an agent keeps what it knows as linked Markdown files it can read and rewrite, because a language model doesn't get bored maintaining cross-references. (Notably, this is the exact article that seeded this very wiki — see `settings/llm-wiki.md`.) A few thousand words, no product attached.
- **Google's Open Knowledge Format** (June) — a published `v0.1` standard packaging organizational knowledge, metrics, tables, and runbooks as plain Markdown any agent can read without a proprietary account, aimed at enterprise context for BigQuery agents.
- **Garry Tan's gstack** — an MIT-licensed Claude Code setup that crossed 66,000 GitHub stars within weeks: 23 specialist roles, each a Markdown file, no runtime or code, running across ten different coding agents.

One aimed at agent memory, one at enterprise data-sharing standards, one at summoning an engineering team from a terminal — different needs, same underlying resource.

## Why Markdown specifically won

CLAUDE.md and AGENTS.md are already present in millions of repos as the first files an agent loads. The formats that "won" this cycle are the ones adoptable with zero infrastructure change: `cat` the file, clone the repo, any tool already parses it — the same dynamic that made Git and JSON durable. MCP remains the interface an agent connects *through*; Markdown is becoming the format that carries the *content*.

## The moat is moving

For roughly two years, the operating belief was that owning the best model meant controlling the developer. That's shifting: gstack keeps working if you swap Claude for GLM or Codex underneath, because the intelligence is replaceable but the documentation isn't. The claim: **the moat is shifting from the model to the Markdown a team owns and accumulates over time** — an OKF bundle of runbooks, metric definitions, and architecture decisions is portable by design across clouds, models, and frameworks, which is exactly why vendor-neutral formats matter.

The stated caveat: durability is the open question, not intent. OKF is a `0.1` draft with a reference implementation, not an ecosystem — "if no one develops consumers for it, it remains just a good idea that Google released on a slow Friday."

## Cross-reference

Directly complements [[The Case for Markdown Skill Files Instead of MCP Servers]] and [[Loop Engineering]] (skills as "intent written down once" — the same accumulation argument applied at the individual-project scale). Note: [[Karpathy's 'Think Before Coding' Skills File]] is a *different* Karpathy artifact than the "LLM Wiki" gist discussed here — worth not conflating the two.


[[Why Most Enterprise Agentic Projects Are Doomed]] reaches the same conclusion from the organizational side — the moat is accumulated signal, not any static asset — and [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is the strongest demonstration of the underlying mechanic, where a durable external record of proven theorems, not a better model, is what made dozens of agents productive together.


[[The LLM Wiki Pattern]] is the individual-scale version of the accumulation argument — one person's notes as the durable asset, rather than an organization's.


[[The Harness Is the Product]] places this strategic claim next to the engineering and market evidence for the same layer.

## Sources
- [Andrej Karpathy, Google and Garry Tan agree Markdown is the answer, but they're not solving the same problem](<../../source/Andrej Karpathy, Google and Garry Tan agree Markdown is the answer, but they're not solving the same problem.md>)

#ai-insights #markdown #agent-memory #skills
