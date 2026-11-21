---
id: collect-261121-veille-ia/minimax/minimax-m3-1-flash-nvidia-nemo-automodel
title: "MiniMax-M3.1-Flash | NVIDIA NeMo AutoModel"
domain: minimax
role: reference
task: article
actors: ["MiniMax"]
dates: []
keywords: ["minimax-m3", "checkpoint", "model", "flash", "automodel", "nemo", "recipe", "weights"]
source: https://docs.nvidia.com/nemo/automodel/model-coverage/vision-language-models/MiniMaxAI/MiniMax-M3.1-Flash
source_anchor: ""
source_lines: [1, 27]
section: "MiniMax"
---
MiniMaxAI/MiniMax-M3.1-Flash is an update of MiniMax-M3, MiniMax AI’s Mixture-of-Experts vision-language model, optimized for agentic coding. NeMo AutoModel loads its mixed-precision checkpoint directly and fine-tunes it with the same model implementation as MiniMax-M3.

## Model Reference

### Model Architecture

## Checkpoint Loading

The released checkpoint stores routed-expert weights in packed NVFP4 (values, block scales, and global scales) and attention and indexer projections in MXFP8. With `checkpoint.dequantize_base_checkpoint: true`, AutoModel decodes these tensors into the training dtype while streaming them into sharded storage, so you do not need to convert the checkpoint first.

The recipe trains with BF16 parameters, FP32 router state, and Transformer Engine FusedAdam with FP32 master weights and moments. This is BF16 fine-tuning of the dequantized model, not NVFP4 training.

## Available Models

- **MiniMax-M3.1-Flash** - registered through the`MiniMaxM3SparseForConditionalGeneration` model class.

## Example HF Models

## Example Recipes

The recipe uses FSDP2 with HybridEP expert dispatch, FA4 attention, and activation checkpointing (TP1, CP1, PP1), and runs final validation on `validation[:256]`. Checkpoint saving is disabled.

The processor sets `size.shortest_edge: 3136` and `size.longest_edge: 451584`. These keep the checkpoint’s image pixel-area bounds under Transformers 5.18 and are not a fixed image width and height.

## Run the Recipe

The recipe needs 16 nodes with 8 GPUs each. See SLURM Cluster to submit it as a multi-node job. To log to Weights & Biases, set `wandb.enable: true` and fill in `wandb.project` and `wandb.entity`.
