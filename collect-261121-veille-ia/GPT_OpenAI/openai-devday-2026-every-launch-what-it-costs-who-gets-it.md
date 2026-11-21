---
id: collect-261121-veille-ia/gpt-openai/openai-devday-2026-every-launch-what-it-costs-who-gets-it
title: "OpenAI DevDay 2026: every launch, what it costs, who gets it"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["chatgpt", "enterprise", "business", "included", "today", "codex", "cloud", "work"]
source: https://andrewlauchner.com/notes/openai-devday-2026
source_anchor: ""
source_lines: [1, 260]
section: "GPT_OpenAI"
---
If you read nothing else: OpenAI now sells you an employee. It is called a dot, it runs 24 hours a day on its own computer in the cloud, and the first one is included if you already pay $100 or more a month, with a month of unmetered use to get you hooked. Everything else today was built to give that employee somewhere to work, a cheaper brain, a faster mouth, and a way to bill for it.

## The sixty-second version

- **Dots.** Always-on agents inside ChatGPT with their own cloud computer and browser. First one
        included on Pro and Business Premium. Rolling out today.
- **ChatGPT Space and Pages.** A shared workspace where your team, ChatGPT and your dot work on the
        same documents. Pro, Business and Enterprise.
- **GPT-6.1 Sol.** Near-flagship intelligence at a fifth of the flagship’s token price. In the
        API today.
- **Ultrafast.** Up to 300 tokens a second. Costs six times standard. Comes with the new
        $500-a-month Pro 500 plan.
- **The platform.** Codex in the cloud, an Agents API that can use a computer, a Decisions API that
        answers in a fraction of a second, Sign in with ChatGPT, and a Marketplace with 32 launch partners.
- **The number.** Sam Altman: “Around 1.2 billion people use ChatGPT.” (44:11)

## Every launch, one line each

Twenty rows. The “who gets it” column is the one to read; most of today was gated by plan.

| Every DevDay 2026 announcement with who gets it and what it costs |  |  |  | 
|---|---|---|---|
| Launch | In one line | Who gets it | Cost | 
|---|---|---|---|
| Dots | Always-on agent with its own cloud computer, browser and 4,000+ app connections | Pro, all three tiers, and Business Premium; Enterprise beta if an admin turns it on. Pro users in the EEA, Switzerland and the UK wait | First dot included; its usage is free for the first month; more dots and speed sold later | 
| ChatGPT Space | Shared workspace for a team, ChatGPT and dots | Pro, Business, Enterprise; desktop and web, mobile read-only for now | Included | 
| Pages | A document type built for people and agents to edit together | Pro, Business, Enterprise | Included | 
| Collaborative slides | Decks edited by teammates and agents at once, export to PowerPoint or Google Slides | Pro, Business, Enterprise | Coming weeks | 
| Teams and shared tasks | Delegate recurring work to run on a schedule or on an event such as a new email | Business, Enterprise | Included | 
| @ChatGPT in Slack and Teams | Mention it in a channel; teammates without a licence can refine the result | Business, Enterprise | Included | 
| Meetings plugin | Notes and action items saved to your Space; audio deleted after | All Pro and Business plans on the macOS desktop app; Enterprise limited alpha | Beta | 
| GPT-6.1 Sol | Upgraded mid-tier model that nearly matches GPT-6 Astra | API today; ChatGPT Work and Codex on Plus and up. Not yet in Chat | $2 in, $0.10 cached, $10 out per million tokens. Same as GPT-6 Sol except cached input, which halved | 
| Ultrafast | Premium speed tier: up to 8× faster in Codex, 6× in the API | Any API account today, at low rate limits; in ChatGPT Work and Codex on Pro 500 and Enterprise only; Sol Ultrafast in days | 6× standard: $60 in, $300 out per million for Astra | 
| Pro 500 | New top plan: 25× the Plus allowance plus Ultrafast | Anyone | $500 a month | 
| Decisions API | Model picks from a fixed set of answers in about 150 milliseconds, for routing and classification | Limited preview today, broad release in days | Not published | 
| Agents API with computer use | Build agents that click through software; same harness that powers dots | API, plus Codex and ChatGPT Work on Pro 500 and Enterprise | API rates | 
| Codex in the cloud | Start a task on your phone, finish on your desktop, with shared team environments | Plus and up | Included | 
| Codex Security Cloud | Scheduled scans of whole repositories with deduplicated findings and prepared fixes | Pro, Business, Enterprise, Edu | Included | 
| Code Review | Review diffs in the desktop app; automatic first-pass reviews in the cloud | All plans | Included | 
| Private Intelligence | Zero data retention with automated safety review; confidential inference preview this fall | Enterprise, by contact | Not published | 
| Plugin extensions | Full apps with their own sidebar panels inside ChatGPT and Codex; Adobe demoed | All plans | Free to build | 
| Sign in with ChatGPT | Users spend their own ChatGPT allowance inside your product | 16 launch partners including Devin, Notion, Vercel, Warp, Amp and T3; Lovable next | Free to integrate | 
| OpenAI Marketplace | Enterprises apply their OpenAI commitment to partner software | 32 launch partners including Adobe, Figma, Salesforce, HubSpot, ServiceNow | Draws down existing commits | 
| Bedrock Managed Agents | OpenAI agents that run entirely inside AWS | AWS customers; AWS’s own page still calls it a preview | Matches OpenAI’s rates, counts toward AWS commits | 

Also announced: MCP events so plugins can start automations when something happens in a connected app; Sites can now host plugins; shareable profiles; a Plugin Creator and a simpler submission flow; a refreshed Codex CLI you can steer by voice. Source: OpenAI’s DevDay 2026 recap and the pricing page, 29 Sep 2026.

## Dots, explained without the demo

A dot is an agent that does not stop when you close the tab. OpenAI’s own description: “Powered by GPT-6 Astra, they have their own cloud computer, learn from feedback over time, and can work towards your goals 24/7.” You give it a name, connect the apps you already use, and hand it responsibilities rather than prompts. On stage the examples were “monitor incoming bug reports, test fixes and draft PRs” and “kick off next year’s planning cycle” (6:01).

Four things decide whether that is useful to you or just a nicer chatbot.

### Where it lives

In ChatGPT on desktop, web and mobile, and in Slack and Microsoft Teams. You can call it on the phone. Texting is “coming soon” (7:14). It carries context across all of those, so the thread you started at your desk is the one it continues from your phone. You can open its computer at any time and watch what it is doing, and you can give it permission to use your own laptop.

### What it costs

“Your first dot is included in your Pro or Business Premium plan at no extra cost,” and conversations with it do not count against your ChatGPT usage. Tasks it kicks off in Codex or ChatGPT Work do. Deeper work draws on an allowance. For the first month that allowance is not metered at all: OpenAI’s help centre says dots usage “won’t count toward eligible Pro, Business, and Enterprise users’ plan allowances” and that usage terms per plan come after. That is the sentence to remember in November. More dots, and faster ones, will be sold separately later. The $100 Pro tier is in: OpenAI’s pricing page lists the dot under Pro “from $100” and OpenAI’s Thibault Sottiaux confirmed it on X after an early BGR report said otherwise. Two exclusions at launch: you must be 18 or over, and Pro users in the EEA, Switzerland and the UK do not get dots yet. Business Premium does, everywhere ChatGPT is sold.

### What it is allowed to do

This is the part OpenAI wrote most carefully, and it is worth reading before you connect anything. Each dot starts with built-in rules for when to act and when to ask. Custom Rules let you allow a specific action, require approval for it, or block it. When you are not working with it, it does “proactive research” using read-only tools: it can look, it cannot send, change or click. Sensitive actions such as changing a password “always stay with you”. Everything it does, including background work, shows in an Activity View. And the sentence OpenAI put at the end of its own page: “Dots can still make mistakes, so always review consequential work.”

      This allows you to trust your dot with as much responsibility as you’re comfortable with.

Sam Altman, keynote, 7:52

### What OpenAI is using them for

The most useful line in the whole launch is not about you. It is about what OpenAI’s own staff have handed over. On stage: “Our engineers have their dot fixing dozens of bugs every day” (15:34). On the page, the enterprise version, called specialist dots, has been piloted internally “across procurement, invoice processing, email marketing, customer support, and commercial contracting”. Those five are the jobs OpenAI thinks an agent can hold today with a person approving the output. Specialist dots are enterprise-only pilots for now, run with OpenAI’s engineers, and will plug into Microsoft’s Agent 365 controls.

Meta shipped a comparable product, Muse, on 8 September, with a free tier and paid tiers reported at $20 and $100 a month. Simon Willison, live-blogging the keynote, noted the dots interface “closely resembles” Muse. The difference for a buyer is the bundle: Muse is sold on its own, a dot comes with a plan you may already have.

## Space and Pages: the office the dot works in

ChatGPT Space is “a new home for your team to collaborate with AI”. In practice it is a shared area with Pages, a document type where “anything you can do in ChatGPT you can do on a page”: write, research, chart, generate images. You tag a teammate or a dot in a comment and it goes to work in the document. In the demo, Holly Li asked her dot to turn a table into a bar chart, pull a statistic out of a Slack DM, and keep a page current every hour (13:23 to 13:45).

It ships on Pro, Business and Enterprise, on the desktop app and the web. On mobile you can find, read and share pages; creating and editing there is “coming soon”. Collaborative slides, with export to PowerPoint and Google Slides, are “in the coming weeks”. Business and Enterprise also get teams, shared tasks that run on a schedule or on an event such as a new email, and @ChatGPT inside Slack and Teams. The Meetings plugin, which takes notes into your Space and deletes the audio afterwards, is a macOS beta for Pro and Business.

Read plainly: this is OpenAI building the parts of Slack, Notion and Google Drive that an agent needs in order to be handed work, inside the product 1.2 billion people already open.

## The models: cheaper, then faster

No new frontier model. GPT-6 Astra is three weeks old. Today was about the price and the speed of getting Astra-class output.

### GPT-6.1 Sol

OpenAI’s claim is one sentence long: near-Astra intelligence “at one-fifth of Astra’s standard input and output token prices”. The API prices are on the page:

| GPT-6.1 Sol API pricing against GPT-6 Astra, per million tokens |  |  |  | 
|---|---|---|---|
| Model | Input | Cached input | Output | 
|---|---|---|---|
| GPT-6.1 Sol | $2.00 | $0.10 | $10.00 | 
| GPT-6 Sol | $2.00 | $0.20 | $10.00 | 
| GPT-6 Astra | $10.00 | $1.00 | $50.00 | 

Standard tier, per million tokens, from OpenAI’s developer pricing page and model pages on 29 Sep 2026. Long-context requests above 272K input tokens cost more; Fast is 2×, Batch and Flex are half.

The line nobody put on a slide: GPT-6.1 Sol is not cheaper than the model it replaces. GPT-6 Sol already cost $2 and $10. The upgrade is free and the only price that moved is cached input, from $0.20 to $0.10. “A fifth of the price” is true against Astra, which is the comparison OpenAI chose. The model takes a 1,050,000-token context, returns up to 128,000 tokens, and knows the world to 30 April 2026.

The benchmark claims, all OpenAI’s own: it matches Astra on DeepSWE, a real-codebase coding test, at roughly a fifth of the cost per task; it comes within 2.1 points of Astra on OSWorld 2.0, a computer-use test, at about a seventh of the cost; and on hard factual prompts it cuts the share of answers with an error from 11.4% to 7.7% against GPT-6 Sol at low reasoning effort. OpenAI’s charts also place it above Anthropic’s Claude Opus 5.5 on two professional-work tests. Those competitor figures are OpenAI’s, taken from public reports, and its own footnote concedes one of them understates the rival’s cost.

Availability has a catch. It is in the API today as `gpt-6.1-sol`, and in ChatGPT Work and Codex for
      Plus, Pro, Business, Enterprise and Edu. It is “not yet available in Chat”, so a Plus subscriber talking to
      ChatGPT the normal way does not have it yet.

### Ultrafast

A premium speed tier: “up to 8× faster token generation (300 tokens per second) in Codex and up to 6× in the API”. Sam Altman framed the price on stage: Fast already costs twice standard for twice the speed, Ultrafast goes further, and “you know what, it’s worth it” (20:47). The multiple is six. OpenAI’s developer pricing page lists Astra Ultrafast at $60 per million input tokens and $300 per million output, against $10 and $50 standard. Inside a Codex subscription it burns eight times your included usage. Any API account can request it today with the service tier set to ultrafast, at low rate limits and US-only processing. In ChatGPT Work and Codex it is on Pro 500 and Enterprise only, not the $100 or $200 Pro tiers. Sol Ultrafast is “in the coming days”. The demo that sold it was Romain Huet rebuilding a 3D scene in real time and generating a small app from a one-line prompt before he finished the sentence (33:39 to 36:05).

### Decisions API and Private Intelligence

The Decisions API takes a question with a fixed set of possible answers and returns one “in a fraction of a second” (22:15). It runs on a variant of the small GPT-6 Luna model and returns one answer from your list, with a confidence score. OpenAI’s developer post puts a call at about 150 milliseconds against roughly 1.6 seconds for Luna through the normal API. It is for routing, classifying and choosing an agent’s next step, not for writing. Limited preview today, broad release in days, no price published.

Private Intelligence is the enterprise privacy bundle: zero data retention with “Private Safety Processing”, so automated safety review happens without OpenAI staff seeing content, and a preview of confidential-computing inference this fall. Contact sales.

## The plan ladder, as of tonight

| ChatGPT plans and prices after DevDay 2026 |  |  | 
|---|---|---|
| Plan | Per month | What changed today | 
|---|---|---|
| Free | $0 | Nothing. Unlimited text chat on GPT-5.6 Luna. | 
| Go | $8 | Nothing. May include ads. | 
| Plus | $20 | GPT-6.1 Sol in Work and Codex; Codex in the cloud; Sign in with ChatGPT allowance. | 
| Pro | $100 | The dot, confirmed for this tier by OpenAI on X; Space and Pages; Meetings beta. Usage stays at 5× Plus. | 
| Pro 200 | $200 | Reopened to new subscribers today, at half the old usage: 10× Plus and 100 GPT-6 Pro messages a week instead of 20× and 200. Existing subscribers keep 20× until 29 October, then drop, with a one-time $2,500 usage credit that expires at the end of 2026 (Engadget, The Next Web). | 
| Pro 500 | $500 | New. 25× the Plus allowance, Ultrafast, and computer use through the Agents API inside ChatGPT Work. The only Pro tier with Ultrafast. | 
| Business | $25 a seat | $20 on annual. No dot on a standard seat. Teams, shared tasks, @ChatGPT in Slack and Teams, Space, Meetings. | 
| Business Premium | $125 a seat | $100 on annual. Five times a standard seat’s usage, no five-hour limit, and the first dot. | 
| Enterprise | Custom | Dots as an admin-enabled beta. Ultrafast and the Agents API in Work. Private Intelligence by contact. | 

Individual prices from chatgpt.com/pricing on 29 Sep 2026, which shows Pro as “from $100” with “your choice of 3 usage tiers”. Business seat prices and the Pro 200 change are from OpenAI’s help centre as quoted by Engadget, The Next Web and Appwrite; OpenAI’s pricing page does not print them.

## For the people who build

**Codex** got the most stage time. The harness that runs Codex, ChatGPT Work and dots is open source (27:38).
      Codex now runs in the cloud with reusable team environments, so “you can start a task on your phone and pick
      it up on your desktop” (28:20). The CLI takes voice, has an /agents view for tracking parallel tasks, and got
      worktree support. Code Review lives in the desktop app and can take an automatic first pass while you are away.
      Codex Security Cloud scans whole repositories on a schedule, deduplicates findings and prepares fixes, with
      OpenAI’s defensive Daybreak Blue models included without a separate application. The CLI’s default model is
      now GPT-6.1 Sol at medium effort. And from the stage, at 52:15, OpenAI reset every Codex user’s usage allowance
      worldwide; each attendee also left with a limited-edition ModRetro Chromatic handheld built with Codex.

**The Agents API** now includes computer use, multi-agent control, tool search and context compaction,
      “the same technology that powers dots” (29:20). Amazon’s Bedrock Managed Agents wrap it so an OpenAI agent
      can run entirely inside AWS. On reliability, Altman claimed “more than 99 percent” uptime and a 45 percent
      cut in time to first token (31:06).

**Distribution** is the quiet strategy. Sign in with ChatGPT lets your users spend their own plan allowance
      inside your app, “so now you don’t have to cover their cost” (44:52). The user sets a cap per app in their
      ChatGPT settings; the app sees a name, an email and a picture, never conversations. Sixteen partners at launch:
      Cognition’s Devin, Notion, Vercel, Warp, Amp, T3, Kilo Code, Conductor, Dactyl, Hyperagent, Vorflux, Hermes
      Agent, OpenCode, OpenClaw and Pi, with Lovable “coming soon”. Airtable, Canva, GitLab, HubSpot and Supabase
      take the sign-in without the allowance. Plugin extensions turn a plugin into a full app with its own sidebar
      panel; plugins can now be discovered mid-conversation and hosted on Sites, of which OpenAI says “millions”
      have been built. The Marketplace lets enterprise customers spend part of their existing OpenAI commitment on
      partner software. OpenAI names twelve of the 32: Adobe and Figma for creative work; Sierra, Decagon, HubSpot,
      Salesforce and ServiceNow for customer experience; Harvey and Legora for legal; Palo Alto Networks and CrowdStrike
      for security; Baseten for open-source models. DevRev, ElevenLabs, CodeRabbit, Datadog, Canva, Notion, Vercel and
      Zendesk have announced themselves. The directory is live; buying through it is “soon”.

## What the keynote did not say

- **No new frontier model.** Astra shipped 3 September. Today made it cheaper and faster to reach, not smarter.
- **Dots are desktop-first, and not everywhere.** You create one in the desktop app or a desktop browser; mobile
        joins after setup. Pro users in the EEA, Switzerland and the UK are excluded at launch.
- **GPT-6.1 Sol is not a price cut.** It costs what GPT-6 Sol cost. The fifth-of-the-price line is against
        Astra.
- **The generous limits expire.** Dots usage is unmetered for a month. The allowance after that is not
        published.
- **Enterprise is off by default.** An admin has to enable the beta.
- **Two demos failed live.** The dot did not answer a voice call (12:05) and Codex voice did not start in the
        terminal (35:21). Both were recovered by typing. Worth knowing before you plan a workflow around voice.
- **Every competitor number is OpenAI’s.** The Sol page compares against Anthropic’s models using public
        reports and says so.
- **Pro 200 got quieter, not louder.** A new $200 subscriber gets half the usage a subscriber from last month
        gets, and the old subscribers join them on 30 October with a $2,500 credit as consolation.

## What to do this week

**If you already pay $100 or more for ChatGPT:** create your dot, connect one read-only app, and set a Custom Rule
      that requires approval on anything that sends or spends. Give it one recurring job with a clear deliverable:
      “every morning, summarise what arrived overnight and list what needs me.” Judge it on that for two weeks
      before you connect anything that can act.

**If you run a team:** open one Space for one project that is already messy across Slack, Docs and email.
      Do not migrate anything. See whether the page stays current without a person babysitting it.

**If you build on the API:** point your GPT-6 Sol workloads at `gpt-6.1-sol` today. Same price,
      better scores, and cached input at half the old rate, so anything that reuses context gets cheaper for free.
      Put the Decisions API on your routing and classification steps. If your users already pay for ChatGPT, Sign in
      with ChatGPT removes your inference bill for them.

**If you sell software to enterprises:** the Marketplace turns OpenAI’s sales commitments into your pipeline.
      Thirty-two partners are in. Ask how the next thirty get in.

**Everyone else:** do not buy Pro 500 for Ultrafast unless you are paid by the hour to wait on Codex. The
      $100 plan now includes the headline product.

## How it landed

**The backdrop the keynote skipped.** On Monday, the day before, OpenAI pulled GPT-6.1 Astra and cancelled
      its October launch after internal tests; The Wall Street Journal, as relayed by TechCrunch, reported higher rates
      of deception and hallucinated tool calls. OpenAI safety lead Saachi Jain told The Decoder the withheld model
      “deceived more often and kept going without permission”. Altman to CNBC: “I wouldn’t over-rotate
      on this one thing.” The Associated Press headline was that the CEO “avoids mention of security
      concerns” at his own developer conference, three months after the Hugging Face breach he has called the most
      severe incident OpenAI has seen.

**The money.** CNBC reported that OpenAI’s enterprise business more than doubled in the third quarter, that
      the company projects a run rate near $68 billion, and that it is in early talks to raise about $30 billion.
      Altman said he has no particular timeline for an IPO. Fortune counted 2.5 million businesses on ChatGPT and read
      the Sol pricing as a move in an ongoing price war with Anthropic, whose Claude Opus 5.5 shipped a week earlier.

**The comparison everyone made.** Meta’s Muse is three weeks old, past three million downloads and top of the
      App Store, with a free tier. TechCrunch called dots a “bubbly, cartoonish persona, similar to Meta’s recently
      released Muse”. Altman, asked about Muse on CNBC: “I have tried it. It seems like a nice product.”
      The Deep View read the day differently: “aimed much more at competing with Anthropic for power users than
      competing with Meta for consumers.” VentureBeat’s Carl Franzen framed the new contest as “which agent a
      person or business is willing to trust with continuous access”. Engadget’s live blog called the event
      “tone deaf” given the week’s incidents, and its dots write-up opened with “What could go wrong?”

**The people who tried it.** Every’s Dan Shipper spent the day inside it and wrote that dots are “too buggy
      for me to recommend now”, with dropped messages and permission glitches, then made a dot his primary way of
      using ChatGPT anyway and suggested waiting a week or two. On Hacker News the dots thread ran past 250 comments;
      the GPT-6.1 Sol thread ran past 450, where the consensus was that the cached-input price, not the model, is the
      news. The Pro 500 thread’s word for the Pro 200 change was “shrinkflation”.

And one detail nobody at OpenAI planned: dot.com belongs to someone else and redirects to a rival’s bot, which X noticed within the hour.

## Sources

- OpenAI, DevDay 2026 Recap, 29 Sep 2026.
- OpenAI, Introducing dots, 29 Sep 2026.
- OpenAI, Introducing GPT-6.1 Sol, 29 Sep 2026.
- OpenAI, ChatGPT pricing, read 29 Sep 2026.
- OpenAI, DevDay 2026 Keynote (full), streamed 29 Sep 2026. Timestamps in the text refer to this video.
- OpenAI, API pricing and the GPT-6.1 Sol model page, read 29 Sep 2026.
- OpenAI, ChatGPT Work and Codex usage and pricing, read 29 Sep 2026.
- Thibault Sottiaux (OpenAI), on dots and the Pro 100 plan, X, 29 Sep 2026.
- Simon Willison, OpenAI DevDay 2026 live blog, 29 Sep 2026.
- Engadget, OpenAI adds $500 Pro subscription, nerfs its existing $200 tier, 29 Sep 2026.
- The Next Web, Pro 200 usage cut and the Pro 500 plan, 29 Sep 2026.
- TechCrunch, OpenAI launches dots, its bubbly agentic avatar, 29 Sep 2026.
- CNBC, OpenAI DevDay 2026 live updates, including Sam Altman’s Halftime Report interview, 29 Sep 2026.
- Fortune, OpenAI takes on Meta’s Muse with new dot agents, 29 Sep 2026.
- Associated Press, OpenAI CEO announces new AI agent and avoids mention of security concerns, 29 Sep 2026.
- The Decoder, OpenAI launches always-on dots agents to rival Meta’s Muse, 29 Sep 2026.
- VentureBeat, OpenAI launches dots and ChatGPT Space, 29 Sep 2026.
- The Deep View, OpenAI rolls out deluge of new tools for AI builders, 29 Sep 2026.
- Engadget, OpenAI DevDay live blog, 29 Sep 2026.
- Every, Dan Shipper, Vibe Check: OpenAI DevDay 2026, 29 Sep 2026.
- Hacker News threads on dots, GPT-6.1 Sol and Pro 500, 29 Sep 2026.
- BGR, Everything OpenAI announced at DevDay 2026, 29 Sep 2026.
- 9to5Mac, OpenAI makes 20+ announcements at DevDay, 29 Sep 2026.
