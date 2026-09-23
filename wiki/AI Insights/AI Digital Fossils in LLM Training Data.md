# AI Digital Fossils in LLM Training Data

"Vegetative electron microscopy" is a nonsense phrase with no real scientific meaning, yet it has appeared in at least 22 published papers and keeps getting produced by major LLMs, including GPT-4o and Claude 3.5. Researchers traced it to two independent errors: a 1950s paper-scanning glitch that merged "vegetative" from one text column with "electron" from an adjacent one, and — decades later, unrelated — a Farsi translation slip where "vegetative" and "scanning" differ by a single dot. Both errors ended up on the web, got scraped into training corpora (most likely via CommonCrawl), and are now baked into model weights.

Testing across model generations found the phrase absent from GPT-2 and BERT but consistently produced by GPT-3 onward — meaning the contamination point can be roughly dated to whatever training data changed between those generations. The researchers call this pattern a **"digital fossil"**: an error that entered the historical record by coincidence and is now effectively permanent, because no one can cleanly scrub a single bad phrase out of a training set that spans millions of gigabytes of scraped text, and no major lab publishes what's actually in that data.

## Why it matters beyond one phrase

- **Self-perpetuation risk.** As AI-assisted writing becomes more common, models that "know" the fossil term can reintroduce it into new documents, which then become future training data — a feedback loop reinforcing the error rather than diluting it.
- **Publisher inconsistency.** When notified, some publishers retracted affected papers; Elsevier initially defended the term's validity before eventually issuing a correction.
- **Open question at scale.** The researchers frame this as almost certainly not an isolated case — the real question is how many other nonsense terms are similarly fossilized and undiscovered, since detection currently relies on catching specific known phrases (e.g. the "Problematic Paper Screener" tool), not finding unknown ones.
- **Adjacent symptom.** Related to (but distinct from) papers turning up with leftover AI-tool artifacts like the literal phrase "I am an AI language model," or "tortured phrases" (e.g. "counterfeit consciousness" for "artificial intelligence") used to dodge plagiarism/AI-detection software.

The core tension: fixing training-data errors requires transparency and scale of access that commercial AI labs don't provide, while the errors themselves quietly become part of the "ground truth" future models and researchers build on.

## Cross-reference

The pathological case of the first consequence listed in [[From Data-Driven Software to Generative AI]] — once behavior is learned from data rather than coded, data quality *is* correctness, and there is no equivalent of a bug fix for a corpus spanning millions of gigabytes. The opacity complaint here is the same one the NLP researchers in [[When ChatGPT Broke an Entire Field - An Oral History]] raised as "API science": nobody outside the labs can audit what the models were trained on.

Sharpest contrast in the wiki: [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is the corpus-integrity problem *solved* — a body of knowledge made machine-checkable so errors cannot silently propagate into what gets built on top — while this page is the same problem left unsolved in the scientific literature, where a nonsense phrase becomes permanent ground truth. Two mitigations at the retrieval end: [[RAG vs. Fine-Tuning]] notes source integrity as RAG's central weakness, and [[A Document-Grounded Rules Referee via Prompt Engineering]] shows the strict form of the fix — a canonical refusal string instead of a plausible-sounding completion.

The inverse case is [[Emergent Agent Dialects and the Oversight Problem]]: this page is about human-origin nonsense becoming permanent model vocabulary through the training corpus, that one about agents minting vocabulary of their own at runtime, with meanings no corpus records. Both make the same point about where meaning is actually fixed — not in anything a reader can inspect.

## Sources
- [A Strange Phrase Keeps Turning Up in Scientific Papers, But Why?](<../../source/A Strange Phrase Keeps Turning Up in Scientific Papers, But Why?.md>)

#ai-training-data #digital-fossils #ai-limitations #data-integrity
