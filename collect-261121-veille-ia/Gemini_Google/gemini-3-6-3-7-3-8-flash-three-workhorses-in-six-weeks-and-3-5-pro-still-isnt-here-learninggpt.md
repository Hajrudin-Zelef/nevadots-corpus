---
id: collect-261121-veille-ia/gemini-google/gemini-3-6-3-7-3-8-flash-three-workhorses-in-six-weeks-and-3-5-pro-still-isnt-here-learninggpt
title: "Gemini 3.6 → 3.7 → 3.8 Flash: three workhorses in six weeks (and 3.5 Pro still isn’t here) — LearningGPT"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "google", "everyone", "model", "here", "three", "models"]
source: https://learninggpt.ai/lessons/gemini/gemini-3-6-flash
source_anchor: ""
source_lines: [1, 148]
section: "Gemini_Google"
---
# Gemini 3.6 Flash lands — and the Pro everyone wants still isn't here.

On July 21, 2026, Google shipped three new Gemini models: **3.6 Flash** (the new everyday workhorse, already in the Gemini app), **3.5 Flash-Lite** (the speed-and-price model, rolling into Google Search), and **3.5 Flash Cyber** (a security specialist you can't have). What Google didn't ship is the one everyone's waiting for: Gemini 3.5 Pro. Here's what actually changed for you, what the confusing names mean, and where the flagship is.

Google has now replaced its workhorse *twice* since this lesson was written: **3.7 Flash** on August 13, then **Gemini 3.8 Flash** on September 2 — Google's "best reasoning and coding model yet," at the same price and speed as 3.7. The pattern to notice: 3.6 Flash went to *everyone* in the Gemini app; 3.7 and 3.8 go to **paying subscribers**. 3.7 Flash ↓ · 3.8 Flash and 3.8 Flash Cyber ↓

The Flash line has a successor: **Gemini 4 Hydrogen** (November 12), which Google pitches as the upgrade path from 3.8 Flash — and, unlike 3.7 and 3.8, it’s listed for the Gemini app and Google Search for everyone. Gemini 4 Hydrogen ↓

## 01 Three models, one minute

| Model | What it's for | Who gets it | 
|---|---|---|
| Gemini 3.6 Flash | The new default workhorse — better coding, knowledge work, and multimodal answers, with less rambling | **Everyone, in the Gemini app** — plus developers (AI Studio, Android Studio, Antigravity) and Gemini Enterprise | 
| Gemini 3.5 Flash-Lite | Speed and volume: 350 tokens/second, Google's cheapest 3.5-class model | Developers today; **rolling out inside Google Search** | 
| Gemini 3.5 Flash Cyber | Fine-tuned to find and fix software security vulnerabilities, inside Google's CodeMender agent | **Governments and trusted partners only** — limited-access pilot | 

**3.6**, while the other two new models are

**3.5**-class. Read "3.6 Flash" as "3.5 Flash, improved" — Google bumped the number for the upgraded workhorse and kept the specialist models in the 3.5 family.

## 02 What 3.6 Flash actually improves

The headline isn't raw intelligence — it's **efficiency**. Google says 3.6 Flash uses **17% fewer output tokens** than 3.5 Flash on the Artificial Analysis index while scoring better on coding (DeepSWE: 49% vs 37%), computer-use tasks (OSWorld-Verified: 83.0% vs 78.4%), and knowledge work. In practice that means faster, less padded answers that take fewer steps to finish multi-part tasks.

For developers it's directly cheaper — $1.50 per million input tokens and $7.50 per million output tokens, less than 3.5 Flash. For everyone else, token efficiency shows up as speed: shorter chains of reasoning between your question and a finished answer. Note those prices are developer API rates, not what Gemini app subscribers pay.

One more quiet upgrade: **computer use is now a built-in tool** — the model can drive on-screen interfaces, which is the plumbing behind agents that click and type for you. We covered that capability when it debuted in 3.5 Flash; 3.6 does it measurably better.

## 03 The elephant: where's 3.5 Pro?

Google's flagship Pro model was last updated in **February**. Since then OpenAI shipped GPT-5.5 and GPT-5.6, and Anthropic shipped Opus 4.8, Sonnet 5, and Fable 5. In Tuesday's post Google says 3.5 Pro is "currently testing with partners" and will be broadly available "as soon as it's ready" — and that pre-training for **Gemini 4**, its "most ambitious" run yet, has already started.

This section is now out of date: Google's Gemini release notes for **October 13, 2026** list **Gemini 4 Argon**, described as built for complex, multi-step tasks, rolling out in the Gemini app to Google AI Pro and Ultra subscribers. The notes don't mention 3.5 Pro. See what Gemini costs for plan details.

The honest read: Flash models are Google's volume play — fast, cheap, and now genuinely good. But if you're choosing a tool for the hardest reasoning work today, Google's best answer is five months old. If your work leans on frontier reasoning, our which-AI-for-which-job lesson stays the guide until Pro lands.

### What to do with this

- In the Gemini app, just keep working — 3.6 Flash is arriving as the everyday model, no toggle hunt needed
- Notice answer style: if Gemini suddenly feels snappier and less wordy this week, that's 3.6 Flash
- Building on the API? Skip straight to **3.8 Flash** — it beats 3.7 Flash on Google's published benchmarks at the same $0.75/$3.75 introductory price (through December 31, 2026). Keep 3.7 Flash only where token cost, not quality, is the constraint
- Don't wait on Flash Cyber — it's a government/partner pilot, not a consumer feature
- Watching for 3.5 Pro? So are we — we'll ship the lesson the day it's real

## 04 August 13: Gemini 3.7 Flash arrives

Three weeks is a short shelf life for a workhorse model. On **August 13, 2026** Google released **Gemini 3.7 Flash**, calling it its "most intelligent workhorse model yet for coding and agents" and crediting developer feedback for the fast turnaround. Every headline number is a jump over the model this lesson is about:

| Benchmark | 3.6 Flash | 3.7 Flash | 
|---|---|---|
| DeepSWE v1.1 (long-horizon software engineering) | 49.0% | **65.3%** | 
| FrontierCode 1.1 Main (production-ready code) | 34.4% | **43.6%** | 
| WebDev Arena (Elo) | 1538 | **1588** | 
| GDP.pdf (complex document comprehension) | 22.0% | **34.0%** | 
| AutomationBench (real business workflows) | 17.0% | **30.4%** | 

And it is **half the price**: **$0.75 per million input tokens and $3.75 per million output tokens**, against 3.6 Flash's $1.50 and $7.50. That is an *introductory* rate — Google's own footnote says it expires on **December 31, 2026**, and from January 1, 2027 the price returns to $1.50/$7.50. Budget on the 2027 number, not the launch one.

**Gemini Spark**— the 24/7 personal agent — and Spark is limited to

**Google AI Pro and Ultra subscribers**in 160-plus supported countries. If you use Gemini for free, Google's announcement does not say you are getting 3.7 Flash. Developers get it in the Gemini API via AI Studio, Android Studio and Antigravity; enterprises get it in Gemini Enterprise.

Nothing in the section above about **3.5 Pro** changed. Google's flagship Pro model is still the February build, and 3.7 Flash is another Flash — more evidence that the volume tier is where Google is putting its shipping energy. If your work needs frontier reasoning rather than fast reasoning, that gap is still the story.

## 05 September 2: Gemini 3.8 Flash (and 3.8 Flash Cyber)

Three Flash releases in six weeks. On **September 2, 2026** Google introduced **Gemini 3.8 Flash**, calling it "our best reasoning and coding model yet, at the same speed and low cost of 3.7." Two variants ship from the same core: 3.8 Flash for everyone building or subscribing, and **3.8 Flash Cyber**, a security specialist that replaces the 3.5 Flash Cyber pilot described above and is available only to "trusted defenders" through Google's new **Fairwind Program**.

| What Google says changed | 3.7 Flash | 3.8 Flash | 
|---|---|---|
| Long-horizon software engineering (DeepSWE v1.1) | 65.3% | **Higher** — Google says 3.8 "outperforms most larger frontier models" here, but published a chart rather than a headline number | 
| HLE-Verified (multi-step reasoning) | — | **54.9%** | 
| Finance and legal agent benchmarks (Vals Finance Agent V2, Harvey's Legal Agent Benchmark) | Baseline | **Ahead of 3.7 Flash** and, per Google, other frontier models | 
| API price (introductory, through Dec 31, 2026) | $0.75 / $3.75 per 1M tokens | **Same** — $0.75 in, $3.75 out; $1.50 / $7.50 from January 1, 2027 | 

The design choice behind the numbers is worth knowing before you switch: **3.8 Flash "works harder."** Google says it takes extra reasoning steps and calls tools repeatedly before answering, and "might use more tokens to maximize performance, especially at higher effort levels." Same per-token price, potentially more tokens per task — so a 3.8 bill can run above a 3.7 bill for the same job. Google's own advice: drop the effort level when compute is the constraint, or stay on 3.7 Flash, which "remains fully supported for efficiency-first workloads."

**Google AI Pro and Ultra subscribers**in the Gemini app, AI Mode in Search, and Gemini in Sheets; developers get it in the Gemini API (AI Studio, Android Studio, Antigravity, Stitch); enterprises in Gemini Enterprise. The announcement does not mention the free Gemini tier, and it does not mention 3.5 Pro at all — the flagship gap in section 03 is unchanged.

On the security side, Google reports 3.8 Flash Cyber patching Chrome vulnerabilities at 2.6× the rate of larger commercial models and finding a critical vulnerability in under two hours. Those are Google's numbers about Google's code, and the model is not something you can sign up for — but it explains why the consumer model also got a measurable jump in prompt-injection resistance (Google cites the Gray Swan benchmark), which matters for anyone letting Gemini act on email and documents.

## 06 November 12: Gemini 4 Hydrogen replaces the Flash line

Google’s next workhorse didn’t get a Flash name. On **November 12, 2026** it released **Gemini 4 Hydrogen**, which Google describes as “a major standalone upgrade for teams migrating from Gemini 3.8 Flash.” Where Gemini 4 Carbon and Argon are the slow, deep thinkers, Hydrogen is built to be the fast one — writing code, running tests, doing everyday agent tasks.

| What Google says | 3.8 Flash | 4 Hydrogen | 
|---|---|---|
| Speed (at default thinking level) | Baseline | **About 2.5× faster** | 
| Terminal-Bench 4.1 (agentic coding) | 18.2% | **57%** | 
| Fabricated claims | Baseline | **Cut by more than 5×** | 
| Thinking levels | Effort levels | Low, medium, high (default), new **xhigh** | 
| API price per 1M tokens | $0.75 / $3.75 intro to Dec 31; then $1.50 / $7.50 | **$1.50 / $7.50** , flat across the 1M-token context | 

The availability pattern flips back. 3.7 and 3.8 Flash went to paying subscribers; Google says Hydrogen goes to **the Gemini app, Google Search for all users globally, every tier of the Gemini API and Google AI Studio, and Gemini for business**, with Google Antigravity access for paid AI Pro and Ultra subscribers. Google also claims Hydrogen edges past Gemini 4 Argon on the Artificial Analysis Intelligence Index (51 vs. 50 at default) — a narrow, single-index result, so don’t read it as “Hydrogen replaces Argon.”

## Gemini 3.6, 3.7 and 3.8 Flash — your questions, answered

## What replaced Gemini 3.8 Flash?

**Gemini 4 Hydrogen**, which it calls a major upgrade for teams migrating from 3.8 Flash: about 2.5× faster, Terminal-Bench 4.1 of 57% vs 18.2%, and more than 5× fewer fabricated claims, per Google. It costs $1.50 / $7.50 per million tokens on the API and is listed for the Gemini app, Google Search and all API tiers.

## What did Google release on July 21, 2026?

**Gemini 3.6 Flash**(new workhorse),

**Gemini 3.5 Flash-Lite**(fastest and cheapest 3.5-class model), and

**Gemini 3.5 Flash Cyber**(a security-specialist model available only to governments and trusted partners via a limited pilot).

## Can I use Gemini 3.6 Flash in the Gemini app?

## Is Gemini 3.6 Flash better than 3.5 Flash?

**17% fewer output tokens**and costing developers less ($1.50/1M input, $7.50/1M output).

## Where is Gemini 3.5 Pro?

**Gemini 4 Argon**as its newest frontier model for Google AI Pro and Ultra subscribers in the Gemini app.

## What is Gemini 3.7 Flash and how is it different from 3.6 Flash?

**August 13, 2026**, three weeks after 3.6 Flash. On Google's published numbers it beats 3.6 Flash on DeepSWE v1.1 (65.3% vs 49.0%), FrontierCode 1.1 Main (43.6% vs 34.4%), WebDev Arena Elo (1588 vs 1538), GDP.pdf (34.0% vs 22.0%) and AutomationBench (30.4% vs 17.0%) — and it costs half as much per token.

## How much does Gemini 3.7 Flash cost?

**$0.75 per million input tokens and $3.75 per million output tokens**— half of 3.6 Flash. Google's footnote says that introductory pricing expires on

**December 31, 2026**; from January 1, 2027 it becomes $1.50/1M input and $7.50/1M output, the same as 3.6 Flash. These are developer API rates, not Gemini app subscription prices.

## Can I use Gemini 3.7 Flash for free in the Gemini app?

**Gemini Spark**, the 24/7 personal agent, which is available to

**Google AI Pro and Ultra subscribers**in over 160 supported countries. That's a change from 3.6 Flash, which Google said was available "for everyone via the Gemini app."

## What is Gemini 3.5 Flash Cyber?

**governments and trusted partners**in a limited-access pilot. As of September 2, 2026 it has been succeeded by

**3.8 Flash Cyber**(below).

## What is Gemini 3.8 Flash?

**September 2, 2026**, three weeks after 3.7 Flash — Google's third Flash release in six weeks. Google calls it its best reasoning and coding model yet, at the same speed and the same introductory price as 3.7 Flash ($0.75 per million input tokens, $3.75 per million output, through December 31, 2026). It scores 54.9% on HLE-Verified and leads 3.7 Flash on Google's software-engineering, finance and legal agent benchmarks.

## Is Gemini 3.8 Flash free in the Gemini app?

**Google AI Pro and Ultra subscribers**in the Gemini app, AI Mode in Search and Gemini in Google Sheets, plus developers (Gemini API via AI Studio, Android Studio, Antigravity, Stitch) and Gemini Enterprise. The free tier is not mentioned.
