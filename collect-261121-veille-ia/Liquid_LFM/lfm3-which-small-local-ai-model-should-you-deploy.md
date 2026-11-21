---
id: collect-261121-veille-ia/liquid-lfm/lfm3-which-small-local-ai-model-should-you-deploy
title: "LFM3: Which Small Local AI Model Should You Deploy?"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["model", "output", "text", "lfm3", "small", "input", "needs", "need"]
source: https://www.digitalapplied.com/blog/lfm3-small-local-models-deployment-guide
source_anchor: ""
source_lines: [1, 118]
section: "Liquid_LFM"
---
Liquid AI released LFM3 on 27 October 2026 as a family of small models for specialised applications. The important choice is not whether a roughly two-billion-parameter model can replace every cloud model. It is which bounded task can move onto a device, with the right input and output capabilities, an acceptable memory budget and evidence that the result is good enough.

The family includes text, vision, audio, omni and decision variants built around a shared base. Those names do not describe interchangeable endpoints. The text model's 256K context does not become a family-wide guarantee, and a vision model that reads video does not generate video. Choosing the wrong variant can make an otherwise sensible local-AI project fail before quality is even evaluated.

This guide uses Liquid's launch and current model cards, checked on 28 October. Performance figures are vendor-reported unless explicitly described as arithmetic. We have not run these models locally or measured their throughput.

1. 01Choose the modality before the model size.Text, visual understanding, speech and typed decisions have different contracts and runtimes.
2. 02One gigabyte is not a full application budget.Weights, context state, encoders, runtime and concurrency all consume memory.
3. 03Keep the published losses.Choose acceptance criteria before comparing benchmark tables, and retain an escalation path for tasks the small model cannot reliably complete.
4. 04Read the actual licence.The checkpoints use LFM Open License v1.0, with commercial-use conditions, rather than an unrestricted Apache or MIT label.

## 01 — Model selectionStart with the input and *output*

The LFM3 announcement describes six specialised models plus the base checkpoint from which they are developed. A text assistant can begin with the post-trained text model. A system that needs to interpret photographs or video needs the visual variant. A voice application needs to distinguish audio input from speech output, and an application combining sight and sound may need the omni model.

Decision models serve another purpose: they produce typed answers to defined questions rather than a general conversational response. That can be attractive when a workflow needs a bounded classification or extraction result. It does not remove the need to validate confidence, missing-field behaviour and downstream permissions. A probability is a model output that needs calibration for the task, not permission to act without a threshold policy.

Write one representative request and the exact output the application needs. If the request is a spoken question about an image, a text-only checkpoint is not a smaller substitute for an omni model. If the output is a yes/no routing decision, generating a long explanation may be unnecessary overhead. The smallest appropriate model is the one that satisfies the whole contract, not simply the checkpoint with the lowest parameter count.

| Variant | Primary selection question | 
|---|---|
| LFM3-1.7B | Do you need text, structured output or text-based tool use? | 
| LFM3-VL-2B | Do images or video need to be understood? | 
| LFM3-Audio-2B | Do you need audio input and text or speech output? | 
| LFM3-Omni-2B | Must the model combine visual and audio information? | 
| d1-2B | Can text/image work be expressed as typed decisions? | 
| d1-Omni-2B | Do those decisions also need audio input? | 
| LFM3-1.7B-Base | Are you training a model rather than deploying a ready assistant? | 

Source: Liquid AI’s LFM3 launch and model cards, checked 28 October 2026. This is a selection guide, not a benchmark ranking.

## 02 — Task boundaryThe small text model has a clear *limit*

The LFM3-1.7B card lists 1.73 billion parameters and a 256K-token context shared by input and output. It targets local agents, tool calling, structured outputs and domain-specific fine-tuning. Crucially, it advises using retrieval or search for knowledge-heavy tasks and does not recommend the model for complex multi-step reasoning or agentic coding.

That makes bounded application work a more credible starting point than an autonomous software engineer. Consider an assistant that converts a user's instruction into a small set of permitted application actions. The output can be checked against a schema, the available tools can be restricted, and ambiguous requests can be handed back to the user. A correct refusal to choose between two similarly named contacts may be better than a confident guess.

The failure case is allowing a narrow assistant to drift into unrestricted problem solving because it can produce fluent text. A large context window does not supply missing knowledge or guarantee accurate reasoning across every part of a long conversation. Keep the tool set, required evidence and stopping rules close to the task. If the application regularly needs broad research or difficult debugging, include a stronger model or human escalation path in the design.

##### Structured local work

Use a small, checkable output and a limited set of actions. Evaluate missing information and ambiguity as well as normal cases.

##### A frontier replacement

Do not infer complex coding or research capability from model size, context length or a favourable average.

## 03 — Resource budgetSeparate weight memory from working *memory*

Liquid describes the text model as taking roughly one gigabyte at four-bit precision. That is a useful indication of compact weights, but it should not be read as the memory requirement for every running application. A language model also retains state for the current context, and the serving process needs additional allocations. Multimodal variants add components that the text-only estimate does not cover.

The launch gives a separate vendor figure of 1.6GB of key/value cache for the text model at a 128K context. The cache stores attention state used while processing the sequence. It illustrates why a long-running conversation can consume substantially more than the weight file alone. Do not add this figure blindly to every runtime configuration; precision, implementation and workload affect the actual allocation.

Measure on the device and context length the product will use. A short demonstration on a laptop does not establish sustained performance on a phone with other applications running. Include startup, peak memory, temperature-related slowdown and multiple concurrent requests where relevant. A model that fits once but repeatedly disrupts the host application has not met a useful local-deployment requirement. The test needs the complete application, not only a successful model load.

Budget weights, context state, encoders/codecs, runtime allocations and concurrent work separately. Record peak application memory on the target device before calling a deployment lightweight.

## 04 — Different contractsThe audio and omni models are not the text *model*

The Audio card and Omni card list 32K shared context, not the text checkpoint's 256K. Audio accepts text or sound and can return text, speech or both. Omni adds visual inputs, including video with its soundtrack. These are meaningful capability differences, not merely different packaging around the same API request.

A product that lets someone ask a question about a photograph by voice needs to preserve the relationship between the spoken request and the image. A system summarising an audio recording needs to decide whether it will return text or spoken output. The selected runtime may expose only part of the checkpoint's capabilities, so inspect the serving documentation as well as the model card. A successful text response is not proof that the speech path works.

Keep the input and output distinction visible in product planning. Understanding a video means extracting or reasoning about its content; it does not mean producing a new video. Similarly, a speech-capable model still needs audio capture, playback, interruption handling and application-level permissions. Those components can dominate the user's experience even when the model itself is compact and fast.

| Checkpoint | Documented shared context | Important boundary | 
|---|---|---|
| LFM3-1.7B | 256K tokens | Text-only checkpoint | 
| LFM3-Audio-2B | 32K tokens | Text/audio in; text/speech out | 
| LFM3-Omni-2B | 32K tokens | Visual and audio inputs; text/speech outputs | 

Source: the three individual Liquid AI model cards, checked 28 October 2026. Context and runtime support must be checked per variant.

## 05 — Evidence readingAverages can hide the task you care *about*

Liquid publishes extensive comparisons against other small models. The useful approach is to inspect the rows that resemble the intended work, including the rows where LFM3 loses. In the text card's table, LFM3-1.7B scores 38.42 on WebArena-Verified against 28.94 for Qwen3.5-9B, while its GPQA Diamond score is 63.37 against 80.40 for that Qwen model. These are Liquid's evaluations, not our independent runs.

The contrast matters more than a universal winner label. A browser-task result can support testing an application agent, while a knowledge or reasoning result can expose a different limit. The table also combines tests with distinct meanings. An unweighted average across them is a summary chosen by the evaluator; it is not the probability that the model will complete your work correctly.

Build the local comparison around acceptance rather than brand. Use the same documents, tool permissions and output checks for each candidate, and keep runtime differences in the record. If one model gets more attempts or a different tool interface, the experiment is comparing systems as well as models. That can be a legitimate product comparison, but it should be described accurately. Retain the failed cases because they determine where escalation is needed.

A stronger browser score and a weaker knowledge score can both be true. Choose the benchmark row that informs the task, then verify the complete workflow yourself.

## 06 — Commercial useOpen weights come with specific *terms*

The checkpoint is labelled LFM Open License v1.0. The licence file includes commercial-use conditions and defines a US$10 million annual-revenue threshold. It should not be described as Apache 2.0, MIT or an unrestricted commercial permission. The exact legal entity and intended use matter when reviewing the terms.

For a deployment team, the practical step is to keep the licence version with the selected checkpoint and have the intended commercial use reviewed against the actual text. Do this before building a product assumption around free local inference. Download access, ability to fine-tune and permission for a particular commercial deployment are separate facts. A hosted service can also have a separate agreement from the downloadable weights.

This article does not resolve a specific organisation's licence eligibility. The useful technical record is precise: repository, revision, checkpoint format, licence file and any separate agreement governing deployment. Preserve those details when moving from a prototype to a customer-facing product. A model can be technically suitable while its terms require another arrangement, and that is a procurement decision rather than a performance defect.

Save the exact checkpoint and licence together. Do not let a generic “open model” label replace review of the terms attached to the files you deploy.

## 07 — AdaptationFine-tune only after identifying a repeatable *gap*

Liquid presents specialisation as a central use case for the family. The appeal is understandable: a compact model can be adapted to a narrow domain, output format or tool vocabulary. But fine-tuning is not the first remedy for every incorrect answer. Some failures come from missing evidence, an ambiguous instruction, an unsuitable tool or an incorrect evaluation target.

Start by classifying failures. If the model repeatedly misformats a known output despite a clear schema, training may be relevant. If the necessary fact is absent, retrieval may be the better intervention. If two users mean different things by the same instruction, product design may need clarification. A training set built before those distinctions are understood can teach the model to imitate an inconsistent process.

Keep a held-out evaluation set that is separate from training examples. Compare the adapted model with the original on both the target task and important refusal or escalation cases. Improving a narrow accuracy measure while making the model less willing to admit uncertainty can create extra operational work. The result worth deploying is a better bounded application, not simply a higher score on examples similar to the training data.

Name the recurring failure, the evidence available to solve it and the acceptance test. If the problem is missing information or an unclear workflow, fix that before collecting more training examples.

## 08 — DeploymentGive local inference an honest cost *model*

Local execution removes a per-call cloud charge only for the parts that actually run locally. It still uses hardware, power, engineering time and maintenance. A workflow may also call remote search, retrieval or other tools. “On-device model” therefore does not automatically mean offline operation or that all data stays on the device.

For a pilot, document which components run locally and which network requests remain. Test the application with the expected connection state, including an offline mode if that is part of the promise. Measure accepted task completion and sustained latency on the target hardware, then include the support burden of model updates and runtime compatibility. A downloadable GGUF or MLX variant is useful, but the selected runtime must support the exact architecture and capabilities needed.

LFM3 is most compelling when a task can be made specific enough to exploit its compact design. Select the variant, set the memory budget, review the terms and compare it with the current workflow. Keep a fallback for tasks outside the boundary. That gives a small model a fair chance to produce a meaningful saving without requiring it to win every frontier-model benchmark.

Decide whether the complete local application meets its quality, memory, latency and connectivity requirements. Report the cloud calls that remain rather than presenting the model location as the whole architecture.

Continue with other local agent models, runtime memory planning, document retrieval choices. For implementation support, explore our AI transformation services.

### Choose a narrow task and measure the complete local application.

The LFM3 release expands the set of tasks worth testing on a device. Its value comes from matching a specialised checkpoint to a clear requirement, with realistic memory, runtime and licence assumptions. Keep the escalation path and the negative results: they define where the small model earns its place.
