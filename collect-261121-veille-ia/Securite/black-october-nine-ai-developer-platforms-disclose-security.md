---
id: collect-261121-veille-ia/securite/black-october-nine-ai-developer-platforms-disclose-security
title: "Black October: Nine AI Developer Platforms Disclose Security..."
domain: securite
role: reference
task: article
actors: []
dates: ["2026-10-30"]
keywords: ["october", "platforms", "keys", "black", "nine", "developer", "calls", "security"]
source: https://www.mlllm.io/articles/3663-black-october-nine-ai-developer-platforms-disclose-security-incidents-in-one-month/
source_anchor: ""
source_lines: [1, 31]
section: "Securite"
---
Wiz published the 'Black October' report on October 30, 2026: within one month, at least nine AI developer platforms, ranging from Fireworks AI to Northflank, disclosed security incidents. According to the company's assessment, this is not about background scanning but targeted operations: attackers searched environment variables for stored AI service API keys to steal and resell access to LLMs, a scheme Wiz calls LLMJacking. In documented cases, stolen corporate Amazon Bedrock keys began working within 30 minutes, indicating that this is not a one-off issue for individual platforms but a shift in the risk profile for the entire AI development engineering infrastructure.

### What happened

The 'Black October' report was released on October 30, 2026, and consolidated nine disclosures spread throughout the month: Fireworks AI on October 6, Andromeda on the 9th, Zeabur on the 14th, Lightning AI on the 15th, OpenCode on October 16â17, Railway on the 20th, Warp on the 23rd, CommandCode on the 28th, and Northflank on the 30th. According to Wiz's reconstruction, attackers acted specifically: they did not scan services sequentially but deliberately sought stored environment variables where teams keep AI service API keys. In two documented episodes, stolen corporate Amazon Bedrock keys began working almost immediately after the leak: the first saw approximately 800,000 calls over 30 hours, the second about 1.7 million calls over 13 hours, and the bill at list prices, according to Wiz's estimate, exceeded $100,000. The same report documents the theft of approximately $371,000 in cryptocurrency via keys stored on Railway.

### Context

To understand why developer platforms were specifically targeted, it is important to see the mechanics Wiz calls LLMJacking: LLM keys are stolen not for the sake of the leak itself, but to resell access to buyers, so a stolen key becomes a commodity almost instantly. On such platformsâhosting, CI/CD, agent launch environmentsâenvironment variables remain the standard way to deliver secrets to an application, and they accumulate keys from many teams: to LLM providers, payment, and adjacent services. One successful breach of such a platform opens up a warehouse of others' credentials entirely to the attacker, not one by one. Hence the conclusion suggested by the described scenarios: periodic secret audits cannot keep up with the speed of monetization, so detection must rely on continuous monitoring of anomalous token consumption and pre-defined scope restrictionsâotherwise the bill accumulates before a team member can notice something is wrong.

### Why this matters for the industry

For the industry, 'Black October' is both a danger signal and a demand map. Secret storage is becoming a product feature of platforms: customers will ask whether there is a vault-like storage or short-lived credentials, whether key scope is limited by default, and whether built-in quotas work. Online detection of inference consumption anomalies is becoming a mandatory layer of the stack: external calls grow faster than any manual reaction, so without automatic budget limits and blocking, the damage can quickly exceed the planned budget. On this wave, a new segment of tools is formingâkey inventory and rotation, per-key limits, spend-guard, and kill switch on spendâand the LLM credential management layer may eventually stand in the same row as CI/CD and SSO as a standard part of infrastructure. A separate effect is the normalization of public disclosure: nine disclosures in one month set a precedent after which silence about such an incident looks like a reputational anomaly, and teams will start including questions about secret storage and limits in the standard vendor checklist before choosing a platform.

### Why this matters for users

Readers should check whether their API keysâfor OpenAI, Anthropic, Amazon Bedrock, Stripe, or any other serviceâwere stored in environment variables on Railway, Lightning AI, Fireworks, Warp, Zeabur, OpenCode, Andromeda, CommandCode, or Northflank. If so, the action plan is as follows: inventory keys and rotate everything that touched these platforms; narrow the scope so that the key can do exactly what it really needs; move to short-lived credentials where possible; set hourly and daily quotas on the provider side and enable budget alerts for each key. In parallel, it is useful to cross-check logs for signs of attack from the Wiz report: a sharp increase in the number of API calls, requests from residential IPs and proxy pools, and the use of global.* inference profiles. Even if the logs are clean, rotation and permission restriction remain a reasonable precaution, because dealing with external charges on your own account is cheaper in advance than after the fact.

### What is currently unknown / limitations

The entire picture traces back to a single primary sourceâthe Wiz report, so its interpretations, such as the targeted nature of the attacks and the economics of LLMJacking, are not yet supported by independent verification. The sample is biased toward disclosures: it only includes platforms that themselves reported incidents, meaning the nine episodes are the lower, showcase boundary of the problem, not its scale. Wiz mentions attempts to use stolen traffic for distillationâlearning model behavior on external callsâbut there is no data on the success of these attempts, so the model extraction scenario remains an unconfirmed hypothesis. The damage estimate in two key episodes was made at list prices and may differ from actual bills, and the full list of affected customers and the number of compromised keys are not disclosed in the report.

### Sources

- Official Wiz blog: 'Black October' for AI Developer Platforms
- Hacker News: Black October â At least nine AI developer platforms breached this month
- ToolNavs: Wiz 'Black October' Report â Nine AI Developer Platforms Disclosed Incidents in One Month

### Author

Look at AI, editorial team
