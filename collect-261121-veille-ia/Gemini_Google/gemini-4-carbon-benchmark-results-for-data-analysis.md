---
id: collect-261121-veille-ia/gemini-google/gemini-4-carbon-benchmark-results-for-data-analysis
title: "Gemini 4 Carbon Benchmark Results for Data Analysis"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-10"]
keywords: ["carbon", "gemini", "argon", "data", "google", "seconds", "model", "tokens"]
source: https://plotly.com/blog/gemini-4-carbon-plotly-data-analytics-bench/
source_anchor: ""
source_lines: [1, 288]
section: "Gemini_Google"
---
# **Gemini 4 Carbon Benchmark Results for Data Analysis**

*Nov 10, 2026: Google's strongest model, tested on real-world data analysis.*

*Update, November 12, 2026: Google has since released Gemini 4 Hydrogen, which scored 43 of 43 for $6.36. See our Gemini 4 Hydrogen post.*

At Plotly, we keep a close eye on model releases. Our agentic analytics products, Plotly Studio Desktop and Plotly Studio Embedded, and our open source libraries Plotly and Dash work with any model, and our customers look to us for recommendations.

On October 28, Google announced **Gemini 4 Carbon**, which it calls its strongest model, built for long-horizon coding, agentic work and cybersecurity. Google first gave access to a small group of cyber defenders, and this week Carbon reached developers through Google AI Studio and OpenRouter. It has a context window of 1 million tokens. Google says Carbon builds on Gemini 4 Argon, released two weeks earlier, but hasn't said how it was trained or how many parameters it has, and the weights aren't public. It launched at the same introductory price as Argon: $2 per million input tokens and $10 per million output tokens, with cached input 95% off.

We ran it through our data analytics exam. Gemini 4 Carbon passed **42 of 43 questions** (98%) for **$11.55**, one point better than Gemini 4 Argon and a little cheaper. It found all five hidden patterns and caught all five statistics traps. That ties it for fourth on our board with Grok 4.7, Kimi K3.1 Pro and Tencent's Hy4. But it's slow, with a median of 93 seconds a question, and Hy4 got the same score for $2.11. On the chart below, Carbon sits off the frontier.

## **Gemini 4 Carbon vs. Gemini 4 Argon, GPT-6.1 Sol and Hy4**

**Gemini 4 Carbon**

**Gemini 4 Argon**

**GPT-6.1 Sol**

**Hy4**

Score

42 / 43

41 / 43

43 / 43

42 / 43

Analytics questions

20 / 20

19 / 20

20 / 20

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

2 / 3

2 / 3

3 / 3

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

4 / 5

Open weights

No

No

No

Announced

Cost of one full run

$11.55

$13.47

$2.61

$2.11

Per person per year*

$671

$783

$152

$123

Median time per question

93 seconds

23 seconds

18 seconds

117 seconds

*1,000 people each asking 10 questions a day, 250 working days a year, at OpenRouter's prices when each model ran.

Carbon was perfect everywhere except data quality, where it got two of the three planted problems, the same as Argon. It also fixed the one analytics question Argon got wrong. Only three models have done better: GPT-6.1 Sol, GPT-6 Astra and Claude Fable 5.2, which all scored 43.

One run per model is a single sample. In our repeat runs of other models, scores moved by up to three points between runs, so treat a one- or two-point gap as close to a tie.

## **Thorough and slow**

Carbon takes its time. Its median question took 93 seconds, four times as long as Argon's 23. Of the 75 models we've tested, that ranks 68th, around the 90th percentile for slowness. Its whole exam took about 1 hour 48 minutes.

That work shows up in tokens, but caching keeps the bill down. Carbon read about 29.3 million input tokens, and 88% of them came from the cache at a twentieth of the input price. Our run cost $11.55, a little less than Argon's $13.47. Google calls the current price introductory, so expect that number to rise.

## **Why Carbon is slow**

*Update, November 18, 2026:* readers asked why Carbon takes so long, so we went back to our logs and ran some extra tests. We sent Carbon and ten other models the same direct requests through OpenRouter, outside our agent: a one-line prompt, and a prompt of about 130,000 tokens of table previews, mostly served from cache, each with a question that needs a tool call. We found four things:

1. **Time to first token is slower than other models.** Carbon took a median of 5 seconds to start on the one-line prompt and 11 seconds on the long one, against 2 to 2.5 seconds for Gemini 4 Hydrogen and 1 to 5 seconds for the GPT, Claude, Grok, Kimi and Qwen models we tried. The tail is long: one request took 50 seconds and one timed out.
2. **Tokens per second is slower, but not by much.** Once the wait is over, Carbon spent about 3 to 9 seconds generating each response in our exam, against about 5 for Hydrogen.
3. **It isn't wordy.** Counting its hidden thinking, Carbon wrote about 500 output tokens per request, against 780 for Hydrogen, and less over the whole exam: 220,000 tokens against 259,000. Models that are slow because they write a lot, such as Claude Fable 5.2 and Kimi K3.1 Pro, write 1,600 to 2,200 tokens per request.
4. **It works in more, smaller steps.** Carbon averaged 10.3 model calls per question against Hydrogen's 7.7, but not because it runs more queries: both made about 15 tool calls per question. Hydrogen averages about two queries per step and Carbon 1.4, so Carbon makes more round trips, and each one pays the startup wait. On the data quality audits that meant 17 round trips against 9 for about the same number of queries.

The fourth point is about behavior rather than serving. Both Gemini models dig hard: on the five questions with hidden patterns, Carbon ran 41 queries on average and Hydrogen 52, against a board median of about 25, and both found all five. When the task is "find all of the issues", that kind of exhaustive search makes time per question look slow, but in practice we'd call it thorough. Carbon's handicap is that it searches one step at a time, so it pays its slow start over and over. The startup wait is about serving and may improve: Gemini 4 Argon, which was quick in our October run, was just as slow to start in these tests.

**Requests per question**

**Seconds per request**

**First token, short prompt**

**First token, long prompt**

**Gemini 4 Carbon**

10.3

14.4

5.2 s

11.2 s

**Gemini 4 Hydrogen**

7.7

7.5

2.0 s

2.5 s

**Gemini 4 Argon**

8.1

7.4

5.1 s

24.6 s

**GPT-6.1 Sol**

6.4

7.5

1.5 s

2.7 s

**Claude Fable 5.2**

4.6

16.8

1.8 s

4.7 s

**Kimi K3.1 Pro**

7.5

34.6

0.7 s

4.2 s

**Grok 4.7**

6.3

9.8

2.3 s

2.6 s

**Hy4**

10.9

15.2

1.4 s

2.7 s

Requests and seconds per request are from each model's exam run. First-token times are medians of 5 to 8 direct requests per model on November 18, 2026, through OpenRouter; for Gemini models they include hidden thinking.

## **Where it got things wrong**

Carbon missed one question. On a data quality audit, the first problem it reported wasn't a real defect: it read an expected feature of the data as missing data, and put that ahead of the real problems it found. Its explanation was careful and its other findings were right, but an analyst would have sent the team chasing a problem that doesn't exist.

## **The Test Harness**

We run every model in Plotly Studio Embedded, a drop-in analytics agent that runs on Dash apps. This harness is tuned to run data analytics queries and make charts with multi-turn tool calling, with less emphasis on writing text. It's meant to put the data front and center.

## **Frequently asked questions**

### How good is Gemini 4 Carbon at data analysis?

On PlotlyDataAnalyticsBench it passed 42 of 43 questions, tied for fourth with Grok 4.7, Kimi K3.1 Pro and Hy4. It found every hidden pattern and caught every statistics trap. GPT-6.1 Sol, GPT-6 Astra and Claude Fable 5.2 scored 43.

### Is Gemini 4 Carbon better than Gemini 4 Argon?

On our exam, by one point: 42 against Argon's 41. It cost a little less per run, but it was four times slower.

### How much does Gemini 4 Carbon cost?

Google's introductory price is $2 per million input tokens and $10 per million output tokens, with cached input 95% off. Our whole exam cost $11.55. GPT-6.1 Sol scored 43 for $2.61.

### How fast is Gemini 4 Carbon?

Slow. Its median question took 93 seconds, against 18 for GPT-6.1 Sol and 23 for Gemini 4 Argon.

### Why is Gemini 4 Carbon so slow?

Mostly waiting for the first token. In our tests on November 18 it took a median of 5 seconds to start answering a one-line prompt and 11 seconds on a long one, against 2 to 3 seconds for Gemini 4 Hydrogen. It also makes about a third more requests per question than Hydrogen. It doesn't write more: its answers are shorter.

### Is Gemini 4 Carbon open source?

No. Google hasn't released the weights. You can use it through Google AI Studio and providers such as OpenRouter.

## **About the benchmark**

We run new models through the same exam as they're released, and we'll keep sharing what we find. We ran Gemini 4 Carbon through OpenRouter with the same harness and grading as every other model. Google AI Studio, its only OpenRouter provider, caps each response at 32,768 tokens, so we capped our requests to match.

To see all 46 models on one chart and in a full results table, along with cost, speed and how the exam works, see Introducing DataAnalyticsBench.

The exam is private and we're sharing it with researchers. We're keeping it private so that the answer key doesn't make its way into the training set. For the same reason, the data is fabricated: the warehouse is a synthetic wind-energy fleet, built to look like a real operator's data, so its answers don't exist anywhere else.

If you'd like access, reach out to Chris Parmer at Plotly.
