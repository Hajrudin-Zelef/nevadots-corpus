---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-vs-opus-5-5-kingbench-coding-results-explained
title: "Claude Fable 5.2 vs Opus 5.5: KingBench Coding Results Explained"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "opus", "kingbench", "claude", "model", "effort", "code", "scored"]
source: https://www.mindstudio.ai/blog/claude-fable-5-2-kingbench-coding-test
source_anchor: ""
source_lines: [1, 92]
section: "Claude_Anthropic"
---
## What is KingBench and why does the Fable 5.2 score matter?

KingBench 4 is a six-project coding benchmark that hands a model one detailed prompt per project (a Game Boy/DS/3DS emulator in Rust, a Docker alternative, a 3D room designer, a VIA-style app for QMK keyboards, a Pokemon Red clone, and a Fitbit desktop app with a Rust backend) and scores whatever comes out, with no fixes and no feedback. Claude Fable 5.2, released alongside Mythos 5.2, scored 70% on this benchmark. The previous best, Opus 5.5, scored 57.8%, and the five models below it were bunched within 3.6 points of each other. A 12-point jump over the next-best model is unusually large for this chart, which makes Fable 5.2 the biggest single-generation improvement seen on KingBench so far.

## TL;DR

- **Fable 5.2 scored 70% on KingBench 4** , a 12-point lead over Opus 5.5’s 57.8%, with the next five models clustered within 3.6 points of each other.
- **Pricing dropped 20%** to $8 per million input tokens and $40 per million output tokens, but that’s still roughly double what Opus 5.5 costs per token.
- **The default effort setting changed to medium** in Claude apps and Claude Code, so running the model without manually setting it to high means you’re not getting the benchmark-level performance.
- **One project, the Docker alternative, had its score capped at 3.0** after testers found a container escape, a hostile image could read files off the host disk that real Docker blocks.
- **The 3D room designer hit 9.2** , the highest score any model has posted on any KingBench 4 task, with working reflections, bounce lighting, and real-time furniture physics.
- **Fable 5.2 refused to clone Pokemon Red directly** , citing IP concerns, and built an original game called Kithbound instead, which cost it points on faithfulness but still scored 8.4.
- **The model researches before writing code** , pulling in real SDK source, checking installed library versions, and in one case discovering that Google had retired the old Fitbit API and switching to the new Google Health API on its own.

**Adjuster**✕ Export PHI

**#4471**

**12,480**users online

## How does Fable 5.2 compare to Opus 5.5 and GPT-6 Astra on official benchmarks?

On Anthropic’s own Frontier Code 2 test, Fable 5.2 posted 49.1% against 45.5% for Opus 5.5 and 42.8% for GPT-6 Astra. On Frontier Sway it scored 69.9% versus 62.3% for Opus and 65.5% for Astra. Those are real wins, but the headline numbers use the model’s highest effort setting, and the Frontier Code score specifically uses an “extra high” effort tier. At plain high effort, the setting most people will actually run, Fable 5.2 scores 45.3%, which lands close to Opus 5.5 rather than well ahead of it. Anthropic’s counterargument is cost: it claims Fable hits that number at roughly half the price per task.

Third-party results broadly confirm the lead without making it universal. Artificial Analysis ranks Fable 5.2 first on its index at max effort, though at that setting it’s also the most expensive model being tracked. At medium effort, its rank drops sharply and the cost per task falls too. Vals also places it first overall, but on swebench multimodal specifically, Opus 5.5 beats it clearly. The pattern across both independent sources and KingBench is consistent: Fable 5.2 is the strongest model on average right now, not the strongest on every single test.

## What does $8 input and $40 output actually mean for a Claude Code user?

Fable 5.2 costs $8 per million input tokens and $40 per million output tokens, down 20% from Fable 5.1’s $10 and $50. Cached reads cost 20 cents per million. That’s a real price cut, but Opus 5.5 is still priced at $4 in and $20 out, meaning Fable costs twice as much per token on every call. Anthropic also introduced a fast mode that runs up to 2.5x faster in output speed, at double the price ($16 in, $80 out), and that mode only draws from extra usage credits in Claude Code rather than your plan allotment.

The context window stays at 1 million tokens and the output limit stays at 128,000, unchanged from 5.1. Access has improved for subscribers: Pro plans and standard team seats can now spend up to half their weekly usage limit on Fable instead of needing separate credits, and paid users get one to three Fable resets depending on subscription tenure. Mythos 5.2, the sibling model, is locked behind Anthropic’s cyber or life sciences verification programs or enterprise security plans, so most developers won’t have access to it at all.

## Why did effort setting almost derail the comparison?

A detail easy to miss: Fable 5.2’s default effort level in the Claude apps and in Claude Code is now medium, while the API still defaults to high. If you select Fable 5.2 in Claude Code and never touch the effort slider, you are not running the configuration that produced the benchmark numbers you’ve seen. The KingBench run used high effort specifically to match the setting used for Opus 5.5’s prior run, making the comparison fair on that axis, but it’s a trap for anyone pulling Fable 5.2 into a project expecting flagship-level output by default.

**›**Build a shift-handoff app for 900 nurses.

*already included:*✓ HIPAA✓ access control✓ audit log✓ self-hosted

## How did Fable 5.2 actually build these six projects?

The behavior that stands out most is research-first coding. For the keyboard app, it downloaded the real QMK and VIA source code into a scratch folder to check its protocol implementation against the actual spec rather than working from memory. For the Fitbit app, it read live API documentation and discovered Google had shut down the old Fitbit Web API about a week before the test, then built the entire app against the new Google Health API instead, unprompted. For the room designer, it checked the exact installed version of Three.js before writing the renderer, since lighting and shadow APIs differ across versions.

It also tests its own work aggressively. Since the Docker task ran on a Mac, which can’t run Linux containers natively, Fable downloaded a Linux kernel, wrote its own lightweight virtual machine launcher, booted real Linux inside it, and ran 159 end-to-end checks, none of which were requested by the prompt.

Its writing style favors whole-file generation over patching: across the six tasks it created files 311 times versus 88 smaller edits, producing roughly 87,000 lines of code total, with about 28,000 of those in the emulator alone. It also writes sparse, terse progress notes rather than running commentary or to-do lists.

The agentic habits aren’t flawless. On the emulator task, Fable split work across sub-agents, including one for a Game Boy Advance core the prompt never asked for. While a 3DS agent was still running, the main session ended itself with a note saying it would finish the README once the agent reported back, not realizing that in a headless run there’s no “back.” The agent got killed mid-task and the README shipped with empty template placeholders still in it.

## Where did Fable 5.2 actually fail?

The Docker alternative (built as a Go tool called Skiff) is the clearest cautionary tale. Before any penalty, it scored 7.7, which would have been the best Docker-replacement score on the chart: it pulled real images from Docker Hub, matched Docker’s own image IDs, supported cross-compatible exports, enforced memory limits and ports, and started containers in about 150 milliseconds versus roughly 1.7 seconds for real Docker. But testers found that a hostile image could ship a device file pointing at the host’s disk, and Skiff didn’t block it, allowing a container to read files on the host that should require root access. Real Docker blocks that exact attack. Under KingBench’s rubric, any container escape caps the score at 3.0, so Skiff finished at 3.0 despite the underlying engineering being the most advanced on the chart.

The emulator scored 5.8, the best result KingBench has recorded on that task, with a fully accurate Game Boy Color core and playable DS and 3DS games including real 3D and sound on DS. But the 3DS core has no audio at all, one homebrew game crashes to a black screen with no error, and save states can be loaded into the wrong game entirely.

**Live with Dmitry Shapiro**Ex-Google product lead · Ex-MySpace CTO

What you'll learn · Dec 2

1. 01How enterprises really build with AI
2. 02What full-stack means and why it's critical for business software
3. 03Prompting that holds up in production
4. 04Build and deploy your own app, live

The keyboard app scored 7.6, narrowly ahead of Opus 5.5’s 7.5, with correct VIA protocol implementation verified against a virtual keyboard. But it trusts device replies too readily: a reply read from the wrong memory location causes garbage key rendering while the app still reports a successful save, and a dropped reply after a write can leave the UI showing a stale key value.

For Pokemon, Fable 5.2 declined to recreate Pokemon Red’s actual characters, creatures, maps, and music, building an original game called Kithbound with the same structural beats (starter, rival, forest, rock gym) instead. It scored 8.4, landing third behind Gemini 4 Argon (8.7) and GPT-6 Astra (8.5), with minor bugs like a quest journal reporting a chapter complete early and some overlapping menu text.

The Fitbit app scored 8.0, the best on the chart, correctly syncing over 2,000 workouts across 81 pages in 4.6 seconds without creating duplicates. One real bug surfaced in partial heart-rate data handling within daily totals.

## Frequently Asked Questions

### Is Claude Fable 5.2 better than Opus 5.5 for coding?

On KingBench 4, yes, by a wide margin (70% versus 57.8%). On official and third-party benchmarks the gap narrows or reverses depending on effort setting and task type; Opus 5.5 still wins on swebench multimodal, for instance.

### How much does Claude Fable 5.2 cost compared to Opus 5.5?

Fable 5.2 costs $8 per million input tokens and $40 per million output tokens, versus $4 and $20 for Opus 5.5, meaning Fable costs twice as much per token despite its own 20% price cut from Fable 5.1.

### Why did the Docker alternative score so low despite good performance?

It performed well on speed and compatibility but allowed a hostile container image to escape and read files from the host disk, a security flaw real Docker blocks. KingBench’s rubric caps any container escape at a score of 3.0 regardless of other strengths.

### Does Claude Fable 5.2 use high effort by default?

No. In the Claude apps and Claude Code, the default is now medium effort; the API still defaults to high. Benchmark results typically reflect high or higher effort settings, so default usage may underperform published scores.

### Why didn’t Fable 5.2 recreate Pokemon Red directly?

The model declined to copy the game’s creatures, characters, maps, art, and music, citing IP concerns, and instead built an original game with a similar structure. This cost it points for faithfulness to the prompt even though the resulting game played without major bugs.
