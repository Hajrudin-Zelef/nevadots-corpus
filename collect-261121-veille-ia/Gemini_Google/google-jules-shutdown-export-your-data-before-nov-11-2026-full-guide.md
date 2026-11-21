---
id: collect-261121-veille-ia/gemini-google/google-jules-shutdown-export-your-data-before-nov-11-2026-full-guide
title: "Google Jules Shutdown: Export Your Data Before Nov 11, 2026 (Full Guide)"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-03"]
keywords: ["jules", "google", "export", "november", "shutdown", "sessions", "before", "github"]
source: https://dev.to/theglitchfixer/google-jules-shutdown-export-your-data-before-nov-11-2026-full-guide-25nc
source_anchor: ""
source_lines: [1, 84]
section: "Gemini_Google"
---
*Last updated: November 3, 2026*

**Short answer:** 2 deadlines decide everything: finish your sessions by Nov 11, 2026, when Jules turns read-only, and note the wind-down on Dec 31, 2026. Export your settings, sessions, memories and setup scripts with Jules Tools into a local folder before Nov 11, keep env-variables.json private, and remember your GitHub code stays safe while Antigravity has no automatic migration or date yet.

Google is shutting down Jules, its AI coding agent. If you have used Jules for coding work, two dates matter more than anything else in this story: November 11, 2026, and December 31, 2026. After the first date, Jules stops accepting new work. After the second, it is gone. This guide gives you the full action plan: what Jules is, why it is closing, what to finish before November 11, how to export everything, what stays safe on GitHub, and what Google has not promised about Antigravity. Every date and claim below comes from Google's own Jules Turndown notice, dated October 29, 2026.

Google Jules is an async AI coding agent that Google launched in December 2024. Async means it works in the background, without you watching every step. You give Jules a coding task, and it works on your GitHub repository: it plans the change, writes or fixes code, and prepares the result as branches and pull requests on GitHub. Jules can also run scheduled tasks, remember context about your projects through memories, and use setup scripts for each repository. In short, Jules is a background coding helper that lives between you and your GitHub code. That last point matters for the shutdown, because it decides what you can lose and what you cannot, as explained below.

Google says it is bringing what it learned from Jules into Antigravity, its agent-first development platform, and working to combine remote autonomous background execution with Antigravity's development environment. In plain words, Google is consolidating its coding tools instead of running Jules as a separate product. Jules is not Google's first tool shutdown of this kind: the Google Mixboard shutdown followed a similar pattern, with users told to save their work before the end date. The same lesson applies here: when a tool announces a shutdown, the export window matters more than the reason.

There are two deadlines, and they mean different things:

- 
**November 11, 2026 — read-only day.** You can continue finishing sessions until November 11, 2026. After that date, Jules becomes read-only. You can still view your past sessions and logs, but you cannot create any new sessions or continue working on existing ones.
- 
**December 31, 2026 — final wind-down.** Jules winds down on December 31, 2026. That is the end of the product.

One honest gap: Google's notice states no cutoff hour and no time zone for either date. Our advice, not Google's claim, is simple: treat November 11 as your last working day and do not wait for the last hour. Deadlines with no stated hour are the ones people miss.

Work through this checklist in order, and do it before November 11, not after:

1. 
**Finish open sessions.** Complete or close every session you still care about, because after November 11 you cannot continue working on existing sessions.
2. 
**Merge what matters on GitHub.** Check your branches and pull requests on GitHub and merge the ones you want to keep, so your finished work is in your main code.
3. 
**Export everything from Jules.** Run the export in the next section. This is the only way to keep Jules' own data: settings, memories, scheduled tasks, session history, and setup scripts.
4. 
**Verify the export folder.** Open the exported folder and confirm the files and counts are there. An export you never checked is not a backup.
5. 
**Protect your secrets.** The export includes real environment variable values. Keep the folder private, do not share it, and rotate any secrets it contains.

This is the same save-before-deadline lesson from the Character.AI shutdown, where users who exported early kept their data and users who waited did not. Deadlines do not move because you were busy.

Google's official Jules Turndown notice provides a ready export prompt for a CLI coding agent. The export saves everything Jules knows about your work into a local folder. Follow these steps exactly:

1. Install Jules Tools with npm install -g @google/jules.
2. Sign in with jules login.
3. Open a CLI coding agent from your home directory.
4. Paste the export prompt from Google's official Jules Turndown notice.
5. Check the ~/jules-export folder and confirm the exported counts and files.
6. Keep env-variables.json private, do not share it, and rotate your secrets.

What lands in the export folder: account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, past sessions with prompts and plans, activity logs, and code patches. The export prompt itself comes from Google's official notice, linked in Sources below, so you always copy the current official version rather than a second-hand copy. Note: this guide does not run the export for you, and the steps above are Google's documented method, not a tested run on this site.

Yes. This is the part most people misunderstand, so here it is plainly: your code, branches, and pull requests are already on GitHub, and Google states they will not be affected by this change. Jules worked on your GitHub repositories, so the finished work lives on GitHub, not only inside Jules. Think of Jules as the worker and GitHub as your warehouse: when the worker leaves, the warehouse keeps everything he delivered there. What does not live on GitHub is Jules' own data: its memories of your projects, your settings, your scheduled tasks, your session history, and your setup scripts. That is exactly what the export saves. So do not panic about your code, and do not skip the export either. Both facts are true at the same time.

Here is the reality check other guides skip. Google says it is bringing its learnings from Jules into Antigravity, but the notice announces no automatic migration, no transfer of your sessions or settings, and no date for Jules-style background execution in Antigravity. That means three things for you: first, nothing moves by itself, so if you do not export, your Jules data does not follow you anywhere. Second, Antigravity is not presented as a same-day replacement for Jules' background execution, so do not plan your work around a feature with no date. Third, any guide that tells you to simply switch to Antigravity and continue as before is skipping the gap. Export first, then decide your next tool on its own merits.

Three questions remain unanswered, and this guide will not invent answers for them. First, whether export still works during the November 12 to December 31 read-only window is not stated explicitly in the official notice, and other guides disagree or avoid the question. Our advice is to export before November 11 and treat that as the safe deadline. Second, no cutoff hour or time zone is given for either date, so do not assume you have until midnight in your own time zone. Third, no separate API or billing deadline was found in the notice. If Google clarifies any of these points, this post will be updated, and the last-updated line at the top will show the real update date.

**What is Google Jules?**

Google Jules is an async AI coding agent launched in December 2024 that works in the background on your GitHub repositories, planning and writing code, creating branches and pull requests, and handling scheduled tasks.

**When is Google Jules shutting down?**

Jules turns read-only after November 11, 2026, when you can no longer create or continue sessions, and it winds down fully on December 31, 2026.

**How do I export my Jules data?**

Install Jules Tools, sign in, open a CLI coding agent from your home directory, paste the export prompt from the official Jules Turndown notice, and check the jules-export folder in your home directory.

**What is included in the Jules export?**

Account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, past sessions with prompts and plans, activity logs, and code patches.

**Will my code on GitHub be deleted when Jules shuts down?**

No. Your code, branches, and pull requests are already on GitHub and are not affected by the Jules shutdown.

**Will my Jules work move to Antigravity automatically?**

No. Google has announced no automatic migration and no date for Jules-style background execution in Antigravity.

**Can I still export after November 11, 2026?**

Google has not stated this clearly in the turndown notice, so export before November 11 to be safe.

**How do I export everything from Google Jules before November 11, 2026?**

Install Jules Tools, sign in, open a CLI coding agent from your home directory, paste the export prompt from the official Jules Turndown notice, and check the jules-export folder in your home directory. The export includes account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, past sessions with prompts and plans, activity logs, and code patches. Export before November 11, 2026 — after that Jules turns read-only, and Google has not stated clearly whether export still works.

## Top comments (0)
