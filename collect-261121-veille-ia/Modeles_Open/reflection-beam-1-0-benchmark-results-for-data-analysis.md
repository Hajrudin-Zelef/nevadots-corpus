---
id: collect-261121-veille-ia/modeles-open/reflection-beam-1-0-benchmark-results-for-data-analysis
title: "Reflection Beam 1.0 Benchmark Results for Data Analysis"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-10-17"]
keywords: ["model", "beam", "data", "reflection", "question", "open-weight", "questions", "every"]
source: https://plotly.com/blog/reflection-beam-1-0-plotly-data-analytics-bench/
source_anchor: ""
source_lines: [1, 128]
section: "Modeles_Open"
---
# **Reflection Beam 1.0 Benchmark Results for Data Analysis**

*Oct 17, 2026: a new US open-weight model from Reflection AI is careful with messy data, and expensive to run on today's providers.*

At Plotly, we keep a close eye on model releases. Our agentic analytics products, Plotly Studio Desktop and Plotly Studio Embedded, and our open source libraries Plotly and Dash work with any model, and our customers look to us for recommendations. On October 16, Reflection AI released the weights for Beam 1.0, its first open-weight model, under the Apache 2.0 license. It's a mixture-of-experts model with 501 billion parameters, 23 billion of which are active for each token, and a 1-million-token context. It's priced at $0.25 per million input tokens and $1.00 per million output tokens on OpenRouter.

We ran it through our data analytics exam. It passed **36 of 43 questions** (84%, a B), the same as DeepSeek V4.1 Flash and Claude Sonnet 5, for **$8.29** for the whole exam. It's the second-best open-weight model from a US lab on our leaderboard of 32 models, after Meta's Muse Spark Open.

## **Thorough with messy data**

Beam caught all three of the data quality problems we planted. The only other open-weight model on our board to do that is Qwen 3.8 27B. On the request with the warning "I'll be quoting these numbers in the monthly ops report, so make sure they're right", it noticed the last day of the month was missing for every turbine, found the duplicated rows, removed them before averaging, and flagged two turbines that reported exactly zero output all month.

It gets there by looking around. It made about 15 model calls per question, more than any other open-weight model we've tested, and it answered every impossible request and every ambiguous question correctly.

**Model**

**Lab**

**Score**

**Cost of one full run**

**Model calls per question**

Muse Spark Open

Meta

38 / 43

$5.16

10

Qwen 3.8 27B

Alibaba

38 / 43

$1.71

8

GLM 5.3 Flash

Z.ai

38 / 43

$0.69

7

**Beam 1.0**

**Reflection AI**

**36 / 43**

**$8.29**

**15**

DeepSeek V4.1 Flash

DeepSeek

36 / 43

$0.46

7

Kimi K3

Moonshot AI

35 / 43

$5.01

5

Nemotron 3 Ultra

NVIDIA

34 / 43

$7.06

10

## **Why it cost $8.29**

Beam's per-token price is low, but almost none of its input was cached. Every model call re-sends the whole conversation so far, and hosts usually charge much less for the part they've already seen. Only 0.4% of Beam's input tokens were billed as cached, against 94% for Muse Spark Open and 92% for DeepSeek V4.1 Flash. Beam read about 740,000 tokens per question, and input alone came to $7.79 of the $8.29.

At the cost per question we measured, 1,000 people each asking 10 questions a day would cost about $480 per person per year. Beam only reached OpenRouter on October 16, so that number should come down as more hosts serve it and add caching.

It's also slow on today's providers. A typical question took 68 seconds, and the slowest took more than an hour of model time. Reflection pitches Beam as efficient at inference, and with 23 billion active parameters it should be. The hosted speed may improve as providers tune their deployments.

## **Run it on your own hardware**

Reflection publishes the weights in BF16, FP8 and NVFP4. The 4-bit NVFP4 version is about 280 GB, so it fits on a single 512 GB Mac Studio once that configuration ships, on an NVIDIA DGX Station, or on an 8-GPU server. With 23 billion active parameters, it generates text faster on that hardware than a model of its size that uses all of its parameters for every token.

## **Where it got things right**

- **Data quality (3 of 3).** It caught the missing day and duplicated rows in the ops-report request, and passed the other two data quality questions too.
- **Impossible requests (5 of 5).** It said so when the data couldn't answer the question.
- **Ambiguous questions (5 of 5).** It handled all five questions that can be read more than one way.
- **Statistics traps (4 of 5).** It handled four of the five questions that test for a misleading statistic.

## **Where it got things wrong**

Beam 1.0 missed 7 of the 43 questions:

- **A unit slip, twice.** Asked for fleet-wide monthly energy in GWh, it divided kilowatt-hours by a billion instead of a million, so every number was a thousand times too small. It reported the whole fleet's 2024 output as 55.7 GWh and didn't stop to check whether that was plausible for thousands of turbines. It made the same mistake on a second question.
- **Hidden needles (1 of 5).** We planted five patterns in the data for the model to find on its own, without being told to look. It found one.
- **Windy days and alarms.** Asked whether windy days drive up SCADA alarms, it answered from the overall relationship and offered to break it down further, without checking whether something else, like the season, explains both.

## **About the benchmark**

We run new models through the same exam as they're released, and we'll keep sharing what we find.

The exam is private and we're sharing it with researchers. We're keeping it private so that the answer key doesn't make its way into the training set. For the same reason, the data is fabricated: the warehouse is a synthetic wind-energy fleet, built to look like a real operator's data, so its answers don't exist anywhere else.

If you'd like access, reach out to Chris Parmer at Plotly.
