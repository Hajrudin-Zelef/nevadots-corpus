---
id: collect-261121-veille-ia/claude-anthropic/ai-weekly-gpt-6-1-terra-agent-skills-and-anthropic-s-s-1
title: "AI Weekly: GPT-6.1 Terra, Agent Skills and Anthropic's S-1"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["terra", "gpt-6", "openai", "tokens", "model", "input", "output", "agent"]
source: https://dev.to/alexmercedcoder/ai-weekly-gpt-61-terra-agent-skills-and-anthropics-s-1-16k5
source_anchor: ""
source_lines: [1, 321]
section: "Claude_Anthropic"
---
**Week of October 21-28, 2026**

OpenAI shipped a cheaper mid-tier model built for computer use. Anthropic handed Agent Skills to a neutral foundation and then filed to go public. Its S-1 put real numbers on what frontier AI costs to run.

Coding agents also kept moving approval decisions from people to classifiers. Here is what changed, what it costs and what you need to do about it.

This was a mid-tier week, not a flagship week. OpenAI released GPT-6.1 Terra, Moonshot shipped Kimi K3.1 Pro, and MiniMax posted open weights. Anthropic released no new public model. It did describe an unreleased research model that built working exploit chains, and it launched an agent app on top of Opus 5.5.

OpenAI released GPT-6.1 Terra on October 26. The API model ID is `gpt-6.1-terra`, and it is the only snapshot so far. OpenAI pitches it for visual reasoning, computer use and clear writing. It sits between GPT-6 Luna below and GPT-6.1 Sol and GPT-6 Astra above.

The model card lists these specs:

- 
**Context window:** 1,050,000 tokens, with a max input of 922,000 tokens
- 
**Max output:** 128,000 tokens
- 
**Knowledge cutoff:** June 1, 2026
- 
**Input:** text and image.**Output:** text only
- 
**Reasoning effort:** none, low, medium (default), high, xhigh and max
- 
**Endpoints:** Chat Completions, Responses and Batch. Tool calling needs the Responses API

Pricing is the headline. Standard rates are $1 per million input tokens and $5 per million output tokens. Cached input costs $0.05 per million, and cache writes with 30-minute retention cost $1.25 per million. OpenAI says that is 50% below GPT-5.6 Terra and one-tenth the per-token price of GPT-6 Astra.

Read the fine print on long prompts. Prompts over 272,000 input tokens cost 2x on input and cache rates and 1.5x on output. Batch and Flex run 50% below Standard. Regional processing adds 10%, and fast mode costs 2x Standard.

The benchmark numbers all come from OpenAI, so treat them as vendor-reported. Decrypt reports that Terra solved 50.3% of Terminal-Bench 4.0 tasks at max effort, up from 21.5% for GPT-5.6 Terra. On an internal test of prompts built to trick models, the factual error rate at low effort fell from 53.6% to 16.7%. At max effort the error rate dropped to 4.3%. OpenAI also says Terra matches GPT-6 Astra on Agents' Last Exam and finishes tasks more than 3x faster than Astra and Sol on average. OpenAI posted no numeric score for that Agents' Last Exam claim.

Terra is live in ChatGPT Work and Codex for Plus, Pro, Business, Enterprise and Edu users, per the Codex changelog. It is not available in Chat, and it does not support UAE inference residency. GitHub added Terra to Copilot the same day.

One practical detail came from the developer forum, not OpenAI. A user reports that `reasoning.effort: "none"` on Terra accepts `temperature` and `top_p`, which GPT-6.1 Sol blocked. That is unconfirmed, so test it before you rely on it.

What does Terra change? It makes computer-use agents cheaper to run at volume. A model that reads screens and fills forms at $1 in and $5 out lets you put an agent on back-office work that did not pencil out at Sol prices. OpenAI itself still points heavy research and complex coding to Sol or Astra.

Moonshot released Kimi K3.1 Pro on October 23. The quickstart guide lists two model IDs: `kimi-k3.1-pro` and `kimi-k3.1-pro-highspeed`. Moonshot says the highspeed variant runs 2-3x faster. Context is 1M tokens. Output defaults to 131,072 tokens and can go as high as 1,048,576.

Reasoning is always on. You pick `low`, `high` or `max` through a top-level `reasoning_effort` field, and `max` is the default. You cannot turn thinking off. The model takes image and video input and supports tool calling and strict JSON Schema output.

Moonshot's guide says pricing is flat, with no tiering by context length. Its pricing page did not render the K3 table when we checked. Simon Willison reported $2 per million input tokens and $10 per million output tokens. Confirm those numbers in the Kimi console before you budget against them.

The benchmark story is messy. On launch week, Moonshot's own quickstart still showed placeholder values ("xx.x%") for Terminal-Bench 4.0 and Terminal-Bench-Science. The one hard number in the guide is a finance hallucination test. Moonshot says its claim-level hallucination rate fell from 23.35% on K3 to 6.85% on K3.1 Pro. That figure is vendor-reported, and Moonshot calls the test a stress test, not a general error rate.

Secondary coverage cites a Moonshot figure of 65.7% on Terminal-Bench 4.0, against 66.4% for Claude Opus 5.5. We did not find that number on a Moonshot page, so treat it as unverified. Press reports also put K3.1 Pro at 55 on the Artificial Analysis Intelligence Index.

Moonshot says full weights will ship by November 4. It has not named a license. If you plan to self-host, wait for the license file before you commit.

Last week we noted that MiniMax promised weights for M3.1 Flash. They landed. The repo `MiniMaxAI/MiniMax-M3.1-Flash` is public and ungated on Hugging Face. Coverage puts the model at 428 billion total parameters with 23 billion active per token, a 1M context and text, image and video input. Hugging Face lists about 220 billion stored parameters in the safetensors files, so check the model card for how MiniMax counts.

The license is not Apache or MIT. Hugging Face tags it as "other." Coverage describes a community license that requires written permission from MiniMax for products above $20 million in yearly revenue. One write-up also says commercial products must display "Built with MiniMax M3.1-Flash." Read the license file in the repo before you ship anything on it. MiniMax recommends SGLang, vLLM and TokenSpeed for serving.

Anthropic shipped no new public model this week. It did three things that matter for model users.

First, Anthropic introduced Claude Duet on October 21. Duet is a proactive agent app built on Claude Opus 5.5. We cover it in the Tooling section below.

Second, Claude Models on Google Cloud (Anthropic managed) went live on October 22. Anthropic runs the Claude API, and you reach it through your Google Cloud project. Google Cloud IAM controls access, and usage lands on your Google Cloud bill and draws down existing spend commitments. The release notes confirm fast mode for Opus 5.5 is available there too. The older Vertex AI offering is now called Claude Models on Google Cloud (Google managed). Pick that one if you need Google as the data processor or strict regional controls.

Third, Anthropic published a research post on October 27 about an unreleased internal research model. Anthropic says the model found and chained flaws in Chrome, Safari, Android, Windows and Linux into proof-of-concept exploits. Google assigned 62 Chrome CVEs, all rated critical or high, including 13 sandbox escapes. On Linux, Anthropic says the model took 48 minutes to leak a kernel pointer and about five more minutes to reach shell execution, through CVE-2026-98432.

These claims come from Anthropic's own account. The Hacker News notes that it is hard to tell how much of each chain the model built alone. All flaws were reported and patched. Anthropic did not say whether it will release the model. It did point to its Cyber Verification Program, which gives vetted security teams access to stronger cyber capabilities.

The lesson for everyone else is simple. Patch cycles need to get shorter. If a model can turn four Chrome bugs into a sandbox escape in a lab, attackers will try the same thing.

Google DeepMind introduced Genie 4 on October 27. It is a real-time world model that generates audio, ambient sound and dialogue along with the scene. You steer it with natural language and keyboard controls. Google uses Gemini 3.8 Flash as a guiding agent to keep scenes coherent. Google gave no resolution, frame rate or session length. It says details "can still shift or drift over time." Genie 4 is a research preview for trusted testers, with Google AI Ultra and Gemini API early access to follow.

Two quieter changes in the Gemini API changelog matter more for data teams. On October 26, Google turned on request storage by default for new paid-tier projects. Calls to `generateContent`, `streamGenerateContent` and `batchGenerateContent` now default to `store=true`. Existing projects keep their current setting. The changelog does not say how long Google keeps stored requests.

If you send customer data or regulated data through new Gemini projects, set `store=false` per request or turn off logging in the Logs and Datasets settings. Do it before your first production call, not after.

On October 27, Google released the Gemini API CLI (`gemini-api`) in beta. It is a stateless, zero-dependency command-line client for coding agents, scripts and CI pipelines. It works with the Interactions API and can run Gemini managed agents.

The OpenAI developer changelog had two smaller entries. On October 21, OpenAI added settings to restrict which service tiers each project or a whole organization can use. That helps you stop a test project from burning money on a priority tier.

On October 22, the Decisions API reached general availability on `gpt-6-luna`. Each request now handles up to 400 questions, up from 200 in beta. Shared input can be cached across requests for prompt-cache discounts, and you can pass structured data with `input_json`.

We found no new model from Meta, xAI, Mistral, DeepSeek or Qwen dated in this window. DeepSeek's news page has nothing after its October 17 Flash update. xAI's release notes show only the Grok Voice 3.0 items we covered last week. Mistral's Large 4 weights deadline of October 31 is still ahead.

The community filled some of the gap. ISTA-DASLab posted GGUF quantizations of DeepSeek-V4.1-Flash-1017, including a 2-bit build. Press reports say it runs on a single 12GB GPU. We did not test that claim.

The price floor keeps dropping in the middle of the market. A frontier lab now sells a 1M-context computer-use model at $1 in and $5 out. Open weights keep arriving, but with custom licenses that cap revenue or require sign-off. And defaults keep shifting under you, like Gemini's new storage setting. Read changelogs every week, not every quarter.

The tooling theme this week was who says yes. GitHub made LLM-judged approvals generally available. Anthropic launched an agent that asks before it pays or signs. Claude Code and Codex both tightened permission defaults. The common pattern is a classifier that approves safe actions and stops for a human on the risky ones.

GitHub made assisted permissions generally available in Copilot CLI, the Copilot app and VS Code. When Copilot wants to run a command, touch a file path or open a URL, an LLM judges the request. Requests it rates "clearly safe" run on their own. Everything else waits for you.

Copilot now has three approval modes:

- 
**Manual:** always ask
- 
**Assisted:** the classifier approves safe requests
- 
**Allow All:** approve everything

You turn it on with `/permissions` in the CLI, the Tool permissions picker in the app or the permissions picker in VS Code. GitHub did not publish the classifier's criteria.

The admin controls are the important part. Enterprise settings can block Allow All while keeping Assisted. Admins can also disable Assisted, set the default mode for new sessions and fall back to Manual when the default is not allowed. Platform teams finally get a middle path between "ask every time" and "approve everything."

On October 27, Copilot added server-to-server authentication in public preview. Agents built with the Copilot SDK can now call Copilot for an organization without a signed-in user. You grant a GitHub App the read-only `Copilot Requests` permission, install it on the org and pass its short-lived installation token to the SDK.

Billing follows the app. Usage is billed to the organization that owns the installation, and that org's model policies, budgets and spending limits apply. Owners can spot app usage in AI usage CSV reports through the `app_client_id` field. This is how you run a Copilot-powered bot in CI without tying it to one employee's seat.

GitHub also widened its usage data. On October 22, the usage metrics API started reporting custom skills, agents and MCP servers. On October 23, it added usage from tools like OpenCode. A global default for Copilot features also reached GA on October 22.

GPT-6.1 Terra arrived in Copilot on October 26 for Pro, Pro+, Max, Business and Enterprise plans. GitHub says it bills "at provider list pricing under usage-based billing." New models turn on by default unless an admin disables the global default or the model itself. If your org pins models, check your policy before the rollout reaches you.

GitHub Universe runs October 28-29. Expect more Copilot news next week.

Claude Code shipped six releases between October 21 and October 27, according to npm. The changelog leaned toward permissions, sandboxing and cloud sessions.

On approvals and safety, these changes stand out:

- 
**Bypass mode and `rm`:** In the Desktop app and VS Code, the dangerous`rm` and`rmdir` prompt in bypass permissions mode now denies the command at once with a rewrite hint. Unattended sessions keep going instead of hanging.
- 
**PowerShell fix:** Some`Set-Location` and`cd` commands with extra arguments were auto-approved. They now prompt.
- 
**Windows hook fix:** PreToolUse and PermissionRequest hooks let some tool calls through when Git Bash was missing. That is fixed.
- 
**`ultrareview` trust:**`claude ultrareview` now requires workspace trust. Scripts must pass`--trust-workspace` .
- 
**Sandbox fallback:** Commands excluded from the sandbox now ask before writing to protected paths when the sandbox is on but cannot run.

Telemetry went back and forth. Version 2.1.307 added a `claude_code.safety_event` OpenTelemetry event for every tool call that a rule, auto mode, hook or sandbox stopped. Version 2.1.312 stopped sending that event six days later. If you built a dashboard on it, check it now.

Version 2.1.307 also added `/claude-api api-version`, which moves your Claude API code to API version `2026-10-21`. That pairs with the breaking API release we covered last week.

On models and cost, `ultrathink` in the terminal now runs its turn at xhigh effort on Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.1 through Anthropic's API. Usage messages now say "5-hour limit" instead of "session limit." On Bedrock, Vertex, Foundry and Mantle, Claude Code now loads model settings from a signed file on downloads.claude.ai. You can turn that off with `CLAUDE_CODE_MODEL_CATALOG=off`.

For enterprises, 2.1.312 added `oidc.user_id_claim` to the Claude apps gateway. You can now identify people by a claim other than `sub`, such as Microsoft Entra ID's `oid`. Version 2.1.310 added cloud sessions and Remote Control permissions to Enterprise custom roles.

OpenAI shipped Codex CLI 0.164.0 on October 23. Credentials got the most attention:

- Enterprise MCP connections can now sign in with eligible Codex account grants or a dedicated sign-in, including OAuth clients that need secrets.
- Codex masks credentials during supported remote Bash and Zsh snapshot sessions, including credentials from custom providers.
- On Windows, HTTPS and WebSocket connections now answer proxy challenges with the user's Negotiate or NTLM credentials.
- Queued prompts now wait for pending permission choices before they continue.

The release also added `/presets`, ten numbered slots for saved prompts and slash commands. You can now set model, effort, speed and permission profile inline with `/model` and `/permissions`. The Amazon Bedrock setup wizard is on by default.

Codex CLI 0.165.0 followed on October 27. It added GPT-6.1 Terra to the model catalog and a built-in Google Cloud Vertex AI provider, set with `model_provider = "gcp"`. Reasoning effort changes mid-conversation are now on by default for supported models. Memory v2 is the default for fresh profiles and can import memories from other agents.

Two fixes matter for approvals. Fresh TUI sessions now honor the server's default approval reviewer. And Codex closed a marketplace URL validation gap that let Git connect to a host other than the one checked against allow patterns.

Anthropic introduced Claude Duet on October 21 as a research preview. Duet runs on Opus 5.5 in its own cloud virtual machine with a browser, so your laptop can be closed. It sends a morning brief from your connected apps and checks them every few hours. It messages you only when something needs a decision.

The approval model is the interesting part. Duet reads and drafts on its own. Before it sends a message, makes a payment, signs a document or deletes anything, it shows you the action and waits. Anthropic says an auto mode classifier handles those approval checks. Two actions are off-limits even with approval: trading securities and moving money between accounts.

Anthropic also published a safety and privacy note on October 23. A dedicated classifier blocks sharing of card numbers and government IDs. Duet never sees passwords or sign-in tokens directly.

Duet is included in Pro and Max plans and shares their usage limits. Background checks count toward those limits. Access starts with a US waitlist for Pro and Max users on iOS, Android and claude.ai. Team and Enterprise plans are not included yet.

On October 21, Claude Security started scanning continuously in public beta for Claude Enterprise. It runs a baseline scan, then rescans changes on a schedule set by risk. Findings close when the fix lands and reopen if the issue comes back. Anthropic says thousands of organizations have found over a million vulnerabilities with it since the April beta.

The new pieces are an API, personal access tokens, webhooks and an MCP connector for Claude.ai and Claude Code. Role-based access controls decide who can view, triage and create projects. Anthropic is giving organizations $25,000 in free usage credits per month for two months, for usage through December 31, 2026.

On October 23, chat and Cowork merged for Claude Enterprise. A conversation starts as chat, and tasks that need code or files move to a cloud environment. The admin controls cover cost directly:

- Model entitlements by role
- Spend limits at the organization, group and member level
- Pooled monthly budgets for groups
- A new setting that keeps the experience on Sonnet and Haiku models to save money

Network egress is off by default for Enterprise cloud sessions, and connector credentials stay outside the sandbox. Organizations on zero data retention, HIPAA-ready setups or customer-managed keys stay on their current setup for now.

On October 23, Anthropic also opened a Claude Tag API in beta for Enterprise orgs. It manages Claude Tag settings on Slack channels through Admin API keys with `read:claude_tag` or `write:claude_tag` scopes.

Cursor's changelog had no entries dated in this window. Its last post was October 20. We found nothing dated October 21-28 from Google Antigravity, Jules, Devin or Kiro.

Standards had the busiest week of the quarter. AGNTCon + MCPCon North America ran October 22-23 in San Jose. Anthropic donated Agent Skills, OpenAI donated Open Responses, and MCP set a new release calendar. The talks focused less on new features and more on security and identity.

Anthropic donated Agent Skills to the Agentic AI Foundation on October 22. Agent Skills is the open format where a skill is a folder with a `SKILL.md` file plus any scripts or reference files it needs. Anthropic handed over the standard, its name and logo, the GitHub organization and the agentskills.io domain.

The adoption numbers are large. Anthropic says more than 70 agent platforms have integrated Agent Skills since the standard was published in December 2025. It names ChatGPT, Cursor, GitHub Copilot and VS Code. Anthropic also reports 1 million published skills and nearly 280 million installs.

Governance now looks like MCP's. Engineers from Anthropic, Google, OpenAI and others will maintain the standard as individuals. David Soria Parra and Mahesh Murag are the lead maintainers. The proposed roadmap adds versioned releases, a formal proposal process and security guidance for people who write and install skills. No dates came with that roadmap. You do not need to join AAIF to use or contribute to the spec.

The AAIF announcement named four new hosted projects:

That brings AAIF to 10 hosted projects, up from three at its December 2025 launch. The other six are A2A, Agent Router, agentgateway, AGENTS.md, goose and MCP. The announcement did not state licenses for the four new projects.

Open Responses deserves a closer look. It turns the shape of OpenAI's Responses API into a shared schema with common streaming events. If other providers adopt it, one client library can talk to many model APIs. That is the same move MCP made for tools.

Why does this matter? Skills, tools, model interfaces, sandboxes and training environments now sit under one neutral roof. A skill you write for one agent has a real path to run in others. And no single vendor controls how the format changes.

The MCP project merged SEP-3398: Release Cycle Updates on October 27. Core Maintainers accepted it on October 14. It sets a fixed schedule for the Model Context Protocol spec.

Here is how it works. Spec releases land on the last Tuesday of March and September. Each release gets an eight-month cycle, and cycles overlap by two months. The SEP text sets these milestones before each release:

The near-term impact is a date change. The SEP moves the December 2026 release target to late March 2027. If you planned around a December spec, plan for March instead. Proposals not finished by soft freeze move to the next cycle, and the release date does not slip.

Each SEP now needs a sponsor, either a Working Group or the Core Maintainer group. A primary and a secondary release manager run each cycle, and the secondary becomes primary next time. For anyone shipping an MCP client or server, this is good news. You get SDK betas six weeks before each release and two dates a year to plan around.

InfoQ's recap of AGNTCon + MCPCon captured the stage claims. InfoQ notes that several figures came without published data, so treat them as speaker-reported.

- 
**Tasks in Claude Code:** David Soria Parra announced MCP tasks support in Claude Code. Tool calls can now run for minutes, hours or days.
- 
**Enterprise-managed authorization:** Den Delimarsky said it is live in Claude and marked stable on the auth roadmap. Proof-of-possession tokens and workload identity federation sit in draft. The roadmap ends with agents that sign in as themselves instead of borrowing a user token.
- 
**Downloads:** Delimarsky put cumulative MCP SDK downloads at 4.17 billion.
- 
**Conformance:** About one-third of the 116 conformance scenarios now test security.
- 
**Roadmap:** InfoQ lists agent messaging, skills over MCP, identity and an HTTP-native transport among five priority areas.

Two talks dealt with tool safety. Thomas Sarlandie of OpenAI said OpenAI rejects submitted MCP servers unless every tool carries annotations. Annotations let ChatGPT fast-track read-only calls and confirm destructive ones. He also shared data from the ChatGPT plugin directory: about three in four tool schemas are under 1 kB, and 56% of plugins use server instructions.

Sarah Drasner of Chrome presented WebMCP, a proposed web standard now in origin trial. It lets websites expose tools that agents call directly instead of scraping the page. Chrome's benchmarks showed actions 2.5x to 7.5x faster. WebMCP adds read-only and untrusted-content hints to help agents resist prompt injection.

If you run MCP servers, the action item is clear. Add tool annotations now. At least one major client already requires them, and the rest will follow.

Last week we reported that only four providers serve skills over MCP the way the Skills Extension spec describes. The official SDK just made that easier. `@modelcontextprotocol/server` 2.4.0 shipped on October 23 with the Skills extension (SEP-2640) under new `ext/skills` subpaths.

The server side gets a `SkillsExtension` that serves `skills/list`, `skills/get` and a `skill://` resource template. A Node helper, `loadSkills(folder)`, loads skills from a directory. The client side gets a `SkillsClient` that verifies size, digest and `SKILL.md` frontmatter on every read. The extension works on both the 2025-11-25 and 2026-07-28 protocol versions. The `attach` method is marked experimental, and the rest of the Skills API is stable.

Two breaking-ish changes ride along. The SDK now rejects `x-mcp-header` on `number`-typed tool parameters, matching the 2026-07-28 spec. Use an integer type instead. And a newer bundled validator rejects some `$id` and `$ref` values with invalid URI schemes, such as `my_tool:output`. Check your tool schemas before you upgrade.

The MCP registry also shipped versions 1.8.4, 1.8.5 and 1.8.6 between October 23 and October 27. Version 1.8.6 now requires at least three characters for nonempty search terms. Shorter searches return HTTP 422.

A2A had no release this week. The latest tag on the A2A repo is still v1.0.1 from May. Agent identity moved forward through vendor features, not a new standard. Copilot's server-to-server tokens, Claude's enterprise-managed MCP authorization and Codex's enterprise MCP sign-in all landed in the same week. Each one gives an agent its own scoped credential instead of a person's password or long-lived token.

The infrastructure story came from an IPO filing. Anthropic's S-1 gave the clearest public view yet of what a frontier lab commits to spend on compute. SK hynix then reported a quarter that shows where much of that money ends up.

Anthropic announced it filed a Form S-1 with the SEC on October 26. It plans to list Class A common stock on the Nasdaq Global Select Market under the ticker "ANT." Share count and price range are not set yet. Goldman Sachs and Morgan Stanley are the named sources for the preliminary prospectus.

The S-1 itself carries the numbers. We pulled these directly from the filing:

- 
**Revenue:** $16.3 billion for the six months ended June 30, 2026, up from $1.2 billion a year earlier
- 
**Q2 2026:** revenue of $11.6 billion, up from $787 million, with operating income of $1.0 billion and an 8% operating margin
- 
**Operating loss:** $693 million for the first half of 2026, down from $2.1 billion
- 
**Prior years:** revenue of $41 million in 2023, $386 million in 2024 and $4.6 billion in 2025

Now the compute side. As of June 30, 2026, Anthropic reports about **$265.6 billion** in expected fixed or minimum guaranteed compute commitments, with terms from one to 20 years. On top of that, it has agreements with no fixed dollar figure, where payments depend on capacity delivered and pricing.

Press coverage has quoted totals from $417 billion to over $600 billion. Those higher figures add estimates and leases on top of the contractual minimums. The $265.6 billion number is the one in the filing's commitment disclosure.

The S-1 describes a "multi-chip compute architecture across AWS Trainium, Google TPUs, NVIDIA GPUs, and AMD GPUs." It names the partners behind each one.

- 
**AMD:** In July 2026, Anthropic signed a hardware supply framework with AMD for Helios rack-scale systems with Instinct MI450 series GPUs. It commits to an initial tranche in calendar 2027. If it buys the full tranche, Anthropic expects spending to exceed $20.0 billion. The deal allows a second tranche of about the same size. AMD agreed to buy up to $5.0 billion of Anthropic's Class N stock if conditions are met.
- 
**NVIDIA:** Access to Grace Blackwell and Vera Rubin systems comes through NVIDIA and through Microsoft, SpaceXAI, Nscale, Lambda and other clouds.
- 
**SpaceXAI:** Between May and October 2026, Anthropic signed and amended agreements with SpaceX.AI LLC, formerly X.AI LLC, for NVIDIA GPU compute.
- 
**Google and Amazon:** Both have note purchase agreements of up to $20.0 billion each. They supply TPU and Trainium capacity.

Why does this matter to you? Model pricing depends on these contracts. A lab that spreads work across four chip families can shift workloads to the cheapest capacity. That is part of why mid-tier and fast-mode prices keep falling. It also shows the lock-in risk runs both ways. Labs commit for up to 20 years, so they need steady token demand to pay for it.

SK hynix reported third-quarter results on October 28 in Seoul. Investing.com's summary of the earnings slides gives these figures:

- 
**Revenue:** 104.62 trillion won, up 32% from Q2 and 328% from a year earlier. It is the first quarter above 100 trillion won.
- 
**Operating profit:** 83.33 trillion won, an 80% operating margin
- 
**Net profit:** 39.78 trillion won, down 58% from Q2 on a 32.63 trillion won non-operating loss

Investing.com notes its summary was AI-assisted, so confirm figures against SK hynix's own filings.

The supply comments matter more than the profit. SK hynix has long-term agreements with 14 key customers, including major cloud providers, on standard five-year terms. Those deals cover more than 50% of total capacity for both 2027 and 2028. Management expects memory demand to outrun supply through 2030.

HBM4 is taking over. SK hynix expects HBM4 to pass HBM3E in both shipments and revenue in the fourth quarter. That tracks with NVIDIA's Vera Rubin ramp. The company also expects 2027 capex to rise significantly as it brings up its first Yongin fab.

Put this next to the Anthropic filing. Labs sign 20-year compute deals, and memory makers sign five-year supply deals. The AI hardware market is moving from spot buying to long contracts. If you plan to buy GPUs in 2027, the memory inside them is already spoken for.

NVIDIA and Oracle announced on October 27 that Vera Rubin NVL72 systems and NVIDIA Vera CPUs are generally available on Oracle Cloud Infrastructure. The two companies plan to grow OCI Superclusters with Vera Rubin to multi-gigawatt scale. They gave no exact gigawatt figure.

Each NVL72 rack links 72 GPUs over NVLink. OCI uses Spectrum-X Ethernet with Spectrum-6 switches and BlueField-4 DPUs. A planned HGX Rubin NVL8 server pairs eight Rubin GPUs with two 64-core Intel Xeon 6 CPUs, 4 TB of DDR5 and 6.4 Tbps of RDMA networking. No date was given for NVL8.

The release adds one useful data point. OpenAI trained and served GPT-6 Astra on an OCI Supercluster in Abilene with more than 100,000 Blackwell GPUs. OpenAI also plans to run Vera CPUs in production on OCI for agent sandbox workloads.

That last detail is a trend. Agents run code, browse and call tools, and much of that work runs on CPUs, not GPUs. NVIDIA wants that CPU business too.

NVIDIA and Meta expanded their collaboration on the Muse model family on October 26. Meta will deploy more NVIDIA infrastructure, including Vera Rubin, Vera CPUs, Spectrum-X Ethernet and BlueField-4. NVIDIA will invest $3 billion in Meta's equity offering, subject to closing conditions.

The two companies will also look at NVIDIA's OpenShell Reference Architecture for safe agent deployment, including the Sentry monitor we covered last week. The release gave no GPU counts, gigawatt figures or timeline.

We found no dated news in the window on Google TPUs, AWS Trainium or Microsoft Maia. Reports that Oracle will host OpenAI's Jalapeño chip and Google TPUs at Oracle AI World came from a single secondary source, and Oracle has not confirmed them in a release we found. We left them out. Intel and Samsung had not posted third-quarter results by our cutoff.

Three threads from this week point at the data layer.

First, defaults now carry governance weight. Gemini's new `store=true` default, Copilot's assisted permissions and Claude Code's sandbox changes all decide what happens to your data before you make a choice. Data teams need to read changelogs the way security teams read CVE feeds.

Second, agent work is getting longer. MCP tasks in Claude Code let a tool call run for hours or days. Duet checks your accounts every few hours and keeps working while you sleep. Long-running agents need data sources with stable identities, clear access rules and an audit trail. A governed lakehouse catalog gives an agent that boundary.

Third, skills are becoming the unit of reuse. With Agent Skills under AAIF and a Skills extension in the official MCP SDK, a skill that teaches an agent how to query your tables can travel between Claude, ChatGPT, Copilot and Cursor. Teams that write good skills for their semantic layer will get more out of every agent they adopt.

- 
**October 28-29:** GitHub Universe
- 
**October 31:** Mistral Large 4 weights deadline, and DeepSeek Search and Fetch billing starts for harness users
- 
**November 4:** Kimi K3.1 Pro open weights, with a license still to be named
- 
**November 16:** Copilot retires Claude Opus 4.8, Opus 5 and GPT-5.6 Terra
- 
**November 30:** Claude Sonnet 4.5 retires
- 
**Ongoing:** Anthropic's S-1 amendments, which should add share count and price range

AI changes fast. Here are tools and resources to help you keep pace.

**Try Dremio Free** - Experience agentic analytics and an Apache Iceberg-powered lakehouse. Start your free trial

**Learn Agentic AI with Data** - Dremio's agentic analytics features let your AI agents query and act on live data. Explore Dremio Agentic AI

**Join the Community** - Connect with data engineers and AI practitioners building on open standards. Join the Dremio Developer Community

**Book: The 2026 Guide to AI-Assisted Development** - Covers prompt engineering, agent workflows, MCP, evaluation, security, and career paths. Get it on Amazon

**Book: Using AI Agents for Data Engineering and Data Analysis** - A practical guide to Claude Code, Google Antigravity, OpenAI Codex, and more. Get it on Amazon

## Top comments (0)
