---
id: collect-261121-veille-ia/glm-zhipu/z-ai-glm-5-3-prime-api-pricing-playground-composite
title: "Z.ai: GLM 5.3 Prime: API, Pricing & Playground | Composite"
domain: glm-zhipu
role: reference
task: article
actors: ["Zhipu AI"]
dates: []
keywords: ["composite", "tokens", "prime", "model", "million", "output", "provider", "glm-5"]
source: https://composite.lucidity.sh/models/z-ai/glm-5.3-prime
source_anchor: ""
source_lines: [1, 119]
section: "GLM_Zhipu"
---
← All models
    Model directory
    
      AI model

      # Z.ai: GLM 5.3 Prime

      `z-ai/glm-5.3-prime`
      Z.ai: GLM 5.3 Prime is a 1,000,000-token model served on Composite at $2.10 per million tokens in and $6.60 per million tokens out. Ranked #49 of 176 models by traffic on Composite over the last 30 days, taking 0.19% of tokens and 0.17% of requests. Measured on Composite, 100.0% of its last 1 requests on Composite succeeded, the typical first token arrives in 20596 ms, output runs at about 218.9 tokens per second.

      Try in Playground
    
    
    
      ## Specifications

      Specifications and pricing as served by Composite
| Model ID | z-ai/glm-5.3-prime | 
|---|---|
        | Provider | cv11 | 
        | Context window | 1,000,000 tokens | 
        | Accepts | text | 
        | Returns | text | 
        | Input price | $2.10 per million tokens | 
        | Output price | $6.60 per million tokens | 
        | Prompt caching | Supported — cached input is billed at the provider's cache rate, on credits and plans alike | 
        | Thinking | Always on for this model | 
        | Moderated | No | 
        | Cost on a monthly plan | About 19 plan credits per typical call (8k in, 1k out) | 
        | Routing endpoints | 2 (base plus 1 routing variant) | 
      
    

    
    
      
        ## Use Z.ai: GLM 5.3 Prime in JanitorAI or SillyTavern

        Make a Composite API key and type `z-ai/glm-5.3-prime` as the model name. JanitorAI takes the full URL `https://composite.lucidity.sh/v1/chat/completions`; SillyTavern, ChubAI, Agnai and RisuAI take `https://composite.lucidity.sh/v1`. It runs on credits, a plan, or your own provider key.

        JanitorAI setup · SillyTavern setup · Best Janitor AI proxies compared

      
    
    
      ## Traffic rank on Composite

#49 of 176

0.19% of tokens and 0.17% of requests over 30 days.

      ## Success rate

100.0%

Across the last 1 requests Composite served for this model.

      ## Time to first token

20596 ms

Median across the same sample.

      ## Output speed

218.9 tok/s

Median generation rate after the first token.

    
    
      ## Routing variants

      The same model behind 1 alternative endpoint. Call the id directly to pin a route; the base id above picks for you.

      | Route | Model ID | Provider | Price | 
|---|---|---|---|
        
        
          | cheapThink | `z-ai/glm-5.3-prime:cheapThink` | cv11 | $2.10 per million tokens input · $6.60 per million tokens output | 
        
      

    
    
      ## Pricing

$2.10 per million tokens input · $6.60 per million tokens output

Credits pay the provider’s price plus 25%; cache hits cost less.

On a monthly plan, a typical call (8k tokens in, 1k out) uses about 19 plan credits, at the provider's own price.

      ## Provider

cv11

    
    
      ## Roleplay fit

GLM models balance strong instruction-following with competitive pricing, a reasonable middle ground for most roleplay setups.

      ## Context behavior

With a 1,000,000-token context window, it can hold a long-running roleplay or a full lorebook in memory without losing earlier plot details, so it suits multi-session campaigns and character cards with heavy backstory.

      ## Cost for regular use

It sits in the mid-range on price, a reasonable pick for regular roleplay use without going for the cheapest option available.

    
    
      
        ## From the provider

        GLM-5.3-Prime is the high-speed variant of Z.ai's GLM-5.3, inheriting its full capabilities while delivering 1.5–2× the output throughput through inference acceleration. It supports text input and output with a 1M-token...

        Supplied by cv11, not written by Composite.
