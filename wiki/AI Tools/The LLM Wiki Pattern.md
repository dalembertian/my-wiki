# The LLM Wiki Pattern

Andrej Karpathy's gist describing a pattern for building personal knowledge bases with LLMs — and, notably, **the exact article that seeded this very wiki project** (kept unmodified as the reference document at `settings/llm-wiki.md`; this page is the wiki's own indexed summary of it, since the reference copy itself doesn't otherwise show up in `1.Index.md`/`3.Articles.md`). Merged here with a practitioner's account of the same idea in miniature: pairing Claude with an Obsidian vault as "connective tissue" between notes rather than as a search engine.

## The core idea

Most LLM-and-documents workflows are RAG-shaped: upload files, retrieve relevant chunks per query, generate an answer — rediscovering knowledge from scratch every time, with nothing accumulating between questions. This pattern is different: the LLM **incrementally builds and maintains a persistent wiki** between the user and the raw sources. When a new source arrives, the LLM doesn't just index it for later retrieval — it reads it, integrates it into the existing wiki (updating pages, flagging where new information contradicts old claims), so the synthesis is compiled once and then kept current, not re-derived on every query. The human curates sources, directs the analysis, and asks questions; the LLM does the summarizing, cross-referencing, filing, and bookkeeping.

## Three layers

- **Raw sources** — the curated, immutable source of truth. The LLM reads from it, never modifies it.
- **The wiki** — LLM-generated markdown: summaries, entity pages, concept pages, an evolving synthesis. The LLM owns this layer entirely.
- **The schema** — a document (CLAUDE.md/AGENTS.md-style) defining structure, conventions, and workflows, co-evolved between user and LLM over time. In this project, that's `settings/schema.md`.

## Three operations

- **Ingest** — read a new source, discuss key takeaways (or not, if batching), write/update pages, update the index, log the change. A single source can touch 10-15 pages.
- **Query** — search the index, read relevant pages, synthesize an answer with citations, in whatever output form fits (markdown page, table, slide deck, chart). The key idea: **good answers should be filed back into the wiki as new pages** rather than disappearing into chat history, so explorations compound the same way ingested sources do.
- **Lint** — periodically health-check the wiki for contradictions, stale claims, orphan pages, missing concept pages, and missing cross-references.

## Indexing, logging, and tooling

`index.md` (content-oriented catalog: link + one-line summary + optional metadata, grouped by category, read first when answering a query) and `log.md` (chronological, append-only, with a consistent line-prefix like `## [date] type | title` so it's greppable) are the two navigation files the pattern recommends. This wiki adopted both directly, with a local twist once the wiki grew: splitting the catalog into a lean `1.Index.md` jump-list plus fuller `3.Articles.md` and `2.Syntheses.md` detail files, and moving `log.md` into a separate `tracking/` folder. At larger scale, the original suggests a dedicated search tool (it names [qmd](https://github.com/tobi/qmd), hybrid BM25/vector search, on-device) once the index file alone stops being enough.

Other tips from the original worth noting: Obsidian Web Clipper for capturing sources; downloading images locally since LLMs can't read inline images in one pass (view them as a separate step instead); Obsidian's graph view to see the wiki's shape — hubs and orphans; Marp for slide decks generated from wiki content; Dataview for frontmatter-driven dynamic tables (this project deliberately opted out of frontmatter/Dataview); and the observation that the wiki is just a git repo, so version history and branching come for free.

## Why it works

The tedious part of a knowledge base isn't the reading or the thinking — it's the bookkeeping: keeping cross-references current, flagging contradictions, staying consistent across dozens of pages. Humans abandon wikis because maintenance burden outgrows the value delivered; an LLM doesn't get bored and can touch 15 files in one pass, so the wiki stays maintained because the marginal cost of maintaining it is near zero. The article connects this to Vannevar Bush's 1945 Memex concept — a private, curated, associative knowledge store — noting Bush's vision was closer to this pattern than to what the web actually became, and that the part he couldn't solve (who does the maintenance) is exactly what the LLM now handles.

## Deliberately incomplete by design

The article is explicit that it describes the idea, not a specific implementation — directory structure, schema conventions, page formats, and tooling are meant to be worked out collaboratively between a user and their LLM to fit their own domain. That's the literal process this wiki went through across several ingest sessions — category structure, the index/articles/syntheses split, frontmatter conventions, block-reference linking, and more — all captured in `tracking/log.md`.

## Connecting an LLM to a vault in practice

A separate practitioner account describes three tiers for actually wiring an LLM up to an Obsidian vault, from least to most capable:

- **No-setup**: copy-paste notes into Claude.ai, or point it at the vault folder manually. Free, but manual.
- **Obsidian Copilot plugin**: adds a chat sidebar inside Obsidian, connected via an Anthropic API key (paid per-use). Good for non-technical users.
- **MCP server + Filesystem extension** (Claude Desktop): gives Claude direct read/write access to vault files — the most powerful option, letting it search, read, and analyze notes without manual copy-pasting. The author notes Claude performs better when folder and note-title naming is clean and consistent.

A further tier: **Claude Code** (the terminal-based coding agent) can read and write markdown files directly, and "can also completely organize massive dumps of notes" — summarizing, tagging, and linking new notes to existing ones automatically. This is architecturally the same capability this wiki project uses.

**What it's useful for**, per the same account: finding hidden connections (an LLM can read many notes at once and surface recurring themes a human would have to reread everything to notice — a complement to Obsidian's native Graph View, which shows structural connections but not thematic ones); fixing messy formatting (turning rough brain-dumps into structured markdown with headings, bullets, and YAML frontmatter); and building study guides (reorganizing scattered notes on a topic into a logical, sequenced learning path).

**Caveats raised**: giving an assistant filesystem access to a vault means being deliberate about which folders/files it can touch (tool permission scoping) — sensitive notes need explicit exclusion. AI summaries shouldn't be trusted blindly; the author frames the ideal use as "a research filter to reduce my own mental clutter," not a replacement for personal judgment. Observed failure modes: grouping unrelated ideas together by mistake, sounding "overly confident" even when misreading a note.

## Cross-reference

See [[Markdown as the New Agent Memory Moat]] for how this pattern is one of three separate, convergent industry bets on Markdown-as-agent-substrate (alongside Google's Open Knowledge Format and Garry Tan's gstack); [[The Case for Markdown Skill Files Instead of MCP Servers]], since this wiki's `settings/schema.md` is itself functionally a "skill file" of the kind that article describes; and `settings/llm-wiki.md` for the unmodified original text of the Karpathy piece.

## Sources
- [Andrej Karpathy llm-wiki](<../../source/Andrej Karpathy llm-wiki.md>)
- [Pairing Obsidian and Claude was the best thing that happened to my note-taking](<../../source/Pairing Obsidian and Claude was the best thing that happened to my note-taking.md>)

#ai-tools #llm-wiki #knowledge-management #obsidian #note-taking #claude
