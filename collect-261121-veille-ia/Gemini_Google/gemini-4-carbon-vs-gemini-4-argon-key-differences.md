---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-vs-gemini-4-argon-key-differences
title: "Gemini 4 Carbon vs Gemini 4 Argon: Key Differences"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-12"]
keywords: ["gemini", "argon", "google", "carbon", "tokens", "price", "november", "model"]
source: https://emergent.sh/learn/gemini-4-carbon-vs-gemini-4-argon
source_anchor: ""
source_lines: [1, 180]
section: "Gemini_Google"
---
#### TL;DR

- **Release:****Gemini 4 Argon** was announced on September 30, 2026.**Gemini 4 Carbon** followed on October 28, 2026 and builds on Argon.
- **Price:** Both models cost $2.00 per million input tokens and $10.00 per million output tokens through November 30, 2026. From December 1, 2026, both cost $4.00 and $20.00.
- **Capability:** On Artificial Analysis,**Gemini 4 Carbon (high)** scores 60 on the Intelligence Index against 50 for**Gemini 4 Argon (high)** .
- **Efficiency:** Google calls**Gemini 4 Argon** "the ideal choice for the majority of software engineering and knowledge work." Carbon uses more tokens to reason and verify.
- **Pick Carbon** for the hardest multi-step coding, science, and security work.**Pick Argon** for routine software engineering and knowledge work.


All figures are as of November 12, 2026.

Google released two frontier models in the Gemini 4 family within four weeks of each other. The two share a price, a family name, and a safety stack, so choosing between them is not obvious. This guide covers the Gemini 4 Carbon vs Gemini 4 Argon question on release timing, pricing, benchmarks, safety, and best use cases. Figures come from Google and Artificial Analysis, as of November 12, 2026.

## What Is Gemini 4 Argon?

Google announced Gemini 4 Argon on September 30, 2026. Google describes it as built to sustain deep reasoning across complex, long-horizon workflows. The first rollout went to a limited group of trusted cyber defenders through Google's Fairwind Program. Google said a broader release would start with paid API customers and Google AI Ultra subscribers.

**Gemini 4 Argon** raised the output limit to 1M tokens, up from 64K. Google reported 77.9% on DeepSWE v1.1 and 51.3% on AutomationBench, which ranked first at launch.

Google's Gemini API pricing page now describes Gemini 4 Argon as "our efficient Gemini 4 frontier reasoning model." It says the model delivers Gemini 4 intelligence with faster responses and lower token spend per turn than Gemini 4 Carbon.

## What Is Gemini 4 Carbon?

Google announced Gemini 4 Carbon on October 28, 2026. Google calls it "our strongest model with state-of-the-art cybersecurity and long horizon capabilities." The post says Carbon builds on **Gemini 4 Argon**. Access started with a subset of cyber defenders and expanded gradually to trusted partners. Google has since made Carbon available to developers, enterprises, scientists, and consumers.

Google's model page lists the same specifications for both: a 1M-token context window, text, image, audio, video, and PDF inputs, and a maximum output of 65K or 1M tokens. In short, Gemini 4 Carbon is the upgrade and Gemini 4 Argon is the efficient option.

## Gemini 4 Carbon vs Gemini 4 Argon at a Glance

| **Feature** | **Gemini 4 Carbon** | **Gemini 4 Argon** | 
|---|---|---|
| Announced | October 28, 2026 | September 30, 2026 | 
| Google's positioning | "Our most capable reasoning model" | "Our efficient Gemini 4 frontier reasoning model" | 
| Relationship | Builds on Gemini 4 Argon | Earlier release in the Gemini 4 family | 
| Input price (through November 30, 2026) | $2.00 per 1M tokens | $2.00 per 1M tokens | 
| Output price (through November 30, 2026) | $10.00 per 1M tokens | $10.00 per 1M tokens | 
| Input price (from December 1, 2026) | $4.00 per 1M tokens | $4.00 per 1M tokens | 
| Output price (from December 1, 2026) | $20.00 per 1M tokens | $20.00 per 1M tokens | 
| Free tier | Not available | Not available | 
| Paid-tier content used to improve Google's products | No | No | 
| Google-reported CWE-bench score | 71.0% | 68% | 
| Gray Swan indirect prompt injection attack success rate | 0.0% | 0.7% | 

Sources: Google's Gemini API pricing page and Google's launch posts, as of November 12, 2026.

## Gemini 4 Carbon vs Gemini 4 Argon: Pricing and Access

The two models share one price list. Google's pricing page shows the same standard rates for both. There is no price reason to pick one over the other. The real cost difference comes from how many tokens each model spends on a task.

Source: Google's Gemini API pricing page, as of November 12, 2026. Rates apply to both Gemini 4 Carbon and Gemini 4 Argon.

**Also Read:** Gemini 4 Carbon pricing for worked cost examples at both rate periods.

### What the Price Change Means

Both models double in price on December 1, 2026. Input moves from $2.00 to $4.00. Output moves from $10.00 to $20.00. Google's launch posts call the current rates an introductory price. Teams that budget on the current rates should model the new rates now. For Argon’s own rate card, including the Contributor tier and cached input, see our Gemini 4 Argon pricing guide.

### Access and Data Use

Neither model is available on Google's Free tier. On the standard paid tier, Google states that content is not used to improve its products.

Gemini 4 Argon also has a Contributor tier, which Google offers as a private preview on an invite-only basis. It costs $1.00 per million input tokens and $5.00 per million output tokens, with cached input at $0.05. Google lists content on this tier as used to improve its products. Speed and reliability are equivalent to Standard tier inference. The Gemini 4 Carbon pricing section shows no Contributor tab (Google, as of November 12, 2026).

Access was staged at launch. Google released Gemini 4 Argon first to trusted cyber defenders, and Gemini 4 Carbon to a subset of cyber defenders and trusted partners. Google's availability post says Gemini 4 Carbon is now available through the API in Google AI Studio and Gemini for business, at the same introductory price as Gemini 4 Argon. It is also rolling out to Google AI Ultra subscribers, Google Antigravity, the Gemini app, and AI Mode in Search.

## Gemini 4 Carbon vs Gemini 4 Argon: Benchmarks

Google publishes most of the head-to-head figures. Artificial Analysis adds independent scores for Gemini 4 Carbon.

### Google-Reported Results

Google's two launch posts share a few benchmarks, which makes a direct comparison possible.

Source: Google's launch posts for Gemini 4 Argon and Gemini 4 Carbon, as of November 12, 2026. Vendor-reported.

Each post also reports results for one model only:

- **Gemini 4 Argon:** 77.9% on DeepSWE v1.1, 51.3% on AutomationBench, and 91.7% on LVBench for long video understanding.
- **Gemini 4 Carbon:** 88.2% on SEC-bench Pro for finding vulnerabilities and 54.8% on ExploitGym for exploitation. Google says it evaluated both cybersecurity results without guardrails. On WebDev Arena, Google reports a rise of 89 points to 1768 Elo, ranking fourth.

These comparisons come from Google's own testing. Independent results should carry more weight.

For each model’s full set of scores, see our Gemini 4 Argon benchmarks and Gemini 4 Carbon benchmarks guides.

### Independent Results for Gemini 4 Carbon

Artificial Analysis tests both models on the same evaluations. This table compares them at high effort:

| **Benchmark** | **Gemini 4 Argon (high)** | **Gemini 4 Carbon (high)** | 
|---|---|---|
| Intelligence Index | 50 | 60 | 
| AA-Briefcase v1.1 (Elo) | 1484 | 1611 | 
| AutomationBench-AA | 77.5% | 77.8% | 
| Terminal-Bench 4.0 | 57.1% | 65.2% | 
| FrontierCode-AA 2.0 | 25.7% | 40.6% | 
| AA-Omniscience v1.1 | 46 | 54 | 
| GDP.pdf | 21.8% | 28.4% | 
| HLE-Diamond | 56.7% | 69.1% | 
| CritPt v2 (Hard Subset) | 52.5% | 69.0% | 
| Terminal-Bench-Science 0.1 | 52.9% | 64.8% | 

Source: Artificial Analysis, as of November 12, 2026. Both models at high effort.

**Gemini 4 Carbon (high)** leads on every benchmark in the table. The gap is widest on FrontierCode-AA 2.0 (40.6% against 25.7%) and CritPt v2 (69.0% against 52.5%). The two are close on AutomationBench-AA, at 77.8% and 77.5%.

### Gemini 4 Carbon at Lower Effort

Artificial Analysis also lists **Gemini 4 Carbon** at lower effort levels. Even at medium effort, it beats **Gemini 4 Argon (high)** on the Intelligence Index.

| **Model** | **Intelligence Index** | **Terminal-Bench 4.0** | **FrontierCode-AA 2.0** | **HLE-Diamond** | 
|---|---|---|---|---|
| Gemini 4 Argon (high) | 50 | 57.1% | 25.7% | 56.7% | 
| Gemini 4 Carbon (low) | 54 | 62.6% | 29.6% | 61.9% | 
| Gemini 4 Carbon (medium) | 58 | 60.6% | 39.5% | 67.4% | 
| Gemini 4 Carbon (high) | 60 | 65.2% | 40.6% | 69.1% | 

Source: Artificial Analysis, as of November 12, 2026.

Google's launch post reported a Carbon score of 59 and a first-place rank on October 28, 2026. Artificial Analysis has since updated the index. Its overall chart now places **Gemini 4 Carbon (high)** second, behind **Claude Fable 5.2 (max)** at 65.

## Speed, Token Spend, and Reliability

The two models differ most in how they work, not what they cost.

### Speed and Token Spend

Google's pricing page says Gemini 4 Argon delivers faster responses and lower token spend per turn than Gemini 4 Carbon across complex coding, agentic, math, and science tasks. Google calls Argon "the ideal choice for the majority of software engineering and knowledge work." Carbon is for tasks that test the limits of a model's intelligence, and it uses more tokens. Both models bill thinking tokens at the same output rate.

Artificial Analysis measured both at high effort:

| **Measure** | **Gemini 4 Argon (high)** | **Gemini 4 Carbon (high)** | 
|---|---|---|
| Time to first answer token | 37.44 s | 66.35 s | 
| End-to-end response time | 110.41 s | 155.76 s | 
| Output speed | 65 tokens/s | 81 tokens/s | 
| Time per Index task | 29.7 min | 25.3 min | 
| Output tokens per task | 116k | 123k | 
| Reasoning tokens per task | 68.3k | 72.2k | 
| Cost per task | $9.95 | $7.76 | 
| Cost to run the Intelligence Index | $7,620 | $8,240 | 
| Output tokens to run the Index | 113M | 205M | 

Source: Artificial Analysis, as of November 12, 2026. Costs use $4.00 input and $20.00 output per 1M tokens, the rates from December 1, 2026.

The numbers are split into two. **Gemini 4 Argon (high)** returns the first answer token sooner (37.44 s against 66.35 s). **Gemini 4 Carbon (high)** generates tokens faster, finishes Index tasks sooner, and costs less per task ($7.76 against 8,240 against $7,620) because it produced 205M output tokens against 113M.

Lower effort levels change the picture. **Gemini 4 Carbon (medium)** costs $6.59 per task and takes 22.9 minutes. **Gemini 4 Carbon (low)** costs $7.19 per task and returns the first answer token in 12.40 s. Both finish tasks faster than **Gemini 4 Argon (high)**.

### Reliability and Safety

**Gemini 4 Carbon** targets a different problem: finishing long tasks without shortcuts or invented claims. Google reports these changes from **Gemini 4 Argon**:

- **Prompt injection:** Google says**Gemini 4 Argon** previously led the Gray Swan indirect prompt injection benchmark with a 0.7% attack success rate.**Gemini 4 Carbon** achieves 0.0%, resisting every evaluated attack attempt.
- **Fabrication and misalignment:** Google reports reduced misalignment and fabrication rates in live coding environments with its own teams.

## Which Should You Choose?

Both models cost the same through November 30, 2026, and the same from December 1, 2026. The choice rests on the work.

| **Use case** | **Better fit** | **Why** | 
|---|---|---|
| Long-horizon coding and debugging | Gemini 4 Carbon | FrontierCode-AA 2.0 at 40.6% against 25.7% (Artificial Analysis, high effort) | 
| Security research and vulnerability patching | Gemini 4 Carbon | 71.0% on CWE-bench against 68%, plus 88.2% on SEC-bench Pro (Google) | 
| Agents that read untrusted content | Gemini 4 Carbon | 0.0% attack success rate on Gray Swan, against 0.7% (Google) | 
| Most software engineering and knowledge work | Gemini 4 Argon | Google calls it the ideal choice for the majority of this work | 
| Fast first answer at high effort | Gemini 4 Argon | 37.44 s to first answer token against 66.35 s (Artificial Analysis) | 
| Cost per hard task | Gemini 4 Carbon (medium) | $6.59 per task against $9.95 for Argon (high) (Artificial Analysis) | 
| Long video analysis | Gemini 4 Argon | 91.7% on LVBench (Google, at launch) | 

Figures from Google's launch posts and Artificial Analysis, as of November 12, 2026.

A simple rule works well. Start with Gemini 4 Argon for routine turns. Move to Gemini 4 Carbon when a task fails, runs long, or touches security. To build that escalation into a workflow, an AI agent builder lets you describe which steps run on which model and where a person reviews. Route by difficulty rather than picking one model for everything.

## Conclusion

Gemini 4 Carbon is the stronger model. Gemini 4 Argon is the efficient one. They cost the same, so the decision rests on task difficulty, token spend, and risk. Gemini 4 Carbon suits long-horizon engineering, security work, and agents that handle untrusted input. Gemini 4 Argon suits routine, high-volume turns. Test both on real tasks before December 1, 2026, when each price doubles.

Choosing a model is only one piece of the work. A model can write code, draft responses, or reason through a problem, but on its own it doesn't give you a working product. You still need an interface, a backend, a database, and somewhere to deploy it. Emergent covers that part. Describe what you want in plain English, and Emergent builds a full-stack app, whether that's an internal tool, a chatbot, or a dashboard.

Start building with Emergent today.
