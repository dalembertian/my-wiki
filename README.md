# my-wiki — an LLM-maintained knowledge base

This repository is a **personal wiki that an AI writes and maintains**, and that a human curates and reads.

It's a working instance of the *LLM Wiki* pattern described by Andrej Karpathy in [this gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) (a copy lives here as [`settings/llm-wiki.md`](<./settings/llm-wiki.md>)). The core idea: instead of dumping documents into a RAG system and re-deriving knowledge on every question, you have an LLM **incrementally compile** what you read into a persistent, cross-linked set of markdown pages. Each new article gets read, summarised, filed, linked to what's already there, and checked against it. The knowledge accumulates instead of being rediscovered.

The topic here happens to be **AI itself** — agents, harnesses, tooling, industry news and analysis — but nothing about the machinery is AI-specific. The same setup works for a research project, a book you're reading, a due-diligence dossier, or your own health notes.

## How it was built

Karpathy's article is deliberately abstract: it describes the pattern, not an implementation. You're meant to hand it to your favourite coding agent and build out the specifics together.

That's exactly what happened here. This wiki was built with [Claude Code](https://claude.com/claude-code) over many sessions, starting from that gist. The conventions weren't designed upfront — they were *negotiated*, one friction point at a time:

- A category folder grew to 10 pages while its sibling had 2 → the categories got renamed to describe an **activity** rather than a topic.
- A lint pass found five pages nobody linked to → **reciprocal links** became a hard rule.
- Pages started cross-referencing each other so densely that a claim existed only "between" them → the **Syntheses** layer was born.

All of that lives in [`settings/schema.md`](<./settings/schema.md>) — with this wiki's own category list split out into [`settings/categories.md`](<./settings/categories.md>) — and the schema is the real heart of this repo — it's the config file that turns a general-purpose coding agent into a disciplined wiki maintainer. [`CLAUDE.md`](<./CLAUDE.md>) just points at it. Every decision and why it was made is recorded in [`tracking/log.md`](<./tracking/log.md>).

The wiki is a plain git repo of plain markdown. It renders fine on GitHub — **you can browse the whole thing right here, no tools required** — and even better in [Obsidian](https://obsidian.md/), where the `[[wikilinks]]`, tags and graph view come alive.

---

## How to read this wiki

**Start at [`1.Index.md`](<./1.Index.md>).** That's the entry point — pure links, no prose, covering everything.

There are four root-level catalogs, numbered in reading order:

| File | What it's for |
|---|---|
| [`1.Index.md`](<./1.Index.md>) | The jump list. Every page in the wiki, one line each, no summaries. |
| [`2.Syntheses.md`](<./2.Syntheses.md>) | The distilled layer, with summaries. **Start here if you want the ideas, not the articles.** |
| [`3.Articles.md`](<./3.Articles.md>) | One-paragraph summary of every article page, grouped by category. |
| [`4.History.md`](<./4.History.md>) | Reverse-chronological table — "what did I read recently?" |

Summary text lives in exactly one place (`2.` and `3.`), so the index can stay lean and nothing drifts out of sync.

### The two kinds of page

**Article pages** live in the category folders under [`wiki/`](<./wiki/>) — `AI News`, `AI Insights`, `Building AI Agents`, and so on. Each one distils **one source** (occasionally two, when they cover the same ground) into a retrieval-friendly page: what it claims, what's interesting, how it relates to everything else already in the wiki. Every article page ends with a `## Sources` section linking back to the raw material in `source/`.

> **⚠️ Those `## Sources` links will 404 on GitHub.** The `source/` folder is gitignored and deliberately not published.
>
> It holds verbatim copies of other people's articles, clipped for the LLM to read. Keeping them locally is ordinary private reading; pushing them to a public repo is redistribution, and citing a URL is attribution, not a licence. The wiki pages are my own summaries and analysis, so those are mine to publish — the articles they're derived from are not.
>
> Nothing is actually lost: every source is a clipped article with its original URL in the frontmatter, so you can always reach the real thing. And in a local clone with its own `source/`, the links resolve normally.

**Syntheses** live in [`wiki/_syntheses/`](<./wiki/_syntheses/>) and are the point of the whole exercise. A synthesis is derived from *other wiki pages*, not from raw sources — it exists to hold a claim that **no single article makes**. For example, [*The Harness Is the Product*](<./wiki/_syntheses/The Harness Is the Product.md>) collects four independent vantage points — four labs' pricing, a Fowler essay, LangChain's middleware design, NVIDIA's silicon strategy — that separately converge on one conclusion none of them states alone.

Syntheses are deliberately **rare**. To qualify, a candidate needs a thesis no spoke states, at least four spokes (ideally from different categories), spokes that remain useful on their own, and real corroboration or genuine disagreement between them. If most pages belonged to one, the signal would be worthless. That's why there are only a handful here.

The structural tell: **a synthesis is the only kind of page with no `## Sources` section.** It cites nothing raw of its own; each spoke carries its own citations.

### Everything else

- **`inbox/`** — the ingest queue, normally empty. Dropping an article here is the signal that it's ready; once it's been ingested the LLM moves it (and its images) into `source/`. Anything still here after a run was skipped and is waiting on a decision. Gitignored, for the same reason as `source/`.
- **`source/`** — the raw material, immutable once it arrives. Articles clipped from the web, with their original URL in the frontmatter. The LLM reads from here and never edits anything in it; the only thing it ever does to this folder is move a freshly ingested article in from `inbox/`. Gitignored, per the note above.
- **[`settings/`](<./settings/>)** — the schema, the category list it applies to ([`categories.md`](<./settings/categories.md>)), and the original Karpathy gist that seeded it. `schema.md` holds the rules and is domain-independent; `categories.md` holds this wiki's seven categories and the boundary test for each.
- **[`tracking/`](<./tracking/>)** — operational bookkeeping, kept out of `wiki/` so the wiki stays pure content. `ingested.md` is an append-only record of which articles have been ingested, and into which pages; `log.md` is an append-only record of every ingest, query, lint and structural edit.
- **Tags** (`#agents`, `#ai-hype`, `#market-impact`, …) do the cross-cutting classification, so the folder tree can stay flat and shallow. Categories answer *"where does this file live"*; tags and syntheses answer *"what is this about"*.

### The three operations

- **Ingest** — a new article lands in `inbox/`, the LLM reads it, files a page, links it both ways into the existing wiki, updates the catalogs, and moves the article into `source/`.
- **Query** — ask a question; the LLM reads the index, pulls the relevant pages, and answers with citations. If the answer turns out to be worth keeping, it gets filed back in — sometimes as a new synthesis.
- **Lint** — a health check across the whole wiki: contradictions, stale claims, orphan pages, one-way links, near-duplicates that should merge, pages that should split, clusters that now deserve a synthesis.

---

## Build your own

You don't have to start from scratch. Karpathy's gist is the from-first-principles route; this repo is the *"here's one that already works, make it yours"* route. The schema encodes maybe a dozen lessons that each cost a session to learn.

### 1. Clone and reset

```bash
git clone https://github.com/dalembertian/my-wiki.git my-wiki
cd my-wiki
rm -rf .git && git init                 # start your own history

# Delete the content — all of it
rm -rf wiki                             # source/ isn't in the clone (see above)
rm 1.Index.md 2.Syntheses.md 3.Articles.md 4.History.md tracking/*.md
rm settings/categories.md               # my categories; yours will differ
```

That's the whole reset. The four catalogs and the two tracking files are fully specified in `settings/schema.md`, and its **Bootstrapping** rule tells the agent to recreate anything missing from that spec on its first run — so there's nothing to hand-stub, and no chance of a stub that disagrees with the schema.

**What survives:** `CLAUDE.md`, `settings/schema.md`, `settings/llm-wiki.md`, `.gitignore`, and this README (rewrite or delete it). `settings/schema.md` is domain-independent — you shouldn't need to edit a word of it to start. **What goes:** everything under `wiki/` — those are my notes, not yours. If you're using Codex or another agent, rename `CLAUDE.md` to `AGENTS.md`.

### 2. Point the agent at it

> Read `CLAUDE.md` and `settings/schema.md`. This is an LLM wiki, currently empty and with no categories decided yet. I'm going to start dropping articles into `inbox/`.

There's nothing to configure first. The categories that were here are gone with `settings/categories.md`, and the schema's rule is to write that file as folders get created — so your categories emerge from the first handful of articles rather than being guessed upfront. Don't try to design them in advance; you'll get them wrong, and the schema is built to let them be renamed later (mine were, twice).

### 3. Add your first article

Create `inbox/` and drop any markdown file into it (in Obsidian, just dragging a file in makes the folder) — ideally with `title:` and `source:` (the URL) in the YAML frontmatter, which is what the Obsidian Web Clipper produces by default. Then:

> There's a new article in `inbox/`. Ingest it.

The agent will ask whether you want to go one-at-a-time (discussing takeaways before writing) or batch. **Go one-at-a-time at first** — the early pages set the tone for everything after them, and this is when you'll discover which conventions you actually want. Every time something feels wrong, say so and have the agent amend the schema. That negotiation *is* the setup process.

### 4. Let it compound

After a dozen or so pages, try `Lint the wiki` and `Which pages now qualify as a synthesis?`. The wiki only starts paying off once there's enough in it for connections to appear — the first few sessions feel like data entry, and then it turns.

### If you use Obsidian

Not required — everything here is plain markdown and GitHub renders it fine — but it makes the whole thing considerably nicer:

- **[Obsidian Web Clipper](https://obsidian.md/clipper)** — a browser extension that turns any web article into clean markdown with the source URL in the frontmatter. This is how essentially every article in `source/` got here. One convention worth stealing (it's in the schema, though the folder itself isn't in this repo): clip into a scratch `Clippings/` folder that the agent is told to ignore entirely, and make **moving a file from `Clippings/` to `inbox/` the manual signal that it's ready to ingest**. Raw clippings usually need work first — renaming, downloading images, stripping paywall cruft, or just deciding they aren't worth keeping — and this keeps the half-finished ones out of the agent's work queue instead of having it ask about them every session.
- **[obsidian-git](https://github.com/Vinzent03/obsidian-git)** — auto-commits the vault every 10 minutes and pushes on a schedule. You get full version history of your wiki for free, without ever thinking about it.
- **Download images locally** — Settings → Files and links → set the attachment folder to `inbox/_assets/`, then bind the "Download attachments for current file" command to a hotkey. The agent can then actually look at the images instead of at dead URLs.
- **Graph view** — the fastest way to see the shape of your wiki: what's a hub, what's peripheral, what's orphaned.
- Work with the agent on one screen and Obsidian on the other. Obsidian is the IDE, the LLM is the programmer, the wiki is the codebase.

---

## License

Two licences, because this repo is two things:

- **The machinery** — `CLAUDE.md`, `settings/schema.md`, `settings/categories.md` and this README — is [MIT](<./LICENSE>). Take it, adapt it, build your own wiki on it. That's the point.
- **The wiki content** — everything under `wiki/`, plus the four catalogs — is [CC BY 4.0](<./LICENSE-CONTENT>). It's my own writing; reuse it freely with credit.

Two things are covered by neither: [`settings/llm-wiki.md`](<./settings/llm-wiki.md>) is Andrej Karpathy's, reproduced verbatim with attribution, and `source/` isn't published here at all (see the note above).

---

*Built jointly, one argument at a time, by* **Rubens & Claude**.
