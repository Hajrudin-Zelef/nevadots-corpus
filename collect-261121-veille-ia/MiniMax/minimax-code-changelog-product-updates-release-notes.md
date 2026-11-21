---
id: collect-261121-veille-ia/minimax/minimax-code-changelog-product-updates-release-notes
title: "MiniMax Code Changelog – Product Updates & Release Notes"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["download", "macos", "windows", "chat", "mini", "agent", "apple", "silicon"]
source: https://code.minimax.io/docs/changelog
source_anchor: ""
source_lines: [1, 1046]
section: "MiniMax"
---
- Desktop
- CLI
- Web

## macOS (Apple silicon)

Download v3.1.16

## macOS (Intel)

Download v3.1.16

## Windows (64-bit)

Download v3.1.16

## Windows (ARM64)

Download v3.1.16

- **Image, video, and document tools, ready to use:** Image generation, video generation, and the Word, Excel, PDF, and PPT plugins are installed by default. Just ask in the chat. Video tasks can ask for missing details and deliver the file when generation finishes.
- **Plugin suggestions when you need them:** When a task needs a plugin that is not ready, install or enable it, or connect the required account, right in the chat. The task then continues from your latest request without restating it.
- **Create, revisit, and manage Mini Apps:** Describe what you want on the Mini App home page to create an app and follow its progress. Recent cards keep a preview of your last session, favorites keep your go-to apps close, and running apps can be viewed, stopped, or closed all at once.
- **Pick up interrupted chats:** After a brief network drop or service issue, the chat can wait and resume. When your 5-hour quota runs out, you see the reset time and can choose to continue automatically after it resets. Completed work is kept.
- **Agent picks up from the current page:** Open a web page, Mini App, or supported Office document and let the Agent keep working on it. Reading and editing a selected Excel range is supported first.
- **Worktree progress you can see:** When you start a task in a new worktree, the chat shows setup and checkout progress, along with the reason if something fails. Branch names can reflect the task, and the environment bar shows the actual branch.
- **Answer in the chat card, then keep going:** When the Agent needs a decision, pick options right in the conversation card. Review and revise multiple answers before submitting them together; the session list shows when a reply is waiting, and a summary stays after you submit.

- Fixed some known issues.

## macOS (Apple silicon)

Download v3.1.14

## macOS (Intel)

Download v3.1.14

## Windows (64-bit)

Download v3.1.14

- **H3 Image is here:** MiniMax Code now supports MiniMax H3 Image, an open-source SOTA model for image generation and editing.

## macOS (Apple silicon)

Download v3.1.13

## macOS (Intel)

Download v3.1.13

## Windows (64-bit)

Download v3.1.13

- Fixed some known issues.

## macOS (Apple silicon)

Download v3.1.11

## macOS (Intel)

Download v3.1.11

## Windows (64-bit)

Download v3.1.11

- **Live voice typing:** Press Control+D on macOS or Ctrl+D on Windows and each sentence appears at your cursor as you speak. Type or edit while recording; nothing is sent automatically.
- **Flexible layout & floating input:** Keep the chat on the left or right, or go full screen to hide it. Read web pages and docs or create on the canvas with the Agent, without breaking your focus.
- **More Mini Apps at once:** The limit now scales with your computer’s memory, up to 10.
- **Easier Canvas editing:** Zoom into chat images and videos and send them to Canvas in one click. Canvas adds Remove Background and Enhance.
- **Sidebar sections:** Right-click a chat or project to group it into your own sections, or ask the Agent to organize your sidebar.
- **Bulk remove projects:** Remove multiple local projects at once in Manage Projects, with full paths shown.
- **Chrome sign-ins on Windows:** Import cookies from Chrome on Windows so the built-in browser stays signed in.
- **Edit PPT directly:** Open a local PPTX to edit text, charts, and animations, or select content and let the Agent revise it. Saves as native PPTX.

## macOS (Apple silicon)

Download v3.1.10

## macOS (Intel)

Download v3.1.10

## Windows (64-bit)

Download v3.1.10

- **Confirm before moving a Mini App to another window:** Moving a Mini App to a different window reloads its page. You’ll now see a confirmation first, letting you know that unsaved input and any recording, camera use or screen sharing in progress may be interrupted.

## macOS (Apple silicon)

Download v3.1.9

## macOS (Intel)

Download v3.1.9

## Windows (64-bit)

Download v3.1.9

**a single project span multiple folders**. Search now reaches into messages, web pages and files, and Mini Apps can use the camera and send system notifications. We’ve also fixed several stability issues in long conversations.

- **Multiple folders per project:** Add more than one folder to a project and switch between them in the file panel on the right. Adding folders isn’t supported for SSH remote projects yet.
- **Search that finds more:** Cmd/Ctrl+K now matches message content directly. Cmd/Ctrl+F follows whatever is open in the right panel, so you can search within a web page, a file, a side chat or a Mini App.
- **Find files faster with @:** Mentioning a file with @ supports fuzzy matching and returns results faster in large projects.
- **Custom project icons:** Pick an emoji and a color for each project. Your choice shows up on your other devices too.
- **See when the Agent needs you:** When the Agent is waiting on your answer, the sidebar marks that conversation with “Needs input”.
- **Smoother Goal mode:** You can edit queued messages, failures now come with a clear explanation, and progress steps can be collapsed. If a goal hits its budget limit, editing the goal lets it keep going.
- **Manage queued side chat messages:** Messages sent from a side chat to the main conversation that are still waiting in the queue can be edited, deleted or reordered.
- **Camera and notifications for Mini Apps:** Mini Apps can now request access to the camera and send system notifications. You can review these permissions before installing.
- **Pick up where you left off:** Switching back to a conversation takes you to where you were last reading.
- **IM follow-ups follow your settings:** Consecutive messages sent from an IM app are either queued or sent straight into the current task, based on Settings › Follow-up behavior.
- **Programmatic Tool Calling (PTC, Beta):** Turn it on in Settings › General, and the Agent can combine tool calls in code, which helps with batch work and multi-step tasks. It takes effect in new conversations or after context compaction.

- Fixed web search and website deployment failing in long conversations when the sign-in session expired.
- Reduced memory usage in long conversations.
- The app no longer modifies your shell config files or your Windows user PATH at startup.
- MiniMax Code now appears in your system’s “Open with” list, but no longer makes itself the default app.
- Pressing Cmd/Ctrl+R inside the built-in browser now reloads only the web page, not the whole app.
- When a link can’t be opened, you’ll see a message and can copy the link with one click.
- If a built-in Agent’s configuration file is missing or damaged, it’s restored automatically at startup, so the Agent can reply again.
- Computer Use overlays now appear only on the screen of the window being controlled, and desktop control locks left behind by a crashed session are released automatically.
- Fixed images with relative paths not showing in Markdown files in the right panel.

## macOS (Apple silicon)

Download v3.1.7

## macOS (Intel)

Download v3.1.7

## Windows (64-bit)

Download v3.1.7

- Fixed an issue where Agent questions inside Mini Apps (such as MiniMax Bot) didn’t appear and the task got stuck.
- Fixed some known issues

## macOS (Apple silicon)

Download v3.1.6

## macOS (Intel)

Download v3.1.6

## Windows (64-bit)

Download v3.1.6

**Hooks, next message suggestions and more flexible scheduled tasks**. You can now choose how the Agent asks you questions, including a new Auto mode. The built-in browser now has device preview, and you can check background task progress at any time. Your routine workflows run on their own, and you can see more clearly how each task is going.

- **Hooks are here:** Set up commands in Settings › Plugins › Hooks, and the Agent runs them automatically at key points in a task. A hook can apply to all projects or to just one.
- **Next message suggestions:** When a reply finishes, the input box suggests a message you could send next. Press Tab to fill it in and Enter to send it. Turn on “Next message suggestion” in Settings › General › Preferences. The feature is off by default. During Beta, it is available to subscribers using official models; BYOK models do not generate suggestions. Generating suggestions counts toward the selected model’s usage allowance.
- **More flexible scheduled tasks:** A scheduled task can now continue in an existing conversation. There’s also a new “Does not repeat” option for tasks that only need to run once.
- **Device preview for web pages:** Open “Device preview” in the built-in browser to switch between 13 preview options across desktop, tablet and mobile and see how your page looks.
- **See your background tasks:** Tasks running in the background all show up under “Background tasks”. You can check their progress or stop them at any time, and you’ll get a notification when they finish.
- **Agent questions, at your pace:** In Settings › General › Questions, choose Block, Skip or Auto. You can set the global default, Goal mode and Plan mode separately. In Auto mode, the Agent judges how important each question is. It waits for your answer on the ones that matter and skips the rest so it can keep working.

## macOS (Apple silicon)

Download v3.1.4

## macOS (Intel)

Download v3.1.4

## Windows (64-bit)

Download v3.1.4

- Fixed some known issues.

## macOS (Apple silicon)

Download v3.1.3

## macOS (Intel)

Download v3.1.3

## Windows (64-bit)

Download v3.1.3

- Fixed some known issues.

## macOS (Apple silicon)

Download v3.1.2

## macOS (Intel)

Download v3.1.2

## Windows (64-bit)

Download v3.1.2

- **Side Chat:** In a local conversation, enter`/side` or click**+** in the right panel to start a side chat with the current context while your main task keeps running. Select text from a reply to discuss it in a side chat, then promote that chat to a new main conversation when you want to take it further.
- **MiniMax Bot:** Build a team of Bots powered by Mini Apps, with group chats and scheduled tasks. Use`@` mentions to hand off work in a group chat. Long-running tasks continue in the background and return their results to the conversation. Each Bot has its own model, reasoning level, memory, and appearance settings.
- **Remote Control across devices:** Connect one phone to multiple computers, or connect to other computers from your desktop. Add a device with a pairing code, select the computer and working directory, and pick up its projects and existing conversations.
- **More file previews:** Preview local ZIP archives, XMind mind maps, email files, and SQLite data directly in MiniMax Code. Add supported mind map nodes, email text, or data cells to a conversation for further discussion or analysis.
- **Windows command environment:** Choose an available PowerShell or Git Bash installation in Settings for Agent commands. The choice applies to new conversations; existing conversations keep their current environment.

- **An improved Mini App experience:** Run analyses independently in a Mini App, track progress, and cancel midway without tying up the main conversation. Pop the workspace out into a separate window without losing its state, and find running Mini Apps in the conversation list.

## macOS (Apple silicon)

Download v3.1.1

## macOS (Intel)

Download v3.1.1

## Windows (64-bit)

Download v3.1.1

- **Usage resets:** Subscribers can view available reset cards and the past 30 days of grant and usage history in Usage settings. Select a card and confirm to reset the applicable limits.
- **Worktree selection:** Projects opened from an existing Git worktree now also support choosing a branch and where to run the task.

- **Usage and subscription reminders:** M Plan users receive reminders when their remaining quota is low or exhausted. Subscriptions without automatic renewal also receive reminders before expiration, with options to check usage, reset limits, or renew.
- **Session status:** The session list shows clearer indicators for running tasks and pending permission requests, including relevant activity in child sessions.
- **Goal results and file downloads:** Completed Goals show a final reply with delivered files in the results area. Choose a save location before downloading generated files.

- Fixed BYOK model order reverting after saving a drag-and-drop reorder.
- Fixed clipped double-digit list numbers, numbered questions being misread as headings, and incorrect rendering of single-line code blocks without a language label.
- Fixed missing files required by the DOCX skill in desktop installation packages.
- Fixed some conversation interruptions after questionnaire responses and context compaction, and improved automatic retries for model timeouts and certain network errors.

## macOS (Apple silicon)

Download v3.1.0

## macOS (Intel)

Download v3.1.0

## Windows (64-bit)

Download v3.1.0

- **A new look and feel:** Refreshed branding and visuals, with refined interactions for a smoother everyday experience.
- **Agent upgrades:** Improved Computer Use, Mini Apps, appearance customization, the Git Graph panel, and live Agent activity updates.
- **Introducing M Plan:** M Plan builds on Token Plan with unchanged subscription prices and text usage quotas. Select tiers include access to the full MiniMax model lineup, including H3.

- **50% off the first month:** Through October 14, new M Plan subscriptions and upgrades to M Plan receive 50% off the first month. Renewals return to the regular price from the second month.
- **Seven days of unlimited usage:** From October 1 through 7, all subscribers can use M3.1-Flash-Preview without usage limits in MiniMax Code.
- **Bonus points for Token Plan users:** All Token Plan users will receive bonus points within 24 hours of this release.

## macOS (Apple silicon)

Download v3.0.74

## macOS (Intel)

Download v3.0.74

## Windows (64-bit)

Download v3.0.74

- **M3.1-Flash-Preview:** MiniMax’s latest text model debuts in MiniMax Code, supporting everyday development work from bug fixes to complete feature implementation.

- **Double check-in points:** From September 28 through October 7, log in to MiniMax Code and complete the daily check-in to receive double free points. New and existing users can participate, and points can be used with all supported text and multimodal models.
- **Token Plan quota reset:** Token Plan quotas have been reset for all users following the launch of M3.1-Flash-Preview.

## macOS (Apple silicon)

Download v3.0.73

## macOS (Intel)

Download v3.0.73

## Windows (64-bit)

Download v3.0.73

- Fixed known issues.

## macOS (Apple silicon)

Download v3.0.72

## macOS (Intel)

Download v3.0.72

## Windows (64-bit)

Download v3.0.72

- **Local-to-cloud task handoff:** Hand local tasks to the cloud to continue work, review files before upload, then download cloud outputs or compare and sync changes back to local files.
- **Infinite Canvas HTML preview and annotations:** Drag HTML files onto the canvas for side-by-side previews, scroll through long pages, resize preview areas, and comment on elements or selected regions for the Agent to revise.
- **Voice input:** Use the microphone in the input box to dictate ideas with real-time transcription, appending new speech to any existing text.
- **Independent Subagent chats:** Start dedicated conversations with configured Subagents, and manage each custom Agent’s name, avatar, instructions, and model in one place.
- **In-app notifications:** A new notification center gathers release updates, official activities, tutorials, and account-related reminders.

## macOS (Apple silicon)

Download v3.0.71

## macOS (Intel)

Download v3.0.71

## Windows (64-bit)

Download v3.0.70

- Updated the macOS app signing certificate to improve installation and launch compatibility.

- Fixed known issues.

## macOS (Apple silicon)

Download v3.0.70

## macOS (Intel)

Download v3.0.70

## Windows (64-bit)

Download v3.0.70

- Fixed login-related issues.

## macOS (Apple silicon)

Download v3.0.69

## macOS (Intel)

Download v3.0.69

## Windows (64-bit)

Download v3.0.69

- **Infinite Canvas:** Gather images and videos from chats and the workspace into one canvas, then search, filter, preview, batch manage, and send selected assets back into the conversation.
- **Local and cloud sync:** The desktop app now shows both local and cloud sessions, and can start cloud work, view results, and download cloud task outputs directly.
- **H3 Max:** Added the H3 Max video model with text-to-video, image-to-video, 480p/768p output, and synchronized audio generation.
- **Custom Mini Apps:** Build a personal visual workspace with natural language, then keep adjusting pages, modules, and connected data sources.

- **AI watermark:** Image generation can now disable watermarks directly, with settings available under General; the default watermark style is also improved.
- **Git and code review:** Review all uncommitted changes or only the last Agent-generated changes, open source files from review, mark files as viewed, and keep Diff display preferences across tasks.

## macOS (Apple silicon)

Download v3.0.68

## macOS (Intel)

Download v3.0.68

## Windows (64-bit)

Download v3.0.68

- **Memory management:** Settings now include a clearer memory management view, and new chats can choose whether to use and generate memory.
- **Goal mode:** Improved long-running task execution by reducing repeated replies, looping, and interruptions, while keeping target edits and status tracking available during a run.
- **Browser Use:** Better page reading, form filling, and issue diagnosis for spotting errors, loading problems, and failed requests.
- **Remote Control:** Added artifact download and task notification cards, plus better remote Plan, Goal, branch, and Worktree management.

- **Settings search:** Quickly find model, connection, shortcut, Worktree, and custom instruction settings by keyword.
- **Project creation:** Improved the project creation flow and related guidance.
- **Permissions and stability:** Improved permission handling, browser verification, and runtime stability.

## macOS (Apple silicon)

Download v3.0.67

## macOS (Intel)

Download v3.0.67

## Windows (64-bit)

Download v3.0.67

- Fixed known issues.

## macOS (Apple silicon)

Download v3.0.66

## macOS (Intel)

Download v3.0.66

## Windows (64-bit)

Download v3.0.66

- **H3 video generation:** The flagship H3 video model is now integrated. Pair it with built-in Skills, Browser Use, Remote Control, and Goal to turn ideas into videos faster.
- **Site / Fork & Rewind / Plan:** Added site management, conversation branching and rewind, plus a Plan mode that reviews before editing.
- **MCP Servers:** Added a dedicated configuration page for viewing, adding, and managing MCP Servers in one place.
- **Text selection comments and preview:** You can comment on selected text, and preview flows for markdown, SVG, LaTeX, and similar files are smoother.

- **Worktree management:** View and clean up worktrees from Settings while protecting active, uncommitted, or locked directories.
- **Plugin Marketplace:** Added WPS and WeCom plugins for common office workflows.
- **BYOK:** Configure Model ID, context window, max output tokens, and reasoning level in custom model settings.
- **Goal:** Improved long-running task continuity, timer display, and pause, edit, and delete entry points.

## macOS (Apple silicon)

Download v3.0.65

## macOS (Intel)

Download v3.0.65

## Windows (64-bit)

Download v3.0.65

- Fixed known issues.

## macOS (Apple silicon)

Download v3.0.63

## macOS (Intel)

Download v3.0.63

## Windows (64-bit)

Download v3.0.63

- **Plugin Marketplace:** Added a built-in plugin marketplace with initial plugins for financial data, company lookup, legal search, knowledge workflows, and Office work.
- **Code Review:** Added a built-in Review command for checking local code changes and linking findings to specific code lines.
- **Red Skill Search:** Added Red Skill search.

- **Conversation Navigation:** Long conversations now support quick jumps to target turns.
- **Browser Use 2.0:** Added multi-tab support for opening, keeping, and switching between pages in the same task.
- **Account, Billing, and Usage:** Improved account, billing, and usage displays.
- **Windows Custom Directory:** Windows now supports a custom data directory.

## macOS (Apple silicon)

Download v3.0.60

## macOS (Intel)

Download v3.0.60

## Windows (64-bit)

Download v3.0.60

- Fixed an issue with file uploads in the conversation flow.

## macOS (Apple silicon)

Download v3.0.59

## macOS (Intel)

Download v3.0.59

## Windows (64-bit)

Download v3.0.59

- Fixed several known issues.

## macOS (Apple silicon)

Download v3.0.58

## macOS (Intel)

Download v3.0.58

## Windows (64-bit)

Download v3.0.58

- **Daily Check-In Rewards:** Check in daily to earn 400 points. Check-ins on days 4 and 7 award 1,000 points, for up to 4,000 points in a full week. Points are valid for 30 days and can be used across MiniMax Code tasks.
- **Text Selection Comments:** Select text, code, or table content in an Agent response to quote and comment on it. Multiple selections can be annotated and sent with the next instruction.
- **Proactive Memory:** MiniMax Code can retain useful long-term context such as communication preferences, work habits, project conventions, and problem-solving experience, then use it when relevant in future tasks.
- **Like and Dislike Responses:** Rate each Agent response individually and add a reason when needed, helping us improve response quality and task execution.

- **Usage and Billing:** The usage page now shows context-window usage details broken down by messages, tools, skills, system prompts, and memory. MiniMax Code also warns when usage is near or at its limit and provides relevant next steps.
- **Windows Office File Preview:** Windows desktop now supports PPTX previews alongside DOCX and XLSX. You can preview all three common Office formats directly in the app, including conversation attachments and local files.
- **Browser Use:** The built-in Browser now includes a mobile preview mode and a visible cursor with movement tracking during Agent operations. You can choose the built-in Browser or the system default browser separately for regular web pages and local development pages, with preferences saved automatically.

## macOS (Apple silicon)

Download v3.0.57

## macOS (Intel)

Download v3.0.57

## Windows (64-bit)

Download v3.0.57

- Fixed known issues.

## macOS (Apple silicon)

Download v3.0.56

## macOS (Intel)

Download v3.0.56

## Windows (64-bit)

Download v3.0.56

- Fixed network errors related to proxies.

## macOS (Apple silicon)

Download v3.0.55

## macOS (Intel)

Download v3.0.55

## Windows (64-bit)

Download v3.0.55

- Fixed a history display issue in edge cases.

## macOS (Apple silicon)

Download v3.0.54

## macOS (Intel)

Download v3.0.54

## Windows (64-bit)

Download v3.0.54

- **Browser Control:** MiniMax Code can now operate continuously inside the built-in browser: reading pages, filling forms, uploading files from the current task or workspace, and checking the result after each action. For outward-facing actions such as publishing, sending, or submitting, it asks for confirmation before proceeding. On first use, you can also confirm importing cookies from Chrome to reuse existing sign-in sessions; the import does not include browsing history, cache, or saved passwords.
- **Goal Mode:** Set complex or long-running work as a Goal so MiniMax Code can keep progressing around a clear objective. You can check the current status and elapsed time at any point. When more information or permission is needed, the Agent asks at the relevant step; when the Goal is completed or can no longer continue, it returns a clear result.
- **Feedback Center:** The issue reporting and response workflow has been upgraded. You can submit feedback from the lower-left menu or from a conversation, with task context, workspace, and client version attached automatically. Screenshots and diagnostic logs can be added, and log upload can be turned off if privacy is a concern. After submission, My Feedback shows the ticket ID and handling status.

- **File outputs in conversations:** File results and interactions in conversations are clearer. Files generated or referenced by MiniMax Code are presented as actionable results, and clicking them can locate the exact file, folder, or code line in the project.
- **Default shortcuts:** New default shortcuts are available, with a unified shortcut management entry in Settings. You can search, edit, delete, or restore shortcuts, and quickly trigger common actions such as creating a task, opening a project, searching, toggling the sidebar, opening the terminal, and switching tasks from the keyboard.

## macOS (Apple silicon)

Download v3.0.52

## macOS (Intel)

Download v3.0.52

## Windows (64-bit)

Download v3.0.52

- **Remote Control:** Connect to your desktop Agent from your phone to check progress, send instructions, and approve permission requests on the go.
- **Built-in Browser:** Open web pages and HTML artifacts directly in the app sidebar, so you can preview results, debug pages, and update code without leaving the task.
- **Coding / Work Modes:** Coding mode keeps your project environment, Git branches, worktree tools, and other development context close at hand. Work mode simplifies technical details so you can focus on task progress and final deliverables.
- **BYOK Model Configuration:** Use your MiniMax API key, or add a custom provider using a base URL, API key, and model name.

## macOS (Apple silicon)

Download v3.0.51

## macOS (Intel)

Download v3.0.51

## Windows (64-bit)

Download v3.0.51

- Fixed an issue where, after upgrading, some users could see an “LLM service error” in existing conversations, experience interrupted responses, or be unable to continue the conversation.

## macOS (Apple silicon)

Download v3.0.49

## macOS (Intel)

Download v3.0.49

## Windows (64-bit)

Download v3.0.49

- Reduced the time from starting a new conversation to the first output, cutting p95/p99 first-token latency by over 90%.
- Reduced unexpected interruptions and silent stalls during conversations, with clearer status feedback when errors occur.
- Improved how conversation state and tool-call data are saved and carried over, reducing cases where conversations are unexpectedly lost or execution context is interrupted.
- Reduced runtime dependencies for smoother performance on low-memory devices.
- Streamlined the Windows installation process to reduce setup time.

## macOS (Apple silicon)

Download v3.0.47

## macOS (Intel)

Download v3.0.47

## Windows (64-bit)

Download v3.0.47

- Fixed an issue causing service unavailability in certain proxy environments;
- Fixed a version data conflict between existing opencode installations and the built-in opencode version.

## macOS (Apple silicon)

Download v3.0.46

## macOS (Intel)

Download v3.0.46

## Windows (64-bit)

Download v3.0.46

- **Worktree base ref picker** : Choose base branch when creating new worktrees; cross-surface ref validation before submit.

- Context compaction (/compact) now shows full feedback loop: thinking indicator → pending state → auto-refreshed history.
- TurnDiffCard rendered per user input turn; Agent Team multi-turn shows one card at turn end only; scratchpad/memory changes excluded from diff.
- Sidebar visual details aligned with Figma; project sort order persisted; group position stable on archive.
- Agent Team switched to explicit user toggle (default team, opt-in single); deep-research skill retired (requires explicit /team).
- Skill Hub deduplication, Workspace panel scroll fix, slash palette ghost entry fix.
- User images render as images instead of file cards; questionnaire internal protocol text hidden.

- Stream stall heartbeat detection and recovery; tool-call blackhole passive recovery; SSE heartbeat extended.
- Windows: Team plan EPERM rename, plugin dir colon separator, dangling junction startup crash, TLS root CA trust via NODE_EXTRA_CA_CERTS.
- macOS Squirrel.Mac update race fix; GUI launcher ENAMETOOLONG tolerance; daemon health-check timeout raised to 60s.
- MCP JSON corruption graceful recovery; cron orphan session cleanup; bash timeout description consistency.
- /resume framework session id misjudgment; compaction regression revert; session restore loader flicker reduction.

- mavis-trash hardened: flag parsing, path resolution, root/PWD bypass prevention, dash-prefixed filename protection.
- Telegram/IM sender verification: owner-on-first-DM gate, hard-block non-p2p events, per-plugin opt-out.

- Computer Use has been temporarily disabled due to compatibility and user experience concerns. We are working on improvements and plan to bring it back in a future version.

## macOS (Apple silicon)

Download v3.0.40

## macOS (Intel)

Download v3.0.40

## Windows (64-bit)

Download v3.0.40

- **IM /stop Command** : Send`/stop` in WeChat, Feishu, or Telegram to interrupt the Agent’s current reply — same as the desktop Stop button.
- **IM Queue Notification** : Text-only channels like WeChat now show queue position when messages are waiting behind an active reply.
- **Cron Auto-Delivery** : Cron task results are automatically pushed to all IM channels bound to the Agent — no extra configuration needed.
- **IDE Quick Open** : New IDE picker in the chat top bar — open the current project in Cursor, VS Code, WebStorm, Zed, and 20+ other editors with one click.
- **Diff Preview on Hover** : Hover over a file change row to see a quick diff preview without opening the full review panel.
- **Skill Localization** : Built-in skills show localized names and descriptions in the slash command palette, with bilingual search support.
- **Deep Research Quick Access** : Deep Research now appears as a top-level entry in both the ”+” menu and slash palette.
- **Feedback Group Entry** : Sidebar user menu now includes a QR code to join the Feishu feedback group.

- Redesigned usage popover showing 5-hour window usage, weekly usage, video quota, and credit balance.
- Sidebar history defaults to project-grouped view with status dots and collapsible project sections.
- Standalone Thinking toggle button next to the model selector.
- Unified toolbar dropdowns with auto-flip near viewport edges.
- Feedback now creates a ticket with screenshots, task context, and diagnostic logs attached automatically, with status available in My Feedback.
- Markdown links open in the system browser; anchor links scroll in-page.
- First-screen HTTP requests merged and cached to reduce concurrent spikes.
- Code blocks in long conversations render lazily — syntax highlighting only triggers when scrolled into view.

- Fixed CLI failing to start on Windows when install path contains Chinese characters.
- Fixed mavis-trash following NTFS junctions and deleting target data on Windows.
- Fixed “Reveal in File Explorer” not selecting the target file on Windows.
- Fixed GPU crash loop on Windows devices with Intel integrated graphics.
- Fixed deleted cron tasks still showing in the sidebar.
- Fixed OpenCode session state lost after restart (session not found).
- Fixed tooltip text unreadable in dark mode.

- File reads outside the workspace directory are now subject to permission mode review.

## macOS (Apple silicon)

Download v3.0.36

## macOS (Intel)

Download v3.0.36

## Windows (64-bit)

Download v3.0.36

- **Run Location Picker** : Choose where the Agent runs when starting a conversation — current project, a fresh branch, or an existing branch. Replaces the old Worktree toggle.
- **Code Change Review** : New file preview panel with diff comparison and one-click commit for reviewing Agent code changes.
- **Sidebar Search** : Search conversation history by keyword in the desktop sidebar.
- **Environment Variable Management** : Securely manage secrets and environment variables within conversations for Agent use.
- **Deep Research** : Built-in five-step research skill that auto-searches, analyzes multiple sources, and produces evidence-backed reports.
- **Agent Team Enhancements** : New task detail panel, automatic stuck-task recovery for long-running tasks, and reduced polling during decision waits.
- **Queue Messages** : Send follow-up messages while the Agent is still working — they’re queued and injected at the right time.
- **Task History by Project** : Sidebar supports grouping conversation history by project directory.
- **Disable Global Shortcut** : The global launch shortcut can now be left empty to disable it, avoiding conflicts with other apps.

- Redesigned home toolbar with Team mode toggle moved into the menu.
- Slash command panel keeps skill submenus open on hover. Multi-image preview order aligned with thumbnails.
- Sidebar Agent list UX unified with session history items.

- Fixed built-in Skill compatibility on Windows — now auto-routes through Git Bash.
- Fixed PowerShell corrupting UTF-8 files on Windows.
- Fixed enterprise proxy environment variables causing connection issues.
- Fixed WeChat QR binding not refreshing after expiry.
- Fixed missing recovery prompt when conversation context overflows.

## macOS (Apple silicon)

Download v3.0.33

## macOS (Intel)

Download v3.0.33

## Windows (64-bit)

Download v3.0.33

- **Desktop Rebrand** : Desktop app officially renamed to MiniMax Code. New name appears in Dock / Taskbar, with a new minimax CLI shortcut added.
- **Enhanced Questionnaire** : The multi-step questionnaire now supports close, skip, and free-text replies. Enabled by default for all users.
- **Skill Auto-Discovery** : Automatically scans and imports existing skills from Claude Code, Codex, and other AI coding tools — no manual reinstall needed.
- **Quick Scroll Navigation** : Click the “scroll to bottom” button once to jump to the previous question, double-click to go to the top.

- Slash command names simplified — redundant prefixes auto-stripped, case-insensitive matching.
- AI reply action buttons left-aligned to body text. File picker dialog centered on screen.
- Scheduled task auto-archive off by default. Task runs refresh on click.
- Sidebar Agents section collapsed by default. Project selector auto-flips when bottom space is tight.
- Onboarding and branding visual updates.

- Fixed Windows install path and shortcut issues during brand migration.
- Fixed permission system not handling file deletions correctly in full-authorize mode.
- Fixed schedule agent selector display issue.

## macOS (Apple silicon)

Download v3.0.32

## macOS (Intel)

Download v3.0.32

## Windows (64-bit)

Download v3.0.32

- **Clear Default Conversation** : The home page default conversation now supports a clear action. Notification toggle persists across sessions.
- **Auto-Generated Archive Titles** : Archived conversations now get a short AI-generated topic summary as the title for easier browsing.
- **Sidebar Context Menu** : Pinned and history sessions now support right-click actions. “Bookmarks” section renamed to “Pinned” for clarity.
- **Windows Install Improvements** : Auto-creates desktop shortcut, warns about system-protected directories, auto-configures firewall rules on first launch.
- **Smart Power Management** : System sleep prevention now activates only when Agent is actively running, saving battery during idle time.
- **Settings Shortcut** : Ctrl+, shortcut to open settings on Windows.

- Input box only auto-detects links with explicit http(s):// protocol, preventing false positives on regular text.
- @ file mention search results show full file path on hover.
- Feishu IM card interaction and notification style improvements.
- Long code replies now scroll horizontally. Multiple visual refinements.

- Fixed schedule agent selector incorrectly showing internal project agents.
- Fixed IM message filter rules not persisting across restarts.
- Fixed project selector dropdown direction in mini-window mode.
- Improved SSL connection compatibility for some enterprise networks.

## macOS (Apple silicon)

Download v3.0.31

## macOS (Intel)

Download v3.0.31

## Windows (64-bit)

Download v3.0.31

- More accurate permission handling for compound commands and file deletions.

- Fixed permission rules not applying correctly in some scenarios.

## macOS (Apple silicon)

Download v3.0.30

## macOS (Intel)

Download v3.0.30

## Windows (64-bit)

Download v3.0.30

- **Skill Lifecycle Management** : New “Needs Attention” and “Archived” filters in the Skill panel. Keep, archive, pin, or restore skills with one click. Unused skills are auto-archived with notifications.
- **Batch Skill Import** : Preview GitHub repo contents before installing. Select and install multiple skills from the same repo in one go.
- **Multi-Step Questionnaire** : Agent can present structured multi-step questions with single/multi-select options, free-text input, and images.
- **Content Filter Graceful Fallback** : When the model can’t complete a response, the original message returns to the input box with a friendly notice instead of an error. Rephrase and resend directly.
- **Avatar Click-to-Upload** : Click the Agent avatar on the home page to upload a custom image.
- **Message Hover Timestamp** : Hover near the copy button to see when a message was sent.
- **Worktree Mode Toggle** : Opt into isolated code development mode when starting a new conversation.
- **Subscription Tier Display** : Current subscription tier shown in user menus with one-click upgrade.

- Markdown heading levels auto-optimized in chat for cleaner visual hierarchy.
- Unified folder permissions: one authorization covers read, write, edit, and search — no repeated prompts for the same folder.
- Smarter permission handling for common command patterns, significantly fewer prompts in daily use.
- Schedule panel and IM connection panel visual polish.

- Fixed occasional “Service Start Failed” after desktop app restart.
- Fixed memory leak when closing tabs.
- Fixed multi-line commands being incorrectly blocked by the permission system.
- Fixed Agent tool calls passing inaccurate parameters in some scenarios.
- Windows firewall blocking now shows dedicated setup guidance.

## macOS (Apple silicon)

Download v3.0.29

## macOS (Intel)

Download v3.0.29

## Windows (64-bit)

Download v3.0.29

- Agent reflection entries now surface as cards instead of occupying the chat stream.
- User-level memory writes now require a reason to prevent project-specific info from leaking into personal memory.

## macOS (Apple silicon)

Download v3.0.28

## macOS (Intel)

Download v3.0.28

## Windows (64-bit)

Download v3.0.28

- **In-Session Search (Cmd+F / Ctrl+F)** : Search the current conversation with keyboard shortcuts. Supports keyword highlighting, prev/next navigation, and IME input.
- **Native MCP Tool Exposure** : Selected MCP tools can now be registered as model-native tools for improved call efficiency and success rate.
- **Windows Recycle Bin Support** : File deletions now go through the system Recycle Bin, making accidental deletions recoverable.
- **Permission Review Reasons** : Permission cards now display LLM review rationale, helping users understand why confirmation is needed.
- **Chat Scroll Progress Bar** : A scroll progress indicator in long conversations for quick orientation.

- Permission mode dropdown reordered: Ask First → Smart Authorize → Always Authorize.
- “Always Allow” label unified across UI and Feishu to match native OS wording.
- Temp directory and home directory reads auto-allowed, reducing unnecessary permission prompts.

- Introduced HARD / SOFT dual-tier risk registries — high-risk commands blocked immediately, medium-risk routed to LLM contextual review.
- Added detection for Windows delete commands, disk erasure, reverse shells, and interpreter inline deletions.

## macOS (Apple silicon)

Download v3.0.27

## macOS (Intel)

Download v3.0.27

## Windows (64-bit)

Download v3.0.27

- **Credit & Usage Limit System** : Clear prompts when credits are exhausted with direct links to subscription renewal — no more infinite loading.
- **Home Page Quick Actions** : Shortcut buttons below the input box to quickly start creating teams, slides, PDFs, documents, and spreadsheets.
- **Feishu One-Click Authorization** : Automatic user authorization after Feishu app registration — one scan to get the recommended permission set.
- **Desktop Notifications** : System notifications when Agent encounters errors or needs permission confirmation.
- **Enhanced Scheduled Tasks** : New hourly frequency option, /watch shortcut command, and improved task result cards.
- **Deeplink Support** : Open specific conversations directly via links.

- Markdown typography overhaul: headings, tables, code blocks, and blockquotes redesigned per Figma specs.
- Project selector improvements: Windows paths shown as folder names with full path tooltips on hover.
- Activity panel redesign with privacy safeguards and day-by-day navigation.

- Permission classifier upgraded from binary to three-state (allow / confirm / block), with 90+ common commands on the fast track.
- Catastrophic commands now require explicit user confirmation and cannot be bypassed.

- Fixed API failures causing infinite Agent restarts.
- Fixed SSE reconnection process leak (100+ processes observed on Windows).
- Fixed orphaned processes remaining after Electron quit.
- Fixed onboarding page reappearing after app updates.

## macOS (Apple silicon)

Download v3.0.26

## macOS (Intel)

Download v3.0.26

## Windows (64-bit)

Download v3.0.26

- **Simplified Permission Modes** : Permission mode selection streamlined to Smart Authorize and Always Authorize.
- **Scoped Permission Cards** : Permission prompts now only show requests from the active session tree.
- **Compression Image** : Optimized image compression pipeline for faster uploads.

- Fixed IM message reply errors.
- Multiple UI stability improvements.

## macOS (Apple silicon)

Download v3.0.25

## macOS (Intel)

Download v3.0.25

## Windows (64-bit)

Download v3.0.25

- Built-in Skills status corrected to show as installed, ensuring proper invocation.
- Migration guide popup visual upgrade with clearer copy.

- Fixed multiple Windows compatibility issues.

## macOS (Apple silicon)

Download v3.0.24

## macOS (Intel)

Download v3.0.24

## Windows (64-bit)

Download v3.0.24

- **Composer Image Preview** : Preview attached images before sending a message.
- **Sidebar No-Project Indicator** : Sessions without an associated project now display a dot icon for quick identification.
- **Workspace Panel Optimization** : Working Folders area supports internal scrolling without expanding the page.
- **Keep-Awake Default** : New installations default to preventing system sleep, ensuring long-running tasks are not interrupted.

- Unified avatar component with consistent visual style.
- Dark mode scan light effect fixed.
- Team member usage display compatibility improvements.

- Fixed data resumption errors when switching sessions.
- Fixed queued messages being incorrectly aborted.

## macOS (Apple silicon)

Download v3.0.23

## macOS (Intel)

Download v3.0.23

## Windows (64-bit)

Download v3.0.23

- **Sidebar Pin & Drag-to-Reorder** : Pin frequently used sessions to the top and reorder them via drag-and-drop.
- **Session-Level Message Management** : Manage and organize messages at the session level.
- **Work Insights** : Activity panel now includes work insight analysis with multimodal model awareness.
- **Auto-Compaction Summaries** : Long conversations display collapsible summary blocks when auto-compacted.

- Session rotation mechanism upgraded for smoother transitions.
- IM message filtering improvements.
- Daemon login state automatically cleared on logout.

- Fixed IM multi-message send pipeline issues.
- Fixed Windows installer display name.

## macOS (Apple silicon)

Download v3.0.22

## macOS (Intel)

Download v3.0.22

## Windows (64-bit)

Download v3.0.22

- **Rich Text Input** : Input box upgraded to a rich text editor with support for richer formatting.
- **@File References** : Reference files with @ in conversations — Agent can read and process them directly.
- **Mini Window Defaults to Main Agent** : Quick-launch mini window now defaults to the main Agent.

- Improved send-immediately and loading experience.
- Fixed multiple permission and security edge cases.

## macOS (Apple silicon)

Download v3.0.21

## macOS (Intel)

Download v3.0.21

## Windows (64-bit)

Download v3.0.21

- **Unified Upload Flow** : File upload pipeline unified for a more consistent experience.

- Agent Team waiting state display improvements.
- Scroll and expand/collapse interaction polish.

- Fixed Electron build signing symlink issues.
- Fixed plugin SDK dependency cold-start delay.
- Fixed MCP tool calls losing tokens.

## macOS (Apple silicon)

Download v3.0.20

## macOS (Intel)

Download v3.0.20

## Windows (64-bit)

Download v3.0.20

**MiniMax Code Desktop fully upgraded to the next-generation architecture**— rebuilt from the ground up for more powerful Agent capabilities and a smoother experience.

- **Agent Persona System** : Configure independent personas for each Agent, including personality, behavior style, and domain expertise.
- **Cross-Session Memory** : Three-layer memory system (User / Agent / Project) for persistent learning across conversations.
- **Multi-Agent Collaboration** : Multiple sub-agents work in parallel, automatically splitting and assigning complex tasks.
- **Security & Permissions** : Built-in content safety and permission management — Agent asks for confirmation before executing sensitive operations.
- **MCP Tool Integration** : Connect to the MCP tool ecosystem to extend Agent capabilities.
- **Multi-Channel Access** : Integrated with Feishu, Telegram, and other IM platforms for 24/7 real-time conversations.
- **Scheduled Tasks** : Create recurring tasks with natural language — Agent executes and reports results automatically.
