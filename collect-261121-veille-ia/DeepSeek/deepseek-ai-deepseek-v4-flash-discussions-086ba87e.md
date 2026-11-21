---
id: collect-261121-veille-ia/deepseek/deepseek-ai-deepseek-v4-flash-discussions-086ba87e
title: "Load model directly"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek-v4-flash", "deepseek-ai", "opened", "model", "months", "sglang", "install", "server"]
source: ""
source_anchor: ""
source_lines: [1, 236]
section: "DeepSeek"
---
Instructions to use deepseek-ai/DeepSeek-V4-Flash with libraries, inference providers, notebooks, and local apps. Follow these links to get started.
- Libraries
-  Transformers How to use deepseek-ai/DeepSeek-V4-Flash with Transformers: # Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="deepseek-ai/DeepSeek-V4-Flash")# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-V4-Flash")
model = AutoModelForCausalLM.from_pretrained("deepseek-ai/DeepSeek-V4-Flash", device_map="auto")
- Inference
- HuggingChat
- Notebooks
- Google Colab
- Kaggle
- Local Apps Settings
-  vLLM  How to use deepseek-ai/DeepSeek-V4-Flash with vLLM: Install from pip and serve model# Install vLLM from pip:
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
	}'Use Dockerdocker model run hf.co/deepseek-ai/DeepSeek-V4-Flash
-  SGLang  How to use deepseek-ai/DeepSeek-V4-Flash with SGLang: Install from pip and serve model# Install SGLang from pip:
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
	}'Use Docker imagesdocker run --gpus all \
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
-  Docker Model Runner  How to use deepseek-ai/DeepSeek-V4-Flash with Docker Model Runner: docker model run hf.co/deepseek-ai/DeepSeek-V4-Flash
Technical requirements?
#61 opened 1 day ago
		by    kirkiarty 
Add community evaluation results for SWE-Bench Pro
#60 opened 19 days ago
		by    nielsr 
Add chat template and response template
#59 opened 24 days ago
		by    Rocketknight1 
Measured: DeepSeek-V4-Flash (UD-Q2_K_XL GGUF) on a 128 GB Strix Halo mini PC — single vs dual vs triple node
#58 opened 29 days ago
		by    geekom-ai-admin 
Add LEXam-hard evaluation result
#57 opened 2 months ago
		by    joelniklaus 
Add LHTB eval result: 2/46 tasks solved (90-min budget, estimated)
#56 opened 3 months ago
		by    zli12321 
Add community evaluation results for SWE-Bench Multilingual
#54 opened 3 months ago
		by    nielsr 
Add new version to metadata
#53 opened 4 months ago
		by    nielsr 
🚀DeepSeek V4 Flash实测视频
#52 opened 4 months ago
		by    leo009 
Update V4 flash ,pls!
😎 10
  12
#51 opened 4 months ago
		by    mcocdaa 
Delete inference/README.md
#50 opened 4 months ago
		by    Pq234 
Update README.md
#49 opened 4 months ago
		by    Pq234 
什么时候满血版v4，梁圣！
 2
#48 opened 4 months ago
		by    KakumuraArimura 
fastllm 启动deepseek-v4-flash 回答问题乱码
 1
#47 opened 4 months ago
		by    zll3053 
Run DeepSeek-V4-Flash locally! 🐳
❤️ 2
 #46 opened 5 months ago
		by    danielhanchen 
Add SkillsBench v1.1 evaluation result
#45 opened 5 months ago
		by    bingran-you 
Listed on OpenModelMap
#44 opened 5 months ago
		by    duola15 
Listed on OpenModelMap
#43 opened 5 months ago
		by    duola15 
Since the DSA indexer selects the top‑k tokens with respect to k, is it really necessary to multiply all tokens by a softmax_scale scalar?
#42 opened 5 months ago
		by    jinluyang 
Deepseek-v4-flash image support
🔥 3
  1
#41 opened 5 months ago
		by    songwang41 
DeepSeek-V4-Flash fine-tuning: can a merged model keep the same FP4+FP8 format as the base model, or is BF16 the only supported merge output?
 1
#40 opened 5 months ago
		by    zhangshuai121 
TemporalMesh Transformer: 29.4 PPL at 48% compute — beats Mamba, new open-source architecture
#38 opened 6 months ago
		by    vigneshwar234 
I'm confused (v4 Flash vs v3.2)
 4
#37 opened 6 months ago
		by    MateoTeo 
Number of layers in inference mock code
#36 opened 6 months ago
		by    NotTete 
🚀 ms-swift Provides DeepSeek-V4 Fine-tuning Practice
#35 opened 6 months ago
		by    study-hjt 
DeepSeek Training Support
#34 opened 6 months ago
		by    SuperXr 
官方是否有兴趣帮忙改进完善一下Apple MLX框架的适配？
#33 opened 6 months ago
		by    hehua2008 
DeepSeek-V4帮我破了《真探3》的案子
❤️ 3
  2
#32 opened 6 months ago
		by    Mephisto1484 
Add Claw-Eval evaluation results
#31 opened 6 months ago
		by    SaylorTwift 
use vLLM with 2 DGX Sparks , response broken
#29 opened 6 months ago
		by    easonchow0419 
Running models with vLLM on the RTX Pro 6000 - SM120
👀👍 2
  12
#28 opened 7 months ago
		by    liku2001 
model anwser with <ds_safety>
#27 opened 7 months ago
		by    rick-jiang 
Claude 4.7 Reasoning into DeepSeek-V4-Flash for Open-Source Edge Intelligence
🚀 1
 #26 opened 7 months ago
		by    muneebkhanniazi 
How to run deepseek on Ada GPUs？Mine is L20.
 5
#25 opened 7 months ago
		by    XiaoZaiyi 
How can I use this with llama.cpp?
➕ 2
  7
#24 opened 7 months ago
		by    KeilahElla 
Questions on MoE Hash Routing
#22 opened 7 months ago
		by    mattduerrmeier 
120B model?
👍 5
  2
#21 opened 7 months ago
		by    jacek2024 
Run DeepSeek-V4-Flash on more hardware: FP8/BF16 adapted versions for 8 AI chips (ready to download)
👍❤️ 2
 #20 opened 7 months ago
		by    Yonghua 
[适配请求] 建议 API 兼容 OpenAI 新规范中的 developer 角色 （It is recommended that the API be compatible with the developer role in the new OpenAI specification
 1
#19 opened 7 months ago
		by    xusu616 
Update README.md to add SGLang deployment option
#18 opened 7 months ago
		by    zjxia 
Is 158B or 284b params ?
 6
#17 opened 7 months ago
		by    celsowm 
Unable to run on 2x RTX Pro 6000 (DEEP_GEMM problem)
➕ 10
  17
#15 opened 7 months ago
		by    stev236 
Local Installation Video and Testing - Step by Step
👍😔 4
 #13 opened 7 months ago
		by    fahdmirzac 
Too big to run locally.
🤯👍 12
  21
#12 opened 7 months ago
		by    Dampfinchen 
Add community evaluation results for GPQA, MMLU-PRO, SWE-BENCH_VERIFIED, HLE, TERMINAL-BENCH-2.0
 1
#11 opened 7 months ago
		by    nielsr 
May I ask if there is a deployment document?
 2
#10 opened 7 months ago
		by    jerryliujiawei 
Will there be a smaller model like Qwen3.5 122 or Nemotron 3 super
➕ 7
  7
#9 opened 7 months ago
		by    mayankiit04
