# When ChatGPT Broke an Entire Field: An Oral History

Quanta Magazine interviewed 19 NLP (natural language processing) researchers about how their field was transformed — and largely absorbed — by the rise of large language models. Structured as three acts plus prologue and epilogue.

## Prologue: before the flood (2017-2019)

Google's 2017 "Attention Is All You Need" paper (the transformer architecture) was initially dismissed by many NLP researchers as "just hacks" — a model deliberately built without linguistic insight that nonetheless worked well. BERT's 2018 release triggered a "BERTology" boom and a benchmark race; one researcher recalls checking incoming leaderboard submissions and noticing results were increasingly just "old or simple ideas scaled up" — an early, easy-to-miss signal that scale, not architecture cleverness, was becoming the main driver of progress.

## Act I: "The Wars of the Roses" (2020-22)

A foundational dispute over whether models trained purely on statistical patterns can ever engage with *meaning* crystallized in the 2020 "octopus test" paper (Bender & Koller): a hyperintelligent octopus that fluently mimics human messages without ever understanding life on land. This "understanding wars" debate turned personal and factional.

**GPT-3** (June 2020) was, for many researchers, the more visceral turning point — described as inducing a "career-existential crisis" as tasks that took PhD students five years suddenly worked "in one shot." The model wasn't publicly released (API access only), which many in the academic NLP community saw as illegitimate — coining the pejorative "API science" for research done on a paid, non-reproducible product.

The 2021 **"On the Dangers of Stochastic Parrots"** paper (Bender, Gebru, and colleagues) escalated the meaning-vs-scale dispute into moral territory, asking what could go wrong as language models scaled indefinitely. The field split into pro- and anti-LLM camps hard enough that a 2022 field-wide self-survey used the phrase "a field in crisis," and one PhD student recalls reading the "30 controversial positions" survey and thinking the respondents "sound like nutcases" — before ChatGPT changed his mind entirely a few months later.

## Act II: "Chixculub" — ChatGPT's impact (Nov 2022 - 2023)

ChatGPT's November 30, 2022 release is described as hitting the field "like an asteroid." Concrete effects reported: entire categories of ongoing research became "no longer interesting — or no longer practical" overnight; Hugging Face reorganized its research team into two tracks (pre-training or post-training) within days, prompting a researcher to describe it as inconsistent with the company's prior open-research culture; a professor was shown a fabricated academic paper written by a student's ChatGPT prompt ("write me a paper in the style of Christiane Fellbaum") convincing enough that she initially believed it was genuine scholarship about her own work.

PhD students describe forming informal support groups, "quiet quitting" research directions that had become intellectually hollow, and a wave of sudden, overwhelming media attention — testifying before Congress, appearing on *60 Minutes*, being asked confident-sounding questions by journalists despite (as one researcher put it) "most of them have no idea what they're talking about." One researcher: "It goes from a relatively sleepy field to, suddenly, I'm having lunch with people who were meeting with the Pope and the President in the same month."

## Act III: "Mutatis Mutandis" (2024-25)

Some researchers describe becoming "LLM-ologists" rather than linguists or computer scientists — studying the behavior of specific proprietary systems rather than building general theory, which one described as "myopic" but justified by there being "not a path forward to understanding language that doesn't have an account of what LLMs are doing." A Global South / Western divide is noted: Indian NLP researchers, for instance, focused pragmatically on culturally-aware translation (a literal English→Hindi translation of "key lime pie" means nothing to most Indian readers) rather than the more philosophical "does it truly understand" debates dominating Western academic discourse.

Yejin Choi describes unusually emotional (rather than methodological) pushback to a 2023 paper showing GPT models fail at multi-digit multiplication — "as if I'd hurt their baby." Financial entanglement is flagged as a new distorting factor: "it's sometimes confusing when we pretend there's a scientific conversation happening, but some of the people in the conversation have a stake in a company potentially worth $50 billion." Ai2 built the fully open-source **OLMo** model specifically as a countermeasure to the opacity of proprietary frontier labs.

## Epilogue: was it actually a paradigm shift?

Opinions split. Some argue the underlying principle (transfer learning from large data) hasn't fundamentally changed since 2013 word-embeddings — only popularity, architecture, and public perception have. Others argue the paradigm shift is real but social/institutional rather than technical: research questions that used to be "central" (e.g. sentiment classification) became "peripheral" almost overnight, media/funding incentives now shape what gets published, and the field's basic question shifted from "how does language work" to "what are LLMs doing."

## Cross-reference

Same "field disrupted overnight, careers had to pivot" pattern as [[The Last Solo Programmers]] (software engineering) and complements [[AlexNet and the Three Nonconformists Who Built the Deep Learning Boom]] as a second first-person account of an AI paradigm shift, this time in language rather than vision. It is the closing act of the origin story collected in [[The Origins of Generative AI]] — the 2022 entry in [[Deep Learning Timeline (1982-2024)]], experienced from inside the field it displaced, and the human cost of the transformer generalization described in [[From Data-Driven Software to Generative AI]].


The "API science" objection recorded here — research conducted on a paid product nobody can inspect or reproduce — has a concrete cost in [[AI Digital Fossils in LLM Training Data]], where a meaningless phrase is now unremovable from model weights and no lab publishes what is in the training data.


[[Is the AI Boom Real]] uses this oral history as the strongest ground-truth evidence against the "it's all hype" reading.

## Sources
- [When ChatGPT Broke an Entire Field An Oral History](<../../source/When ChatGPT Broke an Entire Field An Oral History.md>)

#ai-insights #nlp #llm #ai-history
