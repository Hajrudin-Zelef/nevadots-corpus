---
id: collect-261121-veille-ia/gpt-openai/openai-devday-2026-announcements-prices-dates-signalstack
title: "OpenAI DevDay 2026: Announcements, Prices, Dates | SignalStack"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["gpt-6", "openai", "chatgpt", "openaias", "agents", "codex", "enterprise", "devday"]
source: https://www.signalstack.news/blog/openai-devday-2026-announcements/
source_anchor: ""
source_lines: [1, 103]
section: "GPT_OpenAI"
---
AI-generated illustration

OpenAI held DevDay 2026 on September 29 at Fort Mason in San Francisco; Sam Altman gave the 10 a.m. Pacific keynote. OpenAIâs recap lists more than 20 announcements. Below, each is sorted into available now, preview or promised, with OpenAIâs product pages, API docs and help center as the record.

## TL;DR

- **Three headline launches.** Dots, always-on agents powered by GPT-6 Astra; GPT-6.1 Sol, priced in the API at one-fifth of GPT-6 Astraâs standard input and output rates; and Ultrafast for GPT-6 Astra, a speed tier that costs six times Astraâs standard price in the API.
- **New $500 plan, smaller $200 plan.** Pro 500 costs $500 a month and gives 25 times the Plus usage allowance. Pro 200 reopened to new subscribers with a lower allowance.
- **Rumors, sorted.** The always-on agent shipped as dots, not the rumored Aeon, and the $500 tier shipped as Pro 500. No hardware device and no GPT-6 Cyber model appeared in OpenAIâs recap.

## What OpenAI announced

**Models.** GPT-6.1 Sol (`gpt-6.1-sol` in the API) arrived one week after GPT-6 Sol, which the changelog dates to September 22. OpenAI says it ânearly matches GPT-6 Astraâs intelligenceâ on agentic coding, computer use and professional work. It is live in the API now. In ChatGPT it runs in Work and Codex on Plus, Pro, Business, Enterprise and Edu plans, but not yet in regular Chat. The changelog adds multi-agent delegation in beta.

**Speed.** Ultrafast generates tokens up to 8 times faster in Codex (OpenAI cites 300 tokens per second) and up to 6 times faster in the API. GPT-6 Astra Ultrafast is live in the API and in ChatGPT Work and Codex on Pro 500 and Enterprise plans. GPT-6.1 Sol Ultrafast is listed as coming soon.

**Agents.** Dots run on their own cloud computer and browser, can reach more than 4,000 apps through plugins, and can be messaged in ChatGPT, Slack and Teams. For developers, the Agents API, in public beta since September 10, gained computer use in an OpenAI-hosted browser. With Amazon, OpenAI launched Bedrock Managed Agents in preview, which AWS says runs OpenAI agents entirely inside AWS on the Codex harness and Bedrock AgentCore. A new Decisions API uses OpenAIâs Luna model to pick answers from a fixed, developer-defined set.

**Codex.** Codex now runs in the cloud, so tasks can start from a phone. The CLI gained voice control and an /agents view, the desktop app gained code review for GitHub and GitLab, and Codex Security Cloud scans whole GitHub repositories and prepares fixes.

**ChatGPT for teams.** ChatGPT Space is a shared workspace for teammates, ChatGPT and dots, and Pages are documents people and agents edit together. Team tasks and @ChatGPT in Slack and Microsoft Teams are live on Business and Enterprise plans; collaborative slides arrive âin the coming weeksâ; and a Meetings plugin is in beta on the macOS desktop app for Pro and Business users. Developers get plugin extensions (sidebar panels and file viewers inside ChatGPT), a Plugin Creator and support for the proposed MCP Events specification.

**Accounts and enterprise.** Through Sign in with ChatGPT, Plus and Pro subscribers can draw on their plan allowance in 16 partner tools, including Cognitionâs Devin, Notion and Vercel. Eligible enterprise customers can express interest in OpenAI Marketplace, which applies part of an existing OpenAI commitment to partner products, with 32 initial partners. Private Intelligence pairs zero data retention with automated safety review that OpenAI staff cannot read. A Private Inference preview is due âthis fall.â

## What OpenAIâs pages say about price and availability

OpenAIâs API pricing page and help center put numbers on the launches. API prices below are per million tokens for prompts up to 272,000 input tokens; longer prompts cost more.

| Launch | Price (OpenAIâs pages) | Availability, in OpenAIâs words | 
|---|---|---|
| GPT-6.1 Sol, API | $2 input, $0.10 cached input, $10 output | Available now in the API | 
| GPT-6.1 Sol, ChatGPT | Via listed plans | ChatGPT Work and Codex for Plus, Pro, Business, Enterprise, Edu; ânot yet available in Chatâ | 
| GPT-6 Astra, API (reference) | $10 input, $1 cached input, $50 output | Released Sept 3 | 
| GPT-6 Astra Ultrafast, API | $60 input, $6 cached input, $300 output | Available to all API users at low rate limits | 
| GPT-6.1 Sol Ultrafast | Not published | âComing soonâ (recap); âin the coming daysâ (Sol page) | 
| Pro 500 | $500 a month | âAvailable nowâ; 25x the Plus allowance; only Pro plan with Ultrafast | 
| Pro 200 | $200 a month | Reopened to new subscribers with a lower allowance | 
| Dots | First dot included at no extra cost | Rolling out to Pro and Business Premium in eligible markets; Enterprise, Edu and Healthcare beta if an admin turns it on | 
| Decisions API | Not published | Limited preview; broad release âin the coming daysâ | 
| Collaborative slides | Via listed plans | Pro, Business, Enterprise âin the coming weeksâ | 
| Private Inference | Not published | Preview âcoming this fallâ | 

A few details matter for developers budgeting against these numbers:

- **Where Ultrafast runs.** The Ultrafast guide says it supports US data residency and global processing only, not EU or other regional endpoints. Default Ultrafast limits for GPT-6 Astra are 500,000 tokens per minute for usage tiers 1 to 3, 1 million for tier 4 and 5 million for tier 5. Requests set`service_tier` to`ultrafast` .
- **Other tiers.** GPT-6 Astra output costs $25 per million tokens on Batch or Flex and $100 on Fast (the pricing page says Priority processing was renamed Fast on July 30). Regional data-residency endpoints cost 10% more for models released since March 5, 2026.
- **Caching.** GPT-6.1 Solâs $0.10 cached-input price is half of GPT-6 Solâs $0.20, per OpenAI.
- **Benchmarks are OpenAIâs own.** It reports GPT-6.1 Sol averaging $5.47 per task on Terminal-Bench Science 0.1 at maximum effort, against $23.80 for GPT-6 Astra, which still scored highest (68.1%).

### Pro plans and dots, in detail

OpenAIâs help center lists three Pro plans: Pro 100 ($100), Pro 200 ($200) and Pro 500 ($500). Pro 200 subscribers who qualify for grandfathering keep their previous allowance through October 29, 2026, at the same price. Engadget and The Next Web report that in Codex and ChatGPT Work the new Pro 200 allowance drops to 10 times Plus usage, down from 20 times, and GPT-6 Pro chat messages to 100 a week from 200. OpenAIâs help page confirms a lower allowance but does not give those multiples.

For dots, OpenAI says conversations with a dot do not count toward ChatGPT usage limits, but Codex or ChatGPT Work tasks the dot starts do. A dot must be created on desktop. OpenAIâs dots page lists texting as coming soon; the help center describes a limited US beta for Pro users where available. The Next Web, citing an OpenAI statement to the press, and SiliconANGLE report that dots are not yet available to Pro subscribers in the European Economic Area, Switzerland or the UK. OpenAI is also piloting âspecialist dotsâ with enterprises and is working with Microsoft to integrate them with Agent 365.

## Rumored before DevDay, and one model OpenAI withheld

- **An always-on agent: happened, under another name.** The Verge wrote on September 28 that rumors pointed to an agent called Aeon. OpenAI shipped the always-on agent as dots.
- **A $500 plan: happened, as Pro 500.** BleepingComputer reported on September 25 that an unannounced âPro Maxâ plan at $500 a month had surfaced in ChatGPTâs subscription screens. OpenAI said in August that Ultrafast for GPT-5.6 Sol is powered by Cerebras; its DevDay pages do not say what runs GPT-6 Astra Ultrafast.
- **A hardware device: didnât happen.** WinCentral had reported that OpenAIâs first hardware device could debut at the event. At DevDay, Altman told CNBC that âsomething thatâs worth waiting forâ could come in AI hardware, adding: âI am not worried about being first with new hardware.â
- **GPT-6 Cyber: not in the recap.** Fortune reported on September 24 that OpenAI planned to preview a GPT-6 Cyber model âin the coming weeks.â The recap lists no such model, though Codex Security Cloud includes models from OpenAIâs Daybreak Blue program.
- **GPT-6.1 Astra: withheld (an OpenAI decision, not a rumor).** On September 28, the day before DevDay, OpenAI said it would not release GPT-6.1 Astra because it did not meet the companyâs safety standards, CNBC reported. Altman told CNBC at the event that this fell in the ânormal course category.â Our report on OpenAIâs agent incidents and model review covers that decision.

## Background for investors

OpenAI is privately held; there is no OpenAI stock to trade. CNBC confirmed, citing a person familiar with the talks, that OpenAI is in early discussions on a funding round that could raise around $30 billion, first reported by Bloomberg. It raised $122 billion at an $852 billion valuation in March, CNBC noted. Altman told CNBC he has no particular IPO timeline, and CFO Sarah Friar declined to comment on the talks.

A keynote slide put ChatGPT at 1.2 billion weekly users, and Axios and The Verge framed dots as a response to Metaâs Muse agent. More in our AI and semiconductors coverage.

## What could go right / What could go wrong

**What could go right**

- Near-Astra results at one-fifth of Astraâs API price, if they hold outside OpenAIâs own tests, would lower the cost of running coding and computer-use agents.
- Dots, Space and plugin extensions give developers a way to ship inside a product with 1.2 billion weekly users.

**What could go wrong**

- Several launches are previews or âcoming soon,â and dots exclude some regions, so dates and scope may change.
- Ultrafastâs six-times price and lack of EU or other regional endpoints narrow where it fits.
- Agents with their own computers raise the same control questions behind OpenAIâs recent incident disclosures. Engadgetâs live blog called a dots demo, in which the agent was slow to respond, the dayâs first failed live demo.

## FAQ

Q What did OpenAI announce at DevDay 2026?

A More than 20 launches on September 29, 2026, led by dots (always-on agents powered by GPT-6 Astra), the GPT-6.1 Sol model, the Ultrafast speed tier and a $500-a-month Pro 500 plan, plus Codex, API, plugin and team features.

Q How much does GPT-6.1 Sol cost in the API?

A Standard API pricing is $2 input, $0.10 cached input and $10 output per million tokens for prompts up to 272,000 tokens. That is one-fifth of GPT-6 Astra's standard input and output prices.

Q Who can use OpenAI dots and when?

A Dots began rolling out on September 29 to ChatGPT Pro and Business Premium users in eligible markets, with the first dot included at no extra cost. Enterprise, Edu and Healthcare workspaces can try a beta if an admin enables it.

Q What is ChatGPT Pro 500 and what happened to Pro 200?

A Pro 500 costs $500 a month, offers 25 times the Plus allowance and is the only Pro plan with Ultrafast. Pro 200 stays at $200 with a lower allowance for new subscribers; eligible existing subscribers keep their old allowance through October 29, 2026.

Q Did OpenAI announce a hardware device at DevDay 2026?

A No. Altman told CNBC that "something that's worth waiting for" could come in AI hardware, but no device was announced.
