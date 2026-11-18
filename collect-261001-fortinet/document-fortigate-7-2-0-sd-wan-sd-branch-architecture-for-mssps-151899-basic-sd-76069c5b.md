Basic SD-WAN/ADVPN design
Basic SD-WAN/ADVPN design
We have already noted that the fundamental building block of our SD-WAN/ADVPN solution is the Hub-and-Spoke overlay topology that securely interconnects the SD-WAN sites:
We call each Hub-and-Spoke block a region. Every region is typically served by either one or two Hubs. Dual-Hub regions are the most recommended, for redundancy reasons:
It is common for the entire SD-WAN network to consist out of a single region, but large-scale deployments (with thousands of sites) will be multi-regional:
In a multi-regional deployment, the Hubs will typically build a Full Mesh between them. ADVPN can be enabled within each region, but it can also stretch across the regions, allowing to build inter-regional shortcuts, both Spoke-to-Spoke (between the Spokes in different regions) and Spoke-to-Hub (towards the Hubs serving other regions), as demonstrated on the above diagram.
Let's see how the SD-WAN nodes are configured: