---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-complete-setup-download-and-api-guide
title: "Kimi K3.1 Pro: Complete Setup, Download and API Guide"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["kimi", "work", "model", "code", "install", "download", "hosted", "inference"]
source: https://www.ai.joaoqueiros.com/blog/kimi-k3-1-pro-official-guide-install-download-api-code
source_anchor: ""
source_lines: [1, 180]
section: "Kimi"
---
Kimi K3.1 Pro is live, but “use it,” “install it,” and “download the model” mean three different things. You can use Moonshot AI's hosted model today in a browser, in Kimi Work, through Kimi Code or via the API. You **cannot yet download the full K3.1 Pro weights for local inference**: Kimi says they will be released by November 4, 2026. This guide puts each path in its own lane and explains the model's strengths without treating a launch demo as a guarantee.

**Start here**

For the easiest trial, open Kimi.ai, sign in, select K3.1 Pro and try a small non-sensitive task. For local-file knowledge work, install Kimi Work. For codebase work, install Kimi Code CLI and select `k3.1-pro`. For an application integration, use the Kimi API with model ID `kimi-k3.1-pro`. Hosted use is not offline use.

## Meet Kimi K3.1 Pro

**Source:** This is Kimi AI's official launch video. It demonstrates what the company built; the release post supplies the technical details. Hands-on third-party tests are linked separately below. Availability, pricing and model permissions were checked on 25 October 2026.

## Choose the Route That Matches Your Goal

| You want to... | Use this | What runs on your computer? | 
|---|---|---|
| Try prompts now | Kimi web | Only the browser; inference is hosted. | 
| Work with local files and desktop tasks | Kimi Work | The app and its file/browser tools; model inference is hosted. | 
| Work inside a code repository | Kimi Code CLI | The coding agent and approved tools; model inference is hosted. | 
| Build your own integration | Kimi API | Your client code; model inference is hosted and billed by tokens. | 
| Self-host an offline model | Wait for official weights and license | Full inference would run on your hardware; not available yet. | 

## 1. Try K3.1 Pro in the Browser

1. Visit the official Kimi web app and sign in.
2. Start a new conversation and select **Kimi K3.1 Pro** from the model picker if your account shows it. Kimi says the model is rolled out on the web and mobile apps, but product limits may depend on the account.
3. Try a bounded task: “Summarize this public document and cite the page for each number” or “Plan a simple three-page site, then list tests before writing code.” Check citations, calculations and generated artifacts yourself.
4. For 3D, open Kimi's **3D Make** feature from the app. Kimi says it can preview and edit a generated model and export it for 3D printing, video or Live Photos. Verify dimensions, mesh integrity and printability in your normal CAD/slicer workflow before making a physical part.

Nothing in this path requires downloading model weights. Sending a file to a hosted service is different from processing it only on your device; review the service's current privacy terms before uploading confidential information.

## 2. Install Kimi Work on Desktop

1. Open Kimi's official downloads page. Choose **Windows** or**macOS Apple silicon** for Kimi Work. Kimi's download FAQ says Intel Macs support Chat mode, not the full Work experience.
2. Install the downloaded app, open it and sign in. For K3.1 Pro, Kimi's release note specifies **Kimi Work version 3.1.0 or later** ; update if it is missing.
3. Select K3.1 Pro in the app. Start with a folder containing approved sample files, not your whole home directory. Ask for a read-only inventory or draft before granting write or browser-action permissions.
4. Review the created document, spreadsheet or research output against the originals. Kimi Work can access local folders and automate browser tasks, but it is still a hosted-model workflow rather than local/offline inference.

Kimi's Work product page describes file access, scheduled tasks and a browser agent. The download page notes that Chat conversations sync across devices, while Work conversations remain on the desktop where they were created.

## 3. Install Kimi Code for Projects

The official Kimi Code CLI guide offers an install script and an npm package. The script does *not* install K3.1 Pro weights; it installs a local agent that connects to the model.

### Windows PowerShell

#### Before installing

Install Git for Windows, which supplies the Git Bash shell Kimi Code uses. Use a fresh PowerShell window in a trusted workspace.

#### Official installer

`irm https://code.kimi.com/kimi-code/install.ps1 | iex`

### macOS or Linux

#### Official installer

`curl -fsSL https://code.kimi.com/kimi-code/install.sh | bash`

#### Alternative

With Node.js 22.19 or newer, use `npm install -g @moonshot-ai/kimi-code`. Check the current command on the official docs before running any remote installer.

1. Verify the install with `kimi --version` .
2. Open a project directory and run `kimi` . The agent can read and change project files and run commands, so keep approvals on and review the repository state first.
3. On first launch, enter `/login` and choose**Kimi Code (OAuth)** , or the separate Kimi Platform API-key option if you deliberately want API billing. Do not assume those credentials and quotas are interchangeable.
4. Start a new session, enter `/model` , and choose**K3.1 Pro** . In Kimi Code's model configuration its ID is`k3.1-pro` , not the Open Platform API's`kimi-k3.1-pro` . Use`k3.1-pro-highspeed` only if your plan includes it.
5. Start with a read-only request such as “Map this repository and identify its test commands; do not edit files.” Then request a small change, inspect the diff and run tests before accepting it.

Kimi Code model configuration says K3.1 Pro requires Plus/Moderato or above, while Highspeed and the up-to-1M context window require Pro/Allegretto or above. K3.1 Pro keeps thinking enabled; the Code client supports low, high and max effort, with high as its default. Start a fresh session when switching models or effort to avoid stale reasoning and lost cache benefits. A Kimi Code Desktop app and a VS Code extension are also available if you prefer a graphical coding workflow.

## 4. Make a Kimi API Call

The Open Platform quickstart uses the OpenAI-compatible API. Kimi's K3.1 Pro guide says access unlocks after a successful top-up of at least **$1**. Create an API key in the console, keep it out of code and source control, then set the environment variable for your shell:

```
# PowerShell (current window)
$env:MOONSHOT_API_KEY = "paste-your-key-here"
# macOS / Linux shell (current session)
export MOONSHOT_API_KEY="paste-your-key-here"
```
Install Python 3.9 or later and the SDK with `python -m pip install --upgrade 'openai>=1.0'`. The following is a minimal request following Kimi's documented endpoint and model ID:

```
import os
from openai import OpenAI
client = OpenAI(
    api_key=os.environ["MOONSHOT_API_KEY"],
    base_url="https://api.moonshot.ai/v1",
)
reply = client.chat.completions.create(
    model="kimi-k3.1-pro",
    messages=[{"role": "user", "content": "List three checks for a generated 3D-printable part."}],
)
print(reply.choices[0].message.content)
```
Use `kimi-k3.1-pro-highspeed` to request the faster API variant. K3.1 Pro always uses reasoning; its API accepts `reasoning_effort` values `low`, `high` and `max`, with `max` as the API default. For multi-turn or tool conversations, follow Kimi's guide and return the *complete* assistant message, not just its visible text. See the official examples for streaming, image/video input, tools and JSON output. Do not hard-code the key or expose it in browser JavaScript.

## 5. Download the Weights for Local Use: Not Yet

As of **25 October 2026**, the official release note promises full K3.1 Pro weights **by November 4**; it does not provide a K3.1 Pro checkpoint download. An installer for Kimi Work or Code is not a model download. The previous K3 model's weights, a third-party file with a similar name, and a hosted API are not interchangeable with K3.1 Pro's unreleased files.

When Kimi publishes them, use the checkpoint and license linked from its official release or verified publisher page. Check the precise format, shard sizes, tokenizer, required inference runtime and hardware guidance before planning a local deployment. A 3T-class mixture-of-experts model is a very different hosting target from a small laptop model; no responsible “run it on your PC” command or RAM promise can be given before the artifacts and instructions exist. For a genuinely local starting point today, see our Gemma 4 LM Studio guide.

## What K3.1 Pro Adds

Kimi describes K3.1 Pro as a **3T-class, post-trained version of K3**, not a three-trillion-active-parameter model. The underlying K3 architecture has 2.8 trillion total parameters; Artificial Analysis lists about 104 billion active parameters. The hosted model accepts text and visual input and returns text; its polished 3D, web and document outputs come from writing code, calling tools and building artifacts, not from a native 3D-mesh token stream.

### Full-project coding

#### Kimi's claim

It targets multi-step repository work, complete sites and games, Three.js scenes, and workflows using engines such as Unity, Unreal and Godot. Its published internal walking-simulator run involved 167 missions and 420 reviews.

#### Reader's test

Give it a real repository and acceptance criteria. Check build output, tests, security-sensitive changes and whether the app still works after a second iteration.

### Parametric 3D and 3D Make

#### Kimi's claim

The model can write scripts for Blender and CAD-like tools, use reference images, edit geometry and create configurable parts. Kimi's 3D Make interface supports preview, editing and export.

#### Reader's test

Set explicit units, tolerances and constraints; inspect the exported mesh in a slicer or CAD app. A convincing render is not evidence of a printable or safe mechanical part.

### Finance and document work

#### Kimi's claim

It emphasizes spreadsheet formula linkage, cross-document fiscal reasoning and source-grounded research. Kimi reports gains on both published and internal finance evaluations.

#### Reader's test

Trace every material number to the source filing, recalculate formulas and inspect assumptions. Do not treat a long, cited report as audited financial advice.

Across channels, Kimi specifies a **one-million-token context window** and a standard versus Highspeed option. A large window is capacity, not proof of perfect retrieval or a low bill. In Kimi Code, actual context access depends on membership; in the API, sending very long inputs is still usage-billed.

## Benchmarks, Without the Launch-Chart Shortcut

Moonshot's technical post reports **65.7%** for K3.1 Pro on Terminal-Bench 4.0, narrowly behind its listed Opus 5.5 result of 66.4%. On the with-tools BenchCAD subset it reports **92.7%**, versus 96.2% for Opus. Its OfficeQA Pro v2 result is **64.4**. These are different tasks, scales and test harnesses, not a single “percent smarter” score. Kimi's notes say most K3.1 Pro results use max effort, temperature 1.0 and top-p 0.95, and some competitor numbers come from outside reports.

Artificial Analysis independently scores K3.1 Pro **55** on its Intelligence Index and lists a one-million-token context window. That composite is not a universal product verdict. Its separate launch analysis calls the model a leading candidate among future open-weight releases while noting that the weights are not yet published. It reports its own Terminal-Bench result under its evaluation setup; do not silently combine it with Kimi's vendor run.

Kimi also describes targeted reductions in cyber capability before the open-weight release. Those are *company-reported training and benchmark measures*, not a guarantee that any deployment is safe from misuse or that generated code is secure.

## Know Which Bill You Are Paying

| Open Platform API model | Input / 1M | Output / 1M | Cached input hit / 1M | 
|---|---|---|---|
| `kimi-k3.1-pro` | $2 | $10 | $0.20 | 
| `kimi-k3.1-pro-highspeed` | $4 | $20 | $0.20 | 

These are Kimi's listed first-party API rates on 25 October, excluding applicable taxes and other billable operations. Cache writes can have separate charges; repeated-prefix caching and rate limits matter in long agent sessions. The API pricing guide explicitly distinguishes pay-as-you-go API billing from Kimi membership and Kimi Code. Highspeed is claimed to deliver roughly two to three times the output speed of standard Pro, but doubles its listed input and output token rates. Tool time can dominate end-to-end workflows.

## Common Setup Problems

- **“Where is the model file?”** The full weights are not published yet. The downloads page supplies apps, not an offline K3.1 Pro checkpoint.
- **K3.1 Pro missing in Code:** check the membership table, start a new session and refresh the client or sign in again. Highspeed is a higher-tier option.
- **Wrong model ID:** use`k3.1-pro` in Kimi Code membership clients and`kimi-k3.1-pro` in the pay-as-you-go Open Platform API. Do not mix their keys and base URLs.
- **HTTP 400 for thinking:** K3.1 Pro does not support thinking disabled. Choose low, high or max, using the parameter format documented for your channel.
- **HTTP 403 or quota error:** check membership access or API top-up and rate limits; retrying an ineligible model without changing access will not fix it.
- **“Highspeed is not faster”:** verify the exact model, then separate token-generation time from file reads, tool calls, browser automation and test runs.

## Watch It Tested Beyond the Launch Video

The two independent creator videos below are useful counterweights: one leans into 3D and game output while noting a broken game control; the other shows both an impressive Rubik's Cube and accuracy-sensitive failures. Neither is a controlled laboratory comparison.

### World of AI: 3D, games and Opus cost

Read the organized demo and price breakdown.

### Matthew Berman: visual quality versus fidelity

Read the full hands-on review for the Rubik's Cube, WebGL phone, Blender knot and fabricated benchmark graphic.

## Official Sources and Scope

**Release and capability claims:** Kimi's K3.1 Pro technical post and official video. **Downloads and setup:** Kimi downloads, Code CLI getting started, Code model configuration and Code Desktop quickstart. **API:** quickstart, model guide and billing explanation. **Independent measurement:** Artificial Analysis. We have not installed Kimi Code, billed an API request, independently rerun the benchmarks or verified unreleased model files. Recheck pricing, permissions and weight availability after 25 October 2026.
