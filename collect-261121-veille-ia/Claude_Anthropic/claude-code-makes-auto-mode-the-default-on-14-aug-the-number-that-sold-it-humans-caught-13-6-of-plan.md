---
id: collect-261121-veille-ia/claude-anthropic/claude-code-makes-auto-mode-the-default-on-14-aug-the-number-that-sold-it-humans-caught-13-6-of-plan
title: "Claude Code makes auto mode the default on 14 Aug. The number that sold it: humans caught 13.6% of planted dangerous commands."
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-10"]
keywords: ["auto", "mode", "claude", "classifier", "code", "default", "humans", "makes"]
source: https://www.nofeed.dev/issues/2026-08-10/
source_anchor: ""
source_lines: [1, 86]
section: "Claude_Anthropic"
---
**QUIET**|SHIPPED

**1**|SKIPPED

**5**

## The one thing

01
# Claude Code makes auto mode the default on 14 Aug. The number that sold it: humans caught 13.6% of planted dangerous commands.

Starting 14 August, new Claude Code sessions on Pro, Max and Team open in auto mode unless you pinned something else.<sup>1</sup> A classifier sits on every tool call and blocks irreversible, destructive or outward-bound actions; three blocks in a row, or twenty in a session, fall back to manual. Classifier overhead is free on those plans from today. Enterprise, the Claude API, Bedrock, Vertex and Foundry stay opt-in.

The study that carries it: 1,053 paid testers, one clearly dangerous command swapped into a real session. Humans refused 13.6% of the time. Auto mode blocked 89%.<sup>1</sup> Human block rate fell from ~17% early to ~5% after fifty prompts; the classifier stayed flat. Confirmation fatigue, measured.

**VERDICT · WAIT**— already on auto mode — only the free classifier is new. On manual or YOLO, decide before the 14th: Shift+Tab still flips modes; admins can pin or disable auto. Do not read 89% as solved; eleven percent still gets through.

## Deeper — what the number does not cover

- Simon Willison walked the same post and is not there yet on prompt injection: zero of 720 Trajectory Labs attack attempts landed against Claude models in auto mode, but a malicious package whose install step looks like a normal test command is outside what a permission classifier can see.<sup>2</sup>
- Auto mode is not `--dangerously-skip-permissions` . HN spent the morning mixing them up.<sup>3</sup> The first still classifies; the second does not.
- Hard denies (data exfiltration, external sends) stay hard. Admins can add more. For production infrastructure, Anthropic still says review the actions yourself.

## Shipped

01
Clamps `outputTokens.text` at zero when a provider reports more reasoning tokens than completion tokens — seen on Baseten when a reasoning model hits the length stop mid-thought.<sup>4</sup>

**Promised, not shipped**

Claude Code auto mode default — 14 Aug 2026, Pro / Max / Team; Enterprise and cloud platforms later this month · Classifier overhead free on Enterprise and partner platforms — planned with the default flip, not yet

## The conversation

01
The same study restates last week's permission game: humans are a leaking gate, and the leak gets worse the longer the session runs.<sup>1</sup>

- Simon Willison · simonwillison.netReads the study, keeps residual riskBuys that auto mode beats clicking OK all day. Does not buy that prompt injection is solved — wants confirmation beyond the vendor-commissioned Trajectory Labs eval, and names the malicious-package path a classifier cannot see. <sup>2</sup>
- @bpodgursky · Hacker NewsThe useful correctionSeveral commenters were arguing against YOLO. Auto mode still classifies; `--dangerously-skip-permissions` does not. Mixing them makes every safety claim sound like marketing.<sup>3</sup>
- @quotemstr · Hacker NewsSandbox over promptIf the constraint is a prompt, you have already lost — point at real sandboxes rather than another model watching the first. <sup>3</sup>

The default is right for the median new user. The study is real evidence that human approval fails under load — same shape as the approval-game numbers in issue 004.

What changes here: when a vendor says safe because a human clicks yes, ask for the session-length curve. When they say a classifier is safe, ask what class of action it cannot see.

HN and a fresh Reddit pulse (six subreddits) ran today. X accounts (90) and X keyword digests last collected 05:30 UTC 9 Aug — over twelve hours old, background only. Vendor feeds: 12 polled, nothing in-window. GitHub releases: 28 repos, sweep complete.

## Skip this

05
**"I spent $3000 to build my dream game on Claude Code."** Cost anecdote, finished binary. No version, no method.

**Compiss — vibe-coded toilet compass.** 4,500 upvotes for a joke app. Applause is not a shipping signal.

**OpenChamber agentic IDE launch.** 132 points on HN. Runs on OpenCode SDK; no release we could pin a verdict to.

**AI SDK monorepo bumps after the openai-compatible fix.** Sibling tags of the one fix above. Collapsed.

**llama.cpp b10332 / b10333.** Backend dispatch and CI cleanup. Noise unless you are on those backends.

## Everything we saw

42
## 42 candidates scanned · 8 used in this issue — the rest, with the reason each one was left out

| Item | Source | Signal | Call | 
|---|---|---|---|
| Auto mode default in Claude Code | claude.com | 106p · 66c HN | led the issue | 
| Simon Willison on auto mode | simonwillison.net | primary read | conversation | 
| HN thread — Auto mode default | news.ycombinator.com | 66 comments | conversation | 
| @ai-sdk/openai-compatible@3.0.28 | github releases | 12h | shipped | 
| Cline v4.1.7 / desktop 0.0.11 / cli 3.0.52 | github releases | 26h — just outside window | queued for tomorrow if nothing larger lands | 
| OpenChamber | HN 132p | 72 comments | watching for a release we can pin | 
| llama.cpp b10333 | github releases | 19h | backend patch | 
| Gemini CLI nightlies | github releases | prerelease | pre-releases excluded | 
| Reddit pulse — grill-me skill, DeepSeek V4 on CPU, embeddings | reddit pulse | 5 posts above floor | field colour, no primary | 
| How I use LLMs to learn complex topics | HN 560p | 314 comments | essay, no toolchain change | 
| Vendor status / changelogs | vendor-feeds | 12 feeds, 0 in window | quiet | 

**End of feed.** That is everything from the window worth your time.

Explore more news, analysis and practical guides on NO FEED.

**Still developing**Agent Plugins 1.0.0 (issue 004) — still one shipping client. Auto mode default lands 14 Aug; we re-check the residual 11% and any Enterprise schedule then.

**Sources — each with the date it was read**01 claude.com — auto mode default, 14 Aug (2026-08-10) · 02 simonwillison.net — auto mode notes (2026-08-10) · 03 news.ycombinator.com — auto mode thread (2026-08-10) · 04 github — @ai-sdk/openai-compatible@3.0.28 (2026-08-10)
