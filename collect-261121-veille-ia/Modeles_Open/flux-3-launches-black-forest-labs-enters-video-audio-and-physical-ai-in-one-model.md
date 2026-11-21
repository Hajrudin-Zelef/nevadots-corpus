---
id: collect-261121-veille-ia/modeles-open/flux-3-launches-black-forest-labs-enters-video-audio-and-physical-ai-in-one-model
title: "FLUX 3 Launches: Black Forest Labs Enters Video, Audio, and Physical AI in One Model"
domain: modeles-open
role: reference
task: article
actors: []
dates: ["2026-07-23"]
keywords: ["video", "flux", "model", "audio", "black", "forest", "labs", "clips"]
source: http://www.techtimes.com/articles/321552/20260725/flux-3-launches-black-forest-labs-enters-video-audio-physical-ai-one-model.htm
source_anchor: ""
source_lines: [1, 89]
section: "Modeles_Open"
---
Black Forest Labs (BFL) launched FLUX 3 on July 23, 2026, and it is not an image model with video bolted on. The Freiburg, Germany-based company — whose founding team built the latent diffusion architecture behind Stable Diffusion — trained a single set of weights simultaneously on images, video, and audio, then extended that same architecture to predict robot actions. The result is the first major generative model where 20-second video clips with native synchronized audio, advanced image editing, and factory-floor robotic control all run through one shared backbone.

The launch drew immediate attention not for what it promises but for what is already verifiable: FLUX-mimic, a robotics model built on the FLUX 3 architecture, is currently running on production lines at Audi.

### One Architecture, Not Three Tools

Every major competitor in the video-generation space — Runway, Luma, Kling — assembles audio and video through separate pipelines or attaches audio as a post-generation pass. FLUX 3 does not. BFL trained the model jointly across image, video, and audio from the start. Audio and video are generated together in a single pass, which the company says produces better causal alignment: the sound of an object hitting a surface matches the impact frame, dialogue tracks lip movement, and ambient noise corresponds to what the scene depicts.

The underlying method is called Self-Flow, published by BFL in March 2026. Standard flow-matching architectures — the generative method behind most modern diffusion models, including BFL's own prior FLUX releases — produce strong outputs but tend to generate weak feature representations for downstream tasks. Self-Flow addresses this by unifying generation quality and representation quality in a single framework. BFL reports that this simultaneously improved video-generation benchmarks and robot-control task success rates in simulation.

FLUX 3 is the scaled-up application of that research. According to BFL, video prediction accounts for more than 95% of the model's total training compute. Audio — despite being the feature most prominently marketed for its novelty — represents less than 0.5% of the tokens in a 720p video generation. Once the model learns to predict video dynamics accurately, BFL says, the causal relationship between physical events and their sounds is learnable at low additional cost.

"You can't cheat reality. A model that only learns images can only generate images. But the world is not made of still frames. It moves, sounds, changes, and responds," said Robin Rombach, Co-Founder and CEO of Black Forest Labs.

### Video Capabilities and Benchmark Numbers

FLUX 3 Video — the only component in broad early access at launch — generates clips up to 20 seconds at 720p resolution with audio created in the same generation pass. Supported workflows include text-to-video, image-to-video, video-to-video editing, keyframe-controlled transitions, multilingual dialogue rendering, typography generation, and agentic chaining of individual clips into longer multi-shot sequences.

In preliminary internal evaluations conducted by BFL on 10-second, 720p text-to-video clips, human reviewers preferred FLUX 3's output over Grok Imagine Video in 69% of comparisons, over Kling v3 Pro in 60%, over Runway Gen-4.5 in 77%, and over Luma Ray 3.2 in 93% of comparisons. Reviewers also gave FLUX 3 a slight preference edge — 52% — over Seedance 2.0 and Gemini Omni Flash.

Those numbers carry a significant asterisk that buyers and developers should not skip. BFL has not published the evaluation methodology, sample size, rater selection criteria, or the specific prompt set used in these comparisons. The clips used in comparisons were 10-second generations, not the full 20-second maximum. Independent benchmarks from researchers or publication-grade review sites have not yet been conducted. BFL acknowledged the model is still in development and expects further improvements during the early-access phase.

Characteristics BFL highlights specifically: strong capture of human facial expressions, accurate association of sounds with physical events (footsteps, impacts, door closings), and multilingual dialogue capabilities.

### FLUX-mimic and the Audi Deployment

The most architecturally significant claim in the FLUX 3 launch is not the video benchmark. It is the argument that video generation and robot action prediction are the same underlying problem — and that BFL has demonstrated this at an automotive production line.

Adding action prediction to FLUX 3's training initially caused a roughly 10% drop in human-rated video quality. After approximately 3,500 additional training steps, the model recovered its full prior video quality while also predicting robot actions. BFL interprets this as evidence that the same world representation that enables realistic video — contact dynamics, object weight, cause-and-effect sequences — also encodes the information a robot needs to act.

FLUX-mimic, the robotics model built on this backbone, was developed in partnership with mimic robotics. The architecture trains a lightweight action decoder on top of intermediate features extracted from FLUX 3's video prediction path rather than building a separate robot-learning model from scratch. BFL reports the action decoder outperforms prior vision-language-action models even when the FLUX 3 backbone is entirely frozen — a result that, if reproducible, would suggest the video-trained backbone already encodes world understanding directly transferable to physical manipulation.

The practical implication is fine-tuning efficiency. With a backbone that already models world physics, BFL says a robot can be taught new manipulation tasks with significantly less demonstration data than prior approaches require. The model can be fine-tuned for a specific manipulation task with as little as 30 minutes of robot data, where prior approaches have required 30 or more hours. The mimic-video prior work reported up to 10x sample efficiency for video-action models over traditional vision-language-action models; FLUX-mimic is described as combining that with additional gains from Self-Flow representation quality.

In mimic's optimized deployment on production hardware, the FLUX-mimic backbone runs from sensory input to world representation in under 80 milliseconds on a single NVIDIA RTX 5090 GPU. The full system — including the action decoder, inter-process communication between sensors, model, and actuators, and real-time chunking to overlap prediction and execution — achieves end-to-end reaction times of approximately 101 milliseconds. That is comparable to human visual reaction time.

Audi confirmed the deployment. "We have seen these robots solve complex soft-body manipulation work that would have been simply impossible with conventional robotics. This can have a major impact in assisting our employees, increasing efficiency, and expanding flexible automation across production and logistics operations," said Christoph Schneider of Audi Production Lab.

The tasks being handled include kitting parts into structured trays, inserting electronic control units into tight-fitting fixtures, assembling components, and manipulating soft materials such as seals and cables — categories that Audi says conventional programmed robotics cannot handle cost-effectively in premium-vehicle production due to the variant diversity of each model.

### What Open Weights Would Mean for the Ecosystem

FLUX 3 Dev — the planned open-weight release of the multimodal backbone — may be the most strategically significant element of the entire announcement, and it is not yet available.

BFL has not disclosed a specific release date, license terms, or parameter count for FLUX 3 Dev. What has been confirmed is the plan to release an open-weight version of the same multimodal backbone that generates video, audio, and images — and that supports action prediction.

The implication is visible in the company's own history. BFL's prior FLUX.1 model family became one of the most widely downloaded open diffusion ecosystems, with models from the founding team accumulated across Stable Diffusion and FLUX reaching approximately half a billion downloads. The open-weight FLUX.1 [dev] release spawned thousands of community fine-tunes, LoRA adapters, and ComfyUI integrations that extended BFL's reach far beyond its own API. If FLUX 3 Dev includes the same open-weight multimodal backbone — as BFL has described — it would be the first such model available for researchers and developers to run locally, fine-tune, and integrate: no audio-video multimodal model in that category currently exists in open weights.

All video-generation competitors — Runway, Luma, Sora, Kling, Seedance — are proprietary. If FLUX 3 Dev ships as described, BFL's strategic position would shift from "strong competitor" to "de facto infrastructure layer for open-source multimodal video."

### What Is Not Yet Shipping

As of launch day, three of the four announced FLUX 3 components are not yet available to the public.

FLUX 3 Video is in gated early access — available via API and private weights to selected partners, requiring an application at bfl.ai/models/flux-3. FLUX 3 Action, including FLUX-mimic, is in early access via selected research and commercial robotics partners. FLUX 3 Image — the component most relevant to the designers, marketers, and illustrators who have driven FLUX adoption — is expected to follow in the coming weeks but had not launched as of July 25, 2026. FLUX 3 Dev, the open-weight release, is planned for later in 2026.

No pricing has been announced for any tier. BFL has stated that full benchmark methodology will be published when broader availability rolls out; that commitment cannot be independently verified.

Early access testing partners include Canva, Burda, Magnific (formerly Freepik), Krea, and Picsart.

### Why Black Forest Labs Can Make This Claim

BFL's credibility in staking architectural claims rests on its founding team's track record. Rombach was lead author on "High-Resolution Image Synthesis with Latent Diffusion Models" (CVPR 2022 Oral), the paper that established latent diffusion as the efficient generative architecture behind Stable Diffusion — the model that made high-quality image generation computationally accessible. The same fundamental insight — that operating in a compressed latent space rather than pixel space dramatically reduces compute while preserving quality — now underlies the video prediction work in FLUX 3.

The company is valued at $3.25 billion following a $300 million Series B in December 2025, with total capital raised exceeding $450 million. Investors include a16z, AMP, Salesforce Ventures, NVIDIA, General Catalyst, Adobe Ventures, Figma Ventures, Canva, and Deutsche Telekom's T.Capital. The 100-person team operates between Freiburg, Germany, and San Francisco.

The robotics sector context matters here. The embodied AI market is projected to grow from $3.8 billion in 2026 to more than $7.24 billion by 2030, with robotics companies raising $55.8 billion in 2026 alone — nearly double the prior annual record. BFL's FLUX-mimic deployment at Audi, backed by that architecture, positions it directly in that investment wave.

"We are only beginning to scratch the surface of versatile, capable, unified visual models," Rombach said at launch. "From interactive image and video editing to simulation, physical AI, and computer use, the frontier is wide open."

The architectural thesis is the bet BFL is asking the market to evaluate: that a model which genuinely understands world dynamics — how objects move, how events sound, how surfaces respond to contact — is not an image model that got expanded. It is a world model, and content creation is one thing you can do with it. Robotics is another.

## Frequently Asked Questions

### How does FLUX 3 Video compare to Runway Gen-4.5 and Luma Ray?

In BFL's own preliminary internal evaluations, human reviewers preferred FLUX 3 over Runway Gen-4.5 in 77% of comparisons and over Luma Ray 3.2 in 93% of comparisons, using 10-second 720p text-to-video clips. These are vendor-reported figures with no disclosed methodology, sample size, or independent verification. They should be treated as directional, not definitive. Independent benchmarks from third parties are not yet available.

### What is Self-Flow, and why does it matter for the robotics claim?

Self-Flow is BFL's method for training a generative model so that its internal representations are well-organized and directly usable for downstream tasks — not just for generating outputs. Standard flow-matching models produce realistic video but keep the internal features that drive generation entangled in ways that are hard for other systems to decode. Self-Flow solves this simultaneously: it improves generation quality (measured by Fréchet distance on video) and representation quality (measured by robot control success rates in simulation). The robotics claim depends on Self-Flow: FLUX-mimic's action decoder reads directly from FLUX 3's intermediate video-prediction features rather than running a separate robot-learning model, and BFL says those features are usable because Self-Flow made them structured and decodable.

### When will FLUX 3 be fully publicly available, and will there be open weights?

As of July 25, 2026, only FLUX 3 Video and FLUX 3 Action are available, via gated early access to selected partners. FLUX 3 Image is expected in the coming weeks. FLUX 3 Dev — the open-weight multimodal backbone — is planned for later in 2026, making it the first publicly available open-weight model that jointly generates video, audio, and images from a single architecture. No pricing has been announced for any tier.

### Does the Audi deployment prove the robotics claims are real?

Audi's Christoph Schneider confirmed that FLUX-mimic is currently running in Audi production facilities, handling tasks including soft-body manipulation that conventional robotics cannot perform cost-effectively. This is a real, named, production deployment — not a lab demonstration. What it does not prove is how broadly the approach generalizes across different tasks, different hardware, or different industrial environments. BFL says the sample efficiency advantage comes from the backbone's world representation, not task-specific engineering, which would theoretically make it transferable. That claim awaits independent validation beyond the Audi case.
