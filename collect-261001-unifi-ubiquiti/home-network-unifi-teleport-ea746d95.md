Teleport was originally released in 2018 for the AmpliFi product line of Ubiquiti. But it’s now also available on all UniFi Cloud Gateways and Next-Gen gateways. It allows you to create a VPN connection with one click from your mobile device or desktop to your home network.
With a traditional VPN, you will need to configure your network, maybe open ports, create a username and password, etc, before you can make a VPN connection. With UniFi Teleport, you only need to create an invitation link in your controller.
In this article
In this article, I will explain what you need for UniFi Teleport and how to use it.
What is UniFi Teleport
UniFi Teleport allows you to make a VPN connection to your own network with one click. It uses the WireGuard VPN protocol, which is commonly used by large VPN providers, like NordVPN or Surfshark.
The difference compared to these VPN providers is that with teleport you create a VPN tunnel to your own network. This is ideal when you are on a public wireless network and want to securely access your bank account or other sensitive information.
With Teleport you can not only safely browse the internet, but you can also access your home network. After you have made the VPN connection you can access all your home network devices just like when you are connected to your wireless network at home.
Connecting to Teleport
There are two ways you can connect a device with UniFi Teleport. We can use an invitation link, that we can generate in UniFi Network, or use the single-sign-on method in the WiFiman app from Ubiquiti.
The invitation link is a unique link that is only valid for 24 hours. The link can only be used by one client device. So you need to create an invitation link for each device that you want to give access to. The VPN tunnel is stored on the device after accepting the link, allowing you to use the VPN connection at any moment that you want through the Wifiman app.
Another option to connect to UniFi Teleport is to sign in with your Ubiquiti account. For this to work you need to be a site-admin and Teleport has to be enabled. If you want to offer Teleport to multiple users, then UniFi Identity might also be a good option.
Requirements for UniFi Teleport
All UniFi Cloud Gateway consoles and Next-Gen Gateways support UniFi Teleport. Other requirements for Teleport are:
- UniFi network 7.1 or later
- Remote Access enabled in UniFi OS
- WiFiman mobile or desktop app
Remote Access
Remote access to the UniFi console must be enabled to use Teleport. You can enable remote access in the Control Plane. If you are using an older version, then you will find remote access in the UniFi OS settings.
- Open the UniFi Network App
- Go to Settings > Control Plane
- Open the Console tab
- Check if Remote Access is enabled.
Enable UniFi Teleport
Enabling Teleport is really easy after you have made sure that everything is up-to-date. All we need to do is enable the feature in the UniFi Network app.
- Open the UniFi Network app
- Goto Settings > VPN
- Enable Teleport
You only need to generate a new invitation link (4) after you have enabled Teleport. Keep in mind that the link expires after 24 hours. Copy the link and send it to your mobile device for example.
Using UniFi Teleport
As mentioned there are two ways to use UniFi Teleport, but for both cases, we will first need to install the WiFiman app. This app is currently available for all operating systems and both mobile and desktop devices.
You can download the WiFiman app for desktop applications here on the UniFi download page. For mobile devices, you can find the app in the app stores, or use the QR code from the invitation link.
Connecting with WiFiman Desktop
After you have downloaded and installed WiFiman Desktop, you will see the option to connect to UniFi Teleport when you open the app. From here you have two options, log in with the site-admin account, or use the invitation link.
To log in, click on the blue link “Log in to the UI Account” or click in the top-left corner on the user icon and choose Log In. This will open the browser and allow you to sign in with your Ubiquiti account.
After you have successfully logged in, you can select your UniFi Cloud Gateway from the list and click on Connect. Once connected, you will see a green connection line between your device and the Cloud Gateway and how much data is going through the connection.
Another option to connect the WiFiman desktop app is to use the invitation link. Simply copy and paste the link in the link field and click on Connect. The connection will be remembered, allowing you to reconnect anytime when needed.
Connecting with the Mobile app
The WiFiman mobile app works similarly to the desktop version. Here we also have the option to sign in with our Ubiquiti account or use the invitation link. For the latter, we have a couple of options.
If you open the link on a desktop device, you will see a QR code that you can scan with your mobile device. The link will either take you to an introduction page, where you can download the WiFiman app, or if you already have the app installed, open the app directly.
In the app, click on Connect to add the UniFi Teleport connection. To use the connection, slide the toggle to On to make the connection with your UniFi Cloud Gateway. The first time you will be asked to install the VPN Configuration, after which the connection will be made.
It will take 5 to 10 sec for the connection to build after which you have a secure connection to the internet through your home network.
You can also use the sign-in method instead of the invitation link. Simply sign in with your site-admin account to connect to your network.
Revoking Access to Teleport
There are two ways to revoke access to the UniFi Teleport. The method depends on the status of the invitation. When the invitation is already accepted, you will need to go to Client Devices in the UniFi network, select the device, and Revoke Access under Settings.
If the invitation has not been accepted yet, then you can Revoke the invitation from the Teleport settings screen.
- Expand the Invitation History (click on Show)
- Hover over an invitation
- Click on Revoke
Wrapping Up
UniFi Teleport is a great way to easily set up and make a VPN connection through/to your home network. Just make sure that your UniFi OS and Network app are up-to-date to use this feature.
If you want to use this option with multiple users, then this method might not be the easiest to manage. A better option then is to use UniFi Identity.
Great article.. any plan to refresh about how to setup with zone base firewall rules and restrict access to other zones behind the same firewall appliance. thanks!
I already have an article about the zone-based firewall: UniFi Zone-Based Firewall – What you need to Know
I am not an NW expert but I plan to enhance my home network and install a Unifi Cloud Gateway (CG) Ultra or Max. But I will not be able to connect the CG directly to the Internet due to the router provided by my provider where reconfiguring it as pure modem is disabled. So, I will end-up with a cascaded router setup with double NAT.
As I understand from several forums this setup should work with the CG.
But I still want to access my NW behind the CG from the Internet by VPN. Would the Teleport setup work with it, or what other option do I have?
It will work just fine. Teleport can be used behind a NAT without the need for port forwarding or other firewall rules.
Second paragraph, last line, “create an invention link” – invention? I assume you mean “invitation” 😉 Thanks for all the good lazyadmin tricks and tips!
Thanks
June ’22 I reported that I couldn’t use Teleport over the cellular network. In the mean time I switched ISP’s from KPN to Odido and from that moment on my Teleport is working well. I didn’t need to enable IPv6 on my UDMP.
So my guess is that the problem might have had a relation with the way the ISP configures their equipment.
Have a new updated Cloud Gateway Ultra with latest update and wifiman desktop is connecting but can’t reach any devices on internal network (behind the Cloud Gateway Ultra)
Anyone found a workaround here?
I am not using standard 192.168.1.0 network on internal network.
Adapter Info:
Unknown adapter utun:
Connection-specific DNS Suffix . :
Description . . . . . . . . . . . : WireGuard Tunnel
Physical Address. . . . . . . . . :
DHCP Enabled. . . . . . . . . . . : No
Autoconfiguration Enabled . . . . : Yes
IPv6 Address. . . . . . . . . . . : fd37:5753:430c:4aee:b66a:e44d:1c00:2(Preferred)
IPv4 Address. . . . . . . . . . . : 192.168.2.2(Preferred)
Subnet Mask . . . . . . . . . . . : 255.255.255.255
Default Gateway . . . . . . . . . :
DNS Servers . . . . . . . . . . . : 10.3.52.1
NetBIOS over Tcpip. . . . . . . . : Enabled
Do you have any VLANs and/or traffic/firewall rules set up?
No VLAN’s – firewall rules is just standard setup.
Strange also I tried to setup 2 VPN server variants Wireguard and L2TP – and having same problem when connecting with VPN that could not reach internal network.
Just changed my IP range to 10.x.x.x, connected with wireguard (which has an IP Range of 192.168.2.x) and it all works fine. If you go to Insights > Inspection, do you see any hits on the Firewall or Traffic rules?
Nothing is shown here
No hits – no log entries
Worked after reset and new setup. Not sure what was the problem. But on new setup Cloud Gateway Ultra already had the latest software update from start.
Teleport would be fantastic, if only Unifi let you specify which DNS server is used by remote teleport users. The DNS server handed out to remote teleport users is 8.8.8.8. There is no way of changing this, for instance for internal intranet DNS to reach intranet (web)servers.
So if you don’t want to used Google’s DNS server, or want to reach intranet hostnames that rely on internal DNS servers, then Teleport simply will not work.
Such a shame, that Unifi will not let admins specify which DNS server is used by Teleport. It’s a fantastic product that even works without opening up ports to the internet. Your UDM could even be behind another router and Teleport still works!
for this question is that possible to use this teleport vpn over windows client ?
yes you can use teleport over windows you will need to download WiFiman Desktop available for windows, mac os, linux.
even you have an IP private set up on your WAN DREAM MACHINE PRO/SE. you can remote your Site location (Good to know)
What firewall rules would be required to restrict a Teleport user to a single vLAN (i.e. like a camera vLAN)?
Create the following rules:
– a LAN Out rule permitting the Teleport VPN subnet to vLAN
– a LAN In rule permitting vLAN access to the Teleport VPN subnet.
Hi, is it possible to have 2 Dream Machines : one acts like a VPN Server and the other one as a client ?
I was wondering what are the options for vpn for USG? I understand its an older device but is it possible to setup vpn on it? Thanks
Is it as secure as using Nord or Nord Layer (business)? I can’t seem to figure out what the big difference is.
UniFi teleport is mainly used to route your traffic through your own home network. So you only use it when you are not at home. Nord VPN is often used from home, to unlock Netflix content for example that is not available in your region. Or just to hide your identity on the internet.
I played around with this recently (UDM pro) and connecting worked easily but the iphone was placed onto some other IP range not my remote LAN
instead of sending the new link to your phone paste it into your browser and a QR code will appear and just use your phone to the link
is that possible to use this teleport vpn over windows client ?
Not at the moment. You will need to create a normal VPN server
There is a WiFiman App for Windows and it has Teleport inside. https://www.ui.com/download/app/wifiman-desktop
hi, I tried to connect to UDR with IPV6 enabled, It works so far wit my Android phone but without internet connection.
UDR is reachable and my local net as well but, as I wrote without internet.
VPN on L2TP does not work at all.
Uniquity/Unifi what up ?
I have reported this problem to Unifi support in June and my ticket was promoted via the Escalation Team to the Production Team. So I gathered they’re taking it seriously. But I have had no updates about the status since then.
In the meantime I read something about the role of IPv6 in this matter. One user reported that enabling IPv6 on a UDMPro was necessary to have the Teleport feature working in combination with KPN as the cellular phone provider.
I’m planning to enable IPv6 on my UDMPro, but I think that is not as simple as flipping a switch: it’ll take some time.
My setup: UDMPro, KPN fiber, iPhone XS, KPN 4G.
Dear admin
thx for your response.
Iĺl check the IPV6 option.
Cheers
Rudy, could you expain What exactly you configured on your Devices.
I’m facing exactly the same problem with GSM. On Wi-Fi it’s working.
All requirements are met.
That’s very convenient, such a one click VPN, especially to easily connect to my home devices from a remote location.
But unfortunately it seems to be restricted to mobile devices with a WLAN connection, so when there are no nearby WLAN’s I’m out of luck.
Wouldn’t it be possible to use this Teleport VPN through a GSM connection? I tried but WiFiman/Teleport does not deliver a working connection without a WiFi connection.
I have used it (and tested it again yesterday) on a GSM connection and it works perfectly fine here.