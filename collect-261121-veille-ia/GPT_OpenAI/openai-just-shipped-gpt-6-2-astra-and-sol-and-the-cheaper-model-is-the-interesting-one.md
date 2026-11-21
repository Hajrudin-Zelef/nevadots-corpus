---
id: collect-261121-veille-ia/gpt-openai/openai-just-shipped-gpt-6-2-astra-and-sol-and-the-cheaper-model-is-the-interesting-one
title: "OpenAI Just Shipped GPT-6.2 Astra and Sol, and the Cheaper Model Is the Interesting One"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["model", "astra", "gpt-6", "openai", "cheaper", "cost", "work", "just"]
source: https://dev.to/paullovelace/openai-just-shipped-gpt-62-astra-and-sol-and-the-cheaper-model-is-the-interesting-one-15ln
source_anchor: ""
source_lines: [1, 75]
section: "GPT_OpenAI"
---
OpenAI launched GPT-6.2 Astra and GPT-6.2 Sol on Tuesday, November 17, 2026. Astra is the new frontier model, built for demanding professional and scientific work. Sol is the versatile everyday model, faster and far cheaper. Both are available now in ChatGPT Work and Codex for Plus, Pro, Enterprise and Edu users, and both are on the API as `gpt-6.2-astra` and `gpt-6.2-sol`.

The headline most people will repeat is Astra: 20% cheaper than GPT-6 Astra, second on Artificial Analysis's Intelligence Index, and the first model OpenAI has ever released that it rates High for AI self-improvement. That's all real, and the safety rating deserves a serious look. But the model that actually changes what most developers should do is Sol — because on the coding benchmark FrontierCode 2.0, Sol trails Astra by just 0.5 points at one-fifth of the cost.

That gap, 0.5 points for 80% less money, is the whole story. This is not a review — I have not had hands-on time with either model, and nobody outside OpenAI has had much. It's what OpenAI actually announced, what the numbers say so far, and the questions worth asking before you change anything in your workflow.

Two models, arriving together, aimed at different jobs.

**GPT-6.2 Astra, the frontier model.** OpenAI calls it the most capable model it has ever built for professional and scientific work, trained to handle professional conventions in fields like law, finance and accounting more reliably. It's already the default model for many OpenAI employees internally, for technical and non-technical work alike. On Agents' Last Exam, a benchmark of multi-step professional workflows, Astra scores 5.2 points above GPT-6 Astra at less than a third of the cost, according to OpenAI. On ExploitBench Internal Port, a cyber benchmark, it jumps 22 points over GPT-6 Astra — OpenAI calls it its most capable model ever for cyber.

**GPT-6.2 Sol, the everyday model.** Compared with GPT-6.1 Sol, OpenAI says it is smarter, more efficient, and lets you complete more tasks on the same paid plan. It powers through multi-step agentic workflows, writes clearly, and is meant to be the model you reach for by default. The number that matters: on FrontierCode 2.0, which checks whether code changes meet the quality bar of real projects, Sol trails Astra by just 0.5 points at one-fifth the cost.

**Ultrafast mode spreads wider.** Pro 500 subscribers get Ultrafast with either model. For the first time, Pro 200 subscribers get Ultrafast too, starting with Sol. Ultrafast is also available on the API for both models at launch.

**Where you can use them:** ChatGPT Work and Codex only. This rollout does not add either model to regular Chat. Enterprise and Edu users need their workspace admin to switch them on. OpenAI has not mentioned free accounts or regional limits.

| Model | Input / 1M tokens | Cached input | Output / 1M tokens | 
|---|---|---|---|
| GPT-6.2 Astra | $8 | $0.40 | $40 | 
| GPT-6.2 Sol | $2 | $0.10 | $10 | 

Astra's input and output prices are 20% below GPT-6 Astra's, and cached input is 60% cheaper. Sol keeps GPT-6.1 Sol's pricing exactly. Astra's list price now matches Anthropic's Claude Fable 5.2, which Anthropic cut by 20% a week earlier — the two frontier models are now priced identically, and OpenAI says Astra beats Fable on several tests.

OpenAI's framing is about cost per finished task, not cost per token: both models deliver better results with fewer output tokens, so you get more done inside the same usage limits. From December, that framing gets sharper — Plus, Pro and Business plans will share one allowance across ChatGPT and Codex, measured in tokens, so a model that wastes tokens will cost you allowance twice over.

The frontier model always gets the launch-day attention. But look at the arithmetic from a developer's point of view.

If you are running coding agents — Codex, or your own harness on the API — the question is never "which model is best?" It is "which model is good enough for this task, at what cost?" Sol at one-fifth of Astra's price, trailing by half a point on FrontierCode 2.0, is the kind of trade most teams should take for most tasks. The expensive model becomes the one you escalate to: the hard bug, the security-sensitive review, the multi-step professional workflow where Astra's 5.2-point lead on Agents' Last Exam earns its keep.

That is also, pretty clearly, what OpenAI wants. Sol is the default-workhorse pitch: "super efficient and built to perform where it counts — the real world," in OpenAI's own words on launch day. The company is selling a two-model system, not a single flagship — Astra for the hardest work, Sol for everything else, with Ultrafast now reaching the cheaper Pro tier for the first time.

The honest caveat: FrontierCode 2.0 is one benchmark, and it's OpenAI's chosen showcase for Sol. Independent harnesses will tell us whether that 0.5-point gap holds up on real repos. But the direction is unmistakable — the gap between the flagship and the cheap model has narrowed to the point where defaulting to the flagship is a habit worth questioning, not a decision.

Astra is the first model OpenAI has released that it rates High for AI self-improvement. The accompanying GPT-6.2 system card assesses both models as Critical for cybersecurity and High for biological and chemical capabilities under OpenAI's Preparedness Framework, with only Astra also meeting High for self-improvement.

OpenAI is launching both models with tighter safeguards for higher-risk cybersecurity assistance, and plans to offer them through OpenAI Daybreak, which already gives vetted security researchers access to other models with fewer guardrails.

This is not a footnote. A model rated High for self-improvement, and Critical for cyber, is a model whose misuse case is real enough that OpenAI felt the need to say so in the launch announcement itself — on the same day it also boasted about Astra's 22-point jump on a cyber benchmark. The capability and the safeguard arrived in the same breath, and that is the right way to read them: the most capable cyber model OpenAI has ever shipped is also the one it is most careful about. If you build agents that touch code, credentials or infrastructure, that pairing is worth more of your attention than any benchmark score.

Artificial Analysis, which runs independent tests, ranks Astra second on its Intelligence Index at 62.1, behind Claude Fable 5.2 and ahead of Google's Gemini 4 Carbon and Anthropic's Claude Opus 5.5. Sol scores 57.3 at max effort. Astra is up from 57 for GPT-6 Astra on the same index.

For context, this is a fast-moving family: GPT-6 Astra arrived in September, GPT-6.1 Sol followed a few weeks later at DevDay, and now both lines have moved to 6.2 together. GPT-6.1 Astra never shipped — OpenAI pulled it in September after it failed the company's safety bar — so GPT-6.2 is the first Astra upgrade since GPT-6 Astra itself.

**If you use Codex or the API for everyday coding:** try Sol first. The 0.5-point FrontierCode gap at one-fifth the price is the best deal in this launch, if it holds up on your own work. Measure it on your own repo before you commit — one benchmark is not a migration plan.

**If you do demanding professional work — law, finance, accounting, scientific analysis:** Astra is your model, and the 20% price cut plus fewer output tokens make it cheaper than the model it replaces. The Agents' Last Exam lead is the number to watch.

**If you are on Pro 200:** you get Ultrafast for the first time, starting with Sol. That is a genuine upgrade to your plan, not just a new model name.

**If you are on Plus:** both models are available in ChatGPT Work and Codex today, but check whether your workspace has them switched on — Enterprise and Edu need an admin to enable them, and rollout is still progressing.

**If you build agents that touch production systems:** read the system card before you read the benchmarks. The High self-improvement rating and the tighter cyber safeguards are the part of this launch that will shape what you are allowed to build with these models.

- 
**How the 0.5-point Sol gap holds up independently.** FrontierCode 2.0 is OpenAI's showcase; third-party harnesses have not weighed in yet.
- 
**Real-world token usage.** "Fewer output tokens" is OpenAI's claim; your usage limits will tell you the truth within a week.
- 
**When — or whether — the models reach regular Chat.** This rollout is ChatGPT Work and Codex only.
- 
**What the December allowance change costs in practice.** One shared token allowance across ChatGPT and Codex changes how heavy users budget; the details will matter more than the announcement.
- 
**How the tighter cyber safeguards behave in practice.** More capable cyber model plus more safeguards is a combination developers will test quickly, and the friction will show up in real workflows.

OpenAI's pitch this time is not "bigger model, bigger number." It is "the cheap model is nearly as good, and the expensive model is cheaper than it was." That is a pricing story dressed as a launch story, and it is the right story for where this market is: the frontier models are converging on capability, so the fight has moved to cost per finished task.

Sol is the model to try. Astra is the model to escalate to. And the safety rating is the part of the announcement to take most seriously — the first High for self-improvement is a line being crossed in public, with safeguards attached, and the industry will be watching how that holds.

Are you on Codex or the API today? If Sol really does trail Astra by half a point at a fifth of the price on your own work, would you switch your default — or is the flagship still worth it for the tasks you care about? I'm curious where your line is.

*Sources: OpenAI, "GPT-6.2 Astra and Sol are now available" (community.openai.com, November 17, 2026); Thurrott, "OpenAI Launches GPT-6.2 Astra and Sol Models in ChatGPT Work and Codex" (November 17, 2026, https://www.thurrott.com/a-i/344003/openai-launches-gpt-6-2-astra-and-sol-models-in-chatgpt-work-and-codex); The Decoder, "OpenAI launches GPT-6.2 Astra and Sol with lower API prices and some benchmark gains" (November 17, 2026); Artificial Analysis Intelligence Index scores (Astra 62.1, Sol 57.3) as reported at launch. GPT-6.1 Astra history as reported by MadRobot. No hands-on testing was performed for this article — where a fact is OpenAI's claim, we say so. This breaking article is not the daily buyer-intent post, and it contains no affiliate links. AI disclosure: this article was drafted with AI assistance and checked against the OpenAI announcement and the sources above.*

*Also on ToolsDig: OpenAI GPT-6.2 Astra and Sol: the Cheap Model Is the Interesting One*

## Top comments (0)
