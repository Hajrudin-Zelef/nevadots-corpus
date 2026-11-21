---
id: collect-261121-veille-ia/muse/meta-s-muse-ember-a-low-cost-ai-model-for-agents
title: "Meta's Muse Ember: A Low-Cost AI Model for Agents"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["ember", "muse", "meta", "model", "tokens", "agents", "intelligence", "haiku"]
source: https://www.datacamp.com/blog/muse-ember
source_anchor: ""
source_lines: [1, 299]
section: "Muse"
---
This week, Meta announced Muse Ember, its smallest model. Ember doesn't compete on hard reasoning tasks. Instead, it does the repeated steps of an agent workflow, while a larger model like Muse Wonder does the planning.

Muse Ember costs $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 200,000 tokens. Meta says it delivers more intelligence per dollar than Claude Haiku 5.5 and GPT-6 Luna, and that it matches Haiku's best Artificial Analysis Intelligence Index score at about a third of the cost per task.

In this article, I'll walk you through what Muse Ember is, how its benchmarks and pricing compare, and where it's placed when compared to the larger Muse models.

But what is an AI Agent? Enroll in our Introduction to AI Agents course and understand the fundamentals in just 90 minutes.

Muse Ember is a lightweight multimodal reasoning model from Meta Superintelligence Labs. It's built for the work agents repeat all day, such as tool calls, coding subtasks, routing, data extraction, and long-document work.

It takes text, image, audio, and video inputs, so an agent doesn't need a separate model for each. The context window is 1 million tokens, and it can produce up to 512,000 output tokens. You can use it through Muse Code and the Meta Model API.

|  | Muse Ember | 
|---|---|
| Developer | Meta Superintelligence Labs | 
| Model ID | muse-ember-1.0 | 
| Input types | Text, image, audio, video | 
| Context window | 1,000,000 tokens | 
| Maximum output | 512,000 tokens | 
| API support | Responses API | 
| Availability | Muse Code, Meta Model API | 

 
 
 
 
 
 
 
 
 
 
 
Meta says it's the "smallest" model, but that just refers to its position in Meta's model portfolio. The announcement doesn't state a parameter count.

A chatbot makes one model call per answer, while an agent makes hundreds of them in a session. Meta says most of its tokens go to small, structured steps. For example:

- Retrieval: Read a file or open a document
- Tool calls: Run a test or call an API
- Routing: Send each request to the right workflow
- Edits: Update an application after a targeted code change

Most of these steps don't need a frontier-level model, but the cost adds up. Even a small saving per call matters when an agent makes hundreds or thousands of them.

Meta's answer is a division of labor: "Muse Wonder plans, Muse Ember handles the volume." Wonder takes the hard reasoning, and Ember runs the repeated steps.

Let's say a team migrates a large codebase to a new framework. Wonder plans the order of changes and splits the work into tasks. Ember then reads each file, makes the targeted edit, and runs the tests, hundreds of times over.

## Muse Ember Benchmarks

Meta's claim is that Muse Ember delivers competitive intelligence at a much lower cost than other small models. On the Artificial Analysis Intelligence Index, Ember and Claude Haiku 5.5 both score 36, and GPT-6 Luna scores 28. The difference is cost per task: about $0.14 for Ember, versus about $0.41 for Haiku and $0.15 for Luna.

| Benchmark | Muse Ember | Claude Haiku 5.5 | GPT-6 Luna | 
|---|---|---|---|
| **Artificial Analysis Intelligence Index** | 36 | 36 | 28 | 
| **AutomationBench-AA** | 37.8% | 13.9% | 20.2% | 
| **Agents’ Last Exam** | 61.6% | 57.0% | 57.1% | 
| **Terminal-Bench 4.0** | 40.9% | 39.2% | 16.4% | 
| **FrontierCode 2.0-AA** | 27.2% | 34.9% | 25.2% | 

 
 
 
 
 
 
 
 
 
 
Meta reported these results on November 9, 2026, at maximum reasoning effort. The Intelligence Index, AutomationBench-AA, and FrontierCode 2.0-AA come from Artificial Analysis. Agents' Last Exam is a Meta-internal evaluation. Terminal-Bench 4.0 uses each model's best published harness. You can find the full details in Meta's evaluation report.

### Agentic workflows

Ember's biggest lead is on AutomationBench-AA, which measures end-to-end business workflows. It scores 37.8%, versus 13.9% for Haiku and 20.2% for Luna.

On Agents' Last Exam, the lead is a bit smaller - 61.6%, versus 57.0% and 57.1%. That one is a Meta-internal evaluation on a Linux subset, so it's Meta's own result.

### Coding

Ember scores 40.9% on Terminal-Bench 4.0, versus 39.2% for Haiku and 16.4% for Luna. But each model ran in its own harness, so the comparison isn't an equal one. On the Artificial Analysis Coding Agent Index, Ember scores 46, versus 37 for Haiku and 31 for Luna.

Haiku wins on FrontierCode 2.0-AA, which judges engineering tasks on mergeability. It scores 34.9%, versus 27.2% for Ember and 25.2% for Luna.

### Multimodal tasks

Against Haiku, the multimodal gaps are not that noticeable. On OSWorld 2.1, a computer-use test, Ember scores 69.5% with partial-credit scoring, versus 68.9% for Haiku and 50.5% for Luna.

On PerceptionBench, which measures fine visual detail, it scores 80.6% with tools, versus 78.6% and 62.8%. Both are Meta-internal evaluations. On GDP.pdf-AA, which covers professional work over long documents, Ember and Luna tie at 22.8%, and Haiku scores 20.8%.

## Muse Ember vs Claude Haiku 5.5 and GPT-6 Luna

All three models list at $0.10 per million input tokens and $0.50 per million output tokens for short prompts. That's why it's important to compare them and note the differences.

On intelligence per dollar, Meta says Ember matches Haiku's best Intelligence Index score at about a third of the cost per task, which is roughly $0.14 versus $0.41. The token prices are the same, so the gap comes from how many tokens each model uses per task. Luna costs about the same as Ember at $0.15 per task, but it scores 28 instead of 36.

Ember also leads Meta's agentic and multimodal tests. It's the only one of the three that takes audio and video, and it allows up to 512,000 output tokens, versus 128,000 for the other two. The context windows are about equal.

Haiku is stronger in one place. It scores higher on FrontierCode 2.0-AA, which judges engineering tasks by mergeability. This is important if your agents write code that goes straight into a repository.

Ember looks like a good option for high-volume workflows built from routine steps. But Meta chose and reported these comparisons, so test it on your own workload before you switch.

|  | Muse Ember | Claude Haiku 5.5 | GPT-6 Luna | 
|---|---|---|---|
| **Price per 1M tokens (input/output)** | $0.10 / $0.50 | $0.10 / $0.50 | $0.10 / $0.50 | 
| **Higher rates start above** | 200K tokens | 100K tokens | 272K tokens | 
| **Context window** | 1M tokens | 1M tokens | 1.05M tokens | 
| **Maximum output** | 512K tokens | 128K tokens | 128K tokens | 
| **Input types** | Text, image, audio, video | Text, image | Text, image | 
| **Index score** | 36 | 36 | 28 | 
| **Cost per Index task** | About $0.14 | About $0.41 | About $0.15 | 

 
Index scores and cost per task are at maximum reasoning effort. Pricing details for Haiku 5.5 and GPT-6 Luna come from their published rates.

## Muse Ember Pricing and Availability

Here's what Muse Ember costs per million tokens for prompts of up to 200,000 input tokens:

| API Usage | Price per 1M tokens | 
|---|---|
| Input | $0.10 | 
| Cached input | $0.004 | 
| Output | $0.50 | 

 
Longer prompts cost more. Above 200,000 input tokens, Meta's rates rise to $0.30 for input and $1.50 for output, and cached input goes up to $0.012.

That's three times the short-prompt price. So the headline pricing doesn't apply to every context length, and if your agents regularly send very large prompts, plan around that.

The API model ID is `muse-ember-1.0`, and Ember supports the Responses API. You can use it through Muse Code and the Meta Model API. Meta also lists it on Google Cloud and Oracle Cloud.

Meta describes its Muse models as a portfolio, with each one built for a different task.

### Muse Wonder

Wonder is for the hardest problems, such as difficult reasoning and complex tasks. In an agent workflow, it's the model that plans.

### Muse Spark

Spark is for coding and broader agentic workflows. In Meta's comparison, Spark 1.3 scores 37 on the Intelligence Index versus 36 for Ember, but it costs $2.39 per task versus $0.14. Its list price is $1.25 per million input tokens and $4.25 per million output tokens.

### Muse Ember

Ember is for repeatable coding and agentic tasks at scale. Meta says it's also the first model trained with Muse Wonder Full.

## What Can Developers Build with Muse Ember?

Meta's announcement and its early testers point to a few concrete uses:

- Document analysis: Box used it to score contracts against a negotiation playbook, clause by clause
- Repository work: Cloudflare used it for multi-repo investigation, and Meta says it can run tests and make targeted fixes
- Multimodal extraction: It can read scanned documents or work from meeting recordings
- Cross-app workflows: It can get records and update systems across applications

Let's say a customer-support agent receives a refund request with a screenshot attached. The larger model decides how to resolve the case. Ember then reads the screenshot, gets the order record, and updates the ticket.

The same sequence runs for every ticket, so the price per call determines what the whole queue costs.

## What Muse Ember Means for the Future of AI Agents

The number to watch is the cost per completed task, not the cost per token. A model with a low token price can still be expensive if it needs a lot of tokens to finish a job.

That changes how you choose models. Instead of looking for the strongest one, you match each step to the right capability and price. A larger model plans, a smaller one runs the repeated steps, and a multimodal model like Ember reads screenshots and recordings without a separate model for each.

With long autonomous workflows, the routine steps have to be cheap for the whole run to be affordable.

But Meta isn't the only lab that sells a small model for this role. Haiku 5.5 and GPT-6 Luna list at the same token price, so keep them in mind.

## Conclusion

Muse Ember is built for repetitive work AI agents do all day. It costs $0.10 per million input tokens and $0.50 per million output tokens, has a 1 million token context window, and takes text, image, audio, and video inputs. Meta reports that it matches Haiku 5.5’s score on the Artificial Analysis Intelligence Index at about a third of the cost per task.

It’s meant to work next to larger models like Muse Wonder, which plans while Ember runs the repeated steps. But Meta ran most of these comparisons, and your own results and savings will depend on your workload and evaluation setup.

But the important takeaway is that if Meta is right, the next competitive edge in AI agents may come as much from how smartly a system splits the work as from how capable its largest model is.

What are the Best AI Agents in 2026? Read our blog post to compare frameworks, no-code tools, enterprise platforms, and get step-by-step guidance to choose and deploy agentic automation.

This week, Meta announced Muse Ember, its smallest model. Ember doesn't compete on hard reasoning tasks. Instead, it does the repeated steps of an agent workflow, while a larger model like Muse Wonder does the planning.

Muse Ember costs $0.10 per million input tokens and $0.50 per million output tokens for prompts up to 200,000 tokens. Meta says it delivers more intelligence per dollar than Claude Haiku 5.5 and GPT-6 Luna, and that it matches Haiku's best Artificial Analysis Intelligence Index score at about a third of the cost per task.

In this article, I'll walk you through what Muse Ember is, how its benchmarks and pricing compare, and where it's placed when compared to the larger Muse models.

But what is an AI Agent? Enroll in our Introduction to AI Agents course and understand the fundamentals in just 90 minutes.

## What Is Meta Muse Ember?

Muse Ember is a lightweight multimodal reasoning model from Meta Superintelligence Labs. It's built for the work agents repeat all day, such as tool calls, coding subtasks, routing, data extraction, and long-document work.

It takes text, image, audio, and video inputs, so an agent doesn't need a separate model for each. The context window is 1 million tokens, and it can produce up to 512,000 output tokens. You can use it through Muse Code and the Meta Model API.

Meta says it's the "smallest" model, but that just refers to its position in Meta's model portfolio. The announcement doesn't state a parameter count.

## Why Meta Built Muse Ember for AI Agents

A chatbot makes one model call per answer, while an agent makes hundreds of them in a session. Meta says most of its tokens go to small, structured steps. For example:

Most of these steps don't need a frontier-level model, but the cost adds up. Even a small saving per call matters when an agent makes hundreds or thousands of them.

Meta's answer is a division of labor: "Muse Wonder plans, Muse Ember handles the volume." Wonder takes the hard reasoning, and Ember runs the repeated steps.

Let's say a team migrates a large codebase to a new framework. Wonder plans the order of changes and splits the work into tasks. Ember then reads each file, makes the targeted edit, and runs the tests, hundreds of times over.

## Muse Ember Benchmarks

Meta's claim is that Muse Ember delivers competitive intelligence at a much lower cost than other small models. On the Artificial Analysis Intelligence Index, Ember and Claude Haiku 5.5 both score 36, and GPT-6 Luna scores 28. The difference is cost per task: about $0.14 for Ember, versus about $0.41 for Haiku and $0.15 for Luna.

Artificial Analysis Intelligence IndexAutomationBench-AAAgents’ Last ExamTerminal-Bench 4.0FrontierCode 2.0-AA
Meta reported these results on November 9, 2026, at maximum reasoning effort. The Intelligence Index, AutomationBench-AA, and FrontierCode 2.0-AA come from Artificial Analysis. Agents' Last Exam is a Meta-internal evaluation. Terminal-Bench 4.0 uses each model's best published harness. You can find the full details in Meta's evaluation report.

## Agentic workflows

Ember's biggest lead is on AutomationBench-AA, which measures end-to-end business workflows. It scores 37.8%, versus 13.9% for Haiku and 20.2% for Luna.

On Agents' Last Exam, the lead is a bit smaller - 61.6%, versus 57.0% and 57.1%. That one is a Meta-internal evaluation on a Linux subset, so it's Meta's own result.

## Coding

Ember scores 40.9% on Terminal-Bench 4.0, versus 39.2% for Haiku and 16.4% for Luna. But each model ran in its own harness, so the comparison isn't an equal one. On the Artificial Analysis Coding Agent Index, Ember scores 46, versus 37 for Haiku and 31 for Luna.

Haiku wins on FrontierCode 2.0-AA, which judges engineering tasks on mergeability. It scores 34.9%, versus 27.2% for Ember and 25.2% for Luna.

## Multimodal tasks

Against Haiku, the multimodal gaps are not that noticeable. On OSWorld 2.1, a computer-use test, Ember scores 69.5% with partial-credit scoring, versus 68.9% for Haiku and 50.5% for Luna.

On PerceptionBench, which measures fine visual detail, it scores 80.6% with tools, versus 78.6% and 62.8%. Both are Meta-internal evaluations. On GDP.pdf-AA, which covers professional work over long documents, Ember and Luna tie at 22.8%, and Haiku scores 20.8%.

## Muse Ember vs Claude Haiku 5.5 and GPT-6 Luna

All three models list at $0.10 per million input tokens and $0.50 per million output tokens for short prompts. That's why it's important to compare them and note the differences.

On intelligence per dollar, Meta says Ember matches Haiku's best Intelligence Index score at about a third of the cost per task, which is roughly $0.14 versus $0.41. The token prices are the same, so the gap comes from how many tokens each model uses per task. Luna costs about the same as Ember at $0.15 per task, but it scores 28 instead of 36.

Ember also leads Meta's agentic and multimodal tests. It's the only one of the three that takes audio and video, and it allows up to 512,000 output tokens, versus 128,000 for the other two. The context windows are about equal.

Haiku is stronger in one place. It scores higher on FrontierCode 2.0-AA, which judges engineering tasks by mergeability. This is important if your agents write code that goes straight into a repository.

Ember looks like a good option for high-volume workflows built from routine steps. But Meta chose and reported these comparisons, so test it on your own workload before you switch.

Price per 1M tokens (input/output)Higher rates start aboveContext windowMaximum outputInput typesIndex scoreCost per Index task
Index scores and cost per task are at maximum reasoning effort. Pricing details for Haiku 5.5 and GPT-6 Luna come from their published rates.

## Muse Ember Pricing and Availability

Here's what Muse Ember costs per million tokens for prompts of up to 200,000 input tokens:

Longer prompts cost more. Above 200,000 input tokens, Meta's rates rise to $0.30 for input and $1.50 for output, and cached input goes up to $0.012.

That's three times the short-prompt price. So the headline pricing doesn't apply to every context length, and if your agents regularly send very large prompts, plan around that.

The API model ID is

`muse-ember-1.0`, and Ember supports the Responses API. You can use it through Muse Code and the Meta Model API. Meta also lists it on Google Cloud and Oracle Cloud.
## How Muse Ember Fits into Meta's AI Model Family

Meta describes its Muse models as a portfolio, with each one built for a different task.

## Muse Wonder

Wonder is for the hardest problems, such as difficult reasoning and complex tasks. In an agent workflow, it's the model that plans.

## Muse Spark

Spark is for coding and broader agentic workflows. In Meta's comparison, Spark 1.3 scores 37 on the Intelligence Index versus 36 for Ember, but it costs $2.39 per task versus $0.14. Its list price is $1.25 per million input tokens and $4.25 per million output tokens.

## Muse Ember

Ember is for repeatable coding and agentic tasks at scale. Meta says it's also the first model trained with Muse Wonder Full.

## What Can Developers Build with Muse Ember?

Meta's announcement and its early testers point to a few concrete uses:

Let's say a customer-support agent receives a refund request with a screenshot attached. The larger model decides how to resolve the case. Ember then reads the screenshot, gets the order record, and updates the ticket.

The same sequence runs for every ticket, so the price per call determines what the whole queue costs.

## What Muse Ember Means for the Future of AI Agents

The number to watch is the cost per completed task, not the cost per token. A model with a low token price can still be expensive if it needs a lot of tokens to finish a job.

That changes how you choose models. Instead of looking for the strongest one, you match each step to the right capability and price. A larger model plans, a smaller one runs the repeated steps, and a multimodal model like Ember reads screenshots and recordings without a separate model for each.

With long autonomous workflows, the routine steps have to be cheap for the whole run to be affordable.

But Meta isn't the only lab that sells a small model for this role. Haiku 5.5 and GPT-6 Luna list at the same token price, so keep them in mind.

## Conclusion

Muse Ember is built for repetitive work AI agents do all day. It costs $0.10 per million input tokens and $0.50 per million output tokens, has a 1 million token context window, and takes text, image, audio, and video inputs. Meta reports that it matches Haiku 5.5’s score on the Artificial Analysis Intelligence Index at about a third of the cost per task.

It’s meant to work next to larger models like Muse Wonder, which plans while Ember runs the repeated steps. But Meta ran most of these comparisons, and your own results and savings will depend on your workload and evaluation setup.

But the important takeaway is that if Meta is right, the next competitive edge in AI agents may come as much from how smartly a system splits the work as from how capable its largest model is.

What are the Best AI Agents in 2026? Read our blog post to compare frameworks, no-code tools, enterprise platforms, and get step-by-step guidance to choose and deploy agentic automation.
