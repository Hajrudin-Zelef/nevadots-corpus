---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-decision-1-1-a-twice-as-fast-nowa-ai-tldr
title: "Microsoft-Decision-1.1 â twice as fast, nowâ¦ | AI/TLDR"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft", "microsoft-decision-1", "model", "decision", "twice", "fast", "vercel", "cost"]
source: https://ai-tldr.dev/releases/microsoft-decision-1-1/
source_anchor: ""
source_lines: [1, 58]
section: "Copilot_Microsoft"
---
Microsoft Â· 2026-11-17 Â· major

# Microsoft-Decision-1.1 â twice as fast, now reads images, same cost

In short

Microsoft-Decision-1.1 is now generally available. The decision model is rebuilt on Gemma 4 26B A4B, runs more than twice as fast as Decision-1 at the same cost, accepts images and scores 68 on the Artificial Analysis Decision Index.

Microsoft Â· 2026-11-17 Â· 3 sources checked

Microsoft's second decision model scores fixed answer options faster, with image input and better-calibrated confidence.

## Key specs

| Aa decision index v2.1 | 68 | 
|---|---|
| Speed vs decision 1 | >2x | 

## Quick facts

| Maker | Microsoft | 
|---|---|
| Base model | Gemma 4 26B A4B (MoE) | 
| Input | Text and images | 
| Status | Generally available, November 17, 2026 | 
| Availability | Microsoft Foundry, OpenRouter, Vercel AI Gateway | 
| Model id (Vercel) | microsoft/decision-1.1 | 

## What is it?

Microsoft-Decision-1.1, generally available since November 17, 2026, is the successor to Microsoft-Decision-1. Instead of writing text, a decision model returns a structured answer â yes/no, a choice or a score â with a calibrated confidence. Version 1.1 adds image input, such as photos and screenshots, and is more than twice as fast as the first model at the same cost, according to Microsoft.

## How does it work?

Microsoft rebuilt the model on Google's Gemma 4 26B A4B, a mid-sized mixture-of-experts model, and serves it on a custom inference engine tuned for low cost. It is trained to report its confidence accurately, to split confidence when more than one answer is valid, and to abstain when information is incomplete, so developers can set thresholds for acting, flagging or escalating.

## Why does it matter?

Agent systems make many small calls â which model to use, which team gets a ticket, whether an answer passes â and a large LLM is slow and costly for each one. Microsoft says that when Copilot uses the model to decide whether a request goes to Copilot or Cowork, it matches an LLM's accuracy 2.2x faster and at 4.4x lower cost. On Vercel, existing Decision-1 requests work unchanged after switching the model id.

## Who is it for?

developers building routing, classification and guardrail steps for agents

## Frequently asked questions

- How does Microsoft-Decision-1.1 compare with Microsoft-Decision-1?
- Microsoft says Microsoft-Decision-1.1 is more than twice as fast as Microsoft-Decision-1 at the same cost, is better calibrated, and accepts images. It scores 68 on the Artificial Analysis Decision Index v2.1, six points above the first model. It also moves from a Qwen3.5-9B base to Gemma 4 26B A4B.
- Where can developers use Microsoft-Decision-1.1?
- Microsoft-Decision-1.1 is generally available in Microsoft Foundry, on OpenRouter and on Vercel AI Gateway. Vercel lists the model id microsoft/decision-1.1 and supports it through the AI SDK (ai 7.0.128 or later), an OpenAI-compatible Decisions API (OpenAI Python SDK 3.26.0 or later) and its CLI.
- How does Microsoft-Decision-1.1 compare with other decision models?
- In Microsoft's internal evaluation of 102 benchmarks and more than 385,000 held-out questions, Microsoft-Decision-1.1 had the highest average accuracy of eight decision models and led in seven of nine capability categories. Microsoft's Artificial Analysis comparison names Jev 1.14, Gemini Decision 1 and GPT-6 Luna (Decisions). These are Microsoft's own results.
- Do I need to change my code to move to Microsoft-Decision-1.1?
- On Vercel AI Gateway, existing Microsoft-Decision-1 requests work unchanged; switching only needs the model id changed to microsoft/decision-1.1. Several questions can share one input in a single request, and decision requests show up in request logs, count toward budgets and support fallbacks.

## Try it

`model: 'microsoft/decision-1.1' on Vercel AI Gateway`
