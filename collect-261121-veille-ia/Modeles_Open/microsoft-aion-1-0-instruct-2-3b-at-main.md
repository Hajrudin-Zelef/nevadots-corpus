---
id: collect-261121-veille-ia/modeles-open/microsoft-aion-1-0-instruct-2-3b-at-main
title: "microsoft/Aion-1.0-Instruct-2.3B at main"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["microsoft", "aion-1", "instruct-2", "model", "sglang", "install", "server", "vllm"]
source: https://huggingface.co/microsoft/Aion-1.0-Instruct-2.3B/tree/main
source_anchor: ""
source_lines: [1, 86]
section: "Modeles_Open"
---
- Libraries
-  Transformers How to use microsoft/Aion-1.0-Instruct-2.3B with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="microsoft/Aion-1.0-Instruct-2.3B", trust_remote_code=True)
messages = [
    {"role": "user", "content": "Who are you?"},
]
pipe(messages)
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModelForCausalLM
model = AutoModelForCausalLM.from_pretrained("microsoft/Aion-1.0-Instruct-2.3B", trust_remote_code=True, device_map="auto")
```
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  vLLM  How to use microsoft/Aion-1.0-Instruct-2.3B with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "microsoft/Aion-1.0-Instruct-2.3B"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "microsoft/Aion-1.0-Instruct-2.3B",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
##### Use Dockerdocker model run hf.co/microsoft/Aion-1.0-Instruct-2.3B
-  SGLang  How to use microsoft/Aion-1.0-Instruct-2.3B with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "microsoft/Aion-1.0-Instruct-2.3B" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "microsoft/Aion-1.0-Instruct-2.3B",
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
        --model-path "microsoft/Aion-1.0-Instruct-2.3B" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "microsoft/Aion-1.0-Instruct-2.3B",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
-  Docker Model Runner  How to use microsoft/Aion-1.0-Instruct-2.3B with Docker Model Runner: docker model run hf.co/microsoft/Aion-1.0-Instruct-2.3B
