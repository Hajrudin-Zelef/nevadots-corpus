---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-benchmarks-price-cyber
title: "Gemini 3.8 Flash: Benchmarks, Price, Cyber"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "price", "model", "output", "tokens", "cyber", "reasoning"]
source: https://dev.to/projedefteri/gemini-38-flash-benchmarks-price-cyber-proje-defteri-41i9
source_anchor: ""
source_lines: [1, 207]
section: "Gemini_Google"
---
**TL;DR: Gemini 3.8 Flash in 30 seconds**


**Gemini 3.8 Flash** landed on **September 2, 2026**, three weeks after 3.7 Flash. That is the third Flash release in three months.- Price did not move:
**$0.75 input, $3.75 output** per million tokens. The promo ends **December 31, 2026**, then it doubles.- A second model shipped alongside it:
**Gemini 3.8 Flash Cyber**, tuned for vulnerability discovery and locked behind the new **Fairwind Program**.- The benchmark table is split. It tops the chart on finance, legal, long video and chart reasoning, and it trails
**Claude Opus 5 badly** on long horizon terminal and computer use work.- The model "works harder": more reasoning steps, more iterative tool calls. Same sticker price, potentially a bigger bill.

The Flash release cadence has stopped being funny. 3.6 Flash shipped on July 21, 3.7 Flash on August 13, and **Gemini 3.8 Flash** today, September 2, 2026. Three releases in three months.

There is no price cut this time, so the news is elsewhere. Google is pitching the model as "our best reasoning and coding model yet, at the same speed and low cost of 3.7." The announcement is signed by Tulsee Doshi, senior director of product management, and Raluca Ada Popa, Gemini security lead at Google DeepMind.

That second signature explains the second model: **Gemini 3.8 Flash Cyber**, a defense-only variant.

Let's look at the numbers. 👇🏻

Gemini 3.8 Flash is the newest member of the Flash family. The logic has not changed: Pro models handle the hardest single tasks, Flash models handle **volume**. If you push millions of tokens a day through an agent or a classification pipeline, this is the model that writes your invoice.

The spec sheet:

| Spec | Value | 
|---|---|
| Model ID | `gemini-3.8-flash` | 
| Context window | 1,000,000 tokens | 
| Output limit | 64,000 tokens | 
| Input | Text, image, video, audio, PDF | 
| Output | Text only | 
| Knowledge cutoff | March 2026 (January 2025 in some domains) | 
| Tool use | Function calling, search as a tool, computer use | 
| Open weights | No | 

That knowledge cutoff line matters more than it looks. Most domains are current to **March 2026**, but some stop at **January 2025**. Ask it about a library released last month without grounding enabled and you are inviting a confident wrong answer.

Roughly 750,000 words: an entire mid sized codebase, or a few hundred pages of technical documentation. You can measure your own text in seconds with our token counter.

Google's core behavioral claim fits in one sentence: **3.8 Flash works harder.** On complex tasks it takes extra reasoning steps, calls tools iteratively instead of once, and does not settle for its first answer.

On paper that is good news. In practice it cuts both ways:

- 
**The upside:** accuracy rises on multi step agent work and in domains where a half answer is worthless, like legal review or financial analysis.
- 
**The catch:** thinking tokens are billed as output, and output costs**five times** input. Artificial Analysis flags the model as "very verbose" and reports it produced**120 million** output tokens across their evaluation suite. Same list price, fatter invoice.

Google recommends lower effort levels for efficiency first workloads, and says 3.7 Flash stays **fully supported** for exactly those jobs. Your cheap classification pipeline does not need to migrate tomorrow.

Here is the comparison table. The rivals are Claude Opus 5 and GPT-5.6 Sol.

| Benchmark | 3.8 Flash | 3.7 Flash | Opus 5 | GPT-5.6 Sol | 
|---|---|---|---|---|
| Input price ($/1M) | 0.75* | 0.75* | 5.00 | 5.00 | 
| Output price ($/1M) | 3.75* | 3.75* | 25.00 | 30.00 | 
| AA Intelligence Index | 59 | 56 | - | - | 
| Vals Finance Agent v2 | **61.4%** | 59.0% | 58.6% | 53.8% | 
| Harvey Legal Agent | **10.0%** | - | 6.7% | 2.5% | 
| HLE-Verified | **54.9%** | 53.6% | 54.4% | - | 
| Terminal-bench 2.1 | **89.4%** | - | 89.1% | - | 
| CharXiv Reasoning | **86.2%** | - | 83.7% | - | 
| LVBench (agentic) | **87.8%** | - | 75.4% | - | 
| LABBench2 | **86.2%** | - | 84.2% | - | 
| BioMysteryBench (hard) | **56.5%** | 43.5% | 49.4% | 44.7% | 
| BioMysteryBench (solvable) | 88.8% | - | **90.1%** | - | 
| DeepSWE v1.1 | 71.0% | - | **74.0%** | 72.7% | 
| Terminal-bench 4.0 | 19.1% | 11.2% | **51.8%** | - | 
| OSWorld-2.0 | 59.0% | 50.6% | **75.4%** | - | 
| GDPVal-AA v2 (Elo) | 1545 | - | **1824** | - | 
| GDP.pdf | 35.0% | 34.0% | - | **40.0%** | 

** Introductory pricing, ends December 31, 2026. Sources: Google, Artificial Analysis, OfficeChai compilation*

The table tells two separate stories.

**Story one:** on specialist agent work, 3.8 Flash is the best model in the table. It scores 61.4% on the Vals Finance Agent v2 suite, 10.0% on Harvey's legal agent benchmark, and 54.9% on HLE-Verified multidisciplinary reasoning, beating Opus 5 on all three. On long video understanding in agentic mode the gap is over twelve points: 87.8% against 75.4%. It also leads on CharXiv chart and table reasoning.

Do not read too much into that 10% legal score. The benchmark is brutally hard and nobody in the table reaches double digits twice over. What matters is that Opus 5 sits at 6.7% and GPT-5.6 Sol at 2.5%.

**Story two:** the moment the job becomes long horizon autonomous engineering, the model falls behind. Terminal-bench 4.0 gives it 19.1% against Opus 5's 51.8%. On the OSWorld-2.0 computer use suite it is 59.0% against 75.4%. On DeepSWE v1.1 its 71.0% trails both Opus 5 (74.0%) and GPT-5.6 Sol (72.7%). On the GDPVal-AA v2 knowledge work Elo there are 279 points between them.

Against its own predecessor the gains are real: Terminal-bench 4.0 from 11.2% to 19.1%, OSWorld-2.0 from 50.6% to 59.0%, the hard biology set from 43.5% to 56.5%. On the Artificial Analysis Intelligence Index it moves from 56 to **59**, ranking 16th out of 195 models.

**Price to performance is still the story**

Opus 5 wins terminal and computer use outright. It is also **6.7x** more expensive on input and **6.7x** on output. If you run the same task a million times a day, that spread outweighs the benchmark spread. Routing the critical agent step to the expensive model and the bulk volume to Flash remains the sane architecture.


One speed note. Artificial Analysis clocks the model at **304.6 tokens per second**, the fastest in their index, but time to first token is **13.39 seconds**. It thinks for a while, then writes very fast. In a chat UI you feel that wait. In a batch pipeline it does not matter.

API pricing per million tokens:

| Period | Input | Output | 
|---|---|---|
| Introductory (through December 31, 2026) | $0.75 | $3.75 | 
| From January 1, 2027 | $1.50 | $7.50 | 

If that looks familiar, your memory is fine: it is the same tariff and the same expiry date as 3.7 Flash. On New Year's Day the rate **doubles**.

The subtler cost is not on the price sheet. Because the model takes more reasoning steps, the same prompt at the same rate can produce noticeably more output tokens. Budget for both the January increase and the token inflation.

What does that mean for your workload? Drop your input and output token counts into our LLM cost calculator and compare Gemini 3.8 Flash against Claude and GPT side by side. The model is already in the list.

The second model is defense only. A successor to 3.5 Flash Cyber, **3.8 Flash Cyber** is tuned for autonomous vulnerability discovery, and Google says it beats both 3.5 Flash Cyber and significantly larger frontier models on CyberGym.

The published numbers:

- Over **70% success rate** on real world vulnerability discovery across 20 programming languages.
- 
**47.2% pass@1** on CWE-Bench patching.
- A significant improvement on Gray Swan prompt injection robustness.

Field reports back it up. The Chrome security team says the model produced **2.6 times more correct patches** than leading commercial models. Cloud security vendor Wiz reports 7.5% to 9.7% higher recall at **2.3x to 5.2x lower cost**. Google Cloud's vulnerability research team says it found a critical flaw in **under two hours**.

You cannot call it with your API key. Google is releasing it only to "trusted defenders" through the new **Fairwind Program**: government authorities, critical infrastructure operators and software maintainers.

The reasoning is obvious. The same capability that writes a patch writes an exploit. So the Cyber variant stays gated while the standard model ships with Frontier Safety Framework safeguards against CBRN and cyber offense misuse.

As of September 2, 2026, Gemini 3.8 Flash is available in:

- 
**For developers:** Google AI Studio and the Gemini API, Android Studio, Stitch, and Google Antigravity for agent first workflows.
- 
**For enterprises:** Gemini Enterprise and the Gemini Enterprise Agent Platform.
- 
**For consumers:** the Gemini app for Google AI Pro and Ultra subscribers, AI Mode in Google Search, and Gemini in Google Sheets.
- 
**Cyber variant:** Fairwind Program participants only.

There are no open weights, so self hosting is not an option.

```
from google import genai
from google.genai import types
client = genai.Client(api_key="YOUR_KEY")
response = client.models.generate_content(
    model="gemini-3.8-flash",
    contents="Describe Gemini 3.8 Flash in one sentence.",
    config=types.GenerateContentConfig(
        thinking_config=types.ThinkingConfig(thinking_level="low"),  # low | medium | high
    ),
)
print(response.text)
```
Three things worth knowing:

- 
**Pick the effort level deliberately.** The default leans toward more thinking. For classification, tagging or short summaries, a low level is both cheaper and enough.
- 
**Use caching.** If you resend the same system prompt or the same document repeatedly, caching cuts the bill hard. Artificial Analysis puts the cache discounted blended rate at $0.58 per million tokens.
- 
**Respect the cutoff.** Anything after March 2026 needs search grounding, otherwise you are inviting hallucinations.

The table turns into fairly concrete advice:

- 
**Domain agents in finance, law and biology:** 3.8 Flash, clearly. Vals, Harvey and LABBench2 all land above Opus 5, at roughly a sixth of the price.
- 
**Video, charts, PDFs and long documents:** 3.8 Flash again. The LVBench and CharXiv gaps are not rounding errors.
- 
**Autonomous engineering agents that run for hours, terminal and desktop automation:** Opus 5 wins outright. A 32 point gap on Terminal-bench 4.0 and 16 on OSWorld is not something a budget line closes.
- 
**High volume production workloads:** this is Flash territory. But measure the real token cost on your own data before switching, because the extra reasoning steps show up on the invoice. For simple jobs, 3.7 Flash is still supported and still less chatty.

Three Flash releases in three months says Google now treats Flash as the main line, not the budget tier. The real novelty in this release is not the price, it is the second model: security has become its own product line, and rivals are splitting the same way.

A Pro release, or an answer from the competition, will not take long. Until the Terminal-bench 4.0 and OSWorld gaps close, owning the cheap tier does not make Google the owner of the smartest model.

**Q: When was Gemini 3.8 Flash released?**

A: Google announced it on **September 2, 2026** and shipped it the same day through the Gemini API, Google AI Studio, Android Studio, Antigravity, Gemini Enterprise and the Gemini app.

**Q: How much does Gemini 3.8 Flash cost?**

A: Introductory pricing is $0.75 per million input tokens and $3.75 per million output tokens. It ends December 31, 2026; from January 1, 2027 the rate becomes $1.50 and $7.50.

**Q: Is Gemini 3.8 Flash free?**

A: API access is paid. Google AI Studio offers limited free testing. In the Gemini app the model is available to Google AI Pro and Ultra subscribers.

**Q: Is Gemini 3.8 Flash better than Claude Opus 5?**

A: It depends on the task. It beats Opus 5 on finance (61.4%), legal (10.0%), HLE-Verified (54.9%), long video (87.8%) and chart reasoning (86.2%). It loses on Terminal-bench 4.0 (19.1% vs 51.8%), OSWorld-2.0 (59.0% vs 75.4%) and DeepSWE v1.1 (71.0% vs 74.0%). It is 6.7x cheaper.

**Q: What is the difference between Gemini 3.8 Flash and 3.7 Flash?**

A: 3.8 Flash takes more reasoning steps and calls tools iteratively. Terminal-bench 4.0 rose from 11.2% to 19.1%, OSWorld-2.0 from 50.6% to 59.0%, Vals Finance Agent v2 from 59.0% to 61.4%. Pricing is unchanged, and 3.7 Flash remains supported for efficiency first workloads.

**Q: What is Gemini 3.8 Flash Cyber and how do you get access?**

A: It is a variant tuned for autonomous vulnerability discovery, with over 70% success across 20 languages in real world testing and 47.2% pass@1 on CWE-Bench. It is not publicly available; access runs through the Fairwind Program for government authorities, critical infrastructure operators and software maintainers.

**Q: How big is the Gemini 3.8 Flash context window?**

A: One million input tokens and a 64,000 token output limit. It accepts text, image, video, audio and PDF input, and returns text only.

Take care... 🙂

**AI Generated Content Notice**

This blog is entirely generated by artificial intelligence. While AI helps create content, it may still contain errors or biases. Verify critical details before relying on them.


*Originally published on Proje Defteri, where this post is kept up to date.*

*Also on the site: more English posts on AI models, Arduino and IoT, and free browser tools for makers and developers - token counter, LLM cost calculator, LCD and OLED bitmap converters.*

Your support means a lot! ✨ Comment 💬, like 👍, and follow 🚀 for future posts!

## Top comments (0)
