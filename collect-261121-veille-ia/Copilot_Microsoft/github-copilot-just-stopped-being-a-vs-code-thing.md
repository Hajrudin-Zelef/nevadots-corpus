---
id: collect-261121-veille-ia/copilot-microsoft/github-copilot-just-stopped-being-a-vs-code-thing
title: "GitHub Copilot Just Stopped Being a VS Code Thing"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "xcode", "github", "agent", "jetbrains", "extension", "code", "just"]
source: https://dev.to/paullovelace/github-copilot-just-stopped-being-a-vs-code-thing-bn2
source_anchor: ""
source_lines: [1, 57]
section: "Copilot_Microsoft"
---
Two days, two IDEs that don't belong to Microsoft. On 3 November 2026 GitHub announced Copilot as a selectable agent in JetBrains Air, and on 4 November it announced Copilot inside Xcode's own coding assistant. I've spent years thinking of Copilot as a VS Code thing with a few extensions bolted on elsewhere. That framing just broke.

This isn't a new model or a pricing change. It's a distribution change, and distribution changes are the ones that quietly rearrange your daily workflow. If you write Swift in Xcode or live in a JetBrains IDE, your editor now has a first-class Copilot option that didn't exist last week. Here's what actually shipped, what it costs you, and the fine print I'd want someone to tell me before I switched anything on.

*Honesty note up front: I haven't tested either integration hands-on — this is verified against GitHub's own Changelog entries (3–4 November 2026), the Microsoft Java developer blog, and 9to5Mac's coverage of the Xcode 27.3 beta. Where the announcements give no number or date, I say so.*

The Xcode news is the bigger surprise of the two. GitHub Copilot is now available in Xcode's coding assistant, in public preview starting with Xcode 27.3 beta. You turn it on from Xcode > Settings > Intelligence — no extension install, no separate download, it's part of the editor now.

A few details that matter if you're deciding whether to try it this week:

- 
**You need macOS Tahoe 26.6 or later.** If you're holding back on the OS upgrade, this alone is your blocker.
- 
**All Copilot plans are supported** — so this isn't gated behind the most expensive tier.
- 
**Xcode 27.3 beta 1 landed on 3 November** and also includes Swift 6.4. The stable Xcode 27.3 release is expected in December, per 9to5Mac's reporting.
- 
**The existing GitHub Copilot for Xcode extension still exists as a separate option.** You now have two paths to the same assistant in the same editor, and GitHub hasn't said the extension is going away.

That last point is the interesting one. Two paths means a real choice, and the honest answer is that nobody outside Apple and GitHub knows yet which one will be better maintained a year from now. The built-in route is cleaner — no extension to update, no separate sign-in dance — but the extension has been the working reality for Xcode developers until now, and extensions can sometimes move faster than an OS-tied editor release. My take: try the built-in version, but don't uninstall the extension until you've lived with the native one for a couple of weeks. If the preview has rough edges — and public previews usually do — you want the fallback one click away.

The JetBrains move is different in kind. Copilot is now a selectable agent in JetBrains Air's agent picker, built on the Agent Client Protocol (ACP) — the open protocol that lets an IDE treat an AI agent as a swappable component rather than a built-in feature. That's the part that matters beyond this one announcement: the IDE is becoming an agent picker, and Copilot is just the first big name to slot into it.

It works in two modes. Local runs happen in the IDE, the way you'd expect. Cloud runs go through Air Teams, where each task runs in an isolated Docker container on a fresh virtual machine, traffic goes through a proxy, and no state is carried between tasks. That isolation design is worth noting — it's the shape you'd want if an agent is going to touch your code from a cloud machine.

The billing rule is the one to get right before you click anything: **cloud tasks bill against your Copilot plan, not your JetBrains AI credits.** If you're tracking spend, that's a different bucket than the one JetBrains users usually watch. And in a related change, from JetBrains IDEs 2026.2.4, Copilot can be selected as the provider for Next Edit Suggestions — the inline, next-edit completions inside the editor. So this isn't only an agent-mode story; Copilot is also competing for the quiet, keystroke-level work.

Both announcements are real, and both have edges the launch posts don't headline. Here they are, all in one place:

- 
**Xcode's Copilot is a public preview in a beta IDE.** Beta means beta: expect rough edges, and don't rebuild your release workflow around it this week.
- 
**In JetBrains Air, you cannot choose a personal versus organization plan per task.** If you juggle a personal Copilot plan and an org one, that's a real friction point — check which plan a task will bill before you run it, not after.
- 
**Air analytics don't include Copilot usage.** If your team tracks AI usage through JetBrains Air's analytics, Copilot work will be invisible there. You'll need GitHub's own usage views to see it.
- 
**Admins can't set Copilot spending limits inside Air.** If you manage a team, that's the caveat to flag to your admin before anyone starts running cloud tasks — cost controls live on the GitHub side, not in Air.
- 
**Stable Xcode 27.3 is expected in December** , not this week — the date comes from 9to5Mac's reporting, not from Apple's or GitHub's announcement.
- 
**No hands-on testing was performed for this article, and no benchmarks or prices have been invented.** Nothing here is a performance claim.

Short answer: try it, don't migrate to it — yet.

**If you're an Xcode developer with a Copilot plan:** install the Xcode 27.3 beta on a machine you can afford to be annoyed by (not your release machine), turn Copilot on in Settings > Intelligence, and use it on a real project for a week. Keep the extension installed as your fallback. If the native assistant feels solid by the time stable 27.3 ships in December, that's your switch point.

**If you're a JetBrains developer:** the agent-picker model is the thing to watch, not just Copilot itself. Pick Copilot in Air for a task you'd normally hand to an agent anyway, watch which plan it bills, and check whether your team's analytics gap matters to you. The Next Edit Suggestions option in 2026.2.4 is the lower-risk first step — it changes your typing, not your workflow.

**If you're on the fence about paying for Copilot at all:** this news doesn't change that decision. It makes an existing subscription more useful if your editor is Xcode or JetBrains; it doesn't make a subscription you didn't want suddenly worth buying. Pick by workflow, not by launch-week excitement — the same rule as ever.

The bigger picture is what I'll be watching: Copilot spent years as the VS Code assistant. In 48 hours it became a native Xcode feature and a swappable agent in a JetBrains IDE. The next question isn't whether more editors follow — it's which agent your IDE lets you pick next, and whether "which AI do I use?" becomes a setting you change per task instead of a subscription you marry. That's a genuinely different world for developers, and it started this week.

Are you trying Copilot in Xcode or Air this week — and if you already have, which path did you pick, built-in or extension? I'd genuinely like to hear how the preview holds up on a real codebase.

*Sources: GitHub Changelog, 4 November 2026 — "GitHub Copilot is now available in Xcode's coding assistant"; GitHub Changelog, 3 November 2026 — "GitHub Copilot is now available in JetBrains Air"; Microsoft Java developer blog — "GitHub Copilot in JetBrains Air"; 9to5Mac, 3 November 2026 — Xcode 27.3 beta. No affiliate links in this article.*

## Top comments (0)
