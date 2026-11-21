---
id: collect-261121-veille-ia/copilot-microsoft/microsoft-decision-1-1-is-generally-available
title: "Microsoft-Decision-1.1 is generally available"
domain: copilot-microsoft
role: reference
task: article
actors: ["Microsoft"]
dates: ["2026-11-17"]
keywords: ["microsoft-decision-1", "decision", "page", "microsoft", "board", "generally", "available", "more"]
source: https://modelsystem.one/news/microsoft-decision-1-1-ga/
source_anchor: ""
source_lines: [1, 26]
section: "Copilot_Microsoft"
---
Source Â· Model card: Microsoft-Decision-1.1

Microsoft announced general availability of **Microsoft-Decision-1.1** on 17 Nov 2026. The post is by Achint Srivastava (VP of Software Engineering, Office of the CTO). @msdev posted at 18:24:31 UTC (15:24 BRT). Catalog page: Microsoft-Decision-1.1, scored **6.7 / 7.6 / 15**. The Microsoft-Decision-1 page stays the first model, still **6.4 / 7.9 / 24** on the 10 Oct JevBench note.

## What was checked

- The Foundry catalog says lifecycle **Generally available (GA)** . Input on that page is text and image. It names Gemma 4 26B-A4B and a limit of 8 images. Weights are not published.
- Microsoft Learn now documents 1.1: question types `noul` ,`choice` and`score` , endpoint`/providers/microsoft/v1/systemone` . On scores, the guide says to prefer thresholds over absolute calibrated ratings.
- OpenRouter lists `microsoft/microsoft-decision-1.1` at**$0.042 per 1M input tokens and $0 output** , context 32,768, text and image, release time 2026-11-17T17:57:35Z.
- The Vercel changelog is dated 17 Nov 2026 and uses the gateway id `microsoft/decision-1.1` . It names boolean, choice and score. It does not publish a price. Its sentence that 1.1 is more accurate, better calibrated and faster than Decision-1 is the changelogâs wording, not a measurement.

## Artificial Analysis, then the other boards

Read 17 Nov 2026 from the live Decision Index v2.1 page. Default decision-model rows only.

Microsoft-Decision-1.1 is **8th of 18** at **67.779** (123.25 ms per task, about $140 per million tasks). Jev 1.14 on the same list is 10th at 66.234. Gemini Decision 1 is 11th at 65.977. GPT-6 Luna (Decisions) is 17th at 60.246. d1.2-ultra leads at 74.846. A non-default row named Microsoft-Decision-1, hosted on OpenRouter, is 62.072. The gap from 67.779 to 62.072 is 5.71 points.

The blogâs â68, up six points, ahead of Jev, Gemini Decision 1 and GPT-6 Lunaâ is the vendorâs rounding of this board, and the âahead ofâ clause matches those three names. It is not a first-place claim. This is a different edition from the v2 table of 8 Nov. See the Gemini Decision 1 note for the earlier v2.1 reading, when Gemini was 10th of 18.

The Hugging Face Decision Index file generated 2026-11-17T18:35:28Z (0.4.1, 307 models) has no Microsoft-Decision row. Jev 1.13 Full there is still 59.22. The JevBench API board has no âDecision-1.1â row. The October Microsoft-Decision-1 rows on that board stay on the old page.

## What stays a vendor claim

The blog also says: more than twice the speed of Decision-1 at the same cost; better calibration; 102 internal benchmarks and more than 385,000 questions, highest average of eight decision models; more than 4,000 customers, naming Epic, Equinor, Atos Group and Raiffeisen Bank; Copilot routing 2.2Ã faster and 4.4Ã cheaper than an LLM; SFT plus RL on a custom engine; Foundry zone prices of $0.046/M (US) and $0.05/M (EU and APAC), output free. The Global Standard line of $0.042/M is the one OpenRouter also lists. None of the rest is an independent board, so none of it moves the score by itself.

Accuracy on the new page is 8 because of the AA row above, not because the blog printed 68. There is no Decision Index Full, so the proportional line does not run, and one board is not enough for accuracy 9.
