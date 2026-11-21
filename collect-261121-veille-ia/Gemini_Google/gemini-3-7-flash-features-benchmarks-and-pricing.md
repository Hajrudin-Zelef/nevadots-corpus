---
id: collect-261121-veille-ia/gemini-google/gemini-3-7-flash-features-benchmarks-and-pricing
title: "Gemini 3.7 Flash: Features, Benchmarks, and Pricing"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["flash", "gemini", "google", "model", "most", "pricing", "capable", "coding"]
source: https://www.datacamp.com/blog/gemini-3-7-flash
source_anchor: ""
source_lines: [1, 291]
section: "Gemini_Google"
---
***Note**: Only a few weeks after the launch of Gemini 3.7 Flash, Google launched Gemini 3.8 Flash. See it in action in our Gemini 3.8 Flash API tutorial.*

Google has released Gemini 3.7 Flash, its most capable Flash-tier model yet, aimed squarely at coding and agentic workflows. The launch lands while Google's flagship Gemini 3.5 Pro is still delayed, which Reuters flagged as a test of whether DeepMind can keep pace with Anthropic and OpenAI.

The headline numbers are aggressive. Gemini 3.7 Flash scores 43.6% on FrontierCode 1.1 Main (up from 34.4% for 3.6 Flash), hits 1588 Elo on Code Arena for web development, and ships at an introductory price of $0.75 per 1M input tokens and $3.75 per 1M output tokens, half the original 3.6 Flash cost.

In this article, I'll cover everything new with Gemini 3.7 Flash, looking at its features, exploring the benchmarks, and setting up hands-on tests you can run yourself. To go deeper on the rival models it competes with, see our guides to GPT-5.6 Terra vs Claude Sonnet 5 and how to learn Claude. 

## In a Nutshell

- Gemini 3.7 Flash is Google's most capable Flash model, aimed at coding and agentic workflows, with a 1M token context window.
- Introductory pricing is $0.75/1M input and $3.75/1M output tokens through December 31, 2026, then $1.50/$7.50 standard.
- It tops FrontierCode 1.1 (43.6%), GDP.pdf (34.0%), Harvey LAB-AA (90.7%), and GDM-MRCR long context (97.0%) against Claude Sonnet 5 and GPT-5.6 Terra.
- GPT-5.6 Terra still leads on DeepSWE, Terminal-bench, and OSWorld agentic evals.
- Available via the Gemini API, Google Antigravity, Gemini Enterprise, and Gemini Spark.

## What Is Gemini 3.7 Flash?

Gemini 3.7 Flash is Google's mid-tier workhorse model, positioned as its most capable Flash model for complex coding, agentic workflows, and reliable multi-step execution. It follows Gemini 3.6 Flash, which arrived three weeks earlier, and Google credits developer feedback and algorithmic changes for the jump in quality.

The model is generally available through the Gemini API with a 1M token context window (1,048,576 input tokens) and a 65,536 output-token limit. It accepts text, image, video, audio, and PDF inputs, and returns text only. Google exposes tunable thinking levels of low, medium, and high, though the API returns an error if you request the unsupported `minimal` setting.

The most telling benchmark headline is DeepSWE v1.1, a long-horizon software engineering eval, where 3.7 Flash scores 65.3% against 34.4% for its predecessor on FrontierCode and just 48.6% for 3.6 Flash on DeepSWE itself. That is a large single-generation gain for a model that also dropped in price, which is unusual.

## What's New With Gemini 3.7 Flash?

The story here is a more capable Flash model at a lower price, with better instruction following and less babysitting in agent loops. Here are the changes that matter most for practitioners.

### Stronger coding and issue resolution

You can hand Gemini 3.7 Flash harder debugging and refactoring work and expect higher first-pass accuracy. Google reports 43.6% on FrontierCode 1.1 Main for production code quality, up from 34.4% for 3.6 Flash, and 65.3% on DeepSWE v1.1 for long-horizon software engineering.

For working engineers, the practical read is fewer retries. Google says the model adapts to roadblocks better and follows instructions with more fidelity, which reduces the manual oversight that makes cheaper models expensive in wall-clock time.

### Faster web app and UI generation

Gemini 3.7 Flash generates functional layouts and feature-complete web apps in fewer prompts than 3.6 Flash. On Code Arena's web development leaderboard it scores 1588 Elo against 1538 for 3.6 Flash and 1541 for Claude Sonnet 5.

The model can also match a reference input when generating UI, whether that reference is a screenshot, an image, or a full design system. If you feed it a mockup of a pricing page, it aims to reproduce the design adherence rather than improvise a generic layout.

### Better multi-step agent execution

The model puts more effort into planning and tool calls, which is where cheap models usually fall apart. On AutomationBench, a private eval for real-world business workflows, 3.7 Flash scores 30.4% against 17.0% for 3.6 Flash and 10.7% for Claude Sonnet 5.

Built-in tools available through the API include caching, code execution, file search, function calling, search grounding, grounding with Google Maps, structured outputs, and URL context. Computer use is present but marked as preview, so I would not build production agents around it yet.

### Improved document and knowledge work

Gemini 3.7 Flash handles knowledge-dense fields like finance, law, and biosciences with better reasoning and document comprehension. On the GDP.pdf benchmark for expert PDF comprehension it scores 34.0% against 22.0% for 3.6 Flash, and on Harvey LAB-AA for complex legal workflows it hits 90.7%.

If your work involves pulling structured answers out of dense annual reports or contracts, this is the release worth testing. The model still outputs text only, so it will not generate the charts it reads, only describe or summarize them.

### Lower price for scaling agents

The introductory pricing cuts the cost of running Flash-based agents in half compared to launch 3.6 Flash rates. Through December 31, 2026, you pay $0.75 per 1M input tokens and $3.75 per 1M output tokens.

- Input: $0.75 per 1M tokens (introductory), rising to $1.50 per 1M on January 1, 2027
- Output: $3.75 per 1M tokens (introductory), rising to $7.50 per 1M on January 1, 2027

For comparison, Claude Sonnet 5 sits at $2.00 input and $10.00 output on Google's own benchmark table, and GPT-5.6 Terra at $2.00 input and $12.00 output. On raw token price, 3.7 Flash undercuts both by a wide margin.

## Gemini 3.7 Flash Benchmarks

Google's benchmark table pits Gemini 3.7 Flash against 3.6 Flash, Claude Sonnet 5, GPT-5.6 Terra, and Muse Spark 1.2. The pattern is consistent: 3.7 Flash beats its predecessor across almost every eval and trades blows with pricier rivals, though GPT-5.6 Terra edges it on several agentic coding evals.

### Gemini 3.7 Flash vs the field at a glance

Here is how the key numbers line up across the four models on Google's benchmark table.

| Metric | Gemini 3.7 Flash | Claude Sonnet 5 | GPT-5.6 Terra | 
|---|---|---|---|
| Input price / 1M | $0.75 | $2.00 | $2.00 | 
| Output price / 1M | $3.75 | $10.00 | $12.00 | 
| FrontierCode 1.1 Main | 43.6% | 42.7% | 41.3% | 
| DeepSWE v1.1 | 65.3% | 53.8% | 69.6% | 
| Terminal-bench 2.1 | 85.8% | 80.4% | 87.4% | 
| GDP.pdf | 34.0% | 28.0% | 24.7% | 
| GDM-MRCR v2 (128k) | 97.0% | 81.5% | 93.5% | 

### FrontierCode 1.1 Main

FrontierCode 1.1 Main measures production code quality, so it rewards code that is correct and ready to ship rather than just syntactically valid.

Gemini 3.7 Flash scores 43.6%, ahead of Claude Sonnet 5 at 42.7%, GPT-5.6 Terra at 41.3%, and its own predecessor 3.6 Flash at 34.4%.

For a Flash-tier model to top both frontier rivals on production code quality is the standout result of this release, especially at a third of their token cost.

### DeepSWE v1.1

DeepSWE v1.1 tests long-horizon software engineering, meaning multi-file tasks that require sustained context over many steps.

Here, 3.7 Flash scores 65.3%, a large jump from 48.6% for 3.6 Flash, but GPT-5.6 Terra leads at 69.6%, and Claude Sonnet 5 trails at 53.8%.

The takeaway: 3.7 Flash closed most of the gap to Terra on long-horizon coding but did not overtake it. If you need the absolute ceiling on multi-file agentic coding, Terra still wins that specific eval.

### Terminal-bench 2.1 and 3.0

Terminal-bench measures agentic terminal coding, where the model drives a command line to complete tasks.

On Terminal-bench 2.1, 3.7 Flash scores 85.8% against 78.0% for 3.6 Flash and 80.4% for Claude Sonnet 5, though GPT-5.6 Terra leads at 87.4%.

The newer Terminal-bench 3.0 for general agent capabilities is harder across the board: 3.7 Flash scores 14.9%, well above 3.6 Flash at 5.4%, roughly tied with Claude Sonnet 5 at 14.6%, and behind Terra at 20.8%.

These low absolute numbers are a useful reality check on how far general agent reliability still has to go.

### GDP.pdf and Harvey LAB-AA

GDP.pdf tests expert PDF document comprehension, and Harvey LAB-AA tests complex legal workflows.

Gemini 3.7 Flash leads both: 34.0% on GDP.pdf against 28.0% for Claude Sonnet 5 and 24.7% for GPT-5.6 Terra, and 90.7% on Harvey LAB-AA against 90.1% for Sonnet 5 and 85.2% for Terra.

If your workload is document-heavy legal or financial analysis, this is where the cheapest model on the table also happens to be the most accurate, which is a rare combination.

### GDM-MRCR v2 long context

GDM-MRCR v2 (8-needle) measures long-context retrieval, testing whether the model can find multiple buried pieces of information across a long input.

At the 128k average, 3.7 Flash scores 97.0%, ahead of 3.6 Flash at 91.8%, Claude Sonnet 5 at 81.5%, and GPT-5.6 Terra at 93.5%.

With a 1M token context window and the strongest 8-needle retrieval on the table, 3.7 Flash is a strong pick for pipelines that stuff large documents or long transcripts into a single call.

When we compared Terra and Sonnet 5, long-context retrieval was one of the sharper differentiators between them, and 3.7 Flash now sits above both at this band.

### Where GPT-5.6 Terra still leads

It is worth being honest about the losses.

GPT-5.6 Terra beats Gemini 3.7 Flash on DeepSWE v1.1 (69.6% vs 65.3%), Terminal-bench 2.1 (87.4% vs 85.8%), Terminal-bench 3.0 (20.8% vs 14.9%), and OSWorld-2.0 agentic computer use (50.2% vs 47.9%).

The pattern is that Terra holds a small edge on the hardest agentic and terminal coding evals, while 3.7 Flash wins on production code quality, document comprehension, and long-context retrieval. Which one wins for you depends on whether your bottleneck is agent reliability or cost.

## Gemini 3.7 Flash Pricing and Availability

Gemini 3.7 Flash is generally available now through several Google surfaces.

The API model ID is `gemini-3.7-flash`, and you can start building in Google AI Studio, Android Studio, or the agent-first Google Antigravity environment.

- **Developers:** Gemini API via Google AI Studio and Android Studio; agent workflows in Google Antigravity
- **Enterprises:** Gemini Enterprise Agent Platform and the Gemini Enterprise app
- **Individuals:** Gemini Spark, the 24/7 personal agent in the Gemini app, for Google AI Pro and Ultra subscribers in more than 160 countries

Pricing is $0.75 per 1M input tokens and $3.75 per 1M output tokens through December 31, 2026. On January 1, 2027, standard rates of $1.50 input and $7.50 output apply. Note that audio generation, image generation, and the Live API are not supported for this model, and computer use is preview only.

## Final Thoughts

Gemini 3.7 Flash is Google shipping fast and cutting prices while its flagship Gemini 3.5 Pro stays delayed.

Releasing an improved Flash model three weeks after 3.6 Flash, at half the token cost, reads as a company trying to win on the mid-tier, where most production volume actually lives.

Honestly, I think this is the model to reach for if your workload is document comprehension, production web and app code, or long-context retrieval, where it tops Claude Sonnet 5 and GPT-5.6 Terra outright while costing a fraction as much.

If your bottleneck is the hardest agentic terminal coding, Terra still holds a narrow lead on DeepSWE and Terminal-bench, and you would be paying more for that ceiling.

With the DeepMind leadership overhaul and the Pro model delay, Google is leaning on Flash to prove it can still compete. On the benchmark table it just published, that argument mostly holds for a mid-tier model.

If you want to get hands-on with models like this, I recommend starting with our AI Fundamentals skill track to build a foundation before wiring Gemini 3.7 Flash into your own agent pipelines.

Note: Only a few weeks after the launch of Gemini 3.7 Flash, Google launched Gemini 3.8 Flash. See it in action in our Gemini 3.8 Flash API tutorial.
Google has released Gemini 3.7 Flash, its most capable Flash-tier model yet, aimed squarely at coding and agentic workflows. The launch lands while Google's flagship Gemini 3.5 Pro is still delayed, which Reuters flagged as a test of whether DeepMind can keep pace with Anthropic and OpenAI.

The headline numbers are aggressive. Gemini 3.7 Flash scores 43.6% on FrontierCode 1.1 Main (up from 34.4% for 3.6 Flash), hits 1588 Elo on Code Arena for web development, and ships at an introductory price of $0.75 per 1M input tokens and $3.75 per 1M output tokens, half the original 3.6 Flash cost.

In this article, I'll cover everything new with Gemini 3.7 Flash, looking at its features, exploring the benchmarks, and setting up hands-on tests you can run yourself. To go deeper on the rival models it competes with, see our guides to GPT-5.6 Terra vs Claude Sonnet 5 and how to learn Claude.

## In a Nutshell

## What Is Gemini 3.7 Flash?

Gemini 3.7 Flash is Google's mid-tier workhorse model, positioned as its most capable Flash model for complex coding, agentic workflows, and reliable multi-step execution. It follows Gemini 3.6 Flash, which arrived three weeks earlier, and Google credits developer feedback and algorithmic changes for the jump in quality.

The model is generally available through the Gemini API with a 1M token context window (1,048,576 input tokens) and a 65,536 output-token limit. It accepts text, image, video, audio, and PDF inputs, and returns text only. Google exposes tunable thinking levels of low, medium, and high, though the API returns an error if you request the unsupported

`minimal`setting.
The most telling benchmark headline is DeepSWE v1.1, a long-horizon software engineering eval, where 3.7 Flash scores 65.3% against 34.4% for its predecessor on FrontierCode and just 48.6% for 3.6 Flash on DeepSWE itself. That is a large single-generation gain for a model that also dropped in price, which is unusual.

## What's New With Gemini 3.7 Flash?

The story here is a more capable Flash model at a lower price, with better instruction following and less babysitting in agent loops. Here are the changes that matter most for practitioners.

## Stronger coding and issue resolution

You can hand Gemini 3.7 Flash harder debugging and refactoring work and expect higher first-pass accuracy. Google reports 43.6% on FrontierCode 1.1 Main for production code quality, up from 34.4% for 3.6 Flash, and 65.3% on DeepSWE v1.1 for long-horizon software engineering.

For working engineers, the practical read is fewer retries. Google says the model adapts to roadblocks better and follows instructions with more fidelity, which reduces the manual oversight that makes cheaper models expensive in wall-clock time.

## Faster web app and UI generation

Gemini 3.7 Flash generates functional layouts and feature-complete web apps in fewer prompts than 3.6 Flash. On Code Arena's web development leaderboard it scores 1588 Elo against 1538 for 3.6 Flash and 1541 for Claude Sonnet 5.

The model can also match a reference input when generating UI, whether that reference is a screenshot, an image, or a full design system. If you feed it a mockup of a pricing page, it aims to reproduce the design adherence rather than improvise a generic layout.

## Better multi-step agent execution

The model puts more effort into planning and tool calls, which is where cheap models usually fall apart. On AutomationBench, a private eval for real-world business workflows, 3.7 Flash scores 30.4% against 17.0% for 3.6 Flash and 10.7% for Claude Sonnet 5.

Built-in tools available through the API include caching, code execution, file search, function calling, search grounding, grounding with Google Maps, structured outputs, and URL context. Computer use is present but marked as preview, so I would not build production agents around it yet.

## Improved document and knowledge work

Gemini 3.7 Flash handles knowledge-dense fields like finance, law, and biosciences with better reasoning and document comprehension. On the GDP.pdf benchmark for expert PDF comprehension it scores 34.0% against 22.0% for 3.6 Flash, and on Harvey LAB-AA for complex legal workflows it hits 90.7%.

If your work involves pulling structured answers out of dense annual reports or contracts, this is the release worth testing. The model still outputs text only, so it will not generate the charts it reads, only describe or summarize them.

## Lower price for scaling agents

The introductory pricing cuts the cost of running Flash-based agents in half compared to launch 3.6 Flash rates. Through December 31, 2026, you pay $0.75 per 1M input tokens and $3.75 per 1M output tokens.

For comparison, Claude Sonnet 5 sits at $2.00 input and $10.00 output on Google's own benchmark table, and GPT-5.6 Terra at $2.00 input and $12.00 output. On raw token price, 3.7 Flash undercuts both by a wide margin.

## Gemini 3.7 Flash Benchmarks

Google's benchmark table pits Gemini 3.7 Flash against 3.6 Flash, Claude Sonnet 5, GPT-5.6 Terra, and Muse Spark 1.2. The pattern is consistent: 3.7 Flash beats its predecessor across almost every eval and trades blows with pricier rivals, though GPT-5.6 Terra edges it on several agentic coding evals.

## Gemini 3.7 Flash vs the field at a glance

Here is how the key numbers line up across the four models on Google's benchmark table.

## FrontierCode 1.1 Main

FrontierCode 1.1 Main measures production code quality, so it rewards code that is correct and ready to ship rather than just syntactically valid.

Gemini 3.7 Flash scores 43.6%, ahead of Claude Sonnet 5 at 42.7%, GPT-5.6 Terra at 41.3%, and its own predecessor 3.6 Flash at 34.4%.

For a Flash-tier model to top both frontier rivals on production code quality is the standout result of this release, especially at a third of their token cost.

## DeepSWE v1.1

DeepSWE v1.1 tests long-horizon software engineering, meaning multi-file tasks that require sustained context over many steps.

Here, 3.7 Flash scores 65.3%, a large jump from 48.6% for 3.6 Flash, but GPT-5.6 Terra leads at 69.6%, and Claude Sonnet 5 trails at 53.8%.

The takeaway: 3.7 Flash closed most of the gap to Terra on long-horizon coding but did not overtake it. If you need the absolute ceiling on multi-file agentic coding, Terra still wins that specific eval.

## Terminal-bench 2.1 and 3.0

Terminal-bench measures agentic terminal coding, where the model drives a command line to complete tasks.

On Terminal-bench 2.1, 3.7 Flash scores 85.8% against 78.0% for 3.6 Flash and 80.4% for Claude Sonnet 5, though GPT-5.6 Terra leads at 87.4%.

The newer Terminal-bench 3.0 for general agent capabilities is harder across the board: 3.7 Flash scores 14.9%, well above 3.6 Flash at 5.4%, roughly tied with Claude Sonnet 5 at 14.6%, and behind Terra at 20.8%.

These low absolute numbers are a useful reality check on how far general agent reliability still has to go.

## GDP.pdf and Harvey LAB-AA

GDP.pdf tests expert PDF document comprehension, and Harvey LAB-AA tests complex legal workflows.

Gemini 3.7 Flash leads both: 34.0% on GDP.pdf against 28.0% for Claude Sonnet 5 and 24.7% for GPT-5.6 Terra, and 90.7% on Harvey LAB-AA against 90.1% for Sonnet 5 and 85.2% for Terra.

If your workload is document-heavy legal or financial analysis, this is where the cheapest model on the table also happens to be the most accurate, which is a rare combination.

## GDM-MRCR v2 long context

GDM-MRCR v2 (8-needle) measures long-context retrieval, testing whether the model can find multiple buried pieces of information across a long input.

At the 128k average, 3.7 Flash scores 97.0%, ahead of 3.6 Flash at 91.8%, Claude Sonnet 5 at 81.5%, and GPT-5.6 Terra at 93.5%.

With a 1M token context window and the strongest 8-needle retrieval on the table, 3.7 Flash is a strong pick for pipelines that stuff large documents or long transcripts into a single call.

When we compared Terra and Sonnet 5, long-context retrieval was one of the sharper differentiators between them, and 3.7 Flash now sits above both at this band.

## Where GPT-5.6 Terra still leads

It is worth being honest about the losses.

GPT-5.6 Terra beats Gemini 3.7 Flash on DeepSWE v1.1 (69.6% vs 65.3%), Terminal-bench 2.1 (87.4% vs 85.8%), Terminal-bench 3.0 (20.8% vs 14.9%), and OSWorld-2.0 agentic computer use (50.2% vs 47.9%).

The pattern is that Terra holds a small edge on the hardest agentic and terminal coding evals, while 3.7 Flash wins on production code quality, document comprehension, and long-context retrieval. Which one wins for you depends on whether your bottleneck is agent reliability or cost.

## Gemini 3.7 Flash Pricing and Availability

Gemini 3.7 Flash is generally available now through several Google surfaces.

The API model ID is

`gemini-3.7-flash`, and you can start building in Google AI Studio, Android Studio, or the agent-first Google Antigravity environment.Developers:Gemini API via Google AI Studio and Android Studio; agent workflows in Google AntigravityEnterprises:Gemini Enterprise Agent Platform and the Gemini Enterprise appIndividuals:Gemini Spark, the 24/7 personal agent in the Gemini app, for Google AI Pro and Ultra subscribers in more than 160 countries
Pricing is $0.75 per 1M input tokens and $3.75 per 1M output tokens through December 31, 2026. On January 1, 2027, standard rates of $1.50 input and $7.50 output apply. Note that audio generation, image generation, and the Live API are not supported for this model, and computer use is preview only.

## Final Thoughts

Gemini 3.7 Flash is Google shipping fast and cutting prices while its flagship Gemini 3.5 Pro stays delayed.

Releasing an improved Flash model three weeks after 3.6 Flash, at half the token cost, reads as a company trying to win on the mid-tier, where most production volume actually lives.

Honestly, I think this is the model to reach for if your workload is document comprehension, production web and app code, or long-context retrieval, where it tops Claude Sonnet 5 and GPT-5.6 Terra outright while costing a fraction as much.

If your bottleneck is the hardest agentic terminal coding, Terra still holds a narrow lead on DeepSWE and Terminal-bench, and you would be paying more for that ceiling.

With the DeepMind leadership overhaul and the Pro model delay, Google is leaning on Flash to prove it can still compete. On the benchmark table it just published, that argument mostly holds for a mid-tier model.

If you want to get hands-on with models like this, I recommend starting with our AI Fundamentals skill track to build a foundation before wiring Gemini 3.7 Flash into your own agent pipelines.
