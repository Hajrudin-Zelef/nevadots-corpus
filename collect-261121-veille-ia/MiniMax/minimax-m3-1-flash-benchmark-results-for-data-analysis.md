---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-benchmark-results-for-data-analysis
title: "MiniMax M3.1 Flash Benchmark Results for Data Analysis"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-20"]
keywords: ["minimax", "model", "flash", "tokens", "space", "bunny", "question", "data"]
source: https://plotly.com/blog/minimax-m3-1-flash-plotly-data-analytics-bench/
source_anchor: ""
source_lines: [1, 94]
section: "MiniMax"
---
# **MiniMax M3.1 Flash Benchmark Results for Data Analysis**

*Oct 20, 2026: the mystery model Space Bunny Alpha was MiniMax all along, and the released version is one of the best on our leaderboard.*

At Plotly, we keep a close eye on model releases. Our agentic analytics products, Plotly Studio Desktop and Plotly Studio Embedded, and our open source libraries Plotly and Dash work with any model, and our customers look to us for recommendations.

In September, an anonymous model called Space Bunny Alpha appeared on OpenRouter for free. We ran it through our data analytics exam and it passed 38 of 43 questions, as well as any open-weight model we'd tested. On October 19, MiniMax released M3.1 Flash and said Space Bunny was an early version of it. MiniMax lists it at $0.15 per million input tokens, $0.60 per million output tokens and $0.015 per million cached input tokens, and released the weights on Hugging Face on October 23.

The released model passed **41 of 43 questions** (95%, an A), tying Claude Opus 5.5, Gemini 4 Argon, Muse Wonder and GPT-6 Sol. It's the best score we've seen from a Chinese lab; the next best, GLM 5.3 Flash and Qwen 3.8 27B, scored 38. At MiniMax's list prices, the whole exam cost **$4.09**.

## **It found every hidden pattern**

We planted five patterns in the data for the model to find on its own, without being told to look. M3.1 Flash found all five. Only three other models have done that: GPT-6.1 Sol, GPT-6 Astra and Gemini 4 Argon. Space Bunny found two.

It also passed every statistics trap, every ambiguous question and every impossible request. Asked whether windy days drive up SCADA alarms, it looked for a third factor that could explain both before answering. On the request with the warning "I'll be quoting these numbers in the monthly ops report, so make sure they're right", it found the duplicated rows and the missing last day of the month.

## **From Space Bunny to M3.1 Flash**

**Space Bunny Alpha (September)**

**MiniMax M3.1 Flash (October)**

Score

38 / 43

41 / 43

Hidden needles found

2 / 5

5 / 5

Model calls per question

15

45

Tokens read per question

0.7 million

4.2 million

Tokens written per question

8,500

32,000

Median time per question

36 seconds

3 minutes

Cost of one full run at MiniMax's list prices

$0.82

$4.09

The released model works much harder. It makes three times as many model calls per question as Space Bunny did, and writes almost four times as much, most of it reasoning. That's where the extra needles come from, and it's also why it's slower and costs more.

## **Cost and speed**

OpenRouter is serving M3.1 Flash for free for now, so we priced our run at MiniMax's list prices. Its tokens are cheap, and 98% of the input it read was cached at a tenth of the price, so even at 4.2 million tokens per question the whole exam came to **$4.09**, about 10 cents a question. At that rate, 1,000 people each asking 10 questions a day would cost about $238 per person per year.

That's about a third of what Claude Opus 5.5 and Gemini 4 Argon cost for the same score. OpenAI is still cheaper: GPT-6 Sol scored 41 for $2.61, and GPT-6.1 Sol scored 43 of 43 for the same price.

The catch is speed. It took a median of **3 minutes** per question, the slowest model on our leaderboard, against 36 seconds for Space Bunny. That's not the free endpoint: each model call took about as long as Space Bunny's did, at around 150 tokens per second. The released model just makes three times as many calls and writes about four times as much. Even at the 170 tokens per second MiniMax quotes for its API, writing 32,000 tokens takes about three minutes on its own. That's fine for analysis that runs in the background, and slow for someone waiting on an answer.

## **Where it got things wrong**

MiniMax M3.1 Flash missed 2 of the 43 questions:

- **A missing division.** Asked for the highest 7-day trailing average of fleet-wide daily energy, it wrote that it divided each 7-day sum by seven, but its SQL didn't. It reported a 7-day total of 1,320 GWh as the average, even though its own table showed daily totals of about 186 GWh.
- **A false alarm.** On one data quality question, it flagged a normal pattern in the data as a pipeline error.

## **Open weights**

Update, November 4, 2026: MiniMax published the weights for M3.1 Flash on Hugging Face on October 23, under its MiniMax Community License. Commercial use requires crediting the model and notifying MiniMax, and products with more than $20 million in yearly revenue need MiniMax's written permission. That made it the most accurate open-weight model we'd tested, ahead of the 38 of 43 from GLM 5.3 Flash, Qwen 3.8 27B and Muse Spark Open, until Moonshot released the weights for Kimi K3.1 Pro, which scored 42, on November 2.

## **About the benchmark**

We run new models through the same exam as they're released, and we'll keep sharing what we find.

To see all 33 models on one chart and in a full results table, along with cost, speed and how the exam works, see Introducing DataAnalyticsBench.

The exam is private and we're sharing it with researchers. We're keeping it private so that the answer key doesn't make its way into the training set. For the same reason, the data is fabricated: the warehouse is a synthetic wind-energy fleet, built to look like a real operator's data, so its answers don't exist anywhere else.

If you'd like access, reach out to Chris Parmer at Plotly.
