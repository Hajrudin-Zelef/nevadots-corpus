---
id: collect-261121-veille-ia/gemini-google/gemini-models-explained-3-8-flash-vs-flash-lite-vs-3-1-pro-with-every-release-date
title: "Gemini models explained: 3.8 Flash vs Flash-Lite vs 3.1 Pro, with every release date"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-02"]
keywords: ["gemini", "flash", "flash-lite", "google", "model", "models", "date", "preview"]
source: https://www.thinkfacility.com/blog/gemini-models-explained-flash-flash-lite-pro/
source_anchor: ""
source_lines: [1, 115]
section: "Gemini_Google"
---
# Gemini models explained: 3.8 Flash vs Flash-Lite vs 3.1 Pro, with every release date

Google has shipped six Flash and Flash-Lite models since May, but the newest Pro you can call is still a February preview.

The newest Gemini model is Gemini 3.8 Flash, released on September 2, 2026. The newest Pro model you can call is Gemini 3.1 Pro, and it's still a preview that dates from February 19, 2026.

Since May, Google has shipped 3.5 Flash, 3.6 Flash, 3.7 Flash and 3.8 Flash, plus two Flash-Lite models. Pro hasn't moved. Everything below comes from the Gemini API release notes, its pricing page and Google's own launch posts, all read on September 23, 2026.

## Every Gemini 3 text model, by date

| Date | Model | Status now | In / out per 1M | 
|---|---|---|---|
| Nov 18, 2025 | Gemini 3 Pro Preview | Shut down Mar 9, 2026 | Gone | 
| Dec 17, 2025 | Gemini 3 Flash Preview | Preview | $0.50 / $3 | 
| Feb 19, 2026 | Gemini 3.1 Pro Preview | Preview | $2 / $12 | 
| May 7, 2026 | Gemini 3.1 Flash-Lite | Stable, ends May 7, 2027 | $0.25 / $1.50 | 
| May 19, 2026 | Gemini 3.5 Flash | Stable | $1.50 / $9 | 
| Jul 21, 2026 | Gemini 3.5 Flash-Lite | Stable | $0.30 / $2.50 | 
| Jul 21, 2026 | Gemini 3.6 Flash | Stable | $0.75 / $3.75 | 
| Aug 13, 2026 | Gemini 3.7 Flash | Stable | $0.75 / $3.75 | 
| Sep 2, 2026 | **Gemini 3.8 Flash** | Stable | $0.75 / $3.75 | 
| Not out | Gemini 3.5 Pro | Testing with partners | None yet | 

Dates from the Gemini API release notes, prices from its paid tier, both read September 23, 2026. 3.1 Pro prices are for prompts up to 200K tokens. The $0.75 / $3.75 rates are introductory and end on December 31, 2026.

Every model in that table reads 1,048,576 tokens of context and writes up to 65,536, according to each one's model page. So context isn't a reason to pick one over another. Price is, and so is how much reasoning you want (the thinking levels further down).

## What Flash, Flash-Lite and Pro mean

Google sorts Gemini into tiers by size and cost. The version number tells you when a model shipped, and the tier word tells you what it's for, so a 3.8 Flash and a 3.1 Pro aren't a newer and older copy of the same thing.

**Flash** is the middle tier. It's where nearly all the recent work has gone. Google calls 3.8 Flash "our most intelligent Flash model" and aims it at long coding jobs and agents.

**Flash-Lite** is the cheap, fast tier. Google's launch post for 3.5 Flash-Lite quotes Artificial Analysis at 350 output tokens a second and pitches it for high-volume jobs like document processing and running as a subagent under a bigger model.

**Pro** is the top tier and the priciest. Gemini 3.1 Pro is the only one on the price list with no free tier, and its rates go up to $4 in and $18 out once a prompt passes 200K tokens. It also comes as a second endpoint, `gemini-3.1-pro-preview-customtools`, which Google says is better at picking your own tools when you've given it a mix of those and a bash shell.

## Where Gemini 3.5 Pro went

When Google announced Gemini 3.5 on May 19, 2026, it launched Flash first and wrote of the Pro model: "It's already being used internally, and we look forward to rolling it out next month." June came and went without it.

By the 3.6 Flash launch on July 21, the wording had changed to "Gemini 3.5 Pro is currently testing with partners and we plan to make it broadly available as soon as it's ready." The same post says Google has started its "most ambitious pre-training run yet, for Gemini 4". (Pre-training is the first stage of building a model, before any of the tuning that turns it into something you can use.)

Two months on, the Gemini Pro page on DeepMind's site is still headed Gemini 3.1 Pro, with a "3.5 Pro coming soon" banner above it. I couldn't find a newer date from anyone at Google, and I've left out the dates floating around in press coverage.

In the meantime Google has been saying its Flash models are good enough for jobs you'd once have sent to Pro. At the 3.5 launch it wrote that 3.5 Flash outperformed Gemini 3.1 Pro on several coding and agent benchmarks, and each Flash since has been billed as better than the last.

## Gemini 4 Argon is announced, but you can't call it yet

On September 30, 2026, Google announced Gemini 4 Argon, which its launch post calls Google's new frontier model. It went to a set of trusted cyber defenders first (Google calls the scheme its Fairwind Program), and Google says wider access will start "with paid API customers and Google AI Ultra subscribers". There's no date for that.

The API price is $2 in and $10 out per million tokens, and it's introductory: a footnote says it moves to $4 and $20 afterwards. Argon can also write up to 1M tokens in one answer, where every model in the table above stops at 65,536. It isn't in the API's release notes yet (checked October 1, 2026), so there's nothing to call. Our Gemini 4 Argon page will change when that does.

## Why the newest Flash costs less than 3.5 Flash

This is the part of the price list that looks backwards. Gemini 3.5 Flash, from May, costs $1.50 in and $9 out per million tokens. Gemini 3.8 Flash, from September, costs $0.75 and $3.75.

The reason is an introductory price. When 3.6 Flash launched on July 21, Google priced it at $1.50 in and $7.50 out. Three weeks later, 3.7 Flash arrived at half that, "through the end of the year", and the pricing page now shows 3.6, 3.7 and 3.8 Flash all at $0.75 and $3.75 until December 31, 2026. From January 1, 2027 all three go to $1.50 and $7.50.

So if you're on 3.5 Flash today, moving to 3.8 Flash halves the input rate and cuts the output rate by more than half. After January the input rates match, and 3.8 Flash's output is still $1.50 cheaper. The catch is volume. Google's 3.8 Flash post says the model "works harder", running extra reasoning steps and tool calls, and "might use more tokens to maximize performance, especially at higher effort levels." A cheaper token doesn't help much if the model spends twice as many.

## Gemini 3.8 Flash vs 3.7 Flash

Google shipped them three weeks apart and says 3.8 is its best reasoning and coding model yet at the same price and speed. It also says 3.7 Flash "remains fully supported for efficiency-first workloads", for anyone whose costs come from token count rather than token price.

Both take a thinking level of low, medium or high. Neither accepts "minimal", and the model pages say sending it returns an error, which will catch anyone moving code over from Flash-Lite (3.5 Flash-Lite does go down to minimal, which Google pitches for the fastest, cheapest jobs). The 3.8 Flash model card gives a knowledge cutoff of March 2026, with a caveat that some areas stop at January 2025.

Older code can trip on something else too. On July 21, 2026 Google deprecated the `temperature`, `top_p` and `top_k` settings, the knobs that control how random a model's wording is, so it's worth checking whether your requests still set them.

There's a second 3.8 model most people can't use. Gemini 3.8 Flash Cyber is tuned for finding and patching security bugs, and Google only offers it to vetted defenders through what it calls the Fairwind Program.

## Where you'll meet these outside the API

In Google's own apps you'll rarely see a model name. Gemini 3.5 Flash became the default model in the Gemini app and in AI Mode in Search when it launched in May. Google says 3.8 Flash is available to Google AI Pro and Ultra subscribers in the Gemini app, in AI Mode and in Gemini in Google Sheets.

Developers get one more change to watch. On September 18, 2026 Google limited the Gemini 2.5 models to accounts that have used them before, and told everyone else to start on 3.5 Flash-Lite or 3.8 Flash. The older 3.1 Flash-Lite has a shutdown date of May 7, 2027, with 3.5 Flash-Lite named as its replacement. If you've hit a blocked request along the way, we've written up the PROHIBITED_CONTENT error separately.

Gemini 3.1 Pro Preview has no shutdown date on Google's deprecations page yet (we checked on September 23, 2026).

## Quick answers

### What is the newest Gemini model?

The newest one you can call is Gemini 3.8 Flash, released on September 2, 2026 as a stable model in the Gemini API (`gemini-3.8-flash`). Google announced Gemini 4 Argon on September 30, but only trusted cyber defenders can use it so far.

### Is Gemini 4 out?

Only for a small group. Gemini 4 Argon, announced on September 30, 2026, is rolling out to trusted cyber defenders through Google's Fairwind Program. Google says paid API customers and Google AI Ultra subscribers come next, and it hasn't given a date. As of October 1 it isn't in the Gemini API's release notes.

### Is Gemini 3.5 Pro out?

No. As of September 23, 2026 it isn't in the Gemini API's model list. Google said on May 19, 2026 it hoped to roll it out the following month, and on July 21 wrote that it was "currently testing with partners".

### What's the difference between Gemini Flash and Flash-Lite?

Flash-Lite is the cheaper, faster tier for high-volume work. Gemini 3.5 Flash-Lite costs $0.30 in and $2.50 out per million tokens, against $0.75 and $3.75 for 3.8 Flash at its introductory price. Both read about a million tokens.

### Is Gemini 3.8 Flash better than Gemini 3.1 Pro?

Google said in May that Gemini 3.5 Flash already beat 3.1 Pro on several coding and agent benchmarks, and it has billed each Flash since as better than the one before. 3.8 Flash is also much cheaper: $0.75 in and $3.75 out through 2026, against $2 and $12 for 3.1 Pro.

### When does the Gemini 3.8 Flash price go up?

On January 1, 2027, from $0.75 to $1.50 per million input tokens and from $3.75 to $7.50 per million output tokens. The same change applies to 3.7 Flash and 3.6 Flash.

### Can I still use Gemini 2.5 Pro?

Only if you've used it before. Since September 18, 2026 Google has limited the 2.5 models to past users. They aren't deprecated, and Google points new projects to 3.5 Flash-Lite or 3.8 Flash.

## More on Google

- DeepMind researchers call AI's climb past human level to superintelligence within a decade a serious possibilityNovember 20, 2026
- Google tests Chrome's AI agent on a synthetic web, hidden so models can't tellNovember 19, 2026
- Google finds attackers using an AI tool that turns security patches into exploits, no human involvedNovember 16, 2026
- Demis Hassabis says an AI 'oil spill' will happen unless labs pay far more attentionNovember 16, 2026
