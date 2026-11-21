---
id: collect-261121-veille-ia/gpt-openai/gpt-6-sol-vs-astra-vs-luna-which-gpt-6-to-use-intelsi
title: "GPT-6 Sol vs Astra vs Luna: Which GPT-6 to Use | Intelsi"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-10-05"]
keywords: ["gpt-6", "openai", "astra", "model", "input", "luna", "october", "tokens"]
source: https://intelsi.si/guides/gpt-6-astra-vs-sol-vs-luna
source_anchor: ""
source_lines: [1, 108]
section: "GPT_OpenAI"
---
GPT-6 Sol vs Astra comes down to one tradeoff: Astra is OpenAI's most capable model and costs five times as much per token, while GPT-6.1 Sol gets close to Astra on coding, computer use and professional work at a fraction of the price. GPT-6 Luna is the fast, very cheap option for everyday and high volume tasks. For most people and most apps, Sol is the sensible default (now GPT-6.2 Sol, at the same price as GPT-6.1 Sol), with Astra reserved for the hardest problems.

Here is how the names fit together, what each model costs, and where you can actually use it, as of October 5, 2026.

**What's new (November 19, 2026):** OpenAI released **GPT-6.2 Astra** and **GPT-6.2 Sol** on November 17 (launch post). GPT-6.2 Astra (`gpt-6.2-astra`) costs $8 per million input tokens and $40 per million output tokens, 20% less than GPT-6 Astra, with cached input at $0.40. GPT-6.2 Sol (`gpt-6.2-sol`) keeps GPT-6.1 Sol's $2 and $10, with cached input at $0.10. Both have a context window of about 1.05 million tokens and up to 128,000 output tokens, per OpenAI's model pages. In ChatGPT they are in Work and Codex for Plus, Pro, Enterprise and Edu, not in regular Chat. The API also gained two hour and eight hour background completion windows in public beta, with token rates 30% and 40% below Standard.

**Earlier (October 28, 2026):** GPT-6 now has a Terra model. On October 26 OpenAI released **GPT-6.1 Terra** (`gpt-6.1-terra`) in the API, which its model page describes as "a fast, efficient model for visual reasoning, precise computer use, and clear, natural writing." It costs $1 per million input tokens and $5 per million output tokens, with cached input at $0.05, a context window of about 1.05 million tokens and up to 128,000 output tokens. It takes text and images, not audio or video, and tool calling needs the Responses API, per the API changelog.

**What's new (October 23, 2026):** Since October 20 the API offers a 12 hour prompt cache. Per OpenAI's prompt caching guide, on GPT-5.6 and later a normal cache write costs 1.25 times the input price and keeps the prefix for at least 30 minutes, while a 12 hour write costs 2.5 times the input price. Reads cost the same either way: a tenth of the input price on most of these models and a twentieth on GPT-6.1 Sol, which is where its $0.10 cached input price comes from. You pick the 12 hour lifetime in the Responses API with `prompt_cache_options.ttl`.

**What's new (October 22, 2026):** On October 16 OpenAI updated GPT-6 Pro in the ChatGPT Chat tab. Per OpenAI's release notes, it now answers faster, uses plugins and tools better, and can split a task across several agents that work in parallel, with each agent's progress shown in the conversation. The update is rolling out to eligible Pro users; in managed workspaces it depends on workspace settings.

## How OpenAI names its models now

OpenAI changed its naming scheme with GPT-5.6 in July 2026. In that launch post, OpenAI explained that "the number identifies the generation, while Sol, Terra, and Luna are durable capability tiers that can advance on their own cadence." Sol was the flagship tier, Terra the balanced middle, and Luna the fastest and cheapest, per OpenAI's GPT-5.6 announcement.

GPT-6 added a new tier above Sol. **Astra** arrived on September 3, 2026, as OpenAI's new top model. **GPT-6 Sol** and **GPT-6 Luna** followed on September 22, and **GPT-6.1 Sol** replaced GPT-6 Sol as the recommended mid tier model just a week later, on September 29. **GPT-6.1 Terra** filled the middle slot between Sol and Luna on October 26, 2026, in the API.

That cadence is why the names confuse people: the tier tells you roughly how capable and expensive a model is, and the number tells you which generation it belongs to. A GPT-6.1 Sol is a newer, better Sol, not a different product. OpenAI's next generation is already in testing: the internal model behind its Navier-Stokes proof claim is, by Sam Altman's account, an upcoming GPT model, with no name or date yet.

## GPT-6 Sol vs Astra vs Luna at a glance

| Model | Role | API price per million tokens (input / output) | API ID | 
|---|---|---|---|
| GPT-6.2 Astra | Newest top model (Nov 17) | $8 / $40 (cached input $0.40) | gpt-6.2-astra | 
| GPT-6.2 Sol | Newest everyday model (Nov 17) | $2 / $10 (cached input $0.10) | gpt-6.2-sol | 
| GPT-6 Astra | Previous top model | $10 / $50 | gpt-6-astra | 
| GPT-6.1 Sol | Near Astra intelligence, everyday flagship | $2 / $10 (cached input $0.10) | gpt-6.1-sol | 
| GPT-6 Sol | Previous Sol, superseded by 6.1 | $2 / $10 | gpt-6-sol | 
| GPT-6.1 Terra | Balanced middle tier: visual reasoning, computer use, writing | $1 / $5 (cached input $0.05) | gpt-6.1-terra | 
| GPT-6 Luna | Fast, efficient, high volume | $0.10 / $0.50 | gpt-6-luna | 

In words: Astra costs ten dollars per million input tokens and fifty per million output tokens. Both Sol models cost two dollars in and ten out, and GPT-6.1 Sol's cached input costs just ten cents per million. Luna costs ten cents in and fifty cents out, a hundredth of Astra's price. OpenAI's GPT-6 Sol and Luna post notes that the Sol and Luna prices are half of what GPT-5.6 Sol and Luna cost under their promotional pricing.

Astra also has a **Fast mode** in the API that delivers up to twice the speed at twice the standard price. OpenAI says a GPT-6.1 Sol Ultrafast option with up to eight times faster generation in Codex is coming.

## GPT-6 Astra: when only the best will do

Astra is the model OpenAI recommends when "you want the best results and an uncompromising experience." It leads OpenAI's tables on computer use, agentic coding, scientific workflows and math, and OpenAI says it was its largest training run yet. It is also the first model OpenAI has classed as Critical in cybersecurity, so it refuses some advanced security tasks for ordinary users, as covered in our GPT-6 Astra vs Claude Fable 5.1 comparison.

**Pros:** top capability, Astra Pro variant on higher plans, Fast and Ultrafast options.
**Cons:** five times Sol's token price, refusals on advanced cyber work, and OpenAI says its reasoning is harder to monitor than earlier models'.

One more detail: there will not be a GPT-6.1 Astra for now. OpenAI pulled the planned model after it missed the company's safety bar on staying within scope and accurately describing its own work, as the BBC reported. GPT-6 Astra remains available.

## GPT-6.1 Sol: the default for most work

According to OpenAI's GPT-6.1 Sol post, 6.1 Sol matches Astra on the DeepSWE software engineering benchmark at roughly one fifth of the cost, and comes within about two points of Astra on the OSWorld computer use test at roughly one seventh the cost per task. On scientific workflows at maximum effort, OpenAI puts its average cost at $5.47 per task, against $23.80 for Astra, though Astra still scores highest there.

OpenAI also reports fewer factual errors: at low reasoning effort, the share of responses containing a factual error on a deliberately difficult test set fell from 11.4 percent for GPT-6 Sol to 7.7 percent.

**Pros:** near-Astra results for many tasks, cheap cached input, available on Plus and higher plans in ChatGPT Work and Codex.
**Cons:** still behind Astra on the hardest scientific and computer use tasks; not yet in ChatGPT's regular Chat mode.

## GPT-6 Luna: cheap and fast

Luna is built for high volume, latency sensitive work: classification, extraction, routine drafting, and simple coding. OpenAI says GPT-6 Luna at higher effort matches GPT-5.6 Sol on its factuality evaluation at about a hundredth of the cost. It is also the model behind GPT-6 in ChatGPT's Chat tab for Free and Go users, rolling out from October 8, 2026, per OpenAI's GPT-6 and Intelligent UI post. Its closest rival is Anthropic's equally priced Claude Haiku 5.5; see Claude Haiku 5.5 vs GPT-6 Luna.

**Pros:** extremely low price, fast, good enough for most routine tasks.
**Cons:** noticeably weaker on long, complex agentic work.

## Which GPT-6 model you get in ChatGPT

ChatGPT now has two main modes: Chat, for conversation, and Work, an agent for longer tasks. The newest models land in Work and Codex first. On October 7, OpenAI brought GPT-6 to the Chat tab itself: it is powered by GPT-6 Sol on Plus, Pro, Business and Enterprise, and by GPT-6 Luna on Free and Go, with both tuned for everyday conversation. For why one app can run several models at once, see ChatGPT vs LLM.

- **GPT-6 Astra:** rolling out to Plus, Pro, Business and Enterprise. Pro, Business and Enterprise also get Astra Pro. Enterprise admins must switch Astra on for their workspace.
- **GPT-6 Pro:** in the Chat tab for Pro users. Since October 16 it can run several agents in parallel on one task, and you can open each finished task to see what that agent did.
- **GPT-6.2 Astra and GPT-6.2 Sol:** in ChatGPT Work and Codex for Plus, Pro, Enterprise and Edu since November 17; Enterprise and Edu admins must enable them. Pro 500 gets Ultrafast on both, and Pro 200 gets Ultrafast starting with GPT-6.2 Sol.
- **GPT-6.1 Sol:** in ChatGPT Work and Codex for Plus, Pro, Business, Enterprise and Edu. OpenAI says it is "not yet available in Chat."
- **GPT-6 Luna:** in Work and Codex for paid plans, in the desktop app for Free and Go users, and powering GPT-6 in Chat for Free and Go from October 8.

OpenAI's plans are Free, Go at $8 a month, Plus at $20, and three Pro tiers at $100, $200 and $500. The $500 Pro tier, added on September 29, is the only one that includes Astra Ultrafast, according to OpenAI's release notes. For a plan by plan comparison with Claude and Gemini, see our ChatGPT vs Claude vs Gemini vs Grok guide.

## How to choose

1. **Start with Sol** for coding, documents, analysis and agents: GPT-6.2 Sol where your plan or API account has it, GPT-6.1 Sol otherwise. It is the best balance of cost and capability in OpenAI's lineup.
2. **Escalate to Astra** when 6.1 Sol fails your own tests: hard scientific workflows, long computer use tasks, or work where a better first attempt saves expensive human time.
3. **Drop to Luna** for bulk, simple or real time tasks where cost and speed dominate.
4. **Compare across labs** before committing. Claude Opus 5.5 at $4 and $20 per million tokens and Gemini's Flash models are strong alternatives; see Claude Fable vs Opus vs Mythos and our tool pages for Claude and Gemini.

If you build on the OpenAI API, use explicit model IDs rather than relying on the ChatGPT picker, and check OpenAI's models page for deprecation dates now that 6.1 Sol has superseded 6 Sol. Everyday users of ChatGPT mostly just need to know that Work gets new models before Chat.

**From Ahzy Capital (our own products, labeled as such):** Get the free AI Agent Cheat Sheet: 24 copy and paste AI agent setups for 8 types of small business, with prompts and workflows. We make and sell these, so they aren't independent picks.


Model profiles with full specs: GPT-6 Astra, GPT-6.1 Sol, GPT-6.1 Terra and GPT-6 Luna. Launch dates are on our AI model release timeline.

## FAQ

### Is GPT-6 Sol better than GPT-6 Astra?

No. Astra is OpenAI's most capable model. GPT-6.1 Sol comes close on many coding and professional tasks at about one fifth of Astra's token price, which is why it is the better value for most work.

### What is the difference between GPT-6 Sol and GPT-6.1 Sol?

GPT-6.1 Sol is an upgrade released a week after GPT-6 Sol. It costs the same per token, halves the cached input price to $0.10 per million tokens, and scores higher on coding, computer use, science and factuality, according to OpenAI.

### What happened to GPT Terra?

Terra is back. Terra was the balanced middle tier in the GPT-5.6 family, and on October 26, 2026 OpenAI released GPT-6.1 Terra in the API at $1 per million input tokens and $5 per million output tokens. It sits between GPT-6.1 Sol and GPT-6 Luna on price, and OpenAI pitches it at visual reasoning, computer use and writing.

### Can free ChatGPT users use GPT-6?

Yes. From October 8, 2026, GPT-6 in ChatGPT's Chat tab runs on GPT-6 Luna for Free and Go users, and Luna is also in the desktop app. Astra and GPT-6.1 Sol require a paid plan. For what changed in the new desktop app, see how ChatGPT Classic differs from the new ChatGPT app.

### How much does GPT-6 Astra cost?

Through the API, $10 per million input tokens and $50 per million output tokens, with Fast mode at twice those rates. The newer GPT-6.2 Astra costs $8 and $40. In ChatGPT, Astra is included in Plus, Pro, Business and Enterprise allowances, with credits available for extra usage.
