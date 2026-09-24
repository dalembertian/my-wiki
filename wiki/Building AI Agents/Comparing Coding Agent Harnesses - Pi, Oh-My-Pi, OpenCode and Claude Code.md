# Comparing Coding Agent Harnesses - Pi, Oh-My-Pi, OpenCode and Claude Code

A practitioner's first-impressions review (May 2026) of five coding-agent CLIs — **Pi**, **Oh-My-Pi**, **OpenCode**, **Claude Code** and **Codex** — written specifically against the argument the author keeps being handed: "why don't you just use Pi?". His complaint is that a tool name is not an argument; the review is an attempt to answer the question he says nobody answers for him — what concrete problem does this harness solve better, and which limitation does it remove?

The verdict is deliberately boring: for ordinary software work, Oh-My-Pi and OpenCode perform about the same on the same model, Oh-My-Pi trading polish for tools and knobs, OpenCode trading flexibility for cohesion. Claude Code remains the bar for Opus; Codex is the one to avoid if you have a choice.

## Vocabulary first: model plus harness

The piece restates the decomposition before comparing anything, because "GPT 5.5 vs. Opus 4.7" is the wrong axis. A raw LLM takes text and returns text; the CLI in the middle is what builds the system prompt, loads `AGENTS.md`/`CLAUDE.md` and project rules, declares and executes tools (read, search, edit, shell, tests, browser, LSP, MCP), feeds results back as context, enforces permissions and confirmations, plans and delegates to subagents, compacts context when it fills, preserves memory across turns, and decides how you interrupt and redirect mid-run. **Two CLIs on the same model therefore behave differently** — and that claim is what the rest of the article tests.

## The subscription constraint shapes the choice

A practical point that mostly decides which harness you can use for which model. Per-token API use is expensive for continuous agent work, so subsidised monthly plans change the arithmetic — but Anthropic's Claude Code legal/compliance docs restrict Free/Pro/Max OAuth to Claude Code and Anthropic's own apps and explicitly disallow third parties routing requests with those credentials. Hacks exist (the Claude Code source leak taught people how the auth and transport worked); the author recommends against them, on the grounds that enforcement continues and the downside is losing an account you use daily. OpenCode's own providers page says the same about Claude Pro/Max while listing ChatGPT Plus, GitHub Copilot and GitLab Duo as usable subscriptions. Hence the rule of thumb: **subsidised Opus → Claude Code; subsidised GPT → OpenCode**.

## Codex is the problem, not GPT

The model is rated excellent, the harness inferior: weak planning mode, awkward interruption (often stop everything and continue by hand), looser task tracking, and tasks left unfinished until explicitly told to finish properly. The author's read is that Codex hasn't decided whether it is a chat, an executor, a planner or a terminal assistant — and since GPT 5.5 runs "much less nerfed" through OpenCode, the fix is to change harness, not model.

## Tool inventories, and the one real advantage

He checked the three codebases rather than relying on impressions.

- **Upstream Pi** ships seven tools: `read`, `bash`, `edit`, `write`, `grep`, `find`, `ls`. Clean, and a fine base for building your own distribution — thin as a day-one work tool. The NeoVim-without-plugins analogy runs through the whole piece; Oh-My-Pi is cast as its LazyVim.
- **OpenCode** has the competent normal kit: shell, read, glob, grep, edit/write or apply_patch, task/subagents, todo, web fetch/search, skills, optional LSP, plugins and MCP. Its `read` handles source files, directories, supported images and PDFs.
- **Oh-My-Pi** (a Bun-based, batteries-included fork) registers far more: `ast_grep`, `ast_edit`, `ask`, `debug`, `eval`, `search`, `lsp`, `inspect_image`, `browser`, `task`, `job`, `recipe`, `irc`, `todo_write`, `web_search`, plus memory/hindsight tools.

The clearest win is its **universal `read`**: one tool covering files, directories, archives (`.tar`, `.tar.gz`, `.tgz`, `.zip`), SQLite, images, documents (PDF, DOCX, PPTX, XLSX, RTF, EPUB), Jupyter notebooks, URLs in reader mode, and internal URIs (`skill://`, `agent://`, `artifact://`, `memory://`, `rule://`, `local://`, `mcp://`). The SQLite addressing scheme is the worked example — `file.db` lists tables and row counts, `file.db:table` shows schema and samples, `file.db:table:key` fetches a row, `?limit=/&offset=` paginates, `?where=&order=` filters, `?q=SELECT ...` runs a read-only query. A human could do all of this through `sqlite3`, but then the model has to invent the command, get the escaping right, have the binary installed, and avoid dumping 30,000 lines into context. **The path is described inside the tool, where the model can see it** — which is the article's most transferable point.

## In pure source code the gap narrows

Oh-My-Pi's code-specific conveniences: structural summaries when a file is parseable, multiple ranges per read call, line-hash anchors that make edits safer, more aggressive LSP routing, and AST tools. AST search matches syntax rather than text — `console.log($$$)` regardless of spacing or line breaks, rewriting an import node instead of replacing its text, turning `foo && foo()` into `foo?.()` with the metavariable checked on both sides — which is genuinely useful for codemods and avoids false positives in strings and comments. The caveats are stated plainly: AST patterns are sensitive, the model can get the shape wrong, and for a large rename LSP is still more correct where the language supports it. For ordinary editing, OpenCode's grep + edit + apply_patch + optional LSP covers "90% of tasks."

## The prompt is part of the tool

A tool existing is not enough; the model has to know when to reach for it. Oh-My-Pi's system prompt does explicit routing — prefer LSP for symbol operations, AST for structural search and codemods, `read`/`search`/`find` over shell, parallelise calls, delegate decomposable work — and frontier models follow that kind of instruction well. OpenCode teaches its basics competently but pushes LSP and advanced tools far less insistently; upstream Pi lists tools without routing. The author's counterweight, from using it: OMP sometimes tries *too* hard, forcing tooling onto tasks where Claude Code or OpenCode would be more direct, which can make the answer worse rather than better.

## The PR-review test: no harness found everything

The one head-to-head measurement is a single pull request ([akitaonrails/ai-memory#10](https://github.com/akitaonrails/ai-memory/pull/10)) reviewed three times: Oh-My-Pi + GPT 5.5, OpenCode + GPT 5.5, Claude Code + Opus 4.7. **None found all the problems**, and the misses were not the same misses — schematically, one caught A and B, another B and C, another C and D. Two conclusions are drawn. First, do not trust a single LLM to audit anything important: coverage depends on what it read, how you asked, which files it opened and which tool it chose. Second, and more pointed, **the same GPT 5.5 gave different answers in two harnesses** — direct evidence for the model-plus-harness claim above. He calls Oh-My-Pi and OpenCode tied here and explicitly declines to crown a winner: "I am not crowning a winner. I am killing the fantasy."

An anecdote in the same spirit: asked for a localised edit to one section of a bilingual blog post, Oh-My-Pi edited the Portuguese half correctly and then retranslated the entire English file from scratch — something he says Claude Code and OpenCode have never done to him.

## Two myths, answered

**"Pi can spawn more agents."** Availability of agents doesn't make a task parallelisable. Programming has a critical path for the same reason managing programmers does: one decision depends on the previous one, which depends on a detail that only appeared after running the test, which changes the architecture. Subagents inherit that physics — A must know what B decided, C builds on an API A is still changing, and the main agent has to consolidate, detect conflicts, reread diffs and re-run tests. Past a point "you are not parallelizing, you are inventing management." His own earlier benchmark of planner/executor model mixes found coordination overhead eating the gain: unforced, strong models simply didn't delegate; forced, quality dropped or time rose. What does parallelise is independent batches — translating a strings file into ten languages, converting 200 images, the same mechanical refactor across 50 unrelated files — and he notes those are the easy part. He observed no more parallelism from Oh-My-Pi than from OpenCode or Claude Code, and says that's the correct behaviour.

**"A minimal harness saves tokens."** Saving cents, losing dollars. The system prompt is not where a serious coding agent spends; the work is — reading code, writing patches, running tests, ingesting stack traces and build logs, re-reading files, running tests again. One verbose `npm test` erases any prompt-level saving. The working rule he states: **don't run a serious coding agent with under 200K tokens of context**, and below 100K a long session is unusable, because the harness then compacts constantly and compaction always loses detail — the warning before the error, the decision from half an hour ago, the chunk of stdout that looked like noise and was the clue. This is also his case against local open-source models for serious programming: the binding constraint is KV cache, not model quality, and even a well-specified Mac Studio struggles to approach 300K against frontier windows near a million. If cost is the issue he recommends a cheaper previous-generation cloud model (Kimi K2.6) over a local one.

The corollary is a workflow rule: manually saving tokens — letting the agent suggest a command, running it yourself and pasting back a slice of the output — destroys exactly the context that matters (full error, event order, preceding warning, stdout vs. stderr, the exact command). Let the harness run the command and eat the tokens; that's what the subsidised plan is for.

## The recommendation

- **Claude Code + Opus 4.7** — best proprietary harness if you stay inside Anthropic's ecosystem; planning mode, task list, interruption, prompt injection mid-run, and the most sophisticated memory/compaction of the set (visibly layered, per the leak).
- **OpenCode + GPT 5.5** — OpenAI's plan without Codex; more cohesive and polished, more "product."
- **Oh-My-Pi** — keep in the toolbox for projects heavy on non-source artefacts: PDFs, spreadsheets, SQLite, notebooks, archives, images, web pages to convert to text, artefacts from previous runs, frequent AST codemods. Plausibly shines in data science, investigation, audits and migrations.
- **Codex** — avoid if you have an alternative; good model, obstructive harness.
- **Pure Pi** — only if building the harness itself is the fun.

Closing framing: tools help and harnesses matter, but process still wins — small prompt, clear scope, tests running, careful review, frequent commits.

## Caveats

Self-described first impressions, left at default configuration ("too verbose out of the box" is noted as customisable and not customised), from an author who is openly irritated by the question that prompted the post. The head-to-head is a single PR reviewed once per harness, with the problem matrix given schematically rather than reported; the bilingual-edit failure is one anecdote he flags as proving nothing. Version numbers matter too: Oh-My-Pi v15.2.4 and OpenCode 1.15.10 in a category that ships breaking changes monthly, so the tool inventories date faster than the structural argument does.

## Cross-reference

This is [[Agent Harnesses]] from the user's side of the counter: that page covers labs selling harness runtimes and what they charge, this one is what the free, terminal-side harnesses actually feel like to work in, and it supplies the sharpest available evidence for the decomposition both pages rest on — the same GPT 5.5 producing different review results in two different CLIs. [[Harness Engineering - Guides, Sensors, and Regulation Categories]] is the discipline of building the *outer* harness on top of one of these; read together, they split the work: the vendor's tool registry and prompt routing described here are what Fowler's guides and sensors get layered onto, and this page's PR-review result is a concrete case of his point that inferential sensors are probabilistic rather than complete.

[[Six Python Agent Frameworks Compared]] is the same experimental design in the framework layer — hold the model constant, vary the harness — and lands in the same place from the opposite end: there, output quality converged once prompts and tool descriptions were good, while cost and ergonomics diverged; here, tool descriptions and prompt routing are precisely what differ, and they change *which problems the agent finds*. Both pages argue that the abstraction, not the model, is the variable worth choosing.

[[The Case for Markdown Skill Files Instead of MCP Servers]] is the cost side of Oh-My-Pi's bet: a registry of twenty-odd tools with rich descriptions buys the model a described path to SQLite and notebooks, but tool schemas are the most expensive thing a context window carries, and that page puts numbers on it. The universal `read` is an interesting middle case — one tool's prompt teaching many formats, rather than a server per format.

[[Defending Against Destructive AI Agents]] is by the same author and is the precondition for the advice here. "Let the harness run the command, let it read the log, eat the tokens" is only reasonable if a bad command is recoverable — sandboxing, copy-on-write snapshots and offsite backup are what make the permissive mode this page recommends affordable rather than reckless.

## Sources
- [Akita - First Impressions Using Oh-My-Pi and OpenCode](<../../source/Akita - First Impressions Using Oh-My-Pi and OpenCode.md>)

#harness-engineering #ai-agents #developer-tools #claude-code
