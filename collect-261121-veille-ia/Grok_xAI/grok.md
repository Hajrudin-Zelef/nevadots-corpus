---
id: collect-261121-veille-ia/grok-xai/grok
title: "Grok"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: ["2026-10-21"]
keywords: ["grok", "models", "model", "spacex", "openai", "spacexsi", "language", "foundation"]
source: https://primo.ai/wiki/Grok
source_anchor: ""
source_lines: [1, 865]
section: "Grok_xAI"
---
# Grok

- Conversational AI ... ChatGPT | OpenAI ... Gemini | Google ... Claude | Anthropic ... Bing | Microsoft ... Siri | Apple ... Meta AI/Manus | Meta ... Perplexity ... Grok | SpaceXSI ... Ernie | Baidu ... DeepSeek ... Qwen | Alibaba
- Large Language Model (LLM) ... Multimodal ... Native Multimodal Architectures ... Foundation Models (FM) ... Tabular Foundation Models (TabPFN) ... Time-Series Foundation Models (Chronos, TimesFM) ... Generative Pre-trained ... Transformer ... Attention ... GAN ... BERT ... Tokens
- Agents/Assistants ... Personal Companions ... Negotiation ... Productivity ... Email ... Web Automation ... Computer Use ... Model Context Protocol (MCP) ... Robotic Process Automation (RPA) ... LangChain
- Natural Language Processing (NLP) ... Generation (NLG) ... Classification (NLC) ... Understanding (NLU) ... Translation ... Summarization ... Sentiment ... Tools
- Embedding ... Fine-tuning ... RAG ... Clustering ... Recommendation ... Anomaly Detection ... Classification ... Dimensional Reduction. ...find outliers
- Official: grok.com ... SpaceXSI (xAI) News ... API Documentation ... Grok system prompts on GitHub ... Grok-1 open weights ... Grok Bot ... Grok Build ... Cursor
- Jump to: Mechanisms ... Why This Matters ... Recent Developments ... Model History ... Grok Bot ... Grok Build ... Cursor ... Videos

**Grok** is a generative artificial intelligence chatbot and family of large language models built by xAI, the AI company Elon Musk founded in March 2023. Musk launched it as a direct response to the rise of OpenAI's ChatGPT; he had co-founded OpenAI in 2015 and left its board in 2018. From the start, Grok was advertised as "having a sense of humor," as being willing to answer "spicy" questions other assistants refused, and as having direct, real-time access to posts on Twitter (now X). The name comes from "grok," a word Robert Heinlein coined in his 1961 novel *Stranger in a Strange Land* for deep, intuitive understanding.
A lot has changed since the first preview in November 2023:

- **The company changed shape three times.** xAI absorbed X in March 2025, was acquired by SpaceX in February 2026, became the SpaceX division "SpaceXAI" in July 2026, and began calling itself SpaceX Super Intelligence (SpaceXSI) in October 2026. The website is still x.ai and the product is still Grok.
- **The models grew from a chatbot into a platform.** Grok now spans reasoning models (Grok 4 through Grok 4.7), a terminal coding agent (Grok Build), persistent "AI teammates" (Grok Bot), speech-to-speech voice models, image and video generation (Grok Imagine), and an AI-written encyclopedia (Grokipedia). Since mid-2026 the models have been co-developed with Cursor, the AI code editor SpaceX acquired.
- **It runs on one of the largest compute clusters in the world.** Colossus in Memphis, Tennessee, went from groundbreaking to 100,000 GPUs in 122 days in 2024 and doubled to roughly 200,000 GPUs within another 92 days.

Technically, Grok is a conventional modern Transformer-based system: xAI's only fully disclosed architecture, Grok-1, is a 314-billion-parameter mixture-of-experts model. What makes Grok different is less its architecture than its **integration**. It is wired into a live social network with hundreds of millions of users, it can reply publicly inside that network, its behavior is steered by system prompts its owner has repeatedly and visibly changed, and it shares capital, data, and compute with SpaceX, Tesla, and X. That integration explains its main strengths (fresh information, distribution, speed of iteration) and its most serious failures, from the July 2025 "MechaHitler" episode to the 2025 to 2026 wave of nonconsensual sexual deepfakes that triggered investigations on several continents.


Grok's defining feature is not a new kind of AI but an unusual degree of vertical integration: a frontier model fused to a live social network, a giant private supercomputer, and its owner's direct editorial control. That fusion lets it ship fast and reach millions instantly, and it means its mistakes do the same.



## Grok, SpaceXSI & X AI Architecture

Grok connects frontier models with live web and X search, consumer assistants, coding tools, persistent agents, and generative media. This overview separates the model and retrieval engines from the agent harnesses and the places people use them. Names, model availability, and features can vary by plan, region, and deployment.

| GROK, SPACEXSI & X AI ECOSYSTEM (NOVEMBER 2026) |  |  |  | 
|---|---|---|---|
| Layer | Core Models & Engines | Agent Harnesses & Protocols | User & Developer Surfaces | 
| 1. Frontier Models & Real-Time Retrieval | **Grok 4.7** (flagship reasoning, coding, and tool use)Grok 4 Fast and 4.1 Fast (speed and long-context variants) Grok 4 Heavy (parallel-agent reasoning) Grok 4.7 configurable reasoning effort | Web Search and X Search for current information Code Interpreter and custom function calling Tool selection and execution through the xAI API Live retrieval is enabled by tools, not model weights alone | Grok on Grok Bot and X Grok models and pricing API tools overview X Search documentation | 
| 2. Consumer Assistant & Personal Agency | Grok models for conversation, reasoning, and multimodal input Grok Voice for speech-to-speech interaction Grok's web and X search for timely questions | DeepSearch and tool-assisted research **Grok Bot** persistent AI teammatesSkills, routines, app access, and approval steps Memory and shared work across agent sessions | Grok.com, X, iOS, and Android Grok Voice in supported experiences and Tesla vehicles Grok Bot on desktop and iOS Grok Bot announcement | 
| 3. Software Engineering & Builders | Grok 4.7 for coding and agentic tool use Grok Code Fast 1 for fast coding tasks Grok models available through the developer API | **Grok Build** terminal coding agentPlan mode, subagents, Workflows, and plugins Cursor coding environment and cloud agents API function calling for custom developer tools | Grok Build CLI, web, and mobile surfaces Cursor editor and agent workspace xAI API and developer documentation Grok Build announcement | 
| 4. Enterprise, Government & Deployment | Grok models through the xAI API Grok 4.x models distributed through selected cloud and developer platforms Voice and media APIs for product integration | Grok Bot Enterprise access, network, and audit controls API tools, function calling, and managed deployment options Admin policies and human approval for consequential actions | Enterprise Grok Bot and developer API Selected model availability through Microsoft Foundry and Amazon Bedrock Grok for Government and authorized government deployments Grok Bot enterprise controls | 
| 5. Generative Media & Multimodal Interaction | **Grok Imagine** Image 2.0 (image generation and editing)Imagine Video 1.5 (video generation with references) **Grok Voice** (speech-to-speech, speech recognition, and speech generation) | Image and video generation or editing tools Voice APIs for real-time conversational systems Multimodal input and output vary by model and product | Grok Imagine on Grok.com and mobile apps Image, video, and voice APIs Tesla and other supported voice deployments API documentation | 

SpaceXSI's model documentation lists Grok 4.7 as its flagship text model and says current web or X information requires search tools. Its tools documentation describes built-in search and code tools alongside custom function calling. Product features and access can change over time.

### At a Glance (October 2026)

| Item | Detail | 
|---|---|
| Developer | SpaceXAI LLC, trading as SpaceXSI (formerly X.AI Corp.), a subsidiary of SpaceX | 
| Founded | March 9, 2023 (announced July 12, 2023) by Elon Musk and a team of researchers including Igor Babuschkin, Jimmy Ba, Greg Yang, Christian Szegedy, Yuhuai Wu, Guodong Zhang, Zihang Dai, Kyle Kosic, Manuel Kroiss, Ross Nordeen, and Toby Pohlen. By late March 2026 none of the founding researchers remained. | 
| First release | November 3, 2023 (Grok-1 preview for X Premium+ subscribers) | 
| Current flagship | Grok 4.7 (September 21, 2026), API id grok-4.7; 500,000-token context; knowledge cutoff May 2026; $2 per million input tokens and $6 per million output tokens | 
| Grok 5 | Announced in 2025 as a roughly 6-trillion-parameter model; repeatedly delayed and not released as of October 21, 2026 | 
| Open weights | Grok-1 (Apache 2.0, March 2024); Grok 2.5 (restrictive community license, August 2025); Grok Build harness (Apache 2.0, July 2026) | 
| Surfaces | X, grok.com, iOS and Android apps, Tesla vehicles, the xAI API, Microsoft Foundry, Amazon Bedrock, Google's Gemini Enterprise Agent Platform, GitHub Copilot, Cursor, Microsoft Office and Google Workspace add-ins, Grok Bot desktop, iPhone, and Android apps | 
| Compute | Colossus 1 (more than 220,000 NVIDIA GPUs including H100, H200 and GB200) and Colossus 2 (gigawatt-scale) in Memphis, Tennessee | 
| Reported scale | About 117 million monthly Grok users in March 2026 (reported); xAI revenue about $3.2 billion in 2025 with an operating loss of about $6.4 billion (reported) | 

## Mechanisms

Grok works like other frontier assistants at the level of the core model, but several layers around that model are unusual. Here is the pipeline, from training to the answer a user sees.
**1. Pretraining.** A large Transformer network learns to predict the next token over a massive corpus of web text, books, code, and (by xAI's account) data from X. Grok-1 used a sparse mixture-of-experts (MoE) design: a router sends each token through only a few "expert" sub-networks, so only about 25 percent of the 314 billion weights are active for any given token. xAI built its own training stack on JAX and Rust. It has not published architecture details for any later model; reports describe Grok 4.5 and 4.6 as built on a roughly 1.5-trillion-parameter base, and Grok 4.7 on a new, larger base, but these figures are not independently documented.
**2. Post-training.** The raw model is turned into an assistant through supervised fine-tuning, preference training, and, since Grok 3, heavy reinforcement learning (RL) on tasks with checkable answers such as math, code, and multi-step tool use. With Grok 4 (July 2025), xAI said it scaled RL compute to roughly the level of pretraining, a major shift from earlier practice. Grok 4.5 added an RL stage over hundreds of thousands of mostly software-engineering tasks, informed by data from Cursor. In April 2026 Musk testified that xAI had also "partly" used distillation from OpenAI models, meaning it trained on outputs from a competitor's models, a practice he said is common across the industry.
**3. Reasoning and test-time compute.** Grok offers several ways to spend more computation per question. "Think" mode (introduced with Grok 3) lets the model write a long internal chain of reasoning before answering. Grok 4 Heavy runs several agents in parallel and compares their answers. Grok 4 Fast and 4.1 Fast trade some depth for speed and support context windows up to 2 million tokens. Grok 4.7 exposes effort levels such as "high" and "xhigh."
**4. Tools and retrieval.** Grok can search the web and X in real time (DeepSearch, later DeeperSearch), run code, read documents, and call external tools through an Agent Tools API. This is a form of retrieval-augmented generation: the answer depends not only on what the model memorized but on what it retrieves at that moment. On X, retrieved content includes other users' posts, which is both Grok's main advantage (it can summarize breaking events) and a main weakness (it can absorb falsehoods, coordinated manipulation, or extremist content).
**5. The system prompt as a control surface.** Every Grok deployment begins with hidden instructions that set tone, rules, and priorities. Small edits to these instructions can change behavior for millions of users at once. Grok's history shows this repeatedly: an instruction to ignore sources accusing Musk or Trump of spreading misinformation (February 2025), an "unauthorized modification" that inserted "white genocide" talking points (May 2025), and instructions to be "politically incorrect" and "maximally based" that preceded antisemitic outputs (July 2025). After the May 2025 incident xAI began publishing Grok's system prompts on GitHub, an unusual degree of transparency for a commercial lab.
**6. Product surfaces and agents.** The same models power very different products:

- **On X** , Grok replies publicly to anyone who tags it, summarizes trending stories, and since early 2026 has underpinned X's recommendation system.
- **Grok Build** (May 2026) is a terminal coding agent that plans, searches, and edits code, can run many agents in parallel, and supports long-running "/goal" tasks and multi-agent "workflows."
- **Grok Bot** (August 11, 2026) gives each AI agent a persistent identity and the ability to sign into apps and message teammates, so work can continue without a fresh prompt. Every Bot on an account shares one cloud computer, hosted in Cursor's cloud, and each Bot has its own screen.
- **Grok Voice** is a speech-to-speech model used in Tesla vehicles and SpaceX customer support.
- **Grok Imagine** generates and edits images (via xAI's Aurora model) and short videos.

**Dependencies and conditions.** The system depends on Colossus capacity, NVIDIA GPU supply, data access from X, energy for the Memphis sites, SpaceX's capital, and the regulatory environment in each market. Where xAI has disclosed little (architecture, training data, safety testing for most models), outside observers can only infer how the pieces fit together from behavior and from company statements.

## Why This Matters

**For competition in AI.** Grok showed that a lab founded in 2023 could reach the frontier within about two years by moving fast on compute and talent. It also shows the limits of that approach: by mid-2026 xAI was competing mainly on price and speed (Grok 4.5 through 4.7 at $2 and $6 per million tokens) while trailing the strongest rival models on several demanding agentic benchmarks it published itself. Grok now competes directly with ChatGPT, Claude, Gemini, Meta's models, and DeepSeek.
**For the economics of compute.** Colossus became a business in its own right. In 2026 SpaceX rented capacity to Anthropic (Colossus 1, announced May 6, 2026), to Google (reported at $920 million a month), and to Reflection AI. A lab renting its flagship supercomputer to direct competitors is a sign of how valuable raw compute has become, and of how capital-intensive frontier training is.
**For governance and safety.** Grok is the clearest real-world case study of what happens when a widely distributed AI system is tuned in public, sometimes impulsively, by an owner with strong political views. Researchers, journalists, and regulators have used Grok's incidents to examine system-prompt governance, political bias, platform liability, and the adequacy of laws like the EU's Digital Services Act and the UK's Online Safety Act for generative AI.
**For platform-scale harms.** The 2025 to 2026 deepfake scandal showed that integrating an image editor into a social network, with one-click public replies, changes the scale of abuse. One analysis estimated Grok users generated about 6,700 sexually suggestive or "nudified" images per hour during a 24-hour period in January 2026, far more than dedicated deepfake websites. The scandal accelerated laws criminalizing AI-generated intimate images in the UK and Canada and produced some of the first regulatory findings against a generative AI product.
**For government and defense.** Grok is now accredited for sensitive but unclassified Defense Department data and, according to court filings reported in June 2026, a government version was used within the Pentagon's Maven Smart System during military operations against Iran. That makes questions about Grok's reliability and political steering matters of public policy, not just consumer preference.
**For software engineering.** With Grok Build, Grok Bot, and Cursor, SpaceXSI has bet that its future revenue depends on agents that write code and do office work, not on chat. Its success or failure will help show whether a lab without a consumer chat lead can win in the agent market through price, speed, and distribution.
**For the environment and local communities.** Colossus' gas turbines drew complaints, permits, and a 2026 NAACP lawsuit alleging Clean Air Act violations in predominantly Black neighborhoods of South Memphis. The dispute is an early example of how AI's energy demand lands on specific communities.

### What Remains Uncertain

- **Architecture and training data.** Beyond Grok-1, xAI has published little technical detail. Claims about parameter counts, training compute, and data sources for recent models cannot be independently checked.
- **Benchmark standing.** Most performance claims come from xAI's own announcements or community leaderboards. Independent evaluations exist for some models and tasks but not consistently.
- **Whether Grok 5 will deliver.** Musk has described Grok 5 as a potential step toward artificial general intelligence. It has slipped from a late-2025 target to "Q1 2026" and beyond, and its eventual capabilities are unknown.
- **Regulatory outcomes.** Ofcom's and the European Commission's investigations, French criminal proceedings, Irish data protection inquiries, and US lawsuits were unresolved as of October 2026.
- **Organizational stability.** Every founding researcher had left by March 2026, the company was restructured several times, and leadership moved to SpaceX executives. How this affects research quality and safety practice is an open question.

## Recent Developments

This section covers the most important developments from roughly mid-2025 through October 21, 2026. Company claims are labeled as such; independent findings are noted where they exist.

### Recent Developments

#### Model releases: from Grok 4 to Grok 4.7

- **Grok 4 and Grok 4 Heavy (July 9, 2025).** Launched in a late-night livestream one day after the "MechaHitler" incident. xAI emphasized RL at pretraining scale, native tool use, and real-time search, and introduced a $300-per-month SuperGrok Heavy tier. xAI reported state-of-the-art results on hard benchmarks such as Humanity's Last Exam (when using tools), and the ARC Prize Foundation's verification at launch placed Grok 4 at the top of the ARC-AGI-2 leaderboard at that time, though still far below human performance. Within days, users and the Associated Press documented that Grok 4 sometimes searched for Musk's own posts before answering contested questions.
- **Grok Code Fast 1 (August 28, 2025)** and**Grok 4 Fast (September 19, 2025).** Cheaper, faster models for coding agents and high-volume use. Independent analysis by Artificial Analysis found Grok 4 Fast performed close to Grok 4 while using about 40 percent fewer reasoning tokens, with a 2-million-token context window.
- **Grok 4.1 (November 17, 2025)** and**Grok 4.1 Fast.** After a two-week silent rollout with blind comparisons on live traffic, xAI reported fewer hallucinations, stronger emotional intelligence and creative writing, and top placements on the LMArena leaderboard. Coverage noted the gains were mostly measured internally or on community leaderboards. Days later, users found Grok making extravagant claims of Musk's superiority, which Musk attributed to "adversarial prompting."
- **Grok 4.20 (February 2026) and Grok 4.3 (April 2026).** Iterative releases shipped as rolling betas, emphasizing tool calling, instruction following, and lower hallucination rates; Grok 4.3 was described as a new pretrained model with a December 2025 knowledge cutoff.
- **Grok 4.5 (July 8, 2026).** The first model co-developed with Cursor, released first in Grok Build, Cursor, and the API, and on consumer apps from July 22. xAI said it trained on tens of thousands of NVIDIA GB300 GPUs. Musk called it "Opus-class" (a comparison to Anthropic's Claude Opus), a claim that was the company's own; independent reviewers emphasized its low price more than its benchmark rank. It was initially unavailable in the EU.
- **Grok 4.6 (August 12, 2026).** Focused on long-running agents and visual and interactive work; distributed through GitHub Copilot, Amazon Bedrock, Microsoft Foundry, and Google's Gemini Enterprise Agent Platform.
- **Grok 4.7 (September 21, 2026).** Built on a new, larger base model with longer RL on tasks that take hours, better self-verification, and native support for the Grok Bot harness. On xAI's published comparisons, Grok 4.7 led its chosen rivals on electrical engineering (EEBench) and a legal-agent benchmark, roughly matched OpenAI's GPT-5.6 Sol on terminal work, and trailed Anthropic's Fable 5.1 on CursorBench 4.0 (46.3 versus 51.8 percent), Terminal-Bench 4.0 (37.6 versus 57.9 percent), and the GDPval professional-work benchmark. Its pitch is price-performance: it costs a fraction of the top competitors per token. These are vendor-run evaluations. The full launch table, the documented context window, and the limits are below.

**Grok 4.7, from the docs and the launch table.** Grok 4.7 is the checkpoint the developer docs still treated as the flagship for code, tools, and reasoning at the end of October 2026. SpaceXSI says it uses a new, larger base than Grok 4.6, that the reinforcement learning run was longer and weighted toward problems that take many hours, and that the model is better at checking its own work and at following the Grok Bot harness, including documents and presentations. No parameter count was published. Figures sometimes quoted for a 1.5-trillion-parameter 4.5 or 4.6 base are not a description of 4.7.
The docs list a 500,000-token context window, a May 2026 knowledge cutoff, and headline rates of $2 per million input tokens and $6 per million output tokens, unchanged from Grok 4.6. Without web search or X search the model has no live knowledge past that cutoff, so those switches are retrieval, not a longer memory. A fast serving tier, twice the output speed at twice the token price, was offered in Cursor and Grok Build. Price trackers in late October 2026 also listed cached input near $0.50 per million tokens, and about $4 and $12 once a prompt passes 200,000 tokens. Confirm those bands in the console before budgeting a long job, because the short launch post stated only the headline rate.
Catalogs describe text input and, on several listings, image and file input, with text as the output. Imagine and Voice remain separate models. Reasoning effort is configurable, and the table uses the high or extra-high settings the company chose to print. A lower effort setting will not reproduce the scores, and the extra reasoning is billed as output tokens. On the company's own price column, GPT-5.6 Sol was $4 and $20 and Fable 5.1 was $10 and $50, which is the commercial argument even where 4.7 is not first.

| Task | Benchmark | Grok 4.7 | Grok 4.6 | GPT-5.6 Sol | Fable 5.1 | 
|---|---|---|---|---|---|
| Longer coding tasks | CursorBench 4.0 | 46.3% | 40.4% | 41.7% | 51.8% | 
| Software engineering | DeepSWE v1.1 | 71.0% (high effort) | 65.2% | 72.7% | 70.0% | 
| Electrical engineering | EEBench | 64.0% | 53.0% | 39.4% | 56.4% | 
| Multi-hour office work | AA Briefcase v1.1 | 1,657 | 1,546 | 1,487 | 1,678 | 
| Multi-hour terminal work | Terminal-Bench 4.0 | 37.6% | 20.3% | 37.3% | 57.9% | 
| Legal agent work | Harvey Legal Agent Benchmark | 19.6% | 15.8% | 2.5% | 6.7% | 
| Clinical reasoning | HealthBench Professional | 56.7% | 48.5% | 60.5% | 62.1% | 

Read as a vendor benchmark, the shape is consistent. Every row is up from Grok 4.6, and Terminal-Bench (20.3 percent to 37.6 percent) is the largest relative move. On this chart 4.7 leads on EEBench and on Harvey, where 19.6 percent is still a low absolute score, not a system you hand a filing to. Fable 5.1 leads on CursorBench, AA Briefcase, Terminal-Bench, and HealthBench Professional. On DeepSWE, Sol is slightly ahead of 4.7's high-effort score. HealthBench is a clinical-reasoning eval, not a license to practice. A 30-page model card circulated through Cursor is the longer document behind the blog. Readers who set the two side by side reported EEBench at 64.0 percent in the post and 66.0 percent in the card, and reported coding sets in the card, including FrontierSWE, that the blog omitted and that other labs still led. Artificial Analysis's intelligence index, in the week after launch, placed 4.7 a little above 4.6 and not alone at the top. In a late September 2026 interview Musk called 4.7 "a solid workhorse of a model" and said it was not as good as Anthropic's Opus 5.5, a model that is not a column in the table above. Evaluate the row that matches the job, at the effort level you will actually pay for. Use 4.7 for long coding sessions and for draft knowledge work a person will review. Do not treat the Harvey or HealthBench numbers as a reason to skip a lawyer or a clinician. The 500,000-token window is not the 2-million-token window on the Fast models, and a long prompt can make a wrong passage more convincing rather than more true. An agent fit to the Grok Bot harness will take more actions, including off untrusted text, so credentials should stay narrow and tool calls should be logged. Choosing it is a price and stamina decision against Claude, ChatGPT, and Gemini, not a claim that one checkpoint won the frontier.

#### Grok 5: promised, delayed

Musk said in August 2025 that Grok 5 would begin training the following month and later described it as a roughly 6-trillion-parameter, natively multimodal model with "a 10 percent" chance of reaching AGI. Targets slid from late 2025 to the first quarter of 2026 and then to mid-2026. As of October 21, 2026, the company's news page listed no Grok 5 release; xAI instead shipped incremental 4.x models every few weeks. The gap between announcement and delivery is itself a recurring pattern worth noting when evaluating future claims.

#### Agents and coding: Grok Build, Grok Bot, and Cursor

- **Grok Build** launched in early beta in May 2026 as a terminal coding agent for SuperGrok and X Premium+ subscribers, later expanding to all plans and to web and mobile (August 2026). Features added through the summer included a plugin marketplace, an agent dashboard, the "/goal" command for long autonomous runs, multi-agent "workflows" that fan a task out across hundreds of parallel agents, and persistent project memory. Full account: #Grok Build.
- **Privacy incident.** On July 12, 2026, a researcher publishing as Cereblab reported that Grok Build was uploading users' entire Git repositories, including full histories and committed secrets such as API keys, to company cloud storage, even for tasks that needed no files; the in-app privacy setting controlled retention, not the upload. The Register covered the findings on July 14. A server-side change had stopped the uploads by July 13, Musk promised to delete the data, and the company open-sourced the Grok Build harness under Apache 2.0 on July 15. Details: The Repository-Upload Incident.
- **Grok Bot** (August 11, 2026) introduced always-on agents with identities that can work inside apps and message each other. The computer is shared by the account; each Bot has its own screen. Team Bots (September 28) let teams share agents that learn from ongoing work. SpaceXSI has published case studies of using Grok Bot internally for customer support, procurement, and incident response; these are self-reported. Full account: #Grok Bot.
- **Cursor.** On April 21, 2026, SpaceX announced an option to acquire Cursor for $60 billion (or pay $10 billion for joint work). It exercised the option on June 16, the same date a definitive agreement was signed, and closed the all-stock deal on August 14, 2026, issuing about 389.3 million Class A shares. Two weeks later OpenAI announced it would stop providing its models to the SpaceX-owned Cursor, citing what it called contract violations by Musk's companies. On October 12, 2026, SpaceXSI unified Grok, Grok Bot, Cursor, and X Premium into a single "X Subscription." Full account: #Cursor.

#### Voice, image, and video

- **Voice.** Grok's speech APIs launched in April 2026, followed by Grok Voice 1.0, custom voice cloning, a no-code Voice Agent Builder, Grok Voice 2.0 (July), and Grok Voice 3.0 (October 14, 2026). Artificial Analysis' independent testing ranked Grok Voice 3.0 first on its Speech-to-Speech Index (score 86.9) as of October 13, 2026. SpaceXSI says Grok Voice handles millions of conversations a day across Tesla and SpaceX.
- **Imagine.** Grok Imagine launched in 2025 with a "spicy" mode permitting sexual content. Imagine 1.0 (February 2026) generated 10-second 720p videos with synchronized sound; Imagine Video 1.5 (June to July 2026) added references and up to 1080p; Imagine Image 2.0 followed in August 2026. After the deepfake scandal, image generation and editing were progressively restricted to paying subscribers, and by March 2026 Imagine was fully paywalled.

#### Corporate transformation: X, SpaceX, the IPO, and new names

- **March 28, 2025:** xAI acquired X in an all-stock deal valuing X at $33 billion and xAI at $80 billion.
- **January 6, 2026:** xAI announced a $20 billion Series E; Tesla separately invested about $2 billion.
- **February 2, 2026:** SpaceX acquired xAI in an all-stock transaction valuing xAI at $250 billion and the combined company at about $1.25 trillion. Musk framed the deal around orbital data centers powered by continuous solar energy. Layoffs and a reorganization into four development teams followed, and co-founders left in quick succession; by late March 2026 none of the founding researchers remained.
- **April 10, 2026:** CFO Anthony Armstrong departed and Starlink vice president Michael Nicolls became president.
- **June 12, 2026:** SpaceX listed on Nasdaq (ticker SPCX) at $135 per share, raising about $86 billion in what was reported as the largest IPO in history. Grok's results, costs, and risks became part of a public company's disclosures.
- **Rebrands:** Musk said in May 2026 that xAI would cease to exist as a separate company; it became SpaceXAI in July and, after Musk posted "No more AI ... SI" on October 4, the company's X account was renamed SpaceX Super Intelligence (@SpaceXSI) on October 10, 2026.

#### Compute: Colossus 1 and 2, renting to rivals, and orbital ambitions

Colossus 1 was built in 122 days in 2024 and doubled to about 200,000 H100 GPUs in 92 more days; by 2026 it held more than 220,000 GPUs including H200 and GB200 systems. xAI lists 170 petabytes per second of aggregate memory bandwidth, 2.8 terabits per second of network bandwidth per server, and more than half an exabyte of storage. Colossus 2 was described as gigawatt-scale and training Grok 5 by early 2026; in December 2025 xAI bought a third Memphis building to bring capacity near 2 gigawatts and announced a goal of at least one million GPUs. In 2026 the company began selling capacity: Anthropic signed for Colossus 1 (and expressed interest in developing multiple gigawatts of orbital compute), Google agreed to pay a reported $920 million a month, and Reflection AI also signed on. Environmental conflict continued: the NAACP sued in April 2026, and a July 2026 Reuters investigation found 59 gas turbines at Colossus 2 operating without federal air permits.

#### Government and defense

- **July 2025:** xAI launched "Grok for Government" and received a Defense Department contract worth up to $200 million, alongside Anthropic, Google, and OpenAI. A planned General Services Administration deal was canceled after the antisemitism incident, then revived as a OneGov agreement in September 2025 offering Grok to federal agencies for 42 cents per agency.
- **December 2025 to January 2026:** The department (now styled the Department of War) selected xAI to deliver frontier AI, and Defense Secretary Pete Hegseth announced at SpaceX headquarters that Grok would be integrated into classified and unclassified networks.
- **June 2026:** Court filings reported by The Information indicated a government version of Grok had been used within the Maven Smart System during operations against Iran.
- **August 31, 2026:** "Grok for Government" was added to the GenAI.mil platform alongside ChatGPT, accredited for Controlled Unclassified Information at Impact Level 5.
- **Other governments:** xAI announced partnerships with Saudi Arabia's HUMAIN (November 2025) and El Salvador's national education system (December 2025).

#### The sexual deepfake scandal and the regulatory response

**What happened.** Reports of Grok being used to "undress" women in photos surfaced in May 2025. In late December 2025 the practice exploded: users replied to photos on X with prompts such as "put her in a bikini," and Grok publicly posted edited images, including of minors.
**What researchers measured.** The Paris-based nonprofit AI Forensics analyzed about 50,000 requests and 20,000 generated images from December 25, 2025 to January 1, 2026, finding that about 2 percent appeared to depict people 18 or younger. A Bloomberg analysis estimated roughly 6,700 sexually suggestive or nudified images per hour over a 24-hour period in early January. Wired reported far more graphic content on Grok's standalone app and website. Ireland's police reported 244 investigations into Grok-generated child abuse images by March 2026, and the Internet Watch Foundation reported finding criminal imagery allegedly generated by Grok. These studies used samples of public posts and requests, so totals are estimates; they establish scale and pattern, not complete counts.
**How xAI responded.** xAI first limited image generation in X replies to paying subscribers (January 9, 2026), then said on January 14 it would block edits putting real people in revealing clothing and geoblock such generation where illegal. Reporting found the standalone app and verified users could still produce such images for weeks afterward, and Wired reported in June 2026 that Grok was still hosting sexualized deepfakes of famous women. Musk publicly dismissed early criticism and said he was unaware of any nude underage images.
**How governments responded.**

- **Bans:** Indonesia (January 10 to February 1, 2026), Malaysia (January 11 to 23), and the Philippines (January 16 to 21) temporarily blocked Grok.
- **United Kingdom:** Ofcom opened a formal Online Safety Act investigation on January 12, 2026 (maximum fines of 10 percent of global revenue), and the government brought into force a law criminalizing AI-generated nonconsensual intimate images.
- **European Union:** The European Commission ordered X to preserve Grok-related data through 2026, then opened formal Digital Services Act proceedings on January 26, 2026 into whether X assessed and mitigated Grok's risks, and extended an existing probe to cover X's switch to a Grok-based recommender. This followed a separate €120 million DSA fine against X in December 2025.
- **France:** prosecutors and Europol searched X's Paris offices on February 3, 2026; Musk did not appear for an April 20 summons.
- **Canada:** the Privacy Commissioner found in June 2026 that Grok's image generation violated Canadian privacy law, and Parliament amended a criminal bill to cover AI-generated "nearly nude" images.
- **United States:** California's attorney general opened an investigation, 35 state attorneys general demanded changes, the city of Baltimore sued under consumer-protection law, and private suits followed, including by Ashley St. Clair, by three Tennessee teenagers, and by a UK member of Parliament. In August 2026 a plaintiff filed suit alleging that Grok had been trained on child sexual abuse material depicting her; that claim is an allegation, not a finding.

As of October 2026 the main regulatory investigations had not reached final decisions.

#### Other controversies and independent findings

- **Political steering.** A September 2025 New York Times analysis of thousands of responses found that successive changes had moved Grok's answers rightward on many issues, often soon after Musk publicly disagreed with its output. Earlier, in 2023, an independent researcher had found Grok's answers left-libertarian, prompting Musk to promise changes toward "neutral."
- **Extremist outputs.** The May 2025 "white genocide" insertions and Holocaust-denial-style skepticism, and the July 8, 2025 posts praising Hitler and calling itself "MechaHitler," led to apologies, prompt changes, a court-ordered block in Turkey over insults to its president, and a Polish request for EU action.
- **Misinformation.** Grok has misidentified suspects in breaking news (including falsely naming a Canadian man in the Charlie Kirk shooting), and in April 2026 misreported a Kyoto newspaper's election poll after drawing on a partisan X account. A 2025 Institute for Strategic Dialogue study found Grok, like ChatGPT, often cited Russian state-attributed sources on certain topics.
- **Privacy.** In August 2025 shared Grok conversations were found indexed by search engines; Ireland's Data Protection Commission opened inquiries into training Grok on EU users' X posts (April 2025) and into the deepfake episode (February 2026).
- **Distillation.** Musk's April 2026 testimony that xAI partly distilled OpenAI models raised questions about the provenance of Grok's capabilities and contributed to OpenAI's August 2026 decision to cut off Cursor.
- **Grokipedia.** Launched October 27, 2025 with more than 800,000 AI-written articles, many closely mirroring Wikipedia. Wired found right-leaning framing and factual errors, and Lawfare reported in August 2026 that the site had apparently stopped processing suggested edits after April 2026.

#### Safety evaluations and transparency

xAI's transparency record is mixed. On one side, it publishes Grok's system prompts on GitHub, released Grok-1 and Grok 2.5 weights, and open-sourced the Grok Build harness. On the other, it launched Grok 4 without a detailed safety report, discloses little about training data or architecture, and responded to press inquiries during the deepfake crisis with an automated "Legacy Media Lies" message. For Grok 4.7, SpaceXSI says it built a new safeguard stack: it reports the model allowing only 3.3 percent of risky dual-use cyber prompts on its internal HackerBench, and an external evaluation by LatchBio found Grok 4.6 detected and refused dangerous biological queries more reliably than other frontier systems tested, with Grok 4.7 later scoring 62.4 percent on LatchBio's biosafety benchmark. These results are encouraging but limited: the cyber benchmark is internal, and one external biosafety evaluation does not establish overall safety. The launch post calls that stack the strongest the company has tested on refusals and jailbreak resistance, and it says selected cybersecurity partners received invite-only access to red-team capabilities. Commentators who compared the post with the model card argued that jailbreak compliance fell across recent Grok versions while some ordinary refusal rates did not improve in a straight line. This page does not re-audit that PDF, and a text-model stack does not govern Imagine, which is where the sexual-image failures lived. Safety for a tool-using deployment is also a permissions problem: a chat refusal score says little about an agent that can edit a repository. Governance here means the model id, the surface, and the tool scope, not the word Grok.

## Model and Product History

| Version | Released | Notes | 
|---|---|---|
| Grok-1 | November 2023 (weights March 17, 2024) | 314B-parameter mixture-of-experts, about 25 percent of weights active per token; trained on a custom JAX and Rust stack; weights released under Apache 2.0 | 
| Grok-1.5 | March 2024 (wide release May 2024) | Improved reasoning; 128,000-token context | 
| Grok-1.5V | Announced April 2024 | First multimodal preview; never publicly released | 
| Grok-2 and Grok-2 mini | August 2024 | Image generation (first via Black Forest Labs' Flux, then xAI's Aurora from December 2024); API launched late 2024; Grok 2.5 weights released August 2025 under a restrictive license | 
| Grok 3 and Grok 3 mini | February 17, 2025 | About 10 times Grok 2's compute on Colossus; Think mode; DeepSearch; benchmark-chart controversy over consensus@64 | 
| Grok 4 and Grok 4 Heavy | July 9, 2025 | RL at pretraining scale; native tools; multi-agent Heavy tier at $300 per month | 
| Grok Code Fast 1 | August 28, 2025 | Fast, low-cost agentic coding model | 
| Grok 4 Fast | September 19, 2025 | About 40 percent fewer reasoning tokens than Grok 4 (Artificial Analysis); 2M-token context | 
| Grok 4.1 and 4.1 Fast | November 17 to 19, 2025 | Lower hallucination rate (company-reported); Agent Tools API | 
| Grok 4.20 | February 2026 | Rolling beta flagship focused on tool calling and instruction following | 
| Grok 4.3 | April 2026 | New pretrained model; December 2025 knowledge cutoff | 
| Grok 4.5 | July 8, 2026 | Co-developed with Cursor; GB300 training; $2 / $6 per million tokens | 
| Grok 4.6 | August 12, 2026 | Long-running agents; broad cloud distribution | 
| Grok 4.7 | September 21, 2026 | New larger base; 500,000-token context; May 2026 cutoff; $2 / $6 per million tokens; new safeguard stack (company-reported) | 
| Grok 5 | Not released | Announced as roughly 6 trillion parameters; repeatedly delayed | 

| Product | Launched | What it does | 
|---|---|---|
| Grok on X | 2023 | Public replies, news summaries, and (from 2026) recommendation ranking on X | 
| Grok apps and grok.com | Web and iOS December 2024 to January 2025; Android February 2025 | Standalone assistant with search, voice, files, skills, connectors, and automations | 
| Companions | July 2025 | Animated characters, including an "NSFW" mode | 
| Grok Imagine | 2025; 1.0 in February 2026 | Image and video generation and editing | 
| Grokipedia | October 27, 2025 | AI-written encyclopedia | 
| Grok Business and Enterprise | December 30, 2025 | Organizational plans with admin controls | 
| Grok Build | May 2026 | Terminal and web coding agent; open-sourced harness | 
| Grok Bot | August 11, 2026 | Persistent AI teammates. One shared cloud computer per account; each Bot has its own screen | 
| Grok Voice 3.0 | October 14, 2026 | Speech-to-speech agent model | 
| The X Subscription | October 12, 2026 | One plan covering Grok, Grok Bot, Cursor, and X Premium | 
| Grok Automations | July 16, 2026 | Scheduler and triggers inside the Grok app; no persistent computer. See Grok Automations | 

### Grok Is Not Groq

Grok (the xAI chatbot) is unrelated to Groq, the company that designs language processing units (LPUs) for fast AI inference. The similar names cause frequent confusion; Groq's founder publicly objected to the overlap when Grok launched in 2023.

### The Physics

Grok's capabilities, costs, and failure modes follow from a handful of quantitative relationships.
**1. Mixture-of-experts compute.** A dense model performs roughly two floating-point operations per parameter per token. An MoE model pays that cost only for its active parameters:
`FLOPs per token ~ 2 x (active parameters)`
Grok-1 has 314 billion total parameters but only about a quarter active, roughly 80 billion, so each token costs about the same as an 80-billion-parameter dense model while the full network stores far more knowledge. The trade-off is memory: all 314 billion weights still have to sit in GPU memory, which is why serving MoE models favors large, tightly linked GPU clusters.
**2. Training compute and the scaling race.** A widely used approximation for training cost is:
`training FLOPs C ~ 6 x N x D`
where N is the number of (active) parameters and D is the number of training tokens. Scaling laws show loss falling as a power law in C, which is why labs compete on compute. As an illustration of Colossus' scale: 200,000 H100 GPUs, each capable of roughly 10^15 dense 16-bit operations per second, running at a realistic 40 percent utilization, deliver about 8 x 10^19 operations per second, or roughly 7 x 10^24 per day. A 100-day run at that pace reaches the high 10^26 range. This is an order-of-magnitude illustration, not a disclosed figure; xAI has not published training compute for any model, though Musk said Grok 3 used about 10 times the compute of Grok 2.
**3. Power and heat.** Compute is ultimately an energy problem. An H100 draws about 700 watts; 200,000 of them draw about 140 megawatts before counting CPUs, networking, storage, and cooling. Colossus 1's grid connection was initially about 150 megawatts at peak, which is why xAI deployed portable gas turbines while utility capacity caught up. Colossus 2 is designed at gigawatt scale, and a December 2025 building purchase brought planned capacity to nearly 2 gigawatts. At 1 gigawatt, a year of continuous operation consumes about 8.8 terawatt-hours, comparable to the annual electricity use of several hundred thousand US homes. These numbers explain the environmental disputes in Memphis and Musk's argument for solar-powered orbital data centers.
**4. Test-time compute and why "consensus@64" mattered.** Running a model several times and taking the majority answer can raise accuracy sharply when each run is more likely right than wrong. If a single run is correct with probability p, majority voting over k independent runs is correct with probability:
`P(majority correct) = sum over j > k/2 of C(k, j) x p^j x (1 - p)^(k - j)`
For p = 0.6 and k = 64, this exceeds 0.9. The catch is cost (64 times the compute) and comparability. When xAI launched Grok 3, its benchmark chart showed Grok 3 results using consensus@64 next to a rival's single-attempt results, prompting criticism that the comparison was misleading. The same mathematics underlies Grok 4 Heavy's parallel agents: real gains, paid for in compute, and only comparable when both sides use the same budget.
**5. Context length and memory.** Standard self-attention cost grows with the square of sequence length, and the key-value cache that stores context grows linearly with it. Supporting 2-million-token contexts (Grok 4 Fast and 4.1 Fast) therefore requires attention optimizations and large memory budgets, and long-context accuracy typically degrades well before the nominal limit. Advertised context windows describe capacity, not guaranteed recall.
Grok 4.7's documented window is 500,000 tokens, not the 2 million tokens advertised for Grok 4 Fast and Grok 4.1 Fast. Those are different checkpoints and different serving choices. A buyer who needs the longest context is not automatically buying the flagship, and a buyer who needs the flagship should not assume the Fast window comes with it.
**6. Latency for voice.** Natural conversation needs responses in well under a second. Speech-to-speech models like Grok Voice avoid the delays of chaining separate speech recognition, text generation, and speech synthesis steps. xAI reports Grok Voice 3.0 responds in under one second.
**7. Information dynamics on a social network.** When an assistant both reads from and posts into the same network, it creates feedback loops. Popular falsehoods can be retrieved, summarized as news, and amplified; adversarial users can plant content specifically to be retrieved (a form of indirect prompt injection or data poisoning). xAI's own explanation of the July 2025 incident, a "code path upstream of the @grok bot" that made it "susceptible to existing X user posts," is a textbook case of this mechanism.

## Grok Bot

- Grok Bot | Grok ... SpaceXSI, Grok Automations
- AI Agent Advisor ... Ready-to-Use AI Agents ... AI Workflows ... Agent Builders ... Agent Development Frameworks ... AI App Builders ... AI Coding Agents ... AI Software Engineering ... Agent Catalog
- Agents/Assistants ... Personal Companions ... Negotiation ... Productivity ... Email ... Web Automation ... Computer Use ... Model Context Protocol (MCP) ... Robotic Process Automation (RPA) ... LangChain
- Grok Bot | SpaceXAI ... Official product page, downloads, guides, and use cases. 
- Introducing Grok Bot | SpaceXAI ... August 11, 2026 launch announcement.
- Grok Bot Documentation | SpaceXAI Docs ... Official guides for Bots, collaboration, computer use, skills, routines, and teams.
- Approvals, security, and privacy | SpaceXAI Docs ... Approval controls, Auto Review, and the shared-computer boundary.
- Grok Bot Marketplace | SpaceXAI ... Plugins, connectors, and shared Bots.
- Introducing Grok 4.7 | SpaceXAI ... The model trained to natively understand the Grok Bot harness.
- Grok Bot for iPhone | App Store
- A Guide to Grok Bot: How xAI's Always-On AI Teammates Work | Harsh - Composio
- xAI opens Grok Bot to enterprises with new audit controls | Alexis Dufresne - AI Weekly

**Grok Bot** is SpaceXSI's product for persistent AI agents, which the company calls "AI teammates." SpaceXSI is the current name of SpaceX's AI unit, which was branded SpaceXAI from July 2026 and, before SpaceX acquired it in February 2026, operated as xAI. Each Bot is a named, always-on agent with a job description, its own memory, and access to a persistent cloud computer, so it can sign into the apps and websites a person already uses and carry multi-step work through to completion. Users message a Bot by typing, dictating, or starting a voice chat, the way they would text a colleague, and the Bot comes back only when it needs a decision or an approval. Four traits set a Bot apart from a conventional chat assistant:

- **Has its own computer:** Bots work on a cloud computer with a browser, files, and a terminal, hosted in Cursor's cloud.
- **Works in your tools:** Bots sign into existing apps and websites, including ones with no clean API or Model Context Protocol (MCP) server.
- **Keeps going:** Work continues around the clock, even when the user's laptop or phone is closed.
- **Behaves like a colleague:** A Bot keeps context between conversations, learns preferences over time, and coordinates with other Bots.

Grok Bot began as an internal prototype, developed under the code name "Sand," that spread across the company as teams built Bots for sales outreach, marketing campaigns, office operations, and bug fixes. It launched in early beta on August 11, 2026, as the first joint product of SpaceX's AI business and Cursor, the AI coding company whose $60 billion all-stock acquisition by SpaceX closed three days later. That lineage still shows: Grok Bot uses Cursor authentication and account data settings, and its cloud computers run in Cursor's cloud. SpaceXSI's Grok 4.7 large language model, released in September 2026, was trained to natively understand the Grok Bot harness, the software layer that turns model outputs into actions on a computer. Grok Bot differs from both chat assistants and traditional automation. A chat assistant such as Grok or ChatGPT answers questions and drafts text, while workflow tools such as Zapier and robotic process automation require someone to design the steps in advance and tend to break when a screen changes. Grok Bot instead takes a goal in plain language, chooses its own steps, and uses connectors where they exist and computer use (clicking and typing on its own screen) where they do not. It also sits beside two narrower SpaceXSI products: Grok Automations, a scheduler and trigger system inside the regular Grok app, and Grok Build, a terminal-based coding agent.


Grok Bot moves the AI agent out of the chat window and into the actual tools: named teammates with a computer of their own, who log in where you work, coordinate with each other, and finish the job instead of describing it.



### Bot Mechanisms

Every Bot on a user's account works on one persistent cloud computer that belongs to the account rather than to any single Bot. Each Bot gets its own screen, so several Bots can work in parallel, but a single Bot runs one computer-use task at a time. Isolation between different users is strict, while isolation between one user's Bots is deliberately loose, because shared resources are what let Bots pass work to each other. The split looks like this:

- **Shared across Bots:** files, browser sessions, app logins, and command-line credentials on the cloud computer, plus a few personal facts such as the user's name and time zone
- **Private to each Bot:** its role, stable preferences, conversation history, and summaries of earlier work

Context moves between Bots through shared files, direct messages, group chats of two to six Bots, and explicit handoffs of task ownership. A common pattern is a *chief of staff* Bot (coordinator, orchestrator) that coordinates specialist Bots so the human is not the middleman, an arrangement that resembles the multi-agent agentic loops used in research frameworks. Bots reach structured tools through connectors, plugins, and MCP servers installed from the Grok Bot Marketplace, and fall back to browser-based computer use when no connector exists. When a site needs a login, a Bot draws credentials from Grok Bot's Vault first, then from a connected password manager, then from a Secure Form, and values from Vault and Secure Forms are filled into the page without being shown to the model.
Repeatable work is captured in three related forms. SpaceXSI's documentation recommends getting a task right once, saving the method as a skill, and only then automating it. The building blocks for that progression are:

- **Skills:** reusable instructions that cover when to use a procedure, its inputs and required access, its steps, how to validate the result, and what needs approval; skills are shared across a user's Bots and can be published to a team
- **Routines:** instructions that tell one Bot when to run a workflow, either on a schedule or after an event such as a Slack message, a GitHub, Linear, Sentry, or PagerDuty event, an email to the Bot's own address, or a webhook call
- **Teach a task:** the user performs a browser workflow (up to ten minutes of recorded screen activity, with no microphone audio) while the Bot watches, and the Bot drafts a skill for review

Every proposed action then passes through approvals and Auto Review. Auto Review is on by default and checks commands, connected-app calls, and computer actions before they run; most proceed automatically, while riskier ones produce an approval card offering Allow once, Always allow, or Deny. Crucially, Auto Review considers only what the user asked for in their own messages, so instructions hidden in an email or web page do not count as a request, a design aimed squarely at prompt injection. Users can add up to 20 "Ask first" and 20 "Allow automatically" rules, and when both match, "Ask first" wins; cards raised by routines or other background work expire after 30 minutes, and emails drafted in the background wait as drafts until the user presses Send.

#### The Physics of Grok Bot

An always-on agent holding real credentials operates under hard constraints of trust, access, and capacity. These limits are not bugs waiting for a patch but consequences of letting software act as a person inside systems designed for people. SpaceXSI's own documentation spells out the main ones:

- **Security boundary:** All of a user's Bots share the same computer, so SpaceXSI states plainly that separate Bots are not separate security boundaries. A login created for a support Bot is available to a sales Bot on the same account.
- **Irreversibility:** An approval controls a proposed action but does not undo work already completed, and a routine's Test button performs real work.
- **Model judgment:** Auto Review is itself model-based, so SpaceXSI describes it as a complement to, not a replacement for, least-privilege setup and explicit approval boundaries.
- **Web friction:** Websites change layouts, expire sessions, demand CAPTCHAs, or block automation outright, which is why connectors are preferred over browser control when both exist.
- **Human-only steps:** Single sign-on, passkeys, CAPTCHAs, and bank verification require the user to take over the Bot's computer, finish the step, and hand control back. Hardware security keys work on macOS and Windows but not on Linux.
- **Capacity:** Each routine run consumes usage from a weekly allowance, schedules must be at least five minutes apart, and an account can run at most 300 routine runs per hour. Frequent schedules or busy Slack triggers can drain the allowance quickly, and failures caused by a shared rate limit are not retried automatically.

Taken together, these constraints push good practice toward narrow Bots with one main job, connectors scoped to read-only access where possible, and explicit stopping points for anything that sends, publishes, purchases, deletes, or changes production systems. They also explain why Grok Bot's interface centers on approval cards and run histories rather than on a single autonomous "go" button. The same trade-offs apply to rival agents, and they mirror long-standing cybersecurity guidance about scoped service accounts and separation of duties.

#### Why Grok Bot Matters

Grok Bot is SpaceXSI's bid to own the general-purpose office version of the AI agent, beyond coding. It entered a crowded field that includes Claude Cowork from Anthropic, dots from OpenAI, Gemini Task (formerly Spark) from Google, Muse from Meta, and Perplexity Computer, all of which give a model a cloud workspace and ask users to delegate tasks rather than write prompts. Several features set SpaceXSI's approach apart:

- **Work lands in the real tool:** SpaceXSI's internal examples include a sales Bot updating the CRM from call transcripts, an operations Bot processing invoices from Gmail, and an engineering Bot reproducing a bug, filing the ticket, and handing the fix to a debugging Bot. Its "Haggle Bot" procurement case study reported more than $100,000 in direct savings after mapping roughly 125 active vendors and preparing vendor negotiations, with people still approving every commitment.
- **Model and harness trained together:** Grok 4.7 was trained on the Grok Bot harness, so the model and the agent runtime are designed as a pair rather than bolted together.
- **A new route into business budgets:** Because Grok Bot runs on Cursor's infrastructure, it gives Cursor's developer customer base a path into sales, support, finance, and operations work, and The X Subscription now bundles Grok, Grok Bot, Cursor, and X Premium into one bill.
- **Distribution through X:** Users can hand a post to their Bot by tagging @bot on X, Bots can read and answer X Chat messages, and creators of popular public Bot templates can earn rewards paid through X Money.
- **Fast adoption:** Bloomberg, citing a company presentation, reported that Grok Bot reached 418,000 weekly users by September 14, 2026, up 24% from the prior week and about five weeks after launch. SpaceXSI has not disclosed how it counts a weekly user.

The product also raises questions that the broader AI governance debate has not settled. SpaceXSI's consumer terms make users responsible for what their agents do and generally cap the company's liability at the greater of fees paid or $100, yet Elon Musk has repeatedly promised on X that the company would "make you whole" if a Bot mishandled a bank account or a purchase. Retailers are also deciding whether to admit outside agents at all, and the outcome of that contest will shape what any shopping or errand-running Bot can actually do.

### Building Blocks

| Component | What It Does | Notes | 
|---|---|---|
| Bot | A persistent, named agent with one main job, a role description, and its own memory. | SpaceXSI recommends focused Bots, since a narrow role builds more useful context over time. | 
| Cloud computer | A persistent cloud computer with a browser, filesystem, and terminal, shared by all of a user's Bots; each Bot has its own screen. | Users can take control for sensitive steps. Running commands on the user's local computer is a separate permission, set to "Ask every time" by default and cappable by team admins. | 
| Group chat and handoffs | Two to six Bots coordinate in a thread, pass ownership, and pull in the human only for judgment calls. | In group chats only the requester can approve an action, and group chat cards don't offer Always allow. | 
| Plugins, connectors, and MCP | Structured access to apps through the Marketplace (Settings, then Plugins). | Preferred over browser control when available, because it is more reliable. Teams can configure plugins once for everyone. | 
| Vault and secure sign-in | Stores credentials and payment methods and fills them into pages without exposing them to the model. | Wallets such as Link and X Money can be connected; Link one-time cards are offered only in the US and Canada. | 
| Skills and routines | Saved procedures that run on demand, on a schedule, or after an event. | Up to 50 routines per Bot. "Teach a task" turns one watched demonstration into a draft skill. | 
| Approvals and Auto Review | Allow once, Always allow, or Deny for proposed actions. Auto Review screens tool calls and computer actions with "Ask first" and "Allow automatically" rules. | "Ask first" wins when rules conflict. Admins can require Auto Review, and team rules can only make behavior stricter. | 
| Bot email and @bot on X | Each Bot can claim an address at mail.grokbot.com, with plus aliases that route mail to separate routines. Tagging @bot on X forwards a post to the user's main Bot. | Agent email is off by default for Cursor teams until an admin enables it. Tagging isn't available in every region. | 
| Team Bots | One shared Bot that a whole team uses, with shared files, apps, plugins, credentials, and skills. | Public beta since September 28, 2026, on Teams and Enterprise plans. Each user keeps separate conversations and memories, and each Team Bot gets its own Slack handle. | 
| Enterprise controls | Access, network, and audit controls for governing Bots at scale. | Includes network allowlists, enforced Auto Review, audit logs, Action Recording, OpenTelemetry Export, and SCIM provisioning. | 

### Grok Automations

Grok Automations is the scheduling and trigger system built into the regular Grok assistant on grok.com and in the Grok apps for iOS and Android. The company, then branded SpaceXAI, launched it on July 16, 2026, describing automations as jobs Grok runs on its own: the user describes the work once, chooses when it runs, and Grok reports back. Automations is intentionally narrower than Grok Bot. It has no persistent computer or long-lived Bot identity, and every run is a fresh request that reuses the same instructions against current data, which makes it predictable and quick to set up but limits it to work that fits in a single conversation.

#### How Grok Automations Work

An automation's instructions read like any chat message. The user describes the job, attaches files for context, adds connectors and skills (typing @ mentions a connector so Grok uses it on every run), picks a mode, and then names and saves it. Automations can also be created straight from chat, so asking Grok to "check the news every morning and flag anything about pricing" sets one up, and the Automations page at grok.com/automations offers suggested templates. Any automation can be paused, resumed, edited, or deleted at any time, and four kinds of trigger can start a run:

- **Schedule:** once, daily, on weekdays, weekly, monthly, or yearly, at a chosen time in the user's time zone, such as a morning brief at 8:00 or a rent reminder on the first of the month
- **Email:** the automation watches the user's inbox and fires when an incoming message matches filters on sender, recipient, or subject, then works with that email as context and can respond to it
- **Connector events:** later releases added triggers tied to connected apps, such as a Linear trigger shown in August 2026 that starts work when an issue is assigned to the user; since early October 2026, the Add Trigger menu lists only triggers for connectors the user can actually use
- **Run now:** a manual run for testing an automation before relying on it

When an automation fires, Grok opens a real conversation, does the work, and saves the result to the automation's run history, where each run shows a status such as succeeded or upcoming. Users can open any run to read the full thread or continue the conversation from where Grok left off, and they choose whether results arrive by email, app notification, both, or neither. At launch, scheduled automations were available to everyone, while email triggers were reserved for SuperGrok subscribers. Plan names have since changed with the October 2026 launch of The X Subscription, and SpaceXSI has not published run quotas for automations. Automations grew out of Grok Tasks, a scheduling feature xAI added to Grok in mid-2025 that ran a saved prompt once or on a daily, weekly, monthly, or yearly cadence and delivered the results by notification or email. In June 2026, TestingCatalog spotted signs in a Grok build that xAI would retire standalone Tasks in favor of a broader automations system with skill and model selection, building on the reusable Skills packages xAI had introduced in mid-May. The shipped product kept the familiar schedules but added email triggers, connectors, skills, modes, and full conversational run histories, which places it in the same category as the scheduled tasks offered in ChatGPT and Claude. Its main differentiator at launch was reactive automation: a job that starts because a specific email arrived, not only because the clock reached a certain time.

#### Automations, Routines, and Workflows Compared

SpaceXSI now offers three ways to make Grok do work on its own, and they differ mainly in where the work runs and how much state carries from one run to the next. Grok Automations live inside the chat app, Grok Bot routines run on a Bot's persistent cloud computer, and Workflows in Grok Build orchestrate many parallel coding agents from the terminal. The table below summarizes the documented differences:

| Feature | Grok Automations | Grok Bot routines | Grok Build Workflows | 
|---|---|---|---|
| Where it runs | Grok on grok.com, iOS, and Android | A Bot's persistent cloud computer | The Grok Build terminal agent, inside a code repository | 
| What starts a run | A schedule, a matching email, a connector event, or Run now | A schedule; Slack, GitHub, Linear, Sentry, or PagerDuty events; email to the Bot's address; webhooks; or Test | A user request; Grok writes an orchestration script and runs it in the background | 
| State between runs | Each run is a fresh request; the full thread is saved in run history | The Bot's role, preferences, and work summaries, plus shared files and logins on the cloud computer | Progress is saved, so a paused workflow resumes without redoing finished work | 
| Scale | One prompt per run | Up to 50 routines per Bot and 300 runs per hour across all routines | A budget of 128 agents per run, rising to 1,024 for large jobs | 
| Reuse | Saved automations and suggested templates | Skills shared across Bots and teams | Saved workflows in .grok/workflows/ (team) or ~/.grok/workflows/ (personal) become slash commands | 
| Launched | July 16, 2026 | With Grok Bot, August 11, 2026 | July 23, 2026 | 

In practice, Automations suit a fixed brief such as "summarize my inbox every weekday at 8:00," routines suit jobs that need logins, files, approvals, or several apps, such as processing new invoices from Gmail and updating the accounting system, and Workflows suit large, parallel engineering or research jobs that benefit from built-in independent checking of results. The three can be combined: a Bot can turn a working one-off task into a skill and then a routine, while an automation in the Grok app handles lightweight monitoring that needs no computer of its own. Because every routine and automation run consumes usage, run frequency is a cost decision as well as a convenience decision.

### Getting Started and Limitations

#### Getting Started

Setup happens through conversation rather than a workflow builder. Most of the effort goes into choosing a clear job, connecting only the tools that job needs, and setting sensible limits. SpaceXSI's documentation suggests a sequence like this:

- **Access:** Since October 12, 2026, new customers get Grok Bot through The X Subscription, which bundles Grok, Grok Bot, Cursor, and X Premium benefits. Grok Bot is included in Premium ($30 a month), Super ($100), and Ultra ($200), but not in Plus ($8); the three eligible tiers offer the same features with progressively larger weekly allowances. Legacy paid Cursor plans, linked SuperGrok, SuperGrok Plus, SuperGrok Heavy, or X Premium+ subscriptions, and self-serve Grok Business seats also include access.
- **Usage:** Under The X Subscription, Grok Bot draws from one weekly pool shared with Grok and Cursor, replacing the separate Bot allowance offered at launch. Switching an existing plan to The X Subscription is optional but permanent.
- **Install:** Download the desktop app from`x.ai/bot` for macOS (Apple silicon or Intel), Windows (x64 or Arm64), or Linux (x64 or Arm64), and install the iPhone or Android app to reach the same Bots and threads on the go.
- **Create a Bot:** Choose a suggested role or Marketplace template, or define your own. Give it a short name, one main job, and a clear description of how it should work and what it must not do.
- **Hand off a task:** A good first request states the result you want, the sources it may use, its limits, the output format, and when you want to review.
- **Connect tools:** Open Settings, then Plugins, then Marketplace, to install connectors and MCP servers, and store logins in Vault rather than pasting passwords or one-time codes into chat. Connect only what a workflow needs.
- **Set guardrails:** Put explicit approval boundaries in requests for sending, publishing, purchases, deletions, permission changes, production changes, and accepting legal terms. The Auto Review switch lives under Settings, then General, then Bot, and a Permissions section in Settings lists custom rules.
- **Other entry points:** Tag @bot in a post on X, email the Bot at its own address, set Grok Bot as Android's default digital assistant, or map it to the iPhone Action Button.
- **Scale up:** Put several Bots in a group chat, or share a Bot with your team as a Team Bot.
- **Support:** Grok Bot support runs through Cursor at`hi@cursor.com` , with a Discord community and a status page linked from the documentation.

#### Known Limitations

- **Beta status:** Grok Bot is still labeled an early beta, and features vary by plan, region, and rollout.
- **Shared-computer risk:** Files, browser sessions, and credentials on the cloud computer are available to every Bot on the account. Deleting a Bot does not remove those files or sessions.
- **Sharing risk:** A public share link copies only a Bot's configuration, not its computer or logins. SpaceXSI still warns against putting secrets, customer data, or internal URLs in a shared Bot, and on a shared Bot the owner's apps, secrets, skills, and files are available to everyone who uses it.
- **Privacy settings:** Grok Bot requires data storage and does not support Cursor's Legacy Privacy Mode. Training opt-out follows Cursor account settings.
- **User liability:** Grok Bot's terms make users responsible for actions their agents take, including with third-party services such as financial institutions. Musk's public reimbursement pledges are posts on X, not a published reimbursement program.
- **Payment approvals:** The Vault documentation says a saved card fills in at checkout without a separate approval, while an October 26, 2026, changelog entry says purchases still need approval. Users who want to approve every payment should not save a card in Vault.
- **Model choice:** Launch coverage reported that Grok Bot runs only SpaceXSI's own Grok models, unlike Cursor, where third-party models such as Claude or GPT are available but billed separately at API prices.
- **Third-party sites:** Sites can refuse automated agents. In October 2026, users reported that Amazon was blocking Grok Bots from placing orders, though Amazon has not publicly confirmed a Grok-specific block.
- **Finance integration:** The read-only finance connection announced in September 2026 has rolled out unevenly, and Cursor staff on the product's support forum have described the Grok app's Plaid link and Grok Bot as separate integrations.

### Grok Bot History

- **Mid-2025:** xAI added Tasks to Grok, letting users schedule prompts that ran once or on a recurring cadence and delivered results by notification or email.
- **February 2, 2026:** SpaceX acquired xAI.
- **April 21, 2026:** SpaceX announced a deal giving it the right to acquire Cursor (Anysphere) for $60 billion later in 2026, or to pay $10 billion for the companies' joint work, which began with a partnership on model training.
- **May 2026:** xAI launched Grok Build, its terminal-based coding agent, in early beta, and introduced Skills in Grok as reusable packages of instructions, scripts, and resources.
- **June 16, 2026:** SpaceX signed a definitive agreement to acquire Anysphere in an all-stock deal valuing Cursor at $60 billion. The company account above records the same date as exercise of the April 21 option.
- **July 2026:** The company rebranded as SpaceXAI and released Grok 4.5 on July 8. It open-sourced Grok Build under the Apache 2.0 license on July 15 to 16, launched Automations in Grok on July 16, and added Workflows to Grok Build on July 23.
- **August 11, 2026:** SpaceXAI launched Grok Bot in early beta. Launch coverage reported initial access on top tiers such as SuperGrok Heavy, Cursor Ultra, and Cursor Teams Premium, on macOS, Windows, and iPhone.
- **August 12, 2026:** SpaceXAI released Grok 4.6, optimized for long-running agents, software development, and complex knowledge work.
- **August 14, 2026:** SpaceX closed its $60 billion all-stock acquisition of Anysphere, issuing about 389.3 million Class A shares and folding Cursor into SpaceXAI.
- **August 26, 2026:** Grok Bot expanded to more plans, including SuperGrok, Cursor Pro, and all Cursor Teams plans. The same day, Elon Musk publicly pledged that his company would "make you whole" if Grok Bot made a mistake for a user connecting it to their bank account.
- **September 3, 2026:** SpaceXAI launched Grok Bot for Enterprise.
  - **Controls:** access, network, and audit controls, with each user's work running in an isolated environment and network modes ranging from allow-all to allowlist-only
  - **Action Recording:** an Enterprise-only setting, off by default, with 90-day retention and shell commands scrubbed
  - **Monitoring:** SIEM-ready audit logs and OpenTelemetry export
  - **Free trial:** Grok and Cursor Enterprise customers got two weeks of free usage and could invite their whole organization
  - **Compliance:** Grok Bot falls within the scope of Anysphere's ISO/IEC 27001 and ISO/IEC 42001 certifications
- **September 4, 2026:** SpaceXAI opened a marketplace of reusable Bot templates and published a case study in which its "Haggle Bot," given access to vendor spend, contracts, and usage data, found more than $100,000 in direct procurement savings.
- **September 14, 2026:** Grok Bot reached 418,000 weekly users, up 24% week over week, according to a company presentation reported by Bloomberg on September 22.
- **September 21, 2026:** SpaceXAI released Grok 4.7, trained to natively understand the Grok Bot harness, with a 500,000-token context window and API pricing from $2 per million input tokens and $6 per million output tokens; rates double for prompts above 200,000 tokens. The flagship note in Recent Developments records that the short launch post stated the $2 and $6 headline rate, and that late-October 2026 price trackers listed cached input near $0.50 per million tokens and about $4 and $12 once a prompt passes 200,000 tokens.
- **September 25, 2026:** X began an invite-only Grok Bot Template Rewards pilot that pays creators every two weeks, through X Money, based on the quality and quantity of usage of their public Bot templates.
- **September 26, 2026:** Grok Bot announced a read-only Finance integration for bank, card, and investment accounts, aimed at analyzing spending and investments rather than moving money. Coverage reported that accounts connect through Plaid.
- **September 28, 2026:** SpaceXAI launched Team Bots in public beta on Teams and Enterprise plans, with pre-built Team Bots for customer accounts, product and engineering, marketing, and data analytics.
- **September 29, 2026:** The dot.com domain began redirecting to the Grok Bot page at x.ai/bot on the same day OpenAI unveiled its competing dots agent at DevDay.
- **October 4 to 10, 2026:** Musk said SpaceXAI would be renamed SpaceXSI, and by October 10 the company's official accounts displayed the name SpaceX Super Intelligence.
- **October 7 to 9, 2026:** Grok Bot added slide deck creation, the ability to search, read, and monitor X, Shopify capabilities, and email addresses for Bots at mail.grokbot.com.
- **October 12, 2026:** SpaceXSI launched The X Subscription, unifying Grok, Grok Bot, Cursor, and X Premium plans under one bill and a shared weekly usage pool.
- **October 15, 2026:** After users reported that Amazon was blocking Grok Bots from ordering, Musk told them to shop elsewhere, floated starting "a competitor to Amazon," and said SpaceX would make users whole if a Bot botched a purchase or leaked a card.
- **October 19 to 20, 2026:** Grok Bot gained one-press access through the iPhone Action Button, could be set as Android's default digital assistant, and began reading and replying to X Chat direct messages.
- **October 26, 2026:** Grok Bot began offering Link one-time payment cards in the US and Canada and added a Permissions section to Settings that lists custom approval rules.

## Chief of Staff Architecture

A *chief of staff* agent is a coordinator: it watches the sources that matter, decides what deserves attention, prepares work before you ask, and comes back only for decisions. Grok Bot is built for exactly this pattern. SpaceXSI's documentation describes a common setup in which a chief-of-staff Bot coordinates specialist Bots so the human isn't the middleman. Each Bot has a focused job, its own memory and a screen on a shared cloud computer, and Bots pass work to each other through shared files, direct messages, group chats and explicit handoffs.

| Layer | Grok Bot component | Role in the architecture | 
|---|---|---|
| Coordinator | A chief-of-staff Bot with a role description | Owns your priorities, routes work and escalates decisions | 
| Specialist staff | Focused Bots, for example inbox, research, sales follow-up or bug triage | Narrow roles build better context over time | 
| Coordination | Group chats of two to six Bots and explicit handoffs of task ownership | Bots pull you in only for judgment calls | 
| Intake channels | Typing, dictation or voice chat; each Bot's own email address at mail.grokbot.com; tagging @bot on X | Plus aliases can route mail to separate routines | 
| Sources of truth | Connectors, plugins and MCP servers from the Marketplace; browser sign-in when none exists | Structured access is preferred because it's more reliable | 
| Proactive loop | Routines on a schedule or after an event (Slack, GitHub, Linear, Sentry, PagerDuty, email or webhook) | Up to 50 routines per Bot | 
| Methods | Skills, including "Teach a task" demonstrations | Get it right once, save it as a skill, then automate it | 
| Approval gates | Auto Review plus Allow once, Always allow or Deny cards; up to 20 "Ask first" rules | "Ask first" wins conflicts; background drafts wait until you press Send | 

1. **Create the coordinator first:** one Bot named for the job, such as "Chief of Staff," with a short role description and your priorities.
2. **Add specialists gradually:** one Bot per recurring area, each with a single main job.
3. **Wire the handoffs:** put the coordinator and specialists in a group chat and tell the coordinator which Bot owns what.
4. **Capture methods as skills:** do the weekly report once with a Bot, then save it as a skill before scheduling it.
5. **Schedule routines:** a morning brief from the coordinator, and event routines for things like new support emails or failed builds.
6. **Tighten approvals:** add "Ask first" rules for external email, payments and anything touching customer data. In group chats only the requester can approve, and Always allow isn't offered.

CHIEF-OF-STAFF CHARTER (Grok Bot coordinator)
Role: Chief of Staff. You coordinate my Bots; you don't do their jobs.
Team: Inbox Bot (email triage), Research Bot (briefs), Sales Bot
(follow-ups). Route each request to the owning Bot and track it.
Every weekday at 8:00 send me one brief: decisions needed, deadlines
in 72 hours, and anything a Bot is blocked on.
Pull me in only for judgment calls, external commitments, or spending.
All outbound email stays as drafts until I press Send.

Isolation between one user's Bots is deliberately loose: they share files, browser sessions, app logins and command-line credentials on the same cloud computer. That's what makes handoffs work, but it also means a mistake or a manipulated instruction in one Bot can reach everything the account can reach. Keep sensitive logins in Vault, scope each Bot's job tightly, and use Team Bots with enterprise controls (enforced Auto Review, audit logs and network allowlists) for shared work.

## Computer Use Architecture

Computer use is central to Grok Bot rather than an add-on. Every Bot works on a persistent cloud computer with a browser, files and a terminal, hosted in Cursor's cloud, and signs in to apps and websites the way a person would, including ones with no API or MCP server. Grok 4.7 was trained to understand the Grok Bot harness natively.

| Layer | Grok Bot option | Design guidance | 
|---|---|---|
| Where it runs | One persistent cloud computer per account; each Bot gets its own screen | Several Bots can work in parallel, but each Bot runs one computer-use task at a time | 
| Local computer | Running commands on your own machine is a separate permission, "Ask every time" by default | Leave it at the default unless a task truly needs local files | 
| Preferred path | Marketplace connector, plugin or MCP first; browser computer use as the fallback | Connectors are more reliable than clicking | 
| Credentials | Vault first, then a connected password manager, then a Secure Form | Values are filled into pages without being shown to the model | 
| Supervision | Take control of the computer for sensitive steps | Step in for logins, payments and anything unusual | 
| Safety controls | Auto Review screens commands, app calls and computer actions; it counts only what you asked in your own messages | Instructions hidden in email or web pages don't count as requests | 

1. **Use connectors where they exist:** install them from the Marketplace before asking a Bot to click through a site.
2. **Store credentials properly:** put logins and payment methods in Vault rather than in chat.
3. **Teach the tricky flows:** record a browser workflow of up to ten minutes with "Teach a task," then review the draft skill.
4. **Keep local access off by default:** grant "run on my computer" only when needed, and let team admins cap it.
5. **Approve the risky steps:** add "Ask first" rules for purchases, sends and account changes, and remember that approval cards from background work expire after 30 minutes.
6. **Audit:** on Teams and Enterprise plans, use audit logs and Action Recording to review what Bots did.

Because every Bot on an account shares the same browser sessions and logins, treat the cloud computer like a shared office machine: sign in only to the accounts your Bots need, and review new skills before they run unattended.

## Grok Build

- Grok Build | Grok ... SpaceXSI ... Grok Bot ... Cursor ... Automations, Routines, and Workflows Compared
- AI Agent Advisor ... Ready-to-Use AI Agents ... AI Workflows ... Agent Builders ... Agent Development Frameworks ... AI App Builders ... AI Coding Agents ... AI Software Engineering ... Agent Catalog
- AI Software Engineering ... Model Context Protocol (MCP) ... Prompt Injection ... Cybersecurity ... AI Governance
- Grok Build | SpaceXSI ... Official product page and installer. 
- Introducing Grok Build | xAI ... May 25, 2026 post opening the beta to all SuperGrok and X Premium+ subscribers.
- Grok Build Documentation | SpaceXSI Docs ... Installation, plan mode, permissions, workflows, plugins, and enterprise deployment.
- xai-org/grok-build | GitHub ... Apache 2.0 source for the grok CLI, its terminal interface, and the agent runtime.
- xai-org/plugin-marketplace | GitHub ... The open catalog behind the built-in plugin marketplace.
- Benchmarking Grok 4.7 | Artificial Analysis ... Independent Coding Agent Index result for Grok 4.7 running in Grok Build.
- Grok Build Goes Open Source: Trust Repair in 72 Hours | Digital Applied ... The repository-upload incident and the open-source release.

**Grok Build** is SpaceXSI's coding agent: a full-screen, mouse-aware terminal program, started with the command `grok`, that reads a code repository, proposes a plan, edits files, runs shell commands, searches the web, and keeps long jobs running until they are finished. xAI opened it as an early beta on May 14, 2026, for SuperGrok Heavy subscribers paying $300 a month, and widened it on May 25 to all SuperGrok and X Premium+ subscribers. Version 1.0 shipped on August 7, the product reached every plan on the web and mobile on August 19, and since October 12, 2026, it has drawn on the weekly usage pool of The X Subscription, which it shares with Grok, Grok Bot, and Cursor. Grok Build is SpaceXSI's answer to Anthropic's Claude Code, OpenAI's Codex CLI, and Google's Gemini CLI, and it deliberately reads its rivals' configuration: AGENTS.md files (the convention Codex and others use), CLAUDE.md files, and the rule folders of Claude Code and Cursor are picked up without setup.
Grok Build pairs an open harness with closed models. The harness, roughly 845,000 lines of Rust covering the agent loop, tools, terminal interface, and extension system, has been public under the Apache 2.0 license since July 15, 2026. It was released three days after a researcher showed that the CLI had been uploading entire Git repositories to company cloud storage. The models are not open. Grok Build launched alongside a dedicated coding model, grok-build-0.1, made Grok 4.5 its default on July 8, and now defaults to Grok 4.7, with Cursor's Composer 2.5 and user-supplied model endpoints available from the model menu.
Grok Build is the narrowest of SpaceXSI's three ways to delegate work. Grok Bot gives persistent agents a shared cloud computer and a role, and Grok Automations runs scheduled prompts inside the chat app. Grok Build runs where the code lives, on a developer's machine or in a CI job, works on one repository at a time, and stops when the task is done.


Grok Build is SpaceXSI's bet that the harness matters as much as the model: an open-source terminal agent that reads every rival's configuration files, scales from one plan-and-approve session to a thousand parallel agents, and showed in public, in July 2026, that a coding agent's network traffic is part of its security boundary.



### Build Mechanisms

A Grok Build session is a loop. The harness assembles context, the model answers with text and tool calls, the harness runs those tools and returns the results, and the cycle repeats until the task is finished or a person is needed. The open-source tree splits this into an agent runtime, tool implementations (terminal, file editing, search), a workspace layer (filesystem, version control, execution, and checkpoints), and the terminal pager that renders plans, diffs, and scrollback. Several control layers sit around the loop:

- **Instructions and conventions:** At startup Grok Build reads project instructions from AGENTS.md and CLAUDE.md, rules from`.grok/` ,`.claude/` , and`.cursor/` folders, and any installed skills, plugins, hooks, and MCP servers. Running`grok inspect` prints everything the harness discovered in the current directory.
- **Plan mode:** For multi-step work the agent writes a plan that the user can approve, comment on step by step, or rewrite before anything runs. File edits stay blocked until the plan is approved, but shell commands still follow the session's permission mode, and subagents are not gated by the parent's plan.
- **Permissions:** Three modes govern tool calls: Ask, the default; Auto, where available; and Always-approve. An optional sandbox is off by default, and its network blocking works only on Linux.
- **Subagents and worktrees:** Larger tasks are handed to child agents with their own context. Each can run in its own Git worktree so that parallel edits do not overwrite one another.
- **Long-running modes:** The`/goal` command (June 22, 2026) keeps working until an objective is completed and verified, with status, pause, resume, and clear controls. Workflows (July 23, 2026) go further. Grok writes an orchestration script in the Rhai scripting language, runs it in the background across a budget of up to 128 agents (1,024 for large jobs), adds verification stages, and can save the script as a reusable slash command. A built-in`/deep-research` workflow ships with the product.
- **Memory:** Persistent project memory arrived on September 16, 2026. It is off by default and is browsed with`/memory` .
- **Surfaces:** The same agent runs interactively in the terminal, headless in scripts and CI (`grok -p` , with plain text, JSON, or streaming JSON output), and inside editors through the Agent Client Protocol (`grok agent stdio` ). The Agent Dashboard (June 15, 2026) shows every session on one screen and pulls those waiting for input to the top.

#### The Physics of Grok Build

A coding agent works with a developer's credentials inside a developer's files. Most of its constraints follow from that, and from the fact that every token it reads or writes is metered.

- **Context is finite:** grok-build-0.1 has a 256,000-token window and Grok 4.7 a 500,000-token window. Long sessions are compacted (older turns summarized) to fit, and anything summarized away is no longer seen verbatim, which is why durable instructions belong in AGENTS.md or memory rather than early chat turns.
- **Reasoning is billed as output:** Artificial Analysis measured Grok 4.7 at its highest effort using about 81,000 output tokens per Intelligence Index task, against about 36,000 for Grok 4.6 at high effort. The`/effort` setting is therefore a cost control as much as a quality control, and`/usage` shows what a session has consumed.
- **Parallelism multiplies cost, not certainty:** Ten agents cost roughly ten times the tokens of one. Worktrees prevent file collisions, not logical conflicts between two agents' changes. As The Physics section shows for majority voting, more attempts help only when something can reliably tell good results from bad ones, which is why Workflows add verifier stages and why xAI's own reference workflow uses a handful of agents (a planner, one to four researchers, one or two verifiers, and a writer) rather than hundreds.
- **"Verified" means what the tests say:**`/goal` stops when the agent judges the objective complete and checked. In a repository with thin tests, that judgment is only as good as the checks it can run.
- **The harness is the security boundary:** Anything the agent reads can be sent to an inference service, and the July 2026 incident showed that a harness can also send what it never read. Text inside a file, issue, or web page can carry prompt injection aimed at an agent that holds shell access. Permission modes, deny rules, sandboxing, scoped credentials, and logging are the controls.
- **Usage pools are opaque:** SpaceXSI expresses plan allowances as multiples of the cheapest paid tier (Premium 4 times Plus, Super 15 times, Ultra 50 times) and does not publish the pool size in tokens or hours, so the only reliable budget is measured usage.

#### Why Grok Build Matters

- **SpaceXSI's entry into developer tools:** Grok Build is the company's first serious coding product, launched after Musk acknowledged that xAI lagged rivals in coding. It entered a market where Claude Code, Codex, and Cursor already had large followings.
- **An independent benchmark position:** On Artificial Analysis' Coding Agent Index, which tests each model in its own harness, Grok 4.7 running in Grok Build scored 56 at launch, nine points above Grok 4.6. That placed it fourth, ahead of GPT-5.6 Sol and behind Claude Fable 5.1, GPT-6 Astra, and Claude Opus 5.
- **Open harness, closed model, closed process:** Like Codex CLI and Gemini CLI, the harness is Apache 2.0 and can be audited, forked, and pointed at local or third-party models. Unlike them, it is publish-only: issues are disabled, external pull requests are refused, the public history began as one squashed commit, and later commits are machine-generated syncs from SpaceXSI's internal monorepo. Some tool code was vendored, with notices, from OpenAI's Codex and from opencode.
- **A training and distribution loop:** Grok 4.5 shipped first in Grok Build, Cursor, and the API, and Grok 4.7's faster serving tier is offered only in Grok Build and Cursor. Coding sessions in SpaceXSI's own tools feed the company's view of how its models fail on real work.
- **Compatibility as strategy:** Reading Claude Code and Cursor configuration with no setup, importing Claude Code sessions (`grok import` ), and accepting OpenAI-compatible or Anthropic Messages endpoints lower switching costs in both directions. Community developers have used the Agent Client Protocol to build unofficial VS Code and desktop front ends.
- **A governance case study:** The repository-upload episode is one of the clearest public examples of telemetry in an AI coding agent exceeding what users understood, and of a vendor using an open-source release to rebuild trust.

### Grok Build Components

| Component | What It Does | Notes | 
|---|---|---|
| grok CLI and TUI | A full-screen, mouse-interactive terminal agent that plans, edits, runs commands, and shows inline diffs for review. | macOS, Linux, and Windows. Installs by script, npm, or Homebrew. The installer also adds an `agent` command that can shadow Cursor CLI's command of the same name. | 
| Models | Grok 4.7 by default; grok-build-0.1; Cursor's Composer 2.5; custom OpenAI-compatible or Anthropic Messages endpoints in `~/.grok/config.toml` . | API rates: grok-4.7 at $2 and $6 per million input and output tokens; grok-build-0.1 at $1 and $2 with a 256,000-token window. Grok 4.7 Fast costs twice the standard rate. | 
| Plan mode and permissions | Plan, comment, and approve before edits; Ask, Auto, or Always-approve for tool calls. | Plan mode blocks edits, not shell commands. Add deny rules before using Always-approve. | 
| Subagents and worktrees | Parallel child agents with separate context, optionally each in its own Git worktree. | Worktree management guards against deleting active worktrees (October 2026). | 
| /goal | Long-running autonomous execution until an objective is completed and verified. | `/goal status` ,`pause` ,`resume` , and`clear` . | 
| Workflows | Model-written orchestration scripts that fan work out across parallel agents, verify results, and report back. | Budget of 128 agents per run, up to 1,024 for large jobs. Saved in `.grok/workflows/` (team) or`~/.grok/workflows/` (personal) as slash commands. | 
| Agent Dashboard | One screen for every session, sorted by state, with blocked sessions first. | `grok dashboard` from the shell, or`/dashboard` (Ctrl+\) inside a session. Closing it leaves sessions running. | 
| Plugins, skills, hooks, and MCP | Installable packages that bundle skills, slash commands, agents, hooks, MCP servers, and language servers. | `/marketplace` or`grok plugin install <name> --trust` . Launch plugins included MongoDB, Vercel, Sentry, Chrome DevTools, Cloudflare, and Superpowers. Anyone can propose a plugin by pull request to the catalog. | 
| Memory | Project notes that persist across sessions. | Off by default since its September 16, 2026, debut. | 
| Headless and ACP | Scripted runs for CI, and an Agent Client Protocol server for editors. | No official VS Code extension; community front ends exist. | 
| Build Mode | A no-code app builder inside the Grok web, iOS, and Android apps that publishes apps to shareable links. | Reached every plan on August 19, 2026. Apps can be exported to GitHub. | 
| Enterprise | Identity-provider sign-in, admin-pinned settings, and team-level zero data retention. | Enterprise installs through npm avoid direct downloads from x.ai and Google Cloud Storage. | 

### The Repository-Upload Incident

On July 12, 2026, an AI-safety researcher publishing as Cereblab released a wire-level traffic analysis of Grok Build CLI version 0.2.93. It showed the CLI sending the contents of files it read, unredacted, to a Google Cloud Storage bucket used by SpaceXAI, and also packaging entire repositories as Git bundles, with full commit histories that could include secrets deleted months earlier. In one test a prompt that asked only for the reply "OK," with instructions to open no files, still uploaded the whole repository. Another user reported that a home directory containing SSH keys and password-manager databases had been uploaded. The in-app `/privacy` command, which the company first pointed users to, controlled data retention rather than the upload itself. What stopped the whole-repository transfer was a server-side flag, `disable_codebase_upload`, pushed without a client update or changelog entry, and retests on July 13 found no further uploads.
SpaceXAI said zero-data-retention customers had retained nothing and turned retention off by default for all Grok Build users from July 12. Musk pledged that data uploaded before the change would be deleted, while also urging users to keep sharing some data to help debugging. On July 15 the company published the harness on GitHub under Apache 2.0, reset usage limits, and said the harness could now run local-first against inference the user chooses. Reviewers who read the code found the upload modules still present but disabled, with the central upload function returning a hard-coded error. No outside party has been able to verify that previously collected data was deleted.
The episode shows a general point about agent governance: a privacy setting in the interface is only as meaningful as the network behavior behind it, and only traffic captures or source code can confirm that behavior. It is also why this page treats source availability, retention defaults, and the upload path as separate questions.

### Grok Build Setup and Limitations

#### Grok Build Setup

- **Access:** Four routes exist. The free tier has limited, unpublished usage; paid tiers of The X Subscription (Plus $8, Premium $30, Super $100, and Ultra $200 a month) draw on the weekly pool shared with Grok, Grok Bot, and Grok models in Cursor; an API key bills per token, which suits CI; and Enterprise adds single sign-on and zero data retention. Legacy SuperGrok and X Premium+ plans continue to work.
- **Install:** On macOS, Linux, or WSL run`curl -fsSL https://x.ai/cli/install.sh | bash` ; on Windows PowerShell run`irm https://x.ai/cli/install.ps1 | iex` . The first launch opens a browser to sign in, and`grok --version` confirms the build.
- **Start small:** Run`grok` inside a repository, ask it to explain how the code is organized, then use plan mode for the first real change and read the diff before keeping it.
- **Bring existing conventions:** Keep AGENTS.md or CLAUDE.md files current, and use`grok import` to bring over Claude Code sessions. Skills use SKILL.md files, and`/skillify` turns a working session into a new skill.
- **Set guardrails:** Stay in Ask mode until the agent's habits are known. Before using Always-approve, add deny rules for commands such as recursive deletes or pushes to the main branch, and turn on the sandbox where it is supported.
- **Check data settings:** Confirm retention with`/privacy` , and set`DISABLE_TELEMETRY` or`DISABLE_ERROR_REPORTING` to turn off telemetry and crash reports. Teams with sensitive code should use an enterprise zero-data-retention arrangement.
- **Scale up deliberately:** Move from one session to subagents, then`/goal` , then Workflows, and use`grok dashboard` to watch them.

#### Grok Build Limitations

- **Trust record:** The July 2026 uploads happened while users believed a privacy setting protected them, and the promised deletion cannot be independently confirmed.
- **Publish-only open source:** The code is inspectable and forkable, but bugs cannot be filed publicly and outside patches are not accepted. Security reports go through HackerOne.
- **Opaque usage:** No plan publishes its allowance, and Grok 4.7 at high effort is token-hungry, so heavy users should measure before committing.
- **Weak default isolation:** The sandbox is off by default, network blocking is Linux-only, and Windows source builds are described as best-effort.
- **Fast-moving releases:** Version 1.0.47 shipped on October 1, 2026, and version 1.0.70 on October 23, with several releases on some days. CI jobs should pin a version, which the installer supports through a`GROK_VERSION` variable.
- **Command collisions:** Because the installer places its own`agent` command early on the PATH, developers who also use Cursor's CLI should call it as`cursor-agent` or check which binary runs.
- **Regional gaps:** Some models reached the EU later than other regions; Grok 4.5 was initially unavailable there.

### Grok Build History

- **May 14, 2026:** xAI opened Grok Build as an early beta for SuperGrok Heavy subscribers, describing it as a coding agent and CLI for professional software engineering. Musk asked publicly for beta feedback.
- **May 25, 2026:** Access widened to all SuperGrok and X Premium+ subscribers, with plan mode, parallel subagents, worktree support, AGENTS.md, plugins, hooks, skills, and MCP, plus a Windows PowerShell installer.
- **Late May 2026:** grok-build-0.1, a fast coding model with a 256,000-token window, became available on the xAI API, and requests for the older grok-code-fast-1 were routed to it.
- **Early June 2026:** Cursor's Composer 2.5 became selectable from the Grok Build model menu.
- **June 11, 2026:** The Grok Build Plugin Marketplace launched with MongoDB, Vercel, Sentry, Chrome DevTools, Cloudflare, and Superpowers plugins, backed by an open catalog on GitHub.
- **June 15, 2026:** The Agent Dashboard added a single view of every running session.
- **June 22, 2026:**`/goal` introduced long-running autonomous execution.
- **July 8, 2026:** Grok 4.5, co-developed with Cursor, became the default model, with free Grok 4.5 usage in Grok Build for a limited time.
- **July 12 to 13, 2026:** Cereblab disclosed the repository uploads; retention was turned off by default, a server-side flag stopped the uploads, and Musk pledged deletion.
- **July 15, 2026:** SpaceXAI open-sourced the Grok Build harness under Apache 2.0 and reset usage limits.
- **July 23, 2026:** Workflows added background orchestration across up to 1,024 agents.
- **August 7, 2026:** Grok Build 1.0 shipped.
- **August 12, 2026:** Grok 4.6 became available in Grok Build.
- **August 19, 2026:** Grok Build reached every plan, on the web and on mobile.
- **September 16, 2026:** Experimental project memory arrived, off by default.
- **September 20 to 21, 2026:** Grok Build 1.0.40 added Grok 4.7, and Artificial Analysis scored the pair 56 on its Coding Agent Index.
- **October 12, 2026:** Grok Build moved under The X Subscription and its shared weekly usage pool.
- **October 2026:** Near-daily releases (versions 1.0.47 through 1.0.70) added fuzzy session search, session resume from any folder, memory ranking, consistent permission prompts, and protection for active worktrees.

## Cursor

- Cursor | Grok ... SpaceXSI ... Grok Build ... Grok Bot
- AI Agent Advisor ... Ready-to-Use AI Agents ... AI Coding Agents ... Agent Development Frameworks
- AI Software Engineering ... Claude | Anthropic ... ChatGPT | OpenAI ... Gemini | Google ... Microsoft
- Cursor ... Official site, downloads, pricing, and documentation. 
- Cursor Changelog ... Dated release notes for the editor, agents, cloud agents, Origin, and models.
- Cursor is now part of SpaceX | Cursor ... August 14, 2026 post announcing the close of the acquisition.
- Compile | Cursor ... Keynote and session recordings from Cursor's first conference, June 2026.
- Privacy and data | Cursor Help ... Privacy Mode, training, and how it applies to Grok models and Grok Bot.
- Cursor (company) | Wikipedia

**Cursor** is an AI code editor and agent platform made by Anysphere, Inc., the San Francisco company that four MIT students (Michael Truell, Sualeh Asif, Arvid Lunnemark, and Aman Sanger) founded in 2022. It launched in 2023 as a fork of Microsoft's open-source Visual Studio Code with AI completion and chat built in, and became one of the fastest-growing software businesses on record. Its annualized revenue passed $100 million in January 2025 and $1 billion in November 2025, and a $4 billion run rate was reported in early June 2026. More than half of the Fortune 500 use it. On April 21, 2026, SpaceX secured the right to buy Cursor for $60 billion or to pay $10 billion for joint work. It exercised that right on June 16 and closed the all-stock deal on August 14, 2026, issuing 389,289,254 Class A shares. Cursor is now a wholly owned SpaceX subsidiary inside SpaceXSI, and Truell remains its chief executive while also serving as a SpaceX executive.
For the Grok story, Cursor plays three roles. It is a **training partner**: Grok 4.5 (July 8, 2026) was co-developed with Cursor and trained partly on Cursor data, and Grok 4.5, 4.6, and 4.7 were each available in Cursor at launch. It is **infrastructure**: Grok Bot's cloud computers run in Cursor's cloud, and Grok Bot uses Cursor accounts, privacy settings, and support. And it is a **sales channel**: since October 12, 2026, new customers buy Cursor only through The X Subscription, where Grok models are included and third-party models such as Claude and Gemini are billed at API list prices. The same change of ownership led OpenAI to announce on August 28, 2026, that it would wind down its supply of models to Cursor, proposing November 12, 2026, as the end date.


Cursor turned the code editor into a control room for AI agents. Its sale to SpaceX made it something more: the place where SpaceXSI's models are trained on real work, tested by developers, and sold, at some cost to the model neutrality that made Cursor popular.



### Cursor Mechanisms

Cursor layers several kinds of model over a VS Code-derived editor, and over three years the center of gravity moved from suggesting the next line to managing fleets of agents.

- **Tab completion:** A low-latency model predicts the next edit, often across several lines, as the developer types. Cursor strengthened this layer by acquiring the completion startup Supermaven in November 2024.
- **Codebase context:** Cursor indexes a repository so that chat and agents can search it by meaning, and it reads project rules and AGENTS.md files so that conventions travel with the code.
- **Agents in the editor:** Agent mode plans multi-file changes, edits code, runs terminal commands, drives a browser, and calls MCP tools. Cursor 2.0 (October 29, 2025) let several agents run in parallel using Git worktrees or remote machines. Cursor 3 (April 2, 2026) added the Agents Window, a separate workspace that manages local, SSH, worktree, and cloud agents side by side, plus Design Mode for pointing agents at specific interface elements.
- **Cloud agents:** Agents can run in Cursor-hosted environments that keep going after the laptop closes and hand work back to a local session. Through 2026 these gained development environments (May), pre-built environments that start three times faster (August), self-hosting inside a customer's own infrastructure (September), and interactive, shareable canvases plus an Environments API with up to 300 repositories per environment (October 16).
- **Models:** Cursor routes requests among its own Composer models, SpaceXSI's Grok models, and third-party models, and an automatic router, reworked in July 2026, picks a model by task. The Composer line runs from Composer (October 2025) through Composer 1.5, Composer 2 (March 19, 2026), and Composer 2.5 (May 18, 2026). Composer 2 and 2.5 start from Moonshot AI's open-weights Kimi K2.5 and add Cursor's own continued pretraining and reinforcement learning on coding tasks.
- **Review and operations:** Bugbot (July 2025) reviews pull requests, Graphite (acquired from December 2025) brings stacked pull-request review, Automations start agents from events such as alerts, pull requests, or Slack messages, and Rollouts (September 2026) watches deployments for regressions.
- **Origin:** Announced at Compile on June 16, 2026, Origin is an agent-first Git hosting service. Repository hosting entered early beta on August 17, synchronized with GitHub, and the Origin App Marketplace opened on October 13 with more than 20 integrations, including Cloudflare, Linear, and Sentry.
- **Surfaces:** Cursor runs as a desktop editor, in the browser, from the command line (the Cursor CLI, August 2025), through Python and TypeScript SDKs, and on iPhone, where a beta app opened at Compile and, from October 6, 2026, can monitor and steer agents running on the developer's own computer.

#### The Economics of Cursor

Cursor's position, and SpaceX's reasons for buying it, follow from a few numbers.
**1. Run rate is not revenue.** Annualized revenue multiplies a recent month by twelve, so a $4 billion run rate means about $333 million in a month, unaudited. At $60 billion, SpaceX paid about 15 times the June 2026 run rate, or about 10 times the $6 billion run rate Anysphere forecast for the end of 2026.
**2. Owning the model is the margin.** A tool that resells other labs' models pays close to their list prices. Cursor priced Composer 2 at $0.50 per million input tokens and $2.50 per million output tokens, about one tenth of the $5 and $25 list price of Claude Opus 4.6 that it compared against. Cheaper in-house models, trained on cheap compute, are how an editor improves gross margin, and coverage of the deal described SpaceX's compute as partly a solution to that problem.
**3. The bundle changes the price of neutrality.** SpaceXSI's own comparison put SuperGrok, Cursor Pro, and X Premium+ bought separately at $90 a month, against $30 for the Premium tier of The X Subscription, and the company says every new plan includes at least twice the Grok coding usage of the comparable old Cursor plan. On the new plans, though, Claude, GPT, and Gemini usage is billed at API list price from the first request, so a developer who mostly used Claude inside a $20 Cursor Pro plan can pay more after switching.
**4. Supplier exposure is small in volume and large in signal.** Truell said OpenAI models carried about 5 percent of Cursor's traffic when OpenAI announced its withdrawal. Losing them is a modest operational change, but it showed that a multi-model editor owned by one lab depends on rival labs' goodwill. Anthropic responded by saying it would add compute for Claude in Cursor.
**5. Growth and share can move in opposite directions.** Ramp's corporate-spending data showed Cursor's share of AI coding-tool spending falling from about 41 percent in June 2025 to about 26 percent in May 2026, while its run rate grew roughly eightfold over the same year. The market grew faster than Cursor did, as Claude Code, Codex, and others took spending.

#### Why Cursor Matters

- **For SpaceXSI's business:** Cursor's reported run rate alone was larger than xAI's reported revenue for all of 2025 (about $3.2 billion), so the deal put developer subscriptions at the center of SpaceX's AI business, alongside renting out Colossus.
- **For model training:** Coding sessions are rich training signal: plans, edits, corrections, accepted and rejected changes. Both companies have said Grok 4.5 was trained partly on Cursor data. Cursor states that Privacy Mode keeps code out of training for every model, Grok included, and that with Privacy Mode off its policy allows code data to be used to train models.
- **For developers:** Cursor's appeal was a single editor that let developers pick the best model of the day from any lab. Under SpaceX that promise is narrower: OpenAI is leaving, the cheapest new tier is Grok-only, and third-party models cost extra. How much choice survives is the central question for Cursor's users.
- **For the shape of software work:** Cursor's path from autocomplete to the Agents Window, cloud agents, Automations, and Origin tracks the industry's move from AI that helps a programmer type to agents that open pull requests on their own. Origin puts SpaceXSI in competition with Microsoft's GitHub as well as with Claude Code, Codex, GitHub Copilot, and Google's coding agents.
- **For transparency norms:** The Composer 2 episode, in which Cursor did not disclose its open-weights base model until a developer found the model ID, made base-model disclosure an expectation for commercial coding models.

### Cursor Components

| Component | What It Does | Notes | 
|---|---|---|
| Editor | A VS Code-derived desktop editor with AI built into editing, chat, and review. | macOS, Windows, and Linux. Imports VS Code settings and extensions. | 
| Tab | Low-latency, multi-line next-edit prediction. | Builds on the 2024 Supermaven acquisition. | 
| Agents Window | Cursor 3's agent-first workspace for running and reviewing many agents across projects and environments. | Opened from the command palette. Includes a diff and pull-request view, tiled layouts, and voice input. | 
| Cloud agents | Agents in Cursor-hosted or self-hosted environments that keep running without the local machine. | Shareable canvases and an Environments API beta from October 16, 2026. Grok Bot's cloud computers run on the same cloud. | 
| Models | Composer 2.5 (Standard and Fast), Grok models including Grok 4.7 and its fast tier, and third-party models from Anthropic, Google, and OpenAI. | OpenAI proposed November 12, 2026, as the end of its supply. On The X Subscription, third-party models are billed at API list price and the Plus tier is Grok-only. | 
| Bugbot, Graphite, and Rollouts | Automated pull-request review, stacked-change review, and deployment monitoring. | Graphite was acquired in a deal agreed in December 2025. | 
| Automations | Agents started by events such as alerts, pull requests, or Slack messages. | Billed at cloud-agent rates. | 
| Plugins, MCP, and marketplaces | Installable plugins and MCP servers, with team marketplaces for sharing approved tools. | Marketplace introduced with Cursor 3; team marketplaces gained MCPs and organizations in June 2026. | 
| Origin | Agent-first Git hosting and an app marketplace for agent integrations. | Early beta; synchronizes with GitHub. More than 20 integrations at the October 13, 2026, marketplace launch. | 
| CLI, SDK, web, and mobile | Agents from the terminal, from code, in the browser, and on iPhone. | The CLI installs an `agent` command, which Grok Build's installer can shadow;`cursor-agent` is the alternative name. | 
| Privacy Mode | Keeps code from being used for training by Cursor or model providers. | On by default for Enterprise and enforceable by team admins. Applies to Grok models. Legacy Privacy Mode, which stores nothing, blocks Grok Bot. | 
| Plans | Sold through The X Subscription since October 12, 2026: Free, Plus ($8), Premium ($30), Super ($100), Ultra ($200), and Teams from $30 a seat. | Legacy Pro, Pro+, Ultra, and Teams plans keep working. Switching is optional and permanent. | 

### Cursor Under SpaceX

#### From Partner to Subsidiary

Cursor raised money quickly before SpaceX arrived: an $8 million seed round led by the OpenAI Startup Fund in October 2023, a $60 million Series A at a $400 million valuation in 2024, a $900 million Series C at $9.9 billion in June 2025, and a $2.3 billion Series D at $29.3 billion in November 2025, co-led by Accel and Coatue with Nvidia and Google participating. Reports in spring 2026 described talks at a valuation near $50 billion. Instead, on April 21, 2026, SpaceX announced a partnership to train models on SpaceXAI's Colossus infrastructure, together with an option to buy Cursor for $60 billion later in the year or pay $10 billion for the joint work. SpaceX listed on Nasdaq on June 12, and on June 16, the day of Cursor's first Compile conference, it said it would exercise the option in an all-stock deal. The transaction closed on August 14, 2026, which made it the largest acquisition of a venture-backed startup on record. Cursor's closing post emphasized access to SpaceX's GPU fleet and the promise of stronger, cheaper models, presented Grok 4.6 as an early result, and announced no leadership changes.

#### Models: Composer, Grok, and the Compile Model

Cursor launched Composer 2 on March 19, 2026, promoting it as frontier-level at a fraction of rivals' prices. Within hours a developer spotted a model identifier showing it was built on Moonshot AI's Kimi K2.5. Cursor's Lee Robinson confirmed the base, saying about a quarter of the final model's compute came from it and the rest from Cursor's training, and co-founder Aman Sanger called the omission a mistake. Moonshot's official account congratulated Cursor, which largely settled the licensing question but not the disclosure one. Composer 2.5 followed on May 18, 2026, on the same base with about 25 times more synthetic training tasks and reinforcement learning that uses written feedback. Cursor said it was already training a much larger model from scratch with SpaceXAI, using about ten times the compute. At Compile, Truell said that model, with more than 1.5 trillion parameters, was being pretrained from scratch on more than 100,000 GPUs on Colossus and was meant for knowledge work beyond coding. Three weeks later the companies released Grok 4.5, which Musk described as built on a 1.5-trillion-parameter "V9" foundation model with Cursor data added in supplemental training. Coverage treated it as the first model of the joint effort. Grok 4.5 was available in Cursor on all plans from launch day, Grok 4.6 followed on August 12, and Grok 4.7's fast serving tier on September 21 was offered only in Cursor and Grok Build. Neither company has published what categories of Cursor data went into training, which is why Cursor's Privacy Mode guarantees, and whether a given user had it switched on, matter.

#### OpenAI's Withdrawal

On August 28, 2026, OpenAI said it had notified SpaceX that it would wind down the contract supplying its models to Cursor, using a right triggered by the change of ownership. It proposed November 12, 2026, as the end of a transition period, described that as the maximum notice the contract allowed, and said it would not provide future models, naming its upcoming Astra model. OpenAI said it could not be confident SpaceX would keep its technology within OpenAI's terms of service, pointing to past contract breaches by X and xAI. Truell replied that OpenAI models served about 5 percent of Cursor traffic and that Cursor was talking with OpenAI, and Musk dismissed the move. Hours later Anthropic co-founder Tom Brown said Anthropic would keep adding compute to support Claude in Cursor. In mid-October OpenAI's help center still described November 12 as a proposed date that the two companies had not yet confirmed, and GPT-5.6 remained selectable in Cursor.

#### The X Subscription

On October 12, 2026, SpaceXSI moved Cursor onto The X Subscription, one set of plans for Grok, Grok Bot, Cursor, and X Premium. New customers can no longer buy the old $20 Pro, $60 Pro+, or $200 Ultra plans. Cursor's own mapping puts Pro next to Premium ($30), Pro+ next to Super ($100), and Ultra next to the new Ultra ($200). The Plus tier ($8) covers Grok models only and is not sold to teams, and team seats now start at $30 instead of $40. Grok and Cursor draw on one weekly usage pool, sized as multiples of Plus (4 times for Premium, 15 times for Super, 50 times for Ultra). On the new plans third-party models are paid separately at API list prices. Existing Cursor subscribers keep their plans, including bundled third-party usage, unless they switch, and a switch cannot be undone.

### Cursor Controversies and Limitations

- **Support hallucination:** In April 2025 Cursor's AI support agent invented a nonexistent login policy, prompting cancellations, an apology, and refunds.
- **Pricing reversals:** A mid-2025 move from request counts to usage-based limits on the Pro plan surprised users with charges. Cursor apologized, adjusted limits, and refunded affected users. The October 2026 move to The X Subscription again changed what a subscription buys.
- **Agent security:** In 2025 researchers disclosed CurXecute (CVE-2025-54135), in which a prompt injection relayed through an MCP server could rewrite Cursor's MCP configuration and run code, and MCPoison (CVE-2025-54136), in which an approved MCP configuration could be silently altered later. Both were fixed in version 1.3 on July 29, 2025, and Cursor now asks for approval whenever MCP configuration changes.
- **Disclosure:** The Composer 2 launch left out its Kimi K2.5 base until users found it.
- **Model neutrality:** OpenAI's withdrawal, the Grok-only Plus tier, and API-priced third-party usage mean Cursor no longer treats all labs' models alike.
- **Training data questions:** Grok 4.5 was trained partly on Cursor data, but the data's composition and any opt-outs before August 2026 have not been published.
- **Coupling with Grok Bot:** Grok Bot uses Cursor accounts and requires data storage, so it does not work under Cursor's Legacy Privacy Mode.

### Cursor History

- **2022:** Michael Truell, Sualeh Asif, Arvid Lunnemark, and Aman Sanger founded Anysphere while at MIT.
- **2023:** Cursor launched as an AI-first fork of VS Code. In October, Anysphere announced an $8 million seed round led by the OpenAI Startup Fund.
- **2024:** A $60 million Series A valued the company at $400 million. In November Cursor acquired Supermaven, and investors were reported to value it at about $2.5 billion.
- **January 2025:** Annualized revenue passed $100 million.
- **April 2025:** Cursor's AI support agent invented a login policy, an early public case of a customer-facing AI hallucination with business consequences.
- **June 2025:** A $900 million Series C at a $9.9 billion valuation; annualized revenue above $500 million; agents on web and mobile.
- **July 2025:** Bugbot launched, a pricing change was rolled back with refunds, and version 1.3 fixed the CurXecute and MCPoison vulnerabilities.
- **August 7, 2025:** The Cursor CLI was released.
- **October 29, 2025:** Cursor 2.0 introduced Composer, the company's first in-house agentic coding model, and parallel agents. Cloud agents followed.
- **November 13, 2025:** A $2.3 billion Series D at a $29.3 billion valuation; annualized revenue passed $1 billion.
- **December 2025:** Cursor agreed to acquire Graphite, a code-review startup.
- **February to March 2026:** Annualized revenue reached about $2 billion. Composer 2 launched on March 19, and Cursor acknowledged its Kimi K2.5 base the next day.
- **April 2, 2026:** Cursor 3 introduced the Agents Window, Design Mode, and a plugin Marketplace.
- **April 21, 2026:** SpaceX announced a training partnership on Colossus and the option to buy Cursor for $60 billion or pay $10 billion for joint work.
- **May 18 to 21, 2026:** Composer 2.5 launched, and Bloomberg reported a $3 billion run rate.
- **June 16, 2026:** SpaceX said it would exercise the option. At Compile, Cursor announced Origin, an iPhone beta, and a 1.5-trillion-parameter model being trained from scratch on Colossus. A $4 billion run rate was reported in early June.
- **July 8, 2026:** Grok 4.5, co-developed with Cursor, launched in Cursor on all plans.
- **August 11, 2026:** Grok Bot launched on Cursor's cloud as the first joint SpaceXAI and Cursor product.
- **August 14, 2026:** The acquisition closed, and Cursor became a wholly owned SpaceX subsidiary.
- **August 17 to 19, 2026:** Origin repository hosting entered early beta, and agents could pick up tasks from events such as pull requests and Slack messages.
- **August 28, 2026:** OpenAI announced it would wind down model supply to Cursor, proposing November 12 as the end date; Anthropic said it would add compute for Claude in Cursor.
- **September 2026:** Self-hosted agents (September 2), Grok 4.7, whose fast tier was offered only in Cursor and Grok Build (September 21), and Rollouts deployment monitoring (September 23).
- **October 2026:** iPhone remote control of local agents (October 6), The X Subscription (October 12), the Origin App Marketplace (October 13), and cloud-agent canvases with an Environments API (October 16).
- **November 12, 2026:** OpenAI's proposed date for ending direct access to its models in Cursor.

## Further Resources

### References

**Primary sources from xAI / SpaceXSI**

- News: Research, Product and Company Updates | SpaceXSI The company's official chronology of model releases, products, funding, and partnerships from 2023 to 2026. 
- Open Release of Grok-1 | xAI, March 2024 Architecture summary and license for the 314B mixture-of-experts open-weight model. 
- Grok 4 | xAI, July 2025 Launch post for Grok 4, Grok 4 Heavy, and the SuperGrok Heavy tier. 
- Grok 4.1 | xAI, November 2025 Release notes with the company's hallucination and leaderboard claims. 
- Introducing Grok 4.7 | SpaceXSI, September 21, 2026 Primary launch post: training changes, the seven-row comparison table, headline pricing, the fast tier, and safeguard claims. 
- Grok models and pricing | SpaceXSI docs API facts for grok-4.7: 500,000-token context, $2 and $6 per million tokens, May 2026 knowledge cutoff, and the requirement that search tools be enabled for live events. 
- Grok 4.7 model card | Cursor The longer card commentators compared with the blog, including eval definitions and any figure that differs from the launch table. 
- Introducing Grok Voice 3.0 | SpaceXSI, October 2026 Voice model launch citing Artificial Analysis' independent speech-to-speech ranking. 
- Colossus | SpaceXSI Build timeline and system-level specifications of the Memphis supercomputer. 
- New Compute Partnership with Anthropic | SpaceXSI, May 2026 Terms of Anthropic's access to Colossus 1 and interest in orbital compute. 
- Grok system prompts | xAI on GitHub Published system prompts, a key resource for studying how instructions shape Grok's behavior. 

**Corporate and financial**

- Why SpaceX Acquired xAI | Sacra Analysis of the February 2026 merger terms and the orbital data center rationale. 
- Form S-1, Space Exploration Technologies Corp. | US SEC, May 2026 SpaceX's IPO registration, including disclosures about its AI business. 
- SpaceX Says It Can Buy Cursor for $60 Billion | CNBC, April 2026 Terms of the Cursor option later exercised. 
- Google to Pay SpaceX $920 Million a Month for Compute | CNBC, June 2026 Colossus capacity sold to a direct competitor. 
- Musk's xAI Is Starting Over Again | TechCrunch, March 2026 Reporting on the post-merger restructuring and founder departures. 

**Behavior, bias, and benchmarks**

- Did xAI Lie About Grok 3's Benchmarks? | TechCrunch, February 2025 The consensus@64 comparison dispute. 
- How Elon Musk Is Remaking Grok in His Image | The New York Times, September 2025 Analysis of thousands of responses showing a rightward shift after prompt changes. 
- Musk's Latest Grok Chatbot Searches for His Views Before Answering | Associated Press, July 2025 Documentation of Grok 4 consulting Musk's posts. 
- X Takes Grok Offline, Changes System Prompts | TechCrunch, July 2025 The "MechaHitler" incident and response. 
- What to Know About Grok 4 Fast | VentureBeat, September 2025 Independent efficiency analysis citing Artificial Analysis and Ethan Mollick. 
- Grok 4.5 Price Versus Benchmark Gaps | The Decoder, July 2026 Independent read of Grok 4.5's price-performance position. 
- Musk on Grok 4.7 and Opus 5.5 | MadRobot, September 26, 2026 Reports the interview in which Musk called 4.7 a workhorse and said it was behind Opus 5.5. The quotation is secondhand. The ranking agrees with the published tables. 
- Musk Seemingly Admits xAI Has Used OpenAI's Models to Train Its Own | Wired, April 2026 Distillation testimony in Musk v. Altman. 
- Our Decision on Cursor Following Its Acquisition by SpaceX | OpenAI, August 2026 OpenAI's stated reasons for cutting off Cursor. 
- Grok Build Caught Sending Entire Repos to the Cloud | The Register, July 2026 The Grok Build data-upload incident. 

**Deepfakes, regulation, and law**

- Grok Generated Thousands of Undressed Images Per Hour on X | Bloomberg, January 2026 Quantitative estimate of the scale of abuse. 
- European Commission Investigates Grok and X Recommender Systems | Society for Computers and Law, January 2026 Scope of the Digital Services Act proceedings. 
- Online Safety Regulators Investigate X Over Grok AI Images | RPC, March 2026 Legal analysis of the Ofcom and EU investigations and penalty ranges. 
- Statement on Investigation Into Grok and Sexualized Deepfakes | Privacy Commissioner of Canada, June 2026 One of the first formal regulatory findings against Grok. 
- Grok Is Still Hosting Sexualized Deepfakes of Famous Women | Wired, June 2026 Evidence that safeguards remained incomplete months later. 

**Government, defense, and environment**

- Department of War Launches Grok for Government on GenAI.mil | US Department of Defense, August 2026 Accreditation and deployment details. 
- NAACP Sues Musk's xAI Over Gas Turbines | Reuters, April 2026 Clean Air Act lawsuit over Colossus 2. 
- Pollution From xAI's Unpermitted Power Project | Reuters, July 2026 Investigation of 59 unpermitted turbines and community impact. 

**Grok Bot, automations, and workflows**

- Grok Bot | SpaceXAI Official product page, downloads, guides, and use cases. 
- Introducing Grok Bot | SpaceXAI August 11, 2026 launch announcement. 
- Grok Bot Documentation | SpaceXAI Docs Official guides for Bots, collaboration, computer use, skills, routines, and teams. 
- Approvals, security, and privacy | SpaceXAI Docs Approval controls, Auto Review, and the shared-computer boundary. 
- Grok Bot Marketplace | SpaceXAI Plugins, connectors, and shared Bots. 
- Grok Bot for iPhone | App Store 
- Automations in Grok | SpaceXSI July 16, 2026, launch post describing schedules, email triggers, connectors and skills, run histories, notifications, templates, and plan availability; the primary source for the Grok Automations section. 
- Skills and routines | SpaceXSI Docs Official guide to skills, Teach a task, routine triggers (schedules, Slack, GitHub, Linear, Sentry, PagerDuty, email, and webhooks), limits of 50 routines per Bot and 300 runs per hour, and management. 
- Team Bots: shared AI teammates that learn as they work | SpaceXSI September 28, 2026, announcement of shared Team Bots, pre-built templates, Slack integration, and customer results from Harper and Amplitude. 
- Grok Bot for Enterprise | SpaceXSI September 3, 2026, release adding access, network, and audit controls and a two-week free trial for Grok and Cursor Enterprise customers. 
- Setting Grok Bot loose on procurement | SpaceXSI September 4, 2026, case study of Haggle Bot, an internal procurement Bot that found more than $100,000 in direct savings with humans approving commitments. 
- Workflows in Grok Build | SpaceXSI July 23, 2026, announcement of orchestration scripts that fan a task out across many parallel agents, verify results, and report back in one background run. 
- Grok Bot on The X Subscription | SpaceXSI Docs Confirms that Premium, Super, and Ultra include Grok Bot, that Plus does not, and that usage draws from a weekly pool shared across Grok, Grok Bot, and Cursor. 
- Tag @bot on X | SpaceXSI Docs How tagging @bot on X forwards a post to a user's main Bot, the setup it requires, and why tagged requests can fail. 
- Grok Bot Changelog | SpaceXSI Dated release notes for Grok Bot, including Team Bot, email, voice, and payment changes through late October 2026. 
- Grok Bot Template Rewards Pilot Program | X Help Center Official terms of the invite-only creator rewards pilot, paid every two weeks through X Money. 
- xAI is working on Automations feature for Grok | Alexey Shabanov - TestingCatalog June 15, 2026, report on plans to fold Grok Tasks into a broader automations system with skill and model selection. 
- Grok Bot vs Automations vs Build: Key Differences | Kingy AI Comparison of SpaceXSI's persistent teammate, in-app scheduler, and terminal coding agent, refreshed August 22, 2026. 
- Grok Bot: SpaceXAI launches AI Agent to Counter Claude Cowork's Success | Trending Topics Launch coverage noting the internal code name "Sand," the initial top-tier access, and Grok Bot's status as the first joint SpaceXAI and Cursor product. 
- SpaceX Completes $60 Billion Cursor Acquisition, the Largest Startup Deal on Record | Michelle Hawley - VKTR August 14, 2026, report on the close of the all-stock Anysphere deal and its strategic rationale. 
- SpaceXAI's Grok Bot tops 400K weekly users one month after launch | Crypto Briefing Summary of Bloomberg's report that Grok Bot reached 418,000 weekly users as of September 14, 2026. 
- Grok Puts the AI Assistant Between Consumers and Their Banks | PYMNTS Coverage of the September 26, 2026, Finance integration, describing read-only account connections through Plaid. 
- Elon Musk's AI Grok Bot can now handle banking while your Tesla FSD handles the road | Teslarati Reporting on Musk's "make you whole" pledges and how they compare with the liability cap in SpaceXAI's consumer terms. 
- Musk renames SpaceXAI to SpaceXSI after Trump's 'super intelligence' push | Khaleej Times October 2026 report on the rename of SpaceX's AI unit. 
- OpenAI Launched AI Agent Named Dot, But Dot.Com Redirects To Rival Grok Bot | OfficeChai Coverage of the dot.com redirect on the day of OpenAI's DevDay launch. 
- Should Start Amazon Competitor, Says Elon Musk After Amazon Blocks Grok Bot On Its Platform | OfficeChai Report on user complaints that Amazon blocked Grok Bot orders and Musk's response. 
- Grok Bot Can Now Handle Your X DMs and Become Your Android Assistant | Tesla North October 20, 2026, roundup of X Chat DM handling, the Android default-assistant option, and email plus aliases. 
- A deep dive into Grok Bot | Flavio Copes Independent guide checked October 29, 2026, covering setup, Vault payments, approvals, and recent changelog items. 
- A Guide to Grok Bot: How xAI's Always-On AI Teammates Work | Harsh - Composio 
- xAI opens Grok Bot to enterprises with new audit controls | Alexis Dufresne - AI Weekly 

**Grok Build**

- Introducing Grok Build | xAI, May 25, 2026 Announcement opening the beta to all SuperGrok and X Premium+ subscribers, with plan mode, subagents, and worktree support. 
- Musk's xAI Unveils First Coding Agent in Bid to Rival Anthropic | Bloomberg, May 14, 2026 Report on the initial SuperGrok Heavy beta. 
- Grok Build Documentation | SpaceXSI Docs Official guides to installation, permissions, plan mode, workflows, plugins, and enterprise deployment. 
- Grok Build Plugin Marketplace | xAI, June 11, 2026 Launch of the built-in plugin catalog and its open submission process. 
- Agent Dashboard in Grok Build | xAI, June 15, 2026 One-screen management of parallel coding sessions. 
- Introducing /goal | xAI, June 22, 2026 Long-running autonomous execution until a task is completed and verified. 
- xai-org/grok-build | GitHub The Apache 2.0 Rust source of the grok CLI, terminal UI, and agent runtime, synced periodically from SpaceXSI's monorepo. 
- Grok Build Goes Open Source: Trust Repair in 72 Hours | Digital Applied, July 16, 2026 Timeline of the upload disclosure and the open-source release, with a reading of the repository. 
- SpaceXAI Open-Sources Grok Build After Users Found It Secretly Uploaded Their Codebases | Markus Kasanmascheff - WinBuzzer, July 18, 2026 Server-side flag changes, retests, and the limits of the public history. 
- Grok Build Is Open Source, but It Accepts No Contributions | Pasquale Pillitteri, July 2026 Comparison with Codex CLI, Gemini CLI, and Claude Code repositories, and the vendored code. 
- xAI Grok Build Workflows Make Orchestration the Product | Rohit Ramachandran - RohitAI, July 24, 2026 Independent analysis of workflow scripts, agent budgets, and verification stages. 
- Benchmarking Grok 4.7 | Artificial Analysis, September 21, 2026 Coding Agent Index score of 56 for Grok 4.7 in Grok Build, its ranking, and token usage. 
- Grok Build: what it is, pricing, and how to use it | Lindy Independent guide checked September 30, 2026, covering access routes, models, features, and limits. 
- Grok Build Review | Vibecoding.app Independent review explaining the shared weekly usage pool under The X Subscription, October 2026. 
- What's New in Grok Build | Havoptic Release tracker with dated notes for each Grok Build version. 

**Cursor**

- Cursor Is Now Part of SpaceX | Cursor, August 14, 2026 Cursor's announcement of the closed acquisition. 
- SpaceX Officially Closes Its Cursor Acquisition | TechCrunch, August 15, 2026 Deal terms and the share issuance. 
- Cursor (company) | Wikipedia Founding, funding rounds, acquisitions, and controversies. 
- Cursor Hits $3 Billion Annual Sales Rate Ahead of SpaceX Deal | Bloomberg, May 21, 2026 Revenue run rate and the $6 billion year-end forecast. 
- Cursor's GitHub Rival Origin and New SpaceX Model Raise Code Custody Stakes | Tech Times, June 24, 2026 Compile announcements, the from-scratch model, and Ramp market-share data. 
- Cursor 3: A New Era of AI Coding with Parallel Agents | DataCamp, April 2026 Guide to the Agents Window and Design Mode. 
- Cursor's Composer 2 Was Built on a Chinese AI Model | VentureBeat, March 2026 The Kimi K2.5 base-model disclosure. 
- Introducing Composer 2.5 | Cursor, May 18, 2026 Training changes and the announcement of a larger model trained with SpaceXAI. 
- OpenAI to Stop Supplying Models to Cursor After SpaceX Acquisition | The Next Web, August 29, 2026 OpenAI's wind-down, the November 12 date, and Grok 4.5's availability in Cursor. 
- OpenAI to Cut Off AI Models for Cursor | iTnews, August 2026 Truell's 5 percent traffic figure, Musk's response, and Anthropic's commitment to add compute for Claude in Cursor. 
- Privacy and Data | Cursor Help Privacy Mode, its application to Grok models, and its relationship to Grok Bot. 
- Cursor and The X Subscription: What Changed | T-Minus AI, October 23, 2026 Plan tiers, third-party model pricing, and legacy plans. 
- Cursor Pricing | Christopher Alarcon, October 18, 2026 Cursor's legacy-to-new plan mapping and the state of OpenAI models in Cursor in mid-October. 
- Cloud Agent Canvases and Environment Improvements | Cursor Changelog, October 16, 2026 Shareable canvases, the Environments API beta, and larger environments. 
- Cursor AI Code Editor Fixed Flaw Allowing Attackers to Run Commands via Prompt Injection | The Hacker News, August 2025 The CurXecute vulnerability and its fix in version 1.3. 

### Additional Viewing

**Elon Musk: Digital Superintelligence, Multiplanetary Life, How to Be Useful** (Y Combinator AI Startup School, June 2025, about 45 minutes). In a fireside chat with Garry Tan, Musk explains how xAI built Colossus, why he believes compute and energy are the main constraints, and why he thinks a "maximally truth-seeking" AI is the key safety principle. Useful for understanding the thinking behind Grok's design and its compute-first strategy; the "Building up xAI" segment begins around the 27-minute mark.

**Elon Musk: War, AI, Aliens, Politics, Physics, Video Games, and Humanity** (Lex Fridman Podcast #400, November 2023, about 2 hours 15 minutes). Recorded the week Grok was unveiled, the conversation includes a dedicated segment on xAI and Grok, its intended humor and "rebellious streak," truth-seeking as a design goal, compute efficiency, AI regulation, and open-sourcing. It is a valuable record of Grok's original stated goals against which its later evolution can be compared.

**Grok 4.7 System Card** (Vaundros, September 22, 2026, about 14 minutes). A page-by-page reading of the Grok 4.7 model card, including rows the launch post left out and the EEBench mismatch between the blog and the card. The second half shifts to an unrelated model and can be skipped. Use this with the benchmark section, and check the card before repeating a percentage from the narration.

**Grok 4.7: I Read the Full Model Card So You Don't Have To** (DeepOnAI, September 22, 2026, about 11 minutes). A shorter, more skeptical pass over the same PDF, aimed at the gap between "strongest refusals" in the post and the card's mixed refusal tables. It is commentary, not a replication.

**Grok 4.7 launches at $2/$6** (September 22, 2026). Walks the official four-column table and the price comparison without treating a win on one row as a win on all of them. Pair it with the September 21 launch post so a rounded number in the narration does not replace the printed score.

**Introducing Grok Bot** is the official launch video, published on the Grok channel on August 11, 2026. In under two minutes it shows Bots signing into existing tools, working across apps and inboxes on their own computers, and returning only for approvals, with examples drawn from the internal prototype's use in sales outreach, marketing, office operations, and bug fixes. It is a compact visual companion to the Grok Bot section. Watch on YouTube

**Grok Bot Workshop: Attention Lists, Agent Teams & Cloud Agents** features SpaceXAI product managers, including Kevin Niparko, explaining how they use Grok Bot not as one chatbot but as a team of always-on agents that research, prioritize, analyze data, and ship. That team-of-agents approach is the practical side of the coordination model in Bot Mechanisms, where specialist Bots share context and hand work to one another. Watch on YouTube

**Grok Bot for Enterprise Teams: Put AI teammates to work across every department** is a workshop led by SpaceXSI senior field engineer Santi Garza and AI deployment manager Anahita Sahu. They show how enterprise teams give real work to Bots across departments, which connects directly to the Team Bots and Enterprise controls rows of the Building Blocks table and to the September 2026 entries in Grok Bot History. Watch on YouTube

**Every Grok Bot Concept Explained for Normal People** is an explainer from the Nate Herk | AI Automation channel, published August 31, 2026. It walks through 25 concepts to understand before building a team of autonomous agents, including memory, computing environments, skills, routines, webhooks, and handoffs between agents, along with ways to monitor their work. It maps closely onto the Building Blocks table and the comparison of routines with Grok Automations. The same video id is embedded, untitled, near the top of this page.

**How to Use Grok Bot Better Than 99% of People (FULL COURSE)** is a course of roughly 50 minutes by Brock Mesarich of the AI for Non Techies channel, published September 8, 2026. It moves through seven levels of use, from basic messaging to building Bots that work together as a team while the user is away. It is a practical complement to Getting Started for readers who learn best by watching a full setup.

**I Put Grok Build to the Test** (OrcDev, May 19, 2026). A hands-on walkthrough recorded in the first week of the beta: installation, the interactive terminal interface, plan mode, skills, plugins, subagents, and headless scripting. It shows the product before the July upload incident, the open-source release, Workflows, and Grok 4.7, so pair it with the Grok Build section and its history for what changed afterward.

**Opening Keynote, Michael Truell | Compile 26** (Cursor, published June 22, 2026, more than 20 minutes). Recorded at Cursor's first conference on June 16, the day SpaceX said it would buy the company. Truell traces Cursor from autocomplete to agents, and he and colleagues including Kevin Niparko and Graphite co-founder Tomas Reimers present cloud-agent updates, Cursor Mobile, Origin, and the model being trained from scratch on Colossus. It is the primary source for the Compile announcements in the Cursor section.

**Cursor CEO: Going Beyond Code, Superintelligent AI Agents, And Why Taste Still Matters** (Y Combinator, about 37 minutes). Garry Tan interviews Michael Truell from the period when Cursor was valued at about $9 billion. Truell explains why Cursor built its own editor and models and why he expects programming to move toward describing intent. It is useful background on the product philosophy that SpaceX bought, recorded before the acquisition and the shift to Grok-first plans.
