---
id: collect-261121-veille-ia/deepseek/deepseek-ai-deepseek-v4-1-flash-1017-discussions-9cb60918
title: "Deepseek ai deepseek v4 1 flash 1017 discussions"
domain: deepseek
role: reference
task: article
actors: ["DeepSeek"]
dates: []
keywords: ["deepseek-v4", "deepseek-ai", "flash-1017", "model", "image", "text", "type", "sglang"]
source: ""
source_anchor: ""
source_lines: [1, 306]
section: "DeepSeek"
---
Instructions to use deepseek-ai/DeepSeek-V4.1-Flash-1017 with libraries, inference providers, notebooks, and local apps. Follow these links to get started.
- Libraries
-  Transformers How to use deepseek-ai/DeepSeek-V4.1-Flash-1017 with Transformers: # Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("image-text-to-text", model="deepseek-ai/DeepSeek-V4.1-Flash-1017")
messages = [
    {
        "role": "user",
        "content": [
            {"type": "image", "url": "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/p-blog/candy.JPG"},
            {"type": "text", "text": "What animal is on the candy?"}
        ]
    },
]
pipe(text=messages)# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModelForCausalLM
model = AutoModelForCausalLM.from_pretrained("deepseek-ai/DeepSeek-V4.1-Flash-1017", device_map="auto")
- Inference
- HuggingChat
- Notebooks
- Google Colab
- Kaggle
- Local Apps Settings
-  vLLM  How to use deepseek-ai/DeepSeek-V4.1-Flash-1017 with vLLM: Install from pip and serve model# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "deepseek-ai/DeepSeek-V4.1-Flash-1017"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4.1-Flash-1017",
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
	}'Use Dockerdocker model run hf.co/deepseek-ai/DeepSeek-V4.1-Flash-1017
-  SGLang  How to use deepseek-ai/DeepSeek-V4.1-Flash-1017 with SGLang: Install from pip and serve model# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "deepseek-ai/DeepSeek-V4.1-Flash-1017" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4.1-Flash-1017",
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
	}'Use Docker imagesdocker run --gpus all \
    --shm-size 32g \
    -p 30000:30000 \
    -v ~/.cache/huggingface:/root/.cache/huggingface \
    --env "HF_TOKEN=<secret>" \
    --ipc=host \
    lmsysorg/sglang:latest \
    python3 -m sglang.launch_server \
        --model-path "deepseek-ai/DeepSeek-V4.1-Flash-1017" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4.1-Flash-1017",
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
-  Docker Model Runner  How to use deepseek-ai/DeepSeek-V4.1-Flash-1017 with Docker Model Runner: docker model run hf.co/deepseek-ai/DeepSeek-V4.1-Flash-1017
Football
 1
#84 opened 2 days ago
		by    Fredyco80 
deepseek-v4.1-flash emits tool calls with Chinese parameter keys ("文件" instead of "file_text"), causing destructive tool-call loops in agentic workflows
#83 opened 4 days ago
		by    OpticatModem 
【模型反馈】DeepSeek V4.1 Flash 1017 在复杂项目中存在跨模块回归分析不足的问题
#82 opened 4 days ago
		by    OrekiDawson 
myds: Native Apple Silicon Metal engine for DeepSeek-V4.1-Flash-1017 on 192 GB Mac (~734 t/s prefill, ~49.4 t/s decode, 256K+ context & Vision)
#81 opened 6 days ago
		by    aaa2015 
deepseek-ai/DeepSeek-V4.1-Flash-1017 speed on Mac
#80 opened 8 days ago
		by    konstantinnikol 
DeepSeek V4.1 Flash 1017 Successfully on 3 X RTX Pro 6000D 84GB + 1 x RTX Pro 6000 Workstation Edition 96GB (Huge saving using 6000D)
🚀👍 3
  1
#79 opened 11 days ago
		by    crystech 
DeepSeek V4.1 Flash 1017: Apple Silicon local-serving report with exact checkpoint
 1
#78 opened 15 days ago
		by    Raullen 
Add OmniCompass-VL evaluation result
#77 opened 17 days ago
		by    VCLab-PolyU 
Add CWE-bench evaluation result
 1
#76 opened 18 days ago
		by    SaylorTwift 
Can I use it
#74 opened 19 days ago
		by    Luxmindset 
SWISS Air Kundenservice Schweiz – Telefonnummer, Umbuchung & Stornierung Leitfaden
#54 opened 21 days ago
		by    ytghb 
☎️(Wizz Air infolinia kontakt telefon-Polska +48 22 292 29 29 | Telefon Infolinii
#53 opened 21 days ago
		by    Azdfhst756 
Avianca Airlines teléfono Costa Rica: contacto oficial, reservaciones y asistencia al viajero
#45 opened 21 days ago
		by    Kallo098 
Add Terminal-Bench evaluation results
#40 opened 22 days ago
		by    nielsr 
Add a response template
 3
 #39 opened 24 days ago
		by    Rocketknight1 
DeepSeek-V4.1-Flash-GSQ on one RX 7900 XT vRAM 20 GB + sysRAM 31.7 GiB: 121–127 tok/s prefill, 16 tok/s decode at 6K, 23 s for a 648-token turn
#38 opened 25 days ago
		by    SkyIsNotGreen 
[Bug]: Persian half-spaces (U+200C) are removed from DeepSeek-V4.1-Flash-1017 responses when using tools
 1
#37 opened 25 days ago
		by    Alireza7368 
给训练MOE的神们一个idea
 1
#36 opened 25 days ago
		by    t6am3 
Create Gtvh
#35 opened 27 days ago
		by    Sqlinjection1 
I sure do love my on-device local 700B model on my iphone from 1987
👀 1
 #34 opened 28 days ago
		by    erorajdm 
短请求的乱码问题--Non-thinking mode corrupts replies to short greetings when a system prompt is present (reproduces on the official API; ~47% of cases)
➕👀 7
  2
#33 opened 29 days ago
		by    JunFengTu 
Serving note: Engram tables and DSpark drafter off-node (2x DGX Spark + x86 host), measured
#32 opened 29 days ago
		by    ProprietaryLegal 
excessive consumption
 1
#31 opened 30 days ago
		by    xdjc 
Add evaluation results
#30 opened about 1 month ago
		by    SaylorTwift 
DeepSeek-V4.1-Flash-1017 想的太多 （如：会把“描述/回答”类请求过度升级成完整排查任务）
👀 1
  2
#29 opened about 1 month ago
		by    wudaolj 
GOAT Model
❤️ 1
 #28 opened about 1 month ago
		by    calycekr 
Some compressed reasoning traces in training dataset
 1
#26 opened about 1 month ago
		by    inikishev 
To Deepseek Sect.
👍 1
 #25 opened about 1 month ago
		by    nonmairub 
Hey DeepSeek!
➕ 1
 #24 opened about 1 month ago
		by    GGUFGuy 
Smaller model?
➕ 1
  1
#23 opened about 1 month ago
		by    CyborgPaloma 
최고임 감사합니다
👍 2
 #22 opened about 1 month ago
		by    Inah0309 
Actually the best LLM I've used to this day
👍 6
  1
#21 opened about 1 month ago
		by    jreoka 
I make a patch on Sglang v0.5.22 and successfully run the model on 4xPro6000
 4
#20 opened about 1 month ago
		by    Scott309 
wow, this thing is a beast - and i've NEVER said that about a deepseek model
👍 6
  1
#19 opened about 1 month ago
		by    aaron-newsome 
Which harness used for Agents' Last Exam and Apex Bench v1.1 ? | Agents' Last Exam 和 Apex Bench v1.1 评测使用了哪些 Agent Harness
 1
#18 opened about 1 month ago
		by    pandemo 
Deepseek rvk
#17 opened about 1 month ago
		by    iosrvk0 
🚀实测DeepSeek V4.1视频！6亿Token只花了30元
🔥 2
 #15 opened about 1 month ago
		by    leo009 
take my talents to deepseek
🤯 2
  1
#14 opened about 1 month ago
		by    CAERKKK 
Video of Step-by-Step Review and Testing
🔥 1
 #13 opened about 1 month ago
		by    fahdmirzac 
好奇，为什么v4.1-flash-1017还是挂在了v4的合集中
#12 opened about 1 month ago
		by    gl12b 
复用 DeepSeek-V4.1-Flash 的 chat_template.jinja 嘛？
👍 1
  2
#10 opened about 1 month ago
		by    WilliamKyle 
When is the v4.1-Pro version being released?
 4
#9 opened about 1 month ago
		by    testamentaddress01 
I LOVE DEEPSEEK
🚀 11
  5
#8 opened about 1 month ago
		by    adamm-hf 
为啥评论区人这么少？
 1
#7 opened about 1 month ago
		by    abing7k 
啊不是，今天不是周六吗？ds你不对劲了（狗头
 1
#6 opened about 1 month ago
		by    goodleaner 
在商务部业务系统统一平台技术贸易管理信息应用登记了吗？
 8
#5 opened about 1 month ago
		by    RainPPR 
毫无预兆？？？？
 1
#4 opened about 1 month ago
		by    quanWangg 
!⸮强强?!
❤️ 3
  1
#3 opened about 1 month ago
		by    TQSY 
Did you know that you can run this model with 24 GB of VRAM and 64 GB of RAM?
🔥 7
  18
#2 opened about 1 month ago
		by    lucyferorg
