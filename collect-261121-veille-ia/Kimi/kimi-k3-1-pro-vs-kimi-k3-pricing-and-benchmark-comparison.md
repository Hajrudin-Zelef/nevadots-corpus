---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-vs-kimi-k3-pricing-and-benchmark-comparison
title: "Kimi K3.1 Pro vs Kimi K3: Pricing and Benchmark Comparison"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-04"]
keywords: ["kimi", "tokens", "input", "moonshot", "output", "model", "million", "pricing"]
source: https://www.layer3labs.io/comparisons/kimi-k3-1-pro-vs-kimi-k3
source_anchor: ""
source_lines: [1, 80]
section: "Kimi"
---
# Kimi K3.1 Pro vs Kimi K3: Model Comparison

How the updated post-trained model compares on API pricing, coding benchmarks, output speed, and weights release.

Kimi K3.1 Pro is worth the switch if your cost per finished task drops in your own test. Run that test first. Artificial Analysis rates the model very verbose. Its per-token rates are lower ($2.00/$10.00 against $3.00/$15.00), and Moonshot's own coding benchmark scores are higher.

At Layer3Labs, we build and run AI systems inside other people's businesses, so the number we weigh is cost per finished task, not price per token.

Moonshot AI launched K3.1 Pro as a post-trained iteration of K3 with higher post-training compute and reinforcement learning across hundreds of downstream domains, while keeping the shared 1-million-token context window.

## Kimi K3.1 Pro vs. Kimi K3: Side-by-Side

| Dimension | Kimi K3.1 Pro | Kimi K3 | 
|---|---|---|
| Release | Announced as the first model in the K3.1 series by Moonshot AI. | Not on the vendor page we checked. | 
| Context window | 1,048,576 tokens (1 million tokens). | 1,048,576 tokens (1 million tokens). | 
| API input price | $2.00 per 1M tokens. | $3.00 per 1M tokens. | 
| API output price | $10.00 per 1M tokens. | $15.00 per 1M tokens. | 
| Cached input price | $0.20 per 1M tokens. | $0.30 per 1M tokens. | 
| Kimi Code Bench 3.0 Long-horizon | 61.2% (Moonshot internal benchmark). | 55.95% (Moonshot internal benchmark). | 
| Hallucination (Hallu Bench Hard, stress test) | 8.3% claim-level hallucination rate (Moonshot internal stress test). | 23.4% claim-level hallucination rate (Moonshot internal stress test). | 
| Open weights status | Open weights promised by Nov 4, 2026; labeled proprietary on independent index. | Not on the vendor page we checked. | 
| Speed variant | Kimi K3.1 Pro Highspeed available at $4.00 input and $20.00 output. | Not on the vendor page we checked. | 

Are you one of these vendors? Update your listing

## Kimi K3.1 Pro vs Kimi K3 at a Glance

Kimi K3.1 Pro delivers lower nominal application programming interface (API) rates and improved vendor benchmark scores over Kimi K3 while retaining the same 1,048,576-token context window. Moonshot AI positions K3.1 Pro as its most capable 3T-class open-weight model to date, having applied significantly more post-training compute and joint reinforcement learning (RL) across hundreds of downstream domains.

On nominal pricing, K3.1 Pro cuts API costs by 33 percent across input, output, and cached input tokens compared to K3. Moonshot also offers a separate Kimi K3.1 Pro Highspeed model, which claims 2 to 3 times the generation speed for $4.00 input and $20.00 output per million tokens.

Independent evaluation introduces an operational caveat for teams comparing both models. Artificial Analysis ranks K3.1 Pro fourth out of 215 models on its Intelligence Index with a score of 55, but lists an output speed of 57.5 tokens per second (ranked 141 of 186). It also calls the model very verbose: 150 million output tokens against an 87 million median. The Kimi K3.1 Pro overview and Kimi K3 guide cover each model alone.

## Price and Cost Comparison

API pricing listed on the Kimi pricing documentation makes Kimi K3.1 Pro 33 percent cheaper per token than Kimi K3. Standard K3.1 Pro costs $2.00 per million input tokens and $10.00 per million output tokens, compared to $3.00 input and $15.00 output for K3. Cached input on K3.1 Pro bills at $0.20 per million tokens, representing a 90 percent discount off fresh input, whereas K3 cached input bills at $0.30.

Context caching works automatically across repeated request prefixes with a default time to live (TTL) of 5 minutes. Cache writes are billed separately based on the chosen TTL of 5 minutes or 1 hour, and subsequent cache hits consume the lower cached rate. In addition, Moonshot provides file extraction and file storage APIs that are temporarily free of charge.

Lower per-token rates do not automatically guarantee lower invoice totals. Because Artificial Analysis observed that K3.1 Pro used 150 million output tokens to complete its index run (at an estimated cost of $2.11 per task), higher token verbosity can erode the 33 percent per-token savings. Teams evaluating migration should calculate full task expenditures as detailed on our Kimi K3.1 Pro pricing breakdown.

## Quality Claims and Their Limits

Moonshot's launch announcement reports gains for K3.1 Pro over K3 on its own tests. On Kimi Code Bench 3.0 Long-horizon, Moonshot's internal benchmark, K3.1 Pro scored 61.2 percent against 55.95 percent for K3.

Moonshot reports a lower hallucination rate on its own stress test. On Kimi Hallu Bench Hard, an internal stress test measuring claim-level hallucination rather than general accuracy, Moonshot reports K3.1 Pro produced an 8.3 percent hallucination rate, compared to 23.4 percent for K3. Moonshot also reports finance gains over K3. Financial Modeling is up 7.2 points, Financial Research is up 11.6 points, and Business Operation is about 4.2x.

Outside views are mixed. A ModelCompare headline says Kimi K3.1 Pro puts open-source AI a month behind the best. Community forum posts say it does not look much better than DeepSeek 4.1 Flash and seems slightly worse than GPT 6.1 Sol at certain tasks. Those posts are opinion. Artificial Analysis labels K3.1 Pro 'Proprietary model', while Moonshot calls it open-weight with weights due by Nov 4, 2026. The weights are a promise, not a release.

## Who Should Switch

If you use Kimi K3 for coding agents, Moonshot's own results favor K3.1 Pro, so run a pilot on your tasks. Moonshot says K3.1 Pro can write and run Blender and CAD scripts without MCP connectors and works across Unity, Unreal Engine and Godot. In one internal test it ran 167 missions and 420 independent reviews to build a human walking simulator.

If output speed matters most, test Kimi K3.1 Pro Highspeed. The sources checked give no K3 speed figure, so compare K3 on your own prompts. Standard K3.1 Pro runs at 57.5 tokens per second on independent benchmarks, ranking 141 of 186 models tested. Moonshot says Highspeed is 2 to 3 times faster, but it doubles input to $4.00 and output to $20.00 per million tokens.

Enterprises with strict security and data governance standards must review access controls prior to deployment. Moonshot removed cyber-related data from mid-training and reinforcement learning for the open-weight release, reducing performance on its internal xdev-bench-hard test from 90.5 percent to 81.0 percent. Kimi Enterprise administrators must explicitly enable K3.1 Pro because organization access is toggled off by default, while Zero Data Retention is available for API customers.

## The Verdict

Choose Kimi K3.1 Pro over Kimi K3 for coding agents if your own test shows a lower cost per finished task. K3.1 Pro lowers token costs by 33 percent. On Moonshot's own tests it cuts claim-level hallucination on a stress test from 23.4 percent to 8.3 percent and raises long-horizon coding from 55.95 to 61.2 percent.

Do not assume the 33 percent token price reduction cuts your total invoice by one third. Because independent testing shows K3.1 Pro generates substantial token volumes, your effective cost per completed task depends on output verbosity and reasoning effort settings.

Upgrade to K3.1 Pro if your measured cost per finished task drops during a live test against your standard workload. Our recommendation would change if Moonshot fails to deliver the promised open weights by Nov 4, 2026, or if task verbosity consistently outweighs the lower token rate in production.

Researched from primary vendor documentation and public regulator sources. Pricing and availability are accurate as of Oct 25, 2026 and can change. Confirm current terms with each vendor before you buy.

## Frequently Asked Questions

- Yes, on a per-token basis Kimi K3.1 Pro is 33 percent cheaper than K3. K3.1 Pro charges $2.00 per million input tokens and $10.00 per million output tokens, compared to $3.00 and $15.00 on K3.
- Kimi K3.1 Pro supports a context window of 1,048,576 tokens (1 million tokens), which matches the capacity of Kimi K3.
- Not yet. Moonshot AI says full weights are to be released by Nov 4, 2026, and they are not out at the time of writing. Artificial Analysis labels the model 'Proprietary model'. The license terms are not in the sources checked, so verify them in the Moonshot AI launch post.
- Independent testing from Artificial Analysis recorded an output speed of 57.5 tokens per second for standard K3.1 Pro. Moonshot offers Kimi K3.1 Pro Highspeed, which claims 2 to 3 times faster output at twice the token cost.

### Want Help Putting a Model Into Your Workflow?

At Layer3Labs, we build custom agents, enterprise integrations, and advisory engagements for business teams. Book a workflow audit to map where a model like Kimi K3.1 Pro fits.

Book a Free Audit
