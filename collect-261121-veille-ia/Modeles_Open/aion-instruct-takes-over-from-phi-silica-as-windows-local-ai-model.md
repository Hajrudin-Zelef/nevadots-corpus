---
id: collect-261121-veille-ia/modeles-open/aion-instruct-takes-over-from-phi-silica-as-windows-local-ai-model
title: "Aion Instruct Takes Over From Phi Silica as Windows' Local AI Model"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["model", "windows", "aion", "instruct", "silica", "apps", "output", "devices"]
source: https://www.how2shout.com/ai/aion-instruct-replaces-phi-silica-windows-local-ai.html
source_anchor: ""
source_lines: [1, 27]
section: "Modeles_Open"
---
Microsoft is swapping out the small language model behind Windows’ on-device AI features. Aion Instruct will replace Phi Silica, starting with Windows Insider devices on November 20, and retail Windows devices that meet the hardware requirements will follow in late January 2027.

The new model is smaller, at 2.3 billion parameters against Phi Silica’s 3.8 billion, but it can take in far more text at once. Its input limit jumps from 3,500 tokens to 32,000. It’s also coming to discrete GPUs, including Nvidia’s RTX series and AMD’s RDNA 4 cards.

## The APIs stay put

**Phi Silica** is the model that Windows apps reach through the Windows AI APIs to summarize, rewrite and generate text locally, without sending anything to the cloud. On Copilot+ PCs, it runs on the neural processing unit.

Most apps built on those APIs should keep working after the switch without changes, according to the post. The longer context window is where developers will actually notice a difference, since an app can hand the model a long document or a lengthy chat history in one go instead of chopping it into chunks. Claims of better responsiveness and resource use come without numbers for now, and the technical report that’s supposed to back them up has no date.

## GPU support comes in stages

Aion Instruct will expand to supported GPUs, with Nvidia RTX and AMD RDNA 4 discrete cards named so far. That opens the door for desktops and gaming laptops that have a capable graphics card but aren’t Copilot+ PCs. Support will arrive in stages, as each hardware configuration clears the company’s readiness and quality bar. Intel GPUs don’t get a mention.

## Prompts, LoRAs and output parsing may need work

Not every app gets a free ride.

Developers are being asked to retest if they use heavily customized system prompts, depend on Phi Silica’s exact output strings or behavior, or parse the model’s output in code.

Custom LoRA adapters trained for Phi Silica won’t carry over. They’re the one exception to compatibility, and there’s no migration path described yet.

The parsing issue traces back to a new invisible watermark, added to meet text provenance requirements in certain regions. Those regions aren’t listed, though the post links to the company’s EU AI Act compliance material. Any app that picks apart output strings could be affected.

A standalone Aion Instruct test package has been out since October for checking apps early, and that window effectively closes when retail devices get the model in late January.

January brings one more change. The Limited Access Feature (LAF) token that apps currently need to call the on-device language model is set to go away.
