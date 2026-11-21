---
id: collect-261121-veille-ia/autres/ospf-mpls-vpls
title: "OSPF+MPLS+VPLS"
domain: autres
role: reference
task: article
actors: []
dates: []
keywords: ["vpls", "address", "ospf", "mpls", "help", "please", "both", "config"]
source: https://forum.mikrotik.com/t/ospf-mpls-vpls/135820
source_anchor: ""
source_lines: [1, 41]
section: "Autres"
---
vpls one side is up and other end is down help please, i have checked LDP and MTU on both routers but still its the same and i m stuck

             
            
           
          
            
            
              We need more info  so we can help.

Please state config, cenario, etc…

             
            
           
          
            
            
              Are you sure you have LDP enabled on every LSR between LER’s?

             
            
           
          
            
            
              
Check IP address (/32) of both ends appears on routing table

             
            
           
          
            
            
              
Be sure that the transport address is set on the LDP interface or global LDP options. VPLS requires dynamic targeted LDP sessions which must have a consistent transport address set.

Here is an example of working config for VPLS.

https://stubarea51.net/2018/04/23/wisp-design-building-highly-available-vpls-for-public-subnets/
