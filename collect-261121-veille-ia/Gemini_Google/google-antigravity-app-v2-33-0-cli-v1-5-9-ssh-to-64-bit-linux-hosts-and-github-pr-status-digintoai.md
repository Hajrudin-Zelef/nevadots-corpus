---
id: collect-261121-veille-ia/gemini-google/google-antigravity-app-v2-33-0-cli-v1-5-9-ssh-to-64-bit-linux-hosts-and-github-pr-status-digintoai
title: "Google Antigravity App v2.33.0 & CLI v1.5.9: SSH to 64-bit Linux Hosts and GitHub PR Status | DigIntoAI"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["antigravity", "november", "github", "google", "dated", "pull", "linux", "status"]
source: https://digintoai.com/en/articles/google-antigravity-app-v2-32-1-cli-v1-5-9-release-notes-hydrogen-via-adc-login-r-03e5335be562
source_anchor: ""
source_lines: [1, 33]
section: "Gemini_Google"
---
## On this page

Google Antigravity's official changelog now lists Antigravity App v2.33.0 (dated November 17, 2026; no official clock time) as Latest, and also includes App v2.32.2 (dated November 16, 2026). The Antigravity CLI Latest remains v1.5.9 (dated November 14, 2026). The earlier App v2.32.1 notes (dated November 13, 2026) stay in this article as prior release history. This is not a CLI or SDK version bump, and it is not a first launch of Gemini 4 Hydrogen or Carbon. The changelog still notes that new versions roll out gradually and may take a few days to reach all users.

## **Antigravity App v2.33.0 (November 17, 2026): SSH connections and GitHub pull request integration**

The changelog marks v2.33.0 as the App Latest, dated November 17, 2026, with no official time of day. Officially, this release adds SSH host connections for running projects and agents on remote Linux machines, along with live GitHub pull request status pills, hover cards, and an Overview Pull Requests section. It also brings in-page Cmd/Ctrl+F search to the built-in browser and opens /swarm and UI plugin panels to business and enterprise accounts. It includes 16 improvements and 21 fixes.

**Connect over SSH to a remote 64-bit Linux machine:** You can connect the Antigravity App from the sidebar, the project menu (SSH Host...), or the command palette (Connect to SSH Host...). Officially, Antigravity installs its remote server automatically over OpenSSH using your ~/.ssh/config so projects, Git worktrees, agents, and terminals run on the host and keep working even after you close the window. The remote system is specified as a 64-bit Linux machine.

**GitHub pull request status pills and Overview:** GitHub pull request links in chat and URL artifact cards now render as live status pills with hover cards, alongside a new Pull Requests section in the Overview panel and an optional conversation sidebar indicator.

**In-page find in the built-in browser:** You can search within pages in the built-in browser with Cmd/Ctrl+F from either the browser toolbar or the page itself, while web apps with their own search keep handling the shortcut.

**Business and enterprise /swarm access:** Business and enterprise accounts can now use /swarm and open UI plugin panels from the side panel, Overview, New Tab, and title bar when allowed by organization admin settings.

## **Antigravity App v2.32.2 (November 16, 2026): Multi-turn /btw side-question tabs and a diagnostic logging fix**

This entry is dated November 16, 2026. It is not marked Latest and has no official clock time. Officially, it brings multi-turn /btw side-question tabs with read-only tool access to the Antigravity App and fixes an issue that could truncate diagnostic logs (1 improvement and 1 fix). Side questions (/btw) now open in a dedicated Side Question tab in the side panel, where you can ask multi-turn follow-up questions and let the agent read files, search your workspace, and run read-only commands without interrupting the main conversation.

## **Antigravity App v2.32.1 (November 13, 2026)**

**Gemini 4 Hydrogen via Google Cloud Application Default Credentials:** When signing in with Google Cloud application default credentials, users can now select Gemini 4 Hydrogen in the App model selector at low, medium, or high thinking levels.

## **Antigravity CLI v1.5.9 (November 14, 2026)**

**Bundled-Python run_workflow Tool:** Introduces run_workflow for all users, allowing agents to run Python workflow scripts that fan work out to multiple subagents in parallel or stages and combine results using the CLI's bundled Python runtime without requiring a separate local Python installation.

**Managed pluginControls for Non-Gemini Enterprise Organizations:** Adds a pluginControls section to managed admin settings so organizations not using Gemini Enterprise can govern plugins (Gemini Enterprise users continue following Cloud Console policies). Setting pluginsDisabled stops all plugins—including built-in ones—from loading and blocks installation or activation, whereas marketplaceDisabled disables the plugin marketplace while keeping installed plugins active.

**Auto Mode, /resume & /browser Improvements:** Speeds up Auto mode sandbox-exit reviews by letting the reviewer search conversations, list directories, and inspect files directly; optimizes /resume to avoid fetching full histories upfront and support unique short-ID prefixes; and switches /browser subagent recordings to .mp4 via Chrome's built-in screen recording.

**HTTP MCP Custom Header Redirect Security Fix:** Fixes a security issue where custom headers configured for an HTTP MCP server (such as API keys) could be forwarded to another host upon redirect; headers are now restricted to the configured server's own origin, alongside reliability fixes across window sizing, UTF-8 BOM handling, Windows clipboard/interaction, and macOS Keychain.
