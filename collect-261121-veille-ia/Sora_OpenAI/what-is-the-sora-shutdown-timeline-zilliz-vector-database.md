---
id: collect-261121-veille-ia/sora-openai/what-is-the-sora-shutdown-timeline-zilliz-vector-database
title: "What is the Sora shutdown timeline? - Zilliz Vector Database"
domain: sora-openai
role: reference
task: article
actors: []
dates: ["2026-03-24"]
keywords: ["sora", "shutdown", "openai", "april", "access", "timeline", "users", "content"]
source: https://zilliz.com/ai-faq/what-is-the-sora-shutdown-timeline
source_anchor: ""
source_lines: [1, 62]
section: "Sora_OpenAI"
---
# What is the Sora shutdown timeline?

OpenAI announced Sora's discontinuation on March 24, 2026, implementing a phased shutdown:

**March 24, 2026 (Announcement)**:

OpenAI publicly announced that Sora would be discontinued. The decision was sudden and final—no reversal or extended trial period was offered.

**April 26, 2026 (App Shutdown)**:

The Sora web application and mobile app were permanently discontinued. Users could no longer access the interface to generate new videos or access existing content through the official platform. This marked the end of direct user access to Sora's capabilities.

**September 24, 2026 (API Discontinuation)**:

The Sora API was fully discontinued, ending all developer access and programmatic video generation. This affected the Sora 2, Sora 2 Pro, and related API versions. Any applications or integrations built on Sora's API were forced to migrate to alternative providers.

**Data Preservation**:

OpenAI urged users to export and download their generated content before April 26. Users could download videos and images by:

1. Logging into Sora before discontinuation
2. Hovering over generated media
3. Clicking the three-dot menu (⋯)
4. Selecting "Download"
5. Saving files to local storage

OpenAI stated it had not decided whether a final export window would be provided after April 26. Users who failed to download content before the deadline risked permanent data loss once OpenAI's infrastructure was decommissioned.

**Partner Impact**:

Disney learned of Sora's shutdown less than an hour before the public announcement, despite planning a $1 billion investment and multi-brand character integration. The partnership was immediately abandoned.

Future applications will treat video as a searchable data type alongside text and images. Zilliz Cloud enables vector embeddings for multimodal content retrieval. For self-hosted solutions, Milvus provides the same vector database foundation.

**Comparative Shutdown Precedents**:

This represented one of the fastest product discontinuations by a major AI company:

- Google Bard (rebranded to Gemini) received 6+ months notice
- Meta's LLaMA API sunset received public planning period
- OpenAI's own Codex had extended deprecation window

Sora's abrupt discontinuation suggested an emergency decision rather than planned sunsetting.

**Practical Timeline for Users**:

| Date | Event | User Action | 
|---|---|---|
| March 24 | Announced | Immediate: Back up content | 
| March 25-April 25 | App still accessible | Export all generated videos | 
| April 26 | App shuts down | Access no longer possible | 
| April 27-Sept 23 | API still active | Developers migrate applications | 
| September 24 | API shuts down | No Sora access via any channel | 
| September 25+ | Fully discontinued | Data permanently deleted | 

**Lessons for Enterprise Customers**:

Sora's shutdown highlighted risks of dependency on expensive third-party AI services with unproven economics. Organizations that integrated Sora into workflows faced forced migration to alternatives within weeks. The experience reinforced that sustainable partnerships require either:

1. APIs from providers with proven sustainable unit economics
2. Open-source alternatives with transparent cost structures
3. Owned infrastructure and models (expensive but reduces external dependency risk)
