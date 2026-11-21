---
id: collect-261121-veille-ia/kimi/what-is-kimi-k3-1-pro
title: "What Is Kimi K3.1 Pro?"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "moonshot", "model", "same", "tokens", "post-training", "scores", "input"]
source: https://dev.to/hassann/what-is-kimi-k31-pro-4lb
source_anchor: ""
source_lines: [1, 159]
section: "Kimi"
---
Kimi K3.1 Pro is Moonshot AI's newest flagship model, released on October 23, 2026. It's a post-trained version of Kimi K3: the same 2.8T-parameter MoE design (about 104B active parameters), the same 1,048,576-token context window, and a lower API price of $2 per 1M input tokens and $10 per 1M output tokens — one third below K3. Moonshot says the full weights will be released by November 4.

So what is Kimi K3.1 Pro in practice? It's K3 with much more post-training, aimed at long-horizon coding, finance, 3D modeling scripts, and fewer hallucinations. Once you've picked it, our step-by-step guide on how to use Kimi K3.1 Pro takes you from key to first call, and how to use Kimi K3.1 Pro for free lists the $0 routes.

Kimi K3.1 Pro is the first model in Moonshot AI's K3.1 series. Moonshot describes it as "a post-trained version of Kimi K3, with substantially larger post-training compute." It reads text, images and video, writes text, always thinks before answering, and is available in Kimi's apps, Kimi Code and the Kimi API.

Moonshot calls it "our most capable 3T-class open-weight model to date" — that's the vendor's framing. The architecture didn't change, so if you know K3 from our Kimi K3 explainer, you know the base. What post-training changed is the behavior: long-task planning, tool use, and how often the model makes things up.

| Spec | Kimi K3.1 Pro | 
|---|---|
| Developer | Moonshot AI | 
| Release date | October 23, 2026 | 
| Architecture | Same MoE design as K3: 2.8T total, about 104B active | 
| Context window | 1,048,576 tokens | 
| Input | Text, image, video | 
| Output | Text | 
| Thinking | Always on; `reasoning_effort` of`low` ,`high` or`max` (default`max` ) | 
| API model IDs | `kimi-k3.1-pro` ,`kimi-k3.1-pro-highspeed` | 
| API price | $2 input / $10 output per 1M tokens | 
| Open weights | Promised by November 4, 2026; license not stated | 

Kimi K3.1 Pro keeps K3's architecture and changes five things:

1. 
**More post-training compute.** Same base, heavier post-training.
2. 
**Lower price.** $2/$10 per 1M tokens versus K3's $3/$15.
3. 
**Fewer hallucinations.** 8.3% versus 23.4% on Moonshot's hardest hallucination test with tools.
4. 
**New focus areas.** Finance, 3D modeling and long-horizon coding.
5. 
**Cyber data removed.** Stripped from mid-training and RL before the open-weight release.

Every per-token rate drops by a third, including cached input ($0.30 to $0.20) and cache writes.

On Kimi Hallu Bench Hard, which scores claim-level hallucination with tools, K3 hit 23.4% and K3.1 Pro 8.3%. Moonshot calls it a stress test, not an everyday error rate.

On Moonshot's internal Kimi Code Bench 3.0 Long-horizon, K3.1 Pro scores 61.2% versus 55.95% for K3, with example builds in Unity, Unreal Engine and Godot.

Finance moved the most. On Moonshot's internal finance suite, Financial Modeling went from 50.4 to 57.6, Financial Research from 58.8 to 70.4, and Business Operation from 8.9 to 37 — about 4.2 times K3's score.

The cyber change runs the other way on purpose. On xdev-bench-hard (pass@3), K3.1 Pro scores 81.0%, while an internal checkpoint without that removal hit 90.5%. K3 scored 33.3%. Moonshot says K3.1 Pro's cyber capability stays behind frontier proprietary models that have been public for three months or more.

For K3's own scores, see our Kimi K3 benchmarks breakdown.

Every number in this section comes from Moonshot's own announcement. They're vendor-run and haven't been independently reproduced. Here's how K3.1 Pro compares with Claude Opus 5.5 and GPT-6 Astra where Moonshot reports all three:

On Moonshot's numbers, K3.1 Pro sits within a point of Opus 5.5 on Terminal-Bench 4.0 and BenchCAD without tools, trails both rivals on BenchCAD with tools, and lands between GPT-6 Astra and Opus 5.5 on hallucination.

Moonshot reports a few more scores without a full rival set:

- 
**OfficeQA Pro v2:** 64.4, which Moonshot calls state of the art. It makes the same claim on SpreadsheetBench v2 without printing a number.
- 
**Frontier Finance:** 58.6.
- 
**BenchCAD with tools:** Claude Fable 5.1 scores 92.6 and DeepSeek-V4.1-Flash-1017 scores 81.6, so K3.1 Pro edges Fable there.

BenchCAD ties to a practical claim: K3.1 Pro writes Blender and CAD scripts directly.

For a third-party view, the Artificial Analysis model page gives K3.1 Pro an Intelligence Index of 55, rank 4 of 215. It measured 77.1 output tokens per second and a 2.89-second time to first token on Kimi's API. Simon Willison's write-up notes that Artificial Analysis puts Opus 5.5 at 58 and Claude Fable 5.1, GPT-6 Astra and Gemini 4 Argon at 53. If you're weighing Anthropic's flagship, our Claude Opus 5.5 guide covers it.

Kimi K3.1 Pro costs $2 per 1M input tokens and $10 per 1M output tokens on the Kimi API, pay as you go, before tax. Cached input costs $0.20. You unlock it with a top-up of at least $1. Here's the full table from the official pricing page, read October 24, 2026:

Three rules shape your bill:

- 
**Caching is automatic** for repeated prefixes, with a default TTL of 5 minutes. Cache hits bill at the cached rate only.
- 
**Flagship unlock.** Moonshot's quickstart says K3.1 Pro "is unlocked after a successful top-up (minimum $1)." Your cumulative top-up also sets your tier and rate limits.
- 
**Voucher.** When cumulative recharge reaches $5, you get a $5 voucher. Whether vouchers can pay for K3.1 Pro isn't stated.

At $2/$10, a $1 top-up buys about 500K input tokens or 100K output tokens. OpenRouter lists `moonshotai/kimi-k3.1-pro` at the same $2/$10 with no free variant, and Vercel AI Gateway matches that price.

In Kimi's apps, the free Adagio plan shows K3.1 Pro as the default chat model, and the free-plan message quota isn't published. Kimi Code needs a paid membership; the membership pricing page starts at Plus for $15 a month billed annually.

Kimi K3.1 Pro Highspeed is a faster companion model that Moonshot says runs at "2-3x the speed of K3.1 Pro." It costs double on the API: $4 input and $20 output per 1M tokens, with the same $0.20 cached input. The API model ID is `kimi-k3.1-pro-highspeed`.

Pick Highspeed when latency is the bottleneck, such as interactive coding or agents that chain many short calls. Use the standard model for batch jobs and long documents.

In Kimi Code, `k3.1-pro-highspeed` needs the Allegretto/Pro plan ($31 a month billed annually) or higher and uses about 2x the quota. Our Kimi Code CLI guide walks through the tool itself.

Kimi K3.1 Pro isn't open weights yet. Moonshot's announcement says "the full model weights will be released by Nov 4" 2026. It hasn't stated the license, so you can't yet tell whether commercial self-hosting is allowed.

K3's weights shipped at 594 GB in MXFP4, and K3.1 Pro uses the same architecture, so expect a similar footprint. Our guide to running Kimi K3 locally shows what that takes in practice.

Kimi K3.1 Pro fits these teams best:

- 
**Teams already on the K3 API.** Same architecture, one third cheaper, fewer hallucinations on Moonshot's tests. Switching is a model-ID change.
- 
**Long-horizon coding agents.** A 1M context window and Terminal-Bench 4.0 close to Opus 5.5 on vendor numbers.
- 
**Finance and spreadsheet work.** The largest internal gains and the OfficeQA Pro v2 claim sit here.
- 
**3D and game pipelines.** Blender and CAD scripts, plus Unity, Unreal Engine and Godot builds.
- 
**Teams that want self-hosting later.** Weights are promised by November 4.

Look elsewhere if you need the lowest hallucination rate on Moonshot's own chart (GPT-6 Astra scored lower) or if your security team needs strong offensive-security help, which Moonshot deliberately limited. Zero Data Retention is available for API customers.

The Kimi API is OpenAI compatible at `https://api.moonshot.ai/v1`, so it's easy to regression-test. Don't check responses by hand after every change: let your coding agent drive Apidog CLI.

Install the CLI, log in, and add the agent skill:


```
npm install -g apidog-cli@latest
apidog login --with-token <YOUR_TOKEN>
apidog skill install
```
Or tell your agent: "Read https://apidog.com/apidog-cli-installation-guide.md?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation and set up Apidog CLI."

Then give your agent one [REDACTED PROMPT]

In Apidog project , create an environment for https://api.moonshot.ai/v1.

Create a test case for POST /chat/completions with model "kimi-k3.1-pro",

reasoning_effort "high", and a Bearer [REDACTED] from MOONSHOT_API_KEY. Assert status 200,

a non-empty choices[0].message.content, and a model field that contains "kimi-k3.1-pro".

Run it and summarize any failures.


```
The agent creates the environment, reads the test case schema, validates its payload, writes the case and runs it:
```

bash

apidog environment create kimi-prod --project  --base-url https://api.moonshot.ai/v1

apidog cli-schema get test-case-create

apidog cli-schema validate test-case-create --file case.json

apidog test-case create --project  --file case.json

apidog run --test-case  --project  -e  -r cli,json

```
Test cases need a valid category, and `apidog test-case category --project <projectId>` lists them. Add `-n 5` to repeat the run, or `-r junit --out-dir ./reports` to feed CI. Our [Apidog CLI in Claude Code guide](https://apidog.com/blog/apidog-cli-in-claude-code?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) covers the agent setup, and the [CI/CD pipeline guide](https://apidog.com/blog/apidog-cli-ci-cd-pipeline?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) wires the same run into every pull request. Apidog's Free plan covers up to 4 users.
## FAQ
### Is Kimi K3.1 Pro free?
Partly. Kimi's web and mobile apps show K3.1 Pro as the default model for free accounts, but the free message quota isn't published. The API isn't free: you unlock K3.1 Pro with a $1 minimum top-up. See [how to use Kimi K3.1 Pro for free](https://apidog.com/blog/how-to-use-kimi-k3-1-pro-for-free?utm_source=dev.to&utm_medium=wanda&utm_content=n8n-post-automation) for every $0 route.
### What is the Kimi K3.1 Pro context window?
1,048,576 tokens, per Kimi's pricing page. In Kimi Code, the full 1M context needs the Allegretto/Pro plan or higher.
### Is Kimi K3.1 Pro better than Claude Opus 5.5?
It depends on the task. On Moonshot's vendor-run numbers, Opus 5.5 leads slightly on Terminal-Bench 4.0 (66.4% vs 65.7%) and BenchCAD, while K3.1 Pro shows fewer hallucinations on Kimi Hallu Bench Hard. K3.1 Pro costs $2/$10 per 1M tokens.
### What model ID do I use in the API?
Use `kimi-k3.1-pro`, or `kimi-k3.1-pro-highspeed` for the faster variant, with the base URL `https://api.moonshot.ai/v1`. The [official quickstart](https://platform.kimi.ai/docs/guide/kimi-k3-1-pro-quickstart) has Python and cURL examples, and `reasoning_effort` accepts `low`, `high` or `max`.
### When will Kimi K3.1 Pro weights be released?
Moonshot says by November 4, 2026. It hasn't announced the license yet, so check the [official announcement](https://www.kimi.ai/blog/kimi-k3-1-pro) before you plan commercial self-hosting.
```
## Top comments (0)
