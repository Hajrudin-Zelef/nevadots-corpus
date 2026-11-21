---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-keeps-sonnet-5-s-price-and-posts-anthropic-s-terminal-bench-jump-tmwb
title: "Claude Sonnet 5.5 Keeps Sonnet 5's Price and Posts Anthropic's Terminal-Bench Jump | TMWB"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["sonnet", "anthropic", "opus", "datasheet", "model", "claude", "cache", "effort"]
source: https://www.themakersworkbench.com/ai/claude-sonnet-5-5-keeps-sonnet-5s-price-and-posts-anthropics-terminal-bench-jump
source_anchor: ""
source_lines: [1, 66]
section: "Claude_Anthropic"
---
A USB controller datasheet, a firmware bug with a clear symptom, and a cleanup of lab notes do not need the most expensive model on the bench. On September 28, 2026, Anthropic introduced Claude Sonnet 5.5 for that work. It is the second model in the 5.5 family. The newsroom dates the first, Opus 5.5, to September 22, so Opus 5.5 is already available. Haiku 5.5 is not. Anthropic says it arrives in the coming weeks. It has not shipped.

A token is a chunk of text, often a piece of a word. Per million tokens, Sonnet 5.5 is $2 of input, $10 of output, and $0.20 to reread a cached prefix. That list matches Sonnet 5. A cache is the stored front of a prompt, so later turns can reread it at the lower rate. Anthropic says the model runs more than 30 percent faster than Sonnet 5, and that in their testing a task costs up to 30 percent less because it uses fewer tokens. The sticker did not fall. The bill can.

I would use it for everyday firmware bugs, datasheet summaries, and slide or document polish. Those are the jobs where I would rather not burn Opus 5.5's $20-per-million output rate, or a Fable-class price this page does not even list.

## What actually changed?

Most bench jobs I have are scoped: a suspend bug, the electrical limits in a datasheet, bring-up notes that need a cleanup. Anthropic positions Sonnet 5.5 as the faster, lower-cost complement to Opus 5.5 for that work, including documents, slides, and spreadsheets. Their own testing still finds Opus 5.5 stronger at open-ended jobs that need sustained judgment.

The number that will get repeated is Terminal-Bench 4.0. Anthropic describes it as an agentic coding test: can a model, acting as an agent that takes many steps in a command line, finish complex tasks? An agent is that multi-step loop. Anthropic reports 70.6 percent for Sonnet 5.5 and 10.3 percent for Sonnet 5. Opus 5.5 at its highest effort, labeled Xhigh, scores 66.4 percent on the same table. That gap from 10.3 to 70.6 is enormous. It is Anthropic's number, on their harness, with the method written up in their system card. It is not an independent lab result.

Low or medium effort, on Anthropic's charts, beats Sonnet 5's best score at about a tenth of the cost per task.

## How does the new piece work?

Effort is the knob for how long the loop thinks. Lower effort is faster and spends fewer tokens. Higher effort checks more. In Claude Code and the apps, the default is Medium. On the Claude Platform, the default is High. Check that setting before you blame the model for a sloppy patch.

Storing a cache prefix costs $2.50 per million tokens. Rereading it costs $0.20. The write costs more than a normal input token at $2. The savings show up on later turns, when the datasheet stays at the front. Opus 5.5 on the same table is $4 input, $20 output, $0.20 cache reads, and $5 cache writes. Sonnet 5.5 is half of Opus on fresh input, fresh output, and cache writes. Cache reads match at $0.20.

The API id is `claude-sonnet-5-5`. If you ran Sonnet with thinking off, Anthropic says you need the new `between_tools` setting, which keeps up-front thinking off, before you switch.

Cyber capability is why the safeguards changed. Anthropic says Sonnet 5.5 is comparable to Opus 5 on cybersecurity, so this is the first Sonnet to launch with cyber safeguards and fallbacks like the ones on their most capable models. They also say the safeguards resemble those on Opus 5.5. Routine bug fixing in your own code still works. Higher-risk cybersecurity tasks visibly fall back to Sonnet 5. Biology safeguards match Sonnet 5 and aim at a narrow set of high-risk requests. Ordinary software work is unaffected.

## What does this look like on a real project?

The board fails to reappear after the host suspends. I have the datasheet, a serial excerpt, and notes I would not hand anyone yet. Sonnet 5.5 is the model I would leave selected, at Medium effort in the app. I did not run that sequence on launch day.

First the datasheet: supply limits, the configuration bits that survive suspend, and the timings the firmware uses. The first stored turn pays the $2.50 cache write. Later questions on the same prefix should be cache reads at $0.20 per million instead of $2.

Second, the bug, scoped to the suspend files. I want a patch and one sentence on how to test it. Third, a short bring-up sheet a teammate can read. Anthropic says the writing is clearer and that the model can follow a slide template. If the bug turns into an open question about the state machine, I move that conversation to Opus 5.5. Haiku 5.5 would be the volume model for a pile of similar logs, and it is still not on the platform.

## How does it compare with the previous version?

The list price versus Sonnet 5 is unchanged. Anthropic's claim is speed, fewer tokens, and large gains on their coding tests.

On FrontierCode, which checks whether a change could be merged, Anthropic says High-effort Sonnet 5.5 scores 10 points above Sonnet 5 at the same setting, at about one fifteenth of the cost per task, and matches GPT-6 Sol's best score at about a fifth of the cost. At Max effort the score fell. Anthropic says a split review sometimes timed out or edited past the task.

On CursorBench, Anthropic says the best score sits within about two points of Opus 5.5. SpaceXAI's Sualeh Asif, quoted on the page, gives 55.5 percent. CursorBench does not publish GPT-6 Sol, so the bar shown is GPT-5.6 Sol. On GDPval-AA, Anthropic says Sonnet 5.5 is about 400 points above Sonnet 5 and two points below Opus 5.5. Artificial Analysis ran that test on a pre-release build Anthropic says had a since-fixed bug that may have understated the score.

## Where does it sit next to other tools a maker already uses?

GPT-6 Sol, from September 22, lists the same $2 and $10 fresh-token sticker, with OpenAI's own 90 percent cache-read discount beside it. Anthropic instead charges $2.50 to write a cache prefix and $0.20 to read it. A similar sticker can still produce a different bill.

I would leave Sonnet 5.5 on the scoped bug, the datasheet, and the doc polish, and move to Opus 5.5 when a wrong turn costs board time. Fable stays for work that has outgrown both. Haiku 5.5 is the missing cheap tier, so bulk logs on Claude stay on Sonnet until it ships.

## What does it cost, and who can use it today?

Per million tokens: $2 input, $10 output, $0.20 cache reads, $2.50 cache writes. Opus 5.5 on the same table is $4, $20, $0.20, and $5. Anthropic says a typical task costs up to 30 percent less than Sonnet 5 because fewer tokens are used, and that output is more than 30 percent faster.

Sonnet 5.5 is available now, with zero data retention as on Opus 5.5 and Sonnet 5. Anthropic names their apps, Claude Code, the Claude Platform as `claude-sonnet-5-5`, Amazon Web Services, Google Cloud, and Microsoft Azure. The page says all platforms, including those clouds. It does not print a Free, Pro, or Team matrix.

Higher-risk cyber tasks fall back to Sonnet 5. Anthropic says broader access for cyberdefenders is coming. It is not the default today.

## What is still unproven?

The 70.6 versus 10.3 Terminal-Bench pair needs an outside rerun. A jump that large can be real and still depend on the harness and the effort setting. Anthropic's Opus figure is that model's best score on the test, and Sonnet 5.5 still leads their table. Their own judgment still puts Opus 5.5 ahead on open-ended work. GDPval-AA was run on a pre-release build they later patched. I have not used Sonnet 5.5 on a live board for this piece. Haiku 5.5 remains unshipped.

## Disclosure

**Disclosure:** The author is a paying subscriber to ChatGPT Plus, Claude Pro, and SuperGrok and uses all three services on a daily basis. The Makers Workbench is not affiliated with OpenAI, Anthropic, xAI, Google, or any of the other major AI companies covered in our reporting. No company receives favorable editorial treatment based on the author's personal subscriptions.

## Sources and image credits

- Anthropic announcement (www.anthropic.com)
- Images: published by Anthropic with the announcement.
