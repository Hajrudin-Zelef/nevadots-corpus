---
id: collect-261121-veille-ia/muse/meta-muse-apis
title: "Meta Muse API’s"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "model", "meta", "spark", "files", "code", "text", "models"]
source: https://medium.com/@santoshpasunoorureddy/meta-muse-apis-809ede1ed8de
source_anchor: ""
source_lines: [1, 27]
section: "Muse"
---
*Physics is like sex: sure, it may give some practical results, but that’s not why we do it. — Richard Feyman if you can’t explain it to a six-year-old, you don’t understand it yourself — Albert Einstein*

Meta Model API is an inference API means that it acts as a serving API for the LLM. like if you want to ask a question to AI LLM, you need to have a platform to ask it, this is known as inference. meta model API works in that way. Meta Code is like a coding terminal just like codex and claude. Meta code is also used by Meta Spark for building applications.

Muse Code :- Muse Code is a coding agent you run in a terminal or in CI (automated build pipelines). You install it, sign in, and point it at a project; it can plan changes, edit files, and run commands, with approval prompts and a sandbox (an isolated environment that limits what commands can damage) turned on by default.

Muse Wonder is the most capable model in the family. It is suited to large software engineering jobs such as changing many files in a code repository, delegating work to sub-agents (smaller helper AI runs), and running long multi-step tasks. Its context window of 1M tokens means it can read a very large amount of text in one request; a token is roughly a word fragment. It is currently in preview.

Muse Spark is designed for agentic work (AI that takes multiple actions with tools) and coding at scale. It supports parallel tool calls, which means it can request several tools at once, and streamed tool-call arguments, which lets you see the request being built as it arrives. It also keeps its reasoning across conversation turns and has the same 1M-token context window.

Muse Ember is the efficient, lower-cost model in the family. It accepts the same requests as Muse Spark but charges less per token (the unit of text used for billing). It is well suited to repetitive tasks such as routing requests, calling tools, running sub-agent subtasks, and processing large batches of images, files, or records. A common pattern is letting a larger model plan while Muse Ember does the volume.

Beyond the coding models, Meta provides specialized models for images, speech, and segmentation (finding and outlining objects in pictures or video). Some models are open-weight, meaning you can download them and run them yourself under their own license terms.

Muse Image and Muse Voice are specialized models handle media. Muse Image creates and edits pictures from text descriptions, and Muse Voice Transcribe turns spoken audio into text in real time or from files.

Muse Voice Speech-to-text converts spoken words into written text. This model handles live streaming audio and recorded files, can detect when speakers take turns, can label which speaker said what (speaker diarization), and can be biased toward your own vocabulary such as product names. It supports 25 evaluated languages and returns timestamps per turn, not per word. It does not generate speech.

**Open Weights Model to run ourself:-**

Open-weight models let you download the model files and host them on your own GPUs or devices instead of paying per call. This gives more control over privacy and cost, but you are responsible for the hardware and setup. Each model has its own license terms.

Muse Spark Open is the open-weight version of Muse Spark. It is a very large mixture-of-experts model (a design where only part of the network runs for each token), and you serve it with a runtime such as SGLang or vLLM, which are open-source inference servers. You can also use a hosted provider instead. Its pricing and specs are separate from Model API.

Muse Glimmer is an open-weight multimodal model distilled (a smaller model trained to mimic a larger one) from Muse Spark. It is designed to run on your own hardware, including edge devices close to the user. It is released under the permissive Apache 2.0 license, which allows broad commercial use.

Segment Anything Model SAM — SAM 3.1 is an open-weight segmentation model. You give it a short text prompt naming an object, such as the word car, and it returns a bounding box and a pixel-accurate mask for every match in an image or video. In video it can follow each object across frames.
