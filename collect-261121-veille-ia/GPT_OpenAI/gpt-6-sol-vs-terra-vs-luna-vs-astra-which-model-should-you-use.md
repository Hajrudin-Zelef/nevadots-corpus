---
id: collect-261121-veille-ia/gpt-openai/gpt-6-sol-vs-terra-vs-luna-vs-astra-which-model-should-you-use
title: "GPT-6 Sol vs Terra vs Luna vs Astra: Which Model Should You Use?"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-26"]
keywords: ["gpt-6", "terra", "model", "luna", "gpt-5", "page", "work", "openai"]
source: https://blog.laozhang.ai/en/posts/gpt-6-sol-vs-terra-vs-luna-vs-astra
source_anchor: ""
source_lines: [1, 107]
section: "GPT_OpenAI"
---
## On this page

**Start with `gpt-6.1-sol` for demanding coding and agent work, `gpt-6.1-terra` for fast everyday work such as writing, reading images, and computer use, `gpt-6-luna` for focused tasks you can check cheaply, and `gpt-6-astra` for the hardest multi-step work.** OpenAI released GPT-6.1 Terra on October 26, 2026, so the GPT-6 lineup is now GPT-6 Astra, GPT-6.1 Sol, GPT-6.1 Terra, and GPT-6 Luna. The earlier `gpt-5.6-terra` is still listed and priced as the previous Terra, so keep it in the trial if it already works for your team. Those are starting choices drawn from OpenAI's model guidance and its GPT-6.1 Terra announcement, not measured winners on your workload.

The practical question is whether a model clears your quality bar at an acceptable **cost per accepted task**. Per-token prices alone cannot answer that: output length, reasoning effort, tool use, retries, and human repair can change the result. GPT-6.1 Terra charges exactly half of GPT-6.1 Sol per token, yet on third-party benchmark tasks Sol becomes the cheaper way to reach the same score once Terra runs at its higher reasoning settings. The direct-API prices below are as of **October 28, 2026**.

## Which names are actually GPT-6?

Four models carry the GPT-6 name as of October 28, 2026: GPT-6 Astra (`gpt-6-astra`), GPT-6.1 Sol (`gpt-6.1-sol`), GPT-6.1 Terra (`gpt-6.1-terra`), and GPT-6 Luna (`gpt-6-luna`), the exact four in the flagship table of OpenAI's API pricing page. The version bump applies to Sol and Terra; Astra and Luna are still GPT-6 models. The OpenAI API changelog records the GPT-6 Sol and Luna release on September 22, 2026, and GPT-6.1 Sol on September 29, and OpenAI announced GPT-6.1 Terra on October 26 as the first Terra-class model since GPT-5.6. Use full IDs in tests and configuration so an older model with a similar name cannot be mistaken for a new one.

So yes, there is a GPT-6 Terra, but no model ID `gpt-6-terra` exists: the GPT-6 Terra is GPT-6.1 Terra, called as `gpt-6.1-terra`. The GPT-5.6 Terra model page still names the previous Terra `gpt-5.6-terra`, and that model keeps its own price. When someone says "Terra" without a version number, ask which one they mean, because the two are priced differently in every column.

The original `gpt-6-sol` is now the previous Sol. Its model page is still live and points to GPT-6.1 Sol, and the deprecations page lists no shutdown date for it as of September 30, 2026. Like `gpt-5.6-terra`, it has dropped out of the pricing page's flagship table but is still priced in the full model list.

| Model ID | Generation | A sensible first trial | What would change the choice? | 
|---|---|---|---|
| `gpt-6-luna` | GPT-6 | High-volume extraction, summarization, or focused coding with cheap validation | Repeated semantic errors or repairs consume the savings. | 
| `gpt-6.1-terra` | GPT-6 family, current Terra (October 26, 2026) | Fast everyday work: writing, reading screenshots and documents, computer use, and coding that tests can check | A hard task keeps failing until you raise effort so far that Sol costs less, or Luna passes the same checks. | 
| `gpt-6.1-sol` | GPT-6 family, current Sol | Complex coding and agent workflows that need stronger judgment | A cheaper candidate clears the same acceptance bar, or a harder task still fails. | 
| `gpt-6-sol` | GPT-6, previous Sol | An existing integration that already passes your tests | GPT-6.1 Sol passes the same tests after you adjust reasoning effort and tool-calling setup. | 
| `gpt-6-astra` | GPT-6 | The hardest end-to-end work across code, research, and tools | Its higher spend fails to improve accepted outcomes enough. | 
| `gpt-5.6-terra` | GPT-5.6, previous Terra | A working baseline or a deliberate cross-generation alternative | GPT-6.1 Terra or another GPT-6 candidate improves accepted-task cost or another operational requirement. | 

These roles paraphrase OpenAI's GPT-6 guidance, model-selection guidance, the Work and Codex model guide, which calls GPT-6.1 Terra "a fast, efficient choice for writing, visual understanding, and computer use", and the GPT-5.6 Terra model page. They are evaluation priorities, not an independent ranking of speed or pass rate. If you are moving an existing `gpt-6-sol` integration, GPT-6.1 Sol: Pricing, Access, and What Breaks When You Switch covers the parameter changes. If your question is specifically about the older three-model lineup, see the GPT-5.6 Sol, Terra, and Luna comparison.

## How much does each cost through the direct API?

The following are **OpenAI direct-API Standard rates in USD per one million tokens for requests with at most 272,000 input tokens**, from the official API pricing page as of October 28, 2026. They are token rates, not ChatGPT subscription prices or included Codex usage.

| Model ID | Uncached input | Cached input | Cache write, 30 min (default) | Cache write, 12 h | Output | 
|---|---|---|---|---|---|
| `gpt-6-luna` | $0.10 | $0.01 | $0.125 | $0.25 | $0.50 | 
| `gpt-6.1-terra` | $1.00 | $0.05 | $1.25 | $2.50 | $5.00 | 
| `gpt-6.1-sol` | $2.00 | $0.10 | $2.50 | $5.00 | $10.00 | 
| `gpt-6-sol` (previous Sol) | $2.00 | $0.20 | $2.50 | not listed | $10.00 | 
| `gpt-5.6-terra` (previous Terra) | $2.00 | $0.20 | $2.50 | not listed | $12.00 | 
| `gpt-6-astra` | $10.00 | $1.00 | $12.50 | $25.00 | $50.00 | 

The 12-hour write was added on October 20, 2026. It works only in the Responses API and costs 2.5 times the input rate, against 1.25 times for the default 30-minute write; the pricing page's full model list shows no 12-hour rate for `gpt-6-sol` or `gpt-5.6-terra`. Above 272K input, both write rates double.

The four current models sit on a clean ladder. GPT-6.1 Terra costs exactly half of GPT-6.1 Sol in every column. On input, cache writes and output, Luna costs a tenth of Terra and Astra ten times Terra; on cached input the gaps are 5 times and 20 times. The two Terras are easy to confuse on a bill: GPT-6.1 Terra charges $1 against $2 for input, $0.05 against $0.20 for cached input, and $5 against $12 for output compared with GPT-5.6 Terra.

The Sol–Terra rows deserve a careful read if you are still running the previous Terra. **GPT-6.1 Sol matches GPT-5.6 Terra on Standard short-context input ($2) and 30-minute cache writes ($2.50), costs half as much for cached input ($0.10 versus $0.20), and charges $10 for output versus GPT-5.6 Terra's $12.** The previous GPT-6 Sol differed from GPT-5.6 Terra only on output. For the *same counted token quantities and the same billing mode*, GPT-6.1 Sol's token charge is never higher than GPT-5.6 Terra's and is lower whenever cached input or output is billed. That does **not** establish a lower bill for the same completed job. The two models may use different output or reasoning tokens, produce different numbers of acceptable answers, or trigger different retries.

*The chart compares GPT-6.1 Sol with the previous GPT-5.6 Terra for a like-for-like token count; it is not a measured task-cost result. GPT-6.1 Terra is not shown because its rates are simply half of Sol's.*

Here is a deliberately hypothetical, uncached Standard request with 100,000 input tokens and 20,000 output tokens, no paid tools, and the short-context rate. Multiply each count in millions by its rate: Luna costs **$0.02** (0.1 × $0.10 + 0.02 × $0.50); GPT-6.1 Terra costs **$0.20** (0.1 × $1 + 0.02 × $5); GPT-6.1 Sol costs **$0.40** (0.1 × $2 + 0.02 × $10), the same as the previous GPT-6 Sol because nothing is cached; GPT-5.6 Terra costs **$0.44** (0.1 × $2 + 0.02 × $12); Astra costs **$2.00** (0.1 × $10 + 0.02 × $50).

Caching is where the two Sols separate: if 80,000 of those input tokens were billed as cached input, GPT-6.1 Sol would cost about **$0.25** (0.02 × $2 + 0.08 × $0.10 + 0.02 × $10), GPT-6 Sol about $0.26, GPT-5.6 Terra about $0.30, and GPT-6.1 Terra about $0.12 (0.02 × $1 + 0.08 × $0.05 + 0.02 × $5). Both examples compare an identical token tally, not likely usage or quality on a real task. For a deeper Astra-specific rate breakdown, see GPT-6 Astra API pricing.

### What happens after 272K input tokens?

Once input exceeds 272,000 tokens, the pricing rules apply the long-context rates to the **entire request**, not only the excess. For Standard processing, the long-context input/output pairs per million tokens are Luna **$0.20/$0.75**, GPT-6.1 Terra **$2/$7.50**, GPT-6.1 Sol and GPT-6 Sol both **$4/$15**, GPT-5.6 Terra **$4/$18**, and Astra **$20/$75**. Long-context cached input is $0.10 for GPT-6.1 Terra, $0.20 for GPT-6.1 Sol, and $0.40 for GPT-6 Sol. A 300K-input estimate calculated at short-context prices would therefore be wrong; check the pricing table for every category your request actually uses.

Batch and Flex are 50% below Standard where available, Fast mode is 2× Standard, and regional processing adds 10% where it applies. Ultrafast mode is listed only for Astra and GPT-6.1 Sol, not for Terra or Luna. Tool calls can add charges. Keep the model, service tier, region, token categories, and input-length band together when you compare invoices. A shared 1,050,000-token context window on the GPT-6.1 Sol, GPT-6.1 Terra, and GPT-6 Sol model pages does not make requests above 272K cheap or prove equal task performance.

## Which model should you try first for your work?

Choose the first candidate from the cost of a mistake, then give every candidate the same acceptance test. If a bad result is easy to catch with code or a quick review, start with Luna. If the work is everyday writing, reading screenshots, charts, or scanned documents, filling in forms through computer use, or routine coding that a test suite can verify, start with GPT-6.1 Terra. If the work requires substantial coding or tool decisions, start with GPT-6.1 Sol. If a task crosses several tools or is costly to repair, test Astra. If GPT-5.6 Terra is already deployed successfully, retain it as a measured baseline instead of assuming the newer generation must replace it; GPT-6.1 Terra is the natural challenger because its token rates are lower in every column. This is a trial order, not a claim that one candidate will be faster or better in your environment.

An extraction queue illustrates the tradeoff. A low token rate helps only while validation catches mistakes and reviewers are not spending the difference on fixes. A repository migration has a different failure cost: one superficially plausible but broken change can outweigh many cheap calls. For either workflow, measure end-to-end completion, not just the first model response.

OpenAI describes GPT-6.1 Sol as "near-Astra performance for complex work at a lower cost" and positions Luna for focused, repeatable work. Its GPT-6.1 Terra announcement pitches Terra at visual reasoning, computer use, and writing, and says GPT-6.1 Sol and GPT-6 Astra remain the better choice for the most complex coding. The Astra page describes its higher-capability role. Artificial Analysis moved to Intelligence Index v5.0, built from nine evaluations, between October 27 and 28, 2026, so every earlier score and per-task cost changed. On v5.0, as of October 28, GPT-6.1 Sol at high reasoning effort scores 50 at $0.59 per Index task, against 55 at $3.24 for Astra at the same effort, and Sol at xhigh (52, $0.76) already edges out Astra at medium (51, $2.73). That is a third-party harness, not your prompts. Whether Astra's gap is worth paying for is the subject of GPT-6.1 Sol vs GPT-6 Astra vs GPT-6 Sol, and GPT-6.2 Sol vs Astra. The same context limit across models is a capacity limit, not evidence of equal comprehension of a long document.

Terra against Sol is closer than the 2× token price suggests. On the same v5.0 Index (Artificial Analysis's GPT-6.1 Terra vs GPT-6.1 Sol comparison), Terra at medium scores 38 at $0.17 per task, a little above Sol at low (35 at $0.16), and Terra at high (42, $0.26) is cheaper than Sol at medium (46, $0.36) but four points lower. From xhigh up, Terra's per-task advantage is gone: Terra xhigh scores 45 at $0.40 and 287 seconds, while Sol medium scores 46 at $0.36 and 231 seconds, and Terra max (51, $0.96) costs 1.6 times as much as Sol high (50, $0.59) and takes twice as long. In a small Codex CLI test on October 27, 2026, run with one ChatGPT-subscription account over three tasks, all 17 runs passed, and at medium effort Terra used 121.5 seconds and 2.30 credits in total against Sol's 246.6 seconds and 5.56 credits. The setup and per-task results are in GPT-6.1 Terra vs GPT-6.1 Sol: Half the Price, Not Always Cheaper.

## GPT-6 Luna vs Terra: is 10× the token price worth it?

Choose GPT-6.1 Terra over GPT-6 Luna once a task needs Luna's top reasoning setting; stay with Luna when its low or medium setting already passes your checks. Terra costs 10 times Luna on Standard input, cache writes and output (5 times on cached input), but on Artificial Analysis's Intelligence Index v5.0, as of October 28, 2026, Terra at low effort scores 29 at $0.07 and 48 seconds per Index task, slightly above Luna at max, which scores 28 at $0.15 and 597 seconds. At Luna's top setting you pay twice as much per task as Terra low and wait more than ten times as long for a slightly lower score.

| As of October 28, 2026 | GPT-6 Luna | GPT-6.1 Terra | 
|---|---|---|
| Standard input / output per 1M tokens | $0.10 / $0.50 | $1.00 / $5.00 | 
| Index v5.0 score at low / max effort | 10 / 28 | 29 / 51 | 
| Cost per Index task at low / max effort | $0.01 / $0.15 | $0.07 / $0.96 | 
| Time per Index task at low / max effort | 28 s / 597 s | 48 s / 657 s | 
| Output speed | 116–124 tokens/s | 91 tokens/s | 
| Codex local messages per 5 h on Plus | 350–3,000 | 35–285 | 

Benchmark scores come from Artificial Analysis's GPT-6.1 Terra vs GPT-6 Luna comparison; the message ranges are OpenAI's estimates from the Codex pricing page, not fixed limits.

Luna keeps two real advantages. For simple work that your tests or validators already cover, it is far cheaper per task, $0.01 at low and $0.04 at medium (scoring 18), and it streams output faster. In Work and Codex, the same Plus allowance stretches to roughly ten times as many Luna messages. Both models accept text and image input and both support `none` reasoning effort, so switching between them doesn't force a different reasoning setting. If your comparison is with the previous Terra instead, GPT-5.6 Terra charges $2 for input and $12 for output, 20 and 24 times Luna's rates.

For a same-task test of both models through one API route on October 28, 2026, with pass counts, cost per passed task, and where Luna slipped, see GPT-6.1 Terra vs GPT-6 Luna: 10× per Token, 2.7–6.3× per Task.

## Run an acceptance test before changing the default

1. **Collect representative tasks.** Include ordinary cases, expensive failures, long inputs if you use them, and the tool actions your production workflow really needs. Define a clear pass rule before reading the model answers: for example, tests pass and the patch meets review criteria, or extracted fields match checked source records.
2. **Fix the comparison setup.** Record the exact model ID, prompt, input, tools, service tier, region, and reasoning effort. Compare candidates with equivalent settings where the API supports them; when support differs, record that difference instead of calling the trials identical. GPT-6 Astra and GPT-6.1 Sol do not support`none` reasoning effort; GPT-6 Sol, GPT-6 Luna, and GPT-6.1 Terra do. OpenAI suggests`low` in its place. Tool calling on GPT-6.1 Sol, GPT-6.1 Terra, and Astra requires the Responses API.
3. **Record outcomes and usage.** For each attempt, log whether it met the pass rule, input categories, output tokens (including billed reasoning output), tool charges, elapsed time, retries, and review or repair time. A token estimate from text length is weaker than returned usage and an actual bill.
4. **Compare accepted-task cost.** Divide total measured spend—including failed attempts and paid tools—by the number of accepted tasks. Keep human repair time and latency as separate columns, or translate repair time to money using your team's explicit rate. Choose the least costly candidate that meets your quality and latency requirements; keep the baseline if no challenger does.

*This diagram shows the evaluation method; the comparison cells are deliberately unfilled. GPT-6.1 Terra joins the trial the same way as any other candidate.*

For example, 100 attempted tasks with 80 accepted outcomes and $8 in model/tool spend yield **$0.10 per accepted task before labor**. If another model spends $6 but delivers only 40 accepted outcomes, its corresponding figure is **$0.15**. Those numbers illustrate the calculation; they are not observed results for any of these models. A controlled trial still needs enough representative tasks to reveal expensive failure modes.

## Does the API comparison apply to Work or Codex?

**No direct conversion exists between this per-token API table and a Work or Codex plan allowance.** OpenAI's Work/Codex model guide lists GPT-6.1 Sol, GPT-6.1 Terra, GPT-6 Sol, and GPT-6 Luna in Work and Codex. According to the Codex pricing page, GPT-6.1 Terra is available in Work and Codex on Plus, Pro, Business, Enterprise, and Edu plans, isn't available in Chat, and does not support UAE inference residency. Chat changed on October 7–8, 2026: GPT-6 in Chat is now powered by GPT-6 Sol on Plus, Pro, Business, and Enterprise, and by GPT-6 Luna on Free and Go, rolling out in stages. GPT-6.1 Sol remains a Work and Codex model, and Astra appears in Chat only as GPT-6 Pro on Pro, Business, and Enterprise plans. GPT-6.1 Sol is rolling out to Plus, Pro, Business, Enterprise, and Edu; on Enterprise and Edu it stays off until an administrator enables it, and Free and Go are not included at launch. GPT-5.6 Sol, Terra, and Luna are still listed with unchanged credit rates.

In the Codex CLI, `codex -m gpt-6.1-sol` selects the new Sol and `codex -m gpt-6.1-terra` selects Terra, which needs Codex CLI 0.159.0 or later. As of October 27, 2026, the model guide lists Terra for the desktop app, Work on the web, and the Codex CLI but not the Codex IDE extension, which it does list for Sol and Luna. What appears in a particular picker depends on the account, workspace, client, and rollout. Selecting an ID does not grant access or change workspace permissions.

For a sense of scale, OpenAI's pricing page estimates local messages per five hours on Plus at 5–45 for Astra, 15–160 for GPT-6.1 Sol, 35–285 for GPT-6.1 Terra, and 350–3,000 for Luna, and says these are not fixed message limits; Pro plans currently have no five-hour limit. Credits follow the API ladder: GPT-6.1 Terra costs 25 credits per million input tokens and 125 per million output tokens, half of GPT-6.1 Sol's 50 and 250.

For API development, check model access in the relevant project and use the exact ID in your request. Third-party gateways add new models on their own schedule: laozhang.ai's `GET /v1/models` did not return `gpt-6.1-terra` on October 27, 2026 and did on October 28. For Work or Codex, check the actual model picker and your applicable plan or credit rules. The Standard table above cannot tell you your subscription limit, purchased-credit rate, or availability in a specific account. If your current workflow is already reliable, run the acceptance test before changing a saved default.

Disclosure: laozhang.ai is our own service. Its prices are platform pricing, not an independent comparison.
