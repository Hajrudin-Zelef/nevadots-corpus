---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-benchmarks-pricing-and-what-it-means-for-developers
title: "Gemini 4 Carbon: Benchmarks, Pricing and What It Means for Developers"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["carbon", "google", "gemini", "argon", "developers", "about", "intelligence", "index"]
source: https://dev.to/zhang_tengfei_c85cbcb9e56/gemini-4-carbon-benchmarks-pricing-and-what-it-means-for-developers-563e
source_anchor: ""
source_lines: [1, 108]
section: "Gemini_Google"
---
Google announced Gemini 4 Carbon on October 28, 2026, about two weeks after Gemini 4 Argon reached developers. Carbon is the new top model in the Gemini 4 family, built for long engineering tasks and cybersecurity. It scores 59 on the Artificial Analysis Intelligence Index v5.0, the highest on the index, and Google says internal teams have run it for days on a single multi-step job.

Most developers can't use it yet, though. Google is giving it to cyber defenders first and plans to open it to developers later. Here's what the numbers say and what to plan for.

- Released October 28, 2026, as the top model in Google's Gemini 4 family, above Gemini 4 Argon
- #1 on the Artificial Analysis Intelligence Index v5.0, with a score of 59
- Security: 88.2% on SEC-bench Pro (finding vulnerabilities), 54.8% on ExploitGym (validating exploits), 71.0% on CWE-bench (patching)
- WebDev Arena: 1769 Elo, up 89 points, fourth place
- Introductory price: $2 input and $10 output per million tokens, with cached input at 95% off. The standard price after that is $4 / $20.
- Access: cyber defenders first, then trusted partners, then developers and businesses, with no date given

Google compared Carbon with Argon and with the strongest models from OpenAI and Anthropic:

| Benchmark | Gemini 4 Carbon | Gemini 4 Argon | GPT-6 Astra | Claude Opus 5.5 | 
|---|---|---|---|---|
| AA Intelligence Index v5.0 | 59 | 50 | 57 | 58 | 
| SEC-bench Pro | 88.2% | 83.4% | 85.4% | n/a | 
| ExploitGym | 54.8% | 42.0% | 42.4% | 34.5% | 
| CWE-bench | 71.0% | 68.0% | 68.0% | 67.0% | 
| FrontierSWE 2 | 61.2% | 55.0% | 65.5% | 62.3% | 
| OSWorld-2.1 (offline subset) | 81.5% | 74.7% | 80.9% | 87.2% | 
| GraphWalks 256K–1M | 96.2% | 84.2% | 71.8% | 66.8% | 

Carbon beats Argon on every row and leads on general intelligence and all three security benchmarks. It doesn't lead everywhere. GPT-6 Astra is ahead on FrontierSWE 2, a software engineering benchmark, and Claude Opus 5.5 is well ahead on OSWorld-2.1, which tests computer use. Google ran Carbon and Argon itself, while most competitor numbers are self-reported or taken from public leaderboards, so treat the gaps as approximate.

The GraphWalks result stands out. It tests whether a model can follow connections through 256K to 1M tokens of context. Carbon scores 96.2%, about 30 points ahead of Opus 5.5. For agents that read whole codebases or long logs, that matters more than a couple of points on a coding benchmark.

Google says internal teams have run Carbon for days on multi-step engineering jobs, such as tracking down the root cause of a bug in a large codebase and patching it. Google hasn't published logs or success rates for these runs.

The more measurable claims are about behavior. Compared with Argon, Google reports that Carbon:

- makes about half as many unsupported claims
- oversteps the scope of a task 26% less often, and stops short of finishing it 29% less often
- triggers almost 80% fewer high-severity misalignment alerts in live coding environments
- checks sources and points out discrepancies when instructions are ambiguous, instead of guessing
- holds back a blocked action and lists what's still needed, instead of making something up

On Terminal-Bench 4.0, its rate of successful cheating fell to 0.0%. On Gray Swan's indirect prompt injection benchmark, attacks succeeded 0.0% of the time, compared with 0.7% for Argon. These are Google's own numbers under its own test conditions. They don't mean an agent built on Carbon is immune to prompt injection in your environment.

Security is where Carbon is pitched hardest. Google tested it on three separate steps of defensive security work:

- 
**Discovery (SEC-bench Pro, 88.2%):** finding real vulnerabilities in real software
- 
**Validation (ExploitGym, 54.8%):** checking whether a vulnerability can actually be exploited
- 
**Remediation (CWE-bench, 71.0%):** writing a fix that removes the root cause without breaking anything else

They're different problems. A scanner that flags 100 possible issues isn't much help if a team can't tell which three are exploitable. Carbon's biggest jump over Argon is on validation, from 42.0% to 54.8%, and that's the step that tells a security team what to fix first. Google also reports a strong improvement in secure code generation over Argon on internal benchmarks.

That's also why access starts with defenders. A model that's good at validating exploits is useful to attackers too.

| Per 1M tokens | Introductory | Standard | 
|---|---|---|
| Input | $2.00 | $4.00 | 
| Cached input | 95% off input | n/a | 
| Output | $10.00 | $20.00 | 

The introductory price matches Argon's, which runs through November 30, 2026. Google hasn't said how long Carbon's introductory period lasts, and batch, flex and priority prices aren't announced yet.

The 95% cache discount matters most for agents. A long-running agent sends the same codebase, instructions and history on every step, so most of its input tokens are cache hits. At standard prices, a task that runs for hours and produces a lot of output will cost much more than the per-token price suggests.

Access comes in three stages:

1. A subset of cyber defenders, starting the week of launch
2. Trusted partners through Google's Fairwind Program, which prioritizes governments, critical infrastructure operators and core technology platforms
3. Developers, businesses and consumers, "as soon as possible", with no date

As of October 30, Carbon isn't in the Gemini API changelog or on the pricing page, and Google hasn't published a model ID or context window.

Once it's available, the use cases follow the benchmarks:

- 
**Backend engineers:** tracing an API failure through request handling, database calls, permission checks and other services
- 
**Frontend and full-stack developers:** building complete interfaces and fixing problems where frontend and backend meet, where its WebDev Arena gain applies
- 
**Agent developers:** long multi-step jobs that need planning, tool calls, recovery from errors and checking the result, where the long-context and behavior results matter most
- 
**Security engineers:** reviewing code, ranking vulnerabilities by real risk and proposing fixes, with human review before anything ships

Carbon shows where frontier models are competing now. A year ago the question was how well a model answers or writes code. Carbon's launch focuses on how long it can work without supervision, how rarely it goes off-scope, and whether it admits what it can't do. Those are the qualities that decide whether an agent can run for days.

It also shows that the price of autonomy is rising. Carbon's standard price is double Argon's introductory price, and long tasks multiply output tokens. For most agent workloads, the practical setup is a mix: a frontier model for the hardest planning and security steps, and cheaper models for the many routine calls in between.

While Carbon's rollout is limited, you can build and test agent workflows today on A2Agent. DeepSeek, GLM, MiniMax, Kimi, Qwen and other models sit behind one API key, with OpenAI-compatible and Anthropic-compatible endpoints, so you can route routine steps to a cheap model and compare cost per finished task across vendors.


```
from openai import OpenAI
client = OpenAI(
    api_key="YOUR_API_KEY",
    base_url="https://api.a2agent.me/v1",
)
response = client.chat.completions.create(
    model="deepseek-v4.1-flash-1017",
    messages=[{"role": "user", "content": "Review this function for security issues: ..."}],
)
print(response.choices[0].message.content)
```
See the Models page for current prices. New accounts get $5 in free credit.

- Gemini 4 Carbon: our next frontier model (Google)
- Google announces Gemini 4 Carbon with 'significant' coding gains (9to5Google)
- Gemini 4 Carbon: Google's new frontier model, benchmarks, pricing and access (Data Studios)
- Artificial Analysis Intelligence Index (Artificial Analysis)

## Top comments (0)
