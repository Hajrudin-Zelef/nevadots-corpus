---
id: collect-261121-veille-ia/docs-techniques/office-hours-writeups-office-hours-2026-08-18-raw-md-at-538f881d0124fff2d693ad014402cb7d44531a4a-pam
title: "office-hours-writeups/office-hours/2026_08_18/raw.md at 538f881d0124fff2d693ad014402cb7d44531a4a · pamelafox/office-hours-writeups"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-08-18"]
keywords: ["agents", "code", "office-hours-writeups", "microsoft", "github", "agent", "markdown", "online"]
source: https://github.com/pamelafox/office-hours-writeups/blob/HEAD/office-hours/2026_08_18/raw.md
source_anchor: ""
source_lines: [1, 70]
section: "Docs_Techniques"
---
**🐍 Python + AI Office Hours**

**Weekly News Roundup - Tuesday, August 18, 2026**

**7-day scan:** August 11-18, 2026

**🏢 Microsoft / GitHub**

Microsoft's reasoning model targets coding, mathematics, and agentic workflows.

Apps can automatically return cleaner Markdown when agents request it.

Developers can reproduce production-aligned environments locally.

Kimi K3, MAI-Code-1.1-Flash, parallel CLI tasks, and VS Code 1.133.

Bundle skills and MCP servers once for VS Code, CLI, SDK, and app.

A new reasoning model for agentic coding and long-horizon tasks.

Improved app development, codebase research, and verification.

Amplitude, Endor Labs, LaunchDarkly, and PagerDuty join the workflow.

Existing usage migrates to unified Azure Marketplace consumption units.

**🌐 Industry**

Fission AI positions it as a fluid alternative to spec-kit for coding agents.

`Agent.realtime()` adds WebRTC, tools, dependencies, and Logfire traces.

Hamel Husain organizes 13 sessions around evals, context, and systems.

Statistical token-choice signals aim to identify generated content.

Agents are strong; evals, UI, observability, and open infrastructure lag.

The strongest setup scored 0.613 across 177 tasks and 79 real skills.

Recent benchmarks show retrieval quality drives most of the lift.

Embeddings can live beside operational data without another database.

AI-written code shifts trust toward testing, observability, and guardrails.

**👩💻 What I've Been Up To**

Compares free-form SQL, read-only access, and typed tools.

Intent auth, agent factories, agent SEO, and personalizable software.

Generated fonts, logos, layouts, and PDFs for ingestion testing.

**📅 Upcoming Events**

| Date | Event | Location | 
|---|---|---|
| Weekly Tue | Python + AI Office Hours | Online | 
| Sep 9 | MCP Live! | Online | 
| Sep 14 | MCP Community Connect | San Francisco | 
| Sep 23-25 | WeAreDevelopers World Congress | San Jose | 
| Sep 30 | Copilot Dev Camp Summit | Online | 
| Sep 30 | ACA Sandboxes Livestream | Online | 
| Oct 22-23 | AGNTCon+MCPCon | San Jose | 
| Oct 28-29 | GitHub Universe | San Francisco | 

Ahmed — 11:14 AM I have 3 years of experience as a cloud platform engineer and am currently transitioning toward building AI automations, specifically chatbots and RAG agents (using Google Antigravity).While I can read and understand Python code decently well, I am not a developer from scratch, and I’m deeply skeptical of the raw code that AI agents generate.Given my infrastructure background, what are the best steps, frameworks, or architectural practices I should adopt to bridge this gap and build production-grade, highly reliable AI applications?Would love to hear your insights on balancing safe code execution with AI agent autonomy. Thanks! Justin Trantham [FlowDevs.io] [FLOW], — 11:18 AM Start with something fun and small Pamela FoxRole icon, Microsoft — 11:23 AM https://techcommunity.microsoft.com/blog/appsonazureblog/announcing-public-preview-markdown-for-agents-in-azure-app-service/4537023 TECHCOMMUNITY.MICROSOFT.COM Announcing public preview: Markdown for Agents in Azure App Service... Why Markdown for Agents? Web pages often contain scripts, styles, and HTML markup that are useful to browsers but add noise when the content is sent to an AI... Announcing public preview: Markdown for Agents in Azure App Service... pablocotan — 11:23 AM also we have https://webmcp.dev/ Will that get more used? Justin Trantham [FlowDevs.io] [FLOW], — 11:25 AM I hope so - I create what I think would be helpful https://github.com/CakeRepository/JuanPage which works similar i think (just renders websites real time) user talks to chat and builds frontend at runtime. Then the agent builds the UI are runtime.

Are you using it? GitHub GitHub - CakeRepository/JuanPage: JuanPager — serverless universa... JuanPager — serverless universal page renderer for AI-generated interfaces - CakeRepository/JuanPage JuanPager — serverless universal page renderer for AI-generated interfaces - CakeRepository/JuanPage Pamela FoxRole icon, Microsoft — 11:26 AM https://github.com/webmachinelearning/webmcp GitHub GitHub - webmachinelearning/webmcp: 🤖 WebMCP 🤖 WebMCP. Contribute to webmachinelearning/webmcp development by creating an account on GitHub. GitHub - webmachinelearning/webmcp: 🤖 WebMCP Justin Trantham [FlowDevs.io] [FLOW], — 11:26 AM No its all chat the agent looks at the codebase Its schema driven lol Tell the agent to make you a pizza? Github copilot with the pizza url? Pamela FoxRole icon, Microsoft — 11:32 AM https://googlechromelabs.github.io/webmcp-tools/demos/pizza-maker/?share=JTdCJTIyc2l6ZSUyMiUzQSUyMkV4dHJhJTIwTGFyZ2UlMjIlMkMlMjJzdHlsZSUyMiUzQSUyMkNsYXNzaWMlMjIlMkMlMjJsYXllcnMlMjIlM0ElN0IlMjJzYXVjZSUyMiUzQWZhbHNlJTJDJTIyY2hlZXNlJTIyJTNBZmFsc2UlN0QlMkMlMjJ0b3BwaW5ncyUyMiUzQSU1QiU3QiUyMmVtb2ppJTIyJTNBJTIyJUYwJTlGJThDJUI2JUVGJUI4JThGJTIyJTJDJTIyc2l6ZSUyMiUzQSUyMk1lZGl1bSUyMiUyQyUyMmNvdW50JTIyJTNBNSU3RCU1RCU3RA%3D%3D JohnRole icon, MVP — 11:37 AM Image I think you found it already but you needed to ask it to use the tool the ai assistant thingy you clicked on before clicking on gemnii Justin Trantham [FlowDevs.io] [FLOW], — 11:39 AM It would be nice to never have to build a UI and just have the agent build it at watch time Pamela FoxRole icon, Microsoft — 11:40 AM https://gofastmcp.com/apps/generative FastMCP Generative UI - FastMCP Let the LLM build custom Prefab UIs on the fly. Generative UI - FastMCP JohnRole icon, MVP — 11:40 AM Image pablocotan — 11:41 AM What I understood as very useful is specially if the website has a lot of information to be scraped (and navigated), and you have a tool to find products, then it would be faster, save tokens, and simplify the agent work Justin Trantham [FlowDevs.io] [FLOW], — 11:41 AM have you looked at NLWeb? JohnRole icon, MVP — 11:41 AM ohhh i found that removing the attached webpage works in the message box below there is an x button to attach the reference when asking Pamela FoxRole icon, Microsoft — 11:43 AM https://github.com/ChromeDevTools/chrome-devtools-mcp GitHub GitHub - ChromeDevTools/chrome-devtools-mcp: Chrome DevTools for co... Chrome DevTools for coding agents. Contribute to ChromeDevTools/chrome-devtools-mcp development by creating an account on GitHub. GitHub - ChromeDevTools/chrome-devtools-mcp: Chrome DevTools for co... Justin Trantham [FlowDevs.io] [FLOW], — 11:47 AM all my prompts end with "by any means necessary" (just kidding) Pamela FoxRole icon, Microsoft — 11:48 AM https://github.com/pamelafox/pydanticai-playwright-agent GitHub GitHub - pamelafox/pydanticai-playwright-agent Contribute to pamelafox/pydanticai-playwright-agent development by creating an account on GitHub. Contribute to pamelafox/pydanticai-playwright-agent development by creating an account on GitHub. Justin Trantham [FlowDevs.io] [FLOW], — 11:48 AM Grok 4.6 has been pretty good pablocotan — 11:48 AM https://learn.microsoft.com/en-us/microsoft-edge/web-platform/release-notes/151 for testing webmcp.. in theory Justin Trantham [FlowDevs.io] [FLOW], — 11:49 AM I've been using it through Cursor Pamela FoxRole icon, Microsoft — 11:50 AM https://media.x.ai/v1/website/card-4p6-4cd2dc57.pdf Justin Trantham [FlowDevs.io] [FLOW], — 11:51 AM Rating on the ability to create a bio weapon is actually nuts Pamela FoxRole icon, Microsoft — 11:55 AM https://github.com/Fission-AI/OpenSpec GitHub GitHub - Fission-AI/OpenSpec: Spec-driven development (SDD) for AI ... Spec-driven development (SDD) for AI coding assistants. - Fission-AI/OpenSpec GitHub - Fission-AI/OpenSpec: Spec-driven development (SDD) for AI ... Justin Trantham [FlowDevs.io] [FLOW], — 11:55 AM The idea of WorkOS seems interesting. I've been letting agents run longer overnight pablocotan — 11:57 AM there are several very good articles in MEDIUM comparing different frameworks for spec driven development. Based on the project state, complexity, team size, level of testing, etc. Justin Trantham [FlowDevs.io] [FLOW], — 11:59 AM Lol i love that I've been its fun to hear people are thinking of similar stories. The project we are working on allows for agents to control computers remotely (https://flowrmm.com/) and it actually works. The question becomes, what should we allow it to do FlowRMM Safe AI Endpoint Operations | FlowRMM FlowRMM gives IT operators browser remote console, human-approved actions, audit history, and native MCP. Cohort 01 is $99/mo or $1 per endpoint, whichever is more. Safe AI Endpoint Operations | FlowRMM its an MCP Remote Management pablocotan — 12:00 PM Is it time? Justin Trantham [FlowDevs.io] [FLOW], — 12:00 PM 🙂 Pamela FoxRole icon, Microsoft — 12:00 PM https://www.anthropic.com/news/claude-text-watermark How Claude's text watermarking works Future Claude models will generate text that contains a watermark. This is a way of determining the likelihood that Claude was involved in writing the text, and we, along with several other major AI providers, are implementing this change to comply with the EU AI Act. In this article, we share answers to some of the questions we’ve received a... Ornate quill pen resting on a detailed hand, positioned against a textured background Justin Trantham [FlowDevs.io] [FLOW], — 12:00 PM Boo watermarking Pamela FoxRole icon, Microsoft — 12:01 PM https://www.aipotluck.org/map Open Source AI Gap Map - AI Potluck We are building a public option for AI that is open, auditable, and in service of people over profit. However, the open source AI stack has gaps. Now, together, we can close them. Image https://blog.pamelafox.org/2026/08/building-safe-mcp-servers-for-your.html Building safe MCP servers for your PostgreSQL database Model Context Protocol (MCP) is an open protocol that describes how agents can connect to external tools and data sources, and is now wi... Image https://techcommunity.microsoft.com/blog/azuredevcommunityblog/join-us-for-our-mcp-live-—-a-free-livestream-covering-all-things-mcp/4537980 TECHCOMMUNITY.MICROSOFT.COM Join us for our MCP Live! — A free livestream covering all things... The Model Context Protocol (MCP) is the open standard for connecting AI models to tools and data. It was first introduced to the world by Anthropic in... Join us for our MCP Live! — A free livestream covering all things... Justin Trantham [FlowDevs.io] [FLOW], — 12:02 PM Thanks so much always learning so much here!
