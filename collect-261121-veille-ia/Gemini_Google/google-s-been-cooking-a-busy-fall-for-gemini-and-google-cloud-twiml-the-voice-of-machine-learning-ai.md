---
id: collect-261121-veille-ia/gemini-google/google-s-been-cooking-a-busy-fall-for-gemini-and-google-cloud-twiml-the-voice-of-machine-learning-ai
title: "Google's Been Cooking: A Busy Fall for Gemini and Google Cloud | TWIML - The Voice of Machine Learning & AI"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "gemini", "cloud", "argon", "fall", "model", "introducing", "agent"]
source: https://twimlai.com/articles/googles-been-cooking-busy-fall-gemini-cloud
source_anchor: ""
source_lines: [1, 120]
section: "Gemini_Google"
---
By Sam Charrington

Just a few weeks ago, the AI hype machine seemed ready to write Google out of the AI race. OpenAI had released GPT-6 Astra and GPT-6.1 Sol, Anthropic was rolling out its Claude 5.5 family, and reports of key departures at DeepMind and delays to Google’s next-generation models were fueling questions about whether Google was falling behind.

Since then, Google has come roaring back.

It reminded me of a meme that’s been circulating for a while: Google, OpenAI, and Anthropic endlessly leapfrogging one another, with a “You are here” arrow pointing to whichever company most recently claimed the lead.

It’s a useful reminder of the dangers of recency bias in an industry where the narrative can change almost overnight.

And with its recent Gemini and Google Cloud announcements, Google has given us plenty to reconsider.

## Google Cloud’s Busy Fall

While OpenAI and Anthropic dominated the September hype cycle, Google closed out the month by announcing its long-awaited Gemini 4 frontier model, dubbed “Argon.” Google Cloud followed with a packed October calendar, introducing the Gemini Agent, expanding access to competing models, and rolling out infrastructure for building and running agents. The drumbeat has continued into November.

| Date | Announcement | 
|---|---|
| Sep 30, 2026 | Introducing Gemini 4 Argon | 
| Oct 8, 2026 | Gemini at Work 2026: Introducing the Gemini Agent | 
| Oct 13, 2026 | Gemini 4 Argon available on Google Cloud | 
| Oct 15, 2026 | Bringing the Borderless Lakehouse into the agentic era with innovations for Gemini | 
| Oct 20, 2026 | TorchTPU is now open source: Run native PyTorch on TPUs | 
| Oct 20, 2026 | Announcing llm-d 1.0: Open distributed inference for agents | 
| Oct 20, 2026 | Introducing Gemini for Government Services | 
| Oct 21, 2026 | Google Cloud expands model choice, bringing OpenAI models to customers | 
| Oct 22, 2026 | Expanding Claude availability and deployment choice on Google Cloud | 
| Oct 22, 2026 | Introducing Agent Compute Engine and expanded agent governance | 
| Oct 28, 2026 | Google announces Gemini 4 Carbon | 
| Nov 4, 2026 | Google announces updates to its Agentic Data Cloud | 
| Nov 5, 2026 | Agent Substrate 1.0 goes GA on GKE and joins the CNCF Sandbox | 
| Nov 9, 2026 | Google introduces TPU 8tx and TPU 8i Max | 
| Nov 10, 2026 | Gemini 4 Carbon is now available | 

Beyond these Google Cloud announcements, Google published over a dozen AI announcements during the same period. These included EmbeddingGemma 2, the Genie 4 world model, Gemini Omni 1.2 Flash, new Gemini tools for small business owners, and broader Argon availability in the Gemini app and for developers, with a separate post exploring its use in robotics.

That’s a lot to digest, so let’s take it layer by layer.

## Google in Its Element with New Models

The headline of Google’s fall is Gemini 4, which arrives not as a single model but as a family. It also arrives with a new naming convention. Gone, at least for now, are the version-number-plus-Pro/Flash labels of the Gemini 3.x era. In their place are chemical elements: Argon first, then Carbon, with more apparently on the way.

**Argon** was announced on September 30 and became available to developers and Google Cloud customers on October 13. Google pitches it as its “frontier model for real-world coding, enterprise knowledge work, and cyber defense,” and the self-reported numbers back up the positioning: state-of-the-art results on DeepSWE (77.9%) and long-video understanding (LVBench, 91.7%), the top spot on Zapier’s AutomationBench, and a lead on the Vals Index. Output length jumps to 1 million tokens, up from 64K, which matters a lot more for long-horizon agentic work than it does for chat. Introductory pricing is $2 per million input tokens and $10 per million output tokens (with cached input discounted 95%), rising to $4 and $20 after the introductory period.

**Carbon**, announced four weeks later on October 28, is the more capable of the two. It debuted at #1 on the Artificial Analysis Intelligence Index, putting it squarely in the conversation with the best that OpenAI and Anthropic have to offer. (Its reign was short: Anthropic’s Claude Fable 5.2 took the top spot a few days later.) Google is leaning hard into cybersecurity here, citing state-of-the-art results on vulnerability discovery (SEC-bench Pro), exploitation (ExploitGym), and patching (CWE-bench), along with long-running tasks. Internal teams, Google says, have run Carbon for days on multi-step engineering problems like root-cause analysis and bug patching across large codebases. Carbon is priced identically to Argon.

Carbon became generally available on November 10, and Google has published guidance on how to choose between the two. Both models share the same 1M-token context window, built-in tools, and per-token pricing. The difference is how many thinking tokens each one spends per turn. Carbon thinks longer to explore and verify its work, which means more tokens and more latency per turn. Google recommends it for complex root-cause debugging, architectural design, dense multimodal synthesis, scientific proofs, and security auditing. Argon is the efficient option, with faster responses and lower token spend, and Google positions it for most advanced software engineering and knowledge work, including interactive coding, day-to-day feature work, and fast production loops.

That resolves something that had been bugging me. With both models at the same per-token price, it wasn’t clear why anyone would pick Argon over the better model. The answer is that per-token price isn’t the whole bill. Because Carbon spends more tokens on every turn, it costs more per task and responds more slowly, which makes Argon the sensible default and Carbon the model you reach for when the problem justifies the extra spend.

But just because Carbon is less token-efficient than Argon doesn’t make it an inefficient model. In its GA announcement, Google says Carbon ranks #1 on CursorBench 4.0, Perplexity’s WANDR, and Zapier’s AutomationBench “at more than 2X lower cost per task compared to the next best external model.” By Google’s numbers, Carbon is pricier than its sibling but cheaper per task than the competition.

That framing also hints at how Google expects Carbon to be used. Alongside GA, Google previewed `/swarm` in Antigravity, which turns “a single prompt into a coordinated team of agents.” Developers can assign Carbon to “roles that demand the deepest reasoning,” like architectural design, coordination, and auditing, while “faster, lighter models” work in parallel in isolated workspaces. Carbon as the thinker and lighter models as the doers is an increasingly common pattern in agentic systems, and it’s a smart way to spend expensive reasoning tokens only where they count.

Whether it’s choosing between Carbon and Argon or mixing them with lighter models in a swarm, Google is giving users more nuanced choices than “Pro or Flash.” My read is that this is precisely why Google moved away from version numbers and Pro/Flash tiers. Element names give Google room to ship different checkpoints within the same model family, each with slightly different positioning, without the baggage of implying a strict “this replaces that” hierarchy. If that’s the plan, it suggests Google intends to ship models at a quicker pace than we’ve seen from it in the past. We’ll see whether customers find the periodic table easier to navigate than the old naming scheme.

If the rumor mill is right, we won’t have to wait long to see that quicker pace in action. Leakers have been reporting on a **Hydrogen** model aimed at the fast, cheap spot that Flash used to occupy, with checkpoints reportedly being A/B tested in AI Studio and LMArena, and there’s also chatter about a higher-performing **Krypton** model sitting above Carbon.

## Gemini Agent: The Universal Agent for Work

On October 8, I attended Google’s Gemini at Work virtual event, where the company introduced the Gemini Agent, which Google calls “your new single, universal agent for work.” (The event’s other catchphrase: “Work now starts in the prompt window.”)

Google pitches it as one agent with one interface that you give objectives rather than instructions. It runs persistently in the cloud, follows you across web, mobile, desktop, Workspace, Microsoft 365, and Slack, draws on company data, and routes each job to the right model, which today includes Claude as well as Gemini.

A few things stood out to me.

**The Docs experience.** My strongest reaction was to how the Gemini Agent showed up across Google Workspace. In the demo, you could @mention the agent in a comment, ask it to revise something, and see its suggested edits separately from your own. That’s the kind of human–agent co-writing experience I’ve been missing, and it lives in an editor I already use every day. Google says Gemini Agent will work inline across Gmail, Drive, Docs, Slides, Sheets, Chat, and Calendar.

**One agent that follows you around.** I also liked the idea of a single agent that has its own identity and a memory of your previous interactions, and that comes with you across Workspace. Google even lets you stand up “coworker agents” with their own Workspace accounts that only see what’s been shared with them. My hope is that this finally simplifies a lineup I’ve found confusing: Agentspace, Spark, Gemini Business, Gemini Enterprise, and the agent platform. My caution comes from experience. To try Spark, I had to create a separate Gmail account, and without my work or personal context attached, it just wasn’t very useful. I’ve seen the Gemini Agent demos but haven’t had a chance to use it myself yet, so I’m reserving judgment until I can put it to work with my own context.

**The data agent demo.** This one stood out for a different reason. In this demo a brand manager asks the agent to size a market and then build a propensity-to-buy model. The agent pulled data from BigQuery, produced charts, and trained an XGBoost model on Spark. The code generation itself wasn’t surprising; we’ve all seen agents write plenty of pandas and PySpark by now. What struck me was that a business user could ask for a trained ML model from a chat window. Just a few years ago, companies were worried they’d never be able to hire enough data scientists to do exactly that work.

Under the hood, all of this sits on top of the Gemini Enterprise Agent Platform, the successor to Vertex AI that I covered in my Google Cloud Next ‘26 recap back in April. If Next was about Google laying down the agentic control plane, Gemini at Work was about putting a face on it for end users.

## Google’s Multimodel Enterprise Strategy

Model choice has been part of Google Cloud’s story from the beginning, going back to the launch of Model Garden. But there were some notable gaps, and this fall Google closed the biggest one.

On October 21, Google announced that OpenAI models are coming to Google Cloud, starting with GPT-6 Astra, GPT-6.1 Sol, and GPT-6 Luna. Customers can build with these models through Google’s APIs, and, in a nice touch for developers, “connect their local Codex CLI and Desktop workflows directly to our Gemini platform for inference.” Usage is billed through Google Cloud Marketplace, so it lands on a single invoice and draws down existing cloud commitments. Access is in private preview for select enterprise customers for now, with Gemini for business and Antigravity integration “coming soon.” With this move, Google says it’s now the only hyperscaler offering Gemini, Claude, and GPT models side by side.

Anthropic’s models have been available on Google Cloud since 2024, but Google also added a new option for enterprises that want the latest Claude features the moment they ship: Anthropic-managed Claude Models on Google Cloud, which is now generally available alongside the existing Google-managed offering. Per the announcement, “Both options deliver the same frontier models on day zero, the same pricing, and the same Google Cloud billing. The difference comes down to who acts as your data processor and whether you need access to the new Claude platform features when they launch.”

In practice, the Anthropic-managed option gets Claude Platform features like Managed Agents, skills, and code execution on day zero, with Anthropic acting as data processor. The Google-managed option keeps Google Cloud as the processor, with VPC Service Controls, CMEK, regional data residency, and native integrations with services like BigQuery and Model Armor, but platform features arrive later as they’re integrated.

It’s a pragmatic arrangement, and I think it’s the right call. Enterprises want to choose their models, and increasingly they want to choose how much of a model provider’s platform comes along for the ride. That Gemini Agent can already route work to Claude models tells you how seriously Google is taking the multimodel story.

## Sandboxes for the Agentic Enterprise

Sandboxes have become a hot topic as one of the key ways to exert control over agent behavior. If an agent is going to write and run code, browse the web, and operate for hours or days on its own, you want a hard boundary around what it can touch. Google is working on two complementary technologies here.

**Agent Compute Engine** (ACE) is Google’s hosted option. It gives each agent its own hardware-isolated microVM running on Google’s Titanium offload infrastructure, with a cryptographically verified identity, short-lived credentials, and its own persistent disk for workspace and context. Idle agents can be checkpointed and paused, say, while waiting on a human approval, and restored from snapshots when there’s work to do. Notably, ACE is what Google uses to power the Gemini Agent itself. It’s slated for preview “in the coming months,” and Google announced expanded governance features alongside it, including Agent Gateway cost controls and model routing, and governance for local coding agents like Claude Code and Antigravity CLI.

**Agent Substrate** is the Kubernetes-native, open-source counterpart. It separates an agent’s state (memory, files, and identity, which it calls an Actor) from the compute it runs on (pooled sandboxes called Workers), so idle agents can be snapshotted and their sandboxes returned to the pool. Google says an agent can be restored on any available Worker in under 500 ms, with up to 10x the container density of traditional approaches. Operators can choose between Cloud Hypervisor microVMs or gVisor for isolation. Agent Substrate hit 1.0 and went GA on GKE on November 5, and Google has donated it, along with gVisor and OpenRL, to the CNCF Sandbox.

The split makes sense: teams that don’t want to manage clusters get ACE, and teams that want portability, or are already deep into Kubernetes, get Agent Substrate. Paired with the open-sourcing of TorchTPU and the llm-d 1.0 release, it also continues Google’s pattern of pushing key pieces of its agent infrastructure into open source.

## Expanding the TPU Family

At Google Cloud Next in April, I covered the launch of the eighth-generation TPUs: TPU 8t for training and TPU 8i for inference. On November 9th, Google expanded the family with two new systems.

**TPU 8tx** is pitched primarily as an inference system, “optimized for secure and confidential inference,” with small-scale training noted as a secondary use. That struck me as a bit odd, since it’s built on the same silicon as the training-focused TPU 8t rather than the inference-focused 8i. Google doesn’t spell out exactly why, but the post hints at the logic: 8tx pairs “training-grade compute with a fast inference network,” borrowing TPU 8i’s low-latency Boardfly interconnect for up to 2x lower latency than TPU 8t, so customers can run and customize models on the same system. It’s paired with AMD EPYC hosts and builds confidential computing into the hardware so that model weights and data stay protected and isolated while in use, not just at rest or in transit. Google says it “enables teams to fine-tune models using their private data, distill large models into smaller ones, and train their agents through reinforcement learning, all without needing to migrate sensitive data between separate clusters.” The target customers are regulated enterprises and governments, which lines up nicely with the Gemini for Government Services announcement last month.

**TPU 8i Max** goes after the memory bandwidth bottleneck that shows up when agents work across massive contexts like an entire codebase or hours of video. By putting two TPU 8i chips on a single package, Google is “delivering double the compute, double the SRAM and double the HBM bandwidth of TPU 8i” in the same rack footprint.

Google didn’t share availability dates or pricing for either system.

## Takeaways

Step back from the individual announcements and a clear picture emerges: Google continues to push forward at every layer of the AI stack. At the model layer, it has Gemini 4 Argon and Carbon. At the application layer, the Gemini Agent. At the platform layer, the Gemini Enterprise Agent Platform with real multimodel support spanning Gemini, Claude, and GPT. In data, the Agentic Data Cloud. And at the infrastructure layer, Agent Compute Engine, Agent Substrate, and an expanding TPU lineup. Few companies can claim to compete at all of these layers, and fewer still are shipping at every one of them in the same six-week window.

A few takeaways stand out for me:

- **Google shouldn’t be counted out.** The narrative a few weeks ago was that Google was falling behind. Carbon’s debut at the top of the Artificial Analysis leaderboard, however brief, and a deep bench of enterprise announcements say otherwise. The “You are here” arrow has moved again.
- **Model cadence is accelerating.** The shift to element-named model families, with Argon and Carbon four weeks apart and more reportedly on the way, suggests Google intends to deliver models at a faster clip than before.
- **Simplification remains the challenge.** Gemini Agent, Gemini Enterprise, Gemini (for) Business, the Agent Platform, Agent Compute Engine, Agent Substrate, Agent Sandbox, and so on. Google has a lot to offer, and it still struggles to organize and communicate it in a way customers can easily navigate. The Gemini Agent’s promise of “one agent” is the most encouraging sign yet that Google recognizes this.
- **Much is still in preview.** At Next, most of what Google announced had actually shipped. This fall’s news is more mixed: Agent Compute Engine is slated for preview “in the coming months,” OpenAI models are in private preview, the new TPUs have no availability dates, and Carbon spent two weeks gated behind the Fairwind Program before going GA. The roadmap is compelling, but customers will judge it by what they can actually run.

The hard part, as always, will be turning demos and previews into products customers can get their hands on. How developers split their work between Carbon and Argon, and how the Gemini Agent performs in the real world, will be the things to watch, and I’ll be watching closely.

*What do you make of Google’s fall? Has it changed how you think about the AI race? I’d love to hear from you.*
