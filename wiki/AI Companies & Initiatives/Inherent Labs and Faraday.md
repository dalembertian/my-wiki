# Inherent Labs and Faraday

Inherent Labs (London, founded by Google DeepMind alumni including chief scientist Edward Hughes, $50M seed round) built **Faraday**, an AI agent that outperformed Anthropic's Claude Opus 4.8 and OpenAI's GPT-5.5 at independently replicating published scientific paper results — without being told the answer in advance. The notable part: Faraday runs on a comparatively tiny 27-billion-parameter model (Qwen 3.6), not a frontier-scale one.

## The method mattered more than the win, to them

Inherent's stated priority wasn't beating larger labs at this specific task — it was *how* they got there. Rather than training primarily on descriptions of how science is conducted (a common approach), Inherent leans on **reinforcement learning** — rewarding good research outcomes rather than spelling out rules — betting this generalizes better toward their actual long-term goal: an agent that can contribute original scientific discovery, not just verify known results. Their bar for success explicitly included **"research taste"** — judgment about which experiments are worth running and how to design them well — not just replication accuracy.

Notable build choice: Inherent didn't build its own coding tool for Faraday — it uses OpenAI's GPT-5.5 Codex instead, the same way a human scientist leans on existing software rather than reinventing it. Hughes's framing of the target collaborator behavior: an agent that comes back saying "I got curious about this and went and did these experiments — what do you think?", not one that tells the user what they want to hear.

## Context

All ~12 employees work in person in London's King's Cross, a DeepMind-seeded AI hub. Hughes has publicly opposed UK "garden leave" employment restrictions (which don't apply to US researchers) having been affected by them himself before co-founding Inherent. The company plans to grow to ~20-25 staff by year end, and is positioned as an attractive landing spot for DeepMind staff amid reported unease over Demis Hassabis's new role.

## Cross-reference

The DeepMind-alumni/London-hub thread connects directly to [[Demis Hassabis and DeepMind's Path to AGI]]. Faraday's small-model-plus-RL approach is also a useful data point against assuming frontier capability requires frontier model scale.


[[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is a comparable claimed research-automation result from a frontier lab, and a useful reference point for the benchmark disputes around Faraday.

## Sources
- [Inherent, founded by DeepMind alumni, says its AI 'teammate' just outperformed Anthropic and OpenAI at replicating research](<../../source/Inherent, founded by DeepMind alumni, says its AI 'teammate' just outperformed Anthropic and OpenAI at replicating research.md>)

#ai-companies #deepmind #reinforcement-learning #agi
