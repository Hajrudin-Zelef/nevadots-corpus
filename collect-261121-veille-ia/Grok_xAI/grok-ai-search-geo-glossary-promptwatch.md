---
id: collect-261121-veille-ia/grok-xai/grok-ai-search-geo-glossary-promptwatch
title: "Grok - AI Search & GEO Glossary | Promptwatch"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-09-21"]
keywords: ["grok", "search", "model", "social", "posts", "november", "about", "brand"]
source: https://promptwatch.com/glossary/grok
source_anchor: ""
source_lines: [1, 20]
section: "Grok_xAI"
---
## Definition

Grok is the AI assistant and large language model family built by xAI, available on grok.com, in mobile apps and inside the X social network. Its distinguishing feature is live access to public posts on X alongside regular web search, a form of real-time search that makes it the assistant most likely to reflect what people are saying about a brand right now.

Ownership has changed twice in a short time. xAI acquired X in 2025, and SpaceX acquired xAI on February 2, 2026, in a deal Reuters reported valued xAI at $250 billion. The AI business rebranded as SpaceXAI in July 2026, and xAI's website and developer docs now use the name SpaceXSI; the API still runs at api.x.ai. SpaceX's IPO filing said about 117 million monthly active users used Grok's AI features as of March 31, 2026, within roughly 550 million monthly active users across X and Grok combined.

The model line moves quickly. Grok 4 launched in July 2025 with native tool use and real-time search. Grok 4.5 followed in July 2026 and Grok 4.6 in August 2026. Grok 4.7, released September 21, 2026, is xAI's current flagship: it has a 500,000-token context window and a knowledge cutoff of May 2026, according to xAI's model documentation. At launch it was available through the API, the Grok Build coding agent and Cursor, with xAI's model card saying it would reach the consumer Grok app and Grok in X later. The next model is Grok 4.8: Elon Musk said on November 10, 2026 that pretraining was complete and that xAI had rewritten its training and inference stack in C/C++, and the official Grok account said on November 13 that a soft rollout to Grok Bot users was underway, with no API release, price or benchmarks announced (Zeniteq). xAI's API release notes still listed Grok 4.7 as the newest model on November 13.

How Grok finds information matters for GEO. xAI's docs state plainly that Grok has no knowledge of current events beyond its training data unless search tools are enabled. In the consumer product, Grok searches both the web and X; developers can turn on separate web_search and x_search tools. For harder questions, Grok's DeepSearch mode (introduced with Grok 3 in February 2025) runs a multi-step investigation and writes a cited report. Each path has its own fetcher, and Grok Search, Grok DeepSearch and GrokBot show up as separate user agents in server logs, which Agent Analytics can break out per URL.

Because X posts are a first-class source, Grok answers can pick up complaints, launch reactions and executive posts within hours, sometimes before any article covers them. That makes sentiment monitoring on X part of AI visibility work as well as social media management. A brand whose public X presence is dormant or out of date leaves Grok to describe it from other people's posts. Other engines barely touch X. In our November 2026 social media citation data (October 11 to November 9), X earned 0.02% of Google AI Overviews citations and 0.04% of Perplexity's, even though social platforms overall held 13.16% of AI Overviews citations. What people post on X reaches AI answers mainly through Grok.

Practical steps: keep the brand's X profile and pinned post accurate, respond publicly to recurring complaints, make sure key web pages are crawlable by Grok's fetchers, and compare Grok's answers with ChatGPT and Google AI Mode for the same prompts as part of cross-platform AI visibility tracking. Large gaps usually point to a social narrative the web-based assistants are not seeing.

## Examples of Grok

- A consumer electronics brand finds Grok mentions a battery recall rumor that started on X two days earlier, while ChatGPT and Google AI Mode, which rely on news coverage, say nothing about it.
- A fintech startup's founder posts a clear thread explaining new pricing; within a day Grok answers about the company quote the new prices, while other assistants still show the old ones.
- A developer tests Grok 4.7 through the xAI API with x_search enabled to summarize community reaction to a framework release, then disables it to see what the model knows from training alone.
- A GEO analyst sees requests from the Grok-DeepSearch user agent in server logs after a competitor comparison goes viral on X, and checks that the comparison page renders without JavaScript.
