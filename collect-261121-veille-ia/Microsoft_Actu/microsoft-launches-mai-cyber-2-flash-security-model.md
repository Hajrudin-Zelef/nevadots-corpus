---
id: collect-261121-veille-ia/microsoft-actu/microsoft-launches-mai-cyber-2-flash-security-model
title: "Microsoft launches MAI Cyber 2 Flash security model"
domain: microsoft-actu
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["microsoft", "cyber", "flash", "security", "model", "models", "cost", "mdash"]
source: https://testingcatalog.com/microsoft-launches-mai-cyber-2-flash-security-model/
source_anchor: ""
source_lines: [1, 17]
section: "Microsoft_Actu"
---
Microsoft has unveiled MAI Cyber 2 Flash at Ignite, a specialist security model from Microsoft AI that the company says delivers leading performance on real-world security tasks at a fraction of the cost of larger models. The model is aimed at security teams that need AI to find vulnerabilities, recommend fixes and investigate threats around the clock, a workload where compute bills can quickly add up.

On CyberGym-E2E, a benchmark that measures real-world vulnerability discovery, MAI Cyber 2 Flash scores 92.3% against 91.5% for GPT-6 Astra, using half the reasoning budget and less than 2% of the cost per task. Microsoft calculated that cost at list prices of $0.10 per million input tokens, $0.01 for cached input, and $0.50 for output, compared with $10, $1, and $50 for GPT-6 Astra on Azure OpenAI Global, and notes the figure reflects offline evaluation cost rather than customer spend.

At the same reasoning budget, the model also beats GPT-5.6 Cyber on CyberGym-E2E and WebExploitBench, reaching 40.4% against 38.4%. Microsoft says it found that 92 of the benchmark's 905 tasks were based on the same open-source projects and vulnerability reports as some of its training data, so it excluded them for every model and reports results on the remaining 813. A comparison with GPT-6 Cyber, which is available only to select customers, is planned for the coming weeks.

The model is built on Microsoft's decades of proprietary security data and expertise. It brings a one-million-token context window, four times larger than MAI Cyber 1.1 Flash, so it can analyze more of a codebase during complex, multi-step investigations. Early evaluations through Microsoft Perception show performance comparable to leading models on investigation agent work and security detection authoring. Insight Enterprises reported that MAI Cyber models were 20% more token-efficient in its evaluations than GPT-5.4, GPT-5.3 Codex and GPT-5.4 Mini.

MAI Cyber 2 Flash is coming exclusively to Microsoft security products, including Codename MDASH, where it will automatically replace the existing model in the MAI profile for eligible customers starting in mid-December.

For Microsoft AI, MAI Cyber 2 Flash is the latest in its line of specialist security models, following MAI Cyber 1.1 Flash, and it builds that work directly into Microsoft's security products. The company frames the release as a way to bring advanced security capabilities to more workloads without the cost of running larger models at every step, which matters for defenders who want AI working around the clock.

## Sources and related context

- MAI Cyber 2 Flash model page (Microsoft AI): Supports: Microsoft's model page confirms MAI Cyber 2 Flash is built exclusively for Microsoft security and delivered through Codename MDASH, where it runs up to 90% of tasks while larger models handle the hardest cases.
- MAI Cyber 1.1 Flash: Sharper findings and fewer tokens inside MDASH: Related context: The previous model in the line, which Microsoft says already accounts for almost 50% of MDASH scans, explains the MAI profile that Cyber 2 Flash will replace.
- Maintainers don't need more AI bug reports. They need fixes. (GitHub Blog): Related context: GitHub and Microsoft describe a Codename MDASH pilot that used an MAI Cyber model to turn thousands of raw findings into 99 checked, private reports for six open source projects.
