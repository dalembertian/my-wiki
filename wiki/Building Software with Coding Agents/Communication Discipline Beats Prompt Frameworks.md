# Communication Discipline Beats Prompt Frameworks

Fabio Akita's answer (April 2026) to the endless "Claude blew it / GPT is better" threads: the complaint is almost never about the model, it is about the request. His claim from 500+ hours in Claude Code and Codex — roughly two and a half months at 16 hours a day, ~400,000 effective lines of code — is that he has never seen either one wander off, invent work he didn't ask for, or fail silently; when a task was impossible under his constraints, the model said so upfront. So his first question to anyone reporting a failure is always *what exactly did you ask for?*

## The fake problem, and the real one

The ecosystem's response to "the model wanders off" has been to add layers: Spec Driven Development, fifteen-section prompt templates, frameworks that force the model to interrogate you before starting. Akita reads these as treating the symptom. He practises what he calls **Agile Vibe Coding** — XP habits (pair programming, tests, short feedback loops, continuous refactor) laid over ordinary prompting — and says he needs no framework, only the things team software work always needed: know what you want, know what you don't want, know how to validate it when it lands.

The real problem he names is older than LLMs, and he links back to his own 2013 post on programmers as bad communicators. You carry a pile of context in your head — stack constraints, past failures, a decision made in a meeting two months ago — then fire off a request as if all of it were also in the listener's head. "Do what I'm telling you" actually means "do what I'm thinking," and the two are not the same. His managerial corollary: if communication is bad at volume 1 it stays bad at volume 5. More meetings, longer templates and bigger specs raise volume, not quality.

## The four blocks

The structural core of the piece. A good request communicates four things, not one:

1. **What I want** — the end goal in plain language.
2. **How I want it done** — broad strokes only, leaving room for the model to propose something better.
3. **What I don't want** — the part almost everyone skips, and where the unspoken assumptions that later become bugs actually live.
4. **How we validate it landed** — the expected result, the test, the "done" signal.

The worked example is a 12 TB, 400,000-file ROM collection on a NAS being consolidated into one standardised tree. The *don't* block is all domain knowledge that exists nowhere in the code: never dedup by filename, only by sha1 + size; a Neo Geo romset differs by which emulator consumes it, so the MAME zip, the FBNeo bundle and the Darksoft cart are three incompatible canonical copies; the MAME NAOMI romset is not the GDI; Saturn regions are not duplicates. The *method* block aligns the way he works rather than micromanaging: a `docs/` living knowledge base, numbered idempotent scripts, progress state in a SQLite catalog instead of in-memory variables, so a crash eight hours in doesn't cost the hashing. Then the safety rails, added after seeing the model's plan: zero deletions in the source trees, only failed-extraction temp files may be removed, and the phase that moves files refuses to start until a manually created `docs/.phase4-approved` flag exists — a human gate between planning and applying, the equivalent of committing before a big refactor.

Block four is the one he says clients have failed at for twenty years: it is easy to want something and hard to say how you would measure that it showed up. **Without a success metric, expectations break by definition**, because there was no concrete expectation to begin with.

## Then stop prescribing and start asking

There is a deliberate shift after the opening turns. Front-load goal, constraints, method and validation; once that ground is solid, switch from "implement it this way with that library" to "given everything we've covered, what's the best approach here — research if you need to, compare the options, come back with the one you'd pick." The model has read far more than you have; prescribing line by line throws that edge away. The dependency runs one way: **asking well requires having set the context well first**, since a dry question with no ground returns a generic answer, while a question resting on established context returns real alternatives with trade-offs.

## The prompt is a starting point, not a contract

He stresses that none of this is a document. This very article was written by Claude from one prompt — but that prompt was followed by corrections mid-flight ("we also need to cover X"; "that reference is out of date, fix it"; "go read the real ROM docs and sharpen that example"), a tone pass, and an interruption seconds before the commit to rewrite an anglicism. He also stays in the room during long jobs: noticing a hash ETA longer than the problem warranted, he interrupted to push concurrency up against 10GbE and NFS, with a check that SQLite transaction ordering still held. *"Nobody sits down with a colleague, drops a 15-line task, stands up, and walks away expecting magic."*

The effort is scaled to the stakes: for a weekend toy he cuts way back and accepts that expectations may break, because the cost of a bug is cheap.

## No future model will infer your context

The anticipated objection — shouldn't the AI figure this out itself? — gets a flat no. If the information is not in the code, the docs or the prompt, it does not exist for the model. Emulator-specific romsets, Saturn regions, a NAS with 10GbE, the decision to keep state in SQLite: domain knowledge, environment context and engineering choices respectively, none of them discoverable by osmosis. The rule he draws: **the quality of what you get back is proportional to the effort you put into asking.** Think of it as outsourcing, not magic — a magician solves it without being told, a contractor delivers exactly what was specified with the information provided.

Hence the labour claim: agents won't replace good professionals, they'll replace people who can't frame a question, don't know what they want, and can't validate a result. The closing metaphors are Stark and Jarvis (Jarvis executes, Stark thinks — and even Stark needed 85 iterations to get the suit right, so the AI speeds up each lap rather than erasing the laps) and the number 42, his company's namesake: a technically correct answer to a question nobody bothered to formulate.

## The aside: Claude Code over Codex, on harness not model

Stated as of April 2026 and explicitly expected to age. Claude Opus and GPT-5.4 xHigh he rates as tied models — when one can't do a hard task he swaps and the other usually can. What separates them is the harness:

- **Planning** — Claude Code decomposes a long task into a visible to-do list, parallelises where it can, and doesn't drop items, so "done" is checkable rather than a claim to chase item by item.
- **Parallel execution** — hitting `ESC` mid-task in Claude Code typically leaves the first task running and starts the second alongside it unless cancelling is genuinely required; Codex stops the first to handle the second and often can't resume it without prodding. So Codex forces a serial, one-request-at-a-time rhythm where interrupting is expensive.

Codex is still rated good, and good specifically at unjamming tasks Claude Code gets stuck on.

## Caveats

This is one practitioner's experience argued forcefully, not a study: "never once" seen a model wander off across 500+ hours is a strong claim resting on his own recall, and the survivorship problem is obvious — someone who front-loads context this well may simply not generate the failures others report. The article was itself written by Claude to his prompt, which is offered as the demonstration and also means the prose is the method's own advertisement. The harness comparison is dated by the author and pinned to specific model versions.

## Cross-reference

[[Structured Prompt-Driven Development (SPDD)]] is the position this page argues against by name — the fifteen-section templates and spec-anchored frameworks Akita calls treating the symptom. The disagreement is narrower than it looks: both demand the same four blocks (SPDD's Requirements/Approach/Safeguards and its "what is done?" test map almost one-to-one onto want / how / don't-want / validate). What they dispute is whether that discipline should be a governed, version-controlled artifact a team can review, or personal communication skill exercised live in the conversation. Akita's case for the second is that quality can't be raised by volume; SPDD's case for the first is that individual discipline doesn't survive contact with a delivery org.

[[Comparing Coding Agent Harnesses - Pi, Oh-My-Pi, OpenCode and Claude Code]] is the same author a month later, and the harness aside here is its thesis in compressed form: the model is not the variable, the CLI around it is. That page supplies the evidence this one only asserts — the same GPT 5.5 finding different defects under two harnesses — and it also complicates Akita's confidence here, since its PR-review test found that *no* harness caught everything, however well the question was framed.

[[Loop Engineering]] is the opposite operating mode, and the contrast is the useful part: there the goal is designing a system that prompts the agent unattended on a schedule, while Akita's whole method is refusing to leave the room — status checks, mid-run redirection, a manual approval gate between planning and applying. Both arrive at the same place from different directions, though: that page's "cognitive surrender" is exactly the failure this one predicts for anyone who asks for little and expects a lot.

[[The Last Solo Programmers]] worries that the prompt-only specialist will lack the skill to catch the AI's mismatches; this page answers from the craftsman side of that split, insisting the skill that matters — knowing what to ask for, what not to accept, and how to measure — stays entirely with the human and is what makes the tool pay. Read together they disagree about who is at risk: Baquero fears erosion in people who delegate broadly, Akita says those people were always replaceable and the tool merely made the replacement cheap.

[[Defending Against Destructive AI Agents]] is what Akita does about the distrust he keeps even with good prompting. It opens with this page's diagnosis (agent disasters usually come from vague instructions) and then refuses to rely on it: a sandbox, copy-on-write snapshots and offsite backups, so that one bad command can't cause permanent damage. Read alongside this page, it shows that his confidence in the model comes with a recovery plan.

## Sources
- [Akita - Why LLMs Aren't Giving You the Result You Expect  & Why I Prefer Claude Code Today](<../../source/Akita - Why LLMs Aren't Giving You the Result You Expect  & Why I Prefer Claude Code Today.md>)

#prompt-engineering #developer-tools #claude-code #agents #vibe-coding
