---
id: collect-261121-veille-ia/deepseek/deepseek-ai-deepseek-v4-pro-0813-tree-main-05fecad4
title: "Load model directly"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek-ai", "deepseek-v4-pro-0813", "model", "sglang", "install", "server", "vllm", "curl"]
source: ""
source_anchor: ""
source_lines: [1, 64]
section: "DeepSeek"
---
Instructions to use deepseek-ai/DeepSeek-V4-Pro-0813 with libraries, inference providers, notebooks, and local apps. Follow these links to get started.
- Libraries
-  Transformers How to use deepseek-ai/DeepSeek-V4-Pro-0813 with Transformers: # Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="deepseek-ai/DeepSeek-V4-Pro-0813")# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-V4-Pro-0813")
model = AutoModelForCausalLM.from_pretrained("deepseek-ai/DeepSeek-V4-Pro-0813", device_map="auto")
- Inference
- HuggingChat
- Notebooks
- Google Colab
- Kaggle
- Local Apps Settings
-  vLLM  How to use deepseek-ai/DeepSeek-V4-Pro-0813 with vLLM: Install from pip and serve model# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "deepseek-ai/DeepSeek-V4-Pro-0813"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Pro-0813",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
	}'Use Dockerdocker model run hf.co/deepseek-ai/DeepSeek-V4-Pro-0813
-  SGLang  How to use deepseek-ai/DeepSeek-V4-Pro-0813 with SGLang: Install from pip and serve model# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "deepseek-ai/DeepSeek-V4-Pro-0813" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Pro-0813",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
	}'Use Docker imagesdocker run --gpus all \
    --shm-size 32g \
    -p 30000:30000 \
    -v ~/.cache/huggingface:/root/.cache/huggingface \
    --env "HF_TOKEN=<secret>" \
    --ipc=host \
    lmsysorg/sglang:latest \
    python3 -m sglang.launch_server \
        --model-path "deepseek-ai/DeepSeek-V4-Pro-0813" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Pro-0813",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
	}'
-  Docker Model Runner  How to use deepseek-ai/DeepSeek-V4-Pro-0813 with Docker Model Runner: docker model run hf.co/deepseek-ai/DeepSeek-V4-Pro-0813
