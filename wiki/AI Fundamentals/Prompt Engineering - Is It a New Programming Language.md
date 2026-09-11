# Prompt Engineering: Is It a New Programming Language?

An Oxford-style debate (Hien Luu, QCon SF) arguing both sides of whether prompt engineering constitutes a new programming language. No resolution is reached by design — it's framed as a genuinely open question.

## For the motion

- **Structure exists**: effective prompts follow a recurring shape — persona/role, task, subject detail, context, constraints, output format — functioning like a syntax even though it's not enforced.
- **Modularity**: prompts can be built as reusable "functions" (fixed structure, swappable input) similar to functions in code.
- **Emerging patterns/best practices** parallel software design patterns: few-shot, chain-of-thought (Google, 2022 — just asking a model to "think step by step" unlocks stepwise reasoning), tree-of-thought, and a Vanderbilt-cataloged set of 16 prompt patterns, including "flipped interaction" (asking the AI to interview *you* to gather requirements before executing an unfamiliar task).
- **Specialized knowledge is genuinely required**: understanding *why* prompts work requires understanding next-token probability distributions and tuning knobs like **temperature** (how evenly probability mass is spread — low temperature = deterministic/"follows the recipe," high = "experimental chef") and **Top-P sampling** (filters the candidate word pool by cumulative probability).
- **Meta-prompting**: using a prompt to generate a better prompt, analogous to 4GL metaprogramming.
- Reasoning-focused models (the debate cites OpenAI's o1, released ~2 months prior) shift the working relationship "from junior engineer needing detailed guidance to principal engineer needing only high-level intent" — prompting style itself is evolving quickly.

## Against the motion

- **No formal grammar**: programming languages have BNF-defined syntax enforced by a compiler; prompts are free-form natural language with no equivalent enforcement — typos don't break execution the way syntax errors do.
- **Ambiguity is structural, not incidental**: natural-language examples ("I saw a man with a telescope"; "the engineers informed the managers that they had failed") show that the same prompt can be genuinely ambiguous in ways code cannot be.
- **Non-determinism**: identical prompts can yield different outputs, unlike a pure function's guaranteed identical output for identical input.
- **Lower barrier ≠ equivalent depth**: prompting doesn't require understanding data structures, algorithms, or abstraction design the way software engineering does — critics argue this makes it fundamentally shallower, not just more accessible.
- **Historical-fad parallel**: compared to past technologies once hyped as universal standards that didn't endure — the implication being prompting techniques may be absorbed into other interfaces rather than persisting as a durable discipline.
- **Self-obsolescence risk**: as models get better at inferring vague intent, the *need* for careful prompt engineering may shrink over time — undermining the case for it as a lasting skill.
- **Traditional programming isn't going away**: performance-critical and high-reliability systems (operating systems, etc.) still require traditional code; prompting augments but doesn't replace that.

## Cross-reference

The "structured prompts as reusable functions" argument here connects to pattern #1 (AI-native Git, prompts as a source-of-truth unit) in [[Nine Emerging Developer Patterns for the AI Era]]. The debate's unresolved tension — is this a durable discipline or a fad — is echoed by [[The Last Solo Programmers]]'s worry about which programming skills survive AI-assisted workflows.


Two concrete counterweights to the abstract debate: [[A Document-Grounded Rules Referee via Prompt Engineering]] shows how far pure prompt-level discipline can go when the domain is bounded — a strong data point for the "this is real engineering" side — while [[Everyday ChatGPT Prompting Tips]] is the everyday end of the same skill, where the techniques stay informal and habitual. [[From Data-Driven Software to Generative AI]] explains why the question arises now: once models take complex inputs to complex outputs, the prompt becomes the primary interface to them.

## Sources
- [Prompt Engineering Is it a New Programming Language?](<../../source/Prompt Engineering Is it a New Programming Language?.md>)

#ai-techniques #prompt-engineering #llm
