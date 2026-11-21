---
id: collect-261121-veille-ia/divers-tech/sola-fide-everything-that-happened-in-ai-in-september-2026
title: "Sola Fide - Everything That Happened in AI in September 2026"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["september", "model", "percent", "anthropic", "claude", "released", "models", "deepseek"]
source: https://solafide.ca/blog/2026-10-september-ai-news-roundup
source_anchor: ""
source_lines: [1, 155]
section: "Divers_Tech"
---
September delivered major AI announcements across models, agents, enterprise software, search, science, generative media, hardware, security, and geopolitics.

DeepSeek introduced a new architecture built for cheaper long-context inference. OpenAI released GPT-6 Sol and Luna, followed one week later by GPT-6.1 Sol and a new agent platform at DevDay. Anthropic launched three Claude models and expanded its verified-access programs. Google closed the month with Gemini 4 Argon, initially restricted to approved cyber defenders. Salesforce, xAI, Mistral, Runway, ElevenLabs, Higgsfield, Meta, and Alibaba also shipped significant products.

Here is the complete September 2026 AI news roundup.

## New Models and Inference

**September 1 — Anthropic released Claude Fable 5.1 and Claude Mythos 5.1.** The two products use the same underlying model weights but apply different safeguards. Fable 5.1 is the generally available enterprise model. Mythos 5.1 provides vetted cybersecurity and life-sciences organizations with fewer restrictions through Anthropic’s trusted-access programs.

Anthropic reported that Mythos scored 60.9 percent on Terminal-Bench 4.0, compared with 55.8 percent for Fable when its safeguards were enabled. Fable routes flagged cybersecurity requests to Opus 4.8 and biological requests to Opus 5. API customers must configure their applications to accept this fallback behavior or flagged requests can fail.

**September 10 — DeepSeek released DeepSeek-V4.1-Flash.** The 552-billion-parameter mixture-of-experts model introduced a Causal Encoder–Decoder architecture built for workloads that read large contexts and generate relatively short outputs. Its 20-layer encoder activates 8 billion parameters per input token, while its 20-layer decoder activates 16 billion parameters during generation.

DeepSeek combined Compressed Sparse Attention 2, FP4 quantization, and projected decoder state to reduce the persistent key-value cache to 890 bytes per token. The company reported that the model requires one-quarter of the high-bandwidth memory and one-eighth of the solid-state storage used by its previous generation.

V4.1-Flash also includes a 196-billion-parameter Engram memory system for sparse lookup of recurring token patterns and DSpark speculative decoding, which DeepSeek said improved multi-turn generation speed by 1.6 times. The model supports native visual input and a one-million-token context window.

On September 14, DeepSeek routed V4-Pro API requests to V4.1-Flash after reporting better performance, speed, and total runtime from the newer model. Peak pricing was set at $0.30 per million input tokens and $1.20 per million output tokens. Off-peak rates dropped to $0.15 and $0.60. Community developers also demonstrated three-bit-per-weight deployments on two DGX Spark systems, including methods for reclaiming display-reserved memory for CUDA.

**September 22 — OpenAI released GPT-6 Sol and GPT-6 Luna.** The release cut API prices by approximately 50 percent compared with the previous model generation. Sol targeted coding, computer use, and knowledge work. Luna targeted high-volume classification, routing, and routine tasks. Independent Codexometer tests reported that both models completed difficult dependency-scheduling and version-resolution tasks.

**September 22 — Anthropic released Claude Opus 5.5.** Anthropic positioned Opus 5.5 for complex coding, research, and professional work. The company reported scores of 66.4 percent on Terminal-Bench 4.0, 54.4 percent on FrontierCode v1.1, and 81.8 percent on OSWorld 2.0. Opus 5.5 uses safeguards similar to Fable 5.1 and routes some flagged cyber and biology requests to older Claude models.

**September 28 — Anthropic released Claude Sonnet 5.5.** The second model in the Claude 5.5 family is designed for well-scoped everyday tasks, bug fixes, documents, presentations, and spreadsheets. Anthropic said it runs more than 30 percent faster than Sonnet 5 and costs up to 30 percent less for typical workloads.

**September 29 — OpenAI released GPT-6.1 Sol at DevDay.** OpenAI described the model as approaching GPT-6 Astra’s capability in coding, computer use, and professional work at one-fifth of Astra’s standard token price. GPT-6.1 Sol costs $2 per million input tokens, $0.10 per million cached input tokens, and $10 per million output tokens. GPT-6 Luna costs $0.10 per million input tokens and $0.50 per million output tokens.

The source analysis estimated that a repository review using 50,000 fresh input tokens and 3,000 output tokens would cost approximately $0.13 on GPT-6.1 Sol. OpenAI also reset usage allowances around the launch to encourage immediate testing of the new models.

**September 29 — OpenAI announced Ultrafast serving and Pro 500.** Ultrafast prioritizes inference hardware for latency-sensitive workloads. OpenAI reported up to eight-times faster token generation in Codex, six-times faster generation through the API, and throughput reaching approximately 300 tokens per second. The new Pro 500 plan costs $500 per month, provides the highest included usage among OpenAI’s Pro plans, and includes Astra Ultrafast access in ChatGPT Work and Codex. Enterprise deployments can apply geographic hardware-residency restrictions.

**September 30 — Google announced Gemini 4 Argon.** Argon is the first Gemini 4 model and supports up to one million output tokens for extended software-engineering, legal, financial, and cybersecurity workflows. Google reported scores of 77.9 percent on DeepSWE v1.1, 68 percent on CWE-bench v1, and 91.7 percent on LVBench. Introductory pricing was set at $2 per million input tokens and $10 per million output tokens, with cached input discounted by 95 percent.

## Governed Access and AI Safety

**Anthropic expanded verified access for sensitive work.** Mythos 5.1 is available through the Cyber Verification Program and Life Sciences Verification Program. These programs allow approved organizations to conduct defensive security, drug discovery, clinical development, manufacturing, and research biology tasks that Fable’s general safeguards may restrict.

Anthropic also introduced Enterprise Frontier Safeguards, which run safety processing inside cloud infrastructure controlled by the customer. Mythos access carries different monitoring and data-retention requirements from generally available Claude products.

**Google restricted Gemini 4 Argon to the Fairwind Program.** Fairwind launched September 2 for approved cyber defenders, critical-infrastructure operators, government agencies, and trusted partners. Participants must use organization-level entitlements, hardware-bound phishing-resistant multifactor authentication, role-scoped access, and tenant-level audit logging. Organizations outside Fairwind can use the less capable Gemini 3.8 Flash Cyber model for autonomous vulnerability remediation.

**Anthropic advanced the Model Hardware Standard.** The standard functions as an interoperability layer between AI agents and laboratory or factory equipment. It places deterministic interlocks, approval gates, device limits, and audit trails at the hardware interface rather than relying only on model behavior. Anthropic reported using the standard in a San Francisco wet laboratory for robotic drug-discovery experiments.

**Anthropic reported new research uses for Claude.** An internal Fable 5.1 variant formalized mathematical proofs in Lean, including work related to Fermat’s Last Theorem. Anthropic also reported that AI agents were leading 26 percent of its research toward future models. The company removed the Cowork tab from its interfaces and began binding permissions directly to agents instead of a separate workspace container.

**OpenAI published two safety frameworks.** Its Model Misalignment Reporting Framework established a process for reporting concerning model behavior. A separate publication set priorities for third-party assessments covering biological, chemical, and cybersecurity risks. Leaders from OpenAI, Anthropic, and Google also discussed creating an external AI standards organization, although no formal body launched by the end of September.

## Persistent Agents and Enterprise Software

**OpenAI launched Dots at DevDay.** Dots are persistent agents with dedicated cloud-computing environments. They retain state, continue working while the user’s device is offline, and return when a task reaches a decision that requires human judgment. Users can connect applications, define long-term objectives, and set autonomy boundaries. Dots launched without availability in the European Economic Area or United Kingdom.

OpenAI also announced a planned Microsoft Agent 365 integration for governing specialist Dots through Microsoft’s enterprise security and administration tools.

**The OpenAI Agents API entered public beta.** The API provides hosted execution, memory, tool use, computer use, and multi-agent orchestration. A companion Decisions API entered limited preview using GPT-6 Luna to classify inputs, select predefined actions, and route requests without invoking a larger reasoning model.

OpenAI introduced Spaces, Pages, and Sites for collaborative work with connected organizational data. Model Context Protocol Events allow connected applications to trigger agent workflows through webhooks. Interactive plugin sidebars and design-tool integrations expanded how agents appear inside applications.

**OpenAI announced Private Intelligence and Codex Security Cloud.** Private Intelligence includes zero-data-retention options and Private Safety Processing. Codex Security Cloud entered research preview for repository scanning, finding investigation, and preparation of vulnerability fixes. Amazon Bedrock Managed Agents added another deployment path for OpenAI-powered agents inside AWS.

**Salesforce introduced AIforce at Dreamforce, held September 15–17.** AIforce exposes Salesforce data, workflows, business logic, permissions, security controls, APIs, and Model Context Protocol servers to agents operating through other interfaces. Salesforce framed the product as a way to complete CRM work without opening the traditional Salesforce application.

Agentforce Coworker provides a persistent general-purpose teammate inside Salesforce Lightning. Claudeforce connects Salesforce data with Anthropic’s Claude while enforcing each user’s existing permissions. Slackforce brings Salesforce information and actions into Slack channels.

**Salesforce and NVIDIA announced Koa.** Koa is a CRM-specific reasoning model built by post-training NVIDIA Nemotron 3 Super on synthetic data modeled from Salesforce’s enterprise experience. It is designed for multi-step CRM workflows and accurate tool selection. Salesforce also plans Koa deployments in private cloud, classified, and air-gapped environments.

Salesforce reported more than 30,000 customers using its agent platform and attributed over $1.5 billion in annual recurring revenue to Agentforce.

**September 3 — xAI released Grok Bot for Enterprise.** The persistent agent integrates with enterprise applications, procurement systems, Cursor, and the X platform. Grok and Cursor Enterprise customers received temporary free usage. xAI followed with Team Bots on September 28, allowing organizations to share agents with common files, tools, instructions, and memory.

**September 14 — Mistral began merging Chat and Work into Vibe.** The combined interface lets users move between questions and task execution without changing products. Vibe added spreadsheet consolidation, dynamic mini-app generation, and stricter API-key expiration controls. Mistral retired the experimental Leanstral 1.5 proof model and OCR 4.0 on September 30.

**September 20 — Alibaba released Qwen-Image-2.1.** Alibaba also introduced Qwen3Guard, the first dedicated guardrail model in the Qwen family. The releases paired an open-weight media model with a policy layer intended to support safer enterprise deployment.

## Search, Advertising, Workspace, and Science

**Google expanded AI Mode in Search.** A rollout that began August 28 replaced traditional result lists with fuller generated answers for some queries. Paid subscribers received Gemini 3.7 Flash inside the experience during September, pushing conventional links farther down the page.

**Google launched the AI Contribution pilot.** The invitation-only program compensates publishers when their material contributes meaningfully to an AI-generated answer. Earnings appear through a dedicated Search Console panel. The pilot changes the unit of value from a click to inclusion in a synthesized response.

**Google rolled out the September 2026 Spam Update.** The two-week global update entered a volatile second phase on September 30. It targeted scaled programmatic pages, templated service pages, and low-effort AI-generated content that failed to satisfy search intent. AI-translated user-generated content, particularly on Reddit, gained visibility during the update.

**Google Ads moved more campaigns toward AI Max.** Campaigns using automatically created assets or campaign-level broad-match settings were migrated toward Google’s AI Max framework. The system uses AI for targeting, creative adaptation, and localization.

**Google expanded AI inside Workspace.** Docs and Slides received object-level image editing, 4K upscaling, and element translation. The update moved generation and editing into ordinary document and presentation workflows.

**Google expanded the Gemini 3.8 family.** September releases included Gemini 3.8 Flash, Live, text-to-speech, and Flash Cyber. Flash Cyber provides a less restricted defensive-security option below Gemini 4 Argon and the Fairwind access tier.

**Google announced four applied-science projects.** The AlphaGenome Atlas maps the effects of genetic variation at high resolution. MAPL-EMIT tracks methane emissions from space. Project Suncatcher tests machine-learning hardware in orbital environments. Google researchers also completed additional work mapping the fruit-fly brain.

## Generative Media

**Runway expanded the Gen-4 model family.** Gen-4 and Gen-4.5 focus on object permanence, character consistency, and spatial continuity across short video clips. Gen-4 Turbo targets 1080p output in under ten seconds, allowing creators to iterate more quickly.

**Runway expanded its Media Router.** The routing layer selects among Runway models such as Gen-4, Aleph, and Act-Two and third-party systems including Google Veo 3.1 and Kling 3.0 Pro. Customers can optimize selection for cost, latency, or quality.

September additions to Runway Dev included MiniMax H3 Max, OpenAI GPT Image 2.5 Flare and Sunburst, Eleven v4 speech, and frame-rate conversion up to 120 frames per second. Runway also improved professional color workflows and retired older Gen-3 Alpha Turbo and Aleph endpoints in favor of Gen-4.5, Gen-4 Turbo, and Aleph 2.0.

**Runway announced a $10 million Builder Fund.** The fund subsidizes startups building products on Runway’s media infrastructure.

**September 24 — OpenAI discontinued the Sora API.** The Sora web and mobile experiences had already closed in April. Developers using the API had to migrate their video workflows to other platforms.

**September 28 — ElevenLabs released Eleven v4 and Eleven v4 Turbo.** Eleven v4 supports more than 90 languages, multi-speaker dialogue, and stronger voice cloning from as little as ten seconds of source audio. Scripts can include directorial tags such as `[laughs]`, `[whispers]`, and `[pause]`. The Turbo variant targets real-time agents with median inference latency of approximately 100 milliseconds.

**September 17 — Higgsfield launched its public API.** The unified endpoint routes across more than 35 image, video, and audio models. On September 21, Higgsfield integrated GPT-6 Astra to interpret prompts, select generation models, and provision the required GPU capacity automatically.

**Higgsfield reported a $1 billion annualized revenue run rate.** The company calculated the figure by multiplying its latest four weeks of revenue by 13; it does not represent revenue earned during the preceding twelve months. Higgsfield had also raised a $400 million Series B at a $5.4 billion valuation.

## Meta Connect and Ambient AI Hardware

**September 23–24 — Meta held Connect 2026.** The event expanded the Muse agent across models, virtual reality, AI glasses, and portable hardware. Muse Spark 1.3 targeted reasoning and agentic workflows, Muse Code moved out of beta for long-horizon engineering, and the Meta Model API became generally available.

**Meta announced Meta VR Glasses for spring 2027.** The $1,299 magnesium-alloy device weighs approximately 100 grams—about one-fifth as much as Quest 3—and uses a tethered compute puck. Meta described a 5K display, cinema-grade clarity, and holographic avatars. The glasses use eye tracking, hand gestures, and voice rather than controllers.

The v207 software development kits launched for Unity, Unreal, Android, and the web. The Flux update brought higher-fidelity hand tracking to Beat Saber. Android XR Bridge extended Quest content toward PC virtual reality.

**Meta previewed WebMCP for AI glasses.** Developers can expose selected functions in a web application for Meta AI to call through the glasses. Meta also released Wearables Device Access Toolkit 1.0 and announced connectors that turn external services into tools available through Meta AI.

**Meta announced Ray-Ban Meta Audio.** The 43-gram smart glasses remove the camera and operate as an open-ear audio interface for Muse. The camera-free design reduces visual-surveillance concerns while retaining persistent voice access.

**Meta updated Ray-Ban Meta Gen 3.** The conventional camera-equipped glasses received a 12-megapixel camera, 3K video recording, and a six-microphone array.

**Meta introduced Muse Charm.** The pocket-sized, keychain-style device provides another real-time voice interface for the Muse agent. Meta runs the Llama 4 Maverick-powered assistant inside an isolated Muse Secure virtual machine.

## Security, Geopolitics, and Policy

**September 8 — U.S. agencies issued advisory AA26-251A on industrial-scale model distillation.** The National Security Agency, Cybersecurity and Infrastructure Security Agency, and Federal Bureau of Investigation accused Alibaba, DeepSeek, Moonshot AI, MiniMax, StepFun, and Z.AI of extracting capabilities from OpenAI, Anthropic, and Google models.

The advisory described gray-market proxy networks, fraudulent accounts, and jailbreak prompts used to collect reasoning traces and reinforcement-learning data. It alleged that Alibaba targeted Claude 4, Claude Opus, and GPT-5, while StepFun and Z.AI extracted billions of tokens from Claude 4.5 and GPT-5.5 systems.

The agencies characterized the activity as a core part of China’s AI-development strategy. They argued that low reported training costs—including DeepSeek’s earlier $5.6 million figure—excluded research and development costs borne by the U.S. laboratories whose models supplied the extracted outputs.

The advisory recommended behavioral detection and intentionally degraded responses for suspected distillation networks. That approach introduces a risk of false positives for legitimate enterprise customers whose usage resembles automated extraction.

**Anthropic continued its legal dispute with the Pentagon.** The Department of Defense had designated Anthropic a supply-chain risk after the company maintained restrictions against autonomous-weapons targeting and mass-surveillance uses of Claude.

On September 21, a federal judge granted Anthropic partial summary judgment, finding the designation retaliatory under the First Amendment and arbitrary under the Administrative Procedure Act. On September 26, a 2–1 decision by the D.C. Circuit allowed the Pentagon to maintain the blacklist while further review continued.

**Frontier access continued moving toward tiered control.** Anthropic’s Mythos programs, Google’s Fairwind Program, OpenAI’s premium Ultrafast service, and provider efforts to detect model extraction all placed additional identity, eligibility, monitoring, or economic gates around advanced capabilities.

## September in Brief

September’s releases covered nearly every layer of the AI market:

- DeepSeek, OpenAI, Anthropic, and Google shipped major models.
- OpenAI, xAI, Salesforce, and Mistral expanded persistent enterprise agents.
- Anthropic and Google introduced stricter access programs for sensitive capabilities.
- Google changed search, advertising, Workspace, and scientific tooling.
- Runway, ElevenLabs, and Higgsfield expanded production media infrastructure while OpenAI retired Sora.
- Meta pushed AI into virtual reality, camera-free glasses, and portable voice hardware.
- U.S. agencies, Anthropic, and the Pentagon brought model access and provider policy deeper into national-security disputes.

## Previous Monthly Roundups

Catch up on the earlier months in this series:
