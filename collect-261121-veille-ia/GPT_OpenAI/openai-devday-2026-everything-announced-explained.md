---
id: collect-261121-veille-ia/gpt-openai/openai-devday-2026-everything-announced-explained
title: "OpenAI DevDay 2026: Everything Announced, Explained"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["agent", "openai", "dots", "devday", "agents", "work", "announced", "more"]
source: https://www.theneuron.ai/news/openai-devday-2026-chatgpt-is-becoming-an-ai-operating-system/
source_anchor: ""
source_lines: [1, 411]
section: "GPT_OpenAI"
---
OpenAI announced more than 20 things at DevDay.

The one that got the cute mascot was Dots, its new always-on personal agent. The one that got the benchmark charts was GPT-6.1 Sol. Developers got a new Agents API. Companies got Private Intelligence. ChatGPT got richer plugins, Spaces, Pages, Teams, event-driven automations, and a Marketplace. There was even a new $500/month subscription tier.

You can read all of that as a giant pile of launch-day features.

I think that's the wrong mental model.

**OpenAI spent DevDay assembling the pieces of an operating layer for AI agents.** Not an operating system in the literal Windows/macOS sense. Think of it as the layer sitting between you and all the software, information, and services you normally operate yourself.

Instead of you opening the browser, finding an app, logging in, figuring out the interface, moving information somewhere else, and repeating that process 40 times a day, an agent increasingly handles that sequence for you.

The computer is still underneath. The apps still exist.

You just interact with them less directly.

*Which, as someone who would happily never hunt through another SaaS settings menu again, sounds pretty good to me.*

And that changes the competitive question in AI.

For the past few years, every company wanted to build the model you talked to.

Now everybody wants to build **the agent that represents you**.

## Dots is the easiest place to see where this is going

OpenAI describes Dots as always-on agents powered by GPT-6 Astra. Each Dot gets its own cloud computer and browser, can connect to more than 4,000 apps, learns from your feedback, and can keep working toward goals 24/7.

That already sounds different from normal ChatGPT.

When you use a chatbot, the basic interaction is:

**You ask â AI responds â interaction ends.**

Dots changes the loop:

**You set goals â the agent keeps context â events happen â the agent notices â work continues â you review important decisions.**

OpenAI gives examples that make this much more concrete.

A developer's Dot could watch customer feedback, spot recurring bugs, scope fixes, build them, test them, and return completed pull requests. A researcher's Dot could rerun analysis when new evidence arrives. A sales Dot could keep updating a proposal as requirements change.

OpenAI says an early tester's Dot even noticed he had forgotten to invoice a publication, prepared the invoice, and sent it after approval.

That last example is the one I'd pay attention to.

The valuable part of a personal agent probably won't be asking it to "go research X." You already have agents that can do that.

The step change comes when the agent understands enough about your ongoing life or work to realize **something needs doing before you explicitly create the task**.

Today, OpenAI gives you one primary Dot. Eventually, it says it envisions teams of Dots working together. Companies are also getting "specialist Dots" with separate identities, credentials, hardware, and access to internal systems.

That sounds a lot less like chatbot software and a lot more like hiring digital coworkers.

## The rest of DevDay explains how Dots can actually work

An agent that remembers your goals is neat.

An agent that can actually do things needs infrastructure.

This is where a bunch of announcements that look boring individually start fitting together.

### 1. The agent needs somewhere to work

Dots get their own cloud computers.

Codex can now keep working in the cloud while your own laptop is closed. And the Agents API gives developers access to the same underlying agent harness used by Codex, including hosted sandboxes where agents can run code, manipulate files, and save intermediate work.

A **harness** is basically everything wrapped around the AI model that lets it do useful work over time.

The model supplies intelligence.

The harness handles things such as:

- what tools the agent can use;
- what information stays in context;
- where it stores files;
- how it delegates jobs to other agents;
- how long-running jobs survive multiple context windows;
- what happens when something fails.

That distinction matters more as agents run longer.

A brilliant model that forgets what it was doing two hours ago makes a terrible employee.

OpenAI's Agents API automatically compresses earlier context as sessions get long, loads only the tools relevant to the current job, and allows multiple tool calls or subagents to operate in parallel.

This is infrastructure developers previously had to build themselves.

Now OpenAI wants to provide it.

## 2. The agent needs access to your apps

This was quietly one of DevDay's biggest themes.

OpenAI expanded plugins so they can become interactive experiences inside ChatGPT. A plugin can have its own sidebar destination, panels, file viewers, connected apps, reusable skills, and actions.

Imagine clicking Canva, Adobe, Notion, or another app **inside ChatGPT**, then working with its interface while ChatGPT remains beside you.

The distinction between "using ChatGPT" and "using software through ChatGPT" starts getting blurry.

OpenAI is also supporting MCP Events for plugin automations. That means a connected app can tell ChatGPT when something happened instead of waiting for you to ask about it.

Say a customer leaves a new comment.

The old pattern is:

**You notice comment â open software â read it â decide what to do â start work.**

The event-driven pattern becomes:

**New comment â plugin emits event â agent wakes up â retrieves context â drafts response or starts work â asks you when approval is required.**

That's the connective tissue required for an agent to become persistent.

You stop being the notification system.

## 3. The agent needs shared memory and a place to leave its work

ChatGPT Space gives people, ChatGPT, and Dots a shared environment filled with Pages, files, presentations, spreadsheets, and project context.

Pages are especially interesting because they're built for human and agent collaboration.

An agent can research something, generate charts, revise writing, or update information. Humans can jump in, edit it, leave feedback, and send the agent back to work.

OpenAI is also adding shared team tasks that can run on a schedule or trigger when something happens, such as receiving an email or Slack message.

This sounds mundane until you compare it with today's workflow.

Most AI work currently disappears into chats.

A persistent agent needs persistent **state**, meaning some durable record of the project that survives after an individual conversation ends.

Space is OpenAI's attempt to supply that layer.

We saw an earlier version of this direction when OpenAI launched GPT-5.6 and ChatGPT Work. At the time, the confusing part was how many different ChatGPT surfaces existed.

DevDay makes the intended destination clearer.

Chat, Work, Codex, Pages, plugins, and Dots increasingly look like parts of one larger system.

## 4. The agent needs your identity, permissions, and money

This was my favorite boring announcement.

OpenAI introduced **Sign in with ChatGPT**, which can let participating apps use your ChatGPT identity. More importantly, certain outside services can consume part of your existing Plus or Pro allowance instead of forcing every developer to pay the AI bill for every user.

That's a big deal for AI software economics.

Right now, a developer building an agent often has three ugly choices:

- pay the user's model costs and hope subscription revenue covers them;
- make users paste in API keys;
- build a separate credit system nobody asked for.

OpenAI is creating another option.

You show up with your ChatGPT account and, where supported, **bring your own intelligence budget**.

If that spreads, your ChatGPT subscription starts behaving less like "payment for one chatbot" and more like a compute plan that follows you around the software ecosystem.

OpenAI's new Marketplace pushes the idea even further on the enterprise side. Eligible companies can put part of their existing OpenAI spending commitment toward approved partner products, including software from companies such as Adobe, Figma, Harvey, Notion, Salesforce, ServiceNow, Vercel, and Zendesk.

OpenAI wants to sit between buyers and AI software vendors too.

Remember that part.

## 5. The agent needs cheaper intelligence

A persistent agent can chew through a ridiculous number of tokens.

We got a very literal demonstration of that during our livestream when Corey opened his ChatGPT usage profile and discovered he'd run through more than 12 billion tokens.

Someone in chat claimed nearly 200 billion across their accounts.

*AI has apparently invented a new genre of humblebrag: "My robot employees consumed more text than civilization produced this afternoon."*

More seriously, cost matters because agent workloads are fundamentally different from asking occasional chatbot questions.

An always-on system keeps loading context, checking tools, reading updates, delegating work, and producing intermediate reasoning.

That makes OpenAI's GPT-6.1 Sol announcement more important than its headline benchmark score.

OpenAI says GPT-6.1 Sol approaches Astra performance across agentic coding, computer use, and professional work while charging one-fifth Astra's standard input and output token price. Cached input costs $0.10 per million tokens.

Caching means the system can reuse information it has already processed at a steep discount instead of paying full price every time.

Think of an agent that repeatedly needs your project docs.

Without caching:

**Read 100-page project context â pay â do task â repeat tomorrow â pay again.**

With effective caching:

**Read context â preserve reusable representation â repeatedly reference it much more cheaply.**

That is why tiny improvements in token economics compound quickly.

As Corey put it on the stream: **that's what makes agents doable.**

OpenAI is also selling speed separately. Astra Ultrafast can reach up to 300 generated tokens per second, while the new Pro 500 plan costs $500 per month and is the only personal Pro tier with Ultrafast included.

There's now a very literal price on impatience.

## All of this is an aggregation land grab

This week's agent news already looked like a land grab before OpenAI walked onstage.

Meta has Muse. Manus has its own persistent-agent push. OpenClaw helped popularize the pattern of giving an AI its own computer, memory, tools, and autonomy. We wrote yesterday about how Meta is trying to own your AI front door.

DevDay makes OpenAI's answer obvious.

The company wants ChatGPT to become the primary interface sitting between you and everyone else.

You might still use dozens of agents.

I jokingly called this future **polyagentamorous** during the livestream.

You could have a coding agent, financial agent, research agent, company HR agent, travel agent, sales agent, healthcare agent, and whatever strange specialist arrives next Tuesday.

But one of them may become your chief of staff.

You tell that agent what you want.

It figures out which other tools, models, agents, and companies should provide it.

That's a very powerful position.

The browser currently aggregates websites.

Google aggregates discovery.

App stores aggregate software distribution.

Amazon aggregates shopping.

An effective personal agent could aggregate **action**.

You say, "Find me somewhere to stay in Chicago under $300 near the conference."

The agent could decide:

- which hotel search service to use;
- which loyalty account matters;
- what tradeoffs fit your preferences;
- whether another agent has a better offer;
- which payment method to use;
- when it needs your permission to book.

The winning interface gets enormous influence over every provider underneath it.

And if you're building one of those underlying services, your strategy changes too.

The goal cannot be "everyone should abandon their main AI assistant and come use my agent."

You probably want your product to become extremely easy for **someone else's agent** to discover, understand, call, pay, and trust.

That may become the new SEO.

## There is one giant problem: we have to trust these things

The timing here is almost comically uncomfortable.

OpenAI announced agents with more memory, more autonomy, more app access, more computer use, more credentials, and more ability to operate while you're away.

At almost the exact same moment, OpenAI was dealing with evidence that increasingly capable agents do surprising things when given tools.

WIRED reported that OpenAI cancelled the planned GPT-6.1 Astra launch after the model fell short of its safety bar around staying within scope and authorization.

OpenAI has also been investigating incidents where agents escaped intended boundaries, touched outside systems, or found unexpected ways around restrictions. We've covered why agent permissions are becoming such a difficult safety problem and OpenAI's own reports of agents crossing intended boundaries.

So DevDay contained a second architecture hiding under the agent architecture:

**control.**

Dots run on separate cloud computers by default. Users decide which apps they can access. Proactive background research is restricted to read-only tools. Custom Rules can allow, block, or require approval for specific actions. Auto-review checks sensitive actions against those rules. Some actions always remain human-only.

The enterprise privacy side goes further.

OpenAI's new Private Safety Processing keeps protected customer content in customer-controlled storage, encrypted with customer-managed authorization. Automated safety review happens inside a hardware-attested environment designed to prevent OpenAI employees from reading the underlying content.

Only bounded safety signals and operational metadata can come back out in plaintext.

That's a mouthful, so here's the simpler version:

**Businesses want OpenAI to monitor agents for dangerous behavior without OpenAI employees gaining another path into the company's confidential data.**

Private Safety Processing is OpenAI's answer.

That architecture could become extremely important because agents create an awkward enterprise paradox.

For your agent to become useful, it needs more access.

For your company to trust the agent with more access, it needs stronger isolation, permissions, logging, and enforceable boundaries.

Capability pushes the door open.

Security has to decide how wide.

## The strongest case against the "OpenAI operating system" thesis

OpenAI hasn't won this layer.

Far from it.

Meta's Muse already showed that a simple consumer agent can catch fire. The Neuron's guide to Meta Muse came away especially impressed by how easy it was to use. Readers in their 70s have written to us saying they figured it out.

That's a serious product advantage.

Dots currently begins on paid Pro plans, and OpenAI's cheapest Pro tier is $100 per month. Its first Dot is included, but deeper work has usage allowances.

That pricing makes sense for people using an agent to make money.

It's a very different proposition for my father-in-law saying, "I'd like one of those agent things."

OpenAI also has a product-complexity problem.

ChatGPT now contains Chat, Work, Codex, Sites, plugins, Spaces, Pages, Teams, Tasks, Dots, models, different reasoning levels, different plans, credits, and multiple usage pools.

Power users can figure this stuff out.

The billion-person market should never have to.

The company that wins the personal-agent era might not own the smartest model or the largest pile of features.

It may simply hide the machinery better.

## The $500 plan tells you who OpenAI expects to pay first

DevDay's new Pro 500 tier is worth looking at through that lens.

The plan costs $500 per month, includes OpenAI's largest personal usage allowance, and unlocks Astra Ultrafast. Pro 100 and Pro 200 remain available without Ultrafast.

Most people absolutely do not need this.

I can see the business case.

Imagine two small teams racing to ship competing products.

One team spends $500 for an agent that codes, researches, reviews changes, tests the app, writes docs, and works much faster.

Saving one engineer a few hours can cover the subscription.

The math starts looking different when AI shifts from "software I occasionally ask questions" to "software continuously producing work."

That distinction will matter across this entire market.

Consumers compare subscriptions against Netflix.

Businesses compare them against labor.

## The model is becoming the least interesting part

This might be my biggest takeaway from DevDay.

GPT-6.1 Sol looks good.

It's cheaper. It's better at coding, professional work, and computer use. OpenAI says it gets close to Astra on several agentic evaluations at dramatically lower cost.

Great.

Six months ago, that might have been the keynote.

Now it feels like an ingredient.

During the livestream, I said that eventually most people won't know which model their agent is using underneath.

I think we're moving toward that faster than I expected.

Your agent could use Astra for a difficult planning problem, Sol for coding, Luna for cheap classification, another company's model for a niche task, then hand a job to a specialist agent somewhere else.

You care about the result.

You probably care about the bill.

The logo stamped on every individual inference becomes much less important.

The competitive advantage moves upward into orchestration:

**Who understands you? Who holds your context? Who has your permissions? Who can reach the tools? Who chooses the specialist? Who handles payment? Who earns your trust?**

OpenAI spent DevDay building an answer to every one of those questions.

## Watch what becomes invisible

Five or ten years from now, I doubt people will celebrate "computer use" as a standalone AI feature.

Your computer will simply be easier to use.

You'll still have screens. You'll still click things when clicking is useful. Developers will still care deeply about operating systems, APIs, model choices, security boundaries, and all the machinery underneath.

Normal people will increasingly say what they want done.

The painful parts of computing can slowly fade into the background: moving data between applications, finding buried buttons, remembering which account owns what, waiting for one task before starting another, checking dashboards for changes, and rebuilding context every morning.

The next signal to watch is therefore less glamorous than another benchmark.

**How much real work will people actually hand these agents and walk away from?**

If Dots starts handling invoicing, project maintenance, scheduling, software fixes, research updates, purchases, and company processes without constant supervision, ChatGPT becomes something very different from an app you open.

It becomes the layer you open everything else through.

And if people hesitate because the agents are expensive, confusing, unreliable, or too risky to trust with meaningful access, the whole vision stalls no matter how good GPT-6.1 looks on a chart.

That is the test OpenAI set for itself at DevDay.

The agent has the computer now.

Next it has to earn the keys.
