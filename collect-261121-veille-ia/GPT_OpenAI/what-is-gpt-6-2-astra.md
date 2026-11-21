---
id: collect-261121-veille-ia/gpt-openai/what-is-gpt-6-2-astra
title: "What Is GPT-6.2 Astra?"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "astra", "model", "input", "output", "tokens", "same", "price"]
source: https://apidog.com/blog/what-is-gpt-6-2-astra-sol/
source_anchor: ""
source_lines: [1, 153]
section: "GPT_OpenAI"
---
GPT-6.2 Astra is OpenAI’s new frontier model for hard, long-running work, released on November 17, 2026 alongside GPT-6.2 Sol, a cheaper everyday model. Astra costs $8 input and $40 output per million tokens; Sol costs $2 and $10. Both share a 1.05M-token context window, 128K output tokens and the API IDs `gpt-6.2-astra` and `gpt-6.2-sol`.

So what is GPT-6.2 Astra, and should you switch? The short version: Astra is 20% cheaper than GPT-6 Astra and beats it on OpenAI’s own charts, but early reviews from developers are mixed. Sol keeps the old Sol price and gets better on most tests. This guide covers what each model is, what it costs, where you can use it, and how to pick between them.

## What is GPT-6.2 Astra?

GPT-6.2 Astra is OpenAI’s top model as of November 2026, described in the launch post as its “new frontier model for your hardest projects.” It replaces GPT-6 Astra at the top of the lineup, reasons at configurable effort levels, accepts up to 922K input tokens and returns up to 128K output tokens. Its knowledge cutoff is May 18, 2026.

Astra is the model you reach for when a task is long, multi-step and expensive to get wrong: refactors across a large codebase, agent runs that last hours, legal or scientific analysis, and computer use. OpenAI ships it in the API, in Codex and in ChatGPT Work.

If you used the previous generation, the GPT-6 Astra API guide still describes the request shape. The model ID is the main thing that changes.

## What is GPT-6.2 Sol?

GPT-6.2 Sol is the efficient sibling, which OpenAI calls its “efficient, versatile model for everyday work.” It has the same 1.05M context window and 128K output limit as Astra, a slightly later knowledge cutoff (June 1, 2026), and a price four times lower. OpenAI says many of its own employees use Sol as their default model.

Sol fits chat assistants, retrieval pipelines, classification, code review on normal-sized diffs and anything you call thousands of times a day. It’s the successor to GPT-6.1 Sol, at the same list price.

## GPT-6.2 Astra vs GPT-6.2 Sol at a glance

Both models come from the same release and share most of their plumbing. The differences are price, cutoff date and how hard each one thinks. These figures come from OpenAI’s model pages for GPT-6.2 Astra and GPT-6.2 Sol, checked on November 18, 2026.

| Spec | GPT-6.2 Astra | GPT-6.2 Sol | 
|---|---|---|
| API model ID | `gpt-6.2-astra` | `gpt-6.2-sol` | 
| Input, per 1M tokens | $8.00 | $2.00 | 
| Cached input, per 1M | $0.40 | $0.10 | 
| Cache writes, per 1M (30-min retention) | $10.00 | $2.50 | 
| Output, per 1M tokens | $40.00 | $10.00 | 
| Context window | 1,050,000 | 1,050,000 | 
| Max input | 922,000 | 922,000 | 
| Max output | 128,000 | 128,000 | 
| Knowledge cutoff | May 18, 2026 | June 1, 2026 | 
| Endpoints | Responses, Chat Completions, Batch | Responses, Chat Completions, Batch | 

Both models reject requests that set reasoning effort to `none`, so every call does some reasoning. Developers testing on launch day ran them at `high`, `xhigh` and `max` effort.

## How much does GPT-6.2 Astra cost?

Astra lists at $8 input and $40 output per million tokens. GPT-6 Astra was $10 and $50, so the new model costs 20% less per token. Sol stays at $2 and $10, the same as GPT-6.1 Sol and GPT-6 Sol.

A few pricing details matter more than the headline rate:

- **Long prompts cost more.** VentureBeat reports that requests above 272K input tokens are billed at higher rates, with Astra rising to $16 input and $60 output.
- **Batch is half price** for jobs that can wait, per the same report.
- **Fast and Ultrafast modes cost more** : Fast runs at twice the standard rate and Ultrafast starts at three times, VentureBeat adds.
- **Effort level drives the bill.** Simon Willison’s launch-day test drew the same SVG image with each model. Astra cost 11.55 cents at high effort and 88.71 cents at max. Sol cost 2.41 cents at high and 13.69 cents at max.

That last point is the one teams miss. Per-token price is only half the story; a model that thinks longer spends more tokens. Measure cost per finished task, not cost per million tokens.

For a deeper look at the cost trade-off between the two tiers in the previous generation, see GPT-6 Astra to GPT-6 Sol: the migration.

## Where can you use GPT-6.2 Astra and Sol?

OpenAI’s launch announcement on the developer forum lists three places:

1. **The API** , as`gpt-6.2-astra` and`gpt-6.2-sol` , on the Responses, Chat Completions and Batch endpoints.
2. **Codex** , in the desktop app and the CLI, for Plus, Pro, Enterprise and Edu users.
3. **ChatGPT Work** , for the same plans. OpenAI notes that this rollout does not add either model to regular Chat, and access depends on plan, workspace permissions and rollout timing.

Pro 500 subscribers get Ultrafast on both models. Pro 200 subscribers get Ultrafast on Sol. Enterprise and Edu admins can turn it on for eligible workspaces.

In the Responses API, both models support web search, file search, image generation, code interpreter, hosted shell, apply patch, skills, computer use, MCP and tool search. Realtime, Assistants and fine-tuning aren’t supported.

## What changed from GPT-6 Astra?

OpenAI’s launch charts compare Astra with GPT-6 Astra and Sol with GPT-6.1 Sol across nine benchmarks. Astra wins all nine. Sol wins eight and slips on one. These are the scores VentureBeat transcribed from the launch post:

| Benchmark | 6.2 Astra | 6 Astra | 6.2 Sol | 6.1 Sol | 
|---|---|---|---|---|
| Agents’ Last Exam | 78.3% | 73.1% | 73.0% | 72.5% | 
| Terminal-Bench-Science | 77.1% | 68.1% | 68.0% | 57.0% | 
| BenchCAD 2.1 Core | 82.5% | 62.6% | 73.1% | 52.6% | 
| AutomationBench | 52.4% | 41.4% | 41.2% | 36.1% | 
| Vals Science Index | 68.4% | 65.1% | 58.5% | 59.6% | 

Independent numbers are closer to flat. Artificial Analysis puts Astra at 62 on its Intelligence Index, five points above GPT-6 Astra and second behind Claude Fable 5.2. Its coding-agent score for Astra didn’t move, and Sol’s dropped three points.

OpenAI also says both models reach its “Critical” cybersecurity threshold, so they ship with stricter safeguards than the last generation.

## Why are early reviews mixed?

The launch drew sharper criticism than most OpenAI releases, as VentureBeat’s roundup shows. Developer Theo Browne reported that on long tasks the model “can pull in a lot of subagents” and then stalls on odd internal questions. Ethan Mollick called Astra “very uneven & weird” and harder to steer. Dan Shipper said it often stopped early on end-to-end builds.

The praise is specific too. Browne credited Astra with strong code review, 3D work and computer use. Others singled out animation and video editing. Simon Willison found Astra’s max-effort output the most detailed in his test, while noting it trails Fable 5.2 on several benchmarks.

The practical takeaway: don’t swap your production model on launch day because a chart moved. Run your own prompts against both models and compare the results you actually ship.

## Which one should you use?

Pick by workload, then confirm with your own evals:

- **Use GPT-6.2 Sol** for high-volume calls, chat, retrieval, extraction and routine coding. It’s a quarter of the price and improved on eight of nine OpenAI benchmarks.
- **Use GPT-6.2 Astra** for the hardest 10% of tasks: long agent runs, big refactors, CAD and science work, and computer use, where a better answer saves more than the 4x price.
- **Start on Sol and escalate.** Route a request to Astra only when Sol fails a check. That keeps most traffic at $2 and $10.

Calling either model is a one-line change if you already use the Responses API:

```
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-6.2-sol",
    "reasoning": { "effort": "high" },
    "input": "Summarize the breaking changes in this changelog."
  }'
```
Swap `gpt-6.2-sol` for `gpt-6.2-astra` to compare. Keep the prompt, effort level and input identical so the comparison is fair.

## Let your coding agent test both models with Apidog CLI

Comparing two models by pasting prompts into a chat window doesn’t scale, and it doesn’t catch regressions next month. Let your coding agent write the checks and run them with Apidog CLI instead. You write one prompt; the agent does the setup.

```
npm install -g apidog-cli@latest
apidog login --with-token <YOUR_TOKEN>
npx -y skills add https://apidog.com   # pick your agent when prompted
```
Then give Claude Code, Codex or Cursor one prompt:

```
In Apidog project <projectId>, create a test case for POST https://api.openai.com/v1/responses
that sends the same prompt to gpt-6.2-sol and gpt-6.2-astra at "high" effort.
Assert status 200, that the response "model" field starts with the requested ID,
and that usage.output_tokens is under 4000. Run it against the <envId> environment
and summarize latency and token usage for each model.
```
The agent reads the payload schema, validates its file, creates the case and runs it:

```
apidog cli-schema get test-case-create
apidog cli-schema validate test-case-create --file case.json
apidog test-case create --project <projectId> --file case.json
apidog run --test-case <caseId> --project <projectId> -e <envId> -r cli,json
```
Store `OPENAI_API_KEY` as an environment variable in Apidog, not in the prompt, so the key never sits in your agent’s context. Add `--upload-report` if you want to review the run in the Apidog app afterwards.

The same `apidog run` command with `-r junit` drops into a pipeline, so the comparison reruns whenever OpenAI updates either model. The Apidog CLI in CI/CD guide shows the setup, and Apidog CLI in Codex covers the agent side. Apidog’s Free plan covers teams of up to 4 users.

## FAQ

**What is GPT-6.2 Astra in one sentence?** It’s OpenAI’s frontier model for hard, long-running tasks, released November 17, 2026, priced at $8 input and $40 output per million tokens with a 1.05M-token context window.

**Is GPT-6.2 Astra better than GPT-6 Astra?** On OpenAI’s charts, yes: it wins all nine head-to-head benchmarks and costs 20% less. Independent tests show smaller gains, and some developers report it’s harder to steer on long tasks.

**Is GPT-6.2 Sol free?** No. Sol costs $2 input and $10 output per million tokens in the API. In ChatGPT, both models are in ChatGPT Work and Codex for Plus, Pro, Enterprise and Edu plans, not in regular Chat.

**What is the GPT-6.2 Astra context window?** 1,050,000 tokens, with up to 922,000 input tokens and 128,000 output tokens. Sol has the same limits.

**Can I use GPT-6.2 Astra in Codex?** Yes. Both models are in the Codex desktop app and CLI for Plus, Pro, Enterprise and Edu users, and Pro 500 subscribers can use Ultrafast with either model.

**Should I switch from GPT-6.1 Sol to GPT-6.2 Sol?** It costs the same, so the switch is cheap to test. Run your own prompts on both first: Sol improved on eight of nine OpenAI benchmarks but dropped three points on an independent coding-agent score.
