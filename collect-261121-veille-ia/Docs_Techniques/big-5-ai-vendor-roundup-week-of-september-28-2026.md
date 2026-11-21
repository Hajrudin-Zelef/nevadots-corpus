---
id: collect-261121-veille-ia/docs-techniques/big-5-ai-vendor-roundup-week-of-september-28-2026
title: "Big 5 AI Vendor Roundup: Week of September 28, 2026"
domain: docs-techniques
role: reference
task: article
actors: []
dates: []
keywords: ["openai", "agents", "model", "week", "september", "gpt-6", "accord", "models"]
source: https://www.softwarereviews.com/vendor-technology-notes/big-5-ai-vendor-roundup-week-of-september-28-2026
source_anchor: ""
source_lines: [1, 102]
section: "Docs_Techniques"
---
**Mark Tauschek,**

**Bill Wong,**Infotech


OpenAI shelved its next frontier model this week. GPT-6.1 Astra was due in October, but OpenAI said the model didn't meet the bar for staying within scope and authorization. The next day, DevDay put always-on agents called dots in front of paying customers. Google unveiled Gemini 4 Argon and is releasing it to vetted cyber defenders first while US government prerelease evaluations run. Anthropic shipped Claude Sonnet 5.5, took Claude for Government to general availability, and committed $100 million to train 10,000 forward-deployed engineers. Washington moved on two tracks: Six AI leaders signed a voluntary White House accord on Tuesday, and on Wednesday the FTC confirmed an investigation into OpenAI, Anthropic, and other labs. Microsoft published its Digital Defense Report, and AWS showed Bedrock Managed Agents built with OpenAI.

The market is splitting into two tiers. The most capable models are now withheld, gated, or handed to defenders first. The models most enterprises will use more often are converging on one list price, $2 per million input tokens and $10 per million output tokens, across Sonnet 5.5, GPT-6.1 Sol, and Argon's introductory rate. When the price sheets match, the buying decision moves to reliability, controls, and what an agent is allowed to touch.

### Washington gets a voluntary accord and an FTC investigation in the same week

- 
**Six      AI leaders signed a White House accord the president calls “morally      binding.”** After a September 29 meeting, Sundar Pichai, Dario      Amodei, Mark Zuckerberg, Jensen Huang, Elon Musk, and OpenAI president      Greg Brockman signed a one-page Joint Commitment on Frontier      Responsibilities. Signatories pledge internal controls for cyber, bio, and      chemical risks, monitoring so models don't access systems in unintended      ways, independent external auditors, board-level oversight, and regular      meetings on shared standards. Satya Nadella and Jeff Bezos attended      the lunch, but Microsoft and Amazon aren't among the six signatories.      The accord has no enforcement mechanism, and the president used the      occasion to argue against new regulation. Treat it as a checklist for      vendor due diligence, not as assurance.
- 
**The      FTC confirmed an industry-wide investigation into AI labs.** A senior official      told Reuters on September 30 that the agency plans formal information      demands and executive testimony from OpenAI, Anthropic, and the evaluation      group METR, among others. Reuters called it the first official US      regulatory action focused on rogue AI agents, and FTC Chairman Andrew      Ferguson has suggested developers whose agents cause harm during cyber      testing should be liable. The probe may eventually clarify who pays when      an agent damages someone else’s systems. Until it does, that answer lives      in your contracts.

### OpenAI delays a frontier model while launching persistent agents

- 
**OpenAI withholds GPT-6.1 Astra after it fails internal      safety standards.** OpenAI called off a planned      October release after testing found that the model didn't consistently      stay within a task's scope and authorization or accurately report its      actions. Future Astra models remain planned. The decision shows a release      gate stopping a more capable model, although OpenAI hasn't published the      underlying results. Enterprises should require similar go or no-go      criteria for their own deployments.
- 
**Dots give persistent agents their own cloud computers and access to more than 4,000 applications.** The      GPT-6 Astra-powered agents can work continuously, browse the web, retain      preferences, and operate through ChatGPT, Slack, and Teams. Specialist dots      add identities, credentials, and access to corporate systems. They are      rolling out to Pro, Business Premium, and Enterprise, with managed      workspaces off by default. Enterprises should govern them as nonhuman      workers and managed endpoints.
- 
**DevDay turns OpenAI's agent operating layer into a      broader platform.** The Agents API now supports      computer use, subagents, tool search, context management, and OpenAI or      customer-selected sandboxes. OpenAI also expanded Codex cloud      environments, improved plugin discovery, launched an enterprise      marketplace, and previewed Private Inference using confidential computing.      The services simplify agent development but concentrate state, tools,      memory, policy, and execution inside OpenAI's platform. Buyers should test      whether prompts, skills, traces, business rules, and task state can move      to another runtime.
- 
**GPT-6.1 Sol approaches Astra capability at one-fifth of      the standard token price.** The model costs $2 per      million input tokens, $10 per million output tokens, and $0.10 per million      cached input tokens. OpenAI reports gains across coding, computer use,      documents, and long-running work, but those are company benchmarks. Buyers      should compare total cost per accepted result, including tools, runtime,      retries, failures, and human review.
- 
**OpenAI proposes safety cases for frontier training as      its former safety-report lead resigns.** OpenAI says      structured evidence should be required before a frontier reinforcement      learning run continues, including containment, monitoring, executive      vetoes, automatic pauses, audits, and rollback. It calls the framework an      aspiration still being implemented. Days later, David Robinson resigned after helping write      safety reports for 12 frontier launches, arguing that trial-and-error      deployment is no longer adequate. OpenAI said it pauses training or holds      models back when safeguards aren't ready. The Astra decision supports that      claim, but buyers still need independent evidence that the process works      consistently.
- 
**OpenAI disrupts a campaign intended to extract      protected model reasoning.** OpenAI says more than      15,000 accounts used prompt patterns intended to reproduce hidden      reasoning and improve another model. It attributes the core activity to      people associated with Moonshot AI, while acknowledging that it can't link      every account to one actor. There was no encryption or database breach.      The case shows that coordinated, legitimate-looking traffic can still be      used to extract model capability.

### Anthropic lowers model costs and expands its enterprise delivery network

- 
**Claude Sonnet 5.5 improves speed and task economics      without changing list prices.** Anthropic says the      model runs more than 30% faster than Sonnet 5 and costs up to 30% less per      task because it uses fewer tokens. API pricing remains $2 per million      input tokens and $10 per million output tokens. It is the first Sonnet      release to use the stronger cyber safeguards developed for Anthropic’s      most capable models. These are vendor-run results, so enterprises should      rerun their own quality, safety, latency, and cost tests.
- 
**Anthropic commits $100 million to train 10,000 forward-deployed      engineers.** Claude Frontier Academy combines a      simulated deployment with a 12-week residency built around a real      implementation. Anthropic aims to train 10,000 engineers by the end of      2027. The program confirms that deployment expertise has become a      competitive weapon. Customers should use it to build internal capability      while keeping architecture, evaluations, and process ownership portable      beyond Claude.
- 
**Claude Code mods create a powerful but nonsandboxed extension layer.** TypeScript mods can rewrite prompts, block or retry tools, approve      permissions, redact secrets, replace features, and change the interface.      Anthropic says they run with the same machine access as Claude Code and      aren’t sandboxed. Team and Enterprise plans load a security-default mod      first, and administrators can restrict plugin marketplaces. Organizations      should treat mods as privileged software and keep an approved baseline      that users can't override.

- 
**Claude outage takes down the application, API, Code, Cowork, and sign-in.** On September 29, Anthropic reported elevated errors from 14:00 to 14:59 UTC across Claude.ai, the desktop and mobile apps, the API, Claude Code, and Cowork. A second issue also blocked many sign-ins, new chats, voice sessions, purchases, and file uploads, and some messages sent during the incident may not have been saved. The breadth matters more than the duration. Switching from the web app to the API or another Claude surface wouldn't have restored service. IT leaders need a fallback that uses a different provider, delivery path, and identity system, plus independent health checks and task state stored outside the vendor.

### Google restricts Gemini 4 Argon while expanding reusable agent instructions

- 
**Gemini 4 Argon begins a restricted rollout to trusted      cyber defenders.** Google announced Argon on September      30 but hasn't released it broadly. Early access is through Fairwind while      Google participates in the US government's voluntary prerelease review and      tests more safeguards. Google says the model supports one million output      tokens and will launch at $2 per million input tokens and $10 per million      output tokens before prices double. The terms and availability remain      provisional, so IT leaders shouldn't build committed roadmaps around it      yet.
- 
**Skills will replace Gems across the Gemini app and      Workspace.** Skills are reusable instructions written      in the open Markdown-based “SKILL.md” format. Workspace rollout starts      October 5, and the Gemini app follows October 13. Existing Gems will      become draft skills, but skills don't sync between the two environments.      IT teams should inventory Gems, assign owners, review instructions like      code, and test portability across different tools and permissions.
- 
**Gemini Enterprise adds a catalog of third-party      security agents.** Partner agents can investigate      threats, deploy deception controls, revoke privileged sessions with      approval, and add defenses against prompt injection and data leakage. A      common interface may reduce tool switching, but it also places privileged      actions behind Google's orchestration layer. Security teams should verify      each agent's identity, permissions, evidence, approval path, and failure      behavior.
- 
**SynthID Bio brings watermarking to AI-designed proteins.** The proof of concept embeds a detectable signature in protein sequences      and predicted structures that remains after physical synthesis. Google      says tests across three target proteins found comparable biological      function to unwatermarked designs. Broader validation is needed, but the      approach points toward provenance controls for synthetic biology.

### Microsoft strengthens the business context and plugin layer around Copilot

- 
**Fabric IQ grounds Copilot in governed business      definitions.** Fabric IQ is generally available in      Copilot Chat and Cowork, using Power BI semantic models, metrics,      relationships, and definitions to answer business questions. It is on by      default for Fabric and Power BI customers. The semantic layer may improve      consistency, but it can become harder to move than the model. Administrators      should review default enablement, source permissions, metric ownership,      and export options.
- 
**Microsoft creates one registry for plugins across      Copilot experiences.** Developers can publish once for      supported Copilot surfaces, while administrators approve plugins centrally      through Microsoft 365 and Agent 365. Microsoft also began rolling out GPT-6.1 Sol and Claude Sonnet 5.5 in Cowork      and Copilot Studio under usage-based billing. Model choice is widening,      but the registry, Work IQ context, identity, and billing layer remain      Microsoft-controlled.

### AWS brings OpenAI agents into its governance model

- 
**Bedrock Managed Agents combines OpenAI’s Agents API      with AWS identity and controls.** The preview manages      agent state, code execution, tools, durable sessions, skills, and MCP      connections. Each agent has an IAM role, can require approval before      consequential actions, and records supported API activity in CloudTrail.      Agents run in AWS, but the service is jointly developed with OpenAI and      optimized for OpenAI models. Buyers gain familiar governance while taking      on a two-vendor dependency.
- 
**AWS previews an agent that reviews cloud architecture      and generates fixes.** The Well-Architected Agent      analyzes infrastructure, topology, and infrastructure-as-code templates      across cost, security, reliability, and performance. It returns proposed      code changes, scripts, runbooks, and guided console actions. Operations      teams should validate recommendations against their own standards and keep      production changes behind conventional testing and approval gates.

### Outside the Big 5

- 
**Nvidia launches an open safety platform that enforces      agent limits outside the model****.** OpenShell creates a      deny-by-default runtime boundary, logs permitted and blocked actions, and      controls tools, files, networks, and data. The Sentry reference design      adds an independent hardware watchdog that Nvidia says can quarantine an      agent within milliseconds. Anthropic, Microsoft, and more than 100 other      organizations are participating. The agent shouldn't be able to disable or      reason around the system enforcing its limits.
- 
**Meta creates an enterprise platform around Muse and its      agent stack.** The new business unit will package      Muse, Meta Business Agent, Muse API, Muse Code, models, and infrastructure      for enterprise customers. Meta hired former MongoDB chief executive CJ      Desai to lead it. Pricing, governance, deployment, and availability weren't      announced. The move confirms that competition is shifting from individual      models toward integrated agent platforms.

### Being Reported

These stories are being reported but haven't been fully announced by the companies involved.

- 
**Anthropic has reportedly committed at least $518      billion to AI infrastructure over a decade.** Reuters,      citing a confidential IPO prospectus, reports that about 80% of the      commitments are noncancelable or payable regardless of use. The total      includes at least $111.1 billion with Google, $110 billion with Amazon,      $31.4 billion with Microsoft, and $161.2 billion in Broadcom-related      leases. Anthropic didn't comment. Compute scarcity is turning model      vendors into heavily committed infrastructure buyers tied to companies      that are also investors, distributors, and competitors.

### Our Take

This was the week the operating layer around the model became the main product. Dots, Bedrock Managed Agents, Microsoft plugins, Gemini skills, Fabric IQ, Claude Code mods, and partner security agents package models with memory, identities, credentials, business context, tools, and execution. That should simplify deployment, but it moves risk and lock-in above the model. A company may switch models while remaining dependent on one vendor for task state, permissions, semantic definitions, plugins, policy, telemetry, and billing.

The safety news points in the same direction. OpenAI withheld GPT-6.1 Astra, showing that a release gate can work, but its safety-case framework is still being implemented. Nvidia's OpenShell and Sentry approach is more concrete: Enforce limits outside the model, record decisions, and keep the watchdog beyond the agent's control. Enterprises should apply that principle regardless of platform.

### What IT Leaders Should Be Doing

- 
**Govern      persistent agents as identities, endpoints, and workloads.** Give each one an owner, defined purpose, least-privilege      access, separate credentials, network limits, spending caps, activity      logs, and a review date.
- 
**Put model      and autonomy upgrades behind separate approval gates.** A model may pass quality testing but still be unsuitable for      computer use, external communications, production changes, financial      actions, or unsupervised work. Require evidence for each capability.
- 
**Treat      plugins, mods, skills, and MCP servers as a software supply chain.** Approve publishers, scan and sign packages, pin versions,      control marketplaces, and prevent extensions from overriding enterprise      policy.
- 
**Require      controls that sit outside the model.** Use      deny-by-default sandboxes, short-lived credentials, human approval for      consequential actions, immutable traces, automatic pauses, and a kill      switch the agent can't reach.
- 
**Measure      cost per accepted result.** Include tokens,      cache efficiency, tools, runtime, infrastructure, retries, failures, and      human review. Record which model and route handled each task.
- 
**Preserve      an exit path from the agent platform.** Keep      prompts, skills, business definitions, task state, traces, and evaluations      exportable. Test a second model and runtime before lock-in becomes      difficult to reverse.
