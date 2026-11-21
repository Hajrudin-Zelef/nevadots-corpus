---
id: collect-261121-veille-ia/kimi/moonshotai-kimi-k3-1-pro-at-main
title: "moonshotai/Kimi-K3.1-Pro at main"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["moonshotai", "kimi-k3", "model", "image", "sglang", "install", "server", "vllm"]
source: https://huggingface.co/moonshotai/Kimi-K3.1-Pro/tree/main
source_anchor: ""
source_lines: [1, 118]
section: "Kimi"
---
- Libraries
-  Transformers How to use moonshotai/Kimi-K3.1-Pro with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("image-text-to-text", model="moonshotai/Kimi-K3.1-Pro", trust_remote_code=True)
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModelForSeq2SeqLM
model = AutoModelForSeq2SeqLM.from_pretrained("moonshotai/Kimi-K3.1-Pro", trust_remote_code=True, device_map="auto")
```
- Inference
-  
-  HuggingChat 
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  vLLM  How to use moonshotai/Kimi-K3.1-Pro with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "moonshotai/Kimi-K3.1-Pro"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "moonshotai/Kimi-K3.1-Pro",
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
##### Use Dockerdocker model run hf.co/moonshotai/Kimi-K3.1-Pro
-  SGLang  How to use moonshotai/Kimi-K3.1-Pro with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "moonshotai/Kimi-K3.1-Pro" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "moonshotai/Kimi-K3.1-Pro",
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
        --model-path "moonshotai/Kimi-K3.1-Pro" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "moonshotai/Kimi-K3.1-Pro",
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
-  Docker Model Runner  How to use moonshotai/Kimi-K3.1-Pro with Docker Model Runner: docker model run hf.co/moonshotai/Kimi-K3.1-Pro
