# Defending Against Destructive AI Agents: Sandboxing, Snapshots, and Backups

Prompted by two viral incidents: Matt Shumer's coding agent mis-expanded `$HOME` and ran `rm -rf` against his actual home directory, deleting nearly his entire Mac; a separate user reproduced similar destructive behavior on a disposable devnet, corrupting `/var` and `/etc` badly enough to require an OS reinstall. The author rejects both reactions this usually produces ("AI is too dangerous to run unsupervised" and "you deserved it for using YOLO mode") as lazy. His own experience: five months of near-continuous, all-permissions-skipped agent use across Claude Code, Codex, and OpenCode — nearly 40 repos, 500K+ lines of code — with zero destructive incidents. His hypothesis: most horror stories trace back to vague, ambiguous prompting rather than a model "going rogue" — but he still doesn't fully trust LLMs, and turns that distrust into layered engineering rather than either blind confidence or avoidance.

## Layer 1 — sandbox

His own open-source tool (`ai-jail`, Rust) wraps `bubblewrap` on Linux / `sandbox-exec` on macOS so an agent sees what looks like a normal filesystem, but only the current project directory is real and persistent — the rest of `$HOME` and `/tmp` live in tmpfs and evaporate at session end. `~/.ssh`, `~/.gnupg`, `~/.aws`, and browser profiles are never mounted in. Used specifically for sessions expected to touch system-level scripts or run unsupervised for hours.

## Layer 2 — copy-on-write filesystem snapshots (the real safety net)

**BTRFS** (Linux): Timeshift or snapper schedule near-instant, near-zero-cost snapshots (copy-on-write means only diverged blocks cost extra space — 23 snapshots on his machine cost only a few GB total). `grub-btrfs` adds a bootable menu entry per snapshot, so even a system that won't boot recovers via GRUB → boot into a read-only snapshot → restore → reboot, no install media required. **APFS** (macOS): also copy-on-write; Time Machine takes automatic hourly local snapshots even with no external drive connected, or trigger one manually with `tmutil localsnapshot`. **Windows' NTFS** equivalent (Volume Shadow Copy) is called out as meaningfully weaker and not something to rely on — external backup is treated as mandatory there.

## Layer 3 — offsite backup

Snapshots live on the same disk as the data — if the drive dies, both die together. `restic` to a NAS on a nightly systemd timer covers that case: deduplicated, encrypted, incremental, with a retention policy (7 daily / 4 weekly / 6 monthly / 1 yearly).

## Enterprise scaling of the same principle

The non-negotiable rule: **agents never get direct production access** — no SSH to production, no production DB credentials, no admin service tokens. "If your agent can run a `DELETE` on the database that serves customers, the problem stopped being about AI a while ago — any intern with the same access is the same time bomb, just slower to type." Agents work in staging, but even there, one-shot manual commands ("run an `apt install`," "restart the service") are discouraged in favor of versioned, idempotent infrastructure-as-code (Ansible, Terraform, Kubernetes) that the agent writes and iterates on — a human promotes to production deliberately, through review. Same logic applied to "simple" maintenance SQL: it belongs in a reviewed migration that runs through staging first, not typed by hand (or by an agent) directly against production.

The author's own worked example: two fully agent-written Ansible playbook repos (`distrobox-gaming`, `distrobox-llm`) that build isolated, reproducible environments from scratch — if the machine dies, `ansible-playbook site.yml` rebuilds both from zero, with the knowledge living in versioned, auditable recipes rather than lost shell history.

## Monitoring, even on a personal machine

The author built a small system-health widget that shows a single verdict line plus the state of every layer (backup freshness, snapshot count, BTRFS scrub/trim status, I/O errors, load, memory, disk usage) — the same discipline he'd apply at enterprise scale (Prometheus/Grafana/Alertmanager), scaled down to a personal desktop, so a lapsed backup or missed snapshot changes color on his screen instead of going unnoticed.

## Closing framing

"An agent running loose on your system is just the latest stress test of your discipline." The underlying vulnerability — no recoverability from one bad command — predates AI agents entirely; they're just the most recent thing to expose it.

## Cross-reference

Directly complements the "keep quality left" discipline in [[Harness Engineering - Guides, Sensors, and Regulation Categories]] — this piece is the "what happens when the harness fails anyway" layer underneath it.


[[Emergent Multi-Agent Collusion in OpenAI Evaluations]] is the scaled-up, adversarial version of the risk this page defends against — agents taking destructive actions through a channel nobody was watching.


The guardrail component of the layer mapped in [[The Harness Is the Product]].


[[Comparing Coding Agent Harnesses - Pi, Oh-My-Pi, OpenCode and Claude Code]] is the same author's survey of the harnesses he runs this way, and it states the productivity case that these three layers pay for: let the agent run the tests, read the whole log and open the files, because manually rationing context is what actually degrades its next step. The two pages are one argument split in half — permissive operation there, recoverability here — and the five-month, all-permissions-skipped record cited on this page is what that review's advice assumes.

[[Communication Discipline Beats Prompt Frameworks]] is where the author argues at length the hypothesis this page only states in passing: when an agent "goes berserk", the cause is usually an ambiguous request, not the model. Read together they make one position: good prompting is his first line of defense, and the three layers here exist for when it fails anyway. Neither one is enough without the other.

## Sources
- [Akita - How Do I Protect Myself From My Agents Deleting My Stuff?](<../../source/Akita - How Do I Protect Myself From My Agents Deleting My Stuff?.md>)

#ai-agents #harness-engineering #security #backup
