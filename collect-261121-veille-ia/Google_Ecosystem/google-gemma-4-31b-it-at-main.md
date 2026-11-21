---
id: collect-261121-veille-ia/google-ecosystem/google-gemma-4-31b-it-at-main
title: "google/gemma-4-31B-it at main"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["google", "gemma-4-31b-it", "model", "image", "text", "type", "sglang", "messages"]
source: https://huggingface.co/google/gemma-4-31B-it/tree/main
source_anchor: ""
source_lines: [1, 148]
section: "Google_Ecosystem"
---
- Libraries
-  Transformers How to use google/gemma-4-31B-it with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("image-text-to-text", model="google/gemma-4-31B-it")
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
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoProcessor, AutoModelForMultimodalLM
processor = AutoProcessor.from_pretrained("google/gemma-4-31B-it")
model = AutoModelForMultimodalLM.from_pretrained("google/gemma-4-31B-it", device_map="auto")
messages = [
    {
        "role": "user",
        "content": [
            {"type": "image", "url": "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/p-blog/candy.JPG"},
            {"type": "text", "text": "What animal is on the candy?"}
        ]
    },
]
inputs = processor.apply_chat_template(
	messages,
	add_generation_prompt=True,
	tokenize=True,
	return_dict=True,
	return_tensors="pt",
).to(model.device)
outputs = model.generate(**inputs, max_new_tokens=256)
print(processor.decode(outputs[0][inputs["input_ids"].shape[-1]:]))
```
- Inference
-  
-  HuggingChat 
- Notebooks
-  Google Colab 
-  Kaggle 
-  AMD Developer Cloud 
- Local Apps Settings
-  vLLM  How to use google/gemma-4-31B-it with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "google/gemma-4-31B-it"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "google/gemma-4-31B-it",
		"messages": [
			{
				"role": "user",
				"content": [
					{
						"type": "text",
						"text": "Describe this image in one sentence."
					},
					{
						"type": "image_url",
						"image_url": {
							"url": "https://cdn.britannica.com/61/93061-050-99147DCE/Statue-of-Liberty-Island-New-York-Bay.jpg"
						}
					}
				]
			}
		]
	}'
```
##### Use Dockerdocker model run hf.co/google/gemma-4-31B-it
-  SGLang  How to use google/gemma-4-31B-it with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "google/gemma-4-31B-it" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "google/gemma-4-31B-it",
		"messages": [
			{
				"role": "user",
				"content": [
					{
						"type": "text",
						"text": "Describe this image in one sentence."
					},
					{
						"type": "image_url",
						"image_url": {
							"url": "https://cdn.britannica.com/61/93061-050-99147DCE/Statue-of-Liberty-Island-New-York-Bay.jpg"
						}
					}
				]
			}
		]
	}'
```
##### Use Docker images```
docker run --gpus all \
    --shm-size 32g \
    -p 30000:30000 \
    -v ~/.cache/huggingface:/root/.cache/huggingface \
    --env "HF_TOKEN=<secret>" \
    --ipc=host \
    lmsysorg/sglang:latest \
    python3 -m sglang.launch_server \
        --model-path "google/gemma-4-31B-it" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "google/gemma-4-31B-it",
		"messages": [
			{
				"role": "user",
				"content": [
					{
						"type": "text",
						"text": "Describe this image in one sentence."
					},
					{
						"type": "image_url",
						"image_url": {
							"url": "https://cdn.britannica.com/61/93061-050-99147DCE/Statue-of-Liberty-Island-New-York-Bay.jpg"
						}
					}
				]
			}
		]
	}'
```
-  Docker Model Runner  How to use google/gemma-4-31B-it with Docker Model Runner: docker model run hf.co/google/gemma-4-31B-it
