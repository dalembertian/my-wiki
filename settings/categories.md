# Categories

The category list for *this* wiki — one entry per folder under `wiki/`, with the boundary rule that decides what lands in it. Split out of `settings/schema.md` on 2026-09-11: the schema holds the domain-independent rules about how categories work, this file holds the domain-specific list they're applied to. A wiki about something else replaces this file wholesale and leaves the schema alone.

**Two sources of truth, deliberately.** The folder tree under `wiki/` decides *which* categories exist; this file records *what each one means*. Neither is derivable from the other — a folder name can't carry a boundary rule, and a rule written here for a folder nobody created is fiction. A category folder with no entry here, or an entry with no folder, is a lint error (see the Lint workflow in `settings/schema.md`).

Maintained by the LLM, same as the wiki itself: when a category is created, renamed or retired, its entry here changes in the same pass. Each entry should say what the category holds *and*, where the name alone is ambiguous, the test that separates it from its nearest neighbour.

## Current categories

- `AI News` — news, events, market reactions, announcements
- `AI Tools` — specific tools/products
- `Building AI Agents` — architecting and operating agent systems themselves: harnesses, function calling, MCP, memory, frameworks and libraries, learning resources. The **agent is the thing being built**
- `Building Software with Coding Agents` — using an agent as a tool inside your own development workflow: coding-agent skills files, IDE/editor integration, dev-workflow shifts. The **agent is the thing being used**, and software is what gets built
- `AI Fundamentals` — core ML/deep-learning technical concepts and history, not agent-specific: includes general prompt-engineering technique/mechanics that aren't tied to agents or a dev workflow (split out from what is now `Building AI Agents` once it grew to 11 mixed pages — see `tracking/log.md` 2026-08-25)
- `AI Companies & Initiatives` — profiles of companies/orgs and what they're doing
- `AI Insights` — analytical and opinion pieces: articles arguing a thesis rather than reporting an event. Still one-source-per-page like every other category (contrast with the `Syntheses` layer, which is where cross-page synthesis lives — including good query answers, which used to be filed here)

## Boundary notes

The two `Building …` categories name an **activity, not a topic** — deliberately, and renamed to say so on 2026-09-11 (they were `AI Agents` and `AI Developer Tools`). A topic-shaped name like "AI Agents" attracts anything agent-flavoured, which is how it reached 10 pages while its sibling held 2. Ask which thing is being *built*: a framework comparison for choosing what to build an agent with is `Building AI Agents`, even though it is developer tooling; a skills file that makes your coding agent behave is `Building Software with Coding Agents`, even though it is about an agent.

Note that neither is where *descriptive* agent coverage goes. Reporting on what agents are doing in the world stays split by form across `AI News`, `AI Insights` and `AI Companies & Initiatives`, with `#agents` and the `Syntheses` layer doing the cross-cutting work — categories answer "where does this file live," not "what is this about."
