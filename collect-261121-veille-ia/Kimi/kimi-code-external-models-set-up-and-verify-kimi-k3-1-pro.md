---
id: collect-261121-veille-ia/kimi/kimi-code-external-models-set-up-and-verify-kimi-k3-1-pro
title: "Kimi Code External Models: Set Up and Verify Kimi K3.1 Pro"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["model", "kimi", "provider", "name", "code", "another", "crazyrouter", "endpoint"]
source: https://dev.to/buluo/kimi-code-external-models-set-up-and-verify-kimi-k31-pro-2ih7
source_anchor: ""
source_lines: [1, 98]
section: "Kimi"
---
This article was created with the help of AI. The configuration records and test results were checked against saved evidence.

A working external-model setup has three separate milestones: the provider is saved, the model is selected, and a real answer is returned. This guide walks through all three with Kimi K3.1 Pro in Kimi Code Desktop. It is intended for developers who already have an API key. If you only want free chat, have no key, or do not plan to manage API usage, you do not need this setup. The screenshots and documentation were checked on November 10, 2026.

An official API is usually enough when Kimi is the only model you need. An aggregator becomes useful when you want several model vendors in the same project. The example below uses one aggregator, but the same decision process applies to any compatible service you already use.

Before touching the settings, separate the values that determine a request. Base URL is the destination, the API key is the credential, and the model ID is the model you want to call. A provider label or display name is only for readability.

| Field | Example | Common misunderstanding | 
|---|---|---|
| Provider name | `crazyrouter` | Thinking the label determines the protocol | 
| API Protocol | `OpenAI` | Mixing it with an Anthropic-native endpoint | 
| API Key | Your own key | Showing it in a screenshot or using it with another service | 
| Base URL | `https://api.crazyrouter.com/v1` | Duplicating `/v1` or copying a full request endpoint | 
| Model ID | `kimi-k3.1-pro` | Typing the display name instead of copying the ID | 
| Display name | `kimi-k3.1-pro` | Expecting an alias to change the requested model | 
| Context | Check the model documentation | Treating it as a way to increase model capability | 

The screenshots use `1048576` for Context, but this article does not verify a 1M-token context. Check the actual limit for every model you add.

If you choose another aggregator, replace the Base URL, key, and model ID together. Do not assume an ID from another documentation set will route to the same model.

**Step 1: open Settings.** Click the gear icon in the lower-left corner of Kimi Code Desktop. External models are configured here, not by writing a model name in the chat box.

**Step 2: create a provider.** Select `Model Providers`, then click `Add provider` in the upper-right corner. Your custom provider will appear separately from the built-in Kimi Subscription.

The provider owns the connection details; models sit beneath it. This separation lets you add another model later without duplicating the endpoint configuration.

**Step 3: choose Custom and enter the connection details.** Select `OpenAI` and use this Base URL:


```
https://api.crazyrouter.com/v1
```
The Crazyrouter endpoint guide explains the distinction: an OpenAI-compatible client uses the address through `/v1`, while an Anthropic-native client uses the root domain. A `/v1/v1` error generally means the path was added twice.

Paste the full API key without revealing it in a screenshot. If it is exposed, rotate it at the service. Also avoid copying an address from another client without checking the expected format: some tools need a root domain, some need `/v1`, and a manual HTTP request needs the complete endpoint.

**Step 4: copy the ID from the catalog.** Open the Crazyrouter model catalog, search or filter by Moonshot, and use the copy icon on the model card. Paste the result into `Model ID` in Kimi Code.

Kimi K3.1 Pro is the readable model name; `kimi-k3.1-pro` is the request ID for this service. Another guide may use `k3.1-pro`, but that belongs to a different configuration context. Copying the ID avoids errors with prefixes, capitalisation, dots, hyphens, and spaces.

Check the pasted value once. It should begin with `kimi-` and contain no leading or trailing whitespace.

**Step 5: save one model first.** Review `Model ID`, `Context`, and `Display name`, then click `Save`. Resist adding every available model before the first call. One working entry is easier to verify than a long list.

The list is evidence of saved local settings, not evidence of successful calls.

**Step 6: open More models.** Return to the main window, open the model picker, and click `More models...` at the bottom. The first list can still contain built-in models; custom entries are one level deeper.

**Step 7: select the custom model.** In the `Switch model` window, choose the `crazyrouter` tab and click `kimi-k3.1-pro`. Searching for the complete ID is safer when several names are similar.

**Step 8: confirm the active model and start a new session.** The input box should show `kimi-k3.1-pro`. A new session avoids carrying an earlier model's context into the first check.

If the model is absent, confirm `Save`, reopen the picker, and restart Desktop if needed. Do not create multiple providers with the same name; it makes the active source difficult to identify.

The last screenshot proves selection, not generation. On November 10, 2026, I created an independent session through the local Kimi Code desktop service and bound it to `crazyrouter/kimi-k3.1-pro`. I asked for three configuration checks. The session reached `completed` and returned non-empty text:

1. Copy the complete model ID from the catalog rather than entering it from memory.
2. Match the Base URL to the selected protocol and confirm whether `/v1` is required.
3. Send a real prompt and inspect the returned content instead of treating a connection as the final test.

I then sent a non-streaming request through `https://api.crazyrouter.com/v1`. The available-model list included `kimi-k3.1-pro`; the chat returned HTTP 200, the response named the same model, and the finish reason was `stop`. You can use the Crazyrouter model list endpoint to check the models available to your own key before testing a chat request.

The request used 691 tokens in total, of which 451 were reasoning tokens. Visible answer length and total generation usage are different measurements.

For your own project, add one verification layer at a time. Ask for an explanation of an existing file, then a small edit with a test, and only then a command whose output you can inspect. If something fails, save the prompt, output, and failure category before changing anything. This keeps setup errors separate from model-quality issues.

The evidence here does not cover 1M-context behaviour, file editing, command execution, or long-term stability. It covers a real text generation in Kimi Code and a direct API response.

**The model is not in the picker.** Check `Save` and the custom-provider tab before changing the endpoint. If the ID looks similar to another model, copy it again.

**The response is 401.** Check that the key is complete and valid, and that it belongs to the service in the Base URL. Credentials are not interchangeable between platforms.

**The response is 403 or a permission error.** A valid model ID does not guarantee access. Check the models and capabilities available to your account.

**The response is 404 or model not found.** Inspect the copied ID and look for `/v1/v1`. Keep OpenAI-compatible and Anthropic-native formats separate.

**The request succeeds but there is no visible answer.** Inspect the final text field, finish reason, and output cap. Reasoning tokens may be consumed before visible text, and a small cap can leave no room for the answer.

**Results change after switching models or thinking effort.** Start a new session. Kimi Code's documentation notes that these changes invalidate the existing prompt cache and can increase token usage. Leave Thinking enabled for models that require it rather than misdiagnosing the error as an endpoint issue.

Regular Kimi works well for conversations, reading, search, and everyday agent tasks. Kimi Code is designed around a project and working directory, where the model can participate in a development workflow.

| Area | Regular Kimi | Kimi Code | 
|---|---|---|
| Main context | Conversations, uploaded documents, web information | Projects, repositories, terminals, development tasks | 
| Files | Understands material supplied in the conversation | Searches and edits across project files | 
| Execution | Does not necessarily operate in your current environment | Runs commands, builds, and tests under a permission policy | 
| Model choice | Primarily built-in Kimi models | Multiple providers through compatible protocols | 
| Extensions | Built-in product capabilities | MCP, Skills, Hooks, plugins, and sub-agents | 
| Forms | Web or app | Desktop, CLI, VS Code, and third-party agent integrations | 

A connected model can help you understand a repository, make a small feature change with tests, organise documentation, or compare several models on the same task. MCP and plugins add external tools, while Skills preserve reusable project instructions. These are reasons to use the project environment, not guarantees that every task will succeed.

For occasional questions, regular Kimi is simpler. For project work that needs file edits and verification through real runs, Kimi Code is more useful. Start with a small, checkable task, then expand permissions and complexity only after the connection and output prove reliable.

## Top comments (0)
