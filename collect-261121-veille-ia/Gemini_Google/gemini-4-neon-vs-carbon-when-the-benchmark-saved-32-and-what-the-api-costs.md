---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-vs-carbon-when-the-benchmark-saved-32-and-what-the-api-costs
title: "Gemini 4 Neon vs. Carbon: When the Benchmark Saved 32%, and What the API Costs"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["neon", "tokens", "carbon", "thinking", "output", "cost", "same", "token"]
source: https://dev.to/uulab/gemini-4-neon-the-balance-point-between-carbon-and-hydrogen-benchmarks-api-pricing-and-4pa6#comments
source_anchor: ""
source_lines: [1, 328]
section: "Gemini_Google"
---
*ENGINEERING · 2026. 11. 19*

Will a move from Carbon to Neon lower the cost of finishing the same job? At standard rates and with an identical token mix, Carbon is cheaper through November 30 and Neon is cheaper from December 1. But switching models can change cache hits, thinking tokens and retries. Decide based on the cost of completing a task to the same quality standard, not on the price per token.

Google’s announcement cites an Artificial Analysis comparison in which Neon cost 32% less per task at its default high thinking level, even though its token rates were 1.5× Carbon’s introductory rates. Cost per task and price per token measure different things. That 32% applies to the benchmark, not to a workload we measured.

Carbon and Argon’s introductory prices run through November 30, 2026. From December 1, regular input and output cost $4 and $20 per million tokens, respectively. On those rates Neon is 25% cheaper. Section 06 shows what happens when both models use the same token counts, and when Neon generates more billable output.

**Where to start**

- 
**Estimating the bill?** Start at §06 for the fixed-token calculation and the example that reverses the result. Sections 01–02 explain cache and thinking charges.
- 
**Migrating code or evaluating benchmarks?** Sections 03–05 cover migration pitfalls, harness effects and benchmark conditions.
- 
**Deciding whether to roll out?** Sections 07–09 cover operating constraints and what to record in the adoption test.

Keep common content at the front and send similar prefixes close together to improve the odds of a hit. Timestamps, session IDs, or unstable serialization at the front can reduce reuse.

One mismatched turn does not imply the rest of the loop misses. Check `usage.total_cached_tokens` per turn.

Google’s caching guide lists 4,096 input tokens as the minimum for Neon, not a hit guarantee. On Gemini 4 multi-turn requests, additions below 2,050 tokens (including model output, thought and function results) cause the last 2,050 tokens of the previous request to be processed again at the regular input rate.

Above it, the picture changes. On a hit, input drops from $3.00 to $0.15 per million — a 95% discount.

Neon does not support explicit caching through `cachedContents` / `caches.create`. Any `cached_content` field left over from Gemini 3 code has to go.

If your team used to create caches by hand and manage their TTL, the strategy has to change. Caching stops being a configuration item and becomes a prompt-layout problem.

- Put everything stable first: system instructions, tool schemas, repository map, policy documents.
- Put everything volatile after it: user turns, search results, tool output.
- Keep timestamps, session IDs, and unsorted JSON out of the prefix.

In section 06’s simulation, a 75% input cache hit gives $0.1725 for input (74.2% of the total) and $0.060 for output including thinking (25.8%). Measure the actual hit rate for your workload.

Neon bills output at $15 per million, and that rate includes thinking tokens. `max_output_tokens` caps thinking plus output together.

Raising the thinking level is therefore the same act as buying more tokens at the output rate. Prompt length is not your first cost lever.

Carbon defaults to `medium` and tops out at `high`. Neon and Hydrogen default to `high` and expose `xhigh`.

That makes migration break in both directions. Carbon to Neon raises the default from medium to high, so the same prompt can spend more thinking tokens. Neon to Carbon rejects any code that sets `xhigh`.

On Neon, `minimal` comes back as an error. There is no supported way to turn thinking off.

All three share the same request schema, a 1,048,576-token unified context, and long decode up to 1,048,576 tokens. You can switch model IDs without touching the code — which is what makes the routing in section 04 possible.

| Item | Hydrogen | Neon | Carbon | 
|---|---|---|---|
| Model ID | `gemini-4-hydrogen` | `gemini-4-neon` | `gemini-4-carbon` | 
| Speed | Fast | Moderate | Deliberate | 
| Context | 1,048,576 unified (input + thinking + output) | 1,048,576 unified | 1,048,576 unified | 
| Max output | 65,536 default / up to 1,048,576 with long decode | Same | Same | 
| Thinking levels | low, medium, high, xhigh (default high) | low, medium, high, xhigh (default high) | low, medium, high (default medium), no xhigh | 
| Input per 1M | $1.50 | $3.00 | $2.00 → $4.00 on Dec 1 | 
| Cached input per 1M | $0.075 (95% discount) | $0.15 (95% discount) | $0.10 → $0.20 on Dec 1 | 
| Output per 1M (incl. thinking) | $7.50 | $15.00 | $10.00 → $20.00 on Dec 1 | 
| Free tier | Available; free-tier data used to improve products | None (paid only) | None | 
| Primary use | Everyday coding, UI prototyping, parallel worker agents | Multi-file refactoring, knowledge work, science | Architecture, hardest debugging, security audits, formal proofs | 

Three details are easy to miss.

First, Carbon has no `xhigh`. The habit "if it matters, set xhigh" becomes a rejected request on Carbon.

Second, all three are flat-priced across the full 1M window. 500K tokens cost the same per million as 5K. For workloads that push a whole repository or a bundle of contracts into one call, that gap is far wider than the rate card suggests.

Third, only Hydrogen has a free tier. Prototype on free Hydrogen, validate on paid Neon, escalate the hardest paths to Carbon — that three-stage setup exists because of this one line.

Take working Carbon code, replace the model ID with Neon, and most requests will still succeed. That is the problem. Cost and output quality change silently.


```
// ❌ before — Carbon code with only the model ID swapped
const res = await ai.models.generateContent({
  model: "gemini-4-neon",
  cachedContent: "caches/carbon-prefix-7f21",     // Neon: no explicit caching
  config: {
    temperature: 0.3,                              // in range: ignored, out of range: 400
    maxOutputTokens: 8192,                         // caps thinking + output -> truncation
    thinkingLevel: "xhigh",                        // top thinking cost on every turn
  },
  systemInstruction: `Lead engineer. session=${sessionId} t=${Date.now()}`, // prefix poisoned
  contents: [{ role: "user", parts: [{ text: diff }] }],
});
```
This request leaks in three places: an explicit cache Neon does not have, a prefix that changes every call, and an output cap set to 8,192.

```
// Example based on Google’s JavaScript schema (PREFIX/turn/diff are application inputs). Not run against the live API.
let interaction = await ai.interactions.create({
  model: "gemini-4-neon",
  system_instruction: PREFIX,
  ...(prevId ? { previous_interaction_id: prevId } : {}),
  input: `turn=${turn}\n${diff}`,
  background: true,
  generation_config: { thinking_level: "medium", max_output_tokens: 131072 },
});
for (let attempt = 0; attempt < 180 &&
     ["queued", "in_progress"].includes(interaction.status); attempt++) {
  await new Promise((resolve) => setTimeout(resolve, 5000));
  interaction = await ai.interactions.get(interaction.id);
}
if (interaction.status !== "completed") {
  throw new Error(`Interaction ${interaction.id}: ${interaction.status}`);
}
const text = interaction.steps
  .filter((step) => step.type === "model_output")
  .flatMap((step) => step.content ?? [])
  .filter((part) => part.type === "text")
  .map((part) => part.text).join("");
```
Setting `thinking_level` to `medium` is not a preference. Neon defaults to `high`, so leaving it out means running production on a different setting than the one you evaluated.

Letting the agent pick its own model makes cost vary per call. Move the routing contract out of the prompt and into code.


```
{
  "route": "planner",
  "model": "gemini-4-neon",
  "thinking_level": "high",
  "escalate_to": "gemini-4-carbon",
  "escalate_when": ["same_test_failed_twice", "diff_touches(auth/)"],
  "cache": { "prefix_key": "repo-map:v7", "min_prefix_tokens": 4096 }
}
```
That JSON is a contract your team defines, not a Google API response shape. The values are yours too.

What matters is that `thinking_level` and `prefix_key` are recorded side by side. When an invoice spikes later, those two fields narrow the cause.

One Neon foreground hop is capped at 65,536 output tokens. For more, use background execution or client-side continuation as described in Long Decode.

Background execution returns an ID first. Poll or stream to completion and retrieve the result. It requires `store=true`; paid retention defaults to 55 days (configurable to 7, 14, 28 or 55). Check retention before sending repository material.

Prompting for raw-text JSON tool calls with no declared tools returns `MALFORMED_FUNCTION_CALL`. Tools belong in function calling or structured outputs.

Never copy a thought signature across parallel `functionCall` parts. With a client-managed history, return the model turns and signatures unchanged.

`temperature`, `top_p`, and `top_k` are ignored in range and return a 400 out of range. There is no reason to keep them.

On SWE-sweep, Neon scores 12.8% under the mini-SWE harness and 39.0% (SOTA) under the Antigravity harness. Same model, same tasks.

That number describes the ceiling of the harness, not the skill of the model. Flip it around: a weak harness keeps a strong model at 12.8%.

Antigravity's Orchestrator pairs Neon as planner with Hydrogen as executor inside one session. Neon handles upfront planning, architecture, and complex reasoning. Hydrogen takes package updates, routine code edits, and test runs.

When Hydrogen hits a wall, it hands back to Neon. Neon inspects state, repairs the plan, and re-dispatches. Nobody swaps models by hand.

Google reports that the analogous Carbon + Hydrogen pairing held near-Carbon success rates on Terminal-Bench 4.0 while consuming 24% less quota on average and finishing 19% faster. The logic carries to Neon.

Pay for the expensive model only where judgment is needed; run everything else on the cheap one. That sentence is the conclusion of this article, and it is the part most teams skip.

The launch demos are the harness ceiling, stated as artifacts. A 3D lab bench driving Blender MCP with elementally accurate pairings — Bunsen flames for Carbon and Hydrogen, high-voltage discharge tubes for Argon and Neon — and Alan Turing's 1936 study at King's College rebuilt in interactive 3D, complete with a working Universal Turing Machine on the desk.

Then a single image turned into a 100,000-cell Google Sheet with more than 500,000 live formulas, zero Apps Script, doing edge detection plus 3D depth and relighting. And a first-person WebGL2 ski simulator on real Mont Blanc and Chamonix terrain: 10,200 lines of custom shaders, 120 Hz ski physics.

All of it without pre-made assets, from a single line of prompting. The point for practitioners is one line: harness design, more than model choice, sets the ceiling of what you ship.

The dangerous failure in routing is escalation that never fires. The worker fails quietly and the planner records success.

So the escalation condition belongs in code. Examples of rules a team might choose: the same test fails twice in a row, the diff touches an auth or payment path, the same tool call repeats three times.

Those thresholds are operating rules, not published vendor metrics. They come from your own failure distribution.

| Benchmark | Result | Rank / condition | What it means | 
|---|---|---|---|
| Artificial Analysis Intelligence Index | 61 | #3 overall at `xhigh` | Not the top intelligence; the score at maximum latency and token spend | 
| Artificial Analysis Coding Agent Index | 58.6 | #1 overall | Best end-to-end completion inside a coding-agent harness | 
| Vals Index | 70.9% | #2, on the Pareto frontier | The same score for less money | 
| Vals Legal Research / Tax Agent Bench | – | #1 on each | Legal research and tax agents | 
| Arena Agent Arena Index | 1,336 | #2 over 4,200+ real agent sessions | Real-session performance; Steerability also #2 | 
| Zapier AutomationBench | 56.3% | #1 | Building automation pipelines | 
| Andon Labs Vending-Bench 2 | $16,843 | #1 | Long-horizon autonomous operation, simulated | 
| Agents' Last Exam v1.1 | 75.8% | On par with the leaders in its class | Long-horizon professional workflows | 
| Chartography | 73.5% | – | Reading dense professional charts | 
| Terminal-Bench Science | 70.5% | Best in class | Science and math workloads | 
| RiemannBench | 84.0% | Best in class | Mathematical reasoning | 
| HLE-Diamond | 71.3% | Best in class | Hardest knowledge reasoning | 
| Terminal-Bench 4.1 | 65.2% | – | Terminal-agent task completion | 
| FrontierCode 2.0 | 46.2% | At the default `high` setting | Frontier-difficulty code; a low absolute number reflects the benchmark | 
| SWE-sweep | 12.8% → 39.0% | mini-SWE → Antigravity harness (SOTA) | Same model, same tasks, harness swapped | 

Start with general intelligence: 61, #3. Read it as "not first." The score comes at `xhigh`, and Google itself says it lands just behind larger, slower models. When maximum intelligence is the requirement, Carbon or a competitor's largest model still wins.

Then look at the agentic axis, where the #1s are consecutive: Coding Agent 58.6, AutomationBench 56.3%, Vending-Bench 2 at $16,843. All three are multi-step tasks — tool calls, file reads, state retention, long-horizon capital management — not single answers.

Finally the knowledge and science axis, which is not #1 but "best in class": Terminal-Bench Science 70.5%, RiemannBench 84.0%, HLE-Diamond 71.3%. Google choosing "best in class" over "#1" is a signal to read those as within error of peers.

1. Ranks are among "comparable models."
2. Each score carries its own thinking level (`xhigh` or the default`high` ).
3. Third-party indices weight cost and latency, so they are not pure-accuracy rankings.
4. Agent Arena draws on roughly 4,200 real sessions and is sensitive to sample composition.
5. Honesty and cheating metrics come from Google-internal evaluations and cannot be reproduced externally.
6. Public benchmarks always risk training-data contamination.

With all six in mind, the defensible summary is one line: third overall, first on agentic coding, best in class on science and knowledge.

The 32% figure Google cites is from Artificial Analysis at Neon’s default high setting. Cached and uncached input, output including thinking, and retries can differ by task.

The mix of cached input, uncached input and output is not established here. Unit rates and the cost ratio alone do not identify actual token counts.

Use the rate card and benchmark as starting points. Compare success rate, total cost and latency on the same tasks.

| Item | Neon | Carbon (now → Dec 1) | Hydrogen | 
|---|---|---|---|
| Input | $3.00 | $2.00 → $4.00 | $1.50 | 
| Cached input (95% discount) | $0.15 | $0.10 → $0.20 | $0.075 | 
| Output (incl. thinking tokens) | $15.00 | $10.00 → $20.00 | $7.50 | 
| Caching mode | Implicit only (no explicit caching) | Implicit only | Implicit only | 
| Long-context surcharge | None (flat across 1M) | None | None | 
| Free tier | No | No | Yes | 
| Cost per task (default high) | 32% lower than Carbon | Baseline | – | 
| Cost per task (Vals, Agent Arena, high) | 50–80% lower than top competing frontier models | – | – | 
| Cost per task (xhigh) | 36%+ cheaper than competitors at their highest setting | – | – | 

Assume one task uses 200K input tokens with 150K served from cache, plus 4K output tokens including thinking. The assumption is ours; the rates are the published ones.

| Component | Neon | Carbon (through Nov 30) | Carbon (from Dec 1) | 
|---|---|---|---|
| Uncached input 50K | $0.150 | $0.100 | $0.200 | 
| Cached input 150K | $0.0225 | $0.015 | $0.030 | 
| Output 4K | $0.060 | $0.040 | $0.080 | 
| **Total** | **$0.2325** | **$0.155** | **$0.310** | 

**Apply the formula to your workload.** Token cost in USD = (uncached input tokens × regular input rate + cached input tokens × cache rate + output tokens including thinking × output rate) ÷ 1,000,000. Subtract cache hits from total input to get uncached input. Use rates per million tokens from the official Standard pricing table, add separately billed tools if used, and sum all calls for the task, including retries and failed calls.

In an Interactions API response, cache hits are `usage.total_cached_tokens`. For billable output, add `usage.total_output_tokens + usage.total_thought_tokens`. The output field alone leaves out thinking tokens.

This is a fixed-token simulation, not a measured API run. Neon input is $0.1725 (74.2%) and output including thinking is $0.060 (25.8%), for a total of $0.2325.

On December 1, the rate change doubles Carbon’s cost for this exact token mix, from $0.155 to $0.310.

Keep input and cache hits fixed, but raise Neon’s output including thinking from 4,000 to 10,000 tokens. Neon now costs $0.3225, versus $0.310 for December Carbon at 4,000 output tokens. Under those assumptions the crossover is about 9,167 Neon output tokens. This is a sensitivity check, not a prediction of how many tokens Neon will use.

Under the table’s identical token assumptions, Neon costs 50% more than November Carbon ($0.155) and 25% less than December Carbon ($0.310). This rate-card simulation is separate from the launch’s 32% figure and does not predict your workload savings.

Put three scenarios side by side: (a) Carbon left as is into December, (b) the same traffic on Neon with an explicit hit-rate assumption, (c) Carbon kept only for the hardest paths and everything else split across Neon and Hydrogen.

The Batch API takes 50% off, and Flex and Priority inference are supported (check pricing separately). A standard split: batch for overnight refactors and mass extraction, Priority for interactive IDE loops.

Neon accepts text, images, video, audio, and PDFs in a roughly 1M-token unified context. The default output cap is 65,536 tokens. To exceed it, raise `generation_config.max_output_tokens` and use background execution or client-side continuation. The effective limit is the context window minus the input. Long Decode documentation

Neon ships in the Antigravity desktop app, Antigravity CLI, and IDE extensions for paid Google AI Pro and Ultra subscribers. Enterprises reach it through Gemini for business, either by API or from the model dropdown.

On the Gemini API you call `gemini-4-neon` on the paid tier (Google AI Studio, Android Studio). There is no free tier, so confirm the project is billed before anything else.

Neon is generally available in Copilot for Pro+, Max, Business, and Enterprise plans. The model picker covers VS Code, Visual Studio, Copilot CLI, Copilot coding agent, the Copilot app, github.com, GitHub Mobile on iOS and Android, JetBrains IDEs, Xcode, and Eclipse.

Rollout is gradual, so a missing entry usually means waiting a few days. Billing is usage-based at provider list pricing.

Two things matter at the organization level. Token spend is billed separately from premium request allowances, so budget guardrails are needed. And Enterprise and Business administrators manage access through the model policy in Copilot settings — under default enablement, the model is enabled unless an admin turns off the global default or disables it outright.

Hydrogen runs $1.50 / $7.50 — half of Neon. Gemini 3.8 Flash lands on top of that: $0.75 / $3.75 through December 31, 2026, then $1.50 / $7.50 from January 1, 2027.

Which is exactly Hydrogen's price. The Flash-to-Hydrogen decision stops being "twice as expensive" and becomes "the same price from 2027 anyway, so move now."

In the same announcement, Google says Hydrogen finishes tasks 2.5× faster than 3.8 Flash at its default setting, scores 57% versus 18.2% on Terminal-Bench 4.1, and cuts fabricated claims by more than 5×.

Neon ships a variant tuned for defense rather than offense, rolled out to trusted defenders through the Fairwind Program.

- Secure code generation: safer than Carbon across every AutoBaxBench difficulty, with 76% on the hard tier (SOTA)
- Vulnerability discovery: 90.5% on SEC-bench Pro (SOTA) at roughly half the cost per challenge
- Exploit validation: frontier-level on ExploitGym using half the tokens of Carbon
- Patching: 70.7% on Collinear's CWE-bench v2, a 3.4-point gain over Carbon
- Penetration testing: 51.26% on WebExploitBench v1.2 (SOTA) at half the cost of competing large models

- Legitimate refusals: 1.98% on Terminal-Bench 4.0, 0% on Terminal-Bench-Science
- Cheating and shortcuts: 0.0% on Terminal-Bench 4.0 (matching Carbon), 2.45% on the coding-cheating evaluation (lowest of any Gemini 4 model)
- Misaligned action rate 0.09%, agent honesty pass rate 99.2%
- Under-service down 43–56% and overreach down 15–35% on the agentic behavior evaluation
- Indirect prompt injection: 0.0% attack success on Gray Swan's IPI benchmark, at the highest thinking level with real-time safeguards

Refusal rates, honesty, and misalignment are Google-internal evaluations, not reproducible outside. The IPI figure holds at the highest thinking level with safeguards enabled.

The Carbon launch post, built on the same stack, still notes that advanced automated red teaming finds room for improvement in some attack distributions. If your system feeds web pages, emails, or documents into an agent, a vendor number is not a waiver. Run your own injection red team on a schedule, with malicious instructions planted in documents, issue comments, and pages.

The direction, though, is good news. A 1.98% refusal rate means legitimate security work is not being blocked at the door. In internal security automation, the most common failure mode is a model refusing a security request and stalling the pipeline, and this number suggests that risk is lower.

**When maximum intelligence is the requirement.** 61 at `xhigh` is #3. System architecture, cross-subsystem root-cause work, and formal proofs still belong to Carbon or a competitor's largest model.

**When you need a free prototype.** Neon is paid only. Start on the free Hydrogen tier and promote only the paths that survive validation.

**When the work is bulk and simple.** JSON parsing, translation, short summaries: minimal judgment, so Neon is overspecced. Hydrogen at $1.50 / $7.50, or a Flash-tier model, is the answer.

**When you must manage cache TTL yourself.** Neon offers implicit caching only. A requirement to control cache lifetime in code rules this model out.

**When vendor numbers are your injection defense.** The 0.0% is one condition — highest thinking level with safeguards — from an internal evaluation. If your agent reads external documents, build the check layer yourself.

Two axes only. High frequency with moderate difficulty — multi-file refactoring, code review, document and spreadsheet analysis, agent loops — means Neon. Low frequency with extreme difficulty — system architecture, root-cause tracing, formal proofs, security audits — means Carbon.

High frequency with low difficulty (translation, summarization, JSON extraction at volume) means Hydrogen or Flash-Lite. For a solo developer, the simplest start is interactive IDE work on Neon and overnight automation on Hydrogen batch.

Pull the monthly cost of your current Carbon traffic and place three scenarios side by side. State the cache hit-rate assumption; the multiple becomes visible immediately.

Neon and Hydrogen default to `high`; Carbon defaults to `medium`. Reflect that in code. Run your evaluation suite from `low` through `medium`, `high`, and `xhigh`, then take the lowest level that clears your quality bar as the default.

Keep `max_output_tokens` at 65,536 or more during evaluation. A truncated answer distorts the score.

On the direct API, apply the code checklist below. On Antigravity, enable the Neon + Hydrogen Orchestrator and measure quota first. On Copilot, confirm the plan, rollout status, usage-based billing limits, and model policy.

Run both models on the same evaluation set. Log input and output tokens, cache hits, retries, and human review time. Include failed calls in total cost, then divide by the number of successful tasks. Move a category of work to Neon when it clears the same quality bar at a lower cost per successful task.

- Set the model ID to `gemini-4-neon` (no request schema change)
- Use a paid-tier project (no free tier)
- Set `thinking_level` explicitly: default`high` ,`xhigh` supported,`minimal` errors
- Remove explicit caching (`cachedContents` ,`cached_content` )
- Remove `temperature` ,`top_p` ,`top_k` : in-range values are ignored, out-of-range returns 400
- Declare tools natively (function calling, structured outputs); raw-text JSON tool calls return `MALFORMED_FUNCTION_CALL`
- Use `previous_interaction_id` for multi-turn and tool calls; in client-managed histories return thought signatures unchanged (never copy across parallel`functionCall` parts)
- Use background execution or client-side continuation for output beyond 64K
- Remove hardcoded low `max_output_tokens` caps (e.g. 8,192) and raise them to 65,536
- Recalculate billing estimates: thinking tokens, plus internal grounding instructions and function declarations, are all billed

- Google DeepMind announcement (Nov 18, 2026, Koray Kavukcuoglu): https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/
- Google Antigravity blog (Nov 18, 2026, Antigravity Team): https://antigravity.google/blog/gemini-4-neon-in-google-antigravity/
- GitHub Changelog (Nov 18, 2026): https://github.blog/changelog/2026-11-18-gemini-4-neon-in-github-copilot/
- Gemini API pricing · Caching · Interactions API · Background execution · Long Decode
- Choosing a model: https://ai.google.dev/gemini-api/docs/choosing-a-model
- What's new in Gemini 4 Neon: https://ai.google.dev/gemini-api/docs/whats-new-gemini-4-neon
- Gemini 4 Carbon (Nov 10, 2026), Hydrogen (Nov 12, 2026), Argon (Oct 13, 2026) announcements

Neon is not "the smartest model." Third overall, best in class on science and knowledge, first on agentic coding and automation — together that describes a model that keeps judgment where it matters while lowering the unit cost of repeated execution.

Decide whether to switch using December rates, observed cache hits, latency and cost per successful task including retries. The 32% is limited to the benchmark Google cited.

Decide in this order: frequency × difficulty, then December pricing, then pinned thinking levels, then harness and billing integration. Do it in that order and the December invoice will not surprise you.

## Top comments (0)
