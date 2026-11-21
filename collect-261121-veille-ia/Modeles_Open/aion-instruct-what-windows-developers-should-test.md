---
id: collect-261121-veille-ia/modeles-open/aion-instruct-what-windows-developers-should-test
title: "Aion Instruct: What Windows Developers Should Test"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["windows", "aion", "package", "developers", "instruct", "input", "output", "microsofts"]
source: https://dailytech.ai/aion-instruct-windows-migration/
source_anchor: ""
source_lines: [1, 59]
section: "Modeles_Open"
---
On November 13, 2026, Microsoft announced its plan to replace Phi Silica with Aion Instruct in Windows AI experiences. The published comparison changes the model from 3.8 billion to 2.3 billion parameters and the *input* context limit from 3.5K to 32K tokens. A standalone testing package is available now; a Windows Insider rollout is planned for November 20, with retail availability targeted for late January 2027. Developers with custom prompts, LoRAs or output parsers have work to check before that transition. Microsoft’s announcement sets out the figures and dates.

## What changed, and what has not shipped to everyone

| Item | Phi Silica | Aion Instruct | What it means today | 
|---|---|---|---|
| Parameters | 3.8B | 2.3B | A model size figure, not a measured speed or memory result | 
| Input context limit | 3.5K tokens | 32K tokens | More input can fit; output capacity and answer quality are separate questions | 
| Standalone test package | Existing Phi Silica workflow | Available at Microsoft’s `aka.ms/tryaion` | Developers can start validation now | 
| Windows Insider rollout | Current Windows model | November 20 target | A planned Beta External build, not a completed rollout | 
| Retail transition | Current Windows model | Late January 2027 target | Deployment timing can change | 

The first two rows are Microsoft’s published comparison. The same announcement describes the package and schedule. Microsoft’s Windows AI documentation now also says the package is available for testing and LoRA training. These dates describe different stages. The October reference in the announcement is when the standalone package became available, not the date of the November news.

A 32K input limit is about 9.1 times the 3.5K figure if both K labels use the same convention: 32 ÷ 3.5 ≈ 9.1. That arithmetic is an illustrative comparison of published limits, not a benchmark. It does not tell you the maximum generated output, throughput or whether a longer prompt will improve an answer. Our context window explainer explains why input size and usable answer room need separate checks. For a specific document, use the context window checker as a planning aid; confirm the Windows preview’s own limits in your application.

## Who needs to validate the switch

Microsoft says most developers should not need to change their ordinary integrations, while calling out highly customized system prompts, custom LoRAs, output parsing and applications that depend on Phi Silica’s particular words or behavior. Its announcement also says Aion adds invisible watermarking for text provenance requirements in specific regions. Do not assume that an unchanged API surface means identical strings.

| Integration | Migration check | Pass criterion to define before testing | 
|---|---|---|
| Ordinary Windows AI API use | Run representative real tasks on the package and later the target Windows build | The app completes its important user flows with acceptable answers and failure handling | 
| Custom system prompts | Compare raw outputs on a fixed set of ordinary and difficult inputs | Your task-specific correctness and format checks pass | 
| Custom LoRAs | Re-train or adapt through the documented Aion path, then validate on representative devices | The adaptation loads and meets the application’s quality target | 
| Exact strings or output parsers | Check raw Unicode text, delimiters, whitespace and terminal statuses against parser expectations | The parser accepts valid responses and rejects invalid ones safely | 
| Device-dependent behavior | Test each device class you plan to support | Readiness, download, latency and memory meet your own deployment requirements | 

These are **proposed tests**, not tests we ran or promises that a configuration will pass. Keep the same inputs, scoring rules and application version for comparisons. Save the full responses and the failure reasons. Microsoft’s Aion prompt guide recommends a task contract, representative scenarios, a reproducible baseline and held-out cases. It also recommends changing one meaningful prompt element at a time after examining failures.

For parsers, inspect the raw output before any cleanup. Microsoft’s watermarking warning is a reason to validate your parser, not an instruction to remove provenance marks. A parser that silently rewrites an unexpected answer can hide a regression. If exact formatting matters, validate it in application code and keep a safe error path.

## What the preview can prove today

Microsoft links a standalone package for end-to-end validation. Its sample repository provides a preview application and guidance. Package availability establishes a route to test; it does not establish that Aion is active on a particular retail PC. Record the package, runtime, device, driver and prompt versions alongside your results so you can repeat the comparison when the Insider build arrives.

The prompt guide’s preview API scope lists an optional system prompt, Temperature, TopP and TopK, content-filter options, streamed progress updates and a final response status. It says the preview does **not** expose a maximum generated-token setting, tokenizer-token counts, custom chat-template selection, JSON-schema or regex constrained decoding, or candidate scoring. Those techniques appear elsewhere in the guide as general methods; they are not native controls in this preview. Treat progress callback counts as updates rather than token counts, and validate any required output structure in your app.

The existing Phi Silica hardware page lists GPU and VRAM requirements for Phi Silica. They are not a complete Aion device matrix. Microsoft names supported NVIDIA RTX and AMD RDNA4 GPUs for Aion and says support will expand in stages. Do not turn those families into a guarantee that every card or driver works. Our local LLM VRAM guide explains the memory components to measure, but it cannot substitute for an Aion-specific requirement or a device test.

## A practical decision before rollout

Inventory the places where your app depends on model behavior: prompts, LoRAs, parser rules, exact wording and device assumptions. Pick a small, representative scenario set and write acceptance criteria before running the package. Compare raw outputs and failures. Then repeat the same checks on the planned Insider build and on the retail environment you actually support. Keep a rollback path for a behavior-dependent feature until it meets your criteria.

Microsoft has not published an Aion-specific minimum VRAM figure, measured performance result for your device, output-token ceiling or price in these sources. We have not downloaded or run the package, measured latency, evaluated answer quality or confirmed availability on a particular PC. The AI model tracker lists the release; use Microsoft’s announcement and current Windows documentation for the rollout status before changing a production app.

## Sources

- Microsoft announcement — November 13 release, model comparison, developer impact and planned dates.
- Windows AI documentation — current transition note and existing Phi Silica hardware and API workflow.
- Aion prompt guide — preview API scope and evaluation guidance.
- Aion preview sample — Microsoft’s sample application repository.

All pages read on November 15, 2026.

Free tool
**Will my whole document fit?**

*Check a document*
