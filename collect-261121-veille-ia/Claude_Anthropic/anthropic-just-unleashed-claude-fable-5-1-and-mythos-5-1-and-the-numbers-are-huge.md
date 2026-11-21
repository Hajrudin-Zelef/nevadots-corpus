---
id: collect-261121-veille-ia/claude-anthropic/anthropic-just-unleashed-claude-fable-5-1-and-mythos-5-1-and-the-numbers-are-huge
title: "Anthropic Just Unleashed Claude Fable 5.1 and Mythos 5.1 — and the Numbers Are Huge"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "anthropic", "claude", "mythos", "long-running", "model", "work", "benchmark"]
source: https://thewincentral.com/anthropic-claude-fable-5-1-mythos-5-1/
source_anchor: ""
source_lines: [1, 221]
section: "Claude_Anthropic"
---
Anthropic has introduced **Claude Fable 5.1 and Claude Mythos 5.1**, two new versions of its fifth-generation Claude models that push the company further into long-running coding, research, cybersecurity and scientific workloads.

The announcement is particularly interesting because **Fable 5.1 and Mythos 5.1 are essentially the same underlying model**. The major difference is how Anthropic applies safety safeguards to each version.

Fable 5.1 is designed for broad availability, while Mythos 5.1 is reserved for vetted organizations working in areas such as cybersecurity and life sciences. Anthropic says the new models represent an early glimpse at how increasingly capable AI systems could contribute to scientific discovery.

## **Claude Fable 5.1 is built for long-running work**

Anthropic is positioning Fable 5.1 as its most capable generally available model for **coding and knowledge work**.

Rather than focusing only on short prompts and quick answers, the model is designed for tasks that can continue for hours or span multiple applications.

That includes:

- Large software-engineering projects
- Code reviews and performance optimization
- Deep research and analysis
- Multi-stage knowledge work
- Browser-based tasks
- Long-running autonomous agents
- Working across applications and files
- Creating and testing its own code
- Reviewing visual output against a target design

Anthropic says Fable 5.1 can plan a task, use the tools required to complete it, recover when something goes wrong and keep the user updated while it works.

That makes the release less about simply producing better chatbot responses and more about **AI completing entire workflows with less human supervision**.

## **Fable 5.1 posts major benchmark gains**

One of the biggest numbers in Anthropic’s announcement is its result on **Terminal-Bench-Science 0.1**, an agentic benchmark focused on scientific research.

Fable 5.1 scored **52.6%**, compared with **24.7% for Fable 5** and **29.0% for Opus 5** in Anthropic’s testing setup.

Anthropic notes that the benchmark has a relatively large margin of uncertainty, with a standard error of roughly 3.5 to 4.5 percentage points per model, so individual score differences should not be treated as absolute.

Still, the result is notable because Fable 5.1 more than doubles its predecessor’s score on this particular evaluation.

Other reported results include:

| Benchmark | Claude Fable 5.1 | 
|---|---|
| Terminal-Bench-Science 0.1 | **52.6%** | 
| CursorBench 3.2.0 | **73.4%** | 
| Humanity’s Last Exam | **60.9%** | 
| Humanity’s Last Exam with tools | **65.0%** | 
| AutomationBench | **31.4%** | 
| OSWorld 2.0 | **41.7%** | 
| GDPval-AA v2 | **1853** | 

These numbers are particularly relevant because many of the evaluations are aimed at **real-world knowledge work and agentic behavior**, rather than simply testing whether a model can answer isolated questions.

## **Claude Fable 5.1 is also cheaper for agentic workloads**

Anthropic isn’t only talking about better performance.

The company has also made a significant change to the economics of running long-running AI agents.

Fable 5.1 keeps the standard price at:

- **$10 per million input tokens**
- **$50 per million output tokens**

But cache reads have dropped to just **$0.25 per million tokens**.

That’s a **75% reduction** compared with Fable 5. Anthropic estimates that typical workloads can become around **25% cheaper**, while highly agentic workloads can see savings of **up to approximately 45%**.

That’s important because autonomous agents can repeatedly interact with large amounts of context. Cutting the cost of cached context can make long-running workflows considerably more economical.

For developers building agents that spend hours working through a task, this could ultimately matter almost as much as the benchmark improvements.

## **Fable 5.1 can work across multiple applications**

Anthropic is emphasizing that Fable 5.1 isn’t simply a better coding assistant.

The company says the model can handle jobs involving multiple applications, including browser-based workflows, Slack-related tasks and managed agents running on the Claude Platform.

The model can break a project into steps, decide which tools it needs and continue working when individual operations fail.

That is a significant shift in how Anthropic is positioning Claude.

The goal is increasingly to have Claude **take responsibility for completing a project**, rather than waiting for a user to provide the next instruction after every individual step.

## **Claude Mythos 5.1 takes things further**

Then there is **Claude Mythos 5.1**.

Mythos represents Anthropic’s more capable and less restricted version of the underlying model for carefully controlled use cases.

Anthropic says Mythos 5.1 is aimed specifically at **cybersecurity and biology research**, where its capabilities can be particularly valuable but also potentially dangerous if misused.

The company therefore isn’t making Mythos 5.1 generally available.

Access is currently limited to vetted organizations through Anthropic’s trusted access programs. The Life Sciences Verification Program provides access to advanced life-sciences researchers, while Anthropic is also developing its Cyber Verification Program for defensive cybersecurity work.

Mythos 5.1 also powers **Claude Security**, according to Anthropic.

## **Why are there two versions of the same model?**

This is arguably the most interesting part of the release.

Anthropic says **Fable 5.1 and Mythos 5.1 use the same underlying model**.

The difference comes from the safeguards.

Fable 5.1 has protections designed to make Mythos-level capabilities suitable for broader deployment. Anthropic says its cybersecurity safeguards can allow Fable 5.1 to identify software vulnerabilities in source code, while still blocking or limiting more dangerous activities such as penetration testing, exploit generation and binary-based vulnerability scanning.

The biology safeguards have also been redesigned.

Anthropic says benign biology requests now trigger interventions **85% less often** than they did with the safeguards initially introduced for Fable 5.

For dual-use biology and chemistry questions, however, the system can route requests to Opus models instead.

In other words, Anthropic is trying to find a middle ground: **make the model useful enough for legitimate work without exposing its full capabilities in areas where misuse could cause serious harm.**

## **Mythos 5.1 isn’t available to everyone**

This distinction is important for anyone looking to try the new models.

**Claude Fable 5.1 is generally available**, including to Pro, Max, Team and Enterprise users. Developers can also access it through the Claude API and supported cloud platforms, including Amazon Web Services, Google Cloud and Microsoft Foundry.

**Claude Mythos 5.1 is restricted.**

Anthropic currently provides it only to vetted organizations participating in its trusted access programs, with availability focused on cybersecurity and life-sciences research.

So despite the two models launching together, they aren’t really competing products for ordinary Claude users.

## **Fable 5.1 gets better vision and coding capabilities**

Anthropic is also highlighting improvements beyond raw benchmark scores.

Fable 5.1 can understand **diagrams, charts and tables embedded in files and PDFs**, making it more useful for document-heavy professional work.

That could matter in areas such as:

- Finance
- Legal work
- Analytics
- Architecture
- Research
- Technical documentation

The model can also use vision to inspect the results of its own coding work and compare those results against the intended design.

That creates an interesting feedback loop: Claude can write something, inspect the result and use what it sees to determine whether the implementation actually matches the goal.

## **What about privacy and data retention?**

Anthropic is also introducing changes around enterprise privacy and data handling alongside the models.

For Mythos 5.1 specifically, Anthropic says use currently requires acceptance of a **30-day data-retention policy for safety monitoring by default**.

The company is also working on additional enterprise safeguards that are intended to provide stronger privacy protections for organizations deploying advanced models.

This matters because the more capable AI becomes at handling sensitive research, source code and internal company information, the more important data governance becomes.

## **Anthropic is treating Fable 5.1 as a “Mythos-level” model**

Perhaps the clearest way to understand this launch is that Anthropic has effectively taken the capabilities of its restricted Mythos line and created a version that can be deployed more broadly.

Anthropic describes Fable 5.1 as a **Mythos-level model with safeguards for cybersecurity and biology**.

That strategy allows the company to make significant capability improvements available to developers while keeping the most sensitive capabilities behind additional verification.

It also explains why Mythos 5.1 remains important even though Fable 5.1 is the version most users will encounter.

## **The bigger picture: Claude is becoming an agent, not just a chatbot**

The Fable 5.1 launch is ultimately about something bigger than another benchmark race.

Anthropic is betting that the next major phase of AI will involve models that can **work independently for much longer periods of time**.

Instead of:

Ask Claude → get an answer → ask another question


the workflow increasingly becomes:

Give Claude a goal → Claude plans → uses tools → writes code → checks its work → fixes problems → completes the project.


That’s why improvements in agentic benchmarks, context handling, tool use and cache pricing are so important.

If these systems can reliably work for hours rather than minutes, the economics of AI agents become just as important as their intelligence.

And with Fable 5.1, Anthropic is explicitly targeting that future.

## **Claude Fable 5.1 vs Mythos 5.1**

| Feature | Claude Fable 5.1 | Claude Mythos 5.1 | 
|---|---|---|
| Underlying model | Same | Same | 
| General availability | **Yes** | **No** | 
| Coding | Frontier-level | Frontier-level | 
| Long-running agents | **Yes** | **Yes** | 
| Scientific research | With safeguards | **Advanced access** | 
| Cybersecurity | Safeguarded | Reduced safeguards for vetted users | 
| Biology | Safeguarded | Reduced safeguards for vetted researchers | 
| Input price | $10/M tokens | Starts at $10/M | 
| Output price | $50/M tokens | Starts at $50/M | 
| Cache reads | **$0.25/M tokens** | $0.25/M tokens | 
| Access | Pro, Max, Team, Enterprise, API | Vetted organizations | 

## **Final thoughts**

**Claude Fable 5.1 and Claude Mythos 5.1 represent an important change in Anthropic’s strategy.**

Fable 5.1 isn’t simply a faster or slightly smarter Claude. Anthropic is positioning it as a model capable of handling **long-running coding, research and professional workflows with significantly less supervision**.

The **52.6% Terminal-Bench-Science score**, **73.4% CursorBench result**, **65% Humanity’s Last Exam score with tools** and potentially **45% lower costs for highly agentic workloads** make this one of Anthropic’s more consequential releases.

But Mythos 5.1 may be the more intriguing model.

It shows what Anthropic believes its underlying technology can do when it is deployed specifically for vetted cybersecurity and life-sciences researchers. At the same time, Anthropic’s decision to keep Mythos restricted highlights the growing challenge facing frontier AI companies: **the same capabilities that could accelerate scientific discovery can also create new security risks.**

For now, most users get Fable 5.1.

But Anthropic’s real message is that **AI is moving from answering questions to doing the work.**

For continuing updates on OpenAI, ChatGPT, Codex, GPT models, and AI industry developments, bookmark:
