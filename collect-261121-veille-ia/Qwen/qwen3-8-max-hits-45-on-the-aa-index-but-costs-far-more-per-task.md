---
id: collect-261121-veille-ia/qwen/qwen3-8-max-hits-45-on-the-aa-index-but-costs-far-more-per-task
title: "Qwen3.8 Max Hits 45 on the AA Index, but Costs Far More Per Task"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: ["2026-08-03"]
keywords: ["qwen3", "task", "reasoning", "index", "more", "coding", "model", "alibaba"]
source: https://www.intelligentliving.co/qwen3-8-max-update-smarter/
source_anchor: ""
source_lines: [1, 217]
section: "Qwen"
---
For the past two years, the pattern has been almost predictable: an American lab ships a frontier model, and a few weeks later a Chinese lab ships something nearly as good for a fraction of the price. DeepSeek did it, Qwen has done it repeatedly, and each time the market reacts as if the economics of AI just changed overnight.

Alibaba’s September 2 update to Qwen3.8 Max breaks that pattern in an interesting way. The model did get smarter, jumping from a score of 40 to 45 on the Artificial Analysis Intelligence Index v4.3, putting it level with Z.ai’s GLM-5.3 and within striking distance of the top American models. But it also got roughly twice as expensive to use per task, and unlike almost every Chinese flagship before it, it now costs more per task than some of the very models it is chasing.

The twist is that Alibaba did not raise its prices. The token pricing stayed exactly the same. The model simply started talking a lot more, and on a metered API, verbosity is money.

## A Quiet September Update, and What Actually Changed

Qwen3.8 Max first launched on August 3, 2026, as the most capable model Alibaba has ever released: a 2.4 trillion parameter mixture-of-experts design with roughly 95 billion active parameters per token and a one million token context window. On September 2, Alibaba pushed an upgraded snapshot named Qwen3.8-Max-0902. Same architecture, same pricing, and essentially the same context window, with post-training focused on coding and long-horizon agent work.

On paper it is a free upgrade. In practice, the numbers moved in two directions at once.

| Metric (Artificial Analysis) | Qwen3.8 Max (Aug 3) | Qwen3.8 Max 0902 (Sep 2) | 
|---|---|---|
| Intelligence Index v4.3 | 40 | 45 | 
| Cost per task | $2.67 | $5.41 | 
| Output tokens per task | 63k | 108k | 
| Reasoning tokens per task | 44k | 71k | 
| Code Arena WebDev rank | 4th (1,669) | 1st (1,691) | 
| API pricing (input/output per 1M) | $2 / $6 | $2 / $6 | 

The coding gains are real and independently measured. The updated snapshot took first place on Code Arena’s WebDev leaderboard, edging ahead of Claude Opus 5 Max, and Alibaba reports improvements across all eight coding benchmarks it publishes. The part that deserves more attention is the token column. The new version’s reasoning tokens alone, 71k per task, now exceed the entire output of last month’s version, reasoning and answer combined.

## Version vs. Version: Where the Gains Came From

A single index score hides as much as it reveals, so it is worth looking at how both versions did across every evaluation Artificial Analysis run. The picture that emerges lines up almost exactly with Alibaba’s stated post-training focus: agentic and terminal work improved sharply, knowledge stayed flat, and a couple of specialized tasks actually slipped.

| Benchmark (Artificial Analysis) | Qwen3.8 Max (Aug 3) | Qwen3.8 Max 0902 (Sep 2) | 
|---|---|---|
| Finance & Accounting Index | 41 | **46** | 
| AA-Briefcase (agentic knowledge work) | 44% | **56%** | 
| GDPval-AA v2 (real-world work tasks) | 57% | **59%** | 
| AutomationBench-AA (agentic SaaS workflows) | 49% | **56%** | 
| Terminal-Bench 4.0 (agentic coding) | 19% | **39%** | 
| SciCode (coding) | **53%** | 52% | 
| Humanity’s Last Exam (reasoning & knowledge) | 43% | 43% | 
| GDP.pdf (professional document reasoning) | 20% | **23%** | 
| CritPt (physics reasoning) | **20%** | 18% | 
| AA-Omniscience Accuracy | 32% | 32% | 
| AA-Omniscience Non-Hallucination Rate | 58% | **71%** | 
| AA-LCR v1.1 (long context reasoning) | 78% | **80%** | 
| τ3-Banking (agentic tool use) | **51%** | 48% | 
| MMMU-Pro (visual reasoning) | 82% | **83%** | 

The standout result is Terminal-Bench 4.0, where the score doubled from 19% to 39%, exactly the kind of long-horizon terminal and coding work Alibaba said it targeted. AA-Briefcase and AutomationBench-AA each gained around seven to twelve points, and the non-hallucination rate jumped from 58% to 71%, meaning the model got both more capable and more willing to admit uncertainty.

The trade-offs are visible too. Raw science coding (SciCode) and physics reasoning (CritPt) slipped slightly, banking-style tool use dropped three points, and Humanity’s Last Exam did not move at all. This is not a uniformly smarter model; it is one re-tuned for agentic work, with the extra reasoning tokens concentrated where the gains appear.

One small detail invites speculation. The 0902 snapshot lists a 984k token context window, whereas the original advertised a full one million tokens. The missing headroom could reflect internal instructions or reserved system space, and if the update quietly added heavier built-in prompting, that could also be part of why it deliberates so much longer. Alibaba has not commented, so treat that as informed guesswork rather than fact.

## Qwen3.8 Max on the Frontier Leaderboard

So where does a score of 45 actually sit? Artificial Analysis runs every model through the same ten evaluations, from agentic knowledge work to physics reasoning, and folds the results into a single index. Against the current field, Qwen3.8 Max 0902 lands in a crowded tier just below the true frontier, and roughly level with Meta’s much cheaper Muse Spark 1.3 at its default reasoning setting.

| Model | Intelligence Index v4.3 | Cost per task | 
|---|---|---|
| Claude Fable 5.1 (max) | 53 | $7.63 | 
| GPT-6 Astra (max) | 53 | $3.26 | 
| Claude Opus 5 (max) | 51 | $5.86 | 
| Muse Spark 1.3 (max) | 48 | $1.60 | 
| GPT-5.6 Sol (max) | 47 | $1.99 | 
| **Qwen3.8 Max 0902** | **45** | **$5.41** | 
| GLM-5.3 (max) | 45 | $2.01 | 
| Grok 4.6 (high) | 44 | $1.86 | 
| Kimi K3 (max) | 44 | $2.00 | 
| DeepSeek V4.1 Flash (max) | 40 | $0.27 | 

The table tells the story better than any single number. Qwen3.8 Max matches GLM-5.3 point for point on intelligence, then charges more than 2.5 times as much per task. It beats Grok 4.6 and Kimi K3 on score while costing them both roughly triple.

And it still falls several points short of Fable 5.1, GPT-6 Astra, and Opus 5, the models that define the current frontier. On cost, it lands in genuinely awkward company: cheaper per task than Claude Opus 5 and Claude Fable 5.1, but more expensive than GPT-6 Astra, which is the smarter model.

Output speed is the other sore spot. At roughly 40 tokens per second, Qwen3.8 Max ranks 148th out of 199 models in its class on Artificial Analysis. That is in line with most frontier reasoning models, which trade speed for depth, but Muse Spark 1.3 manages about 238 tokens per second at a similar intelligence score, and DeepSeek’s V4.1 Flash runs five times faster for a fraction of the cost.

## Qwen3.8 Max vs. the Field, Benchmark by Benchmark

### Against the American frontier

Artificial Analysis also runs the updated Qwen3.8 Max 0902 against the strongest models on the board under identical conditions, and the breakdown below is the fairest look available at exactly where Alibaba’s flagship now stands versus the frontier. It holds its own on several agentic evaluations, but the gap on knowledge-heavy tasks is still wide.

| Benchmark (Artificial Analysis) | Qwen3.8 Max 0902 | Claude Fable 5.1 (max) | GPT-6 Astra (max) | Claude Opus 5 (max) | Muse Spark 1.3 (max) | 
|---|---|---|---|---|---|
| Intelligence Index | 45 | **53** | **53** | 51 | 48 | 
| AA-Briefcase (Elo) | **1,622** | **1,662** | 1,562 | 1,645 | 1,589 | 
| GDPval-AA v2 (Elo) | 1,689 | **1,764** | 1,580 | 1,735 | 1,703 | 
| AutomationBench-AA | 56% | 59% | **68%** | 57% | 58% | 
| Terminal-Bench 4.0 | 39% | 52% | **59%** | 49% | 33% | 
| SciCode | 52% | **63%** | 56% | 56% | 59% | 
| Humanity’s Last Exam | 43% | **59%** | 55% | 55% | 49% | 
| GDP.pdf | 23% | 26% | **31%** | 22% | 27% | 
| CritPt | 18% | 30% | **32%** | 29% | 25% | 
| AA-Omniscience Index | 12 | **43** | **43** | 37 | 25 | 
| AA-LCR v1.1 | 80% | **85%** | 81% | 79% | 83% | 
| Blended price per 1M tokens | $1.18 | $7.18 | $7.70 | $3.85 | **$0.78** | 
| Cost per task | $5.41 | $7.63 | $3.26 | $5.86 | **$1.60** | 
| Output tokens per task | 108k | 78k | **27k** | 73k | 60K | 

Several things jump out. On agentic knowledge work, Qwen3.8 Max 0902’s AA-Briefcase Elo of 1,622 is within reach of Opus 5 and ahead of both GPT-6 Astra and Muse Spark, and its long-context reasoning beats Opus 5 outright. But on knowledge-heavy evaluations the frontier pulls away: Humanity’s Last Exam, SciCode, and CritPt all show sizeable gaps, and its AA-Omniscience index of 12 against 25 to 43 shows hallucination control remains its weakest trait even after the update’s big jump in non-hallucination rate.

The other eye-opener is GPT-6 Astra‘s token discipline. It reaches 53 on the index while generating just 27k output tokens per task, less than half of what any other model here uses, which is how it stays at $3.26 per task despite charging $10 and $50 per million tokens. That is the polar opposite of Qwen’s approach and the clearest possible illustration that per-token price and real-world cost are two different things.

### Against its closest score rivals

The second table groups Qwen3.8 Max 0902 with the models closest to it on the leaderboard, GLM-5.3 first among them: both sit at exactly 45, making Z.ai’s model Qwen’s truest comparison point. Kimi K3 and Grok 4.6 trail by a single point, and DeepSeek’s V4.1 Flash anchors the budget end.

| Benchmark (Artificial Analysis) | Qwen3.8 Max 0902 | GLM-5.3 (max) | Kimi K3 (max) | Grok 4.6 (high) | DeepSeek V4.1 Flash (max) | 
|---|---|---|---|---|---|
| Intelligence Index | **45** | **45** | 44 | 44 | 40 | 
| AA-Briefcase (Elo) | **1,622** | 1,511 | 1,488 | 1,534 | 1,424 | 
| GDPval-AA v2 (Elo) | **1,689** | 1,655 | 1,551 | 1,643 | 1,632 | 
| AutomationBench-AA | 56% | 62% | 58% | 67% | **69%** | 
| Terminal-Bench 4.0 | 39% | **42%** | 13% | 21% | 27% | 
| SciCode | 52% | **59%** | **59%** | 56% | 52% | 
| Humanity’s Last Exam | 43% | 42% | **47%** | 43% | 39% | 
| GDP.pdf | **23%** | 11% | 22% | 17% | 13% | 
| CritPt | 18% | 19% | **23%** | 17% | 14% | 
| AA-Omniscience Index | 12 | 14 | 20 | 30 | −5 | 
| AA-LCR v1.1 | 80% | 80% | **89%** | 80% | 84% | 
| Blended price per 1M tokens | $1.18 | $0.90 | $2.31 | $1.35 | **$0.18** | 
| Cost per task | $5.41 | $2.01 | $2.00 | $1.86 | **$0.27** | 

Against this group, Qwen3.8 Max 0902 posts the strongest agentic knowledge work and real-world task scores of the bunch, and it dominates terminal coding, more than tripling Kimi K3’s result. But GLM-5.3 matches its headline 45 while beating it on automation workflows, science coding, and physics reasoning, and Z.ai’s model costs less than $2 per task against Qwen’s $5.41. Qwen also trails on knowledge reliability, where only DeepSeek’s Flash scores worse. The 0902 update bought Alibaba genuine agentic capability, but GLM-5.3 remains the reference point for what this tier of intelligence is supposed to cost. That reference point has already shifted again, with StepFun’s Step 5 Preview matching Grok 4.6 and Kimi K3 at 44 on the Intelligence Index for just $0.71 per task.

## Why Is This Model So Expensive?

Here is the part that trips people up: Qwen3.8 Max’s sticker price is not expensive at all. At $2 per million input tokens and $6 per million output tokens, with cache hits at $0.25, it undercuts Grok 4.6, Kimi K3, and every Claude model on per-token pricing.

The cost per task metric simply multiplies those prices by the number of tokens a model actually burns to finish the job, and this is where Qwen3.8 Max stumbles.

Across the full Intelligence Index, the updated model generated 190 million tokens, against a median of 90 million for comparable models. Artificial Analysis flags it bluntly as “very verbose.”

Post-training appears to have taught the model to deliberate at much greater length, or Alibaba raised its internal reasoning limits, and since every reasoning token is billed at the output rate, the bill arrives whether the extra thinking was warranted or not. The intelligence gain is genuine, but roughly half of the price jump per task comes from the model’s new habit of thinking out loud.

Two things should temper the sticker shock, though. First, Qwen3.8 Max ships with seven reasoning levels, defaulting to xhigh:

- off
- minimal
- low
- medium
- high
- xhigh (default)
- max

The benchmarks we have do not state which level was used, which means the 45 score is probably a default-setting number. Users who dial reasoning down will pay meaningfully less, and anyone who dials it up to max may buy a few more points of intelligence at additional cost. Second, the community has already built a playbook for taming exactly this behavior in the smaller Qwen3.8-27B open model. Because the 27B model shares Qwen’s habit of defaulting to maximum reasoning, users have published workarounds everywhere from Reddit’s LocalLLaMA community to Hugging Face model discussions and GitHub guides: concise system prompts, lowered reasoning effort, or disabling the thinking block outright. Reported results range from token cuts of 70 to 80 percent to multi-fold speedups, though it is not consistent across all tasks. If the same techniques carry over to the flagship’s API, Qwen3.8 Max’s effective cost per task starts looking far more competitive than the raw benchmark suggests.

There is a bigger reason to take the pricing seriously, and it is not really about Alibaba’s choices at all. As Chinese AI models have historically been so much cheaper than American ones, the assumption was that the discount would last forever. But China’s access to top-tier compute is now the binding constraint, and the numbers coming out of the supply chain suggest the era of ever-cheaper Chinese inference may be hitting a wall.

## When Cheap Chips Run Out: The Compute Wall

In early September, Bloomberg reported that DeepSeek plans to deploy at least 160,000 of Huawei’s next-generation Ascend 950DT accelerators at a gigawatt-scale data center in Inner Mongolia, an order worth roughly $2.6 billion. It would be one of the largest clusters of domestically produced Chinese AI chips ever assembled. There is just one problem: Huawei cannot build the chips fast enough to fill it.

Shortages of high-bandwidth memory are expected to cap 2026 output of the 950DT in the low hundreds of thousands, and filling DeepSeek’s order alone could take more than a year. Advanced packaging capacity is similarly tight, and China’s leading memory producer is still running sample batches rather than mass production. The chip design is not the bottleneck; the components around it are.

This is the environment Qwen3.8 Max is being served from. NVIDIA’s share of China’s AI chip market is forecast to collapse from around 40% in 2025 to roughly 8% in 2026, while domestic alternatives ramp on a timeline measured in years.

Several of the large data centers Chinese AI companies are now breaking ground on will not reach full capacity for three to four years. Even Huawei’s existing infrastructure pushes the envelope of what is deployable today, as our earlier look at Huawei’s 384-chip Ascend supernodes showed.

Alibaba, for its part, has been quietly building a hardware escape hatch of its own. Its T-Head subsidiary designed the Zhenwu 810E accelerator, 10,000 of which now power a data center in Shaoguan operated with China Telecom, with an expansion target of roughly 100,000 chips, as we covered when the Zhenwu cluster came online. In May, Alibaba followed up with the Zhenwu M890, claiming triple the performance with 144 GB of memory, explicitly aimed at the long-context agent workloads that models like Qwen3.8 Max generate, plus a roadmap stretching to the V900 in late 2027 and J900 in 2028.

There is even a RISC-V angle: Alibaba’s XuanTie C950, a 64-core server CPU with integrated matrix acceleration, recently ran the open-weights Qwen3.8-27B at 30 tokens per second with no GPU at all and coaxed the flagship 2.4 trillion parameter Qwen3.8 Max itself to a workable, if humble, 7.2 tokens per second, as our XuanTie C950 coverage detailed.

The catch is tempo. The M890 is only just reaching customers, the next big gain arrives in late 2027, and real-world serving capacity depends on memory and packaging supply that remains tight across the entire Chinese industry. Custom silicon gives Alibaba more control over its destiny than most, but it does not fast-forward the calendar.

Alibaba has never competed primarily on being the absolute cheapest, and Qwen3.8 Max’s pricing reflects a company that can pick its margins. But when the most capable Chinese models now start to cost more per task than Grok 4.6, Kimi K3, or Muse Spark, while the compute needed to serve them grows scarcer and more expensive, it suggests the unlimited-cheap-pricing era for Chinese frontier models may already be ending. DeepSeek’s recent API price hike, followed by only a partial rollback for its new Flash model, pointed the same direction, as we covered in our analysis of DeepSeek’s price increase and the alternatives.

## The Open-Weights Counterweight

Alibaba’s strategy has a second track that complicates any simple “China is getting expensive” narrative: while the flagship Qwen3.8 Max stays closed and metered, the company keeps open-sourcing models that are embarrassingly good for their size. And it is not alone. The open-weights movement across Chinese and American labs alike is arguably the bigger story.

Qwen3.8-27B, which scores 34 on the same index, remains one of the most capable open models anyone can run on consumer hardware, and we have since tested how it stacks up against Nanbeige’s 33B agent specialist. Qwen3.8-Flash matches DeepSeek’s V4 Pro on coding benchmarks at a quarter of the price, as we covered when the Qwen3.8-Flash results landed. Both ship with open weights, so the practical floor for capable AI keeps dropping even as the flagship gets pricier: individuals and small companies can run these models locally or on cheap cloud instances, bypassing metered APIs entirely. The same logic now reaches further down the stack, as compressed ternary releases like Bonsai 2 27B fit a Qwen 3.8 27B derivative into 5.9 GB and run on phones and laptops with no per-token cost at all.

The open-weights budget tier around them has never been more competitive. DeepSeek’s new MIT-licensed V4.1 Flash release scores 40, matching last month’s Qwen3.8 Max, at roughly 20 cents per task. Z.ai’s GLM-5.3-Flash reaches an index score of 42, also with open weights. Both can be self-hosted, which is the point: when anyone can run these models on their own hardware or on cheap cloud instances, the price floor keeps falling regardless of what the flagship APIs charge.

The cheapest options overall are not open at all, though, so they belong to a slightly different conversation. Singapore-based Agnes offers Agnes 3.0 Flash for free as a closed model, benchmarking close to DeepSeek’s V4 Pro. And Meta’s Muse Spark 1.3, also closed-source, delivers near-Opus capability for pocket change, especially for contributors who opt into its data program, which drops its effective pricing toward DeepSeek Flash territory. That two-tier picture, open models setting the self-hosting floor while closed models compete ferociously on hosted price, is what Qwen3.8 Max’s $5.41 per task now has to justify.

Below even those sits the ultra-cheap tier, worth a quick mention because it shows how far price compression can go. Xiaomi’s MiMo V2.5 and V2.5 Pro score just 26 on the Intelligence Index, well short of frontier level, but at roughly $0.05 and $0.02 per task they are the cheapest models on the board, and they kept those prices even when DeepSeek raised its own rates. Only Agnes and genuinely free offers beat them on value, and the open-source Inkling, free on OpenRouter, also sits at 26, making it one of the best free models available. MiniMax’s open-source M3, at a score of 30, is another cheap Chinese entry, more expensive than DeepSeek but part of the same pattern of Chinese labs shipping competitive open models at rock-bottom prices. Xiaomi’s next generation, MiMo X and MiMo X Pro, is in beta now, and if it follows the same pricing philosophy at higher benchmark scores, it will be one to watch.

In other words, the “expensive Chinese model” story applies to exactly one model at exactly one setting. Everything below the frontier, whether open weights or closed APIs, is still getting cheaper and faster.

## A Frontier-Chasing Release Meets the Slowdown Debate

The timing of the 0902 update was awkward in a way nobody planned. Days earlier, Anthropic CEO Dario Amodei published an essay titled “We Must Pace the Frontier,” arguing that building too fast is reckless, and was quickly joined by OpenAI’s Sam Altman and xAI’s Elon Musk in backing a slowdown. Anthropic’s accompanying report on misuse of its models, spanning cyber operations to biological weapons attempts, gave the call unusual urgency.

President Trump’s response arrived live on stage. While NVIDIA CEO Jensen Huang was speaking at the All-In Summit, Trump phoned in and was put on speaker for the whole audience. The slowdown push, Trump said, plays into the hands of people who do not want America to lead, “It could also be China. And we’re not going to let that happen. It’s a hoax.” Huang’s reply, met with applause: “You’re right. We’re not going to let that happen, sir.” Notably, the exchange carried no signal that chip sales to China would be reopened, which is the concession the industry actually watches for.

Into that debate lands a Chinese model that scores 45, sits alongside the best open weights in the world, and ships a week after its own predecessor’s benchmark run. It is not passing Fable 5.1 or GPT-6 Astra, but it is close enough that the gap arguments used to justify export restrictions look thinner with every release. Whether the response is faster American development, tighter export controls, or a negotiated slowdown, models like Qwen3.8 Max are the reason all three options are on the table.

## What Comes Next

The pipeline on both sides is unusually full. DeepSeek promised a V4.1 Pro model alongside its Flash release, and it has not arrived yet, though expectations are that it will approach frontier scores at DeepSeek’s usual pricing. Xiaomi is beta testing its next generation, currently named MiMo X and MiMo X Pro, with pricing and benchmark positions still unknown.

- **DeepSeek V4.1 Pro:** promised alongside V4.1 Flash, not yet released, expected near frontier scores.
- **Xiaomi MiMo X / MiMo X Pro:** in beta, pricing unknown, but the MiMo line’s track record suggests it will undercut almost everything on price.
- **Qwen4 family:** architecture already previewed in the open-weights Qwen3.8-Flash-Next release, with the first flagship now live as Qwen4-Max.

On Alibaba’s side, the most interesting forward signal is not about Max at all. In its recent Qwen3.8-Flash-Next release, which opened the weights of a new hybrid-architecture model, the team described it plainly as “an early preview of the architecture used in Qwen4,” following the same playbook that saw Qwen3-Next preview the Qwen3.5 generation. Whatever the compute constraints, Chinese labs are already building the next generation, and in Alibaba’s case, giving away the architecture while they do it.

## Frequently Asked Questions

### Is Qwen3.8 Max the highest-ranked Chinese AI model?

On the Artificial Analysis Intelligence Index v4.3, Qwen3.8 Max 0902 and Z.ai’s GLM-5.3 (max) are tied at 45 as the top-scoring Chinese models, well ahead of DeepSeek’s best current entries. GLM-5.3 achieves the same score for roughly $2 per task versus Qwen’s $5.41, though Qwen holds a clear lead on coding leaderboards like Code Arena WebDev.

### Why did Qwen3.8 Max get more expensive if the price did not change?

Its per-token pricing stayed at $2 per million input and $6 per million output tokens. What changed is how many tokens the model uses: the 0902 update roughly doubled output tokens per task, from 63k to 108k, largely through longer reasoning. Since reasoning tokens bill at the output rate, cost per task rose from $2.67 to $5.41 even though the rate card is identical.

### Can the cost per task be reduced?

Probably. Qwen3.8 Max supports seven reasoning levels and defaults to xhigh, so lowering the effort setting should cut token usage and cost. Community experience with Qwen’s smaller models also shows that explicit instructions to keep responses concise can slash output tokens with minimal quality loss.

### Is Qwen3.8 Max open source?

No. Qwen3.8 Max is a proprietary API model. Alibaba open-sources its smaller siblings instead, including Qwen3.8-27B and Qwen3.8-Flash-Next, which are widely regarded as among the best open models available for their size.

### Does this mean Chinese AI models are no longer cheap?

The frontier-tier model is no longer the bargain it would have been a year ago, and compute constraints around Huawei’s chip production suggest pricing pressure is real. But below the frontier, Chinese models remain dramatically cheaper than American alternatives, and the open-weights ecosystem keeps pushing costs down further.
