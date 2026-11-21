---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-space-bunny-revealed-and-tested
title: "MiniMax M3.1 Flash: Space Bunny Revealed and Tested"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax", "flash", "space", "bunny", "tokens", "free", "model", "openrouter"]
source: https://www.michaelleung.info/latest-news/minimax-m3-1-flash-space-bunny-tested
source_anchor: ""
source_lines: [1, 98]
section: "MiniMax"
---
MiniMax M3.1 Flash is the official name of Space Bunny, the anonymous "stealth" model that appeared on OpenRouter in late September. MiniMax confirmed it on 19 October 2026. On MiniMax's own charts it scores close to the top models in agent and coding tests for a small fraction of the cost, at US$0.15 per million input tokens and $0.60 per million output tokens. I used Space Bunny in OpenCode for most of an Outlook clone build, and it was fast and good. In a small speed test today, though, the free OpenRouter endpoint was slower than MiniMax claims.

## What Is MiniMax M3.1 Flash?

It is MiniMax's new "Flash" model, built for agent work, coding, computer use and cybersecurity. In its announcement MiniMax wrote: "Space Bunny, which processed more than 200 trillion tokens while operating anonymously, was an early version of MiniMax M3.1 Flash."

When I wrote What Is Space Bunny Alpha? on 29 September, the timing around the Mid-Autumn Festival and the model's behaviour pointed to a Chinese lab, most likely MiniMax. That guess turned out right.

What MiniMax and OpenRouter list:

- Available through the MiniMax API, MiniMax's M Plan subscription and its MiniMax Code desktop app.
- On OpenRouter it is listed free as `minimax/minimax-m3.1-flash:free` , with a 1,000,000-token context window, up to 524,288 output tokens, and text, image and video input.
- MiniMax says it made the model faster "through more aggressive sparsity". It has not published the parameter count.
- The weights "will be released later", and a bigger M3.1 Pro model is also coming.

## How Fast Is MiniMax M3.1 Flash?

MiniMax says output speed reaches 200+ tokens per second in everyday use, with a median of 170+ tokens per second, and that online inference is "nearly three times faster". My own test on the free OpenRouter endpoint came in lower.

On 20 October I sent the same three coding prompts to three models through OpenRouter and timed the streamed output: a Python date parser with tests, A* pathfinding on a hex grid in C#, and a one-file HTML times tables quiz.

| Model (provider on OpenRouter) | Time to first token | Output speed | 
|---|---|---|
| MiniMax M3.1 Flash, free (MiniMax) | 0.95–2.66 s | 76–179 tok/s | 
| MiniMax M3, paid (CoreWeave) | 0.39–0.79 s | 188–254 tok/s | 
| DeepSeek V4.1 Flash-1017, paid (InferenceNet) | 0.40–0.50 s | 248–306 tok/s | 

Only one of the three M3.1 Flash runs passed MiniMax's 170 tokens per second. I don't read this as the model being slow. A free endpoint is shared by everyone using it, and three prompts on one evening is a small sample. The two paid models ran on other companies' servers. It does mean the "200+ TPS" figure is not what you should expect from the free tier today.

## How Did Space Bunny Do on a Real Project?

It did most of the coding on my Outlook clone, and that is why I like it. The project is a desktop app in .NET 10 WPF with mail, calendar, contacts, tasks and notes, and sync to Google, iCloud, Nextcloud and other CalDAV and CardDAV servers.

Claude Code wrote the plan. Then I ran the build in OpenCode with Space Bunny from 29 September to 1 October. OpenCode's history shows 1,555 replies from Space Bunny on that project and about 598,000 output tokens. Across all my projects in those five days it wrote about 1.43 million output tokens, including work on a Godot game and a 3D Earth page in Three.js. It cost me nothing, because the stealth model was free.

The project now has about 31,000 lines of C# and 412 automated tests. Not every line is Space Bunny's, but most of the build happened in those two sessions, and it felt very fast and gave good results. That was the free stealth version, before the official launch.

## How Does M3.1 Flash Compare With DeepSeek, Claude and GPT?

On MiniMax's three charts, M3.1 Flash is at or near the top on score and close to the cheapest on cost. These are MiniMax's own charts, so treat them as a vendor's view until independent tests come out.

DeepSWE v1.1 is a real-world coding test. M3.1 Flash scores 73.7%, about the same as Claude Opus 5 (73.6%), for roughly 1/40 of the cost per task. DeepSeek V4.1 Flash-1017 scores higher (77.0%) and costs even less per task on this chart.

| Benchmark (best setting) | M3.1 Flash | Main rivals on MiniMax's chart | 
|---|---|---|
| Automation-Bench v1.0.6 | 62.8%, ~$0.045 a task | DeepSeek V4.1 Flash-1017 ~60.5% (~$0.09), Opus 5.5 ~56.8% (~$0.90), GPT-5.6 ~47.8% (~$0.72), Kimi K3 ~45.8%, GLM-5.3 ~45.0% | 
| DeepSWE v1.1 | 73.7%, ~$0.28 a task | DeepSeek V4.1 Flash-1017 77.0% (~$0.19), GPT-6.1 Sol ~75.2% (~$0.65), Gemini 3.8 Flash ~73.8% (~$2.40), Claude Opus 5 ~73.6% (~$12) | 
| Agents' Last Exam (152 tasks) | 29.6%, $111 total | Claude Opus 5.5 ~38% (~$1,400), GPT-6 Astra ~34% (~$1,100), Claude Opus 5 ~32% (~$1,100), Kimi K3 ~28% (~$600) | 

M3.1 Flash scores are MiniMax's stated numbers. Values marked ~ are my reading of MiniMax's log-scale charts, so they are approximate. Automation-Bench and Agents' Last Exam ran M3.1 Flash in the Claude Code harness; the DeepSWE data is from the leaderboard as of 22 September with mini-swe-agent.

MiniMax also lists 61.7 on OSWorld V2, a computer-use test, and 29.6 on Agents' Last Exam, and says both are first among Chinese models. It scores 82% on CyberGym for finding security bugs and 76.2 on BankerToolBench.

So on these charts M3.1 Flash doesn't beat Claude Opus 5.5 on the hardest work. On Agents' Last Exam Opus 5.5 is still about 8 points ahead. What M3.1 Flash does is reach a similar level on coding and automation for a few cents a task. For more on the Claude side, see my Claude models comparison.

## How Much Does MiniMax M3.1 Flash Cost?

MiniMax charges a flat US$0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million cached input tokens. That is half of MiniMax's own price for the older M3 ($0.30 in, $1.20 out). MiniMax says the total cost of agent tasks is 70% lower than M3's.

Here is how the list prices compare, from each company's own pricing page:

| Model, US$ per 1M tokens | Input | Cached input | Output | 
|---|---|---|---|
| **MiniMax M3.1 Flash** | $0.15 | $0.015 | $0.60 | 
| MiniMax M3 (up to 512K input) | $0.30 | $0.06 | $1.20 | 
| DeepSeek V4.1 Flash-1017, off-peak | $0.15 | $0.003 | $0.60 | 
| DeepSeek V4.1 Flash-1017, peak | $0.30 | $0.006 | $1.20 | 
| GLM-5.3-Flash | $0.15 | $0.03 | $0.50 | 
| GLM-5.3 | $1.40 | $0.26 | $4.40 | 
| Kimi K3 | $3.00 | $0.30 | $15.00 | 

DeepSeek's peak hours are 01:00–04:00 and 06:00–10:00 UTC on weekdays, which is 11am–2pm and 4pm–8pm in Brisbane. Kimi K3 also charges $3.00 per million tokens to write to its cache (5-minute TTL). Z.ai's cached-input storage for GLM is free for a limited time. MiniMax M3 costs double above 512K input tokens.

On list price, M3.1 Flash sits in the cheapest group with DeepSeek Flash off-peak and GLM-5.3-Flash, which is a little cheaper on output. Kimi K3 costs 20 times more per input token and 25 times more per output token.

The price per token isn't the whole bill, because models use different numbers of tokens for the same job. On MiniMax's Automation-Bench chart a task cost about $0.045 with M3.1 Flash, about $0.09 with DeepSeek V4.1 Flash-1017 at peak rates, about $0.18 with GLM-5.3 and about $0.32 with Kimi K3. I covered DeepSeek's update in DeepSeek V4.1 Flash-1017: Same Price, Big Coding Gains. For now you can also try M3.1 Flash free on OpenRouter.

## Can You Run MiniMax M3.1 Flash Locally?

Not yet. MiniMax says the weights "will be released later" and hasn't said how big the model is. Until we know the size, I can't say whether any part of it would fit on my 8GB RTX 4060 Ti. For when a cloud model is the better deal than a local one, see my Local LLM vs Cloud API guide.

## Should You Use MiniMax M3.1 Flash?

If you run coding agents and watch the bill, yes, it is worth trying. It is one of the cheapest models near the top of these charts, it handled a big multi-day build for me, and right now it is free on OpenRouter. If you need the very best on hard tasks, Claude Opus 5.5 still leads. If raw speed matters most, test it at your own busy hours first, because the free endpoint was slower than MiniMax's numbers in my test.

Have you tried Space Bunny or M3.1 Flash in your own agent? I'd like to hear how it went. Send me a note on the contact page.

## Sources

- MiniMax: MiniMax M3.1 Flash: Redefining Agent Performance and Cost (19 Oct 2026), including the Automation-Bench, DeepSWE and Agents' Last Exam cost charts, and pricing.
- OpenRouter: MiniMax M3.1 Flash (free): context length, maximum output and input types.
- MiniMax API pricing: M3.1 Flash and M3 prices.
- Kimi K3 API pricing (Moonshot AI).
- Z.ai pricing: GLM-5.3 and GLM-5.3-Flash.
- DeepSeek API pricing: peak and off-peak prices for V4.1 Flash-1017.
- My own speed test through the OpenRouter API on 20 Oct 2026 (3 prompts per model, up to 2,000 output tokens each, low reasoning effort), and my OpenCode session history from 29 Sep to 3 Oct 2026.

*Checked 20 October 2026. Prices are in US dollars. The cover image was made locally with Qwen Image 2.1, and I made both charts from MiniMax's published numbers and my own test. AI helped me research and edit this article.*
