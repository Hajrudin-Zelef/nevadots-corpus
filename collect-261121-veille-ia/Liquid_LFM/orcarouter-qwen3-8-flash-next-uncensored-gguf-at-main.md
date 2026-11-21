---
id: collect-261121-veille-ia/liquid-lfm/orcarouter-qwen3-8-flash-next-uncensored-gguf-at-main
title: "orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF at main"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: []
keywords: ["qwen3", "orcarouter", "flash-next-uncensored-gguf", "llama", "install", "start", "server", "model"]
source: https://huggingface.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF/tree/main
source_anchor: ""
source_lines: [1, 108]
section: "Liquid_LFM"
---
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  llama.cpp  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with llama.cpp: ##### Install (macOS, Linux)curl -LsSf https://llama.app/install.sh | sh
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M
# Run inference directly in the terminal:
llama cli -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Install from WinGet (Windows)winget install llama.cpp
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M
# Run inference directly in the terminal:
llama cli -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Build from source codegit clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
cmake -B build
cmake --build build -j --target llama-app
# Start a local OpenAI-compatible server with a web UI:
./build/bin/llama serve -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M
# Run inference directly in the terminal:
./build/bin/llama cli -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Use Dockerdocker model run hf.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M
-  Llama new 
-  LM Studio  
-  Jan  
-  vLLM  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF",
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
##### Use Dockerdocker model run hf.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M
-  Ollama  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with Ollama: ollama run hf.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M
-  Unsloth Desktop  
-  Pi  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with Pi: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Configure the model in Pi```
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
          "id": "orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M"
        }
      ]
    }
  }
}
```
##### Run Pi# Start Pi in your project directory:
pi
-  Docker Model Runner  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with Docker Model Runner: docker model run hf.co/orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M
-  Lemonade  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with Lemonade: ##### Pull the model# Download Lemonade from https://lemonade-server.ai/
lemonade pull orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Run and chat with the modellemonade run user.Qwen3.8-Flash-Next-Uncensored-GGUF-Q4_K_M ##### List all available modelslemonade list
-  Hermes Agent  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with Hermes Agent: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Configure Hermes# Install Hermes:
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
hermes setup
# Point Hermes at the local server:
hermes config set model.provider custom
hermes config set model.base_url http://127.0.0.1:9931/v1
hermes config set model.default orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Run Hermeshermes
-  Atomic Chat  
-  OpenClaw  How to use orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF with OpenClaw: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M ##### Configure OpenClaw# Install OpenClaw:
npm install -g openclaw@latest
# Register the local server and set it as the default model:
openclaw onboard --non-interactive --mode local \
  --auth-choice custom-api-key \
  --custom-base-url http://127.0.0.1:9931/v1 \
  --custom-model-id "orcarouter/Qwen3.8-Flash-Next-Uncensored-GGUF:Q4_K_M" \
  --custom-provider-id llama-cpp \
  --custom-compatibility openai \
  --custom-text-input \
  --accept-risk \
  --skip-health ##### Run OpenClawopenclaw agent --local --agent main --message "Hello from Hugging Face"
