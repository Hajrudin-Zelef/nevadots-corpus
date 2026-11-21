---
id: collect-261121-veille-ia/gemini-google/google-is-shutting-down-jules-how-to-export-your-data-and-what-antigravity-s-new-plugin-marketplace-
title: "Google Is Shutting Down Jules: How to Export Your Data and What Antigravity's New Plugin Marketplace Offers - CrazycodersLab"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-29"]
keywords: ["google", "jules", "plugin", "export", "antigravity", "agent", "coding", "down"]
source: https://crazycoderslab.com/google-jules-shutting-down-export-data-antigravity-plugin-marketplace-skills-mcp-hooks-2026/
source_anchor: ""
source_lines: [1, 82]
section: "Gemini_Google"
---
Google is tidying up its AI coding agents. On **29 October 2026**, the team behind **Jules**, Google’s asynchronous coding agent, announced that the product will wind down on **31 December 2026**. On the same day, Google launched a **Plugin Marketplace** in **Google Antigravity**, its agent-first development platform, which is where Jules’ lessons are now flowing.

If you used Jules to clear your GitHub backlog, or you are curious about where Google’s agent tools are heading, here is what changes, what dates matter and how the new plugin system works.

## Jules is shutting down: the key dates

Google says the move is part of its “work to unify our agentic coding efforts.” The timeline is short:

- **Until 11 November 2026:** you can keep working and finish your sessions.
- **After 11 November 2026:** Jules becomes read-only.
- **31 December 2026:** Jules winds down completely.

Google described Jules as an experiment in how asynchronous coding agents could help developers tackle their backlog and ship faster, and says what it learned is shaping products across Google, including Antigravity.

## What happens to your work?

The good news: your code is safe. Your code, branches and pull requests already live in GitHub, so they stay exactly where they are.

What lives *inside* Jules is different. Google now lets you export:

- account settings and environment variables,
- scheduled tasks and connected repositories,
- memories and setup scripts,
- every session, including its prompt, plan, full activity log and code patches.

### An export done by another agent

The export process itself is a neat sign of the times. You install Jules Tools and sign in:

```
npm install -g @google/jules
jules login
```
Then Google suggests opening a CLI coding agent, such as Claude Code, from your home directory and pasting an export prompt that Google provides. The agent checks your setup, runs an export script and reports what it saved. The data lands in a `jules-export` folder with a file of connected repos, a folder per repository (setup script, test command, memories), your settings and a folder per session with a readable `summary.md`, the activity log and diffs.

**Important safety note from Google:** `env-variables.json` contains your environment variable values, so keep the export folder private and out of your repositories. Also connect your GitHub account first, or the export will not include repository data.

## Meet the Antigravity Plugin Marketplace

Antigravity’s new Plugin Marketplace is a central directory for discovering and installing Google-built and third-party plugins, in both the Antigravity App and the Antigravity CLI. Google’s pitch is that connecting an agent to outside systems used to mean manually wiring up MCP endpoints, environment variables, skills and subagents in each repository. Plugins bundle all of that into verified, shareable packages you can install in a click.

### What is inside a plugin?

Each plugin is a versioned bundle defined by a `plugin.json` manifest. It can package up to five building blocks that install, update and switch on or off together:

- **Skills** (`SKILL.md` ): domain knowledge and multi-step procedures that teach the agent how to use tools well. Antigravity uses progressive disclosure, loading only each skill’s name and short description at startup and pulling in the full instructions when relevant or when you call`/skill-name` .
- **MCP servers** (`mcp.json` ): connections to APIs, databases and developer tools over remote Streamable HTTP or local`stdio` . Plugins can ask for tokens or run OAuth 2.0 once during setup.
- **Rules** (`rules/AGENTS.md` or`rules/*.md` ): persistent guardrails and coding conventions, such as query cost limits or read-only modes.
- **Hooks** (`hooks.json` ): deterministic scripts that run at points like`SessionStart` ,`PreToolUse` ,`PostToolUse` and`Stop` . Teams can use them to block sensitive tool calls, redact secrets or require tests to pass before the agent finishes.
- **Custom agents** (`agents/*.md` ): specialised subagents with their own system prompts, tool allowlists, models and command policies, for jobs like code review or log investigation.

### Launch partners

The marketplace opens with plugins from partners including Atlassian, Canva, CrowdStrike, Databricks, Datadog, Dropbox, Figma, monday.com, Ping Identity, SentinelOne, Stripe and Unity, plus Google’s own plugins for Firebase, the Gemini API, Google Cloud, Chrome DevTools, Dart and Flutter, and Google Workspace apps. Zoho’s Bigin and Zoho Projects and Netlify also appear in the catalogue.

### Try it from the terminal

In the app, open the Customizations panel and browse the Plugins tab. From the CLI, Google’s blog shows these commands:

```
# install a plugin from the marketplace
agy plugin install <plugin-name>
# build your own: validate, install locally, then check
agy plugin validate ./<plugin-name>
agy plugin install ./<plugin-name>
agy plugin list
```
## What this means for developers

Two lessons stand out. First, experimental AI products can disappear quickly, so keep your important context (setup scripts, notes, prompts) in your own repo, not only inside a hosted tool. Second, the “plugin = skills + MCP + rules + hooks + subagents” bundle is becoming a common shape across agent tools. Learning to write a good `SKILL.md`, clear rules and safe hooks is a skill that carries over from one agent platform to the next. If you are new to how agents are classified and built, our video below is a good starting point.

Find more agentic AI lessons on our videos page.

## Key takeaways

- Google’s Jules coding agent winds down on 31 Dec 2026; it turns read-only after 11 Nov 2026.
- Code, branches and PRs stay in GitHub; settings, memories and session history can be exported with Jules Tools.
- Keep the export private: it includes your environment variable values.
- Antigravity’s new Plugin Marketplace bundles skills, MCP servers, rules, hooks and custom agents into one installable plugin.
- Install with `agy plugin install` , or build and validate your own plugins locally.

## Comments 1
