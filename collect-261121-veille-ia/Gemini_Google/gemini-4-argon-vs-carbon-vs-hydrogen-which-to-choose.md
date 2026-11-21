---
id: collect-261121-veille-ia/gemini-google/gemini-4-argon-vs-carbon-vs-hydrogen-which-to-choose
title: "Gemini 4 Argon vs Carbon vs Hydrogen: Which to Choose?"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["carbon", "gemini", "model", "hydrogen", "argon", "google", "november", "same"]
source: https://runelab.ai/en/blog/post/which-gemini-4-model-to-choose
source_anchor: ""
source_lines: [1, 123]
section: "Gemini_Google"
---
**In short:** Google's model selection guide recommends Gemini 4 Hydrogen as the default, Carbon for the hardest problems and Argon for frontier reasoning with lower token spend than Carbon. Google's Carbon announcement, for its part, says Argon is the ideal choice for the majority of software engineering and knowledge work. Hydrogen is the cheapest of the three and the only one on the free API tier. The introductory price of Argon and Carbon ends on 30 November 2026 and doubles from 1 December.

The three Gemini 4 models share the same foundation. According to Google's model selection guide, Argon, Carbon and Hydrogen have the same 1 million-token context window, long output of up to 1 million tokens (long decode), and the same built-in tools: Google Search, code execution, function calling, computer use and structured outputs. They differ in how much they think per turn, how fast they respond, what they cost and who can use them. That is why the Argon vs Carbon vs Hydrogen decision, and which model you should choose for a given job, depends less on raw model strength than on how hard the task is, your budget and which Gemini surface your account can reach. On November 18 Google added a fourth model, Neon, which sits between Hydrogen and Carbon, and we cover it in what is Gemini 4 Neon.

The answer depends on how hard the task is and on which Google source you read. The model selection guide names Hydrogen as the default and recommends starting most new projects with it. It recommends Carbon for the hardest problems and Argon for frontier reasoning with lower token spend than Carbon. Google's Gemini 4 Carbon announcement, by contrast, presents Argon as the ideal choice for the majority of software engineering and knowledge work.

Hydrogen is the recommended model for everyday coding, interactive web apps, tool-using agents, multimodal document workflows and parallel subagents. Google's What's new in Gemini 4 Hydrogen page lists it as the recommended upgrade from Gemini 3.8 Flash and earlier Gemini 3 and 2.5 models. If you need the free API tier, Hydrogen is also the only option of the three.

According to the model selection guide, Argon is for work where Hydrogen is not enough but you do not need Carbon's extra verification. The guide says Argon responds faster than Carbon and spends fewer tokens per turn. Complex software engineering, math and long-horizon knowledge work are its territory.

Carbon comes in where the task reaches the limit of the model: system architecture, debugging across many files, dense multimodal analysis, formal proofs and security audits. Google also suggests switching to Carbon when a faster model gets stuck. Because Carbon spends more thinking tokens per turn, its cost per task can be higher even at the same per-token price as Argon.

You do not have to commit to one model either. Because the three share the same request schema, context window and tools, Google recommends routing each step to the model that fits it. Hard decisions can go to Carbon, and code edits and test runs to Hydrogen.

| Gemini 4 models compared (Google AI Studio docs and Google announcements, 18 November 2026) |  |  |  | 
|---|---|---|---|
| Item | Gemini 4 Hydrogen | Gemini 4 Argon | Gemini 4 Carbon | 
|---|---|---|---|
| Model ID | `gemini-4-hydrogen` | `gemini-4-argon` | `gemini-4-carbon` | 
| Google's description | Fast and efficient for everyday coding and agentic workflows | Efficient frontier reasoning model | Strongest Gemini 4 model, for the hardest tasks | 
| Generally available | 12 November 2026 | 13 October 2026 | 10 November 2026 | 
| Context window | 1,048,576 tokens | 1,048,576 tokens | 1,048,576 tokens | 
| Standard price, input / output (USD) | 1.50 / 7.50 | 2 / 10 through 30 November, 4 / 20 from 1 December | Same as Argon | 
| Free API tier | Yes | No | No | 
| Thinking levels | `low` ,`medium` ,`high` (default),`xhigh` | `low` ,`medium` (default),`high` | `low` ,`medium` (default),`high` | 
| Gemini app | All users, free users through auto model selection | Google AI Pro and Ultra | Google AI Ultra | 
| Antigravity | Paid Google AI Pro and Ultra | Google AI Ultra | Google AI Ultra | 

All three are large language models that work with text and multimodal input, but Google positions each one for a different workload. We explain what a large language model is and how it works in what is an LLM.

Hydrogen is for everyday work where speed and efficiency matter. In its Gemini 4 Hydrogen announcement, Google presents the model for everyday coding and agentic workflows and says its price is flat across the whole 1 million-token context. According to Google DeepMind's Hydrogen model card, Hydrogen is adapted from Gemini 4 Carbon.

Google reports a 51 to 50 lead on the Artificial Analysis index for Hydrogen over Argon, at Hydrogen's default high level. On Artificial Analysis's own page, Hydrogen reaches 54 at xhigh, its highest thinking level. We cover the model in detail in what is Gemini 4 Hydrogen.

Argon is Google's efficient Gemini 4 frontier reasoning model, built to solve hard work with fewer tokens than Carbon. According to Argon's first announcement, the model first went to cyber defenders in the Fairwind Program on 30 September 2026. In the same announcement, Google raised Argon's output limit from 64K to 1 million tokens. With Argon's general availability announcement, the model reached developers, enterprises and Google AI Pro and Ultra subscribers on 13 October 2026.

Carbon is Google's strongest Gemini 4 model, positioned for cybersecurity and long-horizon agentic work. It first went to cyber defenders in the Fairwind Program, then opened to developers, enterprises, scientists and consumers on 10 November 2026. Google says Carbon uses more tokens than Argon to explore more thoroughly, reason more deeply and run longer verification loops. We cover Carbon's code security and prompt injection side in our Gemini 4 Carbon guide.

As of 18 November 2026, Hydrogen is the cheapest model on the Gemini API at $1.50 per million input tokens and $7.50 per million output tokens, while Argon and Carbon are on an introductory price of $2 input and $10 output. According to Google's Gemini Developer API pricing page, that introductory price runs through 30 November 2026, and from 1 December 2026 every Argon and Carbon price doubles. No end date is given for Hydrogen's price.

| Gemini 4 API prices, USD per million tokens (Google AI Studio pricing page, 18 November 2026) |  |  |  | 
|---|---|---|---|
| Model | Tier (input / output / cached) | Through 30 November 2026 | From 1 December 2026 | 
|---|---|---|---|
| Argon and Carbon | Standard | 2 / 10 / 0.10 | 4 / 20 / 0.20 | 
| Argon and Carbon | Batch and Flex | 1 / 5 / 0.05 | 2 / 10 / 0.10 | 
| Argon and Carbon | Priority (input / output) | 3.60 / 18 | 7.20 / 36 | 
| Hydrogen | Standard | 1.50 / 7.50 / 0.075 | No end date given | 
| Hydrogen | Batch and Flex | 0.75 / 3.75 / 0.0375 | No end date given | 
| Hydrogen | Priority (input / output) | 2.70 / 13.50 | No end date given | 

The Batch and Flex tiers cost half the Standard price, and the Priority tier 1.8 times as much. Hydrogen also has a free tier, while Argon and Carbon are available on the paid tier only.

Because your bill depends on how many tokens the model spends as much as on the price per token. According to the pricing page, thinking tokens are billed at the output price. When grounding is used, such as Grounding with Google Search, the tools' internal system instructions and function declarations are billed too. We explain what a token is, and why Turkish text uses more of them, in what is a token in AI.

A simple sum: 1 million input plus 1 million output tokens costs $9 on Hydrogen, and $12 on Argon or Carbon through 30 November, then $24 from 1 December. That sum assumes the same token count, but Argon and Carbon do not finish the same job with the same number of tokens. Google says Carbon spends more thinking tokens per turn, which raises token use and latency.

Google's advice is to test your tasks at every thinking level and pick the lowest level that meets your quality bar. Argon and Carbon support low, medium and high, and both default to medium. Hydrogen defaults to high and adds an xhigh level for hard, multi-step problems.

All three support only implicit caching, and explicit caching is not supported. According to Google's What's new in Gemini 4 Carbon page, implicit caching is on by default for prompts of at least 4,096 tokens and takes 95 percent off cached input tokens on a hit. Putting shared system instructions, tool definitions and reference documents at the start of the prompt raises the hit rate. RuneLab's Building Applications with LLM APIs course teaches how to build an LLM app with retries, rate limits and fallbacks that keeps costs in check, using the OpenAI, Claude and Gemini APIs.

Because different rules apply to your data on the free and paid tiers. The pricing page shows the "used to improve our products" row as yes on Hydrogen's free tier and no on the paid tier. Under Google's Gemini API Additional Terms of Service, content on the free tier is used to improve Google's products and may be read by human reviewers. Google says not to send personal or confidential data to this tier.

On the standard paid tiers, Google does not use your prompts and responses to improve its products. Argon and Hydrogen also have an invite-only Contributor tier. It costs less ($1 input and $5 output on Argon, $0.75 and $3.75 on Hydrogen), but data on that tier is used to improve Google's products.

Access depends on the model and on your subscription. TÃ¼rkiye is among the supported regions of the Gemini API and AI Studio. App and subscription features may roll out to your account gradually. Google says the model is rolling out globally in its 13 October Argon announcement and in the Hydrogen announcement. The Carbon announcement says the model is rolling out to Google AI Ultra subscribers.

- Free Gemini app and Google Search: Hydrogen reaches free users through auto model selection when it is the right model for the prompt, and is rolling out in Google Search for all users.
- Google AI Pro: Argon in the Gemini app and AI Mode in Search, plus Hydrogen in Antigravity.
- Google AI Ultra: Argon and Carbon in the Gemini app, AI Mode and Antigravity. Hydrogen in Antigravity too.
- Gemini API: Hydrogen on the free and paid tiers, Argon and Carbon on the paid tier only.
- Enterprises: all three announcements point businesses to Gemini for business. The Enterprise tier on the pricing page runs on Gemini Enterprise Agent Platform and offers options such as provisioned throughput, volume-based discounts and advanced security and compliance. Prices on that platform may differ from the AI Studio list.
- Special programs: cyber defenders can apply to the Fairwind Program for Argon without cyber guardrails and for Carbon for cyber. Scientists can apply for 6 months of free access to Argon and Carbon on Antigravity's highest usage tier, subject to eligibility, capacity and selection.

RuneLab's Google Cloud Vertex AI and Gemini Enterprise Agent Platform course covers building enterprise agents with Gemini on Google Cloud, with Vertex AI Search grounding, context caching and safety controls.

Yes, and Google recommends it. Along with Hydrogen, Google brought Orchestrator to Antigravity. Instead of running a job on one model from start to finish, Orchestrator switches between models such as Carbon and Hydrogen within the same session. On Google's own Terminal-Bench 4.0 run, Orchestrator reached near-Carbon task success while using 24 percent less quota on average and finishing 19 percent faster.

For parallel agent workflows, Google positions Carbon as the coordinator and Hydrogen as the worker and verifier agents that write and test changes. We explain this setup, where a language model picks tools and works step by step, in what is an AI agent.

You can set up similar routing in your own application with an LLM gateway. RuneLab's RuneGate serves OpenAI and Google Gemini models from one OpenAI-compatible endpoint, routes each request by difficulty to a model in the light, mid or power tier, logs which model each request went to and what it cost, and gives each team a monthly budget.

Because Gemini Decision 1 is not a chat model that writes text or code. According to Google's Gemini Decisions API announcement, the model scores closed-set questions, such as yes or no, choice and rubric scoring, in a single pass. Since it writes no answer text, no output tokens are billed. The uses Google lists include tool selection, policy checks and model routing. So Decision 1 does not replace Argon, Carbon or Hydrogen, but it can be useful in the step that decides which of them a request goes to.

The first thing to check is where the data goes. Argon, Carbon and Hydrogen are services hosted by Google, there are no downloadable weights, and every prompt you send to the Gemini API goes to Google.

Under the Additional Terms, on the paid tier Google processes prompts and responses under its Data Processing Addendum for products where Google is a data processor. On the paid tier, prompts are logged for a limited period for abuse monitoring and may be stored transiently or cached in any country in which Google or its agents maintain facilities. According to Google's Gemini API release notes, since 26 October 2026 new paid projects also store generateContent requests by default. You can turn this off per request with `store=false` or in the logging settings in AI Studio. According to the Data logging and sharing page, these logs are kept for at most 55 days by default.

Running in the cloud is not a KVKK breach by itself, and running on premises does not make you compliant by itself. Before you test a job that involves personal data, settle these questions with your legal team: which tier will you use, will prompts carry personal data, what is the legal basis for processing, and how will the cross-border transfer of the data be assessed? This section is not legal advice, and you will find more in our KVKK generative AI guide. We explain which work belongs in the cloud and which in-house in local LLM vs ChatGPT. Where data must never leave the company, the model has to run in-house.

RuneBox is a ready on-premise LLM installation on your company network. Models, RAG and agent flows run on an NVIDIA GPU box in a closed network with no outside connection, and questions, documents and answers never leave. The box runs the open models your organization chooses, not Gemini 4. Because data is processed inside the company boundary, your BDDK and KVKK assessments become easier. Compliance is your organization's own assessment.

All three share a 1,048,576-token context window, long output of up to 1 million tokens and the same built-in tools. Hydrogen is the fast, lowest-priced model for everyday coding and agent work, Argon is the efficient frontier model that spends fewer tokens than Carbon, and Carbon is the strongest model, spending more thinking tokens per turn than Argon. They differ in thinking depth, speed, price and access.

Google's model selection guide recommends Hydrogen as the default, Carbon for the hardest problems and Argon for frontier reasoning with lower token spend. Google's Carbon announcement says Argon is the ideal choice for the majority of software engineering and knowledge work. Make the final call by testing on your own tasks.

On the Gemini API Standard tier, per million tokens, Hydrogen is $1.50 input and $7.50 output, with no end date given for that price. Argon and Carbon's $2 input and $10 output is an introductory price that runs through 30 November 2026. From 1 December 2026, the Argon and Carbon price doubles to $4 input and $20 output.

In the free Gemini app, Hydrogen arrives through auto model selection when it is the right model for the prompt. On the Gemini API free tier, Hydrogen is the only one of the three, and under Google's terms content on that tier is used to improve its products. Argon and Carbon are not on the free tier.

Google presents Carbon as its strongest Gemini 4 model, but in the Carbon announcement it also says Argon is the ideal choice for the majority of software engineering and knowledge work. Because Carbon spends more tokens per turn, it can cost more per task at the same per-token price. There is no single winner, and the choice depends on how hard the task is.

Yes. TÃ¼rkiye is among the supported regions of the Gemini API and AI Studio. Gemini app and subscription features may roll out to your account gradually.

No. Argon, Carbon and Hydrogen are services hosted by Google, with no downloadable weights. For work where data must not leave the company, look at open-weight models that run in-house.

- Google AI Studio, Gemini Developer API pricing: Standard, Batch, Flex, Priority and Contributor prices of the Gemini 4 models, the end of the introductory price, billed tokens, data use by tier and the Enterprise tier
- Google AI Studio, Choosing a model: the shared context, long output and built-in tools of the three models, model choice, step-by-step routing and implicit caching advice
- Google AI Studio, What's new in Gemini 4 Argon: choosing between Argon and Carbon, and thinking levels
- Google AI Studio, What's new in Gemini 4 Hydrogen: Hydrogen's use cases, upgrade advice, free tier and thinking levels
- Google AI Studio, What's new in Gemini 4 Carbon: Carbon's token use and implicit caching
- Google AI Studio, Gemini 4 Argon model page, Gemini 4 Carbon model page and Gemini 4 Hydrogen model page: model IDs and context window
- Google AI Studio, Release notes: Argon's general availability and generateContent requests stored by default
- Google AI Studio, Available regions for Google AI Studio and Gemini API: supported countries
- Google AI Studio, Gemini API Additional Terms of Service: data use on the free and paid tiers, human review, abuse-monitoring logs and where data is stored
- Google AI Studio, Data logging and sharing: default logging and retention
- Google, Introducing Gemini 4 Argon: first access through the Fairwind Program and the 1 million-token output limit
- Google, Gemini 4 Argon is now available for everyone: general availability, surfaces and special programs
- Google, Gemini 4 Carbon is now available: general availability, comparison with Argon, surfaces, Fairwind and the scientist program
- Google, Introducing Gemini 4 Hydrogen: the Artificial Analysis comparison, Orchestrator, multi-model workflows and surfaces
- Google DeepMind, Gemini 4 Hydrogen Model Card: Hydrogen adapted from Carbon
- Google, Gemini Decisions API: what Gemini Decision 1 does, its uses and its billing
- Artificial Analysis, Gemini 4 Hydrogen analysis: Intelligence Index scores by thinking level
