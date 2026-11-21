---
id: collect-261121-veille-ia/claude-anthropic/anthropic-launches-claude-fable-5-1-and-mythos-5-1-with-a-bigger-push-into-coding-and-science
title: "Anthropic launches Claude Fable 5.1 and Mythos 5.1 with a bigger push into coding and science"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["fable", "anthropic", "mythos", "claude", "models", "coding", "safeguards", "also"]
source: https://durovscode.com/anthropic-claude-fable-5-1-mythos-5-1-launch
source_anchor: ""
source_lines: [1, 19]
section: "Claude_Anthropic"
---
Anthropic has introduced Claude Fable 5.1 and Claude Mythos 5.1, which it describes as its most advanced models yet for coding and knowledge work, with new research capabilities aimed at scientific applications. The release also addresses earlier criticism around pricing, data retention and overly restrictive safeguards.

Claude Fable 5.1 and Mythos 5.1 are built on the same underlying model but come with different levels of safeguards. Fable 5.1 is generally available, while Mythos 5.1 is limited to Anthropic’s trusted-access programs, with protections specifically designed for cybersecurity and life sciences.

Alongside the performance improvements, Anthropic has made several changes in response to customer feedback on Fable 5, particularly around cost, privacy and legitimate requests being blocked.

Fable 5.1 is expected to cost around 25% less than Fable 5 for typical token-based workloads, largely because Anthropic has cut the price of cache reads by 75% to $0.25 per million tokens. For highly agentic workloads, the company estimates savings could reach roughly 45%. Standard pricing remains unchanged at $10 per million input tokens and $50 per million output tokens.

Anthropic is also introducing Enterprise Frontier Safeguards (EFS), a system designed to provide enterprise customers with privacy equivalent to a zero-data-retention policy while maintaining protections against misuse. Customer data stays in cloud infrastructure controlled by the customer rather than Anthropic, and EFS will become available in phases beginning later this fall. Until then, eligible customers will be able to use Fable 5.1 with zero data retention.

Anthropic has also adjusted the models’ safeguards to cut down on legitimate requests being blocked. The company says the new filters produce 60% fewer false positives in cybersecurity and 85% fewer in biology, particularly for basic medical and biological questions that previously risked triggering restrictions.

Fable 5.1 can now help identify software vulnerabilities, though it still will not generate exploits for them, opening the model to a broader range of defensive security work. Some dual-use tasks, including penetration testing and exploit generation, will continue to be routed to more tightly controlled Opus models.

Anthropic is positioning scientific research as one of the more significant areas for the new models. In one test, Mythos 5.1 designed high-affinity protein binders for three targets, producing results that the company says were 10 times better than the best entries in specialized competitions.

In another project, Fable 5.1 trained a neural network and generated a new elevation map covering roughly a third of Venus from radar images captured by NASA’s Magellan mission three decades ago. Anthropic has released the map under an open license. Both examples come from the company’s internal and partner testing.

Claude Fable 5.1 is available starting today across major platforms, including Amazon Web Services, Google Cloud and Microsoft Azure, with API access under `claude-fable-5-1`. Mythos 5.1 remains restricted to a limited group of vetted organizations in the US.
