---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-explained-sol-vs-terra-and-which-to-pick
title: "GPT-6.1 Explained: Sol vs Terra and Which to Pick"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "terra", "openai", "models", "tokens", "dollars", "both", "work"]
source: https://www.lilachbullock.com/gpt-6-1/
source_anchor: ""
source_lines: [1, 91]
section: "GPT_OpenAI"
---
**GPT-6.1 is OpenAI's name for two API models: GPT-6.1 Sol, for complex coding, computer use and professional work, and GPT-6.1 Terra, a cheaper, fast model for visual reasoning, precise computer use and clear, natural writing.**

Here's the short version. If you're paying for OpenAI's models through the API, GPT-6.1 Sol is the one to try for hard work, and GPT-6.1 Terra is the one to try for visual work and everyday writing. They have the same context window size and tools, and Terra costs half what Sol does.

Everything below comes from OpenAI's own API documentation, so this is a guide for anyone using these models through the API or paying a developer who does.

## What GPT-6.1 is, and why the names get confusing

OpenAI's model catalogue lists four flagship models, and only two of them carry the 6.1 number. GPT-6.1 Sol and GPT-6.1 Terra are the 6.1 models. GPT-6 Astra and GPT-6 Luna still carry the plain 6, and OpenAI's GPT-6 guide treats all four as one family. If you've read my earlier guide to working with GPT-6, that's the family it belongs to.

OpenAI's news feed dates "Introducing GPT-6.1 Sol" to 29 Sep 2026 and "Introducing GPT-6.1 Terra" to 26 Oct 2026. OpenAI's changelog lists both releases on those days. For where these sit against everything else OpenAI offers, there's my wider rundown of ChatGPT models.

One trap to watch: GPT-6 Sol and GPT-6.1 Sol are two different models, with two different IDs (gpt-6-sol and gpt-6.1-sol).

## GPT-6.1 Sol vs Terra, with Astra and Luna alongside

Figures are from OpenAI's catalogue and model pages. Prices are per million API tokens at standard processing.

|  | GPT-6.1 Sol | GPT-6.1 Terra | GPT-6 Astra | GPT-6 Luna | 
|---|---|---|---|---|
| OpenAI's description | Near-Astra performance for complex work at a lower cost | Fast, efficient; visual reasoning, precise computer use, clear writing | Most capable, for the most demanding work | Most efficient, for focused, high-volume tasks | 
| Input price | 2 dollars | 1 dollar | 10 dollars | 10 cents | 
| Output price | 10 dollars | 5 dollars | 50 dollars | 50 cents | 
| Context window | 1.05M tokens | 1.05M tokens | 1.05M tokens | 1.05M tokens | 
| Knowledge cutoff | Apr 30, 2026 | Jun 1, 2026 | Apr 30, 2026 | May 18, 2026 | 
| Supports "none" reasoning effort | No | Yes | No | Yes | 

Sol and Terra match on more than you'd expect. Both take text and images in and give text out, both pages mark audio and video as not supported, and both list the same tools on the Responses API, including web search, file search and computer use.

So the choice comes down to what each one is for and what it costs. To see these rates against the rest of the range, here's my breakdown of OpenAI's API costs.

## The price details that change your bill

OpenAI's model pages add these, and most apply to both Sol and Terra:

- **Cached input:** 10 cents per million tokens on Sol and 5 cents on Terra, which is 5% of the normal input rate.
- **Long prompts:** anything over 272K input tokens costs 2x on input and cache rates and 1.5x on output, for the full request. On Sol, that turns 2 dollars into 4 and 10 dollars into 15.
- **Batch and Flex:** prices are 50% lower than standard.
- **Fast mode:** 2x standard on both models.
- **Ultrafast mode:** 6x standard, and Sol's page lists it. Terra's doesn't.
- **Regional processing:** adds a 10% premium where it's available.

I'd check the long-prompt rule first. It's charged on the whole request, so if you feed in huge documents, trim them before you send them.

### Want AI doing the heavy lifting in your marketing?

I build the systems that handle the boring 80 percent, so you get your week back. Done properly, with the human kept in.

## Which one should you pick?

Here's my verdict, based on what OpenAI says each model is for and what it costs:

- **Pick GPT-6.1 Terra** if the job is visual or written. OpenAI's own examples are matching product photos to catalogue entries, interpreting technical drawings, acting on application screens and drafting or revising text.
- **Pick GPT-6.1 Sol** if the job is complex coding, computer use or professional work where a wrong answer costs you more than the tokens do.
- **Pick GPT-6 Luna** if you've got a focused task to run at high volume and cost is the main concern. At 10 cents per million input tokens, it's a tenth of Terra's rate.
- **Pick GPT-6 Astra** only when you've tried Sol on your real work and it fell short.

That last one is where I part company with OpenAI. Its catalogue says that if you're not sure where to start, use Astra. I'd start with Sol. OpenAI's own news feed describes Sol as near-Astra intelligence at one-fifth of Astra's standard API token prices, and the table bears out the price part. Why pay five times as much before you've seen whether you need to?

Two caveats. "Near-Astra" is OpenAI's own phrase, and it's OpenAI's claim to prove on your work, not a settled result. The AI bake-off kit for picking an assistant gives you a ready way to run that test on your own tasks. And price per token isn't price per job: OpenAI's guide says Astra got stronger results using substantially fewer output tokens in several evaluations, and that its estimated cost per task came in lower than earlier models. That comparison is with earlier models, not with Sol. So run the same real task through two models and compare the final cost and the quality.

## GPT-6 Sol and GPT-6.1 Sol aren't the same thing

If you or your developer already use gpt-6-sol, OpenAI's GPT-6 guide says to review its migration guidance before switching to GPT-6.1 Sol. It isn't a simple rename:

- **Reasoning:** GPT-6 Sol supports the "none" setting, and GPT-6.1 Sol doesn't. Where a model lacks it, OpenAI's advice is to use low instead.
- **Tools:** GPT-6 Sol supports function calling in Chat Completions only with reasoning set to none. On GPT-6.1 Sol, tool calling requires the Responses API.
- **Data residency:** Fast mode supports EU data residency for GPT-6.1 Sol, and not for GPT-6 Sol.
- **Price:** the changelog lists both at 2 dollars input and 10 dollars output, with cached input at 20 cents on GPT-6 Sol and 10 cents on GPT-6.1 Sol.

## Where GPT-6.1 runs

Both models are in the Responses API, and OpenAI's changelog lists both for Chat Completions too. OpenAI's guide is clear that tool calling needs Responses, so anything using tools belongs there, and my plain guide to the Responses API is the place to start if that's new to you. The docs also state:

- **Multi-agent:** the changelog says GPT-6.1 Sol supports Multi-agent in beta, letting the model delegate work to subagents in a Responses API request. Terra's changelog entry doesn't mention it.
- **Ultrafast:** OpenAI's Ultrafast guide names GPT-6 Astra and GPT-6.1 Sol, and says it's available to all API users. It doesn't name Terra.
- **Data residency:** Fast mode supports EU data residency for Sol, Terra and Luna, and not for Astra. Ultrafast on Sol supports US and EU data residency and global processing.
- **Usage tiers:** both model pages mark the API's Free tier as not supported.
- **Codex and ChatGPT Work:** the Sol model page says GPT-6.1 Sol uses a default context window of 500K tokens in Codex and ChatGPT Work, and that usage drawn from your subscription has no long-context surcharge.

One thing to keep separate: these are API prices and API tiers. What a ChatGPT subscription costs, and which models your plan shows you, is a different question, so check that inside your own ChatGPT account.

## Quick answers

### Is GPT-6.1 Sol the same as GPT-6 Sol?

No. They're separate models with separate IDs, and OpenAI's guide says to review its migration guidance before you move from gpt-6-sol to GPT-6.1 Sol.

### Is GPT-6.1 Sol cheaper than GPT-6 Astra?

Yes, per token. Sol costs 2 dollars per million input tokens and 10 dollars per million output tokens, against 10 and 50 dollars for Astra. That's one-fifth of the price, though the cost of a whole job also depends on how many tokens each model uses.
