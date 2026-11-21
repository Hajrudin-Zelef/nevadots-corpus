---
id: collect-261121-veille-ia/gemini-google/how-to-migrate-gemini-3-8-flash-to-gemini-4-hydrogen-sampling-caching-and-thinking-changes
title: "How to Migrate Gemini 3.8 Flash to Gemini 4 Hydrogen: Sampling, Caching and Thinking Changes"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-12"]
keywords: ["gemini", "hydrogen", "caching", "guide", "thinking", "says", "values", "google"]
source: https://blog.traversaal.ai/how-to-migrate-gemini-38-flash-to-gemini-4-hydrogen-sampling-caching-and-thinkin/
source_anchor: ""
source_lines: [1, 92]
section: "Gemini_Google"
---
To migrate Gemini 3.8 Flash to Gemini 4 Hydrogen, do more than change the model ID. Google's AI Studio migration guide says to remove `temperature`, `top_p` and `top_k`, drop `cached_content`, stop sending the `minimal` thinking level, and plan for a default thinking level of `high` instead of `medium`. Hydrogen was released on November 12, 2026, and the same day it became the default model for the Antigravity agent in Gemini Managed Agents.

**TL;DR:** Moving from Gemini 3.8 Flash to Gemini 4 Hydrogen needs four code changes. Remove sampling parameters: Google's AI Studio guide says out-of-range values return a 400 error, while the Vertex guide says custom values are ignored. Delete explicit caching calls and rely on implicit caching from 4,096 tokens. Replace the minimal thinking level and budget for the new high default, since Google bills thinking tokens. Test on staging first.

## Key Takeaways

- **Sampling parameters:** Google tells you to remove`temperature` ,`top_p` and`top_k` . The AI Studio guide says out-of-range values return a 400 validation error; the Vertex guide says custom values are ignored.
- **Caching:** Explicit`cached_content` is unsupported, and implicit caching runs by default on prompts of at least 4,096 tokens.
- **Thinking:** The default moves from`medium` to`high` , and`minimal` returns an error.
- **Billing:** Google's migration guide lists billing for thinking tokens and function declarations. One place on the page ties function-declaration billing to grounding while the checklist does not, so confirm on your own bill.
- **Timing:** The Antigravity agent in Gemini Managed Agents now defaults to Hydrogen, so an omitted`agent_config` changes models without a code change.

## What actually breaks when you swap the model ID?

Four things change at once: sampling parameters, explicit caching, thinking levels and billing. The first three produce errors or silent behavior shifts in code that worked on Gemini 3.8 Flash. The fourth shows up later, on the invoice.

Google launched Hydrogen on November 12, 2026 and lists it at $1.50 per million input tokens and $7.50 per million output tokens, flat across its 1 million-token context window. The AI Studio changelog entry for the same date says Hydrogen is now the default model for the Antigravity agent in Gemini Managed Agents. The Antigravity agent documentation adds that if you omit `agent_config`, the agent defaults to `gemini-4-hydrogen`.

Teams that never pinned a model should set `agent_config` explicitly to control when the switch happens.

## Do temperature, top_p and top_k still work on Gemini 4 Hydrogen?

No. Google's guides say to remove them. The AI Studio guide says the server ignores in-range custom values if sent, while out-of-range values return a 400 validation error. The Vertex guide, covered below, says custom values are ignored.

The Vertex AI version of the Hydrogen developer guide words this differently: it says the backend ignores custom sampling values and does not mention a 400 for them. It also lists extra hard failures. Passing `frequency_penalty`, `presence_penalty` or a `candidate_count` greater than 1 returns an HTTP 400 error there, and prefilled model turns in chat history are not supported. I could not confirm that the AI Studio and Vertex surfaces behave identically for out-of-range sampling values, so test the surface you actually call.

## What replaces explicit caching on Hydrogen?

Implicit caching replaces it, and you cannot create explicit caches at all. Google states that Hydrogen does not support explicit caching through `cachedContents` or `caches.create`, and tells you to remove `cached_content` fields from requests.

Implicit caching runs by default on prompts of at least 4,096 tokens. The Vertex guide describes it as applying to shared prompt prefixes of 4,096 tokens or more. The practical rule is to put static content first: system instructions, tool descriptions and reference documents at the top, the user's changing question at the bottom. Prompts under 4,096 tokens get no implicit caching, so a 2,000-token instruction block you cached explicitly before no longer qualifies. The AI Studio guide says implicit caching discounts cached input by 95%, to $0.075 per million tokens.

## Why does thinking default to high, and what does it cost?

Hydrogen defaults to `high` thinking where Gemini 3.8 Flash defaulted to `medium`, and Google bills the extra tokens. The AI Studio guide says Hydrogen defaults to `high` rather than `medium`, adds `xhigh`, and rejects `minimal` with an error. The Vertex guide states that `thinking_budget` and `thinking_level="MINIMAL"` return an HTTP 400 error.

Google's AI Studio migration guide lists billing for thinking tokens and function declarations. The page words the function-declaration part two ways: one place ties it to grounding, while the migration checklist omits the "when using grounding" qualifier. Teams should confirm on their own bill. Output pricing includes thinking tokens at $7.50 per million.

Here is the arithmetic on that listed price. A request that produces 10,000 thinking tokens adds $0.075 in output charges. Across 100,000 requests, that is $7,500. This is an illustration of the price list, not a measurement of Hydrogen, because I found no published figure for how many extra thinking tokens `high` produces compared with `medium`.

Google's launch post reports that Hydrogen finishes tasks 2.5 times faster than Gemini 3.8 Flash at its high default setting. That is a vendor claim, so run your own evals before assuming the speed offsets the token bill.

The practical fix is to set `thinking_level` explicitly on every call. Use `LOW` or `MEDIUM` for classification, extraction and routing. Reserve `HIGH` and `XHIGH` for the steps where quality gains justify the spend. Google's migration page says to replace `thinking_budget` with a `thinking_level` in the same pass.

## How should you roll out the migration?

Change one setting group at a time on staging, then compare errors and token counts against the same prompts on Gemini 3.8 Flash. The table below maps each documented change to an action.

| Setting | What Google's docs say about Hydrogen | Migration action | 
|---|---|---|
| `temperature` ,`top_p` ,`top_k` | AI Studio: out-of-range values return a 400 error, in-range values are ignored. Vertex: custom values are ignored | Delete from requests and config files | 
| `cached_content` | Explicit caching is unsupported | Remove calls; put static prefix first | 
| Implicit caching | Runs by default on prompts of at least 4,096 tokens | Check prompt length and prefix stability | 
| `thinking_level` | Default `high` , supports`LOW` ,`MEDIUM` ,`HIGH` ,`XHIGH` ;`minimal` errors | Set explicitly on every call | 
| Billing | Lists billing for thinking tokens and function declarations; one place ties the latter to grounding, the checklist does not | Re-estimate cost per request and confirm on your bill | 
| Antigravity agent | Hydrogen is the default model | Pin the model in `agent_config` | 

Grep for the removed parameters and caching fields, log token usage per request split by thinking level, and alert on 400 responses so a stray value surfaces fast. The Vertex guide notes that Hydrogen is a short-term generally available release with a minimum 45-day retirement notice, so keep the migration repeatable.

## Frequently Asked Questions

### Will my Gemini 3.8 Flash code run unchanged on Gemini 4 Hydrogen?

Probably not. On AI Studio, out-of-range sampling values return a 400 error, `cached_content` is unsupported, and the `minimal` thinking level returns an error. Code that sets none of these will run, but it will use the `high` thinking default and its higher token usage.

### Is explicit context caching available on Gemini 4 Hydrogen?

Google's migration guide says Hydrogen does not support explicit caching. Implicit caching covers prompts of at least 4,096 tokens. Plan around implicit caching.

### Does Hydrogen bill for function declarations?

Google's migration guide lists billing for thinking tokens and function declarations, but the page is not consistent. One place ties function-declaration billing to grounding, while the migration checklist omits that qualifier. Confirm on your own bill, and measure token counts per request before and after the move.

### Which thinking level should I choose?

Set it explicitly. Hydrogen supports `LOW`, `MEDIUM`, `HIGH` and `XHIGH`, with `HIGH` as the default. Start with `MEDIUM` to match what Gemini 3.8 Flash did by default, then raise it only for steps where evals show a quality gain.

## Conclusion

Treat this as a configuration migration with a cost review attached. Remove the sampling parameters, delete explicit caching, set `thinking_level` on every call and pin the model where you manage agents. Do those four things on staging before the change reaches production, and compare token counts against Gemini 3.8 Flash on identical prompts.

### Learn from me

**Agent Engineering Bootcamp: Developers Edition**, my Maven cohort. Advanced agentic RAG, multi-agent orchestration, memory, evals, and guardrails. Take agents from prototype to production. Join the next cohort →

### Hire us

**Traversaal.ai**. We're a team of forward deployed engineers solving the toughest AI problems for Fortune 100 companies: document intelligence, agentic data platforms, and real-time web intelligence, deployed in production. **Work with our team to deploy your next agentic ecosystem.** Talk to Traversaal.ai →

### Join us

Want to solve these problems with us? We're always looking for forward deployed engineers who want to ship production AI. jobs@traversaal.ai
