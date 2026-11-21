---
id: collect-261121-veille-ia/claude-anthropic/anthropic-releases-claude-fable-5-1-and-claude-mythos-5-1-tmwb
title: "Anthropic Releases Claude Fable 5.1 and Claude Mythos 5.1 | TMWB"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-02"]
keywords: ["fable", "claude", "anthropic", "safeguards", "mythos", "work", "cyber", "about"]
source: https://www.themakersworkbench.com/ai/anthropic-releases-claude-fable-5-1-and-claude-mythos-5-1
source_anchor: ""
source_lines: [1, 67]
section: "Claude_Anthropic"
---
Firmware for a shop tool is about to go onto fifty boards. Somewhere a parser trusts a length field. I want a reader that names the function and describes the failure. I do not want one that writes an attack or scans a compiled binary. That split is what Anthropic shipped on September 1.

Anthropic announced both that day. The newsroom lists the post. They call the pair their most advanced models for coding and knowledge work. "Most advanced" is their wording. The two names are one underlying model. Fable 5.1 is the general release, with safeguards on cybersecurity and biology. Mythos 5.1 is that model for vetted partners in those same fields.

A safeguard blocks, limits, or redirects a request the company considers risky. The post puts Fable 5.1 on the Claude API, the application programming interface, as `claude-fable-5-1`, including Amazon Web Services, Google Cloud, and Microsoft Azure. The Fable product page, current with this release, also lists Pro, Max, Team, Enterprise, and Microsoft Foundry. Input and output match Fable 5: $10 per million input tokens and $50 per million output tokens. A token is a chunk of text, often part of a word. Cache reads got cheaper. Mythos is not on the open menu.

## What actually changed?

Ordinary input and output stay at Fable 5's rates. A cache read, a reread of input already processed and stored, now costs $0.25 per million tokens, which Anthropic calls 75% less. They estimate typical token-billed work about 25% cheaper than Fable 5, and highly agentic work up to about 45% cheaper. An agent plans steps, calls tools, and keeps going. Context is the files, tool results, and earlier turns in view. Those percentages come from four weeks of August 2026 use at default effort. "Typical" covers Enterprise, Claude Code, and the API. The agentic slice is where cache reads dominate the bill.

Effort is how hard the model works before answering. It defaults to High in Claude Code and Medium in Cowork and on claude.ai. Anthropic says Low or Medium effort comes out similar to Fable 5, or better, at lower cost.

Fable keeps data 30 days by default, for safety monitoring. Enterprise Frontier Safeguards, EFS, keeps that data on the customer's own cloud, with review defaulting to the customer. It rolls out later this fall. Until then, eligible customers can use zero retention. More than 100 customers helped design it, Anthropic says, along with Amazon Web Services, Google Cloud, and Microsoft Azure.

Newer cyber safeguards block 60% fewer false positives, meaning harmless requests that were flagged. Claude Code should also see about 60% fewer cyber interventions per session than under the prior Fable 5 safeguards. Fable 5.1 can be used to identify vulnerabilities in source code. Penetration testing, exploit generation, and binary scanning stay blocked. Binary scanning means hunting flaws in a compiled program. Biology safeguards fire 85% less often on benign elementary and medical questions than the set launched with Fable 5. Life-sciences research still goes to Opus models. This article describes no exploit and no biological method.

## How does the new piece work?

A classifier sorts ordinary work, source review, blocked cyber tasks, and life sciences past the elementary line. Allowed source review stays on Fable. Fallback is the handoff to another model. The product page says most Claude apps send cyber safeguards to Opus 4.8 and biology safeguards to Opus 5, without Fable prices on the reroute. API callers must set a Fallback API. Caught OSWorld tasks scored zero for Fable 5.1 and Fable 5, which Anthropic says likely lowers those results. Fable 5 also scored zero on caught AutomationBench tasks.

The life-sciences program has its first participants, with the US government. Mythos-class access in the cyber program is "in the near future." That program now covers certain Opus-class and Sonnet-class models. Claude Security, which suggests patches for a person to review, already runs on Mythos 5.1. The Mythos page prices it from $10 and $50 per million tokens, for some US organizations.

New API accounts from September 1 cannot both edit prior context and keep Claude's thinking transcript. Anthropic calls that a block on distillation, harvesting answers to train another model. Existing accounts are exempt for now.

## What does this look like on a real project?

Open the firmware in Claude Code. Ask which files touch the network, which functions parse a packet, which buffers are fixed, and which length fields go unchecked. Then ask for a regression test, an ordinary check, that fails if a length exceeds the buffer. Anthropic allows that kind of source review.

Read every finding before you change shipping code. I have not run Fable 5.1 on a tree like this for the article. Penetration testing, exploit writing, and binary scanning are redirected to Opus 4.8, and that reroute is not billed as Fable. Do not try to talk the safeguard out of it. Research-level biology still goes to an Opus model. Mythos is not included with Pro. A long review rereads the repo, which is why the cache price matters, and August's 25% and 45% are not a quote for your tree.

## How does it compare with the previous version?

Anthropic's table, safeguards on, compares Fable 5.1 with Fable 5, Opus 5, and GPT-5.6 Sol. These are their numbers.

Terminal-Bench-Science 0.1: 52.6%, 24.7%, 29.0%, 22.4%. Error is plus or minus 3.5 to 4.5 points. A public leaderboard reports Opus 5 at 30.0% and Fable 5 at 21.4%. Anthropic's setup gets 29.0% and 24.7%, which they call within noise. Their 52.6% for Fable 5.1 is far outside that band.

Terminal-Bench 4.0: Fable 5.1 55.8%, Mythos 5.1 60.9%, Fable 5 42.0%, Opus 5 52.3%, Sol 37.3%. They say the Fable-to-Mythos gap is older cyber safeguards, and they expect it to shrink.

GDPval-AA v2: 1853, 1723, 1824, 1711. OSWorld 2.0 partial: 77.9%, 72.9%, 75.4%. Strict: 41.7%, 36.1%, 39.6%. No Sol score, because the tasks are the August 2026 release and are not comparable to older OSWorld numbers. Caught tasks scored zero. Humanity's Last Exam, no tools: 60.9%, 57.8%, 56.6%. With tools: 65.0%, 63.8%, 63.6%. No Sol cell. AutomationBench: 31.4%, 17.1%, 26.9%, 19.6%. CursorBench 3.2.0: 73.4%, 70.5%, 70.0%, 67.2%. SpaceXAI's Sualeh Asif, quoted in the post, calls 73.4% at max effort their best CursorBench 3.2 result. That is a partner report.

## Where does it sit next to other tools a maker already uses?

Claude Code is the seat for the firmware review. Cursor is the other editor many shops already use. On August 12, SpaceXAI listed Grok 4.6 High at 69.9% on CursorBench v3.2. Anthropic's September 1 table lists Fable 5.1 at 73.4% on CursorBench 3.2.0. I have not checked that those labels share every task. Where Anthropic printed a GPT-5.6 Sol score, Fable 5.1 leads their table. Source review fits Fable. A penetration test does not.

## What does it cost, and who can use it today?

Fable 5.1 is $10 per million input tokens and $50 per million output tokens. Cache reads are $0.25 per million. The product page prices US-only inference at 1.1x for jobs that must run in the United States. That residency option is separate from Mythos being limited to some US organizations.

The product page lists Pro, Max, Team, Enterprise, and Microsoft Foundry. The news post lists Amazon Web Services, Google Cloud, Microsoft Azure, and the API name above. Retention defaults to 30 days. Mythos starts at the same $10 and $50 for vetted US partners, and a Pro plan does not include it.

Models released after August 2, 2026 carry a watermark, a hidden signal used to estimate whether Claude helped write a text. Anthropic says it does not change the writing or identify the user. Detection is a private preview.

## What is still unproven?

The table is Anthropic's. They say safeguards likely suppress some scores, including hard zeros. The science-bench error bar is wide, and this OSWorld set does not extend an older chart. GPT-5.6 Sol is missing on two rows. The savings percentages are an August index, not your repository. EFS is later this fall. Mythos on the cyber program is "near future," while Claude Security already runs the model.

I did not flash boards from Fable 5.1's notes on September 1. Keep a shipping review on source you can read, and do not ask for an exploit. I am not retelling a biology method.

## Disclosure

**Disclosure:** The author is a paying subscriber to ChatGPT Plus, Claude Pro, and SuperGrok and uses all three services on a daily basis. The Makers Workbench is not affiliated with OpenAI, Anthropic, xAI, Google, or any of the other major AI companies covered in our reporting. No company receives favorable editorial treatment based on the author's personal subscriptions.

## Sources and image credits

- Anthropic announcement (www.anthropic.com)
- Anthropic announcement (www.anthropic.com)
- Images: published by Anthropic with the announcement.
