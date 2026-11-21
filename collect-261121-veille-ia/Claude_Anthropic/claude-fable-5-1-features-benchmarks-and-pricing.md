---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-1-features-benchmarks-and-pricing
title: "Claude Fable 5.1: Features, Benchmarks, and Pricing"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-08-02"]
keywords: ["claude", "fable", "anthropic", "model", "mythos", "pricing", "opus", "benchmarks"]
source: https://www.datacamp.com/blog/claude-fable-5-1
source_anchor: ""
source_lines: [1, 306]
section: "Claude_Anthropic"
---
OpenAI's GPT-5.6 Sol arrived to reviews saying it had drawn level with Anthropic. The evaluation firm Vals AI told the New York Times that Sol was roughly matching Claude Fable 5 on standard benchmarks. Anthropic's reply, in September 2026, is Claude Fable 5.1, which it calls the world's most advanced model for coding and knowledge work alongside its restricted twin, Claude Mythos 5.1.

Fable 5.1 keeps Fable 5's list price of $10 per million input tokens and $50 per million output tokens, and cuts cache reads by 75% to $0.25 per million. Anthropic puts the practical savings at roughly 25% for typical workloads billed by token, and up to about 45% for highly agentic ones. It runs a 1M token context window with 128K max output, and it more than doubles Fable 5's score on Terminal-Bench-Science 0.1.

In this article, I'll cover everything new with Claude Fable 5.1, looking at the new features, the benchmark results, and what it actually costs to run. To see the model in action, read our very thorough hands-on Claude Fable 5.1 API Tutorial. You can also see how it stacks up against the competition in our comparison of Claude Fable 5.1 and GPT-6 Astra.

## TL;DR

- Claude Fable 5.1 is Anthropic's generally available frontier model for long-horizon coding and research work.
- List pricing is unchanged from Fable 5, so the saving comes entirely from a much cheaper cached context.
- It now leads Claude Opus 5 on every benchmark Anthropic published, reversing rows where Opus 5 used to win.
- Reach for it on unattended agent runs, and stay on Sonnet 5 or Opus 5 for everyday chat and drafting.
- Claude Mythos 5.1 is the same model with looser cyber and biology safeguards, and it is invite-only.

## What Is Claude Fable 5.1?

Claude Fable 5.1 is Anthropic's generally available frontier model for demanding reasoning and long-horizon agentic work. It sits at the top of the Claude family on price, above Claude Opus 5 at $5 / $25 per million tokens and Claude Sonnet 5 at $2 / $10, and Anthropic's docs rate its latency as slower than both.

What you get for that premium is capability at Fable 5's list price, plus cache reads at a quarter of what they used to cost. Thinking is adaptive and always on, steered by an effort setting that defaults to `high` on the Claude API. The knowledge cutoff is June 2026.

The headline result is Terminal-Bench-Science 0.1, an agentic scientific research benchmark, where Fable 5.1 scores 52.6% against 24.7% for Fable 5. Anthropic pairs that with a story from the investment firm Millennium, whose senior portfolio manager described a roughly one-in-a-million crash that nobody on his team had explained in 4 to 5 years. Fable 5.1 disassembled an external vendor library, matched it against the core dump, and traced the crash to a bug in that library.

## What Is Claude Mythos 5.1?

Claude Mythos 5.1 is the same model as Fable 5.1 with more permissive safeguards, reachable only through Anthropic's trusted access programs. It is not a larger model or a different architecture, and Anthropic has not published separate pricing for it. The only difference is what the safeguards let it do.

Access currently reaches a set of US organizations, with wider enrollment planned. Two programs gate it, both built in partnership with the US government:

- The Cyber Verification Program covers defensive security work.
- The Life Sciences Verification Program covers professional research and development in the life sciences.

The gap between the twins shows up on coding, where Mythos 5.1 scores 60.9% on Terminal-Bench 4.0 against 55.8% for Fable 5.1. Decrypt attributed that difference to Fable's extra safeguards, routing some risky tasks to Claude Opus 4.8, as we've described in our Claude Fable 5 tutorial. Claude Security, Anthropic's product for scanning codebases and suggesting patches for human review, now runs on Mythos 5.1, and our guide to Claude Mythos 5 covers how the previous generation of this split worked.

## Claude Fable 5.1 Key Features

Five changes in Fable 5.1 alter what you can actually do with a Claude model, and one of them is a pricing decision rather than a capability.

### Leave a coding agent running overnight

You can hand Fable 5.1 a multi-hour job and come back to finished work rather than a stalled loop. Anthropic's early-access partners were unusually specific about this:

- Ron Sanzone at MongoDB described a **prototype built in about 3 days** , where the model first researched their services code and documentation, produced a design, then ran for hours unattended with verification loops.
- Dwight Temple at Ramp reported a **single unattended 38-hour run on a machine learning problem** that diagnosed an earlier result as a label artifact, corrected it, kicked off 6 parallel experiments overnight, and came back with a result and next steps.
- Ben Lafferty at Shopify put it plainly: **the model keeps its own records, reprioritizes as things change, and picks up where it left off** .

The 1M token context window and 128K max output are what make runs that long possible, and Jane Street's Craig Falls noted that Fable 5.1 stays readable across long multi-step tasks where earlier models became hard to follow.

Anthropic measured this model's agentic safety as Mythos 5.1, the same weights with looser safeguards, and reports it as the hardest model it has released to break on an external prompt injection benchmark. Its automated behavioral audit found that Mythos 5.1 both attempts and succeeds at reward hacking less often than Mythos 5, and is less likely to reach for resources outside its test environment when handed an impossible task.

Anthropic also names the gap in that work: the audit gives it less visibility into very long-context and multi-agent settings, which is precisely the territory a 38-hour run occupies.

### Keep a Fable-class agent affordable on cached context

Cache reads now cost $0.25 per million tokens, down 75%, which rewrites the arithmetic of any agent loop that re-reads the same context every turn. Anthropic estimates around 25% lower cost for typical workloads billed by token, and up to roughly 45% for complex coding and highly agentic tasks. If prompt caching is new to you, our guide to prompt caching covers the mechanics.

### Look for software vulnerabilities without constant refusals

Fable 5.1 can now be used to discover software vulnerabilities, though not to develop exploits for them. Anthropic says its newest cyber safeguards block 60% fewer false positives than before, and that Claude Code users should see an average of around 60% fewer interventions per session.

The limits have not gone away. Penetration testing, exploit generation, and binary-based vulnerability scanning still redirect to Anthropic's Opus models, and so does research and development work in the life sciences. On the biology side, the safeguards for Fable 5.1 and Fable 5 now fire 85% less often on benign elementary biology and medical questions, which addresses the false-positive problem rather than the capability restriction.

### Hand it a research problem and let it dig

Fable 5.1 trained a neural network that produced a new high-resolution elevation map covering a third of Venus, working from radar images taken by NASA's Magellan mission more than 30 years ago. The map resolves features down to 2 to 3 kilometers rather than 10 to 20, and its heights are up to 25% more accurate than before. Anthropic released it under a Creative Commons license on Zenodo, ahead of the NASA VERITAS and ESA EnVision missions.

Mythos 5.1 carried the life sciences results, which is worth keeping straight if you are reading the announcement quickly. It designed protein binders with affinities 10 times higher than the best designs submitted to Adaptyv Bio's protein design competitions on 3 targets, and hit a viable-binder rate of nearly 50% across 12 targets, where 10% to 15% is typical today. It also wrote custom GPU kernels that sped up 7 open-source deep learning models by up to 2.5 times with identical outputs, cutting estimated GPU costs on genome-wide analyses by 30% to 60%.

If you work in a lab, the access route matters as much as the capability. Anthropic's AI for Science program gives free credits to researchers on high-impact projects, and there is a steeply discounted Claude Team plan for scientists. The company has previewed a Model Hardware Standard that lets Claude operate laboratory equipment directly.

### Keep enterprise data in your own cloud

Enterprise Frontier Safeguards (EFS) stores customer data on the customer's own cloud infrastructure rather than Anthropic's, which gives the privacy of a zero data retention agreement while still allowing misuse detection. Human review is done by the customer by default. Anthropic built EFS with more than 100 customers and its cloud partners at Amazon Web Services, Google Cloud, and Microsoft Azure.

It arrives in phases starting this fall, covering Claude Code, Claude Enterprise, the Claude Platform, Amazon Bedrock, Google's Agent Platform, and Microsoft Foundry. Until it lands, eligible customers can run Fable 5.1 with zero data retention.

Fable 5.1 beats Claude Opus 5 on every benchmark Anthropic published, which VentureBeat noted reverses earlier cases where Opus 5 led the family. The gains over Fable 5 are largest on agentic science and terminal coding, and smallest on academic reasoning.

| Benchmark | Fable 5.1 | Fable 5 | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| Terminal-Bench-Science 0.1 | 52.6% | 24.7% | 29.0% | 22.4% | 
| Terminal-Bench 4.0 | 55.8% | 42.0% | 52.3% | 37.3% | 
| CursorBench 3.2.0 | 73.4% | 70.5% | 70.0% | 67.2% | 
| GDPval-AA v2 (Elo) | 1853 | 1723 | 1824 | 1711 | 
| AutomationBench | 31.4% | 17.1% | 26.9% | 19.6% | 
| Humanity's Last Exam (with tools) | 65.0% | 63.8% | 63.6% | Not published | 
| OSWorld 2.0 (strict) | 41.7% | 36.1% | 39.6% | Not published | 

Every figure here is Anthropic's own, measured with Fable 5.1's production safeguards switched on.

### Agentic scientific research and terminal coding

This is where the generational gap is impossible to miss. On Terminal-Bench-Science 0.1, which scores agentic scientific research carried out in a terminal, Fable 5.1 reaches 52.6% against 24.7% for Fable 5, 29.0% for Opus 5, and 22.4% for GPT-5.6 Sol.

Read the margin rather than the decimal. Anthropic reports a standard error of ±3.5 to 4.5 points per model on that benchmark, and notes that the public leaderboard puts Opus 5 at 30.0% and Fable 5 at 21.4%, both of which its own setup reproduces within noise.

Terminal-Bench 4.0, which tests multi-step command-line coding and debugging, gives Fable 5.1 55.8% against 42.0% for Fable 5 and 52.3% for Opus 5. On CursorBench 3.2.0, which scores coding inside an IDE workflow, it reaches 73.4%, a few points clear of Fable 5 at 70.5% and Opus 5 at 70.0%.

### Knowledge work and business workflows

Knowledge work improves, but not in the same order. On GDPval-AA v2, an Elo-style measure of knowledge work quality, Fable 5.1 scores 1853 against 1824 for Opus 5 and 1723 for Fable 5. Set beside Opus 5 that is a modest step; set beside Fable 5, it is a real one.

AutomationBench, covering business workflow automation, separates them much further: 31.4% for Fable 5.1, 26.9% for Opus 5, and 17.1% for Fable 5. Anthropic notes that Fable 5 scored zero on tasks where its production safeguards intervened, so treat that bottom figure as a floor.

### Reasoning and computer use

Academic reasoning is the flattest row on the board. Humanity's Last Exam puts Fable 5.1 at 60.9% without tools and 65.0% with them, against 57.8% and 63.8% for Fable 5, and 56.6% and 63.6% for Opus 5. Anthropic published no GPT-5.6 Sol score for this benchmark.

Computer use looks stronger and is harder to trust. Fable 5.1 leads OSWorld 2.0 with 77.9% on the partial setting and 41.7% on strict, against 75.4% and 39.6% for Opus 5. Anthropic scored these on the benchmark authors' August 2026 task release, so the numbers are not comparable to previously published OSWorld 2.0 results, and both Fable models took zeros on tasks where safeguards intervened.

One caveat runs under the whole table. Where safeguards blocked a task, Anthropic had cyber work completed by Claude Opus 4.8 and biology work by Claude Opus 5, which it says likely lowers the reported Fable scores rather than flattering them.

## Which Claude Tier Should You Use?

Fable 5.1 is the right default only when a job runs long enough that a wrong turn costs more than the tokens do. For everything shorter, Anthropic's own family undercuts it on price and beats it on latency, and Sonnet 5 is the sensible everyday pick rather than either flagship.

The variant system has two axes. Fable 5.1 and Mythos 5.1 are one model separated by safeguards, and across both sits an effort setting with Low, Medium, and High levels. Anthropic reports that Fable 5.1 at Low or Medium effort matches or beats Fable 5 at much lower cost, which makes the effort dial a cost control rather than a quality toggle.

The defaults differ by surface, which is worth knowing before you judge the model on a single session. Fable 5.1 defaults to High effort in Claude Code, Medium in Claude Cowork and on Claude.ai, and `high` on the Claude API.

| Use case | Pick | Why | 
|---|---|---|
| Multi-hour unattended agent runs | Fable 5.1 at High effort | Holds a plan across long jobs, and $0.25 cache reads keep the loop affordable | 
| Thorough repo planning and review | Opus 5 | The same 1M context window and adaptive thinking at half the price, $5 / $25, with moderate rather than slow latency | 
| Everyday coding and chat | Sonnet 5 | $2 / $10 for a 1M context window, and Anthropic rates it fast | 
| High-volume or latency-sensitive work | Haiku 4.5 | $1 / $5 and the fastest in the family, though it drops to a 200K context window and 64K max output | 
| Defensive vulnerability research and life sciences work | Mythos 5.1 | The only route past safeguards that redirect this work to Opus models | 
| Agent work you want on Fable but cannot afford at High | Fable 5.1 at Low or Medium effort | Anthropic reports results at or above Fable 5 for much less | 

## Claude Fable 5.1 Pricing and Availability

Fable 5.1 costs $10 per million input tokens and $50 per million output tokens, exactly what Fable 5 costs. Cache reads carry the whole price change, dropping 75% to $0.25 per million.

| Rate | Price per million tokens | 
|---|---|
| Input | $10 | 
| Output | $50 | 
| Cache read | $0.25 | 
| Cache write (5 minute) | $12.50 | 
| Cache write (1 hour) | $20 | 
| Batch API | 50% discount on input and output | 

Anthropic's estimate of the net effect is around 25% lower cost for typical workloads billed by token, and up to roughly 45% for complex coding and highly agentic tasks. Whether reasoning tokens bill at the output rate is not published, which matters more here than it would elsewhere, because thinking is always on.

Fable 5.1 is generally available today across platforms, including Amazon Web Services, Google Cloud, and Microsoft Azure. Mythos 5.1 is not: it goes to vetted cyberdefenders and life scientists through the CVP and LSVP, currently a set of US organizations, and Anthropic is coordinating with the US government to widen that.

Two smaller changes land on developers:

- New API accounts created from launch day onward can no longer edit Claude's prior context in a multi-turn conversation while preserving the transcript of its earlier thinking, an anti-distillation measure that will apply to all accounts with future releases.
- Under the EU AI Act's Code of Practice on Transparency of AI-Generated Content, Anthropic also watermarks output from models released after August 2, 2026, and a detection API is in private preview.

## How to Get Access to Claude Fable 5.1?

The model ID developers type is `claude-fable-5-1`, live on the Claude API from launch day. Beyond the first-party API, Fable 5.1 reaches Claude Code, Claude Cowork, Claude.ai, and the major clouds, with Amazon Bedrock, Google's Agent Platform, and Microsoft Foundry named as EFS-supported surfaces.

A minimal call looks like this:

```
from anthropic import Anthropic
client = Anthropic()
response = client.messages.create(
    model="claude-fable-5-1",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "Trace this crash to its root cause and explain the fix."}
    ],
)
print(response.content[0].text)
```

For the full setup, including cost controls and streaming, see our complete guide to the Claude API. Our Claude Fable 5 API tutorial builds a developer task assistant in Python and needs only the model ID swapped, and our Claude Code best practices piece covers the terminal workflow, where High effort is the default.

## Final Thoughts

Anthropic is betting that the frontier gets measured in unattended hours rather than chat turns, and it has priced Fable 5.1 to make that bet cheap. Holding list price while cutting cache reads by 75% is a more interesting move than any single benchmark row, because it targets the one workload where a $50 output rate is survivable.

I would switch off Fable 5 immediately and leave everyday work on Sonnet 5 or Opus 5. What gives me pause is the number of safeguard zeros scattered through Anthropic's own table, and the fact that the most striking science results came from Mythos 5.1, which almost nobody reading this can get.

If you want to put a Fable-class model to work in your own terminal, I recommend our Claude Code 101 course.

OpenAI's GPT-5.6 Sol arrived to reviews saying it had drawn level with Anthropic. The evaluation firm Vals AI told the New York Times that Sol was roughly matching Claude Fable 5 on standard benchmarks. Anthropic's reply, in September 2026, is Claude Fable 5.1, which it calls the world's most advanced model for coding and knowledge work alongside its restricted twin, Claude Mythos 5.1.

Fable 5.1 keeps Fable 5's list price of $10 per million input tokens and $50 per million output tokens, and cuts cache reads by 75% to $0.25 per million. Anthropic puts the practical savings at roughly 25% for typical workloads billed by token, and up to about 45% for highly agentic ones. It runs a 1M token context window with 128K max output, and it more than doubles Fable 5's score on Terminal-Bench-Science 0.1.

In this article, I'll cover everything new with Claude Fable 5.1, looking at the new features, the benchmark results, and what it actually costs to run. To see the model in action, read our very thorough hands-on Claude Fable 5.1 API Tutorial. You can also see how it stacks up against the competition in our comparison of Claude Fable 5.1 and GPT-6 Astra.

## TL;DR

## What Is Claude Fable 5.1?

Claude Fable 5.1 is Anthropic's generally available frontier model for demanding reasoning and long-horizon agentic work. It sits at the top of the Claude family on price, above Claude Opus 5 at $5 / $25 per million tokens and Claude Sonnet 5 at $2 / $10, and Anthropic's docs rate its latency as slower than both.

What you get for that premium is capability at Fable 5's list price, plus cache reads at a quarter of what they used to cost. Thinking is adaptive and always on, steered by an effort setting that defaults to

`high`on the Claude API. The knowledge cutoff is June 2026.
The headline result is Terminal-Bench-Science 0.1, an agentic scientific research benchmark, where Fable 5.1 scores 52.6% against 24.7% for Fable 5. Anthropic pairs that with a story from the investment firm Millennium, whose senior portfolio manager described a roughly one-in-a-million crash that nobody on his team had explained in 4 to 5 years. Fable 5.1 disassembled an external vendor library, matched it against the core dump, and traced the crash to a bug in that library.

## What Is Claude Mythos 5.1?

Claude Mythos 5.1 is the same model as Fable 5.1 with more permissive safeguards, reachable only through Anthropic's trusted access programs. It is not a larger model or a different architecture, and Anthropic has not published separate pricing for it. The only difference is what the safeguards let it do.

Access currently reaches a set of US organizations, with wider enrollment planned. Two programs gate it, both built in partnership with the US government:

The gap between the twins shows up on coding, where Mythos 5.1 scores 60.9% on Terminal-Bench 4.0 against 55.8% for Fable 5.1. Decrypt attributed that difference to Fable's extra safeguards, routing some risky tasks to Claude Opus 4.8, as we've described in our Claude Fable 5 tutorial. Claude Security, Anthropic's product for scanning codebases and suggesting patches for human review, now runs on Mythos 5.1, and our guide to Claude Mythos 5 covers how the previous generation of this split worked.

## Claude Fable 5.1 Key Features

Five changes in Fable 5.1 alter what you can actually do with a Claude model, and one of them is a pricing decision rather than a capability.

## Leave a coding agent running overnight

You can hand Fable 5.1 a multi-hour job and come back to finished work rather than a stalled loop. Anthropic's early-access partners were unusually specific about this:

prototype built in about 3 days, where the model first researched their services code and documentation, produced a design, then ran for hours unattended with verification loops.single unattended 38-hour run on a machine learning problemthat diagnosed an earlier result as a label artifact, corrected it, kicked off 6 parallel experiments overnight, and came back with a result and next steps.the model keeps its own records, reprioritizes as things change, and picks up where it left off.
The 1M token context window and 128K max output are what make runs that long possible, and Jane Street's Craig Falls noted that Fable 5.1 stays readable across long multi-step tasks where earlier models became hard to follow.

Anthropic measured this model's agentic safety as Mythos 5.1, the same weights with looser safeguards, and reports it as the hardest model it has released to break on an external prompt injection benchmark. Its automated behavioral audit found that Mythos 5.1 both attempts and succeeds at reward hacking less often than Mythos 5, and is less likely to reach for resources outside its test environment when handed an impossible task.

Anthropic also names the gap in that work: the audit gives it less visibility into very long-context and multi-agent settings, which is precisely the territory a 38-hour run occupies.

## Keep a Fable-class agent affordable on cached context

Cache reads now cost $0.25 per million tokens, down 75%, which rewrites the arithmetic of any agent loop that re-reads the same context every turn. Anthropic estimates around 25% lower cost for typical workloads billed by token, and up to roughly 45% for complex coding and highly agentic tasks. If prompt caching is new to you, our guide to prompt caching covers the mechanics.

## Look for software vulnerabilities without constant refusals

Fable 5.1 can now be used to discover software vulnerabilities, though not to develop exploits for them. Anthropic says its newest cyber safeguards block 60% fewer false positives than before, and that Claude Code users should see an average of around 60% fewer interventions per session.

The limits have not gone away. Penetration testing, exploit generation, and binary-based vulnerability scanning still redirect to Anthropic's Opus models, and so does research and development work in the life sciences. On the biology side, the safeguards for Fable 5.1 and Fable 5 now fire 85% less often on benign elementary biology and medical questions, which addresses the false-positive problem rather than the capability restriction.

## Hand it a research problem and let it dig

Fable 5.1 trained a neural network that produced a new high-resolution elevation map covering a third of Venus, working from radar images taken by NASA's Magellan mission more than 30 years ago. The map resolves features down to 2 to 3 kilometers rather than 10 to 20, and its heights are up to 25% more accurate than before. Anthropic released it under a Creative Commons license on Zenodo, ahead of the NASA VERITAS and ESA EnVision missions.

Mythos 5.1 carried the life sciences results, which is worth keeping straight if you are reading the announcement quickly. It designed protein binders with affinities 10 times higher than the best designs submitted to Adaptyv Bio's protein design competitions on 3 targets, and hit a viable-binder rate of nearly 50% across 12 targets, where 10% to 15% is typical today. It also wrote custom GPU kernels that sped up 7 open-source deep learning models by up to 2.5 times with identical outputs, cutting estimated GPU costs on genome-wide analyses by 30% to 60%.

If you work in a lab, the access route matters as much as the capability. Anthropic's AI for Science program gives free credits to researchers on high-impact projects, and there is a steeply discounted Claude Team plan for scientists. The company has previewed a Model Hardware Standard that lets Claude operate laboratory equipment directly.

## Keep enterprise data in your own cloud

Enterprise Frontier Safeguards (EFS) stores customer data on the customer's own cloud infrastructure rather than Anthropic's, which gives the privacy of a zero data retention agreement while still allowing misuse detection. Human review is done by the customer by default. Anthropic built EFS with more than 100 customers and its cloud partners at Amazon Web Services, Google Cloud, and Microsoft Azure.

It arrives in phases starting this fall, covering Claude Code, Claude Enterprise, the Claude Platform, Amazon Bedrock, Google's Agent Platform, and Microsoft Foundry. Until it lands, eligible customers can run Fable 5.1 with zero data retention.

## How Does Claude Fable 5.1 Perform on the Benchmarks?

Fable 5.1 beats Claude Opus 5 on every benchmark Anthropic published, which VentureBeat noted reverses earlier cases where Opus 5 led the family. The gains over Fable 5 are largest on agentic science and terminal coding, and smallest on academic reasoning.

Every figure here is Anthropic's own, measured with Fable 5.1's production safeguards switched on.

## Agentic scientific research and terminal coding

This is where the generational gap is impossible to miss. On Terminal-Bench-Science 0.1, which scores agentic scientific research carried out in a terminal, Fable 5.1 reaches 52.6% against 24.7% for Fable 5, 29.0% for Opus 5, and 22.4% for GPT-5.6 Sol.

Read the margin rather than the decimal. Anthropic reports a standard error of ±3.5 to 4.5 points per model on that benchmark, and notes that the public leaderboard puts Opus 5 at 30.0% and Fable 5 at 21.4%, both of which its own setup reproduces within noise.

Terminal-Bench 4.0, which tests multi-step command-line coding and debugging, gives Fable 5.1 55.8% against 42.0% for Fable 5 and 52.3% for Opus 5. On CursorBench 3.2.0, which scores coding inside an IDE workflow, it reaches 73.4%, a few points clear of Fable 5 at 70.5% and Opus 5 at 70.0%.

## Knowledge work and business workflows

Knowledge work improves, but not in the same order. On GDPval-AA v2, an Elo-style measure of knowledge work quality, Fable 5.1 scores 1853 against 1824 for Opus 5 and 1723 for Fable 5. Set beside Opus 5 that is a modest step; set beside Fable 5, it is a real one.

AutomationBench, covering business workflow automation, separates them much further: 31.4% for Fable 5.1, 26.9% for Opus 5, and 17.1% for Fable 5. Anthropic notes that Fable 5 scored zero on tasks where its production safeguards intervened, so treat that bottom figure as a floor.

## Reasoning and computer use

Academic reasoning is the flattest row on the board. Humanity's Last Exam puts Fable 5.1 at 60.9% without tools and 65.0% with them, against 57.8% and 63.8% for Fable 5, and 56.6% and 63.6% for Opus 5. Anthropic published no GPT-5.6 Sol score for this benchmark.

Computer use looks stronger and is harder to trust. Fable 5.1 leads OSWorld 2.0 with 77.9% on the partial setting and 41.7% on strict, against 75.4% and 39.6% for Opus 5. Anthropic scored these on the benchmark authors' August 2026 task release, so the numbers are not comparable to previously published OSWorld 2.0 results, and both Fable models took zeros on tasks where safeguards intervened.

One caveat runs under the whole table. Where safeguards blocked a task, Anthropic had cyber work completed by Claude Opus 4.8 and biology work by Claude Opus 5, which it says likely lowers the reported Fable scores rather than flattering them.

## Which Claude Tier Should You Use?

Fable 5.1 is the right default only when a job runs long enough that a wrong turn costs more than the tokens do. For everything shorter, Anthropic's own family undercuts it on price and beats it on latency, and Sonnet 5 is the sensible everyday pick rather than either flagship.

The variant system has two axes. Fable 5.1 and Mythos 5.1 are one model separated by safeguards, and across both sits an effort setting with Low, Medium, and High levels. Anthropic reports that Fable 5.1 at Low or Medium effort matches or beats Fable 5 at much lower cost, which makes the effort dial a cost control rather than a quality toggle.

The defaults differ by surface, which is worth knowing before you judge the model on a single session. Fable 5.1 defaults to High effort in Claude Code, Medium in Claude Cowork and on Claude.ai, and

`high`on the Claude API.
## Claude Fable 5.1 Pricing and Availability

Fable 5.1 costs $10 per million input tokens and $50 per million output tokens, exactly what Fable 5 costs. Cache reads carry the whole price change, dropping 75% to $0.25 per million.

Anthropic's estimate of the net effect is around 25% lower cost for typical workloads billed by token, and up to roughly 45% for complex coding and highly agentic tasks. Whether reasoning tokens bill at the output rate is not published, which matters more here than it would elsewhere, because thinking is always on.

Fable 5.1 is generally available today across platforms, including Amazon Web Services, Google Cloud, and Microsoft Azure. Mythos 5.1 is not: it goes to vetted cyberdefenders and life scientists through the CVP and LSVP, currently a set of US organizations, and Anthropic is coordinating with the US government to widen that.

Two smaller changes land on developers:

## How to Get Access to Claude Fable 5.1?

The model ID developers type is

`claude-fable-5-1`, live on the Claude API from launch day. Beyond the first-party API, Fable 5.1 reaches Claude Code, Claude Cowork, Claude.ai, and the major clouds, with Amazon Bedrock, Google's Agent Platform, and Microsoft Foundry named as EFS-supported surfaces.
A minimal call looks like this:

For the full setup, including cost controls and streaming, see our complete guide to the Claude API. Our Claude Fable 5 API tutorial builds a developer task assistant in Python and needs only the model ID swapped, and our Claude Code best practices piece covers the terminal workflow, where High effort is the default.

## Final Thoughts

Anthropic is betting that the frontier gets measured in unattended hours rather than chat turns, and it has priced Fable 5.1 to make that bet cheap. Holding list price while cutting cache reads by 75% is a more interesting move than any single benchmark row, because it targets the one workload where a $50 output rate is survivable.

I would switch off Fable 5 immediately and leave everyday work on Sonnet 5 or Opus 5. What gives me pause is the number of safeguard zeros scattered through Anthropic's own table, and the fact that the most striking science results came from Mythos 5.1, which almost nobody reading this can get.

If you want to put a Fable-class model to work in your own terminal, I recommend our Claude Code 101 course.
