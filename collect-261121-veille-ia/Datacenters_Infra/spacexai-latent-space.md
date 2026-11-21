---
id: collect-261121-veille-ia/datacenters-infra/spacexai-latent-space
title: "SpaceXAI — Latent Space"
domain: datacenters-infra
role: reference
task: article
actors: []
dates: ["2024-05-26"]
keywords: ["grok", "spacexai", "colossus", "anthropic", "nvidia", "infrastructure", "cursor", "model"]
source: https://latent.space/wiki/spacexai-c077decc/
source_anchor: ""
source_lines: [1, 89]
section: "Datacenters_Infra"
---
## Browse · Organizations

# SpaceXAI

The organization behind Grok combines model development, persistent work agents and Colossus compute infrastructure, following SpaceX’s acquisition of xAI in February 2026.

SpaceXAI develops the Grok models and agent products and has agreed to provide Anthropic access to its Colossus AI infrastructure. SpaceX acquired xAI in February 2026; subsequent work included jointly training Grok 4.5 with Cursor, launching the persistent-agent product Grok Bot, and signing an agreement to give Anthropic access to Colossus 1 for additional Claude capacity.[9][14][19][11]

These products and infrastructure grew out of xAI’s earlier work on language models, image generation and large GPU clusters, beginning with its July 2023 company announcement.[15][16]

## Earlier history: xAI and the first Grok models

xAI released Grok-1 on X in November 2023. By May 2024, it had announced Grok-1.5 with long-context capability and Grok-1.5V with image understanding, and had released Grok-1 as open source. On May 26, 2024, it announced a $6 billion Series B, with participants including Valor Equity Partners, Vy Capital, Andreessen Horowitz, Sequoia Capital and Fidelity Management & Research Company. The stated uses were bringing products to market, building infrastructure and accelerating research.[15]

By December 2024, xAI had shipped Grok 2, a developer API and Aurora, its proprietary autoregressive image-generation model for Grok. Its infrastructure centerpiece was **Colossus**: xAI reported 100,000 NVIDIA Hopper GPUs using an NVIDIA full-stack reference design, full operation in 122 days, and workloads beginning 19 days after the first servers arrived. Expansion to 200,000 Hopper GPUs using NVIDIA Spectrum-X Ethernet was then a plan.[16]

xAI closed a $6 billion Series C in December 2024, with NVIDIA and AMD among its strategic investors. On January 6, 2026, it reported completing a $20 billion Series E, exceeding a $15 billion target, with NVIDIA and Cisco Investments participating as strategic investors. In that announcement, xAI reported end-of-2025 capacity exceeding one million **H100 GPU equivalents** across Colossus I and II—an equivalent-capacity measure, rather than a physical H100 card count—and approximately 600 million monthly active users across X and Grok combined.[16][17]

## 2026: media APIs, SpaceX acquisition and external compute

The Grok Imagine API, introduced on January 28, 2026, packaged video generation and editing into creative workflows. It supports native video-audio generation from text or images, scene restyling, adding or removing objects, and motion control. Distribution through both xAI’s own API and partner platforms extended the product beyond the Grok app.[18]

On February 2, 2026, xAI announced that SpaceX had acquired it. By the May 6 Anthropic partnership announcement, the organization was announcing agreements under the SpaceXAI name.[9][11]

SpaceXAI’s agreement with Anthropic committed it to providing access to Colossus 1, which SpaceXAI then described as containing more than 220,000 NVIDIA GPUs, including H100, H200 and GB200 accelerators. Anthropic planned to use the additional compute to improve capacity for Claude Pro and Max subscribers. Anthropic also expressed interest in partnering on multiple gigawatts of orbital AI compute; this was an expression of interest, rather than deployed orbital capacity.[11]

## Cursor collaboration and the Grok 4.5–4.6 generation

Cursor announced the joint release of Grok 4.5 on July 8, 2026. Its team described a mixture-of-experts model trained with SpaceXAI, using trillions of tokens of Cursor data capturing interactions with codebases and software tools. Compared with Cursor’s coding-specialist Composer 2.5, the training mix deliberately included broader STEM tasks, research papers and knowledge work. Cursor continued offering Composer 2.5 as a separate model weight class.[14]

Cursor’s contribution included a **distributed agent system for building training environments**. Engineers specified a problem and how to verify its solution; groups of agents constructed, tested and refined the environment. These realistic reinforcement-learning problems were designed to teach investigation, tool use, recovery from mistakes and verification. In its July 16 Grok 4.5 announcement, SpaceXAI reported training across tens of thousands of NVIDIA GB300 GPUs and reinforcement learning over hundreds of thousands of tasks. Its asynchronous stack allowed agent rollouts to last many hours while learning continued.[14][13]

Cursor disclosed a consequential evaluation limitation: an earlier snapshot of its codebase had accidentally entered Grok 4.5’s training data, giving the model an advantage on CursorBench whose magnitude was unclear. Cursor excluded the benchmark from its release presentation and said the data had been removed for future models.[14]

Grok 4.5 became the default model in **Grok Build**, extending the product’s coding work into office tasks. SpaceXAI described Excel workflows combining web research and multi-sheet formulas, as well as PowerPoint diagrams and Word documents. Its July 16 announcement priced Grok 4.5 at $2 per million input tokens and $6 per million output tokens.[13]

Released on August 12, Grok 4.6 focused on long-running agents and interactive and visual work. SpaceXAI described longer supplemental training with curated model-generated reasoning data, engineering data and an improved optimizer. It used Grok 4.5 to regenerate supervised fine-tuning trajectories, filtered them with model-based checks, and applied agentic reinforcement learning across knowledge work, coding, kernel optimization, web development and computer-aided design. Grok 4.6 launched in Cursor, Grok Build and the API, with starting prices of $2 per million input tokens and $6 per million output tokens.[12]

On August 14, Latent.Space reported that Cursor had announced it was part of SpaceX, with its team joining SpaceXAI to work across Grok, Grok Build, Grok Bot, Grok API and Cursor.[2]

## Grok Bot: persistent work across applications

Grok Bot launched in beta on August 11, 2026, after beginning as an internal prototype. Bots use a shared cloud computer to work across applications and websites, including tools without a clean API or MCP interface. They can message one another, share context in threads and coordinate in group chats. Demonstrated workflows become saved routines that incorporate the user’s corrections.[19]

SpaceXAI reported internal examples including a sales Bot updating CRM records from call transcripts and drafting follow-ups, an operations Bot processing invoices received in Gmail, and an engineering Bot reproducing a UI bug, filing a ticket and handing the fix to a debugging Bot. These examples show the product’s intended unit of work: completing a process across tools and handing off between agents.[19]

Grok Build also drew scrutiny over data handling. In July 2026, Latent.Space reported allegations that its CLI uploaded entire repositories, including private code and secrets. The same report described SpaceXAI’s response: zero-data-retention teams’ trace and code data would not be retained, API-key use would respect those controls, and the `/privacy` command could disable retention and delete previously synced data. These were company assurances reported in response to the allegations.[7]

## Financing

**xAI Series B**

On May 26, 2024, it announced a $6 billion Series B, with participants including Valor Equity Partners, Vy Capital, Andreessen Horowitz, Sequoia Capital and Fidelity Management & Research Company.[15]

**xAI Series C**

xAI closed a $6 billion Series C in December 2024, with NVIDIA and AMD among its strategic investors.[16]

**xAI Series E**

On January 6, 2026, it reported completing a $20 billion Series E, exceeding a $15 billion target, with NVIDIA and Cisco Investments participating as strategic investors.[17]

## From the Latent Space archive

Original posts and separately reviewed explanations.

## Sources

1. 1
2. 2
3. 3
4. 4
5. 5
6. 6
7. 7
8. 8
9. 9
10. 10
11. 11
12. 12
13. 13
14. 14
15. 15
16. 16
17. 17
18. 18
19. 19

Model source and teaching reviews passed. Human editorial review pending. This profile describes the sources as of 2026-09-17.
