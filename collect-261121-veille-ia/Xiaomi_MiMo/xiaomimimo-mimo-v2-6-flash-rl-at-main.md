---
id: collect-261121-veille-ia/xiaomi-mimo/xiaomimimo-mimo-v2-6-flash-rl-at-main
title: "XiaomiMiMo/MiMo-V2.6-Flash-RL at main"
domain: xiaomi-mimo
role: reference
task: article
actors: ["Xiaomi"]
dates: []
keywords: ["xiaomimimo", "mimo-v2", "flash-rl", "model", "sglang", "install", "server", "vllm"]
source: https://huggingface.co/XiaomiMiMo/MiMo-V2.6-Flash-RL/tree/main
source_anchor: ""
source_lines: [1, 89]
section: "Xiaomi_MiMo"
---
- Libraries
-  Transformers How to use XiaomiMiMo/MiMo-V2.6-Flash-RL with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="XiaomiMiMo/MiMo-V2.6-Flash-RL", trust_remote_code=True)
messages = [
    {"role": "user", "content": "Who are you?"},
]
pipe(messages)
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModelForCausalLM
model = AutoModelForCausalLM.from_pretrained("XiaomiMiMo/MiMo-V2.6-Flash-RL", trust_remote_code=True, device_map="auto")
```
- Inference
-  
-  HuggingChat 
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  vLLM  How to use XiaomiMiMo/MiMo-V2.6-Flash-RL with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "XiaomiMiMo/MiMo-V2.6-Flash-RL"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "XiaomiMiMo/MiMo-V2.6-Flash-RL",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
##### Use Dockerdocker model run hf.co/XiaomiMiMo/MiMo-V2.6-Flash-RL
-  SGLang  How to use XiaomiMiMo/MiMo-V2.6-Flash-RL with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "XiaomiMiMo/MiMo-V2.6-Flash-RL" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "XiaomiMiMo/MiMo-V2.6-Flash-RL",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
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
        --model-path "XiaomiMiMo/MiMo-V2.6-Flash-RL" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "XiaomiMiMo/MiMo-V2.6-Flash-RL",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
-  Docker Model Runner  How to use XiaomiMiMo/MiMo-V2.6-Flash-RL with Docker Model Runner: docker model run hf.co/XiaomiMiMo/MiMo-V2.6-Flash-RL
