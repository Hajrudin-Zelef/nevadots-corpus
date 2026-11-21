---
id: collect-261121-veille-ia/glm-zhipu/glm-5-3-flashx-pushes-zhipu-s-inference-to-200-tokens-a-second
title: "GLM-5.3-FlashX pushes Zhipu's inference to 200 tokens a second"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["zhipu", "tokens", "glm-5", "model", "flashx", "second", "more", "tier"]
source: https://tech.iheartgeek.com/news/glm-5-3-flashx-200-tokens-chinese-accelerators
source_anchor: ""
source_lines: [1, 23]
section: "GLM_Zhipu"
---
# Zhipu's GLM-5.3-FlashX hits 200 tokens a second

Zhipu's new serving tier runs on a cluster of more than 100,000 Chinese-made accelerators and costs about 2.5 times the standard Flash price.

Zhipu has started serving GLM-5.3-FlashX, a faster tier of its open-weight GLM-5.3-Flash model. The company's developer documentation lists the new version as live at an inference speed of 200 tokens a second.

### What the faster tier changes

Chinese technology coverage put the previous serving speed at between 30 and 50 tokens a second, which makes the new tier a five- to six-fold jump on response times. Zhipu bills it separately, at roughly two and a half times the standard Flash rate, and the model is still positioned at the cheaper end of the mainstream.

The specification on Zhipu's documentation keeps the rest of the family's shape: the model code is glm-5.3-flashx, the context window is one million tokens, output can run to 128,000 tokens, and it accepts video, images, text and files while returning text.

### The cluster behind the speed

Zhipu attributes the throughput to an inference cluster built on more than 100,000 Chinese-made AI accelerators. The company says system throughput improved 3.2 times within two weeks and describes the resulting hardware efficiency as comparable to Nvidia graphics processors. Those are vendor figures and have not been measured independently.

Zhipu also says an internal agent called InfraAgent, running on GLM-5.3, took part in the infrastructure work, and presents it as China's first publicly disclosed recursive-self-improvement deployment in a production environment. That framing comes from the company as reported in Chinese technology coverage rather than from a published technical paper.

The base model's launch drew a different kind of attention. GLM-5.3 ran anonymously as Ox Alpha on OpenCode and OpenRouter before it was claimed, logging more than 62 trillion tokens in six days, which is an unusually direct measure of how quickly developers adopt an unnamed model when the price is right.

### Our opinion

Selling speed as its own tier is the most telling part of this release. Zhipu is not claiming a smarter model, it is charging more for the same weights answered faster, which is a pricing model taken straight from cloud compute and applied to open models. The accelerator count is the more politically loaded number: a five-fold speed-up on domestic silicon is precisely the argument Chinese vendors want to make while export controls tighten. Treat the self-improvement claim with more scepticism than the throughput claim, because throughput is measurable and an agent tuning its own infrastructure is still a description of the work rather than a result. The 62 trillion tokens logged under a fake name is the figure worth remembering.
