Help me understand why LACP ports were put in err_disabled 
        
    Hi
we have a pair of stacked C9500s with a port on each configured in an Access Port channel connecting to a physically separate ring network based on Hirschmann switches.
For the second time now, all of a sudden, the switch has put both ethernet ports in err_disabled - the first time, months after initial configuration, and Saturday, months after the previous error.
A simple shut / no shut solves the issue, but I would like to understand the root cause of this problem and avoid it in the future, since this connection is very crucial.
I am hypothesizing a BPDU issue in occasion of something happening on their network such as a change in the redundant ring - but the logs are not very explicit:
Mar  2 09:54:47.609: %PM-4-ERR_DISABLE: channel-misconfig error detected on Po17, putting Twe1/0/17 in err-disable state
Mar  2 09:54:47.613: %PM-4-ERR_DISABLE: channel-misconfig error detected on Po17, putting Twe2/0/17 in err-disable state
Mar  2 09:54:47.617: %PM-4-ERR_DISABLE: channel-misconfig error detected on Po17, putting Po17 in err-disable state
Mar  2 09:54:48.609: %LINEPROTO-5-UPDOWN: Line protocol on Interface TwentyFiveGigE1/0/17, changed state to down
Mar  2 09:54:48.614: %LINEPROTO-5-UPDOWN: Line protocol on Interface TwentyFiveGigE2/0/17, changed state to down
Mar  2 09:54:48.614: %LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel17, changed state to down
Mar  2 09:54:49.613: %LINK-3-UPDOWN: Interface TwentyFiveGigE1/0/17, changed state to down
Mar  2 09:54:49.616: %LINK-3-UPDOWN: Interface Port-channel17, changed state to down
Mar  2 09:54:49.616: %LINK-3-UPDOWN: Interface TwentyFiveGigE2/0/17, changed state to down
Mar  2 10:44:42.569: %SEC_LOGIN-5-LOGIN_SUCCESS: Login Success [localport: 22] at 11:44:42 CEST Sat Mar 2 2024
Mar  2 10:50:14.170: %LINK-5-CHANGED: Interface TwentyFiveGigE1/0/17, changed state to administratively down
Mar  2 10:50:14.180: %LINK-5-CHANGED: Interface TwentyFiveGigE2/0/17, changed state to administratively down
Mar  2 10:50:14.217: %LINK-5-CHANGED: Interface Port-channel17, changed state to administratively down
Mar  2 10:52:49.770: %LINK-3-UPDOWN: Interface Port-channel17, changed state to down
Mar  2 10:52:49.771: %LINK-3-UPDOWN: Interface TwentyFiveGigE1/0/17, changed state to down
Mar  2 10:52:49.773: %LINK-3-UPDOWN: Interface TwentyFiveGigE2/0/17, changed state to down
Mar  2 10:52:54.570: %LINK-3-UPDOWN: Interface TwentyFiveGigE1/0/17, changed state to up
Mar  2 10:52:54.620: %LINK-3-UPDOWN: Interface TwentyFiveGigE2/0/17, changed state to up
Mar  2 10:52:56.459: %LINEPROTO-5-UPDOWN: Line protocol on Interface TwentyFiveGigE1/0/17, changed state to up
Mar  2 10:52:56.468: %LINEPROTO-5-UPDOWN: Line protocol on Interface TwentyFiveGigE2/0/17, changed state to up
Mar  2 10:52:57.457: %LINK-3-UPDOWN: Interface Port-channel17, changed state to up
Mar  2 10:52:58.458: %LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel17, changed state to up
There is no port-security enabled on these ports:
Port Security              : Disabled
Port Status                : Secure-down
Violation Mode             : Shutdown
Aging Time                 : 0 mins
Aging Type                 : Absolute
SecureStatic Address Aging : Disabled
Maximum MAC Addresses      : 1
Total MAC Addresses        : 0
Configured MAC Addresses   : 0
Sticky MAC Addresses       : 0
Last Source Address:Vlan   : 0000.0000.0000:0
Security Violation Count   : 0
And ErrDisable reason are set to default:
ErrDisable Reason            Detection        Mode
-----------------            ---------        ----
arp-inspection               Enabled          port
bpduguard                    Enabled          port
channel-misconfig            Enabled          port
community-limit              Enabled          port
dhcp-rate-limit              Enabled          port
dtp-flap                     Enabled          port
evpn-mh-core-isolation       Enabled          port
gbic-invalid                 Enabled          port
iif-reg-failure              Enabled          port
inline-power                 Enabled          port
invalid-policy               Enabled          port
l2ptguard                    Enabled          port
link-flap                    Enabled          port
link-monitor-failure         Enabled          port
loopback                     Enabled          port
loopdetect                   Enabled          port
lsgroup                      Enabled          port
oam-remote-failure           Enabled          port
mac-limit                    Enabled          port
pagp-flap                    Enabled          port
port-mode-failure            Enabled          port
pppoe-ia-rate-limit          Enabled          port
psecure-violation            Enabled          port/vlan
security-violation           Enabled          port
sfp-config-mismatch          Enabled          port
sgacl_limitation:enforcem    Enabled          port
sgacl_limitation:multiple    Enabled          port
storm-control                Enabled          port
udld                         Enabled          port
psp                          Enabled          port
dual-active-recovery         Enabled          port
evc-lite input mapping fa    Enabled          port
vsl-and-non-vsl-port-pair    Enabled          port
fasthello-and-non-fasthel    Enabled          port
mvrp                         Enabled          port
mrp-miscabling               Enabled          port
Does anyone have an idea? Unfortunately I did not know what else to do other than shut / no shut.
Section des commentaires
The error would indicate that the port channel configuration is different on them, I would compare the configuration on each.
If that were the case, it wouldn't error out once every 4 months would it?
All they sent me is a screenshot of the Hirschmann web gui where they enable link aggregation, not much - so I was hoping for someone to have a bit of experience with them.
Spanning tree has etherchannel guard misconfig in addition to the generic ErrDisable cause. This might be a reason for Cisco to disable the ports.
[Edit] found a description for this feature on a blog: "This feature is somewhat of a hack. If you keep receiving BPDUs from several MAC addresses, the feature will assume that you have a bundling problem and shut down the port.:
On the Hirschmann side, are the ports configured as plain LACP access, no funky industrial protocols active?
You might consider to deactivate the ErrDisable Reason if it doesn't impact other functionality.
The ports themselves are, but behind that switch there's a double redundant ring that uses their own protocols, and a whole host of things configured with industrial protocols as well.
I know I can set a short recovery timer for the specific error, and I did, but is there a way to disable errdisable channel-misconfig entirely?
Can it be disabled by port or only for the whole switch?
What are your port configs?
The 2 x access ports and the port channel?
Sorry I just implied it, it's literally just two ports in access with a port channel.
Speed is left to auto with a 1G transceiver, same as the other side.
In that case mate, a nice first step would be to enable bpdu filter on the access ports and the access-port-channel.
That just discards the bpdu's from the other switch, which is what I would bet is sending bpdu's and your switch is seeing the misconfig and getting its knickers in a twist.
It says misconfig error, but you haven’t shown us any port configs.
Sorry I just implied it, it's literally just two ports in access with a port channel.
Speed is left to auto with a 1G transceiver, same as the other side.
It’s hard to say for sure. But technically, your ports aren’t set to access. They’re set to dynamic because you haven’t typed “switchport mode access”. I have had ports flip from access to trunk and back before because they were left dtp dynamic like yours.
You also have to look at the other side of the port channel. An LACP channel will shut down if the other side doesn’t negotiate, by design. So it could easily be something on the other switch (or VM host, or whatever it is).
Is a static port channel an option? 'channel 17 mode on'
With some backup appliances I've where the lacp pdus are sourced from a member interface Mac vs the ether channel vmac. In those cases, if they do maintenance or that particular member port drops, then it will source from another member and then my side err-disables
It's an idea, I haven't used a static channel in 20 years...
Check your spanning tree config on each side too.
Unfortunately I cannot easily check the other side since it's in the hands of a contracted company - I can only make suggestions.
Config problem on the other side. Software defect on the other side causing a behavior that trips up the channel detect. Or fundamental difference in LACP interpretation between the two platforms.
I remember a pair of Cisco 4506s with a channel between them that had a misconfig on one of the four ports. Ran that way for months, perhaps two years. Occasionally would drop packets. Never threw a misconfig error. (May have been mode on though…it’s been a decade and I don’t remember all the details.) The fact that the packet drops were only like once a month was weird though. In theory it should have been all the time (at least for half of the hash).
Yeah that's exactly what I think... the problem is they're trying to throw me under the bug because if enforced gasp segmentation between clients, servers and PLCs...
More than likely it’s either the spanning tree or etherchannel config is mismatched between the two vendors. The problem is most likely being triggered when spanning-tree reconverges on the Hirschmann end due to them switching HA state, or reversing directions on an industrial redundancy ring protocol (MRP).
a) Can you post the Cisco and Hirschmann switchport configs, and also which spanning tree the Hirschmann are running? They need to match/support each vendors end.
b) Confirm the Hirschmann are running the latest recommended stable software version. This is important in my experience for bug fixes on their end.
c) Can you post “show spanning-tree summary” from your Cisco?
d) Try adding “no spanning-tree etherchannel guard misconfig” for a period of time, and see if the problem disappears. If so, then it’s a mismatched configuration issue between the two vendors.
e) Why do you even have a Etherchannel trunk configured? Your post above shows that you only have vlan31 enabled, so using a normal switchport configuration may easily solve the problem. The following works for all our Cisco<>Hirchmann interface links: switchport
switchport access vlan 31
no shutdown
(both vendors are running rapid spanning tree)
f) Running “debug spanning-tree etherchannel” and/or “debug etherchannel event/error/detail” and capturing the problem occurring would be the way to t’shoot the root cause. You could try getting the Hirschmann admin to invoke an HA failover on their end, or invoking a ring protocol reversal to trigger the problem while your Cisco debug is enabled.
g) Hirschmanns typically run rapid spanning tree. You should have no problem participating in the Hirschmann spanning tree instance from the Cisco end, as long as you match their config. You can configure specific per-vlan spanning tree only on those vlans you’re peering on the Hirschmann end (vlan 31). (Also make sure your spanning tree priority is lower than the Hirschmann so you don’t become their root).
Finally, these issues are why you should always opt for converged OT/IT switch infrastructure. Next time have your integrators use your Cisco switches (or separate ones) instead of Hirschmann. Cisco IE 31/32/33/3400 series support all of the industrial protocols really well.
Hey thanks for the great answer.
a) as far as the Ciscos go, it's currently changed to this: interface Port-Channel17 description AUTOMATION switchport access vlan 31 switchport mode access speed 1000 duplex full spanning-tree bpdufilter enable
Unfortunately I don't have direct access to the hirschmanns
b) I can confirm they are definitely NOT running the latest firmware. Industrial equipment types and their refusal to update... they have made a change request to do it, hope it'll help (and that they'll manage before a few months)
c) I'll add it as a reply due to the 1000 character limit. Mostly default (open to recommendations actually - I'm a sysadmin required to also do networks, not really a network engineer)
d) will do
e) we have two stacked switches, it's to provide redundancy on our side. They're checking of that switch can be replaced with stacked switches on theirs as well (don't know if Hirschmann can do that). Rapid PVST on both sides is enabled.
f) I was not aware of what I could do to troubleshoot and so I simply shut / no shut the ports to recover communication. If it ever happens again, now I know, thanks
g) they do and that was the thought behind the initial configuration - but now I figure it would be best to avoid it completely and remove the headache, they use a single vlan and the likelyhood of a loop is near zero
I agree entirely on your end comment, I wish I set up this infrastructure myself, but I inherited it, and the OT side is entirely managed by an external company. I have zero visibility and it's a pain.
Thanks again for your thorough response, I appreciate it very much.
Make sure your trunks are the same vlans, make sure your speed and duplex are the same, make sure port channel configuration has the same speed/settings, and lacp configuration is the same
They're access ports
You'll want to figure out the root cause, and you could also set up error recovery if this is a rare issue. e.g.,
errdisable recovery cause channel-misconfig
errdisable recovery interval 60
yeah setting recovery is a partial solution but I'd really like to solve the actual problem...
I’d recommend setting both the port channel and switchports modes explicitly to access (switchport mode access). By default they’re dynamic auto and will attempt to go to a trunk and with the other end outside of your control (plus the unknown of how the Hirschmann handles this) it may be triggering a mismatch.
Quite possible actually, I'll do that thanks.