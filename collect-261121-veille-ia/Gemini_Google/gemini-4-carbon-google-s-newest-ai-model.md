---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-google-s-newest-ai-model
title: "Gemini 4 Carbon: Google's Newest AI Model"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "gemini", "google", "argon", "model", "index", "opus", "gpt-6"]
source: https://www.datacamp.com/blog/gemini-4-carbon
source_anchor: ""
source_lines: [1, 305]
section: "Gemini_Google"
---
Gemini 4 Carbon tops the Artificial Analysis Intelligence Index. See what's new, how it compares to Opus 5.5 and GPT-6 Astra, pricing, and who can use it.

Two weeks after Gemini 4 Argon became broadly available, Google is back with another model. Gemini 4 Carbon, announced today, is what Google calls its "next frontier model," and it has taken first place on the newly updated Artificial Analysis Intelligence Index v5.0.

Most people can't use it yet. Carbon is launching first to a small group of cybersecurity defenders, with developers, businesses, and consumers getting access only later.

In this article, I'll cover what Gemini 4 Carbon is, what's changed since Argon, how it performs on benchmarks (both Google's and independent ones), what it will cost, and when you can expect to start using it.

Finally, before we get started, I recognize you may have landed on this article after hearing generally about Google AI news. To be clear, Gemini 4 Carbon (what this article is about) is a text- and code-focused frontier large language model; Genie 4, which was also just released, is an interactive, physics-based world model for real-time digital simulations. Genie 4 is also worth reading about.

What Is Gemini 4 Carbon?

Gemini 4 Carbon is the newest model in Google's Gemini 4 family, sitting above Gemini 4 Argon. That's a big story because when Argon was released only a couple weeks ago, it was widely talked about as a big comeback for Google on account of the fact that Argon has such impressive benchmark tests.

Carbon is even better. Google positions the model as great for long-horizon agentic tasks, meaning work that takes many steps, many tool calls, and a lot of recovering from dead ends.

According to Google DeepMind, internal teams have seen Carbon run for days on multi-step engineering problems, such as root-causing and patching bugs in large codebases and low-level systems.

Google also claims gains in:

Coding and full-stack web development

Agentic tasks and computer use

Scientific reasoning

Multimodal understanding

Google hasn't published a technical report, architecture details, or a parameter count. It also hasn't shared an API spec, so we don't know yet whether Carbon keeps Argon's limits (a context window of over 1 million tokens, a 65,536-token default output limit, and text, image, video, audio, and PDF input).

Gemini 4 Carbon vs. Gemini 4 Argon

The Gemini 4 rollout has moved quickly:

Gemini 4 Argon

Gemini 4 Carbon

Announced

September 30, 2026

October 28, 2026

Broad availability

October 13, 2026

Not yet, Fairwind Program only

AA Intelligence Index v5.0

50

59

Introductory price (in/out per 1M tokens)

$2 / $10

$2 / $10

The benchmark results are impressive, no doubt. Notice how Carbon improves on Argon in all 17 rows of Google's published comparison.

Gemini 4 Carbon Benchmarks vs. the Competition

Let me divide this section into two smaller parts: Google's own reported results, and independent evals.

Google-reported results

Carbon has the top score in 15 of 17 rows. The two exceptions are:

Software engineering: On FrontierSWE 2, GPT-6 Astra and Claude Opus 5.5 both beat Carbon.

Computer use: On the OSWorld-2.1 offline subset, Claude Opus 5.5 leads.

Carbon's clearest lead is on long-context reasoning. On GraphWalks, it scores far ahead of GPT-6 Astra and Opus 5.5.

On Vending-Bench 2, a simulated business benchmark, Carbon finished with a balance of $16,484, ahead of GPT-6 Astra and Argon.

Something to note: Google's results don't include uncertainty estimates, and the security benchmarks were run without guardrails, so real-world results may or will differ.

Independent evaluations

Artificial Analysis's independent numbers broadly back up Google's claims, although with nuance:

Intelligence Index v5.0: Carbon takes the top spot, barely ahead of Claude Opus 5.5 and GPT-6 Astra, and well ahead of Argon. Version 5.0 is a harder rebuild of the index, with most of its questions coming from private or withheld tests. That’s why scores are lower than in earlier versions.

Coding Agent Index: Carbon (via Antigravity CLI) scores 59. That's a solid jump from Argon (53), but behind Claude Code with Opus 5.5 (62).

AA-Omniscience (factual knowledge): Carbon tops the chart with a score of 54 and answers 58% of questions correctly (Argon: 48%). However, its hallucination rate rose from 4% to 11%.

On WebDev Arena, Carbon ranks #4 with 1,769 Elo, up 89 points.

The takeaway: Carbon is the strongest all-round model on the leaderboards right now, but it isn't the best at everything. If your workload is mostly agentic coding or computer use, things are still close.

Gemini 4 Carbon Cybersecurity

Security is a big story in this launch. Google breaks its cyber work into three stages, and Carbon reports strong numbers on each:

Vulnerability discovery: 88.2% on SEC-bench Pro (state-of-the-art)

Exploit validation: 54.8% on ExploitGym (state-of-the-art)

Patching: 71.0% on CWE-bench

Google also reports:

More thantwo times Argon's success rate at generating secure code

Carbon was over ten times better than another lab's frontier model at generating high-quality patches (no, the other lab wasn't named)

These abilities cut both ways. A model that finds and exploits vulnerabilities this well is valuable to defenders and attackers alike, which might explain the careful rollout.

Safety and Reliability Improvements

Carbon uses the same layered safety stack as Argon: safety training, real-time misuse defenses, prompt-injection resilience, and chain-of-thought monitoring. Google reports these improvements over Argon:

Fewer unsupported claims: roughly halved

Misalignment alerts: high-severity alerts in live coding environments fell by almost 80%

Scope control: overreach down 26%, under-service down 29%

Benchmark cheating: 0.0% successful cheating on Terminal-Bench 4.0, with attempts down to 0.4%

Note that the "fewer unsupported claims" result comes from Google's internal testing, while Artificial Analysis measured a higher hallucination rate on AA-Omniscience. The two tests do measure different things, but it complicates the overall story.

Gemini 4 Carbon Pricing

Carbon launches at the same introductory price as Argon:

Introductory

After introductory period

Input (per 1M tokens)

$2.00

$4.00

Output (per 1M tokens)

$10.00

$20.00

Cached input

95% off the input rate (~$0.10)

95% off the input rate

Google hasn't said when Carbon's introductory period ends. For reference, Argon's introductory pricing ends on November 30, 2026.

As an example, a workload with 10M input tokens and 1M output tokens would cost about $30 at introductory rates and $60 afterward.

How to Access Gemini 4 Carbon

Right now, you probably can't. Here's the rollout plan:

Now: A subset of cyber defenders get access.

Next: Access expands gradually to more trusted partners through Google's Fairwind Program.

Later: Developers, businesses, and consumers.

In the meantime, Gemini 4 Argon is available now at the same introductory price and is a good way to start building workflows you can later move to Carbon.

Final Thoughts

Ultimately, because Carbon shares the exact same pricing structure as Argon, it is highly likely to quickly overshadow its predecessor for most users. The moment Carbon hits wide public availability, I expect developers and enterprise clients will naturally migrate to it to take advantage of its superior abilities.

In the end, Argon will have been a model with a short shelf life. To me, Google's aggressive rollout shows they are completely willing to phase out Argon quickly, putting aside a good model that failed to capture the top spot on the Artificial Analysis Intelligence Index in favor of a true winner. Because now Gemini 4 Carbon puts Google back at the top of the overall leaderboards. It's another reminder that while we are busy benchmarking today's release, these tech giants already have something significantly more powerful sitting in the lab.

Author

Josef Waples

I'm a data science editor with contributions to research articles in scientific journals. I'm especially interested in linear algebra, statistics, R, and the like.

FAQs

When was Gemini 4 Carbon released?⌄

Google announced Gemini 4 Carbon on October 28, 2026. It's currently available only to selected cyber defenders through the Fairwind Program.

Is Gemini 4 Carbon better than Claude Opus 5.5 and GPT-6 Astra?⌄

On overall intelligence, yes, narrowly. Carbon scores 59 on the Artificial Analysis Intelligence Index v5.0, vs. 58 for Opus 5.5 and 57 for GPT-6 Astra. But Opus 5.5 leads on computer use (OSWorld-2.1) and agentic coding (AA Coding Agent Index), and GPT-6 Astra leads on FrontierSWE 2.

How much does Gemini 4 Carbon cost?⌄

Introductory pricing is $2 per million input tokens and $10 per million output tokens, with a 95% discount on cached input. Later pricing will be $4 and $20.

What's the difference between Gemini 4 Argon and Gemini 4 Carbon?⌄

Carbon is Google's more capable model. It beats Argon on every benchmark in Google's comparison, scores 9 points higher on the AA Intelligence Index (59 vs. 50), and doubles Argon's secure-code success rate. Argon is generally available; Carbon isn't yet.

Does Gemini 4 Carbon hallucinate less than Argon?⌄

It depends on the test. Google reports unsupported claims roughly halved, but Artificial Analysis found Carbon's hallucination rate on AA-Omniscience rose from 4% to 11%, even as accuracy improved.

When will developers get access to Gemini 4 Carbon?⌄

Google hasn't given a date. Access will expand through the Fairwind Program first, then to developers, businesses, and consumers "as soon as possible."

Learn about Gemini 3.1 Pro, Google's latest reasoning model. Explore its features, benchmarks, hands-on tests, and how it compares to Claude Opus 4.6, Claude Sonnet 4.6, and GPT-5.2.

Explore what's new in Anthropic's latest flagship: stronger agentic coding, sharper vision, and better memory across sessions. Compare the benchmarks against GPT-5.4, Gemini 3.1 Pro, and the locked-away Mythos Preview.

Google's table puts Gemini 4 Argon ahead of GPT-6 Astra on legal, finance, and long-context work at a fifth of the price. Astra still leads hard coding and is the one you can use.

Google's new Gemini 4 Hydrogen matches the larger Argon on intelligence while running 2.5x faster than Gemini 3.8 Flash. Here's how it fits into the Gemini 4 family, what the benchmarks show, what it costs, and when you should use it.

Google's new frontier model leads on enterprise knowledge work and real-world software engineering, ships with a 1M output token limit, and goes to cyber defenders first.

Two weeks after Gemini 4 Argon became broadly available, Google is back with another model. Gemini 4 Carbon, announced today, is what Google calls its "next frontier model," and it has taken first place on the newly updated Artificial Analysis Intelligence Index v5.0.

Most people can't use it yet. Carbon is launching first to a small group of cybersecurity defenders, with developers, businesses, and consumers getting access only later.

In this article, I'll cover what Gemini 4 Carbon is, what's changed since Argon, how it performs on benchmarks (both Google's and independent ones), what it will cost, and when you can expect to start using it.

Finally, before we get started, I recognize you may have landed on this article after hearing generally about Google AI news. To be clear, Gemini 4 Carbon (what this article is about) is a text- and code-focused frontier large language model; Genie 4, which was also just released, is an interactive, physics-based world model for real-time digital simulations. Genie 4 is also worth reading about.

## What Is Gemini 4 Carbon?

Gemini 4 Carbon is the newest model in Google's Gemini 4 family, sitting above Gemini 4 Argon. That's a big story because when Argon was released only a couple weeks ago, it was widely talked about as a big comeback for Google on account of the fact that Argon has such impressive benchmark tests.

Carbon is even better. Google positions the model as great for long-horizon agentic tasks, meaning work that takes many steps, many tool calls, and a lot of recovering from dead ends.

According to Google DeepMind, internal teams have seen Carbon run for days on multi-step engineering problems, such as root-causing and patching bugs in large codebases and low-level systems.

Google also claims gains in:

Google hasn't published a technical report, architecture details, or a parameter count. It also hasn't shared an API spec, so we don't know yet whether Carbon keeps Argon's limits (a context window of over 1 million tokens, a 65,536-token default output limit, and text, image, video, audio, and PDF input).

## Gemini 4 Carbon vs. Gemini 4 Argon

The Gemini 4 rollout has moved quickly:

AnnouncedBroad availabilityAA Intelligence Index v5.0Introductory price (in/out per 1M tokens)
The benchmark results are impressive, no doubt. Notice how Carbon improves on Argon in all 17 rows of Google's published comparison.

## Gemini 4 Carbon Benchmarks vs. the Competition

Let me divide this section into two smaller parts: Google's own reported results, and independent evals.

## Google-reported results

Carbon has the top score in 15 of 17 rows. The two exceptions are:

Software engineering:On FrontierSWE 2, GPT-6 Astra and Claude Opus 5.5 both beat Carbon.Computer use:On the OSWorld-2.1 offline subset, Claude Opus 5.5 leads.
Carbon's clearest lead is on long-context reasoning. On GraphWalks, it scores far ahead of GPT-6 Astra and Opus 5.5.

On Vending-Bench 2, a simulated business benchmark, Carbon finished with a balance of $16,484, ahead of GPT-6 Astra and Argon.

Something to note: Google's results don't include uncertainty estimates, and the security benchmarks were run without guardrails, so real-world results may or will differ.

## Independent evaluations

Artificial Analysis's independent numbers broadly back up Google's claims, although with nuance:

Intelligence Index v5.0:Carbon takes the top spot, barely ahead of Claude Opus 5.5 and GPT-6 Astra, and well ahead of Argon. Version 5.0 is a harder rebuild of the index, with most of its questions coming from private or withheld tests. That’s why scores are lower than in earlier versions.Coding Agent Index:Carbon (via Antigravity CLI) scores 59. That's a solid jump from Argon (53), but behind Claude Code with Opus 5.5 (62).AA-Briefcase (knowledge work):1,609 Elo, up 126 points from Argon, but still behind Grok 4.7, Muse Wonder, and Kimi K3.1 Pro.AA-Omniscience (factual knowledge):Carbon tops the chart with a score of 54 and answers 58% of questions correctly (Argon: 48%). However, its hallucination rate rose from 4% to 11%.
On WebDev Arena, Carbon ranks #4 with 1,769 Elo, up 89 points.

The takeaway:Carbon is the strongest all-round model on the leaderboards right now, but it isn't the best at everything. If your workload is mostly agentic coding or computer use, things are still close.
## Gemini 4 Carbon Cybersecurity

Security is a big story in this launch. Google breaks its cyber work into three stages, and Carbon reports strong numbers on each:

Vulnerability discovery:88.2% on SEC-bench Pro (state-of-the-art)Exploit validation:54.8% on ExploitGym (state-of-the-art)Patching:71.0% on CWE-bench
Google also reports:

These abilities cut both ways. A model that finds and exploits vulnerabilities this well is valuable to defenders and attackers alike, which might explain the careful rollout.

## Safety and Reliability Improvements

Carbon uses the same layered safety stack as Argon: safety training, real-time misuse defenses, prompt-injection resilience, and chain-of-thought monitoring. Google reports these improvements over Argon:

Fewer unsupported claims:roughly halvedMisalignment alerts:high-severity alerts in live coding environments fell by almost 80%Scope control:overreach down 26%, under-service down 29%Benchmark cheating:0.0% successful cheating on Terminal-Bench 4.0, with attempts down to 0.4%Prompt injection:0.0% attack success on Gray Swan's indirect prompt-injection benchmark (Argon: 0.7%)
Note that the "fewer unsupported claims" result comes from Google's internal testing, while Artificial Analysis measured a

higherhallucination rate on AA-Omniscience. The two tests do measure different things, but it complicates the overall story.
## Gemini 4 Carbon Pricing

Carbon launches at the same introductory price as Argon:

Google hasn't said when Carbon's introductory period ends. For reference, Argon's introductory pricing ends on November 30, 2026.

As an example, a workload with 10M input tokens and 1M output tokens would cost about $30 at introductory rates and $60 afterward.

## How to Access Gemini 4 Carbon

Right now, you probably can't. Here's the rollout plan:

Now:A subset of cyber defenders get access.Next:Access expands gradually to more trusted partners through Google's Fairwind Program.Later:Developers, businesses, and consumers.
In the meantime, Gemini 4 Argon is available now at the same introductory price and is a good way to start building workflows you can later move to Carbon.

## Final Thoughts

Ultimately, because Carbon shares the exact same pricing structure as Argon, it is highly likely to quickly overshadow its predecessor for most users. The moment Carbon hits wide public availability, I expect developers and enterprise clients will naturally migrate to it to take advantage of its superior abilities.

In the end, Argon will have been a model with a short shelf life. To me, Google's aggressive rollout shows they are completely willing to phase out Argon quickly, putting aside a good model that failed to capture the top spot on the Artificial Analysis Intelligence Index in favor of a true winner. Because now Gemini 4 Carbon puts Google back at the top of the overall leaderboards. It's another reminder that while we are busy benchmarking today's release, these tech giants already have something significantly more powerful sitting in the lab.
