---
id: collect-261121-veille-ia/xiaomi-mimo/wiki-xiaomi-mimo-47c112f6
title: "Wiki xiaomi mimo"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: ["2025-04-30"]
keywords: ["xiaomi", "model", "mimo", "billion", "trillion", "mimo-v2", "edit", "april"]
source: ""
source_anchor: ""
source_lines: [1, 78]
section: "Xiaomi_MiMo"
---
Xiaomi MiMo
| Xiaomi MiMo |  | 
|---|---|
| Developer | Xiaomi | 
| Release | 30 April 2025 | 
| Stable release | MiMo-V2.6-Pro / 22 September 2026 MiMo-V2.6-Flash / 22 September 2026 | 
| Platform | Cloud computing platforms | 
| Available in | English and other languages | 
| Type |  | 
| License | Various; see § List of models | 
| Website | mimo mimo | 
Xiaomi MiMo is a family of large language models (LLMs) developed by Xiaomi.[1][2] It was initially released in April 2025 with the MiMo-7B model.[3] Currently, MiMo is available for developers through API service. It is used as the key AI model in Xiaomi's "Human x Car x Home" ecosystem.[4]
Development
[edit]
Xiaomi developed MiMo as a reasoning-focused language model. Its development team was led by Luo Fuli, who had previously worked at DeepSeek before joining Xiaomi in late 2025.[5][6] The model was trained using multi-token prediction and reinforcement learning, with a particular emphasis on mathematical reasoning and code generation tasks.[7] In March 2026, Xiaomi CEO Lei Jun announced that the company planned to invest at least US$8.7 billion in artificial intelligence over the following three years.[8]
Models
[edit]
List of models
[edit]
| List of MiMo models |  |  |  |  |  | 
|---|---|---|---|---|---|
| Model | Release date | Parameters | License | Knowledge cutoff | Ref. | 
|---|---|---|---|---|---|
| MiMo-7B | 30 April 2025 | 7 billion | MIT | Unknown |  | 
| MiMo-V2-Flash | 17 December 2025 | 309 billion | MIT | December 2024 |  | 
| MiMo-V2-Pro | 18 March 2026 | 1 trillion | Proprietary | May 2025 |  | 
| MiMo-V2-Omni | 18 March 2026 | Unknown | Proprietary | May 2025 |  | 
| MiMo-V2-TTS | 18 March 2026 | Unknown | Proprietary | N/a |  | 
| MiMo-V2.5 | 22 April 2026 | 310 billion | MIT | May 2025 |  | 
| MiMo-V2.5-Pro | 22 April 2026 | 1.02 trillion | MIT | May 2025 |  | 
| MiMo-V2.6-Flash | 22 September 2026 | 310B (15 billion active) | MIT |  | [9] | 
| MiMo-V2.6-Pro | 22 September 2026 | 1.02 trillion (42 billion active) | MIT |  | [9] | 
MiMo-7B
[edit]
MiMo-7B is the first model of this LLM. The base model, MiMo-7B-Base, was pre-trained on approximately 25 trillion tokens using web pages, academic papers, books, and synthetic reasoning data.[7] MiMo-7B-RL underwent supervised fine-tuning and reinforcement learning on 130,000 mathematics and code problems.[7]
MiMo-7B-RL-0530 was released in May 2025. It scaled the fine-tuning dataset from 500,000 to 6 million instances and extended the RL window from 32,000 to 48,000 tokens and improved AIME 2024 scores from 68.2 to 80.1.[7]
MiMo-VL-7B was a vision-language model combining a Vision Transformer encoder with the MiMo-7B backbone. It was trained in four stages consuming 2.4 trillion tokens.[10] Its reinforcement learning variant used Mixed On-Policy Reinforcement Learning (MORL) which integrated reward signals across perception, grounding, and reasoning.[10] Xiaomi also released MiMo-Audio-7B, an audio-language model for voice conversion, style transfer, and speech editing.[11]
MiMo-V2-Flash
[edit]
MiMo-V2-Flash was launched in December 2025.[12] It is an open-sourced Mixture-of-experts model with 309 billion total parameters and 15 billion active parameters. It was trained on 27 trillion tokens using FP8 mixed precision.[13][14] It used hybrid attention interleaving Sliding Window and Global Attention at a 5:1 ratio.[13]
MiMo-V2-Pro
[edit]
Xiaomi publicly introduced MiMo-V2-Pro on 18 March 2026. It has over 1 trillion total parameters, 42 billion active, and a 1-million-token context window.[15] Before the official release, the model had appeared anonymously on OpenRouter under the codename "Hunter Alpha," where it drew substantial usage and topped daily charts for several days, according to Xiaomi and Reuters.[16] During its listing on OpenRouter, the model reportedly processed over one trillion tokens in total usage. Xiaomi later said Hunter Alpha was an early internal test build of MiMo-V2-Pro, and Reuters reported that the model had been mistaken by some users for a possible DeepSeek system before Xiaomi confirmed its origin.[16]
The model was released as a proprietary API product, and Luo Fuli stated that Xiaomi intended to open-source a variant at an unspecified future date. Xiaomi has partnered with several API web platforms to launch the model. All these websites initially offered a free trial of this model for a week, but due to the overwhelming response, Xiaomi later extended the free trial period of the model until 2 April 2026.[17]
MiMo-V2-Omni
[edit]
Alongside MiMo-V2-Pro, Xiaomi launched MiMo-V2-Omni on 18 March 2026. It handles image, video, audio, and text inputs. Before the official release, it was codenamed "Healer Alpha" in OpenRouter.[15]
MiMo-V2-TTS
[edit]
On the same date as the release of MiMo-V2-Pro and MiMo-V2-Omni, a Text-to-Speech model named MiMo-V2-TTS was released also. It is a speech synthesis model. It was trained on audio data, which makes it capable of emotional transitions, mid-sentence tone shifts, singing, and synthesis of regional dialects like Sichuan, Cantonese, Henan, and Taiwanese.[15]
Licensing
[edit]
Xiaomi has used different licensing approaches for different models in the MiMo family. The MiMo-7B series and MiMo-V2-Flash were released as open-weight models. MiMo-V2-Flash was published under the MIT license with model weights and inference code available on Hugging Face.[18]
MiMo-V2-Pro and MiMo-V2-Omni were released as proprietary models. It was accessible through Xiaomi's API platform and third-party API providers. Luo Fuli stated that Xiaomi intended to open-source a variant of MiMo-V2-Pro. Although, she did not specify any timeline.[16] MiMo-V2-TTS was released as a proprietary model with no publicly available weights.
See also
[edit]
References
[edit]
- ↑ "Xiaomi debuts MiMo, an open-source reasoning AI model". Tech in Asia. 30 April 2025.
- ↑ "Xiaomi unveils MiMo open-source AI model for reasoning". FoneArena. 30 April 2025.
- ↑ Schreiner, Maximilian (30 April 2025). "Xiaomi introduces MiMo-7B, a compact model for math and coding tasks". The Decoder.
- ↑ "MWC 2026: Xiaomi introduces a new wave of "Human × Car × Home" driven by AI". EntrepreNerd. 2 March 2026.
- ↑ "Xiaomi strengthens AI capabilities with former DeepSeek researcher joining MiMo team". Digitimes. 16 November 2025. Retrieved 24 March 2026.
- ↑ "Ex-DeepSeek Core Developer Luo Fuli Joins Xiaomi to Lead MiMo Team on Spatial Intelligence". Pandaily. 12 November 2025. Retrieved 24 March 2026.
- Jump up to: 1 2 3 4 Xiaomi (12 May 2025). "MiMo: Unlocking the Reasoning Potential of Language Model — From Pretraining to Posttraining". arXiv:2505.07608 [cs.CL].
- ↑ "Xiaomi to invest at least $8.7 billion in AI over next three years, CEO says". Reuters. 19 March 2026.
- Jump up to: 1 2 https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Pro-RL/blob/main/MiMo_V2_6_technical_report.pdf
- Jump up to: 1 2 Xiaomi (4 June 2025). "MiMo-VL Technical Report". arXiv:2506.03569 [cs.CV].
- ↑ Xiaomi LLM-Core Team (29 December 2025). "MiMo-Audio: Audio Language Models are Few-Shot Learners". arXiv:2512.23808 [cs.SD].
- ↑ "Xiaomi throws its hat into the open-source AI race". The Business Standard. 19 December 2025.
- Jump up to: 1 2 Xiaomi LLM-Core Team (6 January 2026). "MiMo-V2-Flash Technical Report". arXiv:2601.02780 [cs.CL].
- ↑ Jiang, Ben (17 December 2025). "Xiaomi launches open-source model to compete against DeepSeek, Moonshot, OpenAI". South China Morning Post. Retrieved 24 March 2026.
- Jump up to: 1 2 3 "Xiaomi Launches Powerful AI Model MiMo-V2 Pro With 1 Trillion Parametres, 1 Million Token Context Window". NDTV Profit. 19 March 2026.
- Jump up to: 1 2 3 "Mystery AI model revealed to be Xiaomi's following suspicions it was DeepSeek's". Reuters. 18 March 2026.
- ↑ "Xiaomi Extends MiMo-V2 Agent Framework Free Trial". XiaomiTime. 26 March 2026.
- ↑ "MiMo-V2-Flash: Xiaomi's High-Performance Open-Source AI Model in 2025". Tech-Now. 23 December 2025.
External links
[edit]
