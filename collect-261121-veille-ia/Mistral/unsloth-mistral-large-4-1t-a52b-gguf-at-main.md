---
id: collect-261121-veille-ia/mistral/unsloth-mistral-large-4-1t-a52b-gguf-at-main
title: "unsloth/Mistral-Large-4-1T-A52B-GGUF at main"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["unsloth", "mistral-large-4-1t-a52b-gguf", "llama", "install", "ud-q4", "model", "server", "local"]
source: https://huggingface.co/unsloth/Mistral-Large-4-1T-A52B-GGUF/tree/main
source_anchor: ""
source_lines: [1, 85]
section: "Mistral"
---
- Libraries
-  Transformers How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with Transformers: ```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModel
model = AutoModel.from_pretrained("unsloth/Mistral-Large-4-1T-A52B-GGUF", device_map="auto")
```
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  llama.cpp  How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with llama.cpp: ##### Install (macOS, Linux)curl -LsSf https://llama.app/install.sh | sh
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL
# Run inference directly in the terminal:
llama cli -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Install from WinGet (Windows)winget install llama.cpp
# Start a local OpenAI-compatible server with a web UI:
llama serve -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL
# Run inference directly in the terminal:
llama cli -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Build from source codegit clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
cmake -B build
cmake --build build -j --target llama-app
# Start a local OpenAI-compatible server with a web UI:
./build/bin/llama serve -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL
# Run inference directly in the terminal:
./build/bin/llama cli -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Use Dockerdocker model run hf.co/unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL
-  Llama new 
-  LM Studio  
-  Jan  
-  Ollama  How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with Ollama: ollama run hf.co/unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL
-  Unsloth Desktop  
-  Pi  How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with Pi: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Configure the model in Pi```
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
          "id": "unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL"
        }
      ]
    }
  }
}
```
##### Run Pi# Start Pi in your project directory:
pi
-  Docker Model Runner  How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with Docker Model Runner: docker model run hf.co/unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL
-  Lemonade  How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with Lemonade: ##### Pull the model# Download Lemonade from https://lemonade-server.ai/
lemonade pull unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Run and chat with the modellemonade run user.Mistral-Large-4-1T-A52B-GGUF-UD-Q4_K_XL ##### List all available modelslemonade list
-  Hermes Agent  How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with Hermes Agent: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Configure Hermes# Install Hermes:
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
hermes setup
# Point Hermes at the local server:
hermes config set model.provider custom
hermes config set model.base_url http://127.0.0.1:9931/v1
hermes config set model.default unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Run Hermeshermes
-  Atomic Chat  
-  OpenClaw  How to use unsloth/Mistral-Large-4-1T-A52B-GGUF with OpenClaw: ##### Start the llama.cpp server# Install llama.cpp:
brew install llama.cpp
# Start a local OpenAI-compatible server:
llama serve -hf unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL ##### Configure OpenClaw# Install OpenClaw:
npm install -g openclaw@latest
# Register the local server and set it as the default model:
openclaw onboard --non-interactive --mode local \
  --auth-choice custom-api-key \
  --custom-base-url http://127.0.0.1:9931/v1 \
  --custom-model-id "unsloth/Mistral-Large-4-1T-A52B-GGUF:UD-Q4_K_XL" \
  --custom-provider-id llama-cpp \
  --custom-compatibility openai \
  --custom-text-input \
  --accept-risk \
  --skip-health ##### Run OpenClawopenclaw agent --local --agent main --message "Hello from Hugging Face"
