---
id: collect-261121-veille-ia/langages-dev/openzl-using-graphs-to-compress-smaller-and-faster
title: "OpenZL: Using Graphs to Compress Smaller and Faster"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["openzl", "compression", "faster", "compressors", "data", "using", "graphs", "compress"]
source: https://arxiv.org/abs/2605.09928v1
source_anchor: ""
source_lines: [1, 23]
section: "Langages_Dev"
---
# Computer Science > Information Retrieval

# Title:OpenZL: Using Graphs to Compress Smaller and Faster

View PDF
            Abstract:In the last few decades, research techniques have improved lossless compression ratios by significantly increasing processing time. However, these techniques have not gained popularity in industry because production systems require high throughput and low resource utilization. Instead, real world improvements in compression are increasingly realized by building application-specific compressors which can exploit knowledge about the structure and semantics of the data being compressed. Application-specific compressor systems outperform even the best generic compressors, but these techniques have severe drawbacks -- they are inherently limited in applicability, are hard to develop, and are difficult to maintain and deploy. In this work, we show that these challenges can be overcome with a new compression strategy. We propose the "graph model" of compression, a new theoretical framework for representing compression as a directed acyclic graph of modular codecs. OpenZL implements this framework and compresses data into a self-describing wire format, any configuration of which can be decompressed by a universal decoder. OpenZL's design enables rapid development of application-specific compressors with minimal code. Experimental results demonstrate that OpenZL achieves superior compression ratios and speeds compared to state-of-the-art general-purpose compressors on a variety of real-world datasets. Compared to ratio-focused deep-learning compressors, OpenZL is competitive on ratio while being many orders of magnitude faster. Internal deployments at Meta have also shown consistent improvements in size and/or speed, with development timelines reduced from months to days. OpenZL thus represents a significant advance in practical, scalable, and maintainable data compression for modern data-intensive applications.
    

# Bibliographic and Citation Tools

# Code, Data and Media Associated with this Article

# Demos

# Recommenders and Search Tools

# arXivLabs: experimental projects with community collaborators

arXivLabs is a framework that allows collaborators to develop and share new arXiv features directly on our website.

Both individuals and organizations that work with arXivLabs have embraced and accepted our values of openness, community, excellence, and user data privacy. arXiv is committed to these values and only works with partners that adhere to them.

Have an idea for a project that will add value for arXiv's community? **Learn more about arXivLabs**.
