---
id: collect-261121-veille-ia/gemini-google/jules-is-read-only-after-november-11-what-to-do-now-and-where-to-go-next-a-enjoy-agentic-industries
title: "Jules is read-only after November 11. What to do now, and where to go next â Enjoy Agentic Industries"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-10-31"]
keywords: ["jules", "google", "november", "read-only", "notice", "after", "said", "still"]
source: https://enjoy.dev/blog/jules-shutdown-november-11/
source_anchor: ""
source_lines: [1, 106]
section: "Gemini_Google"
---
- To
- Everyone using Google Jules
- From
- Mo, Chief Executive
- Date
- October 31, 2026, 1:17 PM CDT
- Re
- The end of Jules

# Jules is read-only after November 11. What to do now, and where to go next

Under Googleâs notice, Jules turned read-only after November 11, 2026, and it winds down on December 31. If you havenât exported your data, try now: Google hasnât said whether the export still works. Antigravityâs cloud agents reached Google AI Ultra and paid Google AI Pro plans on November 9; Codex Cloud, Claude Codeâs cloud sessions and Copilotâs cloud agent already run tasks that way, on paid plans.

## What Google announced

On October 29, 2026, Google posted a notice titled âJules Turndownâ in Julesâs changelog: âas part of our work to unify our agentic coding efforts, Jules will wind down on December 31, 2026. You have until November 11, 2026 to continue finishing up sessions. After November 11, 2026, we will be turning Jules to read-only until the end of the year.â

November 11 has passed, so under that notice Jules is now read-only. Google hasnât added to it: on November 12, the notice was still the newest entry in the changelog, and Julesâs homepage, docs and usage page still donât mention the shutdown.

| Date | What Google said | 
|---|---|
| October 29, 2026 | Jules will wind down, and you can export your data | 
| November 11, 2026 | The last day to finish sessions | 
| After November 11, until December 31 | Jules is read-only | 
| December 31, 2026 | Jules winds down | 

Jules launched on May 19, 2025 and left beta that August, according to its changelog. It cloned your GitHub repository into a cloud VM, made a plan for you to approve, did the work and helped you open a pull request, and its docs said you were âfree to leave Jules while it is running.â

Googleâs notice says what Jules taught it is âshaping products across Google, including our agent-first platform, Google Antigravity.â The Jules account went further on X: âWeâre working on bringing our learnings from Jules into @antigravity so you get the best of both worlds: autonomous remote background task execution and an agent-first development environment.â Neither gave a date for that. Then on November 3, Antigravityâs changelog said âRemote agents are starting to roll out to Google AI Ultra and Pro subscribers,â and on November 9 it said they had finished.

## Who it affects

Everyone who used Jules, on any plan. Its usage page still lists three: Jules, free, with 15 tasks a day; Jules in Pro, included with Google AI Pro, with 100; and Jules in Ultra, included with Google AI Ultra, with 300.

It also reaches the ways in that arenât the website. The Jules homepage still offers a âjulesâ label for assigning a task from a GitHub issue, the Jules Tools CLI and the Jules API. The notice doesnât treat any of them separately, and Googleâs API page still says âThe Jules API is in an alpha release,â with no shutdown notice. Google hasnât said whether they went read-only with the website. Plan on all of them ending with Jules.

If you pay for Google AI Pro or Ultra, nothing in the notice changes your plan. Googleâs plans page lists Antigravity for coding and doesnât mention Jules, with Google AI Pro at $19.99 a month and Ultra at $99.99 or $199.99.

## How to export your Jules data

Googleâs notice gives the export no deadline, and Google hasnât said whether it still works now that Jules is read-only. If you havenât exported yet, try it now. If a call fails, the prompt asks your agent to tell you which one ârather than guessing at fixes.â

The export covers what lives inside Jules: account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, and every session, âincluding its prompt, plan, full activity log, and code patches.â In Googleâs words, âYour code, branches, and pull requests already live in GitHub, so they stay where they are.â

1. **Connect GitHub first,** if you used Jules with repositories. Google warns that otherwise âthe export wonât include repository data.â
2. **Install Jules Tools and sign in:**`npm install -g @google/jules` , then`jules login` .
3. **Open a CLI coding agent from your home directory** , âsuch as Antigravity CLI,â and paste the export prompt from Googleâs notice. The agent checks your setup, saves and runs a Python script, and tells you what it saved.
4. **Check `~/jules-export`.** It holds`sources.json` , a folder per repository with its setup script, test command and memories, your settings, environment variables and scheduled tasks as JSON, and a folder per session with a`summary.md` , the activity log and its diffs.

Read the script before your agent runs it. It reads your Jules sign-in from the macOS Keychain or Linuxâs secret store, and the prompt warns that the endpoints it calls âare undocumented.â Its output includes secrets: â`env-variables.json` contains your environment variable values, so keep the export folder private and out of your repositories.â

## Where Julesâs work can go

Julesâs model, a repository in a cloud VM ending in a pull request, is what three other companies sell today. Googleâs own pick, Antigravity, now offers something close: its remote agents run in an âisolated, Google-managed remote environmentâ where âno local machine needs to stay on,â and clone the GitHub repositories you attach. Antigravityâs changelog says that on November 9 they âfinished rolling out to Google AI Ultra subscribers and to Google AI Pro subscribers on a paid plan,â though their page still says the feature âis rolling out gradually.â They arenât available âon the free plan, on Google AI Plus, or for business accounts (Google Workspace or Google Cloud),â and the page doesnât mention pull requests, issue labels or how they count against your quota.

| Option | Where it runs | Plan it needs | Scheduled jobs | 
|---|---|---|---|
| Google Antigravity | On your computer; in Googleâs cloud on Ultra and paid Pro plans | Free weekly quota; Google AI Pro, $19.99 a month, for more and for remote agents | Automations, alongside the app | 
| Codex Cloud | OpenAIâs cloud | ChatGPT Plus, $20 a month | ChatGPT scheduled tasks | 
| Claude Code cloud sessions | Anthropicâs cloud | Claude Pro, $20 a month or $17 billed yearly | Cloud scheduled tasks | 
| GitHub Copilot cloud agent | GitHubâs cloud sandboxes | Copilot Pro, $10 a month; sandbox time billed once your GitHub planâs included compute runs out | Copilot Automations, private and internal repositories only | 

**Google Antigravity** is the successor Google names. Its models page gives a paid Google AI Pro plan Gemini 4 Argon, and Ultra also Gemini 4 Carbon, which arrived on November 10. Every plan gets âAccess to all product features,â and accounts without Pro or Ultra get âA meaningful quota, refreshed weekly,â says its plans page. Remote Control lets you âdrive your Antigravity desktop sessions and CLI instances running across your machines from any web browser,â but the machine has to stay online and ânot asleep or suspended.â Its Automations, âformerly called Scheduled Tasks,â are sidecars, âbackground processes that run alongside Antigravity.â On a free account, from November 30, the date Google gave on November 20, its only model is Gemini 3.8 Flash (our memo).

**Codex Cloud** is on ChatGPT Plus and up, at $20 a month on ChatGPTâs pricing page: Codex pricing lists Codex on the web and its cloud-based features from Plus, and for Free and Go only GPT-6 Luna in the desktop app. Codex can check out repositories from your connected GitHub or GitLab account, and each task âhas its own workspace and can keep working while your computer is asleep.â OpenAI warns that âCloud tasks may use more of your allowance than local messages.â ChatGPTâs scheduled tasks on the web âcanât work directly in a folder on your computer.â

**Claude Codeâs cloud sessions** come with Claude Pro, Max, Ultra and Team, and Enterprise with premium seats or Chat + Claude Code seats. Anthropic says a session âkeeps running after you close your laptop,â counts toward your planâs limits, and has âno separate charge for the cloud VM.â Its cloud scheduled tasks run hourly at most and âdraw down subscription usage the same way interactive sessions do.â

**GitHub Copilotâs cloud agent** takes work the way Julesâs GitHub label did: the plans page says you âAssign work to Copilot to research, plan, and write code â with or without a pull request,â and lists it as âNot includedâ on Free. Each session runs in a cloud sandbox, âan isolated Linux environment that GitHub creates for a Copilot session.â GitHub said âBilling for cloud sandbox usage begins in November 2026â: sandbox time draws first on the compute your GitHub plan includes, and after that itâs billed by usage (our Universe memo has the rates). Automations âare available in private and internal repositories only.â

## Keeping more than one agent

You donât have to choose one. Enjoy runs Antigravity on your Google account beside Claude Code on your Claude plan and Codex on your ChatGPT plan, and lets you switch agents in the middle of a conversation. Its scheduled workflows can give any of them a recurring job, but they run while your computer is awake with Enjoy open, so they donât replace a cloud VM that works with your laptop shut. Enjoy is a monthly subscription, with the first seven days free (pricing), and each agent runs on the plan you already have.

## What to do now

- **Try the export, if you havenât.** Google hasnât said whether it still works while Jules is read-only, the script depends on endpoints Google calls undocumented, and Jules winds down on December 31.
- **Save whatâs left of your open work.** Under the notice, sessions canât be finished now. Copy any plan you still need into a file in the repository, and merge or close the pull requests Jules opened, which stay in GitHub.
- **Move your scheduled tasks.** Google hasnât said whether they still run. Open`scheduled-tasks.json` and set each one up again where youâre going.
- **Update what calls Jules.** Scripts on the Jules API or Jules Tools, and issues that use the âjulesâ label, need a new home.
- **Remove Jules from GitHub** once the export is done and youâre finished with it. GitHubâs guide to installed apps shows where.

## What Google hasnât said

- **Whether the export still works** while Jules is read-only, and until when.
- **Whether scheduled tasks and label-assigned tasks stopped** on November 11.
- **Whether the API and Jules Tools went read-only with the website,** and what happens to your data after December 31.
- **How Antigravityâs remote agents count against a planâs quota,** or what âa paid planâ leaves out for Google AI Pro.
- **Anything about prices or refunds** for Google AI Pro and Ultra.

Weâll update this memo when Google answers any of them.

## 8.Questions.

- ### Is Jules read-only now?
- Yes, under Googleâs notice, and it winds down on December 31, 2026. On October 29, Google said you had until November 11 to finish sessions, that âAfter November 11, 2026, we will be turning Jules to read-only until the end of the year,â and that Jules âwill wind down on December 31, 2026.â Julesâs changelog has no newer notice.
- ### How do I export my Jules data?
- Install Jules Tools with npm install -g @google/jules, run jules login, then paste Googleâs export prompt into a CLI coding agent, such as Antigravity CLI, started in your home directory. It saves settings, environment variables, scheduled tasks, repositories, memories and every session to a jules-export folder there. Google hasnât said whether the export still works now that Jules is read-only, so try it now. Your code, branches and pull requests stay in GitHub.
- ### What replaces Jules?
- Google points to Antigravity, whose remote agents, which run in Googleâs cloud with your computer off, finished rolling out to Google AI Ultra and paid Google AI Pro plans on November 9, 2026. Codex Cloud, Claude Codeâs cloud sessions and Copilotâs cloud agent do the same job, and come with ChatGPT Plus, Claude Pro and Copilot Pro and the plans above them.
- ### Is there a free replacement for Jules?
- Not one that runs in the cloud. Jules gave free accounts 15 tasks a day. Antigravity has a free weekly quota on your computer, but its remote agents need Google AI Ultra or a paid Google AI Pro plan, and Codex Cloud, Claude Codeâs cloud sessions and Copilotâs cloud agent all need a paid plan.
- ### Will Google AI Pro or Ultra cost less without Jules?
- Google hasnât said. Its notice doesnât mention prices or refunds. Googleâs plan pages list Antigravity for coding and donât mention Jules, with Google AI Pro at $19.99 a month and Ultra from $99.99.
- ### What happens to my Jules scheduled tasks?
- Google hasnât said whether they still run now that Jules is read-only, so donât count on them. The export saves them in a scheduled-tasks.json file, so you can set them up again as Claude Code cloud scheduled tasks, Copilot Automations, ChatGPT scheduled tasks or Antigravity Automations.
