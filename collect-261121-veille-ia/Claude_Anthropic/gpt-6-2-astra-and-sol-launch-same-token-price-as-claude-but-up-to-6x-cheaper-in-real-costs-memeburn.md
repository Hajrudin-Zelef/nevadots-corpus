---
id: collect-261121-veille-ia/claude-anthropic/gpt-6-2-astra-and-sol-launch-same-token-price-as-claude-but-up-to-6x-cheaper-in-real-costs-memeburn
title: "GPT-6.2 Astra and Sol Launch: Same Token Price as Claude, but Up to 6x Cheaper in Real Costs - Memeburn"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-18"]
keywords: ["astra", "gpt-6", "price", "same", "models", "work", "token", "cheaper"]
source: https://memeburn.com/gpt-6-2-astra-and-sol-launch-same-token-price-as-claude-but-up-to-6x-cheaper-in-real-costs/
source_anchor: ""
source_lines: [1, 92]
section: "Claude_Anthropic"
---
TL;DR

- GPT-6.2 Astra and Sol are rolling out to Plus, Pro, Enterprise and Edu users in ChatGPT's Work tab and Codex, plus the API, but not to the regular Chat tab or the Free and Go plans.
- Astra's API price fell 20% to 8/40 per million tokens, while Sol stays at 2/10; Vals AI measured Astra's cost per task falling about half, short of OpenAI's "less than a third" claim.
- Astra and Claude Fable 5.2 share the same token prices, yet Artificial Analysis puts Astra at $4.00 per test against Fable's $26.54, with Fable scoring three points higher.

When I first looked at GPT-6.2 Astra’s price list, I assumed OpenAI had simply cut its price to match the competition, since $8 per million input tokens and $40 per million output tokens is exactly what Anthropic charges for Claude Fable 5.2.

But once I opened the data from Artificial Analysis, an independent firm that scores AI models, the story looked completely different: to finish the same test, Astra spent only about $4, while Fable 5.2 spent $26.54.

Same unit price, yet the total bill differs by more than six times, so where did the money go? In this article, I’ll walk through what’s new in GPT-6.2, who can actually use it, why the biggest saving isn’t on the price list, and finally which model is worth choosing.

Tradingview Promo Code

Get a $15 Discount

Unlimited advanced charts, indicators, and alerts. Step up your trading.

Code Valid: November 2026

## **Who Can Use GPT-6.2 Astra and Sol, and Where?**

OpenAI released the two models on November 17. According to OpenAI’s announcement, if you’re paying for ChatGPT Plus, Pro, Enterprise or Edu, you already have access. The catch is that you won’t find them in the usual place. ChatGPT is now split into two tabs: a Chat tab for quick questions, and a Work tab for handing off long, multistep jobs that the AI then carries out from start to finish. GPT-6.2 only lives in the Work tab and in Codex, OpenAI’s coding tool, while the Chat tab most people type into every day stays exactly the same.

Free and Go users aren’t on the list. What surprised me a little is that the announcement doesn’t mention the Business plan either, even though the Work tab has been available on Business since day one. So if your company uses Business and you don’t see the new models yet, it’s most likely down to OpenAI’s rollout schedule rather than a problem with your account.

The two models are also built for different kinds of work. Astra is the most powerful one, aimed at long projects such as building slide decks, websites or 3D designs. Sol is the leaner, cheaper option for everyday work and for “agentic” tasks, which means you hand over a job and let the AI carry out the whole chain of steps without prompting it at each one. Pro 500 subscribers get Ultrafast mode (faster responses) on both models, while Pro 200 subscribers get Ultrafast for the first time, starting with Sol. Developers call the two models through the API, the paid connection that lets other apps plug OpenAI’s AI into their own software.

Astra also comes with an extra layer of control. Because OpenAI considers it the company’s best model at cybersecurity, Astra will refuse more firmly when a request could be used to attack systems, while security teams that OpenAI has verified get looser access through its Daybreak program.

## **The Price Only Fell 20%, So Why Did Costs Drop by Half?**

First, it helps to understand how AI is billed. Models don’t charge per question but per token, and each token is a small piece of text, roughly three-quarters of an English word. However much text you send in and however much the AI writes back, all of it gets converted into tokens and multiplied by the unit price.

| **Model** | **Input (per 1M tokens)** | **Cached input** | **Output** | **Compared with the previous version** | 
| GPT-6.2 Astra | $8.00 | $0.40 | $40.00 | Input and output 20% cheaper, cached input 60% cheaper than GPT-6 Astra ($10 / $1.00 / $50) | 
| GPT-6.2 Sol | $2.00 | $0.10 | $10.00 | Same as GPT-6.1 Sol | 

*OpenAI API list prices, checked November 18, 2026.*

The “cached input” column probably needs a little explaining. It’s text the model has already read, for example when you give it a long document and ask it question after question about it, the model doesn’t have to read it again from scratch each time, so that part is billed at a much lower rate.

OpenAI says the new Astra does better work than the old one at less than a third of the cost per task. That sounds very impressive! But if the unit price only fell 20%, where does the rest come from? The answer is that the new Astra “talks less”: it needs fewer tokens to reach the same result, so the bill gets lighter even though the unit price barely moved.

Of course, OpenAI’s numbers are measured by OpenAI, so I went looking for independent data. Vals AI, another evaluator, measured the new Astra at $10.68 per task against $21.28 for the old one, which is a drop of about half rather than the two-thirds OpenAI claims. When I broke that figure down, after taking out the 20% price cut, the new Astra still uses about a third fewer tokens per task than the old version. (How I calculated it: cost is now about 50% of what it was, divided by a unit price that’s now 80%, which leaves token usage at about 63%.) Part of that could come from the cheaper cache price, so you should treat it as an estimate.

There’s one more reason, and it lies in how OpenAI ran its comparison. OpenAI’s models have a setting called “effort”, meaning how hard the model thinks, from low up to the maximum. In its comparison, OpenAI put the new Astra at “xhigh” and the old Astra at “max”, the highest level, which means a lower setting can now do the job that used to require the maximum. I saw exactly this pattern before when analyzing why Claude Sonnet 5.5 is cheaper without a price cut.

## **Where Does GPT-6.2 Stand Against Claude and Gemini?**

To compare the models, I used Artificial Analysis’s scoreboard. It runs each model through nine different tests covering knowledge, coding and science, combines the results into one overall score called the Intelligence Index, and also records how much each model spent to get through the whole set.

| **Model** | **API price (input / output, per 1M tokens)** | **Intelligence Index score** | **Cost per test** | 
| Claude Fable 5.2 | $8 / $40 | 65 | $26.54 | 
| GPT-6.2 Astra | $8 / $40 | 62 | $4.00 | 
| Gemini 4 Carbon | $2 / $10 until Nov 30, then $4 / $20 | 60 | Not used here | 
| Claude Opus 5.5 | $4 / $20 | 59 | Not used here | 
| GPT-6.2 Sol | $2 / $10 | 57 | $1.01 | 

*Scores at each model’s highest effort setting, according to* *Artificial Analysis**. Prices from each company’s official pricing page, checked November 18, 2026.*

Looking at the table, Claude Fable 5.2 is still on top with 65 points, three ahead of Astra, so OpenAI hasn’t taken the number one spot (if you want to know which kinds of work Fable 5.2 is strongest at, our Claude Fable 5.2 vs Fable 5.1 comparison covers it in detail). But look over at the price column. According to Anthropic’s pricing page, Fable 5.2 and Astra charge exactly the same token rates, and Anthropic’s cache price is actually cheaper ($0.20 against $0.40). Yet to get through the same set of tests, Fable spent $26.54 while Astra spent only $4. When the unit price is the same but the total differs by more than six times, there’s really only one explanation: Fable uses far more tokens than Astra. In other words, if your business pays to use Fable through the API, you’re paying about six and a half times as much for 3 extra points.

With Sol, there’s a date you should keep an eye on. Right now, Google’s Gemini 4 Carbon costs exactly the same as Sol, $2 and $10. But that’s only a launch promotion, and according to Google’s Gemini API pricing page, Carbon will rise to $4 and $20 from December 1. That means in just two weeks, Sol will cost half as much as Carbon, even though Carbon edges Sol by 3 points. Claude Opus 5.5 is also at $4 and $20, double Sol’s price, with 59 points, the same price gap I weighed in our Claude Opus 5.5 vs GPT-6 Sol comparison.

The weakest part of this launch turns out to be coding. Artificial Analysis has a separate scoreboard for using AI to write code, run directly inside Codex, and on it the new Astra is no better than the old one, while Sol actually drops 3 points from the previous Sol. OpenAI, for its part, published results on FrontierCode 2.0, a test that involves fixing code in real projects, showing Sol only half a point behind Astra while costing about five times less. The two sources tell different stories, and on FrontierCode 2.0 itself, the top model is still Fable 5.2.

## **In My View, Most Users Should Start With Sol**

If I had to pick one model, I’d pick Sol, and I think most paying ChatGPT users should do the same. The reason comes down to a simple division: Sol scores 57, which is 92% of Astra’s score, while its cost per test is only $1.01, about a quarter of Astra’s. So you’d be paying four times as much just to get the last 5 points. From December 1, Sol will also cost half the list price of both Gemini 4 Carbon and Claude Opus 5.5.

So who is Astra for? In my view, it’s for long, important jobs where a botched attempt costs you more time than the money you’d save, such as a complete slide deck for a presentation, or a document you’d have to rewrite from scratch if it came out wrong.

There’s one exception I want to be clear about. If you mainly use AI to write code in Codex, don’t rush to switch to Sol, because that’s exactly where Sol lost points. Have it try fixing a few pieces of real code in your own project first, and only make it your default once it holds up.

## **FAQs**

### **How much does GPT-6.2 Astra cost in the API?** 

GPT-6.2 Astra costs $8 per million input tokens, $0.40 per million cached input tokens and $40 per million output tokens, 20% cheaper than GPT-6 Astra on input and output. GPT-6.2 Sol costs $2, $0.10 and $10, unchanged from GPT-6.1 Sol.

### **Can free ChatGPT users use GPT-6.2?** 

No. OpenAI’s announcement only lists the Plus, Pro, Enterprise and Edu plans, and the two models only appear in ChatGPT’s Work tab and in Codex, not in the regular Chat tab.

### **Is GPT-6.2 Astra better than Claude Fable 5.2?** 

Not yet. On Artificial Analysis’s Intelligence Index, Claude Fable 5.2 scores 65 while GPT-6.2 Astra scores 62. Even so, Astra spends only about $4 per test against $26.54 for Fable 5.2, even though both charge the same $8 and $40 per million tokens.

### **What is Ultrafast in ChatGPT?** 

Ultrafast is OpenAI’s faster mode for running its models. Pro 500 subscribers can use it with both GPT-6.2 Astra and Sol, while Pro 200 subscribers get it for the first time, starting with GPT-6.2 Sol.
