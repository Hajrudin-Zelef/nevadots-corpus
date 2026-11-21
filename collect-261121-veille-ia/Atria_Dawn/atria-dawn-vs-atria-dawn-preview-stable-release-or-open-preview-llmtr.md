---
id: collect-261121-veille-ia/atria-dawn/atria-dawn-vs-atria-dawn-preview-stable-release-or-open-preview-llmtr
title: "Atria-Dawn vs Atria-Dawn Preview: stable release or open preview | LLMTR"
domain: atria-dawn
role: reference
task: article
actors: []
dates: []
keywords: ["atria-dawn", "preview", "release", "llmtr", "stable", "open", "same", "model"]
source: https://llmtr.com/en/blog/atria-dawn-vs-atria-dawn-preview
source_anchor: ""
source_lines: [1, 96]
section: "Atria_Dawn"
---
## Short answer: Atria-Dawn for a stable flow, Atria-Dawn Preview for trying the open release

LLMTR is a gateway that gives access to language models from different providers through one OpenAI-compatible API. Shanghai AI Lab's Atria-Dawn (internlm/atria-dawn-free) and Atria-Dawn Preview (internlm/atria-dawn-preview-free) are called here on the /v1/chat/completions endpoint at https://llmtr.com/v1 with the same API key, and both are free until 31 October 2026, 23:59 (TRT). For a flow that will keep running on LLMTR, choose Atria-Dawn, the stable release; to try the open release you may plan to run on your own servers, choose Atria-Dawn Preview, whose weights are open under the MIT license. If the input contains images, neither fits; Intern-S2 (internlm/intern-s2-free) and Agents-A1 (internlm/agents-a1-free) accept images. From 1 November 2026, use Nemotron 3 Ultra (nvidia/nemotron-3-ultra-550b-a55b) or Agnes 2.5 Flash (agnes/agnes-2.5-flash) to keep going at no cost, or DeepSeek V4.1 Flash (deepseek/deepseek-flash) or MiniMax M3.1 Flash (minimax/minimax-m3.1-flash) for a paid row.

The cards of the two rows in the LLMTR catalog list the same capabilities; the difference is in how they describe the release. The Atria-Dawn card presents it as an agent model for research and engineering work that follows its environment, uses tools and completes multi-step tasks: it is designed to run problem analysis, solution design, coding, experiment runs, result review and recovery from failures in one loop. The Atria-Dawn Preview card says it is the preview release of the same model, that its weights are published openly under the MIT license, and that Atria-Dawn is the one to use for the stable release.

## Atria-Dawn or Atria-Dawn Preview?

The decision rests on one question: will your flow keep running on LLMTR, or will you later move the model to your own servers? Atria-Dawn is the stable release. The weights of Atria-Dawn Preview are MIT-licensed, so you can first build a flow you plan to move to your own servers on the preview and compare its output with your own samples. The API settings of the two rows are the same, so switching between them only means changing the model id.

The table puts the two rows side by side with the card values in the LLMTR catalog. The context window is shared between the prompt and the output; no separate output limit is listed.

| Atria-Dawn and Atria-Dawn Preview in the LLMTR catalog as of 22 October 2026 |  |  | 
|---|---|---|
| Property | Atria-Dawn | Atria-Dawn Preview | 
|---|---|---|
| Release | stable release | preview release of Atria-Dawn; open weights under the MIT license | 
| How the card describes it | an agent model for multi-step research and engineering work with tools | for trying the open release you may plan to run on your own servers; points to Atria-Dawn for the stable release | 
| Context window | 524,288 tokens | 524,288 tokens | 
| Input | text only; a request with an image returns 400 | text only; a request with an image returns 400 | 
| JSON (response_format) | json_object and json_schema | json_object and json_schema | 
| tool_choice | auto, required or a named function; none with tools attached returns 400 | same as Atria-Dawn | 
| Reasoning | on by default; turned off with "reasoning": false or :fast | on by default; turned off with "reasoning": false or :fast | 
| Price | free until 31 October 2026, 23:59 (TRT) | free until 31 October 2026, 23:59 (TRT) | 
| From 1 November 2026, 00:00 (TRT) | 410 model_retired, no redirect | 410 model_retired, no redirect | 

## Sending the same task to both ids and comparing

Because both rows accept the same request shape, the comparison is a single loop: send the same messages to both ids and read finish_reason, the token counts in usage and the length of the reasoning text side by side. Reasoning is on by default and reasoning tokens count toward max_tokens, which is why max_tokens is 16,384 in the example. If the response comes back with finish_reason: "length", the answer was cut off at max_tokens; raise the limit. reasoning_content is not a standard OpenAI field, so the example reads it safely with getattr.

The allowance is separate for each account and for each model: 500,000 tokens a day, input and output combined, reasoning tokens included. It renews every day at 00:00 (TRT). Sending the same task to both models draws on two separate allowances; using up one does not reduce the other. Once an allowance is used up, the request returns 429 rate_limit_error and the Retry-After header tells you when to try again.

OpenAI Python SDK example that sends the same planning task to Atria-Dawn and Atria-Dawn Preview through https://llmtr.com/v1/chat/completions and prints finish_reason, the token counts and the length of the reasoning text.

```
import os
from openai import OpenAI
client = OpenAI(base_url="https://llmtr.com/v1", api_key=os.environ["LLMTR_API_KEY"])
# Same task on both Atria rows; reasoning stays on (the default).
# Reasoning tokens count toward max_tokens, so keep the limit generous.
MODELS = ["internlm/atria-dawn-free", "internlm/atria-dawn-preview-free"]
messages = [
    {"role": "system", "content": "You are a senior backend engineer."},
    {"role": "user", "content": "Write a numbered plan to add rate limiting to a REST API, with a rollback for each step."},
]
for model in MODELS:
    response = client.chat.completions.create(model=model, messages=messages, max_tokens=16384)
    choice = response.choices[0]
    reasoning = getattr(choice.message, "reasoning_content", None) or ""
    usage = response.usage
    print(model, choice.finish_reason, usage.prompt_tokens, usage.completion_tokens, len(reasoning))
    print((choice.message.content or "")[:300])
```
## Shared settings: JSON, tool choice and reasoning

Both rows accept json_object and strict json_schema in response_format. For tool calling, send auto, required or a named function as tool_choice. With tools attached, tool_choice: "none" returns 400; in a request where you will not use tools, leave the tools field out entirely. A message that contains an image also returns 400, because both rows accept text only; turn the image into text first or choose a row that accepts images.

Reasoning is on by default on both rows and the reasoning text arrives in the reasoning_content field. To turn it off, add "reasoning": false to the request body (in Python, extra_body={"reasoning": False}) or append :fast to the model id; :think or "reasoning": true keeps it on. reasoning_effort returns 400 on these rows, because there are no levels, only on and off. When streaming, send "include_usage": true inside stream_options and usage arrives in the last chunk. No cache discount is listed; sending the same long prompt again counts as full input every time.

## Which one when?

The choices below follow from the difference between the two cards, the data notice and what happens after 1 November 2026; compare output quality with your own samples.

- An agent or long-text flow that will run on LLMTR and needs the stable release: Atria-Dawn.
- If you plan to run the model on your own servers later: try the same flow on Atria-Dawn Preview; its weights are open under the MIT license.
- If the input contains images: neither fits, a request with an image returns 400; Intern-S2 or Agents-A1 accept images.
- Sensitive, personal or customer data: use neither; the data notice says prompts and completions may be logged by the provider and used to improve the model.
- After 31 October 2026, 23:59 (TRT): Nemotron 3 Ultra or Agnes 2.5 Flash to keep going at no cost, DeepSeek V4.1 Flash or MiniMax M3.1 Flash for a paid row.

## Which row to move to after 1 November 2026?

After 31 October 2026, 23:59 (TRT), that is from 1 November 2026, 00:00, both rows return 410 model_retired and requests are not redirected to another model; they have no successor, so you have to change the model id yourself. To keep going at no cost, Nemotron 3 Ultra (nvidia/nemotron-3-ultra-550b-a55b) offers a 1,000,000-token context and JSON mode, runs on a daily quota and has reasoning off by default. Agnes 2.5 Flash (agnes/agnes-2.5-flash) offers a 524,288-token context, json_schema and tool calling; its quota counts requests, not tokens, and a single response can be at most 65,536 tokens. The data notice of Nemotron 3 Ultra also says prompts may be logged; do not use any of the free rows in this article for sensitive data.

On the paid side, DeepSeek V4.1 Flash (deepseek/deepseek-flash) runs at $0.15 input and $0.60 output per 1M tokens outside peak hours; on weekdays between 04:00-07:00 and 09:00-13:00 (TRT), Chinese public holidays excepted, every item costs double. It has JSON mode but does not support json_schema. MiniMax M3.1 Flash (minimax/minimax-m3.1-flash) offers a 1,000,000-token context at $0.135 input and $0.54 output; its reasoning cannot be turned off, and its depth is set with reasoning_effort from low to max. When you move, change the model id and the JSON and tool_choice settings to match the new row's card, and top up your balance for a paid row.

## Comparing Atria-Dawn and Atria-Dawn Preview on the same task

Send the same task to both Atria ids with one LLMTR key, compare the answer and the usage values, then choose the id.

1. **Key.** Create an API key on llmtr.com under Dashboard > API Keys.
2. **Same task.** Send your own task with both model ids to the https://llmtr.com/v1/chat/completions endpoint; give max_tokens a generous value.
3. **Choice.** Compare the answers, finish_reason and the usage values; choose the preview if you plan to move to your own servers, Atria-Dawn otherwise.

## Frequently asked questions

### Do Atria-Dawn and Atria-Dawn Preview have different API settings?

No. Both accept a 524,288-token context, text-only input, json_object and json_schema, and auto, required and named tool_choice; with tools attached, tool_choice: "none" returns 400 on both. Only the model id changes.

### Can I run Atria-Dawn Preview on my own servers?

According to its card on LLMTR, the weights of Atria-Dawn Preview are published openly under the MIT license, and the card recommends it for trying the open release you may plan to run on your own servers.

### What happens to a request sent to Atria-Dawn after 1 November 2026?

It returns 410 model_retired and is not redirected to another model. To keep going at no cost after 31 October 2026 choose Nemotron 3 Ultra or Agnes 2.5 Flash; for a paid row choose DeepSeek V4.1 Flash or MiniMax M3.1 Flash, and change the model id.
