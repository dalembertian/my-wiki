# LLM Wiki — Schema

This is the operating schema for the LLM wiki: the concrete conventions layered on top of the general pattern described in `settings/llm-wiki.md`. This document is the living, evolving config — update it collaboratively with the user as conventions get refined. `llm-wiki.md` itself stays as-is; it's the reference article that seeded this project.

## Boundaries

- **Everything stays inside this vault.** Never read or write any file outside this folder — not elsewhere on the filesystem. If a task seems to require touching something outside this folder, stop and ask — or, in an unattended run, skip the source and move on (see "Interactive vs. unattended ingestion").
- **`inbox/` is the ingest queue.** Added 2026-09-24. The user drops articles here that are ready to ingest, and **everything in it is pending work**: to find out what needs ingesting, list `inbox/`. There's nothing to diff it against. Normally it's empty, apart from an empty `_assets/` folder, which stays. An article still sitting here after a run was skipped or is waiting on a decision (see "Interactive vs. unattended ingestion"). Articles with local images bring them along in `inbox/_assets/<folder>/`.
- **`source/` is the archive of ingested articles.** It holds the user's curated raw material (articles, papers, images, data) once it has been ingested. Read from it freely, but never edit, delete, or rename anything in it. The only write it ever receives is the last step of a successful ingest: moving an article, and its assets folder if it has one, over from `inbox/` (see "Ingest workflow"). Nothing else is created in it. The one exception is a duplicate the user has decided to swap out (see "Duplicates, including translations"). Even then, nothing is deleted: `source/` is gitignored, so the displaced article and its assets go to the vault's `.trash/` and `.trash/_assets/`, where they can still be recovered. Before 2026-09-24 the user dropped articles straight into `source/` and pending work was found by diffing it against `tracking/ingested.md`; `inbox/` replaced that.
- **An *article* is a `.md` file sitting directly in `inbox/` or `source/`** — that, and nothing else. Defined 2026-09-11, so that an unattended run and an interactive session agree on what a folder holds without either of them having to guess. Everything else in those folders is invisible to the ingest: subfolders and their contents (`_assets/` holds images that belong to articles, not articles), non-markdown files, and operating-system artefacts such as `.DS_Store`. Only articles are ingested, recorded in `tracking/ingested.md`, or counted as pending work. An article's images in `_assets/` are still read when they help make sense of it (see "Ingest workflow") — being readable is not the same as being ingestable.
- **`Clippings/` is the user's staging area — ignore it entirely.** Added 2026-09-11. New articles are clipped from the web into here, not into `inbox/`, because they usually need work first (renaming, downloading images, dropping paywall cruft, deciding whether they're worth keeping at all). Treat it as if it weren't there: never read it, never ingest from it, never list its files in `tracking/ingested.md`, and never move anything out of it. **Promotion from `Clippings/` to `inbox/` is a manual user action, and it is the only signal that an article is ready.** An article sitting in `Clippings/` is not pending work — it's work the user hasn't decided on yet, so don't volunteer it, count it as unprocessed, or ask about it each session. The folder is gitignored, so it won't appear in version control either.
- **`wiki/` is LLM-owned.** This is the layer you write and maintain — category folders and pages. The user reads it and will generally not edit it directly.
- **`1.Index.md`, `3.Articles.md` and `2.Syntheses.md` live at the root folder**, not inside `wiki/` — moved there 2026-08-27 so the user can see the catalog without opening the `wiki/` folder first. Still LLM-owned/maintained the same as everything under `wiki/`; only their location differs.
- **`settings/` is co-owned.** Configuration and instructions, including this file. Both the user and the LLM can propose changes here as the wiki's conventions evolve. One exception: `settings/categories.md` is LLM-maintained like the wiki itself, since it has to track the folder tree — see "Wiki structure".
- **`tracking/` is LLM-owned operational bookkeeping**, kept separate from `wiki/` so the wiki itself stays pure content: `log.md` (activity log) and `ingested.md` (append-only record of which articles have been ingested, and into which pages).

## Wiki structure

The wiki is organized by a small set of **category folders** directly under `wiki/`, not by entity/concept/source-type. **The list itself lives in [`settings/categories.md`](<./categories.md>)**, not here — split out 2026-09-11 so that this document stays domain-independent and a wiki about another subject can replace the list without touching the rules. Read that file when deciding where a page goes; the folder tree under `wiki/` is what says which categories exist, and `categories.md` is what says what each one means.

The rules that govern the list, wherever it points:

- **Categories name a slice of the collection, and often an activity rather than a topic.** A topic-shaped name attracts anything of that flavour and grows without bound; a name that states what the page is *doing* stays decidable. When two categories are adjacent, the entry in `categories.md` should carry the test that separates them, not just a description.
- **Keep the list minimal.** Prefer folding a new page into an existing category over creating a new one. Only add a category when several pages clearly don't fit anywhere existing — and when you do, add its entry to `categories.md` in the same pass, with its boundary rule.
- **Pages live flat inside their category folder** — no nested subfolders.
- **The list is expected to evolve.** Renames and splits are normal (see `tracking/log.md` for this wiki's). Log every one as an `edit`.

**Sub-classification via tags, not subfolders.** A page often straddles more than one theme (e.g. a news piece that's also "hype"-flavored). Rather than nested folders, use Obsidian inline tags in the page body (e.g. `#ai-hype`, `#agents`, `#market-impact`) so a page can carry several cross-cutting labels at once. Tags are browsable in Obsidian's tag pane without any plugin. Keep tags loose and reusable — don't invent a new one-off tag per page. Categories answer "where does this file live," not "what is this about" — the cross-cutting work belongs to tags and the `Syntheses` layer.

### The `Syntheses` layer

`wiki/` holds two different kinds of thing, and they are separated by folder:

- **Category folders** (`AI News`, `AI Agents`, …) hold *article pages* — one page derived from one or more files in `source/`. Every such page has a `## Sources` section.
- **`wiki/_syntheses/`** holds *distilled pages* — derived from other wiki pages rather than from source material, existing to hold a claim that spans several of them. Added 2026-09-06. The leading underscore is deliberate: it marks the folder as a different kind of thing from the Title Case category folders and sorts it to the top of the file list.

**The structural marker is the `## Sources` section: syntheses are the only pages without one.** They cite nothing raw of their own; each spoke carries its own citations. This is machine-checkable — a page in `wiki/_syntheses/` with a `## Sources` section, or a category page without one, is a lint error.

A synthesis is a flat folder like any category (no subfolders), same depth as a category folder, so `../../source/` relative links still resolve if one ever needs to cite a source directly.

**Syntheses must stay rare.** The whole value is that a synthesis signals "these pages cohere into something." If most pages belong to one, that signal is gone and the layer is just `1.Index.md` with prose. Prefer adding a spoke to an existing synthesis over starting a new one, and expect the wiki to support only a handful at any size.

To qualify, a candidate needs all four:

1. **A thesis no single spoke states.** If one page already makes the argument, link to that page — it is the hub, and no new page is needed. This is the test that actually filters; everything else is necessary but not sufficient.
2. **At least four spokes**, ideally drawn from more than one category — synthesis boundaries cut across categories, since categories answer "where does this file live" and syntheses answer "what do these pages jointly claim."
3. **Spokes that stay independently useful.** If the spokes only make sense read together, that is a merge (see "Splitting and merging"), not a synthesis.
4. **Corroboration or genuine disagreement between spokes** — several independent vantage points converging on one claim, or arguing about one question. A set of pages that merely share a topic is a tag, not a synthesis.

Syntheses follow every other convention: Title Case filename matching the `# Heading`, reciprocal links to and from every spoke, a trailing tag line including `#synthesis`, and entries in `2.Syntheses.md` (summary text) and `1.Index.md` (link only) — never in `3.Articles.md`. They do **not** get a `4.History.md` row — that file answers "what did I read recently," and a synthesis is not something read.

**No source-entity tracking.** Don't create separate pages for the publisher/outlet/firm behind a source (e.g. a research firm quoted in an article) unless it becomes a recurring subject worth its own `AI Companies & Initiatives` page in its own right — not just because it was cited once.

**Naming:** page filenames use Title Case matching the page's `# Heading` (e.g. `wiki/AI News/Some Page.md` with `# Some Page` as its first line, and no frontmatter — the filename and the `# Heading` are the only place the title lives). Prefer a concise, retrieval-friendly title over reproducing a source's full headline.

**Sources section:** raw-source citations live in a `## Sources` section near the bottom of the page (after "Cross-reference", right before the trailing tag line) — not in frontmatter. A YAML value in frontmatter isn't clickable in Obsidian; a markdown link is. One bullet per source, no blank lines between bullets:

```markdown
## Sources
- [Exact Source Filename](<../../source/Exact Source Filename.md>)
```

When a page synthesizes more than one source (e.g. an article plus a separate notes/comment file on the same piece, two parts of one series, or two independently-sourced pages merged together), every source gets its own bullet in the same section:

```markdown
## Sources
- [First Source](<../../source/First Source.md>)
- [Second Source](<../../source/Second Source.md>)
```

Link text is the source's filename without the `.md` extension. The link always points into `source/`, even though at the time the page is written the article is still in `inbox/`: the move at the end of the ingest is what makes the link resolve (see "Ingest workflow"). The link target is a **plain relative-path link**, not a wikilink — deliberately. Obsidian resolves a bare `[[Name]]` wikilink by title across the *whole vault*, and a source file often shares its exact filename with the wiki page derived from it (e.g. `source/Loop Engineering.md` and `wiki/Building AI Agents/Loop Engineering.md`), so a bare wikilink can silently land on the wrong one. A relative path wrapped in `<>` (needed since source filenames contain spaces) is unambiguous and always resolves to the file actually inside `source/`. Use the same explicit relative-path style (not a bare wikilink) for any other reference that crosses into `source/`.

**Cross-referencing between wiki pages:** use Obsidian wikilinks (`[[Page Name]]`) freely — collision risk is low since these are wiki-authored titles. Tags stay inline in the page body (`#tag`), on the trailing tag line.

**Links must be reciprocal.** If page A's body or Cross-reference section links to page B, then B must link back to A. Adopted 2026-09-06 after a lint found five orphan pages with zero inbound links — all of them from large ingest batches, all of them with outbound links of their own (see `tracking/log.md`).

The failure mode this prevents is structural, not careless: writing a new page's own Cross-reference section is natural, because the new content is in mind and you reach for what it connects to. Creating an *inbound* link means going back and editing a different, already-finished page — so it gets skipped, and the new page looks complete without it. The result is that a page's inbound count degrades into "how many pages happened to be written after it," which leaves peripheral and single-topic pages permanently unreachable no matter how good they are.

Applying it:

- **When creating a page**, after writing its Cross-reference section, open every page it links to and add the reciprocal mention. This is part of "update any other existing pages it materially affects" in the Ingest workflow — the part most easily skipped.
- **Write the reciprocal, don't stub it.** The return link should say what the other page adds *from this page's point of view*, which is usually not the same sentence reversed. A bare "See also [[X]]" appended to a page is worse than nothing — it's link-graph noise that makes the Cross-reference section stop being worth reading.
- **If a reciprocal genuinely can't be written** — the connection is real in one direction but says nothing useful in the other — that is a signal the outbound link was weak. Remove it rather than forcing a hollow return link.
- **Batch ingests need an explicit pass at the end.** Reciprocity fails most under batch pressure, when later pages link to a few central hubs and nothing links back to the periphery. Before closing a batch, re-check every page created in it for inbound links.
- **Syntheses are the exception to spoke-count limits, not to reciprocity**: a synthesis links to all its spokes and every spoke links back to it (see `wiki/_syntheses/`).

### Splitting and merging: denser consolidation

The default is still one source → one page, but don't force it when the content doesn't actually fit that shape. Two moves are available, in either direction, at ingest time or later:

**Splitting one source into multiple pages** — do this when a source bundles genuinely independent sub-topics that would each stand alone (different category fit, different tag cluster, doesn't need the other half's context to make sense) and bundling them actively hurts retrieval (a query about one sub-topic forces wading through the other). Length alone is not a reason to split — a long single-thesis article stays one page. When split:
- Each resulting page's `## Sources` section cites the same original source as its own bullet (plus bullets for any other sources that page also draws on).
- Add explicit `[[...]]` cross-references between the split pages so the reader can still find the sibling content.
- `tracking/ingested.md`'s line for that source lists every resulting page, comma-separated: `- Source File.md — [Page A](<…>), [Page B](<…>)`.

**Merging multiple pages into one** — already supported at ingest time via a multi-bullet Sources section (see above); this extends it to apply retroactively too, whenever overlap is discovered later (typically during a Lint pass, but can happen ad hoc). Merge when two or more pages argue essentially the same thesis from different sources (near-duplicates), or when their cross-references to each other are effectively substituting for sections a single page would have anyway. Don't merge pages that stay independently useful and retrievable on their own — for a cluster of related-but-distinct pages, prefer a short hub page (shared framing plus links to each spoke) over swallowing everything into one page, when each spoke still earns its own entry point. When merged:
- The surviving page's `## Sources` section gains a bullet for every contributing source.
- The old pages are deleted; their entries removed from `3.Articles.md` and `1.Index.md`.
- `tracking/ingested.md` lines for their sources are repointed to the surviving page (or hub).

Log every split or merge as an `edit` entry in `tracking/log.md`, same as any other structural change.

## Bootstrapping

The bookkeeping files — `1.Index.md`, `2.Syntheses.md`, `3.Articles.md`, `4.History.md`, `tracking/log.md` and `tracking/ingested.md` — are fully specified by the sections below, as are the `inbox/`, `source/`, `wiki/` and `wiki/_syntheses/` folders. **If any of them is missing, create it from its spec rather than stopping**, and carry on with the operation. Added 2026-09-11.

`settings/categories.md` is the one file **not** covered by this rule: its contents are domain-specific, so an absent or empty one means the categories haven't been decided yet, not that they need regenerating. Start from whatever the first sources actually are, and write each entry as the folder is created.

This is what makes a fresh wiki startable by deleting content and nothing else: an empty vault plus this schema is a complete starting state, and the first ingest rebuilds the scaffolding on its way past. It also means a reset never involves hand-writing header lines that this document already describes — if the two ever disagreed, the hand-written stub would be the wrong one.

## 1.Index.md, 2.Syntheses.md and 3.Articles.md

Three root-level catalog files, each with one job (plus `4.History.md` below). They are numbered `1.`-`4.` in reading order, which also fixes their sort order in the file list — renamed 2026-09-06. They live at the root folder (not inside `wiki/`), so the user can see the catalog without opening a subfolder — page links from here into `wiki/` therefore start with `wiki/...`.

- **`1.Index.md`** — the entry point. Links only, no summary text, covering the **whole** wiki: syntheses first, then the categories. This is what the Query workflow reads first, so anything missing here is effectively invisible.
- **`3.Articles.md`** — per-page summaries for the category pages. (Named `summaries.md` until 2026-09-06; renamed once `2.Syntheses.md` existed, since both hold summaries and the distinguishing thing is *what* they summarize.)
- **`2.Syntheses.md`** — per-page summaries for the `wiki/_syntheses/` layer.

The invariant that keeps these from drifting: **summary text lives in exactly one file.** `1.Index.md` holds none of it — only pointers — so listing a page there as well as in its detail file is not duplication.

- **`3.Articles.md`** is where the real per-page summary lives — the condensed format: grouped by category folder (mirroring "Wiki structure" above), one bullet per page: `- [Page Title](<wiki/Category Folder/page.md>) — one-paragraph summary. #tag1 #tag2 ^block-id`. The trailing `^block-id` is an Obsidian block reference (short kebab-case, unique per entry) that lets `1.Index.md` link precisely to that bullet. Leave a blank line between bullets here — it's the more spacious, browsable view.
- **`1.Index.md`** is the lean jump-list: no summary text — one line per page, no blank line between entries within a category (blank lines only around headers), linking straight to that page's block in `3.Articles.md`: `- [[3.Articles#^block-id|Page Title]]`. Its layout, as of 2026-09-06:
  - A **navigation line** at the very top linking the three sibling catalogs and `settings/schema.md`. This exists because `1.Index.md` is the documented entry point but previously had no route to `4.History.md` at all, and reached the other two only via deep block links that land you mid-file.
  - `## Syntheses`, linking into `2.Syntheses.md`.
  - A one-line italic description under each of the two `##` headings, stating what that section holds and how the two relate (the syntheses one also says "Start here"). Kept to a single line each — they exist so the distinction is legible when opening the wiki cold, not as preamble.
  - A `---` rule, then `## Categories` (symmetric with `## Syntheses`), under which each category is an `###` heading. The two-level structure keeps the distilled layer visually separate rather than letting it read as just another category.

Use a block reference (`^block-id`), not a heading link — heading-anchor links (`[[3.Articles#Page Title]]`) were tried first and did not reliably scroll to the right spot once the vault's link-consistency plugin rewrote them into plain markdown links with unencoded, space-containing fragments. Block IDs are short and space-free, which sidesteps that.

Beyond the structure described above, no preamble or commentary in either file — the catalogs stay scannable.

**`2.Syntheses.md`** is the third root-level catalog, the entry point for the `wiki/_syntheses/` layer, and it is deliberately *not* two-tier. `1.Index.md`/`3.Articles.md` are split because a combined jump-list grows unwieldy across dozens of article pages; syntheses stay few by design (see "Syntheses must stay rare"), so that pressure never applies. `2.Syntheses.md` therefore carries the summary text inline and links straight to each page — one bullet per synthesis, blank line between bullets, no category headers. It does carry `^block-id` references, same as `3.Articles.md`, so `1.Index.md` can link precisely to an entry:

```markdown
- [Synthesis Title](<./wiki/_syntheses/Synthesis Title.md>) — one-paragraph summary of the claim it holds. #synthesis #tag ^block-id
```

Syntheses appear in `1.Index.md` (as links, under a `## Syntheses` section placed first) and in `2.Syntheses.md` (with their summary text). They do **not** appear in `3.Articles.md`, which stays purely about category pages, nor in `4.History.md`. One entry per synthesis, in one file, so the three catalogs can't drift apart.
 Update both whenever a page is created or its summary changes materially. When answering a query, read `1.Index.md` first (or `3.Articles.md` directly if you need the one-paragraph context) to find candidate pages before drilling into them.

## 4.History.md

The fourth root-level catalog file (alongside `1.Index.md`, `3.Articles.md` and `2.Syntheses.md`), for the "what did I read recently?" case — category pages only, never syntheses — a reverse-chronological table, one row per page: `| Date | Category | Article |`, newest first, linking straight into `wiki/...` the same way `3.Articles.md` does. Within the same date, rows sort alphabetically by article title.

The date recorded is when the page's content was first substantively established in the wiki — the original `ingest` entry for a page that's never been touched since, or the date of the `edit` that created/merged a page into its current form when that differs (e.g. a page assembled by a later merge gets the merge date, not the date of whichever source article was read first). Split pages each keep the date they were split off.

Add a row whenever a new page is filed. If a merge or split changes what pages exist, update `4.History.md`'s rows the same way `3.Articles.md`/`1.Index.md` get updated (drop rows for deleted pages, add rows for new ones, dated to the edit that made the change) — see "Splitting and merging" above.

## tracking/log.md

Append-only, one entry per operation (ingest, query, lint, edit), using this exact prefix so it stays greppable:

```
## [YYYY-MM-DD] type | Title
```

where `type` is one of `ingest`, `query`, `lint`, `edit`. A one- or two-line note under the heading is enough — what happened, what changed. This makes `grep "^## \[" tracking/log.md | tail -5` a quick way to see recent activity.

## tracking/ingested.md

An append-only record of every article that has been ingested, and which page(s) it produced. One line per article, newest at the bottom:

```
- Exact Source Filename.md — [Resulting Wiki Page](<../wiki/Category Folder/Resulting Wiki Page.md>)
```

The link is a relative-path markdown link into `wiki/`, the same style `3.Articles.md` uses. Never link to the article's own file in `source/`: that link would be to the input, not to what it produced.

Until 2026-09-24 this file was a checklist that had to be diffed against `source/` to find unprocessed articles. That job now belongs to `inbox/` (see "Boundaries"), so there's no longer any unticked state and the checkboxes were dropped: an article gets a line only once it has been successfully ingested and moved to `source/`, and a skipped article gets none. Lines are only ever changed afterwards to repoint them after a split or merge (see "Splitting and merging").

## Ingest workflow

**Duplicates, including translations.** Added 2026-09-25, after an English article was ingested as a new source for a page already built from its Portuguese original. Before filing anything, check whether the article is already in the wiki under another name. Filenames won't catch this: a translation, a republication or a re-clip has a different title and a different `source:` URL. Compare these signals against `3.Articles.md` and the articles in `source/`, and treat a match on any two of them as a probable duplicate:
- same author or publisher, and a `source:` URL on the same site (a language segment such as `/en/` or `/pt/` in the path is a strong hint);
- a title that says the same thing in another language;
- the same section structure, the same examples, the same quoted people or the same figures.

Interactive: flag it and ask the user which version to keep. **By default keep the English version**, because the wiki is written in English. Unattended: skip it (see below). If the user keeps the new version, it replaces the old one rather than joining it:
- the page's `## Sources` bullet is repointed to the new article, and any note about the source's language is updated;
- the old article's line in `tracking/ingested.md` is removed, and the new one gets its own line;
- the old article and its assets folder move from `source/` to `.trash/` and `.trash/_assets/`;
- `3.Articles.md`, `1.Index.md` and `4.History.md` change only if the page's summary does. The History date stays the date the page was first written.

If the user keeps the old version, the new article is simply removed from `inbox/`. Log either outcome as an `edit`. A genuinely different piece by the same author on the same topic is not a duplicate; it gets its own page or a second `## Sources` bullet as usual.

**Personal/living-list notes are out of scope.** If a source file has no clipped-article frontmatter (no `source:` URL — i.e. it's the user's own running note, not something clipped from the web), don't fold it into the normal ingest flow. Flag it and ask what to do — an unattended run skips it instead (see "Interactive vs. unattended ingestion"). Decided 2026-08-25: no auto-detection or special "living list" handling — the wiki stays one-article-to-one-static-page throughout. The user removes such files from `inbox/` themselves when they don't want them ingested; treat that as the default resolution unless told otherwise for a specific file.

### Interactive vs. unattended ingestion

Ingestion runs in one of two modes. The only thing that separates them is whether a human is available to answer a question mid-run — the conventions in this document apply identically either way.

**Interactive** — a person is at the other end of the session. New article(s) show up in `inbox/`. Before processing, **ask the user** whether to:
- go **one at a time**, discussing key takeaways together before writing anything, or
- **batch**, processing everything in one pass and reporting a summary afterward.

There's no fixed default — always ask when there's new material to ingest, since it can vary session to session.

**Unattended** — an external automated process performs the ingest on a trigger or a schedule, with nobody to ask. Added 2026-09-10. It is always **batch**: never ask which mode to use, never pause for confirmation, never wait for input at any point.

The trade for that autonomy is a strict rule: **wherever the interactive flow would stop and ask, an unattended run skips that source instead** — leaves it (and its assets) where it is in `inbox/`, records nothing for it in `tracking/ingested.md`, changes nothing in the wiki on its account, and moves on to the next one. Nothing is guessed and nothing is left half-written. A skipped source is not lost: it stays in `inbox/` and gets picked up by a later run once whatever caused the skip is resolved. Concretely, an unattended run skips rather than decides when:

- the source has no clipped-article frontmatter (see "Personal/living-list notes are out of scope" above);
- it looks like a duplicate of something already ingested, a translation included (see "Duplicates, including translations" above);
- no existing category fits it — unattended runs **never create a category folder, and never edit `settings/categories.md`**, since the category list is meant to stay minimal and folding-vs-creating is a judgement worth a human (see "Wiki structure");
- it looks like it wants splitting across pages, or merging into an existing page (see "Splitting and merging");
- `source/` already holds an article with the same filename, or an assets folder with the same name (never overwrite — see step 5 below);
- anything else about it is unexpected — an unreadable file, a situation this schema doesn't cover, or a step that would mean touching something outside this vault.

Two further limits. Unattended runs do **not** create, revise, or retire pages in `wiki/_syntheses/`, and therefore never touch `2.Syntheses.md`: a synthesis has to clear four criteria and stay rare, which is a call for a human-initiated pass, not a side effect of filing new articles. And they keep their own operational record — run times, failures, what was skipped and why — **outside** the wiki; inside the wiki they append to `tracking/log.md` exactly like any other operation, and add nothing else.

What does *not* relax unattended: the end-of-batch reciprocity pass (see "Links must be reciprocal"). It matters more here than interactively, not less, because nobody is reading the pages as they are written.

For each article in `inbox/`:
1. Read it (and any local images in its `inbox/_assets/` subfolder if relevant to understanding it), and check that it isn't already in the wiki in another form (see "Duplicates, including translations").
2. If one-at-a-time: discuss key takeaways with the user before writing.
3. File a page under the category folder it fits best, per `settings/categories.md` (create a new category only if nothing existing fits, adding its entry there in the same pass — see "Wiki structure"; interactive only, an unattended run skips the source instead), and update any other existing pages it materially affects — including adding the reciprocal link to every page this one links to (see "Links must be reciprocal").
4. Add its entry to `3.Articles.md` (one bullet with summary, tags and a `^block-id`), `1.Index.md` (one-line link to that block), and a row to `4.History.md` (dated today).
5. **Move the article from `inbox/` to `source/`, together with its assets folder** (`inbox/_assets/<folder>/` → `source/_assets/<folder>/`), if it has one. Do this only once everything above has been written, so that a failure partway through leaves the article in `inbox/` and still pending. Moving never overwrites: if either target name already exists in `source/`, stop and ask (unattended: skip the article, see above). Find the assets folder by following the article's own image links rather than guessing it from the filename, because Obsidian changes some characters in folder names (`?` becomes `-`, for example). The article and its assets move together, so its relative `_assets/...` image links keep resolving. Afterwards, check that the files really are in `source/` and gone from `inbox/`. When the target already exists, `mv -n` can do nothing at all, and on some systems (macOS included) it still reports success, so the exit status alone doesn't prove the move happened.
6. Append its line to `tracking/ingested.md`.
7. Append an entry to `tracking/log.md`.

## Query workflow

Read `1.Index.md` first to locate relevant pages, then read those pages and synthesize an answer with citations (wikilinks back to the relevant wiki pages, relative-path references for anything in `source/`). Answers can take different forms depending on the question (markdown page, comparison table, chart, etc.) — pick what fits.

If the answer is substantial enough to be worth keeping (a comparison, an analysis, a connection worth remembering), offer to file it back into the wiki rather than letting it disappear into chat history. A query answer that synthesizes several existing pages belongs in `wiki/_syntheses/` if it meets the four criteria there — that is exactly what the layer is for. If filed as a synthesis, add it to `2.Syntheses.md` and `1.Index.md`; if filed as a category page, update `3.Articles.md`, `1.Index.md` and `4.History.md` as usual. Either way, log it as a `query` entry in `tracking/log.md`.

## Lint workflow

On request, health-check the wiki:
- contradictions between pages
- stale claims superseded by newer sources
- orphan pages with no inbound links, and one-way links whose reciprocal is missing (see "Links must be reciprocal")
- topics mentioned repeatedly but lacking their own page
- missing cross-references or tags
- pages that are near-duplicates of each other, or so densely cross-referenced they'd read better merged (see "Splitting and merging" above)
- sources whose content spans clearly unrelated categories that might read better split into multiple pages
- any `source/` links that resolved wrong or point outside the wiki (see "Sources section" note above)
- whether `tracking/ingested.md` has a line for every article in `source/` (and no line for an article that isn't there), whether every `source/_assets/` folder belongs to an article in `source/`, and whether anything has been sitting in `inbox/` since before the last run (a skip that nobody has dealt with)
- two articles in `source/` that are the same piece: translations, republications or re-clips (see "Duplicates, including translations")
- whether the current category list still fits, or needs consolidating/splitting — and whether `settings/categories.md` still matches the folders actually under `wiki/` (a folder with no entry, or an entry with no folder, is an error)
- whether any `Syntheses` page has grown a `## Sources` section, or any category page has lost one
- clusters that now meet the four criteria for a new synthesis — and existing syntheses whose thesis no longer holds up against newer pages

Report findings, apply fixes the user agrees with, and log the pass as a `lint` entry in `tracking/log.md`.
