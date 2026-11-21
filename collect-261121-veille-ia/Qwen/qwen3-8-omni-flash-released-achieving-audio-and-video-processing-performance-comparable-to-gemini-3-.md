---
id: collect-261121-veille-ia/qwen/qwen3-8-omni-flash-released-achieving-audio-and-video-processing-performance-comparable-to-gemini-3-
title: "'Qwen3.8-Omni-Flash' released, achieving audio and video processing performance comparable to Gemini 3.8 Flash."
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["video", "audio", "qwen3", "omni-flash", "performance", "gemini", "flash", "benchmark"]
source: https://gigazine.net/gsc_news/en/20260918-qwen-3-8-omni-flash/
source_anchor: ""
source_lines: [1, 43]
section: "Qwen"
---
# 'Qwen3.8-Omni-Flash' released, achieving audio and video processing performance comparable to Gemini 3.8 Flash.


 Alibaba, a leading Chinese technology company, has added ' **Qwen3.8-Omni-Flash** ,' a next-generation native omnimodal model, to its Qwen series of AI models. It delivers superior results across a wide range of workflows, including video editing, music video production, film production, audiovisual summarization, and real-time conversation.

 **Qwen3.8-Omni-Flash: Omni Senses. Agentic Delivery.**

 

Qwen3.8-Omni-Flash is an AI model that supports a context window of 1 million tokens and significantly improves omnimodal functionality while maintaining text performance equivalent to text-only models of the same size.

Across 29 benchmarks, the average score is more than 25% higher than Qwen3.5-Omni-Plus, while the API cost per hour for voice input is over 98% lower, and the cost per hour for voice and video input is over 93% lower.

Furthermore, in benchmarks for voice and video agent coding and long-term tasks, it achieved high scores of '71.0' in WildClawBench-MM, an increase of 36.5 points from Qwen3.5-Omni-Plus, and '69.6' in UniClawBench.

Core functions such as understanding long audio and audio/video, audio/video inference, audio/video captioning, and multi-speaker recognition have also been significantly improved. For example, it achieved a score of '82.7' in LongAudioSpan, a benchmark for evaluating the ability to understand long audio; '63.4' in OmniVideoBench, a benchmark for evaluating the ability to perform audio/video collaborative inference in multimodal models; '28.2' in OmniCap-IF, a benchmark for evaluating the ability to follow instructions in video caption generation; and '89.7' in AliMeeting, a benchmark for transcribing multi-person, multi-channel conference audio in Chinese. It surpassed Gemini 3.8 Flash in multiple benchmark tests.


 Alibaba explained, 'Qwen3.8-Omni-Flash scales the data, context, and agent environment to achieve audio and video performance close to that of Gemini 3.8 Flash, and overall audio performance that surpasses Gemini 3.8 Flash. These advancements mean that audio and video are evolving from perceptual inputs to core media for agents to understand and reason about their environment and perform tasks.'

 The supported languages for speech recognition and speech generation are as follows:

 **Speech recognition:** 74 languages (Afrikaans, Arabic, Asturian, Azerbaijani, Basque, Belarusian, Bengali, Bosnian, Bulgarian, Cantonese, Catalan, Cebuano, Chinese, Croatian, Czech, Danish, Dutch, English, Esperanto, Estonian, Filipino, Finnish, French, Galician, Georgian, German, Greek, Hebrew, Hindi, Hungarian, Icelandic, Indonesian, Interlingua, Italian, Japanese, Javanese, Kanna) (Da, Kazakh, Korean, Kyrgyz, Lingala, Latvian, Lithuanian, Macedonian, Malay, Malayalam, Maltese, Maori, Marathi, Mongolian, Norwegian Bokmål, Norwegian Nynorsk, Odia, Persian, Polish, Portuguese, Punjabi, Romanian, Russian, Serbian, Slovak, Slovenian, Spanish, Swahili, Swedish, Tajik, Tamil, Telugu, Thai, Turkish, Ukrainian, Urdu, Uyghur, Vietnamese)

 **Speech generation:** 29 languages (Chinese, English, German, Italian, Portuguese, Spanish, Japanese, Korean, French, Russian, Thai, Indonesian, Arabic, Vietnamese, Turkish, Finnish, Polish, Hindi, Dutch, Czech, Urdu, Tagalog, Swedish, Danish, Hebrew, Icelandic, Malay, Norwegian, Persian)

 Qwen3.8-Omni-Flash is available for use with Qianwen AI.

 **Enterprise AI Large Model Clothes Platform - Thousand AI Platform**

 

Audio and video are important media for integrating agents into real-world productivity scenarios. However, Alibaba explains that they also create new system-level challenges. These challenges include the fact that 'long audio and video files are costly to store, transmit, and process over multiple inference processes.' Alibaba points out that existing agent harness frameworks do not natively support these modalities, and workflows that bridge omnimodal understanding with end-to-end task execution are still in their early stages.

To address these challenges, the model, harness tools, and runtime environment need to evolve together. Therefore, Alibaba has further extended

**Qwen-MM-Plugins**, adding on-demand perception, tool usage, and workflow execution capabilities for long audio and video. They have also open-sourced

**Qwen-Live Harness**, a comprehensive open-source harness designed based on the Qwen3.8-Omni-Flash-Realtime API. However, at the time of writing, the Qwen-Live Harness GitHub page is not accessible and returns a 404 error.



**Related Posts:**
