---
id: collect-261121-veille-ia/tencent-hunyuan/tencent-hy4-preview-at-main
title: "tencent/Hy4-preview at main"
domain: tencent-hunyuan
role: reference
task: article
actors: []
dates: []
keywords: ["tencent", "hy4-preview", "model", "sglang", "messages", "install", "server", "vllm"]
source: https://huggingface.co/tencent/Hy4-preview/tree/main
source_anchor: ""
source_lines: [1, 102]
section: "Tencent_Hunyuan"
---
- Libraries
-  Transformers How to use tencent/Hy4-preview with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="tencent/Hy4-preview")
messages = [
    {"role": "user", "content": "Who are you?"},
]
pipe(messages)
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("tencent/Hy4-preview")
model = AutoModelForCausalLM.from_pretrained("tencent/Hy4-preview", device_map="auto")
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
-  HuggingChat 
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  vLLM  How to use tencent/Hy4-preview with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "tencent/Hy4-preview"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "tencent/Hy4-preview",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
##### Use Dockerdocker model run hf.co/tencent/Hy4-preview
-  SGLang  How to use tencent/Hy4-preview with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "tencent/Hy4-preview" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "tencent/Hy4-preview",
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
        --model-path "tencent/Hy4-preview" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "tencent/Hy4-preview",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
```
-  Docker Model Runner  How to use tencent/Hy4-preview with Docker Model Runner: docker model run hf.co/tencent/Hy4-preview
