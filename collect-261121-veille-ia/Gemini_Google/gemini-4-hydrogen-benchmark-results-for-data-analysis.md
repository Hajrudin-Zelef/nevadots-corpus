---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-benchmark-results-for-data-analysis
title: "Gemini 4 Hydrogen Benchmark Results for Data Analysis"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "hydrogen", "model", "data", "argon", "questions", "google", "gpt-6"]
source: https://plotly.com/blog/gemini-4-hydrogen-plotly-data-analytics-bench/
source_anchor: ""
source_lines: [1, 191]
section: "Gemini_Google"
---
# **Gemini 4 Hydrogen Benchmark Results for Data Analysis**

*Nov 12, 2026: Google's fast, efficient Gemini 4 model, tested on real-world data analysis.*

At Plotly, we keep a close eye on model releases. Our agentic analytics products, Plotly Studio Desktop and Plotly Studio Embedded, and our open source libraries Plotly and Dash work with any model, and our customers look to us for recommendations.

On November 12, Google announced **Gemini 4 Hydrogen**, the fast, efficient model in its Gemini 4 family, built for everyday coding and agentic work. Google positions it alongside its frontier models, Gemini 4 Carbon and Gemini 4 Argon, and says it scores 51 on the Artificial Analysis Intelligence Index at its default high thinking level, one point above Argon, and 54 at its highest setting. Google also says it finishes tasks 2.5 times faster than Gemini 3.8 Flash. It has a context window of 1 million tokens, priced flat across the whole window at $1.50 per million input tokens and $7.50 per million output tokens. Google hasn't said how it was trained or how many parameters it has, and the weights aren't public.

We ran it through our data analytics exam. Gemini 4 Hydrogen passed **all 43 questions** for **$6.36**, the fourth model to ace the exam, after GPT-6 Astra, GPT-6.1 Sol and Claude Fable 5.2. It beat both of Google's frontier models, Carbon at 42 and Argon at 41, for about half their cost, and it found all five hidden patterns, which only eight of the 48 models on our board have done. But GPT-6.1 Sol got the same perfect score for $2.61, so on the chart below, Hydrogen sits just off the frontier.

## **Gemini 4 Hydrogen vs. Gemini 4 Carbon, Gemini 4 Argon and GPT-6.1 Sol**

**Gemini 4 Hydrogen**

**Gemini 4 Carbon**

**Gemini 4 Argon**

**GPT-6.1 Sol**

Score

43 / 43

42 / 43

41 / 43

43 / 43

Analytics questions

20 / 20

20 / 20

19 / 20

20 / 20

Ambiguous questions

5 / 5

5 / 5

5 / 5

5 / 5

Impossible requests

5 / 5

5 / 5

5 / 5

5 / 5

Data quality

3 / 3

2 / 3

2 / 3

3 / 3

Statistics traps

5 / 5

5 / 5

5 / 5

5 / 5

Hidden needles

5 / 5

5 / 5

5 / 5

5 / 5

Open weights

No

No

No

No

Cost of one full run

$6.36

$11.55

$13.47

$2.61

Per person per year*

$370

$671

$783

$152

Median time per question

32 seconds

93 seconds

23 seconds

18 seconds

*1,000 people each asking 10 questions a day, 250 working days a year, at OpenRouter's prices when each model ran.

Hydrogen is the first Gemini model to be perfect in every category. Carbon and Argon each missed one of the three planted data quality problems, and Argon also missed one analytics question. Because it's a perfect score, we checked each of Hydrogen's 43 passes against the answer key by hand.

One run per model is a single sample. In our repeat runs of other models, scores moved by up to three points between runs, so treat a one- or two-point gap as close to a tie.

## **What it got right**

The hidden-pattern questions are the hardest part of our exam. Each one points toward something unusual in the data without naming it, and the model has to go looking. Hydrogen found all five. Only seven other models have: GPT-6.1 Sol, GPT-6 Astra, Claude Fable 5.2, MiniMax M3.1 Flash, Kimi K3.1 Pro, and Hydrogen's two bigger siblings. Every Gemini 4 model we've tested has found all five.

It looked hard. On those five questions it ran about 54 queries each, against a board median of about 25, and more than Carbon's 41 or Argon's 46. But it often ran several queries at once: across the five questions it made 95 model calls for 271 tool calls. Google says Hydrogen batches file reads and searches in parallel when it explores a codebase, and it did the same with our queries.

It also caught all five statistics traps, the questions where the obvious calculation gives a misleading answer. Fourteen of the 48 models on our board have done that. And it asked about or stated its assumptions on every ambiguous question, turned down every impossible request, and caught all three planted data quality problems.

## **Cost and speed**

Hydrogen's run cost $6.36, about half of Carbon's $11.55 and Argon's $13.47. It read about 17.8 million input tokens, and 88% of them came from the cache. At our rollout numbers, that comes to about $370 per person per year. Among models that scored 42 or 43, only GPT-6.1 Sol, at $152 per person, and Tencent's Hy4, at $123, cost less.

Google pitches Hydrogen as its fast model, and next to Carbon it is. Its median question took 32 seconds, against Carbon's 93, and its whole exam took about 43 minutes, against Carbon's 1 hour 48 minutes. But Argon was a little faster, at 23 seconds, and GPT-6.1 Sol is still the fastest model with a perfect score, at 18. Hydrogen sits in the middle of our board: 26 of the 48 models have a faster median.

If you're building on Google's models, Hydrogen is now the Gemini model we'd pick for data analysis: it scored higher than Carbon and Argon, at about half the cost, and about three times as fast as Carbon. If you're choosing across providers, GPT-6.1 Sol gets the same score for about 40% of the cost.

## **Google's models over time**

Here are all six Google models we've tested, from Gemini 2.5 Pro in June 2025 to Hydrogen today, with every other model in grey. Gemini 2.5 Pro scored 30 of 43 for $47.43 a run; Hydrogen scores 43 for $6.36. Carbon is the most accurate of the earlier Gemini 4 models but one of the slowest at 93 seconds a question; Hydrogen gets a perfect score at 32 seconds.

## **The Test Harness**

We run every model in Plotly Studio Embedded, a drop-in analytics agent that runs on Dash apps. This harness is tuned to run data analytics queries and make charts with multi-turn tool calling, with less emphasis on writing text. It's meant to put the data front and center.

## **Frequently asked questions**

### How good is Gemini 4 Hydrogen at data analysis?

On PlotlyDataAnalyticsBench it passed all 43 questions, the fourth model to do so after GPT-6 Astra, GPT-6.1 Sol and Claude Fable 5.2. It was perfect in every category, including all five hidden patterns and all five statistics traps.

### Is Gemini 4 Hydrogen better than Gemini 4 Carbon?

On our exam, yes: 43 against Carbon's 42 and Argon's 41. It cost about half as much per run as either, and its median question took 32 seconds, against 93 for Carbon.

### How much does Gemini 4 Hydrogen cost?

Google prices it at $1.50 per million input tokens and $7.50 per million output tokens, flat across its 1 million-token context window. Our whole exam cost $6.36. GPT-6.1 Sol scored 43 for $2.61.

### How fast is Gemini 4 Hydrogen?

Its median question took 32 seconds, against 18 for GPT-6.1 Sol, 23 for Gemini 4 Argon and 93 for Gemini 4 Carbon.

### Is Gemini 4 Hydrogen open source?

No. Google hasn't released the weights. You can use it through the Gemini API in Google AI Studio and providers such as OpenRouter.

## **About the benchmark**

We run new models through the same exam as they're released, and we'll keep sharing what we find. We ran Gemini 4 Hydrogen through OpenRouter on its launch day, with the same harness and grading as every other model.

To see all 48 models on one chart and in a full results table, along with cost, speed and how the exam works, see Introducing DataAnalyticsBench.

The exam is private and we're sharing it with researchers. We're keeping it private so that the answer key doesn't make its way into the training set. For the same reason, the data is fabricated: the warehouse is a synthetic wind-energy fleet, built to look like a real operator's data, so its answers don't exist anywhere else.

If you'd like access, reach out to Chris Parmer at Plotly.
