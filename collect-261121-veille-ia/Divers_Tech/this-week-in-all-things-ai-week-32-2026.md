---
id: collect-261121-veille-ia/divers-tech/this-week-in-all-things-ai-week-32-2026
title: "This Week in All Things AI - Week 32-2026"
domain: divers-tech
role: reference
task: article
actors: []
dates: []
keywords: ["week", "agent", "things", "coding", "qwen3", "model", "most", "beta"]
source: https://paragraph.com/@twiata/this-week-in-all-things-ai-week-32-2026
source_anchor: ""
source_lines: [1, 124]
section: "Divers_Tech"
---
This Week in All Things AI covers key developments in models, agents, tools, infrastructure, and policy curated from discussions in the **All Things AI Telegram group.**

If you follow AI for work, research, investing, or just to understand where the technology is heading, this **weekly brief** is a concise way to scan the most important launches, risks, and resources in a few focused minutes.

The week of 2nd to 8th August 2026 saw a flurry of agentic coding launches. DeepSeek opened a closed beta for its Harness framework, and Qwen previewed its 2.4T-parameter Qwen3.8-Max ahead of an open-weight release. Meta launched Muse Code on the new Muse Spark 1.2 model with steep data-sharing discounts, while Command Code rolled out its GOAT subscription plan bundling access to 30+ coding models. OpenAI, with AWS, Cursor, GitHub, Microsoft, and Vercel, introduced Agent Plugins, an open standard for portable agent skills.

Multimodal generation also advanced: MiniMax open-sourced its H3 omni-modal model with day-zero ComfyUI support, Black Forest Labs expanded FLUX 3 Video access, and Alibaba's Wan3.0 entered public beta with native 30-second clips. xAI closed the week with Imagine Image 2.0, adding precision editing tools built for real-world production use. On the infrastructure side, practitioners flagged lighter headless browsers for cutting agent navigation costs, security researchers warned about trust-handoff exploits in agent harnesses, and a widely shared "Power 2026" primer underscored electricity supply as AI's next major bottleneck."

The sections that follow walk through these items day by day, with short context and links so you can dive deeper into the pieces most relevant to your work or interest

Tianyi Cui from DeepSeek's Harness team posted on X inviting open-source developers of Agent Harness projects to join the closed beta by replying or DMing with GitHub ID and representative work.

DeepSeek Harness is an upcoming agent framework focused on tool calling, context management, and terminal execution, positioned as a competitor to Anthropic's Claude Code and tied to their V4 model's enhanced coding agent capabilities.

The beta targets specialized builders, with recent replies already featuring GitHub links to desktop agent tools and CLI integrators, reflecting strong interest from the AI agent development community.

Alibaba's Qwen team announced Qwen3.8-Max as their most capable model yet with 2.4T parameters, emphasizing autonomous coding over 10+ days, long-horizon planning, and native multimodal intelligence for real production tasks.

Benchmark charts show Qwen3.8-Max leading in software engineering, agent workflows, visual perception, and embodied reasoning, surpassing models like GPT-5.6 Sol (max), Gemini1.3-Pro, and Opus4.8 across most categories.

Open weights for Qwen3.8-Max and the smaller Qwen3.8-27B will release next week; API pricing is set at $2/M input tokens, $6/M output tokens, with demos highlighting extended agentic coding and dynamic visual workflows.

MiniMax AI announced the open-weight release of its 33B-parameter MiniMax-H3 omni-modal model on Hugging Face, enabling text/image/video/audio-to-video generation with native synchronized stereo audio up to 15-second 2K clips at 24 FPS.

The community license includes a US carve-out due to generative video copyright litigation with Hollywood studios It is also unavailable in the EU, UK, and South Korea due to regulatory uncertainties around AI-generated video content.

Users in those areas can submit formal licensing requests by committing to robust local compliance controls and guardrails, with MiniMax planning to monitor and adapt policies as regulations change.

ComfyUI announces day-zero native integration for open-weights MiniMax H3,

via Alex

If you are using agents to crawl websites and get robo-blocked or spend too many tokens for navigation, I recommend headless browser rebuilts like: https://github.com/lightpanda-io/browser

It cut my token use on navigation by 80+%

Flux 3 Video now more available from BFL, OpenRouter, FAL, Replicate

It's capable of generating up to 20-second 1080p clips via text-to-video, image-to-video with multiple keyframes, video continuation, and native audio including multilingual dialogue with accurate lip sync.

Draft mode enables fast creative exploration with low-cost previews at $0.06 per second that preserve subjects and motion for seamless upgrade to full quality rendering from $0.17 per second.

2K, 4K support and open weights planned soon.

via Alex

God of the gaps. Hacking Agents through (un)trusted handover commands in the harnesses.

Many harnesses hardcode trust -> "everything beyong this point is trusted" and this can be used to prompt hijack agents:

Quote from the article:

“Most teams audit what the agent can do. The exposure we keep finding is in what happens after, the handoff between ‘approved’ and ‘executed,’ between ‘read’ and ‘published,’ between ‘fetched’ and ‘trusted.'”


via Alex

TIMES magazine started to expose bots to abbreviated articles with baked in ads.

Meta Launches Muse Code Beta for Full Repo Engineering Tasks

Powered by the new Muse Spark 1.2 model from Meta Superintelligence Labs, Muse Code plans changes, writes code, and validates results across sessions using persistent background sub-agents that parallelize work safely. Benchmarks show it competing with top models from OpenAI and Anthropic on tests like Terminal-Bench 2.1 and DeepSWE 1.1, while pricing starts at $1.25 per million input tokens—with a cheap contributor tier at $0.10 if you opt into data sharing. Mark Zuckerberg and chief AI officer Alexandr Wang emphasized its one-command install and focus on reliable, long-horizon coding tasks.

Deepseek plans to raise API prices significantly but no details announced, their v4-Flash 0.14/0.28/0.0028 is currently the floor for Chinese models including Mimo-v2.5 though Meta offers a lower price point for its recently launched Meta Spark-v1.2-contributor at 0.1/0.2/0.002.

Note that the low cache price in particularly from Deepseek's own provider allows them to train on input as per their ToS/privacy policy. Other providers who also server DSv4-Flash-0731 may have different policies and serve it at different price points. Pricing is always a function of model and provider

Command Code's announces the GOAT plan at $10/month for $70 credits, delivering 7x value and access to over 30 open-weight and closed AI models for coding tasks.

The plan builds on their $1 Go starter tier by providing enough usage for meaningful projects, emphasizing solutions to open model challenges like cost, reliability, and performance via high cache rates and design tools.

Command Code positions itself as a leader in open model coding agents with leaderboard success, a v1 codebase rewrite, new Mods API, and upcoming open source release later this month

PS, It also offers Muse Spark v1.2 and Muse Spark v1.2-contributor

Alibaba's Tongyi Lab announces Wan3.0 public beta, a video generation model featuring native 30-second single-run video creation, high-fidelity reality-grade rendering, and expressive character consistency.

Omni-reference capability allows input of structured files like documents, spreadsheets, slides, PDFs, and webpages alongside text, images, audio, and video for more versatile content generation.

Now available on Alibaba Cloud Model Studio and Qwen Cloud with API pricing from $0.05/sec at 480p; additional demos in the thread showcase 30-second outputs and complex reference handling....

OpenAI Launches Agent Plugins Standard for Shared AI Extensions

OpenAI, teaming up with AWS, Cursor, GitHub, Microsoft Code, and Vercel, introduced Agent Plugins on Thursday. This open standard lets developers package AI agent skills and MCP server configurations into a shared format, working across tools like ChatGPT, Cursor, GitHub Copilot, and VS Code.

Some developers question its depth without broader adoption from players like Anthropic.

Some SpaceX'y news in anticipation of Grok-4.6.

For all intents and purposes, I'm consider the Cursor acquisition by SpaceXAI a done deal even thought as of now it's not yet closed

FWIW, Grok Build is my daily driver. I combine it with MCP's of Monid [put in a dollar or two at a time and use the tools it aggregates without worrying about balance sprawl], TinyFish and Nia [Nia, you could think of it as Context7 on steroids]

Exa and Octen are my favourite tools to use via Monid

Opencode is also massively improving its web search in its upcoming version 2

**Cursor's Smart Router Boosts AI Coding Efficiency and Satisfaction**

**Grok Build Hits 1.0.0 Milestone with Refined Coding Tools**

via Chyi Yan Hshieh

https://paragraph.com/@twiata/this-week-in-all-things-ai-week-31-2026Curious question for the broader group, do you see a demand for FDEs in APAC? Or at least for companies who are doing the legwork of setting up AI systems and agents?

Former Citadel quant Neel Somani highlights power supply as the core AI bottleneck over chips or memory, releasing "Power 2026" primer detailing electricity markets, data center demands, and pricing dynamics.

The guide covers US grid regions like PJM, ERCOT, and MISO, marginal pricing via merit order, heat rates, and how AI data centers—already at ~5% of US power—could exceed total generation by mid-2030s.

Somani's expertise from power/gas trading and data center advising informs hedging strategies, buildout challenges, and calls for faster permitting of gas/coal plants plus transmission upgrades to meet surging demand.

Props to Matthew Mousa of Alpha Compute for pointing me towards this material

xAI's next-generation image model Imagine Image 2.0 was just launched emphasizing precision editing, crisp text rendering, improved factuality, and practical real-world applications like infographics, ads, UI mockups, and game assets.

Key new tools include Magic Wand for targeted region changes, Segmentation for precise area selection, Smart Resize for custom aspect ratios, and templates for workflows such as product shots, collages, icons, and consistent character/world building for video.

It ranks #2 worldwide in both text-to-image and image editing arenas per recent Arena leaderboards, and is now available as the Quality Mode on grok.com/imagine, iOS/Android apps, with API access coming soon.

Below is my personal website which aggregates links to many of my socials as well as the various content and community that I curate. Feel free to share this link to others who you think may find this content/community useful to them

The cover image of this newsletter via generated via the Seedream 5 Pro model within the **Krea** tool via the following prompt

Craft an image of an expansive, rolling prairie, where the golden grasses sway in the breeze, and herds of wild mustangs roam freely beneath an endless, open sky
