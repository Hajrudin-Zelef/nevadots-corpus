Hello Every Body,

I have “RB951G-2HnD” that I have been using for many years. Couple of months ago I though it’ll be a good idea to test a custom firmware on it “OpenWRT”. So I put my mind on it and said why not give it a try and check how it goes. So I dug up for a while on the OpenWRT and the compatibility with my RouterBoard and I found out that it’s compatible, so I started reading on how to install the OpenWRT on my router board and following the instructions on their website I managed to install it on my RouterBoard (After Backing up my license). Started to work on the OpenWRT and configuring my device and give it a shot, saying that I can always revert back to RouterOS once I’m done exploring the OpenWRT, had no intention to stick with OpenWRT, but just curious about it. So I’ve done the configuration and the walk around and put it to test as my home’s main router. Sadly the router started to act out and rebooting every now and then, I told myself there must be something wrong with configuration. Hence, I did a reset to the router and reconfigured it from the beginning hoping that the issue will be resolved and I can do more work on OpenWRT and gain some experience along the way, but the rebooting issue remained. Therefore, I decided to try the OpenWRT on the Raspberry Pi and put back RouterOS to my RouterBoard. I never had any issues/complains with the RouterBoard before and It was working fine until I tried out OpenWRT. I’ve used several MikroTik products for years and still are. Every time I do something that damages the router I’d just do a netinstall and start fresh with no issues (Done it “NetInstall” via serial console cable and Ethernet cable if the device doesn’t have a serial console interface on it plenty of times with no issues that can’t be managed like OS, firewall or antivirus restrictions).

Back to my “RB951G-2HnD”, I tried to restore the RouterOS on it via Netinstall but the device won’t show up. So I tried again and again and again with no success. I thought I’m doing something wrong here let me read the Wiki and check I might be missing something, but all what i did was just right and as the instruction said in the Wiki but the device won’t show up on NetInstall. So I started finding for a solution:

1. Disabling the firewall and the antivirus … No Success.
2. Trying different OS (Win XP, Win 7, Win 8.1, Win 10) … No Success.
3. Enabling/Disabling the Netboot in NetInstall … No Success.
4. Trying different NIC … No Success.
5. Use a switch in between the router and the PC … No Success.
6. Pushing the Reset button for different amount of times before and after powering up the device … No Success.
7. Followed all the possible solutions mentioned in the threads posted by others on different and similar device as the one I have … No Success.
8. Trying different versions of NetInstall going back to NetInstall V3 … No Success.
9. Flash OpenWRT back and tried NetInstall again … No Success.
10. Trying to change the boot order within OpenWRT … No Success.
11. Trying some packages on OpenWRT to help me change the boot order … No Success.
12. Searching for an Image of the device that I can put back on the RouterBoard via TFTP … No Success.
and many many more with no luck

The device is currently running OpenWRT but it’s not stable, even after upgrading and downgrading, I can’t get it to stop acting out on OpenWRT and stop it from rebooting every now and then.

I’ve ran out of things to test in order for me to put back RouterOS into my RouterBoard again. Is there a way that I can try and connect to it via console cable ???

Any help/suggestions/ideas from anyone ?

Sorry for the long elaboration, just wanted to be clear on how things went from the beginning till now.

Thank you all

             
            
           
          
            
            
              Anybody, Ideas, suggestions, thoughts… I’m stuck here   

             
            
           
          
            
            
              You must using ether1 when netinstall, see all description https://wiki.mikrotik.com/wiki/Manual:Netinstall

             
            
           
          
            
            
              Thank you for your reply.

Yes, I’m aware of that. Most if not all RouterBoards that doesn’t have serial console port uses ether1 port for the NetInstall. Tried it many times but with no success.

             
            
           
          
            
            
              Please help I’m open to all and any suggestions

             
            
           
          
            
            
              Do you perhaps know if OpenWRT replaces RouterBoot firmware (the BIOS-like firmware which wakes up device and boots actual OS) when installed? If it does, then you’ll have to take care about that before netinstalling the unit.

             
            
           
          
            
            
              
I believe it did replace it “The Bootloader”, since it’s part of the installation process https://openwrt.org/docs/techref/bootloader. But I haven’t modify or done any sort of adjustment to it, so I assume by default and following the installation procedure of OpenWRT that it’ll change the bootloader in order for OpenWRT to be installed. I had no idea how to access the bootloader or modify it if it’s even possible.

Following the tutorial on OpenWRT website they said it’s possible to revert back to RouterOS via NetInstall but following the instructions on how to use NetInstall on their “OpenWRT” website and MikroTik website didn’t help at all and I the device never showed up in NetInstall no matter what I did.

This is the link for MikroTik products common procedure on installing OpenWRT and for the requirements for the revert back process:

https://openwrt.org/toh/mikrotik/common

             
            
           
          
            
            
              
I’m going to take a look at it, I have a problem with them too, thanks.

             
            
           
          
            
            
              Read the manual https://wiki.mikrotik.com/wiki/Manual:RouterBOOT and try activate backup boot loader as:




RouterBOARD reset button

RouterBOOT reset button has three functions:

Hold this button during boot time until LED light starts flashing, release the button to reset RouterOS configuration (total 5 seconds)

Keep holding for 5 more seconds, LED turns solid, release now to turn on CAPs mode (total 10 seconds)

Or Keep holding the button for 5 more seconds until until LED turns off, then release it to make the RouterBOARD look for Netinstall servers (total 15 seconds)

Note: If you hold the button before applying power, backup RouterBOOT will be used in addition to all the above actions. To do the above actions without loading the backup loader, push the button right after applying power to the device.


Try using USB-TTL UART adapter for connect to UART pad on board https://s00.yaplakal.com/pics/pics_original/8/1/3/3969318.jpg, find unsoldered contact pads GND,RX,TX near the bottom right corner of the picture with UART title. After connect pads to USB-TTL adapter, run terminal program like “putty”, select “Serial” and desired port for USB-TTL adapter, then see activities on console during boot RB951G-2HnD (see https://www.trishtech.com/2015/01/testing-usb-to-ttl-adapter-using-putty)

             
            
           
          
            
            
              
I have USB to Serial Converter, Can I use that to connect to the pads in the board ? Shall I connect to the 4 “Square shaped” pads on the board or what ? Following the manual on using the serial console cable there are 9 pins based on the cable and as shown on the wiki here https://wiki.mikrotik.com/wiki/Manual:System/Serial_Console so which is it that I need to use

Thank you very much

             
            
           
          
            
            
              USB to serial converter can only be connected to USB port of your device, and it only works with RouterOS itself but not for the bootloader, so it’s not an option.

Out of the 9 pins on the DB-9 connector, you only need three - SG, TxD and RxD. But you have to convert the voltage levels from TTL (0 and 5 V) to standard RS-232 levels (+12 and -12 V, respectively). So you need a discrete circuit or an integrated one, like max-232, and the easiest way may be to find a complete module, which you connect using passive wires alone (i. e. no electronic components to add), at your favourite Arduino shop. You connect the ground, TTL sides of Tx and Rx, and power supply (+3 V) to the RB side, and connect the same ground and the RS-232 sides of Tx and Rx to the 9-pin connector. There is an issue - you have to connect the output of one device to the input of the other one, and the Rx and Tx names may be confusing, so better to measure voltage against ground before connecting. What gives anything between 3 and 25 volts in either polarity against ground is an output, what gives less is most likely input. This is an example of such module (there are many different ones out there).

             
            
           
          
            
            
              I used the USB to serial cable to netinstall RB 433 when I bricked it some years ago

USB → PC

Serial > RB 433

 
            
           
          
            
            
              
Yeah, but the RB433 probably had true RS-232 levels, or the USB to serial cable had TTL levels at the serial end as it was one of those suggested by @ysha - the photo shows it must have been the first case.

             
            
           
          
            
            
              So Is it safe to use the one USB-Serial Converter I used on the RB433 (I still have it) ?

I can use a solder to fix on the cables to the board but I don’t know on which pads to connect on the RouterBoard

             
            
           
          
            
            
              
One more time:

- If you want to connect the USB end of that cable to the 951’s USB port, it won’t help as the bootloader doesn’t work with serial over USB.
- If you want to connect the USB end of that cable to the PC, and the 9-pin end to the 951, you need the RS-232/TTL level converter as suggested by me to use your cable. Or you may replace the combination of these two items by a single USB-to-TTL-serial cable as recommended by @ysha.

Regarding the pins to connect - at the photo, there are four pads in the UART area - GND, Rx, Tx, and 3V. For @ysha’s suggestion, it is enough to use GND, Rx,Tx as the converter electronic is powered from the USB end. For my suggestion, you need also the 3V to power the electronic of the level converter, as no power is sent across the DB-9 connector.

             
            
           
          
            
            
              Thank you @sindy & @ysha for your input. I’ll give it a try and will let you know how it goes

             
            
           
          
            
            
              @AMatt, there is still a connection option, but to do this, you will need to open the plastic case of your adapter to access the converter board your adapter and connect to contacts with TTL levels. For example as shown in the diagram below, connect to the pins of the FT232R GND, RXD, TXD chip (between FT232R and RS232 level converter). But you must have the appropriate qualifications to understand the schematic diagram of the adapter you have (or find a similar one on the Internet). The connection points depend on the USB-TTL chip used in your adapter.

)

             
            
           
          
            
            
              Thank you @ysha, I’ll try and get the adapter tomorrow. If I couldn’t find it in the computer and IT Gadget store, I’ll give it a try and try to connect as you suggested.

However, most of USB to UART TTL adapters are using +5v and the RB 951 UART says +3v, is it safe to proceed with the +5v adapter ?

             
            
           
          
            
            
              You shouldn’t do that, then it is better to order something from proposed earlier above. It is possible to convert levels +5 and +3.3 V but additional schemes are required for example https://electronics.stackexchange.com/questions/186168/how-to-convert-uart-voltage-from-5v-to-3-3v. I would prefer to use a ready-made USB-TTL adapter or find another adapter that has a chip powered by a voltage of +3.3V.