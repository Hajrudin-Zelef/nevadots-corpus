FortiGate VPN goes down and won't reconnect 
        
    Hi,
I have a Fortigate that has an IPSec VPN setup to another FortiGate appliance.
I have the tunnel successfully established, and then randomly, the tunnel will be down and won't come back up until I reboot one device.
While the tunnel is down I have run the following tests:
- 
      Successfully ping from one device wan address to the other
- 
      Can successfully trace route from one device to the other
- 
      Run diagnose vpn ike gateway, and can see the status as connecting
- 
      Checked that IKE packets are being sent on port 500 successfully
- 
      Debug IKE and can see the following info.
I'm using IKE v2, and all my proposals and configuration is identical on both sides.
Source settings:
Destination Settings
Any ideas what would be the cause?
As stated, a reboot fixes it for a few days, then it just falls back into this state.
Cheers!
Section des commentaires
Do you have blackhole routes setup on both sides? Ran into similar issues, and flapping the port worked as well as a reboot on the remote fgt. However, without doing that the tunnel(s) would not come back up because the traffic was traversing the wrong interface. Adding the blackhole routes corrected this for me.
ETA: After adding the routes, you need to kill any active/stale sessions to get the tunnels to come back up after making those changes.
I don't have black hole routes setup. Can you please explain how to configure a little further and I'll try that out please?
Here you go: https://community.fortinet.com/t5/FortiGate/Technical-Note-Use-of-Black-hole-route-in-site-to-site-IPsec-VPN/ta-p/192526
Sorry, I thought I had posted that earlier.
Try the commands on this link:- https://community.fortinet.com/t5/FortiGate/Technical-Tip-Inbound-IPsec-traffic-dropped-due-to-layer-2/ta-p/208035 What FortiOS version are FortiGates currently on? How frequent do you get the disconnection? Is the IPsec tunnel shows Green(UP) despite no traffic is flowing? Instead of restarting the Gates, try restarting the IPsec tunnel by going to Dashboard>Network>IPsec and bring down all Phase 2.
I'm running v6.2.9 build 1234 (GA).
Disconnects happen every 2 - 5 days randomly.
The source side was showing red, but the destination green. After enabling dead peer detection to "On Idle" on both sides, now they both are red.
Don't seem to have a "Network" option in my dashboard. When I go to Monitor > IPsec monitor. I can select a tunnel, but the "Bring Down" is greyed out.
After reading through the link provided, and trying to run the "diagnose npu np6 dce 0" command.
It won't accept the "npu" portion, the command does not exist as an option after the diagnose command.
Hmm which model of FortiGate is that ? Maybe it doesnt have np6...
Any chance you can update to 6.4.x as I`m seeing some IPsec VPN issues on the release note of 6.2.9:
https://docs.fortinet.com/document/fortigate/6.2.9/fortios-release-notes/236526/known-issues
Generally NO SUITABLE IKE_SA means that the 2 Gates IPsec config (Phase 1 & 2) are not the same and hence can`t establish the tunnel. It can be Authentication(not the same pre-shared key) /Phase1(Algo,DH Groups)/Phase2 misconfiguration.
Also when I try to type the following it does not recognised the "npu-offload" command.
# config vpn ipsec phase1-interfac
edit <phase1-name>
set npu-offload disable
end
“No suitable IKE proposal” reeks of phase-1 misconfig.
Initially get rid of all the default proposals on the P1/P2 settings and choose the the proposal you actually want to use. Make sure timers are identical. AES128-GCM and SHA384 with DH 21 is a good start for very secure encryption. Cramming 4-5 proposals in IKE and leaving it up to negotiation is not a good approach.
Failing that, post up a sanitised copy of “show vpn ipsec phase1-interface” and “show vpn ipsec phase2-interface” from the CLI output.
Thanks for the suggestion, below is the config are the changes.
Source Config
FortiGate-Source# show vpn ipsec phase1-interface
config vpn ipsec phase1-interface
edit "ToDestination"
set interface "wan1"
set ike-version 2
set keylife 28800
set peertype any
set net-device disable
set proposal aes128gcm-prfsha384
set dpd on-idle
set dhgrp 21
set nattraversal disable
set remote-gw x.x.x.x
set psksecret ENC xxxxxxxxxxxxxxxxxxxxxxx
End
FortiGate-Source# show vpn ipsec phase2-interface
config vpn ipsec phase2-interface
edit "ToDestination"
set phase1name "ToDestination"
set proposal aes128gcm
set dhgrp 21
set keepalive enable
set keylifeseconds 28800
End
Destination Config
FortiGate-Destination # show vpn ipsec phase1-interface
config vpn ipsec phase1-interface
edit "ToSource"
set interface "wan1"
set ike-version 2
set keylife 28800
set peertype any
set net-device disable
set proposal aes128gcm-prfsha384
set dpd on-idle
set dhgrp 21 5
set nattraversal disable
set remote-gw x.x.x.x
set psksecret ENC xxxxxxxxxxxxxxxxxxxxxxxxxxx
End
FortiGate-Destination # show vpn ipsec phase2-interface
config vpn ipsec phase2-interface
edit "ToSource"
set phase1name "ToSource"
set proposal aes128gcm
set dhgrp 21
set keepalive enable
set keylifeseconds 28800
End
I've had exact the same issues. How is your FortiGate connected to the internet?
My FortiGate was connected to a briged G.fast router and when the IPsec tunnels disconnected I could reboot either the Forti or the Briged Router and then the tunnel came up again. So I investigated more and tryed to upgrade the FortiGate to v7.0.6 and the Firmware of the bridged router but without success.
The fix for this case was to change the bridged Router to another model. After the replacement to another model the IPsec tunnels stayed up at all time.
Thanks for your suggestion.
Indeed I do have the internet connected via a modem that is in bridge mode on the source side - this is the side I reboot when I have the issue. The connection is Fibre to the Basement, then up the building riser is copper pair. This then connects to a TP-Link Archer VR1600v router that is in bridged mode.
The LAN port of the VR1600v is connected to WAN1 of the Fortigate.
The Fortigate WAN1 is setup as DHCP and authenticates the PPoE.
Might need to try a different router to use as bridge.
Any suggestions of a good reliable model I should buy?
Thanks!
I've changed from a Zyxel XMG to Draytek Vigor166. Have no issues so far.
Just for my personal interest, could you let me know if you can bring up the IPsec tunnel when you just boot your briged router and not the FW and then try to bring-up phase 1 & 2?
I had my wan1 also as DHCP with PPPoE.
Try enabling DPD .
Now seeing this come through on the debug too.
244A448F935F36386D246D1A95DC6B8451B48E465DC35D2529BA622732D2B8AB7F000000080000402Eike 0:26a4ad471bfc5a2a/0000000000000000:4141: responder received SA_INIT msgike 0:26a4ad471bfc5a2a/0000000000000000:4141: received notify type FRAGMENTATION_SUPPORTEDike 0:26a4ad471bfc5a2a/0000000000000000:4141: incoming proposal:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: proposal id = 1:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: protocol = IKEv2:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: encapsulation = IKEv2/noneike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=ENCR, val=AES_CBC (key_len = 128)ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=INTEGR, val=AUTH_HMAC_SHA2_256_128ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=PRF, val=PRF_HMAC_SHA2_256ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP2048.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP1536.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: proposal id = 2:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: protocol = IKEv2:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: encapsulation = IKEv2/noneike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=ENCR, val=AES_CBC (key_len = 256)ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=INTEGR, val=AUTH_HMAC_SHA2_256_128ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=PRF, val=PRF_HMAC_SHA2_256ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP2048.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP1536.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: proposal id = 3:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: protocol = IKEv2:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: encapsulation = IKEv2/noneike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=ENCR, val=AES_GCM_16 (key_len = 128)ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=PRF, val=PRF_HMAC_SHA2_256ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP2048.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP1536.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: proposal id = 4:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: protocol = IKEv2:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: encapsulation = IKEv2/noneike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=ENCR, val=AES_GCM_16 (key_len = 256)ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=PRF, val=PRF_HMAC_SHA2_384ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP2048.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP1536.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: proposal id = 5:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: protocol = IKEv2:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: encapsulation = IKEv2/noneike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=ENCR, val=CHACHA20_POLY1305 (key_len = 256)ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=PRF, val=PRF_HMAC_SHA2_256ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP2048.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP1536.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: matched proposal id 1ike 0:26a4ad471bfc5a2a/0000000000000000:4141: proposal id = 1:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: protocol = IKEv2:ike 0:26a4ad471bfc5a2a/0000000000000000:4141: encapsulation = IKEv2/noneike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=ENCR, val=AES_CBC (key_len = 128)ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=INTEGR, val=AUTH_HMAC_SHA2_256_128ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=PRF, val=PRF_HMAC_SHA2_256ike 0:26a4ad471bfc5a2a/0000000000000000:4141: type=DH_GROUP, val=MODP2048.ike 0:26a4ad471bfc5a2a/0000000000000000:4141: lifetime=86400ike 0:26a4ad471bfc5a2a/0000000000000000:4141: SA proposal chosen, matched gateway ToDestinationike 0: found ToDestination <SourceIP> -> <DestinationIP>:500ike 0:ToDestination:4141: processing notify type FRAGMENTATION_SUPPORTEDike 0:ToBDestination:4141: responder preparing SA_INIT msgike 0:ToDestination:4141: out 26A4AD471BFC5A2AF6D0AC11CCBD7CEF21202220000000000000017022000030000
I would personally modify phase1 and 2 at both ends ofc, encr and auth params to something more simpler, like aes128 and sha256 and also enable auto-negotiation under phase2 and have a look at it then
Thanks for the suggestion.
I've set both sides to "On Idle" but it's still not coming up.
I'm seeing this with different customers as well. 1x FortiGate 30E (firmware 6.2.X) and 1x FortiGate 60E running V6.4.9 Recently upgraded the FGT60E to 6.4.9 hoping it would fix this issue. Prior to the upgrade it was running on 6.4.9. Reboot helps for a couple of days. Both customers are using IKE v1 Aggressive mode. Haven't found a solution for this yet.
Thanks for sharing your similar experience, hopefully we can figure this out with a little luck and help from others.
Are both of your devices 50E?
What ciphers are you using in phase 1 and phase 2?
Can you show the phase 1 and 2 portion of the config? (CLI)
Yes, both are 50E's
Can't seem to post more screenshots. Here's a link.
https://siasky.net/nADbB4k7L491vdpW3tA7rL_X57S7kKMwk7gzIMhbbpG0lg
Posting the relevant portions of the text config would possibly be easily...
That said, consider the following for testing purposes:
A - reduce the phase 1 proposals to the first 2 ciphers
B - reduce the phase 2 proposals to the first 3 ciphers
C - reduce both proposals to using just DH group 5
D - change key lifetime to 28800
Test that and see what happens to the tunnel
EDIT: Formatting.