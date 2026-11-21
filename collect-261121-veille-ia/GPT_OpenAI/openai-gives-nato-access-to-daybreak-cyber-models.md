---
id: collect-261121-veille-ia/gpt-openai/openai-gives-nato-access-to-daybreak-cyber-models
title: "OpenAI Gives NATO Access to Daybreak Cyber Models"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["openai", "access", "natos", "models", "nato", "daybreak", "tokens", "announcement"]
source: https://www.zeniteq.com/news/openai-gives-nato-access-to-daybreak-cyber-models-yyu30x
source_anchor: ""
source_lines: [1, 79]
section: "GPT_OpenAI"
---
OpenAI said on November 17, 2026, that it is working with NATO’s Communications and Information Agency (NCIA) to give NATO’s core institutions access to advanced cybersecurity models through Daybreak. NATO has used **up to 300 billion raw input and output tokens per month**, according to the company’s announcement.

The announcement describes access for defenders reviewing software, investigating suspicious activity, validating vulnerabilities, and developing and testing fixes. OpenAI also supported a new training stream at NATO’s 2026 Crossed Swords exercise, where participants built, deployed and evaluated AI agents for cyber operations.

The distinction matters: this is access to powerful models through a program for authorized security work. The announcement describes neither an unrestricted release of model weights nor a public distribution of offensive tools. Its significance lies in putting frontier cybersecurity capabilities into institutional workflows, including the human work required to check findings and turn them into fixes.

## What NATO’s Daybreak Access Covers

OpenAI identifies **NATO’s core institutions** as the recipients of access through NCIA. That wording is narrower than a blanket entitlement for every government agency, military unit or contractor across all NATO member states.

Defense One reported that OpenAI announced the expansion at NATO Edge in Izmir, Turkey. The publication describes Daybreak as a program launched in May that gives accredited institutions and the US government access to advanced models for finding software vulnerabilities.

OpenAI’s announcement outlines four connected defensive tasks:

- **Software review:** examining code for possible security weaknesses.
- **Suspicious-activity investigation:** helping defenders analyze activity that may indicate a security problem.
- **Vulnerability validation:** checking whether a suspected weakness is genuine.
- **Fix development and testing:** helping create changes and assess whether they address the problem.

These are Daybreak’s stated use cases, not a published breakdown of NATO’s workload. OpenAI has not disclosed specific model versions, deployment architecture, contract value or a complete list of NATO users.

That missing detail limits comparisons with other products. “Advanced cybersecurity models” describes a capability category, not a reproducible configuration that another organization could purchase and benchmark on identical terms.

My view is that the most consequential feature is the connection between discovery and repair. Finding another suspicious code pattern has limited value if analysts cannot reproduce the issue, prioritize it and verify a fix. Daybreak’s stated scope reaches across that chain, although the announcement provides no NATO-specific measurements of how quickly those steps happen.

## What 300 Billion Tokens Actually Measures

The reported token figure is the clearest indication of workload scale, but it needs careful reading.

Tokens are the units a model processes and generates, representing pieces of text or code. Input tokens cover material supplied to the model; output tokens cover what it produces. OpenAI reports their combined raw volume as **up to 300 billion per month**.

“Up to” matters. It gives an upper reported monthly figure, not a stated average or a guaranteed level of ongoing use. Raw token volume can also include repeated instructions, code and context across a workflow. It is not a count of unique software examined, completed investigations or confirmed vulnerabilities.

A workflow that repeatedly analyzes a large codebase could consume substantial tokens while producing few actionable findings. A more targeted investigation might consume less and still uncover a serious flaw. Those are examples of why usage and effectiveness need separate measures, not claims about NATO’s actual results.

The figure also cannot support a reliable AI pricing estimate. Without the model mix, input/output split and applicable commercial terms, multiplying 300 billion by a public token price would create false precision.

NCIA General Manager Dr Dylan Browne explicitly connects usage analysis to procurement. In OpenAI’s announcement, he says the agency wants to:

“optimise costs, evaluate competing solutions and make informed procurement decisions.”


That is a useful way to frame the number: evidence of substantial activity, with its economics still something the organization intends to understand more deeply.

## Crossed Swords Trains People to Work With Agents

The 2026 Crossed Swords exercise included a new training stream in which participants built, deployed and evaluated AI agents for cyber operations using Daybreak. OpenAI says the exercise, organized by the NATO Cooperative Cyber Defence Centre of Excellence, placed participants alongside agents as operators and emphasized human oversight.

That goes beyond teaching participants how to write a prompt. Building and evaluating an agent means considering what task it should pursue, how it behaves during that task and whether its results are useful enough to act on.

The operational implication is that defenders need skills in supervising automated work as well as performing the underlying security analysis. An agent can generate a plausible finding without proving that a vulnerability exists. Operators still need a basis for accepting a result and deciding what happens next. The published account does not specify the exercise’s approval controls or agents’ tool permissions.

Defense One reports that the exercise’s technical environment included both on-premises AI and, for the first time, OpenAI Daybreak. That describes a mixed environment; the hosting location of OpenAI’s models remains unspecified.

I would treat the training component as an important part of this expansion. Giving a team model access is one step. Teaching operators to evaluate agent behavior addresses a different problem: how to use automation without allowing convincing output to substitute for evidence.

## Governed Access Is Different From Open Weights

Daybreak is presented as a route for trusted defenders to perform authorized security work. Model access lets an institution use a capability under the program’s arrangements. An open-weight release would instead make the model’s underlying learned parameters available for others to obtain and run, subject to whatever license accompanied them.

The NATO announcement concerns the former. It supplies no basis for claiming that NATO received unrestricted model weights or a freely distributable offensive toolkit.

That distinction should not obscure the dual-use nature of cybersecurity capabilities. Analyzing code and validating weaknesses can support both defense and attack. Authorized scope, operator oversight and deployment controls therefore matter alongside model capability.

The published evidence supports the existence of a governed access program and an emphasis on human oversight. It does not expose enough of the implementation to assess every safeguard.

## Allied Examples Connect Discoveries to Fixes

OpenAI also describes work outside NATO, including access for ENISA, France’s ANSSI, and national cyber agencies in Germany and Poland. Its announcement offers several more concrete examples:

## Frequently Asked Questions

3 questions

## 1
### Who gets access to Daybreak through NATO?


OpenAI says it works with NCIA to provide access to NATO’s core institutions. It has not published a complete list of users or promised access to every organization in member countries.
