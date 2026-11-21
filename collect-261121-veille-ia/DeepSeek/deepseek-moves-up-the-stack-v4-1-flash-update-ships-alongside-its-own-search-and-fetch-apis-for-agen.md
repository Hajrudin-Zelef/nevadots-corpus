---
id: collect-261121-veille-ia/deepseek/deepseek-moves-up-the-stack-v4-1-flash-update-ships-alongside-its-own-search-and-fetch-apis-for-agen
title: "DeepSeek moves up the stack: V4.1-Flash update ships alongside its own Search and Fetch APIs for agents | Dealroom News"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek", "model", "search", "update", "fetch", "flash", "agents", "apis"]
source: https://dealroom.co/news/163453-deepseek-moves-up-the-stack-v4-1-flash-update-ships-alongside-its-own-se/
source_anchor: ""
source_lines: [1, 9]
section: "DeepSeek"
---
**What happened?** DeepSeek released DeepSeek-V4.1-Flash-1017 on October 17, an update to the V4.1-Flash model it launched in September. It is live on DeepSeek's website, app and API, served under the existing deepseek-flash model name, and the weights are open-sourced under the MIT licence on Hugging Face and ModelScope. DeepSeek calls it "a Pareto improvement in performance and cost": stronger benchmark scores with faster task completion and shorter outputs.

**The numbers:** On DeepSeek's own comparison, the update scores 77.0 on the DeepSWE v1.1 coding benchmark (up from 74.2), ahead of Claude Opus 5 (74.0) and GPT-5.6 Sol (73.0), and leads its table on Automation-Bench, Agents' Last Exam and Apex Bench. Claude Opus 5 still leads on Humanity's Last Exam, Terminal-Bench 4.0, ProgramBench and NL2Repo-Bench. DeepSeek also says the model now builds complete projects end to end, from games to 3D models, using reinforcement learning with self-feedback.

**Beyond the model:** The bigger shift is infrastructure. DeepSeek opened its Search and Fetch APIs to all developers, powered by an in-house search engine it says serves hundreds of millions of requests a day. Each endpoint costs $0.75 per 1,000 calls; calls made through server-side tools in DeepSeek Harness, Claude Code and Codex stay free for DeepSeek API users until per-request billing starts on November 1, 2026, Beijing time. The model is also tuned for DeepSeek's own agent harness, DeepSeek Harness, and for Tencent's CodeBuddy and WorkBuddy. That gives agent builders a low-cost model plus a first-party web search layer behind one API key, putting DeepSeek in competition with dedicated search-API providers for agent workloads.

**Context:** The release comes as DeepSeek's latest funding round has reportedly grown to between $12 billion and $15 billion at a valuation of about $75 billion, ahead of a planned Shanghai IPO, according to CNBC.

**Sources** DeepSeek on X, Hugging Face model card, DeepSeek Search & Fetch API docs, MarkTechPost. Chart: DeepSeek.
