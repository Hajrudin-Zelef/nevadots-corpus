---
id: collect-261121-veille-ia/apple-ios/static-route-cisco-static-routing-on-cisco-routeres-cisco-ios
title: "Static Route Cisco | Static Routing on Cisco Routeres ⋆ Cisco IOS"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["static", "route", "routing", "router", "cisco", "routes", "config", "configure"]
source: https://ipcisco.com/lesson/static-route-configuration-on-cisco-routers-ccna/
source_anchor: ""
source_lines: [1, 75]
section: "Apple_iOS"
---
To send the traffic to the destination we can use **two types** of routing. The first one is **Static Routing** and the other one is **Dynamic Routing**. Static routes teach the destination network to the router. It is a manual work that is dıne by network engineers. These type of routes work well with small networks. Because it is a manual work. For large scale networks Dynamic Routing will be a better choice. Here we will focus **Static Route Cisco Configuration**.


The below example will explain the configuration of the **Cisco Static Routing Configuration**.


**Configure the Static Routes on Router A**

For our **Static Route Cisco configuration** example, firsttly, run the command **show ip route** to view the **IP routing table** for router A before defining static routes

```
RouterA# 
```
**configure terminal** 
RouterA(config)# **ip route 10.10.12.0 255.255.255.0 10.10.10.2** 
RouterA(config)# **ip route 10.10.13.0 255.255.255.0 10.10.11.2** 
RouterA(config)# **exit**
If we give show ip route command on router A to view the IP routing table we will see both directly connected and static routes detail.


**Configure the Static Routes on Router B** 

Again on Router B, firtly, brun the command **show ip route** to view the IP routing table for router B before defining static routes

```
RouterB# 
```
**configure terminal** 
RouterB(config)# **ip route 10.10.11.0 255.255.255.0 10.10.10.1**
RouterB(config)# **ip route 10.10.13.0 255.255.255.0 10.10.12.2** 
RouterB(config)# **exit**

If we give show ip route command on router B to view the IP routing table we will see both directly connected and static routes detail.


__Configure the Static Routes on Router C__

We will do the same thing on Router C too.  We will run the command **show ip route** to view the IP routing table for router C before defining static routes.

```
RouterC# 
```
**configure terminal**
RouterC(config)# **ip route 10.10.10.0 255.255.255.0 10.10.11.1** 
RouterC(config)# **ip route 10.10.12.0 255.255.255.0 10.10.13.2** 
RouterC(config)# **exit**

If we give **show ip route** command on router C to view the IP routing table we will see both directly connected and static routes detail.


__Configure the Static Routes on Router D__

Lastly, we will do the same thing on Router D. Firstly, we will run the command show ip route to view the IP routing table for router D before defining static routes

```
RouterD# 
```
**configure terminal** 
RouterD(config)# **ip route 10.10.10.0 255.255.255.0 10.10.12.1** 
RouterD(config)# **ip route 10.10.11.0 255.255.255.0 10.10.13.1** 
RouterD(config)# **exit**

If we give show ip route command on router D to view the IP routing table we will see both directly connected and static routes detail.

Here, we have talk about **Static Routing Configuration** and **How to do Static Routing Configuration**. You can try this configuraiton on your own pc and practice on static routing on Cisco routers.

Gokhan Kosem is a Network Engineer, Instructor and the Founder of IPCisco.com with 15+ years of experience in Cisco, Nokia, Huawei, Juniper, Linux, Service Provider Networks, Routing and Switching technologies.

He has worked on the backbone networks of major service providers and network vendors including Nortel, Alcatel-Lucent (Nokia) and has extensive hands-on experience with Cisco, Huawei, Juniper and Nokia networking technologies.

He has trained thousands of networking students worldwide through IPCisco.com, Udemy, books, labs, quizzes, and educational content across multiple social media platforms.

IPCisco.com | Best Route to Your Dreams

## Leave a Reply
