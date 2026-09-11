# A Document-Grounded Rules Referee via Prompt Engineering

A BoardGameGeek forum post sharing a fully-worked master system prompt that turns a ChatGPT Project into a strict, document-only rules adjudicator for complex board wargames — tested against *Fields of Fire* and *Twilight Struggle*, both dense with cross-referenced exceptions and scenario-specific overrides. The approach: upload every official document (core rulebook, scenario/mission books, expansions, errata, FAQ, designer notes) into a Project, then constrain the model with a hardened prompt — no code, no tooling, no RAG pipeline, purely prompt-level discipline.

## Reusable prompt-engineering patterns

- **Zero external knowledge safeguard**: explicitly forbids training knowledge, genre conventions, or "probably how it works" inference. "Document silence does not equal permission."
- **Explicit source hierarchy** (errata > FAQ > scenario book > expansions > core rulebook > designer notes > player aids > examples) resolves conflicts deterministically, with a required "Rule conflict detected" flag whenever the hierarchy has to be invoked.
- **Forced answer structure**: every answer must give Source (document + section), Quote (verbatim), Answer — and only when needed, a separately labeled "Derived interpretation," so inference is never silently blended with what the text actually says.
- **Sequence-of-play validation**: explicit phase/timing legality checks before answering; the system must ask for clarification rather than assume unstated context (mission type, scenario, phase).
- **A canonical refusal string**, used verbatim whenever the documents don't cover something: *"This is not explicitly specified in the available sources."* No filling gaps with plausible-sounding completions.

## Why it's worth having as its own page

This is a narrow, high-precision harness built entirely out of prompt constraints, for a task where hallucinated confidence is worse than an explicit "I don't know" — a genuinely different construction method than the code-based harnesses elsewhere in this wiki, and a useful existence proof that a sufficiently disciplined prompt can substitute for a lot of scaffolding when the domain is bounded (a fixed set of uploaded documents) and the failure mode (invented rules) is well understood.

## Cross-reference

A concrete, non-coding illustration of the discipline [[Prompt Engineering - Is It a New Programming Language]] debates in the abstract, and a natural complement to the guardrail-design discussion in [[Function Calling and Tool Use in LLMs]] — here the guardrail is entirely prompt-level rather than code-level.


[[RAG vs. Fine-Tuning]] frames the two standard ways to give a model domain knowledge; this page is the third path it doesn't cover — no retrieval pipeline and no retraining, just uploaded documents plus a prompt strict enough to refuse anything they don't say.


The canonical refusal string described here is a direct antidote to the failure mode in [[AI Digital Fossils in LLM Training Data]] — a plausible-sounding completion filling a gap the documents never covered.

## Sources
- [Boardgame Rulebook Master Prompt](<../../source/Boardgame Rulebook Master Prompt.md>)

#prompt-engineering #rag
