---
id: collect-261121-veille-ia/microsoft-actu/microsoft-decision-1-1-goes-live-pricing-and-a-practical-routing-check
title: "Microsoft-Decision-1.1 goes live: pricing and a practical routing check"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft", "model", "input", "microsoft-decision-1", "pricing", "tokens", "decision", "vercel"]
source: https://promptszone.com/microsoft-decision-1-1-ga-pricing-routing
source_anchor: ""
source_lines: [1, 44]
section: "Microsoft_Actu"
---
**Summary:**

Microsoft released Microsoft-Decision-1.1 on November 17, moving its specialized decision model into general availability through Foundry, OpenRouter and Vercel. The update adds image inputs and targets the small judgments that accumulate inside an application: where to route a request, which task needs attention and whether a workflow should continue.

The useful distinction is not “a cheaper chatbot.” A decision model selects among bounded answers. A generative model writes the response that follows. Keeping those jobs separate can make an application easier to measure and control.

## What is available now

Microsoft's announcement identifies 1.1 as generally available, rather than a research preview. OpenRouter lists the new version with a November 17 release date and a 33K context window. Its model page explicitly excludes open-ended conversation, translation and summarization from the intended use cases.

Vercel confirms that AI Gateway accepts the same request format as Decision-1. Developers switching there use `microsoft/decision-1.1`. Its launch examples require AI SDK 7.0.128 or later, or OpenAI's Python SDK 3.26.0 or later. Those are Gateway-specific integration instructions, not a universal deployment recipe for every provider.

## Three ways to ask a bounded question

Vercel describes three output types: a probability for a yes-or-no condition, a selected category with probabilities for the alternatives, or a numerical score on a defined scale. Multiple questions can share one input.

For a support queue, that could mean choosing between billing and technical support, rating urgency and checking whether a reply needs review. The application still decides what each answer permits. A routing score should not silently become authorization to issue a refund.

## Pricing starts at $0.042 per million input tokens

Microsoft's Foundry launch pricing is $0.042 per million input tokens for Global Standard, $0.046 for the US Data Zone and $0.05 for EU and APAC Data Zones. Output tokens carry no charge. OpenRouter separately lists $0.042 input and $0 output per million tokens.

At those Foundry rates, 100 million input tokens would cost $4.20, $4.60 or $5.00 respectively, before other application costs. That calculation is not a bill estimate: repeated inputs, retries and the generative model handling the next step still matter. Our guide to request share versus spending share explains why traffic volume alone can mislead an AI budget.

## Read the performance claims in context

Microsoft reports a score of 68 on Artificial Analysis's Decision Index v2.1, six points above its predecessor. Separately, its internal evaluation covers 102 benchmarks and more than 385,000 questions held out from training. These are different evaluation sets, not interchangeable measurements.

For a deployment decision, test the mistakes your own workflow cannot afford. An aggregate score does not tell you how often an urgent ticket goes to the wrong queue or how a new category changes confidence.

## A practical migration check

Microsoft's existing Decision-1 documentation recommends representative labeled examples, clear answer options and thresholds tied to the cost of false positives and false negatives. It also warns that wording and option order can affect scores. The documentation still names version 1, so use the new provider-specific instructions for 1.1 deployment details rather than copying an old model identifier.

Start with a recorded sample of requests. Compare the current route with the proposed route, including the cases sent for human review. Measure latency, input usage and error rates together. Keep a fallback for ambiguous or failed requests, then expand only when the results support it. For decisions affecting people, meaningful human review matters more than a low token price.

## Sources and image credits

- Microsoft: Microsoft-Decision-1.1 general availability, November 17, 2026.
- Vercel: Decision-1.1 on AI Gateway, November 17, 2026.
- OpenRouter: version, context and pricing listing.
- Microsoft Learn: Decision-1 integration guidance and limitations.

Pricing chart: PromptsZone, using Microsoft's November 17 Foundry rates. The official Microsoft provider logo is reproduced from the Microsoft News Center under its editorial-use permission; it is not a separate Decision-1.1 logo.
