---
id: collect-261121-veille-ia/qwen/qwen4-max-vs-qwen-3-8-price-context-benchmarks-and-what-changed
title: "Qwen4-Max vs Qwen 3.8: Price, Context, Benchmarks and What Changed"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-11-16"]
keywords: ["qwen4-max", "tokens", "input", "qwen", "qwen3", "price", "output", "changed"]
source: https://apidog.com/blog/qwen-4-max-vs-qwen-3-8/
source_anchor: ""
source_lines: [1, 163]
section: "Qwen"
---
Qwen4-Max vs Qwen 3.8 comes down to this: Qwen4-Max costs less and does more. It’s $1.50 input and $4.50 output per million tokens against Qwen3.8-Max’s $2 and $6, with the same 1M-token context, a 393K output limit against 131K, and cached input at less than a third of the price. Alibaba’s own benchmarks put it ahead on professional work while using fewer tokens.

That’s the picture on November 16, 2026, after Qwen4-Max moved from preview to general release. Below: the specs side by side, what changed, the price math, what’s still vendor-reported, and a hands-on test you can run on both. New to either model? Start with what Qwen4-Max is and what Qwen 3.8 is.

## Qwen4-Max vs Qwen 3.8 at a glance

Every row comes from Alibaba’s own pages, checked November 16, 2026.

|  | Qwen4-Max | Qwen3.8-Max | 
|---|---|---|
| Status | Generally available, November 2026 | Generally available since August 2026 | 
| Model ID | `qwen4-max` | `qwen3.8-max` | 
| Input price | $1.50 per 1M tokens | $2 per 1M tokens | 
| Output price | $4.50 per 1M tokens | $6 per 1M tokens | 
| Cached input | $0.075 per 1M tokens | $0.25 per 1M tokens | 
| Context window | 1M tokens | 1M tokens | 
| Max output | 393K tokens | 131K tokens | 
| Inputs | Text, images and video | Text, images and video | 
| Rate limit | 5,000,000 tokens per minute | 2,000,000 tokens per minute | 
| Parameters | 2.4T backbone, 71B activated per token | 2.4T total, Mixture-of-Experts | 
| API protocols | OpenAI- and Anthropic-compatible | OpenAI- and Anthropic-compatible | 
| On Token Plan | Yes | Yes, “40% Off Nights” | 

Sources: the Qwen4-Max model page, the Qwen3.8-Max model page and the Token Plan pricing page.

## What changed in Qwen4-Max

The launch post describes a new architecture and reports gains across the board. These are the Qwen team’s own results.

**1. A new architecture at the same scale.** Both models sit at 2.4 trillion parameters. Qwen4-Max rebuilds the sparse Mixture-of-Experts design with Gated DeltaNet layers and Qwen Sparse Attention for long sequences, a Gated Residual design for stable training, and N-gram Embedding for extra capacity. Alibaba says its base model activates 25% fewer parameters than Qwen3.8-Max’s and scores 5% to 13% better on its in-house knowledge, code, QA and STEM evaluations.

**2. Higher scores on professional work.** On Work360, Alibaba’s benchmark of industry tasks across 26 domains, Qwen4-Max scores 52.69 against 47.74 for Qwen3.8-Max.

**3. Fewer tokens per task.** On Work360, token use drops about 24% at Xhigh effort and 35% at High effort. On Terminal-Bench 4.0, it drops about 48% to 49%.

**4. Safer code.** On Last Secure Code Bench, the function-level pass rate for working, secure code rose from 22.0% to 31.8%. The share of code generated securely at function, file and repository level rose from 8% to 14.7%.

**5. Longer runs.** Qwen 3.8’s model page already described autonomous projects “spanning 10+ days.” Qwen4-Max adds Live Delivery, where the agent ships a usable first version early and improves it release by release while you steer. The team open-sourced it as the Qwen-Live-Delivery-Plugin.

The direction hasn’t changed between generations. Qwen4-Max goes further down the same road: coding, professional work and long agentic tasks.

## Price: Qwen4-Max vs Qwen 3.8

Qwen4-Max is 25% cheaper on both input and output, and 70% cheaper on cached input.

| Per 1M tokens | Qwen4-Max | Qwen3.8-Max | 
|---|---|---|
| Input | $1.50 | $2.00 | 
| Output | $4.50 | $6.00 | 
| Implicit cache input | $0.075 | $0.25 | 

The cache rate matters most for agents. A coding agent resends the conversation, tool schemas and file contents on every turn, so most of its input is cache hits. Take a session with 10 million cached input tokens, 1 million fresh input tokens and 500,000 output tokens:

- **Qwen4-Max:** $0.75 + $1.50 + $2.25 = $4.50
- **Qwen3.8-Max:** $2.50 + $2.00 + $3.00 = $7.50

That’s 40% less for the same tokens. Then the token savings stack on top: if Qwen4-Max needs fewer tokens for the job, as Alibaba reports, the gap widens. Alibaba’s own figure is a drop in average cost per Work360 task from $4.43 to $1.67, a 62% cut.

Both models are also on the Token Plan, Alibaba’s subscription for coding tools, where they draw from the same Credit pool. See our Qwen4-Max Token Plan guide for tiers and rules, and the Qwen 3.8 API guide for the older model’s full price sheet.

## What’s still vendor-reported

The preview left most comparison cells empty. The general release filled them, with three caveats.

- **Work360 is Alibaba’s own benchmark.** The launch post also cites outside leaderboards, including Artificial Analysis, LM Arena, the Mercor APEX-Agents leaderboard and the Vals Index. Check those pages for current standings before you quote a score. For Qwen 3.8’s numbers, see our Qwen 3.8 benchmarks breakdown.
- **Token efficiency depends on the task.** A 48% drop on Terminal-Bench 4.0 doesn’t promise 48% on your codebase.
- **Open weights.** Neither Max model has downloadable weights. Alibaba says smaller Qwen4 models are coming to open source, with no names or dates.

For access details on every route, see how to use Qwen4-Max.

## Run your own Qwen4-Max vs Qwen 3.8 test

Vendor benchmarks point in a direction. Your own test settles it. API work fits well, because an endpoint either passes its tests or it doesn’t.

Both models run on the same endpoints, so switching is one model name. With Qwen Code on the Token Plan, list both in `~/.qwen/settings.json`:

```
{
  "$version": 4,
  "env": { "QWENCLOUD_TOKEN_PLAN_API_KEY": "YOUR_API_KEY" },
  "modelProviders": {
    "openai": [
      { "id": "qwen4-max", "baseUrl": "https://token-plan.maas.qwencloudapi.com/compatible-mode/v1", "envKey": "QWENCLOUD_TOKEN_PLAN_API_KEY" },
      { "id": "qwen3.8-max", "baseUrl": "https://token-plan.maas.qwencloudapi.com/compatible-mode/v1", "envKey": "QWENCLOUD_TOKEN_PLAN_API_KEY" }
    ]
  }
}
```
Then set up the fair test:

1. **Write the contract once.** Create one`openapi.json` for a small API, for example`POST /orders` and`GET /orders/{id}` with validation and auth rules. Neither model writes the spec, so neither model grades its own homework.
2. **Make two empty folders** ,`build-qwen4` and`build-qwen38` , each with a copy of the spec.
3. **Send the identical build prompt** in each folder, with`qwen4-max` selected in one and`qwen3.8-max` in the other: “Build the API in openapi.json with Express. Run it on port 3001 (or 3002). Don’t change the spec.”
4. **Record the basics** : time to a running server, tokens used, and whether you had to step in.

Then test both builds against the same suite. That’s where Apidog CLI comes in.

## Score both builds with Apidog CLI

Your coding agent drives Apidog CLI: it imports the shared spec, creates one set of test cases from it, and runs that set against each build. Same tests, two targets, two reports.

### Set up Apidog CLI once

```
npm install -g apidog-cli@latest
apidog auth login --with-token <YOUR_APIDOG_TOKEN>
apidog skill install
```
`apidog skill install` teaches your agent the CLI. The installation guide covers tokens.

### Give your agent one prompt

Any agent can referee; it doesn’t have to be a Qwen model.

```
Import ./openapi.json into Apidog project <projectId>. Create two environments:
qwen4 at http://localhost:3001 and qwen38 at http://localhost:3002. From the spec,
create test cases for POST /orders: 201 on a valid order, 400 on a missing
customerId, 401 without a token, plus a GET /orders/{id} 404 case. Run every case
against both environments, save JSON reports, and give me a pass/fail table per model.
Don't modify either build.
```
### The `apidog` commands it runs

Every flag below comes from `apidog <command> --help` on Apidog CLI 2.2.12:

```
apidog import --project <projectId> --format openapi --file ./openapi.json
apidog environment create qwen4 --project <projectId> --base-url http://localhost:3001
apidog environment create qwen38 --project <projectId> --base-url http://localhost:3002
apidog cli-schema get test-case-create
apidog cli-schema validate test-case-create --file ./case-201.json
apidog test-case create --project <projectId> --file ./case-201.json
apidog run --test-case <caseId> --project <projectId> -e <qwen4EnvId> -r cli,json --out-file qwen4-{GENERATE_TIME}
apidog run --test-case <caseId> --project <projectId> -e <qwen38EnvId> -r cli,json --out-file qwen38-{GENERATE_TIME}
```
`cli-schema get` hands the agent the exact JSON shape for a test case, and `cli-schema validate` checks each file before anything is written. The agent repeats `test-case create` and the two `apidog run` lines for each case. The `cli` reporter prints a pass/fail summary, and `json` saves a report in `apidog-reports`, which the agent reads to build its table. Add `--upload-report` to view a run in the Apidog app.

### What to read in the reports

Count three things per model: cases passed on the first build, fix rounds needed to reach all green, and any case where the model changed the contract instead of the code. Run the whole test two or three times, since one run tells you little. For more agent-driven patterns, see the Apidog CLI agent workflow and Apidog CLI in Claude Code. Apidog’s Free plan covers up to 4 users.

## Which should you use?

Match the model to the job:

- **New projects:** Qwen4-Max. It’s cheaper per token, has a larger output limit and a higher rate limit, and nothing on the spec sheet favors the older model.
- **Production apps already on Qwen 3.8:** switch after a test. The API shape is the same, so it’s a one-line model change, but run your own evaluation suite first. Prompts tuned for one model can behave differently on the next.
- **Coding with an agent:** Qwen4-Max, on the Token Plan or the API. Run the test above on a task from your real backlog, not a toy.

Keep the model ID in one config value, so the next swap is a one-line change. See also Qwen 3.8 for coding and our guide to the best Qwen models.

## FAQ

**Is Qwen4-Max better than Qwen 3.8?** On Alibaba’s numbers, yes: 52.69 against 47.74 on Work360, with fewer tokens per task and a higher secure-coding pass rate. Those are vendor results, so test both on your own task.

**Can I call Qwen4-Max through the API like Qwen 3.8?** Yes. Both are on QwenCloud’s pay-per-token API with OpenAI- and Anthropic-compatible endpoints. The model IDs are `qwen4-max` and `qwen3.8-max`.

**Does Qwen4-Max have a 1M-token context like Qwen 3.8?** Yes. Both list a 1M context. Qwen4-Max allows up to 393K output tokens; Qwen3.8-Max allows 131K.

**How much does Qwen4-Max cost compared with Qwen 3.8?** Qwen4-Max costs $1.50 input and $4.50 output per million tokens. Qwen 3.8 costs $2 and $6. Cached input is $0.075 against $0.25.

**Should I switch my app from Qwen 3.8 to Qwen4-Max?** Probably, after a test. It’s cheaper and scores higher on Alibaba’s benchmarks, and the API is compatible. Run your evaluation suite on both before you change production.
