---
id: collect-261121-veille-ia/claude-anthropic/claude-sonnet-5-5-vs-sonnet-5-what-actually-changed-378a0211
title: "Claude sonnet 5 5 vs sonnet 5 what actually changed"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-28"]
keywords: ["sonnet", "claude", "opus", "anthropic", "work", "tokens", "effort", "more"]
source: ""
source_anchor: ""
source_lines: [1, 66]
section: "Claude_Anthropic"
---
Claude Sonnet 5.5, released on September 28, 2026, is a clear step up from Sonnet 5 at the same token price. It generates output more than 30% faster, and Anthropic says it costs up to 30% less per task. The biggest jump is in agentic coding: its Terminal-Bench 4.0 score rose from 10.3% to 70.6%.
It arrived six days after Opus 5.5 and is pitched as the faster, cheaper model for well-scoped everyday work: bug fixes, documents, slides and spreadsheets. For most people on Sonnet 5, upgrading is an easy call. Developers with production code should check a handful of breaking API changes first. Unless noted, every figure below comes from Anthropic’s own launch post and docs.
At a glance
|  | Sonnet 5 | Sonnet 5.5 | 
|---|---|---|
| API model ID | claude-sonnet-5 | claude-sonnet-5-5 | 
| Price per million tokens (input / output) | $2 / $10 | $2 / $10 | 
| Output speed | Baseline | 30%+ faster | 
| Cost per task | Baseline | Up to 30% lower | 
| Cyber safeguards and fallbacks | Not applied | Applied; higher-risk requests fall back to Sonnet 5 | 
Sources: Anthropic launch post and Sonnet 5.5 model docs.
Speed and cost: what “up to 30% cheaper” really means
The price list did not change. Sonnet 5.5 still costs $2 per million input tokens, $10 per million output tokens and $0.20 per million for cache reads. The saving comes from efficiency: it typically needs far fewer tokens, and makes fewer tool calls, to finish the same job. A workload dominated by long fixed inputs may therefore see a smaller drop than 30%.
Effort level matters more than the headline. Anthropic says that on several benchmarks, Sonnet 5.5 at Low or Medium effort beats Sonnet 5’s best score for about a tenth of the cost per task. On FrontierCode at High effort it scores 10 points above Sonnet 5 at roughly one-fifteenth of the cost. Claude Code and the Claude apps default to Medium effort; the Claude Platform defaults to High.
Customers quoted in the launch post report similar patterns. Balyasny Asset Management measured about 121k tokens per answer on its finance tasks, against 497k for Sonnet 5. Box reported 2.4x faster responses and 12% fewer tokens. These are vendor-selected testimonials, so treat them as directional and run your own evals.
Benchmarks: coding, knowledge work and computer use
| Benchmark | Sonnet 5 | Sonnet 5.5 | Opus 5.5 | 
|---|---|---|---|
| Terminal-Bench 4.0 (agentic terminal coding) | 10.3% | 70.6% | 66.4% | 
| FrontierCode 1.1 (mergeable code changes) | 42.4% | 46.2% at Max, 52.1% at Xhigh | 54.4% | 
| CursorBench 4.0 (real coding sessions) | 34.1% | 55.5% | 57.8% | 
| GDPval-AA v2.1 (real-world work, 44 occupations) | 1449 | 1844 | 1846 | 
| AA-Briefcase v1.1 (long-horizon knowledge work) | 1359 | 1811 | 1822 | 
| Humanity’s Last Exam (with tools) | 54.9% | 64.5% | 67.7% | 
| OSWorld 2.1 (computer use) | 57.0% | 80.1% | 81.8% | 
| Chartography (chart recognition, no tools) | 15.6% | 61.6% | 64.4% | 
The Terminal-Bench gap is the headline, but read it carefully. Opus 5.5’s 66.4% is reported at its Xhigh effort setting, and Anthropic states that Opus 5.5 remains clearly stronger at complex, open-ended work that needs sustained judgment. A benchmark lead is not a general lead.
The FrontierCode row shows why more effort is not always better. Sonnet 5.5 scored lower at Max than at Xhigh because, per Anthropic, it more often launched Claude Code’s multi-agent code-review skill, which in two examined cases caused a timeout or edits beyond the task’s scope. FrontierCode penalises out-of-scope changes.
On CursorBench, Sonnet 5.5 lands within about two points of Opus 5.5. Base44 reported that across 118 real app builds it matched Opus 5 quality in 3.6 iterations per build on average, versus 7.7 for Opus 5. That is a customer-reported figure from Anthropic’s post.
Knowledge work, writing and design
On GDPval-AA, Sonnet 5.5 sits two points below Opus 5.5 and about 400 points above Sonnet 5. Anthropic also says it writes more clearly than the previous generation, and early testers called it a better collaborator than Sonnet 5.
Design and documents are a stated focus. Testers said it adds polish to interfaces and follows slide templates closely enough that decks need little editing. In one internal Anthropic test, it received a public company’s quarterly earnings materials, call transcripts and a slide template, and produced a 10-slide operating review. Two experts judged the first draft ready to send as it was. That is a single vendor-run test, not an independent result.
It is also the first Sonnet model to finish Pokémon Red using only screenshots, which Anthropic cites as evidence of stronger long-horizon work and image understanding.
Safety and cyber safeguards
The biggest policy change is in cybersecurity. Four points matter:
- Alignment: on Anthropic’s roughly 1,850-scenario automated audit, Sonnet 5.5 matches or improves on Sonnet 5 across most measures. Opus 5.5 is still slightly better overall. Anthropic adds that no evaluation catches every failure.
- Cyber: Sonnet 5.5’s cyber capability is comparable to Opus 5’s, so it is the first Sonnet launched with Opus-style safeguards. Higher-risk cyber requests visibly fall back to Sonnet 5, while routine bug finding and fixing is unaffected. Defenders can apply to a Cyber Verification Program for broader access.
- Biology: the safeguards are the same as Sonnet 5’s. Some microbiology and virology requests may be flagged in error.
- Distillation: it is the first Sonnet with classifiers that block reasoning extraction, and thinking is now tied to the account that produced it. Teams that move conversations between accounts should read the docs on preserved thinking.
Breaking changes for API developers
Five changes can break code already running on Sonnet 5:
- Thinking off now means the new between_tools setting, which keeps up-front thinking off. It works at High effort or below.
- Forced tool use returns an error.
- Thinking blocks are tied to the model and conversation that produced them.
- On the Claude API and Google Cloud, the earlier computer_20251124 computer use tool is not accepted.
- The advisor tool rejects Opus 4.8, Opus 4.7 and Sonnet 5 as advisors.
One more change breaks nothing but alters behaviour: text between tool calls now arrives in thinking blocks. An app that streams that text to users will go quiet between tool calls until you set a display value that returns it, or switch to between_tools. Also note that setting temperature, top_p or top_k to a non-default value returns a 400 error.
Read the migration guide before swapping the model ID.
Where it sits next to Opus 5.5
Sonnet 5.5 costs half as much per token as Opus 5.5: $2 and $10 per million input and output tokens, against $4 and $20. Cache writes are $2.50 against $5. Anthropic says the two work best together. Sonnet 5.5 shines at lower effort settings, where it is cheaper per task, and can match Opus at similar cost at higher settings. Opus 5.5 stays the pick for difficult, open-ended work. One early tester described letting Opus set a game’s architecture and Sonnet implement it.
Should you upgrade?
- Claude app users: nothing to do. Sonnet 5.5 is the current Sonnet, running at Medium effort by default.
- Developers on Sonnet 5: yes, but test first. Work through the five breaking changes, then run your own evals at Low and Medium effort, since that is where the cost savings are claimed.
- Cyber and security teams: expect some higher-risk requests to fall back to Sonnet 5 until you are verified for broader access.
- High-volume, cost-sensitive workloads: consider waiting. Anthropic says Haiku 5.5 will join the 5.5 family in the coming weeks.
Sonnet 5.5 is available on the Claude Platform, Amazon Web Services, Google Cloud and Microsoft Azure, with zero data retention.
Verdict: for the same price, you get a faster model that finishes more agentic coding tasks at a fraction of the token spend. It is the default choice for everyday work, and Opus 5.5 remains the choice for the hardest problems. Benchmark figures are Anthropic’s own and were measured on its own setups, so validate them against your workload.
Sources
- Introducing Claude Sonnet 5.5 (Anthropic, Sept 28, 2026)
- Claude Sonnet 5.5 model overview (Claude Platform Docs)
- Models overview (Claude Platform Docs)
Enjoy Worthview?
Add Worthview as a Preferred Source on Google to see more of our stories in Search.
Worthview Editorial Team is the shared byline for content created collaboratively by Worthview’s editors and contributors. Since 2008, we’ve published thousands of articles across technology, AI, finance, health, home, travel, and lifestyle. Our editorial process emphasizes original research, reputable sources, regular content updates, and clear attribution. Articles covering higher-trust topics, including health and finance, may also undergo review by qualified subject-matter experts.
2 thoughts on “Claude Sonnet 5.5 vs Sonnet 5: What Actually Changed”
Comments are closed.
