---
id: collect-261121-veille-ia/claude-anthropic/more-ways-to-work-with-claude-sheets-whiteboards-and-interactive
title: "More ways to work with Claude: Sheets, Whiteboards, and Interactive"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-10"]
keywords: ["claude", "data", "whiteboards", "sheets", "interactive", "more", "work", "connectors"]
source: https://blogs.techbytes.app/posts/more-ways-to-work-with-claude-sheets-whiteboards-and-interactive/
source_anchor: ""
source_lines: [1, 92]
section: "Claude_Anthropic"
---
# More ways to work with Claude: Sheets, Whiteboards, and Interactive

Anthropic launched Claude Sheets, Whiteboards, and Data alongside interactive connectors for Adobe, Atlassian, and Salesforce to expand workspace workflows.

By Dillip Chowdary â¢ Nov 10, 2026 â¢ Source: Claude Blog

At a glance

- Marketplace
- more than 9,000 connectors and plugins
- Artifacts
- Claude Sheets, Whiteboards, Claude Data
- Data sources
- Snowflake and Salesforce warehouses
- Auto-connect
- Beta on eligible Team and Enterprise tiers
- Gusto turnaround
- Over a week to a few hours

In this post

Anthropic launched an expansion of its AI collaboration platform, introducing new artifact types alongside an upgrade to interactive connectors. As detailed in Claude Blog's report, the release introduces Claude Sheets, Claude Whiteboards, and Claude Data, while teasing the upcoming Claude Meetings capability. The release aims to extend the collaborative development dynamic pioneered by Claude Code to knowledge workers across enterprise teams.

This report breaks down the technical capabilities, enterprise administration settings, and verified production deployments associated with the update. It is designed for enterprise architects, data managers, operations leads, and knowledge teams evaluating generative AI workspaces. The analysis outlines how cross-application context sharing and certified business data pipelines function without requiring custom code.

## 01Model Context Protocol and embedded partner plugins

Context retrieval across distributed business systems relies on the Model Context Protocol, the open standard Anthropic introduced for connecting artificial intelligence to enterprise tools. The Claude Marketplace now lists more than 9,000 connectors and plugins, encompassing both partner integrations and community-built additions. This protocol foundation enables the platform to traverse corporate repositories, email stores, and cloud drives that sit outside traditional codebase repositories.

Interactive connectors allow third-party partners to embed their applications directly into the Claude workspace so users work without switching browser tabs. Native integrations currently support systems from Adobe and Atlassian, allowing employees to access project assets within conversational threads. Additionally, targeted plugins from Salesforce, NetSuite, and Workday have entered beta with preconfigured functional skills.

These skill-equipped plugins handle specialized domain tasks directly from prompt inputs. Account representatives can prepare client dossiers before sales calls, accountants can analyze ledger data through NetSuite, and human resources personnel can process employee expenses and time-off requests inside Workday. By keeping application controls inside the workspace, workers manage business transactions without navigating disparate user interfaces.

## 02Collaborative sheets, whiteboards, and certified data dashboards

The new artifact portfolio allows teams to build functional spreadsheets, visual whiteboards, and operational dashboards alongside the conversational assistant. Claude Sheets lets groups organize numerical records, generate complex formulas, and structure data across multiple workbook tabs. Complementing this, Claude Whiteboards provides a shared canvas to map ideas during planning sessions, which Claude can subsequently summarize into structured takeaways.

Claude Data, formerly known as Dashboards, connects directly to data warehouses including Snowflake and Salesforce to generate interactive visual assets. When configured, it relies on metric definitions certified by internal data teams to output validated dashboards, data stories, or narrated short video recaps. These components integrate with existing document, slide, and design generators, as well as the Claude Motion beta for animated visual assets.

Advertisement

Tech Pulse Daily

### Get tomorrow's pulse first

Join engineers who read Tech Pulse before stand-up. Free, weekday mornings.

Teams can construct custom artifacts suited to their specific operational rhythms, including project status boards, daily executive briefings, account trackers, and contract review matrices. Because Claude preserves persistent contextual state across every session phase, source details flow seamlessly from whiteboard sketches into analytical spreadsheets and reporting dashboards. Looking ahead, the upcoming Claude Meetings tool will join live conference calls to capture meeting notes and revise shared artifacts.

## 03Internal redline automation and interactive finance updates

Inside Anthropic, non-technical operational units built specialized internal tooling without writing software code. The internal legal department created a Slack-based contract review system that automatically marks redlines based on the team's established legal playbook. This deployment reduced marketing contract turnaround times from three full days down to 24 hours, addressing straightforward adjustments before attorneys inspect drafts.

Similarly, the company's internal finance group restructured their executive reporting processes by converting a static corporate update document into an interactive web page. Finance staff now collaborate directly with Claude to maintain and refresh executive figures collectively. Both internal implementations demonstrate how operational experts convert institutional domain knowledge into persistent software utilities without requiring engineering assistance.

## 04Customer workflow gains across legal and finance

Enterprise adopters report measurable efficiency gains across specialized operational teams. At payroll platform Gusto, a member of the commercial legal team assembled a custom utility in a few hours that transforms negotiated deal terms into signature-ready agreements. This automation shrank turnaround intervals from over a week to a few hours while retaining attorney review for all modified legal clauses.

Financial planning and analysis teams experienced similar reductions in administrative overhead during regular reporting cycles. At Advantage Solutions, Director of Financial Planning and Analysis Ash Gupta reported that automated assembly cut slides, validation, and commentary preparation from 10 hours per cycle to under 30 minutes. Customer success staff at HubSpot use live dashboards to draft customer follow-up messages within 30 minutes of call completion.

Software teams are also shifting repetitive planning steps to automated assistant pipelines. Datadog Chief Product Officer Yanbing Li noted product managers generate feature briefs, product requirement documents, and testable prototypes rapidly, freeing time for product evaluation. At Accenture, Legal Chief Technology Officer and Chief Intellectual Property Officer Mindy Lok confirmed the firm is restructuring legal operational models using the platform.

## 05Enterprise access management and auto connect controls

To support large-scale enterprise rollouts, Anthropic integrated compliance and security governance controls across administrative dashboards. Administrators on Enterprise subscription tiers can designate which departments access specific connectors, while applying granular spend constraints across organizational units. These safeguards ensure corporate oversight matches existing regulatory standards as conversational models connect to core infrastructure.

Integration management is streamlined through Claude auto-connect, currently in beta testing on eligible Team and Enterprise tiers. This capability provides a one-time administrative configuration for services like Notion, Atlassian, and Dropbox, automatically authenticating employees when they first interact with a tool. The platform enforces existing credential boundaries and allows administrators to restrict connector permissions, such as permitting Slack reading while blocking automated messaging.

## Frequently asked questions

### What are the new artifact types introduced in Claude?

Anthropic introduced Claude Sheets for multi-tab spreadsheets and formulas, Claude Whiteboards for collaborative idea mapping, and Claude Data for dashboards and data stories. Teams can also build custom artifacts such as project boards and contract review tables.

### How does Claude Data connect to enterprise information?

Claude Data connects directly to Snowflake and Salesforce, using certified definitions from internal data teams to produce validated dashboards and narrated videos. It operates alongside Claude Motion, which turns content into short animations.

### What integrations are supported by interactive connectors?

Partners including Adobe and Atlassian embed directly in Claude, while plugins from Salesforce, NetSuite, and Workday are available in beta with preconfigured functional skills. The Claude Marketplace offers more than 9,000 connectors built on the Model Context Protocol.

### What administrative controls does Claude offer for enterprises?

Enterprise tier administrators can designate connector access, enforce granular spend limits, and restrict action permissions such as allowing Slack reading while prohibiting messaging. Claude auto-connect in beta enables one-time setup for tools like Notion, Dropbox, and Atlassian.

## Sources

## Related on Tech Bytes

Advertisement
