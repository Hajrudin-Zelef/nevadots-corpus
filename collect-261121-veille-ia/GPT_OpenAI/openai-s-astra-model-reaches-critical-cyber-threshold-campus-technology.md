---
id: collect-261121-veille-ia/gpt-openai/openai-s-astra-model-reaches-critical-cyber-threshold-campus-technology
title: "OpenAI's Astra Model Reaches Critical Cyber Threshold -- Campus Technology"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: ["2026-09-10"]
keywords: ["astra", "model", "openai", "critical", "threshold", "monitoring", "reaches", "cyber"]
source: https://campustechnology.com/articles/2026/09/10/openais-astra-model-reaches-critical-cyber-threshold.aspx
source_anchor: ""
source_lines: [1, 31]
section: "GPT_OpenAI"
---
### OpenAI's Astra Model Reaches Critical Cyber Threshold

        
        
        - By John K. Waters
- 09/10/26

OpenAI has unveiled GPT-6 Astra, its most capable broadly deployed model and the first system the company says has reached the "Critical" cybersecurity capability threshold under its Preparedness Framework. This marks a significant escalation in both what its AI models can do and the safeguards required to release them.

    
The designation means that, given the appropriate tools and access, Astra can identify previously unknown security vulnerabilities and develop ways to exploit them across multiple well-protected systems without a human directing each step, according to OpenAI.

That capability has prompted the company to introduce stronger restrictions on Astra, including stricter isolation, checkpoint encryption, monitoring of tool-use sessions, and additional controls to detect potentially harmful or unauthorized model behavior.

But the release also exposes an increasingly difficult problem for frontier AI developers: As models become more capable of acting autonomously, some of the techniques used to understand and control their behavior may become less reliable.

OpenAI said Astra is better aligned and substantially more resistant to jailbreaks than its GPT-5.6 Sol predecessor. At the same time, the company found that Astra has become better at controlling what appears in its own chain of thought, making some potentially problematic behavior harder for monitoring systems to detect.

"Chain of thought," commonly abbreviated CoT, refers to the intermediate reasoning a model produces as it works through a problem. Researchers can use signals from that reasoning as an additional way to determine whether a model may be planning an unauthorized or potentially harmful action. OpenAI describes CoT as potentially providing a richer monitoring signal than looking only at a model's actions and final outputs.

That makes Astra's performance in adversarial testing notable.

When researchers deliberately instructed the model to evade monitoring, Astra could remain undetected while strategically underperforming on evaluations, a behavior known as "sandbagging," and could sometimes evade internal monitors while carrying out simulated sabotage tasks.

"These findings indicate that the Astra class models could evade our CoT monitors under adversarial conditions," OpenAI said in its safety overview.

The company emphasized that the findings came largely from adversarial evaluations in which the model was pushed to evade monitoring. OpenAI said it has not found evidence that Astra uses steganographic chain-of-thought reasoning, in which hidden reasoning is concealed within seemingly unrelated text, and its broader evaluations found Astra less likely than GPT-5.6 Sol to violate safety and security restrictions.

OpenAI nevertheless said the results underscore the need for alignment-auditing techniques that do not depend solely on examining a model's chain of thought.

### From Vulnerability Detection to Exploitation
