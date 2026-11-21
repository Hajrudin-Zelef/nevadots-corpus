---
id: collect-261121-veille-ia/docs-techniques/changelog
title: "Changelog"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-11-11"]
keywords: ["jules", "export", "github", "commit", "folder", "settings", "repositories", "authorship"]
source: https://jules.google/docs/changelog/
source_anchor: ""
source_lines: [1, 601]
section: "Docs_Techniques"
---
Weâre writing to let you know that as part of our work to unify our agentic coding efforts, Jules will wind down on December 31, 2026. You have until November 11, 2026 to continue finishing up sessions. After November 11, 2026, we will be turning Jules to read-only until the end of the year.

When we launched Jules, it was an experiment to learn how asynchronous coding agents could help you tackle your backlog and ship faster. What you built with it taught us where agentic development is headed, and those learnings are shaping products across Google, including our agent-first platform, Google Antigravity.

You can now export everything Jules knows about your work into a local folder: account settings, environment variables, scheduled tasks, connected repositories, memories, setup scripts, and every session, including its prompt, plan, full activity log, and code patches.

Your code, branches, and pull requests already live in GitHub, so they stay where they are. The export covers the configuration and history that live inside Jules.

Then open a CLI coding agent, such as Antigravity CLI, from your home directory and paste the export prompt below. The agent checks your setup, runs the export, and tells you what it saved.

Show the export prompt

Your data lands in ~/jules-export: connected repositories in sources.json, a folder per repository with its setup script, test command, and memories, your settings, environment variables, and scheduled tasks as JSON, and a folder per session with a readable summary.md, the full activity log, and its diffs.

env-variables.json contains your environment variable values, so keep the export folder private and out of your repositories. If you use Jules with GitHub, make sure your GitHub account is connected first, or the export wonât include repository data.

Gemini 3.1 Pro is now available in Jules

March 9, 2026

Gemini 3.1 Pro is now available in Jules for Google Pro plan users. 3.1 Pro is a smarter, more capable baseline for complex problem-solving.

This replaces Gemini 3 Pro as the default model for Pro users. Give it a try and let us know what you think.

Auto-Fixing CI Failures and configure Jules to commit as you

February 19, 2026

CI Fixer

Jules now automatically detects and fixes CI failures on pull requests it creates. When a check fails in GitHub Actions, Jules receives the error, works through a fix, and resubmits the PR â all without manual intervention.

Previously, a failed CI check on a Jules PR would stall your workflow. Resolving it required setting up REST API calls or manually copy-pasting error logs back to Jules. CI Fixer closes that gap. Jules now operates in a loop â fixing, committing, and resubmitting â so your PRs keep moving forward.

Commit Authoring

You can now control how commits from Jules are attributed on GitHub. Three authorship modes are available:

Jules: Jules is the sole author of all commits (default, existing behavior).

Co-authored (Jules + You or You + Jules): Commits are co-authored by both you and Jules, crediting both contributors.

User only: You are the sole author. Jules applies its changes under your identity.

Previously, Jules authored every commit and PR, which meant your GitHub contribution graph didnât reflect the work you initiated. Setting the commit authorship in to co-authored to you or you + Jules, makes it clear to contributors and PR reviewers who is driving each change.

It applies at the user level across all repositories and all task types â scheduled, suggested, and manually triggered tasks.

Setting commit authorship

Go to the Jules Settings page

Open the Commit Authoring section

Select your preferred authorship mode

All future sessions will use the selected attribution

MCP support comes to Jules

February 2, 2026

Today weâre launching MCP Server support in Jules starting with: Linear, Stitch, Neon, Tinybird, Context7, and Supabase.

Add a service connection in the Settings page and will trigger the MCP server tools in a session when it detects the need for a tool call.

Getting started

Our MCP server integration uses API key authentication. To get started:

Get the serviceâs API key (such as Linear or Stitch)

Go to the Jules Settings page

Click on the MCP section

Plug in your API key for that specific service

Kick off a new session

Jules will trigger the MCP server tools when it detects the need for the call

Focus on Security

It might seem odd for us to limit which MCP servers Jules can connect to. However, we are wanted to start with a focus on security. Jules operates within a cloud development environment connected to your GitHub repositories, so we prioritize a security-first approach to third-party integrations.

By hand-selecting these initial servers, we can:

Validate Data Flow: We ensure that each server adheres to strict data-handling standards, keeping your source code and API keys isolated.

Audit Tool Permissions: We verify that each server only requests the specific permissions it needs to function, preventing âover-privilegedâ access to your environment.

Ensure Stability: A vetted integration means fewer connection drops and a more predictable experience when Jules interacts with your external data.

We plan to expand this list as we work with more partners.

Tell us what to add next

If you have an MCP server in mind, you can let us know in the settings panel what we should add next.

Gemini 3 Flash is now the base model in Jules

January 30, 2026

Today weâre launching Gemini 3 Flash in Jules for all users on all tiers.

Itâs our new base model thatâs faster and significantly more capable than our previous base model, Gemini 2.5 Pro. If it feels like Jules feels like it had a double espresso, you now know why.

Give it a spin and let us know what you think.

Introducing the Planning Critic for Auto-Approved Plans

January 26, 2026

Weâve introduced a secondary agentâthe Planning Criticâto review all plans that do not require human intervention. Before Jules executes a single line of code, this agent rigorously critiques and refines the proposed plan. While this adds a small amount of time to the planning phase, the result is a 9.5% reduction in task failure rates and significantly higher quality execution paths.

Jules now finds performance optimizations

January 26, 2026

Jules now surfaces Performance Optimizations Jules has expanded its detection capabilities.

In addition to standard TODOs, Jules now proactively identifies performance optimizations within your code. These suggestions appear alongside your existing TODOs in the Suggested Tasks view, helping you catch bottlenecks and inefficiencies just as easily as you catch unfinished code.

REST API: Repoless, File Outputs, and Activity Filters

January 26, 2026

Repoless support in the REST API

Repoless comes to the REST API and we think this has massive potential. A repoless session in a sense becomes serverless, because it creates an ephemeral cloud dev environment with Node, Python, Rust, Bun, and other runtimes preloaded on the image.

The massive potential is that this serverless environment comes with an AI coding agent.

With a single API call you can spawn a serverless dev environment with only the context in your prompt. When itâs done you can download the file outputs from the session.

We canât wait to see what you all do with this one.

Get the entire file outputs from a session

Speaking of file outputs, we just made that a lot easier. When a session completes a task, you can now get the entire change set.

This is stored in a git patch format for parsing changes such as additions, modifications, and deletions.

Timestamp based filters for Session Activities

Each Jules Session has a collection of activities that represent the steps Jules took to complete the task. These activities are immutable and can be used to reconstruct the session.

Activities follow the event source pattern, meaning once they occur they never change. They are a record in time of the steps Jules took to complete the task. Since they never change, you can cache them aggressively.

We added a createTime filter that works like a range cursor and allows you to only fetch new activities over the network.

This may seem like a small optimization but stay tuned. We have some big news around this one.

Update Scheduled Tasks

January 26, 2026

Quick quality-of-life update: You no longer need to delete and recreate a scheduled task just to fix a typo or change the timing.

You can now Edit, Pause, and Resume your tasks directly from the menu. Itâs a small change that saves a lot of clicking.

Put routine maintenance on autopilot with Scheduled Tasks

December 10, 2025

You can now set recurring tasks for Jules. Whether itâs a weekly dependency check, a nightly lint fix, or a monthly cleanup, just define it once and Jules will handle the rest on your schedule.

To get started, navigate to the Scheduled tab on your repo page to configure your first task.

Set it and forget it: Define your maintenance chores once. Jules will wake up, perform the task, and open a PR without you needing to lift a finger.

Never miss a beat: No more manual prompts for the stuff that needs to happen every week. Ensure consistent code quality and dependency hygiene by automating your routine work.

This feature is available today for all Jules users.

Enable suggested tasks to let Jules find issues proactively

December 10, 2025

Jules can now work proactively in the background to improve your code. With Suggested Tasks, Jules scans your repository for potential improvements and presents them on your repo page.

To get started, enable the proactive suggestions toggle on the repo page.

Tackle the #TODOs: In this initial release, Jules focuses on identifying #TODO comments in your code. It reads the context, formulates a plan, and presents it for your approvalâturning idle comments into active solutions. More use cases coming soon.

Continuous Improvement: Once enabled, Jules continuously monitors your codebase. You donât have to ask; just check your dashboard for new suggestions and approve the ones you want.

This experimental feature is available today for Google AI Pro and Ultra subscribers on up to five repositories.

Weâve integrated with Render to handle the last mile of shipping code. Jules can now detect failed builds, analyze the logs, and push fixes to its PRs before you even know your build failed.

To enable this, go to your Render Dashboard, open the Help menu (top-right), and click Coding Agents to provision your API Key. Paste this key into Settings > Integrations in Jules.

Instant Recovery: No more context switching to dig through console logs. Jules detects the failure immediately and identifies the root cause.

Proactive Fixes: Instead of just alerting you to an error, Jules writes the code to fix it. Review the solution as a standard commit on your PR, merge it, and get back to green.

This integration is available now. Check out the docs for the full setup guide, including enabling PR previews.

Note: For now, Jules only fixes PRs it has created.

Start from scratchâinstantly

November 20, 2025

You can now start a Jules task immediately without selecting a repository. Weâve removed the speed bump between your idea and your code, allowing you to capture that spark of inspiration without the overhead.

To trigger this, simply click the âXâ next to the selected repo to start a fresh, repoless session.

Skip the Detour: Previously, starting a fresh journey meant hopping over to GitHub to create an empty repo first. Now, you can bypass that context switch entirely and keep your momentum.

Instant Ideation: Whether you are prototyping a new feature or writing a quick script, you can dive straight into the logic. Just describe what you want, and Jules gets to work.

This update is available now for all users starting a new task.

Introducing Gemini 3 Pro

November 19, 2025

Gemini 3 Pro is now available in Jules. This is the newest generation of the Gemini family, bringing clearer reasoning, stronger instruction following, and a meaningful lift in day-to-day reliability.

Coherent Planning: Multi-step tasks hold together more naturally. The agent requires less management during transitions, meaning your work moves forward with fewer detours.

Visual Verification: Leveraging the improved multimodal capabilities of Gemini 3 Pro, Jules renders and verifies web app outcomes with significantly higher precision.

Agentic Memories: The new model utilizes context more effectively, helping Jules adapt to your coding preferences and project nuances more reliably over time.

Gemini 3 Pro is rolling out now to Google AI Ultra users and will be available to Pro users in the coming days.

New Jules Tools CLI Updates: Side-by-Side Diffs, Repo Inference, and More

November 10, 2025

Weâve been busy shipping a bunch of new updates to the Jules Tools CLI to make your experience smoother and more powerful. Hereâs a rundown of whatâs new:

Parallel Task Execution

You can now start multiple parallel tasks for the same prompt using the --parallel flag with jules remote new. This is useful for getting multiple suggestions from Jules at once (max of 5).

Added: --parallel flag to remote new command.

v0.1.40 - WSL/Arch Linux Credential Fixes

Weâve refactored how we handle authentication to resolve credential issues for users on WSL and Arch Linux. This means broader platform support and no more login issues.

v0.1.39 - OAuth2 Error Handling Improvements

Weâve enhanced our OAuth2 flow to be more resilient with better error recovery, making the authentication process more reliable.

v0.1.38 - Repository Inference Feature

To shorten CLI commands and reduce configuration, weâve added repository inference. Now, Jules can automatically detect the repository from your current directory, so you donât have to specify it manually.

v0.1.37 - PNPM Installation Fixes

Weâve added better support for the PNPM package manager, ensuring full compatibility for a wider range of JavaScript projects.

v0.1.36 - Side-by-Side Diff Viewer + Bug Fixes

Reviewing code is now faster and more readable with the new side-by-side diff viewer in the TUI. Weâve also added comprehensive test coverage and fixed bugs related to auto-approval and timeout validation.

Introducing the Jules API

October 3, 2025

You can now programmatically access Julesâs capabilities to automate your work and build powerful integrations. The Jules API is designed to help you seamlessly integrate Jules into your existing development workflows, unlocking new ways to automate and enhance the entire software development lifecycle.

With the API, you can:

Create custom integrations with tools like Slack for âChatOpsâ workflows, allowing you to assign tasks directly from your chat client.

Automate bug fixing and feature implementation by connecting Jules to your project management tools like Linear or Jira.

Integrate Jules directly into your CI/CD pipelines in services like GitHub Actions.

Hereâs a quick example of how to create a new task (a âSessionâ) using a cURL command:

Weâre launching Jules Tools, a new command-line interface designed to give you direct control over your AI coding agent, making it scriptable, customizable, and easy to integrate into your existing workflows.

Key Features:

Direct Control: Create tasks (jules remote new), list active sessions (jules remote list), and monitor Jules without leaving your command line.

Apply Patches Locally: Instantly pull work-in-progress code from an active Jules session and apply it to your local machine. This lets you test changes immediately, without waiting for a commit to GitHub.

Scriptable & Composable: Integrate Jules into your automations by piping in output from other tools like gh, jq, or cat.

Interactive Dashboard: For a more guided experience, launch the built-in terminal user interface (TUI) to create and manage tasks step-by-step.

How to Install:

Install globally via npm:
npm install -g @google/jules

Or run directly without a permanent installation:
npx @google/jules

Starter Commands to Try:

See all available commands:
jules help

List all repos connected to Jules:
jules remote list --repo

Create a new task in a specific repo:
jules remote new --repo torvalds/linux --session "write unit tests"

If you run into any issues, please share your experience with us via in-app feedback or on our Discord channel.

Use Environment Variables In Jules

October 1, 2025

You can now provide Jules with environment variables at the repository level. This enables Jules to access the project-specific configurations it needs to complete tasks, like running builds, executing tests, or interacting with different services.

How It Works:

Add Variables in Repo Settings: Navigate to your repositoryâs settings page to add your environment variables. They will be associated directly with your project.

Enable for a Task: When you start a new task with Jules, youâll have the option to make these environment variables available to it.

Task-Long Access: Once enabled for a specific task, Jules will have access to the variables for the entire duration of that task. Please note that this setting cannot be changed after the task has begun.

Weâre excited to see how this unlocks new and more complex workflows for you and your team! Let us know if you have any feedback.

Jules gains memory!

September 30, 2025

Jules Memory for Repositories: Weâre excited to introduce a new Memory feature! Jules now has the ability to learn from your interactions.

How it works: During a task, Jules will save your preferences, nudges, and corrections.

The benefit: The next time you run the same or a similar task in that specific repository, Jules will reference its memory to better anticipate your needs and follow your established patterns, leading to more accurate results with less guidance.

Settings: You can toggle memory on or off for the repo in the repo settings page under âKnowledgeâ

Tell Jules exactly what file to work on using file selector

September 29, 2025

You can now tell Jules exactly which files to work with for any given task. Use the new file selector to easily and precisely reference specific files.

This removes ambiguity and gives you more granular control over Julesâs actions, helping to tighten the context for your task.

Jules Acts on PR Feedback

September 23, 2025

Jules is now able to read and respond to your comments on pull requests!

When you start a review, Jules will add a ð emoji to each comment to let you know itâs been read. Based on your feedback, Jules will then push a commit with the requested changes.

For more control, you can switch to Reactive Mode in your global Jules UI settings. In this mode, Jules will only act on comments where you specifically mention @Jules.

All Hands on Deck!

September 19, 2025

Ahoy, mateys! To celebrate International Talk Like a Pirate Day, weâve given Jules a temporary map to the treasure.

Jules Speaks Pirate: Youâll find your AI agentâs responses are a bit moreâ¦ swashbucklingâ¦ for today only.

Same Great Logic: Fear not! Beneath the eyepatch and Jolly Roger, itâs the same powerful coding engine ready to help you plunder that backlog and send bugs to Davy Jonesâ locker.

Image upload

September 9, 2025

You can now upload images when creating a task in Jules. Use this to show frontend bugs, design inspiration, UI mocks, or any visual context you want Jules to consider while generating code.

For now:

Only JPEG and PNG formats are supported.

You can uplaod as many images as you want, as long as the total size is under 5MB.

Image upload is only supported at task creation (weâre working on enabling it for follow-up prompts soon).

Note: If your task involves using assets (e.g. logos) directly in code, those must still be committed to your GitHub repo.

To improve the code review experience, weâve introduced a new stacked layout for the diff viewer. This change displays diffs for multiple files vertically on a single screen. The stacked view makes it easier to see related changes across your codebase at a glance, providing better context and speeding up your review process.

Changes:

The diff viewer now stacks file changes vertically by default

You can also toggle back to the previous tabbed diff viewer

Improved Jules Critic

September 3, 2025

Weâve shipped significant improvements to the Jules critic agent, making its feedback more insightful and reliable. To increase transparency and give you more insight into its evaluation process, you can now see the criticâs real-time analysis as it works.

Changes:

The criticâs thought process is now visible in the UI, showing its step-by-step evaluation of the code in real-time.

The criticâs now incorporates more contextual information when making decisions, leading to more accurate and relevant feedback on potential bugs and logic flaws.

Jules Sample Prompts

September 2, 2025

To help new users get started with Jules, weâve added sample prompts to the home page. These static prompts provide examples of how to use Jules and can be added to the text box with a single click.

Changes:

Sample prompts are now displayed on the home page for all users.

Clicking on a sample prompt will add the text of the prompt to the input box.

Render images in the diff viewer

August 22, 2025

Jules now intelligently renders images within the diff viewer, providing an immediate visual context for your modifications.

This means:

Instant Visual Feedback: When Jules generates images (like charts, diagrams, or web UI screenshots), youâll see the actual image in the diff, not just its code representation.

Streamlined Workflow: No need to switch between tools or download files to see the results. Jules keeps everything in one place.

Try it out! Ask Jules to render an output, like a graph based on data, and commit it to your repository. Youâll be able to see the generated image seamlessly within your diff viewer.

Export at any time

August 15, 2025

Youâre now in full control of when your code gets to GitHub. No need to wait for a task to finish or ask Jules to do it for you. At any point during a task, just click the GitHub icon in the top right to publish the current work-in-progress as a new branch or open a pull request. This gives you more flexibility and control to review, test, or take over whenever youâre ready.

Increasing the VM Size to 20GB

August 15, 2025

We heard your feedback about running into disk space limits on larger projects. To address this, weâve significantly increased the available disk space in the Jules VM to 20GB. This provides more room for large dependencies, build artifacts, and complex repositories, reducing disk-related failures so Jules can tackle bigger tasks. Happy Julesing!

Jules can surf the web

August 8, 2025

Jules can now proactively search the web for relevant content, documentation, or code snippets to help complete your tasks. This means Jules can get the information it needs, resulting in more accurate and successful task completion.

In Summary:

Jules can find the latest documentation for dependencies/libraries youâre using

Jules can proactively find examples or code snippets that can help inform its implementation

Note: web search works best when working on technical documentation. Queries like: âWhat is the latest news today?â are not supported.

Interactive Plan

August 8, 2025

Meet Interactive Plan. Instead of jumping straight to the solution, Jules will now read your codebase, ask clarifying questions, and work with you to refine the plan. This collaborative approach gives you more control and ensures youâre on the same page, leading to higher-quality code and a more reliable solution.

In summary:

Trigger the interactive plan from the dropdown when you start a task

Jules will start a brainstorm with you and ask clarifying questions

Critic Agent

August 8, 2025

Great developers donât just write code, they question it. And now, so does Jules. Weâve built the Jules critic agent to ensure that every line of code isnât just functional, but robust, secure, and efficient. It acts as an internal peer reviewer, challenging every proposed change to elevate the quality of the final output.

Some high level notes:

Critic-augmented generation: The Jules critic is integrated directly into the generation process. Every proposed change undergoes adversarial review before completion.

Improved code quality: The critic flags subtle bugs, missed edge cases, and inefficient code. Jules then uses this feedback to improve the patch in real-time.

A new kind of review: The critic is not just another linter or test. It understands the intent and context behind code, similar to a human peer reviewer.

Built on research: This feature draws on research into multi-step, tool interactive critiquing and actor-critic reinforcement learning, where an âactorâ generates and a âcriticâ evaluates.

Jules can test web-apps and show you the results

August 7, 2025

Next time you are working on a front end project with Jules, ask it to verify its work and itâll render the website and send you back a screenshot!

Ask Jules to complete a web development task and to verify the front end

Jules will send you a screenshot of the front end along with any code changes

The default Jules base image now includes Playwright for front end testing

Users can also add images in the form of public URLs for Jules to use as input

Jules is out of beta!

August 6, 2025

Today we are thrilled to announce that Jules is no longer in beta! Since launch just two months ago, Jules has passed over 140k public commits. Thank you to our amazing beta users for all your support and feedback.

In addition, weâre launching our pricing plans to unlock higher task limits, along with a bunch of quality improvements in the Jules app and agent. Here are the details:

Get higher task limits through the Google AI Pro and Ultra plans. More details at Limits and Plans.

Jules now uses the power of Gemini 2.5 thinking when creating its plan, resulting in higher quality plans and more complete tasks

Numerous bug fixes so Jules gets stuck less, and is better at following your instructions in agents.md

Environment snapshots for faster tasks

August 5, 2025

Jules now creates a snapshot of your environment when you add environment setup scripts. For complicated environment, users should see faster and more consistent task execution.

In summary:

Jules will now snapshot your environment when you provide an environment setup script

Snapshots are loaded automatically next time you run a task

This provides for faster task startups, especially for complex environments

You can find environment configuration by clicking the âcodebaseâ in the left hand panel, or by clicking the âconfigure environmentâ button in the task pane.

Open A PR directly from Jules

August 4, 2025

Closing the loop from task to merge ð¤

Jules can now open a pull request directly from the UI.
After a task completes, just use the new dropdown next to the âPublish Branchâ button to open a PR. Jules will request to merge the newly published branch into main, streamlining your entire workflow. Less context switching, faster merging.

Added Bun runtime support

July 18, 2025

Jules now supports Bun. You can run tasks using Bun out of the box, no extra setup required. This expands compatibility for projects that use Bun instead of Node.

Read more about the jules base image and what tooling works with Jules.

Improved task controls and other ð UI delight

July 3, 2025

Pause, resume, and delete tasksâwithout losing your sense of place. Available from sidebar and repo view. You can even quickly copy task urls!

Non-urgent task icons are now more recessive

Certain hover statesâwhich did not look goodâhave been toned back.

System messages have more consistent padding and borders

Performance upgrades: Enjoy a smoother, faster Jules experience with recent under-the-hood improvements.

Quickly copy and download code: New copy and download buttons are now available in the code view pane, making it easier to grab your code directly from Jules.

Stay focused with task modals: Initiate multiple tasks seamlessly through a new modal option, allowing you to keep your context and workflow intact. Learn more about kicking off tasks.

Adjustable code panel: Customize your workspace by adjusting the width of the code panel to your preferred viewing experience.

Check out the docs to learn more about how to download code that Jules writes.

A faster, smoother and more reliable Jules

May 30, 2025

This week, our focus has been on improving reliability, fixing our GitHub integration, and scaling capacity.

Hereâs whatâs we shipped:

Updated our limits to 60 tasks per day, 5 concurrent.

We substantially improved the reliability of the GitHub sync. Export to GitHub should also be fixed on previously created tasks.

Weâve decreased the number of failure cases by 2/3

Weâve been heads down improving stability and fixing bugsâbig and smallâto make Jules faster, smoother, and more reliable for you.

Hereâs whatâs fixed:

Upgraded our queuing system and added more compute to reduce wait times during peak usage

Publish Branch button is now part of the summary UI in the activity feed so itâs easier to find

Bug vixes for task status and mobile

Learn more about how to publish a branch on GitHub.

Jules is here

May 19, 2025

Today, weâre launching Jules, a new AI coding agent.

Jules helps you move faster by working autonomously on tasks in your GitHub repo. It can fix bugs, update dependencies, migrate code, and add new features.

Once you give Jules a task, it spins up a fresh dev environment in a VM, installs dependencies, writes tests, makes the changes, runs the tests, and opens a pull request. Jules shows its work as it makes progress, so you never have to guess what code itâs writing, or what itâs thinking.
