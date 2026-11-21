---
id: collect-261121-veille-ia/claude-anthropic/use-sonnet-5-5-in-claude-code-model-effort-and-access
title: "Use Sonnet 5.5 in Claude Code: model, effort and access"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-09-29"]
keywords: ["model", "claude", "sonnet", "effort", "access", "code", "provider", "client"]
source: https://ofox.ai/blog/claude-sonnet-5-5-claude-code-setup/
source_anchor: ""
source_lines: [1, 150]
section: "Claude_Anthropic"
---
To select Sonnet 5.5 explicitly in Claude Code, use **`claude --model claude-sonnet-5-5`** for a new session or **`/model claude-sonnet-5-5`** inside a session, on a provider that accepts that model ID. First check the client version and your account’s model permissions. Anthropic’s current documentation requires Claude Code **v2.1.284 or later** for Sonnet 5.5.

This guide separates three things that are easy to confuse: the client recognizing the model, the provider serving it, and the account being allowed to use it. The instructions were checked against Claude Code’s model configuration documentation on September 29, 2026. They do not establish that every subscription or third-party endpoint has access.

## Check and select the model

In your terminal:

```
claude --version
claude update
claude --model claude-sonnet-5-5 --effort medium
```
`claude update` changes the installed client. Follow your organization’s software-management process if the installation is managed. In an existing interactive session, use:

```
/model claude-sonnet-5-5
/status
```
Check the displayed model and provider rather than assuming the command succeeded. Do not use the generated answer’s self-description as proof of the serving model; the client state and response metadata are more useful evidence.

The short alias `sonnet` is convenient, but its meaning depends on the provider and client version. The current documentation maps it to Sonnet 5.5 for the Anthropic API while some cloud-provider aliases still resolve to older Sonnet models. A full model name makes your intent explicit, but you must still use the identifier and access route supported by that provider.

## Why changing the default is not enough

Claude Code’s default model is not necessarily the newest Sonnet. Current documentation lists Opus 5.5 as the default for several account/provider categories. Selecting Sonnet is a deliberate action. If an organization restricts model choices, the available list may differ from examples in a public guide.

An environment variable such as `ANTHROPIC_DEFAULT_SONNET_MODEL` can also change what the alias resolves to. Before editing configuration, inspect the applicable user, project and managed settings. Keep a note of the old value so you can undo a local experiment. Avoid overwriting a shared team’s provider settings to fix one session.

Use explicit selection for a reproducible test, then choose whether a persistent default is appropriate. A one-off test does not require changing every repository or every agent definition.

## Pick effort for the task

The Sonnet 5.5 API defaults to high effort. Claude Code’s model configuration describes medium as Sonnet 5.5’s default there. Those statements refer to different entry points, so they are not interchangeable.

For a well-specified code change, medium is a reasonable documented starting point. Increase effort only when the task and results justify it. Use `/effort` in the interactive client or `--effort` when launching, subject to your account’s restrictions. Managed effort caps can limit the effective setting even when you requested something higher; record the applied configuration where it is available.

The effort guide explains why a higher label is not automatically a better result. Start with an isolated branch, explicit acceptance criteria and a reviewable diff. The model still needs permission to run tools and you still need to inspect the changes before merging them.

## Separate subscription access from API billing

A Claude subscription login and an API key are different access routes. A message saying the organization disabled subscription access for Claude Code is an account-policy block, not proof that the Sonnet model is down. Ask the administrator to review the approved access route; do not attempt to bypass a managed restriction.

During preparation for this guide, our local Claude Code v2.1.281 run returned that organization-access message and did not execute a Sonnet task. It was also below the documented minimum version. We therefore do not claim a successful model test, timing result or cost measurement from that attempt. The command examples above are documentation-verified instructions.

If an approved API route is available, its requests follow API billing rather than becoming free because you also have a subscription. Check the exact endpoint, model, account and current rates before testing. See the Sonnet API cost guide for the vendor rate-card calculation.

## Establish a clean baseline before asking for code changes

Use a small repository you control, with no production credentials or unrelated uncommitted work. Record its current commit and run the existing tests before opening Claude Code. Otherwise a pre-existing failure can be mistaken for a Sonnet regression, and unrelated edits can be credited to the model. These local commands inspect the repository; substitute its actual test command rather than installing an arbitrary framework.

```
git status --short
git rev-parse HEAD
claude --version
claude auth status
```
Check the authentication output locally and retain only the account type and approval status needed for your run record. Do not copy credentials or a complete environment dump into a blog, issue or prompt. If your team uses managed installation or authentication, resolve that through its approved administrator process before testing. Updating the client cannot override a disabled subscription policy.

After an authorized update, run `claude --version` again. The update command being issued is not proof that the executable used by your shell changed. Multiple installations can leave an older binary earlier on the command path; check `command -v claude` when the reported version remains old. Record the path as well as the version, particularly if a terminal and an IDE show different behavior.

## Choose a session without accidentally changing the team’s defaults

The current model documentation gives selection precedence: an in-session choice, then the startup flag, then `ANTHROPIC_MODEL`, then the settings `model` field, followed by the default-model setting. Managed restrictions still apply. This explains why editing one settings file may have no visible effect when a higher-priority choice is active.

For an isolated trial, launch with the full supported ID and explicit effort:

`claude --model claude-sonnet-5-5 --effort medium`
Inside the session, inspect `/status` and the model picker. The current documentation says typing `/model <name>` saves that choice to user settings for future sessions. To switch only the current session, open `/model` and use the picker’s session-only action (`s` in the documented default key bindings). Do not describe a direct `/model` command as temporary without checking this behavior.

If you intended a permanent change, note the prior setting and verify a newly opened session as well as the current one. If you intended a one-off test, use the startup flag and avoid editing shared repository settings. A provider alias and a pinned model ID serve different purposes: the alias follows that provider’s recommended version, while the explicit ID states which version the test requested. Neither proves a gateway actually served it; inspect the available client or provider metadata.

## Give the first task a checkable, bounded result

A useful first coding task is a local arithmetic bug with an existing failing test, not “improve this project.” Here is an editorial practice fixture. The data are synthetic; this is not a patch Sonnet produced during our preparation.

```
# expenses.py: intentionally incorrect practice function
def total(rows):
    return sum(row["unit_price"] for row in rows)
```
For three notebooks at $4.50 and two pens at $1.25, the incorrect function returns $5.75. The required result is $16.00 because quantities must be multiplied before summing. Save this acceptance test beside the function:

```
# test_expenses.py
from decimal import Decimal
from expenses import total
def test_total():
    rows = [
        {"quantity": 3, "unit_price": Decimal("4.50")},
        {"quantity": 2, "unit_price": Decimal("1.25")},
    ]
    assert total(rows) == Decimal("16.00")
def test_empty():
    assert total([]) == 0
if __name__ == "__main__":
    test_total()
    test_empty()
```
Run `python3 test_expenses.py` before invoking the model. The first assertion should fail. If it does not, inspect your files and import path; you have not established the intended starting state. The exact traceback varies, so verify the incorrect arithmetic rather than matching a screenshot or line number.

Then supply a complete task rather than an unexplained error:

```
Fix total(rows) in expenses.py. Each row's contribution is quantity times
unit_price, using the existing Decimal inputs. Empty input must return 0.
Change only expenses.py. Do not change tests or install dependencies.
Run python3 test_expenses.py and report the result. Explain the cause,
show the changed line, and list any remaining untested assumptions.
Do not commit, push, access external services or claim tests you did not run.
```
This prompt fixes the task boundary and makes the expected result independently knowable. It does not prove the model will follow it. Inspect the diff to ensure tests were not weakened and unrelated files were not modified. The acceptance criteria deliberately cover only valid rows and empty input; negative quantities, missing keys and malformed data need separate product decisions rather than invented behavior inside this small fix.

## Accept the change using local evidence

After the session, run `git diff -- expenses.py test_expenses.py` and execute the test yourself. A passing final message is weaker evidence than the actual process exit code and output. Check the arithmetic on the two rows and confirm the test file is unchanged. A model that edits the assertion to $5.75 has made the test green while preserving the bug, which must be rejected.

Keep a compact record: starting commit, client version, selected provider and model, requested effort, test command, result, diff and human review decision. If the task failed before a model response because of account policy, label it “access blocked,” not “coding failed.” If a tool permission was denied, record that separately from an incorrect patch. If the model proposes a correct change but never runs tests, mark it “patch proposed, unverified” until local verification finishes.

For a real repository, expand the checks to its relevant regression suite and type or build checks. Do not infer broad coding quality from this two-test fixture. Its purpose is to prove that your access route, file-editing workflow and acceptance process work together before trusting the tool with a larger task.

## Return to the previous configuration cleanly

If you used only the launch flag, end that session and inspect the next session’s status rather than assuming its model. If you changed a saved default, restore the prior user setting through the supported picker or configuration path and verify it. Keep managed settings intact. Remove only the disposable practice files you created; never reset or clean a shared repository blindly to undo a model trial.

This gives four useful outcomes: access blocked; model reached but no acceptable patch; patch proposed but unverified; patch independently accepted. Keeping those outcomes separate prevents an installation or permission problem from being reported as a model benchmark, and prevents a confident explanation from being mistaken for working code.

## Troubleshoot the actual failure

| Symptom | Check first | 
|---|---|
| Client does not recognize the model | Client version and exact model ID | 
| Model is absent from a picker | Provider support, organization restrictions and client version | 
| Organization-disabled subscription message | Administrator-approved account access | 
| 401 authentication failure | Credentials and the selected billing/provider route | 
| API 400 after selection | Request fields and Sonnet 5.5 migration requirements | 
| Session appears quiet between tools | Response/display behavior, not only model availability | 

Do not use a 404 as proof of a global outage or a 429 as proof of a subscription cancellation. Save a redacted error body, timestamp and client version. Never post tokens, full environment dumps or private repository context with a support request.

For native request changes, use the Sonnet 5.5 API migration checklist. For deciding whether to switch from an older model at all, see Sonnet 5 versus 5.5.

## Frequently Asked Questions

- Does /model sonnet always mean Sonnet 5.5?
- No. Aliases can vary by provider and version. Check the current mapping and use the supported full ID when you need a pinned selection.
- Why does my organization block Claude Code?
- Only your administrator can confirm that policy. The error is an access restriction, not evidence about Sonnet's coding quality or general availability.
- Was Sonnet successfully tested for this article?
- No. The local attempt was blocked by organization access and used an older client. The tutorial is based on current documentation, with those limits stated explicitly.
