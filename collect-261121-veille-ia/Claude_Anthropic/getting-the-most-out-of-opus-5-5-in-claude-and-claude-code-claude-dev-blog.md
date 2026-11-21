---
id: collect-261121-veille-ia/claude-anthropic/getting-the-most-out-of-opus-5-5-in-claude-and-claude-code-claude-dev-blog
title: "Getting the most out of Opus 5.5 in Claude and Claude Code / claude.dev Blog"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "opus", "code", "works", "keep", "long", "line", "matters"]
source: https://claude.dev/blog/getting-the-most-out-of-opus-5-5/
source_anchor: ""
source_lines: [1, 155]
section: "Claude_Anthropic"
---
How to prompt Opus 5.5, steer a long run, and check your results in Claude apps and Claude Code.

Opus 5.5 works well with the way you already use Claude. A few things behave differently, though: it works for longer on its own, it tells you plainly what it did, and it thinks before every reply. This guide covers how to work with Opus 5.5 in Claude apps and Claude Code, including how to prompt the model, steer a long run, and check your results.

**Three things to try in your first session with Opus 5.5**

**What to do.** Give the whole task in one message. Name the finish line, like âthe tests passâ or âevery endpoint is migrated.â Then let it cook.

**Why it matters on Opus 5.5.** Opus 5.5 keeps going on long, multi-part work better than Opus 5 did. Compared to prior Opus models, its biggest gains are on multi-step work, like carrying a change through a large repository until the tests pass. Early testers had it run long coding tasks for hours with little oversight. With a clear finish line, it knows when itâs done.

**How.** In Claude Code, for example:

Migrate the payment endpoints from the old client to the new one.
Done means: every endpoint uses the new client, the old client is deleted, and the test suite passes.
Stop and ask me only if a test fails for a reason you can't explain.

**What to do.** Remove âthink carefully,â âthink step by step,â and similar lines from your prompts and your saved instructions.

**Why it matters on Opus 5.5.** Opus 5.5 always thinks before it replies, and it decides how much. You donât need to ask it to think. In our testing in a chat product, removing a âthink carefullyâ line made replies start sooner, with no clear drop in quality.

**How.** Delete the line. For a quick answer to a simple question, say so: âAnswer directly.â To change how much it thinks in Claude Code, change effort.

**What to do.** If you remember something mid-run, you can type a follow-up while it works.

**Why it matters on Opus 5.5.** Runs are longer now, so a restart costs more.

**How to do it.** In Claude Code, type the message and press Enter while Claude works, for example, âAlso keep the old endpoint names as aliases.â

**What to do.** When you ask for a page, an app, or an artifact, list the design habits you want left out.

**Why it matters on Opus 5.5.** With no design direction, Opus 5.5 falls back on a few default styles. A general instruction like âavoid a generic lookâ mostly swaps one default for another. A list of specific patterns works much better.

**How.** Name the patterns:

Build a personal website with placeholder content.
Don't use a cream or off-white background, italic accent words in headings, numbered "01 / 02 / 03" section labels, monospace labels, or pill-shaped buttons.

Then look at what it chose instead. If you donât like that either, add it to the list and ask again.

**What to do.** Put a short rule in your CLAUDE.md file about when to stop and ask, and when to keep going.

**Why it matters on Opus 5.5.** Opus 5.5 keeps you posted as it works. On a long task, it sometimes stops to report instead of going on: a summary that names the next step without taking it, an offer to continue, or a list of choices that donât block the work. It follows instructions that name these stops. Name the stops you want, too.

**How.** Add this to CLAUDE.md, and edit it to fit your project:

When a step doesn't need my input, keep going. Put status notes in the same message as your next action.
Stop and ask only when you can't continue without me, or before anything destructive: deleting data, force-pushing, or changing anything outside this repository.

If a run stops with âWant me to continue?â reply âcontinue.â If that happens often, the rule above will help.

A rule to keep going means fewer stops, so keep your own check before anything risky or hard to undo. The last line of the rule above does that. Keep permission prompts on for destructive commands too.

For pair programming, you may want the opposite: a one-line plan before it starts and a short recap at the end. Say that in your CLAUDE.md instead. Opus 5.5 follows either one.

**What to do.** For an audit, a migration, or a review across a large codebase, ask Opus 5.5 to split the work across subagents and check each result.

**Why it matters on Opus 5.5.** Early testers had Opus 5.5 coordinate parallel subagents on long audits and migrations, with little oversight.

**How.**

Audit every service in services/ for the retry bug in the linked issue.
Give each service to its own subagent. When a subagent reports back, check its evidence before you accept it.
Finish with one table: service, affected yes or no, and the evidence.

**What to do.** For a run that will take a while, ask Opus 5.5 to keep its task list in a file and update it as it goes. Then read the file, not the scrollback, to see where the run is.

**Why it matters on Opus 5.5.** Runs are longer now. A long run fills the context window, and Claude Code then summarizes older turns. A list in a file survives that, and it shows you at a glance whatâs done and whatâs left.

**How.** âKeep a checklist in TASKS.md. Tick each item when itâs done, and add anything new you find.â

**What to do.** When a long run ends, look first for anything Claude is waiting on you for, like a decision it left open or a change it wants you to approve. Then read the rest of Claudeâs summary.

**Why it matters on Opus 5.5.** Opus 5.5 reports on its work more clearly than Opus 5. Its updates and its final summary say what it did, what it found, and what it needs from you, in plain language.

**How.** To change the summaryâs format, say so in CLAUDE.md, for example, âEnd every run with three headings: Blocked on me, Changed, Found.â

**What to do.** Ask Opus 5.5 to review a diff or a pull request before a person does.

**Why it matters on Opus 5.5.** One early tester said Opus 5.5 at its lowest effort caught more bugs than Opus 5 at high effort, with fewer false alarms. It also explains its changes in plain language, so its pull request descriptions are easier to review.

**How.** Feed this prompt to Claude:

Review the diff on this branch against main.
List only problems you'd block the merge for. For each one, give the file and line, why it's wrong, and how to show it fails.

**What to do.** For research and analysis, ask it to say what it couldnât find or couldnât check.

**Why it matters on Opus 5.5.** âI couldnât find thisâ is worth reading, and asking for it makes it easy to find.

**How.** Add âMark anything you couldnât confirm, and say where you lookedâ to the request. This works in a Claude research report and in Claude Code.

First, check that the model picker says Opus 5.5.

**What to do.** Attach the chart, diagram, screenshot, or slide. Donât retype the numbers.

**Why it matters on Opus 5.5.** Opus 5.5 reads charts, diagrams, and screenshots more accurately than Opus 5, and it needs no extra steps to do it. Itâs also better at meaning that depends on where things are in the image: which boxes an arrow connects, what changed between two versions of a diagram, or when a meeting starts and ends in a calendar screenshot.

**How.** Attach the image and ask a specific question: âWhich of these services call the billing API directly?â

**What to do.** Give it a long plan, report, or deck, and ask it to find mistakes.

**Why it matters on Opus 5.5.** Opus 5.5 pays more attention to detail than prior Opus models. In our testing, it caught a date that fell on the wrong weekday in a long planning thread, and a chart that didnât match the numbers in a deck.

**How.** Submit the prompt: âCheck this deck for anything that contradicts itself: numbers, dates and names. Quote each problem and say where it is.â

**What to do.** When you want a spreadsheet or a document, ask for the file, not an outline.

**Why it matters on Opus 5.5.** The spreadsheets and documents Opus 5.5 makes need less editing than Opus 5âs before you share them.

**How.** âMake this a spreadsheet I can share: one row per vendor, with columns for cost, contract end date and owner.â

**What to do.** If follow-up questions in a long chat feel slow, add an instruction that earlier answers are settled.

**Why it matters on Opus 5.5.** In a long chat, Opus 5.5 sometimes goes back over an earlier answer while it thinks about a short follow-up. That slows the reply.

**How.** Add this to the projectâs instructions:

`Once you have answered something, treat that answer as done. Focus on what I'm asking now, and don't go back over an earlier answer unless I ask about it or point out a problem with it.`
Leave it out of projects for long analysis, where a later step can show a mistake in an earlier one.

Opus 5.5 is the first Opus model to launch with Fable-level bio and cyber safeguards. In Claude apps and Claude Code, most flagged messages move to an older model, and your work goes on there. Finding security vulnerabilities in source code is allowed, and everyday health and educational questions should still work. These safeguards can sometimes flag legitimate work, and weâre tuning them to cut down on incorrect flags. If youâre switched, hereâs what youâll see and what to do.

**What you see.** A notice that starts with âSwitched toâ and the name of an older model. Claude answers on that model, and the chat stays on it.

**What to do.**

The check covers everything in the conversation, including files and search results. So a flag can come from earlier content, not only your last message.

**What you see.** A notice that names the older model. The session continues on that model.

**What to do.** Remove requests to reproduce its internal reasoning in the reply from your prompts and instructions.

**Why it matters on Opus 5.5.** A request to reproduce its internal reasoning in the reply can be declined. Itâs one of the flag categories.

**How.** Ask Claude for what you need instead, for example, âExplain why you chose this approach in three sentences.â

**What to do.** In Claude Code, use fast mode for back-and-forth work, where you read each reply before you send the next message.

**Why it matters on Opus 5.5.** Fast mode is available for Opus 5.5 at launch as a research preview. You get the same model, and the text arrives sooner. It needs extra usage turned on, and it costs more per token than standard mode.

**How.** Type /fast into Claude.

Run through this before your next long task.

**Asking**

**Long runs in Claude Code**

**Checking**

**Flags**

Start building with Opus 5.5!

*With thanks to Molly Vorwerck for reviewing.*
