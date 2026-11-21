---
id: collect-261121-veille-ia/mistral/mistral-large-4-arrives-as-the-top-open-model-outside-china-with-improved-coding-performance-and-63-
title: "Mistral Large 4 arrives as the top open model outside China with improved coding performance and 63% lower cost-per-task"
domain: mistral
role: reference
task: article
actors: ["Mistral AI"]
dates: []
keywords: ["model", "mistral", "preview", "open", "weights", "large", "lower", "october"]
source: https://venturebeat.com/technology/mistral-large-4-le-chonk-arrives-as-the-top-open-model-outside-china-with-improved-coding-performance-and-63-lower-cost-per-task
source_anchor: ""
source_lines: [1, 184]
section: "Mistral"
---
Mistral has released the downloadable open weights for its trillion-parameter Large 4 model under a custom license, giving enterprises a new option for running and customizing a near-frontier AI model themselves. Independent tests now show that its biggest advances over the preview involve efficiency, lower costs and fewer fabricated answers, alongside a small increase in overall capability.

In its October 29 assessment, the independent evaluation firm Artificial Analysis gives ML4 at its high reasoning setting 26 points on Intelligence Index v5, up from 25 for the preview. That puts it one point above Muse Spark Open at xhigh, making ML4 the highest-scoring model with available weights developed outside China in that evaluation.

It remains six points behind the open-weight leaders, DeepSeek V4.1 Flash 1017 and GLM-5.3, both tested at their maximum reasoning settings.

The larger change is economic: Artificial Analysis reports that the updated model costs $2.06 per Intelligence Index task, down from $5.54 for the preview, a **63% reduction.** 

Its coding-agent assessment ranks ML4 second among current open-weight entries, using roughly half the tokens of the leading GLM-5.3 setup. Mistral’s launch announcement links the downloadable model on Hugging Face and introduces substantially lower API rates.

The release’s Mistral Ecosystem Open-Weight License makes the commercial boundary explicit. It contains no revenue threshold. Instead, the requirement for a separate commercial agreement depends on whether the user or an affiliate operates a model-as-a-service business, subject to specific exceptions.

For Marjorie Janiewicz, Mistral’s chief revenue officer, the commercial argument extends beyond the scores. In an in-person interview with VentureBeat earlier this week, she described customers seeking models they can retain, customize around their own information and use without depending entirely on a provider’s continuing permission.

“I don’t want to get disconnected. I want to own my model,” Janiewicz said, describing that customer demand. “Second, I want to customize it to ensure that I derive value. And then third, it’s business continuity.”

The independent results sharpen that proposition: ML4 offers a competitive coding agent and improved efficiency, alongside the control that comes with downloadable weights. They also put boundaries around Mistral’s leadership claims. Its narrow lead among open-weight models developed outside China does not make it the strongest open-weight model worldwide, and its scores vary with the software and settings used to run it.

## **What changes from the October preview**

When VentureBeat covered ML4 on October 6, Mistral offered a preview through its API and planned to publish weights on October 27. The weights arrived on October 29 after roughly three additional weeks of reinforcement learning and feedback from customers and security partners.

The model’s basic identity remains intact: a very large, sparsely activated system that accepts visual and textual information and generates text. Artificial Analysis now lists a context capacity of 1 million tokens, and its release assessment specifies 1.05 trillion total parameters and 52 billion active parameters. The development since the preview is additional training and deployment choice, rather than the unveiling of a separate model family.

Some apparent specification changes also need qualification. Our original report cited 49 billion active parameters; the new release cites 52 billion. Mistral’s Hugging Face listing explains that the smaller figure describes activity per token, while the larger total also counts embeddings and output layers. That difference alone does not show that Mistral expanded the model during the preview period.

The latest materials specify 3,800 Nvidia Grace Blackwell GPUs for training, rather than the approximately 4,000 reported initially. The public launch adds deployment details: Mistral Studio and its API, OpenRouter, Vibe Code, Microsoft Foundry with private Azure deployment options, and Nvidia-hosted endpoints and an optimized NIM container. Mistral says the downloaded weights can be served with vLLM or SGLang.

Three reasoning settings introduce another practical choice: low replaces the previous “None” mode, medium targets everyday tasks, and high is the default for the hardest requests. A development team could reserve deeper reasoning for a difficult debugging task and use a lower setting for routine requests. Whether that reduces the cost of successfully completing work will depend on the task and how often a cheaper attempt needs correction.

## **Benchmark task costs fall 63%, but cheaper rivals remain**

The final release combines lower token consumption with API rates of $0.70 per million input tokens, $0.07 for cached input and $2.10 for output. Artificial Analysis’s new assessment confirms those rates, which are roughly half the earlier $1.36, $0.14 and $4.18 respectively.

AA now measures the economic result for the released checkpoint, rather than leaving it as a projection from token prices:

| **Model and setting** | **Cost per Intelligence Index task** | 
|---|---|
| Mistral Large 4 Preview | $5.54 | 
| Mistral Large 4, high | $2.06 | 
| Muse Spark Open, xhigh | $2.02 | 
| DeepSeek V4.1 Flash 1017, max | $0.60 | 
| MiMo-V2.6-Pro | $0.26 | 

*Source: Artificial Analysis’s October 29 assessment; ML4’s current model page also reports $2.06. These figures concern Intelligence Index tasks, not Coding Agent Index tasks.*

ML4’s cost falls about 63% versus its preview, bringing it close to Muse Spark Open. Yet DeepSeek and MiMo achieve higher overall Intelligence Index scores at substantially lower measured task costs. ML4’s favorable output-token usage does not translate into the lowest bill: token rates, input traffic and caching also affect spending.

These benchmark costs do not measure the expense of self-hosting, customization, operational oversight or human review. Enterprises still need to compare the cost of work completed to an acceptable standard in their own environments.

## **What the license allows — and when Mistral requires a deal**

The final license grants broad rights to run, copy, modify, further train, distribute, sublicense and sell the model and its derivatives, provided the copyright and permission notices accompany copies or substantial portions. It is a modified MIT license with an additional commercial restriction, so describing ML4 simply as MIT-licensed would omit a material condition.

Section 2 requires a separate commercial license before commercial use of the model or its derivatives if the licensee **or any affiliate** operates a "model-as-a-service" business. 

The definition covers giving third parties access for inference or post-training, through an API, hosted endpoint or other means, when those users have meaningful control over inputs, parameters or training data.

Two exclusions narrow that definition: end-user products whose model capabilities are solely embedded within specific features or harnesses, and services that merely relay requests to models hosted by others. Section 3 separately exempts internal use that does not make the model or its derivatives available to third parties, as well as use accessed through Mistral’s official products.

That means an enterprise using ML4 internally does not trigger Section 2’s commercial-license requirement, even if it or an affiliate operates a model-as-a-service business.

An end-user product with model capabilities solely embedded within specific features or harnesses is excluded from the model-as-a-service definition; its operator still needs to consider whether another activity by it or an affiliate triggers Section 2.

An operator offering customers hosted inference or post-training with the specified control faces the separate-license requirement before commercial use, subject to the stated exceptions.

The distinction is business activity and access, rather than company size: the model-specific license sets no revenue cutoff. The permission to run downloaded weights also does not make hosted services or the infrastructure required for self-hosting free.

## **Second among open-weight coding agents, with lower token use**

Artificial Analysis now independently reports ML4’s position on its Coding Agent Index. The relevant comparison includes both the model and the agent software that directs its tool calls and coding work:

| **Model and reasoning setting** | **Agent software** | **Coding Agent Index** | **Total tokens per task** | 
|---|---|---|---|
| GLM-5.3, max | Opencode | 49 | 15.9 million | 
| **Mistral Large 4, high** | **Mistral Vibe** | **48** | **8.5 million** | 
| DeepSeek V4.1 Flash 1017, max | DeepSeek Harness | 46 | 16.7 million | 

*Source:* *Artificial Analysis’s October 29 assessment and follow-up posts**. Rankings refer to currently available open-weight entries.*

ML4 sits one point behind GLM-5.3 and two above DeepSeek. Its 8.5 million tokens per task represent about 47% less usage than GLM’s 15.9 million, supporting the efficiency comparison in Mistral’s announcement. Mistral characterizes the scores as effectively tied; Artificial Analysis ranks ML4 second. The reported one-point difference alone does not establish statistical significance.

These are comparisons of complete agent configurations. They do not isolate how the underlying models would perform if every other component were identical. The coding index also has its own mix of tests: FrontierCode v1.1, Terminal-Bench 4.0 and SWE-Atlas-QnA-AA. It is distinct from the broader Intelligence Index, which includes FrontierCode 2.0 and additional knowledge-work and reasoning evaluations.

That distinction matters when reading Mistral’s other coding claims:

| **Evaluation** | **Mistral’s published comparison** | **AA’s Intelligence Index evaluation, ML4 high** | 
|---|---|---|
| Terminal-Bench 4.0 | 41% | 25% | 
| FrontierCode 2.0 | 19.7% | 15% | 

Mistral reports that its Terminal-Bench result rose from a self-reported 28% for the preview to 41%, while output-token usage fell 30%. AA’s separate Intelligence Index assessment reports 25% on that test. These figures come from different reported evaluation contexts; the available material does not isolate the precise contribution of each software or configuration difference. The vendor’s 41% should not be substituted for AA’s 25%, or either result presented as universal performance for ML4.

The FrontierCode difference similarly changes the ranking: Mistral’s chart places its 19.7% ahead of DeepSeek’s 19.3%, whereas AA reports ML4 at 15%, behind DeepSeek at 29% and GLM-5.3 at 22%. The independent evaluation therefore does not support extending Mistral’s first-place FrontierCode claim beyond the company’s own comparison.

Mistral also reports a Surge Code blind preference study in which ML4 wins 64% of comparisons against GLM-5.3 and Kimi K3, 56% against DeepSeek V4.1 Flash, and 22% against Claude Opus 5.5. That is another type of evidence — human preference in the reported comparison — rather than a substitute for task-completion benchmarks.

Our original coverage focused on Mistral’s preliminary 62% DeepSWE result. The latest materials do not replace that number with an updated DeepSWE score, and neither Terminal-Bench nor the coding composite can be compared directly with it.

“But it’s really for developers at home, all the way to developers in the enterprise,” Janiewicz said. She said organizations could also adapt ML4 to their own software, making performance on internal work a further consideration beyond public tests.

## **A narrow lead outside China, with larger gains in efficiency**

The final model’s 26-point Intelligence Index score provides a like-for-like headline comparison with the preview’s 25 on v5. It is a modest increase, rather than evidence of a broad leap in capability. Artificial Analysis identifies token efficiency and reduced hallucination as the more substantial changes.

The geographic distinction also needs context. ML4’s one-point advantage over Muse Spark Open makes it the highest-scoring open-weight model developed outside China in AA’s assessment. Seven open-weight models score higher globally. DeepSeek V4.1 Flash 1017 and GLM-5.3 lead that group at 32, while MiMo-V2.6-Pro scores 30. Among all models, the current leaderboard places Gemini 4 Carbon at 59, Claude Opus 5.5 at 58 and GPT-6 Astra at 57 in the listed high-effort configurations.

AA identifies coding and agentic knowledge work as major sources of ML4’s gap with the open-weight leaders. In addition to its lower FrontierCode result, ML4 scores 1,349 Elo on AA-Briefcase, compared with 1,570 for DeepSeek and 1,504 for GLM. Against Muse Spark Open, however, AA highlights Terminal-Bench as ML4’s largest advantage: 25% versus 8% in the Intelligence Index evaluation.

Efficiency improves more sharply. AA reports an average of 82,000 output tokens per Intelligence Index task for the final model, compared with 135,000 for the preview — a 39% reduction. The seven open-weight models scoring above ML4 consume between 113,000 and 225,000 output tokens per task. That gives ML4 a useful balance of capability and output-token use, without establishing that it is the least expensive option.

Those output-only averages are separate from the coding index’s 8.5 million total tokens per task, which includes input and cached tokens as well as output. The two evaluations also use different task collections. Combining them would produce a misleading efficiency comparison.

There is one further historical distinction. Artificial Analysis gave the preview 38 at its October debut, before Intelligence Index v5 introduced harder evaluations and increased the share using private tests or withheld answers to 60%. The earlier 38 and today’s 26 belong to different scales; the relevant v5 comparison is 25 to 26.

## **Fewer hallucinations, but factual accuracy remains limited**

On AA-Omniscience v1.1, Artificial Analysis reports a hallucination rate of 28% for the released ML4, down from 41% for the preview and equal to Muse Spark Open’s result. That is a 13-percentage-point reduction on this test.

Its accuracy score is still 26%, compared with 27% for Ling 3.1 Flash 1019, 40% for DeepSeek V4.1 Flash 1017 and 42% for Muse Spark Open. Matching Muse’s hallucination rate therefore does not mean matching its factual accuracy.

AA-Omniscience evaluates factual knowledge and how models handle uncertainty. For enterprise buyers, the distinction matters: a system that produces fewer unsupported answers may be easier to supervise, but it still needs to answer enough of the relevant questions correctly. The benchmark does not establish ML4’s accuracy on a company’s proprietary records or its reliability across every production workflow.

## **Cybersecurity access is central to the pitch**

Artificial Analysis confirms a Cyber Index score of 51 for ML4 at high reasoning, putting it third among current open-weight models. DeepSeek V4.1 Flash 1017 at max leads with 57, followed by MiMo-V2.6-Pro at 56.

The component results show why the ranking is more nuanced than a single security score:

| **Cyber evaluation** | **Mistral Large 4, high** | **DeepSeek V4.1 Flash 1017, max** | **MiMo-V2.6-Pro** | 
|---|---|---|---|
| CyberGym-E2E-AA | 79% | 76% | 79% | 
| DeepsecBench-AA | 15% | 25% | 26% | 
| CWE-Bench-AA | 58% | 69% | 63% | 

*Source: Artificial Analysis’s follow-up to its October 29 assessment.*

ML4 beats DeepSeek and matches MiMo on CyberGym, but trails both on the other two components. The earlier Mistral slide’s 79% result is therefore supported by AA’s current CyberGym assessment; it is not the score for the full Cyber Index. The new breakdown gives security teams a more useful basis for choosing tests relevant to their own work than a claim of blanket cybersecurity leadership.

Mistral also reports a 96.6% average refusal rate on malicious cyber prompts drawn from JailbreakBench, StrongREJECT and AgentHarm. These are company-reported measurements on specified tests. The same blog acknowledges potential for the model to assist real attacks and says Mistral is modifying its guardrails.

Separately from the public-weight release, Mistral says enterprise design partners such as Orange Cyberdefense are getting preview access to a specialized variant for advanced security workflows. That access requires vetting and contractual safeguards, with testing restricted to owned or explicitly authorized systems in controlled environments.

Janiewicz said customers increasingly ask for defensive capabilities as they consider threats involving other models and agents. Her argument for releasing weights is that organizations should be able to shape the controls around those capabilities themselves.

“We believe that the best way for the customers to protect themselves is to have access to those models and be able to establish and customize some of those guardrails themselves,” she said.

That answers the question of who sets the controls, rather than proving that an openly distributed model cannot be repurposed for attacks. In the interview, Janiewicz did not present evidence that downstream users would be unable to remove or modify safeguards. The trade-off is that giving legitimate operators more control also reduces the original provider’s control over subsequent deployments.

For an enterprise security team, the potential benefit is continuity: a locally operated model could remain available during an incident without requiring a remote service to approve each request. The corresponding responsibility is to test its behavior and constrain its access to production systems. Downloading weights does not itself establish that an autonomous security agent is safe to run.

## **More enterprise benchmarks, with different baselines**

The public blog adds measurements beyond the coding and cyber results in the earlier materials:

| **Evaluation** | **Earlier checkpoint cited by Mistral** | **New result** | **What changed** | 
|---|---|---|---|
| Finch financial workflows | Preview: 67.4% | 73.3% | Up 5.9 percentage points; behind Kimi K3’s 77.3% in the chart | 
| BrowseComp web research | Early supervised-fine-tuning checkpoint: 75% | 84.5% | Up 9.5 points from that early checkpoint, which is not identified as the October preview | 
| B3 Agent Security attack resistance | Preview: 93.3% | 97.1% | Up 3.8 points on the reported test | 

The original VentureBeat article rounded the early Finch result to 67%; the published release now provides the more precise baseline and an updated score. These are benchmark gains, not measurements of customer revenue or productivity. The B3 result likewise measures resistance to the tested attacks; it does not establish that every deployed agent will be safe without additional controls.

## **Customization is the business — measured results still take time**

Janiewicz repeatedly returned to industries where proprietary information and specialized processes determine whether a model is useful. She pointed to manufacturing and financial services, citing BMW crash-testing work and HSBC customer-verification workflows as examples of the broader enterprise problems Mistral targets. Those examples should not be mistaken for published results from production deployments of the newly released ML4.

“The models we build are purpose built for the industries we serve,” she said.

She described two routes for customization: technically capable organizations can use Mistral Forge themselves, while others can work with Mistral’s deployment engineers and applied scientists. In either case, the commercial proposition includes the work needed to adapt a model to the customer’s operations.

That distinction helps explain the strategy.

For example, a manufacturer may care less about which system answers the broadest range of questions than whether it interprets the company’s engineering documents reliably, respects access restrictions and works within an approved computing environment. A bank may prioritize consistent handling of its own customer records and review procedures. These are potential applications of the approach, not measured outcomes established by the release.

Janiewicz said some existing customers are seeing costs decline. “A lot of our customers, particularly over the last six months, are really beginning to see their bills decreasing,” she said.

But when asked specifically for business results attributable to ML4, she said systems incorporating the new model were still being built and that meaningful operational measurements would take longer.

The published blog includes qualitative endorsements from organizations including Belfius, ABN AMRO, ASML and the European Space Agency, alongside security partners.

Those statements describe interest, testing and deployment possibilities; they do not provide quantified ML4-specific business results from which to calculate the return on a deployment investment.

For Mistral, the new evidence strengthens a specific proposition: downloadable weights, a competitive coding-agent setup and a materially less expensive model than its own preview. Whether that combination outweighs cheaper, higher-scoring alternatives depends on an organization’s workloads, deployment requirements and reasons for wanting control of the model.
