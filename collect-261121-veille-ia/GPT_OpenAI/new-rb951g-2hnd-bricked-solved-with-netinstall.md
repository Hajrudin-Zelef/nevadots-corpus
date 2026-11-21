---
id: collect-261121-veille-ia/mikrotik/new-rb951g-2hnd-bricked-solved-with-netinstall
title: "New RB951G-2HnD bricked? [Solved with Netinstall]"
domain: mikrotik
role: reference
task: article
actors: ["OpenAI"]
dates: ["2016-01-25"]
keywords: ["netinstall", "router", "then", "mikrotik", "device", "bricked", "port", "rb951g-2hnd"]
source: https://forum.mikrotik.com/t/new-rb951g-2hnd-bricked-solved-with-netinstall/70728
source_anchor: ""
source_lines: [1, 304]
section: "GPT_OpenAI"
---
Just got a new RB951G-2HnD today. Downloaded and copied routeros-mipsbe-6.5.npk to the root folder using Winbox, and rebooted the router by power cycling.

When I power on the device, it does beep. When I connect an Ethernet wire, the corresponding light comes on, and the computer identifies the link speed as 100Mbps (static IP 192.168.88.7/24 configured on desktop).

However, the device is not being detected by Winbox or NetInstall. Have tried rebooting and resetting the device multiple times. No response from the device at all.

Is the firmware corrupt? Have I bricked the device… Any suggestions how I can recover it…

Thanks for your help!!

             
                
            
           
          
            
            
              I believe that your router board is not dead after the upgrade. You may try to disable a firewall on your PC and try again to use a Winbox. If it does not help then you may try to restore a factory default settings. In that case you will be needed to insert a pin into the ‘reset’ hole then connect a power cable to the router and wait about 10 seconds. After that you may remove pin and the router will continue booting up. In the end you should be able to see a blinking WiFi led. If WiFi led is blinking then try to connect to you router again.

             
            
           
          
            
            
              Thanks! I did try that. The router remains dead. The ACT Light blinks a few times on powering on, with the reset PIN pressed. Then there is a beep, and then the same status - no wifi, no response from any interface.

Have also tried disabling the firewall and AV and tried with Winbox (Port 2) as well as NetInstall Netboot (Port 1). Mikrotik support also suggested disabling the firewall and AV. But the status remains the same - an unresponsive router…  

Is there anything else you think I could try… And thanks again for your help and support!

             
            
           
          
            
            
              You may try to use port 3. If it’s will be the same then ask Mikrotik Support person if it’s possible to recover somehow after the incorrect flashing during the upgrade. Probably you will be needed to send your box to they service center.

             
            
           
          
            
            
              Another one point. I think that your bootloader may have the incorrect version for the installed firmware and needs to be upgraded also. More information: http://wiki.mikrotik.com/wiki/RouterBOARD_Bootloader_upgrade

             
            
           
          
            
            
              I had exactly this error, good news is, it’s not bricked (mostly).

You need to follow the instructions for netinstall, set it up to boot from network location (there’s plenty of documentation on how to use netinstall to do this). Power on the Mikrotik whilst keeping the reset port shorted and the device should appear in netinstall. It took me a few goes and then I had absolutely no base config, good learning experience rebuilding from scratch.

Good luck.

             
            
           
          
            
            
              Success! My Mikrotik is back in working condition!! Thank you all

RB951G2HnD, Thank you so much for your guidance.

Dallen, You gave me the most important ingredient - hope… Mikrotik support just left me hanging with “Use NetInstall”. But hearing your story made me try try and try till it worked…

Let me post what I did so that anyone else with the same problem may be able to fix it themselves.

a) Disable Firewall

b) Connect PC to Port 1 on Mikrotik

c) Disable all other network interfaces on the PC - LAN, Wireless, Virtualbox

d) Set static IP of 192.168.88.3 subnet mask 255.255.255.0 gateway 192.168.88.1 on PC

e) Run NetInstall

f) Select “Net Booting”

g) Mark “Boot Serve Enabled”

h) Selected Client IP address of 192.168.88.1

i) Keep reset pressed while powering on

j) Keep holding the reset pin till a beep sounds. Release immediately

k) Router showed up in Netinstall’s list (**)
l) Unmarked “Keep old configuration” and “Configure Script”
m) Unzipped “all_packages-mipsbe-6.5.zip”
n) Selected all packages by browsing to the unzipped directory and using the “Select All” button
o) Clicked on “Install”
p) The progress bar moved to 100% as the packages were uploaded (**)

q) The router rebooted - Beep once, some time later (1 min or so if I recollect) a second beep

After a while after the second beep short (30 secs or so if I recollect) , the wireless light turned on, and the router configuration was as clean as the day I unpacked it (just 6 days ago!).

The nightmare was in steps (k) and (p) both marked with (**). The router would just not show up in NetInstall’s list - must have tried it 30 times before this worked - But once I got it to work, it started working every time. And then after Clicking on “install” the router status would go to “Installing” and then to “Ready” without doing anything - no idea why the installation worked properly after 20 times of trying!

Irrespective, my router is back to being better than new - now upgraded to the later version of OS, and now with some additional packages!

Thanks you for your help and encouragement. Made all the difference for a beginner like me!

Now I need to get my production network running with dual-WAN and VPN…

Best regards,

Pradeep

             
                
            
           
          
            
            
              ROS 6.5 bricked my RB951G-2HnD too, with updating from 5.26 .

I’ve upgraded from 5.26 to 6.4, downgraded back to 5.26.

But upgrading from 5.26 to 6.5 makes RB951G-2HnD bricked.

Since 6.5 has some issues to me so I want to downgrade back to 5.26, but I’m afraid it bricked again when I upgrade it to 6.5 or 6.6 again.

Could you please fix it in the next version? Recovering it from brick takes a lot of time to re-setup all configures, users and meta-routers.

             
            
           
          
            
            
              
Which RouterBOOT version do you have? And can you send us your supout.rif file from v5.26 before you make the upgrade?

             
            
           
          
            
            
              
I upgraded routerboard firmware from 3.08 to 3.10 after I recovered the RB from netinstall v6.5 packages.

I can’t find older supout.rif, and metarouter configs are lost after upgrade.

             
            
           
          
            
            
              I successfully managed to upgrade several different devices to v. 6.5 from v. 5.25. Some of them did not even had 5.25, so I:

1. put only necessary packages (of v. 5.25 if lower version was in the device) into files by dragging from the folder to winbox
2. pressed system - reboot, confirm.
3. upgraded firmware (routerboard - upgrade, reboot)

Then repeated with v. 6.5 the same steps.

I did it with non-critical devices to check how it will be working, because I had several times problems to successfully work with older v. 6.x. But v 6.5 seems to work good so far…

No bricking, no problems. After some days of testing and checking that all scripts would work on v 6.5 I am going to upgrade the rest.

             
            
           
          
            
            
              
I can confirm that after struggling with router not showing up in the netinstall, following these steps exactly made it work on the first go. I used `sudo wine netinstall.exe` on Linux, not windows.

             
            
           
          
            
            
              
This process still works as of April 2015, using mipsbe v 6.27 and netinstall 6.27.

I had one variation from Pradeep’s list that I wanted to note in case someone else runs across this thread. I got two new units yesterday, one of which was bricked out of the box.

After following steps E-Q above multiple times, I had varying levels of “almost” success. Eventually, the router started acting like it was successfully rebooting, but instead of the single beep noted in Q, I was getting a quick double-beep. This happened both after the netinstall procedure and if just rebooting without anything connected. The wireless light never turned on.

I noted that step L didn’t mention the “Default configuration” option, which was unchecked, so I enabled that. (None of the screenshots I’ve looked up show that option, so I’m guessing it’s a recent addition). The router rebooted successfully, the wireless light came on, and I was able to see the configuration web page.

Thanks to everyone in this thread for the help!

             
            
           
          
            
            
              Just to add - NEVER select “ALL” packages on RouterBOARD devices. Many of them have little space available, better check only the packages you know you need. Usually for basic operation you need system, advanced tools, wireless - maybe something specific, but that’s it. routing package is only for dynamic routing, and ntp is only for server, so you mostly don’t need to install these.

             
            
           
          
            
            
              my sxt 2hnd is bricked…after powering the user led and power button glows non stop along with two other led’s…is there a way to fix it or its damaged?

             
            
           
          
            
            
              
I had to do this process too. Took me about 2-3 attempts to get the timing right and have the unit show up. It also helps to know which interface you need to be hooked upto depending on which model you might be doing netinstall on. But process certainly works well.

             
            
           
          
            
            
              
I believe you have a wiki article that talks about this. I should probably go find and link it here.

             
            
           
          
            
            
              I Guess you were referring to this link?

http://wiki.mikrotik.com/wiki/Manual:System/Packages

Thanks for the advice.

It worked for me wonderfully.

The only difference was that the unit got bricked by installing a backup that had features enabled not support by the RouterOS version, as the OS was not upgraded first. The initial backup was created in RouterOS 6.29.1 and the device had 6.23 on at the time.

I selected the standard 3, advanced-tools, system & wireless, as prescribed by Normis.

Also I had to select -keep old configuration.

For some reason it refused to install with deleting the old config.

This was on a Mikrotik BaseBox

             
            
           
          
            
            
              I just received MikroTik RB951Ui-2HnD from one of my friend to setup the hotspot with it.

Well during my experiments (which included firmware upgrade to 6.33.5) somehow I bricked it.

Google search landed me on this page. I tried everything mentioned here but my modem never picked up by netinstall.exe

After some more search on net I finally succeeded.

I thought it would be interesting to share the whole procedure in following simple steps:

**How to unbrick MikroTik RB951Ui-2HnD**

You must first have downloaded these:

1. netinstall.exe
2. and the required firmware (eg. routeros-mipsbe-6.33.5.npk) from MicroTik website.

Of course your modem must be hooked up with PC through LAN cable.

Use ‘port 1’ (labelled PoE In) on your modem.

1. Disable your PC’s Firewall and Antivirus.
2. Disable all ethernet adapters (eg. VMWare, Wifi etc)
3. But left enable only ONE ethernet adapter which we are using to connect the modem with the PC.
4. Set the adapter’s IP to: 10.0.0.1 and subnet mask to 255.255.255.0  (Leave rest of the settings blank)
5. Run netinstall.exe
6. In Net Booting option: Check Enable Boot Server & set client IP as: 10.0.0.2
7. Now boot your modem while pressing the reset button for at least 15 seconds (until activity indicator turns off) then let it go. This will reboot your modem (with a single beep after a few seconds) with netinstall search.
8. The MAC address will appear in netinstall. Select it.
9. Select your firmware (which you must have already downloaded).
10. Check Apply default config.
11. Hit install and after a while (a few seconds) press reboot (if appears). Done!
12. Don’t forget to revert all of the adapter changes you made in previous steps (2-4)

This is how I got my modem back to work. 

Hope it will help you.

Thanks

Dr. Zeeshan

25 Jan 2016
