<!-- source: https://huggingface.co/poolside/Laguna-S-2.1-GGUF -->

**Use on OpenRouter** ·
  **Use on Vercel AI Gateway** ·
  **Release blog post**

GGUF conversions of Laguna S 2.1 for llama.cpp, plus the DFlash speculative-decoding draft model. See the base model card for architecture details, license, and usage guidance.

| File | Size | Notes | 
|---|---|---|
| `laguna-s-2.1-F16.gguf` | 235 GB | full precision | 
| `laguna-s-2.1-Q8_0.gguf` | 129 GB | routed experts Q8_0, signal path (attention, shared experts, embeddings) kept BF16 | 
| `laguna-s-2.1-Q4_K_M.gguf` | 68 GB | routed experts Q4_K (imatrix), signal path kept Q8_0 | 
| `laguna-s-2.1-DFlash-BF16.gguf` | 2.2 GB | DFlash drafter for speculative decoding | 
| `laguna-s-2.1.imatrix` | 0.4 GB | importance matrix used for the K-quants | 

Serve with Poolside's llama.cpp fork, branch
`laguna`, which carries full
Laguna support including DFlash speculative decoding. (Base Laguna support is also
in upstream review: ggml-org/llama.cpp#25165.)

```
git clone --branch laguna https://github.com/poolsideai/llama.cpp
cd llama.cpp && cmake -B build && cmake --build build -j
./build/bin/llama-server -m laguna-s-2.1-Q4_K_M.gguf --jinja --port 8000
# with DFlash speculative decoding:
./build/bin/llama-server -m laguna-s-2.1-Q4_K_M.gguf \
  -md laguna-s-2.1-DFlash-BF16.gguf \
  --spec-type draft-dflash --spec-draft-n-max 15 \
  --spec-draft-override-tensor '.*=CUDA0' \
  -fa on --jinja --port 8000
```
These GGUFs ship configured for a 262,144-token (256K) context window. This is the configuration we recommend for best output quality.

The weights are native 1M checkpoints: training included a long-context extension stage up to 1,048,576 tokens. To use more than 256K of context with llama.cpp, override the rope configuration at load time:

```
--ctx-size 1048576 --rope-scaling yarn --rope-scale 128 --yarn-orig-ctx 8192
```
You may experience quality degradation with the 1M configuration. If you use it, we recommend sampling with `--temp 0.7 --top-p 0.95`.

This release also corrects the embedded `yarn_attn_factor` metadata (now 1.0; llama.cpp derives the YaRN attention scaling internally).

- Downloads last month
- 651,679