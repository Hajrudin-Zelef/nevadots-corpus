---
id: collect-261121-veille-ia/langages-dev/openzl-a-graph-based-model-for-compression
title: "OpenZL: A Graph-Based Model for Compression"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["compression", "openzl", "model", "data", "graph-based", "these", "compressors", "arxivlabs"]
source: https://arxiv.org/abs/2510.03203
source_anchor: ""
source_lines: [1, 24]
section: "Langages_Dev"
---
# Computer Science > Information Retrieval

# Title:OpenZL: A Graph-Based Model for Compression

View PDF HTML (experimental)
            Abstract:Research techniques in the last decade have improved lossless compression ratios by significantly increasing processing time. These techniques have remained obscure because production systems require high throughput and low resource utilization. In practice, application-specific compression algorithms that leverage knowledge of the data structure and semantics are more popular. Application-specific compressor systems outperform even the best generic compressors, but these techniques have some drawbacks. Application-specific compressors are inherently limited in applicability, have high development costs, and are difficult to maintain and deploy.

In this work, we show that these challenges can be overcome with a new compression strategy. We propose the "graph model" of compression, a new theoretical framework for representing compression as a directed acyclic graph of modular codecs. OpenZL compresses data into a self-describing wire format, any configuration of which can be decompressed by a universal decoder. OpenZL's design enables rapid development of tailored compressors with minimal code; its universal decoder eliminates deployment lag; and its investment in a well-vetted standard component library minimizes security risks. Experimental results demonstrate that OpenZL achieves superior compression ratios and speeds compared to state-of-the-art general-purpose compressors on a variety of real-world datasets. Internal deployments at Meta have also shown consistent improvements in size and/or speed, with development timelines reduced from months to days. OpenZL thus represents a significant advance in practical, scalable, and maintainable data compression for modern data-intensive applications.

# Bibliographic and Citation Tools

# Code, Data and Media Associated with this Article

# Demos

# Recommenders and Search Tools

# arXivLabs: experimental projects with community collaborators

arXivLabs is a framework that allows collaborators to develop and share new arXiv features directly on our website.

Both individuals and organizations that work with arXivLabs have embraced and accepted our values of openness, community, excellence, and user data privacy. arXiv is committed to these values and only works with partners that adhere to them.

Have an idea for a project that will add value for arXiv's community? **Learn more about arXivLabs**.
