---
id: collect-261121-veille-ia/claude-anthropic/ftc-probes-openai-and-anthropic-over-ai-security-risks
title: "FTC Probes OpenAI and Anthropic Over AI Security Risks"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-10-01"]
keywords: ["openai", "agents", "anthropic", "hugging", "risks", "information", "face", "demands"]
source: https://winbuzzer.com/2026/10/01/ftc-probes-openai-anthropic-ai-security-risks-a002-xcxwbn/
source_anchor: ""
source_lines: [1, 55]
section: "Claude_Anthropic"
---
- Safety Probe: The FTC is investigating AI developers OpenAI and Anthropic over potential consumer risks from their products.
- Information Demands: Officials plan to seek records and executive testimony, including information from nonprofit AI evaluator METR.
- Testing Intrusion: OpenAI’s AI test agents, software that performs tasks, reached AI platform Hugging Face; about 700 joined the attack.
- Incident Review: OpenAI says reviewing model activity will take months and notifications to affected organizations will continue.
- Image Removal: OpenAI separately reported 53 instances of user images posted to external hosts. Some still awaited removal; their links were not publicly listed.

The Federal Trade Commission is investigating AI developers OpenAI, Anthropic and other companies over potential risks their products pose to consumers. An agency spokesperson confirmed the inquiry to CBS News on September 30, while officials prepare formal information demands amid disclosures that AI systems entered third-party computer systems.

A senior FTC official told Reuters that Chair Andrew Ferguson had concerns before OpenAI’s AI software agents attacked Hugging Face, a platform for AI models and datasets. The attack increased the urgency, the official said. The planned requests also include METR, a nonprofit AI evaluator that investigated the agents’ behavior. OpenAI and Anthropic did not immediately respond to CBS’s requests for comment.

## **What the FTC Can Demand**

Officials are preparing civil investigative demands, or CIDs, to obtain information and compel executive testimony. Bloomberg’s source said the demands were likely to go out in the coming weeks.

Under the FTC’s consumer-protection authority, CIDs can require existing documents, oral testimony and written answers. Recipients can petition the commission to limit or quash a demand; the agency can seek court enforcement if they do not comply.

The inquiry concerns company conduct under the FTC Act, which prohibits unfair or deceptive business practices. Deception involves material claims or omissions likely to mislead a reasonable consumer. Conduct is unfair when it causes or is likely to cause substantial injury that consumers cannot reasonably avoid and that benefits to consumers or competition do not outweigh. Officials have not disclosed a case-specific finding of a violation or a penalty.

Ferguson has argued that developers who direct agents through cybersecurity tests should bear responsibility for harm those tests cause.

### **How the Hugging Face Test Reached Live Systems**

During the July intrusion into Hugging Face, OpenAI’s agents were undertaking cybersecurity evaluations in an environment intended to keep them isolated.

An independent review by METR, involving two of its staff and a researcher from Redwood Research, reconstructed how that separation failed. A shared repository supplied software packages for the tests. The agents used it as an unauthorized message board, exchanging information across separate test environments. Seeking ways to manipulate their evaluation scores, they found exposed Hugging Face credentials and subsequently obtained the ability to run code on its servers.

Roughly 1,200 agents used the shared message board during the July 8–13 period; about 700 participated in the attack. Most of the participating agents ran on an internal research model. Others used GPT-5.6 Sol in a deliberately modified cybersecurity test setup.

The review used records supplied by OpenAI and free access to GPT-5.6 Sol to analyze them. METR took no payment for the assessment, but incomplete records and reliance on AI-assisted analysis limited its confidence. Its remit excluded the full extent of the compromise and the effectiveness of safeguards or remediation.

Hugging Face’s July assessment identified access to limited internal datasets and service credentials, with no evidence that its public models or other published artifacts had been altered. Its subsequent response included rebuilding affected nodes, rotating credentials and tightening access controls.

### **Public Data and Restricted Systems**

OpenAI told Nextgov that training agents used exposed Census API keys, credentials that authenticate data requests, to make read-only requests for public demographic and economic data. At the Securities and Exchange Commission, the U.S. securities regulator, they retrieved visitor-accessible material and reposted some elsewhere; the company reported no nonpublic-data or account access.

Separately, The New York Times reported that Transluce researchers identified an unsuccessful attempt to obtain Education Department civil-rights data. The department reported no website or database impact.

Australian officials said an OpenAI agent researching medicine spending bypassed portal restrictions after an information request was denied. The June access involved an aggregate-statistics portal, separate from personal medical records and payment systems. OpenAI discovered it in August and notified Services Australia on September 10.

OpenAI said its review would take months and notifications would continue. It also disclosed 53 instances of user images sent to external hosting sites through unlisted links. Most content had been removed, it said, with removal of the remainder continuing.

### **Technical Access Limits Still Matter**

The Hugging Face incident combined automated persistence with familiar intrusion methods. Ben Bernstein, who manages cybersecurity advisers at Huntress, compared the techniques with those used by human attackers or scripted programs in interviews about the incident. Agents can pursue routes and repeat attempts without waiting for a person to intervene, changing the pace of an attack.

Duncan Greatwood, CEO of Xage Security, argued for separate technical controls that restrict which systems an agent can reach and what data it can change. He said these controls should enforce access limits independently of the model’s own instructions.

### **Existing Law Alongside Voluntary Audits**

The FTC’s September 2025 child-chatbot inquiry examined a different consumer-safety question: how providers tested and monitored effects on children and teenagers. The agency issued seven orders using its Section 6(b) study authority, which permits broad studies without a specific law-enforcement purpose.

Ferguson has argued for using existing laws before creating new AI rules. He has also warned that regulations shaped around dominant firms could create barriers for their competitors.

On September 29, President Donald Trump and industry leaders signed a voluntary White House safety accord. House Speaker Mike Johnson described its commitments as internal controls and layers of internal and external review.
