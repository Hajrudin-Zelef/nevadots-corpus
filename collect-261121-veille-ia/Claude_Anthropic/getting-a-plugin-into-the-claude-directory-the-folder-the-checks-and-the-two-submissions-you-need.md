---
id: collect-261121-veille-ia/claude-anthropic/getting-a-plugin-into-the-claude-directory-the-folder-the-checks-and-the-two-submissions-you-need
title: "Getting a plugin into the Claude directory: the folder, the checks, and the two submissions you need"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-25"]
keywords: ["plugin", "claude", "directory", "folder", "server", "submissions", "getting", "checks"]
source: https://www.aicodex.to/articles/submit-plugin-to-claude-directory
source_anchor: ""
source_lines: [1, 177]
section: "Claude_Anthropic"
---
# Getting a plugin into the Claude directory: the folder, the checks, and the two submissions you need

On September 25, 2026, Anthropic opened a developer portal for submitting plugins and connectors to the Claude directory. The directory is the catalog people browse under **Customize** in claude.ai and the desktop app. Two days earlier Anthropic launched Claude Marketplace, the public website that lists the same plugins alongside partner products and services. The portal is how something gets onto both.

Three things changed with the portal:

- **Anyone on Pro, Max, Team, or Enterprise can submit.** There is no partner program to apply to. On Team and Enterprise, an Owner submits, or on Enterprise a member with the**Directory** permission in a custom role.
- **Every version is checked, and updates ship from Git.** After the first listing, you merge to a branch and the directory scans and publishes the new commit. You do not resubmit.
- **You get numbers after launch.** A published plugin has a**Usage** tab with installs by product surface and version, how often each skill and MCP server runs, and error rates. The listing also shows views and the searches that led to it.

Submissions made through the older Claude Console form are no longer supported. Existing listings stay up, but to get versioned updates and usage figures you resubmit through the portal.

## What a plugin is

A plugin is a folder that packages any combination of these:

- **Skills** :`SKILL.md` files that teach Claude a workflow, such as the steps, the defaults, and what good output looks like. Skills setup covers how people use them.
- **An MCP connector reference** : a pointer to a remote MCP server you host, so Claude can reach your product's data and actions. MCP (Model Context Protocol) is the standard Claude uses to call outside tools.
- **Commands and agents** : named actions a user can run, and specialists Claude can hand work to.
- **An MCP App** (optional): interactive UI that your MCP server shows inside the conversation.

People add a plugin once and use it on claude.ai, in the desktop and mobile apps, in Cowork, and in Claude Code. Not every surface loads every part:

| Component | Chat | Cowork | Claude Code | 
|---|---|---|---|
| Skills | Yes | Yes | Yes | 
| Commands | Loaded as a skill | Yes, as `/plugin:command` | Yes, as `/plugin:command` | 
| Remote MCP server | After the user connects it | After the user connects it | Connects directly | 
| Agents and hooks | Ignored | Yes | Yes | 
| Local MCP server | Ignored | Only on the user's computer | Yes | 
| Top-level `bin/` folder | Blocks install | Blocks install | Yes | 

Design for the narrowest surface your users are on. If your plugin depends on an agent, chat users will not get it and will not see an error.

## The folder the directory expects

A minimal plugin for a product called Example Expenses looks like this:

```
expense-reports/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── file-expense/
│       └── SKILL.md
├── .mcp.json
├── README.md
└── LICENSE
```
The manifest goes in `.claude-plugin/plugin.json`, and nothing else goes in that folder:

```
{
  "name": "expense-reports",
  "displayName": "Expense Reports",
  "version": "1.0.0",
  "description": "File, track, and approve expense reports from a conversation, using your finance system's connector and your company's approval rules.",
  "author": { "name": "Example Corp", "url": "https://example.com" },
  "license": "MIT"
}
```
`name` is permanent. People install and refer to the plugin by it, so pick something specific to your product and change `displayName` if you want a new label later. Raise `version` on every release.

`.mcp.json` points at your server by URL:

```
{
  "mcpServers": {
    "expenses": {
      "type": "http",
      "url": "https://mcp.example.com/mcp"
    }
  }
}
```
Do not put API keys in this file. Everyone who installs the plugin gets a copy of it. Authentication happens through your server's OAuth flow when each user connects.

The skill tells Claude when and how to use those tools:

```
---
name: file-expense
description: File an expense report. Use when the user mentions a receipt, reimbursement, or expense, or asks to submit spending for approval.
---
To file an expense:
1. Read the amount, date, merchant, and currency from the receipt.
2. Call the expenses connector's `create_report` tool with those fields.
3. If the amount is over the user's approval limit (check with `get_policy`), add their manager as approver.
4. Reply with the report number and its approval status.
```
Claude decides whether to load a skill from its `description`, so write it as the situations a user would be in. A connector without a skill leaves Claude to guess your workflow from tool names, and that is where most plugins lose quality.

Two files are optional for loading and required for listing:

- **README.md** of at least 40 words, not counting code blocks. The directory shows it as the listing description. Say what the plugin does, how to use it, and what data it sends where.
- **A license** , as a`LICENSE` file or the`license` field in`plugin.json` .

Validation blocks a submission that is missing either.

## Test before you submit

Run these from the folder that contains your plugin folder, with Claude Code installed:

```
# Check the manifest and component files
claude plugin validate ./expense-reports
# Start a session with the plugin loaded from your working copy
claude --plugin-dir ./expense-reports
```
The first prints `✔ Validation passed` or names the field to fix. In the second, your skill appears as `/expense-reports:file-expense` and `/mcp` shows whether the server connected.

For claude.ai and Cowork, zip the folder and upload it under **Customize > Plugins > Add > Upload plugin**. Ask Claude which plugin skills it has, then connect the bundled connector from the plugin's **Connectors** tab.

Loading proves the parts appear. It does not prove the plugin helps. claude plugin eval runs test cases with and without the plugin and scores the difference. Run it before you put the plugin in front of strangers.

## Two submissions, not one

If your plugin points at an MCP server you run, submit the server too. The portal asks **What would you like to submit?** and offers two options:

|  | Plugin bundle | MCP connector | 
|---|---|---|
| Source | A GitHub repository (public before it goes live) | The server's `https://` URL | 
| Review | Validation and security scan on every version; a person reviews the first listing | Automatic policy scan; listed as **Community** by default, may be escalated to**Verified** | 
| What you get | A plugin listing and a Usage tab | A connector listing, auth settings, and a dashboard with server health and usage by tool | 

Submitting both from the same organization lets you pair the two listings. Use the same server URL in both, so a person who has your connector and your plugin sees one set of tools.

Local MCP servers packaged as desktop extensions (MCPB) are no longer accepted as their own listings. To distribute a local server, put it inside a plugin.

## What the connector submission checks

For the connector side, have these ready before you open the portal:

- **Every tool has a `title` and a `readOnlyHint` or `destructiveHint` annotation.** The portal syncs your tools from the live server and flags any that are missing them.
- **OAuth 2.0** if your tools act on a user's account. No authentication is fine for public data.
- **A reviewer test account** with realistic data in it. Only reviewers see the credentials.
- **Listing material** : a documentation URL, a privacy policy URL, a support contact, and an icon. MCP Apps also need 3 to 5 PNG screenshots at least 1,000 px wide, cropped to the app's response.

The listing slug is permanent once published. The portal also asks you to confirm seven policy acknowledgments, covering things like financial transactions, prompt injection, and conversation data collection.

## Submitting the plugin bundle

In the portal, choose **Plugin bundle** and fill in:

- **Repository** :`owner/repo` or the GitHub URL.
- **Plugin path** : the folder holding`.claude-plugin/plugin.json` , if it is not the repository root. One repository can hold several plugins, each as its own submission.
- **Branch or tag** : what the directory follows for new versions. Empty means the default branch.

Select **Validate**. A validation result covers one commit, so if you push a fix, validate again.

Your GitHub account must be connected to claude.ai in the organization you submit from, and it must have push access to the repository. The repository can stay private through validation and review if you install the Claude GitHub App on it and agree to upload the source for scanning. It must be public before the listing goes live.

Then answer the data-handling questions: whether the plugin reads or stores personal data, whether it sends data anywhere other than its declared connectors, how long it keeps data, and whether it is meant for people under 18. A security scan that finds data going somewhere you did not declare fails the version with a category such as **Sends data to an undisclosed destination**.

An organization can create 10 submissions in 24 hours, and drafts and withdrawn submissions count. The first organization to submit a given repository folder owns that listing, so submit from the organization that should own it long term.

## After you submit

Each scanned version ends up in one of three states:

- **Passes every check** : select**Publish** . By default a reviewer then publishes it. Anthropic can switch your plugin to a setting where only the first version needs a reviewer and later passing versions go live on their own.
- **Held for a reviewer** : a person reads it before it can go live.
- **Doesn't pass** : the portal lists the rules the version breaks.

Review time is not fixed. The portal shows where each submission sits.

To ship an update, merge to the tracked branch. The directory checks on a schedule, or immediately if you set up the GitHub push webhook on the plugin's **Settings** tab. The listing keeps serving the last published version until a new one passes. If the security scan flags a version, later versions also wait for a reviewer, so one bad commit can hold up your next few releases.

## Is it worth listing?

A listing makes sense when people outside your organization will use the plugin: you sell a product with an API, or you maintain a workflow many teams share. The directory reaches every paid Claude user across chat, Cowork, and Claude Code.

It is the wrong route for internal tools. A plugin that encodes your company's approval rules or points at an internal server belongs in your own organization's plugin list, where an Owner can install it by default for members, or in a private plugin marketplace in a Git repository. Building skills for your team covers that path.

Before you submit, ask one question: would a stranger who installs this get a better result on their first try than they would without it? If you cannot show that with an eval, the listing will collect installs and then get switched off.

Sources: Build plugins for Claude (Anthropic, September 25, 2026), Submit your plugin, Submit a connector, Plugin structure and testing.
