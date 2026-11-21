---
id: collect-261121-veille-ia/glm-zhipu/zai-org-glm-5-3-tree-main-8a7967a9
title: "Load model directly"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["model", "zai-org", "glm-5", "sglang", "messages", "install", "server", "vllm"]
source: ""
source_anchor: ""
source_lines: [1, 89]
section: "GLM_Zhipu"
---
Instructions to use zai-org/GLM-5.3 with libraries, inference providers, notebooks, and local apps. Follow these links to get started.
- Libraries
-  Transformers How to use zai-org/GLM-5.3 with Transformers: # Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="zai-org/GLM-5.3")
messages = [
    {"role": "user", "content": "Who are you?"},
]
pipe(messages)# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("zai-org/GLM-5.3")
model = AutoModelForCausalLM.from_pretrained("zai-org/GLM-5.3", device_map="auto")
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
- Inference
- HuggingChat
- Notebooks
- Google Colab
- Kaggle
- Local Apps Settings
-  vLLM  How to use zai-org/GLM-5.3 with vLLM: Install from pip and serve model# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "zai-org/GLM-5.3"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "zai-org/GLM-5.3",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'Use Dockerdocker model run hf.co/zai-org/GLM-5.3
-  SGLang  How to use zai-org/GLM-5.3 with SGLang: Install from pip and serve model# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "zai-org/GLM-5.3" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "zai-org/GLM-5.3",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'Use Docker imagesdocker run --gpus all \
    --shm-size 32g \
    -p 30000:30000 \
    -v ~/.cache/huggingface:/root/.cache/huggingface \
    --env "HF_TOKEN=<secret>" \
    --ipc=host \
    lmsysorg/sglang:latest \
    python3 -m sglang.launch_server \
        --model-path "zai-org/GLM-5.3" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "zai-org/GLM-5.3",
		"messages": [
			{
				"role": "user",
				"content": "What is the capital of France?"
			}
		]
	}'
-  Docker Model Runner  How to use zai-org/GLM-5.3 with Docker Model Runner: docker model run hf.co/zai-org/GLM-5.3
