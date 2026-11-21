---
id: collect-261121-veille-ia/claude-anthropic/ai-and-ml-2026-08-10-claude-code-puts-auto-mode-in-the-drivers-seat-5285326-15d3c0ee
title: "Ai and ml 2026 08 10 claude code puts auto mode in the drivers seat 5285326"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-10"]
keywords: ["claude", "mode", "auto", "code", "anthropic", "classifier", "puts", "seat"]
source: ""
source_anchor: ""
source_lines: [1, 12]
section: "Claude_Anthropic"
---
AI AND ML
Claude Code puts auto mode in the driver's seat
Walk away and hope the classifier catches anything irreversible or destructive
Anthropic is making auto mode the default in Claude Code from August 14, claiming its classifier is "as safe or safer than an average user clicking through prompts."
Users with a different default already set might receive a one-time prompt asking whether they want to switch. It applies to new sessions on Pro, Max, and Team plans. It will remain opt-in for now on Claude Enterprise, the Claude API, Claude Platform on AWS, Amazon Bedrock, Google Cloud's Agent Platform, and Microsoft Foundry. Anthropic plans to make it the default across those services within the coming month.
Anthropic has also stopped charging Pro, Max, and Team users for the extra tokens consumed by the classifier, and plans to do the same on the other platforms.
Auto mode was launched in March as a research preview and became generally available on July 10. It was an alternative to Claude Code's default permissions, in which every file write and bash command required manual approval. This conservative approach meant running a large task and walking away wasn't possible. The alternative was the --dangerously-skip-permissions flag, which, as the name suggests, lets Claude act without those checks and can lead to risky or destructive results.
Auto mode sends each tool call through a classifier designed to block actions that are "irreversible, destructive, or aimed outside your environment." When the classifier blocks something, Claude will try to find a safer way to proceed. If there are three blocks in a row or 20 across a session, Claude Code falls back to manual approvals.
"We spent the last several months testing whether auto mode is as safe or safer than an average user clicking through prompts," Anthropic said. "We ran internal red-teaming, third-party red-teaming and prompt-injection evaluations, a controlled study with 1,053 paid testers, and analysis of real production sessions. On every measure we tested, auto mode matched or outperformed manual review."
In the controlled study, testers caught a deliberately inserted dangerous command just 13.6 percent of the time. Auto mode blocked 89 percent of the same commands. Anthropic also found that Claude Code users approve 97 percent of permission prompts, suggesting the human checkpoint often amounts to little more than muscle memory.
Anthropic produced the usual set of charts showing how wonderful its new feature is compared to the competition, with its auto mode stopping all 720 attack attempts tested, compared to GPT-5.6 Sol running Codex's Auto-review mode, which let 5.83 percent of attacks through.
The company also described three potentially damaging actions that auto mode blocked inside Anthropic. These were an off-network data leak, a destructive mass operation, and a privilege escalation. Anthropic stated: "In each case, Claude either found a safer path on its own or checked in with the user before proceeding." ®
