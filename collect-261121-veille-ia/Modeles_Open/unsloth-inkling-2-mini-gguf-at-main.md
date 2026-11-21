---
id: collect-261121-veille-ia/modeles-open/unsloth-inkling-2-mini-gguf-at-main
title: "unsloth/Inkling-2-Mini-GGUF at main"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["unsloth", "inkling-2-mini-gguf", "llama", "install", "ud-q4", "model", "server", "start"]
source: https://huggingface.co/unsloth/Inkling-2-Mini-GGUF/tree/main
source_anchor: ""
source_lines: [1, 108]
section: "Modeles_Open"
---
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  llama.cpp  How to use unsloth/Inkling-2-Mini-GGUF with llama.cpp: ##### Install (macOS, Linux)curl -LsSf https://llama.app/install.sh | sh
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M
# Run inference directly in the terminal:
llama cli -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Install from WinGet (Windows)winget install llama.cpp
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M
# Run inference directly in the terminal:
llama cli -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Build from source codegit clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
cmake -B build
cmake --build build -j --target llama-app
# Start a local OpenAI-compatible server with a web UI:
./build/bin/llama serve -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M
# Run inference directly in the terminal:
./build/bin/llama cli -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Use Dockerdocker model run hf.co/unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M
-  Llama new 
-  LM Studio  
-  Jan  
-  vLLM  How to use unsloth/Inkling-2-Mini-GGUF with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "unsloth/Inkling-2-Mini-GGUF"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "unsloth/Inkling-2-Mini-GGUF",
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
##### Use Dockerdocker model run hf.co/unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M
-  Ollama  How to use unsloth/Inkling-2-Mini-GGUF with Ollama: ollama run hf.co/unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M
-  Unsloth Desktop  
-  Pi  How to use unsloth/Inkling-2-Mini-GGUF with Pi: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Configure the model in Pi```
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
          "id": "unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M"
        }
      ]
    }
  }
}
```
##### Run Pi# Start Pi in your project directory:
pi
-  Docker Model Runner  How to use unsloth/Inkling-2-Mini-GGUF with Docker Model Runner: docker model run hf.co/unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M
-  Lemonade  How to use unsloth/Inkling-2-Mini-GGUF with Lemonade: ##### Pull the model# Download Lemonade from https://lemonade-server.ai/
lemonade pull unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Run and chat with the modellemonade run user.Inkling-2-Mini-GGUF-UD-Q4_K_M ##### List all available modelslemonade list
-  Hermes Agent  How to use unsloth/Inkling-2-Mini-GGUF with Hermes Agent: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Configure Hermes# Install Hermes:
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
hermes setup
# Point Hermes at the local server:
hermes config set model.provider custom
hermes config set model.base_url http://127.0.0.1:9931/v1
hermes config set model.default unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Run Hermeshermes
-  Atomic Chat  
-  OpenClaw  How to use unsloth/Inkling-2-Mini-GGUF with OpenClaw: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M ##### Configure OpenClaw# Install OpenClaw:
npm install -g openclaw@latest
# Register the local server and set it as the default model:
openclaw onboard --non-interactive --mode local \
  --auth-choice custom-api-key \
  --custom-base-url http://127.0.0.1:9931/v1 \
  --custom-model-id "unsloth/Inkling-2-Mini-GGUF:UD-Q4_K_M" \
  --custom-provider-id llama-cpp \
  --custom-compatibility openai \
  --custom-text-input \
  --accept-risk \
  --skip-health ##### Run OpenClawopenclaw agent --local --agent main --message "Hello from Hugging Face"
