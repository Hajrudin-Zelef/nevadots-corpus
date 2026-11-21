---
id: collect-261121-veille-ia/deepseek/deepseek-ai-deepseek-v4-flash-at-main
title: "deepseek-ai/DeepSeek-V4-Flash at main"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek-ai", "deepseek-v4-flash", "model", "sglang", "install", "server", "vllm", "curl"]
source: https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash/tree/main
source_anchor: ""
source_lines: [1, 77]
section: "DeepSeek"
---
- Libraries
-  Transformers How to use deepseek-ai/DeepSeek-V4-Flash with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="deepseek-ai/DeepSeek-V4-Flash")
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-V4-Flash")
model = AutoModelForCausalLM.from_pretrained("deepseek-ai/DeepSeek-V4-Flash", device_map="auto")
```
- Inference
-  
-  HuggingChat 
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  vLLM  How to use deepseek-ai/DeepSeek-V4-Flash with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "deepseek-ai/DeepSeek-V4-Flash"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Flash",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
	}'
```
##### Use Dockerdocker model run hf.co/deepseek-ai/DeepSeek-V4-Flash
-  SGLang  How to use deepseek-ai/DeepSeek-V4-Flash with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "deepseek-ai/DeepSeek-V4-Flash" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Flash",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
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
        --model-path "deepseek-ai/DeepSeek-V4-Flash" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Flash",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
	}'
```
-  Docker Model Runner  How to use deepseek-ai/DeepSeek-V4-Flash with Docker Model Runner: docker model run hf.co/deepseek-ai/DeepSeek-V4-Flash
