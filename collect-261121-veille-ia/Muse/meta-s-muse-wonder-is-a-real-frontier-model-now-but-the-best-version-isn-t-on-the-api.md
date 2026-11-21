---
id: collect-261121-veille-ia/muse/meta-s-muse-wonder-is-a-real-frontier-model-now-but-the-best-version-isn-t-on-the-api
title: "Meta's Muse Wonder Is a Real Frontier Model Now — But the Best Version Isn't on the API"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-12"]
keywords: ["meta", "wonder", "model", "muse", "full", "models", "frontier", "version"]
source: https://dev.to/paullovelace/metas-muse-wonder-is-a-real-frontier-model-now-but-the-best-version-isnt-on-the-api-1c67
source_anchor: ""
source_lines: [1, 75]
section: "Muse"
---
On October 12, 2026, Meta released Muse Wonder and Muse Wonder Full — the first frontier-class models from Alexandr Wang's rebuilt Meta Superintelligence Labs. The claim from Wang: "true frontier models," roughly level with the best from OpenAI and Anthropic. Nearly a month later, with independent scores in, that claim mostly holds. The catch is not the benchmarks. It's which version you can actually use.

I have not run Muse Wonder hands-on yet, and this article is built from Meta's announcement and model card, plus independent coverage from VentureBeat and the Artificial Analysis scores — checked November 10, 2026. Where a claim is Meta's, I'll say so, because a launch scorecard is a vendor claim, not an independent result.

Two models, developed under the internal codename "Watermelon." Muse Wonder is the standard model; Muse Wonder Full is the larger, stronger version. Both are built for the same job: personal and professional agentic work — long-running tasks where the model has to plan, use tools, remember context, and finish something, not just answer a question.

The specs that matter:

- 
**1M-token context window** at inference, per the official model card.
- 
**Multimodal input** — text, image, video, and audio in; text out.
- 
**Active Memory** — Meta's headline feature. The model is designed to remember things you mentioned days, weeks, or months ago and use them in later tasks. Meta's own MemoryBench puts Wonder at 85.6 and Wonder Full at 85.5, against 81.5 for Claude Opus 5.5 and 81.4 for GPT-6 Astra. Read that carefully, though: MemoryBench is Meta's own benchmark, measuring recall on Meta's own definition of remembering. The gap is real but narrow, and it's Meta grading its own homework.
- 
**Active Focus** — the model's ability to stay on a long task without drifting, which is the difference between an agent that finishes a job and one that wanders off after step three.

Both models were trained at Prometheus, Meta's gigawatt-scale data center in Ohio. That detail matters less as trivia and more as a signal: this is the $100-billion-plus compute bet Zuckerberg and Wang committed to at the end of 2025, before the model existed. The lab itself is tiny by frontier standards — fewer than 200 researchers — built after Llama 4 landed badly in April 2025 and Zuckerberg paid $14.3 billion for a 49% stake in Scale AI to bring Wang in.

Here is Meta's published scorecard, all at maximum reasoning effort. I've kept the independent Artificial Analysis row first, because it's the one number nobody at Meta chose.

| Benchmark | Wonder Full | Wonder | GPT-6 Astra | Claude Opus 5.5 | 
|---|---|---|---|---|
| Artificial Analysis Intelligence Index | 56 | 52 | 53 | **58** | 
| DeepSWE 1.1 (long-horizon coding) | **79.0** | 78.8 | 74.1 | 74.2 | 
| SWE-Marathon (long-running coding) | **57.5** | 53.1 | 42.5 | 46.3 | 
| Harvey LAB-AA (legal tasks) | **13.1** | 10.0 | 8.6 | 4.2 | 
| AutomationBench-AA (business workflows) | **45.4** | 39.1 | 41.6 | 41.2 | 
| MemoryBench | 85.5 | 85.6 | 81.4 | 81.5 | 
| Terminal-Bench 4.0-AA | 57.1 | 52.0 | 59.1 | **59.6** | 
| HealthBench Pro | 62.3 | 60.5 | 65.3 | **65.6** | 

What this actually says: Wonder Full ties Claude Sonnet 5.5 at 56 on the Artificial Analysis Intelligence Index, ahead of GPT-6 Astra (53) and Claude Fable 5.1 (53), but behind Claude Opus 5.5 (58). The standard Wonder scores 52 — ninth of 216 models in its class on that index, one point behind GPT-6 Astra.

The pattern in the rest of the table is consistent and worth noticing. Wonder Full leads clearly where the work is long and agentic — DeepSWE, SWE-Marathon, legal work, business automation. It loses where the work is general knowledge or health reasoning, and it trails slightly on Terminal-Bench. That is not a random scatter. It is a model tuned for agents, and it shows.

Two caveats, both from Meta's own model card. First, the scorecard mixes Meta-run evaluations, third-party results, and official leaderboard scores, and the card says competitor runs are best-effort — prompts, tools, and runtimes may not be equally optimized for every model. Second, on SWE-Marathon specifically, the GPT-6 and Fable results were produced with Codex and Claude Code respectively, so that row is not a controlled common-harness comparison. Neither caveat erases the result, but both are the reason you should read the table as "roughly level at the frontier" rather than "Meta wins."

Through the Meta Model API, standard Muse Wonder costs **$4 per million input tokens, $0.20 per million cached input tokens, and $20 per million output tokens**. That is exactly the same as Gemini 4 Carbon's regular pricing, and half of Claude Fable 5.2's $8/$40. On price alone, Wonder is competitive.

Here is the catch, and it's the most important fact in this article: **Muse Wonder Full is not available through the API at all.** The stronger model — the one that ties Sonnet 5.5 and leads the coding benchmarks — is only available to Muse Code subscribers, on $50 or $200 monthly plans. The standard Wonder is also in the Muse app on $20 or $100 plans, while the free tier stays on the smaller Muse Spark.

And there is a second, quieter break with Meta's past: **neither Wonder model ships with downloadable weights.** This is a closed model, hosted only by Meta, sold by subscription and by the token like everyone else's. The company that built its reputation on open Llama weights is now competing the closed way. If you were waiting for an open frontier model from Meta, this isn't it.

The practical question is not whether Wonder is "frontier" — on the independent index it is, just barely, and on long agentic coding tasks it's genuinely strong. The question is whether it's worth your tokens and your time.

- 
**Try the API Wonder first on your longest coding task.** DeepSWE and SWE-Marathon are the benchmarks that map closest to real work: a multi-file refactor, a long debugging session, a feature that takes hours, not minutes. At $4/$20 with $0.20 cached input, it's priced to be tried. Give it the task that usually takes your current model two or three attempts and see if it finishes in one.
- 
**Don't pay for Wonder Full until you've hit Wonder's ceiling.** The Full model's advantage is real on the scorecard, but it's locked behind a Muse Code subscription. If the API model handles your work, the subscription buys you a benchmark edge, not a different capability.
- 
**The memory feature is the one to watch, not the benchmarks.** If Active Memory works the way Meta describes it — remembering project decisions and preferences across weeks — that's the feature that changes how you use an agent day to day. But it's also the feature with the least independent verification so far. MemoryBench is Meta's own test. Treat it as a promise to verify, not a result.
- 
**Plan around a closed model.** No weights means no self-hosting, no fine-tuning on your own hardware, and no exit if Meta changes pricing or terms. That's the same deal as OpenAI and Anthropic, and it's now Meta's deal too. If open weights matter to your stack, Wonder doesn't change your options.

- 
**Independent reproduction of the agentic benchmarks.** The Artificial Analysis index is independent; most of the task-specific rows are Meta-run or Meta-selected. Nobody outside Meta has run the full suite independently yet.
- 
**How Active Memory behaves in real projects.** A benchmark with distractor conversations is not the same as six weeks of real project history.
- 
**The full safety report.** Meta says a full report with additional safety evaluations is coming; for now the 13-page model card is the main public technical document. It reports CyberGym at 88.4% and ExploitBench at 58.9% for Wonder Full, and notes the unmitigated model was assessed at moderate or lower risk for cybersecurity and loss of control.
- 
**What happens to the Muse app's free tier.** Wonder starts in paid subscriptions because it costs more to serve. Whether it ever reaches free users is not announced.

Meta is back in the frontier race, and this time the claim survives contact with independent scores. Muse Wonder Full ties Sonnet 5.5 on the Artificial Analysis index and beats the field on long-horizon coding, legal work, and business automation. The standard Wonder, the one you can actually call through an API, is a step behind at 52 — good, not leading.

But the story isn't really the scoreboard. It's that Meta spent $100 billion-plus, rebuilt its entire AI lab around a 29-year-old who had never trained a frontier model, and shipped something that belongs in the same conversation as Claude and GPT-6. The old Meta — the one that gave away Llama weights and competed on distribution — is gone. The new Meta competes on the model itself, behind a paywall, at the same price as everyone else.

That makes Wonder worth trying, not worth switching to sight unseen. Run your hardest task through the API, compare it against whatever you use today, and let your own work decide. The benchmarks say it's close. Your task is the only judge that matters.

Would you give Wonder your hardest coding task this week — or does the fact that the best version isn't on the API keep you where you are?

*Sources: Meta announcement and Muse Wonder model card, October 12, 2026; VentureBeat, October 12, 2026; Artificial Analysis Intelligence Index scores as reported October 2026; The Business Times, October 13, 2026; Times of India, November 9–10, 2026 (Wang/Zuckerberg Cheeky Pint interview). All capability, benchmark and pricing claims are Meta's or the cited third parties', stated as such. No hands-on testing was performed for this article. This breaking article is not the daily buyer-intent post, and it contains no affiliate links.*

## Top comments (0)
