---
id: collect-261121-veille-ia/liquid-lfm/liquid-ai-introduces-lfm3-models-for-enterprise-automation
title: "Liquid AI introduces LFM3 models for enterprise automation"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["liquid", "models", "model", "lfm3", "enterprise", "liquids", "automation", "text"]
source: https://smntcn.com/en/article/liquid-ai-predstavlyaet-modeli-lfm3-dlya-avtomatizatsii-biznes-protsessov-8197
source_anchor: ""
source_lines: [1, 95]
section: "Liquid_LFM"
---
# Liquid AI introduces LFM3 models for enterprise automation

According to VentureBeat, Liquid AI has announced LFM3, a series of compact artificial intelligence models designed to shift more enterprise work onto company-controlled hardware. The company reports that its smallest text model can significantly outperform larger counterparts in certain tasks related to browser automation and software tool usage.

The October 27 release includes six models spanning language, vision, speech, and structured decisions. They are all based on a foundation of 1.7 billion parameters, with specialized versions having around 2 billion parameters. Liquid claims that enterprises can customize each model using a single GPU and deploy them on devices, private servers, or cloud infrastructure.

This pitch is particularly relevant for companies building repeatable workflows: routing requests, extracting information from documents, inspecting images, or operating business applications. A smaller model that reliably handles a defined job could reduce dependence on remote AI services and give organizations more control over where inference occurs.

For companies that prefer a hosted service, Liquid’s console lists the text model at $0.08 per million input tokens and $0.20 per million output tokens, with cached input priced at $0.02. Developers must sign in to the Liquid console to view the LFM3 rate card and obtain credentials for direct API access.

Liquid’s detailed results also show where trade-offs remain. For instance, its text model trails Qwen3.5-9B in several coding, knowledge, and long-context evaluations. The hosted offering lists a smaller context limit than the downloadable text model. This makes LFM3 a candidate for targeted pilots, with the business case depending on workload, deployment costs, and licensing terms.

The flagship LFM3-1.7B text model supports a documented 256K-token context window—the combined input and output budget. Liquid’s hosted console lists a 128,000-token context limit for its API version, a distinction enterprises should account for when choosing between hosted and self-managed deployments.

Liquid claims it trained the model to work within agent frameworks including OpenClaw, Hermes Agent, and Pi, supporting tasks such as inbox organization, scheduling, and web research. The strongest evidence for its enterprise relevance comes from individual benchmarks, rather than overall averages.

In Liquid’s tests, LFM3-1.7B scores 38.42 on WebArena-Verified, compared to 28.94 for Qwen3.5-9B and 3.02 for Liquid’s previous LFM2.5-2.6B. On AutomationBench’s public evaluation, it scores 41.29, versus 15.10 and 0.94, respectively. The following selection includes both wins and weaknesses. These are Liquid’s reported results, not independent VentureBeat testing.

The differences in evaluations argue against treating LFM3 as a universal replacement for larger models. It leads in selected browser and function-calling tests but falls behind in MCP-Atlas and is slightly below Qwen in EnterpriseOps-Gym. Its advertised context capacity should not be mistaken for superior accuracy across long inputs.

Liquid acknowledges the broader limitation in its written response to VentureBeat: “Larger models still win on agentic coding and knowledge, where size matters most.” Its documentation is more explicit: “It is not recommended for agentic coding or complex multi-step reasoning due to its small size.” The company did not directly answer whether every comparison used equivalent tools and reasoning allowances.

Its published announcement names Mercedes-Benz and Shopify as partners in testing and evaluating the models before release but does not provide independent validation of the benchmark results. Teams should test their own workflows, including failed tool calls, retries, and escalation to another model, before projecting production savings.

LFM3’s architectural change aims to make that smaller footprint more useful. The new design combines convolution layers, which efficiently process nearby information, with attention mechanisms that examine either a limited window or a wider context. Liquid calls this a tri-hybrid architecture.

The shared foundation was pretrained on 22 trillion tokens. Subsequent training emphasizes tool use, business workflows, and other agent tasks. Liquid claims the resulting 1.7B text model improves on its previous 2.6B version despite having roughly 35% fewer parameters.

Memory requirements deserve particular care. Liquid describes the text model as fitting into approximately 1GB at four-bit precision. However, that does not establish that an entire application can run within 1GB, especially when processing lengthy inputs.

The company’s latest response clarifies that its separate 1.6GB figure for a 128K-token context refers to the KV cache—the working memory used to retain information during generation. Model storage and other runtime requirements remain additional considerations. Liquid did not provide a complete memory budget at the maximum 256K context length.

For local speed testing, Liquid compares models on an AMD Ryzen AI Max+ 395 system, an Apple M5 Max system, and an iPhone 17 Pro. It reports generation speeds between 1.8 and 2.8 times those of the next-fastest model in each comparison, using llama.cpp, four-bit Q4_0 models, a 32K-token input, and one request at a time.

The published performance tables put concrete numbers behind that claim: test system, input processing, tokens/sec, generation, tokens/sec, reported memory. These measured footprints reinforce why the roughly 1GB model-size claim should not be used as the total deployment memory budget. The figures describe a particular compressed model and workload, not a guarantee for every device or application.

At server scale, Liquid reports capacity equivalent to 3.6 billion generated tokens daily from one Nvidia B300 at 1,024 concurrent requests. That test uses BF16 precision, SGLang, 1,024 input tokens, and 256 output tokens per request, averaged across three runs. It measures aggregate throughput, rather than how quickly a single user completes a complex task.

A separate latency test on one Nvidia B200 reports a 95th-percentile completion time of 150 milliseconds at 500 requests per second, using 128 input tokens and 64 output tokens. In that test, 95% of requests finish within the reported time. With longer requests of 1,024 input tokens and 256 output tokens, the figure rises to 668 milliseconds at 100 requests per second.

Liquid uses its RIPL serving software for its own models and vLLM for competitors in this comparison. The result evaluates the combined model and serving system; it does not isolate the architecture alone or establish the latency customers will experience through the hosted API.

Beyond text agents, the family expands the kinds of applications Liquid can address. Models LFM3 describe capabilities such as language generation and tool use, with a model context of 256K, 128,000 through the listed API. This could be a narrowly scoped internal agent.

The LFM3-VL-2B model provides image and video interpretation with text responses, which may be useful for document processing or inspection. The LFM3-Audio-2B model supports speech and text input, and can provide responses in speech or text in 18 languages, making it suitable for local transcription or voice-operated tools.

The LFM3-Omni-2B model combines visual, audio, and text inputs, and can provide responses in speech and text, making it suitable for creating a hands-free assistant that also interprets images. The d1-2B model provides structured decisions based on text and images, which may be useful for ticket classification or invoice extraction.

Proposed pilots are editorial analysis, not claims of established customer deployments. The vision model adds video input to Liquid’s LFM vision line. Its company-reported average across 62 evaluations is 71.4, compared to 57.6 for LFM2.5-VL-3B and 64.0 for Qwen3.5-2B. The aggregate is an unweighted average of the 62 evaluations. In the document, optical character recognition and chart category, Liquid reports 83.9 for LFM3-VL-2B, compared to 71.2 for its predecessor and 79.1 for Qwen3.5-2B. These results are more directly relevant to document-processing pilots than the overall average, although they still do not establish accuracy on a customer’s own documents.

The audio model adds the ability to interpret recordings and trigger software tools from spoken requests. Liquid claims its new encoder processes incoming speech before the speaker finishes, allowing the first response token to arrive in as little as 50 milliseconds afterward. This is a company-reported best-case figure, not a complete spoken-response time or API service guarantee.

The LFM3-Omni-2B is Liquid’s first model to combine vision, speech, and language in this way. It has 2.2 billion parameters despite its rounded product name and can interpret video alongside its soundtrack or combine a spoken instruction with an image to produce a tool call.

The published announcement offers one concrete enterprise direction: Mercedes-Benz is exploring how local AI could improve its MBUX Virtual Assistant. The post identifies Mercedes-Benz and Shopify as pre-release testing partners, but provides neither a confirmed production rollout of LFM3 at either company nor measured customer business outcomes.

Decision models offer a different approach from conversational agents. Liquid positions d1-2B as the successor to d1-3B, with new structured answer formats extending the earlier model’s yes/no, choice, and scoring capabilities. Liquid claims they evaluate multiple questions about an input in one processing pass and return structured answers with probabilities. New answer formats include extracting fields, locating objects, estimating numerical values, and selecting multiple applicable options.

For an enterprise that needs to classify an incoming ticket or extract an invoice date, that narrower output may be more useful than generating a conversational response. Probability scores would still need validation against the company’s own records before driving automated decisions.

Liquid also provides a concrete example of customization. It adapted LFM3-VL-2B using approximately 3,000 public wind-turbine images, with several hours of training on an Nvidia A100, then evaluated it on roughly 1,400 held-out images. The company reports classification accuracy of 67%, up from below 10% before specialization. However, its separate detection F1 score—which reflects performance at identifying and locating defects—reaches only 0.11, up from 0.003. This example demonstrates a substantial gain from task-specific training; it does not establish readiness for autonomous industrial inspection. The announcement links to a wind-turbine customization example and fine-tuning notebooks for developers evaluating the approach.

A robotics example uses about six GPU-hours on an AMD Instinct MI325X to adapt the vision model for operating a simulated robot arm through software tools. Liquid describes robotics as a research extension, so that result should be distinguished from a production deployment.

The console pricing makes the hosted deployment option more concrete. Developers must log in to the Liquid console to see these LFM3 prices and obtain an API key to call the new models directly. The authenticated catalog reviewed by VentureBeat lists four LFM3 generation models and marks each as publicly accessible within the service.

At those rates, processing one million uncached input tokens and generating one million output tokens costs $0.28 with LFM3-1.7B. The equivalent text-token totals are $0.60 for the vision model and $1.10 for Audio or Omni, before any audio-token charges. These calculations illustrate token costs, rather than the cost of completing a particular business task.

All four hosted LFM3 entries list 128,000-token context limits. Their rate-limit fields contain dashes, but the console does not promise unlimited requests: its accompanying notice says usage limits are shared across an organization’s API keys. The console also lists d1.1 at $0.02 per million input tokens, the same price for cached input and no output charge. This hosted identifier is distinct from the downloadable d1-2B name; the screenshots do not establish that they are the same model.

For privacy-sensitive buyers, the console states that all listed models have zero data retention. Liquid claims it uses submitted material to fulfill requests and does not retain or log that content, or use it for training. This is the company’s stated service commitment; hosting regions and support guarantees were not specified in the materials reviewed.

Teams choosing local deployment have a different cost structure. Liquid links to the LFM3 weights on Hugging Face, its Playground, and its API console. Its model catalog documents available formats and runtimes, with support varying across text, vision, audio, and decision models. Buyers should check the specific model and format rather than assume every capability works in every runtime.

The downloadable weights also carry commercial conditions. The LFM3-1.7B documentation explicitly identifies LFM Open License v1.0. Under Liquid’s published licensing terms, businesses exceeding $10 million in annual revenue need a separate agreement for commercial use. Organizations may keep their adaptations private, and Liquid’s pricing page says enterprise license fees depend on deployment scale.

This means local deployment can remove per-token API charges without eliminating licensing, hardware, power, or operational expenses. A locally running agent can also transmit information through connected web searches, messaging, and other external tools, even when model inference stays on company hardware.

For enterprise technical leaders, the most defensible starting point is a bounded workflow with measurable outcomes: a document field to extract, a ticket category to assign, or a sequence of approved tool calls. Evaluate the accuracy and latency of the deployed version, then account for licensing, hardware, maintenance, and human review. Liquid’s results make a credible case for testing how much useful work a small model can perform. Whether LFM3 earns a production role will depend on how reliably it completes the particular job an enterprise needs done.

This article was generated by an AI from the original publication. AI can misstate details (numbers, names, dates, quotes) — verify critical information against the original source.

## In the AI reference

Prabha Jha shares her experience studying AI and writing a book

Nvidia and Oracle expand Vera Rubin cloud infrastructure for business

Liquid AI launches Pipette for benchmarking models on devices.

Liquid AI has released LFM2.5-DSpark models with decoding speedup of up to 3.18x.

A 1.5B-active model outperformed a 26B on a significant test.

Liquid AI has launched the LFM2.5-8B-A1B model featuring 8.3 billion parameters for devices.
