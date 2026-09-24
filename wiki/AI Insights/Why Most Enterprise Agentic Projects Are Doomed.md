# Why Most Enterprise Agentic Projects Are Doomed

An Accenture team's (Jess Grogan-Avignon & Jack Wang) account of a real project: an agentic application built in two weeks, then taking 12 more months to reach production — not because the code was wrong, but because infrastructure, security, the AI gateway team, data governance, and the application team all had to separately sign off before anything shipped. Framed explicitly as an organizational problem, not a technology one: GitHub averaged 275 million commits/week in 2025, on pace for 14 billion for the year, while approval infrastructure was never designed for that throughput.

## Five tensions that predict success or failure, before the project starts

- **Speed** — AI iterates in days; production processes still run on ~6-month cycles. Approval needs to become executable code, not a longer chain of signoff meetings.
- **Value/funding model** — traditional business cases demand committed ROI per project; agentic work is better funded like a VC backs a portfolio, expecting some bets to fail. The right question shifts from "will this project pay back" to "what's the cost of *not* trying this path."
- **Delivery** — treating agents as "features to be delivered" against a milestone plan misfits what they actually are: closer to research. Better run as hypothesis-driven loops than fixed roadmaps.
- **Trust** — success shouldn't be defined by project completion. Trust is earned by graduating through shadow mode → advisory mode → controlled autonomy, with each step gated by actual outcome evidence (an eval suite), not by "the plan says we're done."
- **Moat** — the durable advantage isn't the data already sitting in a company's ERP; it's the living memory a product accumulates from real, current customer signals. Without a deliberate feedback loop collecting that signal, a competitor can copy whatever gets shipped.

## Cross-reference

A real-world, organizational-scale case study of exactly the "loop, not a one-shot deliverable" mindset in [[Loop Engineering]] (applied to teams and approval processes, not just individual engineers). The "moat is the accumulated signal, not the static asset" argument echoes [[Markdown as the New Agent Memory Moat]]'s claim that durable advantage comes from what a team accumulates over time, not any single model or dataset.


[[From Data-Driven Software to Generative AI]] supplies the underlying reason these projects are hard to run like ordinary software: learned behavior has to be validated on unseen data rather than asserted against a specification. [[Claude's Computer-Checked Proof of Fermat's Last Theorem]] is the instructive contrast — a large agentic project that succeeded, and one where an unambiguous ground-truth checker existed, which is precisely what most enterprise deployments lack.


This page supplies the base rate for [[The Harness Is the Product]] — most agentic projects fail — and is a ground-truth check in [[Is the AI Boom Real]] against predictions of imminent displacement.

[[Team Structures for an Agentic World - Pyramid to Hourglass]] turns this page's diagnosis into an org chart. What it calls "Model A" is the setup that stalled this project for twelve months: engineering builds the agent and hands it to a separate team to run and approve. Its answer is small senior pods that own a workflow end to end, sitting on a platform that enforces policy as code. That is this page's "approval as executable code" built into the org chart.

## Sources
- [Most Enterprise Agentic Projects Are Doomed, Here's Why](<../../source/Most Enterprise Agentic Projects Are Doomed, Here's Why.md>)

#ai-insights #enterprise-ai #agents
