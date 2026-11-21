---
id: collect-261121-veille-ia/nvidia-hardware/nvidia-nvidia-nemotron-3-nano-30b-a3b-bf16-at-main
title: "nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16 at main"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["nvidia", "nvidia-nemotron-3-nano-30b-a3b-bf16", "model", "sglang", "messages", "install", "server", "true"]
source: https://huggingface.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16/tree/main
source_anchor: ""
source_lines: [1, 101]
section: "NVIDIA_Hardware"
---
- Libraries
-  Transformers How to use nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16 with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16", trust_remote_code=True)
messages = [
    {"role": "user", "content": "Who are you?"},
]
pipe(messages)
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16", trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained("nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16", trust_remote_code=True, device_map="auto")
messages = [
    {"role": "user", "content": "Who are you?"},
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
```
- Inference
-  
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  vLLM  How to use nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16 with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
##### Use Dockerdocker model run hf.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16
-  SGLang  How to use nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16 with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16",
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
        --model-path "nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
-  Docker Model Runner  How to use nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16 with Docker Model Runner: docker model run hf.co/nvidia/NVIDIA-Nemotron-3-Nano-30B-A3B-BF16
