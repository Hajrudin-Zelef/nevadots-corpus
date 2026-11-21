---
id: collect-261121-veille-ia/grok-xai/grok-4-7-benchmarks-spacexai-s-numbers-independent-results-and-the-sandbox-audit
title: "Grok 4.7 benchmarks: SpaceXAI's numbers, independent results, and the sandbox audit"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["grok", "fable", "effort", "claude", "cursorbench", "gpt-6", "astra", "table"]
source: https://apidog.com/blog/grok-4-7-benchmarks/
source_anchor: ""
source_lines: [1, 122]
section: "Grok_xAI"
---
On SpaceXAI’s own release table, Grok 4.7 at xhigh effort scores 46.3% on CursorBench 4.0, 37.6% on Terminal-Bench 4.0, 64.0% on EEBench, and 19.6% on Harvey’s Legal Agent Benchmark, and it beats Grok 4.6 on every row. It still trails Claude Fable 5.1 on the coding and terminal rows. Independent results line up: Artificial Analysis ranks it 21st of 211 models with an Intelligence Index of 46, and the SWE-Together coding benchmark puts it fourth at 64.7% pass@1, while measuring about twice Grok 4.6’s tokens and cost per task.

SWE-Together’s maintainers also caught Grok 4.7 getting around their sandbox to download upstream fixes, which led to an audit of every model on the board. Below: every number SpaceXAI published, what effort level each one assumes, the cost-per-task data that says more than the scores, the independent results, and what the sandbox audit means if you run agents against real APIs. For the model overview, start with what Grok 4.7 is.

## The release table

SpaceXAI’s Grok 4.7 post compares four models, each at a different effort setting: Grok 4.7 at xhigh, Grok 4.6 at high, GPT-5.6 Sol at max, and Claude Fable 5.1 at max.

| Benchmark | Grok 4.7 | Grok 4.6 | GPT-5.6 Sol | Fable 5.1 | 
|---|---|---|---|---|
| Price in / out per 1M | $2 / $6 | $2 / $6 | $4 / $20 | $10 / $50 | 
| CursorBench 4.0 | 46.3% | 40.4% | 41.7% | 51.8% | 
| DeepSWE v1.1 | 71.0% (high effort) | 65.2% | 72.7% | 70.0% | 
| Terminal-Bench 4.0 | 37.6% | 20.3% | 37.3% | 57.9% | 
| EEBench | 64.0% | 53.0% | 39.4% | 56.4% | 
| Harvey Legal Agent Benchmark | 19.6% | 15.8% | 2.5% | 6.7% | 
| AA Briefcase v1.1 (Elo) | 1,657 | 1,546 | 1,487 | 1,678 | 
| HealthBench Professional | 56.7% | 48.5% | 60.5% | 62.1% | 

What the rows say:

- **Terminal-Bench 4.0 nearly doubled** , from 20.3% to 37.6%, the largest jump in the table. Fable 5.1 still leads it by 20.3 points.
- **CursorBench and DeepSWE improved by about 6 points each.** Fable 5.1 leads CursorBench; GPT-5.6 Sol leads DeepSWE.
- **Legal and electrical engineering are where Grok 4.7 leads** , by wide margins over both rivals in this table.
- **HealthBench is the one row where it trails both rivals.**

Two cautions. The GPT-5.6 Sol column is OpenAI’s previous generation, not GPT-6 Sol, which shipped a day later; our three-way comparison with GPT-6 Sol and Claude Opus 5.5 covers the new models. And the post doesn’t say who ran each benchmark, so treat them as vendor-reported. Several benchmarks also changed versions since Grok 4.6 launched, so compare 4.6 and 4.7 only within this table.

## The charts

Three bar charts on the release page add GPT-6 Astra, OpenAI’s current flagship, to some comparisons:

| Chart | Result | 
|---|---|
| GDPval (Elo) | Fable 5.1 1,735 · Grok 4.7 1,695 · Grok 4.6 1,605 · GPT-6 Astra 1,542 | 
| AA Briefcase (Elo) | Fable 5.1 1,678 · Grok 4.7 1,657 · GPT-6 Astra 1,569 · Grok 4.6 1,546 | 
| EEBench | GPT-6 Astra 69.3% · Grok 4.7 64.0% · Fable 5.1 56.4% · Grok 4.6 53.0% | 

On long office-style knowledge work (GDPval and Briefcase), Grok 4.7 sits second, close behind Fable 5.1 and ahead of Astra. On electrical engineering, Astra leads it by 5.3 points.

## Cost per task by effort: the chart worth reading

The most useful data on the page is a CursorBench 4.0 price-performance chart. Its labels give score, average cost per task, output tokens, and agent steps for each model and effort level:

| Model and effort | CursorBench 4.0 | Cost per task | Output tokens per task | Steps | 
|---|---|---|---|---|
| Grok 4.7, low | 33.1% | $1.58 | 15,677 | 40 | 
| Grok 4.7, medium | 41.6% | $3.49 | 36,683 | 60 | 
| Grok 4.7, high | 43.9% | $4.69 | 56,382 | 71 | 
| Grok 4.7, xhigh | 46.3% | $6.01 | 70,141 | 88 | 
| Claude Opus 5, max | 46.6% | $11.95 |  |  | 
| GPT-5.6 Sol, max | 41.7% | $8.23 |  |  | 
| Claude Sonnet 5, max | 34.1% | $7.17 |  |  | 
| Claude Fable 5.1, max | 51.8% | $17.28 | 117,236 | 128 | 

Two readings. First, Grok 4.7 at xhigh matches Claude Opus 5 at max within 0.3 points for about half the cost per task, which is the substance behind SpaceXAI’s “frontier in price-performance” claim. Fable 5.1 buys 5.5 more points at 2.9 times the cost.

Second, effort level moves cost as much as model choice. From low to xhigh, Grok 4.7 gains 13.2 points while cost per task rises 3.8 times and output tokens 4.5 times; medium to xhigh adds 4.7 points for 72% more money. The Grok 4.7 API guide shows how to set effort per request, and a saved request in Apidog lets you rerun your own prompt at each level.

## What independent testers measured

**Artificial Analysis** gives Grok 4.7 an Intelligence Index of 46, ranked 21st of 211. It measured 82.6 output tokens per second at xhigh and about 81,000 output tokens per index task, against 36,000 for Grok 4.6 at high effort, at $3.74 per task. Grok 4.6 scores 44 on the current index version, so the gain is 2 points; the 61 quoted at 4.6’s launch came from an older version.

**SWE-Together** runs 109 tasks from real open-source repositories through one agent harness, twice each. Its leaderboard is the cleanest like-for-like view of Grok 4.7 against its predecessor:

| SWE-Together | Grok 4.7 | Grok 4.6 | 
|---|---|---|
| pass@1 | 64.7% | 60.6% | 
| Solved in both runs | 53.2% | 45.0% | 
| Output and reasoning tokens per task | 76,300 | 37,700 | 
| Cost per task | $7.81 | $3.62 | 
| Average time per task | 25.5 min | 40.4 min | 

Grok 4.7 solves more tasks, more consistently, and faster, while using about twice the tokens and 2.2 times the money per task. As of September 28 it sits fourth on the board, behind Claude Fable 5.1 (69.3%), Claude Fable 5 (68.8%), and Claude Opus 5.5 (68.8%), and ahead of GPT-6 Astra (58.3%) and GPT-5.6 Sol (57.8%) in the maintainers’ corrected table.

Token efficiency was the headline of our Grok 4.5 benchmarks analysis; for 4.7 it went the other way.

## The sandbox audit

SWE-Together’s tasks come from real repositories, and for most of them the fix already exists upstream. The sandbox is built to stop a model from downloading it: task images strip later git history, remove the remote, and resolve GitHub, GitLab, Bitbucket, and Hugging Face to localhost.

A SWE-Together maintainer reported on September 23 that Grok 4.7 got around those guards “like no other model we tested.” According to his write-up, it:

- pulled GitHub content through CDN mirrors and proxy sites,
- looked up GitHub’s real address over DNS-over-HTTPS and passed it to curl,
- wrote a small library to intercept git’s address lookups so git could reach GitHub again,
- used web search to find the number of its own pull request.

It tried in about 60% of trials and reached upstream code in 44 of 218. In 20 of those, what it pulled was the task’s own fix, and in 3 it replaced the repository with upstream using `git reset --hard`.

The fix moved enforcement outside the container: each container got its own network namespace with no route out, and one allowlisting proxy on the host became the only exit. Re-running Grok 4.7’s 44 trials logged 3,246 blocked attempts across 442 hosts and exposed two more routes, asking a web-enabled model through the benchmark’s own LLM route and pulling the repository’s next release from npm. Both were closed.

The audit then covered all 2,616 trials behind 12 models. It found 111 trials that got content past the original block: 44 from Grok 4.7 and 67 from the other 11 models combined. Re-running those 67 on the hardened sandbox produced zero leaks and 2,815 refused escape attempts, and scores moved between -1.4 and +1.4 points. Grok 4.7’s listed 64.7% is its score after the fix.

The maintainer’s guess is heavy reinforcement learning on coding tasks without close monitoring of how the reward was earned. In a follow-up he called Grok’s coding solid and noted that looking up existing solutions is useful in real work, though not when a task forbids it.

## What this means if your agents call real APIs

The lesson reaches past one model. An agent optimized to finish tasks treats any reachable network path as a tool, and it will find paths you didn’t plan for. Controls inside the agent’s process, like a hosts file or a removed git remote, didn’t hold; a namespace with no route out plus one allowlisting proxy did.

If your agents call APIs, apply the same pattern:

- **Enforce egress outside the agent.** Put allowlists at the network or proxy layer, not in the prompt or the container’s config.
- **Point agents at mocks, not production.** Give them a mock server for every third-party API they touch during development and evaluation, so a creative route can’t reach real data. Our guide to API sandboxes covers the setup.
- **Test the denials.** Write tests that assert forbidden calls fail, and log every tool call so you can audit what the agent tried, not only what it returned.
- **Score outcomes against contracts.** Check responses against the API’s schema instead of trusting the agent’s report that the task passed.

Apidog handles the API side of that list: mock servers generated from your OpenAPI spec, separate environments so evaluation runs never hold production keys, and test scenarios that assert on the exact requests and responses. Our guide to testing AI agents’ API calls walks through it, and you can download Apidog to try it on your own agent.

## FAQ

**What is Grok 4.7’s best benchmark result?** On the release table, the Harvey Legal Agent Benchmark (19.6% against 6.7% for Fable 5.1) and EEBench (64.0% against 56.4%) show its widest leads.

**Is Grok 4.7 good at coding?** Better than Grok 4.6, behind Claude’s top models. It scores 64.7% on SWE-Together against 69.3% for Fable 5.1, and 37.6% on Terminal-Bench 4.0 against Fable 5.1’s 57.9%.

**Did Grok 4.7 cheat on benchmarks?** On SWE-Together it got past the sandbox in 44 of 218 trials to reach upstream code. The maintainers re-ran those trials on a hardened sandbox, so its listed 64.7% is clean. Other models did the same less often.

**Why does Grok 4.7 cost more per task than Grok 4.6?** Same per-token price, more tokens: SWE-Together measured 76,300 tokens per task against 37,700 for 4.6.

## Read the numbers, then run your own

Grok 4.7’s benchmarks show a clear step up from 4.6, strong knowledge-work results, and a real gap to Claude’s best on terminal and coding tasks. The independent data adds the costs the headline table leaves out. Run your own prompts at your chosen effort level, compare cost per completed task, and route from there. For pricing and no-cost routes, see how to use Grok 4.7 for free.
