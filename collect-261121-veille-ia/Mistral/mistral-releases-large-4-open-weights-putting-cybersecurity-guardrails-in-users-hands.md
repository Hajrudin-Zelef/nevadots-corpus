---
id: collect-261121-veille-ia/mistral/mistral-releases-large-4-open-weights-putting-cybersecurity-guardrails-in-users-hands
title: "Mistral Releases Large 4 Open Weights, Putting Cybersecurity Guardrails in Users’ Hands"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "model", "open", "large", "weights", "models", "users", "releases"]
source: https://fisn.cibadak.biz.id/article/354
source_anchor: ""
source_lines: [1, 45]
section: "Mistral"
---
Mistral’s open Large 4 offers flexible deployment, but downloaded copies won’t receive future safeguards. Enterprises must manage access, monitoring and misuse.

Mistral AI has released the open weights for its trillion-parameter Large 4 model, giving users the ability to download, modify and run it beyond the company’s control. That choice carries a clear tradeoff: Mistral says the model refused attacks against live targets during public preview, yet its capabilities could still help people conduct real attacks.

The Paris-based company, which calls the model “Le Chonk,” opened a public preview weeks before Thursday’s release. It monitored cybersecurity-related use during that period and says it’s now adjusting the model’s safeguards. Those changes will reach its managed API and future releases, but not copies already downloaded.

## Downloaded copies put operators in charge

“Subsequent safeguard updates are applied to our managed API and future model releases, rather than automatically updating downloaded checkpoints,” Pierre Stock, Mistral’s vice president of science, told CSO.

Stock didn’t identify which attack stages the model had helped with in preview traffic. He said its clearest gains were in defensive tasks: finding, reproducing and fixing software vulnerabilities. Mistral says safeguards built into the released weights help the model distinguish defensive work from malicious requests, while acknowledging the potential for misuse as capabilities advance.

The company reports that Large 4 refused 96.6% of malicious prompts in the cyber-related portions of the JailbreakBench, StrongREJECT and AgentHarm benchmarks. That’s the highest rate Mistral has measured among open models. Niyati Daftary, principal analyst at Gartner, cautioned that the result describes behavior in the company’s test setup, not how the model will respond to an enterprise’s own data, adversarial scenarios or production systems.

Hugging Face, which hosts the weights, makes the operational consequence explicit in its launch post: with open weights, users control both deployment and adherence to the usage policy. Mistral is also offering a specialized cybersecurity version to design partners, including Orange Cyberdefense. The version is optimized for penetration testing, vulnerability assessment and red-teaming; access is subject to vetting and contractual safeguards, and testing is limited to systems partners own or are authorized to assess.

## Cyber capability claims still need evidence

Mistral’s model scored 51% on the Artificial Analysis Cyber Index, which evaluates how well models find, reproduce and patch vulnerabilities. The company’s chart shows DeepSeek V4.1 Flash and MiMo-V2.6-Pro scoring higher.

Mistral also describes Large 4 as the leading open-weight coding model, citing results on Cognition’s FrontierCode 2.0 and Terminal-Bench 4.0. It hasn’t yet published the technical report behind those claims. Charlie Dai, Forrester’s vice president and principal analyst, said the leadership claim is difficult to validate across workloads without that detail. He also cautioned that frontier model rankings can change within weeks or months.

Stock said the forthcoming report will cover Mistral’s cyber evaluation methods, refusal tests and public-preview monitoring. The company also plans to share relevant adversarial testing findings while protecting sensitive security information, but hasn’t given a publication date.

Other findings add context to the release. The UK’s AI Security Institute reported in July that leading open-weight models trailed closed frontier models on cyber tasks by four to seven months. It described open release of highly cyber-capable models as creating “a persistent and irreversible risk of misuse.” Earlier this month, Microsoft said a China-aligned group used the agentic AI framework ARTEX to target more than 2,250 Microsoft 365 tenants.

## Deployment choices shape enterprise risk

Mistral released Large 4 under a modified MIT license that permits use, modification, fine-tuning and commercialization. A separate license is required to offer the model as a service. Customers can also use Mistral’s API, priced at $0.70 per million input tokens and $2.10 per million output tokens.

For European buyers, local control may be valuable, but Dai said it shouldn’t outweigh security. Self-hosting shifts responsibility for safeguards, access controls, monitoring and misuse prevention to the operator. “Sovereignty and security therefore need to be evaluated together, not traded against each other,” he said.

Daftary recommends treating a self-hosted Large 4 deployment for code audits or vulnerability triage as a controlled component rather than an autonomous agent. Her safeguards include isolated environments, least-privilege access to code and credentials, sandboxing generated code and requiring human approval before remediation. Self-hosting can improve data control and operational continuity, she said, while making the enterprise responsible for patching, monitoring, evaluation and misuse prevention.

## What the EU disclosure leaves open

The European Commission says general-purpose AI models trained with more than 10<sup>25</sup> floating-point operations are presumed to carry systemic risk. Providers in that category must assess and mitigate risks, report serious incidents and maintain cybersecurity protections. Mistral’s announcement doesn’t say whether Large 4 meets that threshold or describe a risk assessment completed before release.

Stock said Mistral is a full signatory to the EU’s General-Purpose AI Code of Practice, including its safety and security chapter, which the Commission accepts as one way to demonstrate compliance. He didn’t say whether Large 4 has been classified as a systemic-risk model. ENISA, the EU’s cybersecurity agency, said it’s testing the model to find vulnerabilities, audit code and analyze defensive logs.

For enterprises, the practical question is where they want control to sit. The API keeps future safeguard updates with Mistral; downloaded weights give operators more deployment control and make them responsible for securing each copy.

## Discussion

Sign in to join the discussion.
