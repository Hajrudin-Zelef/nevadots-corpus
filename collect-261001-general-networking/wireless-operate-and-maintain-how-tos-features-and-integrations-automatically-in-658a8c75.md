Automatically Integrating Cisco Umbrella with Meraki Networks
Learn more with these free online training courses on the Meraki Learning Hub:
Overview
Warning: Cisco Meraki deprecated the ability for customers to automatically integrate Cisco Umbrella (Umbrella) with their Meraki organizations on April 26, 2025. After the deprecation date, however, customers may still manually integrate Umbrella with their new Meraki organizations by following the instructions in our documentation.
Automatic Umbrella integration allows Meraki administrators to link their Meraki dashboard with the Cisco Umbrella dashboard effortlessly and easily assign predefined Umbrella content filtering and security policies to protect wireless clients from malicious content or unwanted web resources.
Once you assign desired Umbrella policies, MR access points will intercept all DNS requests from wireless clients and redirect them to Cisco's Umbrella for evaluation. Based on the disposition from Umbrella, the client’s request will either be allowed or blocked.
Warning: If Automatic Umbrella is explicitly provisioned, using Manual integration with Umbrella in such organizations will not be possible. This process is irreversible.
Prerequisites
This integration is available in both per-device (PDL) and co-termination (Co-Term) licensing models.
Note: Before May 2022, adding an MR Advanced or MR Upgrade license to a co-term organization would initiate a PDL conversion process; however, this is no longer the case. Conversions from co-term to PDL are no longer supported, as referenced in the Per-Device Licensing article.
Note: MR Upgrade licenses are intended to be used as an add-on for MR Enterprise licenses and are not standalone licenses.
Requirements for PDL Organizations
- 
    All MR access points in the desired Meraki network must have MR Advanced or MR Enterprise + MR Upgrade licenses.
- 
    Network(s) where you wish to enable this integration must run to MR 26.1 or more recent firmware.
Note: Due to the nature of the PDL model, it’s possible to have this integration enabled in some (but not all) networks within a single Meraki organization.
Requirements for Co-Term Organizations
- 
    All MR access points in the Meraki organization must have MR Advanced or MR Enterprise + MR Upgrade licenses. Once your Meraki organization meets this condition, its MR Product Edition will change to Advanced Enterprise.
- 
    Network(s) where you wish to enable this integration must run to MR 26.1 or more recent firmware.
Note: Due to the nature of the co-term model, it’s not possible to have this integration enabled in some (but not all) networks within a single Meraki organization. Please refer to the Meraki MR Licensing Guide to learn more about MR license types.
Predefined Umbrella Policies in the Meraki Dashboard
There are seven predefined Umbrella policies, which consist of different combinations of security settings and content filtering.
Security & Appropriate Use Filtering
- 
    Security & Full Appropriate Use Filtering
- 
    Security & Basic Appropriate Use Filtering
- 
    Security Filtering Only
- 
    Security & Moderate Appropriate Use Filtering (Default)
Appropriate Use Filtering
- 
    Full Appropriate Use Filtering
This policy is targeting a corporate SSID use case with the most restrictive content policies. Corporate employees should not have access to categories like Alcohol, Chat, Dating, Drugs, Gambling, Games, Instant Messaging, Lingerie/Bikini, Nudity, Photo Sharing, Pornography, Social Networking, Video Sharing, and others.
- 
    Moderate Appropriate Use Filtering
This policy is meant for the guest SSID use case. Content settings for this category will allow users to access common chat apps (e.g. Facebook Messenger, WhatsApp), file storage platforms (e.g. box.com, dropbox.com), photo sharing (e.g. instagram.com), social networking (e.g. facebook.com, twitter.com), and video sharing (e.g. youtube.com) while being blocked from visiting Drugs, Gambling, Hate/Discrimination, Lingerie/Bikini, Nudity, Pornography, Terrorism, Weapons, and other content categories that should not be accessed on a typical guest wireless network.
- 
    Basic Appropriate Use Filtering
This policy is meant for school environments. Students will be protected from viewing inappropriate content, while still being allowed to do the necessary research for their classwork or homework.
Predefined Umbrella Policies Breakdown
Security & Full Appropriate Use Filtering
Security settings:
- Malware
- C&C Callbacks
- Phishing Attacks
- Cryptomining
Content filtering settings:
- 
    Adult Themes
- 
    Adware
- 
    Alcohol
- 
    Chat
- 
    Classifieds
- 
    Dating
- 
    Drugs
- 
    File Storage
- 
    Forums/Message Boards
- 
    Gambling
- 
    Games
- 
    German Youth Protection
- 
    Hate/Discrimination
- 
    Instant Messaging
- 
    Internet Watch Foundation
- 
    Lingerie/Bikini
- 
    Nudity
- 
    P2P/File Sharing
- 
    Photo Sharing
- 
    Pornography
- 
    Proxy/Anonymizer
- 
    Sexuality
- 
    Social Networking
- 
    Tasteless
- 
    Terrorism
- 
    Video Sharing
- 
    Visual Search Engines
- 
    Weapons
- 
    Webmail
Security & Basic Appropriate Use Filtering
Security settings:
- Malware
- C&C Callbacks
- Phishing Attacks
- Cryptomining
Content filtering settings:
- 
    German Youth Protection
- 
    Internet Watch Foundation
- 
    Pornography
- 
    Proxy/Anonymizer
- 
    Sexuality
- 
    Tasteless
Security Filtering Only
Security settings:
- Malware
- C&C Callbacks
- Phishing Attacks
- Cryptomining
Content filtering settings - none.
Security & Moderate Appropriate Use Filtering (Default)
Security settings:
- Malware
- C&C Callbacks
- Phishing Attacks
- Cryptomining
Content filtering settings:
- 
    Adware
- 
    Alcohol
- 
    Dating
- 
    Drugs
- 
    Gambling
- 
    German Youth Protection
- 
    Hate/Discrimination
- 
    Internet Watch Foundation
- 
    Lingerie/Bikini
- 
    Nudity
- 
    Pornography
- 
    Proxy/Anonymizer
- 
    Sexuality
- 
    Tasteless
- 
    Terrorism
Full Appropriate Use Filtering
Security settings - none
Content filtering settings:
- 
    Adult Themes
- 
    Adware
- 
    Alcohol
- 
    Chat
- 
    Classifieds
- 
    Dating
- 
    Drugs
- 
    File Storage
- 
    Forums/Message Boards
- 
    Gambling
- 
    Games
- 
    German Youth Protection
- 
    Hate/Discrimination
- 
    Instant Messaging
- 
    Internet Watch Foundation
- 
    Lingerie/Bikini
- 
    Nudity
- 
    P2P/File Sharing
- 
    Photo Sharing
- 
    Pornography
- 
    Proxy/Anonymizer
- 
    Sexuality
- 
    Social Networking
- 
    Tasteless
- 
    Terrorism
- 
    Video Sharing
- 
    Visual Search Engines
- 
    Weapons
- 
    Webmail
Moderate Appropriate Use Filtering
Security settings - none
Content filtering settings:
- 
    Adware
- 
    Alcohol
- 
    Dating
- 
    Drugs
- 
    Gambling
- 
    German Youth Protection
- 
    Hate/Discrimination
- 
    Internet Watch Foundation
- 
    Lingerie/Bikini
- 
    Nudity
- 
    Pornography
- 
    Proxy/Anonymizer
- 
    Sexuality
- 
    Tasteless
- 
    Terrorism
Basic Appropriate Use Filtering
Security settings - none
Content filtering settings - block the following categories:
- 
    German Youth Protection
- 
    Internet Watch Foundation
- 
    Pornography
- 
    Proxy/Anonymizer
- 
    Sexuality
- 
    Tasteless
Security Categories Definitions
- 
    Malware - Block requests to access servers hosting malware and compromised websites through any application, protocol, or port.
- 
    Command Control Callbacks - Prevent compromised devices from communicating with hackers' command and control servers through any application, protocol, or port, and help identify potentially infected machines on your network
- 
    Phishing Attacks - Protect users from fraudulent hoax websites designed to steal personal information
- 
    Cryptomining - Allows you to block identities from accessing known cryptomining pools where miners group together and share resources—processing power—to better gather and share cryptocurrencies, and from known web cryptomining source code repositories
Content Categories Definitions
- 
    Adult Themes—Sites that are adult in nature and are not defined in other rating categories
- 
    Adware—Sites that distribute applications that display advertisements without the user's knowledge or choice. It does NOT include sites that serve advertising
- 
    Alcohol—Sites about alcohol use, commercial and otherwise
- 
    Chat—Sites where you can chat in real time with groups of people; includes IRC and video-chat sites
- 
    Classifieds—Sites for buying and selling (or bartering) goods and services; includes sites with real-estate and housing listings
- 
    Dating—Sites for meeting other people
- 
    Drugs—Sites about illegal or recreational drug use
- 
    File Storage—Sites that offer space for hosting, sharing, and backup of digital files
- 
    Forums/Message Boards—Sites with discussions, including bulletin boards, message boards, and forums
- 
    Gambling—Sites that offer gambling or information about gambling
- 
    Games—Sites that offer game-play and information about games (news, tips, cheat codes)
- 
    German Youth Protection—Content deemed harmful to minors. This category helps prevent viewing of youth-endangering content in Germany. Blocked pages for this category will include German text. This list is not controlled by Umbrella and is created to be controlled by the BPjM (Federal Review Board for Media Harmful to Minors) to be compliant with German law. For more information, see http://www.bundespruefstelle.de/bpjm/Service/english.html. 
Note: Cisco Umbrella does not guarantee compliance with German law.
- 
    Hate/Discrimination—Sites that promote intolerance based on gender, age, race, nationality, religion, sexual orientation, or other group identities
- 
    Instant Messaging—Sites that offer access or software to communicate in real time with other individuals
- 
    Internet Watch Foundation (IWF)—Sites that contain child sexual abuse content; for more information about this category, see Internet Watch Foundation
- 
    Lingerie/Bikini—Sites displaying or dedicated to clothing that could be considered adult-only
- 
    Nudity—Sites that provide images or representations of nudity
- 
    P2P/File Sharing—Sites that facilitate the sharing of digital files between individuals, especially through peer-to-peer software, including torrent sites
- 
    Photo Sharing—Sites for sharing photographs, galleries, and albums
- 
    Pornography—Anything relating to pornography, including mild depiction, soft pornography, or hard-core pornography
- 
    Proxy/Anonymizer—Sites providing proxy bypass information or services; also, sites that allow the user to surf the net anonymously or send anonymous emails
- 
    Sexuality—Sites that provide information, images, or implications of bondage, sadism, masochism, fetish, beating, body piercing, or self-mutilation; this category is not intended for LGBT-related sites that do not fall under the aforementioned criteria
- 
    Social Networking—Sites that promote interaction and networking between people
- 
    Tasteless—Sites that contain information on subjects such as mutilation, torture, horror, or the grotesque; includes pro-Anorexia and pro-suicide related sites
- 
    Terrorism—Sites that promote terrorism or are linked with terrorist organizations
- 
    Video Sharing—Sites for sharing video content
- 
    Visual Search Engines—Sites that allow searching for images based on keywords
- 
    Weapons—Sites about weapons, commercial and otherwise
- 
    Webmail—Sites that offer the ability to send or receive email
Note: If Umbrella blocks a website based on configured content filtering policy, you can do a domain lookup on OpenDNS.com to learn more about the categorization.
Note: If you are running into issues with this integration, please contact Meraki Support. A Meraki Support Engineer will escalate with Umbrella Support on your behalf if necessary. There is no need to contact Umbrella Support directly, as they cannot support this Meraki feature.
Provisioning Automatic Umbrella in Co-term and PDL Organizations
Please follow these steps to provision Automatic Umbrella:
- Claim an appropriate quantity of MR Advanced licenses or MR Enterprise + MR Upgrade licenses on the Organization > Configure > License info page. Licenses also need to be assigned to specific APs in a PDL organization.
- Go to the Wireless > Configure > Firewall & Traffic shaping page and click the Enable Umbrella protection button.
3. Select the checkmark next to Automatically provision Umbrella and click the Auto Provision button.
4. Confirm that you want to provision Automatic Umbrella.
Warning: This process is irreversible. Once the Automatic Umbrella is provisioned, using Manual integration with Umbrella in your organizations will not be possible.
5. Wait a few minutes for the provisioning process to complete.
Enabling Automatic Umbrella Integration on an SSID
- Navigate to Wireless > Configure > Firewall & traffic shaping
- Select the SSID from the dropdown on the top of the page.
- Link your Meraki organization to the Umbrella organization we automatically provisioned for you in the previous step to retrieve the list of the predefined Umbrella policies.
4. Select the desired predefined policy and save the changes at the bottom of the page.
Disabling Automatic Umbrella Integration on an SSID
To disable Umbrella protection on an SSID
- Navigate to Wireless > Configure > Firewall & traffic shaping
- Select the SSID from the dropdown on the top of the page.
- Go to Block applications and content categories and click Disable Umbrella protection.
4. Confirm your desired changes by clicking Yes and save changes on the bottom of the page.
Note: Disabling Automatic Umbrella simply "unlinks" your Meraki organization from the automatically created Umbrella organization; however, the API integration between your Meraki organization and the Umbrella organization still exists. You can always link your SSID back to your automatically provisioned Umbrella organization, as shown below.
Therefore, it's not possible to manually link such a Meraki organization to your own Umbrella organization via API. You can confirm that your Meraki organization is still connected to the automatically provisioned Umbrella organization by checking the Network-wide > Configure > General page (Cisco Umbrella account).
DNS Umbrella Exclusions
Warning: Changing an excluded domain (adding or removing) will result in all clients being temporarily disconnected from the SSID.
While it’s impossible to customize predefined Umbrella policies, it’s possible to exclude some domain names from Umbrella protection.
- Navigate to Wireless > Configure > Access control
- Select the desired SSID from the dropdown on the top of the page
- Select Bridge mode: Make clients part of the LAN in the Addressing and traffic section
- Save changes on the bottom of the page.
- Navigate to Wireless > Configure > Firewall & traffic shaping
- Select the SSID from the dropdown on the top of the page.
- Go to Block applications and content categories
- Add the desired domain names that should be excluded from Umbrella protection.
- Save changes on the bottom of the page.
DNS requests for excluded domains are not redirected to Umbrella and are forwarded to the DNS server specified by the client. This behavior is beneficial for preventing DNS requests for local resources from being sent to Umbrella instead of allowing them to reach internal DNS servers to resolve correctly. MRs automatically add the '.local' and 'in-addr.arpa' domains to be excluded from Umbrella redirection by default.
Enabling Automatic Umbrella Integration via Group Policy
- Navigate to Network-wide > Configure > Group Policies > Add a group
- Type a name for the Group Policy.
- Go to Firewall and traffic shaping section and select Custom SSID firewall and shaping rules.
4. Save changes on the bottom of the page.
- 
    Navigate back to the newly created policy.
- 
    Select Link Umbrella to [Group Policy name] under DNS layer protection (Cisco Umbrella).
8. Select a desired predefined policy from the dropdown.
9. Save changes on the bottom of the page
Note: Group Policies do not support umbrella DNS exclusions.
Applying Group Policy with Umbrella Protection to Wireless Clients
- Navigate to Network-wide > Monitor > Clients
- Click on the client and select Different policies by SSID
- Select your Group Policy name. Click Save under the Device policy drop-down.
- Click Save
Disabling Automatic Umbrella Integration in a Group Policy
- Navigate to Network-wide > Configure > Group Policies and select the desired group policy
- Click Disable Umbrella protection under DNS layer protection.
3. Click Yes in the dialog box to confirm that you wish to enable Umbrella integration.
4. Save changes on the bottom of the page
Umbrella DNS Protection Traffic Flow
This section describes the expected DNS traffic flow after Umbrella policies have been applied to a wireless client on either SSID-level or group policy-level.
- 
    A client sends a DNS query for the desired domain name (e.g., twitter.com).
- 
    Upstream MR access point intercepts the DNS query and attaches an Umbrella identifier to it, allowing Umbrella to determine which policy to enforce.
- 
    MR then encrypts the DNS query using DNSCrypt, source NATs the packet to the MR management IP, and redirects it to the appropriate Umbrella resolver.
- 
    Once received, the Umbrella resolver decrypts the DNS query and enforces the appropriate Umbrella policy (based on the attached identifier).
- 
    Umbrella returns an encrypted DNS response with the appropriate IP if the request is allowed per configured policy.
- 
    If the request should be blocked, Umbrella returns an encrypted DNS response pointing to the Umbrella block page IP address.
- 
    The client is sent to the desired domain name (e.g. twitter.com) or Umbrella Block page based on the applied policy.
Cisco Umbrella DNS filtering has the following general limitations:
- 
    If a client machine is programmed to reach out to an IP address of a remote server directly, a DNS-filtering solution like Umbrella will not prevent this communication, since there is no DNS query that MR can intercept.
- 
    If a client is using some form of end-to-end encryption (e.g. VPN solution) that encrypts traffic between the client and a remote server (including DNS queries), MR will not be able to intercept those queries and forward them to an Umbrella resolver.
DNSCrypt Compatibility
Access points that do not support 802.11ac, such as the MR18, will still be able to utilize Umbrella DNS services, but do not support the use of DNSCrypt when communicating to the Umbrella servers. All access points that are capable of 802.11ac or newer fully support the use of DNSCrypt with Umbrella DNS.
Blocking HTTPS Websites
Some websites (e.g., twitter.com, facebook.com, instagram.com, dropbox.com) have an HTTP Strict Transport Security (HSTS) security policy. This policy means that a website does not allow HTTP connections. So, for example, if a user types “dropbox.com” in the web browser address bar, which usually implies http://dropbox.com, the browser will automatically connect to https://dropbox.com.
As explained below, this presents a unique challenge for displaying an Umbrella block page.
If a user tries to visit an HTTPS website by typing "https://example.com," or if the website has an HSTS security policy (e.g., dropbox.com), the following will happen:
- 
    The client machine sends a DNS query for dropbox.com asking for an IP address.
- 
    Upstream MR access point intercepts the DNS query and attaches an identifier to it, allowing Umbrella to determine which policy to enforce.
- 
    MR then encrypts the DNS query using DNSCrypt, source NATs the packet to the MR management IP, and redirects it to the appropriate Umbrella resolver.
- 
    Once received, the Umbrella resolver decrypts the DNS query and enforces the appropriate Umbrella policy (based on the attached identifier).
- 
    Let’s say that “Full Appropriate Use Filtering” is applied to the SSID and, therefore, https://dropbox.com (File Storage) should be blocked.
- 
    Umbrella will return an encrypted DNS response pointing to the Umbrella block page IP address (e.g., 146.112.61.106). This response will be decrypted by the MR and sent to the client.
- 
    The client will try to establish a TLS session with 146.112.61.106 (Umbrella server) IP address received in the DNS response.
- 
    Umbrella’s block page presents an SSL certificate to browsers that make connections to HTTPS sites. The certificate will match the requested site name (Common Name - CN) but will be signed by the Cisco Umbrella Root Certificate Authority (CA). If this CA is not trusted by your browser, an error may be displayed. For example, you can see the TLS certificate presented by OpenDNS, Inc. for dropbox.com here:
As you can see, the main issue here is that “Cisco Umbrella Root CA” is not trusted.
- 
    Here is an example of what we would see in the Wireshark packet capture taken on the client machine:
10. Most modern browsers (like Chrome, Firefox, Safari) will prevent users from accessing a website with an untrusted/unexpected TLS certificate. Typical errors include: "Your connection is not private" (Chrome), "Did Not Connect: Potential Security Issue" (Firefox), or “Safari Can’t Open the page” (Safari). Although the error is expected, the messages displayed can confuse end-users.
We recommend installing Cisco Root Certificate on client machines to avoid these errors. This certificate can be installed per browser or device for personal use or small deployments. In addition, an automatic installation through GPO can be done for larger deployments. Note that the automated installation will only work for Internet Explorer, Edge, or Chrome users on Windows systems. If your network includes users on Firefox or Safari browsers, the manual installation procedures must be followed.
Once you have installed the Cisco Root Certificate, users will be presented with Umbrella Block Page even for HTTPS and HSTS websites.
.
NOTE: Cisco Umbrella's resolvers live at 208.67.222.222/32 and 208.67.220.220/32; Meraki sends DNS traffic to either one. Make sure any upstream devices allow bi-directional UDP 443 to these addresses.
NOTE: The instructions above presume that you have access to an existing Umbrella dashboard. If you do not have such access you can download the Cisco Umbrella Root CA certificate.
Using the Security Center to View MR DNS Events
The Meraki Security Center provides reporting functionality for MR DNS events for all networks in the organization. To view these reports, navigate to Organization > Monitor > Security Center > MR DNS Events.
It’s possible to search for a particular blocked website by adding "uri:" to the search string (e.g. “uri:exampleadult.com”) or a particular blocked client by adding "client:" to the search string (e.g. client:192.168.219.13).
You can also use the Filter option to filter DNS events by type (Content or Security) and/or action (Allowed or Blocked).
Note: Content categories are shown in gray in the Categories column while Security categories are shown in red.
Note: If the network where Umbrella is provisioned is bound to a configuration template, DNS events reported for that network will have the template name in the "Network" column.
Note: For newly provisioned Umbrella accounts, it takes up to 60 minutes to pull MR DNS events from the Umbrella dashboard. The same applies to recently added MRs after provisioning the Umbrella account. It takes 10-20 minutes to pull any subsequent new events.