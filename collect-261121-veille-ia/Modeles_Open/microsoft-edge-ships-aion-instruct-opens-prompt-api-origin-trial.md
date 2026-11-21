---
id: collect-261121-veille-ia/modeles-open/microsoft-edge-ships-aion-instruct-opens-prompt-api-origin-trial
title: "Microsoft Edge Ships Aion Instruct, Opens Prompt API Origin Trial"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["microsoft", "edge", "aion", "trial", "instruct", "prompt", "model", "origin"]
source: https://www.technobezz.com/news/microsoft-edge-aion-instruct-prompt-api-trial
source_anchor: ""
source_lines: [1, 13]
section: "Modeles_Open"
---
Microsoft Edge has replaced Phi-4-mini with Aion Instruct, a 2.3B-parameter model for AI tasks performed on the device, while opening an origin trial for the Prompt API on live websites. Microsoft says the replacement is smaller and runs faster than Phi, with similar quality on common web tasks. Developers can use the trial to test browser-based AI with visitors beyond the audience previously reached through developer channels.

Aion Instruct is generally available both within Edge and separately through Hugging Face as an open-weights model. Microsoft also plans to bring it to Windows later this year, describing that step as groundwork for additional local AI experiences on Windows and Copilot+ PCs. The model's general availability does not extend to the Prompt API, which remains in an origin trial, or the experimental Decisions API, which remains a developer preview.

**Read more:** Microsoft CEO Satya Nadella Takes the Witness Stand in OpenAI Trial Over 2018 Emails

Edge fetches Aion Instruct when a website or browser feature first requires it, limiting downloads to devices capable of running the model adequately. Users can disable local AI through Edge settings; doing so deletes the model and prevents it from being downloaded again. Microsoft says Aion can run on more devices than Phi, alongside its claimed speed and size improvements.

The Prompt API previously required developers to work in Edge's Canary or Dev channels, where it was offered as a developer preview. Website developers can now register for the Prompt API Origin Trial to assess their implementations with real usage on live sites. Microsoft is seeking feedback about model behavior and compatibility between browsers and models as the API undergoes consideration for standardization.

The separate Decisions API uses Aion Instruct to evaluate text and return probabilities plus a confidence value, without generating a text response. It supports questions requiring a numerical score, a choice between options or a yes/no answer, processing them in one pass. Microsoft suggests developers could use it to identify intent, route submitted content or rank items.

Developers can prototype with the Decisions API in Edge Canary and Dev, and Microsoft warns that its design and implementation may change. Microsoft's broader approach allows sites to choose local models for suitable tasks and turn to a configured cloud provider for wider device support or more capable inference.
