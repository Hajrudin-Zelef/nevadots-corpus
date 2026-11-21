---
id: collect-261121-veille-ia/robotics-kyutai/kyutai-linkedin
title: "Kyutai | LinkedIn"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: []
keywords: ["aura", "video", "frames", "kyutai", "models", "world", "diffusion", "model"]
source: https://www.linkedin.com/company/kyutai-labs/
source_anchor: ""
source_lines: [1, 59]
section: "Robotics_Kyutai"
---
Meet AURA: a talking avatar you can hold a real conversation with, generated live on a single GPU. In this video, one of our researchers chats with an AURA avatar connected to Unmute, our cascaded voice assistant. The face isn’t pre-recorded: every frame is generated on the fly from the assistant’s voice, as it speaks, in real-time. AURA is a streaming causal audio-driven video diffusion model. Bidirectional models need several seconds of future audio before they can render anything. AURA generates video chunk by chunk in a causal manner with a rolling KV cache and needs only the audio of the current chunk to start. AURA 1.3B streams 576p video in real time on one GPU (20.7 fps for 16 fps playback), 23× faster than its bidirectional teacher. The live demo runs it at 480p. AURA 5B streams 720p video at 9.7 fps, matching its teacher’s quality while running 17× faster Both are distilled in two stages: trajectory matching, then Distribution Matching Distillation We’re releasing the weights of both models, the code, and a dataset of 30K synthetic talking-head videos with their precomputed teacher trajectories. Work by Nathaniel Cohen, Nicolas Dufour, Amélie Royer, Alasdair Newson (ISIR, Sorbonne Université) and Patrick Pérez. AURA will also be presented at BMVC 2026 next week by Nathaniel. Try the demo: https://aura.kyutai.org Paper: https://lnkd.in/dPy635Xa Project page: https://kyutai.org/aura Code: https://lnkd.in/dmCNeX6i Weights & dataset: https://lnkd.in/dEwXeVtp

## À propos

Build and democratize Artificial General Intelligence through open science.

- Secteur
- Technologie, information et Internet
- Taille de l’entreprise
- 11-50 employés
- Siège social
- Paris, Île-de-France
- Type
- Non lucratif
- Fondée en
- 2023

## Employés chez Kyutai

## Lieux

## Nouvelles

- 
              
        
    Two papers from Kyutai will be presented at EMNLP 2026 in Budapest this week. Feel free to drop by Poster Session F on Tuesday to hear more about each work and talk with our researchers Franck Signe Talla, Hippolyte Pilchen and Atsumoto Ohashi !
- 
              
        
    Many world models borrow their video autoencoder from text-to-video generators, and with it, a noticeable lag on every action. We built CADRE, open CAusal Diffusion REpresentation autoencoders that let real-time world models react fast, on a single GPU Why the lag? To reach high frame rates at low cost, video autoencoders pack several frames into one latent. But a latent can only be generated once all its actions are in. Pack too many frames and the lag becomes noticeable. We built 7 CADRE models differing only in spatio-temporal compression, to see how it shapes interactive world models. The encoder comes from MIRA, our Rocket League model: frozen DINOv3 + trained compression layer. The decoder is new: causal diffusion, self-distilled to 1 step. We trained the same world model on each, for robot and human manipulation, driving and indoor walks. Compared with the autoencoders of Wan 2.2, Cosmos, LTX-2, DC-AE and MIRA, CADRE forms most of the latency-quality frontier. Finding 1: strongly compressed latents benefit from a diffusion decoder, and it can be fast. A regression decoder like MIRA's blurs together the many videos a latent could be; diffusion picks one sharp video. Self-distilled into a flow map in one stage, ours needs one step. Finding 2: time is best compressed in the autoencoder, not in the world model. Grouping frames inside the world model lowers both image and motion quality, and the gap grows with the number of frames grouped. At 8 frames, actions are also followed less faithfully. Finding 3: manipulation and navigation call for opposite compressions. In manipulation the camera barely moves, so packing consecutive frames in time costs little. In navigation the whole view changes: keep frames apart and compress space, which also keeps lag low. Finding 4: in manipulation, coarser tokens barely change how frames look, but they hurt motion and, for the robot, action following. So scoring rollouts frame by frame, as FID does, hides much of what goes wrong. What do these world models pick up from video and actions alone? Cloth that hangs where it is not held, a towel that folds, other cars that drive on their own, and roads we leave on purpose, where no recorded driver went. As always at Kyutai, it's all open: training and inference code, models, and a demo where you drive the world models from your keyboard Paper: https://lnkd.in/e_Nx9Mrx Blog & demo: https://lnkd.in/ecNnUJWm
- 
              
        
    Kyutai a republié ceci Pocket TTS has been getting updates those last few days. While it was ~100M parameters, we release smaller versions, down to 1.9M: an 18.2M model with the quality of the original, a 6.9M model, and a 1.9M model that runs 103x faster than real time on one CPU core. All with voice cloning. How do you get there? We distill a ~300M teacher, then shrink the student in steps: cut the model, retrain it, cut again. Cutting straight to the final size gives a clearly worse model; going step by step does not. Every part shrinks, not only the transformer. The sampling head goes from 9.8M to 0.84M parameters, and the audio decoder from 10.3M to 2.6M at about the same quality, or 1.0M for the smallest model. Each model also comes in a version fine-tuned with rewards for naturalness and intelligibility, which raises the audio quality score of all three. Most of these tricks are not specific to speech and should carry over to other transformers. The full recipe, with ablations, is in the blog post: https://lnkd.in/gikMvPmm Weights: https://lnkd.in/gFStuypx Code: https://lnkd.in/gXuRBkdp You can also try the models in your browser on the blog post.
- 
              
        
    Pocket TTS has been getting updates those last few days. While it was ~100M parameters, we release smaller versions, down to 1.9M: an 18.2M model with the quality of the original, a 6.9M model, and a 1.9M model that runs 103x faster than real time on one CPU core. All with voice cloning. How do you get there? We distill a ~300M teacher, then shrink the student in steps: cut the model, retrain it, cut again. Cutting straight to the final size gives a clearly worse model; going step by step does not. Every part shrinks, not only the transformer. The sampling head goes from 9.8M to 0.84M parameters, and the audio decoder from 10.3M to 2.6M at about the same quality, or 1.0M for the smallest model. Each model also comes in a version fine-tuned with rewards for naturalness and intelligibility, which raises the audio quality score of all three. Most of these tricks are not specific to speech and should carry over to other transformers. The full recipe, with ablations, is in the blog post: https://lnkd.in/gikMvPmm Weights: https://lnkd.in/gFStuypx Code: https://lnkd.in/gXuRBkdp You can also try the models in your browser on the blog post.
- 
              
        
    Today we are releasing REKI, a 2.4B vision-language-action model pretrained on public data only. With a simple design, REKI rivals larger models trained on in-house data with complex multi-stage pipelines. On LIBERO-Plus, RoboTwin Clean2Rand and RoboCasa365, it is outperformed only by models about twice its size. REKI also has a memory of its recent past. A few register tokens in the action expert summarize the output of the VLM at each frame and persist in the expert's cache, so the memory comes at negligible cost. It improves success on every benchmark and on a real UR5 arm, most of all on longer, multi-step tasks. All model weights are available today, and our optimized training and inference code, from pretraining to deployment, will be fully open source. We hope REKI gives the community an open and efficient starting point for VLA research. Joint work with Juliette MARRIE, Paul Pacaud, Adrien RAMANANA RAHARY, Shizhe Chen, and Moritz Böhle, at Kyutai, Inria and École des Ponts. Paper: https://lnkd.in/dyx5qBtx Project page: https://kyutai.org/reki Code: https://lnkd.in/dwjmp5a4 Models: https://lnkd.in/d2J3dbxH
- 
              
        
    Kyutai a republié ceci I'll be in SF for COLM. First, Timothee Weisselberger will present his work "Voice of Reason" on using RL to improve the math abilities of speech-native models. Come to our poster, session 6, location #20 on Thursday at 4:40 PM PDT. Then, I'll give a talk on "Discrete and continuous tokenization for audio-text modeling" at the Second Tokenization Workshop on Friday morning, 1:40pm (edited, used to be in the morning but was moved!), thanks Valentin Hofmann for the invitation. Please reach out during the week if you want to learn more about Kyutai or Gradium!
- 
              
        
    We release a blogpost today showing that Pocket TTS, our 100M parameter on-device TTS, can be trained with drifting, the new one-step generative objective developed by Kaiming He and his team at MIT earlier this year. Less than 1% WER, high quality and voice cloning. As far as we know, it's the first speech model, and the first autoregressive model, trained this way. Unlike previous methods, this requires no fancy Jacobian-vector product at training time. Most of the original drifting paper's tricks turned out to be unnecessary. Our codec's latents already make a good feature space, and autoregressive conditioning narrows each frame's distribution so much that one positive per frame is enough. The key idea we had to come up with to make it work: a learned kernel temperature. Links in comments!
- 
              
        
    We're releasing Voice of Reason, a speech-native model that does math out loud. Give it a spoken problem — no transcription, no text LLM in the loop — and it reasons and answers in speech. GSM8K goes from 27.3% for GLM-4-Voice to 77.1%. Math is a good benchmark for models’ intelligence where speech models typically lag behind text ones. They have to answer immediately, and out loud, while cascading around a text model (transcribe, think, speak) adds too much latency. How do we bring them up to par ? To do this, we start from GLM-4-Voice, finetune it on synthesized spoken question answering, then train it with RL against a verifiable reward. With no extra reasoning tokens at all — the model answering straight out — this reaches 70.3%, already past speech models that needed explicit reasoning traces. Then we let it think without speaking, using the streaming reasoning scheme of STITCH (Chiang et al., arxiv.org/abs/2507.15375): it reasons in silent chunks while its previous sentence is still playing. 77.1% on GSM8K, and the thinking costs no added latency. We will present this work at COLM 2026, come and join us if you are around. Timothee Weisselberger, Edouard Grave, Alexandre Défossez
- 
              
        
    Kyutai a republié ceci Je représente Kyutai au Salon BiG DATA & Ai PARIS ce mercredi 16 septembre pour donner une présentation intitulée « Ancrer l’IA dans le monde physique grâce aux World Models » ! 🎤 Au-delà des modèles de langage, comment donner aux machines une forme d’« imagination » leur permettant de mieux comprendre, anticiper et agir dans le monde réel ? C’est tout l’enjeu des World Models 🌍, ou modèles du monde : des modèles capables de construire une représentation de leur environnement et d’en simuler l’évolution. Une approche particulièrement prometteuse pour les applications de l’IA qui doivent interagir avec le monde physique.
