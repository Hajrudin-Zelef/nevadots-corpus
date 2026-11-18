Configuring a VPN Appliance for Site to Site Connections
The VPN Appliance is powered by OPNsense, a leading open source network security platform based on FreeBSD. See https://www.opnsense.org. 
This is a powerful vpn platform with security built in protocols that will allow you to easily create a site to site VPN tunnel between one or more of your servers and external peers. The configuration of the VPN Appliance is done using the OPNsense user interface. Some configuration settings on your server must be performed via the command line. This guide explains how to set up your site to site VPN. If you would like to have GSS perform the setup and configuration for you, contact office@gss-portal.com
This is a step by step instruction on how to configure a Site to Site vpn with opnsense.
- Step 1 > Access your vpn management interface > go to VPN > select IPSEC > Tunnel Settings and
configure a new phase 1 details > Connection Method, Key Exchange, Interface, Description:
- Continue setting up the phase 1 connection details with: 
Authentication methods, Connection Identifier, Pre shared key, Encryption Algorithm, dh key group
- Make sure to check tunnel isolation box.
- Click the + sign to start a new phase 2 setup.
- Add a new tunnel (phase 2 details)
Local Network, Remote Network, Encryption Protocol, Encryption Algorithm, Hash Algorithm, pf
- Enable IPSEC for the connection.
- Check status of the connection.