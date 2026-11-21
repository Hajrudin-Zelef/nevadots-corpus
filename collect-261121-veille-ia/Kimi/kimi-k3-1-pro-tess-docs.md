---
id: collect-261121-veille-ia/kimi/kimi-k3-1-pro-tess-docs
title: "Kimi K3.1 Pro - Tess Docs"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: ["2026-11-04"]
keywords: ["moonshot", "opus", "kimi", "financial", "models", "same", "tools", "model"]
source: https://docs.tess.im/en/kimi-k3-1-pro
source_anchor: ""
source_lines: [1, 101]
section: "Kimi"
---
Kimi K3.1 Pro is the first model in the K3.1 series. Moonshot describes it as its most capable 3T-class open-weight model to date, with a 1-million-token window. On this page it is for **the terminal, a 3D part from views, and a financial document**.
Artificial Analysis still classifies the model as proprietary and lists text and image input, text output, and a 1-million-token context. Moonshot says the weights ship by November 4, 2026. The announcement does not include the license. Check the model card when the weights arrive.
## When to use K3.1 Pro, and when to use Opus 5.5

K3.1 Pro is for a terminal command with tests that already exist, a part reconstructed from views, or a brief on financial documents. Claude Opus 5.5 stays ahead on the two tests where Moonshot publishes both side by side.
## What Moonshot measured

The figures below are the ones Moonshot published, at max reasoning effort, temperature 1.0, and top-p 0.95. Several scores for other models were taken from blogs or official leaderboards, not from the same harness.
### Terminal

On Terminal-Bench 4.0, with the Claude Code harness, Moonshot measured 65.7%. On its table, GPT-6 Astra is at 58.2% and Claude Opus 5.5 at 66.4%. The other models’ scores are the highest each one published on its blog or on the official leaderboard. Do not treat the three as the same run.
On Kimi Code Bench 3.0, an internal test of multi-step execution and tool use, K3.1 Pro scored 61.2%, against 55.95% for K3.
### A part in 3D

On BenchCAD, 3D reconstruction from renderings through CAD code, Moonshot measured 92.7% with tools, on a 1,000-file subset. On the same account, Claude Opus 5.5 is at 96.2% and GPT-6 Astra at 95.9%. Without tools, K3.1 Pro is at 72.8% and Opus 5.5 at 73.0%.
### A financial document

On OfficeQA Pro v2, analysis of real financial documents delivered as images, Moonshot measured 64.4 and called the result state of the art. On Frontier Finance, 58.6.
On internal tests, K3.1 Pro is up 7.2 points on financial modeling (57.6 against K3’s 50.4) and 11.6 points on financial research (70.4 against 58.8). On a simulated business operation, Moonshot reports about 4.2 times K3’s result (37 against 8.9).
On Kimi Hallu Bench Hard, an internal stress test, the claim-level hallucination rate with tools fell from 23.4% on K3 to 8.3%. The share of responses with at least one factual error was 42.4% of the predecessor’s rate. On the same test with tools, Moonshot lists GPT-6 Astra at 5.5% and Opus 5.5 at 15.0%. Without tools, it lists Astra at 4.2%, K3.1 Pro at 5.4%, and Opus 5.5 at 6.8%. Moonshot says the test was built to provoke errors and that these rates are not the rates of ordinary use.
### What was left out on purpose

For the open-weight release, Moonshot says it removed cybersecurity data from mid-training and from the reinforcement-learning environment mix, and kept general programming tasks. It says the released model’s offensive capability stays behind frontier proprietary models that had already been public for at least three months. This page does not describe those tasks.
## What Artificial Analysis measured

Artificial Analysis scored **55 on the Intelligence Index**, rank 4 of 220 models in the same class. The band median is 28. In that run the model generated 150 million output tokens, against a median of 81 million. The average cost was 2.11 dollars per index task. Output speed was 41.4 tokens per second, rank 186 of 192. The band median is near 85. That measurement is for `kimi-k3.1-pro`, not the Highspeed variant.
The table mixes Moonshot figures with the Artificial Analysis Intelligence Index. Other models’ Terminal-Bench and BenchCAD scores do not always come from the same harness. Results depend on the harness, the tools, and the effort setting. Validate on your own flow before changing a production configuration.

### Where it does not lead

- At 41.4 tokens per second, standard Pro is slow. Highspeed is there for when the wait is the problem.
- 150 million output tokens on the index, against a median of 81 million, is a verbose model. The task price rises with the text.
- On Moonshot’s Terminal-Bench 4.0 table, 65.7% sits behind Opus 5.5 at 66.4%. On BenchCAD with tools, 92.7% sits behind 96.2% and 95.9%.
- On the factual stress test with tools, 8.3% is still above the 5.5% Moonshot lists for Astra.
- The weights are not out yet. Artificial Analysis still classifies the model as proprietary.

## Where it creates the most value

### A terminal change, with tests

The behavior is described and the tests already exist. K3.1 Pro edits, runs the check, and stops if the second attempt fails.
**Expected result:** a diff inside the scope and the requested tests passing.
### A part from the views

The drawing or the rendering shows the shape. The model returns measurements, what the image does not show, and the script, without treating the file as ready to print.
**Expected result:** a list of what is in the view, and the gap for what still needs measuring.
### A brief on the documents

The question is financial and the answer has to come from the files. The model cites the passage and lists what it did not find.
**Expected result:** every number has a source, with no investment recommendation.
## How to apply it in Tess

Pick the terminal, the part, or the dossier

Prefer a change with tests, a part from views, or a brief on documents. For a short answer while someone is waiting, use Highspeed.

Close what counts as done

State the result, the files that may change, and the check that ends the task.

Set tools and approvals

Enable only the tools the task needs. Require confirmation before sending, publishing, or changing data.

Keep one effort level per session

The published scores use max effort. Changing the level mid-session drops the cache, according to the DataCamp guide drawing on the Kimi Code docs.

Sample before you scale

Run one full case, check what changed, and only then repeat the flow.

## Three playbooks to start

### 1. A terminal change with tests

**Input:** the repository, the expected behavior, and the tests that already exist.

**Tools:** read, edit, and run the tests.
**Done when:** the requested tests pass and the diff stays inside the scope.
### 2. A part from the views

**Input:** the views or the sketch, and the list of what to measure.

**Tools:** image reading.
**Done when:** every measurement has a view, or the absence is written down.
### 3. A brief with a citation

**Input:** the financial documents and the question.

**Tools:** file reading.
**Done when:** every fact has a source and a missing datum is explicit.
## Limits and good practice

- The Kimi API list price is 2 dollars of input and 10 dollars of output per million tokens. Cache read is 0.20 dollars. DataCamp reports that this price is flat across the window and sits below K3’s 3 dollars of input and 15 dollars of output.
- Highspeed is faster and, on DataCamp’s account, costs more. Use that variant when the wait is the limit.
- On Kimi Enterprise, K3.1 Pro access starts off. An administrator has to enable it.
- The model writes a lot. Ask for the answer in a format, so you do not pay for text nobody will read.
- Keep a human approval for financial, legal, clinical, or high-impact decisions.

See also: Models and cost · Kimi K3 · Claude Opus 5.5 · Kimi K3.1 Pro — Moonshot · Kimi K3.1 Pro — Artificial Analysis.
