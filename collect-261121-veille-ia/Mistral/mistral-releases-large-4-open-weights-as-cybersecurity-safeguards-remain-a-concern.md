---
id: collect-261121-veille-ia/mistral/mistral-releases-large-4-open-weights-as-cybersecurity-safeguards-remain-a-concern
title: "Mistral Releases Large 4 Open Weights as Cybersecurity Safeguards Remain a Concern"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["mistral", "large", "model", "safeguards", "weights", "open", "cyber", "preview"]
source: https://info.dilmipaint.com/article/433
source_anchor: ""
source_lines: [1, 45]
section: "Mistral"
---
Mistral Large 4 is downloadable and modifiable, but its safeguards won’t update on existing copies and its cyber benchmarks need more scrutiny.

Mistral AI has put the weights for its trillion-parameter Large 4 model beyond the reach of its own ongoing safeguards. That gives organizations more control over deployment, while leaving them responsible for securing every downloaded copy.

The Paris-based company released the weights on Thursday, weeks after opening the multimodal model for public preview. Mistral, which calls the model “Le Chonk,” said it monitored cybersecurity-related activity during that preview. It saw the model refuse black-box attempts against live targets, but warned that its capabilities could still help users carry out real attacks.

“Though we observed the model refusing to comply with black-box attacks against live targets, we see a potential for uplift to conduct real attacks,” the company wrote in its announcement.

## Downloaded safeguards won’t keep pace

Large 4 includes safeguards in its weights, Mistral vice president of science Pierre Stock told CSO. But new protections will apply to Mistral’s managed API and later model releases, not automatically to checkpoints already downloaded.

That creates a practical divide: Mistral can update the safeguards around its hosted service, while teams running local copies must manage changes themselves. Mistral says it will continue strengthening its protections in the coming months.

Stock didn’t specify which stages of an attack the model had helped with during preview traffic. He pointed instead to defensive applications, including finding, reproducing and fixing software vulnerabilities. Mistral says Large 4 can distinguish those tasks from malicious requests.

On the cyber-related portions of JailbreakBench, StrongREJECT and AgentHarm, Mistral reported that Large 4 refused 96.6% of malicious prompts, the highest rate it had measured among open models. Gartner principal analyst Niyati Daftary cautioned that the figure describes performance in Mistral’s test conditions, not behavior against a company’s own data, adversarial prompts or production threats.

Open weights also shift control of use and deployment to whoever downloads them, Hugging Face said in its launch post. That concern sits alongside a July assessment by the UK’s AI Security Institute, which found that leading open-weight models lagged closed frontier models on cyber tasks by four to seven months. For highly capable models, the institute warned, open release carries a “persistent and irreversible risk of misuse.”

Open-source AI is already appearing in attack activity. Earlier this month, Microsoft said a China-aligned group used the agentic AI framework ARTEX to target more than 2,250 Microsoft 365 tenants.

## Cyber claims need more evidence

Mistral reported a 51% score for Large 4 on the Artificial Analysis Cyber Index, which tests a model’s ability to find, reproduce and patch vulnerabilities. The chart in the company’s announcement shows DeepSeek V4.1 Flash and MiMo-V2.6-Pro scoring higher.

The company also describes Large 4 as the leading open-weight model for coding, pointing to results on Cognition’s FrontierCode 2.0 and Terminal-Bench 4.0. It hasn’t published the technical report behind those claims. Forrester vice president and principal analyst Charlie Dai said that makes it difficult to judge performance across different workloads. He added that model rankings can shift within weeks or months.

Stock said the forthcoming report will cover Mistral’s cyber evaluation methods, refusal tests and preview monitoring. The company also plans to share relevant adversarial testing results while protecting sensitive security details, though it hasn’t set a publication date.

Mistral is restricting access to a separate cybersecurity variant designed for penetration testing, vulnerability assessment and red-teaming. Design partners, including Orange Cyberdefense, can preview it under vetting and contractual safeguards, with testing limited to systems they own or have permission to assess.

## Enterprise control comes with operating duties

Large 4 is released under a modified MIT license that permits use, modification, fine-tuning and commercialization. A separate license is required for businesses offering it as a model-as-a-service product. Mistral also sells API access for $0.70 per million input tokens and $2.10 per million output tokens.

For European organizations, local deployment can support data and operational control. Dai said that benefit shouldn’t outweigh security concerns: self-hosting transfers responsibility for safeguards, access controls, monitoring and misuse prevention to the operator. Mistral’s announcement doesn’t say whether Large 4 meets the EU AI Act’s systemic-risk threshold for models trained with more than 10<sup>25</sup> floating-point operations, or describe a risk assessment ahead of release.

Stock said Mistral has signed the EU’s General-Purpose AI Code of Practice, including its safety and security chapter, which the European Commission accepts as one route to demonstrating compliance. He didn’t say whether the company has classified Large 4 as a systemic-risk model. The EU cybersecurity agency ENISA said it is testing the model to find vulnerabilities, audit code and analyze defensive logs.

Daftary’s advice for companies using self-hosted Large 4 for code audits or vulnerability triage is to keep it inside a controlled workflow, rather than giving it autonomy. That means isolating the environment, limiting access to code and credentials, sandboxing generated code, and requiring human approval before fixes are applied. The operating model matters as much as the weights: self-hosting offers more control, but leaves the enterprise responsible for keeping the system secure.

## Discussion

Sign in to join the discussion.
