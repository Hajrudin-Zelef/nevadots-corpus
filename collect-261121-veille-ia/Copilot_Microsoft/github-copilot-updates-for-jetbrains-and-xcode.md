---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-updates-for-jetbrains-and-xcode
title: "GitHub Copilot updates for JetBrains and Xcode"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "github", "jetbrains", "xcode", "review", "assistant", "same", "every"]
source: http://dev.to/kami_butt_69b922ade8de012/github-copilot-updates-for-jetbrains-and-xcode-1h1d
source_anchor: ""
source_lines: [1, 31]
section: "Copilot_Microsoft"
---
GitHub just pushed Copilot into JetBrains Air and Xcode's built-in coding assistant in the same week, and the pitch is the same one every vendor makes: the assistant is now where you already work. That's true, and it's the least interesting part of the announcement. The real story is what GitHub quietly bundled alongside it — a pile of controls for watching agents, reviewing their work, and merging it without babysitting every step. Copilot isn't just spreading to more editors. It's admitting that the bottleneck was never access. It's oversight.

- 
**Copilot lands in JetBrains Air with multiple-session tracking.** You can run tasks locally in your IDE or in the cloud through Air Teams, on your existing Copilot subscription. The useful part isn't the new entry point — it's that GitHub is finally treating "several agents running at once" as a normal state that needs a dashboard, not an edge case.
- 
**Don't over-read it, though.** The announcement says nothing about resource limits, performance, or how work moves between local and cloud. And tracking sessions is not the same as coordinating them — nothing here says agents share context, split a project, or resolve conflicting changes. Visibility is not orchestration, no matter how much the marketing implies otherwise.
- 
**The subscription line is narrower than it sounds.** "Use your existing Copilot subscription" is not a promise of unlimited cloud execution or universal entitlement. Plan eligibility and Air Teams conditions are unstated. Check before you assume.

- 
**Copilot is inside Xcode's built-in coding assistant, in public preview from Xcode 27.3 beta.** For Apple developers this is genuinely convenient — no extension, no sidecar, the assistant lives where the code does.
- 
**But preview means preview.** No general-availability date, no support commitment, no list of limitations. And don't assume the JetBrains session-management features carry over — the announcement documents them for JetBrains only. Each integration is its own deal, with its own gaps.

- 
**Agent Merge now stays enabled while required checks are pending.** Set it up from a pull request and Copilot merges when it's ready. This is not a bypass — required checks still have to pass. The change is that the automation stops switching itself off during the wait, which is the kind of small friction fix that actually matters in a real review queue.
- 
**Reviewer status, extra review requests, and quoting selected comment text to discuss with Copilot** all land in the app. Discussing a comment is not the same as auto-implementing the reviewer's request, and GitHub doesn't claim otherwise. Good — the line between "help me think about this review" and "just do what the reviewer said" is exactly where agents should stay supervised.
- 
**Session housekeeping gets serious:** archive pinned sessions whose PRs merged or closed, pin or unpin in bulk, split nested chats without losing the thread, and more than ten saved custom views in My Work. Boring features. Also the ones that decide whether agent-assisted review scales or collapses into tab chaos.
- 
**Context-limit compaction arrives with honest limits.** When a conversation hits its context limit you can compact it or inspect what's eating the space. It doesn't remove the limit, and compaction doesn't guarantee every detail survives. A documented escape hatch, not a fix.

- 
**Personal instructions for assisted approval are opt-in and local-only.** You can tell Copilot which tool requests to approve without asking — but only in local sessions. GitHub drew the boundary itself: convenience where you can see it, explicit approval everywhere else. That's the right instinct, and it's worth noting precisely because so many agent vendors are racing to erase that boundary instead.

The full breakdown — JetBrains Air session tracking, the Xcode preview conditions, and every Copilot app review control — is on Agentic Affairs: GitHub Copilot updates for JetBrains and Xcode

Originally published on Agentic Affairs (https://agentic-affairs.com)

## Top comments (0)
