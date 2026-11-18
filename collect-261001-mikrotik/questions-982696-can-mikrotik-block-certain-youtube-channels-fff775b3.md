I want to block some adult content from YouTube.
Can MikroTik routers block certain Youtube Channels? Or certain content from YouTube?
Well, yes and no. You can block specific videos from YouTube, but as far as I know, not a certain YouTube channel due the format of YouTube URLs.
Through RouterOS, you can accomplish this using one of these two methods:
1) You can create Layer 7 Protocol specifications in the Firewall section, and then firewall rules to block them;
2) You can enable RouterOS built-in web-proxy and block them there.
Hope that helps.
Best regards.