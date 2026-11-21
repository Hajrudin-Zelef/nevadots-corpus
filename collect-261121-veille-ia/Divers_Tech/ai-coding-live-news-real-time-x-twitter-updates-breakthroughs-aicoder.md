---
id: collect-261121-veille-ia/divers-tech/ai-coding-live-news-real-time-x-twitter-updates-breakthroughs-aicoder
title: "AI Coding Live News — Real-Time 𝕏/Twitter Updates & Breakthroughs — AICoder"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["daybreak", "cyber", "gpt-6", "access", "blue", "must", "project", "organization"]
source: https://aicoder.com/news
source_anchor: ""
source_lines: [1, 9]
section: "Divers_Tech"
---
On November 20, Michael Luo, who works on product at OpenAI, announced on X that the Agents API now supports OpenAI's Daybreak cybersecurity access program. Official docs show developers set access_programs.cyber (daybreak_blue or daybreak_red) when creating or updating a session rather than on individual messages; Blue pairs with general models such as GPT-6 Sol, Red unlocks GPT-5.6 Cyber, and GPT-6 Cyber needs additional approval. Organizations must already be approved for Daybreak and enable it in a named, non-default project.

#### Key Takeaways

- Usage: add access_programs.cyber to the request body when creating or updating an Agents API session, set to daybreak_blue or daybreak_red; it cannot be set per message
- Models: the Blue example uses GPT-6 Sol and GPT-6 Luna also qualifies; the Red example uses GPT-5.6 Cyber; GPT-6 Cyber needs Red plus additional model approval
- Prerequisites: the organization must be approved for Daybreak, an org owner enables it in a named (non-default) project, and requests use that project's API key
- Compliance: every Daybreak Red organization must require SSO for the API organization holding Cyber access and enforce MFA through its identity provider
- Use cases (listed by Michael Luo): investigating suspicious activity, implementing and verifying security fixes, triaging for human review, and quarantining suspicious actors
