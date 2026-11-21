---
id: collect-261121-veille-ia/claude-anthropic/claude-fable-5-2-error-or-not-working-fix-by-the-exact-message
title: "Claude Fable 5.2 Error or Not Working: Fix by the Exact Message"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-11-09"]
keywords: ["fable", "claude", "model", "code", "message", "update", "anthropic", "claude-fable-5-2"]
source: https://blog.laozhang.ai/en/posts/claude-fable-5-2-error
source_anchor: ""
source_lines: [1, 149]
section: "Claude_Anthropic"
---
## On this page

Claude Fable 5.2 has been out since November 9, 2026, and "not working" covers several unrelated failures, each with its own fix; the exact text on your screen tells you which one you have. In Claude Code, start with three checks. Anthropic lists 2.1.321 as the minimum version for Fable 5.2, so if `claude --version` shows anything older, run `claude update` or update the desktop app or IDE extension you work in. The model ID is `claude-fable-5-2` with hyphens, and `claude-fable-5.2` with a dot fails with "There's an issue with the selected model". If Fable 5.2 is simply missing from the `/model` picker, type `/model claude-fable-5-2`, because that picker bug survives the update to 2.1.322.

In Claude Code, "You've reached your Fable limit" means your plan's Fable allowance is used up, not that something broke. Pro and standard Team seats have Fable included only until November 16, 2026 at 11:59 pm PT and pay for it with usage credits after that, and the Free plan has no Fable model at all. A message saying Fable 5.2's safeguards flagged your message comes from Anthropic's safety classifier, so edit the message or switch to Opus with `/model`. If your own code started getting 400 errors after you changed the model ID, the request shape is the problem: Fable 5.2 rejects disabled thinking, a forced `tool_choice`, assistant prefill, the older `computer_20251124` tool and edited history sent back with its thinking blocks.

A few cases aren't yours to fix. Repeated 529 errors and "Fable is experiencing high load" mean Anthropic is short of capacity for this model, so switch models and watch status.claude.com. A safeguards block tagged `[reasoning_extraction]` has no automatic fallback. On Enterprise plans an admin has to enable Fable 5.2, and an organization without 30-day data retention can't use it until that setting, or its agreement with Anthropic, changes. If you reach Fable 5.2 through a relay, a clean 200 or a vague 400 may not mean what it would on Anthropic's own API.

## Fable 5.2 error messages: what each one means and the fix

Find the line you see, or the closest symptom, in the table below. Claude Code prints these messages in English only, so apart from version numbers and model IDs the quoted text should match yours word for word; the safeguards, Fable limit and high-load lines appear verbatim in the Claude Code 2.1.322 binary. Rows that need more than one step have their own section further down.

| Message or symptom | Where you see it | Cause | What to do | 
|---|---|---|---|
| `… does not support this model; version … or newer is required` | Claude Code | The binary that sent the request is older than 2.1.321 | Update that binary: `claude update` , the desktop app, the IDE extension or the Agent SDK package | 
| Fable 5.2 missing from `/model` ; the Fable row still says Fable 5.1 | Claude Code 2.1.322 and later, CLI and IDE | Open picker bug | Type `/model claude-fable-5-2` | 
| `fable` runs Fable 5.1, with no warning | Claude Code before 2.1.321 | The old client maps the alias to 5.1 | Update, then pick Fable again | 
| `Claude Code doesn't recognize the model "claude-fable-5-2", so it assumes a context window of 200k tokens` | Older CLI, desktop app Code tab | The client has no entry for Fable 5.2 | Update; see the 200k section | 
| `There's an issue with the selected model (claude-fable-5.2)` | Claude Code | Dotted ID, no access, or a stale ID in settings | Use `claude-fable-5-2` or`fable` | 
| Session label changed to Haiku 4.5 | Claude desktop app | Reported after an app update or a forced sign-in | `/model claude-fable-5-2` | 
| Fable row grayed out with a note | Claude Code on an API key | The organization can't use Fable, for example under zero data retention | Admin or Anthropic account team | 
| `Fable 5.2's safeguards flagged this message` …`Details: [cyber]` | Claude Code, Claude app | Safety classifier | Edit the message, accept the Opus fallback or switch models | 
| `You've reached your Fable limit.` | Claude Code | Fable share of your plan used up | Usage credits or another model | 
| `Fable limit reached · continuing on Fable 5.2 uses usage credits, and the prompt to confirm went unanswered — nothing was sent` | Remote Control, background, agent-team and SDK-hosted sessions | Usage-credit consent prompt timed out | Answer it where the session runs | 
| `API Error: Repeated 529 Overloaded errors` or`Fable is experiencing high load` | Claude Code, desktop app | Anthropic capacity | Switch models, check status.claude.com | 
| `… Invalid` …`The block is bound to a different conversation.` | Your code on the Messages API; Claude Code adds `API Error: 400` | History edited before thinking blocks were sent back | Keep history append-only | 
| `400 In order to access this model, your organization or workspace must have data retention enabled.` | API, Claude Code on an API key | No 30-day data retention | Enable it for a workspace | 
| 400 `invalid_request_error` right after switching the model ID | Your code on the Messages API | A request shape Fable 5.2 rejects | See the API 400 table | 
| 200 with an empty text block and `stop_reason: "max_tokens"` | Messages API | Thinking used the whole `max_tokens` budget | Raise `max_tokens` | 
| 503 "no available channels" or a 400 with no reason | A relay | The relay's own response | See the relay section | 

## /model picker omits Fable 5.2, though /model claude-fable-5-2 works

As of November 14, 2026, this is an open Claude Code bug, not an account problem: on 2.1.322 the `/model` picker's Fable row still offers Fable 5.1, yet typing `/model claude-fable-5-2`, or starting with `claude --model claude-fable-5-2`, runs Fable 5.2 in the same session. Users have reported it on macOS, Windows, Linux and WSL, in VS Code and JetBrains, on Max, Ultra, Team and personal plans, in GitHub issues anthropics/claude-code#108955, #109316 and #108898, and Anthropic's Claude Code changelog lists no fix through 2.1.325 (November 13, 2026). On one Team plan the Default row even resolved to Fable 5 while the Fable row offered 5.1 (#108924).

Anthropic's Claude Code model configuration page describes only two picker states for Fable: a row grayed out with a note when your organization can't use Fable at all, for example under zero data retention, and a row marked "Requires usage credits" when Fable bills to credits. A row that is missing or stuck on 5.1 is neither, which is why the threads treat it as a bug. The people in those threads traced it to four setups. `DISABLE_TELEMETRY=1`, for example set through `"env"` in `~/.claude/settings.json`, stops Claude Code from loading the list of models your account can use, and the debug log shows `[servedCatalog] off (flag)` (#108726). The VS Code extension sets that same variable itself when VS Code's `telemetry.telemetryLevel` is `off`, and VSCodium turns telemetry off by default. A custom `ANTHROPIC_BASE_URL` pointing at a gateway or proxy logs `[servedCatalog] off (not_first_party)`. And some users with none of these still had only `claude-fable-5-1[1m]` in the `additionalModelOptionsCache` entry of `~/.claude.json` after a refresh on November 11.

To get Fable 5.2 back while the bug is open:

1. Type `/model claude-fable-5-2` in the session, or start Claude Code with`claude --model claude-fable-5-2` . The reports agree that this works, and it is the same model the picker would have set.
2. Run `claude --version` . Below 2.1.321 the picker has no Fable 5.2 to show at all, so update first and come back to step 1.
3. If you set `DISABLE_TELEMETRY` yourself, remove it and restart Claude Code. In VS Code, turning telemetry back on stops the extension from setting it; in VSCodium, users reported success with`"CLAUDE_CODE_ENABLE_TELEMETRY": "1"` and`"DISABLE_TELEMETRY": ""` in the extension's environment settings.
4. Behind a gateway, or if nothing above helps, keep selecting the model by ID, or list `claude-fable-5-2` under the`modelPicker` setting so it gets its own row. The entry's exact format is on the Claude Code settings reference for modelPicker; the snippets in the GitHub comments are users' versions, not Anthropic's.

## Picked "fable" but got Fable 5.1? Claude Code before 2.1.321

On Claude Code older than 2.1.321, the `fable` alias still means Fable 5.1, and nothing on screen says so. A test on November 14, 2026 ran Claude Code 2.1.316 and 2.1.322 in `-p` mode on macOS against a local mock server that recorded what each version sent. With `--model fable`, 2.1.316 sent `claude-fable-5-1` at effort high with `max_tokens` 64000 and printed no warning, while 2.1.322 sent `claude-fable-5-2` at effort medium with `max_tokens` 128000. A mock run on November 10 with 2.1.316 and 2.1.321 gave the same result. The test shows what the client sends, not how the model behaves, but it means that a "Fable" session on an old build is a Fable 5.1 session, whatever its answers feel like.

When Anthropic's API refuses a request from a client that is too old, it returns a 400 with the code `claude_code_version_too_old`, and Claude Code prints a line such as `API Error: 400 Claude Code 2.1.219 does not support this model; version 2.1.255 or newer is required. Run 'claude update', or update the Claude desktop app, then try again.` That example on the Claude Code error reference is for Fable 5.1; for Fable 5.2 the floor is 2.1.321. The version checked is that of the binary that made the request, so update whichever one sent it: the CLI, the desktop app, the VS Code extension or the Agent SDK package.

A current client can still land on 5.1 because of a saved setting. On Pro, Max, Ultra, Team and Enterprise plans, a `claude-fable-5-1` saved in your user settings now moves to the `fable` alias, but Console accounts, third-party providers, and project, local or managed settings keep `claude-fable-5-1` as written, so check those files if a project keeps opening on 5.1. In Claude apps gateway sessions, the `fable` alias resolves to Fable 5, so use the full `claude-fable-5-2` there. One report (#108898) also found subagents asked for `fable` still running Fable 5.1 in a long session. The effort difference in the test above is expected: Claude Code Effort Level: Model Defaults and Why It Won't Stick explains the per-model defaults, and Fable 5.1 vs 5.2: Cheaper at Medium, Costlier at High and Max covers what else changed between the two versions.

## "Assumes a context window of 200k tokens" and constant auto-compaction

This warning means the Claude Code binary running your session has no entry for `claude-fable-5-2`, so it treats a model with a 1M-token window as if it had 200k and compacts the conversation far too early. In the November 14, 2026 test, Claude Code 2.1.316 started with `--model claude-fable-5-2` printed this line and then sent the request at effort high with `max_tokens` 32000:

The Claude desktop app's Code tab ships its own copy of the CLI, and when that copy is older than Fable 5.2 the same 200k assumption applies. In anthropics/claude-code#108785, a desktop user whose bundled CLI was 2.1.317 saw the session compact at about 170K tokens, eight times in 23 minutes, each time freeing only 5k–10k tokens. Updating the desktop app, or running `claude update` for the terminal CLI, is the fix that matches the cause.

One report doesn't fit yet. Anthropic's changelog says 2.1.323 (November 12, 2026) raised the window assumed for an unrecognized model ID from 200K to 1M, with `/autocompact` to compact earlier and `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` to keep 200K. Yet on November 14 a user on 2.1.323 reported that `/autocompact 400k` for `claude-fable-5-2` printed `Auto-compact window for claude-fable-5-2 set to 400k tokens (capped to model limit of 200k)` (#109750). So check what you can see rather than trust one setting: `claude --version` should show 2.1.321 or later, and the warning above should no longer appear. If it still does behind a gateway or with a custom model ID, set the window as the linked model configuration section describes.

## "There's an issue with the selected model (claude-fable-5.2)"

With Fable 5.2, check the spelling first. The product name is written with a dot, but the model ID uses hyphens, `claude-fable-5-2`, and Claude Code doesn't catch the difference before sending. In a test on November 14, 2026, Claude Code 2.1.322 started with `--model claude-fable-5.2` passed the dotted ID to the server unchanged. When the mock server answered with a 404 `not_found_error`, the API's standard answer for an unknown model, Claude Code printed this line and exited with code 1:

That is the `-p` wording; in an interactive session the last sentence reads "Run /model to pick a different model." Replace the ID with `claude-fable-5-2`, or use the `fable` alias on 2.1.321 or later.

If the spelling is right, Anthropic's error reference entry for "There's an issue with the selected model" gives two other causes. Your account may not have access: the Free plan has no Fable, an Enterprise admin may not have enabled Fable 5.2 yet, and Claude Mythos 5.2 is limited to verified organizations. Or a stale ID is saved somewhere, which shows up as the wrong model coming back after you change it; check the places a model can be set in the priority order on the model configuration page. Behind an LLM gateway set through `ANTHROPIC_BASE_URL`, if every model fails with this line, check that the gateway accepts Claude Code's tool search fields (`defer_loading`, `tool_reference` and the `anthropic-beta` header that enables them). In the Agent SDK the line has no hint, because you set the model in code; handle the structured `model_not_found` error there.

## Desktop app switched a Fable 5.2 session to Haiku 4.5

Two users reported on November 13, 2026 that the Claude desktop app moved their Code sessions off Fable 5.2 without asking. In #109549, an app update from 4.23118.1 to 5.11492.2 switched sessions on `claude-opus-5-5` and `claude-fable-5-2` to `claude-haiku-4-5-20251001`, with the log line `recorded model "claude-fable-5-2" is off the catalog`. In #109628, a forced sign-in during a period of high load did the same; the picker then listed only Haiku, Sonnet and Opus, and typing `/model claude-fable-5-2` brought Fable back. If a long-running session suddenly writes worse code, look at the model name on its latest replies before you trust what it changed.

## "Fable 5.2's safeguards flagged this message": cyber, bio, reasoning_extraction

This block comes from Anthropic's safety classifiers, which run on every request to Fable 5.2, 5.1 and 5, and check everything the model reads, including memory, connectors, web results and files, not just your last message. Claude Code 2.1.322 shows it in this form, with the category at the end (text from a user report in #109211, matching the strings in the 2.1.322 binary):

What happens next depends on the category, according to Anthropic's help article Why Claude switched models in your conversation with Fable:

| Category | What happens by default | What you can do | 
|---|---|---|
| `[cyber]` , offensive security such as exploits, malware or attack tooling | The request re-runs on Opus 4.8 in the same conversation, labeled with the model that answered | Accept the Opus answer, or edit the message; if Opus blocks it too and the work is defensive, apply to the Cyber Verification Program | 
| `[bio]` , biology, chemistry and life sciences | Falls back to Opus 5 | Accept the fallback or edit; a refusal before any output is billed in this category | 
| Frontier LLM development | Falls back to Opus 5 | Same as above, also billed when refused before output | 
| `[reasoning_extraction]` , attempts to expose the model's summarized thinking | Blocked outright, no fallback | Edit the message, or pick another model with `/model` , and report false positives with`/feedback` | 

Automatic switching is on by default. To stop it, turn off "Switch models when a message is flagged" in Settings > Capabilities, or in Config > MODEL & OUTPUT in Claude Code; a blocked request then pauses the conversation instead. After a fallback the conversation stays on Opus, and switching back to Fable can trigger the same block while the original request is still in the conversation, so edit that earlier message first. On the Claude API nothing switches automatically: until you configure fallbacks, a blocked request returns a 200 response with a stop reason.

Blocks that happen before any output are charged only in the biology, distillation and frontier LLM development categories; other categories blocked before output are free, and a block in the middle of a stream charges the tokens sent so far. Anthropic says Fable 5.2 makes about 55% fewer unnecessary blocks than Fable 5.1, but users have reported `[reasoning_extraction]` stopping ordinary coding and documentation work (#109116, #109211, #109348), and in #109348 one user said billing support refused to credit 11 blocked requests. If blocks keep interrupting your work, Claude Fable 5.2 vs Opus 5.5: When Fable High Beats Opus Max helps decide whether Fable should be your default at all.

## "You've reached your Fable limit": plan caps, usage credits, launch week

"You've reached your Fable limit." means you have used the Fable share of your plan, and the line points you to `/usage-credits` to keep going or `/model` to switch. Which share you have depends on the plan, per Anthropic's help articles on Fable models on paid plans and the Fable 5.2 launch offers. Max, Ultra and premium Team or seat-based Enterprise seats can spend up to 50% of their weekly limit on Fable, drawn from the same weekly limit rather than added to it; after that, Fable runs on usage credits or you switch models. Pro and standard Team seats normally pay for Fable with usage credits from the first message. During launch week they get Fable included up to 50% of the weekly limit, plus one Fable limit reset, until November 16, 2026 at 11:59 pm PT. The Free plan has no Fable model.

If you were already paying before launch, check Settings > Usage for an unused Fable limit reset from the same offer before you buy credits. A "Requires usage credits" note on the Fable row of the `/model` picker is not an error; it tells you Fable will bill to credits on your plan. For the numbers behind each plan, see Claude Usage Limits by Plan: What 5x and 20x Actually Multiply, and for what credits cost, Claude Extra Usage Cost: Credit Math and When Max Is Cheaper.

A different line appears when nobody answers the usage-credit prompt: `Fable limit reached · continuing on Fable 5.2 uses usage credits, and the prompt to confirm went unanswered — nothing was sent · answer it where this session is running, or /model to change`, or the variant starting `Fable 5.2 now uses usage credits`. It shows up in Remote Control, background, agent-team and SDK-hosted sessions, where the prompt closes after five minutes by default (the `dialogExpiry` setting). Nothing was billed; send another message where the session actually runs and answer the prompt there. In `-p` mode, and in Agent SDK apps that don't show the prompt, Claude Code never asks and bills usage credits straight away, so check your usage-credit settings before you script Fable on a Pro plan.

## "Repeated 529 Overloaded" or "Fable is experiencing high load"

Both mean Anthropic's servers are short of capacity for Fable at that moment; a 529 is not your usage limit and doesn't count against it, and because capacity is tracked per model, switching with `/model` helps. Claude Code 2.1.322 contains the messages `Fable is experiencing high load, please use /model to switch to Sonnet` for the terminal and `Fable is experiencing high load. Switch to Sonnet.` for the desktop app. Anthropic's status page logged "Elevated errors on Fable 5.2 and Mythos 5.2", overloaded errors lasting four minutes, resolved on November 10, 2026 at 21:47 UTC. In the November 14, 2026 mock test with `CLAUDE_CODE_MAX_RETRIES=2`, Claude Code 2.1.322 sent three requests and then printed `API Error: Repeated 529 Overloaded errors. The API is at capacity — this is usually temporary. Try again in a moment. If it persists, check your inference gateway (127.0.0.1:18794).`; on Anthropic's own API the last sentence points to status.claude.com instead, so the wording tells you which side to check. Fallback setup and retries are covered in Claude Code API Overloaded (529): Switch Models or Add a Fallback.

## Fable 5.2 API 400 errors after switching the model ID

If your code worked on Fable 5.1 or Opus and now gets a 400 on `claude-fable-5-2`, the request is using something Fable 5.2 rejects. Fable 5.2 keeps Fable 5.1's restrictions, and Anthropic's Fable 5.2 migration guide lists four changes that can break working code: the prefix check, the `computer_20251124` tool, the advisor model, and Fable 5.1 being unable to read 5.2's thinking blocks. The first three return a 400; the last drops the blocks and succeeds. Apart from the prefix check and data retention, Anthropic's pages give only the error type, `invalid_request_error`, not the message text.

| What your request does | What the Claude API returns | Fix | 
|---|---|---|
| Changes `system` ,`tools` or earlier messages, then sends old thinking blocks back | 400: `… Invalid signature in thinking block. The block is bound to a different conversation.` | Keep history append-only; as a stopgap, send the `thinking-binding-controls-2026-08-01` beta header with`prefix_mismatch_behavior: "drop_block"` | 
| `thinking: {"type": "disabled"}` or`budget_tokens` | 400 | Remove it; adaptive thinking is always on, so lower `effort` instead | 
| `temperature` ,`top_p` or`top_k` | 400 for some values | Omit all three | 
| `tool_choice` of`any` or a named`tool` | 400 `invalid_request_error` , also on token counting | Use `auto` with`strict: true` , or JSON outputs | 
| Prefilled assistant message | 400 | Put the instruction in the system prompt | 
| Tool type `computer_20251124` | 400 `invalid_request_error` everywhere except Amazon Bedrock | Declare `computer_toolset_20260801` | 
| Advisor tool with any model other than Fable 5.2 or Mythos 5.2, including Fable 5.1 | 400 `invalid_request_error` | Set the advisor `model` to`claude-fable-5-2` | 
| Organization or workspace without 30-day data retention | 400 `invalid_request_error` :`In order to access this model, your organization or workspace must have data retention enabled.` | Enable 30-day retention for a workspace, or ask your Anthropic account team | 

The prefix check is the one most likely to surprise you, because on Fable 5.1 it was enforced by default only for accounts created on or after August 31, 2026, while on Fable 5.2 every request gets it. It fails when anything before a thinking block has changed since the model produced it, and retrying the same body doesn't clear it. Anthropic says nothing changes for you if Claude Code, claude.ai, Claude Managed Agents or the Claude Agent SDK manages the conversation history, so this is a problem for code that builds its own `messages` array. When the mock in the November 14, 2026 test returned this error, Claude Code 2.1.322 printed it in full after `API Error: 400` and did not retry; the data-retention 400 was printed the same way. The `drop_block` setting lets the request through without the affected reasoning, which can raise output tokens, so treat it as temporary.

Anthropic's prefix-check guide names three code patterns that trip it most often. Rebuilding the system prompt with today's date, a mode or instruction files breaks the check as soon as the value changes (a date in `system` fails after midnight); adding, removing or rewording tools, including tools that appear when an MCP server connects, changes `tools`; and trimming or summarizing older turns while keeping the latest ones changes earlier messages. For the first two, stop editing `system` and `tools` and append the new instruction as a `role: "system"` entry at the end of `messages`; for the third, let the API write the summary or remove the kept turns' thinking. Server-side compaction at a token threshold and context editing don't count, because the check compares what you sent, not what Claude read.

An empty answer is not an error at all. Thinking tokens count toward `max_tokens`, so a small limit can end the response with `stop_reason: "max_tokens"` before any text, and the migration guide notes that prompt lines asking Fable 5.2 not to deliberate don't stop it thinking at medium effort and above. In a test through a relay on November 14, 2026, `max_tokens: 300` on a task that needed long reasoning returned a 200 with an empty text block and 300 billed output tokens at effort high, and the same at effort low. Raise `max_tokens` first. Two more checks if the ID itself fails: on Amazon Bedrock the ID is `global.anthropic.claude-fable-5-2` or `us.anthropic.claude-fable-5-2`, served only on the `bedrock-runtime` endpoint, and a 401 is a key problem, covered in Claude Fable 5.2 API Key: How to Access It With a Regular Key.

## Fable 5.2 through a relay: a 200 or a vague 400 can hide the real error

A relay, meaning any API gateway that forwards your requests to Anthropic, can accept requests Anthropic's API rejects and can replace Anthropic's error text with its own. Two rounds of real calls to `claude-fable-5-2` through laozhang.ai's Anthropic Messages endpoint, one call per case, showed both; everything in this section is that relay's behavior, not Anthropic's API, whose documented 400s were not reproduced with an Anthropic key. On November 12, 2026, wrong IDs such as `claude-fable-5.2`, `fable-5-2` and `claude-fable-5-2-latest` came back as 503 "Current group default has no available channels for model …" without a charge. `thinking: {"type": "disabled"}` and `temperature: 0.2` both returned 200, although Anthropic documents a 400 for them on Fable 5.2, and a forced `tool_choice: any` returned a 400 with a generic message.

Disclosure: laozhang.ai is our own service. Its prices are platform pricing, not an independent comparison.

On November 14, 2026, the prefix check didn't fire through the relay. After a first turn that produced a thinking block, resending the unchanged history used 378 input tokens; editing the system prompt and sending the old thinking block back returned 200 instead of a 400, and input fell to 116 tokens, so the old reasoning had been silently dropped. Editing the first user message also returned 200. The `computer_20251124` tool and an advisor set to `claude-fable-5-1` each returned 400 `invalid_request_error`, but with the relay's generic text, "The request was not accepted. Please check the request parameters and content and try again, or contact support.", in place of Anthropic's reason. That round cost about $0.09 at Fable 5.2's list price of $8 input and $40 output per 1M tokens.

So code that works through a relay can still fail on Anthropic's API. Before you move it, check it against the API 400 table above, because the relay may have accepted thinking settings, sampling parameters or edited history that the direct API refuses, and treat that table as your list of suspects whenever a relay returns a 400 with no reason. A 503 about "no available channels" means the relay doesn't serve the exact ID you sent, so correct it to `claude-fable-5-2`; on laozhang.ai a pay-per-request token gets the same message, because Fable 5.2 is billed there by tokens only.

## Claude Fable 5.2 errors: other questions

### Why can't I see Claude Mythos 5.2?

Mythos 5.2 (`claude-mythos-5-2`) is the same model as Fable 5.2 with fewer safeguards, and as of November 14, 2026 it is available only to organizations verified through Anthropic's programs, such as the Cyber Verification Program. A normal account not seeing it is expected, not an error.

### Why is Fable 5.2 missing on our Enterprise plan?

On Enterprise plans Fable 5.2 stays off until an admin enables it. Seven days after the November 9, 2026 launch, Anthropic adds it to every role that already has a Fable model enabled, and moves Fable defaults to 5.2, unless an admin opts out first.

### Does Fable 5.2 work from Russia, mainland China or Hong Kong?

No. The Claude API's supported regions list has 175 entries as of November 12, 2026, and Russia, Belarus, mainland China and Hong Kong are not among them; requests from unsupported regions are refused with a 403-type error, and this applies to every model, not only Fable 5.2.

### Why does api.anthropic.com return 401 for Fable 5.2?

A 401 is about the key, not the model: on November 12, 2026 a made-up key returned `invalid x-api-key`, and a request without a key returned `x-api-key header is required`. Fable 5.2 doesn't need a separate key, and Claude Fable 5.2 API Key: How to Access It With a Regular Key covers what the organization needs.
