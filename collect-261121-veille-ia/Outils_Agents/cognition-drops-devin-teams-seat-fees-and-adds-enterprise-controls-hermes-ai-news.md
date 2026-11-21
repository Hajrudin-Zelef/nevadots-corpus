---
id: collect-261121-veille-ia/outils-agents/cognition-drops-devin-teams-seat-fees-and-adds-enterprise-controls-hermes-ai-news
title: "Cognition Drops Devin Teams Seat Fees and Adds Enterprise Controls | Hermes AI News"
domain: outils-agents
role: reference
task: article
actors: []
dates: []
keywords: ["teams", "cognition", "devin", "free", "desktop", "paid", "swe-2", "controls"]
source: https://hermes-ai.net/news/cognition-drops-devin-teams-seat-fees-and-adds-enterprise-controls/
source_anchor: ""
source_lines: [1, 59]
section: "Outils_Agents"
---
## Takeaways

- Devin Desktop is now free for every Teams member, with paid quota seats retiring in 30 days.
- Every Teams user gets two concurrent free sessions powered by Cognition's SWE-2 model.
- SSO via Okta, Entra ID, or any SAML/OIDC provider now available on the Teams plan .
- Admins can set three tiers of per-user usage limits with request-and-approve flows for overages.
- Standard HIPAA Business Associate Agreement now requestable in-product without an enterprise contract.
- Startup program offers $15K upfront credits plus up to $50K in matching grants.

Cognition has revised the Devin Teams plan to remove paid Desktop seats, include free runs on its SWE-2 model, and add single sign-on, per-user spending controls, and a standard Business Associate Agreement. Devin is a coding agent that can inspect repositories, edit code, run tools, and prepare pull requests, so the new packaging changes both who can access it and how administrators govern paid work.

## Desktop loses its paid-seat gate

Cognition has removed the quota-seat requirement from Devin Desktop, allowing every member of a Teams account to use the latest client. A quota seat was the paid account assignment that previously unlocked Desktop access. Paid sessions remain available in addition to the included free capacity.

Cognition says it will retire quota seats 30 days after the plan announcement. The Teams limit also falls from 200 seats to 100. The announcement does not explain how Cognition will handle existing accounts that exceed the new cap.

## SWE-2 creates a free baseline

Each Teams user can run two free SWE-2 sessions concurrently, with paid sessions available on top. A session is an active Devin agent run for a task and codebase, and the concurrent limit governs how many free runs can operate simultaneously. Cognition has also made free SWE-2 access in Desktop and the command-line interface a permanent benefit of the Teams, Pro, and Max plans.

Cognition built SWE-2 as its internal coding model. The company says it approaches the performance of leading coding models at lower cost and starts faster by focusing on the portions of a repository relevant to a task. Using that model for included sessions gives teams a low-cost way to test Devin across more workflows.

## Teams gains tighter guardrails

The revised plan adds identity, budgeting, and prompt controls designed for broader organizational deployments:

- **Single sign-on:** Administrators can connect Okta, Microsoft Entra ID, or another SAML or OpenID Connect provider. They can disable email login after enabling SSO.
- **Per-user budgets:** Administrators can assign usage limits through as many as three spending tiers, giving different groups separate allowances.
- **Approval workflows:** Requests for additional budget can require manual review or receive automatic approval up to a configured ceiling.
- **Prompt guidance:** Devin Coach reviews prompts before submission and can recommend a lower-cost execution mode for a small task.

## Healthcare gets a shorter contract path

Cognition now lets Teams customers request a standard Business Associate Agreement directly from Settings, shortening the contract process for eligible healthcare deployments. The standard agreement removes the separate enterprise negotiation that previously accompanied BAA access.

Under HIPAA, covered entities and business associates generally need a BAA before a service creates, receives, maintains, or transmits protected health information on their behalf. Customers still need appropriate configurations, access controls, and internal policies because the agreement covers only part of their compliance obligations.

## The $65,000 startup offer has two stages

The expanded startup program combines upfront credits with conditional matching grants:

- **$15,000 upfront:** Cognition loads the credits into the account upon approval. Startups can apply them to pull requests, bug fixes, refactors, and migrations.
- **Up to $50,000 in matching grants:** Cognition ties the additional credits to the company’s next $100,000 of Devin usage.
- **Program benefits:** Accepted startups receive unlimited seats, invitations to founder events and product sessions, and program merchandise.

Cognition advertises unlimited seats for accepted startups, and the standard Teams plan now caps accounts at 100 seats. The announcement does not explain whether startup accounts receive an exception to that limit.

## Usage becomes the growth engine

Cognition competes with coding tools from Cursor, GitHub Copilot, Anthropic’s Claude Code, and OpenAI Codex for teams adopting agent-driven development. Seat charges can discourage access for engineers who use an agent occasionally, even when a smaller group runs many sessions.

Removing the Desktop seat fee lets Cognition distribute the client across an entire team and earn more as customers run additional paid work. SSO, budget tiers, and approval ceilings give security and finance teams direct control over that expansion. Exact savings will depend on each account’s paid-session volume and contract terms.

Cognition reports that Finch Legal has shipped about 3,000 pull requests since adopting Devin and estimates that the work saved roughly 20 months of engineering time. During one on-call week, usage reached 37 Devin sessions in a day. At that volume, per-user limits and approval ceilings give administrators a practical way to contain spending while preserving the included SWE-2 capacity.

Reporting is indexed from AlphaSignal. Rights remain with the original publisher and cited sources.

Read original report
