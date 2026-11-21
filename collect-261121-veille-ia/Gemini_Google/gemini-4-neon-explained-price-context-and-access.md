---
id: collect-261121-veille-ia/gemini-google/gemini-4-neon-explained-price-context-and-access
title: "Gemini 4 Neon Explained: Price, Context and Access"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["gemini", "google", "neon", "model", "tokens", "reasoning", "access", "context"]
source: https://www.layer3labs.io/guides/gemini-4-neon-explained
source_anchor: ""
source_lines: [1, 110]
section: "Gemini_Google"
---
# Gemini 4 Neon Explained

Released on November 18, 2026, Gemini 4 Neon pairs a 1-million-token context window with frontier reasoning for high-frequency workflows.

Gemini 4 Neon is an artificial intelligence (AI) model released by Google on November 18, 2026, positioned between Gemini 4 Carbon and Gemini 4 Hydrogen. At Layer3Labs, we build custom AI agents and workflow automations for small and mid-sized businesses (SMBs), so model price and routing decide which model each workflow runs on. Google built Gemini 4 Neon to bring frontier-level intelligence with fast end-to-end task completion across complex software engineering, analytical reasoning, and production-grade enterprise workflows.

Under the model identifier gemini-4-neon, the system processes a unified 1-million-token context window with pricing set at $3.00 per 1 million input tokens and $15.00 per 1 million output tokens on the paid Standard Application Programming Interface (API) tier. The model does not offer a free API tier. Google makes Gemini 4 Neon accessible through the paid Gemini API in Google AI Studio, Gemini for business, Google Antigravity for paid Pro and Ultra subscribers, and the Gemini app for Pro and Ultra subscribers.

Gemini 4 Neon balances reasoning depth against operational efficiency. Route complex, high-frequency coding and enterprise tasks to Gemini 4 Neon, reserve Gemini 4 Carbon for your hardest long-horizon projects, and run everyday automation on Gemini 4 Hydrogen.

## What Gemini 4 Neon Is

Gemini 4 Neon is Google's frontier-level reasoning model released on November 18, 2026, engineered to deliver fast end-to-end task completion for complex coding, analytical reasoning, and production-grade enterprise workflows. Google introduced the model under the identifier gemini-4-neon, as detailed in the official introductory blog post. Google designed the system to bridge the operational space between its deep reasoning frontier model, Gemini 4 Carbon, and its high-speed execution engine, Gemini 4 Hydrogen.

On Google's paid Standard API tier, Gemini 4 Neon costs $3.00 per 1 million input tokens and $15.00 per 1 million output tokens, counting thinking tokens as output. The system processes a 1-million-token unified context window and supports long-decode generation up to 1M tokens. Google published these rates on the official Gemini API pricing page. Gemini 4 Neon is not available on the Gemini API free tier, meaning every token processed is billed.

Developers can access the model through the Gemini API in Google AI Studio and Android Studio, while enterprise organizations can access it via Gemini for business through the API and the model drop-down menu. Subscribers to Google AI plans with Pro and Ultra tiers can run the model inside the Gemini app, while eligible subscribers can also access it in Google Antigravity. Google's launch post does not name Vertex AI or Google Cloud as an available surface for Gemini 4 Neon.

## Where Neon Sits in Gemini 4

Google structures the Gemini 4 family around specific performance and operational profiles. In Google's words, Gemini 4 Neon balances depth and efficiency, delivering frontier-level reasoning for complex, high-frequency workflows faster and more cost efficiently. Google positions Gemini 4 Carbon as its most capable model for your hardest long-horizon projects, while Gemini 4 Hydrogen handles fast everyday execution.

| Model Name | Role in Google's Words | Input / Output Rate per 1M Tokens (Through Nov 30, 2026) | Input / Output Rate per 1M Tokens (Starting Dec 1, 2026) | Cached Input Rate per 1M Tokens | Free API Tier Available | Primary Access Surfaces | 
|---|---|---|---|---|---|---|
| Gemini 4 Neon | Balances depth and efficiency for complex coding, analytical reasoning, and high-frequency production workflows. | $3.00 / $15.00 | $3.00 / $15.00 | $0.15 | No | Gemini API, Google AI Studio, Gemini app (Pro/Ultra), Antigravity (Pro/Ultra), Gemini for business. | 
| Gemini 4 Carbon | Most capable reasoning model, built for complex software engineering, deep math, science, and long-horizon agents. | $2.00 / $10.00 | $4.00 / $20.00 | $0.10 through Nov 30, then $0.20 | No | Gemini API in Google AI Studio, Gemini for business, Gemini app (Ultra), Antigravity (Ultra), Search AI Mode. | 
| Gemini 4 Argon | Efficient frontier reasoning model delivering intelligence with faster responses and lower token spend per turn. | $2.00 / $10.00 | $4.00 / $20.00 | $0.10 through Nov 30, then $0.20 | No | Gemini API, paid tier only | 
| Gemini 4 Hydrogen | Speed and efficiency for everyday coding and agentic workflows. | $1.50 / $7.50 | $1.50 / $7.50 | $0.075 | Yes | Gemini API, free and paid tiers | 

Routing decisions depend on task complexity and execution frequency. Teams managing high-frequency production pipelines can route baseline agent tasks to Hydrogen, while shifting deeper reasoning and analytical tasks to Neon. Operators tracking frontier spending can review our breakdown of Gemini 4 Argon pricing alongside our guide to Gemini 4 Neon pricing.

Google says Carbon uses more tokens than Argon to explore thoroughly, and describes Neon as delivering frontier-level reasoning for high-frequency workflows faster and more cost efficiently. In Google Antigravity, Google states that Neon can operate as a standalone agent or serve as an Orchestrator directing parallel Hydrogen subagents.

## Price and Limits

Google prices Gemini 4 Neon on four tiers: Standard, Batch, Flex and Priority. Standard is $3.00 per 1 million input tokens, $15.00 per 1 million output tokens and $0.15 per 1 million cached input tokens. Google calls the cached rate a 95% discount. Batch and Flex are $1.50 input, $7.50 output and $0.075 cached. Priority is $5.40 input, $27.00 output and $0.27 cached.

Neon supports implicit caching only, and explicit caching is not available. Every token is counted and billed, including thinking tokens, tool declarations and system instructions. Grounding with Google Search on the Standard and Priority tiers includes 5,000 free requests a month, shared across all Gemini 3 and newer models. After that Google charges $14 per 1,000 requests, up to 1,500 requests per day. Grounding with Google Maps shares a 5,000-request monthly free pool before charging $14 per 1,000 search queries. Google notes on its pricing page that paid-tier data is not used to improve Google products.

Our Gemini 4 Neon pricing guide models the same token volumes. Neon costs 1.5 times Carbon and Argon through November 30, 2026. From December 1, 2026 it costs 0.75 times their rate, and it costs exactly 2.0 times Hydrogen on every date. That is same-token price math, not a per-task result.

## Who Can Use It Today

Google outlined specific access channels for Gemini 4 Neon in its November 18, 2026 launch announcement across developers, enterprise customers, individual consumers, and specialized security teams. Review our walkthrough on how to use Google Antigravity to configure agent workflows.

| User Group | Supported Platform | Access Method | Operational Details | 
|---|---|---|---|
| Software Developers | Gemini API in Google AI Studio and Android Studio | gemini-4-neon model string on paid API tier | Paid API billing required; Pro and Ultra subscribers can also test inside AI Studio. | 
| Enterprise Organizations | Gemini for business | API endpoints and model drop-down selector | Production deployment access via API or model drop-down selector. | 
| Pro and Ultra Subscribers | Google Antigravity | Antigravity interface | Paid Google AI Pro and Ultra subscribers can use Neon here, and Google also lists Antigravity for enterprises. | 
| Individual Subscribers | Gemini app | Model drop-down selector | Requires an active personal Google AI Pro or Google AI Ultra subscription tier. | 
| Cybersecurity Defenders | Google Fairwind Program | Account request via Google Cloud account team | Eligible Fairwind Program members receive Neon for cyber access over rolling deployments. | 

Google's launch post lists paid API access, Pro and Ultra subscriptions, Gemini for business and Fairwind membership as the routes. The Gemini API has no Free tier for Neon.

## What Google Reports

Google published performance evaluations for Gemini 4 Neon inside the Google DeepMind evals methodology release on November 18, 2026. The published scores compare Gemini 4 Neon against Claude Opus 5.5, developed by Anthropic, whose pricing page lists its rates.

| Benchmark Evaluation | Gemini 4 Neon Score | Claude Opus 5.5 Score | Reported Source Label | 
|---|---|---|---|
| Artificial Analysis Intelligence Index v5.0.1 | 61 (#3 overall at xhigh thinking) | 59 | Google-reported (methodology table) | 
| Vals Index v2.2 | 70.9% (#2 on that index) | 70.1% | Google-reported (methodology table) | 
| Terminal-bench 4.1 (cheating adjusted accuracy) | 65.2% | 65.8% (Opus leads) | Google-reported (methodology table) | 
| FrontierCode 2.0 (main score, Neon high thinking) | 46.2% | 47.3% (Opus leads) | Google-reported (methodology table) | 
| SWE-sweep (provider harness) | 39.0% | 10.1% | Google-reported (methodology table) | 
| SWE-sweep (mini-SWE harness) | 12.8% | 3.6% | Google-reported (methodology table) | 
| Terminal-Bench-Science 0.1 | 70.5% | 63.3% | Google-reported (methodology table) | 
| HLE-Diamond | 71.3% | 67.9% | Google-reported (methodology table) | 
| Chartography | 73.5% | 66.3% | Google-reported (methodology table) | 

According to Google, Neon at default high thinking provides a 32% lower cost per task than Gemini 4 Carbon, though Google's launch post does not state the pricing basis for that calculation. Google also reports that at high thinking Neon costs roughly 50% to 80% less per task than top competing frontier models, and that Neon at xhigh thinking costs 36% or more less per task than competing models at their highest setting.

For specialized cybersecurity workloads, Google reports Neon for cyber scores of 90.5% on SEC-bench Pro, 70.7% on CWE-bench v2 (a 3.4% uplift over Carbon), and 51.26% on WebExploitBench v1.2 at half the cost of competing large models. Google reports these cybersecurity figures. Its methodology page says SEC-bench Pro and ExploitGym were self-computed, CWE-bench v2 comes from the benchmark's public leaderboard, and WebExploitBench v1.2 was validated by AgentCyberRange. These figures carry no security, vulnerability-triage, or defensive hardening advice.

## Who Should Skip Neon

Neon is not the right choice for every workflow. If your team relies on free API access for prototyping or testing, skip it. Neon has no free tier on the Gemini API. Choose Gemini 4 Hydrogen, which keeps a free API tier.

Tight token budgets point the same way. Hydrogen costs $1.50 per 1M input tokens and $7.50 per 1M output tokens, exactly half of Neon's rate. For everyday extraction, basic classification and routine script generation, Hydrogen gives you a lower bill.

If your work centers on frontier science, deep mathematical proofs or autonomous agents that need the deepest exploration, select Gemini 4 Carbon. In its Carbon availability post, Google says Carbon uses more tokens than Argon to explore thoroughly. Teams that want efficient frontier reasoning can compare Gemini 4 Argon explained.

## What Would Change Our Answer

Our advice rests on Google's published rates and current platform availability. On December 1, 2026, Carbon and Argon rise from $2.00 input and $10.00 output to $4.00 and $20.00 per million tokens. Neon's cost then falls from 1.5 times Carbon to 0.75 times Carbon.

Our routing advice would change if Google alters Neon's published Standard rates, adds a free API tier or expands the access list. It would also change if independent per-task cost data shows Carbon finishing complex tasks in fewer tokens than Neon. Evidence that contradicts Google's reported benchmarks on Artificial Analysis would do the same.

## Our Takeaway

Gemini 4 Neon fits production systems that need frontier-level intelligence for complex software engineering and high-frequency analytical workflows. Google reports that Neon scores 61 on the Artificial Analysis Intelligence Index at xhigh thinking and costs 32% less per task than Carbon at default high thinking. Per the same-token price math in our Gemini 4 Neon pricing guide, Neon costs 25% less per token than Carbon from December 1, 2026.

If you already run workloads on the Gemini API, keep Gemini 4 Carbon for your hardest long-horizon projects. Run high-frequency everyday execution on Gemini 4 Hydrogen. Send complex analytical and coding work to Gemini 4 Neon.

To begin, open Google AI Studio, set up your paid API project and test gemini-4-neon at the default high thinking level.

## Frequently Asked Questions

- Gemini 4 Neon is a Google model released on November 18, 2026 for complex software engineering, analytical reasoning and enterprise workflows. Its identifier is gemini-4-neon. It has a 1-million-token unified context window and sits between Gemini 4 Carbon and Gemini 4 Hydrogen in Google's model family.
- Gemini 4 Neon costs $3.00 per 1 million input tokens and $15.00 per 1 million output tokens on the paid Standard API tier. Cached input is $0.15 per million. Batch and Flex are $1.50 input, $7.50 output and $0.075 cached. Priority is $5.40 input, $27.00 output and $0.27 cached.
- No. Gemini 4 Neon is not available on the Gemini API free tier. Every token is counted and billed, including thinking tokens and grounding declarations. In the Gemini app it needs a paid Google AI Pro or Ultra subscription.
- Developers can use Neon through the paid Gemini API in Google AI Studio and Android Studio. Enterprises can use Gemini for business and Google Antigravity. Pro and Ultra subscribers can use it in the Gemini app, and eligible cybersecurity defenders get access through Google's Fairwind Program.
- Google reports 61 on the Artificial Analysis Intelligence Index v5.0.1 at xhigh thinking and 70.9% on the Vals Index v2.2. It reports 65.2% on Terminal-bench 4.1, 46.2% on FrontierCode 2.0 at high thinking and 39.0% on SWE-sweep with the provider harness. All figures are Google's.

### Architect Your Gemini 4 Neon Integration

Deploying cost-effective agentic models requires reliable context caching, rate-limit management, and secure workflow routing. Book a consultation with Layer3Labs to map model architectures and optimize token spending.

Book a Consultation
