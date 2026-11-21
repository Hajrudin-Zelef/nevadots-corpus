---
id: collect-261121-veille-ia/grok-xai/ldp-hello-adjacencies-not-forming
title: "LDP hello adjacencies not forming"
domain: grok-xai
role: reference
task: article
actors: ["xAI"]
dates: []
keywords: ["hello", "running", "mpls", "circle", "ldp", "adjacencies", "forming", "vpls"]
source: https://forum.mikrotik.com/t/ldp-hello-adjacencies-not-forming/164541
source_anchor: ""
source_lines: [1, 3]
section: "Grok_xAI"
---
I am doing some testing with a handful of RB760iGS running 6.49.7 regarding MPLS/VPLS with LDP neighbors.   If you view the diagram below I have MPLS/VPLS/LDP running without issue in the black circle.   However,  I can not get MPLS/LDP running in the red circle despite my best efforts.   I placed a small unmanaged switch between the two routers in the red circle and started a packet capture and can see the LDP hello packets.

I am at a loss,  I tried first in EVE-NG, reconfiguring from scratch, and finally moving to physical gear with no success.     Any suggestions would be greatly appreciated.
