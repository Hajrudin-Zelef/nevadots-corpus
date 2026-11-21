---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-benchmarks-pricing-and-access
title: "Gemini 4 Argon: Benchmarks, Pricing, and Access"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["argon", "google", "tokens", "gemini", "output", "model", "work", "gpt-6"]
source: https://www.datacamp.com/blog/gemini-4-argon
source_anchor: ""
source_lines: [1, 451]
section: "Gemini_Google"
---
OpenAI spent September 29, 2026, at DevDay shipping GPT-6.1 Sol, a mid-tier model built to fix GPT-6 Astra's cost problem. A day later, Google announced Gemini 4 Argon, and the release goes to cybersecurity teams before it goes to developers.

Koray Kavukcuoglu, SVP of Google DeepMind, introduced Argon on September 30, 2026 as Google's frontier model for long-horizon work: real-world software engineering, legal and financial knowledge work, and cyber defense.

The headline engineering change is the output token limit, which jumps to 1M tokens from 64K. It sets a new state of the art on DeepSWE v1.1 at 77.9%, and it is rolling out first through the Fairwind Program to a cohort of trusted cyber defenders.

For context on the rivals, see our guides to GPT-6.1 Sol and Claude Sonnet 5.5.

## TL;DR

- Argon is Google's new frontier model, and it goes to trusted cyber defenders through the Fairwind Program before it reaches the public API.
- The structural change that matters is output length, not input context: Argon can write a single trajectory 16x longer than the previous 64K ceiling.
- It leads every enterprise knowledge-work benchmark Google published, and the margin on legal work is not close.
- GPT-6 Astra still wins terminal-based science tasks and FrontierSWE v2, and Claude Opus 5.5 still wins Terminal-bench 4.0.
- The introductory price of $2 per million input tokens undercuts Astra by 5x, but it doubles when the promo ends.
- There is nothing to switch to yet. No public model ID, no cloud listing, and no third-party reproduction of any score.

## What Is Gemini 4 Argon?

Gemini 4 Argon is Google's new frontier model, positioned at the top of the Gemini family above the Gemini 3.8 line that shipped through September 2026.

Google describes it as built to sustain deep reasoning across complex, long-horizon workflows, with three named target domains: software engineering, enterprise knowledge work like legal and finance, and cybersecurity defense.

The clearest generational change is the way Google expanded the output token limit to 1M tokens, up from 64K, which means Argon can generate hundreds of thousands of tokens inside one trajectory rather than being forced to stop and be re-prompted. 

## Gemini 4 Argon Key Features

Argon's feature set is organized around tasks that run for hours rather than seconds, which shows up in the output budget, the agent work, and the cyber tooling. Here are the four capabilities that change what you can actually build.

### Write 1M tokens in a single pass

Argon can produce up to 1M output tokens in one generation, against 64K on the previous generation.

Google's argument is that headroom changes reasoning quality, not just length: when the model can think and generate across hundreds of thousands of tokens in one trajectory, it solves harder problems without the stitching and state loss of a chunked workflow.

For comparison, GPT-6 Astra caps output at 128K tokens with a 1.05M context window.

If you have ever had an agent lose the plot halfway through a large refactor because it hit an output ceiling and had to be restarted, this is the constraint being removed.

The caveat is, of course, billing. Long generations at $10 per million output tokens add up fast, and Google has not published whether reasoning tokens bill at the output rate.

### Run codebase migrations end-to-end

Argon agents are already migrating C and C++ codebases to Rust inside Google, scaling from tens of thousands of lines in core libraries like re2 and libgav1 up to 800K+ lines for the Fuchsia OS Zircon kernel.

That is the kind of work that usually consumes engineer-quarters, and Google says these rewrites go through automated and manual auditing, emulation testing, and review before production.

The libgav1 result is the one I keep coming back to.

Argon agents took an existing Rust port of Google's open source video decoder and replaced 32K lines of hand-written SIMD code by running rounds of profile-guided experiments and studying compiler output until the compiler vectorized safe Rust automatically.

The result is a memory-safe decoder that runs 2.7x faster than the Rust port with identical video output.

### Internal deployment results at Google

Two internal numbers are worth pulling out because they are the closest thing to a real-world result in the whole announcement.

- Google says a team of Argon agents analyzed fleet-wide profiling telemetry and autonomously applied memory optimizations across its data centers, freeing over 300 TiB of memory once rolled out, with an estimated 500 TiB to 1 PiB in total savings.
- The second is from quantum computing research, where Argon helped optimize the spacetime resources (qubits multiplied by gates) of subroutines that bottleneck applications. In one case, it beat the published baseline by 40% in minutes.

Neither result has external verification, but both are specific enough to be checkable later.

### Handle finance, legal, and business automation with visual context

Argon is aimed squarely at knowledge work where the evidence lives in charts, filings, and video rather than in a clean text prompt.

Google cites professional chart analysis, pulling details from long videos, and taking action across a series of documents as first-class use cases, and Argon scores 91.7% on LVBench for long video understanding.

The practical version of this, for example, is a multi-step financial research task where the model reads a deck, cross-checks a table, and then executes against a business system.

Argon ranks first on Zapier's AutomationBench at 51.3%, which measures end-to-end execution across core business functions rather than single-turn answers.

### Find and patch vulnerabilities with the guardrails off

This is the most unusual thing in the release.

For trusted defenders in the Fairwind Program and for Google's internal teams, Google is shipping Argon *without* cyber guardrails, so those teams get the model's full offensive-adjacent capability for defensive work: autonomously finding, validating, and patching software vulnerabilities.

Wiz is already running Argon through its Scan for Good initiative, which remediates high-risk exposures in public infrastructure for free.

In an early demonstration, Argon found a severe vulnerability exposing sensitive personal information across healthcare software used by hospitals worldwide, one that Google says previous frontier models missed.

Alongside that, Google says Argon is its most resilient model against indirect prompt injection and leads Gray Swan's IPI benchmark, and that chain-of-thought and action monitors halt execution when the model drifts outside user intent.

Google also notes it deliberately kept monitoring findings out of training so Argon's reasoning would not be shaped to evade the monitors, which is a detail I wish more labs published.

Argon leads 13 of the 19 benchmarks Google published against GPT-6 Astra, Claude Fable 5.1, and Claude Opus 5.5, and the wins cluster in knowledge work, long context, and vulnerability remediation.

The losses cluster in terminal-driven science and one of the two software engineering suites, which is a more interesting pattern than a clean sweep would have been.

### Enterprise knowledge work

This is Argon's strongest area and the reason Google framed the launch around legal and finance.

On the Vals Index, which weights finance, coding, legal, and tax work by U.S. GDP contribution, Argon scores 68.9% against 67.0% for Claude Opus 5.5, 65.8% for Claude Fable 5.1, and 63.1% for GPT-6 Astra.

The gaps widen on the domain-specific agent evals.

Argon hits 65.4% on Vals Finance Agent v2 (multi-step financial research) versus 58.9% for Fable 5.1 and 53.5% for Astra, and 51.3% on AutomationBench versus 42.5% for Opus 5.5 and 31.4% for Fable 5.1.

Harvey's Legal Agent Benchmark is the outlier.

Argon scores 19.6% against 6.7% for Fable 5.1, 5.4% for Astra, and 3.8% for Opus 5.5. Every model is failing most of this benchmark, so read it as a relative signal on legal research and drafting rather than a model you would hand a contract to unsupervised.

### Agentic coding

Argon takes DeepSWE v1.1 at 77.9%, ahead of Claude Opus 5.5 at 74.2%, GPT-6 Astra at 74.1%, and Claude Fable 5.1 at 67.4%.

DeepSWE measures long-horizon software engineering against real repositories, so it rewards models that can hold a plan across many edits rather than producing one good diff.

That result reads differently once you price it.

In our coverage of GPT-6.1 Sol, we noted Sol matched Astra on DeepSWE v1.1 at roughly one-fifth of Astra's cost, at $2 input and $10 output per million tokens.

Argon's introductory rate is exactly the same $2/$10, and it beats Astra's DeepSWE score by 3.8 points.

Vibe Code Bench goes to Argon too, at 91.9% against 90.3% for both Claude models and 89.6% for Astra, although a 1.6-point spread across four frontier models is close to noise.

### Long context and multimodal understanding

The long-context gap is the widest in the whole table.

On GraphWalks BFS F1 across 256K to 1M tokens, Argon scores 84.2% versus 71.8% for Astra, 66.8% for Opus 5.5, and 65.0% for Fable 5.1.

At up to 128K, everyone is high, but Argon still leads at 99.7%.

GraphWalks asks a model to traverse a graph described in the prompt, so it degrades sharply when retrieval across a very long window gets unreliable.

A 12.4-point lead over Astra at the 1M end is the kind of margin that decides whether you can feed a whole repository or a full deposition set into one call.

On multimodal work, Argon takes LVBench at 91.7% (against 87.5% for Astra and 79.7% for Fable 5.1) and Chartography at 71.6%, narrowly ahead of Astra's 71.0%.

For scale on that second one, our Claude Sonnet 5.5 coverage recorded 61.6% on Chartography for Sonnet 5.5 and 64.4% for Opus 5.5.

On CWE-bench v1, which evaluates whether a model can remediate real security vulnerabilities, Argon ties GPT-6 Astra for first place at 68%, with Opus 5.5 at 67% and Fable 5.1 at 58%.

A tie is a fair result here, and Google builds its case on the discovery side instead.

Against 3.8 Flash Cyber, Google reports that Argon uncovered exposures across codebases spanning 20 programming languages on its internal vulnerability benchmark, and beat 3.8 Flash Cyber on Wiz's black-box penetration testing benchmark at mapping attack surface, finding vulnerabilities, and producing proof-of-concept evidence.

Black-box means no source code access, which is closer to how an external attacker sees a live web system.

### Where Argon falls behind

Google published the losses, which I appreciate.

Four of them I think are worth highlighting:

- **FrontierSWE v2:** Argon 55.0% against GPT-6 Astra's 65.5% and Opus 5.5's 62.3%, a 10.5-point deficit on the harder of the two SWE suites.
- **Terminal-bench 4.0:** Argon 57.4% against Opus 5.5 at 66.4%. Our Claude Sonnet 5.5 testing put the mid-tier Sonnet at 70.6% on the same benchmark, so Argon trails a model priced well below it on terminal-driven agent work.
- **Terminal-Bench Science 0.1:** Argon 57.6% against Astra's 68.1%, a 10.5-point gap on scientific tasks executed through a shell.
- **PostTrainBench and OSWorld-2.0:** Argon scores 45.3% on ML engineering against Opus 5.5's 49.3%, and 69.2% on the OSWorld-2.0 offline subset against Astra's 72.6%.

Argon does hold science and math elsewhere, with 88.8% on LABBench 2 against 85.4% for Astra and 68.6% for Fable 5.1, and 76.0% on RiemannBench against 72.0% for Astra.

The pattern I can see here is that Argon is the better planner and reader, while Astra and Opus 5.5 are still better at driving a terminal.

Here is the full published set, so you can find your own workload in it.

| Benchmark | Gemini 4 Argon | GPT-6 Astra | Claude Fable 5.1 | Claude Opus 5.5 | 
|---|---|---|---|---|
| **Vals Index** | **68.9%** | 63.1% | 65.8% | 67.0% | 
| **AutomationBench** | **51.3%** | 41.4% | 31.4% | 42.5% | 
| **Vals Finance Agent v2** | **65.4%** | 53.5% | 58.9% | 58.6% | 
| **Harvey's Legal Agent Benchmark** | **19.6%** | 5.4% | 6.7% | 3.8% | 
| **DeepSWE v1.1** | **77.9%** | 74.1% | 67.4% | 74.2% | 
| **FrontierSWE v2** | 55.0% | **65.5%** | 56.3% | 62.3% | 
| **Vibe Code Bench** | **91.9%** | 89.6% | 90.3% | 90.3% | 
| **Terminal-bench 4.0** | 57.4% | 58.2% | 57.9% | **66.4%** | 
| **PostTrainBench** | 45.3% | 44.3% | 40.2% | **49.3%** | 
| **Terminal-Bench Science 0.1** | 57.6% | **68.1%** | 52.6% | 63.3% | 
| **LABBench 2** | **88.8%** | 85.4% | 68.6% | 73.1% | 
| **RiemannBench** | **76.0%** | 72.0% | 65.6% | 69.6% | 
| **GraphWalks (up to 128K, BFS F1)** | **99.7%** | 98.7% | 91.4% | 90.6% | 
| **GraphWalks (256K to 1M, BFS F1)** | **84.2%** | 71.8% | 65.0% | 66.8% | 
| **Agent's Last Exam (pass rate)** | **39.5%** | 34.2% | — | 38.2% | 
| **OSWorld-2.0 (offline subset)** | 69.2% | **72.6%** | — | — | 
| **Chartography** | **71.6%** | 71.0% | 46.2% | 66.3% | 
| **LVBench** | **91.7%** | 87.5% | 79.7% | 83.7% | 
| **CWE-bench v1** | **68.0%** | **68.0%** | 58.0% | 67.0% | 

## What Google Is Doing About Safety Before Wide Release

Google is holding Argon back from general release while it hardens four safeguard areas, and it named all four in the announcement. This matters for anyone planning to deploy an agent with tool access.

- **Misuse:** Argon refuses cyber and CBRN requests while preserving dual-use scientific research, per Google's Frontier Safety Framework, with internal and external red teams running manual and automated attacks. Google is also monitoring the model's internal activations to spot misuse.
- **Indirect prompt injection:** Google reports Argon leads Gray Swan's IPI benchmark after automated red teaming and adversarial training.
- **Misalignment:** Monitors watch chain-of-thought and actions and halt execution when Argon exceeds user intent, and Google says it kept those findings out of training so the model would not learn to evade them.
- **System hardening:** Sandboxed environments are isolated and sealed before high-risk training or evaluation runs, following Google's agent control roadmap.

## Gemini 4 Argon Pricing

Argon launches at an introductory rate of $2 per million input tokens and $10 per million output tokens, with cached input tokens priced at 95% off the input rate. After the introductory period, the rate rises to $4 per million input and $20 per million output. Google has not published whether the reasoning tokens bill at the output rate.

Set against the field, the introductory price is aggressive, and the standard price is not. At $2/$10, Argon matches GPT-6.1 Sol's rate exactly while undercutting GPT-6 Astra's $10/$50 by 5x. At the post-promo $4/$20, it lands on the same rate we documented for Claude Opus 5.5 in our Opus 5.5 API tutorial.

One more cost angle: raw per-token price hides the cost of a task. In our GPT-6.1 Sol coverage, we recorded $23.80 per Terminal-Bench Science 0.1 task for GPT-6 Astra against $5.47 for Sol, so a model that wins a benchmark can still lose on the invoice. Google has not published per-task costs for Argon, which is the number I would want before committing an agent fleet to it.

Google has not published the length of the introductory period, so budget for the $4/$20 rate on anything that outlives a pilot.

The bill is roughly (price per token) × (tokens used), and a model can be cheap on the first thing and expensive on the second. So, long reasoning or thinking traces and verbose answers can be expensive. Also, with agentic workflows, there is multi-step use, which means each step re-sends a growing context, and the input costs pile up. Which is why some people are more skeptical about the costs:

Also worth flagging on the competitive side: Anthropic's September 2026 positioning for Claude Opus 5.5 was performance-comparable to Fable 5.1 at roughly 40% lower cost to run than Opus 5, so the pressure Argon faces at the $4/$20 standard rate will come from cheaper models rather than better ones.

## Gemini 4 Argon Availability

Availability is the real constraint. Argon is rolling out to a set of trusted cyber defenders through the Fairwind Program, and Google says it is engaged in the U.S. government's voluntary process for pre-release model access while it expands gradually. General access comes later, starting with paid API customers and Google AI Ultra subscribers. There is no free tier, no waitlist link, and no published rate limits in the announcement.

One thing to watch: Google's public AI pricing page was last updated September 24, 2026, and the rates visible there ($0.75 input and $3.75 output per million, promotional through December 31, 2026, rising to $1.50/$7.50 on January 1, 2027) belong to the Gemini 3.8 Flash tier, not Argon.

## How to Get Access to Gemini 4 Argon?

You mostly cannot, yet. Google has not published an API model ID for Gemini 4 Argon, and I would not guess one from the marketing name. Access today runs through the Fairwind Program cohort of trusted cyber defenders and Google's internal teams.

As of September 30, 2026, Argon is not listed in the OpenRouter catalog or the models.dev catalog, and it does not appear in the Google Vertex AI, Gemini CLI, Cursor, or GitHub Copilot model docs. Azure AI Foundry and AWS Bedrock model pages could not be confirmed either way. When it does land for paid API customers, the call shape will be the standard Gemini one:

```
from google import genai
client = genai.Client()
response = client.models.generate_content(
    model="<argon-model-id>",
    contents="Audit this module for memory-safety issues and propose a patch.",
)
print(response.text)
```

If you want to get the plumbing ready in the meantime, our Claude Opus 5.5 API tutorial walks through building an agentic investigator with read-only tools and programmatic tool calling, and the same patterns port across vendors with a model ID swap.

## What People Are Saying About Gemini 4 Argon

Almost nobody outside the Fairwind cohort has used Argon yet, so most reactions are first impressions of the announcement, not hands-on tests. Here's how the conversation has played out, from broad to specific.

Big picture / first impression thinking: people are impressed. 

The first impressions can't be separated from the benchmark results. The performance on Vals Index and the legal and finance results got the most attention, with the asterisk that these are all Google's own evals.

But one reaction captures what this release is really about. Argon is built for work that runs for hours, like rewriting a kernel in Rust, and that could have real implications for the future of work. 

## Final Thoughts

Google is betting that the next round of frontier competition is won on enterprise knowledge work and cyber defense rather than on chat quality, and Argon's Vals Index, AutomationBench, and GraphWalks numbers back that bet.

That being said, nobody outside the Fairwind cohort has yet reproduced a single score.

When it opens up to regular users, I would reach for Argon on long-context document and repository work and stay with Claude Opus 5.5 or GPT-6 Astra for terminal-driven agent loops, where Argon trails by 9 to 10 points.

If you want to build the agentic workflows these models are aimed at, start with our AI Fundamentals skill track. 

OpenAI spent September 29, 2026, at DevDay shipping GPT-6.1 Sol, a mid-tier model built to fix GPT-6 Astra's cost problem. A day later, Google announced Gemini 4 Argon, and the release goes to cybersecurity teams before it goes to developers.

Koray Kavukcuoglu, SVP of Google DeepMind, introduced Argon on September 30, 2026 as Google's frontier model for long-horizon work: real-world software engineering, legal and financial knowledge work, and cyber defense.

The headline engineering change is the output token limit, which jumps to 1M tokens from 64K. It sets a new state of the art on DeepSWE v1.1 at 77.9%, and it is rolling out first through the Fairwind Program to a cohort of trusted cyber defenders.

For context on the rivals, see our guides to GPT-6.1 Sol and Claude Sonnet 5.5.

## TL;DR

## What Is Gemini 4 Argon?

Gemini 4 Argon is Google's new frontier model, positioned at the top of the Gemini family above the Gemini 3.8 line that shipped through September 2026.

Google describes it as built to sustain deep reasoning across complex, long-horizon workflows, with three named target domains: software engineering, enterprise knowledge work like legal and finance, and cybersecurity defense.

The clearest generational change is the way Google expanded the output token limit to 1M tokens, up from 64K, which means Argon can generate hundreds of thousands of tokens inside one trajectory rather than being forced to stop and be re-prompted.

## Gemini 4 Argon Key Features

Argon's feature set is organized around tasks that run for hours rather than seconds, which shows up in the output budget, the agent work, and the cyber tooling. Here are the four capabilities that change what you can actually build.

## Write 1M tokens in a single pass

Argon can produce up to 1M output tokens in one generation, against 64K on the previous generation.

Google's argument is that headroom changes reasoning quality, not just length: when the model can think and generate across hundreds of thousands of tokens in one trajectory, it solves harder problems without the stitching and state loss of a chunked workflow.

For comparison, GPT-6 Astra caps output at 128K tokens with a 1.05M context window.

If you have ever had an agent lose the plot halfway through a large refactor because it hit an output ceiling and had to be restarted, this is the constraint being removed.

The caveat is, of course, billing. Long generations at $10 per million output tokens add up fast, and Google has not published whether reasoning tokens bill at the output rate.

## Run codebase migrations end-to-end

Argon agents are already migrating C and C++ codebases to Rust inside Google, scaling from tens of thousands of lines in core libraries like re2 and libgav1 up to 800K+ lines for the Fuchsia OS Zircon kernel.

That is the kind of work that usually consumes engineer-quarters, and Google says these rewrites go through automated and manual auditing, emulation testing, and review before production.

The libgav1 result is the one I keep coming back to.

Argon agents took an existing Rust port of Google's open source video decoder and replaced 32K lines of hand-written SIMD code by running rounds of profile-guided experiments and studying compiler output until the compiler vectorized safe Rust automatically.

The result is a memory-safe decoder that runs 2.7x faster than the Rust port with identical video output.

## Internal deployment results at Google

Two internal numbers are worth pulling out because they are the closest thing to a real-world result in the whole announcement.

Neither result has external verification, but both are specific enough to be checkable later.

## Handle finance, legal, and business automation with visual context

Argon is aimed squarely at knowledge work where the evidence lives in charts, filings, and video rather than in a clean text prompt.

Google cites professional chart analysis, pulling details from long videos, and taking action across a series of documents as first-class use cases, and Argon scores 91.7% on LVBench for long video understanding.

The practical version of this, for example, is a multi-step financial research task where the model reads a deck, cross-checks a table, and then executes against a business system.

Argon ranks first on Zapier's AutomationBench at 51.3%, which measures end-to-end execution across core business functions rather than single-turn answers.

## Find and patch vulnerabilities with the guardrails off

This is the most unusual thing in the release.

For trusted defenders in the Fairwind Program and for Google's internal teams, Google is shipping Argon

withoutcyber guardrails, so those teams get the model's full offensive-adjacent capability for defensive work: autonomously finding, validating, and patching software vulnerabilities.
Wiz is already running Argon through its Scan for Good initiative, which remediates high-risk exposures in public infrastructure for free.

In an early demonstration, Argon found a severe vulnerability exposing sensitive personal information across healthcare software used by hospitals worldwide, one that Google says previous frontier models missed.

Alongside that, Google says Argon is its most resilient model against indirect prompt injection and leads Gray Swan's IPI benchmark, and that chain-of-thought and action monitors halt execution when the model drifts outside user intent.

Google also notes it deliberately kept monitoring findings out of training so Argon's reasoning would not be shaped to evade the monitors, which is a detail I wish more labs published.

## How Does Gemini 4 Argon Perform on the Benchmarks?

Argon leads 13 of the 19 benchmarks Google published against GPT-6 Astra, Claude Fable 5.1, and Claude Opus 5.5, and the wins cluster in knowledge work, long context, and vulnerability remediation.

The losses cluster in terminal-driven science and one of the two software engineering suites, which is a more interesting pattern than a clean sweep would have been.

## Enterprise knowledge work

This is Argon's strongest area and the reason Google framed the launch around legal and finance.

On the Vals Index, which weights finance, coding, legal, and tax work by U.S. GDP contribution, Argon scores 68.9% against 67.0% for Claude Opus 5.5, 65.8% for Claude Fable 5.1, and 63.1% for GPT-6 Astra.

The gaps widen on the domain-specific agent evals.

Argon hits 65.4% on Vals Finance Agent v2 (multi-step financial research) versus 58.9% for Fable 5.1 and 53.5% for Astra, and 51.3% on AutomationBench versus 42.5% for Opus 5.5 and 31.4% for Fable 5.1.

Harvey's Legal Agent Benchmark is the outlier.

Argon scores 19.6% against 6.7% for Fable 5.1, 5.4% for Astra, and 3.8% for Opus 5.5. Every model is failing most of this benchmark, so read it as a relative signal on legal research and drafting rather than a model you would hand a contract to unsupervised.

## Agentic coding

Argon takes DeepSWE v1.1 at 77.9%, ahead of Claude Opus 5.5 at 74.2%, GPT-6 Astra at 74.1%, and Claude Fable 5.1 at 67.4%.

DeepSWE measures long-horizon software engineering against real repositories, so it rewards models that can hold a plan across many edits rather than producing one good diff.

That result reads differently once you price it.

In our coverage of GPT-6.1 Sol, we noted Sol matched Astra on DeepSWE v1.1 at roughly one-fifth of Astra's cost, at $2 input and $10 output per million tokens.

Argon's introductory rate is exactly the same $2/$10, and it beats Astra's DeepSWE score by 3.8 points.

Vibe Code Bench goes to Argon too, at 91.9% against 90.3% for both Claude models and 89.6% for Astra, although a 1.6-point spread across four frontier models is close to noise.

## Long context and multimodal understanding

The long-context gap is the widest in the whole table.

On GraphWalks BFS F1 across 256K to 1M tokens, Argon scores 84.2% versus 71.8% for Astra, 66.8% for Opus 5.5, and 65.0% for Fable 5.1.

At up to 128K, everyone is high, but Argon still leads at 99.7%.

GraphWalks asks a model to traverse a graph described in the prompt, so it degrades sharply when retrieval across a very long window gets unreliable.

A 12.4-point lead over Astra at the 1M end is the kind of margin that decides whether you can feed a whole repository or a full deposition set into one call.

On multimodal work, Argon takes LVBench at 91.7% (against 87.5% for Astra and 79.7% for Fable 5.1) and Chartography at 71.6%, narrowly ahead of Astra's 71.0%.

For scale on that second one, our Claude Sonnet 5.5 coverage recorded 61.6% on Chartography for Sonnet 5.5 and 64.4% for Opus 5.5.

## Cybersecurity remediation

On CWE-bench v1, which evaluates whether a model can remediate real security vulnerabilities, Argon ties GPT-6 Astra for first place at 68%, with Opus 5.5 at 67% and Fable 5.1 at 58%.

A tie is a fair result here, and Google builds its case on the discovery side instead.

Against 3.8 Flash Cyber, Google reports that Argon uncovered exposures across codebases spanning 20 programming languages on its internal vulnerability benchmark, and beat 3.8 Flash Cyber on Wiz's black-box penetration testing benchmark at mapping attack surface, finding vulnerabilities, and producing proof-of-concept evidence.

Black-box means no source code access, which is closer to how an external attacker sees a live web system.

## Where Argon falls behind

Google published the losses, which I appreciate.

Four of them I think are worth highlighting:

FrontierSWE v2:Argon 55.0% against GPT-6 Astra's 65.5% and Opus 5.5's 62.3%, a 10.5-point deficit on the harder of the two SWE suites.Terminal-bench 4.0:Argon 57.4% against Opus 5.5 at 66.4%. Our Claude Sonnet 5.5 testing put the mid-tier Sonnet at 70.6% on the same benchmark, so Argon trails a model priced well below it on terminal-driven agent work.Terminal-Bench Science 0.1:Argon 57.6% against Astra's 68.1%, a 10.5-point gap on scientific tasks executed through a shell.PostTrainBench and OSWorld-2.0:Argon scores 45.3% on ML engineering against Opus 5.5's 49.3%, and 69.2% on the OSWorld-2.0 offline subset against Astra's 72.6%.
Argon does hold science and math elsewhere, with 88.8% on LABBench 2 against 85.4% for Astra and 68.6% for Fable 5.1, and 76.0% on RiemannBench against 72.0% for Astra.

The pattern I can see here is that Argon is the better planner and reader, while Astra and Opus 5.5 are still better at driving a terminal.

Here is the full published set, so you can find your own workload in it.

Vals Index68.9%AutomationBench51.3%Vals Finance Agent v265.4%Harvey's Legal Agent Benchmark19.6%DeepSWE v1.177.9%FrontierSWE v265.5%Vibe Code Bench91.9%Terminal-bench 4.066.4%PostTrainBench49.3%Terminal-Bench Science 0.168.1%LABBench 288.8%RiemannBench76.0%GraphWalks (up to 128K, BFS F1)99.7%GraphWalks (256K to 1M, BFS F1)84.2%Agent's Last Exam (pass rate)39.5%OSWorld-2.0 (offline subset)72.6%Chartography71.6%LVBench91.7%CWE-bench v168.0%68.0%
## What Google Is Doing About Safety Before Wide Release

Google is holding Argon back from general release while it hardens four safeguard areas, and it named all four in the announcement. This matters for anyone planning to deploy an agent with tool access.

Misuse:Argon refuses cyber and CBRN requests while preserving dual-use scientific research, per Google's Frontier Safety Framework, with internal and external red teams running manual and automated attacks. Google is also monitoring the model's internal activations to spot misuse.Indirect prompt injection:Google reports Argon leads Gray Swan's IPI benchmark after automated red teaming and adversarial training.Misalignment:Monitors watch chain-of-thought and actions and halt execution when Argon exceeds user intent, and Google says it kept those findings out of training so the model would not learn to evade them.System hardening:Sandboxed environments are isolated and sealed before high-risk training or evaluation runs, following Google's agent control roadmap.
## Gemini 4 Argon Pricing

Argon launches at an introductory rate of $2 per million input tokens and $10 per million output tokens, with cached input tokens priced at 95% off the input rate. After the introductory period, the rate rises to $4 per million input and $20 per million output. Google has not published whether the reasoning tokens bill at the output rate.

Set against the field, the introductory price is aggressive, and the standard price is not. At $2/$10, Argon matches GPT-6.1 Sol's rate exactly while undercutting GPT-6 Astra's $10/$50 by 5x. At the post-promo $4/$20, it lands on the same rate we documented for Claude Opus 5.5 in our Opus 5.5 API tutorial.

One more cost angle: raw per-token price hides the cost of a task. In our GPT-6.1 Sol coverage, we recorded $23.80 per Terminal-Bench Science 0.1 task for GPT-6 Astra against $5.47 for Sol, so a model that wins a benchmark can still lose on the invoice. Google has not published per-task costs for Argon, which is the number I would want before committing an agent fleet to it.

Google has not published the length of the introductory period, so budget for the $4/$20 rate on anything that outlives a pilot.

The bill is roughly (price per token) × (tokens used), and a model can be cheap on the first thing and expensive on the second. So, long reasoning or thinking traces and verbose answers can be expensive. Also, with agentic workflows, there is multi-step use, which means each step re-sends a growing context, and the input costs pile up. Which is why some people are more skeptical about the costs:

Also worth flagging on the competitive side: Anthropic's September 2026 positioning for Claude Opus 5.5 was performance-comparable to Fable 5.1 at roughly 40% lower cost to run than Opus 5, so the pressure Argon faces at the $4/$20 standard rate will come from cheaper models rather than better ones.

## Gemini 4 Argon Availability

Availability is the real constraint. Argon is rolling out to a set of trusted cyber defenders through the Fairwind Program, and Google says it is engaged in the U.S. government's voluntary process for pre-release model access while it expands gradually. General access comes later, starting with paid API customers and Google AI Ultra subscribers. There is no free tier, no waitlist link, and no published rate limits in the announcement.

One thing to watch: Google's public AI pricing page was last updated September 24, 2026, and the rates visible there ($0.75 input and $3.75 output per million, promotional through December 31, 2026, rising to $1.50/$7.50 on January 1, 2027) belong to the Gemini 3.8 Flash tier, not Argon.

## How to Get Access to Gemini 4 Argon?

You mostly cannot, yet. Google has not published an API model ID for Gemini 4 Argon, and I would not guess one from the marketing name. Access today runs through the Fairwind Program cohort of trusted cyber defenders and Google's internal teams.

As of September 30, 2026, Argon is not listed in the OpenRouter catalog or the models.dev catalog, and it does not appear in the Google Vertex AI, Gemini CLI, Cursor, or GitHub Copilot model docs. Azure AI Foundry and AWS Bedrock model pages could not be confirmed either way. When it does land for paid API customers, the call shape will be the standard Gemini one:

If you want to get the plumbing ready in the meantime, our Claude Opus 5.5 API tutorial walks through building an agentic investigator with read-only tools and programmatic tool calling, and the same patterns port across vendors with a model ID swap.

## What People Are Saying About Gemini 4 Argon

Almost nobody outside the Fairwind cohort has used Argon yet, so most reactions are first impressions of the announcement, not hands-on tests. Here's how the conversation has played out, from broad to specific.

Big picture / first impression thinking: people are impressed.

The first impressions can't be separated from the benchmark results. The performance on Vals Index and the legal and finance results got the most attention, with the asterisk that these are all Google's own evals.

But one reaction captures what this release is really about. Argon is built for work that runs for hours, like rewriting a kernel in Rust, and that could have real implications for the future of work.

## Final Thoughts

Google is betting that the next round of frontier competition is won on enterprise knowledge work and cyber defense rather than on chat quality, and Argon's Vals Index, AutomationBench, and GraphWalks numbers back that bet.

That being said, nobody outside the Fairwind cohort has yet reproduced a single score.

When it opens up to regular users, I would reach for Argon on long-context document and repository work and stay with Claude Opus 5.5 or GPT-6 Astra for terminal-driven agent loops, where Argon trails by 9 to 10 points.

If you want to build the agentic workflows these models are aimed at, start with our AI Fundamentals skill track.
