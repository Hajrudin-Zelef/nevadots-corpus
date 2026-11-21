---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-debuts-fourth-globally-three-points-off-the-lead
title: "Kimi K3.1 Pro Debuts Fourth Globally, Three Points Off the Lead"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-04"]
keywords: ["kimi", "model", "three", "points", "open-weight", "index", "moonshot", "artificial"]
source: https://www.intelligentliving.co/kimi-k3-1-pro-debuts-fourth-globally/
source_anchor: ""
source_lines: [1, 154]
section: "Kimi"
---
Moonshot AI has released Kimi K3.1 Pro, the first model in its new K3.1 series and its most capable 3T-class model to date. Within a day of the October 23 launch, independent testing by Artificial Analysis placed it fourth among 215 models worldwide on the Artificial Analysis Intelligence Index, three points behind the top score.

The jump over its predecessor is the most striking part of the release. Kimi K3.1 Pro scores 55 on the index, eleven points above Kimi K3, and it now sits between the proprietary frontier and every other open-weight model in the world. Once Moonshot publishes the full model weights on November 4, it will become the highest-scoring open-weights model available anywhere.

Here is what the release delivers, what the numbers say against Claude, GPT, and DeepSeek, and the practical tradeoffs worth knowing before making a switch.

K3.1 Pro is a post-trained version of Kimi K3, built with substantially more post-training compute. To run reinforcement learning jointly across hundreds of downstream domains,

Moonshot says it reworked its entire training pipeline, from the reinforcement-learning framework to the sandbox infrastructure models practice in, so that large-scale training across hundreds of task domains delivers steady, predictable gains.

The underlying architecture is unchanged from K3. The model keeps 2.8 trillion total parameters with 104 billion active per token in a mixture-of-experts design, a one-million-token context window, and text plus image input.

It is a reasoning model with adjustable thinking effort, and alongside it Moonshot is shipping Kimi K3.1 Pro Highspeed, a variant the company says runs two to three times faster for more responsive interactions.

The launch caps a busy year for the Kimi lineup. In September, the K2.8 Preview quietly became the default coding checkpoint inside Kimi Code, and K3.1 Pro now lands above everything in the family as the new flagship.

Artificial Analysis computes its Intelligence Index as a weighted composite of ten evaluations spanning coding, agentic work, reasoning, and knowledge. Only three models in the world score higher than K3.1 Pro, and two of those are locked together at the same score: Claude Sonnet 5.5 and Muse Wonder Full.

The generational jump deserves context. When Kimi K3 scored 44 on the same index in October, it still led every open-weight model on the planet but sat well below the proprietary frontier. A single post-training refresh has moved Moonshot from the open-weight tier into direct contention with the closed labs, at least on this composite.

The open-weight implications are larger still. Artificial Analysis notes that once K3.1 Pro’s weights are published, the gap between the leading open-weight model and the leading proprietary model on this index will narrow from twelve points to three. The twelve-point baseline is today’s distance between Claude Opus 5.5’s 58 and the best published open-weight score of roughly 46, and the projection assumes that leaderboard holds until the weights publish, with no new release moving either end. Europe has joined the same race from the other side: Mistral’s Large 4 “Le Chonk” is a trillion-parameter open-weight release that claims the open lead on agentic coding and cybersecurity, though its independent intelligence scores sit well below the Chinese leaders.

How the jump was earned is the more interesting question, and Artificial Analysis has measured both generations on the same evaluations to answer it. The gains are strikingly lopsided. Terminal-Bench 4.0, the terminal-coding evaluation, went from barely clearing one task in ten to clearing two in three. The two agentic Elo benchmarks each moved by hundreds of points. The knowledge tests, by contrast, barely moved: SciCode and CritPt each gained just over a point, and Humanity’s Last Exam five and a half.

| Evaluation (AA-measured) | Kimi K3.1 Pro | Kimi K3 | Change | 
|---|---|---|---|
| Intelligence Index v4.3.2 | **55** | 44 | +11 | 
| Terminal-Bench 4.0 | **67.2%** | 12.6% | +54.6 | 
| GDPval-AA v2.1 (Elo) | **1,846** | 1,534 | +312 | 
| AA-Briefcase v1.1 (Elo) | **1,678** | 1,499 | +179 | 
| AutomationBench-AA | **73.3%** | 58.3% | +15.0 | 
| GDP.pdf | **31.4%** | 22.0% | +9.4 | 
| Humanity’s Last Exam | **52.5%** | 46.9% | +5.6 | 
| AA-Omniscience Index | **30** | 20 | +10 | 
| SciCode | **60.8%** | 59.5% | +1.3 | 
| CritPt | **24.6%** | 23.4% | +1.2 | 
| AA-AnalystAgent | **38.8%** | 38.8% | 0.0 | 
| Terminal-Bench Science 0.1 | **34.3%** | 8.1% | +26.2 | 
| AA-3D (Elo) | **1,110** | 982 | +128 | 
| MMMU-Pro (visual reasoning) | **84%** | 81% | +3 | 
| FrontierCode 1.1 | **47.1%** | 43.6% | +3.5 | 
| AA-LCR v1.1 (long context) | 86.3% | **88.7%** | -2.4 | 

*Both columns measured independently by Artificial Analysis on the same evaluations; Intelligence Index figures use version v4.3.2.*

The pattern reads as a model taught to work rather than a model taught more. Reinforcement learning across hundreds of downstream domains concentrates its gains exactly where multi-step execution and tool use live, while evaluations that track raw knowledge, which post-training barely touches, hold nearly steady. The single decline, two and a half points on the long-context evaluation AA-LCR, lands where K3 was already exceptional, so it reads as a small give-back from a very high base rather than a regression.

The comparison table below comes from Moonshot’s own runs with reasoning effort set to maximum, with rival scores taken from official leaderboards and technical reports. The independent picture sits a little higher: Artificial Analysis measured K3.1 Pro at 67.2 percent on Terminal-Bench 4.0 in its own harness, above Moonshot’s 65.7 percent figure and ahead of every model it tested, including Claude Sonnet 5.5. The two figures are not in conflict: they come from different evaluation setups, one vendor-run and one independent, and the 1.5-point spread between them is a working lesson in how much harness choice matters at this level.

Terminal-Bench 4.0 is the clearest single result: the benchmark evaluates coding agents on realistic terminal tasks with strict task-specific tests, and K3.1 Pro lands within a single point of Claude Opus 5.5 and well clear of GPT-6 Astra. No model of any kind measured higher, which makes this the largest generational leap of any evaluation in the release.

One note of caution before reading it: this table mixes Moonshot’s own runs with rival scores from different harnesses, so it is a vendor-assembled comparison rather than a controlled head-to-head. One row especially rewards skepticism: Terminal-Bench Science 0.1 shows K3.1 Pro at 38.1 against 63.3 for Opus 5.5 and 68.1 for Astra, a deficit of roughly thirty points on scientific terminal tasks that sits awkwardly against the five-fold leap on the ordinary Terminal-Bench. The most useful row is Agents’ Last Exam, which Moonshot reports for K3.1 Pro at 43.2, a point clear of Opus 5.5, though its configuration and rivalry coverage make it hard to compare directly with other vendors’ numbers. On Kimi Code Bench 3.0 Long-horizon, Moonshot’s internal coding evaluation, Astra’s 66.5 actually tops K3.1 Pro’s 61.2, a reminder that the vendor still trails at least one rival on its home turf.

Long-horizon coding is another strength. On SWE-Marathon, which tests very long coding jobs, K3.1 Pro leads every rival in the table, Claude Opus 5.5 included. BenchCAD, the 3D reconstruction benchmark, puts it behind only the two proprietary leaders when tools are allowed, and without tools the gap to Opus 5.5 narrows to a fraction of a point, one of the few settings where an open model draws nearly even. Moonshot says it works across Unity, Unreal Engine, and Godot, writes and runs scripts for Blender and CAD software without connectors, and ships with a new 3D Make feature on kimi.com for previewing, editing, and exporting models.

| Benchmark | Kimi K3.1 Pro | Claude Opus 5.5 | GPT-6 Astra | Claude Fable 5.1 | DeepSeek V4.1-Flash-1017 | GLM-5.3 | 
|---|---|---|---|---|---|---|
| Terminal-Bench 4.0 | 65.7 | **66.4** | 58.2 | 57.9 | 42.3 | 41.8 | 
| SWE-Marathon v1.1 | **51.7** | 46.3 | 42.5 | 45.6 | 40.0 | 42.5 | 
| DeepSWE v1.1 | 75.5 | 74.2 | 74.1 | 67.4 | **77.0** | 66.9 | 
| Terminal-Bench Science 0.1 | 38.1 | 63.3 | **68.1** | 40.0 | 15.7 | 8.1 | 
| Kimi Code Bench 3.0 Long-horizon | 61.2 | 58.5 | **66.5** | 58.8 | 51.5 | 45.5 | 
| Agents’ Last Exam | **43.2** | 42.6 | 32.8 | – | 37.7 | 26.2 | 
| BenchCAD (with tools) | 92.7 | **96.2** | 95.9 | 92.6 | 81.6 | – | 
| HLE-Full (with tools) | **70.0** | 67.7 | 57.2 | 65.6 | 58.8 | 62.5 | 
| OfficeQA Pro v2 | **64.4** | 62.2 | 54.4 | 54.5 | 40.4 | 31.1 | 
| Frontier Finance | 58.6 | 56.7 | 45.0 | **60.8** | 54.5 | 57.5 | 
| GDPval-AA v2.1 (Elo) | 1,846 | **1,866** | 1,573 | 1,754 | 1,746 | 1,650 | 
| OSWorld V2.1 | 79.6 | **81.8** | 75.8 | 80.7 | 69.3 | – | 
| GDP.pdf | **34.6** | 30.6 | 34.2 | 27.6 | 17.2 | 11.2 | 
| ChartOgraphy (with tools) | 90.2 | 89.0 | **91.0** | 88.4 | 83.0 | – | 
| ZeroBench main (with tools, pass@5) | **63.0** | 61.0 | 58.0 | 59.0 | 51.0 | – | 
| AutomationBench-AA | **73.3** | 69.5 | 68.5 | 59.4 | 66.6 | 62.2 | 

*Moonshot’s comparison figures from the Kimi K3.1 Pro quickstart guide, all models at maximum thinking effort; Claude scores may include fallbacks and GPT scores may include cyberguards.*

The strongest improvements show up in agentic work, where a model plans, calls tools, and finishes multi-step jobs on its own. K3.1 Pro ranks third globally on GDPval-AA, which rates models on real-world work tasks, behind only Claude Opus 5.5 and Muse Wonder Full, and jumps hundreds of Elo points past its predecessor. AutomationBench-AA tells the same story: a rise of more than fifteen points moves it from the middle of the field to the top of the table.

Finance is a focus area for this release. In Moonshot’s own comparison runs, K3.1 Pro posts the best OfficeQA Pro v2 score of any model it tested, ahead of Claude Opus 5.5, on a benchmark for complex fiscal analysis over real financial documents.

- **Financial Modeling:** 7.2 points better on formula linkage, financial logic, and final workbook quality in real-world modeling tasks.
- **Financial Research:** 11.6 points better at producing auditable research under evidence and cutoff constraints.
- **Business Operation:** roughly 4.2 times K3’s performance on long-horizon management and decision-making in a simulated business environment.

Financial work punishes invented facts, so Moonshot also targeted factual accuracy. On Kimi Hallu Bench Hard, its internal stress test, the claim-level hallucination rate fell to roughly a third of what K3 produced, and the share of responses containing at least one factual error dropped to 42.4 percent of its predecessor’s rate, a figure worth keeping precisely because it appears in no other table or chart. Moonshot is careful to note these are stress-test figures, not everyday error rates. GPT-6 Astra still leads this particular test.

Independent measurement points the same direction, on a separate test. On Artificial Analysis’s own AA-Omniscience evaluation, distinct from Moonshot’s stress test above, K3.1 Pro’s composite score rose by half again, and its non-hallucination rate now stands at 56 percent. Two different tests, both moving in the right direction.

A third scoreboard, Vals.ai’s Vals Index, weights agentic performance across finance, coding, legal, and tax work by each sector’s share of United States GDP. On it, K3.1 Pro is the leading open-weight model and sits eighth overall, and the comparison with K3 shows the same lopsided pattern as the Artificial Analysis index: large gains where agents write code, declines where they practice law.

| Evaluation (Vals.ai-measured) | Kimi K3.1 Pro | Kimi K3 | Change | 
|---|---|---|---|
| Vals Index | **62.52%** | 52.95% | +9.57 | 
| Terminal-Bench 4.0 | **63.64%** | 17.17% | +46.47 | 
| Vibe Code Bench v1.1 | 84.89% | **84.97%** | -0.08 | 
| Finance Agent v2.1 | **61.49%** | 60.01% | +1.48 | 
| EMB v1.1 | 70.00% | **71.26%** | -1.26 | 
| Vals Finance Index | 56.01% | **57.95%** | -1.94 | 
| Legal Research Bench | 37.50% | **46.15%** | -8.65 | 
| Tax Agent Bench | 24.87% | **33.37%** | -8.50 | 
| Vals Legal Index | 67.04% | **71.72%** | -4.68 | 
| HLAB-Vals-1.1 | 11.67% | **12.92%** | -1.25 | 

*Vals.ai measurements; K3 baselines from Vals.ai comparison pages and model listings.*

The coding side is where the release earns its rank. Vals.ai reports Terminal-Bench 4.0 moving from the middle of its table to the top three overall, with K3.1 Pro using about half the output tokens of K3 on that benchmark, finishing roughly four times faster, and costing about a third as much per test. Finance is mixed: Finance Agent improves, but at roughly triple the output tokens, while EMB slips on comparable-company and leveraged-buyout modeling and the overall Finance Index lands below K3. Legal and tax both decline outright, and Legal Research is where the model burns the most, around eight times the output tokens of K3, mostly reasoning, for a lower score.

The most unusual section of Moonshot’s launch report concerns cybersecurity. As post-training scaled up, the company observed vulnerability research performance rising steadily. Tracing the source, it found cyber-related content embedded in the general mid-training corpus and cyber-related tasks present in the reinforcement learning environment mix, a natural byproduct of training on broad data.

For the open-weight release, Moonshot removed that cyber-related data from both training stages while keeping general-purpose coding tasks. The measured effect is substantial. On xdev-bench-hard, an internal benchmark built on real, recently disclosed software vulnerabilities, the uncleaned checkpoint solves 90.5 percent of exploitation tasks at pass@3. The final K3.1 Pro reaches 81.0 percent, still far above K3’s 33.3 but deliberately below what the same pipeline could produce. DeepSeek’s rival open model V4.1-Flash scores higher still, which means the handicap, not a capability ceiling, is what puts it below a comparable open model.

Moonshot frames this as policy rather than weakness. The company states it will not release open weights that extend the absolute frontier of offensive cyber capability, while arguing that gradually diffusing capable models helps defenders, since more teams gain the means to audit the code they maintain, including legacy systems that rarely receive dedicated security reviews.

Token prices dropped by a third compared with K3. Input costs $2.00 per million tokens, down from $3.00, output costs $10.00, down from $15.00, and cache hits cost $0.20. The Highspeed variant trades roughly double the price for two-to-three-times the throughput.

| Pricing (per 1M tokens) | Kimi K3.1 Pro | K3.1 Pro Highspeed | Kimi K3 (previous) | 
|---|---|---|---|
| Input | **$2.00** | $4.00 | $3.00 | 
| Output | **$10.00** | $20.00 | $15.00 | 
| Cache hit | **$0.20** | $0.20 | $0.30 | 

Against the model it replaces, K3.1 Pro is cheaper at every rate a bill measures, as the table shows. What the table cannot show is that the model writes far more. Artificial Analysis measured 150 million output tokens across its full index evaluation against a median of 87 million, making K3.1 Pro one of the most verbose models it has tested. Per completed index task, that averages out to roughly 55,000 output tokens, about 14 percent more than K3 needed: about 20,000 in the visible answer and roughly 35,000 spent reasoning before it. The two forces nearly cancel on cost: a completed task costs about what it did on the old model, near two dollars, while scoring eleven points higher on the index. What does not cancel is time, at least against its predecessor. At roughly double K3’s output speed, the longer answers still finish about 40 percent faster than the old model managed. Against the class median, the picture has deteriorated, which is why per-task costs hold flat rather than falling with the price cut.

Speed has fallen since launch week. Artificial Analysis now measures K3.1 Pro at 57.5 output tokens per second, down from 80.7 measured earlier, and ranks it #141 of 186 models against a median of 82. The page’s own summary calls it “notably slow,” a fair description at this reading. The drift continues to be the point: across successive measurement windows the figure has moved repeatedly, so any single speed number for this model is a snapshot of a loaded endpoint, not a fixed spec. The generational comparison flatters it more than the class comparison does: K3 averaged about 39 tokens per second on the same measurement, so the successor is roughly twice as fast as the model it replaces.

For chat use, these are workable numbers. For agent pipelines running hundreds of turns, the math now compounds unfavorably: well-below-median speed multiplied by this model’s unusually large token counts means long total waits, which is exactly the workload the Highspeed variant exists to serve.

The model is available now across the Kimi product family:

- **Web and mobile:** kimi.com and the Kimi apps on iOS, Android, and HarmonyOS.
- **Kimi Work:** the desktop app, version 3.1.0 or later, for Windows and Apple silicon Macs.
- **Kimi Code:** select Kimi K3.1 Pro with the /model command.
- **API:** select kimi-k3.1-pro or kimi-k3.1-pro-highspeed on the Kimi API platform, with Zero Data Retention available for API customers.
- **Enterprise:** Kimi Enterprise administrators can enable the model for their organization; access is off by default at launch.

The full model weights are promised by November 4, 2026, which is the date that turns this from a strong API release into the new open-weight frontier.

Not yet. The weights are due for release by November 4, 2026, and until then the model is proprietary, accessible only through Kimi’s own products and API. Once published, it will be the highest-scoring open-weights model in the world. The Kimi family ships under custom licenses that permit commercial use rather than a standard OSI-approved open-source license, so open weights is the more accurate term.

API pricing is $2.00 per million input tokens and $10.00 per million output tokens, with cache hits at $0.20. The Highspeed variant costs $4.00 and $20.00 respectively. Because the model is unusually verbose, the real-world cost per completed task stays near two dollars, close to what K3 charged despite the price cut.

Not consumer hardware. At 2.8 trillion parameters, self-hosting requires data-center-class multi-accelerator configurations. NVIDIA has published validated configurations for the K3 family on its Blackwell GB200 systems, using 256 GPUs for full fine-tuning runs. Most people will instead use the Kimi app or API, where the model runs on Moonshot’s infrastructure.

NVIDIA officially supports Kimi K3 through its NeMo framework on Blackwell hardware, and reports have also claimed Moonshot trained K3 using advanced Blackwell GPUs despite United States export restrictions, relying on H20 chips to serve the model at scale. Moonshot has not disclosed the specific hardware behind K3.1 Pro’s post-training run.

Kimi is Moonshot AI’s consumer chat assistant, available on the web and on iOS, Android, and HarmonyOS. K3.1 Pro is live there now, so anyone can try the fourth-ranked model in the world without touching an API.

On Vals.ai’s independently run benchmarks, K3.1 Pro leads all open-weight models on the overall Vals Index at 62.52 percent and ranks eighth overall. The pattern is uneven: it improves over K3 on Finance Agent and Terminal-Bench 4.0 but declines on Legal Research, Tax Agent, and the Finance Index. For factual reliability specifically, Artificial Analysis’s AA-Omniscience figures put accuracy at 51 percent with a 56 percent non-hallucination rate, while Moonshot’s internal stress test reports a claim-level error rate of 5.4 percent on standard prompts and 8.3 percent on hard prompts with tools.

On some individual benchmarks the two are now separated by under two points, and K3.1 Pro leads on SWE-Marathon, OfficeQA Pro v2, and AutomationBench-AA. On the composite Intelligence Index it still trails by three points, and on offensive cyber work the gap is wider by design, a consequence of the training-data removals Moonshot applied before release.

Kimi K3.1 Pro is the strongest evidence yet that the open-weight tier has joined the frontier conversation. A model that anyone will soon be able to download now sits three points off the global lead on the most-watched independent index, matches the proprietary frontier on terminal coding, and does it at a fraction of the price those frontier models charge.

The tradeoffs are real: measured output speed has fallen to well below the class median, it writes far more than most rivals to get there, and offensive cyber capability is intentionally held back. But the direction is unmistakable. When the weights publish on November 4, the distance between downloadable AI and the best proprietary models will be three points, and whether the closed labs respond with a jump of their own is the next question worth watching.
