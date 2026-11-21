---
id: collect-261121-veille-ia/grok-xai/grok-4-6-now-available-on-snowflake-cortex-ai
title: "Grok 4.6 Now Available on Snowflake Cortex AI"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["snowflake", "grok", "work", "across", "cortex", "frontier", "data", "complex"]
source: https://www.snowflake.com/en/blog/announcing-grok-4-6-snowflake-cortex-ai/
source_anchor: ""
source_lines: [1, 81]
section: "Grok_xAI"
---
We are thrilled to announce the public preview availability of Grok 4.6, SpaceXAI's latest frontier model, on Snowflake Cortex AI. Grok 4.6 is built for long-running agents and ambitious interactive and visual work, staying with complex tasks across many steps, whether researching a topic, analyzing information, working across a codebase, or turning an idea into a polished application or work artifact. It delivers frontier intelligence across agentic coding and knowledge work benchmarks, making it a valuable addition to the leading frontier models available within the secure Snowflake perimeter. It is available for use with Snowflake Cortex AI Functions and Cortex Inference, with Snowflake CoCo and Snowflake CoWork coming soon.

## Grok 4.6

Grok 4.6 is SpacexAI's most capable frontier model to date, sustaining complex tasks over many steps, whether researching a topic, analyzing information, working across a codebase, or turning an idea into a polished application or work artifact. It achieves frontier intelligence across several agentic coding and knowledge work benchmarks on the Artificial Analysis Intelligence Index, a composite score of nine benchmarks.


<sub>Source: xAI, Aug 12, 2026</sub>


## What you can do with Grok 4.6 on Snowflake

Grok 4.6 joins other leading frontier models in Snowflake Cortex AI, enabling a wide range of enterprise use cases, all within Snowflake's perimeter.

With Grok 4.6, you can:

- **Speed up development with Snowflake CoCo:** Snowflake's AI coding agent understands your enterprise data, governance policies and workflows, enabling teams to go from idea to production quickly without breaking trust or impacting scale. Now, with Grok 4.6, CoCo enables improved reasoning, code generation, support for more complex, long-running tasks and agentic execution, bringing secure, Snowflake-aware assistance directly into the CoCo desktop, Snowsight interface, the CLI and local dev tools. For data engineers, analysts and builders, Grok 4.6 is especially strong at turning a broad product idea into a working first version, it can research unfamiliar domains, structure the application, implement the core interactions, and continue refining the result through several rounds of feedback.

- **Build advanced personal work agents:** Snowflake CoWork is the personal work agent for every knowledge worker. From Day 1, it understands your data and your work, answering difficult "why" questions with cited research and running tasks on demand, on schedule and across the tools your business already uses. Every knowledge worker can reason deeply over all your enterprise data, automate routine work and move faster from idea to decision to action. Grok 4.6 on Cortex AI is built for long-running agents, sustaining complex, multi-step tasks across many steps and checking its own work as it goes, so CoWork can carry a project from research through action with greater consistency and less supervision, all optimized with Snowflake's security, governance, and context. With Deep Research, proactive automations, reusable User Skills, MCP connectors for Gmail, Jira, Slack and Salesforce and a native mobile app that puts your work agent wherever you are, all inside Snowflake's governance perimeter, every knowledge worker spends less time searching, waiting and coordinating, and more time making the decisions that move the business forward.

- **Analyze structured and unstructured data with unparalleled precision:** Cortex AI Functions make it easy to build scalable AI pipelines across multimodal enterprise data using SQL. They offer high-performance processing at a lower cost than manually orchestrated AI pipelines, enabling trusted insights across the enterprise while maintaining the security and governance capabilities Snowflake is known for. With Grok 4.6, you can apply frontier reasoning to complex, multi-step analysis directly in your pipelines, summarizing documents, classifying records and extracting insight across large volumes of enterprise data. For example, with the AI_COMPLETE function, analysts can apply natural language instructions directly in SQL using models such as grok-4-6-fast.


```
SELECT AI_COMPLETE(
'grok-4-6-fast', 
PROMPT('Review the following financial filing and summarize key revenue trends, margin changes, and any notable risk factors mentioned: {0}', my_table.filing)) FROM my_table
```
- **Developers can build AI applications closer to their secure data:** Use the Cortex Inference to build agentic enterprise applications within the Snowflake security perimeter. Grok 4.6's OpenAI-compatible endpoint makes it easy to get started using the OpenAI SDK by pointing it at your Snowflake Cortex REST API endpoint, enabling a seamless integration with familiar OpenAI tooling, while the dedicated fast variant lets you balance reasoning depth and latency on a task. You can get started quickly using the example below.


```
from openai import OpenAI
client = OpenAI(
    api_key="<SNOWFLAKE_PAT>",
    base_url="https://<account-identifier>.snowflakecomputing.com/api/v1/openai"
)
response = client.chat.completions.create(
    model="grok-4-6",
    messages=[
        {
            "role": "system",
            "content": "You are a helpful business analyst"
        },
        {
            "role": "user",
            "content": 
                "Sales are down 18% in Q2. We had lower ad spend and reduced campaign performance in APAC. Can you analyze the image below to examine the cause?"
        },
        {
            "type": "image_url",
            "image_url": {
                "url": "<base64 url>"
            }
        }
    ]
)
print(response.choices[0].message)
```
## Use cases with Grok 4.6

Grok 4.6 underwent a longer supplemental training run than Grok 4.5, with curated model-generated data for reasoning and advanced technical concepts, high-quality engineering data, and an improved optimizer and training recipe. This produced a stronger foundation for the SFT and RL stages that followed. SpaceXAI then used Grok 4.5 to regenerate the SFT trajectories across reasoning efforts, agent harnesses, and domains such as STEM, software engineering, and knowledge work, filtering out problematic traces with model-based checks. The resulting checkpoint shows strong performance and improved behavior.

Grok 4.6 is trained on a wide range of agentic RL tasks, including knowledge work, general coding, and domain-specific environments for kernel optimization, web development, and computer-aided design. On longer trajectories, the model also began to show more self-testing and verification, checking its own work before moving on, and it produces stronger first passes on visual and interactive projects than Grok 4.5, establishing structure and visual language for an application in a single pass.

## Safety and capabilities

Grok 4.6's safeguards have been improved and calibrated in line with the model's capabilities. SpaceXAI's safety stack is designed to maximize utility and security across legitimate use cases. Grok 4.6 supports use cases including vulnerability patching, accelerating the engineering design cycle, and augmenting AI research. This reflects Grok 4.6's expanded capabilities, with SpaceXAI's widest-ever suite of pre-deployment testing for capabilities and safeguard calibration, as well as extensive post-deployment and third-party testing.

Moving agentic AI from concept to production is now a strategic imperative. Powered by Grok 4.6, agents can handle complex, long-running tasks with greater consistency and reduced supervision, driving real work forward at scale. With multiple entry points from CoCo Desktop to CoWork, Cortex AI Functions and Cortex AI Gateway, customers can easily adopt this capability and accelerate their path to high-impact, production-ready agentic AI at scale.

## Learn more

- Build agentic applications with Cortex Agents: Get started with Cortex Agents and learn how to build AI experiences grounded in your enterprise data.
- Get started with Snowflake CoWork: Learn more about Snowflake CoWork and try it out with our getting started guide.
- Start building with Snowflake CoCo: Learn more about Snowflake CoCo and get hands-on with the CoCo Desktop getting started guide.


<sub>Forward-looking statements</sub>

<sub>This article contains forward-looking statements, including about our future product offerings, and are not commitments to deliver any product offerings. Actual results and offerings may differ and are subject to known and unknown risk and uncertainties. See our latest 10-Q for more information.</sub>
