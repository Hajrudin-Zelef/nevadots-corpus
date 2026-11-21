---
id: collect-261121-veille-ia/qwen/unsloth-qwen3-8-27b-gguf-at-main
title: "unsloth/Qwen3.8-27B-GGUF at main"
domain: qwen
role: reference
task: article
actors: ["Alibaba"]
dates: []
keywords: ["unsloth", "qwen3", "b-gguf", "llama", "install", "ud-q4", "local", "server"]
source: https://huggingface.co/unsloth/Qwen3.8-27B-GGUF/tree/main
source_anchor: ""
source_lines: [1, 78]
section: "Qwen"
---
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  llama.cpp  How to use unsloth/Qwen3.8-27B-GGUF with llama.cpp: ##### Install (macOS, Linux)curl -LsSf https://llama.app/install.sh | sh
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M
# Run inference directly in the terminal:
llama cli -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Install from WinGet (Windows)winget install llama.cpp
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M
# Run inference directly in the terminal:
llama cli -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Build from source codegit clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
cmake -B build
cmake --build build -j --target llama-app
# Start a local OpenAI-compatible server with a web UI:
./build/bin/llama serve -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M
# Run inference directly in the terminal:
./build/bin/llama cli -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Use Dockerdocker model run hf.co/unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M
-  Llama new 
-  LM Studio  
-  Jan  
-  Ollama  How to use unsloth/Qwen3.8-27B-GGUF with Ollama: ollama run hf.co/unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M
-  Unsloth Desktop  
-  Pi  How to use unsloth/Qwen3.8-27B-GGUF with Pi: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Configure the model in Pi```
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
          "id": "unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M"
        }
      ]
    }
  }
}
```
##### Run Pi# Start Pi in your project directory:
pi
-  Docker Model Runner  How to use unsloth/Qwen3.8-27B-GGUF with Docker Model Runner: docker model run hf.co/unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M
-  Lemonade  How to use unsloth/Qwen3.8-27B-GGUF with Lemonade: ##### Pull the model# Download Lemonade from https://lemonade-server.ai/
lemonade pull unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Run and chat with the modellemonade run user.Qwen3.8-27B-GGUF-UD-Q4_K_M ##### List all available modelslemonade list
-  Hermes Agent  How to use unsloth/Qwen3.8-27B-GGUF with Hermes Agent: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Configure Hermes# Install Hermes:
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
hermes setup
# Point Hermes at the local server:
hermes config set model.provider custom
hermes config set model.base_url http://127.0.0.1:9931/v1
hermes config set model.default unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Run Hermeshermes
-  Atomic Chat  
-  OpenClaw  How to use unsloth/Qwen3.8-27B-GGUF with OpenClaw: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M ##### Configure OpenClaw# Install OpenClaw:
npm install -g openclaw@latest
# Register the local server and set it as the default model:
openclaw onboard --non-interactive --mode local \
  --auth-choice custom-api-key \
  --custom-base-url http://127.0.0.1:9931/v1 \
  --custom-model-id "unsloth/Qwen3.8-27B-GGUF:UD-Q4_K_M" \
  --custom-provider-id llama-cpp \
  --custom-compatibility openai \
  --custom-text-input \
  --accept-risk \
  --skip-health ##### Run OpenClawopenclaw agent --local --agent main --message "Hello from Hugging Face"
