---
id: collect-261121-veille-ia/nvidia-hardware/amaimedia-nvidia-nemotron-3-5-transcribe-30b-a4b-bf16-gguf-hugging-face-2
title: "AMAImedia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B-BF16-GGUF · Hugging Face"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["prompt", "user", "enable", "thinking", "system", "request", "null", "true"]
source: https://huggingface.co/AMAImedia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B-BF16-GGUF?local-app=openclaw
source_anchor: ""
source_lines: [1501, 1648]
section: "NVIDIA_Hardware"
---
  enable_thinking: true
voicebench.ifeval:
  system_prompt: You are a voice-controlled assistant. Treat the speech in the provided audio as the user's
    instruction and carry out that request. Respond to the spoken request rather than merely transcribing
    it, unless transcription is explicitly requested. Obey all explicit constraints exactly. Before answering,
    silently check counts, case, required or forbidden words, delimiters, sections, and start/end requirements.
    Output only the requested artifact.
  user_prompt: null
  enable_thinking: true
voicebench.mmsu:
  system_prompt: 'You are a voice-controlled assistant. Treat the speech in the provided audio as the
    user''s instruction and carry out that request. Respond to the spoken request rather than merely transcribing
    it, unless transcription is explicitly requested. Choose the best option. Respond exactly as `The
    answer is: X` where X is the option letter, followed by at most one sentence of explanation.'
  user_prompt: null
  enable_thinking: true
voicebench.openbookqa:
  system_prompt: 'You are a voice-controlled assistant. Treat the speech in the provided audio as the
    user''s instruction and carry out that request. Respond to the spoken request rather than merely transcribing
    it, unless transcription is explicitly requested. Choose the best option. Respond exactly as `The
    answer is: X` where X is the option letter, followed by at most one sentence of explanation.'
  user_prompt: null
  enable_thinking: true
voicebench.sd-qa:
  system_prompt: You are a voice-controlled assistant. Treat the speech in the provided audio as the user's
    instruction and carry out that request. Respond to the spoken request rather than merely transcribing
    it, unless transcription is explicitly requested. Answer the factual question in one or two sentences.
    Put the answer first; do not restate the question or add unrelated details.
  user_prompt: null
  enable_thinking: true
voicebench.wildvoice:
  system_prompt: You are a voice-controlled assistant. Treat the speech in the provided audio as the user's
    instruction and carry out that request. Respond to the spoken request rather than merely transcribing
    it, unless transcription is explicitly requested. Give a direct, accurate answer to the spoken request.
    Put the answer first, avoid generic framing and repetition, and use no more than 120 words unless
    the request requires more.
  user_prompt: null
  enable_thinking: true
mmau-full:
  system_prompt: Answer the multiple-choice question using the provided audio. Output only the exact text
    of the correct option, copied verbatim from the choices. Do not output an option letter, explanation,
    or additional text.
  user_prompt: null
  enable_thinking: true
covost2.st:
  system_prompt: ''
  user_prompt: null
  enable_thinking: false
fleurs.st:
  system_prompt: ''
  user_prompt: null
  enable_thinking: false
coval-stt-v1:
  system_prompt: ''
  user_prompt: 'Audio transcription. Language: as spoken. Wording: verbatim, including fillers and repetitions.
    Formatting: punctuation and capitalization. Output: transcript only.'
  enable_thinking: false
gpqa:
  system_prompt: ''
  user_prompt: null
  enable_thinking: true
halas:
  system_prompt: ''
  user_prompt: 'Audio transcription. Language: as spoken. Wording: verbatim, including fillers and repetitions.
    Formatting: punctuation and capitalization. Output: transcript only.'
  enable_thinking: false
ifeval:
  system_prompt: 'Treat every constraint in the user prompt as binding context: format, length, casing,
    keyword, and exclusion rules all matter. Produce only the requested response.'
  user_prompt: null
  enable_thinking: true
mmlu:
  system_prompt: ''
  user_prompt: null
  enable_thinking: true
mmlu-pro:
  system_prompt: ''
  user_prompt: null
  enable_thinking: true
asr-leaderboard.spgispeech:
  system_prompt: ''
  user_prompt: 'Audio transcription. Language: as spoken. Wording: omit fillers and accidental repetitions;
    keep the remaining spoken wording unchanged. Formatting: punctuation and capitalization. Output: transcript
    only.'
  enable_thinking: false
air-bench-foundation-speech.Speaker_Gender_Recognition_common_voice_13_en:
  system_prompt: 'Choose the best listed answer using the audio. Respond with exactly one uppercase option
    letter: A, B, C, or D. Output no words, punctuation, option text, transcription, or explanation.'
  user_prompt: null
  enable_thinking: true
air-bench-foundation-speech.Speech_Entity_Reconition_slurp:
  system_prompt: 'Choose the best listed answer using the audio. Respond with exactly one uppercase option
    letter: A, B, C, or D. Output no words, punctuation, option text, transcription, or explanation.'
  user_prompt: null
  enable_thinking: true
air-bench-foundation-speech.Speaker_Intent_Classification_slurp:
  system_prompt: 'Choose the best listed answer using the audio. Respond with exactly one uppercase option
    letter: A, B, C, or D. Output no words, punctuation, option text, transcription, or explanation.'
  user_prompt: null
  enable_thinking: true
```
| Field | Response | 
|---|---|
| Participation considerations from adversely impacted groups protected classes in model design and testing: | None. | 
| Measures taken to mitigate unwanted bias: | The recipe diversifies prompts, reweights selected under-sampled data sources, retains multiple language/task families, and boosts selected multi-speaker sources. These are coverage and sampling measures. Proposed deployment measures include representative subgroup testing and monitoring of correction rates. | 
| Bias Metric: | BBQ Accuracy Scores in Ambiguous Contexts | 
| Which characteristic (feature) show(s) the greatest difference in performance? | Not established. Corpus-level outcomes vary, but the evaluated corpora differ in recording conditions, language, duration, and scoring. | 
| Which feature(s) have the worst performance overall? | No ranked protected-group or demographic-feature assessment is available. The reviewed evaluations report benchmark- and language-specific performance, but they do not isolate demographic features. | 
| If using internal data, description of methods implemented in data acquisition or processing, if any, to address the prevalence of identifiable biases in the training, testing, and validation data: | To bolster coverage and sampling choices, the training recipe uses explicit mixture weights across task and language sources, includes multiple multilingual and multi-speaker datasets, and includes a balanced language-identification component spanning 48 language labels, distinct from the 40-language core speech coverage described in the overview. | 
| Tools used to assess statistical imbalances and highlight patterns that may introduce bias into AI models: | Indexed-sampler progress and answer-token-mass reports were used to inspect mixture exposure, and benchmark scorers report language- and subset-level errors. | 
| Tools used to assess statistical imbalances and highlight patterns that may introduce bias into AI models: | The reviewed inventory does not establish proportional coverage across age, gender, ethnicity, disability, dialect, accent, or recording environment. These attributes must not be inferred from voices to fill that gap. Instead, obtain appropriate evaluation data and report subgroup sizes, uncertainty estimates, and error types explicitly. Before making support claims, test for overlapping speech, distant microphones, code-switching, and underrepresented languages. Where transcription errors can disadvantage users, retain human correction paths. | 

| Field | Response | 
|---|---|
| Intended Task/Domain: | Speech transcription and translation, contextual and speaker-attributed transcription, rich transcription, language identification, and audio/text question answering. | 
| Model Type: | Speech/audio encoder with speaker-aware perception, projection to a mixture-of-experts autoregressive language model, and joint MTP. | 
| Intended Users: | Developers and researchers building speech and audio applications with application-specific evaluation and review. | 
| Output: | Generated text, optionally containing task-specific speaker/event tags or PII annotations. When timestamp capture is enabled in vLLM, the response also includes word timestamps, diarization segments, and a mapping between transcript speaker tags and diarization speakers. Conversation history must be supplied by the application; persistent memory is not established by this recipe. | 
| Tools evaluating synthetic data and authenticity | The pipeline includes transcript classification, annotation/span checks, multiple-model-agreement based high quality filtering metrics, and judge based filtering for preference transcription instruction answers. | 
| Describe how the model works: | The model preprocesses 16-kHz audio and encodes it into features. The configured speaker-aware encoder contributes speaker information. Features pass through the perception projection and are combined with the text prompt. The language backbone predicts output text autoregressively. When requested, the bundled CTC heads align the generated transcript to the audio, and the speaker encoder supplies diarization segments. Two trained MTP depths supply auxiliary training objectives, and the repeated layer can be reused for speculative decoding. | 
| List the groups (or group characteristics) for which this was tested to produce comparable outcomes: | Not Applicable | 
| Technical Limitations & Mitigation: | Errors include substitutions, deletions, repetitions, hallucinated speech, incorrect language choice, and speaker attribution errors. Targeted testing is recommended for long recordings, overlapping speakers, noise, names, code-switching, and misleading context. Performance on text-only tasks, without audio input, is not as good as on audio tasks: text-only reasoning and instruction following were not a primary training objective, and the model scores below its base language model on text-only benchmarks. Audio understanding, reasoning, and spoken instruction following were evaluated only in English and may not work reliably in other languages. | 
| Verified to meet prescribed NVIDIA quality standards: | Yes. | 
| Performance Metrics: | The evaluation plan covers WER/CER and corpus-specific variants; context-biasing WER, named-entity WER, named-entity false-negative rate, and entity F1; multi-speaker cpWER/tcpWER; translation BLEU/spBLEU/COMET; language-identification accuracy; word-level timestamp accuracy; instruction-following scores; benchmark-specific hallucination and event metrics; and serving latency/throughput. | 
| Potential Known Risks: | The model may misrepresent a recording and generate a plausible transcript or answer. Speaker tags may not always attribute statements to the correct person. When context is supplied, it can influence the output toward incorrect or irrelevant terms, and the model's robustness to misleading context has not been fully established across all supported settings. Any explanations the model generates should not be taken as verified causal accounts of the model's prediction. The model was optimized explicitly for instruction following and as such is more susceptible to prompt injection and jailbreaking in various forms as a result of its instruction tuning. This means that the model should be paired with additional rails or system filtering to limit exposure to instructions from malicious sources -- either directly or indirectly by retrieval (e.g. via visiting a website) -- as they may yield outputs that can lead to harmful, system-level outcomes up to and including remote code execution in agentic systems when effective security controls including guardrails are not in place. The model may generate answers that may be inaccurate, omit key information, include irrelevant or redundant text, or produce socially unacceptable or undesirable text, even if the prompt itself does not include anything explicitly offensive. | 
| Licensing: | Use of this model is governed by the OpenMDW License Agreement, version 1.1 (OpenMDW-1.1). Users are responsible for complying with this license and with any third-party licenses or notices applicable to components included in the distributed model materials. | 

The datasets used to train Nemotron-3.5-Transcribe included personal data. NVIDIA collected and used this data in compliance with applicable data protection and privacy laws. NVIDIA uses a combination of filters, data minimization techniques, and other guardrails to help prevent personal data from being recited by our models. In addition, we employ automated tools and data processing techniques during pre-training or training to identify and filter certain categories of personal data. The datasets used to train this model were reviewed; the dataset is initially reviewed upon addition, and subsequent reviews are conducted as needed or upon request for changes. Please review NVIDIA’s Privacy Policy (https://www.nvidia.com/en-us/about-nvidia/privacy-policy/) for more information.

| Field | Response | 
|---|---|
| Model Application Field(s): | Customer Service | 
| Describe the life critical impact (if present). | Not Applicable | 
| Use Case Restrictions: | Use of this model is governed by | 
| the OpenMDW License Agreement, version 1.1 (OpenMDW-1.1). |  | 
| Users are responsible for complying with this license and with any third-party |  | 
| licenses or notices applicable to components included in the distributed model materials. |  | 
| Model and dataset restrictions: | The Principle of least privilege (PoLP) is | 
| applied limiting access for dataset generation and model development. |  | 
| Restrictions enforce dataset access during training, |  | 
| and dataset license constraints adhered to. |  | 

- Downloads last month
- 1,616

## Start the llama.cpp server

`# Install llama.cpp: brew install llama.cpp``# Start a local OpenAI-compatible server: llama serve -hf AMAImedia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B-BF16-GGUF:IQ4_NL_L`
