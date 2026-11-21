---
id: collect-261121-veille-ia/grok-xai/spacexsi-launches-grok-voice-3-0-for-ai-customer-support
title: "SpaceXSI Launches Grok Voice 3.0 for AI Customer Support"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["voice", "grok", "customer", "model", "support", "spacexsi", "calls", "also"]
source: https://www.techrepublic.com/article/news-spacexsi-grok-voice-3-ai-customer-support/
source_anchor: ""
source_lines: [1, 43]
section: "Grok_xAI"
---
Businesses evaluating AI customer support now have another voice model to test, with stronger benchmark results and unchanged base pricing.

SpaceXSI announced Grok Voice 3.0 on Oct. 14, reporting the highest score on Artificial Analysisâ Speech-to-Speech Index. For IT teams, the practical question is whether those gains translate into correctly resolved calls and reliable actions in customer accounts.

The model scored 86.9, ahead of OpenAIâs GPT-Live-1 in its Astra, medium configuration at 83.2 and Googleâs Gemini 3.8 Live with Extended Thinking set to High at 82.6. Grok Voice 2.0 scored 81.3.

Grok Voice 3.0 also achieved 97% on Full Duplex Bench, which measures how naturally AI handles pauses, interruptions and turn-taking. Its speech reasoning score reached 98.5% on Big Bench Audio.

However, the model did not win every category. On Ï-Voice, a benchmark designed to test AI agents on customer support calls involving accents, background noise and interruptions, Grok Voice 3.0 scored 71.6%, behind GPT-Live-1âs Astra, medium configuration at 74.5%.

## Customer service is the real test

SpaceXSI says Starlink uses Grok Voice to handle about 20,000 customer calls daily across multiple languages, resolving roughly 70% without human intervention. The announcement does not specify whether those figures apply exclusively to version 3.0.

The system can verify customer identities, troubleshoot hardware, issue account credits and arrange replacement equipment. The company also says one in four sales calls handled by the system ends in a purchase before the conversation finishes.

Grok Voice 3.0 also powers millions of daily conversations in Tesla vehicles, supporting hands-free requests such as checking calendars and handling email. The model runs Grok Botâs voice mode, too.

SpaceXSI also reports a leading accuracy score of 74% on ServiceNowâs EVA-Bench, which evaluates voice agents across more than 200 enterprise scenarios. The company says the model offers a leading balance of accuracy and customer experience.

In a statement included in SpaceXSIâs announcement, Nitin Aggarwal, ServiceNowâs senior director of product management for Agentic Evals and RAI, said the model âpairs top accuracy with a natural, responsive conversation.â

## Pricing and response-time trade-offs

Grok Voice 3.0 is available through SpaceXSIâs Voice Agent API at a base audio rate of $0.08 per minute, or $4.80 per hour, matching Grok Voice 2.0 pricing. The API documentation also lists a $0.004 charge per billable text input message. Default voice-activity-detection sessions are billed for the full session duration, while push-to-talk sessions are billed for audio sent and received.

Developers using the grok-voice-latest alias have been switched to the new version automatically. Teams using that alias should rerun representative support calls to check tool use, escalation behavior and response timing after the change.

Businesses should compare the total cost per successfully resolved call, including integration, monitoring and human escalation. A lower model charge may offer limited savings if callers need repeated attempts or staff must correct the agentâs actions.

Artificial Analysis measured Grok Voice 3.0âs average time to first audio at 0.81 seconds, compared with 1.34 seconds for GPT-Live-1âs Astra, medium configuration. But Grok Voice 2.0 recorded 0.70 seconds, meaning the latest release is slightly slower to begin responding than its predecessor.

## What the launch means for businesses and users

For businesses, the strongest case for Grok Voice 3.0 is its ability to complete customer service actions rather than simply answer questions. Automating routine requests could reduce call queues and free human agents to handle complex complaints. Before deployment, teams should define which account changes the agent may make, which require approval and when a call must transfer to a person.

Testing should include failed identity checks, disputed charges and requests outside company policy. Teams should log account changes and track repeat calls alongside resolution rates, so an apparently completed conversation is not mistaken for a solved problem.

SpaceXSIâs 70% autonomous resolution figure is encouraging, but it comes from the company itself. The announcement does not detail the measurement period, call sample or criteria for successful resolution, making it difficult to predict whether other businesses would achieve similar results.

For customers, better turn-taking and faster responses could make automated support less frustrating, particularly when checking accounts or troubleshooting connectivity problems. Yet a convincing voice is not enough: an agent that confidently gives incorrect information or fails to resolve an issue can make customer service worse. Businesses should assess successful resolutions and customer satisfaction alongside benchmark scores before handing AI more responsibility.

**Read more: Before giving voice agents access to customer accounts, explore why AI guardrails need data boundaries and how scoped permissions and audit logs can help limit unauthorized actions.**
