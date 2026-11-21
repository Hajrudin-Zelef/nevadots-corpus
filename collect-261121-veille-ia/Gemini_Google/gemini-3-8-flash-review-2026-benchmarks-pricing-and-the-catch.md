---
id: collect-261121-veille-ia/gemini-google/gemini-3-8-flash-review-2026-benchmarks-pricing-and-the-catch
title: "Gemini 3.8 Flash review 2026: benchmarks, pricing, and the catch"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-09-02"]
keywords: ["flash", "gemini", "google", "model", "catch", "cyber", "tokens", "output"]
source: https://www.eesel.ai/blog/gemini-3-8-flash
source_anchor: ""
source_lines: [1, 205]
section: "Gemini_Google"
---
Google shipped Gemini 3.8 Flash on 2 September 2026, its third Flash release in six weeks, alongside a locked-down security sibling called 3.8 Flash Cyber. It costs $0.75 per 1M input tokens and $3.75 per 1M output, which is exactly what 3.7 Flash costs, and both numbers double on 1 January 2027.

It beats 3.7 Flash on every benchmark Google published, and beats Claude Opus 5 on three of them. The honest catch is in Google's own docs: 3.8 Flash is built on 3.7 Flash rather than a new base model, it deliberately "works harder" by burning more thinking tokens, and Google tells you outright to stay on 3.7 Flash for efficiency-first workloads. Independent testing at Artificial Analysis puts it 3rd of 196 for output speed and 74th for verbosity, with a 13.30 second time to first token against a 2.99 second median.

My read: if you run long agentic coding jobs where quality beats cost, switch, because it is free to. If you run high-volume short tasks a human waits on, stay on 3.7. And if you are picking a model to answer customer tickets, the model is not your bottleneck. Retrieval and testing are, which is the part eesel handles.

## What Gemini 3.8 Flash actually is

Gemini 3.8 Flash is Google's workhorse model, shipped as `gemini-3.8-flash` and pitched squarely at long-horizon coding and autonomous agents rather than chat.

The specs are unchanged from the last generation. It takes text, image, audio, video and PDF in, gives text out, and holds a 1M token context window with a 64K output ceiling. It does function calling, search as a tool, and computer use. Google publishes all of this on the model card.

Where you can actually reach it splits by audience. Developers get it in Google AI Studio, the Gemini API, Android Studio, and Google Antigravity, where it is now the default model. That last one matters if you are shopping for AI coding assistants, since it means you may already be running 3.8 without choosing it.

Enterprises get it through Gemini Enterprise. Consumers get it in the Gemini app, AI Mode in Search, and Google Sheets, but only on Google AI Pro or Ultra. If you are weighing those consumer tiers, I went through them in the Google AI Plus writeup, and there is a separate guide on turning Gemini off across Workspace if it turned itself on for your team.

There is a second model in the same announcement, and most coverage buried it. Gemini 3.8 Flash Cyber is a security variant with deliberately looser mitigations for cyber work, and it is gated to trusted defenders through the new Fairwind Program. More on that below, because the numbers are the most impressive part of the whole launch.

## The part Google says out loud, and almost nobody quoted

Here is the sentence that reframes the release. From the model card, repeated in four separate sections:

"Gemini 3.8 Flash is based on Gemini 3.7 Flash."


That is not a throwaway line. The Architecture, Training Dataset, Hardware, Software, Safety Policies and Acceptable Usage sections all defer to the Gemini 3.7 Flash model card rather than describing anything new. 3.8 Flash is 3.7 Flash trained further, not a new base model.

Once you know that, the rest of the launch stops being surprising. It explains the three-week cadence. It explains why the price did not move a cent. And it sets a realistic expectation: this is an iteration, and it behaves like one.

Google's blog is equally direct about the mechanism behind the gains. The wording is worth reading closely, because it is a trade-off disguised as a feature:

"These performance gains stem from a core design choice: 3.8 Flash works harder. On complex tasks, it exhibits greater diligence, executing extra reasoning steps, and calling tools iteratively. At times, the model might use more tokens to maximize performance, especially at higher effort levels."


Then the line that should decide your upgrade: Google recommends that developers who care about compute efficiency "continue to rely on Gemini 3.7 Flash, which remains fully supported for efficiency-first workloads." A vendor telling you its old model is still the right pick is rare enough that it is worth taking at face value.

## Pricing: identical to 3.7 Flash, and it doubles in January

There is no premium for the newer model. The Gemini API pricing page lists 3.8 Flash and 3.7 Flash at the same rates, line for line.

| Tier | Input / 1M | Output / 1M (incl. thinking) | Cache read / 1M | From 1 Jan 2027 | 
|---|---|---|---|---|
| **Free** | $0 | $0 | $0 | unchanged | 
| **Standard** | $0.75 | $3.75 | $0.075 | $1.50 / $7.50 | 
| **Batch** | $0.375 | $1.875 | $0.0375 | $0.75 / $3.75 | 
| **Flex** | $0.375 | $1.875 | $0.0375 | $0.75 / $3.75 | 
| **Priority** | $1.35 | $6.75 | $0.135 | $2.70 / $13.50 | 

Three things in that table deserve more attention than they get.

**The output price includes thinking tokens.** You pay $3.75 per million for reasoning you never see, only a summary of. On a model whose headline feature is that it thinks harder, that is the meter that moves.

**Cache storage is billed separately.** Reads are $0.075 per million, but storage runs $0.50 per 1M tokens per hour, also doubling in January.

**Grounding is shared, not per-model.** You get 5,000 free Google Search requests a month across every Gemini 3.x model combined, then $14 per 1,000. Same allowance for Google Maps grounding.

And the headline number is a promo. Every rate here doubles on 1 January 2027. If you are sizing a 2027 budget on $0.75, you are sizing it on the wrong number, which is a mistake I have watched teams make with LLM costs more than once. Worth pairing with real LLM tracking tools rather than a spreadsheet, because the thinking-token overhead is the part nobody forecasts correctly.

For context on what the neighbours charge, Claude Opus pricing and OpenAI rate limits are the two comparisons that come up most when teams model a switch.

## The benchmarks Google leads with

Google published four headline results, and 3.8 Flash tops all four. Here is the full set from the DeepMind model page, which is the primary source rather than a press recap.

| Benchmark | Gemini 3.8 Flash | Gemini 3.7 Flash | Claude Opus 5 | GPT-5.6 Sol | GPT-5.6 Terra | Claude Sonnet 5 | 
|---|---|---|---|---|---|---|
| HLE-Verified | **54.9%** | 53.6% | 54.4% | 54.5% | 51.1% | 31.0% | 
| Vals Finance Agent v2 | **61.4%** | 59.0% | 58.6% | 53.8% | 54.4% | 53.9% | 
| Harvey's Legal Agent | **10.0%** | 8.8% | 6.7% | 2.5% | 0.8% | 5.0% | 
| DeepSWE v1.1 | top spot | lower | lower | n/a | n/a | n/a | 

Two honest caveats before anyone screenshots that table.

The Harvey Legal Agent row looks like a rout, but the winning score is 10.0%. Every model in that column is failing the benchmark; 3.8 Flash is failing it least. Beating Opus 5 by 3.3 points on a test nobody passes is a real result and a small one.

And on HLE-Verified, the gap over Opus 5 is half a point. Google is claiming a tie with a frontier model at a fraction of the price, which is the actual story, not a knockout. If you want the generation-on-generation version of that argument, Gemini vs Claude Opus and ChatGPT vs Gemini both hold up.

The right-hand column is the part Google's post does not cover, and it comes from people who ran the model rather than announced it.

## The independent numbers tell a more useful story

Artificial Analysis ran 3.8 Flash at high effort across its nine-benchmark Intelligence Index, and the four summary cards are more decision-useful than anything in the launch post.

Intelligence of 59, ranked 17th of 196, against a class median of 36. Cost of $0.58 per index task, ranked 60th. Speed of 302.1 output tokens per second, ranked 3rd. And verbosity of 120M output tokens to complete the index, ranked 74th, against a 71M median. Artificial Analysis calls it "very verbose" in its own summary. Running the whole index on it cost $825.83.

That verbosity number is the pricing story in one figure.

A model that uses 70% more output tokens than the median at $3.75 per million is not as cheap as its rate card suggests. That was the sharpest correction in the Hacker News thread, which ran to 775 points on launch day:

"On artificial analysis it's only equal to opus 5 medium effort. Opus 5 max scores 63. Further, opus 5 medium outputs 4x fewer tokens to achieve the same result, negating a lot of the speed difference."


Then there is the number I think almost everyone missed, and it is the one that would change my own architecture decision.

Artificial Analysis measures time to first token at 13.30 seconds, against a class median of 2.99 seconds. The third-fastest model in the world by throughput is roughly four and a half times slower than typical to say its first word, because it is thinking first.

That distinction matters enormously and gets flattened constantly. Throughput is how fast it finishes a long job. Time to first token is how long a human stares at a blank screen. For a batch agent grinding through a repo overnight, throughput wins and TTFT is irrelevant. For anything a person is waiting on, like a support reply or a chat widget, those 13 seconds are the entire user experience.

## What people said after actually running it

Launch-day reaction split cleanly, and the split lines up with that throughput-versus-latency divide.

Google's own developer advocate framed the token burn as a feature rather than a cost, which is the most honest version of the vendor case:

"It takes smaller steps and verifies its work more often. This can leads to higher token usage but it uses those extra tokens effectively to run tests and verify changes, giving you much higher quality work overall, especially for more complex and long-running goals."


Worth noting he works at Google DeepMind, so treat it as the vendor's framing rather than an independent test. His post drew 308 likes and, two replies down, a flat contradiction from someone who had tried it on real work:

"Do not want to be negative, but it is absolutely not usable for SWE. Same very negative experience as it is with Gemini 3.7 Flash. Cost more than Fable on a really simple SWE tasks."


The most useful review I found came from a developer who ran a long benchmark prompt through it and posted a clean pros-and-cons list the same evening:

"This model takes its time to think and work through the tasks you have given it, at a really great price and speed. Plus - long thinking, not a airhead model anymore there is subsistence here. Cons - I want to be able to have a better chat with the model before it begins it work, can't test its own work, and cant run and view in its own browser in antigravity."


For chat rather than code, the read was more positive, and specifically about the thing Flash models have historically been bad at:

"The context window remains the same at 1 million tokens, but its coding reasoning ability has improved significantly, and hallucinations have been greatly reduced, so the issues where it would hallucinate or make grammatical missteps have decreased a lot."


On Hacker News, the most practical workflow idea in the thread was not about replacing anything. It was about pairing:

"I've been using 3.7 Flash to audit the work of Opus High, and Flash finds lots of subtle and insidious defects even while all the unit tests are green. Then I tell Opus to read the audit report and implement what it agrees with. Flash is really good at this, and it is blazing fast in Antigravity CLI."


The skeptics had a specific, checkable objection rather than a vibe. One commenter pointed at the benchmark Google left out:

"There are important gaps in that hot take. For example, it's not even close to Opus 5 on Terminal-bench 4.0, 19.1% vs. 51.8%."


And several called benchmark saturation on DeepSWE, which is fair given how quickly every lab now tops it. As one put it, the benchmark has "clearly been 'dealt with' already." My own advice is the same as zuzululu's in that thread: keep your own eval set, because public leaderboards stopped separating these models a while ago.

## Two things in the model card nobody is quoting

I read the model card properly, and two details there did not make it into any coverage I saw.

**Two safety metrics regressed.** Google's own table lists Multilingual Safety at +5.4pp where lower is better, and Unjustified-refusals at +1.1pp, also lower-is-better. Google states it plainly rather than hiding it: "Safety performance across non-English languages regressed slightly relative to 3.7 Flash." If you serve non-English users, that is a real consideration, and it is the kind of thing that only shows up in production.

Related, and worth knowing: Google did not re-run the full Frontier Safety assessment on 3.8 Flash. The card says the team evaluated 3.7 Flash, then concluded that because 3.8 Flash has no "meaningful new capabilities," the 3.7 results carry over. Reasonable given the shared base, but it is inference rather than measurement.

**The comparison table has a mislabelled baseline.** The safety table is headed "Gemini 3.8 Flash vs. Gemini 3.7 Flash," but footnote 1 says the Tone row is measured "compared to Gemini 3 Flash." That is a different, older model sitting in a column that claims to be a 3.7 comparison. A Hacker News reader caught it first and called it "Chartcrime, the major AI lab tradition." I checked the live card and the footnote still reads that way.

Neither of these is a scandal. Both are the kind of detail that separates reading the primary source from rewriting the press release.

## Gemini 3.8 Flash Cyber, the more impressive launch

The security variant got a fraction of the attention and has the stronger numbers.

On CyberGym, the standard benchmark for autonomous vulnerability discovery, Google says 3.8 Flash Cyber surpasses both 3.5 Flash Cyber and "significantly larger frontier models." On an internal benchmark spanning 20 programming languages, it exceeds a 70% success rate. On CWE-Bench, an external patching benchmark run by Collinear, it scores 47.2% pass@1 against a leading frontier model's 47.8%, at much lower cost.

The deployment numbers are better than the benchmarks. Google's Chrome Security team found it produced 2.6 times more correct patches to Chrome vulnerabilities than the best, much larger commercial models. Wiz measured +7.5 to +9.7% higher recall on its internal penetration testing benchmark at 2.3x to 5.2x lower cost. And Google's Cloud Vulnerability Research team used it to find a critical foundational vulnerability in under two hours, work they say normally takes months.

The catch is access. You cannot buy it. It ships only to trusted defenders through the Fairwind Program, which means government authorities, critical infrastructure operators and software maintainers who apply and get approved. Everyone else gets the standard model with CBRN and cyber-offense safeguards intact.

## So should you actually switch?

Since the price is identical, this is purely a question of workload shape.

**Switch to 3.8 Flash if** you run long-horizon agentic coding, multi-step research, or document-heavy work where quality matters more than token count. Glean's AI product lead reported it "completing more than three times as many tasks as Gemini 3.7 Flash" on long-running document workflows in their evaluations. It is the default in Antigravity now, so you may already be on it.

**Stay on 3.7 Flash if** your workload is high-volume, short-context, or latency-sensitive. Google says this itself. The verbosity gap means 3.8 can cost more per completed task despite the identical rate, and the 13.30 second TTFT is disqualifying for anything interactive.

**Consider neither if** you are choosing a model to power a product feature rather than to code with. That is worth spelling out, and it is the same conclusion I reached writing up the best AI agents and AI agents for small business: the model rarely decides the outcome.

## What this means if you are putting AI on a support queue

This is where I want to be direct, because it is the mistake I see most often.

Teams evaluating AI for customer support spend weeks comparing model benchmarks, then ship something that gives confidently wrong answers anyway. A support team I worked with hit exactly this. Their bot told customers "yes, we support your car model" for vehicle brands that were not in their database, because their knowledge base said "we support all models." No benchmark score fixes that. It was never a model failure. It was a retrieval and scoping failure, and a smarter model would have stated the wrong answer more fluently.

The pattern repeats. One customer put the requirement about as clearly as anyone can:

"The AI will never be able to answer 100% of the questions. I need an AI who is only handling the tickets that it's confident to handle and all the other ones, leave them alone."

a DTC supplements CX lead, eesel customer research

That is a product requirement, not a model requirement. Confidence gating, escalation rules, scoped topic coverage, and a dry run against tickets you have already answered are what decide whether AI support works. None of it appears on a leaderboard. If you want the longer version, I wrote up why AI chatbots answer incorrectly separately.

The upstream work matters just as much. Getting ticket classification and support tagging right is what lets a bot know which tickets it should not touch, and a well-maintained knowledge base is what stops it inventing an answer in the first place.

The build-versus-buy version of this comes up constantly too:

"We could try to write our own LLM application but we didn't want to invest our time into that. We wanted something that we would not have to maintain."

Karel, GENERAL BYTES

Models like 3.8 Flash are infrastructure. They are the engine, not the employee. Wiring one into a helpdesk means building retrieval over your docs and past tickets, tool calls into your ticketing system, confidence thresholds, escalation paths, and a way to test all of it before a customer sees it. That is the actual project, and it is the same project whether the model underneath is Gemini, Claude or an OpenAI model.

## Try eesel

If what you actually want is AI answering tickets rather than a model to wire up yourself, eesel is where I would start. eesel sells ready-to-work AI teammates rather than raw infrastructure, and the AI helpdesk teammate joins your existing queue in Zendesk, Freshdesk or Gorgias, trained on your help centre and your past tickets, in a few minutes.

The differentiator is the part model benchmarks cannot give you. Every eesel rollout gets simulated against your real historical tickets first, so you see what the AI would have said to customers you already served, before it says anything to a live one. That is the check that catches the "we support all models" failure, and it exists because eesel has watched confident bots get things wrong on real queues for years. It is free to try, and the AI blog writer teammate runs on the same platform if content is your bottleneck rather than support.

For more on the surrounding decisions, my AI helpdesk software roundup covers the market, and ticket automation covers the workflow layer underneath it.

On the money side, AI support cost savings has the maths, and agent versus human cost is the comparison finance teams actually ask for.

And if you are still model-shopping rather than product-shopping, Gemini alternatives and top AI agents are the two most useful starting points.

## Frequently Asked Questions

`gemini-3.8-flash`. It handles text, image, audio, video and PDF input with a 1M token context window and 64K output, and it is tuned for long-horizon coding and autonomous agents. Google's own model card says it is built on Gemini 3.7 Flash rather than a new base model. If you are comparing it against the wider field, my Gemini alternatives roundup covers the rest.
