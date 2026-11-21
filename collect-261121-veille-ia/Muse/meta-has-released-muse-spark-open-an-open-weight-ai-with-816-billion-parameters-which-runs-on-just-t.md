---
id: collect-261121-veille-ia/muse/meta-has-released-muse-spark-open-an-open-weight-ai-with-816-billion-parameters-which-runs-on-just-t
title: "Meta has released 'Muse Spark Open,' an open-weight AI with 816 billion parameters, which runs on just two Mac Studios."
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: []
keywords: ["muse", "meta", "spark", "open", "model", "parameters", "released", "billion"]
source: https://gigazine.net/gsc_news/en/20261016-meta-muse-spark-open/
source_anchor: ""
source_lines: [1, 89]
section: "Muse"
---
# Meta has released 'Muse Spark Open,' an open-weight AI with 816 billion parameters, which runs on just two Mac Studios.


 On October 15, 2026, Meta released 'Muse Spark Open,' an AI model with 816 billion parameters. It is an open-weight model that allows users to download the internal data of the AI model and use it in their own environment, and it also supports local operation on 'Mac Studio' in cooperation with Apple.

 **Muse Spark Open | Meta** 

**Introducing Muse Spark Open, The Most Capable US Open‑Weight Model to Date | Meta AI Research
https://research.meta.ai/blog/muse-spark-open**

https://research.meta.ai/blog/muse-spark-open

**Local and Distributed Large LLM Inference with MLX and RDMA on Mac Studio - Apple Machine Learning Research
https://machinelearning.apple.com/research/mlx-distributed-inference**

https://machinelearning.apple.com/research/mlx-distributed-inference

 Today we're releasing Muse Spark Open, the most capable US open-weight model to date.

 Built for coding and agentic work, it excels at long-horizon tasks, complex software engineering, and multi-step reasoning.

 Key Model Specifications:

 → 816B total / 42B active parameters

 → 1M token… pic.twitter.com/yAtoxNeYJm



 Meta announced its AI model 'Muse Spark' in April 2026 and has since continued to improve the model for AI agents that automate programming and complex tasks. In August 2026, they also released a relatively small open model called 'Muse Glimmer,' but the high-performance Muse Spark series was mainly provided through Meta's services and APIs.

 **Meta releases 'Muse Glimmer,' an open model that runs locally, and is more powerful than Google's Gemma 4 31B on the 30B - GIGAZINE**


 When integrating AI models into a company's own system, using external services can impose restrictions on usage fees and data handling. Therefore, Meta has released Muse Spark Open, which is based on Muse Spark but with additional security measures, allowing it to run on a company's own servers or compatible computers.

 Muse Spark Open has a total of 816 billion parameters, but thanks to a mechanism called 'Mixture of Experts (MoE)' that selects and runs only the necessary parameters from multiple specialized processing parts, it uses only about 42 billion parameters during processing. It can also handle information of up to 1 million tokens at once and supports text, image, video, and audio input. It can be used as an agent to read large amounts of program code to find areas for correction, or to work with multiple tools, and can even handle long-term tasks where it checks the results and corrects problems that occur along the way.

 The performance comparison released by Meta is as follows: It scored 61.8 in 'JobBench,' which solves work-related problems; 89.2 in 'MCP-Atlas,' which evaluates the ability to use external tools; and 26.1 in 'HLE-Diamond,' which measures expertise and reasoning ability. While it outperformed other companies' AI models in several categories, other companies' models scored higher in some categories, such as programming ability.

 


 Furthermore, in the ' 


 On the other hand, the sheer scale of 816 billion parameters presents a major challenge when running the AI model on your own computer. Saving Muse Spark Open at normal accuracy requires approximately 1.6TB of memory just for the model data, making it difficult to run on a typical personal computer.

 Therefore, Meta collaborated with Apple to develop a way to run Muse Spark Open on Mac Studio using MLX, an AI development platform optimized for Apple chips. By using 'quantization,' which adjusts the accuracy of the data to reduce its size, the model size was reduced to approximately 493GB. Furthermore, by distributing the processing across multiple Mac Studios, they were able to run the massive AI model in a local environment.

 **This is what happens when you create an AI cluster with 1.5TB of VRAM using four Mac Studios and RDMA over Thunderbolt - GIGAZINE**


 The mechanisms Apple has revealed for distributing processing across multiple computers are as follows: The 'pipeline parallelization' on the left divides the processing of the AI model into two halves and executes them separately, while the 'tensor parallelization' on the right divides the same processing among multiple computers.

 


 Apple measured the performance of Muse Spark Open by connecting two or four Mac Studios equipped with the M5 Ultra. Thunderbolt 5 was used to connect the Mac Studios, utilizing RDMA technology, which enables high-speed data transfer between computers in memory.

 First, the following graph compares the time it takes from asking the AI a question until the first character of the answer is generated. A larger number indicates a shorter waiting time, with blue representing the results when 8192 tokens of information were input, and green representing the results when 65,536 tokens were input. When tensor parallelization was performed using two Mac Studios, Muse Spark Open was 1.03 times faster with 8192 tokens of input and 0.68 times faster with 65,536 tokens. The comparison standard for Muse Spark Open is when pipeline parallelization is used with two Mac Studios, while the other AI models are compared using one Mac Studio. In Muse Spark Open, tensor parallelization actually increased the waiting time when processing long inputs.

 


 On the other hand, increasing the number of Mac Studio instances to four improved performance by 1.76 times with 8192 tokens and 1.17 times with 65,536 tokens. This shows that increasing the number of computers handling the processing can shorten the time it takes for the first answer to be displayed.

 


 The following graph shows the results of comparing the text generation speed. It measures how quickly text can be output from the start of response generation. Using two Mac Studio machines, the generation speed of Muse Spark Open improved by 1.59 times with 8192 tokens and by 1.66 times with 65,536 tokens. Unlike the time it takes for the first response to appear, the speed improved even with longer inputs.

 


 Using four Mac Studio machines resulted in even faster performance, achieving a 2.33x speed increase with 8,192 tokens and a 2.41x speed increase with 65,536 tokens.

 


 According to Apple, using four Mac Studio machines, Muse Spark Open can generate up to approximately 36.7 tokens per second, exceeding the average human reading speed.

 Muse Spark Open can be downloaded from the AI model sharing service 'Hugging Face' and can be used on Apple hardware as well as environments from NVIDIA, AMD, and others. Meta has released Muse Spark Open under its own license, and developers can also customize the AI model according to their needs.

 **Muse Spark Open - a meta-models Collection https://huggingface.co/collections/meta-models/muse-spark-open**

Meta has indicated its intention to continue providing an environment where developers can freely use high-performance AI models.

**Related Posts:**
