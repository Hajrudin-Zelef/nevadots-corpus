---
id: collect-261121-veille-ia/opnsense-pfsense/opnsense-security-and-hardening-best-practice-guide-zenarmor-com
title: "OPNsense Security and Hardening Best Practice Guide - zenarmor.com"
domain: opnsense-pfsense
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["hardening", "security", "firewall", "opnsense", "access", "best", "system", "network"]
source: https://www.zenarmor.com/docs/network-security-tutorials/opnsense-security-and-hardening-best-practice-guide
source_anchor: ""
source_lines: [1, 620]
section: "Gemini_Google"
---
Firewalls require security and hardening to work successfully as the first line of protection against cyber attacks. Firewalls safeguard the network from external threats such as viruses, hacking attempts, and illegal access. Hardening the firewall entails putting in place a variety of security measures to prevent unwanted access and lower the risk of exploitation. Regular software upgrades, removing unnecessary services, establishing access limits, and adopting robust authentication mechanisms are all part of this.

OPNsense is a secure operating system based on HardenedBSD, which provides a strong foundation for security. It includes features like packet filtering, stateful firewall, intrusion detection and prevention, vpn, and etc. While OPNsense is secure by default, you can further enhance its security.

In this article, we outline the importance of firewall security hardening and how you can increase the security of your firewall by applying the best practices for the OPNsense platform. The topics covered in this writing are as follows:

- 
Updating and Upgrading OPNsense
- 
Changing Default Passwords
- 
Enabling Two-Factor Authentication
- 
Configuring Country Blocking
- 
Disabling SSH Connections
- 
Taking Regular Backups and Protecting Backup Files
- 
Configuring OPNsense for high availability
- 
Disabling root access For the WebGUI
- 
Restricting access to Management Portal
- 
Enabling IPS/IDS
- 
Creating Notifications for Changes

After applying the best practices explained in this article on your OPNsense firewall you may read and deploy the best practices of FreeBSD security as well to enhance hardening.

## Why are Security and Hardening Important?

Systems hardening is a set of technologies, approaches, and best practices designed to decrease vulnerability in technology applications, systems, infrastructure, firmware, and other domains. Systems hardening aims to decrease security risk by removing potential attack vectors and reducing the attack surface of the system. By deleting unnecessary programs, account functions, apps, ports, permissions, and access, attackers, and malware have fewer possibilities to penetrate your IT infrastructure.

System hardening necessitates a thorough strategy for auditing, identifying, closing, and controlling any security vulnerabilities throughout the enterprise. There are several sorts of system-hardening activities that are listed below::

- 
Operating system hardening
- 
Server hardening
- 
Endpoint hardening
- 
Database hardening
- 
Network hardening

Although the fundamentals of system hardening are universal, particular tools and procedures differ depending on the type of hardening being performed. System hardening is required throughout the whole technology lifespan, from initial installation through decommissioning at the end of its useful life. Systems hardening is a requirement of regulations such as PCI DSS and HIPAA, and cyber insurers are increasingly demanding it.

Firewall hardening refers to the practice of protecting a firewall by decreasing potential vulnerabilities through configuration adjustments and specialized actions.

### What are the Benefits of Firewall Hardening?

Firewall hardening provides the following benefits:

- 
**Improved Security:** Hardening the firewall reduces the likelihood of potential security breaches and unauthorized network access.
- 
**Compliance:** Hardening the firewall can help firms comply with PCI DSS, HIPAA, and other security and compliance laws.
- 
**Improved Performance:** Optimizing the firewall's performance and reducing the danger of overload which results in sluggish network speeds or outages is possible by fortifying it.
- 
**Increased Visibility:** Hardening the firewall increases network visibility, which aids in detecting and preventing attacks in real time.
- 
**Better Risk Management:** Hardening the firewall facilitates risk management by minimizing possible threats and weaknesses.
- 
**Increased Reliability:** Reliability is increased by lowering the risk of an outage or data loss by fortifying the firewall.

## Updating and Upgrading OPNsense

Maintaining an update of a network security appliance is essential. It is even more necessary to upgrade a network firewall because smartphones, web browsers, and desktop operating systems are constantly updated. IT experts should constantly assess and enhance their enterprise's firewall architecture and deployment to enhance network security.

OPNsense should be patched according to the severity and danger of vulnerabilities. Evaluate each patch based on your organization's inherent dangers. You may easily update your OPNsense firewall via Web UI. The OPNsense project provides many tools to rapidly patch the system, revert a package to a prior (earlier version), or revert the kernel:

- `opnsense-update` : The`opnsense-update` utility provides integrated kernel and base system upgrades through remotely acquired binary sets, in addition to package upgrades via`pkg` . The syntax of the `opnsense-update` command is given below:

```
opnsense-update [-BbCdefikPpRsuVvz] [-A mirror_abi] [-a abi_hint]
[-D device] [-l directory] [-m mirror_url]
[-n mirror_dir] [-N crypto_lib] [-r release] [-t type]
```
```
opnsense-update [-cf [-bkpt]]
```
```
opnsense-update [-LSTU [-bkp]]
```
```
opnsense-update [-GKMO]
```
The `-b`, `-k`, `-p`, and `-t` options may be stacked to generate selected updates using a minor update sequence. `-bkp` is often used to update all presently installed components simultaneously. Major upgrades are triggered with `-u` instead of `-bkp`, unless `-bkp` is explicitly given.

- `opnsense-revert` : The`opnsense-revert` utility provides the option to safely install prior versions of packages included in an OPNsense release, provided that the specified mirror caches such release. The`automatic` and`vital` package flags will be reset to their anticipated settings. The options of`opnsense-revert` utility are as follows:
  - `-i` : Ignore the outcome of the signature verification.
  - `-l` : Respect active locking mechanisms. The default behavior is to remove them and continue with the reversion.
  - `-r release` : Choose the version of the package to install. Note that the release is not the version of the particular package, but rather the version included in the OPNsense release.
  - `-z` : Utilize the snapshot directory that does not utilize minor versioning.

For example, to revert `Unbound` package to the version found in OPNsense 22.7.1, you may run the following command:

```
opnsense-revert -r 22.7.1 unbound
```
To bring `Unbound` package back to the current version, you may run the next command:

```
opnsense-revert unbound
```
- `opnsense-patch` : The`opnsense-patch` function accepts all parameters as commit hashes from the upstream git repository, downloads them, and then applies them sequentially. Patches can be reversed by reapplying them, however in order to succeed, many patches must be applied in reverse. The syntax of the`opnsense-patch` utility is given below:

```
opnsense-patch [-defiNV] [-c repo_default] commit_hash ... 
```
```
opnsense-patch [-defiNV] [-a account] [-P prefix_dir] [-p patch_level] [-r repository] [-s site] commit_hash ...
```
```
opnsense-patch -l [-c repo_default]
```
```
opnsense-patch -l [-r repository]
```
## Changing Default Passwords

Passwords are one of the most crucial security measures employed today. It is essential that the administrator and all users have secure, difficult-to-guess passwords. Having a strong password is the most crucial security measure you can take.

To improve security, we recommend that you change the default password. Failure to do so exposes the client to the danger of being hacked.

### How to change the default password on OPNsense?

You may change the default OPNsense password via CLI by following the steps below:

1. 
Connect your OPNsense node via SSH or console.
2. 
Login using default root credentials. The default credentials are "root" and "opnsense". This will display the console menu similar to the one given below: ```
*** OPNsense.localdomain: OPNsense 23.1 ***
LAN (vtnet1) -> v4: 10.10.10.1/24
MyWireGuard (wg0) -> v4: 10.45.45.2/24
WAN (vtnet0) -> v4/DHCP4: 192.168.0.34/24
HTTPS: SHA256 3E 1A C6 00 C6 D0 FE B9 B8 39 74 07 3D AC 03 02
75 1A DE 02 92 83 7B 3E F9 AC E7 70 75 DE 89 D1
SSH: SHA256 NESuGb+GNjFA9egfXIvbmqm2FE1Og6r431BE2zZRYn0 (ECDSA)
SSH: SHA256 MVpoHjreB+CQu0kE8t3E9U0/kBYzORr+OlDlJqakaZU (ED25519)
SSH: SHA256 5q/ESIEfJsO29/1BSlD1rRg1tdV5nfj8x/p+1RpU6NQ (RSA)
0) Logout 7) Ping host
1) Assign interfaces 8) Shell
2) Set interface IP address 9) pfTop
3) Reset the root password 10) Firewall log
4) Reset to factory defaults 11) Reload all services
5) Power off system 12) Update from console
6) Reboot system 13) Restore a backup
Enter an option:
```
3. 
Type `3` for selecting`3) Reset the root password` option.```
The root user login behavior will be restored to its default.
Do you want to proceed? [y/N]: y
Type a new password:
Confirm new password:
```
4. 
Press `y` for confirmation.
5. 
Type a new password.
6. 
Retype the password for verification.

You may change the default OPNsense password via Web UI by following the steps below:

1. 
Connect your OPNsense node via your favorite browser.
2. 
Login using default root credentials. The default credentials are "root" and "opnsense".
3. 
Navigate to the **System** >**Access** >**Users** .

**Figure 1.** *System Users Settings on OPNsense*

1. 
Click the `Edit` button with a pen icon next to the`root` user.
2. 
Type a new root password in the **Password** field.

**Figure 2.** *Setting New root Password on OPNsense*

1. Click the **Save and go back** button at the end of the page.

## Enabling Two-Factor Authentication

Phishing attempts and using weak or repeated passwords can compromise traditional password-based authentication systems. A compromised password allows an attacker to obtain unauthorized access to a user account since many systems are accessible from anywhere over the Internet.

Two-factor authentication, often known as two-step verification, is intended to increase the security of user accounts. Logging into a 2FA-enabled account requires the user to provide an extra authentication factor in addition to a password.

Two-factor authentication, often known as 2FA or 2-Step Verification, is an authentication system that consists of two components: a PIN/password and a token. Two-factor authentication adds an additional layer of protection to an application or service. Additionally, it is typically simple to apply and uses minimal effort. OPNsense provides full support for two-factor authentication (2FA) throughout the system using Google Authenticator and you may easily enable 2FA on your OPNsense node. The subsequent OPNsense services support 2FA:

- 
Virtual Private Networking (OpenVPN & IPsec)
- 
Caching Proxy
- 
OPNsense Graphical User Interface
- 
Captive Portal

## Configure Country Blocking

There is no one-size-fits-all solution to country blocking, GeoIP blocking, as the countries that should be prohibited on a firewall depends on the organization's individual needs and objectives. Country blocking is used for the following reasons:

- 
To prevent users from accessing content that is illegal or prohibited in the country.
- 
To comply with government regulations or restrictions on specific types of content.
- 
To avoid potential legal liability for hosting or transmitting illegal or prohibited content.
- 
To protect against cyberattacks originating from specific countries.

Before determining which countries to restrict on their firewall, organizations should thoroughly evaluate their unique needs and objectives.

You may configure GeoIP blocking on your OPNsense firewall by following the 3 main steps:

1. 
Generate MaxMind GeoIP License Key
2. 
Define GeoIP Alias
3. 
Define Firewall Rule for Country Blocking

Each of these steps will be explained in more detail below.

### How to Generate MaxMind GeoIP License Key?

On OPNsense, you can block or allow one or more countries or entire continents. OPNsense accomplishes this by utilizing the MaxMind GeoIP database, which requires a license key. MaxMind, an industry leader in the accuracy of IP geolocation provides and maintains lists that are used by OPNsense. The MaxMind license key is completely free. The MaxMind License Key field description includes a link to the MaxMind registration page.

Websites host content and media on servers all over the world, so be cautious about blocking too much. Inadvertently blocking some of these IP addresses may result in broken websites or unavailable downloads.

To obtain your license key, fill out the registration form on the MaxMind sign-up page.

**Figure 3.** *MaxMind GeoLite2 Sign Up page*

After creating your account and verifying your email, you may proceed to generate your license key. The license key is necessary for OPNsense to automatically download the database.

Under your MaxMind account's settings, there should be a Manage License Keys option.

**Figure 4.** *MaxMind Managing license keys*

Here, you may produce a new key, describe it, and verify it.

**Figure 5.** *Generating MaxMind License Key*

### How to Define GeoIP Alias?

To download the GeoIP database from MaxMind you may follow these steps:

1. Navigate to the **Firewall** >**Aliases** >**GeoIP Settings** .

**Figure 6.** *GeoIP Alias Settings on OPNsense*

1. 
Type `https://download.maxmind.com/app/geoip_download?edition_id=GeoLite2-Country-CSV&license_key=YOUR_LICENSE_KEY&suffix=zip` where`YOUR_LICENSE_KEY` is your newly generated MaxMind License key in the URL field.
2. 
Click **Apply** button to start downloading the GeoIP database.**Last updated** and**Total number of ranges** fields should be updated.

**Figure 7.** *Downloading MaxMind GeoIP database*

1. 
Switch back to the **Aliases** tab on OPNsense Web UI.
2. 
Click **Add** button with`+` icon to create an alias.
3. 
Type a descriptive name, such as `NorthKorea` in the**Name** field.
4. 
Select **GeoIP** from the**Type** drop-down menu. You may select both`IPv4` and`IPv6` .
5. 
You may leave **Category** field empty or select a category if you have defined one before.

**Figure 8.** *Enabling GeoIP alias*

1. 
Select **Korea (North)** in the**Countries** drop-down menu next to the`Asia` Region.
2. 
You may click the **Statistics** checkbox to maintain a set of counters for each table entry.
3. 
Type a descriptive name, such as `Block North Korea` in the**Description** field.

**Figure 9.** *Adding alias for country blocking*

1. Click **Save** to save the alias.

**Figure 10.** *Applying GeoIP alias*

1. Click **Apply** to activate the settings.

### How to Define Firewall Rule for Country Blocking?

To define a firewall rule for country blocking you may follow the following steps given below:

1. 
Navigate to the **Firewalls** >**Rules** >*WAN* *.
2. 
Click Add button with `+` icon to add a rule.
3. 
Select **Block** in the**Action** option.
4. 
Select newly defined alias, such as `NorthKorean` in the**Source** field.
5. 
Type a descriptive name, like `GeoIP Blocking` in the**Description** field.
6. 
You may leave other options as default.
7. 
Click **Save**

**Figure 11.** *Applying GeoIP Firewall Rule*

1. Click **Apply Changes** to activate the rule.

## Disable SSH Connections

If you do not need remote access, disable any services, such as SSH, that are not necessary for your network to function. Disabling SSH connection is one of the first steps you can do to strengthen OPNsense. Disabling SSH increases system security overall by limiting the number of open ports and potential attack vectors. Some rules or business policies may necessitate the deactivation of unwanted or extra services, such as SSH.

By following the steps you can disable ssh access on OPNsense:

1. 
Navigate to the **System** >**Settings** >**Administration** .
2. 
Uncheck `Enable Secure Shell` option in the**Secure Shell** pane.

**Figure 12.** *Disable SSH Connections*

## Regularly Backup and Protect Backup Files

A software or hardware failure, a human-caused incident, or even natural disasters such as floods, fires, earthquakes, or tornadoes might result in a total system wipeout. And sometimes unfortunate events occur when we least anticipate them or are least prepared for them. The data backup makes it accessible in the event of data loss or corruption. You can only retrieve data from a previous time period if you have a backup.

To safeguard system data and ensure its accessibility, you must regularly back up your OPNsense. To harden your OPNsense security you need to protect your backup files.

### How to Backup/Restore Configuration Manually?

You may manually backup your OPNsense configuration by following the steps given below:

1. 
Navigate to the **System** >**Configuration** >**Backups** .
2. 
You may enable **Encrypt this configuration file** option. If you select this option, fill in the`Password` and`Confirmation` fields with a strong password.
3. 
Click **Download configuration** to download the configuration file.

**Figure 13.** *Backup OPNsense Configuration Manually*

You may manually restore your OPNsense configuration from a backup file by following the steps given below:

1. 
Navigate to the **System** >**Configuration** >**Backups** .
2. 
Select the **Restore Area** , such as`All` to store the full configuration.
3. 
Click **Choose File** to select the backup file.
4. 
You may select `Configuration file is encrypted.` and type the encryption password used during the backup operation.

**Figure 14.** *Restoring OPNsense Configuration Manually*

Full or incremental restores from backups are possible. Due to the interdependence of configuration parts, it is best to restore the whole configuration rather than just individual parts.

You may see the changes that have been made to your settings and even go back to a previous version by following the steps given below:

1. 
Navigate the **System** >**Configuration** >**History** menu.
2. 
Pick the older configuration using the left column of radio choices, then select the updated configuration using the right column.

**Figure 15.** *Viewing OPNsense Configuration Differences*

1. Click **View differences** button to see the changes between the two.

**Figure 16.** *Viewing OPNsense Configuration History*

Choosing the number of backups to preserve from this option is useful when a greater auditability standard is needed.

### How to Backup Configuration Automatically?

You can use a cronjob script to automate the backup process

1. Install the `os-api-backup` plugin.

**Figure 17.** *Installing os-api-backup plugin*

1. Go to **System** >**Access** >**Groups** in the OPNsense WebUI.

**Figure 18.** *Navigating System > Access > Groups*

1. 
Click **Add** button with`+` to create a new group with restricted rights.
2. 
Type a **Group name** , like backup.
3. 
Type a descriptive name in the **Description** field.

**Figure 19.** *Adding backup API Group*

1. 
Click **Save** .
2. 
Click **Edit** button with a pen icon for editing newly created group.
3. 
Click the **Edit** button with a pen icon on Assigned Privileges pane.

**Figure 20.** *Editing Assigned Priveleges of backup API Group*

1. Type `Backup API` in the search field and tick "Backup API" in the list. Leave the rest of the options unchecked.

**Figure 21.** *Saving Backup API System Privileges*

1. 
Click **Save** . This will save the privilege settings and take you back to the Group settings.
2. 
Clicking **Save** to activate the settings.
3. 
Go to **System** >**Access** >**Users** in the OPNsense WebUI to create a new user.
4. 
Click **Add** button with a`+` icon to add a user.
5. 
Set the **Username** , such as backup
6. 
Set a strong password.

**Figure 22.** *Adding Backup User*

1. Set group membership of the user by selecting the `backup` group.

**Figure 23.** *Setting group membership*

1. 
Click **Save & go back** button.
2. 
Edit the newly created backup user by clicking on the **Edit** button with a pen icon.
3. 
Scroll down to the **API keys** section.

**Figure 24.** *Adding Backup API Key*

1. 
Click **Add** button with a`+` icon to add a new key. This will create an api key and open an dialog box..
2. 
Click **Save** to download the tiny text file containing an API key and a secret, which you should keep somewhere safe.

**Figure 25.** *Downloading Backup API Key*

1. 
Click **Save and go back** .
2. 
Write the following script in a remote Linux or BSD-based machine using your preferred text editor:

```
#!/bin/bash
KEY="api_key"
SECRET="api_secret"
HOST="opnsense_hostname"
PATH="/path/to/backups"
curl -s -k -u $KEY:$SECRET https://$HOST/api/backup/backup/download \
-o $PATH/opnsense-config-$(date +%Y%m%d).xml
find $PATH/ -type f -name '*.xml' -mtime +30 -exec rm {} \;
```
Do not forget to update the variable values according to your settings.

This script will save backup files with the name "opnsense-config-yyyymmdd.xml" and delete anything older than 30 days.

You may try to use another advanced backup script given at `https://codeberg.org/SWEETGOOD/andersgood-opnsense-scripts/src/branch/main/backup-opnsense-via-api.sh` as well.

1. Set up a cron job to execute this on the schedule you like, and you're done.

## Configure OPNsense for High Availability

For hardware failover, OPNsense uses the Common Address Redundancy Protocol or CARP. A failover group is set up with two or more firewalls. If a main interface breaks or the primary goes offline totally, the secondary becomes operational. Using high-availability capability of OPNsense produces a completely redundant firewall with smooth failover. While switching to the backup network, customers experience minimal disruption to their network connections. You may easily configure high availability on your OPNsense.

## Disable root Access for the WebGUI

The default login after installing OPNsense is the root user. Because the root user has complete access to files and processes, it is typically not recommended to log in as `root`. Users of Linux, for example, are prompted to create a distinct user account after installation. The user can then use `sudo` to boost their privileges and carry out administrative activities. Theoretically, even if the user account is compromised, the root account is still safe (assuming there is no privilege escalation vulnerability being exploited or the password has been discovered). There is an option inside OpenSSH to prevent root access to the SSH server. It disables immediate log in as the root user as a security measure. OPNsense, which is based on FreeBSD (HardenedBSD, to be precise), does not deviate from this advice.

To disable the root access you may follow the next steps listed below:

1. 
Navigate to the **System** >**Settings** >**Administration** .
2. 
Uncheck `Permit User Login` option in the**Secure Shell** pane.

**Figure 26.** *Disabling root access*

## Restrict Access to Management Portal

Using a management network to gain administrative access to your network devices has numerous advantages:

- 
**Reliability:** When management traffic is separate from production or business traffic, management access can be preserved during production network reconfiguration because it won't have to compete for resources.
- 
**Simpler policies:** By separating the management from the production traffic, policies are more easily implemented. Policies with specific purposes are easier to understand and troubleshoot.
- 
**Security:** When network devices' management access is on a different network, accessing those devices on the production network is more challenging.

All administrative access should be restricted to a single interface or VLAN interface in the management network. All other interfaces should have administrative access disabled. To do this in OPNsense, you would utilize firewall rules, keeping in mind that the default is to deny all (implicit deny) incoming connections with the exception of the LAN interface.

## Enable IPS/IDS

Intrusion detection and prevention systems (IDPS) should be activated in OPNsense for hardening since they give an extra layer of protection to the firewall by detecting and blocking hostile or unauthorized network activity. IDPS systems identify threats and preventing them from reaching their intended targets using approaches such as signature-based detection, anomaly-based detection, and reputation-based detection. By activating IDPS in OPNsense, enterprises proactively defend their network against threats and reduce the likelihood of data breaches and other security issues. You can find the details about how to enable IPS on the OPNsense manual page: `https://docs.opnsense.org/manual/ips.html`

## Create Notifications for Changes

Change management notifications are essential for firewall hardening because they give a transparent record of any configuration changes. This helps guarantee that no illegal modifications are done and makes it easier to recognize and resolve any problems that may arise as a consequence of changes. Moreover, change management notifications can assist firms in meeting legal obligations and demonstrating due care in the case of a security problem. By giving a clear record of modifications, change management alerts make it simpler to debug and restore to a prior configuration if necessary, mitigating the impact of any potential issues.

For monitoring purposes, OPNsense uses the Monit framework. Given the breadth of Monit's monitoring capabilities, it comes as no surprise that the tool's setup choices are similarly flexible.

Monit is a program that checks your filesystems, drives, processes, and system, among other things. It operates on the OPNsense firewall host and delivers messages or performs actions in response to a variety of events.

First, you must choose what you want to monitor and what failure looks like. It is advantageous to understand how Monit alerts are configured.

### What is M/Monit?

M/Monit is an IT infrastructure monitoring and management solution that operates automatically and proactively. M/Monit is able to keep tabs on and manage a wide variety of computer systems, perform routine maintenance and repairs automatically, and take appropriate causal action in the event of an issue.

All of your hosts and services can be managed and monitored with M/Monit since it employs Monit as its agent. If a service isn't running, M/Monit can initiate its launch; if it has stopped responding, it can be restarted; and if it's using too many system resources, M/Monit can put a hold on it.

Keep an eye out for changes in your hosts' filesystems, directories, files, processes, and other system properties. If a value deviates from a predetermined threshold, an alert may be generated and a predetermined action is taken.

All of the data from the systems under surveillance is compiled and kept in a central repository. The gathered data may be explored using drill-down and filtering features. Charts and tables are automatically updated with the latest status and events from all monitored systems.

### What are the Benefits of M/Monit?

Monit provides the following advantages:

- 
**Easy-to-use:** In contrast to other solutions, M/Monit does not need extensive configuration or the installation of any additional third-party software.
- 
**Improved Uptime:** The uptime of your computer systems will improve since M/Monit can deal with problem scenarios automatically, in many cases without any human interaction.
- 
**Intuitive User Interface:** Monit's interface is intuitive and well-designed, making it a good choice whether you need to monitor two hosts or a thousand.
- 
**Open-Source:** The whole source code and build system are available for review. The M/Monit framework also includes freely available open-source software.
- 
**Cost-effective:** In exchange for a one-time fee, you get perpetual access to an M/Monit license. When compared to comparable commercial systems, the cost is negligible, and the time investment is little in comparison to that of an equivalent open-source solution.
- 
**Scalable Application:** The M/Monit application server is a state-of-the-art option that is both small and extensible. High performance is achieved with the use of thread-pools and an event-driven, non-blocking i/o architecture. M/Monit is compatible with all POSIX systems and requires just around 10 MB of memory. A connection pool that can communicate with MySQL, PostgreSQL, and SQLite manages access to the database.

### How to Enable and Configure Monit?

To enable and configure the Monit for OPNsense monitoring, you may follow the steps below:

1. 
Navigate to **Services** >**Monit** >**Settings** on OPNsense Web UI.
2. 
Enable Monit by clicking on the **Enable Monit** checkbox.
3. 
Set the port of the mail server in the **Mail Server Port** option. Typically you may set`465` for SSL or`25` for TLS and nonsecure connections.
4. 
Enter your email address into the **Mail Server Username** field for authentication.
5. 
Enter your email password into the **Mail Server Password** field for authentication.
6. 
You may enable encryption for mail server communication by clicking on the **Mail Server SSL Connection** option.

**Figure 27.** *Enabling Monit Service*

1. 
Click **Apply** to activate the settings.
2. 
To start the **Monit** service, click on the**Start** button at the upper right corner of the page.

**Figure 28.** *Starting Monit Service*

1. To view the status of the `Monit` service, navigate to the**Services** >**Monit** >**Status** . You should see the*status* field is`OK` .

You may view all general settings by clicking on the `advanced mode` option on the **General Settings** page.

| Setting | Description | 
|---|---|
| Enable Monit | Turns Monit on or off. | 
| Polling interval | How often Monit checks the health of the things it's watching. | 
| Start delay | Time in seconds before Monit begins verifying its components after being started. | 
| Mail Server | A list of mail servers to send notifications to. | 
| Mail Server Port | The mail server port to use. 25 and 465 are common examples. | 
| Username | The username used to log into your SMTP server, if needed. Often, but not always, the same as your e-mail address. | 
| Password | The password used to log into your SMTP server, if needed. | 
| Secure Connection | Use TLS when connecting to the mail server. | 
| SSL Version | The TLS version to use. AUTO will try to negotiate a working version. | 
| Verify SSL Certificates | Checks the TLS certificate for validity. If you use a self-signed certificate, turn this option off. | 
| Log File | The log file of the Monit process. This can be the keyword syslog or a path to a file. | 
| State File | The state file of the Monit process. | 
| Eventqueue Path | The path to the eventqueue directory. | 
| Eventqueue Slots | The number of eventqueue slots. | 
| Enable HTTPD | Turns on the Monit web interface. | 
| Monit HTTPD Port | The listen port of the Monit web interface service. | 
| Monit HTTPD Access List | The username:password or host/network etc. for accessing the Monit web interface service. | 
| M/Monit URL | The M/Monit URL, e.g. `https://user:[email protected]:8443/collector` | 
| M/Monit Timeout | Set the timeout for requests to M/Monit to this value of seconds. | 
| M/Monit Register Credentials | Registration in M/Monit is performed automatically by the transfer of Monit credentials. | 

More than one server may be entered into the "Mail Server" field. When sending an email, Monit will attempt to connect to each mail server in sequence, beginning with the first and moving on to the second if the first fails. No more attempts will be made to send the email by Monit if no server is found to be functional.

To aggregate information from several instances of Monit, M/Monit provides a paid service. Fill up the required data and add the necessary firewall rules in order to utilize it via OPNsense.

1. 
Type `From: [email protected]` in the "Mail format" box if your mail server needs the "From" field to be correctly configured.
2. 
Put the new settings into effect and **Save** the alert.

The Monit daemon is configured using the `os-monit` plugin. Install the `os-monit` plugin before using Monit. As a dependency, it installs the `monit` package.

Reload the GUI when the installation is complete and browse to Services->Monit->Settings.

The first step is to ensure that the plugin installer successfully imported your System->Notification settings. Then check out the other tabs. The installer has included some common entries to help you get started.

To establish a monitoring system, first construct Service Tests, then Services to check, and finally Alerts.

Begin with the Service Test Settings. A condition and an action are both part of a test. It is possible to assign it to one or more services. The Monit documentation includes examples of potential testing. Simply remove the IF and THEN statements to use it.

The following step is to set up service checks. Depending on the service type, we must specify a route, start/stop scripts, and assign already established tests. The same tests might be assigned to various service checks.

On the Alert Settings page, you can choose who receives notifications for particular events and who does not.

You can also format the email text. E.g. Subject: $SERVICE on $HOST failed on $DATE
