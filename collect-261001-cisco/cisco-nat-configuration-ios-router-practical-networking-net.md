In this article, we will illustrate the Cisco NAT *configuration* on IOS Routers. This is a follow up article to the Network Address Translation article series which thoroughly covered the *operation* of NAT and answers the questions “*What is NAT?*” and “*How does NAT work?*“.

There are only four types of network address translation: Static NAT, Static PAT, Dynamic PAT, Dynamic NAT. We will look at the Cisco NAT configuration commands and explore the syntax for each of these types of address translation.

It is *highly* recommended to read each article in the NAT article series *before* attempting to configure NAT using this guide. In addition, since the configuration below applies to *Cisco* routers, we will be using Cisco NAT terminology to reference IP addresses (and/or ports) involved in the translation.

##### Contents:

### Designating Inside and Outside interfaces

The first step to configuring NAT on any Cisco IOS router is designating which interfaces should be considered â*Inside*â and which should be considered â*Outside*â.

It is easy to look at a network topology diagram to determine which interfaces are facing the Internet and which interfaces are facing the internal servers. Routers however, cannot see the entire network topology. Instead, they must be explicitly told which of their interface(s) are acting as the Outside and which interface(s) are acting as the Inside.

On Cisco Routers, the designation uses the commands **`ip nat outside`** and **`ip nat inside`**:

interface fa0/0
  **ip nat outside**
interface fa0/1
  **ip nat inside**
interface fa0/2
  **ip nat inside**

In this example, we are designating `fa0/0` as the Outside interface, and both `fa0/1` and `fa0/2` as the Inside interfaces.

With the Inside and Outside interfaces defined, we can proceed with the individual address translation configurations. Note that each item below *first requires designating Inside and Outside interfaces*.

### Static NAT

A Static NAT is a translation in which only the IP addresses are being modified, and the mapping between pre-translation and post-translation IP addresses is explicitly defined.

This is the illustration of a Static NAT from the NAT article series:

To configure Static NAT on a Cisco IOS router to match the translation depicted above, first designate the Inside and Outside interfaces, then apply the following command:

**ip nat  inside source  static  10.2.2.33  73.8.2.33**

This will create a permanent, bidirectional mapping between the Inside Local IP 10.2.2.33 and the Inside Global IP 73.8.2.33.

The command above uses the following syntax:

**ip nat  inside source  static  <Inside Local IP>  <Inside Global IP>**

The syntax is comprised of the following individual elements:

| **ip nat** | All NAT commands are preceded with these two words. | 
| **inside source** | Translate the source of packets arriving on interfaces labeled with `ip nat inside` . | 
| **static** | Create a static translation (as opposed to a dynamic translation). | 
| **<Inside Local IP>** | Address of the Inside host, as seen from the Inside network. | 
| **<Inside Global IP>** | Address of the Inside host, as seen from the Outside network. | 

### Static PAT

A Static PAT is a translation in which the IP Addresses *and* Port numbers are being modified, and the mapping between pre-translation and post-translation attributes is explicitly defined.

This is the illustration of a Static PAT from the NAT article series. Click the tabs to view the Outbound or Inbound flow:

To configure Static PAT on a Cisco IOS router to match the translation depicted above, first designate the Inside and Outside interfaces, then apply the following commands:

**ip nat  inside source  static  tcp  10.4.4.41 8080  73.8.2.44 80  extendable**
**ip nat  inside source  static  tcp  10.4.4.42 443  73.8.2.44 443  extendable**

This will create two permanent IP:Port mappings. The first between 10.4.4.41:8080 and 73.8.2.44:80, and the second between 10.4.4.42:443 and 73.8.2.44:443.

The commands above use the following syntax:

**ip nat  inside source  static  <protocol>  <Inside Local IP:Port>  <Inside Global IP:Port>  extendable**

The syntax is comprised of the following individual elements:

| **ip nat** | All address translation commands are preceded with these two words. | 
| **inside source** | Translate the Source IP of packets arriving on interfaces labeled with `ip nat inside` . | 
| **static** | Create a static translation (as opposed to a dynamic translation). | 
| **<protocol>** | Designates which protocol is being translated, typically this will be TCP or UDP. | 
| **<Inside Local IP:Port>** | Attributes of the Inside host, as seen from the Inside network. | 
| **<Inside Global IP:Port>** | Attributes of the Inside host, as seen from the Outside network. | 
| **extendable** | Allow a single global address to be mapped to multiple local address. | 

The **`extendable`** parameter is what allows a *single* global address to be mapped to *multiple* local addresses (as we did in our example). The parameter could be omitted if you were explicitly mapping ports between *one* global address and *one* local address, as you might in a hole punching scenario.

Some versions of Cisco IOS automatically append the **`extendable`** parameter every time you configure a Static PAT. The existence of this parameter causes no negative side effect, even if a global address is only mapped to a single local address.

As with the Static NAT configuration above, a Static PAT is bidirectional and applies to both outbound and inbound traffic.

### Dynamic PAT

A Dynamic PAT is a translation in which the IP addresses *and* Port numbers are being modified, and the mapping between pre-translation and post-translation attributes is dynamically determined by the Router.

Said another way, a Dynamic PAT allows multiple internal hosts with Private IP addresses to share one (or more) Public IP addresses.

This is the illustration of a Dynamic PAT from the NAT article series. Click the tabs to view the Outbound or Inbound flow.

To configure Dynamic PAT on a Cisco IOS router to match the translation depicted above, first designate the Inside and Outside interfaces, then apply the following commands:

**ip access-list standard *INSIDE-NET*
 permit 10.6.6.0 0.0.0.255
ip nat pool *SHARED-IP* 32.8.2.66 32.8.2.66 prefix-length 24
ip nat inside source list *INSIDE-NET* pool *SHARED-IP* overload**

There are three parts to the configuration:

1. Defining the pre-translation addresses
2. Defining the post-translation addresses
3. Configuring the NAT statement

#### Defining the pre-translation addresses

The first step is to identify which addresses must be translated. The predominant tool to identify traffic on an IOS Router is an Access-List (ACL). This is the syntax for the ACL configuration above:

**ip access-list  standard  <ACL Name>
  permit  <Network ID>  <Wildcard Mask>**

| **ip access-list** | Command to configure an access-list. | 
| **standard** | Designates that this ACL is only matching on Source IP. As opposed to an `extended` ACL which can match on Source and Destination IP â which would only be required in a Policy NAT. | 
| **<ACL Name>** | The name of this particular access-list. This will be used later to tie this ACL to a NAT statement. | 
| **permit** | The keyword designating we are matching on the specified type of traffic. | 
| **<Network ID>** | The network ID of the traffic intended to be translated. | 
| **<Wildcard Mask>** | The wildcard match correlating to the network ID of the traffic intended to be translated. | 

Additional instances of **permit <Network ID> <Wildcard Mask>**

In the configuration above, we are configuring a â*named,* *standard*â access-list, but any type of access-list can be configured. The access list only needs to identify the traffic to be translated.

#### Defining the post-translation addresses

Now that we have defined which addresses should be translated, the next step is to define what they should be translated to. This will be defined in a construct known as an IP NAT Pool:

**ip nat pool  <Pool Name>  <Start IP> <End IP>  prefix-length <CIDR>**

| **ip nat pool** | Command to define an IPv4 address NAT Pool. | 
| **<Pool Name>** | The name of this NAT Pool. This will be used later to tie this pool to a NAT statement. | 
| **<Start IP> <End IP>** | Specifies the inclusive range of addresses in the NAT pool. If you are only translating traffic to *one* IP address, the Start IP and End IP in the command will be identical. | 
| **prefix-length <CIDR>** | Ensures every IP address identified in the start/end range prior is contained in the same IP subnet. | 

The **prefix-length <CIDR>**

ip nat pool  <Pool Name>  <Start IP> <End IP>  **netmask <Subnet Mask>**

#### Configuring the `ip nat` statement

Finally, now that we have defined both the addresses that are being translated and what they are being translated to, we can tie them together with an **ip nat**

**ip nat  inside source  list <ACL Name>  pool <NAT Pool>  overload**

| **ip nat** | All address translation commands are preceded with these two words. | 
| **inside source** | Translate the Source IP of packets arriving on interfaces labeled with `ip nat inside` . | 
| **list <ACL Name>** | Designates the ACL which identifies the pre-translation addresses. | 
| **pool <NAT Pool>** | Designates the NAT Pool which identifies the post-translation addresses. | 
| **overload** | This keyword allows the addresses in the NAT Pool to be used by multiple internal hosts. This keyword is what makes this configuration a Dynamic PAT — without this keyword you would be configuring a Dynamic NAT. | 

If a dedicated shared IP or IP Range is not available and instead you wish to use a particular interfaceâs address as the shared IP address, you may specify an interface instead of using a NAT pool:

ip nat  inside source  list <ACL Name>  **interface <Intf>**  overload

For example, to configure the traffic which matches the access-list INSIDE-NET to be translated using Dynamic PAT to share the IP address of Ethernet0/0, you would use the following syntax:

ip nat  inside source  list INSIDE-NET  **interface Eth0/0**  overload

When defining the post-translation address as an Interface IP address, configuring an IP NAT Pool would not be required.

### Dynamic NAT

A Dynamic NAT is a translation in which only the IP addresses are being modified, and the mapping between pre-translation and post-translation IP addresses is dynamically determined by the Router.

Said another way, a Dynamic NAT allows multiple internal hosts with Private IP addresses to temporarily own a dedicated Public IP address so long as they have an active session.

It should be stated that traditionally when multiple internal hosts need to share IP addresses, a Dynamic *PAT* is used (despite often being mistakenly called Dynamic *NAT*). True Dynamic *NAT* is rarely used in the industry.

This is the illustration of the Dynamic NAT from the NAT article series:

To configure Dynamic NAT on a Cisco IOS router to match the translation depicted above, first designate the Inside and Outside interfaces, then apply the following commands:

**ip access-list standard *INSIDE-NET*
 permit 10.7.7.0 0.0.0.255
ip nat pool *SHARED-IPs* 54.5.4.1 54.5.4.3 prefix-length 24
ip nat inside source list INSIDE-NET pool *SHARED-IPs***

There are three parts to the configuration, and they are nearly identical to the configuration of a Dynamic PAT â with one key difference.

The first two parts of the configuration are identical to a Dynamic PAT: configure an access-list to define the pre-translation addresses and configure an IP NAT Pool to define the post-translation addresses.

The third part, which ties the two prior parts together, is *nearly* identical to the **ip nat***exclusion* of the **`overload`** keyword.

*Without* the **`overload`** keyword, the Router will only translate the source *IP address* of internal hosts to an available address in the NAT Pool. Since the Port is *not* being translated, there can only be *one* active translation for each IP address. Consequently, if you have more internal hosts than you have available IP addresses in your NAT Pool, traffic from some hosts will be dropped until IP addresses become available.

*With* the **`overload`** keyword, the Router will translate the source *IP and Port* as necessary to ensure every internal host will always have an external address they can use when speaking through the NAT router. Each IP address in the NAT Pool can allow approximately 65,000 connections from any number of internal hosts.

### Summary – Cisco NAT Configuration

To conclude this article, below is a summary of all the NAT syntax commands we discussed above:

| **Designating Inside and Outside Interfaces** | interface fa0/0   ip nat outside  interface fa0/1   ip nat inside  interface fa0/2   ip nat inside | 
| **Static NAT** | ip nat  inside source  static  <Inside Local IP>  <Inside Global IP> | 
| **Static PAT** | ip nat  inside source  static  <protocol>  <Inside Local IP:Port>  <Inside Global IP:Port>  extendable | 
| **Dynamic PAT**(NAT Pool) | ip access-list  standard  <ACL Name>   permit  <Network ID>  <Wildcard Mask>  ip nat pool  <Pool Name>  <Start IP> <End IP>  prefix-length <CIDR>  ip nat  inside source  list <ACL Name>  pool <NAT Pool>  overload | 
| **Dynamic PAT**(Interface IP) | ip access-list  standard  <ACL Name>   permit  <Network ID>  <Wildcard Mask>  ip nat  inside source  list <ACL Name>  interface <Intf>  overload | 
| **Dynamic NAT** | ip access-list  standard  <ACL Name>   permit  <Network ID>  <Wildcard Mask>  ip nat pool  <Pool Name>  <Start IP> <End IP>  netmask <Subnet Mask>  ip nat  inside source  list <ACL Name>  pool <NAT Pool> | 

The main goal of this article was to explore the Cisco NAT configuration syntax on an IOS Router. This article answers the question “*How to configure NAT?*“, while the NAT article series answers the questions “*What is NAT?*” and “*How does NAT work?*” The combination of the series and this configuration guide should give you everything you need to know to configure NAT on a Cisco IOS Router.

A wonderful explanation, thanks for sharing your time with us, maybe a great idea would be to add NAT in CISCO Firewalls

Hi Carlos, glad you enjoyed it! I actually

justfinished an article on NAT for ASAs. I’m in my final proofreading phase. That will be uploaded by the end of the month. Stay tuned =)
Edit: The ASA NAT configuration article is live!

“ip nat inside source list pool overload” is not the example of dynamic nat instead it would be dynamic pat. May be a typo

Oops, good catch. Yes, that was a typo. It is fixed. Thank you =)

Did you have the link to your NAT article on ASA?

Yup:

http://www.practicalnetworking.net/stand-alone/cisco-asa-nat/

really helpful, thank you guy.

Hello,

I want to accomplish a hairpin NAT. For my home Mikrotik router this is really easy, see

https://wiki.mikrotik.com/wiki/Hairpin_NAT

In other words just add another SNAT statement for the server to access it with pub IP.

For cisco ios router it seems you need an NVI and loads of config rules.

Is it possible to do it with snat like the mikrotik example?

Tnx

Love u ….. “Is it magic or what ” ….I do not need to go through long videos any more . Feeling excited and its night 3 am and i can stop going through your article. ð ð ….” Respect for your work”

really great explanation of NAT & PAT – finally I got it right this time

Thanks for sharing informative content!

You’re welcome, Devid!

You can see all excursions, tours and activities in Fethiye with <a href=”https://www.fethiyetours.com”>Fethiye Tours</a>

http://fethiyetours.com/ tours, activities and excursions in Fethiye Turkey

great sir beneficial article thanks for sharing.

Old is gold. Good information. I like this website very much.

One more thing I want to say about this website is that it is very informative and we can learn something technical working very naturally.

By the way if anyone is interested in ctypto then plz visit my site.

Thank you very much for the information. Great Article.

Concise and correct. Great work.