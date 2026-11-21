---
id: collect-261121-veille-ia/liquid-lfm/lfm3-hugging-face
title: "LFM3 · Hugging Face"
domain: liquid-lfm
role: reference
task: article
actors: ["Liquid AI"]
dates: ["2026-11-05"]
keywords: ["none", "attention", "model", "layers", "heads", "bool", "size", "hidden"]
source: https://huggingface.co/docs/transformers/main/en/model_doc/lfm3
source_anchor: ""
source_lines: [1, 240]
section: "Liquid_LFM"
---
Transformers documentation

LFM3

*This model was contributed to Hugging Face Transformers on 2026-11-05.*

LFM3 is the third generation of Liquid Foundation Models by Liquid AI. Like LFM2 it is a hybrid of gated short convolutions and grouped-query attention, here split between full-attention and sliding-window layers.

LFM3 stacks three kinds of layers: gated short-convolution layers, full-attention layers and sliding-window attention layers. The attention layers apply an RMSNorm to the query and key heads and gate their output with a sigmoid per head. Full-attention layers use partial rotary embeddings, sliding-window layers use their own RoPE base, and every layer applies a sandwich norm to the operator and feed-forward outputs before the residual addition.

The short convolution uses the Hub `causal-conv1d` kernels with `use_kernels=True`, which also keep packed sequences
apart via `seq_idx` (padding-free training). Without kernels it falls back to PyTorch.

The following example shows how to generate an answer using the `AutoModelForCausalLM` class.

```
from transformers import AutoModelForCausalLM, AutoTokenizer
# Load model and tokenizer
model_id = "LiquidAI/LFM3-1.7B"
model = AutoModelForCausalLM.from_pretrained(model_id, device_map="auto")
tokenizer = AutoTokenizer.from_pretrained(model_id)
# Generate answer
prompt = "What is C. elegans?"
inputs = tokenizer.apply_chat_template(
    [{"role": "user", "content": prompt}],
    add_generation_prompt=True,
    return_tensors="pt",
    tokenize=True,
).to(model.device)
output = model.generate(**inputs, max_new_tokens=512)
print(tokenizer.decode(output[0], skip_special_tokens=False))
```
### class transformers.Lfm3Config

< source >
( transformers_version: str | None = Nonearchitectures: list[str] | None = Noneoutput_hidden_states: bool | None = Falsereturn_dict: bool | None = Truedtype: str | torch.dtype | None = Nonechunk_size_feed_forward: int = 0is_encoder_decoder: bool = Falseid2label: dict[int, str] | dict[str, str] | None = Nonelabel2id: dict[str, int] | dict[str, str] | None = Noneproblem_type: Literal['regression', 'single_label_classification', 'multi_label_classification'] | None = Nonevocab_size: int = 128000hidden_size: int = 2048intermediate_size: int = 7680num_hidden_layers: int = 24num_attention_heads: int = 16num_key_value_heads: int = 4max_position_embeddings: int = 128000initializer_range: float = 0.02norm_eps: float = 1e-05use_cache: bool = Truepad_token_id: int | None = 124893bos_token_id: int | None = 124894eos_token_id: int | list[int] | None = 124900tie_word_embeddings: bool = Truerope_parameters: dict | None = Noneconv_bias: bool = Falseconv_L_cache: int = 7layer_types: list[str] | None = Nonehead_dim: int = 128sliding_window: int | None = 256use_sandwich_norm: bool = Trueuse_attention_gate: bool = Trueattention_bias: bool = Falseattention_dropout: float = 0.0 )

Parameters

-  **vocab_size** (`int` ,*optional* , defaults to`128000` ) —
Vocabulary size of the model. Defines the number of different tokens that can be represented by the`input_ids` .
-  **hidden_size** (`int` ,*optional* , defaults to`2048` ) —
Dimension of the hidden representations.
-  **intermediate_size** (`int` ,*optional* , defaults to`7680` ) —
Dimension of the MLP representations.
-  **num_hidden_layers** (`int` ,*optional* , defaults to`24` ) —
Number of hidden layers in the Transformer decoder.
-  **num_attention_heads** (`int` ,*optional* , defaults to`16` ) —
Number of attention heads for each attention layer in the Transformer decoder.
-  **num_key_value_heads** (`int` ,*optional* , defaults to`4` ) —
This is the number of key_value heads that should be used to implement Grouped Query Attention. If`num_key_value_heads=num_attention_heads` , the model will use Multi Head Attention (MHA), if`num_key_value_heads=1` the model will use Multi Query Attention (MQA) otherwise GQA is used. When
converting a multi-head checkpoint to a GQA checkpoint, each group key and value head should be constructed
by meanpooling all the original heads within that group. For more details, check out this
paper. If it is not specified, will default to`num_attention_heads` .
-  **max_position_embeddings** (`int` ,*optional* , defaults to`128000` ) —
The maximum sequence length that this model might ever be used with.
-  **initializer_range** (`float` ,*optional* , defaults to`0.02` ) —
The standard deviation of the truncated_normal_initializer for initializing all weight matrices.
-  **norm_eps** (`float` ,*optional* , defaults to`1e-05` ) —
The epsilon used by the layer normalization layers.
-  **use_cache** (`bool` ,*optional* , defaults to`True` ) —
Whether or not the model should return the last key/values attentions (not used by all models). Only
relevant if`config.is_decoder=True` or when the model is a decoder-only generative model.
-  **pad_token_id** (`int` ,*optional* , defaults to`124893` ) —
Token id used for padding in the vocabulary.
-  **bos_token_id** (`int` ,*optional* , defaults to`124894` ) —
Token id used for beginning-of-stream in the vocabulary.
-  **eos_token_id** (`Union[int, list[int]]` ,*optional* , defaults to`124900` ) —
Token id used for end-of-stream in the vocabulary.
-  **tie_word_embeddings** (`bool` ,*optional* , defaults to`True` ) —
Whether to tie weight embeddings according to model’s`tied_weights_keys` mapping.
-  **rope_parameters** (`dict` ,*optional* ) —
Dictionary containing the configuration parameters for the RoPE embeddings. The dictionary should contain
a value for`rope_theta` and optionally parameters used for scaling in case you want to use RoPE
with longer`max_position_embeddings` .
-  **conv_bias** (`bool` ,*optional* , defaults to`False` ) —
Whether to use bias in the conv layers.
-  **conv_L_cache** (`int` ,*optional* , defaults to 7) —
L_cache dim in the conv layers.
-  **layer_types** (`list[str]` ,*optional* ) —
A list that explicitly maps each layer index with its layer type. If not provided, it will be automatically
generated based on config values.
-  **head_dim** (`int` ,*optional* , defaults to`128` ) —
The attention head dimension. If None, it will default to hidden_size // num_attention_heads
-  **sliding_window** (`int` ,*optional* , defaults to`256` ) —
Sliding window attention window size. If`None` , no sliding window is applied.
-  **use_sandwich_norm** (`bool` ,*optional* , defaults to`True` ) —
Whether to apply an RMSNorm to the operator and feed-forward outputs before they enter the residual stream.
-  **use_attention_gate** (`bool` ,*optional* , defaults to`True` ) —
Whether the attention layers scale each head’s output by a sigmoid gate computed from the layer input.
-  **attention_bias** (`bool` ,*optional* , defaults to`False` ) —
Whether to use a bias in the query, key, value and output projection layers during self-attention.
-  **attention_dropout** (`float` ,*optional* , defaults to`0.0` ) —
The dropout ratio for the attention probabilities.

This is the configuration class to store the configuration of a Lfm3Model. It is used to instantiate a Lfm3 model according to the specified arguments, defining the model architecture. Instantiating a configuration with the defaults will yield a similar configuration to that of the LiquidAI/LFM3-1.7B

Configuration objects inherit from PreTrainedConfig and can be used to control the model outputs. Read the documentation from PreTrainedConfig for more information.

### class transformers.Lfm3Model

< source >
( config: Lfm3Config )

Parameters

-  **config** (Lfm3Config) —
Model configuration class with all the parameters of the model. Initializing with a config file does not
load the weights associated with the model, only the configuration. Check out the
from_pretrained() method to load the model weights.

The bare Lfm3 Model outputting raw hidden-states without any specific head on top.

This model inherits from PreTrainedModel. Check the superclass documentation for the generic methods the library implements for all its model (such as downloading or saving, resizing the input embeddings, pruning heads etc.)

This model is also a PyTorch torch.nn.Module subclass. Use it as a regular PyTorch Module and refer to the PyTorch documentation for all matter related to general usage and behavior.

#### forward

< source >
( input_ids: typing.Optional[torch.LongTensor] = Noneattention_mask: typing.Optional[torch.Tensor] = Noneposition_ids: typing.Optional[torch.LongTensor] = Nonecache: transformers.cache_utils.Cache | None = Noneinputs_embeds: typing.Optional[torch.FloatTensor] = Noneuse_cache: bool | None = None**kwargs: *<class 'transformers.utils.generic.TransformersKwargs'> ) → BaseModelOutputWithPast or `tuple(torch.Tensor)`

Parameters

-  **input_ids** (`torch.LongTensor` of shape`(batch_size, sequence_length)` ,*optional* ) —
Indices of input sequence tokens in the vocabulary. Padding will be ignored by default.Indices can be obtained using AutoTokenizer. See PreTrainedTokenizer.encode() and PreTrainedTokenizer. **call** () for details.
-  **attention_mask** (`torch.Tensor` of shape`(batch_size, sequence_length)` ,*optional* ) —
Mask to avoid performing attention on padding token indices. Mask values selected in`[0, 1]` :
  - 1 for tokens that are **not masked** ,
  - 0 for tokens that are **masked** .
- 1 for tokens that are 
-  **position_ids** (`torch.LongTensor` of shape`(batch_size, sequence_length)` ,*optional* ) —
Indices of positions of each input sequence tokens in the position embeddings. Selected in the range`[0, config.n_positions - 1]` .
-  **cache** (`~cache_utils.Cache` ,*optional* ) —
Pre-computed hidden-states (key and values in the self-attention blocks and in the cross-attention
blocks) that can be used to speed up sequential decoding. This typically consists in the`cache` returned by the model at a previous stage of decoding, when`use_cache=True` or`config.use_cache=True` .Only Cache instance is allowed as input, see our kv cache guide. If no `cache` are passed, DynamicCache will be initialized by default.The model will output the same cache format that is fed as input. If `cache` are used, the user is expected to input only unprocessed`input_ids` (those that don’t
have their past key value states given to this model) of shape`(batch_size, unprocessed_length)` instead of all`input_ids` of shape`(batch_size, sequence_length)` .
-  **inputs_embeds** (`torch.FloatTensor` of shape`(batch_size, sequence_length, hidden_size)` ,*optional* ) —
Optionally, instead of passing`input_ids` you can choose to directly pass an embedded representation. This
is useful if you want more control over how to convert`input_ids` indices into associated vectors than the
model’s internal embedding lookup matrix.
-  **use_cache** (`bool` ,*optional* ) —
If set to`True` ,`cache` key value states are returned and can be used to speed up decoding (see`cache` ).

The Lfm3Model forward method, overrides the `__call__` special method.

Although the recipe for forward pass needs to be defined within this function, one should call the `Module` instance afterwards instead of this since the former takes care of running the pre and post processing steps while
the latter silently ignores them.


- **last_hidden_state** (`torch.Tensor` of shape`(batch_size, sequence_length, hidden_size)` ) — Sequence of hidden-states at the output of the last layer of the model.If `cache` is used only the last hidden-state of the sequences of shape`(batch_size, 1, hidden_size)` is output.
- **cache** (`Cache` ,*optional* , returned when`use_cache=True` is passed or when`config.use_cache=True` ) — It is a Cache instance. For more details, see our kv cache guide.Contains pre-computed hidden-states (key and values in the self-attention blocks and optionally if `config.is_encoder_decoder=True` in the cross-attention blocks) that can be used (see`cache` input) to speed up sequential decoding.
- **hidden_states** (`tuple(torch.Tensor)` ,*optional* , returned when`output_hidden_states=True` is passed or when`config.output_hidden_states=True` ) — Tuple of`torch.Tensor` (one for the output of the embeddings, if the model has an embedding layer, +
one for the output of each layer) of shape`(batch_size, sequence_length, hidden_size)` .Hidden-states of the model at the output of each layer plus the optional initial embedding outputs.
- **attentions** (`tuple(torch.Tensor)` ,*optional* , returned when`output_attentions=True` is passed or when`config.output_attentions=True` ) — Tuple of`torch.Tensor` (one for each layer) of shape`(batch_size, num_heads, sequence_length, sequence_length)` .Attentions weights after the attention softmax, used to compute the weighted average in the self-attention heads.

### class transformers.Lfm3ForCausalLM

< source >
( config: Lfm3Config )

Parameters

-  **config** (Lfm3Config) —
Model configuration class with all the parameters of the model. Initializing with a config file does not
load the weights associated with the model, only the configuration. Check out the
from_pretrained() method to load the model weights.

The Lfm3 Model for causal language modeling.

This model inherits from PreTrainedModel. Check the superclass documentation for the generic methods the library implements for all its model (such as downloading or saving, resizing the input embeddings, pruning heads etc.)

This model is also a PyTorch torch.nn.Module subclass. Use it as a regular PyTorch Module and refer to the PyTorch documentation for all matter related to general usage and behavior.

#### forward

< source >
( input_ids: typing.Optional[torch.LongTensor] = Noneattention_mask: typing.Optional[torch.Tensor] = Noneposition_ids: typing.Optional[torch.LongTensor] = Nonecache: transformers.cache_utils.Cache | None = Noneinputs_embeds: typing.Optional[torch.Tensor] = Nonelabels: typing.Optional[torch.LongTensor] = Noneuse_cache: bool | None = Nonelogits_to_keep: typing.Union[int, torch.Tensor] = 0**kwargs: *<class 'transformers.utils.generic.TransformersKwargs'> ) → CausalLMOutputWithPast or `tuple(torch.Tensor)`

Parameters

-  **input_ids** (`torch.LongTensor` of shape`(batch_size, sequence_length)` ,*optional* ) —
Indices of input sequence tokens in the vocabulary. Padding will be ignored by default.Indices can be obtained using AutoTokenizer. See PreTrainedTokenizer.encode() and PreTrainedTokenizer. **call** () for details.
-  **attention_mask** (`torch.Tensor` of shape`(batch_size, sequence_length)` ,*optional* ) —
Mask to avoid performing attention on padding token indices. Mask values selected in`[0, 1]` :
  - 1 for tokens that are **not masked** ,
  - 0 for tokens that are **masked** .
- 1 for tokens that are 
-  **position_ids** (`torch.LongTensor` of shape`(batch_size, sequence_length)` ,*optional* ) —
Indices of positions of each input sequence tokens in the position embeddings. Selected in the range`[0, config.n_positions - 1]` .
-  **cache** (`~cache_utils.Cache` ,*optional* ) —
Pre-computed hidden-states (key and values in the self-attention blocks and in the cross-attention
blocks) that can be used to speed up sequential decoding. This typically consists in the`cache` returned by the model at a previous stage of decoding, when`use_cache=True` or`config.use_cache=True` .Only Cache instance is allowed as input, see our kv cache guide. If no `cache` are passed, DynamicCache will be initialized by default.The model will output the same cache format that is fed as input. If `cache` are used, the user is expected to input only unprocessed`input_ids` (those that don’t
have their past key value states given to this model) of shape`(batch_size, unprocessed_length)` instead of all`input_ids` of shape`(batch_size, sequence_length)` .
-  **inputs_embeds** (`torch.Tensor` of shape`(batch_size, sequence_length, hidden_size)` ,*optional* ) —
Optionally, instead of passing`input_ids` you can choose to directly pass an embedded representation. This
is useful if you want more control over how to convert`input_ids` indices into associated vectors than the
model’s internal embedding lookup matrix.
-  **labels** (`torch.LongTensor` of shape`(batch_size, sequence_length)` ,*optional* ) —
Labels for computing the causal language modeling loss. Indices should be in`[-100, 0, ..., config.vocab_size]` (see`input_ids` docstring). Tokens with indices set to`-100` are ignored (masked), the
loss is only computed for the tokens with labels in`[0, ..., config.vocab_size]` .The labels are **shifted internally** so that`labels[..., 1:]` are used to predict`logits[..., :-1, :]` .
Pass unshifted labels matching`input_ids` (i.e.`labels = input_ids` ). Do**not** shift the labels
yourself before passing them.If the labels are already shifted (for example with sequence parallelism), pass them as the `shift_labels` keyword argument and they are used as is.`labels` must still be passed for the loss to be computed.
-  **use_cache** (`bool` ,*optional* ) —
If set to`True` ,`cache` key value states are returned and can be used to speed up decoding (see`cache` ).
-  **logits_to_keep** (`Union[int, torch.Tensor]` ,*optional* , defaults to`0` ) —
If an`int` , compute logits for the last`logits_to_keep` tokens. If`0` , calculate logits for all`input_ids` (special case). Only last token logits are needed for generation, and calculating them only for that
token can save memory, which becomes pretty significant for long sequences or large vocabulary size.
If a`torch.Tensor` , must be 1D corresponding to the indices to keep in the sequence length dimension.
This is useful when using packed tensor format (single dimension for batch and sequence length).

A CausalLMOutputWithPast or a tuple of
`torch.Tensor` (if `return_dict=False` is passed or when `config.return_dict=False`) comprising various
elements depending on the configuration (Lfm3Config) and inputs.

The Lfm3ForCausalLM forward method, overrides the `__call__` special method.

Although the recipe for forward pass needs to be defined within this function, one should call the `Module` instance afterwards instead of this since the former takes care of running the pre and post processing steps while
the latter silently ignores them.


- **loss** (`torch.Tensor` of shape`(1,)` ,*optional* , returned when`labels` is provided) — Language modeling loss (for next-token prediction).
- **logits** (`torch.Tensor` of shape`(batch_size, sequence_length, config.vocab_size)` ) — Prediction scores of the language modeling head (scores for each vocabulary token before SoftMax).
- **cache** (`Cache` ,*optional* , returned when`use_cache=True` is passed or when`config.use_cache=True` ) — It is a Cache instance. For more details, see our kv cache guide.Contains pre-computed hidden-states (key and values in the self-attention blocks) that can be used (see `cache` input) to speed up sequential decoding.
- **hidden_states** (`tuple(torch.Tensor)` ,*optional* , returned when`output_hidden_states=True` is passed or when`config.output_hidden_states=True` ) — Tuple of`torch.Tensor` (one for the output of the embeddings, if the model has an embedding layer, +
one for the output of each layer) of shape`(batch_size, sequence_length, hidden_size)` .Hidden-states of the model at the output of each layer plus the optional initial embedding outputs.
- **attentions** (`tuple(torch.Tensor)` ,*optional* , returned when`output_attentions=True` is passed or when`config.output_attentions=True` ) — Tuple of`torch.Tensor` (one for each layer) of shape`(batch_size, num_heads, sequence_length, sequence_length)` .Attentions weights after the attention softmax, used to compute the weighted average in the self-attention heads.

Example:

```
>>> from transformers import AutoTokenizer, Lfm3ForCausalLM
>>> model = Lfm3ForCausalLM.from_pretrained("LiquidAI/LFM3-1.7B")
>>> tokenizer = AutoTokenizer.from_pretrained("LiquidAI/LFM3-1.7B")
>>> inputs = tokenizer.apply_chat_template(
...     [{"role": "user", "content": "What is C. elegans?"}], add_generation_prompt=True, return_tensors="pt"
... )
>>> generate_ids = model.generate(**inputs, max_new_tokens=64)
>>> tokenizer.decode(generate_ids[0, inputs["input_ids"].shape[1] :], skip_special_tokens=True)
```
