---
id: collect-261121-veille-ia/deepseek/deepseek-opens-app-features-through-its-api-casrai
title: "DeepSeek Opens App Features Through Its API — CASRAI"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: ["2026-11-13"]
keywords: ["deepseek", "features", "data", "search", "announcement", "deepseeks", "model", "through"]
source: https://casrai.org/news/deepseek-app-capabilities-api-2026
source_anchor: ""
source_lines: [1, 32]
section: "DeepSeek"
---
On 13 November 2026 DeepSeek announced what it calls the “Full-Suite App API Release,” stating that the capabilities behind the DeepSeek App are now available through its API. According to DeepSeek’s announcement, they can be switched on as built-in features of a chat request or called as standalone APIs. For research offices and researchers who build on or evaluate large language model services, the change matters less for the headline than for what it adds to the data flows involved.

## What DeepSeek announced

DeepSeek’s announcement dated 13 November 2026 lists two groups of capabilities.

- **Built-in features enabled with request parameters:** web search (web or image search with citations, in non-thinking mode, enabled with`deepseek_web_search` ); deep thinking search, which searches and reads pages during multi-step reasoning; file input, which parses PDFs, Office files, images and other formats into the conversation context; URL reading, which fetches links in user messages (enabled with`deepseek_read_url` ); and safety checks that screen inputs and outputs and return refusal indicators when risks are detected.
- **Standalone APIs:** Search, Fetch, OCR, ASR (speech recognition) and TTS (speech synthesis), offered for custom workflows.

DeepSeek states that the model supporting these features is V4.1-Flash (`deepseek-flash`), available across its Messages, Responses and Chat Completions interfaces, and that tool calls and context assembly are handled server-side. The announcement also points to a demo frontend, an open-source code repository and a guide titled “build_a_chatbot.” This follows DeepSeek’s earlier opening of its in-house search API on 17 October 2026, as recorded in its update log.

## Pricing, conditions and developer responsibilities

DeepSeek says the extended capabilities “incur tool usage fees in addition to standard token charges.” The announcement presents the rates in an image, so no figures are quoted here; check DeepSeek’s own pricing page before budgeting.

The announcement also sets conditions. It states that public-facing apps must enable safety checks, and that developers remain responsible for content moderation, applicable model registration and AI-generated content labelling. It does not state numeric rate limits.

## What it means for researchers and research offices

The following points are CASRAI’s editorial interpretation, not statements by DeepSeek.

- **Documents and links now enter the request.** File input and URL reading mean that PDFs, images and web pages can be passed to the service as part of a request. Before any project data, unpublished manuscripts, participant material or third-party confidential documents are sent to an external API, the project’s data management plan and data-sharing agreements need to permit it.
- **Personal data triggers formal review.** Where material includes personal data, the usual assessments apply, for example a data protection impact assessment (DPIA) and a lawful basis under the GDPR. DeepSeek’s announcement does not describe data location, retention or transfer terms, so those need to be checked in its terms and privacy documentation rather than assumed.
- **Institutional and funder rules differ.** Some universities and funders restrict specific AI services, and research security or export-control offices may have a view on sending controlled or sensitive technical data to a foreign-operated service. Check local policy; this post does not assess whether any rule applies to DeepSeek.
- **Reproducibility.** Search-grounded answers depend on live web content and on a model that DeepSeek updates over time. A study that uses these features should record the model name, the date of the run, and which tools were enabled. The NIST generative AI profile guide is a useful framework for documenting such use.
- **Disclosure.** Output produced with search or OCR features still falls under journal and funder generative AI disclosure rules.

## What to check next

Read DeepSeek’s announcement and its terms directly, confirm pricing for the standalone Search, Fetch, OCR, ASR and TTS APIs, and ask your information security and research compliance teams before adding the service to a research workflow.

## Sources
