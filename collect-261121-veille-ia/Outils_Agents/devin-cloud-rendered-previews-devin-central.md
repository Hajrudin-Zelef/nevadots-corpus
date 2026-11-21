---
id: collect-261121-veille-ia/outils-agents/devin-cloud-rendered-previews-devin-central
title: "Devin Cloud: Rendered Previews — Devin Central"
domain: outils-agents
role: reference
task: article
actors: []
dates: ["2026-10-30"]
keywords: ["devin", "rendered", "files", "finding", "previews", "findings", "cloud", "markdown"]
source: https://devincentral.com/continuity/cloud/2026-w44
source_anchor: ""
source_lines: [1, 161]
section: "Outils_Agents"
---
3 releases · updated

# Devin Cloud: Rendered Previews

Devin Cloud had three releases this week, with the main change being rendered previews for Markdown and patch files. This helps users who work with these file types. The releases also included various improvements to the Files tab, security connections, and more.

## October 30, 2026 · October 30, 2026

This release introduced rendered previews for Markdown and patch files, improvements to the Files tab, and new security connections. It also added features like live-reloading personal plugins and new finding tags. Additionally, it included various other improvements and fixes.

## Official release notes

**Rendered Previews for Markdown and Patch Files from Devin’s Machine**

Markdown and .patch/.diff files opened from Devin’s machine now open as a rendered preview, like attachments do, with a toggle back to the source.

#### Files Tab Improvements

Folders that contain only a single sub-folder open all the way down in one click (shown as parent / child), and each session’s expanded folders are kept when you switch between sessions.

#### Multi-line Secret Values

Editing an existing secret now accepts multi-line values (for example private keys or JSON files) without collapsing them to a single line.

**GitHub File Anchors Open the Review at That File**

GitHub pull request links that point at a specific file (#diff-…) now open Devin Review at that file in the web app and Devin Desktop.

**Header Scripts for MCP Servers on the Session Machine**

MCP servers pinned to the session machine can set a header script: a command run before each call that prints the headers to send, for servers that need short-lived tokens. Admins set it on a plugin’s server in Settings.

**Personal Plugins Live-Reload into Running Sessions**

Personal plugins (and their MCP tools) that you install or edit now show up in your running sessions without starting a new one. With “Auto-approve actions on your behalf” on, Devin can save, install and update your personal plugins and skills without an approval card, and reinstalling a plugin updates it in place.

**New Security Connections: SonarQube, Linear and Vanta**

Security connections can now ingest findings from SonarQube, a Linear team (with two-way state and priority sync) and Vanta. Dismissing, reopening or resolving a GitHub Advanced Security finding in Devin updates the alert in GitHub, and issues Devin creates in Linear link back to the finding.

#### Typed Finding Tags

Threat-model profiles can define tag kinds (for example CVE); scans and connection imports report each finding with a value per kind, shown as key/value tags on the finding, filterable under Fields in the findings list.

#### Findings Workflow Improvements

Dismissing a finding now lets you pick a reason (Duplicate, False positive, Not actionable, Accepted risk, Mitigated, Other); findings can be copied as Markdown from the detail panel; and the Findings table columns can be resized.

**Remediation and Search in the API and Devin MCP**

The findings API and Devin MCP can search findings by text, remediate every open finding of a scan in one call (select_all), and target remediation PRs at a chosen base branch.

#### Code Channel as the Default Thread Mode

Slack admins and individual users can pick “Code channel” as the default for where Devin starts new sessions, and Devin now reads replies inside threads in code channels, not just top-level messages.

#### Opt Out of Access-Request DMs

Slack admins can opt out of the DMs Devin sends for organization access requests.

#### Share a Private Reply with the Chat

In Teams private group chats, each Devin reply has a Share with chat link that posts just that reply to the whole group chat.

#### Reaction Trigger for Teams Automations

Teams automations can trigger when someone reacts to a channel message, with a reaction picker in the trigger conditions.

#### Network Access Request Outcomes in Teams

When a network access request is approved or denied from the web app, the card in the Teams thread shows the outcome and the requested domains instead of disappearing.

**Azure DevOps Webhooks Are Removed on Disconnect**

Disconnecting an Azure DevOps connection now removes the service hook subscriptions Devin installed for it.

#### Pylon Setup Dialog

After connecting Pylon, a setup dialog asks which Devin capabilities to turn on and reminds you to review the Pylon security profile.

#### Self-serve SSO Connections

Enterprise admins can verify ownership of their email domains with a DNS record, then create, name, edit and delete their own OIDC or SAML SSO connections from Settings > Authentication without contacting support.

**Personal Access Tokens Default to Approval Required**

Enterprises that have not configured a PAT policy now default to approval required: members can request a PAT and an enterprise admin approves it. Admins can still switch to self-serve or disable PATs.

#### Budgets

Launching budgets, allowing enterprises to organize user spend and organization spend in a budget hierarchy, and enterprise admins can grant granular permissions to manage a budget with sub-budget limits.

## October 28, 2026 · October 28, 2026

This release updated the Progress tab to show tasks live and refined the transcript and composer. It also added features like pull request tabs and plugin item views. Furthermore, it improved Azure DevOps and Bitbucket integration, and added new automation triggers.

## Official release notes

#### Progress Tab Shows Your Tasks Live

The Progress tab now shows the session’s task list as a live drawer; clicking a task filters the Activity stream to the work done for it. Each session has a single Progress tab, and the separate Tasks tab is retired.

#### Transcript and Composer Refinements

End / Cmd+Down jumps to the end of the transcript in one press. Shell cards for chained commands show the whole command and each step’s output. Attached .diff/.patch files render as a Changes list, and you can quote a selection from an attachment’s code view. Error toasts auto-dismiss by default, with a preference to keep them. Text typed in the composer is kept while the session’s organization loads. Images open in a Slack-style zoomable lightbox, people Devin mentions are drawn as avatar pills, file trees use Seti file icons, and the Explorer remembers whether it was open per session. Redaction markers name the secret they matched, plugins that failed to install are listed in session info, and accessibility computer-use actions appear in the worklog.

#### Pull Request Tabs

The add-tab Pull requests menu separates the session’s own PRs from mentioned PRs.

#### Plugin Item View and User-only Skills

A plugin’s skills, rules, hooks and MCP servers open in one item view. The composer’s @ Skills category lists your user-level skills, and mentioning one invokes it.

#### Azure DevOps and Bitbucket

Azure DevOps PR feedback (comments, CI, merge conflicts) is routed to the session that owns the PR, as on GitHub, and webhook setup checks build access before subscribing to build events. On Bitbucket Data Center, Devin can decline pull requests and keeps reviewers when updating one; Bitbucket Cloud webhook setup reports missing scopes and asks you to reconnect.

#### Automations

Channel-only Slack “Reaction added” triggers can be edited, and a refused LLM check shows a clear error.

#### Devin Oncall

Responder writes are rendered as automation-style session cards, and a banner offers to migrate an auto-triage automation to Oncall.

## October 26, 2026 · October 26, 2026

This release merged the Progress and Shell tabs into a single Activity view and introduced richer artifacts. It also refined the transcript and composer, and added features like recording viewer improvements and edited Slack message marking. Additionally, it included new Terraform resources and data sources.

## Official release notes

**New Activity Tab Replaces Progress and Shell**

The Progress and Shell tabs are merged into a single Activity view: one stream of everything Devin did, with search, type filters and a pinnable details drawer. Rows fold under the user message they respond to, and shell commands list the files they edited inside their card.

#### Richer Artifacts, Shown Inline

Devin builds HTML artifacts (reports, dashboards, tables, charts) on a new component kit that matches the app’s design system, and can embed an artifact directly in its message instead of only attaching it.

#### Transcript and Composer Refinements

Blocked prompts show a blocked card under the message explaining why. A long shell command is clipped to a preview that opens on click. The browser tab is labelled with the preview’s name until the page reports a title. “Keep Devin awake” stays available while a sleeping session wakes. You can star a mode as your default from the capability picker in an existing session, and copy a Devin’s link from its rail menu.

#### Recording Viewer Opens on the Video

Session recordings now open on the video by default.

#### Edited Slack Messages Are Marked

When Devin edits a Slack message, its footer says (edited). The “Channel for repository updates” dropdown in Slack settings is now searchable.

#### PR Cards and Hovercards in Devin Review

Hovering a PR link shows the PR card even when no session you can view owns it, with a one-click Devin Review action. The session header’s PR chip lists the PRs the session references, and PR pills are coloured by PR state.

#### Terraform Oncall Responder Resource

The Terraform provider adds a `devin_oncall_responder` resource and `devin_oncall_schemas` data source.

*Fan coverage from Devin Central — not an official Cognition announcement. Devin is a trademark of Cognition.*
