---
id: collect-261121-veille-ia/gemini-google/antigravity-update-tracker-full-change-log-for-2-0-ide
title: "Antigravity Update Tracker: Full Change Log for 2.0 & IDE"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: ["2026-11-18"]
keywords: ["antigravity", "gemini", "neon", "google", "hydrogen", "fixfixed", "november", "pull"]
source: https://techpp.app/update-tracker/antigravity-update-tracker
source_anchor: ""
source_lines: [1, 775]
section: "Gemini_Google"
---
# Antigravity Update Tracker

Almost everyone, including OpenAI and Anthropic Claude, has their own IDE app. Google is also creating one, and they are calling it Antigravity. Antigravity also hosts other models, which is its best advantage. Compared to others, the pricing is also lower, making it the best option for people looking for affordability.

However, Antigravity is also known for its bugs and frequent changes without informing users. And if you are an Antigravity user, you should definitely know the latest changes that are coming to your app. In this guide, we track all the latest updates to Google's Antigravity across both Antigravity 2.0 and the Antigravity IDE.

Official release notes: Antigravity changelog.

## Antigravity Releases & Updates

## Model launch
### Gemini 4 Neon and Orchestrator (Neon)

November 18, 2026

### Gemini 4 Neon and Orchestrator (Neon)

Adds Gemini 4 Neon, which becomes the default model if you haven't picked one, and Orchestrator (Neon), which plans with Neon and hands the work to Gemini 4 Hydrogen.

- IMPROVEDGemini 4 Neon is available on Google AI Pro, Google AI Ultra and Gemini Enterprise, with four thinking levels.
- IMPROVEDNeon is now the default model if you haven't chosen one.
- IMPROVEDOrchestrator (Neon) plans with Gemini 4 Neon and runs Gemini 4 Hydrogen subagents in parallel to do the work.

## 2.33.0
### SSH connections and GitHub pull request status

November 17, 2026

### SSH connections and GitHub pull request status

Adds SSH connections to remote Linux machines, live GitHub pull request status, Cmd/Ctrl+F search in the built-in browser, and /swarm and UI plugin panels for business and enterprise accounts.

- IMPROVEDConnect to a remote 64-bit Linux machine over SSH from the sidebar, the project menu or the command palette. Projects, agents and terminals run on the host and keep working after you close the window.
- IMPROVEDGitHub pull request links show live status pills with hover cards, and the Overview panel gets a Pull Requests section.
- IMPROVEDSearch inside pages in the built-in browser with Cmd+F or Ctrl+F.
- IMPROVEDBusiness and enterprise accounts can now use /swarm and open UI plugin panels when their admin allows it.
- IMPROVEDAudio and video files up to 200 MB open in the side panel, up from 20 MB.
- IMPROVEDAdds the Nord dark theme, R and R Markdown highlighting, and partner plugins from Cloudflare, Grafana Cloud, SonarQube and others.

- FIXFixed going offline or a short network failure signing you out.
- FIXFixed macOS showing Keychain Not Found dialogs every hour after a keychain write failed.
- FIXFixed timed-out Git status checks on Windows leaving git.exe processes running and using gigabytes of memory.
- FIXFixed attaching an empty text, Markdown, JSON or HTML file making every later message in the conversation fail.
- FIXFixed long reasoning turns timing out after five minutes of thinking.

## 2.32.2
### Multi-turn /btw side questions

November 16, 2026

### Multi-turn /btw side questions

Side questions asked with /btw now open in their own tab, where you can ask follow-ups without interrupting the main conversation.

- IMPROVED/btw opens a Side Question tab where you can ask multi-turn follow-ups, and the agent can read files, search your workspace and run read-only commands.

- FIXFixed an issue that could cut off diagnostic logs.

## 2.32.1
### Gemini 4 Hydrogen with Google Cloud credentials

November 13, 2026

### Gemini 4 Hydrogen with Google Cloud credentials

Adds Gemini 4 Hydrogen to the model picker when you sign in with Google Cloud application default credentials.

- IMPROVEDWith Google Cloud application default credentials, you can pick Gemini 4 Hydrogen at low, medium or high thinking.

## 2.32.0
### Dynamic Workflows and Everforest themes

November 13, 2026

### Dynamic Workflows and Everforest themes

Adds Dynamic Workflows for running parallel subagents and multi-step tasks in Python, Everforest Light and Dark themes, prompt navigation shortcuts and stronger protections for browser tabs the agent controls.

- IMPROVEDDynamic Workflows let agents run parallel subagents and multi-step tasks in Python, using the runtime bundled with the app.
- IMPROVEDAdds Everforest Light and Everforest Dark themes.
- IMPROVEDKeyboard shortcuts jump to the previous or next prompt in a conversation, and you can bind an optional Submit Prompt shortcut.
- IMPROVEDIf the browser redirect fails during sign-in, you can copy a sign-in link or paste a one-time code.
- IMPROVEDCross-site frames in tabs the agent controls load signed out and stay hidden from the agent unless you allow them.
- IMPROVEDWith remote agents, you can clone Cloud GitHub repositories locally from the workspace picker.

- FIXFixed some websites blocking the built-in browser.
- FIXFixed quitting the app leaving background processes running.
- FIXFixed bold Chinese, Japanese and Korean text showing at regular weight.
- FIXFixed scrolling up through long conversations stalling or losing your place.

## Model launch
### Gemini 4 Hydrogen and Orchestrator

November 12, 2026

### Gemini 4 Hydrogen and Orchestrator

Adds Gemini 4 Hydrogen, a fast model for coding and agent work, and Orchestrator, which plans with Gemini 4 Carbon and hands the work to Hydrogen.

- IMPROVEDGemini 4 Hydrogen is available with four reasoning effort levels: low, medium, high and xhigh.
- IMPROVEDOrchestrator switches between Gemini 4 Carbon and Hydrogen in one conversation: Carbon plans and coordinates while Hydrogen runs the worker and checker agents.
- IMPROVEDGoogle says Orchestrator gets close to Carbon's results while using 24% less quota and finishing 19% faster on average.
- IMPROVEDHydrogen is available in Antigravity for Google AI Pro and Ultra subscribers.

## 2.31.2
### Gemini 4 Carbon with Google Cloud credentials

November 11, 2026

### Gemini 4 Carbon with Google Cloud credentials

Adds Gemini 4 Carbon to the model picker when you sign in with Google Cloud application default credentials.

- IMPROVEDWith Google Cloud application default credentials, you can pick Gemini 4 Carbon at low, medium or high thinking.

## 2.31.1
### Gemini Enterprise sign-in fix on Windows

November 11, 2026

### Gemini Enterprise sign-in fix on Windows

Fixes Gemini Enterprise sign-in on Windows dropping your license and making requests fail.

- FIXFixed signing in to Gemini Enterprise on Windows with a Workforce Identity Federation account dropping your license, switching your plan to Agent Platform and causing permission errors.

## Model launch
### Gemini 4 Carbon

November 10, 2026

### Gemini 4 Carbon

Brings Gemini 4 Carbon, Google's strongest model for long software engineering tasks, knowledge work, science and cybersecurity, to Antigravity.

- IMPROVEDGemini 4 Carbon is now available in Antigravity.

## 2.31.0
### /swarm, font size setting and browser download approvals

November 9, 2026

### /swarm, font size setting and browser download approvals

Adds /swarm for coordinating many subagents on long tasks, a font size setting, plugins in split view and approvals for browser agent downloads.

- IMPROVEDType /swarm to coordinate several subagents over a shared message board on long tasks.
- IMPROVEDSet the interface font size to 12, 14 or 16 pixels in Settings > Appearance.
- IMPROVEDFull-page plugin views can share split view with a conversation.
- IMPROVEDFiles the browser agent downloads wait for your approval, and pages it controls can no longer lock your mouse or keyboard.
- IMPROVEDVoice call transcripts show a short summary, and spoken answers move multi-question prompts along.
- IMPROVEDInstalling a plugin or skill from a card in the agent's reply resumes the conversation on its own.

- FIXFixed sign-in getting stuck on "Signing in..." for some personal accounts.
- FIXFixed the reading position jumping when content loaded above it.
- FIXFixed voice mode failing for up to two minutes after one failed connection.
- FIXFixed requests failing when a thinking model took over a minute to start answering.

## 2.30.0
### Knowledge Work layout and a setup that fits your work

November 9, 2026

### Knowledge Work layout and a setup that fits your work

Adds a simpler Knowledge Work layout for research and writing, asks about your work at sign-in to set things up, and opens voice mode and remote agents to more people.

- IMPROVEDA simpler Knowledge Work layout for research and writing hides the coding tools (not for business or enterprise accounts).
- IMPROVEDSign-in asks what kind of work you do and sets up a matching layout with suggested starting points.
- IMPROVEDVoice mode is now available to everyone except business and enterprise accounts, and remote agents reached Ultra and paid Pro subscribers.
- IMPROVEDYou can import MCP servers, skills, commands, subagents and instructions from Claude Code, Codex and Cursor into a plugin.
- IMPROVEDYour messages now show as chat bubbles on the right, and the sidebar and title bar share one background.
- IMPROVEDPermission presets now apply only to the current conversation, and the Auto preset is no longer marked Preview.

- FIXFixed signing in to Google with a phone passkey in the built-in browser failing with a Bluetooth message.
- FIXFixed queued follow-ups being sent while subagents or background tasks were still running.
- FIXFixed Python and PowerShell output on Windows garbling emoji and non-English text.
- FIXFixed the Review tab saying diff contents were no longer available for subagent changes.

## 2.29.1
### Voice mode fixes for sign-in, links and transcripts

November 6, 2026

### Voice mode fixes for sign-in, links and transcripts

Fixes several voice mode issues. Voice mode now asks you to sign in again when needed, and it can see the links and @ mentions in messages you type.

- FIXFixed voice mode showing "Couldn't connect" when your sign-in needed an update. It now asks you to sign in again.
- FIXFixed voice mode missing links and @ mentions in typed messages, and skipping messages that held only a link.
- FIXFixed voice mode sometimes joining something you said earlier in a call to your latest words, which could stop it from answering.
- FIXFixed the transcript showing parts of a reply you never heard after a brief drop in a voice call.

## 2.29.0
### Voice mode, remote agents and a redesigned Automations page

November 3, 2026

### Voice mode, remote agents and a redesigned Automations page

Starts rolling out voice mode with Gemini Live and remote agents that run in the cloud, and adds redesigned Home and Automations pages, a Jupyter notebook viewer, and downloads and history in the built-in browser.

- IMPROVEDVoice mode, powered by Gemini Live, lets you talk to Antigravity and hand requests to the agent. Start it from the button in an empty message box. It is rolling out gradually.
- IMPROVEDRemote agents are rolling out to Google AI Ultra and Pro subscribers. Conversations in a cloud project run in the cloud, and you can open them at antigravity.google.com/remote.
- IMPROVEDThe Automations page now sorts automations into Scheduled, Always on and Paused lists with next and last run times.
- IMPROVEDJupyter notebook (.ipynb) files open in the file viewer with code cells, Markdown, tables, images and outputs.
- IMPROVEDThe built-in browser saves downloads straight to your downloads folder, suggests pages from your history, and has a redesigned address bar.
- IMPROVEDYou can change keyboard shortcuts in Settings > Shortcuts, and Settings > Appearance adds a full-width Fill option, a font menu and a Screen Reader Mode toggle.
- IMPROVEDAntigravity starts faster from a cold start.

- FIXFixed Antigravity not restoring its window size, position and full-screen state after a restart.
- FIXFixed Sign in with Google and OAuth popups failing inside the built-in browser.
- FIXFixed a message sent while the agent was idle going into the queue instead of starting right away.
- FIXFixed a background database watcher looping and causing high CPU usage.

## 2.28.2
### Partner plugins in the Marketplace and plugin sign-in

October 29, 2026

### Partner plugins in the Marketplace and plugin sign-in

Adds partner plugins like Figma, Linear, Notion and Stripe to the Marketplace with a Featured spotlight, and lets you sign in to a plugin's account right after you add it.

- IMPROVEDThe Marketplace now has partner plugins from Canva, Figma, Linear, Notion, Stripe, Supabase and Vercel on new shelves.
- IMPROVEDA new Featured spotlight sits at the top of the Marketplace.
- IMPROVEDConnected plugins get an Accounts tab on their page, so you can sign in right after adding one (not yet for business or enterprise accounts).
- IMPROVEDThe agent can suggest a plugin from the Marketplace with an add card (not yet for business or enterprise accounts).
- IMPROVEDThe separate Conversation History page is gone. Search now sits behind a button at the top left, and archived conversations are a filter in the sidebar.

## 2.28.1
### Conversation navigator and third party tools notice

October 28, 2026

### Conversation navigator and third party tools notice

Adds a navigator for jumping around long conversations, a notice on third party plugins, and developer, version and link details on plugin pages.

- IMPROVEDA rail with one dot per turn lets you hover to preview a turn and click to jump to it. Long chats are grouped by date.
- IMPROVEDThe Marketplace and third party plugin pages show a notice that links to Google's privacy policy.
- IMPROVEDPlugin pages show the developer, version, and website and repository links.

## 2.28.0
### Built-in browser and attachments of any file type

October 27, 2026

### Built-in browser and attachments of any file type

Adds a built-in browser in the side panel, a built-in PDF viewer, attachments of any file type up to 20 MB, and word-level diff highlighting.

- IMPROVEDWeb pages now open in a built-in browser in the side panel, and links open there by default (you can change this in settings or from the right-click menu).
- IMPROVEDPDFs open in a built-in viewer.
- IMPROVEDAttach any file type, up to 20 MB.
- IMPROVEDDiffs highlight the exact words and characters that changed, and added text is easier to see in dark mode.
- IMPROVEDCmd+Shift+T or Ctrl+Shift+T reopens side panel tabs you closed.
- IMPROVEDScreen readers now announce agent activity, responses, and cancellations or failures.

- FIXFixed conversations switching models after a restart or reconnect.
- FIXFixed some enterprise accounts getting signed out after a restart.
- FIXFixed subagents asking again about commands or sites you had already approved.
- FIXFixed plugin installs failing on a broken leftover folder; there is now a Replace option.

## 2.27.0
### Markdown previews and overview sections in new side panel tabs

October 26, 2026

### Markdown previews and overview sections in new side panel tabs

Shows Markdown code blocks as rendered previews with a Raw toggle, and fills new side panel tabs with overview sections.

- IMPROVEDNew side panel tabs show overview sections, with documents and artifacts first and search pinned at the top.
- IMPROVEDMarkdown code blocks render as previews by default, with a Raw toggle.
- IMPROVEDLong conversations open and run faster.
- IMPROVEDDisabled plugins get an Enable button on their page.
- IMPROVEDThe Marketplace and Installed search boxes get a clear button.

- FIXFixed subagents in auto mode asking you to approve every command outside the sandbox.
- FIXFixed a sign-in screen flashing at startup.
- FIXFixed Mermaid diagrams missing from PDF exports.
- FIXFixed plugin installs failing right after an update.

## 2.26.0
### Notification settings, docked prompts and palette filters

October 22, 2026

### Notification settings, docked prompts and palette filters

Adds a Notifications page in Settings, docks question and permission prompts above the message box so you can keep typing, and adds sort and filter options to the Conversations palette.

- IMPROVEDA new Notifications page in Settings lets you turn completion sounds on or off and set the agent's sound volume.
- IMPROVEDQuestion prompts and permission requests now sit above the message box instead of replacing it, so you can keep typing or queue a follow-up.
- IMPROVEDThe Conversations palette (Cmd+P or Ctrl+P) can sort by Last Updated or Last Prompt and filter by unread, automations or archived conversations.
- IMPROVEDMCP servers installed by a plugin get a Configure button, so you can update their variables without turning the plugin off and on.
- IMPROVEDPin and Archive were added to the sidebar's more-actions menu for conversations.

- FIXFixed the agent stopping instead of retrying after socket errors when the computer wakes from sleep or switches Wi-Fi networks.
- FIXFixed Git diffs failing for files in subfolders on Windows.
- FIXFixed a failed undo silently resetting the transcript and overwriting your draft.
- FIXFixed LaTeX display math inside lists and paragraphs not rendering as block equations.

## 2.25.0
### Plugin sidebar badges and window shortcuts

October 20, 2026

### Plugin sidebar badges and window shortcuts

Shows a plugin badge on projects managed by plugins, hides empty plugin projects, and fixes window shortcuts and conversation search.

- IMPROVEDProject folders managed by UI plugins show a plugin badge in the sidebar.
- IMPROVEDEmpty projects managed by plugins are hidden from the sidebar.
- IMPROVEDOnboarding shows the Google Cloud project auto-assignment panel when no licenses are pre-assigned.

- FIXFixed skill and rule descriptions containing colons breaking YAML front matter parsing.
- FIXFixed Cmd+W not closing the window when no tabs or panels were open.
- FIXFixed conversation search failing when its summaries database was corrupted; it now recovers on its own.
- FIXFixed the app's Dock presence disappearing on macOS after closing the last window.

## 2.24.0
### Redesigned side panel, /compact and model choice for automations

October 15, 2026

### Redesigned side panel, /compact and model choice for automations

Redesigns the side panel with an overview of everything in a conversation, adds a /compact command for long chats, and lets you pick the model an automation runs on.

- IMPROVEDThe redesigned side panel has an overview that gathers artifacts, changed files, links and uploads from the conversation in one place.
- IMPROVEDType /compact to summarize a long conversation and free up room for the agent to keep working.
- IMPROVEDAutomations can run on Flash Lite, Flash or Pro. New automations still start on Flash.
- IMPROVEDWith your own Google Cloud project on Vertex AI, the model picker hides third-party models your project cannot use, and ADC sign-in can offer Claude Opus 5.5 and Claude Sonnet 5.5.
- IMPROVEDA new Auto Pin option pins a conversation whenever you message it (off by default).
- IMPROVEDCmd+Shift+M or Ctrl+Shift+M maximizes the side panel, and Cmd+Shift+V or Ctrl+Shift+V pastes plain text.

- FIXFixed every sandboxed command failing with "Access is denied" on Windows when PowerShell came from the Microsoft Store.
- FIXFixed an incomplete WebM video making every later message in a conversation fail.
- FIXFixed renamed conversations not showing up when you searched for the new name.
- FIXFixed an empty model list after signing out and back in.

## 2.23.1
### License picker fix

October 13, 2026

### License picker fix

Fixes the license picker during onboarding, which could grow taller than the window and hide the buttons below it.

- PATCHLong license lists in the onboarding license picker now scroll inside the card instead of covering the buttons.

## 2.23.0
### Permissions picker, full-screen media viewer and .zip attachments

October 12, 2026

### Permissions picker, full-screen media viewer and .zip attachments

Adds a permissions picker to the message box, a full-screen viewer for images and videos, and .zip and folder attachments. Onboarding now shows what Antigravity can do instead of asking you to pick a theme.

- IMPROVEDChoose the agent's permissions preset from a picker in the message box, or switch a project back to your global preset.
- IMPROVEDImages and videos open in a full-screen viewer with zoom, pan, playback speed and keyboard controls.
- IMPROVEDAttach .zip archives (the agent unpacks them) or add a whole folder from the + menu.
- IMPROVEDWhen an MCP server fails to start, the Customizations page offers Fix with Agent, and MCP server menus get a Restart option.
- IMPROVEDThe sidebar shows a needs-attention icon when a subagent is waiting for your approval.
- IMPROVEDThe archive shortcut is now Cmd+Option+A on macOS and Ctrl+Alt+A on Windows and Linux.

- FIXFixed the terminal not opening on Windows until the app lost and regained focus.
- FIXFixed a finished notification arriving after you stopped the agent yourself.
- FIXFixed custom agents ignoring skills, rules, plugins and hooks listed with relative paths.
- FIXFixed restarting an MCP server sometimes leaving it stopped.

## 2.22.0
### Sidebar drag and drop and UI extensions

October 8, 2026

### Sidebar drag and drop and UI extensions

Lets you drag conversations between sidebar sections, adds UI extensions that plugins can show in the side panel, and switches timestamps to month names.

- IMPROVEDInstalled plugins can show their own custom interfaces in the side panel.
- IMPROVEDDrag and drop conversations between sections in the sidebar to organize projects.
- IMPROVEDConversation timestamps now use month names instead of numeric dates.

## 2.21.1
### Advanced search, Automations and grouped agent actions

October 7, 2026

### Advanced search, Automations and grouped agent actions

Adds content search with Cmd+K or Ctrl+K, renames Scheduled Tasks to Automations, and groups the agent's file edits, commands and reads into one collapsible row.

- IMPROVEDCmd+K (Ctrl+K on Windows) searches conversations by their contents.
- IMPROVEDScheduled Tasks are now called Automations, with Run Now and a Create with Prompt option that walks you through a new one.
- IMPROVEDFile edits, terminal commands and file reads between replies collapse into one row with a short summary.
- IMPROVEDAdded the Gruvbox Material theme preset for light and dark modes.
- IMPROVEDAuto-generated conversation titles now consider images attached to your first message.

- FIXFixed settings breaking after the settings file was saved in Windows Notepad or PowerShell.
- FIXFixed a truncated MP4 or MOV video making every later message in a conversation fail.
- FIXFixed switching conversations sometimes showing the wrong model.
- FIXFixed Rust files and code blocks showing without syntax highlighting.

## 2.19.1
### Message subagents, Markdown to PDF and conversation-only undo

September 30, 2026

### Message subagents, Markdown to PDF and conversation-only undo

Lets you message a subagent directly, export rendered Markdown as a PDF, and undo only the conversation.

- IMPROVEDSend a message straight to a subagent from the message box.
- IMPROVEDExport rendered Markdown in artifacts and side pane files as a PDF, including tables, code blocks and diagrams.
- IMPROVEDThe Confirm Undo dialog can revert only the conversation.
- IMPROVEDOn Windows and Linux, clicking the tray icon reopens or focuses the window.

- FIXFixed custom agents ignoring your global and project rules.
- FIXFixed an expanded terminal command step collapsing again when the command finished.

## 2.18.1
### Plugin marketplace in the Customizations tab

September 28, 2026

### Plugin marketplace in the Customizations tab

Adds a Customizations tab with a marketplace for finding, installing and managing plugins, plus a long list of sidebar and chat changes.

- IMPROVEDDiscover, install and manage plugins in the new Customizations tab, with curated shelves for developer tools and workspace integrations.
- IMPROVEDTerminal commands in permission requests are syntax highlighted, so they're easier to read before you approve them.
- IMPROVEDSlash commands from plugins show the plugin's name in the / menu.
- IMPROVEDThe token usage breakdown shows rules and customizations as separate bars against their own budgets.
- IMPROVEDOption+Command+P on macOS or Alt+Shift+P on Windows and Linux pins the active conversation.

- FIXFixed files in .git, .env and .vscode folders being editable without a permission prompt under the Default and Request Review presets.
- FIXFixed Google Sign-In failing with a browser security error.
- FIXFixed scrolling stalls and jumps in long conversations.
- FIXFixed copied dark mode text carrying a dark background into Google Docs or Gmail.

## 2.17.0
### Plan before you build

September 22, 2026

### Plan before you build

Type /plan and the agent drafts a plan you can read and edit before it writes code. The setting behind it is now called Plan Review Policy.

- IMPROVED/plan drafts an editable plan first, and Plan Review Policy lets you review every plan, only when the agent thinks it is worthwhile, or never.
- IMPROVEDRules get their own 20,000-token budget, so a big rules file no longer crowds out skills, subagents and MCP tools.
- IMPROVEDPer-project settings now load from .gemini/config.json; the older .agents/settings.json is no longer read.
- IMPROVEDCustom agents can declare their own hooks in their front matter.
- IMPROVEDProjects get a Reveal in Finder (Show in File Explorer on Windows) menu item.

- FIXFixed rewinding a conversation sometimes restoring out-of-date files.
- FIXFixed large files and diffs freezing the app; files over 100 KB now show as plain text.
- FIXFixed custom rules, workflows and agents failing to load on large projects.
- FIXFixed /btw side questions falling back to the default agent.

## 2.16.0
### WSL support, Office attachments and live subagent cards

September 22, 2026

### WSL support, Office attachments and live subagent cards

Connects to a Windows Subsystem for Linux distribution, accepts Word, Excel and PowerPoint attachments, and shows subagents as live cards you can follow and stop.

- IMPROVEDConnect to an installed WSL distribution from Application Settings on Windows.
- IMPROVEDDrag Word, Excel and PowerPoint files into a prompt, and Gemini models read them natively.
- IMPROVEDSubagents show as live cards with running, waiting and completed status and a stop control.
- IMPROVEDRight-click an image to copy or save it.
- IMPROVEDSkill and customization discovery on large projects dropped from over a minute to under a second.

- FIXFixed reopening an older conversation re-applying its old file edits over newer work.
- FIXFixed browser sign-in failing for good if the app reloaded or the computer slept mid sign-in.
- FIXFixed web search returning no summary with reasoning models.

## 2.15.1
### Windows sandbox support

September 19, 2026

### Windows sandbox support

Adds file and network sandboxing on Windows.

- IMPROVEDFile and network sandboxing are now supported on Windows.

## 2.15.0
### Custom agent controls and keyboard navigation

September 18, 2026

### Custom agent controls and keyboard navigation

Lets custom agents switch off the default prompts and tools, remembers your chosen agent after a reload, and adds keys for moving around a conversation.

- IMPROVEDCustom agents can turn off the default prompt sections and tools and add back only what they need.
- IMPROVEDPage Up, Page Down, Home and End scroll a conversation, and Escape leaves the prompt box.
- IMPROVEDThe custom agent you pick is remembered after a reload.
- IMPROVEDSwitching conversations in the sidebar is faster.

- FIXFixed saved settings and the project list being overwritten when the app failed to read its state file.
- FIXFixed duplicate permission entries making conversation files huge and slowing the app.
- FIXFixed a false crash recovery notice when starting a new conversation.

## 2.14.0
### Terminal and Git for Enterprise and Business

September 15, 2026

### Terminal and Git for Enterprise and Business

Brings the integrated terminal and Git version control in the sidebar to Enterprise and Business accounts, and makes long conversations load faster with less memory.

- IMPROVEDEnterprise and Business accounts can use the integrated terminal and Git version control in the sidebar.
- IMPROVEDLong conversations load faster and use less memory.
- IMPROVEDSkills, rules and customization files are no longer rescanned on every message, so messages are acknowledged sooner.
- IMPROVEDSettings, artifacts and terminals opened in their own tab load faster.
- IMPROVEDScratch files the agent creates are no longer watched, synced or shown in the Artifacts panel and Review pane.
- IMPROVEDThe conversation menu was reorganized, with Share and Move to Group added to the right-click menu.
- IMPROVEDPermissions labels in settings were renamed to Global Permissions and Inherit Global.

- FIXFixed screen reader announcements for side questions being repeated or dropped when browsing question history.
- FIXFixed the agent producing Mermaid diagram types the app cannot render.

## 2.13.0
### Documents sidebar and virtualized artifact viewer

September 9, 2026

### Documents sidebar and virtualized artifact viewer

Adds a Documents section for external files, a virtualized viewer for code and data artifacts, and a hide-whitespace option in diffs.

- IMPROVEDAdded a Documents section in the sidebar, above Artifacts, for external files such as Google Drive links, PDFs, and Office documents.
- IMPROVEDCode and data artifacts such as SQL and JSONL now open in a virtualized viewer with syntax highlighting, line numbers, and inline comments.
- IMPROVEDAdded a Hide Whitespace Changes option to the Review Changes overflow menu and the file diff viewers.
- IMPROVEDClosing a side question now minimizes it to a button in the composer toolbar instead of clearing it.

- FIXFixes across interface consistency, accessibility, and stability.

## 2.12.2
### Gemini 3.8 Flash for enterprise

September 3, 2026

### Gemini 3.8 Flash for enterprise

Lets enterprise users pick Gemini 3.8 Flash through Application Default Credentials.

- IMPROVEDEnterprise users can select and use Gemini 3.8 Flash via ADC for agentic tasks.

## 2.12.0
### Quoting responses and the /boost command

September 2, 2026

### Quoting responses and the /boost command

Adds response quoting, a /boost multi-agent reasoning command for paid users, and settings, chat, and sidebar improvements.

- IMPROVEDHighlight part of an Antigravity response to quote it as context for a follow-up prompt.
- IMPROVEDNew /boost slash command raises thinking effort using a multi-agent reasoning pipeline.
- IMPROVEDGeneral Settings now shows which projects override a setting and links straight to that project's settings.
- IMPROVEDTerminal split layouts are preserved across window reloads.
- IMPROVEDThe side question panel gained back and forward controls for stepping through questions asked in a session.
- IMPROVEDYou can send a message with the send button while dictation is still running.

- FIXFixes across chat interactions and sidebar navigation.

## 2.11.0
### Generative UI and interface improvements

August 26, 2026

### Generative UI and interface improvements

Adds generative UI capabilities and a set of interface improvements.

- IMPROVEDAdded generative UI capabilities.
- IMPROVEDImproved the Antigravity interface.

## 2.10.0
### Embedded terminals and Git controls

August 24, 2026

### Embedded terminals and Git controls

Adds embedded terminals, built-in Git version-control tools, and audio attachments.

- IMPROVEDAdded embedded terminal support.
- IMPROVEDAdded Git version-control controls.
- IMPROVEDAdded audio attachments.

## 2.9.1
### Remote Control and performance

August 20, 2026

### Remote Control and performance

Improves Remote Control and overall performance.

- IMPROVEDImproved Remote Control workflows.
- IMPROVEDImproved performance.

## 2.8.1
### Responsiveness improvements

August 13, 2026

### Responsiveness improvements

Improves responsiveness across the application.

- IMPROVEDImproved application responsiveness.

## 2.8.0
### Persistent sidebar folders

August 12, 2026

### Persistent sidebar folders

Adds persistent sidebar folders and resolves crash issues.

- IMPROVEDSidebar folders now persist.

- FIXFixed crash issues.

## 2.7.1
### Reliability, accessibility, and image diffs

August 11, 2026

### Reliability, accessibility, and image diffs

Improves reliability, performance, accessibility, and image-diff workflows.

- IMPROVEDImproved accessibility and image diffs.
- IMPROVEDImproved performance and reliability.

## 2.6.0
### Faster long conversations and more reliable agents

August 7, 2026

### Faster long conversations and more reliable agents

Makes long conversations open faster and improves custom hooks, subagents, connected-tool policies, chat, comments, and the file viewer.

- IMPROVEDImproved long-conversation loading, hook validation, subagent behavior, and media previews.

- FIXFixed reliability and interface issues across chat, comments, and the file viewer.

## 2.5.0
### Enterprise sign-in and administrator controls

July 31, 2026

### Enterprise sign-in and administrator controls

Adds Gemini Enterprise account sign-in and Workforce Identity Federation through Advanced SSO.

- IMPROVEDAdded enterprise sign-in, regional model inference, reasoning-effort controls, and administrator policies.

- FIXIncluded reliability fixes for enterprise and general workflows.

## 2.4.3
### Preview tabs, attachments, and MCP timeouts

July 28, 2026

### Preview tabs, attachments, and MCP timeouts

Adds preview tabs, JSON, Markdown, and CSV attachments, MCP connection and tool-call timeouts, keyboard shortcuts, and performance fixes.

- IMPROVEDAdded preview tabs, more attachment formats, MCP timeouts, diagnostics export, and unread conversation filtering.

- FIXFixed issues affecting agents, queued messages, diffs, text selection, Markdown rendering, and notifications.

## 2.3.1
### Startup stability fix

July 16, 2026

### Startup stability fix

Fixes a startup failure caused by an empty or malformed Antigravity configuration file.

- PATCHFixed malformed or empty config.json files preventing Antigravity from loading.

## 2.3.0
### Queued messages and text attachments

July 13, 2026

### Queued messages and text attachments

Adds queued messages, immediate sending, plain-text attachments, and stability improvements.

- IMPROVEDAdded queued-message controls, plain-text file attachments, faster startup, and improved command permission handling.

- FIXFixed conversation search, side questions, background tasks, large diffs, and agent hangs.

## 2.2.1
### Guide skill, audio playback & smarter search

June 30, 2026

### Guide skill, audio playback & smarter search

Adds a built-in Antigravity Guide skill, audio file playback in the sidebar and artifact viewer, smarter file search, performance boosts, and bug fixes.

- IMPROVEDNew built-in Guide skill answers questions about Antigravity itself.
- IMPROVEDAudio file playback (.mp3, .wav, .ogg, .m4a) now renders in the sidebar file viewer and artifact viewer.
- IMPROVEDSmarter file search for finding files across a project faster.
- IMPROVEDPerformance improvements plus refinements to the chat, workspace, and diff UI.

- FIXVarious critical bug fixes.

## 2.1.4
### Quota redesign, PDF attachments & /btw

June 11, 2026

### Quota redesign, PDF attachments & /btw

Adds a redesigned quota screen, PDF attachment support, and a new /btw side-question command, alongside the Gemini CLI to Antigravity CLI transition.

- IMPROVEDQuota screen redesign in the Models tab of Settings gives a clearer view of used versus remaining credits.
- IMPROVEDAdded PDF attachment support so PDFs can be attached directly to a conversation.
- IMPROVEDNew /btw slash command sends a side question to an ephemeral, single-response agent that still has the current conversation's context.
- IMPROVEDGemini CLI is transitioning to Antigravity CLI.

## 2.0.6
### Antigravity IDE integration

May 22, 2026

### Antigravity IDE integration

Added integration with Antigravity IDE.

- IMPROVEDAdd an install IDE button if Antigravity IDE is not installed.
- IMPROVEDAdd an open IDE button if Antigravity IDE is installed which allows you to open the current project in Antigravity IDE.

## 2.0.1
### AGY 2.0 Bug Fixes

May 19, 2026

### AGY 2.0 Bug Fixes

Antigravity 2.0 Launch bug fixes.

- FIXFixed project migration issues for projects with CJK characters in their titles.
- FIXFixed an issue that caused duplicate projects to be created when importing from Antigravity 1.0.
- FIXResolved an issue where Google One credits were not being applied or utilized.

## Comments

0
Share your thoughts, questions, or feedback about this guide.

## Leave a Comment
