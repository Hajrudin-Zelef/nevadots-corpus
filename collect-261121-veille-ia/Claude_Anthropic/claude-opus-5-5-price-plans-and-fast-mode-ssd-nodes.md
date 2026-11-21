---
id: collect-261121-veille-ia/claude-anthropic/claude-opus-5-5-price-plans-and-fast-mode-ssd-nodes
title: "Claude Opus 5.5: price, plans and fast mode · SSD Nodes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["opus", "mtok", "input", "output", "claude", "tokens", "label", "costs"]
source: https://www.ssdnodes.com/learn/claude-opus-5-5-price-and-plans
source_anchor: ""
source_lines: [1, 198]
section: "Claude_Anthropic"
---
## How much does Claude Opus 5.5 cost?

Claude Opus 5.5 costs **$4** per million input tokens and **$20** per million output tokens on the Claude API, as of October 2026. Fast mode doubles both rates and gives you faster output in return. Opus 5.5 is included in the Pro, Max, Ultra, Team and Enterprise plans. The Free plan gets Sonnet and Haiku, not Opus.

The prices in this guide come from the Claude pricing page and from Anthropic's Opus 5.5 announcement of 22 September 2026. They were checked on 13 November 2026. Prices change, so recheck the pricing page before you plan a budget around them.

Two terms first. A token is a small piece of text, often part of a word, and Claude bills by the token. MTok means one million tokens, and every price here is per MTok. To get an idea of how much text that is, read how much text one million Claude tokens really holds.

## Opus 5.5 API prices: input, output and prompt caching

## The data behind this chart

```
[
  {
    "label": "Input",
    "usd_per_mtok": 4
  },
  {
    "label": "Output",
    "usd_per_mtok": 20
  },
  {
    "label": "Cache write",
    "usd_per_mtok": 5
  },
  {
    "label": "Cache read",
    "usd_per_mtok": "0.20"
  }
]
```
Input tokens are everything you send: your prompt plus everything sent with it, such as attached files and earlier turns of the conversation. Output tokens are the text the model writes back. Output costs five times as much as input here. A task that writes long answers therefore costs far more than a task that reads a lot and answers briefly.

The two cache rows matter most for coding agents. Prompt caching lets you mark the long, unchanging start of a prompt, such as a system prompt or a set of project files, so later requests can reuse it. The first request writes that prefix to the default five-minute cache at **$5** per MTok, a little more than normal input. A one-hour cache write costs $8 per MTok. Every later request that reuses it reads it at **$0.20** per MTok, which is one twentieth of the normal input rate. Claude Code sends your project context again on every turn, so much of its input is billed as cache reads. That is why a long coding session costs less than its raw token count suggests. How Claude Code spends tokens explains where those tokens go.

## How does Opus 5.5 compare with Opus 5, Sonnet 5.5 and Fable 5.1?

## The data behind this chart

```
[
  {
    "label": "Opus 5.5",
    "input_usd_per_mtok": 4,
    "output_usd_per_mtok": 20
  },
  {
    "label": "Opus 5",
    "input_usd_per_mtok": 5,
    "output_usd_per_mtok": 25
  },
  {
    "label": "Sonnet 5.5",
    "input_usd_per_mtok": 2,
    "output_usd_per_mtok": 10
  },
  {
    "label": "Fable 5.1",
    "input_usd_per_mtok": 10,
    "output_usd_per_mtok": 50
  },
  {
    "label": "Opus 5.5 fast mode",
    "input_usd_per_mtok": 8,
    "output_usd_per_mtok": 40
  }
]
```
Opus 5 lists at **$5** input and **$25** output per MTok. Opus 5.5 is 20% lower on both. Sonnet 5.5, at **$2** and **$10**, costs half as much per token as Opus 5.5. Fable 5.1, at **$10** and **$50**, costs 2.5 times as much.

The last row is Opus 5.5 in fast mode, at **$8** and **$40**. It is the same model, so the answers do not get better. You pay for speed only. Even so, it costs less per token than Fable 5.1.

A lower price does not make a model the right one. For a simple task, Sonnet 5.5 at half the price will often do the job just as well. For the hardest work you have, a stronger model can finish in fewer attempts, and fewer attempts means fewer tokens. Choosing between Opus, Sonnet and Haiku covers that decision. Claude Fable 5 pricing and the jobs it suits covers the model above Opus.

## Is Opus 5.5 really 40% cheaper than Opus 5?

Not on the input and output rates. Two different figures describe Opus 5.5, and they measure different things.

The input and output list prices fell 20%. You can check this in the chart above: each Opus 5.5 rate is four fifths of the Opus 5 rate. Cache writes fell by the same 20%, from $6.25 to $5 per MTok. Cache reads fell further, by 60%, from $0.50 to $0.20 per MTok. Anthropic prices Opus 5.5 cache reads at 5% of the input rate, while Opus 5 and most other models pay 10%.

Anthropic's announcement also says Opus 5.5 is "40% less to run than Opus 5". That is Anthropic's claim about the cost to finish a task. It is not a price. The cost of a task is the tokens it uses multiplied by the price per token. The announcement gives two reasons the task cost falls by more than the 20% cut on input and output. Cache reads, which it says "make up the majority of agentic and coding work costs", are 60% cheaper. And Opus 5.5 "uses fewer tokens per task" in Anthropic's tests. Your tasks are not Anthropic's tasks. To see what you save, run the same job on both models and compare the token counts each one reports, including the cache read counts.

## Which Claude plans include Opus 5.5?

The Claude pricing page lists Opus in the Pro, Max, Ultra, Team and Enterprise plans. The Free plan gets Sonnet and Haiku only. If you are on Free and need Opus, the cheapest upgrade is Pro, described in Claude Pro's price and usage limits. If you already hit Pro's limits, the comparison of Max and Ultra shows what each one adds. Max now has one $100/month tier; the former Max 20x plan is now Ultra at $200/month for web subscriptions. Ultra includes 10 times Pro's weekly usage and has no five-hour limit for verified accounts.

Anthropic also raised the five-hour usage limits on Pro, Max, Team and seat-based Enterprise plans with this release. The five-hour limit is the window that decides how much you can use before you have to wait for it to reset. At launch on 22 September 2026, subscribers also got a rate limit reset, which Anthropic said they could save and use whenever they chose. The help center says an unused reset expires at the time shown on the offer, so check Settings > Usage before you count on it. The announcement does not publish a message count for any plan, so this guide does not give one either. A message count would mislead you anyway, because one message with large files attached uses far more of your limit than a short question. How Claude's usage limits work explains the window in detail.

The announcement gives no context window for Opus 5.5, but Anthropic's model page for it does. The context window is the largest amount of text the model can read in one request. For Opus 5.5 it is 1 million tokens, and the model writes up to 128,000 output tokens per response on the standard Messages API.

## Where else can you run Opus 5.5?

Outside Anthropic's own API, Opus 5.5 is available on AWS, Google Cloud and Azure. This is useful if your company already buys cloud services from one of them, because the usage is charged on the same bill. A few details differ from the Anthropic API. On Amazon Bedrock the model ID is `anthropic.claude-opus-5-5`. Google Cloud, Microsoft Foundry on Azure and Claude Platform on AWS use the same `claude-opus-5-5` as the Claude API. On Bedrock and on the Google-managed Claude offering on Google Cloud, the price you pay is the one on the provider's own pricing page, which this guide does not quote. Microsoft Foundry and Claude Platform on AWS bill through the cloud marketplace at Anthropic's standard per-token rates. Google Cloud also has an Anthropic-managed Claude offering, which bills through Google Cloud Marketplace with the usage metered by Anthropic. Fast mode for Opus 5.5 is available on that Anthropic-managed Google Cloud offering and on Microsoft Foundry deployments hosted on Azure. It is not available on Bedrock or Claude Platform on AWS. Claude Code itself supports fast mode through the Anthropic API, a Claude subscription and Google Cloud's Agent Platform. On Agent Platform, fast mode supports Fable 5.2 only, requires Claude Code v2.1.322 or later and needs the `global`, `us` or `eu` region. This Agent Platform support does not extend to Opus 5.5.

## How to use Opus 5.5 in Claude Code

Claude Code is Anthropic's coding agent for the terminal. If it is not installed yet, follow the guide to running Claude on the Linux desktop and in the CLI, then come back here.

Start a session pinned to Opus 5.5 with the full model ID:

`claude --model claude-opus-5-5`
Inside a running session, type `/model` to open the model picker and switch without restarting. The switch takes effect immediately. Press Enter to switch and save the choice as your default, or press `s` to switch for this session only. Type `/status` to confirm which model the session uses. It should name Opus 5.5.

If it names another model, look in the transcript for a notice that the model switched. Opus 5.5 runs with safety classifiers, and Claude Code re-runs cybersecurity-flagged requests on Opus 4.8 and biology-flagged requests on Opus 5 by default. Anthropic's help center adds that Opus 5.5 also falls back to Opus 5 for a small set of frontier LLM development requests, and that the model picker then stays on the less capable model for the rest of the conversation. Distillation flags block the request outright, without falling back to another model. To be asked before any switch, choose **Ask each time** in `/config` or set `switchModelsOnFlag` to `false`. If there is no switch notice, run `/model` again and pick Opus 5.5 from the list.

To make Opus 5.5 the default for every session, set it in your user settings file at `~/.claude/settings.json`:

```
{
  "model": "claude-opus-5-5"
}
```
You can also set it for one shell with an environment variable:

```
export ANTHROPIC_MODEL=claude-opus-5-5
claude
```
The short alias `opus` works too. On the Anthropic API it resolves to Opus 5.5 from Claude Code v2.1.280 on, but aliases point to the recommended version and update over time. When a newer Opus is released, the alias moves, and your costs move with it. On some clouds the alias points to an older model: on Microsoft Foundry, `opus` resolves to Opus 4.6. To stay on Opus 5.5, pin the full ID, as the commands above do.

Note the hyphens. The ID is `claude-opus-5-5`, not `claude-opus-5.5`. A dotted ID is not a valid model name, so the API rejects the request. The API check further down shows the error format.

### Turn fast mode on and off

Type `/fast` in a Claude Code session to toggle fast mode. Anthropic describes it as "up to 2.5x speed" for the same Opus 5.5 model, at the doubled rate shown in the chart above. On the Claude API, fast mode on Opus 5.5 needs no access request or beta header: organizations in the Start, Build and Scale usage tiers get a default fast mode rate limit, while organizations still on the free Evaluation tier cannot use it. Opus 5.5 fast mode is available without the research-preview designation; fast mode on Opus 5 is in research preview, while fast mode on Opus 4.8 is deprecated. Fast mode on Opus 4.8 was deprecated on 30 October 2026 and will be removed on 30 November 2026. Leave it off by default. Turn it on for the work where you are actively waiting for the answer, and do it at the start of a session. The first time you turn it on in a conversation, you pay the full uncached fast mode input rate for the whole conversation so far.

## When does fast mode pay for itself?

Fast mode doubles both the input rate and the output rate. For identical token counts and the same mix of uncached input, cache writes and cache reads, fast mode costs exactly twice standard speed. Switching speeds mid-conversation changes that mix because fast mode and standard speed keep separate caches. The only question is whether the time you save is worth that second bill.

Here is one worked example, computed from the list prices only. Take a single agent task that sends 2 million input tokens and receives 200,000 output tokens. The example ignores caching to keep the arithmetic simple. Anthropic applies its caching multipliers on top of the fast mode rates, so a cache read in fast mode costs $0.40 per MTok, twice the standard rate. Real Claude Code sessions use the cache heavily, so your cost at either speed will usually be lower than this.

## The data behind this chart

```
[
  {
    "label": "Standard Opus 5.5",
    "input_cost_usd": 8,
    "output_cost_usd": 4,
    "total_cost_usd": 12
  },
  {
    "label": "Fast mode",
    "input_cost_usd": 16,
    "output_cost_usd": 8,
    "total_cost_usd": 24
  }
]
```
At standard speed the task costs **$8** for input plus **$4** for output, **$12** in total. In fast mode it costs **$24**. The extra charge is the same as the whole standard bill, **$12**.

Now add time. Assume the task takes 60 minutes at standard speed. At the full 2.5x speed-up it would take 24 minutes, which saves 36 minutes for the extra charge. You are paying about $20 for each hour you save (12 divided by 0.6). If you only get 1.5x, the task takes 40 minutes and you save 20 minutes, so each saved hour now costs $36. "Up to 2.5x" refers to output tokens per second, so a whole task with tool calls and other waiting does not get the same speed-up. Plan with a lower figure until you have timed your own work.

So fast mode pays for itself when you sit and wait for the result, and an hour of your waiting time is worth more than that figure. Short interactive loops fit that description, such as fixing a failing test while a release waits on it. Long jobs you start and leave do not fit, such as overnight refactors or batch runs. Nobody is waiting, so the speed buys nothing.

On a subscription the picture is different. The per-token rates above are what you pay on the API. If you run Claude Code on a Pro, Max, Ultra, Team or Enterprise plan, fast mode is not covered by your plan's usage limits. You pay for it only from usage credits, which must be turned on first, and it draws on them even when your plan still has usage left. On Team and Enterprise, an Owner also has to enable fast mode for the organization. Comparing API billing with a subscription helps you decide which way of paying fits how you work.

## Check the model ID and your token use with one API call

If you pay through the API, one request confirms that your key can reach Opus 5.5. It also shows exactly what the request was billed for. Set your key in `ANTHROPIC_API_KEY` first.

```
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model": "claude-opus-5-5", "max_tokens": 64, "messages": [{"role": "user", "content": "Reply with one word."}]}'
```
A healthy reply is a JSON object whose `model` field reads `claude-opus-5-5` and whose `usage` field lists `input_tokens` and `output_tokens`. Multiply those counts by the rates in the first chart and you have the cost of the call. The `usage` field also reports `cache_creation_input_tokens` and `cache_read_input_tokens`. Those are billed at the cache write and cache read rates.

Anthropic documents this JSON format for a `not_found_error`:

`{"type":"error","error":{"type":"not_found_error","message":"The requested resource could not be found."},"request_id":"req_..."}`
`not_found_error` means the requested resource was not found. Check that the model ID is `claude-opus-5-5`. An `authentication_error` with HTTP status 401 is a different problem: the key is missing or wrong. Check that `ANTHROPIC_API_KEY` is set in the same shell that runs `curl`.

## FAQ

### How much does Claude Opus 5.5 cost per million tokens?

As of October 2026, Claude Opus 5.5 lists at **$4** per million input tokens and **$20** per million output tokens. Five-minute cache writes cost **$5** and cache reads **$0.20** per million tokens. Input, output and cache writes are 20% below Opus 5, and cache reads are 60% below. Recheck the Claude pricing page before you budget, because prices change.

### Can I use Opus 5.5 on the free Claude plan?

No. The Claude pricing page lists Opus on the Pro, Max, Ultra, Team and Enterprise plans, and the Free plan gets Sonnet and Haiku. Upgrading to Pro is the cheapest way to get Opus 5.5 in the Claude apps. The API is a separate option, billed per token.

### What does fast mode cost in Claude Code?

Fast mode bills Opus 5.5 at **$8** per million input tokens and **$40** per million output tokens. That is double the standard rate, for what Anthropic calls "up to 2.5x speed". The model is the same, so the answers are not better. On Pro, Max, Ultra, Team and Enterprise plans you pay for it from usage credits, not from your plan's limits. Toggle it with `/fast`, and use it only when you are waiting for the result.

### What is the context window of Claude Opus 5.5?

Anthropic's model page for Opus 5.5 lists a context window of 1 million tokens and up to 128,000 output tokens per response. The launch announcement itself does not give a figure.

### Is Opus 5.5 really 40% cheaper than Opus 5?

The input and output list prices are 20% lower, and the cache read price is 60% lower. The "40% less to run" figure is Anthropic's claim about the cost to finish a task. That cost also depends on how many tokens the model uses and how many of them are cache reads. To find your real saving, run the same task on both models and compare the token counts each one reports.
