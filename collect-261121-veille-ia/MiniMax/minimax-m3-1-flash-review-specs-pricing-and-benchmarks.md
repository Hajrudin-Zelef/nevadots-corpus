---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-review-specs-pricing-and-benchmarks
title: "MiniMax M3.1 Flash Review: Specs, Pricing and Benchmarks"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["flash", "minimax", "deepseek", "model", "runs", "benchmarks", "coding", "tokens"]
source: https://aimlapi.com/blog/minimax-m3-1-flash-review
source_anchor: ""
source_lines: [1, 148]
section: "MiniMax"
---
MiniMax M3.1 Flash is MiniMax's low-cost reasoning model for agents and coding, released on October 19, 2026, with a 1M-token context. On AI/ML API it costs $0.195 per million input tokens and $0.78 per million output tokens. In our test it passed 49 of 51 runs and was the cheapest of 4 models per coding task, but it often invented facts.

*Prices, model lists, benchmark scores and test results are as of October 21, 2026.*

## What is MiniMax M3.1 Flash?

MiniMax M3.1 Flash is the fast, cheaper tier of MiniMax's M3.1 generation and the first of it to ship; MiniMax says a larger Pro model will follow later. MiniMax's launch post aims it at long agent runs: multi-step business workflows across SaaS tools, coding, operating a computer, and security work such as finding vulnerabilities.

### What changed from MiniMax M3

MiniMax says M3.1 Flash runs online inference nearly 3 times faster than MiniMax M3 and cuts the overall cost of agent tasks by 70%, with output above 200 tokens per second and a median above 170; Artificial Analysis measured 93 tokens per second, and our test 109. M3 could answer with thinking turned off, while M3.1 Flash has always-on thinking and lets you choose how deeply it reasons on 5 levels, from `low` to `max`. Video joined text and images as an input.

### MiniMax M3.1 Flash specs

## How good is MiniMax M3.1 Flash on benchmarks?

On MiniMax's own MiniMax M3.1 Flash benchmarks it leads the low-cost tier on agent work and trails DeepSeek V4.1 Flash by 3 to 7 points on coding.

### MiniMax's launch benchmarks

*DeepSeek V4.1 Flash scores for BankerToolBench, OSWorld and CyberGym are from the earlier V4.1 Flash build, which has no 1017 score; its OSWorld score is MiniMax's own reproduction. Agents' Last Exam and CyberGym are DeepSeek's own scores from its model cards (**1017**,* *V4.1 Flash**), run in DeepSeek's setup. All other DeepSeek scores are MiniMax's runs of the 1017 build.*

M3.1 Flash has the top score on Automation-Bench and LOCA-Bench, ties Claude Opus 5 on BankerToolBench, and trails DeepSeek V4.1 Flash 1017 on coding by 3 to 7 points; DeepSeek's own model cards put DeepSeek ahead of it on Agents' Last Exam and CyberGym too. MiniMax ran all of these itself, in agent tools such as Claude Code, Codex and mini-SWE-agent, so a rival can score differently in its own vendor's setup, and it did not report SWE-bench Verified, GPQA or AIME.

Sources: MiniMax launch post (the table is published as an image), DeepSeek's model cards for V4.1 Flash 1017 and V4.1 Flash.

### Independent benchmark results

M3.1 Flash is the cheapest model here per Artificial Analysis task and recalls the most facts, while DeepSeek V4.1 Flash 1017 leads both overall indexes and runs twice as fast. On Plotly's data-analysis exam it tied Claude Opus 5.5 at 41 of 43, but it wrote about 32,000 tokens per question and took 3 minutes over each, the slowest on Plotly's board.

Sources: Artificial Analysis, Vals AI, Plotly.

### What the 93% hallucination rate means

The 93% figure doesn't mean M3.1 Flash is wrong 93% of the time. It answered 56.2% of AA-Omniscience's hard factual questions correctly, ahead of DeepSeek V4.1 Flash's 42.9%, but when it didn't know an answer it almost never said so, and 92.8% of its misses were confident inventions, against 18.4% for MiniMax M3.

We checked this with 10 questions about things that don't exist, such as a Norwegian physicist, Halvard Eskeland, who doesn't exist, and a 1987 film that was never made, asking each question twice with no system prompt and twice with the line "If you are not sure that something exists or is true, say you don't know instead of guessing."

GLM 5.3 Flash declined or corrected the premise in all 40 runs, and the one-line prompt cut DeepSeek V4.1 Flash's invented answers from 6 to 1. It did not help M3.1 Flash: its declines stayed at 5 of 20, and runs that had used up the token limit returned a guess instead, so invented answers rose to 14 of 20. It gave the nonexistent Estonian town of Vellmarsk 4 different populations, from 86 to 1,300, and named 3 different directors for the nonexistent film. For factual work, give M3.1 Flash the source text in the prompt or check its answers in a second step, because a system prompt alone didn't stop it guessing.

### MiniMax M3.1 Flash vs DeepSeek V4.1 Flash

DeepSeek V4.1 Flash 1017 scores higher on both independent indexes, passed all 51 of our runs, answered twice as fast and invented fewer answers. M3.1 Flash leads MiniMax's business-workflow benchmark and matched DeepSeek on our agent, image and Three.js tasks. On AI/ML API its tokens cost half as much as DeepSeek V4.1 Flash's, but it writes more of them, so our 51 runs came to $0.46 against $0.39.

### MiniMax M3.1 Flash vs MiniMax M3

M3.1 Flash scores 14 points higher than MiniMax M3 on Artificial Analysis's index and costs 57% less per task there, $0.22 against $0.51, which is close to MiniMax's 70% claim. On AI/ML API it also has about twice M3's context at half M3's token price. Our test does not back the "3 times faster" claim: across all tasks M3.1 Flash was 10% faster per task than M3, and on coding tasks at its default `max` effort it was slower, 49 seconds against 37. M3 keeps two advantages, answering without reasoning for short calls and declining more often when it doesn't know.

## How did MiniMax M3.1 Flash do in our test?

MiniMax M3.1 Flash passed 49 of our 51 task runs, level with DeepSeek V4.1 Flash on agent, image and Three.js tasks and ahead of GLM 5.3 Flash and MiniMax M3 on Three.js, but it took twice as long per task as DeepSeek and was the most likely to invent facts.

### How we tested

- **Test matrix:** 17 tasks in 5 groups (8 coding tasks graded by hidden unit tests, a one-shot Three.js scene, 3 multi-step agent tasks with mock tools, 2 retrieval questions over a 200,000-token log, and 3 questions about images), run 3 times on each of 4 models through AI/ML API, plus the 10 questions about things that don't exist.
- **Measurement:** we streamed every response and timed the first token and the full answer on our side, which means times include network latency; cost is each response's token count times the model's AI/ML API price, reasoning tokens included.
- **Grading and settings:** hidden unit tests graded the code, the exact tool calls and their arguments graded the agent tasks, and we rendered each Three.js scene in a browser and checked it by eye and for console errors; we labelled each answer about a nonexistent thing as invented, declined or no answer by hand. Every model ran at its default reasoning setting (`max` for M3.1 Flash) and default temperature, with output limits of 65,536 tokens for code, 8,192 for the nonexistent-things questions and 16,384 for the rest, and the effort comparison ran M3.1 Flash's 8 coding tasks twice at each of the 4 lower levels. On AI/ML API,`deepseek/deepseek-v4.1-flash` also answers as`deepseek-v4.1-flash-1017` .

### Results by task type

All 4 models solved the tool-calling and image tasks every time, including a refund case where the customer's older order held the same item and a data task where one file's revenue column was in thousands. All 3 of M3.1 Flash's Three.js scenes had a titled overlay, trees, rings of waves and a turning beam, while DeepSeek V4.1 Flash built simpler islands, and GLM 5.3 Flash and MiniMax M3 each ran out of output tokens in 1 of 3 runs before finishing the code. At `max` effort M3.1 Flash spent 49 seconds on a median coding task, where DeepSeek V4.1 Flash spent 9.

### Which reasoning effort to pick

For coding tasks, start with `low`: in our test it passed 15 of 16 runs, as often as the default `max`, with 87% fewer reasoning tokens, about a quarter of the time and a sixth of the cost, $0.0010 per task against $0.0058. With 16 runs per level, a difference of 1 or 2 runs is within chance. Keep `xhigh` or `max` for long agent runs, where MiniMax's own Automation-Bench chart drops from 62.8% at `max` to 61.2% at `xhigh`, 59.2% at `high`, 57.7% at `medium` and 52.8% at `low`.

### Where it failed

M3.1 Flash missed 2 of its 51 runs. Asked for the final contract value of a vendor called Aldermoor, it answered $545,750 in 1 of 3 runs, because it added the $96,000 contract of a different vendor, Aldermore Systems, instead of stopping at $449,750. In 1 of 3 runs of a duration parser, its pattern accepted a lowercase "s" but not a capital "S", so it rejected "1D 1H 1M 1S", although the task said units were case-insensitive.

### From Space Bunny to M3.1 Flash

Before launch, M3.1 Flash spent almost a month in public under the name Space Bunny Alpha. It appeared on OpenRouter on September 23, where OpenRouter says it was the #1 model for 10 straight days and processed 56.7 trillion tokens before it was pulled on October 5. MiniMax says it ranked first on OpenCode for 12 days and puts its total across all channels above 200 trillion tokens.

On September 27, users spotted a `MiniMax-M3.1-Flash-Preview` build in MiniMax Code, the version MindStudio tested. On October 19, MiniMax confirmed that Space Bunny was an early M3.1 Flash and released the final model.

## What is MiniMax M3.1 Flash pricing on AI/ML API?

On AI/ML API, MiniMax M3.1 Flash costs $0.195 per million input tokens and $0.78 per million output tokens. Reasoning tokens are billed as output. Cached input costs $0.0195 per million. That is half the token price of DeepSeek V4.1 Flash and MiniMax M3. An average coding task from our test, with about 7,400 output tokens of which 7,100 were reasoning, cost $0.0058 on M3.1 Flash, against $0.0063 on DeepSeek V4.1 Flash and $0.0098 on MiniMax M3.

The rivals' prices on AI/ML API, per million input and output tokens, are $0.39 and $1.56 for DeepSeek V4.1 Flash and $0.195 and $0.65 for GLM 5.3 Flash, and MiniMax M3 API pricing is the same as DeepSeek's. DeepSeek's long-log cost is low because prompt caching billed most of the repeated 200,000-token prompt at DeepSeek's cached-input rate. Because it always reasons, M3.1 Flash wrote about 5,900 output tokens per task in our test, 62% more than DeepSeek V4.1 Flash, so its lower token price only partly offsets the longer answers. Lowering `reasoning_effort` closes that gap on routine tasks.

## How do you use MiniMax M3.1 Flash via API?

You need an AI/ML API key and Python 3.9 or later with the OpenAI SDK (`pip install openai`); there is nothing to download and no GPU to set up. Create a key on the API keys page, store it in the `AIMLAPI_KEY` environment variable, and point the OpenAI client at our base URL:

When we ran it, the model returned the pattern `^\d{4}-\d{2}-\d{2}$` with a note to use `datetime.date.fromisoformat()` for stricter checks. The request used 222 input tokens, 195 of them read from the cache, and 185 output tokens, of which 116 were reasoning. If you already call the MiniMax M3 API on AI/ML API, switching is a change of the model ID.

Leave out `reasoning_effort` to run at the default `max`; every parameter the model accepts is listed in our MiniMax M3.1 Flash API docs. A request with `reasoning_effort="none"` fails with HTTP 400, because the model can't run without thinking. For JSON output, add `response_format={"type": "json_object"}`.

To try the model without code, open it in the AI/ML API Playground. Besides AI/ML API, MiniMax serves the model through the MiniMax API, its M Plan coding subscription and its MiniMax Code desktop app.

## What are MiniMax M3.1 Flash's pros and cons?

Pros:

- Half the token price of DeepSeek V4.1 Flash on AI/ML API and the cheapest coding task of the 4 models in our test
- Passed every agent, image and Three.js run in our test, and 49 of 51 runs overall
- Three.js scenes with a titled overlay, trees and wave rings in all 3 runs
- The cheapest per task of its rivals on Artificial Analysis, at $0.22, and the top score on MiniMax's business-workflow and long-context agent benchmarks
- 1M-token context and video input

Cons:

- Invented answers in 10 of 20 runs of questions about things that don't exist, and in 14 of 20 when told to say it didn't know
- Twice as slow per task as DeepSeek V4.1 Flash in our test, 16.5 seconds against 8.3
- Long reasoning makes all 51 of our runs cost more than on DeepSeek V4.1 Flash, $0.46 against $0.39
- Thinking can't be turned off, and at the default `max` effort it spent a median 49 seconds per coding task
- No published weights yet, and no local or self-hosted run

## Who should use MiniMax M3.1 Flash?

MiniMax M3.1 Flash fits teams that run many agent, coding or document-reading calls whose results something else checks: unit tests, a schema validator, a tool's response or a human review. In that work it matched the other 3 models on almost every task in our test at half DeepSeek's token price. Setting `reasoning_effort` to `low` makes routine steps about 6 times cheaper and 4 times faster. Pick DeepSeek V4.1 Flash when response time matters or when answers reach users without a check, and GLM 5.3 Flash, which declined in all 40 of our runs about nonexistent things, when a model must say "I don't know". To try it, create an AI/ML API key and call `minimax/minimax-m3.1-flash`. If it doesn't fit your workload, see the 8 MiniMax M3.1 Flash alternatives we tested on coding, agent and vision tasks.

## FAQ

### Is MiniMax M3.1 Flash free?

No. On AI/ML API, MiniMax M3.1 Flash costs $0.195 per million input tokens and $0.78 per million output tokens, with reasoning tokens billed as output. In our test, all 51 runs cost $0.46 on M3.1 Flash, against $0.39 on DeepSeek V4.1 Flash and $0.75 on MiniMax M3. The weights aren't published yet, so there is no free local version today.

### Is MiniMax M3.1 Flash better than DeepSeek V4.1 Flash?

Not overall. DeepSeek V4.1 Flash 1017 scores higher on Artificial Analysis (45 against 43) and Vals, passed all 51 of our runs against 49, answered twice as fast and invented fewer facts. MiniMax M3.1 Flash leads MiniMax's business-workflow benchmark and costs half as much per token on AI/ML API.

### What is MiniMax M3.1 Flash good for?

MiniMax M3.1 Flash is good for high-volume agent and coding work that something checks automatically. In our test it passed all 9 tool-calling runs, all 9 image questions and 23 of 24 coding runs. A coding task cost $0.0058 on AI/ML API. Avoid it for factual answers without a source in the prompt, because it guesses instead of declining.

### Is MiniMax M3.1 Flash censored?

No independent test of MiniMax M3.1 Flash on politically sensitive topics has been published yet, and we didn't run one. If that matters for your use, test it on your own prompts before you rely on it. On the coding, agent and image tasks in our test, it never refused a request.

### Can you run MiniMax M3.1 Flash locally?

Not yet. MiniMax's X account named October 23, 2026, for the weights, with no license so far and no Hugging Face repo yet, so MiniMax M3.1 Flash runs only through an API. MiniMax published the M2.5, M2.7 and M3 weights, but the hardware a local copy needs is unknown, because the parameter count isn't disclosed.

### How do I get a MiniMax M3.1 Flash API key?

Sign up on AI/ML API and create a key on the API keys page; the same key works for MiniMax M3.1 Flash and every other model in our catalog. Then call `minimax/minimax-m3.1-flash` at `https://api.aimlapi.com/v1` with any OpenAI-compatible SDK, such as the OpenAI Python library. Calls are billed per token from your account balance.

## Key Takeaways

- MiniMax M3.1 Flash, released on October 19, 2026, is a 1M-context reasoning model that always thinks; `reasoning_effort` from`low` to`max` sets how long.
- In our 51-run test on AI/ML API it passed 49, against 51 for DeepSeek V4.1 Flash and 50 each for GLM 5.3 Flash and MiniMax M3, and took twice as long per task as DeepSeek.
- It rarely admits not knowing: it invented answers in 10 of our 20 runs about nonexistent things, and Artificial Analysis measured a 92.8% hallucination rate, although it answered 56.2% of that test correctly, the most in its tier.
- On our coding tasks, `low` effort passed 15 of 16 runs, as often as the default`max` , with 87% fewer reasoning tokens and a quarter of the time.
- MiniMax's benchmarks are self-reported; independently, DeepSeek V4.1 Flash leads Artificial Analysis and Vals, while M3.1 Flash is cheapest per task on Artificial Analysis, $0.22 against $0.26.
- MiniMax M3.1 Flash costs $0.195 per million input tokens and $0.78 per million output tokens on AI/ML API: create a key and call `minimax/minimax-m3.1-flash` .
