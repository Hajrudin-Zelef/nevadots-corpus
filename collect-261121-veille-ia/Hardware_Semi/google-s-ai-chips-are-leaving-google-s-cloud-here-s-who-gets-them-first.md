---
id: collect-261121-veille-ia/hardware-semi/google-s-ai-chips-are-leaving-google-s-cloud-here-s-who-gets-them-first
title: "Google's AI chips are leaving Google's cloud. Here's who gets them first."
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["google", "oracle", "cloud", "enterprise", "first", "infrastructure", "chip", "running"]
source: https://www.techtimes.com/articles/329628/20261028/oracle-ai-world-2026-deal-that-just-rewired-enterprise-ai-hardware-race.htm
source_anchor: ""
source_lines: [1, 37]
section: "Hardware_Semi"
---
Google's 8th-generation Tensor Processing Units — the custom silicon behind every major Google AI model — are coming to Oracle Cloud Infrastructure beginning in 2027, the headline move from a four-day enterprise summit that wrapped Wednesday in Las Vegas and the first time in the chip's 11-year history that enterprise customers can access Google TPUs without running workloads inside Google Cloud.

The deal redraws the map of enterprise AI hardware access at a moment when fewer than 20% of AI-driven business decisions in live production systems carry a named human owner — a governance gap that leaves the infrastructure race running well ahead of accountability.

## Google's TPUs Step Outside Google's Cloud — For the First Time

Oracle and Google revealed the partnership Tuesday at The Venetian Convention and Expo Center, where tens of thousands of enterprise attendees gathered for Oracle's renamed flagship conference — formerly Oracle CloudWorld, now repositioned entirely around enterprise AI.

The arrangement makes Oracle Cloud Infrastructure the first hyperscaler outside Google to offer TPUs to external customers. Karan Batta, Senior Vice President, Product, Oracle Cloud Infrastructure, told *Data Center Dynamics* at the event: "This is going to be a cloud gigawatt-scale investment from Oracle over time," adding that when Google releases a future TPU generation, Oracle will carry it from day one. Amin Vahdat, Google's Senior Vice President and Chief Technologist for AI and Infrastructure, framed the arrangement as extending flexibility for organizations running complex AI workloads — language that positions the deal as a distribution play rather than a concession to rivals.

Initial deployment capacity was not specified. Availability is confirmed for calendar year 2027, with Oracle and Google describing a multi-year, multi-gigawatt ramp-up.

## Two Chips, Two Entirely Different Problems

Google's 8th-generation portfolio divides into architecturally distinct products targeting separate phases of the AI workload lifecycle. The TPU 8t handles large-scale model training: a 9,600-chip superpod delivers 121 ExaFLOPs of peak FP4 compute alongside two petabytes of shared high-bandwidth memory. The chip retains SparseCore — an on-chip unit that has been part of Google's TPU design since the fourth generation, handling the irregular embedding lookups that routinely bottleneck mixture-of-experts model training without consuming cycles from the main compute fabric. The 8i's inference-first design removes SparseCore in favor of a Collectives Acceleration Engine optimized for agent latency.

The TPU 8i targets production inference, especially the multi-step agentic pipelines now central to enterprise AI deployments. Its Boardfly interconnect topology cuts maximum network diameter by more than 50% — from 16 hops on a standard 3D torus to 7 hops at comparable pod scale — directly compressing the cumulative latency that makes agentic reasoning costly at production volume. Google claims 80% better inference performance per dollar against the prior generation's inference chip, a vendor figure worth independent validation before driving procurement decisions.

Oracle supplies the physical layer: power, cooling, and its Acceleron networking platform, which connects TPU clusters to OCI's broader compute and storage fabric using AMD Pensando DPUs, Zero Trust Packet Routing, and optical links running at 400G and 800G.

## When Hardware Isn't the Core Problem

The TPU deal dominated the conference's final day, but Oracle spent three earlier days addressing a failure mode that faster silicon cannot fix: AI agents querying correct data while reasoning from the wrong definition of what that data actually means.

On Monday, Oracle announced the Business Semantic Layer, built into its Autonomous AI Lakehouse. The layer codifies enterprise metric definitions — revenue, churn rate, gross margin — once, drawing from catalog annotations, SQL descriptions, and AI-suggested analytic views, then makes those definitions available automatically across Oracle's Select AI interface. Any agent querying the platform references the same governed definition rather than inferring meaning from raw column names. The Business Semantic Layer is expected to be available within 12 months.

A separate announcement, the Business Simulation Engine, brings Oracle Essbase — a multidimensional OLAP engine that entered commercial production in 1992 — directly into the Lakehouse environment. Enterprises can run historical analytics and forward-looking scenario modeling in a single platform without routing planning data through standalone EPM tools such as Anaplan or OneStream. Unlike the Business Semantic Layer, the Simulation Engine does not carry an explicit 12-month availability qualifier in Oracle's official announcement; general availability timing has not been separately specified and pricing for both capabilities remains unannounced.

Pareekh Jain, principal analyst at Pareekh Consulting, assessed the combined approach, suggesting it could simplify data architecture and accelerate decisions for CIOs managing fragmented planning and analytics stacks — while noting that the semantic layer's real-world value depends entirely on the quality of definitions loaded into it from the start.

## The Governance Problem That Infrastructure Cannot Solve

Oracle AI World's infrastructure narrative landed against a troubling backdrop for the industry. Research published last week by Sopra Steria Next found that fewer than 20% of AI-influenced decisions across large organizations carry a named business owner with explicit rules and formal accountability, and that under 25% of production AI decisions can be traced and replayed end-to-end. Those figures describe live systems — not pilots — already executing binding choices about customers, credit, and operations.

EU AI Act high-risk obligations — covering employment screening, credit scoring, healthcare resource allocation, and critical infrastructure — take effect December 2, 2027 under Regulation EU 2026/1744. Initial compliance costs for large enterprises are estimated between $8 million and $15 million. Exposure through enforcement and litigation runs structurally higher.

Valérie Thiblet, Partner in Data & AI at Sopra Steria Next, framed the governance imperative clearly: "A system that cannot be stopped in real time is not governed," she said. Oracle's week in Las Vegas demonstrated that the enterprise AI infrastructure stack is maturing at pace. Whether the accountability layer catches up before December 2027 remains the most consequential open question for organizations scaling AI in regulated industries.
