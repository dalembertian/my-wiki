# A Minimal Coding Agent Harness in Python

A two-part code walkthrough (Google Chat-generated) that makes the "Agent = Model + Harness" split concrete by building the same harness twice — once around a scripted fake model, once around the real Claude API — and showing that only the "brain" changes.

## Part 1: the mock harness

A `MockCodingAgent` returns a scripted sequence of JSON actions rather than calling any real model: (1) write a `divide()` function with an obvious bug (no zero-check), (2) run a test expected to crash with `ZeroDivisionError`, (3) read the resulting error and patch the file to handle it, (4) declare the task done. A separate `CodingAgentHarness` class is the actual scaffolding: it runs the loop (ask the agent for the next action → execute it → feed the result back into history), and enforces safety independent of what the "agent" asks for — `_safe_write_file` resolves the target path and rejects anything that escapes the sandboxed workspace directory (blocking path-traversal attempts like `../../etc/passwd`), and `_safe_execute_command` rejects a denylist of dangerous keywords (`rm -rf`, `sudo`, `shutdown`, `mkfs`) and enforces a hard timeout so a hung command can't stall the loop.

## Part 2: swap in the real model

The "not-so-mock" version replaces `MockCodingAgent` with a `LiveCodingAgent` that calls the real Anthropic API (`claude-3-5-sonnet`, temperature 0 for deterministic coding output), constrained by a system prompt to respond only with a JSON object matching the same `{thought, tool, params}` schema the mock used. The harness class — sandboxing, command denylist, timeout, feedback loop — is **reused essentially unchanged**. Only the piece that decides what to do next changed; the scaffolding that keeps it safe and gives it feedback stayed identical.

## Why this is worth having as its own page

It's a minimal, readable, working illustration of the exact "harness" concept discussed more abstractly elsewhere in this wiki: the security guardrails here are a concrete instance of a **maintainability/behaviour harness** (see [[Harness Engineering - Guides, Sensors, and Regulation Categories]]), and the mock-vs-live swap is a clean demonstration of why the harness/model boundary matters — you can build, test, and safety-check your scaffolding entirely before ever spending a token on a real model call.

## Cross-reference

Companion conceptual pieces: [[Agent Harnesses]] (the market/pricing side of this same concept) and [[Harness Engineering - Guides, Sensors, and Regulation Categories]] (the formal engineering discipline this code is a small instance of).


[[Defending Against Destructive AI Agents]] is the necessary counterpart to a harness this permissive — what to put in place before letting a loop like this touch real files. [[AI Agent Learning Roadmap and Resources]] situates this code in a broader learning path.


[[Six Python Agent Frameworks Compared]] is the buy-side of what this page builds: the same agent implemented in six off-the-shelf frameworks, in ~95 to ~340 lines, which is the yardstick for judging when writing the loop yourself is still worth it.


[[The Harness Is the Product]] is the synthesis this code is the smallest concrete instance of.

## Sources
- [A Mock Harness](<../../source/A Mock Harness.md>)
- [A Not-So-Mock Harness](<../../source/A Not-So-Mock Harness.md>)

#ai-techniques #agents #harness-engineering #python
