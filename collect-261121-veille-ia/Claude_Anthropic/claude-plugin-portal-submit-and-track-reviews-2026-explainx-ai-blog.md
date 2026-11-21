---
id: collect-261121-veille-ia/claude-anthropic/claude-plugin-portal-submit-and-track-reviews-2026-explainx-ai-blog
title: "Claude Plugin Portal: Submit and Track Reviews (2026) | explainx.ai Blog"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["claude", "plugin", "portal", "submit", "submission", "anthropic", "review", "usage"]
source: https://www.explainx.ai/blog/claude-plugin-developer-portal-submit-track-usage-2026
source_anchor: ""
source_lines: [1, 84]
section: "Claude_Anthropic"
---
Anthropic's developer-facing account, @ClaudeDevs, posted on September 26, 2026: *"It's now easier to build plugins for Claude. We built a new portal to submit your plugin, track review, and see usage. Plugins package MCP and skills, and are becoming the way to build for Claude. MCP usage across Claude products is up 110x this year!"*

That's a real shift in how Anthropic wants developers to ship for Claude — not a bigger feature list, but a proper front door: one place to submit, one place to watch review status move, one place to see who's actually installing what you built. If you've been sitting on an MCP server or an agent skill wondering how to get it in front of Claude users instead of just your own config file, this post is the exact path from where you are to a live, tracked listing.

## TL;DR

| Question | Answer | 
|---|---|
| What launched? | A dedicated Claude plugin developer portal — submit, track review, see usage analytics | 
| Where do I submit? | claude.ai/directory/manage/new | 
| How many submission paths? | Two — single MCP connector, or a full plugin bundle (MCP + skills) via GitHub | 
| What do I need for a single connector? | A remote MCP server URL, nothing else | 
| What do I need for a bundle? | A GitHub repo combining one or more MCP servers with skills | 
| What analytics do I get post-launch? | Installs by product and version, listing views, discovery search terms | 
| Why does this matter now? | Anthropic says MCP usage across Claude products is up 110x this year | 
| Is this free? | Submission is free; you still pay for whatever infra your MCP server runs on | 
| Does this replace claude-plugins-official? | No — it's the submission and tracking layer; plugins still surface through Claude's directory and marketplaces, see our top 25 Claude plugins guide | 

## What the portal actually does

Before this, getting a plugin in front of Claude users meant formatting a marketplace entry by hand, opening a PR against a community or official marketplace repo, and having no idea afterward whether anyone installed it, let alone whether it worked for them. The new portal collapses that into three functions:

1. **Submission** — you describe and register your plugin directly through the portal UI, choosing one of two submission paths (below), instead of hand-editing marketplace JSON or opening a PR against someone else's repo.
2. **Review tracking** — you can see where your submission sits in Anthropic's review pipeline instead of submitting into a void and refreshing GitHub notifications.
3. **Usage analytics** — once live, the portal surfaces real usage data: installs broken down by product and version, how many people viewed your listing, and which searches actually led someone to find it.

That third piece is the one that changes the calculus for developers most. Prior to this, a plugin author had no first-party way to know if their tool was being used at all — no install counts, no sense of which Claude surface (Code vs. Desktop vs. Claude.ai) actually adopted it. Now that data comes from Anthropic directly, the same party that runs the review and the directory.

## The two submission paths

Anthropic's follow-up post from @ClaudeDevs spells out exactly two ways in:

"Two ways to submit: Single MCP connector: point to your remote MCP server. Plugin bundle: combine MCP servers and skills in a GitHub repo. You make submissions directly in Claude: claude.ai/directory/manage/new"


### Path 1 — single MCP connector

If you already run a remote MCP server — the kind covered in our MCP servers explainer — this is the lightest path. You submit the server's URL through the portal; there's no need to package skills, write a manifest, or maintain a separate repo structure just to get listed. This is the right path if your entire offering is "here's a tool Claude can call," with no bundled instructions or workflow beyond what the MCP tool schema already exposes.

### Path 2 — plugin bundle

If you want to ship more than a bare connector — instructions for how Claude should use your tool, multiple MCP servers working together, slash commands, or packaged domain knowledge — you submit a **plugin bundle**: a GitHub repository that combines one or more MCP servers with skills. This is the structure our top 25 Claude plugins roundup and Claude Knowledge Work Plugins guide both describe: a packaged directory with MCP config, SKILL.md files, and often slash commands, all installed as one unit.

The distinction matters for planning, not just mechanics. A single connector gets you into the directory fastest with the least maintenance surface. A bundle gets you a richer listing — and a stronger candidate for the analytics to actually mean something, since a bundle typically does more per install than a bare tool call.

Make three shipping decisions:

## Why the 110x MCP usage stat is the real headline

Anthropic didn't just ship tooling — it attached a growth number to justify why developers should care: **MCP usage across Claude products is up 110x this year.** That's the kind of stat that's easy to skim past, but it's doing real work in the announcement. It's Anthropic's answer to the question every plugin author asks before investing engineering time: *is anyone actually going to use this?*

Context matters here. Max Stoiber's move to OpenAI's Plugin Developer Platform team made a nearly identical argument from the other side of the ecosystem — that frontier models are inert without the connectors domain experts build, and that OpenAI's own plugin review queue was running roughly three weeks deep as of late August 2026 purely from submission volume. Anthropic's 110x figure and OpenAI's backlog are two data points pointing at the same conclusion: both major model vendors are treating third-party plugins and MCP connectors as core infrastructure, not a side feature, and both are visibly straining to keep review pipelines up with demand. For a developer deciding where to spend limited time building integrations, that's a signal worth taking seriously rather than dismissing as marketing.

It's also worth being precise about what 110x measures — Anthropic didn't publish a base number, so treat the multiple as directional evidence of steep growth, not a verified absolute install count. The claim is about MCP calls and usage across Claude Code, Claude Desktop, and Claude.ai collectively since the start of 2026, not any single plugin's traction.

## Step-by-step: from MCP server or skill to a live, tracked plugin

Here's the actual path, written for two starting points.

### If you're starting from an MCP server

1. **Confirm your server is remote and reachable.** The single-connector path needs a live URL Anthropic's review process can hit — a local-only`stdio` server won't work here without wrapping it for remote access first.
2. **Go to claude.ai/directory/manage/new** and choose the single MCP connector path.
3. **Enter your server URL** and fill in the listing metadata — name, description, category — the same fields that will show up in search results inside Claude's plugin directory.
4. **Submit and watch review status** in the portal rather than guessing. Anthropic reviews connectors before they go live, the same trust boundary claude plugin eval warns about for third-party plugins generally: a plugin's tools execute with the user's own permissions, so review exists to catch obviously unsafe or broken connectors before they reach anyone.
5. **Once approved, check analytics regularly** — installs by product and version tell you where your users actually are (if Claude Code adoption dwarfs Claude.ai, that's where your docs and support effort should go), and search-term data tells you what language people use to look for a tool like yours, which is free positioning research you didn't have before.

### If you're starting from a skill (or several)

1. **Package your skill(s) alongside any MCP servers they depend on** into a single GitHub repository. If you haven't built a skill yet, our step-by-step agent skill guide covers the SKILL.md structure and progressive disclosure pattern this needs to follow.
2. **Structure the repo as a bundle** — MCP server config plus SKILL.md files in the layout Anthropic's plugin format expects (the same shape used across the plugins catalogued in our top 25 list).
3. **Go to claude.ai/directory/manage/new** and choose the plugin bundle path, pointing it at your GitHub repo.
4. **Before submitting, run `claude plugin eval`** against your own bundle if you haven't already — our plugin eval guide walks through scoring your plugin's actual contribution with the plugin on vs. off, which catches "does this actually help" problems before a reviewer or a user does.
5. **Submit, track review, then monitor usage** exactly as with a single connector — but with a bundle, pay closer attention to which individual skill or MCP server inside the bundle is driving installs, since bundle-level numbers can hide which piece of a multi-part plugin is actually doing the work.

## What this changes for plugin authors already shipping

If you already have a plugin listed through a community or official marketplace, this doesn't obsolete that work — the portal is the submission and tracking layer, not a replacement for the directory or marketplace structures covered in our top 25 Claude plugins guide. What it adds is visibility you didn't have: before this, "is my plugin working for people" was answered by GitHub stars and issue volume, both lagging and biased toward vocal users. Install counts by product and version, plus search terms, are leading indicators — they tell you what's happening now, not what got loud enough to file an issue.

For plugin authors running evals to justify continued investment in a plugin, this pairs directly with `claude plugin eval`: eval tells you whether your plugin helps on a controlled set of test prompts, and the portal's analytics tell you whether real usage is showing up at all. Together they answer both halves of "should I keep building this" — does it work, and does anyone use it.

## Related reading

*Portal details, submission paths, and the 110x usage figure are accurate as of the September 26, 2026 announcement from @ClaudeDevs; Anthropic may adjust submission requirements or analytics fields as the portal matures.*
