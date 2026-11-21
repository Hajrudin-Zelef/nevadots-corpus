---
id: collect-261121-veille-ia/muse/what-is-muse-ember-2026-built-with-muse
title: "What Is Muse Ember? (2026) | Built with Muse"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-11-09"]
keywords: ["ember", "muse", "model", "meta", "built", "work", "developers", "wonder"]
source: https://builtwithmuse.com/blog/muse-ember
source_anchor: ""
source_lines: [1, 126]
section: "Muse"
---
**Muse Ember is Meta's smallest Muse model, built for high volume agent work at low cost, and it is for developers, not for app users.**Meta launched Ember on November 9, 2026, on the Meta Model API and Muse Code, at $0.10 per 1M input tokens and $0.50 per 1M output tokens, with a 1M token context window. It matches Claude Haiku 5.5 on the Artificial Analysis Intelligence Index at about a third of the cost per task. Nothing in the Muse consumer app changes because of Ember.

## What is Muse Ember?

Muse Ember is the smallest and most efficient model in Meta's Muse family. Meta Superintelligence Labs introduced it on November 9, 2026, as a model designed for complex reasoning execution and for the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing. Meta's announcement post puts the idea in one line: Ember delivers more intelligence per dollar than its small model rivals, matching the best score of Claude Haiku 5.5 at about a third of the cost.

Think of Ember as the worker model. A personal agent spends most of its tokens on many small, structured steps: reading a file, calling a tool, checking a result, routing a task to the right place. Those steps do not need the biggest model. They need a fast, cheap model that is good enough to get each step right, thousands of times a day. Ember is built for exactly that volume. The big models plan. Ember executes.

Ember is multimodal. It natively understands text, images, audio, and video, so an agent built on Ember can read screenshots, charts, and scanned documents, or work from voice memos, meeting recordings, and video, without a separate model. It can work through long real world documents, operate a computer directly from screenshots, and pick out fine visual detail. That breadth matters for agents, because real work arrives in every format at once.

Ember is also the first model trained using Muse Wonder Full, Meta's largest model. Meta said when it introduced Wonder that it would use Wonder to develop specialized models it could bring to everyone, and Ember is the first of those. The pattern is worth watching: a frontier model used to teach smaller, cheaper models that do specific jobs well. Ember is the proof that the pattern works, at least for agent execution.

## When did Muse Ember launch, and where can you use it?

November 9, 2026. Meta announced Ember in a research post the same day it became available, with no waitlist and no staged rollout described. It launched on two surfaces at once: the Meta Model API, where developers call it over the internet, and Muse Code, Meta's coding tool. Meta's post also says Ember, Spark, and Wonder are now available on Google Cloud and Oracle Cloud, which gives developers more than one way to reach the model.

Ember is cloud only. Meta has published no weights, no download, and no way to run Ember on your own hardware. That is a deliberate difference from Glimmer, the open twin of Spark that Meta released in August 2026 with open weights. If you want to run a Muse model locally, Ember is not the one. If you want to call a cheap, fast model over an API, Ember is the one. Launch coverage frames the choice plainly: Ember is a cloud subagent model, not a local model.

Ember supports the Responses API, which is the interface developers use to build agents with tool calling and structured steps. The model ID developers use is given in the developer section near the end of this guide. There is no app, no website, and no chat interface for Ember itself. You reach it through code, through Muse Code, or through a cloud provider that hosts it.

The launch date matters for context. Ember arrived two months after Muse launched in the US, one month after Muse Wonder arrived in October, and four days after Muse launched in Mexico. Meta is shipping model news and product news on parallel tracks, and they are easy to confuse. Ember is model news. It does not add a feature to the Muse app, change the Muse pricing, or open Muse in a new country. Those are product tracks, and they move separately.

## What is Muse Ember for?

Ember is for the repeated steps inside agent workflows. Meta names five: tool calls, coding subtasks, routing, extraction, and long document parsing. In practice, that means the agent that reads fifty files to find one answer, the agent that calls a dozen tools in sequence to complete a business workflow, the agent that pulls records from one app and updates another, and the agent that scores a contract clause by clause against a playbook. Each step is small. Together, they are the bulk of what agents actually do.

The Box example from the launch shows the shape. Yashodha Bhavnani, VP of AI Products at Box, said Ember can score a contract against a company's negotiation playbook clause by clause and land on the right overall risk rating, or screen candidates against a detailed brief and flag the borderline ones who still qualify as alternates. Her summary: Muse Ember is a fast, efficient foundation for agents doing real work on enterprise content. That is the target customer in one sentence: enterprises running agents over large volumes of documents.

Ember is also for coding subtasks at volume. Meta describes the coding jobs an agent sends off hundreds of times a session: reading files, running tests, making targeted fixes. Ember handles those, while a bigger model plans the hard problems. Meta's pairing advice is explicit: for the hardest problems, pair Ember with Muse Wonder. Wonder plans, Ember handles the volume. The same logic applies to Spark: plan with the big model, execute with the small one, and keep the bill low.

Ember is not for everything. It is not the model for the hardest reasoning, the longest planning, or the most delicate judgment. Those jobs still belong to Wonder and Spark. And it is not a consumer product at all: there is no Ember app, no Ember chat, and no Ember setting in Muse. If you are not a developer, a builder, or someone who pays an API bill, Ember affects you only indirectly, through cheaper and faster agents built on it.

## How much does Muse Ember cost?

Meta's list pricing is $0.10 per 1M input tokens, $0.004 per 1M cached input tokens, and $0.50 per 1M output tokens. That is the price Meta published in its announcement post, and it is the price to use for planning. For comparison, Meta lists Muse Spark 1.3 at $1.25 per 1M input tokens and $4.25 per 1M output tokens. Ember input is twelve and a half times cheaper than Spark input, and Ember output is eight and a half times cheaper than Spark output.

There is a second pricing tier that Meta's post does not spell out, and it matters for long context work. Launch coverage from eesel reports that the list price applies up to 200K input tokens, and that above 200K input tokens the whole request reprices to $0.30 per 1M input tokens, $0.012 per 1M cached input tokens, and $1.50 per 1M output tokens. Note the word whole: it is not only the tokens above 200K that cost more, the entire request moves to the higher tier. If you plan to use Ember's full 1M token context window, budget at the higher tier, and confirm the current tiers on Meta's pricing page before you commit.

The cost per task is where Ember's case is strongest. Meta reports about $0.14 per Artificial Analysis task for Ember at max reasoning, against $0.41 for Claude Haiku 5.5 and $2.39 for Muse Spark 1.3 at max reasoning. On Meta's own numbers, Ember is about 17 times cheaper per task than Spark 1.3, for an Intelligence Index score of 36 against Spark's 37. That is the trade in one line: nearly the same measured intelligence, at a small fraction of the cost. It is why Meta calls Ember a price that scales, and why it says it has even more efficient models in training.

For developers, the practical question is not the sticker price, it is the monthly bill for a real workload. An agent that makes thousands of small calls a day lives or dies on input token price and cached input price, because most agent calls repeat a lot of context. Ember's cached input price of $0.004 per 1M tokens is the number that makes repeated agent loops affordable. Run your own workload through the pricing, with the 200K tier boundary in mind, before you assume the headline rate is the rate you will pay.

## How good is Muse Ember?

On Meta's chosen benchmarks, Ember is the efficiency leader among small models. The headline comparison, from Artificial Analysis data dated November 9, 2026, and published in Meta's post, gives Ember an Intelligence Index of 36, the same as Claude Haiku 5.5 at max reasoning, and ahead of GPT-6 Luna at 28. Meta's claim is that no model scores higher for less per task, and that Ember sits on the efficiency frontier of the full Artificial Analysis leaderboard. Those are Meta's words about third party data, and they are the strongest form of the claim. Treat them as a vendor's best case, not as a neutral verdict.

The agent benchmarks are where Ember shines brightest. On AutomationBench, a test of end to end business workflows, Ember scored 37.8, against 13.9 for Haiku 5.5 and 20.2 for GPT-6 Luna. On Agents' Last Exam, a test of end to end agent tasks, Ember scored 61.6, against 57.0 and 57.1. On Terminal Bench 4.0, a test of agentic terminal coding, Ember scored 40.9, against 39.2 and 16.4. On OSWorld 2.1, a test of computer use across desktop apps, Ember scored 69.5 on partial credit, against 68.9 and 50.5. On PerceptionBench with tools, Ember scored 80.6, against 78.6 and 62.8. The pattern is consistent: Ember leads on agent work, coding in harnesses, computer use, and visual perception.

The honest weak spot is hard engineering. On FrontierCode 2.0, a test of real engineering tasks judged on mergeability, Ember scored 27.2 percent, behind Haiku 5.5 at 34.9 percent, though ahead of GPT-6 Luna at 25.2 percent. That gap is the reason Meta pairs Ember with Wonder for the hardest problems: Ember executes volume, Wonder handles depth. Coverage of the Artificial Analysis review also flags a second weak spot: on knowledge questions, Ember answered 21.1 percent correctly and declined to answer 64 percent of the time, which reads as caution, not knowledge. A launch summary notes the same trade: strong on agent execution, weaker on recall.

How much should you trust these numbers? They are vendor selected benchmarks, published by Meta, from a third party evaluator, on launch day. That is better than vendor only numbers, and worse than independent testing after release. The benchmarks favor the jobs Ember was built for, which is fair, because those are the jobs buyers will use it for. But no benchmark tells you how Ember handles your documents, your tools, and your edge cases. If you are choosing a model for production agent work, run your own evaluation on your own workload, with the same prompts, against Haiku 5.5 and your current model, and decide on your numbers, not Meta's.

On safety, Meta says Ember was evaluated under the Meta Superintelligence Scaling Framework, covering chemical and biological risks, cybersecurity risks, and loss of control risks, and that it remained below the framework's high risk thresholds across the categories assessed. Meta also says Ember received the same core safety training and testing as its larger models, and was evaluated on how it behaves when acting on users' behalf: whether it respects user intent, handles sensitive information appropriately, resists instructions hidden in web pages and documents that try to redirect it, and seeks human input before high impact actions. Those are Meta's claims, published in its own post, and they are the right questions to ask of any agent model. Independent safety evaluation will take time.

## How does Ember fit with Spark and Wonder?

Ember is the fourth Muse model, and the family now has a clear shape. Muse Spark arrived in April 2026 as the coding and agentic workflows model. Glimmer arrived in August 2026 as Spark's open twin, with open weights for developers who want to run a model themselves. Muse Wonder arrived in October 2026 as the model for the hardest problems, with a larger Wonder Full variant used to train specialized models. Ember, from November 2026, is the small, cheap, fast model for repeatable work at scale. Launch coverage describes the same four model lineup.

Meta's own summary of the portfolio is worth quoting in full, because it is the clearest statement of how the models divide labor: Muse Wonder for the hardest problems, Muse Spark for coding and agentic workflows, and Muse Ember for repeatable coding and agentic tasks at scale. Read our guide to Muse Wonder for what the top of the family does. The practical rule for developers is to route by job: Wonder plans, Spark builds, Ember repeats. A workflow that uses only one model for all three jobs is either overpaying or underperforming.

The training relationship matters too. Ember is the first model trained using Muse Wonder Full, and Meta says it has even more efficient models in training. That suggests Ember is not a one off, it is the first product of a pipeline: frontier model teaches specialized model, specialized model ships cheap. If that pipeline keeps working, expect more small Muse models tuned for specific jobs, at lower prices, in the months ahead. For developers, that means the model you pick today may not be the cheapest good option in six months, and routing logic that can swap models will age better than hard coded model choices.

For Muse app users, the family shape explains something you may have noticed: the app gets smarter without a new version number. Meta says it is rolling out specialized versions of its models to Muse, trained with learnings from Muse Wonder. Ember itself is not in the app, but the technique behind Ember, specialized models distilled from Wonder, is how the app improves. The model news and the product news are connected, just not in the way a headline suggests.

## Is Muse Ember open source?

No. Ember is cloud only, and Meta has published no weights. You cannot download Ember, run it on your own hardware, or fine tune it yourself. You call it over the Meta Model API, use it inside Muse Code, or reach it through Google Cloud or Oracle Cloud. That is a clear break from Meta's open weights history, and from Glimmer, the open twin of Spark that shipped with open weights in August 2026.

Why would Meta keep Ember closed? The honest answer is that Meta has not said, and any reason offered here would be speculation. What can be said is what the closure means for developers. You cannot self host Ember, you cannot inspect its weights, and you depend on Meta's API availability and pricing. In exchange, you get a managed model with no infrastructure to run, no GPU to buy, and a price low enough to use at volume. That trade suits teams that want an API, and it does not suit teams that need to run a model inside their own walls.

If open weights matter to you, Glimmer is the Muse family member to look at, not Ember. Glimmer is Spark's open twin, and it is the model Meta offers for developers who want weights. Ember and Glimmer are different answers to different needs: Ember is cheap managed execution, Glimmer is open weights you control. Do not confuse them, and do not wait for Ember weights that Meta has not promised.

The cloud only choice also affects privacy planning. Every Ember call sends your data to Meta's API or to a cloud provider hosting the model. If your workload involves sensitive documents, read the data handling terms for the surface you use, and check whether your data is used for training, how long it is retained, and where it is processed. Those terms live in Meta's API documentation and the cloud provider's terms, not in the launch post, and they matter more than the benchmark scores for regulated work.

## Does Muse Ember change the Muse app?

No. Ember changes nothing in the Muse app on your phone, your Mac, or the web. There is no Ember setting, no Ember toggle, no Ember feature to turn on, and no Ember badge in the app. If you use Muse as a consumer, November 9, 2026, was an ordinary day. The app you opened on November 10 was the app you opened on November 8, as far as Ember is concerned.

This is the most common confusion about Ember, and it is worth stating twice. Headlines that say Meta launched a new Muse model do not mean your Muse app changed. Models and apps are different layers. The app is the interface you touch. The model is the engine behind it. Meta can ship a new engine for developers without touching the interface, and that is what happened here. Ember is an engine for developers building agents. The Muse app is a product for people living their lives.

Could Ember affect the app indirectly? Yes, over time. If developers build cheaper, faster agents on Ember, some of those agents may reach consumers through products that use the Meta Model API. And Meta's technique of distilling specialized models from Wonder may improve the models inside the Muse app. But those are second order effects, on no published timeline. If you are waiting for a visible change in the Muse app, watch for product announcements like Muse Spaces or Muse Voice, not for model launches like Ember.

If you came to this guide because you use the Muse app and you wondered whether you need to do anything about Ember, the answer is no. You need to do nothing. Keep using the app as before. If you want to understand what is new in the app itself, read what Muse is and how it works, and check the app's release notes for the version you have. Ember is for the developers building the next wave of agents. You will meet their work when it ships, not before.

## How do developers start with Ember?

Start on the Meta Model API. Ember is available there under the model ID `muse-ember-1.0`, with a 1M token context window and up to 512K output tokens, and it supports the Responses API. If you already call Meta models over the API, Ember is a model ID change and a pricing change, not a new integration. If you are new to the API, Meta's developer documentation covers authentication, the Responses API, and tool calling, and it is the primary source for anything this guide summarizes.

Start inside Muse Code if you write code for a living. Ember is available in Muse Code as the model for coding subtasks at volume: reading files, running tests, making targeted fixes. The pairing advice applies here too: let Wonder or Spark plan the hard problems, and let Ember handle the repeated steps. If your Muse Code bill is mostly small, repeated calls, Ember is the model to route those calls to first.

Start with your own evaluation, not with Meta's benchmarks. Pick ten real tasks from your workload: five agent execution tasks, three coding subtasks, two long document tasks. Run them on Ember, on Haiku 5.5, and on your current model, with the same prompts, and score the outputs blind if you can. Measure cost per task as well as quality, and watch the 200K input token tier boundary, because a workload that crosses it pays the higher rate on the whole request. Launch coverage makes the same point: Ember's case is cost per task on real work, not a leaderboard score.

Start narrow. Route one job to Ember first, such as extraction or routing, and watch it for a week before you route more. Agent models fail in specific ways on specific workloads, and a week of real traffic tells you more than a day of benchmarks. Keep a fallback: if Ember declines to answer too often on your knowledge heavy tasks, route those to a bigger model and keep Ember on execution. The goal is a routing table, not a single model for everything.

## What should you watch next?

Watch for the next specialized model. Meta says it has even more efficient models in training, and Ember is the first trained using Muse Wonder Full. If the pipeline works, more small models will follow, each tuned for a job, each cheaper than the last. For developers, that means routing logic matters more than model loyalty. Build the router, not the marriage.

Watch for independent evaluation. Launch day benchmarks are a vendor's best case. Independent testing, production reports, and cost analyses from teams running Ember at volume will arrive over the coming weeks, and they will say more about Ember's real strengths and failure modes than any launch post. Pay attention to reports on the knowledge question weakness and the FrontierCode gap, because those are the places where Ember's limits will show first.

Watch for pricing changes. The 200K tier boundary, the cached input rate, and the per task cost are the numbers that decide Ember's real bill, and any of them can change. Meta has already shown, with other products, that early pricing is not final pricing. Check the pricing page before you budget, and recheck it before you scale. A model that is cheap today and expensive in March is a trap for anyone who builds without a routing fallback.

And if you are not a developer, watch for the agents built on Ember, not for Ember itself. The model is infrastructure. The products are what you will touch. When a product you use gets faster, cheaper, or more capable in the coming months, Ember or its successors may be part of the reason, and you will never see the model name. That is how infrastructure is supposed to work: invisible, cheap, and everywhere.

## Get started

Ember is a developer model, so getting started means code, not the app. If you build agents, try Ember on the Meta Model API for your highest volume, most repeated job, and measure cost per task against your current model. If you use the Muse app, nothing changes, and you need to do nothing.

If you are new to Muse itself, get a free code from the community pool and redeem it within 48 hours of joining for the token reward.

## Frequently asked questions

- What is Muse Ember?
- Muse Ember is Meta's smallest and most efficient Muse model, launched November 9, 2026, for developers building agents. It is built for the work agents repeat all day: tool calls, coding subtasks, routing, extraction, and long document parsing. It runs on the Meta Model API and Muse Code, and it is not a change to the consumer app.
- How much does Muse Ember cost?
- Meta's list pricing is $0.10 per 1M input tokens, $0.004 per 1M cached input tokens, and $0.50 per 1M output tokens. Launch coverage reports a second tier above 200K input tokens, where the whole request reprices to $0.30, $0.012, and $1.50. Meta reports about $0.14 per Artificial Analysis task, against $0.41 for Claude Haiku 5.5.
- How does Muse Ember compare with Claude Haiku 5.5?
- On the Artificial Analysis Intelligence Index, both score 36 at max reasoning, but Ember costs about a third as much per task. Ember leads on agent benchmarks like AutomationBench, and trails on hard engineering tasks like FrontierCode, where Haiku scores higher. Meta's numbers are a vendor best case, so run your own evaluation before choosing.
- Is Muse Ember open source?
- No, Ember is cloud only, and Meta has published no weights. You call it over the Meta Model API, use it in Muse Code, or reach it through Google Cloud or Oracle Cloud. If you need open weights, Glimmer, the open twin of Spark, is the Muse family model to look at.
- Does Muse Ember change the Muse app?
- No, there is no Ember setting, toggle, or feature in the Muse app, and app users need to do nothing. Ember is a developer model for building agents, and any effect on the app would be indirect and later, through cheaper agents built on Ember or through the same distillation technique improving app models.
- What is the Muse Ember context window?
- Ember has a 1M token context window, with up to 512K output tokens. That is enough to take in a full codebase or document set in a single call. Note the pricing tier: launch coverage reports that requests above 200K input tokens reprice the whole request to a higher tier, so budget long context work at the higher rate.

Built with Muse is an independent community project. Not affiliated with or endorsed by Meta.
