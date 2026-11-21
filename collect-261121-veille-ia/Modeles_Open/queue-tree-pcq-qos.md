---
id: collect-261121-veille-ia/modeles-open/queue-tree-pcq-qos
title: "Queue Tree PCQ QoS"
domain: modeles-open
role: reference
task: article
actors: []
dates: []
keywords: ["limit", "premium", "queue", "tree", "traffic", "download", "type", "total"]
source: https://forum.mikrotik.com/t/queue-tree-pcq-qos/57256
source_anchor: ""
source_lines: [1, 55]
section: "Modeles_Open"
---
Hello, everyone:)

Trying to set up RB1200 with QoS using queue tree as per US11 Megis—

I have Address list as follows:

Premium Traffic marked in mangle

**Premium Traffic Address list includes IPs of custumers 1, 2, 3, etc**

Then the tree…

-Total Download (type= default, max limit=30)

----Premium Traffic Download(type=PCQ, parent=Total Download, limit=5M, Max Limit=10M)

The question…

1- Does it distribute automatically 5 meg to **each IP in the address list**???

**or**

2- Do I have to make an individual child with every Client IP as follows?

-Total Download (type= default, max limit=30)

–Premium Traffic Download((type=PCQ, parent=Total Download, limit=5M, Max Limit=30M)

----Premium Client#1 Down((type=PCQ, parent=Premium Traffic, limit=5M, Max Limit=10M)

----Premium Client#2 Down((type=PCQ, parent=Premium Traffic, limit=5M, Max Limit=10M)

Thank you

             
            
           
          
            
            
              Evelio,

Yes, your each client will get 5mbit.

             
            
           
          
            
            
              Each client will get 5 mbit only if you set Rate on PCQ queues in the queue types tab to 5M.

On the Queue tree you can only limit the total limit.

It’s always better to read wiki, there is a lot of examples.
