I've a new CRS326 switch with just the RouterOS installed (no other packages installed).

Recently I got also another switch, a used CRS125. There are more packages installed:



[admin2@CRS125] /system package> print

Flags: X - disabled

# NAME                                                                        VERSION          SCHEDULED

0   routeros-mipsbe                                                             6.46.6

1   system                                                                      6.46.6

2 X ipv6                                                                        6.46.6

3   wireless                                                                    6.46.6

4   hotspot                                                                     6.46.6

5   mpls                                                                        6.46.6

6   routing                                                                     6.46.6

7   ppp                                                                         6.46.6

8   dhcp                                                                        6.46.6

9   security                                                                    6.46.6

10   advanced-tools                                                              6.46.6

```
>
Questions:
1) I think currently I only need just the basic RouterOS package. If I uninstall the other packages, how would I install them later if needed?
2) What tools are in the "advanced-tools" package"?
3) What kind of security does the "security" package offer?
4) What is in the package "system"? Is it optional?
5) What about the "routing" package? Is this an extra/advanced routing stuff?
6) Surely there must be some documentation of/about these packages. Can someone kindly tell me the link? Thx
```

             
            
           
          
          
            
            
              
Thanks, this explains it all, indeed.

A related question: What does the following from the above link do? I assume it downgrades to the previous version. Is it also possible to specify the version to downgrade to?

/system package downgrade; /system reboot;

Btw, on my CRS326 switch the “system” package is not installed. It seems necessary(?) only if the device is a router, or in router mode.

             
            
           
          
            
            
              Don’t think you can specify a specific version. Just upgrade (to the latest) or downgrade (to previous) within the channel chosen (eg. stable/long-term/…) (and I’m not even sure on the channel-flag)

             
            
           
          
            
            
              When you put package files into proper place in device and reboot device, it will instal packages … if extra packages versions match system package … or if new system package version is newer than currently installed. If you want to install older version, you have to run */system package downgrade* - which simply disables safeguard against downgrading.

IIRC changing release channel allows downgrading without playing abovementioned games.

             
            
           
          
            
            
              
Would the following method work to install a specific version: uploading the routeros.npk file to the Files directory and then rebooting the device ?

             
            
           
          
            
            
              
So, can you confirm: do i need to do these steps for installing an older version from an .npk file? :

Step 1: /system package downgrade

Step 2: upload routeros.npk to Files directory

Step 3: reboot the device

And: how can I get which “channel” is active? I think this information is missing in the above link, isn’t it?

             
            
           
          
            
            
              I have no DHCP server configured on the above said CRS125 device, so I want to disable the “dhcp” package,

but it doesn’t work: it gets scheduled for disable, but after reboot it’s still active. How come? What to do?

             
            
           
          
            
            
              In current ROS there’s bundle of packages which is what you get by downloading “Main package”. You can not uninstall individual packages if installed with bundle, you can only disable them.

But you can “unbundle” ROS by installing different version of ROS, but taking necessary packages from ZIP downloaded as “extra packages”. Bare minimum is “system” package. And there are some dependencies, security depends on dhcp (it’s been explained why dependency, IIRC it has to do with IPsec, but I forgot the details). If you need neither DHCP server nor client, you can disable package (but has to be installed).

Note that with ROS v7, Mikrotik went away from packaged installation, currently you can only install bundle and there’s no way of unbundling the installation.

You can check configured update channel by running */system package update print*.

BTW, you can not downgrade to version lower than factory-software (displayed using */system resource print*).

             
            
           
          
            
            
              
I have been able to disable most of the packages, but some won't even disable, take a look:



/system package print

Flags: X - disabled

# NAME                                                                        VERSION        SCHEDULED

0   routeros-mipsbe                                                             6.46.6

1   system                                                                      6.46.6

2 X ipv6                                                                        6.46.6

3 X wireless                                                                    6.46.6

4 X hotspot                                                                     6.46.6

5 X mpls                                                                        6.46.6

6   routing                                                                     6.46.6

7 X ppp                                                                         6.46.6

8   dhcp                                                                        6.46.6

9   security                                                                    6.46.6

10 X advanced-tools                                                              6.46.6

```
>
But Uninstalling these already Disabled packages is not possible: it gives an error saying "Couldn't perform action - can not uninstall bundled package (6)"
Is this a bug?
.
> Note that with ROS v7, Mikrotik went away from packaged installation, currently you can only install bundle and there's no way of unbundling the installation.
>
> You can check configured update channel by running > _/system package update print_> .
>
> BTW, you can not downgrade to version lower than factory-software (displayed using > _/system resource print_> ).
I want to install a version that is >= the factory-software version.
I put the .npk into the Files dir, and then rebooted the device, but it did not replace the current version with the given older version; it just deleted the .npk from the Files dir.
So, what are the correct steps to install an older version .npk?
```

             
            
           
          
            
            
              To upgrade you can use the System > Packages window and click on “Check for Upgrades”.

From there you can also change channels (stable, long-term, rc, dev)

If you want to downgrade to a specific version, then you have to upload the packages manually to the router and then from the System > Packages menu you click Downgrade.

Simply rebooting without clicking downgrade will do nothing.

When ROS sees packages uploaded, it will only try to upgrade and not downgrade after a reboot.

And only if the packages are the correct arch (obviously).

             
            
           
          
            
            
              
I tried both variants, but neither worked 

I think that even the standard downgrade function isn’t working. I have currently this version installed:

/system package update print

channel: stable

installed-version: 6.46.6

To which version will it downgrade if one calls the normal downgrade function (ie. w/o supplying manually an .npk)?

I’ll now try this, but I think I already tried, and it didn’t function.

Update:

Like I said: even the standard downgrade function does not work, as the version is the same as before, after the reboot.

             
            
           
          
            
            
              Discerning (astute) device, it does not like you!    Has Mikrotik developed  networking equipment similar to brooms in Harry Potter?  

             
            
           
          
            
            
              
Then you are doing something wrong.

Read the fine manual again and retry until you succeed.

Only with exercise you will learn how to properly use ROS.

             
            
           
          
            
            
              
[admin2@CRS125] /ip service> /system package update print

channel: stable

installed-version: 6.46.6

[admin2@CRS125] /ip service> /system package downgrade; /system reboot;

Router will be rebooted. Continue? [y/N]:

y

system will reboot shortly

Reboot, yes? [y/N]:

Received disconnect from 192.168.128.253: 11: shutdown/reboot

And after reboot and new login:

[admin2@CRS125] > /system package update print

channel: stable

installed-version: 6.46.6



 Didn’t work as can be seen.

Man, it seems another rocket science to downgrade this device 

IMO there is simply a bug in the downgrade function of this software. Logic & the countless tries with negative outcome today tells me  

             
            
           
          
            
            
              Perhaps operator error--------- bug incurred during conception??

             
            
           
          
            
            
              
You post too much, you read too little. You have half my posts in 2 months and I’ve been here for 15 years.

RTFM and then come back with proper results.

             
            
           
          
            
            
              Does anybody else have a constructive contribution to make (ie. help, tips, hints) for this problem of downgrading to an older RouterOS version?

             
            
           
          
            
            
              You mean constructive, like telling you exactly how to downgrade and you ignoring it?

             
            
           
          
            
            
              It’s childs play. And there is no bug. Processes that are very old and are used daily usually don’t contain bugs anymore.

As programmer you see bugs everywhere , like someone with a hammer sees nails everywhere.

Upload the needed npk to the files directory. Either in RAM (above /flash, the preferred place) or in a top directory in /flash.

**Make sure it is the correct npk (hardware variant) and it is the only instance of that npk in the files directory.**

The hardware variant selection can be confusing.  For CRS125 its the MIPSBE variant. If you load the ARM variant of the CRS325 it will just be deleted.

Check the size of the uploaded npk to verify it is complete. (incomplete files will not install, if there are multiple (incomplete) files it will try one and possibly delete another. So it tries the bad one, fails, and then deletes the good one. Next upload same story.)http://forum.mikrotik.com/t/issues-installing-the-dude/139569/1

If it is a higher version number : take upgrade. If it is a lower version number : take downgrade.  (The channel does not matter in this. E.G. I like the 6.45.6 from the stable channel)

The only, very classical, hurdle in upgrading is that sometimes you need to do an intermediate upgrade step (to be able to upgrade the bootloader firmware).

(I think it was from 6.44 towards 7.0beta … http://forum.mikrotik.com/t/special-instructions-for-installing-routeros-7-beta2/133553/1)

I have never seen a case that needs an intermediate downgrade step.