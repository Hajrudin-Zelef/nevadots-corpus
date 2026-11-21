---
id: collect-261121-veille-ia/gemini-google/google-s-gemini-4-lineup-explained-argon-carbon-hydrogen-neon
title: "Google's Gemini 4 Lineup Explained: Argon, Carbon, Hydrogen, Neon"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "carbon", "model", "hydrogen", "google", "gemini", "argon", "each"]
source: https://www.mindstudio.ai/blog/google-gemini-4-model-lineup
source_anchor: ""
source_lines: [1, 106]
section: "Gemini_Google"
---
## What is Google’s Gemini 4 lineup?

Google is naming its Gemini 4 model releases after chemical elements instead of version numbers. So far that includes Argon, Carbon, Hydrogen, and Neon, each shipped within weeks of the last. The names don’t map to a strict ranking of “best to worst.” Instead they represent different points on a tradeoff curve between raw capability, speed, and cost, with Carbon positioned as the heavier, higher-capability model and Neon as a newer release that claims to beat it on quality while being faster and cheaper to run.

## TL;DR

- Google has been **shipping Gemini 4 variants at a rapid clip** , naming each one after an element (Argon, Carbon, Hydrogen, Neon) rather than using a sequential version scheme.
- **Neon** , the most recent release covered here, is pitched as better, cheaper, and faster than Carbon, which until then was the heavier “flagship” model in the lineup.
- **Hydrogen sits further down the cost curve** , trading some quality for even lower latency and price, while still performing strongly on independent evaluations.
- On the **Artificial Analysis Intelligence Index** , Neon reportedly lands around third place, with a strong showing on the Pareto frontier that plots capability against cost.
- Gemini 4 models reportedly take the **top spots on a range of narrower benchmarks** , including vending-machine simulation tests, legal and tax agent benchmarks, and finance/spreadsheet-modeling evaluations from third-party evaluators.
- On **coding specifically, Anthropic’s Claude models (Opus 5.5) remain the benchmark to beat** , though Google claims Neon edges ahead on at least one coding evaluation.
- The naming scheme reflects a broader shift at Google: **optimizing the price-to-performance curve** rather than chasing a single “biggest model” title.

**›**Build a safety-inspection app for 60 plants.

*already included:*✓ SOC 2✓ role-based access✓ audit log✓ self-hosted

## Why is Google naming models after elements?

There’s no official explanation confirmed in Google’s documentation here, but the pattern that’s emerged is functional rather than arbitrary. Each element name corresponds to a distinct release with its own tradeoffs on the capability-versus-cost curve, rather than a simple “v1, v2, v3” progression where each release strictly supersedes the last.

This has created confusion, including for people steeped in AI daily. A common question creators and users have been asking Google’s own developer relations team is some version of “what do these names actually mean, is Neon the small fast model and Hydrogen the mid-tier one, like Anthropic’s haiku/sonnet/opus tiers?” The short answer: not exactly. The elements aren’t cleanly nested by size the way Claude’s naming is. Instead, each new element release seems to represent Google’s latest attempt to push the Pareto frontier forward, meaning better results per dollar and per millisecond, not necessarily a bigger model.

## What differentiates Argon, Carbon, Hydrogen, and Neon?

Based on how each has been positioned:

**Carbon** has served as the heavier, higher-capability model in the family, the one other releases get compared against.

**Hydrogen** sits at a lower cost and latency point. It sacrifices some quality relative to Carbon and Neon in exchange for being cheaper and faster, though it’s still described as a strong performer in absolute terms.

**Neon** is the newest entrant and currently the headline release. It’s positioned as beating Carbon on quality while also being cheaper and faster, which is the unusual part. Normally a cheaper, faster model implies a quality tradeoff. Neon appears to break that pattern on a number of benchmarks, which is part of why it’s drawn attention.

**Argon** rounds out the current set of four, though less detail is available on how it’s differentiated from the others.

The practical takeaway for builders: don’t assume the alphabetical or chemical order tells you which model to pick. Check what tradeoff you need (cost, latency, raw capability) and match it to whichever element currently sits at that point on the curve, since Google appears to be iterating on all four simultaneously rather than retiring older ones.

## How does Neon perform on benchmarks?

Neon reportedly ranks around third on the Artificial Analysis Intelligence Index and second on that same group’s Pareto Frontier chart, which plots model capability against cost and latency. It’s also cited as a top performer on Vending Bench 2, a simulated business-operation benchmark from Andon Labs that tests how well an agent can run a simple commercial operation over time.

On more specialized evaluations, Gemini 4 models (plural, not just Neon) reportedly top the Vals index, along with dedicated legal research and tax agent benchmarks. Surge AI’s evaluations reportedly show Gemini 4 models leading across finance, accounting, spreadsheet modeling, and complex constraint-following tasks. Zapier’s automation benchmark and the previously mentioned Vending Bench 2 are also cited as wins. Neon is additionally described as best-in-class on terminal-based benchmarks, science reasoning benchmarks, and HLE Diamond, a harder subset of the Humanity’s Last Exam benchmark set.

*Enterprises build with AI on Remy.*

- Self-hosted
- SOC 2
- HIPAA
- Audit log
- Role-based access

On coding, the picture is murkier. Claude’s models, specifically Opus 5.5, remain the standard most coding benchmarks point to as the leader. Google has pointed to at least one benchmark where Gemini 4 Neon on its highest reasoning setting edges out both Opus 5.5 and competing models by a narrow margin, but that claim hadn’t been independently verified through hands-on coding tests at time of release.

## Is Neon actually better, or just cheaper?

The claim worth scrutinizing is that Neon beats Carbon on quality while also being cheaper and faster. That’s an unusual combination. Normally, labs ship a cheaper tier by trimming capability (this is effectively what Hydrogen represents in this lineup). Neon instead seems to represent a genuine step up in efficiency, Google compressing more capability into a cheaper, faster package rather than just offering a worse model at a lower price.

That said, “better” is benchmark-dependent. Neon’s strengths concentrate in agentic and domain-specific tasks: finance, legal research, tax analysis, spreadsheet work, automation workflows, and simulated business operations. On general coding tasks, Anthropic’s models still hold an edge according to most public coding benchmarks. So the honest framing is that Neon is a strong generalist with particular strength in structured, rules-heavy domains, not an unconditional upgrade over every competing model in every category.

One more data point worth noting: informal testing using competitive bot-vs-bot arenas (where models write scripts that compete against each other in a game-like environment, adapting and improving over multiple rounds) showed Neon improving faster than other models tested, including some from competing labs, even without access to shared learnings that other bots in the same test had accumulated. That’s a small, early, and informal signal rather than a rigorous benchmark, but it points to strong self-improvement characteristics in agentic, iterative settings.

## What does this mean for people building with AI?

If you’re choosing among Gemini 4 models, the practical approach is to match the element to the job rather than assuming the newest name is always the right pick:

- For latency- and cost-sensitive applications at high volume, Hydrogen is the one to evaluate first.
- For general-purpose agentic work, especially anything touching finance, legal, tax, or structured business logic, Neon currently looks like the strongest option in the family.
- For raw coding tasks, it’s still worth benchmarking Claude’s Opus 5.5 and Google’s own Carbon or Neon side by side on your actual codebase rather than trusting a single leaderboard number.
- Expect this lineup to keep shifting. Google has been releasing new elements every few weeks, and each release has reset which model sits where on the cost-capability curve.

## Frequently Asked Questions

### What is Gemini 4 Neon?

Neon is the most recent model in Google’s Gemini 4 family, positioned as better, cheaper, and faster than Carbon, the previous heavier model in the lineup. It performs strongly on agentic and domain-specific benchmarks covering finance, legal research, tax analysis, and automation.

### Does Neon replace Carbon and Hydrogen?

Not necessarily. Google appears to be running multiple Gemini 4 variants in parallel, each occupying a different point on the cost-versus-capability tradeoff, rather than retiring earlier releases each time a new one ships.

### Is Gemini 4 Neon better than Claude for coding?

*Without shadow IT.*

**Every app, approved**

*✓*

*✓*

*✓*

*✓*

*✓*

On most public coding benchmarks, Anthropic’s Claude models, particularly Opus 5.5, still lead. Google has cited at least one benchmark where Neon narrowly outperforms Opus 5.5, but that result hasn’t been broadly verified through independent hands-on testing.

### Why does Google name models after chemical elements instead of version numbers?

Google hasn’t published an official explanation referenced in available sources. The pattern suggests each element represents a distinct tradeoff point on the capability-cost-speed curve rather than a simple sequential upgrade, which differs from naming schemes like Anthropic’s tiered haiku/sonnet/opus system.

### What benchmarks does Gemini 4 Neon lead on?

Reported strengths include the Vals index, legal research and tax agent benchmarks, Surge AI’s finance and spreadsheet modeling evaluations, Zapier’s automation benchmark, Vending Bench 2, terminal-based benchmarks, science reasoning tests, and HLE Diamond. It ranks around third on the Artificial Analysis Intelligence Index and second on that group’s Pareto Frontier.
