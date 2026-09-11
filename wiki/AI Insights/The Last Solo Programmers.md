# The Last Solo Programmers

Carlos Baquero (ACM) traces a lineage of programming-skill erosion that predates AI and argues AI-assisted coding is the latest, largest step in a long pattern — not a unique rupture.

## The lineage

Comptometer operators (skilled mechanical-calculator users, 20,000+ trained per year in the 1920s) were a real profession whose skill vanished once better input methods (barcodes, RFID) made it unnecessary. Programming has followed a similar, gradual drift from first-principles understanding toward automation-assisted output:

1. **Manual typing in plain editors** (VI, EMACS) forced frugality — you'd simplify `int main(int argc, char *argv[])` to `int main()` if you didn't need the arguments, because retyping boilerplate is tedious.
2. **IDEs** (1990s) auto-generated scaffolding, simplifying development but introducing code programmers didn't necessarily understand.
3. **Stack Overflow** (2000s) enabled copy-paste solutions some programmers used without understanding — e.g. migrating `printf()` to `std::cout` without grasping I/O streams.
4. **AI code assistants / "vibe coding"** (Karpathy's term, post-2020): describing a problem in natural language and letting AI generate the full solution.

Each step widens the gap between "programming from understanding" and "programming from automation," and the author argues AI assistants accelerate this faster than any prior step — with a specific new risk: someone who specializes in prompting *instead of* programming may lack the skill to catch the AI's mismatches, producing "Stack Overflow copy-paste on steroids, now for whole projects."

## Craftsmen vs. Vibe Coders

The author frames a growing split: **Craftsmen** use AI selectively, for tasks that don't hinder their own planning and understanding, with careful review — like a carpenter using power tools to cut precisely pre-planned pieces, not asking the tool to design the table. **Vibe Coders** delegate broadly, which risks never developing the underlying skill at all — already observed in some undergraduate courses where students "deceived their way in without any proficiency in writing or programming." The stakes framed starkly: someone who becomes purely an "AI supervisor" providing minimal added value risks becoming "a mere cog in a machine, to be supervised by an AI" themselves.

The author is careful to distinguish this from simple technological decline — a Formula 1 driver isn't less skilled than a jockey, just differently equipped — but insists programming specifically still requires training and experience that pure delegation skips.

## A meta-note from the author

The essay itself was "planned and written by hand," then proofread and improved in places by AI writing tools, and reviewed via a prompt asking Perplexity.ai to flag missing points — a small real-world demonstration of the "craftsman" mode of AI use the piece advocates for (targeted assistance, human-retained judgment) rather than the "vibe coding" mode it warns against.

## Cross-reference

Directly complements [[Prompt Engineering - Is It a New Programming Language]] (same "is this a durable skill or an erosion of one" tension) and [[When ChatGPT Broke an Entire Field - An Oral History]] (a parallel account of skill/field disruption, in NLP research rather than software engineering).


[[Is the AI Boom Real]] uses this page as a ground-truth check against the claim that AI's labor effects are pure hype.

## Sources
- [The Last Solo Programmers](<../../source/The Last Solo Programmers.md>)

#ai-insights #vibe-coding #developer-skills
