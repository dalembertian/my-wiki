# Karpathy's 'Think Before Coding' Skills File

A different Andrej Karpathy artifact than his "LLM Wiki" gist (see [Markdown as the New Agent Memory Moat](<../AI Insights/Markdown as the New Agent Memory Moat.md>)): "andrej-karpathy-skills," a Claude Code plugin that — stripped of its packaging — is a single 65-line CLAUDE.md file laying out four coding principles, the first being *"Think Before Coding."* The blog author tracked it going from 3.5K to 3.9K GitHub stars in a single day.

## Porting it elsewhere

The author doesn't use Claude Code, so he ported the same rules file into a VS Code extension and a Cursor extension. Most of the post is actually about the friction of *publishing* an extension rather than the rules file itself: the VS Code Marketplace requires six months plus one prior published extension before granting "Verified Publisher" status; the Cursor/open-vsx path required chaining an open-vsx account, an Eclipse Foundation account, linking GitHub, signing an Eclipse agreement, and filing a GitHub issue just to claim a namespace.

## Did it actually help?

His honest answer: hard to tell. Given the non-deterministic nature of these models, a single test (a simple refactor) left him unsure whether the guideline changed anything — he found the agent "very reluctant to make changes" but couldn't confidently say the result was better. What he does find striking is the asymmetry it represents: labs spend "millions and millions of dollars" training a model with teams of engineers optimizing output, and a 65-line personal text file including the words "think before coding" apparently moves the needle enough to earn thousands of GitHub stars.

## Cross-reference

A real-world data point for [[The Case for Markdown Skill Files Instead of MCP Servers]] — a minimal skill file achieving outsized reach and influence, for better or for uncertain.


[[Markdown as the New Agent Memory Moat]] places this artifact in the wider industry bet on Markdown as agent substrate — while cautioning that it is a *different* Karpathy artifact from the "LLM Wiki" gist discussed there.

## Sources
- [Andrej Karpathy Skills](<../../source/Andrej Karpathy Skills.md>)

#developer-tools #skills #claude-code
