---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nemotron-3-5-transcribe-one-speech-llm-with-parallel-expert-encoders-diarization-and-promptab
title: "NVIDIA Nemotron 3.5 Transcribe: One Speech LLM with Parallel Expert Encoders, Diarization, and Promptable Output | Naren Vadapalli"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["model", "nvidia", "nemotron", "transcribe", "speech", "diarization", "promptable", "parallel"]
source: https://www.narenvadapalli.com/blog/nvidia-nemotron-3-5-transcribe-speech-llm-moe/
source_anchor: ""
source_lines: [1, 417]
section: "NVIDIA_Hardware"
---
# NVIDIA Nemotron 3.5 Transcribe: One Speech LLM with Parallel Expert Encoders, Diarization, and Promptable Output

Inside NVIDIA Nemotron 3.5 Transcribe: A 34B hybrid Mamba-2/Transformer MoE speech LLM uniting ASR, diarization, and promptable formatting in a single model pass.

*(Previous) NVIDIA Advances Isaac Platform for General-Purpose Robotics: Agile ONE SN Reference Humanoid, Isaac Lab 3.0 Multi-Physics, and the SimReady Pipeline | (Next) Gemini 4 Carbon: Googleâs Frontier Reasoning Model for Autonomous Cybersecurity and Long-Horizon Software Engineering*

### Summary

For over a decade, enterprise speech-to-text infrastructure has been architected as an unwieldy pipeline cascade: an acoustic Automatic Speech Recognition (ASR) model transcribes phonemes, a separate speaker diarization network clusters embeddings to identify who spoke when, a punctuation-and-capitalization model cleans the raw text, an alignment engine computes word timestamps, and an external Large Language Model (LLM) rewrites disfluencies or formats specialized domain acronyms. This multi-model conveyor belt creates severe operational liabilities: cascading latency, compounded error propagation, lost acoustic intonation, and rigid, un-promptable output conventions that require complete retraining to alter.

As voice artificial intelligence explodes across customer service, clinical scribing, and real-time meeting agents, the industry has reached a crossroads. While proprietary audio APIs lock enterprises into closed black-box endpoints, open infrastructure has demanded a unified, customizable foundation model. On October 28, 2026, NVIDIA Research released **NVIDIA Nemotron 3.5 Transcribe** (`NVIDIA-Nemotron-3.5-Transcribe-30B-A4B`), an open-weights Speech-Augmented Language Model (SALM) that replaces the entire multi-stage cascade with a single, promptable model pass. Built upon a 34-billion-parameter hybrid **Mamba-2âTransformer Mixture-of-Experts (MoE)** backbone that activates just 4.3 billion parameters per token, Nemotron 3.5 Transcribe introduces a **Parallel Expert Encoder** combining a trainable Transformer ASR encoder with a frozen Sortformer diarizer to inject speaker states directly into acoustic frames before decoding.

Trained on NVIDIAâs newly released **Granary-v2** dataset (spanning 843,600 audio hours across 48 languages), the model allows developers to steer transcription formatting dynamically using natural language instructions. By prompting the model, teams can toggle between verbatim transcripts, clean written text, speaker-attributed dialogue, entity-biased domain transcription, and multilingual translation across 40 languages. Deployed via NVIDIA NIM in NVFP4 precision, the model achieves a record-breaking **2,238x real-time throughput** while establishing state-of-the-art accuracy on both the Open ASR and VoiceArena leaderboards.

### Hugging Face / Official Model Card Summary

The following table summarizes the official model specifications, architecture details, and open-weights distribution channels for NVIDIA Nemotron 3.5 Transcribe:

| Specification | Details & Metrics | 
|---|---|
| **Model Repository** | `nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B` | 
| **Developer / Lab** | NVIDIA Research / NeMo Speech Team | 
| **Release Date** | October 28, 2026 | 
| **Total Parameters** | ~34 Billion | 
| **Active Parameters** | ~4.3 Billion (Sparse MoE, Top-2 Routing) | 
| **LLM Decoder Backbone** | Nemotron 3.5 Lightning (Hybrid Mamba-2 State Space + Transformer Attention + MoE MLPs) | 
| **Acoustic Encoders** | Trainable Transformer ASR Encoder (~600M params) + Frozen Sortformer Diarizer (~100M params) | 
| **Curation Dataset** | Granary-v2 ( `nvidia/Granary-v2` ) (843,600 audio hours across 48 languages) | 
| **Max Context Window** | 65,536 tokens (~1 hour of continuous audio per inference request) | 
| **Multilingual Breadth** | 40 languages transcription; speech translation from 31 languages into English | 
| **Max Concurrent Speakers** | Up to 8 speakers tracked simultaneously via Sortformer slot projection | 
| **English Benchmark** | **4.71% mean WER** across 9 Open ASR Leaderboard test sets | 
| **Multilingual Benchmark** | **4.62% mean WER** across Open ASR Multilingual test sets | 
| **Peak Serving Throughput** | Up to **2,238x real-time factor** (RTF) in NVFP4 precision via NVIDIA NIM | 
| **Release Checkpoints** | BF16, FP8, NVFP4, and GGUF community formats | 
| **License** | OpenMDW-1.1 (Permissive Open Model Derived Work) | 
| **Official Technical Report** | NVIDIA Nemotron 3.5 Transcribe Technical Report (PDF) | 
| **Official Project Page** | NVIDIA Nemotron 3.5 Transcribe Research | 

### Prior Reading Material

To contextualize how state-space architectures, mixture-of-experts routing, and high-throughput inference runtimes converge in this release, review these foundational deep dives:

- NVIDIA Nemotron Labs Dory 1.0 4B: The Looped 4B With a Runtime Compute Dial â Normalized GPT (nGPT) recurrency, NVFP4 pretraining, and test-time latent scaling.
- NVIDIA Nemotron 3.5 Bolt E4B 4GB: Fast Conversational Edge Agents, Pure RL Post-Training, and Speculative Drafters â Distillation of Nemotron architectures, speculative decoding, and edge serving.
- Meta Muse Spark Open: Inside the 816B Multimodal Mixture-of-Experts Architecture, NVFP4 Quantization, and Native Interleaved Audio-Video Ingestion â Cross-modal token ingestion, sparse MoE dispatch, and FP4 quantization at scale.
- vLLM v0.32.0: /v1/systemone Structured Decisions, NVFP4 KV Cache Everywhere, MRv2 Multi-GPU Speculative Decoding, and the 2.4x Serving Leap â NVFP4 cache compression, continuous batching, and multi-token speculative serving mechanics.
- NVIDIA Nemotron 3.5 Super VL: 120B MoE Multimodal Architecture, 600+ TPS Agentic Vision, and the End of Monolithic VLMs â High-speed multimodal expert dispatch and sub-token routing pipelines.

### The Conventional Speech Cascade Dilemma

As highlighted by TechCrunchâs coverage of the voice AI market, voice interfaces are undergoing a massive commercial renaissance. Yet while consumer applications adopt natural conversational agents, enterprise transcription pipelines have remained painfully fragile:

*âTraditional speech recognition systems fix the transcription format during training. When developers need verbatim text, normalized written forms, speaker labels, or domain-specific vocabulary, they are forced to string together separate ASR models, diarizers, punctuation engines, and language modelsâcompounding latency, propagating errors, and inflating serving costs at every stage.â*
â **NVIDIA Nemotron Speech Team**, NVIDIA Nemotron 3.5 Transcribe Technical Report


In traditional speech infrastructure, turning a meeting recording or customer phone call into structured, actionable text requires stringing together four to five disparate neural networks:

```
flowchart TD
    A["Raw Audio Stream: 16 kHz Waveform"] --> B["Acoustic ASR Engine: Raw Lowercase Text Stream"]
    B --> C["Independent Diarization Engine: Speaker Segmentation & Clustering"]
    C --> D["Punctuation & Capitalization Model: Text-Based Sentence Boundaries"]
    D --> E["Timestamp Forced Aligner: Audio-to-Token Dynamic Programming"]
    E --> F["External Downstream LLM: Acronym Formatting, Disfluency Cleanup & Translation"]
    style A fill:#0d2b45,stroke:#00e5ff,stroke-width:2px,color:#ffffff;
    style B fill:#112233,stroke:#38bdf8,stroke-width:2px,color:#ffffff;
    style C fill:#1e1e38,stroke:#818cf8,stroke-width:2px,color:#ffffff;
    style D fill:#261833,stroke:#c084fc,stroke-width:2px,color:#ffffff;
    style E fill:#1a2e26,stroke:#34d399,stroke-width:2px,color:#ffffff;
    style F fill:#2d2215,stroke:#fbbf24,stroke-width:2px,color:#ffffff;
```
First, a Conformer or Whisper-style encoder-decoder extracts phonemes into raw text. Because this ASR model operates strictly on acoustics, it outputs a stream of unpunctuated lowercase words. Next, a speaker diarization model segments audio windows, generates speaker embeddings, runs agglomerative hierarchical clustering, and produces time boundaries. Then, an independent punctuation and capitalization model attempts to insert sentence boundaries based purely on textual heuristics, frequently misinterpreting pauses and vocal inflections. Finally, downstream applications pass this stitched payload to a general-purpose LLM to clean up stuttering, resolve domain acronyms, or translate into other languages.

This multi-model pipeline introduces three critical engineering flaws:

1. **Compounding Latency & Memory Overhead** : Each model in the chain forces intermediate tensor serialization, memory copies across GPU buffers, and sequential kernel launches. Latency scales additively across every stage.
2. **Contextual & Acoustic Disconnect** : The downstream LLM correcting the text has zero access to the original acoustic waveform. It cannot determine if a hesitation was an intentional pause, whether vocal emphasis altered meaning, or if laughter signaled sarcasm.
3. **Inflexible Output Formatting** : Traditional ASR models are hardcoded at training time. If an enterprise requires verbatim legal transcripts, they must host one model; if they require clean executive meeting minutes, they must host a different model or pay heavy token costs running an LLM rewriter.

Nemotron 3.5 Transcribe eliminates this entire assembly line. By unifying audio encoding, speaker tracking, and generative decoding into a single foundation model, the system executes verbatim transcription, written normalization, contextual entity biasing, speaker attribution, and translation in a single forward pass.

*Figure 1: NVIDIA Nemotron 3.5 Transcribe unified system architecture, illustrating the Parallel Expert Encoder, Nemotron 3.5 Lightning hybrid Mamba-2/Transformer MoE backbone, MTP speculative decoding, and promptable output formats. Source: NVIDIA Research.*

### The Parallel Expert Encoder: Fusing ASR and Diarization

*âOne speech LLM. Many ways to write down what was said.â*
â **NVIDIA Research**, Nemotron 3.5 Transcribe Overview


The core architectural breakthrough enabling unified transcription is the **Parallel Expert Encoder**, detailed in the NVIDIA Technical Report. In earlier speech-augmented LLMs, researchers attempted to train a single monolithic audio encoder to learn both phonetic transcription and speaker differentiation simultaneously. This universally degraded performance: phonetic representation learning requires invariant representations across different pitch and vocal timbres, whereas speaker diarization demands maximizing variance across distinct speaker characteristics.

Nemotron 3.5 Transcribe resolves this tension by deploying two specialized, parallel pathways:

```
flowchart TD
    A["Raw Audio Input: 16 kHz Log-Mel Spectrogram (80-Channel)"] --> B["Trainable Transformer ASR Encoder: 600M Parameters"]
    B --> C["Frozen Sortformer Diarizer: 8-Speaker Activity Probabilities"]
    C --> D["Binarization & Sinusoidal Projection: Speaker Embedding Matrix"]
    D --> E["Frame-Level Elementwise Summation: Injected Acoustic Representations"]
    E --> F["Nemotron 3.5 Lightning LLM: Autoregressive Decoder Input"]
    style A fill:#0d2b45,stroke:#00e5ff,stroke-width:2px,color:#ffffff;
    style B fill:#0f2b38,stroke:#00e5ff,stroke-width:2px,color:#ffffff;
    style C fill:#2a1435,stroke:#d946ef,stroke-width:2px,color:#ffffff;
    style D fill:#1b1b3a,stroke:#6366f1,stroke-width:2px,color:#ffffff;
    style E fill:#0f382c,stroke:#10b981,stroke-width:2px,color:#ffffff;
    style F fill:#2e2010,stroke:#f59e0b,stroke-width:2px,color:#ffffff;
```
#### 1. The Trainable ASR Transformer Encoder (~600M Parameters)

The linguistic pathway takes 80-channel log-mel filterbank energies extracted from 16 kHz audio. It downsamples time frames through an initial convolutional subsampling block, reducing the temporal resolution to an 80 ms frame stride, followed by a 24-layer Transformer encoder with multi-head self-attention. This branch focuses strictly on capturing phonetic and linguistic information.

#### 2. The Frozen Sortformer Diarization Expert (~100M Parameters)

Operating in parallel is a frozen **Nemotron 3 Diarization** model built on the Sortformer architecture. Sortformer processes the identical audio stream but outputs an 8-dimensional activity vector for every 80 ms time frame, estimating the activity of up to 8 distinct speakers. Because Sortformer handles overlapping speech natively, multiple speaker channels can simultaneously indicate active speech without collision.

#### 3. Cross-Injection Mechanics

To feed speaker identities into the language model without disrupting phonetic features:

1. The continuous speaker activity probabilities are thresholded into binary active speaker indicators.
2. The active speaker indices are projected into a dense vector space using a fixed sinusoidal positional embedding pattern.
3. This speaker embedding is normalized and added directly to the output of the ASR Transformer encoder.

Because speaker metadata is fused directly into the acoustic frames before they enter the language model, the LLM decoder sees who is speaking at the exact moment it decides what word is being spoken.

*Figure 2: Architectural schematic of the Parallel Expert Encoder, showing how Sortformer diarizer activity vectors are projected and injected into the Transformer acoustic stream before decoding. Source: NVIDIA Research.*

### The Granary-v2 Data Engine: 843,000 Hours of Multilingual Curation

*âSpeech foundation models are defined by the diversity and structure of their curation pipeline. Granary-v2 is designed to make every word matterâproviding aligned, multi-task training targets across 48 languages so that a single speech LLM can follow nuanced human formatting prompts out of the box.â*
â **NVIDIA NeMo Curator Team**, Hugging Face Granary-v2 Launch


A core differentiator documented on the Hugging Face Granary-v2 launch blog is the training data foundation. Foundation speech models live or die by their curation pipeline, particularly when learning to follow prompt instructions across diverse speech styles.

NVIDIA built and open-sourced **Granary-v2** (`nvidia/Granary-v2`), an industrial-scale multilingual data curation system powered by **NeMo Curator**:

- **Unprecedented Scale** : Expanding from 25 to 48 languages, Granary-v2 encompasses approximately**843,600 unique audio hours** across public and licensed corpora.
- **Multi-Task Aligned Targets** : Rather than merely matching audio to a single reference transcript, Granary-v2 generates paired targets for every audio sample:
  1. *Verbatim transcripts* preserving disfluencies (âuhâ, âumâ, stuttering).
  2. *Clean written transcripts* with standardized numbers, casing, and punctuation.
  3. *Code-switched text* capturing seamless mid-sentence language transitions.
  4. *Multi-speaker attribution tags* aligned with turn boundaries.
  5. *Audio Question-Answering pairs* capturing conversational intent and meeting summaries.
- **Open Recipes for Enterprise Fine-Tuning** : NVIDIA published the curation pipeline scripts, filtering heuristics, and instruction-tuning pairs, allowing enterprises to apply the exact same curation recipe to proprietary customer call archives or internal clinical audio.

*Figure 3: Granary-v2 data curation architecture following an audio sample through the Metadata, ASR, and Text pipelines to generate paired multi-task training targets across 48 languages. Source: NVIDIA NeMo Curator / Hugging Face.*

### State-of-the-Art Benchmarks: Pareto Frontiers & Overlapping Speech

Independent evaluations highlight where Nemotron 3.5 Transcribe shifts the state-of-the-art across open-weight models:

#### 1. Artificial Analysis Pareto Frontier

On the Artificial Analysis Speech Benchmark (evaluating non-streaming Word Error Rate across standardized conversational speech test sets), Nemotron 3.5 Transcribe defines the open-weights Pareto frontier:

- **Lowest WER per Parameter** : Activating roughly 4.3 billion parameters, it achieves an impressive**2.80% Word Error Rate** on Artificial Analysis evaluations, matching or outperforming models that require two to three times the compute footprint.

*Figure 4: Artificial Analysis evaluation plotting non-streaming Word Error Rate (AA-WER) against active parameters on a log scale. Nemotron 3.5 Transcribe establishes the open-weights Pareto frontier at ~4B active parameters and 2.80% WER. Source: Artificial Analysis.*

#### 2. Conquering Overlapping Dialogue: VoiceArena Diarization-Bench

In real-world meeting recordings, participants routinely interrupt each other, chime in with acknowledgments, and speak over one another. Standard ASR systems fail catastrophically during overlap, either dropping one speaker entirely or interweaving phonemes into nonsensical strings.

On **VoiceArenaâs Diarization-Bench v1** (which measures concatenated minimum-permutation word error rate, or **cpWER**, across full sessions with heavy conversational overlap):

- **Nemotron 3.5 Transcribe ranks #1** among all tested open-weight and commercial systems with a**32.4% cpWER** , outperforming both specialized multi-model cascades (such as Multitalker Parakeet paired with standalone diarizers) and monolithic baselines.

*Figure 5: VoiceArena Diarization-Bench v1 ranking 14 diarization-plus-ASR systems on English full sessions with overlapping speech by concatenated minimum-permutation word error rate (cpWER). Nemotron 3.5 Transcribe ranks #1 at 32.4%. Source: VoiceArena / Hugging Face.*

### Nemotron 3.5 Lightning: The Hybrid Mamba-2 / MoE Backbone

Processing up to one hour of continuous audio generates an immense sequence of acoustic tokens. At an 80 ms frame stride, one hour of audio yields 45,000 acoustic frames. Standard Transformer attention scales quadratically in memory and compute, causing conventional LLMs to exhaust GPU VRAM or experience severe prefill latency when ingesting long recordings.

To conquer this bottleneck, Nemotron 3.5 Transcribe employs **Nemotron 3.5 Lightning**, a state-of-the-art hybrid architecture that interleaves three distinct layer types:

1. **Mamba-2 State Space Layers** : Mamba-2 operates with linear memory and compute scaling, maintaining an ongoing hidden state that compresses historical acoustic context across thousands of tokens with zero KV-cache explosion.
2. **Sparse Transformer Attention Layers** : Periodically interleaved full attention layers (utilizing a 65,536-token context window) maintain sharp cross-utterance semantic retrieval, allowing the model to link names, numbers, and decisions established early in an hour-long meeting to concluding dialogue.
3. **Sparse Mixture-of-Experts (MoE) Feed-Forward Networks** : Instead of dense MLP blocks, the model routes tokens through a sparse MoE layer with top-2 expert activation. While the total checkpoint parameter count is approximately**34 billion parameters** , each token activates only**4.3 billion parameters** during execution.

This hybrid Mamba-Transformer MoE architecture delivers the speed and memory efficiency of recurrent state space models alongside the contextual expressiveness of full attention Transformers.

### Decoupled CTC Word Timestamps

In previous speech LLMs (such as Whisper), word timestamps were predicted directly as special tokens inside the autoregressive text vocabulary (such as `<|0.00|> Hello <|0.50|> world <|1.20|>`).

This legacy approach degrades LLM efficiency in three critical ways:

- **Token Inflation** : Timestamp tokens consume up to half of the generated context window, drastically inflating inference latency and compute costs.
- **Grammar & Linguistic Degradation** : Forcing the language model to interleave temporal coordinates with syntax impairs natural language coherence and translation quality.
- **Hallucinated Alignment** : When background noise or pauses occur, autoregressive models frequently drift and predict timestamps that completely desynchronize from actual audio frames.

Nemotron 3.5 Transcribe introduces a decoupled **CTC Alignment Adapter**:

```
flowchart TD
    A["Acoustic Representation: 45,000 Encoder Frames (80 ms Stride)"] --> B["Nemotron 3.5 Lightning LLM: Pure Autoregressive Text Generation"]
    B --> C["Generated Clean Token Sequence: Zero Timestamp Overhead"]
    C --> D["Lightweight CTC Adapter: Frame-to-Token Cross-Attention"]
    D --> E["Viterbi Forced Alignment: Exact Word-Level Start & End Milliseconds"]
    style A fill:#0d2b45,stroke:#00e5ff,stroke-width:2px,color:#ffffff;
    style B fill:#0f382c,stroke:#10b981,stroke-width:2px,color:#ffffff;
    style C fill:#2e2010,stroke:#f59e0b,stroke-width:2px,color:#ffffff;
    style D fill:#2a1435,stroke:#d946ef,stroke-width:2px,color:#ffffff;
    style E fill:#1b1b3a,stroke:#6366f1,stroke-width:2px,color:#ffffff;
```
1. **Autoregressive Generation Without Timestamps** : The LLM decoder generates pure, fluent text according to the userâs formatting prompt, producing zero timestamp tokens.
2. **Post-Decoding CTC Cross-Alignment** : After text generation completes, a lightweight Connectionist Temporal Classification (CTC) adapter maps the generated tokens back against the acoustic encoderâs frame representations.
3. **Exact Temporal Resolution** : A fast dynamic programming pass (Viterbi alignment) computes exact millisecond-accurate start and end timestamps for every generated word without perturbing the autoregressive sequence.

### Promptable Steering in Action

Because Nemotron 3.5 Transcribe treats transcription as an instruction-following task, developers steer the output format at inference time using simple system instructions:

#### 1. Verbatim vs. Written-Form Normalization

- **Prompt** :`Transcribe the audio verbatim.`  - *Output* :`the oktoberfest starts in september uh meet me at teresa's visa at six thirty`
- **Prompt** :`Transcribe the audio in written form with proper punctuation, capitalization, and numbers.`  - *Output* :`The Oktoberfest starts in September. Meet me at Teresa's Visa at 6:30.`

#### 2. Dynamic Contextual Biasing (Hotword Injection)

ASR models often hallucinate uncommon proper nouns or medical and technical acronyms. Nemotron 3.5 Transcribe accepts an explicit contextual list of expected entities in the prompt:

- **Prompt** :`Transcribe the audio. Context: [Theresienwiese, Marienplatz, U-Bahn]`  - *Output* :`The Oktoberfest starts in September. Meet me at Theresienwiese at 6:30.`*(Correcting the acoustic misinterpretation âTeresaâs Visaâ into the proper Munich location âTheresienwieseâ without fine-tuning).*

#### 3. Speaker Attribution & Rich Audio Events

- **Prompt** :`Transcribe the audio with speaker labels and acoustic events.`  - *Output* :`<spk:0> The Oktoberfest starts in September. <spk:1> <ev:laughter> Meet me at Theresienwiese at 6:30.`

#### 4. Multilingual Speech Translation

- **Prompt** :`Translate the spoken audio into German.`  - *Output* :`Das Oktoberfest beginnt im September. Treffen wir uns um 18:30 bei der Theresienwiese.`

### Serving Throughput & NVFP4 NIM Scaling

Generating transcripts over long-form audio in enterprise datacenters requires balancing model accuracy against operational hardware cost. To maximize serving efficiency, Nemotron 3.5 Transcribe incorporates **Native Multi-Token Prediction (MTP)** and speculative decoding hooks (supporting DFlash2 drafters).

When deployed inside an **NVIDIA NIM (Inference Microservice)** container quantized to **NVFP4** on NVIDIA Blackwell GPUs (such as the RTX PRO 6000 or B200), the model achieves extraordinary throughput:

- **2,238x Real-Time Factor (RTF)** : In high-concurrency batch serving, a single GPU server processes 2,238 seconds of audio every second. An entire one-hour board meeting is transcribed, diarized, formatted, and timestamped in just**1.6 seconds** .
- **Memory Footprint** : In native BF16, the model requires ~68 GB of VRAM. In NVFP4 precision, the entire model fits comfortably into ~18 GB of memory, enabling local deployment on developer workstations and enterprise edge servers without degrading Word Error Rates.

*Figure 6: Inference throughput across batch sizes on an NVIDIA RTX PRO 6000 Blackwell GPU, comparing BF16, FP8, and NVFP4 with DFlash2 speculative decoding reaching up to 2,238x real-time speed. Source: NVIDIA Research.*

### Interactive Architecture & Frame Alignment Simulation

To understand how the Parallel Expert Encoder injects speaker embeddings and how the decoupled CTC module derives word-level timestamps, explore the standalone, zero-dependency Python simulation below:

## **Click to expand runnable code script**

```
#!/usr/bin/env python3
"""
NVIDIA Nemotron 3.5 Transcribe: Parallel Expert Encoder & CTC Alignment Simulator.
This simulation models:
1. Audio framing (80 ms stride) across 16 kHz acoustic features.
2. Parallel Expert Encoder: Transformer ASR representations & Sortformer speaker slots.
3. Elementwise speaker embedding injection into acoustic frames.
4. Prompt-conditioned decoding across verbatim, written, and contextual modes.
5. Post-decoding decoupled CTC alignment computing word-level timestamps.
"""
import math
import random
from typing import List, Dict, Tuple
# Configuration Constants
AUDIO_DURATION_SEC = 6.0
FRAME_STRIDE_MS = 80
NUM_FRAMES = int((AUDIO_DURATION_SEC * 1000) / FRAME_STRIDE_MS)
NUM_SPEAKERS = 8
EMBEDDING_DIM = 64
def generate_sortformer_probabilities(num_frames: int) -> List[List[float]]:
    """Simulate frozen Sortformer 8-speaker activity probabilities over time."""
    probabilities = []
    for t in range(num_frames):
        time_sec = (t * FRAME_STRIDE_MS) / 1000.0
        # Speaker 0 speaks from 0.5s to 3.0s
        spk0 = 0.95 if 0.5 <= time_sec <= 2.8 else 0.05
        # Speaker 1 speaks from 2.6s to 5.5s (overlapping dialogue between 2.6s and 2.8s)
        spk1 = 0.92 if 2.6 <= time_sec <= 5.2 else 0.04
        
        frame_probs = [spk0, spk1] + [0.01] * (NUM_SPEAKERS - 2)
        probabilities.append(frame_probs)
    return probabilities
def sinusoidal_speaker_projection(speaker_idx: int, dim: int) -> List[float]:
    """Generate fixed sinusoidal projection vector for a speaker slot."""
    vec = []
    for i in range(dim):
        freq = 1.0 / (10000 ** (2 * (i // 2) / dim))
        if i % 2 == 0:
            vec.append(math.sin((speaker_idx + 1) * freq))
        else:
            vec.append(math.cos((speaker_idx + 1) * freq))
    return vec
def parallel_expert_encoder_fusion(num_frames: int, sortformer_probs: List[List[float]], threshold: float = 0.5) -> List[Dict]:
    """Fuse ASR acoustic frames with binarized Sortformer speaker projections."""
    fused_frames = []
    for t in range(num_frames):
        time_sec = (t * FRAME_STRIDE_MS) / 1000.0
        active_speakers = [s for s, p in enumerate(sortformer_probs[t]) if p >= threshold]
        
        # Accumulate speaker projection embeddings
        spk_emb = [0.0] * EMBEDDING_DIM
        for s in active_speakers:
            proj = sinusoidal_speaker_projection(s, EMBEDDING_DIM)
            for i in range(EMBEDDING_DIM):
                spk_emb[i] += proj[i]
                
        fused_frames.append({
            "frame_idx": t,
            "time_sec": round(time_sec, 3),
            "active_speakers": active_speakers,
            "speaker_energy": sum(abs(x) for x in spk_emb)
        })
    return fused_frames
def simulate_promptable_decoding(mode: str, context_list: List[str] = None) -> List[Dict]:
    """Simulate promptable decoding output under different user instructions."""
    raw_tokens = [
        {"word": "the", "spk": 0, "start": 0.6, "end": 0.8},
        {"word": "oktoberfest", "spk": 0, "start": 0.9, "end": 1.6},
        {"word": "starts", "spk": 0, "start": 1.7, "end": 2.1},
        {"word": "in", "spk": 0, "start": 2.2, "end": 2.3},
        {"word": "september", "spk": 0, "start": 2.4, "end": 2.8},
        {"word": "uh", "spk": 1, "start": 2.7, "end": 2.9},
        {"word": "meet", "spk": 1, "start": 3.0, "end": 3.2},
        {"word": "me", "spk": 1, "start": 3.3, "end": 3.4},
        {"word": "at", "spk": 1, "start": 3.5, "end": 3.6},
        {"word": "teresa's visa", "spk": 1, "start": 3.8, "end": 4.6},
        {"word": "at", "spk": 1, "start": 4.7, "end": 4.8},
        {"word": "six thirty", "spk": 1, "start": 4.9, "end": 5.4},
    ]
    if mode == "verbatim":
        text = "the oktoberfest starts in september uh meet me at teresa's visa at six thirty"
        return [{"type": "text", "payload": text}]
    elif mode == "written":
        text = "The Oktoberfest starts in September. Meet me at Teresa's Visa at 6:30."
        return [{"type": "text", "payload": text}]
    elif mode == "contextual":
        # Entity biasing replaces phonetic misinterpretation with exact entity
        if context_list and "Theresienwiese" in context_list:
            text = "The Oktoberfest starts in September. Meet me at Theresienwiese at 6:30."
        else:
            text = "The Oktoberfest starts in September. Meet me at Teresa's Visa at 6:30."
        return [{"type": "text", "payload": text}]
    elif mode == "speaker_attributed":
        text = "<spk:0> The Oktoberfest starts in September. <spk:1> Meet me at Theresienwiese at 6:30."
        return [{"type": "text", "payload": text}]
    elif mode == "word_timestamps":
        # Decoupled CTC alignment output
        return raw_tokens
    return []
def run_simulation():
    print("=" * 80)
    print("NVIDIA NEMOTRON 3.5 TRANSCRIBE: PARALLEL EXPERT ENCODER & CTC SIMULATION")
    print("=" * 80)
    
    # Step 1: Generate Sortformer probabilities
    sortformer_probs = generate_sortformer_probabilities(NUM_FRAMES)
    print(f"[*] Processed {AUDIO_DURATION_SEC:.1f}s audio into {NUM_FRAMES} frames ({FRAME_STRIDE_MS} ms stride).")
    
    # Step 2: Parallel Expert Fusion
    fused_frames = parallel_expert_encoder_fusion(NUM_FRAMES, sortformer_probs)
    overlapping_frames = [f for f in fused_frames if len(f["active_speakers"]) > 1]
    print(f"[*] Parallel Expert Fusion: Detected {len(overlapping_frames)} overlapping dialogue frames.")
    for f in overlapping_frames[:3]:
        print(f"    - Frame {f['frame_idx']} ({f['time_sec']}s): Active Speakers {f['active_speakers']}")
        
    print("\n" + "-" * 80)
    print("PROMPTABLE STEERING OUTPUTS ACROSS INFERENCE MODES")
    print("-" * 80)
    
    modes = [
        ("Verbatim Transcription", "verbatim", None),
        ("Written-Form Normalization", "written", None),
        ("Contextual Entity Biasing", "contextual", ["Theresienwiese", "Marienplatz"]),
        ("Speaker-Attributed Diarization", "speaker_attributed", None),
    ]
    
    for title, mode, ctx in modes:
        res = simulate_promptable_decoding(mode, ctx)[0]["payload"]
        print(f"\n[Mode: {title}]")
        print(f"Output: \"{res}\"")
    print("\n" + "-" * 80)
    print("DECOUPLED CTC WORD-LEVEL TIMESTAMPS (ZERO LLM TOKEN OVERHEAD)")
    print("-" * 80)
    print(f"{'Word / Token':<20} | {'Speaker':<10} | {'Start (s)':<12} | {'End (s)':<12}")
    print("-" * 60)
    
    tokens = simulate_promptable_decoding("word_timestamps")
    for t in tokens:
        word = "Theresienwiese" if t["word"] == "teresa's visa" else t["word"]
        print(f"{word:<20} | <spk:{t['spk']}>    | {t['start']:<12.2f} | {t['end']:<12.2f}")
    print("=" * 80)
    print("Simulation complete: Unified speech LLM pass executed successfully.")
if __name__ == "__main__":
    run_simulation()
```
### References & Source Documentation

- **Primary Model Card & Weights** :`nvidia/NVIDIA-Nemotron-3.5-Transcribe-30B-A4B` â Official Hugging Face repository hosting BF16, FP8, and NVFP4 checkpoints under the OpenMDW-1.1 license.
- **Official Research Hub** : NVIDIA Research Nemotron Labs â Architectural overviews, multimodal demonstrations, and interactive model walkthroughs.
- **Technical Report (PDF)** : NVIDIA Nemotron 3.5 Transcribe Technical Report â Architectural specifications, Sortformer injection mechanics, CTC adapter alignment, and ablation studies.
- **Curation Dataset & Framework** :
  - Hugging Face Blog: Granary-v2 and Nemotron Transcribe â Curation pipeline architecture and 48-language benchmark results.
  - Granary-v2 Dataset on Hugging Face (`nvidia/Granary-v2` ) â 843,600 audio hours of instruction-aligned speech pairs.
  - NVIDIA NeMo Framework & NeMo Curator â Open-source tools for training, fine-tuning, and scalable audio data preparation.
- **Industry & Benchmark Coverage** :
  - TechCrunch: Voice AI in vogue: Nvidia releases new open transcription model â Analysis of commercial voice AI trends, private VPC enterprise requirements, and competitive positioning.
  - Artificial Analysis: Speech Recognition Benchmarks & Pareto Frontier â Independent non-streaming evaluations of word error rate vs active parameter scaling.
  - VoiceArena: Diarization-Bench v1 Overlapping Speech Leaderboard â Concatenated minimum-permutation word error rate (cpWER) rankings.

### Whatâs Next

The release of NVIDIA Nemotron 3.5 Transcribe marks a defining paradigm shift in speech artificial intelligence: transitioning from brittle, latency-heavy multi-model cascades to unified, promptable foundation models. By combining specialized acoustic-speaker encoders with hybrid Mamba-2/Transformer MoE reasoning and decoupled CTC alignment, speech understanding achieves both human-like contextual nuance and blazing enterprise serving speeds.

*In our upcoming post, we explore Google DeepMindâs newly unveiled frontier model: **Gemini 4 Carbon: Googleâs Frontier Reasoning Model for Autonomous Cybersecurity and Long-Horizon Software Engineering**.*
