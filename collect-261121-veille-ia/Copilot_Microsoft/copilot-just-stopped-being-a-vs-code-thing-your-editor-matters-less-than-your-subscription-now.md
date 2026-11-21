---
id: collect-261121-veille-ia/copilot-microsoft/copilot-just-stopped-being-a-vs-code-thing-your-editor-matters-less-than-your-subscription-now
title: "Copilot Just Stopped Being a VS Code Thing. Your Editor Matters Less Than Your Subscription Now"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["copilot", "xcode", "jetbrains", "inside", "november", "assistant", "code", "thing"]
source: https://www.devdaynotes.com/code/github-copilot-xcode-jetbrains-air-not-just-vs-code/
source_anchor: ""
source_lines: [1, 57]
section: "Copilot_Microsoft"
---
I read GitHub's weekly Copilot changelog on Friday morning, the way I read most changelogs: coffee, skim the headlines, stop when something contradicts what I thought I knew. Two lines stopped me this week, and they landed 24 hours apart. On November 3, GitHub announced Copilot as a selectable agent in JetBrains Air. On November 4, it announced Copilot inside Xcode's own built-in coding assistant, in public preview starting with Xcode 27.3 beta.

That is the week Copilot stopped being a VS Code thing. Not gradually, not in a roadmap slide — in two shipping announcements, in two editors that do not belong to Microsoft, in 48 hours.

I have written about Copilot a lot this year, and most of it has been about what happens inside VS Code: the Fusion router, the model retirements coming November 16, the mixed-harness sessions in 1.145. All of that assumed the same thing, which is that Copilot lives in VS Code and everything else is a side door. This week breaks that assumption, and the interesting question is not whether the integrations are good yet. It is what happens to the editor decision when the assistant you already pay for follows you into the other editors you actually use.

## What shipped, in the order it shipped

JetBrains Air came first, on November 3. Air is JetBrains' coding-agent environment — an agent picker where you choose which agent runs a task, built on the Agent Client Protocol, the open protocol that lets an IDE treat an AI agent as a swappable component instead of a built-in feature. Copilot now sits in that picker. You can run tasks locally inside a JetBrains IDE, or in the cloud through Air Teams, which runs them in a Docker container on a fresh virtual machine, with traffic routed through a proxy and no state kept between tasks.

The billing rule is the one to get right before you click anything: cloud tasks bill against your Copilot subscription, not against JetBrains AI credits. Air administrators can disable Copilot by governance policy, but they cannot set Copilot spend limits from Air. That is a real gap to know about if you are on a team plan — the off switch exists, the budget dial does not, at least not from the JetBrains side.

There is a second JetBrains detail that matters more for day-to-day use than the cloud sessions. Starting with JetBrains IDEs version 2026.2.4, Copilot can also be selected as the provider for Next Edit Suggestions — the inline, multi-file edit suggestions inside the editor that used to be JetBrains-model-only. That is the part of this announcement that will touch people who never open Air at all.

Xcode came a day later, on November 4, and it is the bigger surprise of the two. GitHub Copilot is now available in Xcode's built-in coding assistant, in public preview starting with Xcode 27.3 beta. You set it up under Xcode, then Settings, then Intelligence. You need macOS Tahoe 26.6 or later, and all Copilot plans are supported — Individual, Business, and Enterprise. Xcode 27.3 beta 1 landed on November 3 and also includes Swift 6.4; the stable Xcode 27.3 release is expected in December, according to 9to5Mac's reporting of the beta, not from Apple's or GitHub's announcement.

One thing to get straight: this is not the extension. The existing GitHub Copilot for Xcode extension has been available for some time and it still exists as a separate option. What changed in 27.3 is that Copilot can now be selected as the intelligence provider inside Xcode's own coding assistant UI. Two paths into Copilot inside the same IDE, one of them now native.

## Why this is different from another integration announcement

Every AI coding tool announces integrations. Most of them are a plugin that renders a chat panel inside someone else's window. This is not that, and the difference is worth naming because it decides whether the integration survives contact with real work.

In Xcode, Copilot is a provider inside Apple's own assistant surface. That means it inherits the assistant's placement, its keyboard shortcuts, its relationship with the code you have open — the things an extension can only approximate by drawing its own panel next to the real one. A provider is a first-class citizen; an extension is a guest. The extension still works, and for people on a stable Xcode release it is the only option until December, but the direction is obvious. Apple let a Microsoft-owned assistant into its native coding surface. That does not happen because of a partnership press release. It happens because Xcode's own assistant needs a model bench deep enough to be useful, and building one from scratch takes longer than opening the door.

In JetBrains Air, the difference runs the other way. Copilot is not embedded in a JetBrains surface at all — it is a swappable agent behind an open protocol. ACP is the bet that the agent layer and the editor layer separate, the way language servers separated from editors a decade ago. If that bet pays off, the question "which editor has the best AI" stops being a question, because the AI is a component you bring with you. Air is the first place that bet is visible in production, and Copilot is the first big agent to sit behind the picker.

Put the two together and the pattern is hard to miss. In the same week VS Code 1.145 made it possible to run Claude, Codex, and Copilot harnesses in one session — the editor becoming an agent control plane — Copilot showed up in two editors that are not VS Code. The editor is becoming a place where agents meet, and the agent is becoming something that is not owned by any one editor. Both directions point the same way: the assistant is unbundling from the window it lives in.

## The money question, because it is the whole decision

Here is the part that decides whether any of this matters to you, and it is good news for exactly one group of people: if you already pay for Copilot Pro, these integrations cost you nothing new. Pro is $10 a month on monthly billing, a reference price that changes, and the Xcode preview works with every plan while the JetBrains Air sessions run on the subscription you already have. No second subscription, no per-IDE seat, no JetBrains AI credits consumed by Copilot's cloud tasks.

That is the trade, stated plainly. The subscription you bought for VS Code now covers Xcode and JetBrains too. If you are the kind of developer who lives in one editor, this changes nothing — you already had Copilot where you work. If you are the kind who moves between editors, which is most iOS developers I know and most Java and Kotlin teams, this collapses a decision you have been deferring. You no longer have to choose between paying for a second assistant in your second editor or going without one there. The assistant you already pay for is now in both places, and the only thing you have to decide is whether the preview quality is good enough to use.

The catch is the one the announcements do not headline. In JetBrains Air, cloud tasks run on a fresh VM with no state kept between tasks, which is the right design for isolation and the wrong design if your workflow assumes the agent remembers what it did last time. And the spend-limit gap is real: an Air administrator can turn Copilot off by policy, but cannot cap what it spends from the JetBrains side. If your team runs Copilot on a shared plan, the budget control lives in GitHub's settings, not in Air's. Know that before someone on your team discovers it by running a cloud session against a large repo.

On the Xcode side the catch is simpler and more familiar: it is a public preview in a beta IDE. Beta means the stable release is expected in December, and preview means the rough edges are still being found. If your iOS work is your income, the extension on stable Xcode is the sane choice until the stable 27.3 ships. If you have a side project in Xcode that can absorb a beta, the preview is worth trying now, because the setup is just a settings pane and the cost of finding out is an afternoon.

## Who should care, and who should wait

I would put it this way. If you write Swift in Xcode and already pay for Copilot, try the preview on a side project this week and keep the extension on your production work until December. You will learn whether the native provider feels different from the extension — and it should, because it lives inside Apple's assistant instead of beside it — without betting your shipping build on a beta.

If you work in JetBrains IDEs, the Next Edit Suggestions change in 2026.2.4 is the thing to try first, before Air. It is the smallest surface, the closest to your normal typing, and the easiest to judge: do the inline suggestions get better with Copilot as the provider, or do you switch back? Air itself is worth a look if your team already runs agents in the cloud, but the no-state-between-tasks design and the missing spend cap mean it is a team decision, not a personal experiment.

If you live in VS Code and never leave it, nothing changed for you this week, and that is the point. The news is not for you. It is for the people whose work spans editors and who have been paying the tax of a second assistant, or going without one, in their second home.

And if you are on a managed machine — organization-managed Copilot settings, enterprise policy — check before you plan anything around either integration. The same week VS Code 1.145 shipped mixed harnesses that managed settings can disable entirely, Air shipped with a policy off-switch and no spend dial. The pattern across all three is that your organization's policy wins over your local toggles, every time.

## The decision underneath the announcements

I keep coming back to one line in the JetBrains announcement: Copilot runs "using your existing Copilot subscription." That is the whole story, in eight words. The editor wars used to be decided by which editor had the best features. Then they were decided by which editor had the best extensions. Now they are being decided by which subscription you already hold, because the assistant follows the subscription, not the editor.

That is a good deal for developers and a complicated one for the editors. VS Code built the best agent control plane this year and then watched its flagship agent walk into Xcode and JetBrains Air. JetBrains built an open protocol for swappable agents and then watched the biggest agent in the market take the first seat. Apple opened its native assistant to a Microsoft product. Everyone gave something up, and the thing they all gave up is the same: the idea that the editor owns the assistant.

For me, the practical conclusion is narrow, and I like it narrow. I am not switching editors because of this, and I am not adding a subscription either. I already pay for Copilot, and now that subscription covers the two other editors I touch in a normal month. The Xcode preview is worth an afternoon on a side project. The JetBrains Next Edit Suggestions change is worth a week of normal typing. Air is worth a team conversation, not a solo experiment. And the real thing to watch is not either integration on its own — it is whether Claude and Codex follow Copilot through the same doors, because the week they do, the editor you choose will be a preference, not a lock-in. That week has not arrived yet. This week just made it plausible.

← Back to all posts
