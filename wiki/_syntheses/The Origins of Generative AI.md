# The Origins of Generative AI

A hub page for the cluster of pages covering how today's generative AI came to exist — the 2012 AlexNet moment, the three independent bets that made it possible, and the decade from image classification to ChatGPT. Start here, then follow the spoke that matches what you're after; every factual claim below is sourced on the page it links to.

## The shared through-line

The origin story has an unusual shape: **no single breakthrough caused it.** Three bets, each made independently, each widely doubted at the time, had to be in place simultaneously before anything happened:

- **The algorithm** — Geoffrey Hinton's decades-long insistence on neural networks through two winters, and the 1986 backpropagation paper that made deep networks trainable.
- **The compute** — Jensen Huang's 2006 bet that GPUs could serve general scientific computing, and CUDA. Nvidia's stock fell 70% by 2008 and CUDA downloads *declined* for three years running.
- **The data** — Fei-Fei Li's ImageNet, 14M labeled images across ~22,000 categories, which a mentor told her had been taken "way too far."

None of the three was pursuing generative AI. AlexNet in 2012 was the point where all three arrived at once, and the result beat the field by ten percentage points. Everything after is, in a sense, consequence: transformers (2017) generalized the approach past vision and removed the need for task-specific architectures; scale plus self-supervised learning on internet-scale data turned narrow classifiers into models that emit complex outputs; ChatGPT (2022) put that in everyone's hands.

The recurring lesson the sources draw — worth holding onto — is that **the consensus was wrong for a decade at a time, in all three cases.** Today's "scale is the answer" orthodoxy is itself a lesson learned from AlexNet, which is precisely why it deserves suspicion.

## The spokes

**The story of the people** — [[AlexNet and the Three Nonconformists Who Built the Deep Learning Boom]]: Hinton, Huang, and Li as three separately-derided bets converging, and what happened to each of them afterward.

**How it actually works** — [[How Computers Got Good at Recognizing Images]]: neurons, backpropagation, convolutional layers, feature detectors, and why CNNs succeed by brute-force pattern matching rather than any understanding of geometry.

**When things happened** — [[Deep Learning Timeline (1982-2024)]]: the chronology from the Hopfield Network through LeCun's CNNs, ImageNet, AlexNet, the Transformer, ChatGPT and LLaMA, to Hinton and Hopfield's 2024 Nobel.

**What it did to software** — [[From Data-Driven Software to Generative AI]]: the paradigm reading — coded rules give way to learned behavior, and capability tracks along one axis from complex-inputs/simple-outputs to complex-inputs/complex-outputs.

**What the 2022 rupture felt like from inside** — [[When ChatGPT Broke an Entire Field - An Oral History]]: 19 NLP researchers on transformers being dismissed as "just hacks," GPT-3 as career-existential crisis, and ChatGPT landing "like an asteroid."

**Where the compute bet ended up** — [[NVIDIA's Agent Infrastructure Bet]]: Huang, twenty years on from CUDA, now selling "CPU for Agents" — the same parallel-compute wager, one layer up the stack.

**Where the arc is claimed to lead** — [[The AI Revolution - From ANI to Superintelligence]]: the exponential extrapolation past the present, and the ANI→AGI→ASI framing that a lot of current argument implicitly assumes.

## Cross-reference

For the two conclusions most often drawn *from* this history, in tension with each other: [[Free Intelligence and Radical Abundance]] takes the optimistic extrapolation, while [[Cory Doctorow - The Reverse-Centaur Critique of AI]] argues the whole trajectory is mispriced. The largest recent demonstration of what the resulting models can do when properly scaffolded is [[Claude's Computer-Checked Proof of Fermat's Last Theorem]].

[[Is the AI Boom Real]] is the companion synthesis on what this history is claimed to imply next, and [[The Harness Is the Product]] covers where the engineering difficulty moved once the models arrived.

#synthesis #deep-learning #ai-history #computer-vision #llm
