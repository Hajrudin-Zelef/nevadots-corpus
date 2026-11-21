---
id: collect-261121-veille-ia/modeles-open/ling-3-1-flash-1019-same-setup-new-weight-shards
title: "Ling 3.1 Flash 1019: same setup, new weight shards"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["same", "shards", "file", "json", "ling-3", "chat", "setup", "files"]
source: https://huggingface.co/blog/alphabell/ling-3-1-1019-public-interface-diff
source_anchor: ""
source_lines: [1, 42]
section: "Modeles_Open"
---
Ling-3.1-flash-1019 reports higher agent scores than the earlier chat release. We compared their public repositories at fixed Hugging Face revisions without downloading the model weights. Eight setup, tokenizer and loader files match byte for byte. Every one of the 54 published weight shards has a different LFS SHA-256 hash.

The comparison is between Ling-3.1-flash at `276cca72` and Ling-3.1-flash-1019 at `dddfee4b`.

| Published file | Byte comparison | SHA-256 prefix | 
|---|---|---|
| config.json | Same | `7b1732fd23b932c2` | 
| chat_template.jinja | Same | `f9beb6cdd406923d` | 
| tokenizer_config.json | Same | `616f1e4ef8474790` | 
| tokenizer.json | Same | `40fb9d7d7795b8bd` | 
| special_tokens_map.json | Same | `69b63b9f81044ead` | 
| configuration_bailing_moe_v3.py | Same | `f2c048966aec8a2f` | 
| modeling_bailing_moe_v3.py | Same | `8722d34e4617db0d` | 
| model.safetensors.index.json | Same | `f01006f7a65eb4b3` | 

The `README.md` and `.gitattributes` files differ between releases; the eight runtime and setup files above do not.

The two repositories have the same 54 shard paths and file sizes, but **54 of 54 LFS hashes differ**. Hugging Face exposes those hashes in its revision-pinned metadata for the earlier release and 1019. A changed shard hash shows changed file bytes; it does not reveal which tensors changed.

You can repeat the shard check without downloading the checkpoints:

```
import json
from urllib.request import urlopen
releases = {
    "Ling-3.1-flash": "276cca72bbd8b95693032848bfd638460b529b8b",
    "Ling-3.1-flash-1019": "dddfee4bb43519a92761fbc5cefe3f0d3ff4aded",
}
shards = []
for name, revision in releases.items():
    url = f"https://huggingface.co/api/models/inclusionAI/{name}/revision/{revision}?blobs=true"
    info = json.load(urlopen(url))
    shards.append({file["rfilename"]: file["lfs"]["sha256"]
                   for file in info["siblings"] if file["rfilename"].endswith(".safetensors")})
assert shards[0].keys() == shards[1].keys()
print(sum(shards[0][path] != shards[1][path] for path in shards[0]), "of", len(shards[0]))
```
The separate Ling-3.1-flash-base is a foundation checkpoint for further training. Its published config, chat template and tokenizer files differ from the chat releases. The base config uses EOS ID 156892; both chat configs use 156895. Use the earlier chat checkpoint as the control for a paired agent test, and state a separate prompt protocol for any base-model study.

The 1019 model card reports an Artificial Analysis Intelligence Index rise from 41 to 44, AA-Briefcase from about 1400 to 1524 Elo, and AutomationBench-AA from 62% to 68%. The repository diff establishes changed weight files with unchanged published setup files. It does not identify a training intervention or explain those reported scores.

For a useful A/B, pin both revisions and one serving build. Keep the same agent scaffold, tasks, prompt serialization, sampling settings, hardware, context and output budgets. Record success per task, reasoning and output tokens, tool calls, wall time, failed runs and the actual answer. Publish paired traces alongside aggregate scores so users can see where 1019 behaves differently.
