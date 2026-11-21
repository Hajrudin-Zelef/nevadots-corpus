---
id: collect-261121-veille-ia/muse/meta-muse-explained-personal-ai-agent-guide-for-business
title: "Meta Muse Explained: Personal AI Agent Guide for Business"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-09-08"]
keywords: ["muse", "meta", "agent", "machine", "guide", "personal", "business", "spark"]
source: https://www.halmob.de/blog/meta-muse-personal-ai-agent-guide
source_anchor: ""
source_lines: [1, 99]
section: "Muse"
---
**Meta Muse** is a personal AI agent that Meta launched on **8 September 2026**. It does not just answer questions. It sends emails, fills out forms, books travel and buys things for you, and it keeps working after you close the app. On 29 September 2026 Meta opened it to small businesses too. This guide explains what Meta Muse does, what it costs, where the open questions are, and what a small business should check before it hands an agent the keys.

#### The short version

**Muse Spark**model inside a private virtual machine that Meta sets up for each user. A second agent, called

**Sentinel**, guards its internet access. Muse is free with a weekly limit, and paid plans cost $20 or $100 a month. It launched in the US first. Amazon blocked it from its store within two weeks.

## What Meta Muse does

Think of Muse as an assistant with its own computer. You give it a goal, it works on it in the background, and it comes back when something changes or when it needs your approval. Meta's launch post for Muse lists tasks like these:

- Send emails, fill out forms and book travel.
- Negotiate on your behalf, for example to lower a bill or sell a car.
- Remember details you mention and make suggestions without being asked.
- Turn a saved Instagram recipe reel into a grocery list.

Muse is available on iOS, Android and the web at muse.ai. Meta says support for its AI glasses is coming. The difference from a chatbot is simple: a chatbot waits for your next message, while Muse keeps going on its own. That is the same shift we describe in our guide to proactive AI agents.

## How Meta Muse works under the hood

### The Muse Spark model

Meta calls Muse Spark its "most capable model to date." heise reported that the agent uses version 1.3 of Muse Spark. Meta has not published benchmarks for the agent itself, so it is hard to compare it with other agents on paper.

### A private virtual machine for every user

Each user gets a dedicated virtual machine, which Meta calls the **Muse Secure VM**. The agent and your data live there, and Muse uses its own browser on that machine. A separate agent, **Sentinel**, runs on the same machine but is isolated from Muse. According to Meta, Muse cannot reach the internet unless Sentinel approves.

Meta also announced a stronger version, the **Muse Confidential VM**, which would encrypt the whole machine with a key only you hold. In the launch post it was planned for "later this year." Until it ships, the protection rests on Meta's own controls.

### Connections and payments

You choose which apps Muse can use and how much access it gets. For email, you can allow reading only, or reading and sending. You can change or remove access at any time. Meta says logins are kept in secure storage that the agent can use without seeing, and Muse shows an audit trail of past and planned actions.

For payments, Muse checks out with **Link by Stripe**. Link creates a one-time card for each purchase, so your real card number stays hidden. Meta says Muse is the first AI agent covered by Link's purchase protections. Shop Pay and 1Password support were announced as "coming soon." Muse asks you before sensitive steps such as sending an email or making a purchase.

## Meta Muse plans and pricing

Muse is free for everyday use, with a weekly usage limit. Two paid plans raise that limit. Meta's help page on Muse subscriptions lists them like this:

| Source: Meta Help Center, checked 3 November 2026. |  |  |  | 
|---|---|---|---|
| Plan | Price | Weekly allowance | Notes | 
|---|---|---|---|
| Free | $0 | Not stated on the help page | Enough for most needs, according to Meta | 
| Power | $20 per month | 500M Muse tokens | Renews monthly until you cancel | 
| Max | $100 per month | 3B Muse tokens | Some coverage calls it "Maximum" | 

Some articles say the free tier allows 100 million tokens per week, quoting Mark Zuckerberg. We could not find that number on a Meta page, so treat it as unconfirmed. Paid plans are only offered where Muse is available.

## Muse for small businesses: the 29 September expansion

Three weeks after launch, Meta brought Muse to small businesses. TechCrunch reported that the business version can connect to:

- **Commerce and payments:** Shopify, Stripe, Intuit QuickBooks, Klaviyo.
- **Files and teamwork:** Dropbox, Box, Slack, Notion, Asana, Zoom, Granola.
- **Design and building:** Canva, Figma, Lovable, HighLevel.
- **Meta's own tools:** Instagram professional analytics, Facebook Pages and Meta ad accounts.

Meta says the agent learns what a business sells, its brand voice and its customers' most common questions. Muse for Small Business is free with usage limits, and paid plans add more usage. A day earlier, on 28 September, Meta launched the Meta Enterprise Platform, which offers Muse, Meta Business Agent, the Muse API and Muse Code to companies and developers.

## The Amazon dispute

Amazon was the first big retailer to push back. According to heise's report on the block, Amazon started blocking Muse on the evening of Sunday, 20 September 2026. Bloomberg reported it on Monday, 21 September. Users who try to shop on Amazon with Muse now see a message that this breaks Amazon's terms of use.

Amazon's complaints, as reported:

- Meta did not tell Amazon that Muse would access its store.
- Muse does not identify itself as an agent while browsing.
- Muse appears to collect and store customer credentials.
- Amazon asked Meta to remove Amazon from the Muse experience, and Meta refused.

Meta points to its design: Muse cannot see passwords or payment methods, and logins sit in secure storage. The Register's coverage places the dispute in a wider fight over who controls agent shopping. As of 3 November 2026, we found no report of a court case.

## Privacy and trust questions

Meta makes several promises in the launch post:

- You can opt out of having your Muse interactions used to train Meta's models.
- Muse conversations and VM data are not shared with Meta's ad systems.
- You can tell Muse to forget specific things it has learned.

These are good starting points, but some questions remain open. The Confidential VM, which would lock Meta itself out, had not shipped at launch. Meta has not published how often Sentinel blocks or escalates an action. And Amazon's complaint shows that the websites an agent visits may not agree to be visited. For anyone in the EU, there is one more point: Muse launched in the US, and on 27 October 2026 Meta announced it for Singapore. A business elsewhere should check availability and data rules before planning around it.

## What a small business should check before using Meta Muse

1. 1**Start with read-only access.** Give Muse read access to email and files first. Allow sending or buying only after you trust the results.
2. 2**Map every connected app.** Write down which tools Muse can reach and who in your team owns each one.
3. 3**Turn off training if you need to.** Check the setting that lets Meta use your interactions to train its models.
4. 4**Read the audit trail weekly.** The list of past and planned actions is your best early warning.
5. 5**Check the sites you depend on.** If a supplier or marketplace blocks agents, as Amazon did, your workflow breaks.
6. 6**Plan for limits.** Free and paid plans have weekly caps. Know what happens when a busy week hits the cap.

If you are comparing options, read our guide to OpenAI Dots, launched three weeks after Muse, and our OpenClaw 101 guide for a self-hosted alternative.

## How Halmob helps

Picking an agent is the easy part. Deciding what it may touch, and checking that it behaves, is the real work. Our agent platform consultancy helps a business choose, set up and roll out Meta Muse, OpenAI Dots, Hermes Agent, OpenClaw, Pi Durable or Claude Duet. We also build **proactive agents** that watch your systems and act on their own, for example to follow up a lead or flag a late invoice. If you want to test Muse in your business without guessing, get in touch.
