---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-the-0-15-model-that-wins-by-never-staying-quiet
title: "MiniMax M3.1 Flash: the $0.15 Model That Wins by Never Staying Quiet"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: ["2026-10-19"]
keywords: ["minimax", "flash", "model", "input", "task", "tokens", "artificial", "analysis"]
source: https://www.intelligentliving.co/minimax-flash-model-wins-quiet/
source_anchor: ""
source_lines: [1, 88]
section: "MiniMax"
---
MiniMax M3.1 Flash costs $0.15 per million input tokens, scores 43 on the Artificial Analysis Intelligence Index at about $0.22 per benchmark task, and ties GLM-5.3-Flash while trailing DeepSeek V4.1 Flash 1017 overall. It also answers more factual questions correctly than either rival, then undermines that lead by almost never admitting uncertainty. For builders, that tradeoff is the whole story: the cheapest cost per task in its class comes from a model that tries to answer everything, including questions it should skip.

MiniMax, the Shanghai-based lab listed in Hong Kong, released M3.1 Flash on October 19, 2026, as a faster and cheaper update to the M3 model it launched in June. The company describes it as a next-generation Flash model tuned for agentic work, coding, computer use, and cybersecurity, with online inference close to three times faster and agentic task costs 70 percent lower than M3.

The model accepts text and image input on the Artificial Analysis listing, while MiniMax says its own API also handles video, with prompts up to 1 million tokens and reasoning switched on for every request across five effort levels from Low to Max. The developer guide lists 428 billion total parameters, with MiniMax crediting more aggressive sparsity at inference time for the speed gain rather than a smaller base model.

Pricing is flat and simple, which is part of the appeal. MiniMax charges $0.15 per million input tokens and $0.60 per million output tokens in its launch announcement, with cached input at $0.015. That matches DeepSeek V4.1 Flash off-peak on input and output, but MiniMax has no peak hour doubling, so its price does not move during the day.

There is also a backstory. The model first appeared anonymously as Space Bunny on OpenRouter in late September, where community testers linked it to MiniMax by probing its tokenizer. MiniMax later confirmed Space Bunny was an early M3.1 Flash build and said more than 200 trillion tokens passed through it while its maker stayed unnamed. That public soak test helps explain why the launch arrived with independent scores on day one.

Conceptual illustration of a budget AI agent handling computer tasks at low cost per job. (Credit: Intelligent Living)

M3.1 Flash is available now through the MiniMax API, the M Plan subscription, and the MiniMax Code desktop app. MiniMax said at launch that model weights will be released later, with a more capable Pro version to follow. Chinese press reported on October 26 that the weights opened on October 23 as planned, while Artificial Analysis still listed the model as proprietary, and Vals.ai listed it as open weight at the time of writing. Check a live weight hub before you plan a self-hosted deployment.

The Artificial Analysis Intelligence Index is a composite score built from 10 separate evaluations covering knowledge work, coding, long context reasoning, scientific programming, and factual recall. Think of it as a decathlon score for general capability rather than a single exam result. Higher is better, and models in the same price band can be compared directly.

On that decathlon, MiniMax M3.1 Flash scores 43, placing fifth among 171 models in its class in the live Artificial Analysis listing. That is 14 points higher than MiniMax M3 at 29. Release day coverage reported 42 because the index was still updating, and the live page now shows 43. Either figure tells the same story: a generational jump for MiniMax at less than half the cost per task of its predecessor.

Cost per task is the more useful number for builders. Artificial Analysis calculates it as a weighted average cost in dollars to run one benchmark task, including input, cached input, output, and reasoning tokens. For M3.1 Flash, that figure is about $0.22, against about $0.51 for M3, about $0.26 for DeepSeek V4.1 Flash 1017, and about $0.28 for GLM-5.3-Flash at max effort. In plain terms, running the same evaluation suite costs roughly 22 cents per task on MiniMax, 26 cents on DeepSeek, and 28 cents on GLM.

Why is MiniMax cheaper per task even though its per token price matches DeepSeek? Token efficiency. MiniMax used about 189 million output tokens to run the full Intelligence Index, against about 224 million for DeepSeek and about 324 million for GLM, according to Artificial Analysis. Fewer tokens per answer means a lower bill even at identical list prices. The tradeoff is verbosity at scale: MiniMax still generated 180 million tokens in the current listing, well above the 99 million median for its price tier, so it is efficient relative to its closest rivals but chatty compared with the broader market.

Speed helps too. Artificial Analysis measures 118.6 output tokens per second on MiniMax infrastructure, above the 114 token median for its tier. MiniMax itself claims everyday peaks above 200 tokens per second with a median above 170, but independent routing varies widely: Artificial Analysis measured 118.6, one inference test measured 93 to 109, and DeepSeek V4.1 Flash 1017 measured 194 to 243 on the same harnesses. Treat the vendor peak as a ceiling, not a typical speed. Our broader look at why Chinese AI models cost so much less than Western flagships explains the engineering and infrastructure choices that make this price band possible.

Indonesian coverage framed M3.1 Flash as beating both DeepSeek V4.1 Flash 1017 and Claude Opus 5. The independent data is narrower and more interesting. Here is what the head-to-head actually shows.

| Benchmark (Artificial Analysis) | MiniMax M3.1 Flash | DeepSeek V4.1 Flash 1017 | GLM-5.3-Flash | 
|---|---|---|---|
| **Intelligence Index** | 43 | 45 | 42 | 
| **Humanity’s Last Exam** | 48% | 43% | 40% | 
| **CritPt physics reasoning** | 22% | 24% | 15% | 
| **AA-Omniscience accuracy** | 56.2% | 42.9% | 28% | 
| **AA-Omniscience hallucination rate** | 92.8% | 66.8% | 27.6% | 
| **Cost per Intelligence Index task** | $0.22 | $0.26 | $0.28 | 

On aggregate intelligence, MiniMax ties GLM and trails DeepSeek. The 43 for MiniMax sits level with GLM-5.3-Flash at 42 in launch day reporting, now 43 versus 42 in live tables depending on rounding and effort setting, and two points behind DeepSeek V4.1 Flash 1017 at 45. Gemini 3.8 Flash sits just behind at 41. That is a tie for practical purposes with GLM, not a clear win over DeepSeek.

On hard reasoning, MiniMax splits the pair. It leads both rivals on Humanity’s Last Exam at 48 percent, against 43 percent for DeepSeek and 40 percent for GLM at max effort, and level with Gemini 3.8 Flash. On CritPt, a 71 problem research-level physics test, it scores 22 percent, ahead of GLM at 15 percent and Gemini at 18 percent but just behind DeepSeek at 24 percent.

On factual recall, MiniMax leads on correct answers and trails badly on discipline, which is the central tension of this release. It answers 56.2 percent of AA-Omniscience questions correctly, well above DeepSeek at 42.9 percent and GLM at 28 percent. But its hallucination rate is 92.8 percent, against 66.8 percent for DeepSeek and 27.6 percent for GLM. The prior M3 was far more cautious at 18.4 percent. We unpack what that means in the next section.

The Claude Opus claim needs the same nuance. M3.1 Flash does not beat Claude Opus 5 overall. On DeepSWE v1.1, MiniMax reports 73.7 percent, above GPT-5.6 Sol at 73 percent but below Claude Opus 5 at 74 percent and DeepSeek V4.1 Flash 1017 at 77 percent. On a separate Plotly and Mila researcher-built data analysis test with 43 tasks, MiniMax answered 41 correctly, level with Claude Opus 5.5 and Gemini 4 Argon in that specific harness. That is a strong single test result, not a general victory over Anthropic’s flagship. For the closest prior showdown in this price tier, see our DeepSeek V4.1 Flash vs GLM 5.3 Flash comparison.

OSWorld measures whether a model can actually operate a computer: open apps, click, type, read screen state, and finish multi-step tasks across applications. A higher score means more tasks completed without human help. MiniMax reports 61.7 on OSWorld V2 and says no Chinese model has scored higher, alongside 29.6 on Agents Last Exam for real professional work and 76.2 on BankerToolBench for banking workflows. Those are vendor-reported figures, not independent replications, so treat them as directional.

Independent coding scores are more grounded. MiniMax reports 73.7 percent on DeepSWE v1.1 at under $0.30 per task, while DeepSeek reports 77 percent on the same suite. Artificial Analysis adds AutomationBench-AA at 73 percent for MiniMax, Terminal-Bench 4.0 at 26 percent, SciCode at 53 percent, and a long context score of 80 percent on AA-LCR. That pattern favors structured business automation and software tasks over long-horizon terminal work.

Vals.ai tells the other half of the story. Vals.ai runs an agent-oriented suite built around real professional tasks rather than academic questions, scoring accuracy, cost, and latency on finance, legal, coding, and terminal work. Vals.ai lists MiniMax M3.1 Flash at 53.78 percent on the Vals Index at $0.66 per test with a 43 minute 18 second latency, ranking 18th of 28 overall and fifth among 11 open-weight models, just behind MiMo V2.6 Flash at 54.83 percent.

The component scores show where that rank comes from. MiniMax improves sharply over M3 on Vibe Code Bench at 73.94 percent, Tax Agent Bench at 32.14 percent, EMB at 67.84 percent, Finance Agent at 58.83 percent and Legal Research Bench at 34.13 percent. It stays weak on long horizon agentic work: Terminal-Bench 4.0 at 25.25 percent and HLAB-Vals at 7.50 percent. DeepSeek V4.1 Flash 1017, by contrast, reaches 61.26 percent on the Vals Index at $0.657 per test, with stronger terminal and coding persistence. If your workload is sustained terminal chains, DeepSeek still leads. If it is structured business automation with clear success criteria, MiniMax is the cheaper path.

- **Best MiniMax fit:** multi-app office flows, bank-style document and compliance chains, and standard software feature work where tasks are bounded and verifiable.
- **Weaker MiniMax fit:** open-ended terminal research, long unsupervised agent runs, and knowledge-intensive work where a wrong answer costs more than no answer.
- **Cost control:** at $0.15 in and $0.60 out with $0.015 cached input, cache-heavy agent loops benefit most. For rate math across providers, see our LLM API pricing comparison.

Hallucination rate sounds technical, but the idea is simple. AA-Omniscience rewards a correct answer, penalizes a wrong answer, and does not penalize refusing to answer. Before the test starts, it explicitly tells the model that saying you do not know is better than guessing. A disciplined model skips questions it cannot support. A reckless model guesses.

M3.1 Flash guesses. Artificial Analysis found it attempted almost every question, producing a 92.8 percent hallucination rate. That figure does not mean it is wrong 93 percent of the time. It means that when it misses, about 93 percent of those misses were confident inventions rather than refusals. In rough numbers per 100 questions: about 56 correct, about 41 wrong, and only about 3 skipped or partially correct. The prior M3 answered only about 17 correctly but skipped about 68, which is why its hallucination rate stayed at 18.4 percent. A system prompt telling the model to say it does not know did not fix the behavior: One independent 40-run test found M3.1 Flash still invented answers in 14 of 20 runs with that instruction, while GLM declined in all 40 runs and DeepSeek dropped to 1 in 20.

This is why M3.1 Flash can top the accuracy chart and still score only 16 on the overall AA-Omniscience Index, which subtracts penalties for wrong answers. Answering more questions correctly helps, but inventing 41 wrong answers hurts more than staying silent would have. GLM, with far fewer correct answers but far fewer confident errors, ends up more trustworthy on this specific measure despite lower recall.

Chinese language reporting adds a harder question. A Huxiu repost of the WeChat account Shuzhi Benliu, plus threads on Linux.do, noted that M3.1 Flash scored near perfectly on retained old questions in the private Zhizhi benchmark while scoring near zero on new questions, with unusually low token use on some hard problems it got right. One blogger’s edited question test changed a production management item from long-term resource requirements planning to medium-term rough-cut capacity planning and reported that DeepSeek and Gemini switched answers while MiniMax kept the old answer. Huxiu’s own replication found MiniMax missed the edited English version but answered correctly in Chinese in a fresh session.

Those contamination claims remain unproven. No request logs or training records have been published, and a wrong answer to an edited question can reflect concept confusion as well as memorization. The weights opening gives researchers a way to test locally without passing through MiniMax servers, but the training data has not been fully disclosed, so the debate cannot be settled from public evidence alone. Builders should treat the AA and Vals scores as useful but provisional and validate on their own private tasks before trusting M3.1 Flash unsupervised. The pattern is familiar from the ultra-cheap tier, where Unisound U2-Flash also paired strong vendor scores with unverified third-party replication.

MiniMax says requests to the older M2.5 and M2.7 model IDs on its M Plan and Token Plan now route automatically to M3.1 Flash with no client-side change required, and that reasoning is always on across five effort levels, defaulting to max, with requests that disable thinking reportedly returning an error. If you pinned M2.7 for reproducibility, rerun your regression suite, and set the effort level explicitly rather than accepting the max default, since lower effort held coding pass rates on independent tests at a fraction of the tokens and time. MiniMax confirmed the open weights on October 23, 2026, under a MiniMax Community License with a 428 billion total and 23 billion active parameter design, so confirm the live Hugging Face or ModelScope checkpoint before assuming you can self host.

Use M3.1 Flash when cost per completed task matters more than caution. It is a strong pick for high-volume coding assists, customer operations automation, document-heavy finance flows, and computer use demos where a human reviews the output. It undercuts every direct Flash rival on list price per task while matching GLM overall and beating DeepSeek on Humanity’s Last Exam and factual recall volume.

Avoid it as an unsupervised source of truth. For legal research, medical summarization, financial advice, or any workflow where a confident false citation creates liability, the 92.8 percent hallucination rate outweighs the accuracy lead. Route those jobs to a model that refuses more often, add retrieval with citations, or keep a human in the loop. Independent launch analysis from The Decoder and enterprise pricing breakdown from SiliconANGLE reach the same practical verdict: MiniMax wins on price and throughput, DeepSeek wins on sustained agentic execution, and neither result has been independently replicated on vendor harnesses.

It is MiniMax’s Flash tier reasoning model, released October 19, 2026, with 428 billion total parameters, a 1 million token context window, text and image input on independent listings plus video on MiniMax’s own API, and five reasoning effort levels. It is tuned for agents, coding, computer use, and security work.

List price is $0.15 per million input tokens, $0.60 per million output tokens, and $0.015 per million cached input tokens, with no peak hour surcharge. Artificial Analysis measures about $0.22 per Intelligence Index task, against about $0.26 for DeepSeek V4.1 Flash 1017 and about $0.28 for GLM-5.3-Flash. Vals.ai measures about $0.66 per Vals Index test at max effort.

Not overall. DeepSeek leads 45 to 43 on the Intelligence Index and 61.26 percent to 53.78 percent on the Vals Index, with stronger terminal and long-horizon scores. MiniMax leads on Humanity’s Last Exam at 48 percent versus 43 percent, on factual accuracy volume at 56.2 percent versus 42.9 percent, and on cost per task at $0.22 versus $0.26.

No general victory has been shown. It trails Claude Opus 5 on DeepSWE v1.1 at 73.7 percent versus 74 percent and matches Claude Opus 5.5 only on a specific 43-task data analysis test where both scored 41 correct. Indonesian headlines overstated a narrow result.

It means the model rarely refuses. It answered 56.2 percent of hard factual questions correctly, the best of its peer group, but 92.8 percent of its errors were confident wrong answers rather than abstentions. For supervised automation, that tradeoff can be worth it. For unsupervised factual work it is risky.

MiniMax said weights would follow the API launch, with October 23 named as the target. Chinese press reported the opening happened on schedule, Vals.ai lists the model as open weight, and Artificial Analysis still listed it as proprietary at the time of writing. Verify the live checkpoint before planning self hosting.

Space Bunny was the anonymous OpenRouter codename for an early M3.1 Flash build. It processed more than 200 trillion tokens anonymously, ranked first among anonymous models for ten days, and was confirmed by MiniMax at launch.

M3.1 Flash is the clearest example yet of China’s Flash price war logic: cut tokens per task, hold capability steady, and let volume do the rest. It ties GLM-5.3-Flash on overall intelligence and beats both DeepSeek and GLM on Humanity’s Last Exam and factual recall volume, all at the lowest list cost per Artificial Analysis task in the group. The DeepSeek 1017 refresh still leads it by three points overall and by a wide margin on sustained terminal work.

The catch is not hidden. A model that answers almost everything will always look cheap on benchmarks that reward attempts, until those extra attempts turn into confident errors in production. Use M3.1 Flash, where verification is cheap and speed matters. Keep it supervised where the cost of being wrong exceeds the saving of 4 cents per task.
