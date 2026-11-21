---
id: collect-261121-veille-ia/docs-techniques/systemcards-org-5
title: "systemcards.org"
domain: docs-techniques
role: reference
task: article
actors: []
dates: ["2026-11-21"]
keywords: ["card", "text", "archived", "copy", "model", "version", "change", "release"]
source: https://cards.douwmarx.com/docs/deepseek-deepseek-v4-pro-0813-model-card
source_anchor: ""
source_lines: [1, 213]
section: "Docs_Techniques"
---
## DeepSeek-V4-Pro-0813 Model Card

### Version history

| Fetched | Content type | Size | Fingerprint | Original | What changed | 
|---|---|---|---|---|---|
| 2026-11-21T06:21:30Z | text/html | 291,092 | `239dc14244ab1de7` | archived copy | — | 
| 2026-10-06T06:22:57Z | text/html | 267,079 | `849e4e3140d441aa` | archived copy | — | 
| 2026-09-27T11:26:27Z | text/html | 265,428 | `19b5b3c14a84e615` | archived copy | Extractor drift: 1 line(s) missing from the extracted text are still present in the raw capture; not a publisher deletion. | 
| 2026-09-09T06:19:02Z | text/html | 271,533 | `22822cd3034ab6b4` | archived copy | — | 
| 2026-08-27T06:18:23Z | text/html | 260,819 | `fe6ddcb58fd3fb61` | archived copy | Hugging Face card now declares an Evaluation results entry for Terminal-Bench 2.1 with a leaderboard link, and newly shows model tree, Spaces (5), collection, and linked paper sections. Download counter change is noise. | 
| 2026-08-20T06:20:09Z | text/html | 244,305 | `1207dec759d4460f` | archived copy | No substantive change detected (page furniture or extraction noise). | 
| 2026-08-14T07:04:53Z | text/html | 218,671 | `97d7786fcf3bc280` | archived copy | initial version | 

### Provenance

## Show 11 changelog entries (when, how, and why this entry changed)

1. 
        2026-08-14T07:04:53Z
        **add** run 2026-08-14T07:00Z-local · agent · lead: agent_search
          First-party Hugging Face model card for DeepSeek-V4-Pro-0813, the GA release of DeepSeek-V4-Pro announced the same day on DeepSeek's own news feed (api-docs.deepseek.com/news/news260813). The card states it supersedes the preview version, is built on the V4-Pro preview structure with a DSpark speculative decoding module attached, and reports a ten-benchmark comparison table (HLE, Terminal Bench 2.1, NL2Repo, Cybergym, DeepSWE, Toolathlon-Verified, Agents' Last Exam, AutomationBench, DSBench-FullStack/Hard) against V4-Flash-0731, both preview models, GLM-5.2, Kimi K3, Opus-4.8 and Fable-5. This is a distinct GA release rather than a quantization or size variant, and it follows the corpus precedent of cataloguing DeepSeek-V4-Flash-0731 separately from the April V4-Pro/V4-Flash preview card. has_safety_evals is false and honest: the card contains no safety, red-team, refusal, misuse or risk content - Cybergym appears purely as a capability score - so an open-weight MIT-licensed frontier release ships with capability benchmarks only.Criteria: document_retrievable=pass; min_publication_date=pass; publisher_on_allowlist=pass · attested about_a_specific_model_or_eval=true · attested distinct_model_release=true · attested notable_release=true · attested primary_source=true
2. 
        2026-08-20T06:20:09Z
        **new_version** run 2026-08-20T06:18Z-local · monitor · lead: monitor
          Content fingerprint changed on scheduled re-fetch
3. 
        2026-08-27T06:18:23Z
        **new_version** run 2026-08-27T06:17Z-local · monitor · lead: monitor
          Content fingerprint changed on scheduled re-fetch
4. 
        2026-08-31T10:09:59Z
        **field_update** run manual-20260831T100959Z · human
          related_urls: [] → [{"kind": "other", "note": "Superseded DeepSeek-V4-Pro preview card (2026-04-24), catalogued as its own row", "url": "https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro"}]Structured related-resources backfill: companion URLs previously stashed in free-text notes, verified live 2026-08-31.
5. 
        2026-08-31T10:15:48Z
        **field_update** run manual-20260831T101548Z · human
          change_summary: null → "Hugging Face card now declares an Evaluation results entry for Terminal-Bench 2.1 with a leaderboard link, and newly shows model tree, Spaces (5), collection, and linked paper sections. Download counter change is noise."Change-summary backfill (2026-08-31): drafted from the stored version diff.
6. 
        2026-08-31T10:15:57Z
        **field_update** run manual-20260831T101557Z · human
          change_summary: null → "No substantive change detected (page furniture or extraction noise)."Change-summary backfill (2026-08-31): drafted from the stored version diff.
7. 
        2026-09-09T06:19:02Z
        **new_version** run 2026-09-09T06:17Z-local · monitor · lead: monitor
          Content fingerprint changed on scheduled re-fetch
8. 
        2026-09-27T11:26:27Z
        **new_version** run 2026-09-27T11:24Z-local · monitor · lead: monitor
          Content fingerprint changed on scheduled re-fetch
9. 
        2026-09-27T11:26:27Z
        **field_update** run 2026-09-27T11:24Z-local · monitor
          change_summary: null → "Extractor drift: 1 line(s) missing from the extracted text are still present in the raw capture; not a publisher deletion."Deleted lines found verbatim in the new raw capture
10. 
        2026-10-06T06:22:57Z
        **new_version** run 2026-10-06T06:20Z-local · monitor · lead: monitor
          Content fingerprint changed on scheduled re-fetch
11. 
        2026-11-21T06:21:30Z
        **new_version** run 2026-11-21T06:19Z-local · monitor · lead: monitor
          Content fingerprint changed on scheduled re-fetch

### Extracted text

## Show extracted text (from the latest fetched version; formatting is lossy — use the original for reading)

```
Instructions to use deepseek-ai/DeepSeek-V4-Pro-0813 with libraries, inference providers, notebooks, and local apps. Follow these links to get started.
- Libraries
-  Transformers How to use deepseek-ai/DeepSeek-V4-Pro-0813 with Transformers: # Use a pipeline as a high-level helper
from transformers import pipeline
pipe = pipeline("text-generation", model="deepseek-ai/DeepSeek-V4-Pro-0813")# pip install -U transformers accelerate
# Load model directly
from transformers import AutoTokenizer, AutoModelForCausalLM
tokenizer = AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-V4-Pro-0813")
model = AutoModelForCausalLM.from_pretrained("deepseek-ai/DeepSeek-V4-Pro-0813", device_map="auto")
- Inference
- HuggingChat
- Notebooks
- Google Colab
- Kaggle
- Local Apps Settings
-  vLLM  How to use deepseek-ai/DeepSeek-V4-Pro-0813 with vLLM: Install from pip and serve model# Install vLLM from pip:
pip install vllm
# Start the vLLM server:
vllm serve "deepseek-ai/DeepSeek-V4-Pro-0813"
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:8000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Pro-0813",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
	}'Use Dockerdocker model run hf.co/deepseek-ai/DeepSeek-V4-Pro-0813
-  SGLang  How to use deepseek-ai/DeepSeek-V4-Pro-0813 with SGLang: Install from pip and serve model# Install SGLang from pip:
pip install sglang
# Start the SGLang server:
python3 -m sglang.launch_server \
    --model-path "deepseek-ai/DeepSeek-V4-Pro-0813" \
    --host 0.0.0.0 \
    --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Pro-0813",
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
        --model-path "deepseek-ai/DeepSeek-V4-Pro-0813" \
        --host 0.0.0.0 \
        --port 30000
# Call the server using curl (OpenAI-compatible API):
curl -X POST "http://localhost:30000/v1/completions" \
	-H "Content-Type: application/json" \
	--data '{
		"model": "deepseek-ai/DeepSeek-V4-Pro-0813",
		"prompt": "Once upon a time,",
		"max_tokens": 512,
		"temperature": 0.5
	}'
-  Docker Model Runner  How to use deepseek-ai/DeepSeek-V4-Pro-0813 with Docker Model Runner: docker model run hf.co/deepseek-ai/DeepSeek-V4-Pro-0813
DeepSeek-V4-Pro-0813
Introduction
DeepSeek-V4-Pro-0813 is the official release of DeepSeek-V4-Pro, superseding the preview version, with greatly enhanced agentic capabilities and performance improvements that are especially pronounced in production environments. It is built on the DeepSeek-V4-Pro (Preview) model structure, with a DSpark speculative decoding module attached.
DeepSeek-V4-Pro-0813 outperforms DeepSeek-V4-Pro (Preview) on the benchmarks listed below, and is broadly competitive with the strongest proprietary models available.
| Benchmark | DeepSeek-V4-Pro-0813 | DeepSeek-V4-Flash-0731 | DeepSeek-V4-Pro (Preview) | DeepSeek-V4-Flash (Preview) | GLM-5.2 | Kimi K3 | Opus-4.8 | Fable-5 (w/ fallback) |
|---|---|---|---|---|---|---|---|---|
| HLE (wo / w tools) | 42.7 / 60.0 | 37.8 / 51.5 | 37.7 / 48.2 | 34.8 / 45.1 | 40.5 / 54.7 | 43.5 / 56.0 | 49.8 / 57.9 | 53.3 / 63.0 |
| Terminal Bench 2.1 | 87.9 | 82.7 | 72.1 | 61.8 | 81.0 | 88.3 | 85.0 | 88.0 |
| NL2Repo | 61.5 | 54.2 | 38.5 | 39.4 | 48.9 | - | 69.7 | - |
| Cybergym | 83.3 | 76.7 | 52.7 | 38.7 | - | 80.0 | 78.3 | 83.1 |
| DeepSWE | 62.7 | 54.4 | 12.8 | 7.3 | 46.2 | 67.5 | 58.0 | 70.0 |
| Toolathlon-Verified | 74.1 | 70.3 | 55.9 | 49.7 | 59.9 | 76.5 | 76.2 | 77.9 |
| Agents' Last Exam | 25.7 | 25.2 | 16.5 | 15.8 | 23.8 | 27.6 | 25.7 | - |
| AutomationBench (Public) | 31.8 | 25.1 | 12.8 | 10.8 | 12.9 | 30.8 | 27.2 | 29.1 |
| DSBench-FullStack † | 71.1 | 68.7 | 41.8 | 37.0 | 61.8 | 73.7 | 71.6 | 77.2 |
| DSBench-Hard † | 67.2 | 59.6 | 31.1 | 25.8 | 54.5 | 63.0 | 71.7 | 68.3 |
Notes:
- For the code-agent tasks among the public benchmarks above, DeepSeek-V4-Pro-0813 is evaluated with the minimal mode of DeepSeek Harness as the agent framework, using the max reasoning effort level withtemperature = 1.0, top_p = 0.95 .
- † DSBench-FullStack is an internal full-stack development test set; DSBench-Hard is an internal test set of difficult coding-agent problems.
Chat Template
This release does not include a Jinja-format chat template. Instead, we provide a dedicated encoding folder with Python scripts and test cases demonstrating how to encode messages in OpenAI-compatible format into input strings for the model, and how to parse the model's text output. Please refer to the encoding folder for full documentation.
The reasoning_effort parameter now supports three levels — low, high, and max — which control how much deliberation the model spends before answering.
A brief example:
from encoding_dsv4 import encode_messages, parse_message_from_completion_text
messages = [
    {"role": "user", "content": "hello"},
    {"role": "assistant", "content": "Hello! I am DeepSeek.", "reasoning_content": "thinking..."},
    {"role": "user", "content": "1+1=?"}
]
# messages -> string
prompt = encode_messages(messages, thinking_mode="thinking", reasoning_effort="max")
# string -> tokens
import transformers
tokenizer = transformers.AutoTokenizer.from_pretrained("deepseek-ai/DeepSeek-V4-Pro-0813")
tokens = tokenizer.encode(prompt)
How to Run with vLLM
DSpark speculative decoding is enabled with a single flag — add --speculative-config with method: dspark to your vLLM launch command:
--speculative-config '{"method":"dspark","num_speculative_tokens":7,"draft_sample_method":"greedy"}'
For example, the command below serves the model with vLLM on a single 4×GB300 node. See the vLLM recipe for detailed instructions and other hardware configurations.
vllm serve deepseek-ai/DeepSeek-V4-Pro-0813 \
  --trust-remote-code --kv-cache-dtype fp8 --block-size 256 \
  --data-parallel-size 4 --enable-expert-parallel \
  --moe-backend deep_gemm_mega_moe \
  --attention-config '{"use_fp4_indexer_cache": true}' \
  --speculative-config '{"method":"dspark","num_speculative_tokens":7,"draft_sample_method":"greedy"}'
How to Run with SGLang
Enable DSpark with --speculative-algorithm DSPARK and do not set a separate --speculative-draft-model-path as the target and draft weights therefore come from the same checkpoint.
See the SGLang cookbook for detailed instructions, benchmarks and other hardwares configurations.
sglang serve \
  --trust-remote-code \
  --model-path deepseek-ai/DeepSeek-V4-Pro-0813 \
  --tp 4 \
  --moe-runner-backend flashinfer_mxfp4 \
  --speculative-algorithm DSPARK \
  --mem-fraction-static 0.90 \
  --chunked-prefill-size 4096 \
  --swa-full-tokens-ratio 0.1 \
How to Run Locally
Please refer to the inference folder for detailed instructions on running DeepSeek-V4 locally, including model weight conversion and interactive chat demos.
For local deployment, we recommend setting the sampling parameters to temperature = 1.0, with top_p = 0.95 for agentic scenarios and top_p = 1.0 otherwise. For the high and max reasoning effort levels, we recommend a maximum output length of 384K tokens.
License
This repository and the model weights are licensed under the MIT License.
Citation
@misc{deepseekai2026deepseekv4,
      title={DeepSeek-V4: Towards Highly Efficient Million-Token Context Intelligence},
      author={DeepSeek-AI},
      year={2026},
}
Contact
If you have any questions, please raise an issue or contact us at [email protected].
- Downloads last month
- 43,223
Model tree for deepseek-ai/DeepSeek-V4-Pro-0813
Spaces using deepseek-ai/DeepSeek-V4-Pro-0813 20
Collection including deepseek-ai/DeepSeek-V4-Pro-0813
Paper for deepseek-ai/DeepSeek-V4-Pro-0813
Evaluation results
- datacurve/deep-swe · Deep Swe View evaluation results    leaderboard   62.7 *
- hkust-nlp/Toolathlon · Toolathlon Verified View evaluation results      74.1
- harborframework/terminal-bench-2.1 · Terminalbench 2 1 View evaluation results    leaderboard   87.9 *
```
