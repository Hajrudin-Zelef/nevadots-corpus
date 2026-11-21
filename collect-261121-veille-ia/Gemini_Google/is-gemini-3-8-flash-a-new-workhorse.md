---
id: collect-261121-veille-ia/gemini-google/is-gemini-3-8-flash-a-new-workhorse
title: "Is Gemini 3.8 Flash a new workhorse?"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-05-19"]
keywords: ["flash", "gemini", "asia", "test", "time", "first", "workhorse", "different"]
source: https://medium.com/google-developer-experts/is-gemini-3-8-flash-a-new-workhorse-323c9698b34d?source=post_page---post_publication_info--9e2d18f244a0
source_anchor: ""
source_lines: [1, 199]
section: "Gemini_Google"
---
Gemini 3.5 Flash was a workhorse for a lot of projects for quite some time (at least in AI universe terms).

Then we saw the release of 3.6 Flash, 3.7 Flash, and finally 3.8 Flash going one after another (and not in preview — but straight into GA). Leaving aside the reason and politics of this series of rapid releases — it looks like it stopped at 3.8 Flash, and 3.8 Flash will stay with us for some time (again — in AI terms…).

So, does it mean it is finally time to update projects with Gemini 3.5 Flash to 3.8? And is 3.8 just a pure update?

First official dates:

```
+-------------------+-------------------+----------------------------+
| Model Endpoint ID | Release Date (GA) | Retirement Date (EOL)      |
+-------------------+-------------------+----------------------------+
| gemini-3.5-flash  | May 19, 2026      | May 19, 2027 or later      |
| gemini-3.6-flash  | July 21, 2026     | November 19, 2026          |
| gemini-3.7-flash  | August 13, 2026   | January 28, 2027           |
| gemini-3.8-flash  | September 2, 2026 | September 2, 2027 or later |
+-------------------+-------------------+----------------------------+
```
*Note: Release dates and EOL dates are kind of different a lot of times between Vertex AI and AI Studio (which is weird, to be honest). Dates above come from Vertex AI.*

Obviously, 3.6 and 3.7 were some kind of bridge releases, and I hope you haven't spent too much time and tokens switching your system to them. But 3.8 looks more promising and may be considered the next workhorse model…

Obviously, you will need to evaluate a new model on your dataset and for your use case — cause it is all very specific to the project.

But one thing that we can compare (and it is true for all) — latency. So, let’s do that!

But before we compare latency, there are several things that we need to discuss first.

## **Regions**

Gemini 3.5 Flash and Gemini 3.8 Flash are actually available in different sets of regions.

To be precise, both models are supported in Global and Multi-Region endpoints:

**global**
**us**
**eu**

But Gemini 3.5 Flash is also supported in specific Regional endpoints:

**North America:**

- `northamerica-northeast1` (Montréal, Canada)

**Europe:**

- `europe-west2` (London, United Kingdom)
- `europe-west3` (Frankfurt, Germany)

**Asia Pacific:**

- `asia-northeast1` (Tokyo, Japan)
- `asia-south1` (Mumbai, India)
- `asia-southeast1` (Singapore)
- `australia-southeast1` (Sydney, Australia)

One thing jumps at us — Asia. While specific Europe or NA regions can potentially be replaced with multi-regional endpoints, there is no multi-regional endpoint for Asia.

That is one thing we need to consider in our tests — how consumers in Asia would be impacted.

## **Effort levels**

Selection of effort levels has also changed — specifically, 3.8 Flash lost the minimal effort level (to be precise, it started from the 3.7 model).

So, the second special case we need to consider is: what if some system was running on minimal and has to switch to Low now?

Now let’s start our test!

## **Test 1: What happens with Asia?**

In the first test, let’s look at what impact a change from 3.5 to 3.8 would have on someone running an app in the Asia region. We assume that the app was using the local Asia region (asia-northeast1) and now has to switch to one of the multi-regional endpoints.

*First, several notes on the test itself:*

- *To make it more representative (and not one call), the test is made by making parallel calls to different Gemini models every 5 minutes during 24 hours.*
- *To save my money on tokens — both models are running at a low effort level in this test.*
- *Graphs for different prompt sizes use different scales. At first glance, it may look like all prompts take approximately the same time — but it is not the case; check the scale.*
- *The test used Standard Tier and not Priority Tier or Provisional Throughput (that would be in next story).*

Time to first token (from Asia by region)

*Note: triangle shows slowest single call (max); an arrowhead at the right edge means it is beyond the axis.*

What we can see from that:

- If your app is in the Asia region, calling Gemini 3.5 Flash in the dedicated Asia endpoint is always faster than using Global or Multi-Regional endpoints. Global and Multi-regional endpoints have comparable latency.
- When it comes to 3.8 Flash — all endpoints have comparable latency. So, selecting a particular endpoint does not make much of an impact.
- For short prompts, 3.5 Flash provides the first token faster. The difference is even more significant if a dedicated Asia endpoint is used.
- For medium and large prompts, Gemini 3.8 Flash returns the first token significantly faster than Gemini 3.5 Flash. The difference is so significant that it beats the benefits of a dedicated Asia endpoint.
- Both models experience occasional terrible delays. For example, while p95 for 3.8 Flash to us is 14 seconds (example with largest spread), max is 52 seconds. A similar problem can be seen with Gemini 3.5 Flash, but it looks like it got even worse with Gemini 3.8.

However, the most concerning thing that we see is: Gemini 3.8 Flash has an **enormous spread**. You can get the first token in 2 seconds or 14 seconds for the same prompt, and sometimes — 60 seconds. And the spread increases with prompt size.

Now let’s look at the total response time:

Total response time (from Asia by region)

We see the same situation here:

- The dedicated Asia endpoint for Gemini 3.5 beats all other endpoints of 3.5.
- For short prompts, Gemini 3.5 Flash beats Gemini 3.8 Flash, especially when using the dedicated Asia endpoint.
- For medium and long prompts, Gemini 3.8 Flash beats Gemini 3.5 Flash.
- Gemini 3.8 Flash has a huge spread that grows with prompt size.

**Summary of Test 1**

What does it really mean for your app?

If you switch from Gemini 3.5 to Gemini 3.8 for your app running in the Asia region:

- Short terms would become slower. However, they are kind of fast anyway — so it should not be a big problem.
- For medium and long prompts, both time to first token and time to full answer should improve (or at least stay the same). Time to first token has the biggest improvement.
- **The biggest concern is that your app would “freeze” from time to time. And you can’t do anything about it (so at least log it).**

## Test 2: Similar test, but from an app running in the NA region.

Now let’s see if an app running in the NA region will have the same concerns.

Time to first token (from NA by region)

Total response time (from NA by region)

The situation is the same as we have seen in Test 1, with the only exception that Gemini 3.5 Flash does not get the benefit of a dedicated Asia endpoint anymore.

**Summary:**

Gemini 3.8 Flash is slower on small prompts (both to first token and final result), but on average faster for medium and large prompts. However, the bigger spread and being more prone to “freezes” is a significant concern.

## Test 3: Now let’s see if the same repeats on Medium effort level

On the Medium level, the situation repeats.

Time to first token (Medium effort level)

Total time (Medium effort level)

We see that on the Medium level, 3.8 Flash is now slower than 3.5 Flash on all prompt sizes tested. However, as prompt size grows, the difference becomes smaller.

Unfortunately, now Gemini 3.8 Flash has even lost the advantage of the first token that we saw before.

And, as before, while medium is comparable, spread is the biggest concern. While both models have a risk of getting “stuck” on some requests (independently of request size and content), 3.8 Flash, when stuck, has a risk of showing delays measured in minutes.

## Test 4: Comparing 3.5 Flash Minimum to 3.8 Flash Low.

As mentioned before, there was a change in architecture between the 3.5 Flash and 3.8 Flash models. One of the consequences of this change is that Minimum thinking effort was removed from the options.

Minimum thinking effort was added to the 3.5 Flash model as a replacement for the ability to define a thinking budget as 0 (setting the thinking budget to a numeric value was a way to control effort level in previous Flash models, which was later replaced with fixed levels like Minimum, Low, etc.). On Minimum thinking effort, 3.5 Flash was not producing any thinking tokens (basically was going straight to the output). However, in 3.8 Flash, Minimum was removed, and the lowest level is now Low. On the Low effort level, 3.8 Flash is now not producing any thinking tokens (at least most of the time).

So, what happens if you used to rely on 3.5 Flash with Minimum effort and now have to switch to 3.8 Flash with Low? (I think you can already guess…)

Time to first token (3.5 Flash Minimum VS 3.8 Flash Low)

Total time (3.5 Flash Minimum VS 3.8 Flash Low)

As we could have guessed, 3.5 Flash on Minimum works significantly faster. And, unfortunately, the biggest issue is the spread for 3.8 Flash results.

It means that if you relied on 3.5 Flash with Minimum effort level — your only option is to explore the Flash-light model further.

## Test 5: Comparing 3.5 Flash Medium to 3.8 Flash Low.

One can make an argument that 3.8 Flash is a smarter model, and as a result, it can solve the same problems as 3.5 Flash, but with lower effort.

You can find many articles that compare those models’ intelligence, like https://opencode.ai/data/compare/google/gemini-3-5-flash/google/gemini-3-8-flash or https://artificialanalysis.ai/models/comparisons/gemini-3-8-flash-medium-vs-gemini-3-5-flash-medium.

So, what if your application was running on 3.5 Flash Medium and you now switch to 3.8 Flash Low?

Time to first token (3.5 Flash Medium to 3.8 Flash Low)

Total response time (3.5 Flash Medium to 3.8 Flash Low)

**Results:**

- For short prompts, 3.5 Flash still wins on average (both TFT and Total Response time).
- For longer prompts, on average, 3.8 Flash shows much faster results (both TFT and Total Response time). Which sounds like great news, but…
- The spread of 3.8 Flash is huge.

## Summary:

Gemini 3.8 Flash is supposed to replace Gemini 3.5 Flash. Likely, unlike 3.6 and 3.7, the end-of-life date of Gemini 3.5 Flash has not yet been announced — which gives plenty of time to migrate.

But should you migrate now? Well, it really depends…

If your main reason for using the Gemini Flash model was speed — you really need to think hard (and test):

- Gemini 3.8 Flash does not have a dedicated Asia endpoint (at least not yet), and using the global endpoint will add some delay.
- Gemini 3.8 Flash does not have the Minimum effort level, so you will have to switch to Low. And that would make your operations slower.
- On short prompts, Gemini 3.5 Flash is faster than Gemini 3.8 Flash.

Unfortunately, there is no Gemini 3.8 Flash Light yet to test. So, if speed is the main concern and you have one of the cases above, **don’t migrate yet**.

If you are running Gemini 3.5 Flash on Low (or higher) and your prompts are reasonably long, Gemini 3.8 Flash would be faster on average than Gemini 3.5 Flash, both to the first token and to the final response.

Considering that it is also more intelligent and cheaper — that looks like a strong argument to **migrate**.

However, there is one caveat — Gemini 3.8 Flash has a much bigger spread when it comes to response times, and from time to time it is “stuck” for up to several minutes. And that is definitely not good for real-time applications — so if you can’t afford periodic “stucks”, **don’t migrate yet**.

However, all of this is related to the Standard priority of requests. In the next part, we will look at whether Priority and Provisional Throughput can help.

If you want to see further tests, see Part 2.
