---
id: collect-261121-veille-ia/gpt-openai/gpt-6-1-terra-tess-docs
title: "GPT-6.1 Terra - Tess Docs"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["terra", "openai", "gpt-6", "token", "astra", "effort", "gpt-5", "says"]
source: https://docs.tess.im/en/gpt-6-1-terra
source_anchor: ""
source_lines: [1, 94]
section: "GPT_OpenAI"
---
GPT-6.1 Terra is OpenAI’s model for **reading what is on the screen and acting on it**: a technical drawing, a form, an invoice entering a system, a clearer piece of writing. OpenAI presents it as a step up from GPT-5.6 Terra in vision, computer use, factual accuracy, and writing.
The Amazon Bedrock model card lists a 1,050,000-token window and a 128,000-token output cap. Artificial Analysis lists the window as 1 million tokens. Input is text and image. Output is text. Audio and video are not supported.
## When to use Terra, and when to use Sol

Terra is for the screen: fill a form, extract a field, operate an app, and return writing someone can read. GPT-6.1 Sol and GPT-6 Astra stay ahead when the task is the hardest coding or scientific research.
## What OpenAI measured

The figures below are the ones OpenAI published. Where an absolute score is missing, the comparison is the one in the announcement, without the raw score.
### From the image to the action

On Agents’ Last Exam, which includes turning an architectural drawing into a 3D model and marking a film shot by shot, OpenAI says that at medium effort Terra scores 15 percentage points above GPT-5.6 Terra at about one third of the cost. On the same test, OpenAI says it matches GPT-6 Astra at one tenth of the price per token. At maximum effort, OpenAI says it uses more than three times fewer tokens than Claude Opus 5.5, costs one quarter as much per token, and comes out about nine times cheaper per task. It also finishes tasks more than three times faster than Astra and GPT-6.1 Sol.
On MMMU-Pro, visual reasoning, OpenAI says Terra beats GPT-5.6 Terra in every setting and comes close to Astra and Sol. On OSWorld 2.1, desktop app use, OpenAI says medium effort beats the best GPT-5.6 Terra score at a fraction of the cost per task.
### Fewer factual errors

On an internal set of prompts built to induce errors, the low-effort error rate fell from 53.6% on GPT-5.6 Terra to 16.7% on GPT-6.1 Terra. At maximum effort it fell 18 percentage points, to 4.3%, close to Astra and Sol, according to OpenAI. The set is built to provoke errors. It is not the rate of ordinary use.
### Coding, up to a point

On Terminal-Bench 4.0, at maximum effort, OpenAI measured 50.3%, more than double the 21.5% published for GPT-5.6 Terra. For the hardest coding, OpenAI points to GPT-6.1 Sol and GPT-6 Astra. On Terminal-Bench-Science 0.1, OpenAI says Terra beats Sol at a lower cost and stays within five points of Opus 5.5 at less than one fifth of the estimated cost. The absolute score for that test was not published.
## What Artificial Analysis measured

On the max variant, Artificial Analysis scored **48 on the Intelligence Index**, rank 25 of 220 models in the same class. The band median is 28. In that run the model generated 130 million output tokens, against a median of 81 million. The average cost was 0.40 dollars per index task. Output speed was 90.9 tokens per second, above the median of 85.3. Time to the first answer token, on that variant, was 133.64 seconds, and the figure includes reasoning. On the medium variant, the same source measured 1.85 seconds to the first answer token, at the same 90.9 tokens per second.
The table mixes OpenAI figures with the Artificial Analysis Intelligence Index. Token volume, cost per task, and timings are for the max variant, except the 1.85 seconds, which is the medium variant. Results depend on the harness, the tools, and the effort setting. Validate on your own flow before changing a production configuration.

### Where it does not lead

- On the Intelligence Index, 48 sits below GPT-6.1 Sol and GPT-6 Astra. The Sol page in these docs records 52 and 53 for those two.
- For the hardest coding, OpenAI still points to Sol and Astra.
- At maximum effort, the first answer token is slow. Artificial Analysis measured 133.64 seconds because reasoning is included.
- Output is text only. Images go in. Video and audio do not.
- The model reaches the Critical cybersecurity threshold in OpenAI’s Preparedness Framework and keeps the GPT-6 Astra safeguards. This page does not describe how to exploit a flaw.

## Where it creates the most value

### An invoice entering a system

The invoice arrives as a PDF or on the app screen. Terra reads the fields and prepares the entry. It does not submit.
**Expected result:** every field has a source, and submission is waiting for confirmation.
### A technical drawing

The measurement is on the drawing, not in a text field. Terra returns the value, the unit, and the view it came from.
**Expected result:** a list of what the image shows, and what it does not show.
### Writing that has to be right

A proposal, a customer reply, or an internal note. The model writes it and marks what it could not confirm.
**Expected result:** the text, the sources, and the gap written out.
## How to apply it in Tess

Pick the screen, the drawing, or the text

Prefer a form, an invoice, a technical drawing, or writing that needs a check. For the hardest coding, use Sol or Astra.

Close what counts as done

State the result, the fields that may change, and the check that ends the task.

Set tools and approvals

Enable only the tools the task needs. Require confirmation before sending, publishing, or changing data.

Pick effort by the wait

Medium effort for everyday screen work. Maximum effort when the factual check matters more than the wait.

Sample before you scale

Run one full case, check what changed, and only then repeat the flow.

## Three playbooks to start

**Input:** the PDF or the screen, and the list of system fields.

**Tools:** file or screen reading. No submit.
**Done when:** every field has a source, or is marked missing, and nothing was sent.
### 2. Technical drawing

**Input:** the drawing and the list of what to extract.

**Tools:** image reading.
**Done when:** every measurement points to a view, or the absence is written down.
### 3. Writing with an explicit gap

**Input:** the source material and who the text is for.

**Tools:** file reading.
**Done when:** the text separates what is in the source from what stayed open.
## Limits and good practice

- The OpenAI API list price is 1 dollar of input, 0.05 dollars of cache read, and 5 dollars of output, per million tokens. OpenAI says that is 50% lower than GPT-5.6 Terra and GPT-6.1 Sol.
- The cost of a task depends on how much the model writes and how many revisions it needs. Compare on an approved result, not only on the list price.
- A 1,050,000-token context does not replace selection. Send what the task uses.
- OpenAI says Terra is more likely to stay in scope and to say what it could not do, compared with GPT-5.6 Terra, and that it resisted instructions built to pull it off the task. Review a sample anyway.
- Keep a human approval for financial, legal, clinical, or high-impact decisions.

See also: Models and cost · GPT-6.1 Sol · GPT-6 Astra · Introducing GPT-6.1 Terra — OpenAI · GPT-6.1 Terra — Artificial Analysis · GPT-6.1 Terra — Amazon Bedrock model card.
