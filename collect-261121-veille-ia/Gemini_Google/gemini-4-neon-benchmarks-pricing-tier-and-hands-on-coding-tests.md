---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-benchmarks-pricing-tier-and-hands-on-coding-tests
title: "Gemini 4 Neon: Benchmarks, Pricing Tier, and Hands-On Coding Tests"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "bench", "gemini", "coding", "benchmarks", "tests", "model", "carbon"]
source: https://www.mindstudio.ai/blog/gemini-4-neon-release
source_anchor: ""
source_lines: [1, 102]
section: "Gemini_Google"
---
## What is Gemini 4 Neon?

Gemini 4 Neon is Google’s latest model release in its expanding Gemini 4 lineup, positioned as a mid-tier option that trades a small amount of raw capability for significantly lower cost and faster output. It landed within weeks of Argon, Carbon, and Hydrogen, Google’s other Gemini 4 variants, and is built specifically for complex coding, analytical reasoning, and production-grade enterprise workflows. Early testing shows it handling 3D code generation, multi-agent coordination, and live cloud deployment tasks without hand-holding.

## TL;DR

- **Release cadence** for Gemini 4 models has compressed sharply: Argon, Carbon, Hydrogen, and now Neon all shipped within about two months, following Gemini 3.1 Pro back in February.
- **Neon targets the price-performance curve** rather than peak capability alone, offering frontier-level reasoning at a lower cost and faster speed than the larger Carbon model.
- On the **Artificial Analysis Intelligence Index** , Neon ranks third overall and second on the Pareto Frontier (the cost-versus-capability curve), with a first-place finish on Vending Bench 2.
- Neon tops multiple domain-specific benchmarks including **legal research bench, tax agent bench, Zapier’s automation bench** , and ranks best-in-class on terminal bench, science bench, and HLE Diamond.
- On raw coding benchmarks, **Claude’s Opus 5.5 and a model called Fable 5.2 still lead** , though Google claims Neon edges ahead by a narrow margin on some coding-specific tests.
- Hands-on testing in Google’s anti-gravity coding agent shows Neon generating 3D scenes from pure code (no Blender, no 3D software), coordinating **agent swarms** of up to seven workers, and deploying a working on-call monitoring agent to Google Cloud Run.
- A consistent behavioral pattern across tests: Neon **tests simple versions of a problem before attempting the complex version** , and builds its own verification and critique tools rather than trusting a single-pass output.

**Adjuster**✕ Export PHI

**#4471**

**12,480**users online

## How does Gemini 4 Neon compare to Carbon and Hydrogen?

Google now organizes its Gemini 4 releases around a kind of internal hierarchy, nicknamed informally by element names: Carbon is the large, heavy “racing” model built for maximum capability. Neon sits below it, offering comparable or better results on many benchmarks while running faster and cheaper. Hydrogen sits a tier below Neon, sacrificing more quality for even lower cost and latency, though it’s still described as a strong model in its own right.

The naming scheme has confused plenty of people, including developers directly asking Google’s own staff to clarify which model maps to which “tier” of capability. The practical takeaway: Neon is not a downgrade in the way cheaper models traditionally have been. By several measured benchmarks, it beats Carbon despite costing less and responding faster, which breaks the usual pattern where speed and price cuts come at the expense of quality.

## What do the benchmarks actually show?

Neon ranks third on the Artificial Analysis Intelligence Index and second on the Pareto Frontier, the chart that plots capability against cost and speed. It takes first place on Vending Bench 2, a benchmark testing long-horizon agentic behavior. It also leads the Vals legal and tax benchmarks (legal research bench and tax agent bench), and performs best-in-class across finance, accounting, spreadsheet modeling, and complex constraint-following tasks on Surge AI’s evaluation suite. It also tops Zapier’s automation bench, terminal bench, science bench, and HLE Diamond.

On coding specifically, the picture is more mixed. Google has published benchmark results claiming Neon edges out Opus 5.5 and Fable 5.2 by a small margin on certain coding tests, putting it in the number one spot. That claim hadn’t been independently verified with deep coding tasks at time of testing, and on most established coding benchmarks, Opus 5.5 and Fable 5.2 remain difficult to beat. Neon also scores well on cybersecurity-related evaluations, an area that’s increasingly becoming a standard benchmark category for frontier models.

## How does Neon perform on real coding tasks?

Testing inside Google’s anti-gravity agent environment (using the “extra high” reasoning setting) produced several notable results:

**3D generation without 3D tools.** Given a single image of a dragon, Neon was tasked with producing a signed distance function, a mathematical formula describing the dragon’s surface entirely in code, with no 3D model file involved. It worked independently for about 38 minutes, writing its own critique tool that rendered the dragon from five angles each round and compared the output against the reference image. The result closely matched the source image using pure code, no Blender or similar software involved.

**Animated SVG from video.** Neon converted a short reference video of a peacock into a single animated SVG file, an image format where every shape is defined in text rather than pixels. It wrote Python programs to generate the bird, tail, forest background, and camera movement, then rendered and checked the output across three rounds.

Build an onboarding app for 4,000 new hires.

Build ↵
**3D walkthroughs from floor plans.** Asked to find a floor plan online and turn it into a walkable house, Neon selected a 1959 plan by architect Marcel Breuer and built a full 3D tour. Initial output looked cartoonish and the camera clipped through walls; after a follow-up prompt asking for realistic visuals and proper door-based navigation, it corrected the issue.

## Can Neon coordinate multiple agents at once?

Anti-gravity recently added a “swarm” feature where one coordinator agent splits a task among several worker agents. Given a brief to build a browser-based military diorama made of small 3D blocks, Neon’s coordinator split the work across seven agents, each owning a distinct piece (terrain, vehicles, frame-rate verification, and so on). The team ran 15 rounds of improvements over about four hours.

The swarm approach used roughly 16 million tokens, the rough equivalent of 160 novels, most of it spent on agents reading and re-checking each other’s code. That token burn is a real cost consideration: swarms make sense for complex, multi-part builds, but they are not a cheap default for simple tasks.

## Can Neon deploy real infrastructure, not just demos?

This is the test most relevant to working developers. Given access to a Google Cloud project, Neon was tasked with building “Pager,” an on-call monitoring agent that watches a live app, detects failures, and fixes them with human approval. Working alone for about 65 minutes, it built a sample coffee shop app, simulated fake user traffic, deployed everything to Cloud Run (Google’s serverless container platform), and built the agent on top of Google’s Agent Development Kit (ADK).

When a bad deploy was pushed that dropped a payment timeout from seconds to milliseconds, causing a spike in checkout failures, the agent identified the exact configuration change within a minute, recognized a similar past incident using a memory bank, and paused for human approval before rolling back. After approval, the error rate dropped to zero. The agent was designed to read logs and shift traffic but not delete anything, a deliberate guardrail for agents touching live systems.

## Does Neon hold up on research-grade experiments?

Given access to a GPU cluster, Neon was tested against several published research tasks. It implemented a “growing neural cellular automata” paper, training a tiny neural network that regrows a pixel-based lizard image after damage, in about 23 minutes. It wrote a reinforcement learning algorithm from scratch to teach a simulated four-legged robot to run, first validating its approach on a simpler pole-balancing task before attempting the harder one, and the robot reached roughly twice the target performance score.

It also simulated a galaxy collision involving more than half a million particles, first validating a single galaxy in isolation to confirm energy conservation stayed within a hundredth of a percent before running the full collision. In a weight-space programming test based on recent MIT research, Neon wrote the weights of a small neural network directly, without training, to play Breakout, averaging about 550 points per game against a published benchmark of around 25 points for a trained network on the same task. On a harder game in the same test set, it scored less than half of the trained network’s result, showing the technique doesn’t generalize evenly across tasks.

## Is Gemini 4 Neon worth using?

**1,284**

**37**

**6**

| Vendor | Tier | Risk | Approval <sup>NEW</sup> | 
|---|---|---|---|
| **Northwind Supply** | Tier 1 | Low | Approved | 
| **Acme Logistics** | Tier 2 | Medium | Approve | 
| **Helios Data** | Tier 1 | High | Pending | 
| **Granite Freight** | Tier 3 | Low | Approved | 
| **Pinecrest Labs** | Tier 2 | Low | Approve | 

For developers who need strong reasoning and coding ability without paying Carbon-tier prices, Neon looks like a genuinely strong middle option rather than a compromise. It consistently tests simpler versions of a problem before tackling harder ones, builds its own verification tools, and pauses for human approval before making irreversible changes, all of which make it more trustworthy for production-adjacent work than a model that simply outputs a single-pass answer. The main caveat is that extra-high reasoning mode can burn a large number of tokens even on simple tasks, so reasoning levels need to be matched to task complexity rather than maxed out by default.

## Frequently Asked Questions

### What makes Gemini 4 Neon different from Gemini 4 Carbon?

Carbon is Google’s larger, higher-capability model. Neon delivers comparable or better results on many benchmarks while being faster and cheaper to run, making it the more practical choice for everyday coding and reasoning work.

### Does Gemini 4 Neon beat Claude on coding benchmarks?

Google has published results claiming Neon narrowly leads Opus 5.5 and Fable 5.2 on specific coding benchmarks, but on most established coding evaluations, those Claude-family models remain the stronger performers.

### What is anti-gravity, the tool used to test Neon?

Anti-gravity is Google’s agentic coding environment where models like Neon can plan, write, test, and deploy code autonomously, including coordinating multiple agents in a “swarm” for complex multi-part tasks.

### What reasoning levels does Gemini 4 Neon support?

Neon offers four thinking levels: low, medium, high, and extra high. Extra high produces the most thorough results but can consume very large numbers of tokens, so it’s best reserved for genuinely complex tasks.

### Can Gemini 4 Neon be trusted with live production systems?

In testing, Neon built and deployed a working on-call monitoring agent to Google Cloud, correctly diagnosed a live failure, and paused for human approval before rolling back a bad deploy, suggesting it can handle production-adjacent tasks when guardrails like human approval are kept in place.
