---
id: collect-261121-veille-ia/gpt-openai/openai-devday-2026-every-announcement-with-prices-and-availability
title: "OpenAI DevDay 2026: every announcement, with prices and availability"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["chatgpt", "openai", "codex", "agents", "plan", "price", "astra", "cloud"]
source: https://dev.to/axrisi/openai-devday-2026-every-announcement-with-prices-and-availability-1mbh
source_anchor: ""
source_lines: [1, 140]
section: "GPT_OpenAI"
---
OpenAI DevDay 2026 ran on Tuesday in San Francisco, and the official recap counts "more than 20 major announcements across ChatGPT, Codex, our models, and entirely new forms of working with AI." That is a lot to track from a one-hour keynote. Below is every launch in one table, with who gets it and what it costs, followed by the parts that matter if you write code for a living: the new model's real price, the Decisions API, the Agents API, and Sign in with ChatGPT, which quietly changes who pays for your users' tokens.


- 
**Dots** are always-on agents running on GPT-6 Astra, each with its own cloud computer and access to over 4,000 apps. The first one is included in Pro and Business Premium.
- 
**GPT-6.1 Sol** costs $2 per million input tokens and $10 per million output, the same list price as GPT-6 Sol from a week ago. The real cut is cached input at $0.10, which is 95 % off.
- 
**Artificial Analysis** puts Sol at 52 on its Intelligence Index, one point under Astra, at about 22 % of Astra's cost per task. OpenAI's "near-Astra at a fifth of the price" holds up. Claude Opus 5.5 is still #1 at 58.
- 
**Ultrafast** runs Astra at up to 300 tokens a second for six times the standard price, and ChatGPT's new**Pro 500** plan gets 25 times the Plus allowance.
- 
**Sign in with ChatGPT** lets a user's own ChatGPT plan pay for the OpenAI usage inside your app, starting with 16 partners.

Availability and prices come from OpenAI's recap, the launch posts and the API pricing page as read on launch night. Where a page gives no price or plan, the table says so.

| Launch | What it is | Who gets it | Price / availability | 
|---|---|---|---|
| Dots | Always-on agents on GPT-6 Astra with their own cloud computer, 4,000+ apps via plugins, in ChatGPT, Slack and Teams | Pro and Business Premium in eligible markets; Enterprise, Edu and Healthcare in beta, off by default | First dot included in the plan; texting "coming soon" | 
| Specialist dots in Microsoft Agent 365 | Dots managed through Microsoft's admin tools | Businesses | Rollout details not in the recap | 
| GPT-6.1 Sol | New mid-tier model, "near-Astra intelligence" | API, Plus, Pro, Business, Enterprise, Edu (ChatGPT Work and Codex only; not yet in Chat) | $2 in / $0.10 cached / $10 out per 1M tokens | 
| Ultrafast | Up to 300 tokens/s; "up to 8× faster in Codex and up to 6× in the API" | API; ChatGPT Work and Codex on Pro 500 and Enterprise | Astra Ultrafast $60 in / $300 out per 1M; Sol Ultrafast "coming soon" | 
| Pro 500 | New top ChatGPT plan, 25× the Plus allowance, includes Ultrafast | Consumers | Available now | 
| Pro 200 | The plan whose new sign-ups were paused in September | Consumers | Reopened on stage | 
| Private Intelligence | Zero data retention with Private Safety Processing | Not specified | Private Inference preview "coming this fall" | 
| Codex in the cloud | Codex tasks run remotely | Plus, Pro, Business, Healthcare, Education, Enterprise | Included in plans | 
| Refreshed Codex CLI | Start and steer tasks by voice, new `/agents` view | All plans | Included | 
| Code Review | Reviews GitHub PRs and GitLab MRs from the ChatGPT desktop app, automatic reviews in the cloud | All plans | Included | 
| Codex Security Cloud | Scans whole GitHub repos on demand or on a schedule, prepares fixes in the cloud | Pro, Business, Enterprise, Edu | Included | 
| Decisions API | Luna answers user-defined questions with finite, pre-defined answers | Developers | Limited preview, broad release "in the coming days" | 
| Agents API with computer use | Codex's multi-agent loop, tool search, tool calling and compaction, hosted by OpenAI | API; Codex and ChatGPT Work on Pro 500 and Enterprise | Available | 
| Bedrock Managed Agents, powered by OpenAI | OpenAI agents that run entirely in AWS | AWS customers | Via Amazon Bedrock | 
| Sign in with ChatGPT | Users log into your app with ChatGPT; their plan covers eligible usage | Plus and Pro users, 16 launch partners | Usage counts toward the user's plan limits | 
| Plugin extensions | Sidebar home, interactive panels, file viewers for your plugin | All plans | Included | 
| Plugin Creator | Build plugins, with a redesigned submission flow | Developers | Not specified | 
| Sites can host plugins | Your site ships a plugin | Business and up | Not specified | 
| MCP Events | Support for the proposed MCP Events specification | Developers | Not specified | 
| ChatGPT Space | Shared home for a team, its dots and ChatGPT | Not specified | Not specified | 
| Pages | "A new type of document, built for human and agent collaboration" | Not specified | Not specified | 
| Collaborative slides | Shared decks, export to PowerPoint or Google Slides | Not specified | "In the coming weeks" | 
| Teams and shared tasks | Group workspaces with shared task lists | Not specified | Not specified | 
| @chatgpt in Slack and Teams | Mention ChatGPT inside a chat channel | Slack and Teams workspaces | Not specified | 
| Meetings plugin | Meeting notes; "Audio is deleted once your notes are ready" | macOS desktop app | Beta | 
| Shareable profiles | Public ChatGPT profiles | Not specified | Not specified | 
| OpenAI Marketplace | Enterprises spend part of their OpenAI commitment on partner software | Enterprises, 32 first partners incl. Adobe, Figma, Harvey, CrowdStrike, Baseten | Counts against existing commitments | 
| The reset button | A usage-limit reset pressed live on stage | "Everyone in the world," per the stage | Announced on stage; no official post | 

A dot is an agent that keeps running when you close the tab. The dots launch post says they are "powered by GPT‑6 Astra, they have their own cloud computer, learn from feedback over time, and can work towards your goals 24/7." They connect to over 4,000 apps through plugins, live in ChatGPT, Slack and Teams, take voice calls, and will text you later.

The plan math is simple: "Your first dot is included in your Pro or Business Premium plan at no extra cost," and "Conversations with your dot don't count toward your ChatGPT usage limits." Tibo Sottiaux, who runs Codex, added that this includes the Pro 100 plan. Extra dots and more work per month are described as coming later.

OpenAI's safety post sets the limits: read-only tools for proactive research, auto-review, and "Certain sensitive tasks, such as changing a password, always stay with you." The same page says: "Dots can still make mistakes, so always review consequential work."

The live demo made that last line concrete. Holly Li asked her dot something by voice and got "Checking that now." Then "Still checking." Then, from Holly: "I think maybe Dotty is having kind of a slow morning." Tibo later wrote that the "Live demos suffered from rolling out all the updates at the same time, but were committed to keep doing these live in the future." Within about an hour, Hacker News had two open-source clones: Open Dots and OpenDots.

For companies, specialist dots get their own identity and credentials and are managed through Microsoft Agent 365. Your agent reports to Microsoft's admin console.

The GPT-6.1 Sol post claims it "nearly matches GPT‑6 Astra's intelligence on agentic coding, computer use, and professional work at one-fifth of Astra's standard input and output token prices." The API model id is `gpt-6.1-sol`.

| Model (per 1M tokens) | Input | Cached input | Output | 
|---|---|---|---|
| gpt-6-astra | $10 | — | $50 | 
| gpt-6.1-sol | $2 | $0.10 | $10 | 
| gpt-6-luna | $0.10 | — | $0.50 | 

GPT-6 Sol launched a week earlier at the same $2 / $10. What changed is the cache: $0.10 per million is "95% less than standard input pricing and 50% less than GPT‑6 Sol's cached input pricing." On Hacker News, minimaxir called the cache "the actual big announcement," and dcchambers asked: "GPT-6 Sol released a week ago. Shortest model life ever?" If your agent resends a long system prompt and tool list on every turn, the cached rate is where your bill goes down.

OpenAI's own charts are OpenAI's own charts. The Terminal-Bench Science table puts Sol at $5.47 per task against $23.21 for Opus 5.5 and $23.80 for Astra, and states that "GPT‑6 Astra still achieves the highest score among the models tested at 68.1%." One footnote on the AutomationBench chart admits: "The datapoint for Claude Fable 5.1 understates its actual cost, as it omits the cost of fallbacks, which occurred on ~40% of tasks."

The independent check came from Artificial Analysis on launch night:

| Model (max effort) | Intelligence Index | Cost per Index task | Speed | 
|---|---|---|---|
| Claude Opus 5.5 | 58 (#1) | $5.98 | — | 
| GPT-6 Astra | 53 | $3.26 | 57 tok/s | 
| GPT-6.1 Sol | 52 | $0.72 | 66.8 tok/s | 

$0.72 against $3.26 is 22 %, so "near-Astra at about a fifth" survives an outside test. GPT-6 Sol scored 48 a week ago, so the point release gained four points. The same table shows the top model is still Anthropic's, which OpenAI's page only compares on cost. One HN user, dom96, reported that Sol "matches the performance of Astra on my benchmark" and runs "head to head with Opus 5.5 on both the price and pass rate."

Ultrafast is the speed tier: "up to 8× faster token generation (300 tokens per second) in Codex and up to 6x in the API." It costs six times the standard price, $60 input and $300 output per million tokens for Astra. On stage the line was "an incredible 300 tokens per second. You know what, it's worth it," and the room laughed. The side-by-side demo had two agents build a rocket; the Ultrafast one launched while the regular one was still being built.

In ChatGPT, Ultrafast needs Pro 500, "our highest usage allowance at 25 times the ChatGPT Plus allowance." The same keynote reopened Pro 200, the plan whose new sign-ups OpenAI paused in September because of Astra demand. Sol Ultrafast is listed as "coming soon."

When to pay for it: interactive loops where a human waits on the model, such as pair-programming in Codex. For batch work, Sol at standard speed with a warm cache is roughly thirty times cheaper per output token than Astra Ultrafast.

Hacker News user softwaredoug wrote that "The big takeaway is they announced decision model API for Luna," and I agree. The recap describes it as "focusing Luna's intelligence on a specific set of user-defined questions with finite pre-defined answers," meant to "classify content, route requests, or choose an agent's next action." The demo answered multiple-choice questions "in a fraction of a second," with the presenter adding: "This is not sped up."

The use case is the glue code every agent app already has: pick a tool, pick a sub-agent, label a ticket, flag a message. Today that is usually a big model call with a JSON schema and a retry loop. A small model with a closed answer set is faster and easier to test, because the output space is a list you wrote. It is in limited preview; OpenAI says broad release comes "in the coming days." The recap gives no separate price; Luna's standard list price is $0.10 in and $0.50 out per million tokens.

Codex got the most launches. Codex in the cloud runs tasks remotely on most paid plans. The refreshed CLI lets you "start and steer tasks with your voice" and adds an `/agents` view. On stage the voice part produced "We might have some voice difficulties at the moment, which is pretty unfortunate. But, it's okay." The presenter typed instead, and later sent the cloud version "rewrite the entire back end in rust. Why not?"

Code Review works from the ChatGPT desktop app on GitHub pull requests and GitLab merge requests, with automatic reviews in the cloud, on all plans. Codex Security Cloud can "Scan entire GitHub repositories on demand or on a schedule … prepares fixes in the cloud, even with your laptop closed," on Pro, Business, Enterprise and Edu.

The Agents API "brings Codex's multi-agent capabilities, tool search, tool calling, and context compaction into your application. OpenAI runs the underlying infrastructure." Computer use is part of it. If you maintain your own agent loop with a scheduler, a tool router and a context-trimming step, this is OpenAI offering to host all three.

Bedrock Managed Agents, powered by OpenAI, lets you "build OpenAI agents that run entirely in AWS." For teams whose data and procurement already live in AWS, that removes the usual reason for not trying OpenAI agents.

Private Intelligence covers the compliance side: zero data retention with Private Safety Processing, described as "automated safety reviews without giving OpenAI personnel access to the underlying content." Private Inference arrives as a preview "this fall."

This is the one that touches your wallet. According to OpenAI's help page, a Plus or Pro user can log into a partner app with ChatGPT, and "Eligible OpenAI usage counts toward your plan limits." The recap names 16 launch partners, including Cognition's Devin, Notion, Vercel, T3, OpenClaw and Dactyl. Tibo mentioned "over 60 partners" on X; the official number is 16.

For an app developer the model is new: the user's subscription pays for the inference, so your margin stops depending on how much a heavy user prompts. The trade is dependency. Your login, your users' limits and your cost structure all run through OpenAI's account system. On Hacker News, ramoz put it in six words: "Sign-in with sub accounts seems big."

Next to it: plugin extensions give your app a sidebar home in ChatGPT, Plugin Creator handles building and submission, sites can host plugins, and OpenAI now supports the proposed MCP Events specification. The distribution pitch is ChatGPT's "collective 1.2B weekly users." Romain demonstrated the API driving hardware with a robot duck named Lavender, which quacked on request.

ChatGPT Space is a shared home for a team, its dots and ChatGPT. Pages is "a new type of document, built for human and agent collaboration," with collaborative slides coming in the next few weeks. That puts ChatGPT directly next to Notion and Google Docs, and one of the Sign in with ChatGPT partners is Notion.

The keynote ended with a physical red button. OpenAI joked that Tibo has campaigned to rename the company "the reset company," called it "a low key reset for everybody here, like no confetti cannons," counted down, and said "that's obviously for everyone in the world, not just people here at Dev Day." No official post describes exactly what was reset; reactions on X treated it as a usage-limit reset. Sam Altman closed by saying the future "can be much more like a new Renaissance than a new industrial revolution."

1. 
**Move batch and agent jobs to GPT-6.1 Sol** and structure prompts so the long, stable prefix is cached. At $0.10 per million cached tokens, the system prompt stops being a cost.
2. 
**Try the Decisions API for routing and classification** once it opens up, and compare latency and accuracy against your current big-model-plus-schema call.
3. 
**Read the Sign in with ChatGPT terms** if you ship a consumer app on OpenAI. Letting users bring their own plan can turn your heaviest users from a cost into a neutral line.
4. Treat dots, the "AI research intern" and the day-long research claims as OpenAI's claims until there is a paper or an outside eval.

I stamped DevDay 2026 NEEDS REVIEW. The prices check out and the independent index agrees with OpenAI's headline on Sol. The live demos stalled, and the boldest claims, including Sam Altman's statement that OpenAI now has "an AI research intern," come with no paper or public evaluation yet. The products are shipping; the grading so far is OpenAI grading OpenAI.

**What was announced at OpenAI DevDay 2026?**

More than 20 launches: dots (always-on agents), GPT-6.1 Sol, Ultrafast, the Pro 500 plan, the Decisions API, Codex in the cloud, Code Review, Codex Security Cloud, the Agents API with computer use, Bedrock Managed Agents, Private Intelligence, Sign in with ChatGPT, plugin extensions, MCP Events, ChatGPT Space, Pages and the OpenAI Marketplace.

**How much does GPT-6.1 Sol cost?**

$2 per million input tokens, $0.10 per million cached input tokens and $10 per million output tokens in the API. Its Ultrafast version is listed as coming soon.

**Who can use OpenAI dots?**

Pro and Business Premium subscribers in eligible markets get their first dot included; Enterprise, Edu and Healthcare have it in beta, off by default.

**Is GPT-6.1 Sol better than Claude Opus 5.5?**

On Artificial Analysis's Intelligence Index, no: Opus 5.5 scores 58 and Sol 52. Sol costs $0.72 per Index task against $5.98 for Opus 5.5.

*This article expands on an episode of **The Daily Diff**, a five-minute daily video on what shipped and what broke in tech. Watch the episode · Subscribe on YouTube · the written diff lands in your inbox every morning at thedailydiff.dev.*

## Top comments (1)

Some comments may only be visible to logged-in visitors. Sign in to view all comments.
