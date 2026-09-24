# Who Will Check the AI?

[[The Harness Is the Product]] ends on a dividing line. Where a mechanical ground truth exists, agents deliver at remarkable scale. Where it doesn't, the human stays in the loop as the verifier of last resort. This page picks up from there, because seven pages in this wiki are, underneath, about that verifier. Taken together, they suggest the job is a resource that adoption uses up.

## The claim no single page makes

**The ability to verify AI output is built by doing the work that AI now absorbs.** Each page below sees one part of this at one scale. None of them follows it across all four scales, and none weighs the remedies against each other:

| Page | Scale | How the verifier gets weaker | The proposed defense |
|---|---|---|---|
| [[The Last Solo Programmers]] | Individual | A prompt-only specialist never builds the skill to catch the AI's mismatches | Personal craft: use AI selectively, like a carpenter using power tools on pieces already planned by hand |
| [[Loop Engineering]] | Individual | "Comprehension debt" and "cognitive surrender": the faster a loop ships code you didn't write, the less you understand what exists | A maker/checker split, and deliberately reading what the loop produced |
| [[Structured Prompt-Driven Development (SPDD)]] | Team | A single big review gets skimmed or approved by default; generated volume swamps attention | Six narrow checkpoints, with the human kept as gatekeeper by design |
| [[Team Structures for an Agentic World - Pyramid to Hourglass]] | Organization | Cutting juniors leaves no one to become the seniors who verify; "who verifies the AI in 5 years?" | The hourglass: senior pods inside an org that keeps hiring AI-literate juniors |
| [[Cory Doctorow - The Reverse-Centaur Critique of AI]] | Labor market | The surviving worker becomes an "accountability sink", held responsible for errors they can't catch at machine speed | Labor power: sectoral bargaining, not better tooling |

Two further pages mark the edges of the set. [[Communication Discipline Beats Prompt Frameworks]] argues that the loss is not new. [[Mews's AI Agents for Hotel Front Desks]] is a working deployment built so that the human keeps making the decision.

## Where they agree

- **Checking is harder than generating, and it is getting relatively harder.** The AWS deck's "verification tax" is 10× faster generation against 3× harder review. SPDD's authors say reviewers "skim, defer, or approve by default" once a review asks for more attention than they have. Loop Engineering says "done" from a verifier sub-agent is "a claim, not a proof." Three unrelated sources describe the same asymmetry.
- **Where no ground truth exists, verification doesn't automate away.** SPDD concedes that "human judgement is still load-bearing" until automated verification exists for its specs. Loop Engineering puts verification "still on you." Team Structures makes seniors' verification the whole reason its pods work. None of them expects a model to take over the checker role in domains without a checker like Lean.
- **The skill that matters is judgment, and judgment comes from experience.** Baquero's craftsman, Akita's "know how to validate it", the AWS talk's "the need for judgment through experience does not change": all three name the same thing, and all three say it isn't taught by delegating.

## Where they disagree

**Whether anything is being lost.** Baquero sees a new erosion: each tool generation widens the gap between programming from understanding and programming from automation, and AI widens it fastest. Akita reads the same facts the other way. The people agents replace are those who could never frame a question or validate an answer, and "they were always replaceable, only now the replacement is cheap." On his reading there's no shrinking pool of verifiers. There's a pool that was always smaller than headcount suggested. Doctorow shifts the question from skill to blame. His accountability sink can be perfectly skilled and still fail, because nobody catches errors at superhuman speed. The problem is responsibility without capacity, not a lack of competence.

**Where the fix belongs.** The remedies sit at four levels, and each page assumes its own level is the one that matters:
- *Personal discipline* (Baquero, Akita): keep your hands on the work.
- *Process* (SPDD, Loop Engineering): design the checkpoints so review can't collapse into rubber-stamping.
- *Organizational structure* (AWS): keep the junior base so the pipeline survives.
- *Political economy* (Doctorow): verification fails because it's cheaper to let it fail, so the lever is labor law.

Mews is the one page that puts the fix into the product itself. An advisory agent that "advises, explains, and lets staff approve" keeps the human exercising the judgment instead of merely signing off on it.

## The tension the set exposes

Read together, the pages show a trade-off none of them states. **The arrangements that produce the best verification today are the ones that stop producing verifiers for tomorrow.** The AWS talk's inverted-pyramid pod, made of seniors plus agents, is described as "what actually works for delivery", and in the same breath as having "no learning path." Loop Engineering's unattended loops are the most efficient mode on this list and also the one it warns breeds comprehension debt. SPDD and Akita keep the human in the loop at a cost in speed that both acknowledge.

The remedies also work on different clocks. Process fixes (checkpoints, maker/checker splits, approval gates) protect this quarter's reviews. Structural fixes (the hourglass, protected junior hiring) protect verification capacity five to ten years out. No page proposes both, and the market signal the AWS deck reports points the wrong way for the second: junior hiring down 73% in European tech while senior AI roles boom.

## How much to trust this

It's mostly argument rather than measurement. The hard numbers come from one vendor keynote quoting secondary sources: the 17% comprehension drop, the 2.74× vulnerability rate and the junior-hiring collapse. The deskilling concern is otherwise supported by anecdote and analogy (comptometer operators, undergraduate courses). The claim would weaken if automated verification arrived for work without a mechanical checker. SPDD names that as its open problem, and it's exactly the gap [[The Harness Is the Product]] identifies. It would also weaken if junior hiring recovered without any deliberate effort. It would strengthen if the verification failures in [[Is the AI Boom Real]]'s "watch" list began showing up in domains where the experienced reviewers have already retired.

## Related syntheses

[[The Harness Is the Product]] establishes that the human stays as verifier of last resort wherever no mechanical ground truth exists; this page asks whether that human will still be there. [[Is the AI Boom Real]] maps the displacement argument on two axes; this page adds a third consequence, that displacement at the junior end quietly removes the people the other pages rely on to catch errors.

#synthesis #future-of-work #developer-skills #ai-labor
