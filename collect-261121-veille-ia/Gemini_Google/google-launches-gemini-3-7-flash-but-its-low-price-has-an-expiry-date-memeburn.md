---
id: collect-261121-veille-ia/gemini-google/google-launches-gemini-3-7-flash-but-its-low-price-has-an-expiry-date-memeburn
title: "Google Launches Gemini 3.7 Flash but Its Low Price Has an Expiry Date - Memeburn"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "flash", "google", "tokens", "points", "million", "output", "model"]
source: https://memeburn.com/gemini-3-7-flash-review-pricing-benchmarks-2026/
source_anchor: ""
source_lines: [1, 106]
section: "Gemini_Google"
---
TL;DR

- Gemini 3.7 Flash launched on August 13, three weeks after Gemini 3.6 Flash.
- Google lists introductory pricing of $0.75 per million input tokens and $3.75 per million output tokens through December 31, 2026.
- On January 1, 2027, those rates double to $1.50 for input and $7.50 for output.
- The model scored 65.3% on DeepSWE v1.1 and 30.4% on AutomationBench, with clear gains over Gemini 3.6 Flash.
- Gemini Spark access excludes the European Economic Area, the United Kingdom, Switzerland and Nigeria at launch.
- GPT-5.6 Luna and DeepSeek V4 Flash remain cheaper at their published August 14 rates, although DeepSeek has announced an imminent pricing change.

Google released Gemini 3.7 Flash on August 13, 2026, only three weeks after Gemini 3.6 Flash. The new model delivers substantial gains in coding, document analysis and workflow automation while cutting the introductory API price to $0.75 per million input tokens and $3.75 per million output tokens.

Those improvements are real. Gemini 3.7 Flash climbed from 49.0% to 65.3% on DeepSWE v1.1 and reached 30.4% on Zapier’s AutomationBench, up from 17.0% for its predecessor. Independent testing from Artificial Analysis also placed it among the fastest reasoning models at roughly 340 output tokens per second.

The launch headline leaves out several conditions. The discounted price expires at the end of 2026, other models remain cheaper, and the consumer rollout through Gemini Spark excludes several large markets. Google’s model also loses to rivals on some agent and computer-use evaluations. Developers therefore need to separate a strong Flash upgrade from the broader claim that it is the best or cheapest model for every workload.

## **What Is Gemini 3.7 Flash**

Gemini 3.7 Flash is Google’s latest high-speed model for coding, agents and knowledge work. It replaces Gemini 3.6 Flash as the company’s main workhorse model only three weeks after the previous version arrived.

Google describes the update as the result of developer feedback and algorithmic improvements. Gemini 3.7 Flash is designed to follow instructions more reliably and recover from roadblocks across multi-step tasks.

MemeBurn reported Gemini 3.7 Flash references appearing in Google’s Python SDK one day before the official launch. Google has now confirmed access through the Gemini API, Google AI Studio, Android Studio, Antigravity and Gemini Enterprise.

For individual users, Gemini 3.7 Flash also powers Gemini Spark for Google AI Pro and Ultra subscribers in supported countries. That consumer route has regional restrictions that do not necessarily apply to developer API access.

## **The Benchmark Gains Are Substantial**

Google’s official Gemini 3.7 Flash announcement shows consistent improvement over Gemini 3.6 Flash across coding, web development, document analysis and automation.

| **Benchmark** | **Gemini 3.7 Flash** | **Gemini 3.6 Flash** | **Change** | 
| DeepSWE v1.1 | 65.3% | 49.0% | Up 16.3 points | 
| FrontierCode 1.1 Main | 43.6% | 34.4% | Up 9.2 points | 
| WebDev Arena Elo | 1,588 | 1,538 | Up 50 points | 
| GDP.pdf | 34.0% | 22.0% | Up 12 points | 
| AutomationBench | 30.4% | 17.0% | Up 13.4 points | 

The DeepSWE improvement matters for coding agents because the benchmark covers longer software engineering tasks. FrontierCode indicates stronger first-pass code quality, while WebDev Arena points to better web interfaces.

Artificial Analysis gave Gemini 3.7 Flash High an Intelligence Index score of 56, four points above Gemini 3.6 Flash. It also measured output at around 340 tokens per second, nearly three times the speed of GPT-5.6 Terra in its comparison. Production teams should still measure task success, retries and total token use rather than extrapolating from speed.

## **The Half-Price Offer Ends in December**

Google has set Gemini 3.7 Flash at $0.75 per million input tokens and $3.75 per million output tokens, half the original price of Gemini 3.6 Flash.

Google’s introductory rate expires on December 31, 2026. Beginning January 1, 2027, input pricing increases to $1.50 and output pricing rises to $7.50.

The offer is clearly disclosed, but budgets should use the 2027 rate for applications expected to remain in production next year.

At the time of writing on August 14, Gemini 3.7 Flash is also not the lowest-priced major API model. OpenAI lists GPT-5.6 Luna at $0.20 for input and $1.20 for output. MemeBurn has covered how the GPT-5.6 Luna price cut changed the market for high-volume AI workloads.

DeepSeek lists V4 Flash at $0.14 for uncached input and $0.28 for output, making its current output rate more than 13 times lower. New DeepSeek V4 pricing takes effect on August 17, so the gap may soon narrow.

Price comparisons also need to account for capability. Luna and V4 Flash target high-volume efficiency, while Gemini 3.7 Flash is competing on stronger coding, multimodal and agent performance. The relevant figure is cost per successful task, not the cheapest token in isolation.


## **Strong Results Do Not Make It the Best at Everything**

Gemini 3.7 Flash performs well across a broad benchmark set, but Google’s own comparison does not show a universal lead.

TechTimes reported that GPT-5.6 Terra scored 50.2% on OSWorld 2.0, ahead of Gemini 3.7 Flash at 38.1%. Claude Sonnet 5 also led on Agent’s Last Exam with 33.3%, compared with Gemini at 26.3%. Artificial Analysis placed Gemini at 56 on its Intelligence Index, one point behind GPT-5.6 Terra and Muse Spark 1.2.

OSWorld measures interaction with computer environments, while Agent’s Last Exam tests difficult multi-step reasoning. Gemini’s coding and speed advantages do not guarantee the same ranking in those tasks.

AutomationBench needs similar context. The benchmark is built by Zapier and tests end-to-end workflows across 47 tools in sales, marketing, operations, support, finance and human resources. Gemini 3.7 Flash currently leads its public leaderboard at 30.44%.

The tasks reflect patterns across Zapier’s platform, making the result useful for business automation. It remains one environment, and teams using different tools, permissions and data may see another ranking.

## **Gemini Spark Is Not Available Across Much of Europe**

Google says Gemini 3.7 Flash will power Gemini Spark for Google AI Pro and Ultra subscribers. Spark can manage multi-step tasks, work with connected apps and continue certain jobs in the background.

The Gemini Apps support page states that Spark is unavailable in the European Economic Area, the United Kingdom, Switzerland and Nigeria. Users also need to be at least 18, use a personal Google account and keep Gemini Apps activity enabled.

This restriction applies to Spark rather than the model’s entire availability. Developers may have other routes through Google AI Studio, the Gemini API or enterprise products.

Google says Spark reaches more than 160 countries, but the excluded markets still cover a significant share of European users.

## **Who Should Use Gemini 3.7 Flash**

Gemini 3.7 Flash suits coding agents, document processing and high-throughput web applications, especially where latency and first-pass code quality matter.

Enterprise trials should record task success, response time, token use, retries and human review, then recalculate the result using the January 2027 price.

Teams choosing primarily on API cost have cheaper options. GPT-5.6 Luna and DeepSeek V4 Flash currently undercut Gemini, although their capability profiles differ. MemeBurn’s broader GPT-5.6 model comparison explains where OpenAI’s Sol, Terra and Luna tiers fit.

Consumers in the EEA, Britain, Switzerland and Nigeria cannot use Gemini Spark at launch.

Gemini 3.7 Flash is therefore a strong workhorse release with unusually fast output and meaningful coding gains. Its value depends on whether those advantages survive a team’s production workload after the introductory discount disappears.

## **FAQs**

### **How Much Does Gemini 3.7 Flash Cost**

Gemini 3.7 Flash costs $0.75 per million input tokens and $3.75 per million output tokens through December 31, 2026. The rates rise to $1.50 for input and $7.50 for output on January 1, 2027.

### **Is Gemini 3.7 Flash Better Than Claude Sonnet 5**

It depends on the task. Gemini 3.7 Flash leads on Zapier’s AutomationBench and offers faster, cheaper inference. Claude Sonnet 5 scored higher on Agent’s Last Exam in the published comparison, indicating an advantage on that difficult multi-step agent test.

### **Is Gemini 3.7 Flash the Cheapest AI Model**

No. As of August 14, 2026, GPT-5.6 Luna and DeepSeek V4 Flash have lower published input and output prices. DeepSeek has announced that its V4 API pricing will change on August 17.

### **Where Is Gemini 3.7 Flash Available**

Developers can access the model through the Gemini API, Google AI Studio, Android Studio and Antigravity. It is also available in enterprise products. Consumer access through Gemini Spark excludes the EEA, the United Kingdom, Switzerland and Nigeria at launch.
