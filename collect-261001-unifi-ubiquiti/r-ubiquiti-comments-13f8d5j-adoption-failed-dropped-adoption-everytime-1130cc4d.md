Adoption Failed, Dropped Adoption EVERYTIME 
        
        
        
    
    
    Hello everyone, I'm frustrated with their APS and seems like my adoption keep failing.
I have UDM PRO SE, I tried to adopt newer APS such as U6 PRO, U6 Mesh, and my old ac mesh, and most likely my old mesh pro..
I had usg and cloudkey, and have my cloudkey connected to the mini manageable switch (ubiquiti 8 port), but i disconnected my wire from the USG to the switch, and used a wire to goes from my udm pro se to that switch.
when I did not adopt the AP (just let the old one run with old setting), all the aps will work just fine
BUT... when I adopt the AP, things start failing.it would work for a bit, then if failed, it mentioned that it was unable to adopt the device
at first I thought it was my VLAN setting, but it still happen even after i consolidate all my network into a single ip and vlans.
what do i do wrong?I assume that the USG is the router that gives the DHCP addresses, so as long as i unplugged it from the switch, my UDM PRO SE will give the addresses so i don't think the USG is the problem, but I could be wrong...
everytime the APS are failed to run, they fall back into some ip that even out of my range.. it falls into 192.168.2.xxx
UPDATE 1:I tried several different thing...
- 
      I tried to connect the AP straight to my UDM PRO SE, still gives me adoption failed
- 
      I tried to ssh to the device, it worked and it send the adoption message to my controller, but then when I finally adopt it, and I changed the IP into a static, it lost it again and failed to adopt, ALTHOUGH, it does not fall back to its "default" ip of whatever it is 192.168.2.xxx
- 
      I cannot cange that 192.162.2 because it's being used by teleport DHCP, so I don't think i can change the range or do anything with it
- 
      After I change the IP, I cannot ssh into it, or at least the password changed? I tried to ssh to the new IP that i set, and it asked about fingerprint, etc, i press yes, and it connected, but when i tried to enter ubnt on password, it denies it.
so, should I keep the AP on a DHCP? or should i make it as a static AP?seems like i could still connect to the internet thru that AP, but my controller just unable to see anything into it? Or hopefully that is the case...
UPDATE 2:I tried legacy ubiquiti interface, I was able to readopt the APS, so let's see if i will lost it again or not... I was not able to readopt using the newer interface, but i guess it worked with the legacy interface...
UPDATE 3:I did several stuffs that seems to work...
- 
      When I'm trying to adopt the AP, I make sure that the port that AP connected to only giving the "controller AP", i.e. my controller is located at 192.1.1.1/23, then on that port, I only give that "setting"
- 
      I turn on my Teleport, I guess it helped with rouge IPS in 162.2 lol, somehow when i do this and connected my phone to the teleport, I was able to reset the AP and make it submit to the range of my port setting LOL... idk what happen but it seems to work.
- 
      I adopted the AP
- 
      Even after adopting the AP, it keep saying "applying setting" or something like that of some sort, you need to refresh the controller again several time, and it will adopt... IF it does not want to adopt again.. you need to go to the legacy controller setting, and do the advance control thru the legacy controller, somehow it works better, perhaps because of the set option that you usually send to the AP is based on 192.168.1.1/24, I think the newer controller does not have an option to change that, so pretty much going back and forth between the legacy and new controller
- 
      After the AP changed its ip, now you change the setting back to all network on that port, and it will work properly, at least this far... lol
- 
      I think I have to change the IP into static, and it works "better" so far...
Impressive how long this process is, I keep resetting and keep falling back to 192.168.2.xxx addresses, and have to ssh-ing to the APS to force adoption and many more stuffs... Pretty advance stuffs imho, and the fact that you cannot change the teleport range into different IP for its DHCP is frustrating, I think if I'm able to expand my default IP from /24 to /23, I might be able to adopt the AP easier (because it will fall into .2, so perhaps if it goes to its fallback IP, i should be able to ssh it or my controller should be able to see it)
I also have a blackmagic device, old one... The UDM PRO SE is unable to see it, but my device is located inside the network, and i can access it from other computer thru the network... STRANGE..
Anyway, Let's see if my setting holds, hopefully it does not fallback again... and failed to adopt again... I'm starting to lose my hair...
      UPDATE 4 (10/22/2023)
I had a same problem again when I tried to link an AP, I figured a "hack"
It would be easier for me to adopt a device when I don't have it connected to the UDM PRO SE directly. So, I power it up using the POE injector, then I uses my other AP to "MESH" the connection. Apparently it works great in helping me to adopting the access point, make it way easier to adopt without any complication
    
Section des commentaires
Hello! Thanks for posting on r/Ubiquiti!
This subreddit is here to provide unofficial technical support to people who use or want to dive into the world of Ubiquiti products. If you haven’t already been descriptive in your post, please take the time to edit it and add as many useful details as you can.
Please read and understand the rules in the sidebar, as posts and comments that violate them will be removed. Please put all off topic posts in the weekly off topic thread that is stickied to the top of the subreddit.
If you see people spreading misinformation, trying to mislead others, or other inappropriate behavior, please report it!
I am a bot, and this action was performed automatically. Please contact the moderators of this subreddit if you have any questions or concerns.
Have you tried resetting ? Is it wired? Disable auto meshing
Did all of that, i figure since I’m running wire i will disable auto meshing
And yes in fact i reset one of them for like 2-4 times lol So frustrating
I had similar issues on 3 of my ap but I can’t recall what fixed the issue. But I did power cycle the ports on use pro. Restated both udm and usw reset the AP put new cables and put the AP on a newer ports but I do remember resetting multiple times standing on the ladder lol
Same thing here. Tried to adopt a new U6 Mesh and it seemed to crash the AP upstream.
When I say crashed, I mean the device upstream immediately errored with failed adoption even though the devices have been running fine for months.
Moved it to a different location that was wired to the USW Lite switch and it crashed the switch.
POS product
added an update to my initial post
Here’s the picture of adoption failed
https://imgur.com/a/OzbLUFh
Ubiquiti software is among the worst I've ever encountered in my whole 20ish years in IT and networks. Absolute fkn trash.
Wasted DAYS worth of time in the past with this horseshit adoption process. Randomly it decides to work and like a house of cards you try desperately to never touch it or even look at it again for fear it might break.
Well.... 6 months later.
I literally just updated one of my APs using the "update" button in unifi. Sure enough it auto rebooted the AP and now adoption has failed and no matter wtf I do I cant get it back online so half my network is out cuz its the AP for that side.
Will NEVER EVER EVER EVER EVER EVER even look at an ubiquiti product again, let alone buy one. You couldnt fkn pay ME to use this shit.
thumb fade public light dinosaurs soft spark price threatening cautious
This post was mass deleted and anonymized with Redact
Yah, outside of the APs I currently have I won't be adding anymore of theirs.
I've had some success sshing into the device and sending the callback command to the Unifi controller ip and port.
Generally have to do that multiple times too tho, just cuz.
I had a similar problem recently. The fix for me was to completely remove the AP from the controller so there was no entry for it at all. Then, I reset the AP and logged in via SSH and used the set-inform command to manually set the IP of the controller. From then on it worked just fine.
Where can i get the id and password dor the aps?
If it’s reset then it should just be Ubnt/ubnt
I tried this earlier, work for some time.. like 2-3 seconds, and then failed to work again
It goes to its fallback ip, 192.168.2.191 I want to switch my default so i can discover the ubnt, too bad that ip range is used for teleport
Hi u/jhanbali We apologize for the frustrations. Please share more info and any related support tickets here so we can properly escalate and assist: http://community.ui.com/social-feedback
I havent get a support ticket, where do i submit the question again?
updated the problem information with more diagnostic that I just did.
I am having the same issue! Except that my AP IP address kept falling back to 192.168.10.xxx. Extremely frustration.
Hi, Dries from UniHosted, please make sure that the inform ip is set correctly in your system.properties. Also if you can, try to access the db and make sure that the failing devices are completely removed from the "devices" collection.
Commentaire supprimé par le membre
Can you elaborate?
refuses to fucking elaborate