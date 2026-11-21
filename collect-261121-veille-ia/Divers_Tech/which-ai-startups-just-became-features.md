---
id: collect-261121-veille-ia/divers-tech/which-ai-startups-just-became-features
title: "Which AI Startups Just Became Features?"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["agent", "just", "openai", "openais", "became", "code", "startups", "features"]
source: https://aisignal.veletica.com/p/which-ai-startups-just-became-features?img=https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F299f3825-a8c1-4f31-bfcf-cbd8094327af_1920x1920.jpeg%22&open=false
source_anchor: ""
source_lines: [1, 150]
section: "Divers_Tech"
---
In 2013, Apple put a flashlight button on the iPhone, and a whole category of flashlight apps quietly stopped mattering. Nobody held a funeral. People just stopped downloading them, the way we all quietly stopped owning alarm clocks.

On September 29, OpenAI’s DevDay may have done the same thing to a big chunk of the AI agent industry, except this flashlight bills three cents per 20 minutes.

So which AI startups just became features? My verdict: a meaningful chunk of today’s agent ecosystem may suddenly be sitting one layer too low in the stack. If a company’s main product is a part every agent needs, OpenAI now sells its own version of that part. If its product is knowing how one specific job gets done, it probably just got a cheaper engine.

By the end of this, you’ll know which agent parts now come built into OpenAI’s platform and what you can build with them this week. You’ll also get a table to hold up against every AI tool you pay for.

### First, what “one layer too low” means

Think of an AI agent like a house.

The model, the AI brain itself, is the foundation. The parts that let an agent actually do things are the plumbing and wiring: memory, a browser, a safe room to run code, and a manager that splits up the work. The app you use for your job is the room you live in.

For two years, a whole industry sold plumbing to people building on OpenAI’s foundation. On Tuesday, the foundation company started installing its own plumbing.

A quick decoder for the terms below:

- **Sandbox:** a sealed-off computer where an agent can run code without touching yours.
- **Orchestration:** the manager that decides what the agent does next.
- **Subagents:** helpers the main agent hands pieces of a job to.
- **Persistent execution:** the agent keeps working after you close your laptop, then picks up where it left off.
- **Agents API:** OpenAI’s developer kit for building agents, in public beta since September 10.

Use a startup when you need something the platform can’t do. Use the built-in feature when you just need the part to work.

### 1. What OpenAI Just Put in the Box

Seven pieces of agent plumbing now ship with OpenAI’s platform, most of them inside the Agents API:

- **Memory:** agent sessions keep their state between turns and resume where they left off. OpenAI’s always-on Dots also learn from your feedback.
- **Browser:** an agent can drive an OpenAI-hosted web browser and ask your permission before visiting
- each new website.
- **Sandbox:** an OpenAI-hosted Linux workspace with Python and Node.js, where the agent runs code and saves files.
- **Orchestration:** OpenAI runs the same managed harness that powers Codex, so keeping the agent on track and recovering from errors is now OpenAI’s job.
- **Persistent execution:** sessions are durable, and Codex Cloud keeps coding tasks running with your laptop closed.
- **Subagents:** the main agent can hand pieces of a job to helpers that work in parallel, each with its own context.
- **Integrations:** MCP connections and plugins, plus MCP events that let a connected app kick off an automation. Each Dot can connect to more than 4,000 apps.

Before anybody writes an obituary, look at who OpenAI invited to the party. The Agents API launch named nine sandbox partners, including Cloudflare, E2B, Modal and Vercel, and the new OpenAI Marketplace lists 32 partners where enterprise customers can spend part of their OpenAI commitment.

So some startups became features, and others became suppliers. Those are two very different Tuesdays. And this isn’t only an OpenAI move: Anthropic’s Claude Managed Agents also run in a cloud sandbox or one you host yourself.

### 2. What You Can Build Now That You Couldn’t Last Month

Last month, building an agent that could read websites and run code meant going shopping. You rented browsers from one company and a code sandbox from another, added a memory layer, and wrote the glue code that held it all together at 2 AM.

Now OpenAI says you can create an agent in a single API call by naming the task, the model, the tools, and the computer it runs on.

Here’s what that looks like for regular people with regular jobs:

- **A two-person online shop** builds an agent that checks public competitor pages each morning and drops a price summary into a file. They approve each website once, and they never touch a server.
- **A support lead** hands a messy customer case to one agent, which splits it across helpers for billing history, product logs, and past tickets. The main agent merges what they find into one answer.
- **A freelancer** builds a small agent that signs into a client portal with an email and a verification code to grab weekly reports. One catch: it can’t handle passkeys or QR-code sign-in yet.
- **A buyer at any company** gets a sharp new question for every AI vendor: “What do you do that’s not in the box now?”

The real shift is in who gets to build. An agent used to be a small infrastructure project with its own budget meeting. Now it’s closer to filling out a form, which opens the door for anyone who's ever had a good idea but no team of engineers.

### 3. The Feature Check Table

Hold every AI tool you pay for up against this. The right-hand column is where a specialist still earns its bill.

Most of that right-hand column comes down to working across AI companies or working inside your own walls. The rest is deep knowledge of one industry, and no platform ships that in a box.

### 4. The Real Cost

Everything above is paid and proprietary. OpenAI says the Codex harness underneath is open source, and you can read its code on GitHub, but the hosted service bills like everything else.

What actually generates the bill:

- **No platform fee.** OpenAI says the Agents API itself costs nothing extra. You pay for model tokens, tools, and sandbox time.
- **Tokens.** At OpenAI’s published rates for shorter prompts, GPT-6.1 Sol costs $2 per million input tokens, $0.10 per million cached input tokens, and $10 per million output tokens. GPT-6 Astra input costs $10 per million, five times Sol’s. (Tokens are the chunks of text an AI reads and writes. Cached input is text you reuse, like standing instructions, so it bills at a discount.)
- **Sandbox time.** Hosted sandboxes bill at OpenAI’s container rates: $0.03 per 20-minute session for 1 GB of memory, up to $1.92 for 64 GB. A footnote says eligible sessions bill by the minute, with a 5-minute minimum.
- **The quiet cost is switching.** When OpenAI runs your agent’s loop, more of your app depends on OpenAI’s design choices and prices. HyperFRAME Research flagged that trade-off on Oct. 1.
- **Data limits.** The Agents API keeps data only in the US and doesn’t offer zero data retention, the setting where OpenAI keeps none of your data. Using your own sandbox doesn’t change that.

And here's the twist. OpenAI didn't win on price. For a 20-minute run on a small sandbox, an independent pricing tracker estimates E2B at about $0.036 and Anthropic's agent runtime fee at about $0.027, right next to OpenAI's $0.03. Change the job's length or size, and that order can flip. The platform won by giving you one bill and one setup.

### 5. What They Said vs. What the Tests Showed

**Claim:** There are no additional fees for using the Agents API (OpenAI, Sept. 10, 2026).

**The evidence:** True for the API itself, but the hosted sandbox still bills container time. OpenAI’s pricing page lists two billing units for it, per 20-minute session and per minute with a 5-minute minimum for “eligible” sessions. TokenCost, an independent pricing tracker, flagged that the page never defines “eligible” (accessed Oct. 1, 2026). Budget at the 20-minute rate until OpenAI clarifies.

**Claim:** Customers report big gains. SafetyKit told OpenAI its cost per case dropped 60%, and Hypha said failed agent responses fell 86% (OpenAI’s launch page, Sept. 10, 2026).

**The evidence:** These are customer quotes that OpenAI picked and published. As of Oct. 1, I haven’t found an independent test comparing the Agents API with a self-built setup. Treat them as case studies, not averages.

**Claim:** Computer use lets agents operate software through its interface (OpenAI’s DevDay recap, Sept. 29, 2026).

**The evidence:** It does, with fine print in OpenAI’s own docs. Approving a website doesn’t force the agent to ask before each purchase or deletion, and sign-in doesn’t support passkeys or QR codes. AIReiter’s developer recap (Sept. 30) adds that your app still owns approvals, audit logs, and permissions. Don’t point it at your company credit card yet.

### The bigger thing

At OpenAI’s first DevDay in November 2023, the company launched the Assistants API, and plenty of people declared a wave of AI startups dead on the spot. On August 26, 2026, OpenAI shut the Assistants API down.

Sit with that for a second. The feature that was supposed to kill a generation of startups didn’t outlive a lot of them.

Platform features get replaced too. What lasts is knowing a job so well that the plumbing is the least interesting thing you sell.

If you build agent plumbing for a living, this week probably stings, and I’m sorry. For everyone else, it’s good news. The scariest part of building an agent just got handed to a company with a lot of servers. What’s left for you is the fun part, where you know something about a real job that nobody else does.

**Do this:** List every AI tool you pay for and run each one through The Feature Check Table. For any tool whose main job now shows up in the middle column, ask the vendor what they do that’s not in the box now.

**Skip this:** Rebuilding your whole setup this week. The Agents API is still in beta, and OpenAI says it will keep changing it quickly based on feedback.

**Wait on this:** The Agents API if you need data stored outside the US or zero data retention. Also wait on the Decisions API, which is still in limited preview.

**Steal this line:** “If your product is a part every agent needs, the platform will eventually sell that part.”

Go audit your subscriptions. Then cancel the one you forgot about, because we both know there’s one. I’ll see you in the next one.

### Sources

**Opening and verdict**

- OpenAI: DevDay 2026 Recap (Sept. 29, 2026). Backs the DevDay date, computer use arriving in the Agents API, Codex Cloud, the Decisions API’s limited preview, and the 32-partner Marketplace.

**First, what “one layer too low” means**

- OpenAI: Introducing the Agents API (Sept. 10, 2026). Backs the public beta date and the managed Codex harness.

**1. What OpenAI Just Put in the Box**

- OpenAI API docs: Agents API overview (accessed Oct. 1, 2026). Backs session state, resuming sessions, managed orchestration and recovery, US-only data residency, and no zero data retention.
- OpenAI API docs: Computer use in the Agents API (accessed Oct. 1, 2026). Backs the OpenAI-hosted browser and the approval before each new website.
- OpenAI API docs: OpenAI-hosted sandboxes (accessed Oct. 1, 2026). Backs the Linux workspace with Python and Node.js.
- The AI Insider: OpenAI DevDay Turns ChatGPT Into a Hub for Agents, Apps and Office Work (Sept. 30, 2026). Backs Dots connecting to more than 4,000 apps.
- GrandLinux: What Is the OpenAI Agents API? (Sept. 13, 2026). Backs Claude Managed Agents running in a cloud or self-hosted sandbox.

**2. What You Can Build Now That You Couldn’t Last Month**

- OpenAI: Introducing the Agents API (Sept. 10, 2026). Backs the single-API-call setup and the nine sandbox partners.

**4. The Real Cost**

- OpenAI API docs: Pricing (accessed Oct. 1, 2026). Backs Sol and Astra input prices, cached input prices, the container rates, and the per-minute footnote.
- AIToolsReview: OpenAI DevDay 2026, Everything Announced (Sept. 29, 2026). Backs Sol’s $10 output price.
- HyperFRAME Research: OpenAI DevDay Expands From Models to the Enterprise Agent Stack (Oct. 1, 2026). Backs the dependence trade-off.
- UsagePricing: The agent sandbox meter hasn’t converged (accessed Oct. 1, 2026). Backs the E2B and Anthropic sandbox estimates.

**5. What They Said vs. What the Tests Showed**

- TokenCost: OpenAI Agents API hosted sandbox pricing (accessed Oct. 1, 2026). Backs the two billing units and the undefined “eligible” sessions.
- AIReiter: OpenAI DevDay 2026 Recap for Developers (Sept. 30, 2026). Backs what your application still owns.

**The bigger thing**

- Second Order AI: Everything You Need to Know About OpenAI DevDay (Nov. 2023). Backs the Nov. 6, 2023 DevDay and the Assistants API launch.
- OpenAI Developer Community: Assistants API beta deprecation, August 26, 2026 sunset (accessed Oct. 1, 2026). Backs the Aug. 26, 2026 shutdown date.
