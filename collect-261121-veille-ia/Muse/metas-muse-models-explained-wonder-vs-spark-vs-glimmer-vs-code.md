---
id: collect-261121-veille-ia/muse/metas-muse-models-explained-wonder-vs-spark-vs-glimmer-vs-code
title: "Meta’s Muse Models Explained - Wonder vs Spark vs Glimmer vs Code"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-10-12"]
keywords: ["muse", "spark", "model", "wonder", "meta", "code", "glimmer", "metas"]
source: https://www.scaler.com/topics/meta-muse-models-explained-wonder-vs-spark-vs-glimmer-vs-code/
source_anchor: ""
source_lines: [1, 254]
section: "Muse"
---
# Meta’s Muse Models Explained: Wonder vs Spark vs Glimmer vs Code

Meta’s Muse lineup is easier to understand once you separate the model from the product using it. **Muse Spark** and **Muse Wonder** are model families. **Muse Glimmer** is an open-weight model intended for local workflows. **Muse Code** is a coding agent, not a model.

The distinction became more important when Meta introduced **Muse Wonder and Wonder Full on 12 October 2026**. Meta positions the pair for demanding personal and professional agentic tasks, while Spark remains relevant for lower-cost hosted work and Glimmer serves developers who want more local control.

**Quick answer:** Start with Spark for cost-sensitive cloud workloads, evaluate Wonder for harder agentic tasks and consider Glimmer for local deployment. Choose Muse Code when you need a coding-agent environment, then select a supported model within it. Spark Open is a separate, much larger open-weight option with its own licence.

This is a research-based comparison, not a claim that we personally tested every model. Vendor and independent benchmark results are identified separately.

## **Meta Muse Models: Quick Comparison**

| Name | What it is | Best fit | Access and trade-off | 
|---|---|---|---|
| **Muse Spark 1.3** | Hosted AI model | Everyday coding, assistance and agentic workflows | API and supported products; lower reported token price than Wonder | 
| **Muse Wonder** | Frontier-focused model | Complex reasoning, coding and multi-step work | Meta Model API and Muse Code; higher API price | 
| **Muse Wonder Full** | More capable Wonder variant | Demanding agentic coding in Muse Code | Eligible Muse Code subscriptions; no public per-token API price identified | 
| **Muse Glimmer 30B** | Open-weight model | Local agents and tool use | Downloadable under Apache 2.0; needs suitable hardware | 
| **Muse Code** | Terminal coding agent | Work across software repositories | Uses supported models; plan and limits determine access | 
| **Muse Spark Open** | Separate large open-weight model | Larger self-hosted agentic and coding deployments | Downloadable under the Muse Model License; substantial infrastructure needs | 

Sources include Meta’s Wonder announcement, its Glimmer research post and its Spark Open release.

## **What Is Muse Spark?**

Muse Spark was the first model family from Meta Superintelligence Labs under the Muse name. Meta designed the initial release to be relatively small and fast, while supporting reasoning and multimodal tasks.

In its Spark announcement, Meta said the model powered its Meta AI app and website and planned a rollout across additional products. The model is the underlying component; availability and features depend on the app or API using it.

### **What is Muse Spark 1.3?**

Muse Spark 1.3 is a later hosted version that Meta says improves coding and agentic workflows. The official Spark 1.3 research post says the model is available through the Meta Model API and Muse Code.

A practical use case is a recurring classification or coding workflow where latency and cost matter. Start with a representative test set, then escalate the harder cases to a stronger model only if doing so improves the result.

## **What Is Muse Wonder?**

Muse Wonder is Meta’s newer frontier-focused series for complex personal and professional agentic tasks. Meta introduced Wonder and Wonder Full as its most capable models at the time, building on work from Spark.

Meta highlights two capabilities:

- **Active Memory:** Carrying relevant context across conversations and retrieving it when useful.
- **Active Focus:** Continuing towards a difficult goal, trying alternatives, recovering from blockers and seeking clarification when needed.

These are Meta’s descriptions, not guarantees that an agent will remember the right detail or complete a task safely. Product settings, permissions and implementation also matter. Scaler’s explanation of how AI agents use models and tools provides context for that distinction.

### **Muse Wonder vs Muse Wonder Full**

| Feature | Muse Wonder | Muse Wonder Full | 
|---|---|---|
| Position | Frontier model for agentic tasks | More capable Wonder variant | 
| API access | Meta Model API, subject to eligibility | No public per-token API offering identified in the sources reviewed | 
| Muse Code | Available | Available through eligible higher-tier subscriptions | 
| Best starting use | Programmatic applications that need stronger reasoning | Difficult coding work inside an eligible Muse Code plan | 

BetaNews reported that Wonder Full required Muse Code Power Usage or Maximum Usage. Confirm current plan terms and local availability before subscribing.

## Build an AI-First Career, Master the Complete Skillset

Choose from our industry-leading programs designed for career success

### **Is Wonder available in the consumer Muse app?**

At the October announcement, Meta said Wonder would **soon** power Muse experiences, rolling out first to subscribers. That was a planned rollout, not evidence that every subscriber already had access. Check the model options in the relevant product and account.

## **What Is Muse Glimmer?**

Muse Glimmer is an **open-weight, approximately 30-billion-parameter model** designed for local agentic workflows. Meta’s Glimmer model card lists an Apache 2.0 licence, text and image input, and text output.

Meta says the model is intended for uses such as local agents, function calling and coding assistance. Its weights can be downloaded, allowing a team to run compatible configurations on infrastructure it controls.

### **Can Glimmer run on a laptop?**

Some configurations can run on suitable consumer hardware, but not every laptop will be sufficient. Meta says approximately 4-bit quantisation reduces the language-model weights to under 20 GB, with the full working configuration targeting a **24 GB or 32 GB memory envelope** in its stated setups.

Requirements vary with context length, image processing, runtime and other workloads. Read the model card and test the actual configuration before purchasing hardware or promising a response speed.

### **Does local inference guarantee privacy?**

No. Local inference can reduce dependence on a hosted model provider, but connected tools, logs or external APIs may still transmit data. The team operating the system must manage access controls, updates and monitoring.

## **What Is Muse Code?**

Muse Code is Meta’s terminal-based coding agent. It is a **product and agent environment**, not another model in the same category as Spark or Wonder.

A coding agent can combine a model with access to project files, a terminal and other supported tools. Depending on permissions, it may help a developer understand a repository, propose changes, run checks or iterate on an error.

This is why comparing **Muse Code vs Muse Wonder** requires care: one is an agent environment and the other is a model that can power work within it. Scaler’s guide to AI coding tools explains the wider range of assistants and developer workflows.

Human review remains essential. Passing tests do not prove a change is secure or that it meets every requirement.

## **What Is Muse Spark Open?**

Muse Spark Open is a separate open-weight model released on **15 October 2026**. Meta’s announcement lists **816 billion total parameters**, **42 billion active parameters** and a **1 million-token context window**.

The active-parameter figure does not make Spark Open equivalent to a dense 42B model in storage needs. Meta describes a deployment using two Apple M5 Ultra Mac Studios and also lists hosted inference providers.

Spark Open uses the **Muse Model License**, not Glimmer’s Apache 2.0 licence. “Open-weight” describes the availability of weights under particular terms; it does not mean all open-weight models have identical commercial permissions.

## **Wonder vs Spark vs Glimmer: Which Performs Better?**

The best choice depends on the task and setup. Meta positions Wonder above Spark for more demanding agentic work, while Glimmer is designed for a smaller local deployment rather than maximum frontier capability.

In an Artificial Analysis evaluation published on 12 October 2026, the Intelligence Index v4.3 scores at maximum reasoning effort were:

| Model | Reported index score | 
|---|---|
| Muse Spark 1.3 | 48 | 
| Muse Wonder | 52 | 
| Muse Wonder Full | 56 | 

Artificial Analysis also reported coding-agent results using a specified Muse Code setup. These numbers are useful for shortlisting, not a guarantee that Wonder Full wins every task. Benchmark dates, model versions, reasoning effort, tool access and harnesses affect results.

A production comparison should measure correct outcomes, latency, token use, review time and the cost of failures on representative tasks. Scaler’s guide to AI agent frameworks explains why the surrounding orchestration layer affects agent behaviour.

## **Muse Spark and Muse Wonder Pricing**

The following US dollar prices were reported in the sources reviewed on 2 November 2026. Confirm current rates, taxes, availability and cache terms before budgeting.

| Option | Reported price | Important caveat | 
|---|---|---|
| Spark 1.3 API | $1.25 input and $4.25 output per million tokens | Cached-token and tool charges may differ | 
| Wonder API | $4 input and $20 output per million tokens | Artificial Analysis reports $0.20 per million cached input tokens | 
| Wonder Full | Available through eligible Muse Code plans | No public per-token API price identified | 
| Glimmer | Downloadable weights | Hardware, hosting and maintenance still cost money | 
| Spark Open | Downloadable weights under a custom licence | Provider and infrastructure costs vary | 

Zapier’s Muse comparison and Artificial Analysis’s evaluation report the hosted model rates. Official Meta developer pricing pages were not directly accessible during the review, so verify the final amount in your developer account.

### **Example: token cost for a single request**

For **one million input tokens** and **200,000 output tokens**, with no caching, tool fees or other charges:

| Model | Input | Output | Illustrative total | 
|---|---|---|---|
| Spark 1.3 | $1.25 | $0.85 | **$2.10** | 
| Wonder | $4.00 | $4.00 | **$8.00** | 

An agent may make several calls, resend context or use tools. Compare the cost of an accepted result, including retries and human review, rather than relying only on the price per token. For an introduction to how tokens and context affect requests, see Scaler’s prompt engineering basics.

### **How much does Muse Code cost?**

Zapier reports $50 and $200 Muse Code plans with Wonder Full. The official subscription page was not directly accessible during this review, so check the current Meta Muse Code subscription documentation and checkout for plan names, limits and local prices.

## **How to Choose the Right Muse Model**

### **Choose Spark 1.3 for lower-cost hosted work**

Spark is a reasonable first candidate for routine coding, classification, drafting and agent prototypes. Evaluate it on real tasks before routing more expensive requests to Wonder.

### **Choose Wonder for harder reasoning and API access**

Wonder is relevant when a workflow involves difficult software changes, complex documents or multiple tool calls. Measure whether any quality improvement reduces enough rework to justify the higher cost.

## How Scaler Transformed Careers in Different Fields

Scaler learners achieved 2.5x salary growth with average post-Scaler CTC reaching ₹23L.

### **Choose Wonder Full for demanding Muse Code work**

If your eligible plan includes Full, test it on the hardest tasks in your repository. Track accepted changes and review time, not just benchmark scores.

### **Choose Glimmer for local control**

Glimmer is suitable for teams that can operate local infrastructure and value downloadable weights. Test quality, memory usage, network access and model licence requirements together.

### **Choose Spark Open for larger self-hosted deployments**

Spark Open is a different scale of project. Review hardware, serving, licence terms and the alternatives from hosted providers before building a deployment plan.

## **What Is Meta Superintelligence Labs?**

Meta Superintelligence Labs is the Meta organisation behind the Muse releases discussed here. Meta frames its work around **personal superintelligence**, a product vision for AI systems that can help people with complex goals.

That vision should not be confused with proof that a model is generally intelligent, error-free or safe to operate without controls. The distinction between a model, an agent and an application is explored in Scaler’s comparison of generative AI, agentic AI and LLMs.

## **How to Get Started With Meta Muse**

### **For general users**

- Check whether the relevant Meta AI or Muse experience is available in your region.
- Read the product’s privacy settings and connected-app permissions.
- Start with a low-risk request and verify the response.
- Confirm which model is actually available in your account.

### **For developers**

- Review the Meta developer site and current model access.
- Build an evaluation set using representative tasks.
- Compare supported models under similar tool and reasoning settings.
- Track cost, correctness, latency and review time.
- Limit tool permissions and require approval for consequential actions.

For a local Glimmer experiment, begin with its model card, compatible runtime and hardware requirements. For a structured path into agent design and evaluation, explore Scaler’s Advanced AIML with Specialisation in Agentic AI.

### Turn Learning into Career Growth

## Conclusion

Meta’s Muse lineup is not a single model with four names. Spark is a hosted model family, Wonder targets harder agentic work, Glimmer offers a smaller local open-weight route and Muse Code is the environment for a coding agent. Spark Open adds a separate large open-weight option with different infrastructure and licensing requirements.

Choose by the work you need to complete, then test the full workflow under realistic conditions. A model’s price, benchmark score or open-weight label is useful only when it is considered alongside reliability, security and operating cost.

## FAQs

### 1. What is Muse Spark?

Muse Spark is a model family developed by Meta Superintelligence Labs. It supports assistant, coding and agentic use cases in supported products.Spark 1.3 is a hosted model; Spark Open is a separate open-weight release.

### 2. What is the difference between Spark and Wonder?

Spark is positioned as a faster, lower-cost option for many workloads. Wonder is Meta’s newer frontier-focused series for more demanding tasks. Independent October testing reported higher index scores for Wonder.

### 3. What is Muse Wonder Full?

Wonder Full is the more capable variant in the Wonder series.

Meta made it available through eligible Muse Code subscriptions.The sources reviewed did not identify a public per-token API offering for Full. Check current plan terms before subscribing.

### 4. Is Muse Glimmer open-weight?

Yes. Meta released Glimmer weights under the Apache 2.0 licence.The model is designed for local agentic workflows on suitable hardware.Its operating requirements depend on quantisation, context and runtime.

### 5. Is Muse Code a model?

No. Muse Code is a terminal-based coding agent.

It can use supported Muse models for software-development tasks. The agent environment and the selected model both affect results.

Available models and limits depend on the plan and account.

### 6. Is Muse Spark Open the same as Glimmer?

No. Both offer downloadable weights, but they are different models. Spark Open has 816B total parameters and uses the Muse Model License. Glimmer is approximately 30B parameters and uses Apache 2.0. Their hardware and deployment requirements differ substantially.

### 7. Can developers access Muse Wonder through an API?

Meta announced standard Wonder through the Meta Model API.

Access can depend on regional availability and account eligibility.Wonder Full was tied to eligible Muse Code subscriptions in the sources reviewed.

Verify the current model list and terms in your developer account.

### 8. Which Muse option is best for coding?

Spark can be a useful lower-cost starting point for routine tasks.Wonder and Wonder Full are candidates for more difficult agentic coding. Glimmer provides a local option when its hardware and quality fit.

### 9. Does a large context window mean perfect memory?

No. A context window limits how much information fits in a request.It does not guarantee perfect recall or permanent cross-session memory. Persistent memory depends on the product and its settings.Verify important details and manage what information an agent can use.

### 10. Is running Glimmer locally automatically private?

Local inference can keep model processing on infrastructure you control. External tools, network calls and logs can still share information.Security depends on permissions, storage and system configuration.Audit the entire workflow before processing sensitive data.
