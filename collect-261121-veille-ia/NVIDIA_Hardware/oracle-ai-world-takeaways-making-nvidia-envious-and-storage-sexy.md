---
id: collect-261121-veille-ia/nvidia-hardware/oracle-ai-world-takeaways-making-nvidia-envious-and-storage-sexy
title: "Oracle AI World takeaways: making Nvidia envious, and storage... sexy?"
domain: nvidia-hardware
role: reference
task: article
actors: ["NVIDIA"]
dates: []
keywords: ["oracle", "cloud", "going", "world", "tpus", "make", "google", "there"]
source: https://www.datacenterdynamics.com/en/opinions/oracle-ai-world-takeaways-making-nvidia-envious-and-storage-sexy/
source_anchor: ""
source_lines: [1, 57]
section: "NVIDIA_Hardware"
---
Who knew the AI cloud world would increasingly become a pick-and-mix of chips?

After a day at Oracle AI World’s Las Vegas event centered primarily on applications (understandable, given the company’s focus, but nonetheless not where *DCD* gets its kicks), day two of the event saw a tidal wave of chip news thrust upon us.

And while Nvidia is, obviously, still the big dog, Oracle is embracing other chip companies and expanding beyond the GPU architecture.

Perhaps most exciting from our point of view is the fact that Oracle is set to provide access to Google’s custom TPUs (tensor processing units), a move that will make it the first hyperscaler to offer the chips outside of Google Cloud Platform itself.

Why is this exciting? Well, it's just a little odd, but then it's been odd since Google announced that it would no longer keep TPUs as its secret sauce, and would instead offer them to a “select group of customers,” later announcing a joint venture with Blackstone to set up another TPU-powered cloud platform.

One can only assume that a) demand is insane for compute (which we know), and b) Google has stocks of TPUs that it doesn’t have the data center space or power ready for yet, and this will prevent a valuable asset from sitting idle.

*DCD* spoke to Oracle about the partnership, with Karan Batta, SVP of Oracle Cloud Infrastructure, telling us: “What I can tell you is that this is not an experiment. This is going to be a cloud gigawatt-scale investment from Oracle over time, and the best way to think of it is that when Google launches a new TPU, we will have it on day one.”

An experiment in terms of commitment to the deployment it indeed isn’t, especially when you agree to purchase gigawatts of chips and do so over many years.

But there is a degree of experimentation that plays into the overall strategy.

Batta explains that in the current context, Oracle is operating in a “very supply-constrained world, but we are also living in a world where costs are increasing, whether it's memory or SSDs and storage,” and therefore, the company's attitude is “the more flexibility and choice that people have, the better decisions they can make.”

With this in mind, Oracle is also negotiating how some large customers purchase capacity in terms of power, signing on for 100MW, or even 1GW of capacity, several years in the future.

“But in three years' time, we have no idea what supply is going to look like. We don't know what the market's going to do in terms of cost. And so you want to be able to make decisions, and you may not know what you're going to put into that data center that far out," Batta said. "So you need extreme choice at that point. Are you putting a high-powered rack? Are you putting in a low-powered rack? Are my needs going to change because I'm having a lot more inference? Are my needs going to change because there's some breakthrough in model training that I need to run?

“So there are lots of these characteristics, not just the workload side, but also the supply side, the cost side, the market dynamics, and so if you already have the power, you want to be able to make those decisions as late as possible.”

Beyond that, there is also the question of power utilization. GPUs create a high-power-draw rack, while TPUs are a low-power-draw rack. With that in mind, Oracle needs to be ready for that reality. “There are all these different variables that you have to sort of put into the calculator and be like, great, I'm going to do this. So, but to be able to do that, we need to be able to validate TPUs and make sure that they work and are available on the cloud.”

The TPUs are well established. As Mahesh Thiagarajan, EVP and head of Oracle Cloud Infrastructure, notes during a press conference, “Google is in the long lineup of great technology, but they also come with a different attitude because it's the hardware that actually trained Gemini and partially trained Anthropic. So what you have is that credibility from them, their technology, all this technology, all these data centers, all this network. You combine that together, you reach a new market.”

Beyond Google (and, of course, Oracle’s long-standing partnerships with Intel, AMD, and Nvidia), some more interesting emergents are Cerebras, Positron AI, and SambaNova.

According to Thiagarajan, this is in part due to the sheer level of demand Oracle is seeing. He told *DCD* that everything they are bringing online is sold out for 2027 and 2028 already, and demand for AI compute is “insatiable.” Simply put, the more diversity in chips, the larger quantity of chips Oracle can get online “fast-tracked” and deliver to customers.

But also, looking ahead, Thiagarajan suggests that AI will trend towards more “specialized” inferencing workloads, and chips of the ilk of Cerebras, SambaNova, and Positron, all of which are optimized for specific requirements rather than taking the generalized approach of the CPU or GPU chip, could position Oracle to support those workloads in the future.

He explains: “If you look at the partnership we announced bringing Cerebras online in the first chapter of next year, or bringing SambaNova online, or bringing Positron online. Each one of them has some uniquely different work.

“The way I think about why they actually make sense is that AI is getting more specialized. I think we're in like round one of what is a large wave. People don't understand the TAM (total addressable market) of what we're talking about. The TAM is exceptionally high, and AI is the first phase, and you combine the two, and you look at what the next phase of AI, and what the two years are going to look like, the four years are going to look like, what the six years are going to look like, is we're going to get into these very specialized inference opportunities that actually can bring the dollar down.”

Beyond the chip talk, an area that went mostly under the radar but definitely deserves attention is Oracle’s shifting storage strategy.

The company announced partnerships with the likes of Vast Data and WEKA, both of which offer more intelligent data storage solutions optimized for the era of AI.

*DCD* spoke to Oracle’s Rob Czarnecki, VP of storage & data services, about the subject.

“We don't need to do all that work ourselves. You know, we're not building chips, but we're offering all the chips from all the providers. So why shouldn't we do the same for storage?” he argues, explaining that AI is bringing new challenges to the storage world - data volumes are dramatically increasing, and there are varying attitudes towards storage duration, such as the need for “hot storage” to feed AI models and the proportion that should go into archive, and so on - and there are companies out there that have already done the hard work of finding solutions.

Currently, Oracle is working with them on a software basis alone, as this is the “fast path for us to put this in the customers' hands,” and if it resonates, then there is the potential to adopt the hardware solutions as well.

“With all of this focus on storage and AI and changing usage patterns, it's really exciting that storage is back in conversation, that it's at the heart of this AI revolution. I think it's giving some long overdue attention to some really smart people that have been working behind closed doors delivering the durability and reliability that businesses have been built on, and I think that there's an opportunity for storage to accelerate that AI flywheel and to offer the right options at the right time,” he said, adding that in the future (over the next few years), he expects storage to go through some new iterations and generate conversations.

“I think there's a really interesting narrative on how AI can better inform and manage existing storage systems and make them smarter for the customers. Does that make an archive? Does that pull a cache up? Does that actually need to move? There are a bunch of interesting questions that I think we're gonna see folks really spend time innovating on this over the coming years.”

This only scratches the surface of Oracle AI World, but one thing has been made clear to *DCD.* The world of AI hardware is not just limited to the next generation of GPU; the conversation is getting more interesting, and the problem more complicated. Get excited.

## Comments
