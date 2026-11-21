---
id: collect-261121-veille-ia/actu-business/pricing
title: "Pricing"
domain: actu-business
role: reference
task: article
actors: []
dates: []
keywords: ["tokens", "ling-3", "flash", "pricing", "prices", "billing", "flash-vl", "ming-image-0"]
source: https://developer.ant-ling.com/en/docs/models/price
source_anchor: ""
source_lines: [1, 58]
section: "Actu_Business"
---
# Pricing

This page provides detailed pricing information for Ling’s models. Billing is pay-as-you-go based on token consumption, with no minimum spend.

**Limited-Time Benefits**:

1. **Ling-3.1-flash 75% off, Ling-3.0-flash-VL and Ling-3.0-flash 72% off, and Ming-Image-0.1-Design and Ming-Image-0.1-Design-Layer 70% off — on sale now!** Original prices will gradually resume after the promotional period ends — please keep an eye on your account balance.
2. During the promotional period, the same models on OpenRouter and ZenMux are also discounted — refer to each platform’s official pricing for specific discounts. Original prices resume after the promotion ends.

All prices below are in CNY.

| Model | Use Case | Input Price | Unit | Output Price | Unit | Cache Read | Unit | 
|---|---|---|---|---|---|---|---|
| **Ling-3.2-flash** | Latest native multimodal | ¥2.00 | 1M tokens | ¥6.00 | 1M tokens | ¥0.40 | 1M tokens | 
| **Ling-3.1-flash** | New long-horizon | ~~¥2.00~~ ¥0.50 | 1M tokens | ~~¥6.00~~ ¥1.50 | 1M tokens | ~~¥0.40~~ ¥0.10 | 1M tokens | 
| **Ling-3.0-flash-VL** | Native multimodal | ~~¥0.50~~ ¥0.14 | 1M tokens | ~~¥1.50~~ ¥0.42 | 1M tokens | ~~¥0.10~~ ¥0.028 | 1M tokens | 
| **Ling-3.0-flash** | Latest high-speed | ~~¥0.50~~ ¥0.14 | 1M tokens | ~~¥1.50~~ ¥0.42 | 1M tokens | ~~¥0.10~~ ¥0.028 | 1M tokens | 
| **Ming-Image-0.1-Design** | Design image generation | — | — | ~~¥0.10~~ ¥0.03 | image | — | — | 
| **Ming-Image-0.1-Design-Layer** | Transparent layer extraction | — | — | ~~¥0.10~~ ¥0.03 | image | — | — | 

All prices below are in USD.

| Model | Input (per 1M tokens) | Output (per 1M tokens) | Cache Read (per 1M tokens) | 
|---|---|---|---|
| **Ling-3.2-flash** | $0.30 | $0.90 | $0.06 | 
| **Ling-3.1-flash** | ~~$0.30~~ $0.075 | ~~$0.90~~ $0.225 | ~~$0.06~~ $0.015 | 
| **Ling-3.0-flash-VL** | ~~$0.075~~ $0.021 | ~~$0.22~~ $0.062 | ~~$0.015~~ $0.0042 | 
| **Ling-3.0-flash** | ~~$0.06~~ $0.0168 | ~~$0.18~~ $0.0504 | ~~$0.012~~ $0.0034 | 

Please refer to the official OpenRouter pricing for actual prices: https://openrouter.ai/inclusionai


| Model | Input (per 1M tokens) | Output (per 1M tokens) | Cache Read (per 1M tokens) | 
|---|---|---|---|
| **Ling-3.2-flash** | $0.30 | $0.90 | $0.06 | 
| **Ling-3.1-flash** | ~~$0.30~~ $0.075 | ~~$0.90~~ $0.225 | ~~$0.06~~ $0.015 | 
| **Ling-3.0-flash-VL** | ~~$0.075~~ $0.021 | ~~$0.22~~ $0.062 | ~~$0.015~~ $0.0042 | 
| **Ling-3.0-flash** | ~~$0.06~~ $0.0168 | ~~$0.18~~ $0.0504 | ~~$0.012~~ $0.0034 | 

Please refer to the official ZenMux pricing for actual prices: https://zenmux.ai/inclusionai


- **Input tokens** include user messages, system prompts, and conversation history.**Output tokens** are model-generated content.
- Bills are generated daily at midnight for the previous day’s usage.

Before your first API call, bind an Alipay account and activate the Ling service on the API Console .

- **Recharge** : Click “Recharge” in the API Console to top up via Alipay Mini Program Cloud .
- **Usage & bills** : View usage statistics and billing history in the Billing Center .
- **Invoices** : Individual users can apply for invoices in Invoice Management . Invoice support for enterprise legal entities is under development.

**What happens when the free quota runs out?**

If you have bound a Mini Program Cloud account and signed the auto-debit agreement, billing automatically switches to pay-as-you-go. Otherwise, API calls are suspended until you complete the binding or the quota resets the next day.

**What payment methods are supported?**

Only personal Alipay accounts are supported for payment. Enterprise Alipay accounts are not currently supported. Enterprise users can contact customer service to discuss specific cooperation arrangements.
