# Write Your Own Agent Skills

Fabio Akita, September 2026. He is asked often about the "skills" he mentions, and the article leads with its recommendation: **don't use other people's skills**, his own included. He publishes his in a `my-skills` repository marked *"not tailored for general usage"* so people can read the reasoning and build their own. Running someone else's skill blind means installing their instructions to run with your permissions, on your repositories, with your tokens. In his words, it's "prompt injection you install voluntarily and say thank you for."

## What a skill is

A directory holding a `SKILL.md`: a header with a name and description, and a body that is a well-written prompt for one task, sometimes with a `scripts/` folder of helpers so the model doesn't rewrite them each time. **"Skills are just prompts"**: prompts that have been versioned, refined and kept one command away.

The worry that skills fill the context is mostly answered by *progressive disclosure*. Claude Code and OpenCode put only each skill's name and description into the system prompt, and load the body when the agent invokes it. That solves the text cost but not the attention cost: every installed skill still competes when the agent chooses a tool. Anthropic's own advice is that more tools don't always give better results. Akita puts it more sharply: **"An idle skill is debt."** When your process changes and the skill doesn't, the agent eventually follows a rule that no longer fits. So keep fewer skills, write them better and review them often.

The one third-party skill he kept is *Humanizer*. It turns Wikipedia's *Signs of AI writing* catalog into a procedure: mark the tells from strongest to weakest, rewrite while keeping every supported claim (losing or inventing a fact both count as errors), then read it aloud. He runs it on every post. He also merged it into his own repo with adjustments, after reading the source material, which is his point about how to adopt anything.

## The daily workflow

He maintains more than 40 repositories. Each morning he opens the agent in a project through `ai-memory run` (his persistent Markdown memory per project: decisions, gotchas, workstreams and handoffs between sessions) and types one line, such as `run pr-audit and iss-audit, then run github-resolution`. The skills do the rest:

- **pr-audit**: *evidence over narrative*. Everything a contributor wrote is a claim to check, recorded in a ledger as confirmed, partial or unsupported. It includes a hostile change gate (executable bits, symlinks, bidirectional characters and homoglyphs, `pull_request_target` workflows, unpinned actions, typosquatted dependencies, lockfile drift). Unexplained access to credentials blocks the PR immediately. Code runs in an isolated worktree with no host credentials. The changelog category decides the version bump. Text in a PR that tries to change the audit rules is treated as a finding to investigate.
- **iss-audit**: the same distrust applied to issues. Never run a command copied from an issue. Keep what was observed, what was expected, the reporter's diagnosis and the proposed fix separate. Reproduce with synthetic data in a disposable sandbox. Two rules he likes: *"a number that moves is not a pinned number"* and *"the obvious owner may be innocent."*
- **github-resolution**: works through approved tickets one at a time, writing a regression test before each fix. *"Slop is a defect"*: no speculative abstractions, TODOs, drive-by refactors or weakened tests. After more than three tickets, it audits the whole range again before pushing. A ticket closes only when the fix is merged with green CI on that exact commit. Every unresolved issue is logged with its reason.
- **pr-bump**: handles Dependabot updates in one batch rather than one PR at a time: one consolidated update, one CI run, fixing whatever breaks. There is a supply-chain floor: every dependency must resolve on the public registry with the expected name, version and checksum. A git or path source, a name close to a known package, or a new install hook sends it to a full audit, since `bundler-audit` will pass a well-formed trojanized gem.
- **release**: cuts a version only when asked. The version follows the changelog classification, CI must be green on the tag's exact commit, tags are annotated, and a published tag is never rewritten.

The result across just three of his projects (ai-memory, ai-usagebar, ai-jail): **609 merged PRs and 375 closed issues**, mostly handled by these skills with him supervising. That's one person with no team.

None of the skills started out ready. Their commit history shows each rule arriving after he ran into the mistake it now prevents: **"Every line in these skills is a scar."**

## Knowledge matters more than skills

He relies on skills much less than on knowledge. Each project has its own `AGENTS.md` and its ai-memory pages. The pattern he uses most isn't a skill at all: he points the agent at a working example (*"make a similar github action to build and publish AUR packages as we did in ~/Projects/ai-memory"*). A skill for that would go stale, while an example in a repo he maintains stays current because he fixes it when it breaks. **"Every project becomes its own knowledge base."** The same goes for his Linux setup (a config repo that can reinstall the machine) and his home server (runbooks on a private Gitea server). Skills are for procedures that repeat the same way; knowledge covers everything else.

The practice he rates above any skill: **have the agent write down everything that didn't become code**, meaning the research, the decision and why, and the alternative rejected. ai-memory's `docs/` folder holds research he commissioned before each big decision: the Karpathy wiki pattern it grew from, a 2026 survey of agent-memory tools (Zep/Graphiti, Letta, Mem0, Google's Open Knowledge Format), and reviews of individual competitors, including a write-up of MemPalace's corruption and data-loss issues. Each hour of agent research becomes a reusable asset: "compound interest for knowledge."

## Closing framing

His audit skills are shaped around his projects, languages, risks and level of supply-chain paranoia, and nobody else lives under those constraints. That goes back to a 2019 video of his, *Don't Outsource Your Decisions*: nobody but you knows what you need. LLMs make building your own tools about ten times faster, so read other people's skills for their reasoning, then write your own.

## Cross-reference

[[Karpathy's 'Think Before Coding' Skills File]] is the case this page warns against: a single skills file gaining hundreds of GitHub stars a day and being ported into other editors, adopted by people who didn't write it. Akita's test for it would be whether you read it, adapted it to your stack and merged it as your own, as he did with Humanizer. The two pages agree that a skill is just a well-written prompt, and disagree about whether a good one can be shared.

[[The Case for Markdown Skill Files Instead of MCP Servers]] makes the architectural argument for skills; this page adds a practitioner's limits on it. Progressive disclosure keeps unused skills cheap in context but not in the agent's attention, and his split between procedures (skills) and knowledge (`AGENTS.md`, memory pages, living examples) is narrower than that page's "know vs. do" split. For Akita most of the "know" side shouldn't be a skill at all.

[[The LLM Wiki Pattern]] is where ai-memory comes from: its own research notes name Karpathy's wiki as the project's origin. Akita runs one such Markdown wiki per code project, kept by the agent across sessions, the same pattern this vault uses for reading.

[[Communication Discipline Beats Prompt Frameworks]] is the same author on the same foundation: "skills are just prompts" only works if the prompts are good. That page is about writing the request well in the moment; this one is about how the requests worth repeating get saved as skills, each rule added after a failure.

## Sources
- [Akita - Talking a Bit About My AI Skills](<../../source/Akita - Talking a Bit About My AI Skills.md>)

#skills #developer-tools #security #agent-memory #claude-code
