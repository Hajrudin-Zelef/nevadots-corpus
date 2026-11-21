---
id: collect-261121-veille-ia/claude-anthropic/anthropics-claude-fable-5-1-helps-build-a-3d-aircraft-engine
title: "Anthropic’s Claude Fable 5.1 Helps Build a 3D Aircraft Engine"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-01"]
keywords: ["fable", "claude", "anthropic", "anthropics", "mythos", "tasks", "said", "safety"]
source: https://hodl.press/anthropics-claude-fable-5-1-helps-build-a-3d-aircraft-engine/
source_anchor: ""
source_lines: [1, 43]
section: "Claude_Anthropic"
---
- Anthropic released Claude Fable 5.1 and Claude Mythos 5.1 on Sept. 1, 2026, offering stronger performance on coding and long-running knowledge tasks while reducing typical workload costs.
- The company kept token prices unchanged but cut cache-read pricing by 75%, which it said lowers total costs by about 25% for typical workloads and by up to 45% for context- and tool-heavy tasks.
- Fable 5.1 is available through all paid Claude plans, Anthropic’s API and cloud partners, while Mythos 5.1 is restricted to vetted cybersecurity and life-sciences organizations.

Anthropic described Claude Fable 5.1 and Claude Mythos 5.1 as the same base model with different safety restrictions. Fable 5.1 is the public version with additional classifiers, while Mythos 5.1 has looser restrictions for approved specialists.

According to Anthropic’s announcement, the update addresses three major customer complaints about Fable 5: pricing, data retention and safety mechanisms that blocked legitimate requests. Payments service Ramp estimated that Fable 5 represented about 11% of Anthropic customers’ spending more than two months after its launch as businesses selected cheaper models.

## Performance and benchmarks

Fable 5.1’s largest gains concern complex, long-running work, including multi-file programming, large refactors, research, document creation, analysis of dense charts and tables, computer use and reasoning across its 1 million-token context window. Its maximum output is 128,000 tokens.

On Terminal-Bench-Science 0.1, a benchmark for agentic scientific tasks, Fable 5.1 scored 52.6%, compared with 24.7% for Fable 5. Its Terminal-Bench 4.0 result rose to 55.8% from 42.0%, while Mythos 5.1 scored 60.9% on the same test. Anthropic attributed that gap to cyber classifiers blocking some tasks for Fable 5.1.

Fable 5.1 scored 73.4% on CursorBench 3.2.0, up from 70.5% for Fable 5. It recorded 65.0% with tools and 60.9% without tools on Humanity’s Last Exam. On OSWorld 2.0, it scored 77.9% under lenient scoring and 41.7% under strict scoring.

Anthropic said the tests used production safety defenses, meaning the model received zero points when classifiers blocked a task. Early-access partner Browserbase said Fable 5.1 completed 82% of tasks on its most difficult browser benchmark, compared with 74% for Opus 5 and 57% for Fable 5. Financial service Rogo reported accuracy matching Fable 5 while using 20% fewer tokens.

## Pricing, effort levels and access

Fable 5.1 costs $10 per million input tokens and $50 per million output tokens, unchanged from Fable 5. Cache reads now cost $0.25 per million tokens, a 75% reduction. Anthropic’s Batch API continues to offer a 50% discount.

The model provides five effort settings: Low, Medium, High, Extra high, or xhigh, and Max. Higher settings allow more extensive processing but increase latency and cost. Anthropic said Low and Medium can match or exceed Fable 5’s results at lower cost. Thinking remains enabled at every level and cannot be turned off.

Fable 5.1 is available through the Claude API as `claude-fable-5-1`, Amazon Bedrock, Google Cloud Vertex AI and Microsoft Foundry. It is also offered through paid Pro, Max, Team and Enterprise plans, including Claude Code version 2.1.250 and later. The free plan does not include Fable models, and version 5.1 has no promotional access period.

## Safety and enterprise privacy

Anthropic said it reduced false positives from Fable 5.1’s cybersecurity classifiers by about 60%. The model may now search code for vulnerabilities, although exploit development, penetration testing and binary vulnerability analysis are still routed to Opus models. Safety mechanisms for harmless basic biology and medical questions trigger 85% less often, while advanced life-sciences research continues to be routed to Opus.

The company also announced Enterprise Frontier Safeguards, an optional system intended to provide zero data retention-level confidentiality while retaining automated abuse protections. Customer data remains in cloud infrastructure controlled by the customer and protected by the customer’s encryption keys. The customer handles incident reviews by default.

Anthropic developed the system with more than 100 companies and cloud partners AWS, Google Cloud and Microsoft Azure. Its phased rollout is scheduled to begin in the fall, and eligible customers may use Fable 5.1 with zero data retention until the system is ready.

## Practical test and limitations

Incrypted tested Fable 5.1 by asking it to create a single-file HTML artifact showing an interactive 3D cutaway of a turbofan engine. The generated model included mouse-controlled rotation, a throttle slider, labeled engine sections, changing exhaust glow and an information panel. A follow-up request added animated airflow paths and a toggle between cutaway and airflow views without breaking the existing scene.

Safety classifiers remain a limitation for work involving virology, toxicology, drug or molecule design and offensive cybersecurity. The checks cover user messages as well as memory, connector data, web results and uploaded files. Full Mythos 5.1 capabilities remain limited to vetted participants in Anthropic’s cybersecurity and life-sciences verification programs.

Anthropic also embeds an invisible statistical marker in text generated by models released after Aug. 2, 2026, under the European Union’s Code of Practice on AI content transparency. The company said the marker contains no user data and does not affect response quality.

Source: Incrypted
