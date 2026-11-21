---
id: collect-261121-veille-ia/gemini-google/google-jules-shutdown-read-only-after-november-11-gone-december-31-how-to-export
title: "Google Jules shutdown: read-only after November 11, gone December 31, how to export"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-29"]
keywords: ["jules", "google", "export", "november", "notice", "december", "read-only", "users"]
source: https://www.datastudios.org/post/google-jules-shutdown-december-31-read-only-november-11-antigravity
source_anchor: ""
source_lines: [1, 190]
section: "Gemini_Google"
---
Google shuts down Jules on December 31, with the coding agent read-only after November 11

Nov 1

7 min read

Google is shutting down Jules, the asynchronous AI coding agent it built on Gemini and connected to GitHub. A notice dated October 29, 2026 in the official Jules changelog says the service will wind down on December 31, 2026, and that users can keep working on sessions only until November 11, 2026. After that date Jules becomes read-only for the rest of the year, which leaves current users ten days of active use from today and sixty days to retrieve anything they want to keep.

Google presents the decision as part of an effort to unify its agentic coding products. The notice describes Jules as an experiment in how asynchronous coding agents can help developers ship faster, and the official Jules account wrote on X that the team is bringing what it learned into other Google products. The named destination is Google Antigravity, Google's agent-first development platform, which on the same day launched a Plugin Marketplace for installing third-party and Google-built tools into its desktop app and command-line interface.

The practical impact is limited for code and larger for workflow data. Code, branches and pull requests that Jules created already live in GitHub and stay there. What disappears with the service is everything stored inside Jules itself: account settings, environment variables, scheduled tasks, repository setup scripts, memories and the full history of every session. Google has published an export procedure for that data, and it depends on the Jules Tools command-line client, so users who rely on Jules for recurring tasks have a short window to export and rebuild those workflows elsewhere.

··········


THE JULES SHUTDOWN TIMELINE AND WHAT CHANGES ON NOVEMBER 11.

The notice sets two cutoffs. Until November 11, Jules works as before and users can finish sessions in progress. From November 11 to December 31 the service stays online in read-only mode, so past sessions remain visible but new work cannot be started. On December 31 Jules is turned off. The notice gives no time of day for either cutoff, so users should not leave anything for the last day.

........

Date

What happens to Jules

What users can still do

October 29, 2026

Shutdown announced in the Jules changelog

Everything, including exporting data

Until November 11, 2026

Normal operation

Run and finish sessions, export

November 11 to December 31, 2026

Read-only

View past sessions

December 31, 2026

Jules winds down

Nothing inside Jules; code stays in GitHub

........

The read-only phase gives users about seven weeks to export, but scheduled tasks are the item most exposed to the earlier date. A task that Jules runs on a schedule, such as a weekly dependency update or a recurring test fix, is a form of new work, and the notice does not describe any exception that would keep scheduled runs going after November 11. Teams that depend on such tasks should plan for them to stop at that point and move them to another tool before then.

The notice does not set separate dates for the Jules REST API or for Jules Tools. The export itself runs through Jules Tools, and Google has not stated whether that route keeps working during the read-only phase, which is a strong reason to export before November 11.

··········

WHAT THE JULES EXPORT SAVES AND HOW TO RUN IT.

Google's export does not come as a download button. The notice asks users to install Jules Tools, sign in, and then hand an export prompt published on the notice page to a command-line coding agent, which checks the setup, runs the export and reports what it saved. The notice names Claude Code as an example of such an agent and says to open it from the home directory. The first two steps use the commands listed in the notice:

npm install -g @google/jules
jules login

The export lands in a folder called ~/jules-export. Its structure, as described by Google, separates repository-level configuration from session history, which makes it possible to rebuild a setup in another tool without reading every activity log.

........

Exported item

Where it goes

What it is useful for

Connected repositories

sources.json

List of repos Jules was linked to

Setup scripts, test commands, memories

One folder per repository

Rebuilding the environment in another agent

Account settings

JSON file

Reference for preferences

Environment variables

env-variables.json (contains values)

Recreating secrets; keep private

Scheduled tasks

JSON file

Recreating recurring jobs elsewhere

Sessions

One folder per session with summary.md, activity log and diffs

Audit trail of prompts, plans and patches

........

Two conditions in the notice affect whether the export is complete. Users who connected Jules to GitHub must have their GitHub account connected before running the export, otherwise repository data is left out. And because env-variables.json stores the actual values of environment variables, Google tells users to keep the export folder private and out of their repositories. The notice also says the export relies on internal endpoints that are not part of the documented API, and the export prompt asks the agent to access locally stored login credentials, so it should be read before it is run, particularly on a shared or work machine.

··········

[[ADPLUS_300x250_1]]

WHAT JULES USERS ON GOOGLE AI PRO AND ULTRA SHOULD DO NOW.

Jules was never sold as a separate subscription. According to its limits page, the base tier allowed 15 tasks per rolling 24 hours with 3 running at once, Jules in Pro (included with Google AI Pro) raised that to 100 daily tasks and 15 concurrent, and Jules in Ultra (included with Google AI Ultra) to 300 daily tasks and 60 concurrent. Those allowances end with the service, while the Google AI subscriptions that carried them continue, and the same plans include access to Antigravity.

........

Google AI plan

Monthly price

Jules limits until shutdown

Antigravity rate limit

Jules base (no paid plan)

Free

15 tasks a day, 3 concurrent

No paid quota

Google AI Pro

$20

100 tasks a day, 15 concurrent

Entry quota (baseline)

Google AI Ultra

$100 or $200

300 tasks a day, 60 concurrent

5x Pro ($100) or 20x Pro ($200)

........

The Antigravity figures come from Google's May 19, 2026 plan change, which introduced the $100 Ultra tier, cut the top Ultra tier from $250 to $200, put Gemini Flash and Gemini Pro on one shared rate limit drawn down at API price ratios, and kept a separate fixed limit for non-Gemini models. Google AI credits now act only as an overage mechanism once a plan quota runs out. Plans and limits change often, and our guide to Google Antigravity pricing, free access and limits covers the tiers in more detail.

For most users the to-do list is short. Finish or close any important session before November 11. Run the export while the service is fully active, check that the repository folders and scheduled tasks are present, and move the environment variable file somewhere secure. Then decide where recurring work goes next. Antigravity is Google's intended replacement, but it works differently: it is an agent workspace with a desktop app and a CLI, while Jules ran tasks in the cloud against a GitHub repository and returned pull requests without the user's machine being involved. Developers who used Jules mainly for that hands-off, GitHub-driven style may find the closest equivalents in other cloud coding agents, which our comparison of AI coding agent pricing sets side by side.

··········

GOOGLE ANTIGRAVITY AND ITS NEW PLUGIN MARKETPLACE.

The Plugin Marketplace that Google launched alongside the Jules notice is a central directory, inside the Antigravity app and CLI, for discovering and installing plugins. Each plugin is a versioned bundle defined by a plugin.json manifest and can package skills, MCP server configurations, rules, lifecycle hooks and custom agents, which install and toggle together. Until now, connecting an outside service to Antigravity meant configuring MCP servers, environment variables, skills and subagents by hand.

In the app, plugins appear under the Customizations panel. In a terminal, the documented command is agy plugin install <plugin-name>, which defaults to the official marketplace and also accepts a GitHub repository URL or a local path; agy plugin list, enable, disable and uninstall manage what is installed, and developers can check their own plugins with agy plugin validate. Plugins that need credentials prompt for API keys or open a browser for OAuth when they are enabled.

Launch partners named by Google include Atlassian, Canva, CrowdStrike, Databricks, Datadog, Figma, Stripe and others across software delivery, operations, analytics, design, commerce and security, plus Google's own plugins for Firebase and Google Workspace apps. The marketplace documentation does not say which Google AI plans can use plugins, and it does not describe enterprise controls for approving them.

··········


GOOGLE'S CONSOLIDATION OF CODING TOOLS AROUND ANTIGRAVITY.

Jules is the second Google coding product folded into Antigravity this year. On May 19, 2026 Google announced that Gemini CLI would be retired in favor of Antigravity CLI, and from June 18, 2026 Gemini CLI and the Gemini Code Assist IDE extensions stopped serving requests for Google AI Pro and Ultra subscribers and for users of the free Gemini Code Assist for individuals. Enterprise licenses of Gemini Code Assist were left unchanged in that transition.

The Jules turndown follows the same pattern for consumer subscribers: a separate tool is closed and its users are pointed to Antigravity. The difference is the length of notice. Gemini CLI users had about a month between announcement and cutoff, while Jules users get thirteen days of active use from the announcement and about two months in total before the service disappears. Jules first became generally available in August 2025, when Google moved it out of beta on Gemini 2.5 Pro and introduced the Pro and Ultra tiers, so the product will have lasted less than a year and a half as a general release.

··········

JULES SHUTDOWN: CONFIRMED FACTS AND OPEN QUESTIONS.

The dates are confirmed by Google's own changelog: active use until November 11, 2026, read-only until December 31, 2026, and shutdown on December 31. So is the scope of the export, the location of the exported data and the fact that code, branches and pull requests remain in GitHub. Google's plan pages confirm that Google AI Pro and Ultra subscriptions continue and include Antigravity at the published rate limits.

Several points remain undocumented. Google has not said whether scheduled tasks will run during the read-only phase, how long the Jules API stays available after November 11, whether any Jules-style cloud task feature is planned for Antigravity and on what date, or whether subscribers receive any adjustment for losing Jules allowances. The export also depends on undocumented endpoints, which makes running it early, while the service is fully active, the lowest-risk choice for anyone with history or configuration in Jules they want to keep.
