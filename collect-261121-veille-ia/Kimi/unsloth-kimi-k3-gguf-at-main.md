---
id: collect-261121-veille-ia/kimi/unsloth-kimi-k3-gguf-at-main
title: "unsloth/Kimi-K3-GGUF at main"
domain: kimi
role: reference
task: article
actors: ["Moonshot AI"]
dates: []
keywords: ["unsloth", "kimi-k3-gguf", "llama", "install", "model", "server", "image", "ud-q4"]
source: https://huggingface.co/unsloth/Kimi-K3-GGUF/tree/main
source_anchor: ""
source_lines: [1, 188]
section: "Kimi"
---
- Libraries
-  Transformers How to use unsloth/Kimi-K3-GGUF with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("image-text-to-text", model="unsloth/Kimi-K3-GGUF")
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModel
model = AutoModel.from_pretrained("unsloth/Kimi-K3-GGUF", device_map="auto")
```
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  llama.cpp  How to use unsloth/Kimi-K3-GGUF with llama.cpp: ##### Install (macOS, Linux)curl -LsSf https://llama.app/install.sh | sh
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL
# Run inference directly in the terminal:
llama cli -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Install from WinGet (Windows)winget install llama.cpp
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL
# Run inference directly in the terminal:
llama cli -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Build from source codegit clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
cmake -B build
cmake --build build -j --target llama-app
# Start a local OpenAI-compatible server with a web UI:
./build/bin/llama serve -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL
# Run inference directly in the terminal:
./build/bin/llama cli -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Use Dockerdocker model run hf.co/unsloth/Kimi-K3-GGUF:UD-Q4_K_XL
-  Llama new 
-  LM Studio  
-  Jan  
-  vLLM  How to use unsloth/Kimi-K3-GGUF with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "unsloth/Kimi-K3-GGUF"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "unsloth/Kimi-K3-GGUF",
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
##### Use Dockerdocker model run hf.co/unsloth/Kimi-K3-GGUF:UD-Q4_K_XL
-  SGLang  How to use unsloth/Kimi-K3-GGUF with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "unsloth/Kimi-K3-GGUF" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "unsloth/Kimi-K3-GGUF",
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
        --model-path "unsloth/Kimi-K3-GGUF" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "unsloth/Kimi-K3-GGUF",
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
-  Ollama  How to use unsloth/Kimi-K3-GGUF with Ollama: ollama run hf.co/unsloth/Kimi-K3-GGUF:UD-Q4_K_XL
-  Unsloth Desktop  
-  Pi  How to use unsloth/Kimi-K3-GGUF with Pi: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Configure the model in Pi```
# Install Pi:
npm install -g @earendil-works/pi-coding-agent
# Add to ~/.pi/agent/models.json:
{
  "providers": {
    "llama-cpp": {
      "baseUrl": "http://localhost:9931/v1",
      "api": "openai-completions",
      "apiKey": "none",
      "models": [
        {
          "id": "unsloth/Kimi-K3-GGUF:UD-Q4_K_XL"
        }
      ]
    }
  }
}
```
##### Run Pi# Start Pi in your project directory:
pi
-  Docker Model Runner  How to use unsloth/Kimi-K3-GGUF with Docker Model Runner: docker model run hf.co/unsloth/Kimi-K3-GGUF:UD-Q4_K_XL
-  Lemonade  How to use unsloth/Kimi-K3-GGUF with Lemonade: ##### Pull the model# Download Lemonade from https://lemonade-server.ai/
lemonade pull unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Run and chat with the modellemonade run user.Kimi-K3-GGUF-UD-Q4_K_XL ##### List all available modelslemonade list
-  Hermes Agent  How to use unsloth/Kimi-K3-GGUF with Hermes Agent: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Configure Hermes# Install Hermes:
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
hermes setup
# Point Hermes at the local server:
hermes config set model.provider custom
hermes config set model.base_url http://127.0.0.1:9931/v1
hermes config set model.default unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Run Hermeshermes
-  Atomic Chat  
-  OpenClaw  How to use unsloth/Kimi-K3-GGUF with OpenClaw: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Kimi-K3-GGUF:UD-Q4_K_XL ##### Configure OpenClaw# Install OpenClaw:
npm install -g openclaw@latest
# Register the local server and set it as the default model:
openclaw onboard --non-interactive --mode local \
  --auth-choice custom-api-key \
  --custom-base-url http://127.0.0.1:9931/v1 \
  --custom-model-id "unsloth/Kimi-K3-GGUF:UD-Q4_K_XL" \
  --custom-provider-id llama-cpp \
  --custom-compatibility openai \
  --custom-text-input \
  --accept-risk \
  --skip-health ##### Run OpenClawopenclaw agent --local --agent main --message "Hello from Hugging Face"
