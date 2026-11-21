---
id: collect-261121-veille-ia/glm-zhipu/zai-org-glm-ocr-at-main
title: "zai-org/GLM-OCR at main"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["model", "zai-org", "glm-ocr", "type", "transformers", "main", "text", "pipeline"]
source: https://huggingface.co/zai-org/GLM-OCR/tree/main
source_anchor: ""
source_lines: [1, 40]
section: "GLM_Zhipu"
---
# Use a pipeline as a high-level helper
# Warning: Pipeline type "image-to-text" is no longer supported in transformers v5.
# You must load the model directly (see below) or downgrade to v4.x with:
# pip install "transformers<5.0.0"
from transformers import pipeline
pipe = pipeline("image-to-text", model="zai-org/GLM-OCR")
messages = [
{
"role": "user",
"content": [
{"type": "image", "url": "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/p-blog/candy.JPG"},
{"type": "text", "text": "What animal is on the candy?"}
]
},
]
pipe(text=messages)

# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForMultimodalLM
tokenizer = AutoTokenizer.from_pretrained("zai-org/GLM-OCR")
model = AutoModelForMultimodalLM.from_pretrained("zai-org/GLM-OCR", device_map="auto")
messages = [
{
"role": "user",
"content": [
{"type": "image", "url": "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/p-blog/candy.JPG"},
{"type": "text", "text": "What animal is on the candy?"}
]
},
]
inputs = tokenizer.apply_chat_template(
messages,
add_generation_prompt=True,
tokenize=True,
return_dict=True,
return_tensors="pt",
).to(model.device)
outputs = model.generate(**inputs, max_new_tokens=256)
print(tokenizer.decode(outputs[0][inputs["input_ids"].shape[-1]:]))
