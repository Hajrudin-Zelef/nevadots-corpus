---
id: collect-261121-veille-ia/robotics-kyutai/reki-registering-knowledge-in-vlas
title: "REKI: Registering Knowledge in VLAs"
domain: robotics-kyutai
role: reference
task: article
actors: []
dates: []
keywords: ["reki", "model", "data", "registering", "knowledge", "vlas", "arxivlabs", "pretrained"]
source: https://arxiv.org/abs/2610.22178
source_anchor: ""
source_lines: [1, 25]
section: "Robotics_Kyutai"
---
# Computer Science > Robotics

# Title:REKI: Registering Knowledge in VLAs

View PDF HTML (experimental)
            Abstract:We introduce REKI, a 2.4B vision-language-action model (VLA) pretrained exclusively on public data that sets a new Pareto frontier between performance and model size: on LIBERO-Plus, RoboTwin Clean2Rand and RoboCasa365, it is outperformed only by models about twice its size. REKI consists of a vision-language model (VLM) followed by a flow-matching action expert, and is pretrained on a curated mixture of public robot, egocentric human and vision-language data. Additionally, we endow it with a memory that gives the policy access to its recent past at negligible cost: a few register tokens in the action expert summarise the output of the VLM at each frame and persist in the expert's cache. This memory improves success in simulation and on real robots, and makes oscillations between poses about four times rarer. REKI uses a simple model design and training paradigm, and is optimised to be efficient at training and inference. We release the full code across data curation, training, inference and evaluation, deployment, and all pretrained and finetuned model weights: this https URL.
    

### Current browse context:

# Bibliographic and Citation Tools

# Code, Data and Media Associated with this Article

# Demos

# Recommenders and Search Tools

# arXivLabs: experimental projects with community collaborators

arXivLabs is a framework that allows collaborators to develop and share new arXiv features directly on our website.

Both individuals and organizations that work with arXivLabs have embraced and accepted our values of openness, community, excellence, and user data privacy. arXiv is committed to these values and only works with partners that adhere to them.

Have an idea for a project that will add value for arXiv's community? **Learn more about arXivLabs**.
