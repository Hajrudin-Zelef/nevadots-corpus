---
id: collect-261121-veille-ia/tencent-hunyuan/tencent-hy4-flash-at-main
title: "tencent/Hy4-flash at main"
domain: tencent-hunyuan
role: reference
task: article
actors: []
dates: []
keywords: ["tencent", "hy4-flash", "model", "image", "text", "type", "sglang", "install"]
source: https://huggingface.co/tencent/Hy4-flash/tree/main
source_anchor: ""
source_lines: [1, 125]
section: "Tencent_Hunyuan"
---
- Libraries
-  Transformers How to use tencent/Hy4-flash with Transformers: ```
# Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("image-text-to-text", model="tencent/Hy4-flash")
messages = [
    {
        "role": "user",
        "content": [
            {"type": "image", "url": "https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/p-blog/candy.JPG"},
            {"type": "text", "text": "What animal is on the candy?"}
        ]
    },
]
pipe(text=messages)
```
```
# pip install -U transformers accelerate
# Load model directly
from transformers import AutoModelForSeq2SeqLM
model = AutoModelForSeq2SeqLM.from_pretrained("tencent/Hy4-flash", device_map="auto")
```
- Notebooks
-  Google Colab 
-  Kaggle 
- Local Apps Settings
-  vLLM  How to use tencent/Hy4-flash with vLLM: ##### Install from pip and serve model```
# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "tencent/Hy4-flash"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "tencent/Hy4-flash",
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
##### Use Dockerdocker model run hf.co/tencent/Hy4-flash
-  SGLang  How to use tencent/Hy4-flash with SGLang: ##### Install from pip and serve model```
# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "tencent/Hy4-flash" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "tencent/Hy4-flash",
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
        --model-path "tencent/Hy4-flash" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/chat/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "tencent/Hy4-flash",
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
-  Docker Model Runner  How to use tencent/Hy4-flash with Docker Model Runner: docker model run hf.co/tencent/Hy4-flash
