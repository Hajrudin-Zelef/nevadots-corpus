---
id: collect-261121-veille-ia/claude-anthropic/claude-code-context-window-size-compact-and-clear
title: "Claude Code Context Window: Size, /compact and /clear"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["context", "claude", "window", "code", "million", "usage", "compact", "conversation"]
source: https://postyourbuildideas.com/guides/claude-code-context-window
source_anchor: ""
source_lines: [1, 135]
section: "Claude_Anthropic"
---
# Claude Code context window: size, /compact, /clear and auto-compaction

As of October 2026, Claude Code runs Fable 5.1, Fable 5, Sonnet 5 and later, Haiku 5.5 and Opus 4.7 and later with a 1 million token context window by default. Opus 4.6 and Sonnet 4.6 get 1 million only through their [1m] variants. When the window nears its limit Claude Code compacts the conversation into a summary on its own. You can act earlier with /compact, start fresh with /clear, and check usage with /context.

## How big is the Claude Code context window?

The context window holds everything Claude knows about your session: your instructions, the files it reads, its own responses, and content that never appears in your terminal. Its size depends on the model. The numbers below come from the model configuration page of the Claude Code docs, checked in October 2026.

| Context window by model in Claude Code (October 2026) |  |  | 
|---|---|---|
| Model | Window | How you get it | 
|---|---|---|
| Fable 5.1 and Fable 5 | 1 million tokens | Default, no suffix needed | 
| Sonnet 5 and later (including Sonnet 5.5) | 1 million tokens | Default | 
| Haiku 5.5 | 1 million tokens | Default on the Anthropic API; prompts longer than 100K tokens cost more per token | 
| Opus 4.7 and later (including Opus 5.5) | 1 million tokens | Default | 
| Opus 4.6 | 200K, or 1 million with [1m] | Select claude-opus-4-6[1m]; included with Max, Ultra, Team and Enterprise plans, needs usage credits on Pro | 
| Sonnet 4.6 | 200K, or 1 million with [1m] | Select the [1m] variant; needs usage credits on every subscription plan | 

On the Anthropic API the 1 million window uses standard pricing with no premium for tokens beyond 200K, except on Haiku 5.5. To pin sessions to 200K, for example for compliance reasons, set CLAUDE_CODE_DISABLE_1M_CONTEXT=1. Which model is which is covered in the models guide.

## What fills the context window?

Some of it is full before you type anything. CLAUDE.md files, auto memory, MCP tool names and skill descriptions all load at the start of a session. From there each file Claude reads, each command output, each rule that loads for a matching file and each hook that adds text takes space. A subagent can do research in its own separate context window, and only its summary comes back to yours.

Two costs follow from this. Claude Code sends your full conversation with each request, so a long session re-reads all of it on every turn. That is discounted while the prompt cache is warm and full price once it expires. The cache lasts about an hour for the main conversation on a subscription and five minutes on an API key, a cloud provider or usage credits. Second, instructions from early in a conversation can get lost when it is summarized, which is why standing rules belong in CLAUDE.md, not in chat. The memory guide covers that file.

## How do you see how full your context is?

- **/context** shows live context usage as a colored grid, with optimization suggestions for context-heavy tools and memory bloat. It also lists which CLAUDE.md and auto memory files loaded. Pass all to expand the per-item breakdown.
- **The status line** can show a context usage percentage continuously. Configure it with /statusline.
- **/usage** shows session cost and, on subscription plans, plan usage with a breakdown of what counted against your limits. The usage limits guide covers those.

If auto-compaction keeps refilling, /context helps you find out why. Compare the Messages row against the others: if Messages is the largest, something in the conversation is refilling the window. If the other rows together are larger, too much loads at startup.

## What does /compact do?

Run /compact to free context while staying in the same conversation. Claude Code summarizes the conversation so far and replaces it with that summary. You can add instructions that tell it what to keep.

Do it before you start a long new task, so the summary keeps what you choose instead of what the automatic pass guesses matters. You can also set standing instructions in CLAUDE.md:

Compaction is itself a large request, because it reads the whole conversation to summarize it, more so once the cache has expired. As of v2.1.198 the summarization request inherits your session's extended thinking configuration. The table shows what survives.

| What happens to each kind of context after compaction |  | 
|---|---|
| Mechanism | After compaction | 
|---|---|
| System prompt and output style | Still apply | 
| Project-root CLAUDE.md and unscoped rules | Re-injected from disk | 
| Auto memory | Re-injected from disk | 
| The plan written in plan mode | Re-injected from disk | 
| Nested CLAUDE.md files and rules with paths: frontmatter | Reloaded on demand when Claude touches matching files | 
| Files Claude read or edited | Up to five re-read, most recently modified first; a file over 5,000 tokens comes back as a path reference | 
| Invoked skill bodies | Re-injected, capped at 5,000 tokens per skill and 25,000 total, oldest dropped first | 
| Background commands and subagents | Keep running | 
| Your earlier conversation | Summarized; detailed early instructions may be lost | 

The practical rule: anything that must survive belongs in the project-root CLAUDE.md, not in a path-scoped rule or in chat.

## When does auto-compaction run, and how do you change it?

Auto-compaction is on by default. Claude Code compacts when the conversation reaches the model's context limit, with these exceptions from the docs. Models with a native 1M window compact before it fills, at about 967K tokens by default. Opus 4.6 and Sonnet 4.6 without extended context compact at the 200K boundary, and so do native-1M models when you set CLAUDE_CODE_DISABLE_1M_CONTEXT=1. Cloud sessions compact as the conversation approaches the limit.

You can move that point earlier, which is useful if you would rather summarize often than carry a huge conversation.

Accepted values run from 100K to 1M tokens: a plain count such as 200000, a k or M suffix such as 500k or 1M, or a bare number from 100 to 1000 meaning thousands. Claude Code caps the window at the model's own. For every model at once, set autoCompactWindow in a settings file, for example 200000. In scripts and cloud environments, CLAUDE_CODE_AUTO_COMPACT_WINDOW takes precedence over the command, the flag and the setting, and a bare number there means tokens, not thousands. The /autocompact command needs Claude Code v2.1.221 or later.

Idle compaction is a separate default. In an interactive session on the one-hour cache, once a conversation passes 200,000 tokens and about an hour has gone by since the last request, Claude Code compacts it so your next message sends a summary. You see a notice starting with Compacted while idle, before the prompt cache expired. Set idleCompaction to false to stop it.

To turn off automatic compaction, set autoCompactEnabled to false, or set DISABLE_AUTO_COMPACT=1 for one session. Manual /compact keeps working. With the 200K hold and auto-compaction off, a session stops at the boundary with a context-limit error instead of compacting, so most people should leave it on.

## What is the difference between /clear, /compact and /rewind?

| Choosing between the context commands |  |  | 
|---|---|---|
| Command | What it does | Use it when | 
|---|---|---|
| /clear | Starts a new conversation with empty context. /clear name labels the old one in the /resume picker. Aliases /reset and /new | You switch to unrelated work, or two corrections failed and you want to restart with a better prompt | 
| /compact | Summarizes the conversation and keeps going in the same session. Accepts focus instructions | The task continues but the history is heavy | 
| /rewind | Rolls code, conversation or both back to an earlier point. Its Summarize from here and Summarize up to here options compact part of the conversation | You want to undo a wrong turn, or condense only a stale stretch | 
| /autocompact | Sets how full the window gets before the automatic pass | You prefer earlier, more frequent summaries | 

You can return to a cleared conversation with /resume. The best-practices page adds a rule of thumb: after two failed corrections, run /clear and write one better prompt that folds in what you learned, instead of piling on a third correction.

## How do you keep Claude Code's context lean?

1. **Clear between tasks.** /clear costs nothing, and the next task stops paying for the last one's history. Name the session first if you want to return: /clear oauth-migration.
2. **Compact with a focus** before a long new stage, not after the model has already drifted.
3. **Keep CLAUDE.md short.** Every line in it takes attention on every task. Keep what Claude cannot work out from the code: the exact test, lint and typecheck commands, misleading parts of the code, decisions that are not in the repo, and anything you have corrected twice.
4. **Scope rules to folders.** Put a rule that matters in one part of the codebase in .claude/rules/ with a paths: filter so it loads only there.
5. **Delegate big reads.** Send research to a subagent so file contents stay in its context window, not yours. The subagents guide shows how. The docs caution that each subagent costs extra tokens, so use it for noisy or parallel work.
6. **Read in pieces.** If a file or tool output is so large that context refills after every summary, ask Claude to read a specific line range or function.
7. **Watch background work.** A /loop, a goal check after every turn and subagents all keep sending requests while you are not typing, and each request carries your context.
8. **Run parallel jobs in separate sessions.** Isolated checkouts keep each conversation small; see the worktrees guide.

If you need an idea to practice on, pick a small one from the board. Each has a generated build prompt, which is a good model of a specific first message.

## What does “Autocompact is thrashing” mean?

It means automatic compaction worked, but a file or tool output immediately refilled the window several times in a row. Claude Code stops retrying after a few attempts instead of looping on calls that make no progress. The docs list four recovery steps:

1. Ask Claude to read the oversized file in smaller chunks, such as a line range or one function.
2. Run /compact with a focus that drops the large output, for example /compact keep only the plan and the diff.
3. Move the large-file work to a subagent so it runs in a separate context window.
4. Run /clear if the earlier conversation is no longer needed.

If the error returns after a /clear, run /context and compare the Messages row with the rows above it, as described earlier.

## Frequently asked questions

### How big is the Claude Code context window?

It depends on the model. As of October 2026, Fable 5.1, Fable 5, Sonnet 5 and later, Haiku 5.5, and Opus 4.7 and later run with a 1 million token window by default. Opus 4.6 and Sonnet 4.6 have 200K unless you select their [1m] variants, and access to those depends on your plan.

### What does /compact do in Claude Code?

It frees context by summarizing the conversation so far and continuing in the same session. You can add focus instructions, such as /compact keep the API changes and the test commands, so the summary keeps what you choose. CLAUDE.md, auto memory and the plan from plan mode are re-injected from disk afterward.

### What is the difference between /clear and /compact?

/clear starts a new conversation with empty context, so nothing from the old one is carried over. /compact keeps the same conversation but replaces its history with a summary. Use /clear when switching to unrelated work and /compact when the task continues. You can return to a cleared conversation with /resume.

### Does Claude Code compact automatically?

Yes. Auto-compaction is on by default and runs as the conversation approaches the model's limit, at about 967K tokens for models with a native 1M window. You can move that point earlier with /autocompact, or turn it off with autoCompactEnabled set to false or DISABLE_AUTO_COMPACT=1. Manual /compact still works.

### How do I check how much context I have used?

Run /context for a colored grid of current usage with suggestions, including which memory files loaded. You can also add a context percentage to the status line with /statusline. /usage shows session cost and, on subscription plans, how your usage counts against plan limits.

### Why does my Claude Code usage climb in a long session?

Claude Code sends your full conversation with every request, so a session open for hours keeps re-reading its history. That is discounted while the cache is warm and full price after it expires. Subagents, loops and compaction add their own requests. Run /clear between unrelated tasks to stop paying for old history.

Created with AI assistance and edited for this site. Examples are illustrative and do not represent customer research. About the publisher.
