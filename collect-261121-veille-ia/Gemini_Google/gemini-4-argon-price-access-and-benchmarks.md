---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-price-access-and-benchmarks
title: "Gemini 4 Argon: Price, Access and Benchmarks"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "gemini", "argon", "price", "input", "claude", "opus", "gpt-6"]
source: https://dev.to/projedefteri/gemini-4-argon-price-access-and-benchmarks-1g43
source_anchor: ""
source_lines: [1, 111]
section: "Gemini_Google"
---
**Gemini 4 Argon in 30 Seconds**


- Google DeepMind announced
**Gemini 4 Argon** on **30 September 2026**. Today it is only rolling out to vetted cyber defenders in the **Fairwind Program**.- Introductory price:
**$2 input / $10 output** per million tokens, cached input 95% off. That is exactly half of Claude Opus 5.5.- Output limit jumps from
**64K to 1 million tokens**.- First on Vals Index, DeepSWE v1.1 and the legal and finance agent benchmarks. Opus 5.5 and GPT-6 Astra still lead on Terminal-Bench 4.0 and FrontierSWE.
- No public date. Next in line:
**paid API customers** and **Google AI Ultra** subscribers.

Google's fourth-generation model is here, just not for you yet. **Gemini 4 Argon** goes first to teams that defend systems against cyberattacks; developers, businesses and consumers get it once Google finishes tightening its safeguards. The price is already public though, and it is aggressive: half of Claude Opus 5.5, a fifth of GPT-6 Astra.

The announcement is signed by Koray Kavukcuoglu, Chief AI Architect at Google. "Argon" had been floating around in leaks for weeks; in our Gemini 4 release date roundup we flagged it as unconfirmed. It is now the official name.

*Source: Google*

Probably not. Here is the rollout order Google laid out:

1. 
**Now:** trusted cyber defenders through the**Fairwind Program** , plus Google's own teams. Some of them get Argon with the cyber guardrails switched off.
2. 
**Early testers** give feedback while Google iterates on guardrails. Google is also taking part in the U.S. government's voluntary pre-release access process.
3. 
**Next:** paid**Gemini API** customers and**Google AI Ultra** subscribers.
4. 
**Later:** broader enterprise and consumer access.

There is no date for steps 3 and 4, only "as soon as possible". If you want to be ready on day one, the practical move is to have a paid Gemini API project set up. Until then the newest Google model you can put in production is Gemini 3.8 Flash.

Google calls this an **introductory** price, so expect it to move later. Cached input is 95% off the input rate, which works out to **$0.10** per million tokens.

| Per million tokens | Gemini 4 Argon | Claude Opus 5.5 | GPT-6 Astra | Gemini 3.8 Flash | 
|---|---|---|---|---|
| Input | $2 | $4 | $10 | $0.75 | 
| Output | $10 | $20 | $50 | $3.75 | 
| Cached input | $0.10 | $0.20 | - | - | 

*Source: Google (Argon introductory price), Anthropic and OpenAI price lists.*

Worked example: a job with 1M input and 200K output tokens costs $4 on Argon, $8 on Opus 5.5 and $20 on GPT-6 Astra. Plug in your own numbers: Argon is already in our LLM cost calculator and token counter.

**Is Gemini 4 free?** No. There is no free tier announced, and the first public access goes to paying API customers and AI Ultra subscribers. Nothing has been said about the free Gemini app.

Google compares Argon with GPT-6 Astra, Claude Fable 5.1 and Claude Opus 5.5. Our pick of the rows:

| Benchmark | Gemini 4 Argon | GPT-6 Astra | Claude Fable 5.1 | Claude Opus 5.5 | 
|---|---|---|---|---|
| Vals Index (knowledge work) | 68.9% | 63.1% | 65.8% | 67.0% | 
| AutomationBench (business workflows) | 51.3% | 41.4% | 31.4% | 42.5% | 
| Vals Finance Agent v2 | 65.4% | 53.5% | 58.9% | 58.6% | 
| Harvey Legal Agent | 19.6% | 5.4% | 6.7% | 3.8% | 
| DeepSWE v1.1 (agentic coding) | 77.9% | 74.1% | 67.4% | 74.2% | 
| FrontierSWE v2 (agentic coding) | 55.0% | 65.5% | 56.3% | 62.3% | 
| Terminal-Bench 4.0 | 57.4% | 58.2% | 57.9% | 66.4% | 
| Terminal-Bench Science 0.1 | 57.6% | 68.1% | 52.6% | 63.3% | 
| LABBench 2 (science) | 88.8% | 85.4% | 68.6% | 73.1% | 
| GraphWalks 256K-1M (long context) | 84.2% | 71.8% | 65.0% | 66.8% | 
| OSWorld-2.0 (computer use) | 69.2% | 72.6% | - | - | 
| LVBench (long video) | 91.7% | 87.5% | 79.7% | 83.7% | 
| CWE-bench v1 (cybersecurity) | 68.0% | 68.0% | 58.0% | 67.0% | 

*Source: Google DeepMind model page. Methodology at deepmind.google/models/evals-methodology/gemini-4-argon.*

*Source: Google*

The short read: Argon clearly leads on knowledge work (legal, finance, workflow automation) and long context. The legal gap is striking, almost three times the next model. It is also top on long video understanding. Terminal-heavy coding is a different story: Opus 5.5 wins Terminal-Bench 4.0, and GPT-6 Astra wins FrontierSWE and the science terminal tasks. So Argon is not "best at everything"; its edge is long, multi-document, multi-step work.

The old cap was 64K output tokens. Argon's is **1 million**, which Google calls industry-leading. In practice the model can think and write hundreds of thousands of tokens in a single run: rewriting a large module, drafting a long report in one pass, or working a hard problem without chopping it up. It also shows up on the bill: a full million output tokens is $10.

The concrete examples in the announcement say more than the table:

- 
**Memory:** a team of Argon agents mined fleet-wide profiling data and applied memory optimizations across Google's data centers on its own. 300 TiB freed so far, 500 TiB to 1 PiB expected in total.
- 
**C/C++ to Rust:** migrations running from libraries like re2 and libgav1 up to the 800K+ line Zircon kernel of Fuchsia OS, with automated and manual audits before anything ships.
- 
**Video decoder:** in the Rust port of libgav1 it replaced 32K lines of SIMD code with safe Rust, producing a memory-safe decoder 2.7x faster than the previous Rust port.
- 
**Quantum:** it beat a published baseline for quantum subroutine resources by 40% in minutes.

Argon was trained to find, validate and patch vulnerabilities on its own, and Google is handing it to trusted defenders **without cyber guardrails**. It is the same Fairwind route used earlier for Gemini 3.5 Flash Cyber and 3.8 Flash Cyber.

*Source: Google*

On Google's real-world vulnerability discovery benchmark Argon scores **85.8%** against 71.0% for 3.8 Flash Cyber. On Wiz's black-box penetration test, which only sees the live website, it scores **70.9%** against 58.2%. On CWE-bench v1 it shares first place at 68% with Grok 4.7 and GPT-6 Astra.

There is a field example too: through its free Scan for Good program, Wiz used Argon to find a critical flaw exposing sensitive personal data in healthcare software used by hospitals worldwide. Google says earlier frontier models had missed it.

Google lists four areas it is hardening before broad availability: refusing CBRN and cyber misuse (including monitoring the model's internal activations), resistance to indirect prompt injection, misalignment monitoring that watches the chain of thought and actions and can stop execution, and sealed sandboxes for high-risk training and evals.

*Gray Swan IPI, lower is better. Source: Google DeepMind*

The prompt injection number is the one to note if you plan to run Argon as an agent over email, web pages or documents: after 15 attempts the attack success rate is **0.7%**. Kimi K3 sits at 52.7% on the same chart.

Gemini 4 Argon was announced on 30 September 2026, but only trusted cyber defenders in the Fairwind Program have access so far. There is no public release date for developers or consumers.

The introductory price is $2 per million input tokens and $10 per million output tokens. Cached input is 95% off, so $0.10 per million. That is half the price of Claude Opus 5.5.

No. Google says the first public access goes to paid API customers and Google AI Ultra subscribers. No free tier or free Gemini app access has been announced.

Today only through the Fairwind Program for vetted cyber defenders. The next wave is paid Gemini API customers and Google AI Ultra subscribers, so a paid API project or an Ultra plan is the way to be first in line.

Not in this announcement. The only model Google introduced is Gemini 4 Argon.

*Originally published on Proje Defteri, where this post is kept up to date.*

*More: English posts and free browser tools.*

Your support means a lot! ✨ Comment 💬, like 👍, and follow 🚀 for future posts!

## Top comments (0)
