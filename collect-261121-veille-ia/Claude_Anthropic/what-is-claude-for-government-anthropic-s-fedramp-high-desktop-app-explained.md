---
id: collect-261121-veille-ia/claude-anthropic/what-is-claude-for-government-anthropic-s-fedramp-high-desktop-app-explained
title: "What Is Claude for Government? Anthropic's FedRAMP High Desktop App, Explained"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-30"]
keywords: ["claude", "government", "anthropic", "page", "desktop", "says", "anthropics", "public"]
source: https://digitalmatters.me/artificial-intelligence-ai/what-is-claude-for-government/
source_anchor: ""
source_lines: [1, 160]
section: "Claude_Anthropic"
---
**Claude for Government** became generally available on September 30, 2026. It is Anthropic’s version of Claude for public agencies, delivered through what Anthropic calls a FedRAMP High authorized environment. It had been in public beta since July.

This post covers who can buy it, what is included, where the data lives, how billing and audit work, and what is still missing. It also places it next to ChatGPT Gov and Gemini for Government. It is written for agencies, and for the web teams that serve state and local government and public universities.

The short version: Claude for Government is the Claude Desktop app with Chat, Cowork and Code tabs, connected to a separate government service. Conversation history stays on each user’s device. Billing is prepaid usage with a hard cap and, per the GA announcement, no seat fees. There is no web or mobile version. Claude Code’s command-line tool and Claude for Microsoft 365 are in early access. Before you recommend it, confirm eligibility, how chat history gets backed up, and which models your seat tier allows.

## What Claude for Government includes

Anthropic’s GA announcement says agencies get capabilities comparable to Anthropic’s commercial customers. New capabilities generally arrive on the commercial release cadence.

| Part | What it is | Status, October 2026 | 
|---|---|---|
| Claude Desktop | Chat, Cowork and Code tabs, set up through one managed setting | Generally available | 
| Admin portal | Tenant, organization and account views in the browser | Generally available | 
| Microsoft 365 connector | Reads Outlook, OneDrive, SharePoint and Teams for each user | Built in, admin sets it up | 
| Claude Code CLI | The terminal coding tool, pointed at the government service | Early access | 
| Claude for Microsoft 365 | The Office add-in, deployed from the Microsoft 365 admin center | Early access | 

It is the same Claude Desktop app used everywhere. Anthropic’s migration FAQ says one managed setting, deployed by IT, connects it to Claude for Government. Without that setting, the app shows the regular claude.ai sign-in.

Skills, plugins and projects are included. The Cowork tab is the same agent we described in our post on the Cowork merger. Admins can turn Cowork on later than Chat, so a rollout can start small.

Anthropic does not list model names on the GA page. Its government solutions page says new models typically arrive shortly after their commercial release. So a new model such as Claude Sonnet 5.5 may not appear on day one. Each user sees only the models their seat tier allows.

## Who can buy Claude for Government

The GA announcement names federal and state agencies. The solutions page goes wider. It says the product is available to U.S. federal, state and local government agencies and to organizations that support government work, such as contractors.

Public universities and community colleges are not named on either page. If you serve a state university system, ask Anthropic directly whether it qualifies.

This is a change from earlier guidance. A public sector FAQ last updated March 25, 2026 pointed state and local governments to Claude Enterprise instead. That page predates GA, so treat the solutions page as current and confirm in writing.

## Where Claude for Government keeps your data

This is the biggest difference from Claude Enterprise. Anthropic’s security and data handling page says conversation content stays on the user’s device.

Model requests pass through the Claude for Government service to a model endpoint inside the FedRAMP High boundary. The service records per-request metadata, not content. Anthropic says the service does not log request or response bodies.

Projects work the same way. A project is stored on one user’s device, and projects are not shared between people. Files added to a project stay on local disk and are not indexed.

Three points follow from that design:

- **Backup is your job.** Chat history exists only on the device that created it. Anthropic tells agencies to back up the app’s data folder through endpoint management tools.
- **The location is fixed.** It cannot be moved to a roaming profile or a synced folder.
- **Web search is the exception.** The search provider’s API is the one place traffic leaves the FedRAMP High boundary. An admin must accept a disclosure before turning search on.

Anthropic’s pages do not name a cloud provider or region for the service itself. If data residency matters to your client, ask.

## Admin, spend and audit controls in Claude for Government

The administrator guide describes a three-level setup: one tenant per agency, then organizations, then users. An organization might be a bureau, an office or a program.

**Identity.** Sign-in uses single sign-on through the agency’s identity provider, such as Microsoft Entra, Okta or ADFS. SCIM provisioning is supported. Routing rules place each person in an organization. Someone who matches no rule cannot sign in.

**Seat tiers.** A tier sets which models a user may use and how much they may spend. Self-managed tiers carry a 5-hour and a 7-day spend limit per user.

**Spend caps.** Tenant admins can cap each organization over the same two windows. A $0 cap pauses that organization. When a cap is hit, requests are refused until the window rolls forward.

**Tool policy.** Admins can set each tool and connector to ask, allow or blocked, and users cannot override that. Sending mail and changing calendar events through the Microsoft 365 connector always ask first.

**Audit.** The Compliance API streams admin events, such as sign-ins, role changes and seat assignments, into a SIEM. It does not return conversation content or tool calls. Separately, each desktop session writes a local audit log that never leaves the device. Agencies can also send tool-call metadata to their own OpenTelemetry collector.

There is no built-in DLP gate. Anthropic says the inspection points are your own network proxy and that telemetry export.

## How agencies pay for and buy Claude for Government

Anthropic describes the pricing as prepaid usage in fixed increments, with no per-seat fee and a hard not-to-exceed cap. No dollar figures are published. Contact sales for a quote.

The admin docs add a wrinkle. They describe two kinds of seat tier. Anthropic-managed tiers are paid for as seats. Self-managed tiers draw from the prepaid credit balance. Ask which kind your agreement uses, because only self-managed tiers count against spend caps.

Older figures are still online. The March 2026 public sector FAQ listed a per-seat monthly price for Claude for Government. The GA pages describe a different model, so do not quote the old figure to a client.

Buying routes listed on the solutions page:

- Directly from Anthropic.
- Through Carahsoft, which carries the GSA Multiple Award Schedule and cooperative contracts such as NASPO ValuePoint and OMNIA Partners.
- For California, through the state’s Statewide IT Shared Services program, agreement STP-GENAI-26-ANTHROPIC.

For comparison, Anthropic lists Claude Enterprise at $20 per seat per month, billed annually, plus usage, with a 20-seat minimum. Claude Enterprise is not FedRAMP authorized, per the same page.

## What is missing or still early

Some gaps will matter to agencies used to commercial Claude.

- **No web or mobile app.** The browser is used only to sign in and administer.
- **No shared projects.** Admins can share skills with everyone by adding them as a plugin.
- **Users cannot add connectors.** Only admins can. A plugin’s own local MCP server does not run.
- **Early access only** for the Claude Code CLI and Claude for Microsoft 365.
- **Auto mode is off by default** and is turned on through device management, not the admin portal.

Teams moving from the older Claude for Government Web app can import their own conversations and projects. Roles and admin settings do not carry over. Anthropic says your account representative confirms the timeline for the move.

## Claude for Government vs ChatGPT Gov and Gemini for Government

The other two large vendors have their own government offerings. They are built differently.

| Offering | How it is delivered | Authorization stated by the vendor | 
|---|---|---|
| Claude for Government | Desktop app connected to an Anthropic-run government service | FedRAMP High | 
| ChatGPT Gov | App the agency installs and runs in its own Microsoft Azure environment | Agency manages its own compliance; OpenAI’s separate managed ChatGPT Enterprise and API are FedRAMP Moderate | 
| Gemini for Government | Gemini Enterprise and Google Cloud services in an Assured Workloads folder | FedRAMP High or DoD IL4 | 

OpenAI’s FedRAMP help article describes ChatGPT Gov as a containerized app customers manage in their own Azure environment. OpenAI announced FedRAMP Moderate for ChatGPT Enterprise and its API on April 27, 2026.

Google’s deployment guidance, updated September 24, 2026, is written for federal agencies and DoD departments. It requires a US multi-region location.

For state and local teams, eligibility differs across all three. Check each vendor’s terms for your client type.

## Questions to ask before recommending Claude for Government

Treat this like any other hosted service decision. The same habits from our managed WordPress host buyer’s framework apply: get the specifics in writing.

1. **Are we eligible?** Get written confirmation for local governments, special districts and public universities.
2. **Seats or credits?** Ask whether your tiers are Anthropic-managed or self-managed, and what the increments cost.
3. **Which models?** Ask which models are available now and how long new releases usually take.
4. **How will chat history be kept?** It lives only on each device. Decide how it gets backed up, and what happens when a laptop is replaced.
5. **Where does the service run?** Ask for the cloud provider and region if your client needs them.
6. **Can your devices run it?** Cowork and Code need a local virtual machine. Check the Windows fleet checklist against agency images.
7. **What will your SIEM see?** The Compliance API covers admin events. Tool-call detail comes only through your own telemetry collector.
8. **Can developers use the CLI yet?** It is early access, so confirm before planning work around it.

For background on how local and cloud agents differ, see our post on where AI agents actually run. Our Claude Code explainer covers the coding tool itself.

## Frequently Asked Questions

## What is Claude for Government?

It is Anthropic’s Claude offering for public agencies. Users work in the Claude Desktop app, connected to a separate government service that Anthropic says is FedRAMP High authorized.

## When did Claude for Government become generally available?

September 30, 2026. It had been in public beta since July 2026.

## Can state and local governments buy Claude for Government?

Anthropic’s solutions page says it is available to U.S. federal, state and local agencies and to government contractors. The GA announcement names federal and state agencies. Confirm in writing for your client type.

## Can public universities use Claude for Government?

Anthropic’s pages do not mention universities or colleges. Ask Anthropic directly before you recommend it to a public university.

## Does Claude for Government include Claude Code and Cowork?

Yes. The desktop app has Chat, Cowork and Code tabs. The separate Claude Code command-line tool is in early access, as is Claude for Microsoft 365.

## Where are Claude for Government conversations stored?

On each user’s device. Anthropic says the service records per-request metadata only and does not log request or response bodies. Agencies need to back up the app’s data folder themselves.

## How much does Claude for Government cost?

Anthropic does not publish prices. It describes prepaid usage in fixed increments with a hard not-to-exceed cap and no per-seat fee. Contact Anthropic or Carahsoft for a quote.

## Is Claude for Government on the GSA schedule?

Anthropic says it is available through Carahsoft, which carries the GSA Multiple Award Schedule and cooperative contracts such as NASPO ValuePoint and OMNIA Partners. It can also be bought directly from Anthropic.

## Is there a web or mobile version?

No. Anthropic says there is no web-based experience or mobile access. The browser is used only for sign-in, account pages and administration.
