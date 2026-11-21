---
id: collect-261121-veille-ia/gemini-google/gemini-4-hydrogen-tops-argon-rolls-out-to-everyone
title: "Gemini 4 Hydrogen Tops Argon, Rolls Out to Everyone"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["gemini", "hydrogen", "google", "model", "argon", "tokens", "million", "free"]
source: https://shiftdelete.net/en/gemini-4-hydrogen
source_anchor: ""
source_lines: [1, 138]
section: "Gemini_Google"
---
# Gemini 4 Hydrogen Tops Argon, Rolls Out to Everyone

Google's new fast model finishes tasks 2.5x faster than Gemini 3.8 Flash, costs $1.50 per 1M input tokens and reaches free users. What does it offer?

Google's blog post published on November 12, 2026 by Koray Kavukcuoglu introduced Gemini 4 Hydrogen as the speed and efficiency focused model in the Gemini 4 family; Google says it is bringing the model to daily coding, agentic workflows, the Gemini app and Search with global availability.

According to Google, Hydrogen scores 51 on the Artificial Analysis Intelligence Index at the default high thinking level, and it is priced at $1.50 per million input tokens and $7.50 per million output tokens across the full 1 million token context window. In a November 12, 2026 report, 9to5Google noted that Google is not using the Flash name for this model, and that Hydrogen effectively occupies the tier that follows Gemini 3.8 Flash from September.


Image: Google

## What is Gemini 4 Hydrogen positioned to do?

Google is not presenting Gemini 4 Hydrogen as the model for every hardest long-horizon reasoning task. Instead, the company positions it as a fast execution model for broad everyday use. In Google's own framing, Gemini 4 Carbon and Gemini 4 Argon remain the upper models for difficult, sustained reasoning work. Hydrogen is designed for daily coding, debugging, tool use and multi-step agent workflows where completion speed matters.

This positioning makes the Gemini 4 lineup easier to understand. Our earlier coverage of Gemini 4 Carbon pricing showed how Google separated its most complex work into a premium tier. Our coverage of Gemini 4 Argon placed Argon closer to code, analysis and advanced reasoning. Hydrogen is the model Google wants users to meet more often in daily work.

That is why Google's claim that Gemini 4 is now for everyone matters. The statement includes the Gemini app, where free users will get Hydrogen through automatic model selection when a prompt is appropriate. It also includes Google Search, where the model upgrade is rolling out globally to all users.

## What does the Artificial Analysis score show?

Google says Gemini 4 Hydrogen scores 51 on the Artificial Analysis Intelligence Index at the default high thinking level. On the same index, Gemini 4 Argon is listed at 50. Hydrogen reaches 54 when the new xhigh thinking level is used.

Those numbers matter because Google is not describing Hydrogen as a simple lightweight model. The company says Hydrogen generally finishes work faster than models in its class. However, there is an important limitation: Google's blog post does not name the rival models or provide their scores, so readers cannot fully evaluate which direct comparisons are being made.

The Artificial Analysis score should not be treated as the only basis for a buying or deployment decision. Hydrogen's main promise is to keep quality high enough while making latency, cost and token use more predictable. Its real value will depend less on a single score and more on how consistently it performs inside repeatable work.

## How much does Gemini 4 Hydrogen cost?

According to the Google AI Studio pricing page, Gemini 4 Hydrogen uses fixed pricing across the full 1 million token context window. Paid usage costs $1.50 per million input tokens and $7.50 per million output tokens. Output pricing includes thinking tokens.

The free tier lists input and output as free. Google also notes a privacy distinction that matters for developers and organizations: data in the free tier may be used for product improvement, while data in the paid tier is not used for product improvement.

The pricing page also lists context caching at $0.075 per million tokens, limited to automatic or implicit caching. Grounding with Google Search includes 5,000 free requests per month, shared across Gemini 3 and later models. After that, the price is $14 per 1,000 requests.

## How does Orchestrator use Carbon and Hydrogen together?

Google's developer pitch is built around the idea that real software work often needs more than one model. In that workflow, upper models such as Carbon and Argon handle architecture decisions, difficult bugs and complex reasoning. Hydrogen handles faster execution steps such as writing code, running tests and validating changes.

The new Orchestrator in Google Antigravity can dynamically switch between models such as Carbon and Hydrogen in the same session. According to Google's Terminal-Bench 4.0 data, Orchestrator achieves task success close to Carbon while using 24% less quota on average and completing tasks 19% faster.

In parallel agent setups, Carbon can act as the coordinator while Hydrogen runs as worker and verifier agents in separate workspaces. Those agents can make changes and test them at the same time. This makes Hydrogen look less like a stand-alone chatbot and more like a high-throughput execution engine inside a multi-model system.

## What changed compared with Gemini 3.8 Flash?

According to 9to5Google's November 12, 2026 report, Google did not use the Flash brand for Hydrogen. The Gemini app model picker places 3.5 Flash-Lite under fast responses, 4 Hydrogen under everyday help, 3.1 Pro under detailed research for AI Pro users, 4 Argon under code and analysis, and 4 Carbon under the most complex work.

Google's own data says Hydrogen completes tasks 2.5 times faster than Gemini 3.8 Flash at the default high setting. On Terminal-Bench 4.1, Hydrogen reaches 57%, while 3.8 Flash is listed at 18.2%. Google describes that as more than a threefold improvement.

The company also says Hydrogen reduces fabricated claims by more than 5 times. It is described as adapting more gracefully when a tool call fails or a search returns empty results. In early partner tests across complex graph, SQL and data-heavy enterprise workflows, Hydrogen ran 2 to 2.8 times faster than 3.8 Flash and used 12% to 23% fewer tokens.

## Which use cases and benchmarks stand out?

Google emphasizes repository-level coding and debugging. The company says Hydrogen makes targeted, surgical edits and does not rewrite unrelated files. Google also says Hydrogen beats similarly positioned models on SWE-sweep, although the blog post does not provide a numeric result for that benchmark. Parallel batched file reading and search calls are presented as another advantage for developer workflows.

Open-ended agent tasks are another major part of Google's claim. Hydrogen ranks 5th on arena.ai's live Agent Arena benchmark. Google says it leads its class on HLE-Diamond with 58.8%, ranks first overall on Vals Tax Agent Bench with 52.3%, and shares first place on Vals Legal Research Bench with 55.8%. Google also says Hydrogen leads its class on AutomationBench-AA, but does not give a number.

Multimodal knowledge work is also central to the launch. Hydrogen supports native 1 million token input and up to 1 million token output. Google says it can create grounded reports from PDFs, tables, charts, audio and video recordings. The company reports 53.4% on the new Multimodal Knowledge Agent benchmark and 71.6% on Agents' Last Exam v1.1, saying Hydrogen beats other models there.

For web development, Google highlights live interface prototyping and design-to-code work. The demos include Geminisland, where a single prompt adds structures to a shared voxel island that can be explored in first person; a WebGL2 shader e-commerce storefront generated in Antigravity; a Google Sheets animated pixel canvas; and a Google Slides deck created from a YouTube keynote video with an automatic layout correction loop. Google also says Gemini in Google Sheets produces interactive models from financial data more than 30% faster than previous models in Sheets canvas.

## What are the four thinking levels for?

Gemini 4 Hydrogen offers four thinking levels: low, medium, high and the new xhigh. The default setting is high. Google says this gives users predictable scaling across quality, latency and cost. In practical terms, users can wait and pay only for the amount of reasoning depth a task actually needs.

Google also says Hydrogen matches or outperforms some rival models in its class across all levels, usually with faster completion and fewer output tokens. Again, the limitation is that the blog does not name those rival models or provide full per-level comparison tables. That makes the claim interesting, but not fully independently checkable from the launch post alone.

## What changes in the Gemini app?

In the Gemini app, Google says Hydrogen follows instructions better, calls the right tools and produces reliable output with less back and forth. The model is described as more faithful to tone, format and rules. For everyday work such as drafting emails, preparing work notes and rewriting text, Google is aiming for first-try usable results.

Hydrogen can create tables for product comparisons. It can also pull context from uploaded files, Gmail, Google Drive and connected apps such as Airtable and Dropbox. Google showed demos including an interactive 3D globe and music engine based on real-time lightning data, a WebGL2 water and glass visualizer, and an archival video edit about the element Hydrogen.

According to the Gemini app account on X on November 12, 2026, free users will access Hydrogen through automatic model selection when it is appropriate for the prompt. The model is rolling out globally on desktop, mobile and web.

## What will Hydrogen change in Google Search?

In a November 12, 2026 Google Search blog post, Robby Stein described Hydrogen as the biggest model upgrade yet for Search. The upgrade is global for all users.

Google says Hydrogen improves understanding of multi-part, typo-heavy or highly specific questions. Search responses are expected to become more visual, with images, structured answers, comparison tables and custom tools. Google's query fan-out technique is also said to be stronger, enabling a wider range of links and previously undiscovered sites to appear.

This part matters for publishers as well as users. If Search truly surfaces a broader link set, well-structured pages with clear sources may gain visibility. The real effect, however, will only be visible through Search Console data after the rollout reaches actual queries.

## What do Google's safety claims cover?

Google says Hydrogen reduces overstepping in agent tasks by up to 81% compared with 3.8 Flash, and reduces underserving by up to 79%. In tool-use and coding tests, the company says misaligned actions and task shortcuts are reduced by 12 times.

For impossible or blocked tasks, Hydrogen is said to be up to 2 times less likely to exceed permission boundaries. Google also reports a 96.7% honesty rate in an internal agent honesty test. Across more than 200,000 supervised turns, the company reports 99.9% chain-of-thought readability and zero deceptive reasoning manipulation.

For indirect prompt injection, Hydrogen is described as more than 2.3 times more resistant on the Gray Swan IPI benchmark. On HealthBench Pro, Google says false refusals are below 0.4%, and that the model better distinguishes biosecurity risks from legitimate medical and scientific questions. Google says the detailed safety information is available in the model card.

## Who can use Gemini 4 Hydrogen?

Gemini 4 Hydrogen is available globally starting November 12, 2026. Developers can use it through the Gemini API in Google AI Studio and in the paid tier of Android Studio. Paid Google AI Pro and Ultra subscribers can use Hydrogen in Google Antigravity.

For organizations, access is available through Gemini for business, either via API or the model menu. Google Antigravity and Gemini in Google Sheets for eligible business subscriptions are also part of the enterprise access path. For everyone, Google lists the Gemini app model menu and Google Search. Gemini in Google Sheets is coming first to Google AI Pro and Ultra subscribers. Google's November 12, 2026 post on X also lists Gemini in Chrome for AI Pro and Ultra subscribers.

## What do the benchmark and access tables show?

| Area | Hydrogen result shared by Google | Note | 
|---|---|---|
| Artificial Analysis Intelligence Index | High: 51, xhigh: 54 | Argon is listed at 50 | 
| Terminal-Bench 4.1 | 57% | 3.8 Flash: 18.2% | 
| Agent Arena | 5th place | arena.ai live benchmark | 
| HLE-Diamond | 58.8% | Google says class-leading | 
| Vals Tax Agent Bench | 52.3% | Google says first overall | 
| Vals Legal Research Bench | 55.8% | Shares first place | 
| Multimodal Knowledge Agent | 53.4% | New benchmark | 
| Agents' Last Exam v1.1 | 71.6% | Google says it beats other models | 

| Use | Price or access | Detail | 
|---|---|---|
| Gemini API paid input | $1.50 per 1M tokens | Fixed across 1M token context | 
| Gemini API paid output | $7.50 per 1M tokens | Includes thinking tokens | 
| Free tier | Input and output are free | Data may be used for product improvement | 
| Context caching | $0.075 per 1M tokens | Automatic implicit caching only | 
| Grounding with Search | 5,000 requests per month free | Then $14 per 1,000 requests | 
| Developers | Google AI Studio, Gemini API, paid Android Studio tier | Global from November 12, 2026 | 
| Everyone | Gemini app and Google Search | Automatic model selection when appropriate | 

Meet Gemini 4 Hydrogen: built for rapid coding, multimodal synthesis, and everyday workflows.

It finishes tasks 2.5x faster than 3.8 Flash and surpasses Argon on the @ArtificialAnlys Intelligence Index. https://t.co/pY6eZlggqD


## Our take: what does Hydrogen really mean?

Gemini 4 Hydrogen's biggest meaning is that Google is trying to move high-end AI from a premium research-only tool into the everyday production line. Carbon and Argon remain positioned for harder reasoning work, while Hydrogen takes on the fast and frequently repeated parts of the workflow. That is a more realistic model for software development, data operations, file-based reporting and internal enterprise agents.

### How to read the numbers

If Google's claims hold up, Hydrogen is not just faster than Gemini 3.8 Flash. It may also be more controlled and less wasteful. A 2.5x task speed increase, a 57% Terminal-Bench 4.1 result, more than 5x fewer fabricated claims and 12% to 23% lower token use in partner tests would matter for developer cost and reliability. But most of these figures come from Google's own tests and have not been independently verified.

On pricing, Hydrogen sits in a different class from Carbon at $1.50 per million input tokens and $7.50 per million output tokens. However, Google's blog post does not include a direct price comparison with Gemini 3.8 Flash. That means the real cost advantage over the older Flash tier can only be understood through independent tests on the same workloads.

The benchmark story also needs caution. Google's post does not name rival models or list their scores, using the broader phrase models in its class instead. Some benchmarks, including Multimodal Knowledge Agent, are newly announced or less widely known. That does not make Hydrogen weak; it simply means the measurement context matters when reading the claims.

Overall, Gemini 4 Hydrogen looks like the model that strengthens Google's triangle of speed, cost and access. Its largest impact may not come from single chat answers, but from multi-model Orchestrator workflows, the Gemini app and more visual, more connected Search answers. The key question for developers and organizations is simple: when is Hydrogen enough on its own, and when does a workflow still need Carbon or Argon?
