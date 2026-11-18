# pfSense or OPNsense - which software gateway should I choose for the network?

Choosing the right solution for building a firewall becomes key for businesses and individuals who want to protect their networks from security threats, configure more complex network settings and effectively manage traffic. In this post, we will compare pfSense with OPNsense, including their functions and differences, to help you make an informed choice to protect your network.

## What is pfSense?

Most people have heard of pfSense. It is a popular, free Firewall based on FreeBSD, an operating system known for its reliability and performance. This multifunctional platform offers a rich set of features, including:

- static firewall
- Network Address Translation (NAT)
- Virtual Private Network (VPN) support
- traffic management and prioritization
- Intrusion prevention
- system DHCP server and DNS resolver and server
- IPv6 support
- high availability and switching
- two proxy types to choose from
- Extensibility with plugins

## How pfSense works?

pfSense checks incoming and outgoing network traffic and applies firewall rules to allow or prohibit data transmission based on certain criteria. In addition, users can use advanced features such as intrusion detection and web filtering to protect their networks from new threats and dangers.

The platform also offers an extensive set of monitoring tools that allow users to get a more accurate picture of the performance and security of their network.

Although pfSense is a powerful and mature platform, it has its drawbacks. For some users, the interface (UI) may seem less intuitive and a bit outdated than OPNsense. In addition, the community-oriented pfSense development model may lead to slower adoption of new features and technologies than a more structured OPNsense release cycle.

Finally, Netgate, the company behind pfSense, has recently focused on the commercial Enterprise version, practically ceasing to release updates for free. Today it is no longer possible to say that pfSense is an open source product that has been audited by the community. However, this does not beg for its merits.

## What is OPNsense?

OPNsense is a free, open source firewall and routing platform based on HardenedBSD. It was created as an offshoot of pfSense in order to provide a more modern and secure alternative to users. OPNsense focuses on code quality, security and convenience, offering an intuitive graphical user interface (GUI), reliable traffic encryption and many advanced features.

Thanks to an active developer community and a growing user base, OPNsense has established itself as a reliable and powerful firewall solution.

## OPNsense features

Some of the key features of OPNsense include:


- Firewall with status tracking: Like pfSense, OPNsense offers a stateful firewall that monitors active connections and applies rules based on connection status.
- Network Address Translation (NAT): OPNsense supports NAT by allowing users to map multiple internal IP addresses to a single public IP address.
- VPN Support: OPNsense supports various VPN protocols, including IPsec, OpenVPN and L2TP, Wireguard and others, providing secure connection of remote users and networks. There is no emphasis on OpenVPN, as in pfSense, and users have a richer choice of tunnel construction tools.
- Intrusion detection and prevention: OPNsense includes a content filtering system, which, although it is paid and supplied by a third-party company, is still there. There is no such variation for pfSense yet.

In everything else related to traffic prioritization, filtering and broadcasting, it is practically a copy of pfSense.

## OPNsense cons

Despite its many advantages, OPNsense has some disadvantages. For example, a smaller list of supported hardware.

In addition, in comparison with OPNsense, the pfSense distribution is the principle of "less is better". Yes, OPNsense has a much richer selection of components, but there are also much more complaints from users about their poor performance.

## From a technical point of view: differences

pfSense and OPNsense share a common ancestry, as OPNsense was originally separated from pfSense. However, over time, both projects developed in different directions.

One of the main differences is their underlying operating systems: while pfSense is built on FreeBSD, OPNsense uses HardenedBSD, a security-oriented fork of FreeBSD.

In addition, OPNsense has a more demanding network setup and a fixed release cycle consisting of two major releases per year and weekly security updates. This allows you to include more recent versions of software packages with minor code fixes in the distribution.

But the most significant, perhaps, is the openness of the OPNsense platform code – something that pfSense has not had since recently.

## The main differences between pfSense and OPNsense

Thus, the main differences between pfSense and OPNsense include:

  


Basic operating system: pfSense is built on FreeBSD, while OPNsense is based on HardenedBSD.


- User Interface: OPNsense offers a more modern and intuitive graphical interface than the traditional pfSense interface.
- Security and Code Quality: Both platforms prioritize security, but OPNsense's more structured approach to integrating additional features and its emphasis on code quality can lead to increased overall security.
- Traffic Prioritization capabilities: Both firewalls provide packet queue management, but OPNsense has a more advanced implementation.
- Intrusion Detection Systems: Both platforms support IDS, but OPNsense is generally considered to have a more robust implementation.
- Plugin Availability and Integration: While OPNsense offers a wider selection of plugins, pfSense is more selective, prioritizing package stability.

One of the most significant advantages of pfSense over OPNsense is the DNS query filtering package, pfBlockerNG, which allows you to centrally restrict the display of ads and surveillance of users. OPNsense uses the notorious PiHole for this purpose, but pfBlockerNG can also work with IP addresses and is successfully integrated into the Web interface of the platform.

## Comparison of pfSense and OPNsense user interface

The user interface is a very important aspect when choosing a firewall solution. OPNsense boasts a modern and intuitive interface with multiple themes to choose from. The only disadvantage of the OPNsense interface is that it is too small.

On the other hand, the pfSense interface is more traditional and may seem less intuitive, especially to beginners. However, it is perfectly readable on any screens with any resolution, and does not slow down on weak computers.

## Plugins available for pfSense vs OPNsense

Both pfSense and OPNsense support third-party plugins that allow users to extend the functionality of their firewalls with additional features such as web filtering, a DHCP server or a direct caching proxy server.

## pfSense and OPNsense VPN Capabilities

Virtual Private Networks (VPNs) are necessary to securely connect remote offices, employees, or devices to a central network. Both pfSense and OPNsense offer VPN support, including easy configuration of the OpenVPN, IPsec and L2TP client.

However, some users may find the VPN implementation in OPNSense more user-friendly and easy to set up, especially for those with limited technical knowledge. OPNsense supports more VPN protocols, including native Wireguard support, a plugin for Zerotier and others.

### Advanced Routing and Network Address Translation (NAT)

Both pfSense and OPNsense support advanced routing features, including dynamic routing protocols such as OSPF and BGP. They also offer a Network Address Translation (NAT) feature that allows users to map multiple internal IP addresses to a single public IP address.

This is important for managing network resources and ensuring uninterrupted communication between internal and external networks.

### High availability and load balancing

High availability and load balancing are essential for complex network settings that require maximum uptime and optimal performance. Both pfSense and OPNsense support hardware configurations for failover and redundancy, ensuring that network services remain operational even in the event of a hardware failure.

Load balancing features help distribute network traffic evenly across multiple connections or servers, preventing bottlenecks and improving overall performance.

### Web filtering and proxy server capabilities

Web filtering and proxy server capabilities are critical for businesses, schools and organizations that need to control Internet access and protect their networks from malicious content.

Both pfSense and OPNsense offer web filtering features using third-party plugins such as Squid and SquidGuard. These tools can block access to certain websites or categories, monitor Internet usage, and apply content restrictions based on user-defined policies.

### Captive portal and wireless network support

Captive portal technology is essential for businesses and public Wi-Fi networks that require user authentication and access control. Both pfSense and OPNsense offer captive portal functionality that allows network administrators to configure a login page, manage user access, and enforce bandwidth restrictions.

In addition, both firewalls support wireless network configurations, allowing users to integrate wireless access points and manage their Wi-Fi networks along with wired connections.

### Управление правилами брандмауэра и переадресация портов

Эффективное управление правилами бра

### Configuration synchronization and backup

The ability to synchronize settings and backup configuration data is necessary for organizations with multiple firewall devices or complex network configurations. Both pfSense and OPNsense offer configuration synchronization features that allow users to replicate settings across multiple devices and ensure consistent policy enforcement.

In addition, both platforms provide options for backup and recovery of configuration data, guaranteeing users a quick recovery from hardware failures or configuration errors.

### Customization and extensibility

Both pfSense and OPNsense are designed with customization and extensibility in mind, allowing users to adapt platforms to their unique needs.

This includes changing the user interface, integrating additional features with plugins and third-party packages, as well as developing custom functionality using platform APIs and development tools.

## Conclusions

Both platforms offer a robust set of features and capabilities, reliable support for advanced security practices, and integration of additional tools and services. However, it seems that pfSense has stopped in its development, and OPNsense

Ultimately, the choice between pfSense and OPNsense will depend on personal preferences and the specific needs of your network. In any case, you can't go wrong with either pfSense or OPNsense, since both solutions will effectively protect your network.

*Ron Amadeo 25/09.2023*