---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-review-knowledge-cutoff-truestandard
title: "Gemini 3.8 Flash Review: Knowledge Cutoff | TrueStandard"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["million", "flash", "tokens", "price", "output", "knowledge", "gemini", "cutoff"]
source: https://truestandard.ai/blog/gemini-3-8-flash-review
source_anchor: ""
source_lines: [1, 287]
section: "Gemini_Google"
---
Gemini 3.8 Flash emits 1.77 times the output tokens of 3.7 Flash on the same evaluation suite. So it lists cheap and bills more, and you should run it at medium effort, not high. It has a one million token context window, 64,000 max output tokens and a March 2026 knowledge cutoff. Google shipped it on 2 September 2026. It documented the extra tokens as a design choice before anybody measured them. The list price did not move, and the invoice did.

This review covers the first week. We ran no benchmark of our own. We checked whether the week's most repeated claims hold up against their sources. Price the work by tokens emitted. On Artificial Analysis, it now costs more per task than GPT-5.6 Sol on high, yet it lists at a fifth of Sol's per-token price. Where a claim below rests on one person, it says so.

## The short answer

Gemini 3.8 Flash shipped on 2 September 2026 as gemini-3.8-flash. It carries a one million token context window, a max output of 64,000 tokens and a knowledge cutoff of March 2026. It accepts text, images, audio, video and PDF, and writes text only. Introductory pricing runs at $0.75 per million input tokens and $3.75 per million output, with caching at $0.075. Those rates hold until 31 December 2026 and double the next day. On Artificial Analysis it scores 47 against 45 for 3.7 Flash, and it costs $0.74 per task against $0.55.

Google built 3.8 Flash to spend more tokens on the same job. It said so in its own launch post, and it pointed compute-constrained users back at 3.7 Flash. A price list ranks models by dollars per million tokens. Your invoice ranks them by tokens emitted times that price.

## Two models shipped, one with public specs

Two things shipped on 2 September, and only one has public specs. The numbers below come from Google DeepMind's own model card, read on 6 September 2026.

| Spec | Gemini 3.8 Flash | Why it matters | 
|---|---|---|
| API model ID | gemini-3.8-flash | The Cyber variant beside it has no public identifier. | 
| Context window | 1,000,000 tokens | Unchanged from 3.7 Flash, so context is no reason to move. | 
| Max output | 64,000 tokens | The ceiling on one response, and the half of the bill that grew. | 
| Input | Text, image, audio, video, PDF | Output is text only. The vision inputs are the reason to pick it. | 
| Knowledge cutoff | March 2026 | Google qualifies this one heavily. Read the caveat. | 
| Input price | $0.75 per million | Intro rate. It doubles on 1 January 2027. | 
| Output price | $3.75 per million | Intro rate, and the line that decides the bill. | 
| Context caching | $0.075 per million | Intro rate. It doubles with the rest. | 

Each row is one spec from Google DeepMind's model card, read on 6 September 2026. The last column says why the spec matters to you.

Google puts the knowledge cutoff for this model at March 2026. Then it adds that users "can expect updated information for some domains while in others they may experience the model's knowledge is limited to January 2025." Vendors rarely publish that sentence. Treat March 2026 as a ceiling, and put anything recent into the prompt yourself.

The rates expire on a fixed date, so you can plan for the jump.

| Token type | Through 31 December 2026 | From 1 January 2027 | 
|---|---|---|
| Input | $0.75 per million | $1.50 per million | 
| Output | $3.75 per million | $7.50 per million | 
| Context caching | $0.075 per million | $0.15 per million | 

Each row is one token type. Each price is in dollars for one million tokens. Every rate doubles on 1 January 2027.

Modelling 2027 spend from a 2026 invoice takes two steps. Double each token line first, then apply the token growth you measure in the next section. Both effects push the same way, and they multiply.

The second model is Gemini 3.8 Flash Cyber, and almost nothing about it is public. Google limits it to its Fairwind Program for governments, critical-infrastructure operators and trusted defenders. There is no API identifier, no published price and no published spec. Compare Cyber against a commercial security model, and you are comparing against numbers Google never released.

## Built to spend more tokens on the same job

Google's launch post says 3.8 Flash takes more reasoning steps at higher effort levels. It also makes more repeated tool calls, trading tokens for quality. The same post tells users short on compute to drop to a lower effort level, or to stay on 3.7 Flash. Artificial Analysis measured the size of the extra spend.

| Model, high effort | Output tokens | Cost per task | Intelligence Index | 
|---|---|---|---|
| Gemini 3.8 Flash | 140M | $0.74 | 47 | 
| Gemini 3.7 Flash | 79M | $0.55 | 45 | 
| GPT-5.6 Sol | 24M | $0.61 | 48 | 

Each row is one model at high effort on Artificial Analysis, Intelligence Index v4.2, on 6 September 2026. Output tokens is the total the model wrote on the whole test suite. A higher Intelligence Index score is better.

Those are live numbers from Artificial Analysis on 6 September 2026, running Intelligence Index v4.2. The ratio on output tokens is 1.77 to one. Gemini 3.8 Flash on high costs more per task than GPT-5.6 Sol on high. Yet it lists at roughly a fifth of Sol's per-token price. Launch-week numbers put 3.7 Flash at 64 million against 120 million for 3.8. So the direction and size of the gap both held across two versions of the index.

Does more thinking also mean fewer made-up sources? We tested that on Gemini 3.7 Flash at all four thinking levels. Only the top level cut its rate, from 13.3% to 4.5%: does Gemini hallucinate less with more thinking.

Three measurements of the increase circulated, and all three are real. They run from about 30 percent to roughly ten times, because each measures different work.

### A fixed evaluation suite: 1.77 times

Artificial Analysis runs the same tasks against each model. On that set 3.8 Flash emits 1.77 times the output of 3.7 Flash. This is the reproducible figure, and also the smallest of the three.

### One video creator: about 30 percent

A creator running their own side-by-side put the increase near 30 percent. Those prompts were shorter and less agentic than the evaluation suite, so the gap is smaller to match.

### Agentic web-app builds: far higher

Users building apps inside an agent loop reported much steeper numbers, and one put it near ten times. Another watched their input-to-output ratio move from about 1:3 to about 1:8, and a third drained 100,000 tokens in ten messages.

Extra reasoning steps and extra tool calls add up with each round. A task with forty tool calls carries the extra cost forty times, and a single-shot question carries it once. So a fixed benchmark understates what an agent loop costs you, and an average of the three would describe nobody's workload.

Tokens are one axis, and we measured another on the same pair: whether the newer model invents more sources. 3.8 Flash fabricated more in every one of three runs, and the test still cannot call it worse. When Google's Pro tier moved, we ran the same test on it: Gemini 4 Argon against Gemini 3.1 Pro.

Notice that there are three measurements of one model, and the one you quote decides the answer you get. Paste a draft into TrueStandard, and four frontier models from different vendors check it in parallel. Each place they disagree shows up, instead of averaging into one confident answer.

## What people built in week one

Filter out the demos that only prove a model can emit something shaped like software. Seven builds hold up, and each names its source.

### A crop-loss evidence app for Indian farmers

Fasal-Pramaan is a progressive web app for India's PMFBY crop insurance scheme. It pairs 3.8 Flash vision with written field analysis. There is an on-device OpenCV shutter lock and a Sentinel-2 fire check. It had eight stars in week one, and nothing else we saw published on the three launches sits this close to a real institution.

### A caregiver scribe and doctor handover

EMA records what a caregiver observes, and turns that into a handover a doctor can read. It runs on Cloud Run with Firestore and Secret Manager, and it is past the demo stage.

Source: the build log, on X.

### A live ISS tracker with a correct terminator

The tracker calls a position API every five seconds. It overlays live weather, and it also draws the day and night terminator correctly. That is the detail most hobby trackers get wrong.

Source: Prompt Engineering on YouTube.

### A castle battle that added things nobody asked for

It was built inside Antigravity. It arrived with unprompted sound effects, a cart mode for dropping cannonballs, a dragon, and sliders for time of day and weather. The same prompt in the plain Gemini app gave much less.

Source: Prompt Engineering on YouTube.

### A web app drawing behind the Android status bar

A developer asked for a layout change and got back a capability they had not known progressive web apps had. The model reached for an API they would never have found.

Source: r/google_antigravity.

### A Minecraft clone with real first-pass bugs

The clone worked overall, but the player spawned underground, the crafting menu duplicated items, and furnace smelting made items disappear. A second prompt fixed most of it.

Source: QuartzRouter on YouTube.

### A Three.js animation in under 70 seconds

One creator timed the same scene animation on two models. Gemini 3.8 Flash finished in under 70 seconds for about $0.12. Claude Opus 5 took up to five minutes and $1.86.

Read those seven together, and the tool the model ran inside explains more of the spread than the prompt does. Four ran in Antigravity or a deployed cloud service. The weakest result came from the plain chat app, on the same model and the same prompt as the strongest.

## One well-backed practice: medium effort

One practice across all three launches has two kinds of backing: Google's own docs and three independent users state it the same way. Gemini produced it, and GPT-6 Astra produced no such practice. On Claude Fable 5.1, four of six rested on a single person.

| Practice | Source | Strength | 
|---|---|---|
| Run it at medium effort instead of high | Google's docs point compute-constrained users at lower effort levels. Three independent users landed on the same claim. One reports it getting stuck on high. One uses medium as a default. One notes low is the faster option by design. | Documented plus three users | 
| Use it to orchestrate or review, not to write the code | One developer plans and codes with Grok, then gates the result through 3.8 Flash on high. Another runs it as the orchestrator above a planner, an adversarial reviewer and a coder. | Two independent users | 
| Prefer it to GPT-6 Astra when code discipline matters | Two independent users report cleaner, more disciplined output at similar or lower cost. | Two independent users | 
| Prepay through OpenRouter as a hard budget stop | A prepaid balance halts the run when it empties. A rate limit will not. Sensible given the token behaviour above, and reported by one person. | Single source | 

The first row is the one to act on today, with the strongest support of any practice here. Vendor docs plus three unconnected people naming one setting is a different kind of claim from four people liking a model. Medium effort emits fewer tokens because it takes fewer reasoning steps, so the setting that saves money also lowers the ceiling.

Look down the third column. Two rows rest on two people each, and one rests on one. They still made the table, because testing a prompting practice costs a single run. A claim about your customers is not that cheap to test. TrueStandard runs four frontier models from different vendors over a draft in parallel. Each disagreement shows up, instead of resolving quietly into one voice.

## The tool you run it in changes the result

Two groups of users found this on their own, without looking for it. Run 3.8 Flash in one tool and it looks strong, then run it in another and it looks unreliable. The model is identical.

### Identical prompts, two very different outputs

A video creator ran the same prompts through the plain Gemini app, then through Google's Antigravity IDE. The quality gap became the finding of that test. The castle simulation above came out of the Antigravity run.

### AI Studio came back brittle

Users described the same model in AI Studio hitting too many tool calls errors. It answered some sub-questions and silently skipped others. The same work went through Antigravity cleanly, and two people found this on their own.

### Antigravity gets the new Flash models first

Two users report that Antigravity receives same-day free-tier access to new Flash releases, alongside or ahead of AI Studio. On day one, the tool you pick also decides whether you pay.

When somebody calls a model good or bad, the agent harness they ran it in, the tool that wraps the model, did part of the work. Almost nobody names it. Two reviewers can disagree about one model, and both can be reporting accurately.

## Seven problems to plan around

Seven things came up often enough to plan around. The first is the loudest complaint and the weakest evidence here.

### Many people found it level with or worse than 3.7

Three independent video creators and six independent Reddit users reached that verdict on their own work. One scored it 7.48 out of 20 on their own coding rubric. Several moved back to 3.7 Flash, and one commenter pushed back at length. A pile of single anecdotes does not prove the model got worse, he argued, and he is right. The token measurement above is reproducible on a published suite. Each complaint is a sample of one.

### It can loop on a single error

One user ran the same bug-fix prompt on both models. Gemini 3.7 Flash finished in 37 seconds, and Gemini 3.8 Flash ran past eight minutes. It entered a loop, and needed a human to stop it. More reasoning steps mean more chances to keep going when stopping was right.

### Rate limiting arrives after about a week

Heavy daily users reported hard rate limiting roughly a week in. Plan for it first, and do not build a workflow that assumes an answer whenever you ask.

### One experienced user calls the Flash tier reckless for coding

He keeps a paid plan, and still reaches for a stronger model whenever context matters. The extra reasoning steps buy two points of index. Two points do not move a cheap fast model into a different class.

### There is no DeepSWE number to quote

Google's blog says only that 3.8 Flash "outperforms most larger frontier models" on DeepSWE v1.1, and no figure is attached. Two secondary outlets printed 71% and 73.7%, but neither traces back to Google, so we cite neither.

### The Cyber variant's results are all Google's own

Google reports that Cyber found a 13-year-old Chrome bug in under two hours. It also produced 2.6 times more correct patches than competing commercial models. Wiz reported 7.5 to 9.7 percent higher vulnerability recall, at 2.3 to 5.2 times lower cost. Google owns Wiz. Each of those numbers is a vendor grading its own security model.

### Three Flash releases in six weeks, and no new Pro

3.8 follows 3.7, which followed 3.6, and anyone who pinned a model in July has re-evaluated three times since. Gemini meanwhile is still on 3.1 Pro, and the 3.5 Pro promised for June 2026 has not shipped. Fast movement at the cheap tier with none at the top is a planning problem.

One more thing broke, and Google then fixed it. After launch, Gemini 3.8 Flash stopped linking to sources for some queries inside AI Mode. An answer without its sources is the failure our post on how to check if AI citations are fake exists for. A missing citation is at least visible. A fabricated one is not.

None of this makes 3.8 Flash a bad model. It reads five input types and holds a million tokens. It animated a Three.js scene in 70 seconds for twelve cents. It does mean the model on the price list and the model on your invoice are two different products.

## 3.8 Flash against 3.7 Flash: a little stronger, dearer per task

Here is the comparison people want, on the numbers that exist. Both models' numbers come from one board on one day.

| Measure | Gemini 3.8 Flash | Gemini 3.7 Flash | 
|---|---|---|
| Intelligence Index, high effort | 47 | 45 | 
| Output tokens on the same suite | 140M | 79M | 
| Cost per task | $0.74 | $0.55 | 

Each row is one measure from Artificial Analysis on 6 September 2026. Both models ran at high effort. Output tokens is the total for the whole test suite.

Two points of Intelligence Index cost you 35 percent more per task and 77 percent more output tokens. Whether that trade pays depends on whether the two points show up in your work. Most people who tested it said they did not. Google agrees far enough to send compute-constrained users back to 3.7 Flash in its own launch post.

One caution about each number in this section. Artificial Analysis re-versioned its Intelligence Index from v4.1.1 to v4.2 between 3 and 6 September. Each model's score fell: Gemini 3.8 Flash went from 59 to 47. Claude Fable 5.1 went from 66 to 57, GPT-6 Astra from 61 to 55, GPT-5.6 Sol from 61 to 51, and Claude Opus 5 from 63 to 54. No model changed in those three days, and each launch-week screenshot is now out of date. That is the argument our post on why AI model rankings do not measure truth makes at length.

Buy 3.8 Flash for the vision inputs and the million-token context. Buy it for the price you will pay during 2026. Do not buy it because it sits at the bottom of the price list.

## Frequently asked questions

### What is Gemini 3.8 Flash?

A fast, cheap Google model released on 2 September 2026, available as gemini-3.8-flash. It holds a one million token context window and emits up to 64,000 tokens per response. Its knowledge cutoff is March 2026. It accepts text, images, audio, video and PDF, and writes text only. A restricted variant, Gemini 3.8 Flash Cyber, shipped alongside it, and that one has no public API identifier and no pricing.

### How much does Gemini 3.8 Flash cost?

$0.75 per million input tokens, $3.75 per million output tokens, and $0.075 per million for context caching. Those introductory rates hold through 31 December 2026 and double on 1 January 2027. On Artificial Analysis it costs $0.74 per task against $0.55 for Gemini 3.7 Flash. The reason: it emits 1.77 times the output tokens on the same suite.

### When does Gemini 3.8 Flash pricing go up?

On 1 January 2027. The introductory rates run through 31 December 2026, and then each line doubles: input from $0.75 to $1.50 per million, output from $3.75 to $7.50 per million, and context caching from $0.075 to $0.15 per million. The date is fixed, so budget 2027 at double the 2026 token line.

### Is Gemini 3.8 Flash better than Gemini 3.7 Flash?

On Artificial Analysis it scores 47 on the Intelligence Index against 45 for 3.7 Flash. So it is a little stronger, and much dearer per task at $0.74 against $0.55. Many users disagree that the gain shows up in real work. Three independent video creators and six independent Reddit users rated it level with or worse than 3.7, and several moved back. Those verdicts are anecdotes. The cost difference is a measurement.

### Why does Gemini 3.8 Flash use more tokens?

Because Google built it that way and says so. Its launch post describes more reasoning steps and more iterative tool calls at higher effort levels, trading tokens for quality. It points compute-constrained users at lower effort, or at 3.7 Flash. Artificial Analysis measured 1.77 times the output tokens on a fixed suite. Users running long agent loops report much larger multiples, because each iteration compounds the effect.

### What effort level should I use with Gemini 3.8 Flash?

Medium, unless the output forces you higher. Google's docs point compute-constrained users at lower effort levels. Three independent users landed on the same advice. One reports it getting stuck on high, one runs medium as a default, and one notes low is faster by design. Docs plus three unconnected people naming one setting is the strongest backing any tip from this launch week has.

### What is Gemini 3.8 Flash's context window?

One million tokens of input, and up to 64,000 tokens of output in a single response. That input window matches Gemini 3.7 Flash, so a bigger context is no reason to upgrade. Watch the output ceiling instead. Output tokens cost five times what input tokens cost. And this model emits 1.77 times as many as its predecessor.

### Is Gemini 3.8 Flash good for coding?

It is better as a review or orchestration layer than as the primary coder. Two developers reached that setup on their own, and one gates Grok's output through 3.8 Flash on high. One runs it as orchestrator over a planner, an adversarial reviewer and a coder. Two others found it writes cleaner code than GPT-6 Astra at similar cost. It can also loop on one error. 3.7 fixed a bug in 37 seconds where 3.8 ran past eight minutes.

### What is Gemini 3.8 Flash's knowledge cutoff?

March 2026, with a caveat Google publishes itself. The model card says users "can expect updated information for some domains while in others they may experience the model's knowledge is limited to January 2025." March 2026 is a ceiling. That is the most the model might know, so put anything recent into the prompt or fetch it with a tool call.

### What is Gemini 3.8 Flash Cyber?

A restricted security variant that shipped the same day. It is gated behind Google's Fairwind Program for governments, critical-infrastructure operators and trusted defenders. It has no public API identifier, no published pricing and no published specs. Google reports that it found a 13-year-old Chrome bug in under two hours. It also produced 2.6 times more correct patches than competing commercial models. Each of those results is Google's own, or from Google-owned Wiz.

## Keep reading

### Is There a Most Accurate AI Model?

The honest answer is no. The ranking changes with the task, the benchmark, and the month, and even the leader still hallucinates.

### Is Jev Really 193x Faster?

We measured the same model two ways and got 1.7x and 100x. Both numbers are honest. What separates them is the thing you put on the other side.

### Jev Images Tested Against Three Vision LLMs

We gave Jev 1.14 and three cheap vision models 360 screenshots of AI search answers. Jev and Claude Haiku 5.5 made no mistakes. Jev was 3 to 7 times faster and the cheapest per image.

### GPT-6.1 Terra Hallucinations Tested

OpenAI reports fewer errors on its own test. We counted Terra's fake citations on one day.

### TrueStandard vs Parafact

Both verify claims before you publish. The real difference is what one model can miss, and whether your long-form draft fits inside the check at all.

## What we measured on this model

Each release page carries our own fabrication data for one model version, measured against the model it replaced in the same vendor's line.

## A price list ranks models. Your invoice ranks them differently.

Each number on this page carries the source that produced it. The sourced ones disagree with the ones that circulated. TrueStandard runs your draft past four frontier models from different vendors at once. It shows each place they disagree before it ships.

TrueStandard is for writers who publish AI-assisted drafts. Paid plans check every claim and citation across four AI models before you hit publish.

Free: one check a day, no signup. Pro is $20 a month for 250 drafts.

Start Checking →
