---
id: collect-261121-veille-ia/kimi/moonshotai-kimi-k3-at-main
title: "moonshotai/Kimi-K3 at main"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["moonshotai", "kimi-k3", "pipeline", "transformers", "model", "main", "import", "trust"]
source: https://huggingface.co/moonshotai/Kimi-K3/tree/main
source_anchor: ""
source_lines: [1, 8]
section: "Kimi"
---
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("image-text-to-text", model="moonshotai/Kimi-K3", trust_remote_code=True)

# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModel
model = AutoModel.from_pretrained("moonshotai/Kimi-K3", trust_remote_code=True, device_map="auto")
