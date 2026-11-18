Internal Only - General 
CIS Cisco IOS XE 17.x 
Benchmark 
v2.2.1 - 07-17-2025

Page 1 
Internal Only - General 
Terms of Use 
Please see the below link for our current terms of use: 
https://www.cisecurity.org/cis-securesuite/cis-securesuite-membership-terms-of-use/ 
 
For information on referencing and/or citing CIS Benchmarks in 3rd party documentation 
(including using portions of Benchmark Recommendations) please contact CIS Legal 
(legalnotices@cisecurity.org) and request guidance on copyright usage.  
NOTE:  It is NEVER acceptable to host a CIS Benchmark in ANY format (PDF, etc.) 
on a 3rd party (non-CIS owned) site.

Page 2 
Internal Only - General 
Table of Contents 
Terms of Use ................................................................................................................. 1 
Table of Contents .......................................................................................................... 2 
Overview ........................................................................................................................ 6 
Important Usage Information ................................ ................................ ................................  6 
Key Stakeholders ................................................................................................................................... 6 
Apply the Correct Version of a Benchmark ........................................................................................ 7 
Exceptions .............................................................................................................................................. 7 
Remediation ........................................................................................................................................... 8 
Summary ................................................................................................................................................. 8 
Target Technology Details ................................ ................................ ................................ .... 9 
Intended Audience ................................ ................................ ................................ ................. 9 
Consensus Guidance ................................ ................................ ................................ ...........10 
Typographical Conventions ................................ ................................ ................................ .11 
Recommendation Definitions ..................................................................................... 12 
Title ................................ ................................ ................................ ................................ ........12 
Assessment Status ................................ ................................ ................................ ...............12 
Automated ............................................................................................................................................ 12 
Manual ................................................................................................................................................... 12 
Profile ................................ ................................ ................................ ................................ ....12 
Description ................................ ................................ ................................ ............................12 
Rationale Statement ................................ ................................ ................................ .............12 
Impact Statement ................................ ................................ ................................ ..................13 
Audit Procedure ................................ ................................ ................................ ....................13 
Remediation Procedure ................................ ................................ ................................ ........13 
Default Value ................................ ................................ ................................ .........................13 
References ................................ ................................ ................................ ............................13 
CIS Critical Security Controls® (CIS Controls®) ................................ ................................ ..13 
Additional Information................................ ................................ ................................ ..........13 
Profile Definitions ................................ ................................ ................................ .................14 
Acknowledgements ................................ ................................ ................................ ..............15 
Recommendations ...................................................................................................... 16 
1 Management Plane ................................ ................................ ................................ ............16 
1.1 Local Authentication, Authorization and Accounting (AAA) Rules ......................................... 17 
1.1.1 Enable 'aaa new-model' (Automated) .............................................................................. 18 
1.1.2 Enable 'aaa authentication login' (Automated) ................................................................ 20 
1.1.3 Enable 'aaa authentication enable default' (Automated) ................................................. 22

Page 3 
Internal Only - General 
1.1.4 Set 'login authentication for 'line vty' (Automated) ........................................................... 24 
1.1.5 Set 'login authentication for 'ip http' (Automated) ............................................................ 26 
1.1.6 Set 'aaa accounting' to log all privileged use commands using 'commands 15' 
(Automated) .............................................................................................................................. 28 
1.1.7 Set 'aaa accounting connection' (Automated) ................................................................. 30 
1.1.8 Set 'aaa accounting exec' (Automated) ........................................................................... 32 
1.1.9 Set 'aaa accounting network' (Automated) ...................................................................... 34 
1.1.10 Set 'aaa accounting system' (Automated) ..................................................................... 36 
1.2 Access Rules .................................................................................................................................. 38 
1.2.1 Set 'privilege 1' for local users (Manual) .......................................................................... 39 
1.2.2 Set 'transport input ssh' for 'line vty' connections (Automated) ....................................... 41 
1.2.3 Set 'no exec' for 'line aux 0' (Automated) ........................................................................ 43 
1.2.4 Create 'access-list' for use with 'line vty' (Manual) .......................................................... 45 
1.2.5 Set 'access-class' for 'line vty' (Automated) ..................................................................... 47 
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 'line aux 0' (Automated) ........ 49 
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 'line console 0' (Automated)  ....... 51 
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 'line vty' (Automated).................. 53 
1.2.9 Set 'http Secure-server' limit (Automated) ....................................................................... 55 
1.2.10 Set 'exec-timeout' to less than or equal to 10 min on 'ip http' (Automated) ................... 57 
1.3 Banner Rules .................................................................................................................................. 59 
1.3.1 Set the 'banner-text' for 'banner exec' (Automated) ........................................................ 60 
1.3.2 Set the 'banner-text' for 'banner login' (Automated) ........................................................ 62 
1.3.3 Set the 'banner-text' for 'banner motd' (Automated) ........................................................ 64 
1.3.4 Set the 'banner-text' for 'webauth banner' (Automated) .................................................. 66 
1.4 Password Rules ............................................................................................................................. 68 
1.4.1 Set 'password' for 'enable secret' (Automated) ............................................................... 69 
1.4.2 Enable 'service password-encryption' (Automated) ......................................................... 71 
1.4.3 Set 'username secret' for all local users (Automated) ..................................................... 73 
1.5 SNMP Rules .................................................................................................................................... 75 
1.5.1 Set 'no snmp-server' to disable SNMP when unused (Manual) ...................................... 76 
1.5.2 Unset 'private' for 'snmp-server community' (Automated) ............................................... 78 
1.5.3 Unset 'public' for 'snmp-server community' (Automated) ................................................. 80 
1.5.4 Do not set 'RW' for any 'snmp-server community' (Manual) ............................................ 82 
1.5.5 Set the ACL for each 'snmp-server community' (Manual) ............................................... 84 
1.5.6 Create an 'access-list' for use with SNMP (Manual) ........................................................ 86 
1.5.7 Set 'snmp-server host' when using SNMP (Automated) .................................................. 88 
1.5.8 Set 'snmp-server enable traps snmp' (Automated) ......................................................... 90 
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3 (Automated) ............................... 92 
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' when using SNMPv3 (Manual) . 94 
2 Control Plane ................................ ................................ ................................ .....................95 
2.1 Global Service Rules ..................................................................................................................... 95 
2.1.1 Setup SSH ................................................................................................................................. 95 
2.1.1.1 Configure Prerequisites for the SSH Service .................................................................. 96 
2.1.1.1.1 Set the 'hostname' (Automated) ................................................................................ 97 
2.1.1.1.2 Set the 'ip domain-name' (Automated) ...................................................................... 99 
2.1.1.1.3 Set 'modulus' to greater than or equal to 2048 for 'crypto key generate rsa' (Manual)
 ................................................................................................................................................ 101 
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less (Automated)  .................... 103 
2.1.1.1.5 Set maximum value for 'ip ssh authentication-retries' (Automated) ........................ 105 
2.1.1.2 Set version 2 for 'ip ssh version' (Manual) .................................................................. 107 
2.1.2 Set 'no cdp run' (Manual) ............................................................................................... 109 
2.1.3 Set 'no ip bootp server' (Manual) ................................................................................... 111 
2.1.4 Set 'no service dhcp' (Automated) ................................................................................. 113 
2.1.5 Set 'service tcp-keepalives-in' (Automated) ................................................................... 115 
2.1.6 Set 'service tcp-keepalives-out' (Automated) ................................................................ 117

Page 4 
Internal Only - General 
2.1.7 Set 'no service pad' (Manual) ........................................................................................ 119 
2.2 Logging Rules .............................................................................................................................. 121 
2.2.1 Set 'logging enable' (Automated) ................................................................................... 122 
2.2.2 Set 'buffer size' for 'logging buffered' (Automated) ........................................................ 124 
2.2.3 Set 'logging console critical' (Automated) ...................................................................... 126 
2.2.4 Set IP address for 'logging host' (Automated) ............................................................... 128 
2.2.5 Set 'logging trap informational' (Automated) .................................................................. 130 
2.2.6 Set 'service timestamps debug datetime' (Automated) ................................................. 132 
2.2.7 Set 'logging source interface' (Automated) .................................................................... 134 
2.2.8 Set 'login success/failure logging' (Automated) ............................................................. 136 
2.3 NTP Rules ..................................................................................................................................... 137 
2.3.1 Require Encryption Keys for NTP ........................................................................................ 138 
2.3.1.1 Set 'ntp authenticate' (Automated).............................................................................. 139 
2.3.1.2 Set 'ntp authentication-key' (Automated) .................................................................... 141 
2.3.1.3 Set the 'ntp trusted-key' (Automated) ......................................................................... 143 
2.3.1.4 Set 'key' for each 'ntp server' (Manual) ....................................................................... 145 
2.3.2 Set 'ip address' for 'ntp server' (Automated) .................................................................. 147 
2.4 Loopback Rules ........................................................................................................................... 149 
2.4.1 Create a single 'interface loopback' (Automated) .......................................................... 150 
2.4.2 Set AAA 'source-interface' (Manual) .............................................................................. 152 
2.4.3 Set 'ntp source' to Loopback Interface (Automated) ...................................................... 154 
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface (Manual) .................................. 156 
3 Data Plane ................................ ................................ ................................ ........................ 157 
3.1 Routing Rules ............................................................................................................................... 158 
3.1.1 Set 'no ip source-route' (Manual) ................................................................................... 159 
3.1.2 Set 'no ip proxy-arp' (Manual) ........................................................................................ 161 
3.1.3 Set 'no interface tunnel' (Manual) .................................................................................. 163 
3.1.4 Set 'ip verify unicast source reachable-via' (Manual) .................................................... 165 
3.2 Border Router Filtering ............................................................................................................... 167 
3.2.1 Set 'ip access-list extended' to Forbid Private Source Addresses from External Networks 
(Manual) .................................................................................................................................. 168 
3.2.2 Set inbound 'ip access-group' on the External Interface (Manual) ................................ 171 
3.3 Neighbor Authentication ............................................................................................................. 172 
3.3.1 Require EIGRP Authentication if Protocol is Used ............................................................. 173 
3.3.1.1 Set 'key chain' (Manual) .............................................................................................. 174 
3.3.1.2 Set 'key' (Manual) ....................................................................................................... 176 
3.3.1.3 Set 'key-string' (Manual) ............................................................................................. 178 
3.3.1.4 Set 'address-family ipv4 autonomous-system' (Manual) ............................................ 180 
3.3.1.5 Set 'af-interface default' (Manual) ............................................................................... 182 
3.3.1.6 Set 'authentication key-chain' (Manual) ...................................................................... 184 
3.3.1.7 Set 'authentication mode md5' (Manual) .................................................................... 186 
3.3.1.8 Set 'ip authentication key-chain eigrp' (Manual) ......................................................... 188 
3.3.1.9 Set 'ip authentication mode eigrp' (Manual) ............................................................... 190 
3.3.2 Require OSPF Authentication if Protocol is Used .............................................................. 192 
3.3.2.1 Set 'authentication message-digest' for OSPF area (Manual) ................................... 193 
3.3.2.2 Set 'ip ospf message-digest-key md5' (Manual) ......................................................... 195 
3.3.3 Require BGP Authentication if Protocol is Used ................................................................ 197 
3.3.3.1 Set 'neighbor password' (Manual) .............................................................................. 198 
Appendix: Summary Table ....................................................................................... 200 
Appendix: CIS Controls v7 IG 1 Mapped Recommendations ................................ 206 
Appendix: CIS Controls v7 IG 2 Mapped Recommendations ................................ 207 
Appendix: CIS Controls v7 IG 3 Mapped Recommendations ................................ 210

Page 5 
Internal Only - General 
Appendix: CIS Controls v7 Unmapped Recommendations ................................... 213 
Appendix: CIS Controls v8 IG 1 Mapped Recommendations ................................ 214 
Appendix: CIS Controls v8 IG 2 Mapped Recommendations ................................ 216 
Appendix: CIS Controls v8 IG 3 Mapped Recommendations ................................ 219 
Appendix: CIS Controls v8 Unmapped Recommendations ................................... 222 
Appendix: Change History ....................................................................................... 223

Page 6 
Internal Only - General 
Overview 
All CIS Benchmarks™ (Benchmarks) focus on technical configuration settings used to 
maintain and/or increase the security of the addressed technology, and they should be 
used in conjunction with other essential cyber hygiene tasks like: 
• Monitoring the base operating system and applications for vulnerabilities and 
quickly updating with the latest security patches. 
• End-point protection (Antivirus software, Endpoint Detection and Response 
(EDR), etc.). 
• Logging and monitoring user and system activity. 
In the end, the Benchmarks are designed to be a key component of a comprehensive 
cybersecurity program.  
Important Usage Information 
All Benchmarks are available free for non-commercial use from the CIS Website. They 
can be used to manually assess and remediate systems and applications. In lieu of 
manual assessment and remediation, there are several tools available to assist with 
assessment: 
• CIS Configuration Assessment Tool (CIS-CAT® Pro Assessor) 
• CIS Benchmarks™ Certified 3rd Party Tooling 
These tools make the hardening process much more scalable for large numbers of 
systems and applications.  
NOTE:  Some tooling focuses only on the Benchmark Recommendations that can 
be fully automated (skipping ones marked Manual). It is important that ALL 
Recommendations (Automated and Manual) be addressed since all are 
important for properly securing systems and are typically in scope for 
audits.  
Key Stakeholders 
Cybersecurity is a collaborative effort, and cross functional cooperation is imperative 
within an organization to discuss, test, and deploy Benchmarks in an effective and 
efficient way. The Benchmarks are developed to be best practice configuration 
guidelines applicable to a wide range of use cases. In some organizations, exceptions 
to specific Recommendations will be needed, and this team should work to prioritize the 
problematic Recommendations based on several factors like risk, time, cost, and labor. 
These exceptions should be properly categorized and documented for auditing 
purposes.

Page 7 
Internal Only - General 
 
Apply the Correct Version of a Benchmark 
Benchmarks are developed and tested for a specific set of products and versions and 
applying an incorrect Benchmark to a system can cause the resulting pass/fail score to 
be incorrect. This is due to the assessment of settings that do not apply to the target 
systems. To assure the correct Benchmark is being assessed:  
• Deploy the Benchmark applicable to the way settings are managed in the 
environment: An example of this is the Microsoft Windows family of 
Benchmarks, which have separate Benchmarks for Group Policy, Intune, and 
Stand-alone systems based upon how system management is deployed. 
Applying the wrong Benchmark in this case will give invalid results. 
• Use the most recent version of a Benchmark: This is true for all Benchmarks, 
but especially true for cloud technologies. Cloud technologies change frequently 
and using an older version of a Benchmark may have invalid methods for 
auditing and remediation. 
Exceptions 
The guidance items in the Benchmarks are called recommendations and not 
requirements, and exceptions to some of them are expected and acceptable. The 
Benchmarks strive to be a secure baseline, or starting point, for a specific technology, 
with known issues identified during Benchmark development are documented in the 
Impact section of each Recommendation. In addition, organizational, system specific 
requirements, or local site policy may require changes as well, or an exception to a 
Recommendation or group of Recommendations (e.g. A Benchmark could Recommend 
that a Web server not be installed on the system, but if a system's primary purpose is to 
function as a Webserver, there should be a documented exception to this 
Recommendation for that specific server). 
In the end, exceptions to some Benchmark Recommendations are common and 
acceptable, and should be handled as follows: 
• The reasons for the exception should be reviewed cross-functionally and be well 
documented for audit purposes. 
• A plan should be developed for mitigating, or eliminating, the exception in the 
future, if applicable. 
• If the organization decides to accept the risk of this exception (not work toward 
mitigation or elimination), this should be documented for audit purposes. 
It is the responsibility of the organization to determine their overall security policy, and 
which settings are applicable to their unique needs based on the overall risk profile for 
the organization.

Page 8 
Internal Only - General 
Remediation 
CIS has developed Build Kits for many technologies to assist in the automation of 
hardening systems. Build Kits are designed to correspond to Benchmark's 
“Remediation” section, which provides the manual remediation steps necessary to make 
that Recommendation compliant to the Benchmark. 
When remediating systems (changing configuration settings on 
deployed systems as per the Benchmark's Recommendations), 
please approach this with caution and test thoroughly. 
The following is a reasonable remediation approach to follow: 
• CIS Build Kits, or internally developed remediation methods should never be 
applied to production systems without proper testing. 
• Proper testing consists of the following: 
o Understand the configuration (including installed applications) of the targeted 
systems. Various parts of the organization may need different configurations 
(e.g., software developers vs standard office workers). 
o Read the Impact section of the given Recommendation to help determine if 
there might be an issue with the targeted systems. 
o Test the configuration changes with representative lab system(s). If issues 
arise during testing, they can be resolved prior to deploying to any production 
systems. 
o When testing is complete, initially deploy to a small sub-set of production 
systems and monitor closely for issues. If there are issues, they can be 
resolved prior to deploying more broadly. 
o When the initial deployment above is completes successfully, iteratively 
deploy to additional systems and monitor closely for issues. Repeat this 
process until the full deployment is complete.  
Summary 
Using the Benchmarks Certified tools, working as a team with key stakeholders, being 
selective with exceptions, and being careful with remediation deployment, it is possible 
to harden large numbers of deployed systems in a cost effective, efficient, and safe 
manner. 
NOTE: As previously stated, the PDF versions of the CIS Benchmarks™ are 
available for free, non-commercial use on the CIS Website. All other formats 
of the CIS Benchmarks™ (MS Word, Excel, and Build Kits) are available for 
CIS SecureSuite® members. 
CIS-CAT® Pro is also available to CIS SecureSuite® members.

Page 9 
Internal Only - General 
Target Technology Details 
This document, Security Configuration Benchmark for Cisco IOS, provides prescriptive 
guidance for establishing a secure configuration posture for Cisco Router running Cisco 
IOS version 17.06. This guide was tested against Cisco IOS 17 XE. To obtain the latest 
version of this guide, please visit http://benchmarks.cisecurity.org. If you have 
questions, comments, or have identified ways to improve this guide, please write us at 
benchmarkinfo@cisecurity.org. 
 
Intended Audience 
This benchmark is intended for system and application administrators, security 
specialists, auditors, help desk, and platform deployment personnel who plan to 
develop, deploy, assess, or secure solutions that incorporate Cisco IOS on a Cisco 
routing and switching platforms.

Page 10 
Internal Only - General 
Consensus Guidance 
This CIS Benchmark™ was created using a consensus review process comprised of a 
global community of subject matter experts. The process combines real world 
experience with data-based information to create technology specific guidance to assist 
users to secure their environments. Consensus participants provide perspective from a 
diverse set of backgrounds including consulting, software development, audit and 
compliance, security research, operations, government, and legal.  
Each CIS Benchmark undergoes two phases of consensus review. The first phase 
occurs during initial Benchmark development. During this phase, subject matter experts 
convene to discuss, create, and test working drafts of the Benchmark. This discussion 
occurs until consensus has been reached on Benchmark recommendations. The 
second phase begins after the Benchmark has been published. During this phase, all 
feedback provided by the Internet community is reviewed by the consensus team for 
incorporation in the Benchmark. If you are interested in participating in the consensus 
process, please visit https://workbench.cisecurity.org/.

Page 11 
Internal Only - General 
Typographical Conventions 
The following typographical conventions are used throughout this guide: 
Convention Meaning 
Stylized Monospace font 
Used for blocks of code, command, and 
script examples. Text should be interpreted 
exactly as presented. 
Monospace font 
Used for inline code, commands, UI/Menu 
selections or examples. Text should be 
interpreted exactly as presented. 
<Monospace font in brackets> Text set in angle brackets denote a variable 
requiring substitution for a real value. 
Italic font 
Used to reference other relevant settings, 
CIS Benchmarks and/or Benchmark 
Communities. Also, used to denote the title 
of a book, article, or other publication. 
Bold font 
Additional information or caveats things like 
Notes, Warnings, or Cautions (usually just 
the word itself and the rest of the text 
normal).

Page 12 
Internal Only - General 
Recommendation Definitions 
The following defines the various components included in a CIS recommendation as 
applicable.  If any of the components are not applicable it will be noted, or the 
component will not be included in the recommendation.    
Title 
Concise description for the recommendation's intended configuration.  
Assessment Status 
An assessment status is included for every recommendation. The assessment status 
indicates whether the given recommendation can be automated or requires manual 
steps to implement. Both statuses are equally important and are determined and 
supported as defined below:  
Automated 
Represents recommendations for which assessment of a technical control can be fully 
automated and validated to a pass/fail state. Recommendations will include the 
necessary information to implement automation. 
Manual 
Represents recommendations for which assessment of a technical control cannot be 
fully automated and requires all or some manual steps to validate that the configured 
state is set as expected. The expected state can vary depending on the environment. 
Profile 
A collection of recommendations for securing a technology or a supporting platform. 
Most benchmarks include at least a Level 1 and Level 2 Profile. Level 2 extends Level 1 
recommendations and is not a standalone profile. The Profile Definitions section in the 
benchmark provides the definitions as they pertain to the recommendations included for 
the technology.  
Description 
Detailed information pertaining to the setting with which the recommendation is 
concerned. In some cases, the description will include the recommended value. 
Rationale Statement 
Detailed reasoning for the recommendation to provide the user a clear and concise 
understanding on the importance of the recommendation.

Page 13 
Internal Only - General 
Impact Statement  
Any security, functionality, or operational consequences that can result from following 
the recommendation. 
Audit Procedure  
Systematic instructions for determining if the target system complies with the 
recommendation. 
Remediation Procedure 
Systematic instructions for applying recommendations to the target system to bring it 
into compliance according to the recommendation. 
Default Value 
Default value for the given setting in this recommendation, if known. If not known, either 
not configured or not defined will be applied.  
References 
Additional documentation relative to the recommendation.  
CIS Critical Security Controls® (CIS Controls®) 
The mapping between a recommendation and the CIS Controls is organized by CIS 
Controls version, Safeguard, and Implementation Group (IG). The Benchmark in its 
entirety addresses the CIS Controls safeguards of (v7) “5.1 - Establish Secure 
Configurations” and (v8) '4.1 - Establish and Maintain a Secure Configuration Process” 
so individual recommendations will not be mapped to these safeguards. 
Additional Information  
Supplementary information that does not correspond to any other field but may be 
useful to the user.

Page 14 
Internal Only - General 
Profile Definitions  
The following configuration profiles are defined by this Benchmark: 
• Level 1 
Items in this profile intend to: 
o be practical and prudent; 
o provide a clear security benefit; and 
o not inhibit the utility of the technology beyond acceptable means. 
• Level 2 
This profile extends the "Level 1" profile. Items in this profile exhibit one or more 
of the following characteristics: 
o are intended for environments or use cases where security is paramount. 
o acts as defense in depth measure. 
o may negatively inhibit the utility or performance of the technology.

Page 15 
Internal Only - General 
 
 
Acknowledgements 
This Benchmark exemplifies the great things a community of users, vendors, and 
subject matter experts can accomplish through consensus collaboration. The CIS 
community thanks the entire consensus team with special recognition to the following 
individuals who contributed greatly to the creation of this guide: 
 
Contributor 
Raphael Precigout 
Eric Pinnell  
Gavin Day  
Vikas Gothwal CISSP 
David Poirier  
Curtis Starnes  
Josh Eblin  
Darren Stevenson  
 
Editor 
Darren Stevenson

Page 16 
Internal Only - General 
Recommendations 
1 Management Plane 
Services, settings and data streams related to setting up and examining the static 
configuration of the firewall, and the authentication and authorization of firewall 
administrators. Examples of management plane services include: administrative device 
access (telnet, ssh, http, and https), SNMP, and security protocols like RADIUS and 
TACACS+.

Page 17 
Internal Only - General 
1.1 Local Authentication, Authorization and Accounting (AAA) Rules 
Rules in the Local authentication, authorization and accounting (AAA) configuration 
class enforce device access control, provide a mechanism for tracking configuration 
changes, and enforcing security policy.

Page 18 
Internal Only - General 
1.1.1 Enable 'aaa new-model' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
This command enables the AAA access control system. 
Rationale: 
Authentication, authorization and accounting (AAA) services provide an authoritative 
source for managing and monitoring access for devices. Centralizing control improves 
consistency of access control, the services that may be accessed once authenticated 
and accountability by tracking services accessed. Additionally, centralizing access 
control simplifies and reduces administrative costs of account provisioning and de-
provisioning, especially when managing a large number of devices. 
Impact: 
Implementing Cisco AAA is significantly disruptive as former access methods are 
immediately disabled. Therefore, before implementing Cisco AAA, the organization 
should carefully review and plan their authentication criteria (logins & passwords, 
challenges & responses, and token technologies), authorization methods, and 
accounting requirements. 
Audit: 
Perform the following to determine if AAA services are enabled: 
 hostname#show running-config | inc aaa new-model 
If the result includes a "no", the feature is not enabled. 
Remediation: 
Globally enable authentication, authorization and accounting (AAA) using the new-
model command. 
hostname(config)#aaa new-model 
Default Value: 
AAA is not enabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a2.html#GUID-
E05C2E00-C01E-4053-9D12-EC37C7E8EEC5

Page 19 
Internal Only - General 
Additional Information: 
this is a cosmetic change: adjusting regex to match 'aaa new-model' and nothing from 
line which follows next. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 5.6 Centralize Account Management 
 Centralize account management through a directory or identity service.  ● ● 
v7 
16.2 Configure Centralized Point of Authentication 
 Configure access for all accounts through as few centralized points of 
authentication as possible, including network, security, and cloud systems. 
 ● ●

Page 20 
Internal Only - General 
1.1.2 Enable 'aaa authentication login' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Sets authentication, authorization and accounting (AAA) authentication at login. 
Rationale: 
Using AAA authentication for interactive management access to the device provides 
consistent, centralized control of your network. The default under AAA (local or network) 
is to require users to log in using a valid user name and password. This rule applies for 
both local and network AAA. Fallback mode should also be enabled to allow emergency 
access to the router or switch in the event that the AAA server was unreachable, by 
utilizing the LOCAL keyword after the AAA server-tag. 
Impact: 
Implementing Cisco AAA is significantly disruptive as former access methods are 
immediately disabled. Therefore, before implementing Cisco AAA, the organization 
should carefully review and plan their authentication methods such as logins and 
passwords, challenges and responses, and which token technologies will be used. 
Audit: 
Perform the following to determine if AAA authentication for login is enabled: 
hostname#show running-config | incl aaa authentication login 
If a result does not return, the feature is not enabled. 
Remediation: 
Configure AAA authentication method(s) for login authentication. 
hostname(config)#aaa authentication login {default | aaa_list_name} [passwd-
expiry] 
[method1] [method2] 
  
Default Value: 
AAA authentication at login is disabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a1.html#GUID-
3DB1CC8A-4A98-400B-A906-C42F265C7EA2

Page 21 
Internal Only - General 
Additional Information: 
Only “the default method list is automatically applied to all interfaces except those that 
have a named method list explicitly defined. A defined method list overrides the default 
method list.” (1) 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 5.6 Centralize Account Management 
 Centralize account management through a directory or identity service.  ● ● 
v7 
16.2 Configure Centralized Point of Authentication 
 Configure access for all accounts through as few centralized points of 
authentication as possible, including network, security, and cloud systems. 
 ● ●

Page 22 
Internal Only - General 
1.1.3 Enable 'aaa authentication enable default' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Authenticates users who access privileged EXEC mode when they use the enable 
command. 
Rationale: 
Using AAA authentication for interactive management access to the device provides 
consistent, centralized control of your network. The default under AAA (local or network) 
is to require users to log in using a valid user name and password. This rule applies for 
both local and network AAA. 
Impact: 
Enabling Cisco AAA 'authentication enable' mode is significantly disruptive as former 
access methods are immediately disabled. Therefore, before enabling 'aaa 
authentication enable default' mode, the organization should plan and implement 
authentication logins and passwords, challenges and responses, and token 
technologies. 
Audit: 
Perform the following to determine if AAA authentication enable mode is enabled: 
hostname#show running-config | incl aaa authentication enable 
If a result does not return, the feature is not enabled 
Remediation: 
Configure AAA authentication method(s) for enable authentication. 
hostname(config)#aaa authentication enable default {method1} enable  
Default Value: 
By default, fallback to the local database is disabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a1.html#GUID-
4171D649-2973-4707-95F3-9D96971893D0

Page 23 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 5.6 Centralize Account Management 
 Centralize account management through a directory or identity service.  ● ● 
v7 
16.2 Configure Centralized Point of Authentication 
 Configure access for all accounts through as few centralized points of 
authentication as possible, including network, security, and cloud systems. 
 ● ●

Page 24 
Internal Only - General 
1.1.4 Set 'login authentication for 'line vty' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Authenticates users who access the router or switch remotely through the VTY port. 
Rationale: 
Using AAA authentication for interactive management access to the device provides 
consistent, centralized control of your network. The default under AAA (local or network) 
is to require users to log in using a valid user name and password. This rule applies for 
both local and network AAA. 
Impact: 
Enabling Cisco AAA 'login authentication for line VTY' is significantly disruptive as 
former access methods are immediately disabled. Therefore, before enabling Cisco 
AAA 'login authentication for line VTY', the organization should plan and implement 
authentication logins and passwords, challenges and responses, and token 
technologies. 
Audit: 
Perform the following to determine if AAA authentication for line login is enabled: 
If the command does not return a result for each management access method, the 
feature is not enabled 
hostname#show running-config | sec line | incl login authentication 
Remediation: 
Configure management lines to require login using the default or a named AAA 
authentication list. This configuration must be set individually for all line types. 
hostname(config)#line vty {line-number} [<em>ending-line-number] 
hostname(config-line)#login authentication {default | aaa_list_name} 
Default Value: 
Login authentication is not enabled. 
Uses the default set with aaa authentication login. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-k1.html#GUID-
297BDF33-4841-441C-83F3-4DA51C3C7284

Page 25 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 5.6 Centralize Account Management 
 Centralize account management through a directory or identity service.  ● ● 
v7 
16.2 Configure Centralized Point of Authentication 
 Configure access for all accounts through as few centralized points of 
authentication as possible, including network, security, and cloud systems. 
 ● ●

Page 26 
Internal Only - General 
1.1.5 Set 'login authentication for 'ip http' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
If account management functions are not automatically enforced, an attacker could gain 
privileged access to a vital element of the network security architecture 
Rationale: 
Configure the device to enforce authentication for the HTTP server (ip http) by defining 
a valid login method. This ensures that only authorized users can access the device's 
web-based management interface. Use AAA (Authentication, Authorization, and 
Accounting) or a predefined local username and password database to enforce login 
authentication 
Impact: 
Enabling Cisco AAA 'line login' is significantly disruptive as former access methods are 
immediately disabled. Therefore, before enabling Cisco AAA 'line login', the 
organization should plan and implement authentication logins and passwords, 
challenges and responses, and token technologies. 
Audit: 
Perform the following to determine if AAA authentication for line login is enabled: 
If the command does not return a result for each management access method, the 
feature is not enabled 
hostname#show running-config | inc ip http authentication 
Remediation: 
Configure management lines to require login using the default or a named AAA 
authentication list. This configuration must be set individually for all line types. 
hostname#(config)ip http secure-server 
hostname#(config)ip http authentication {default | _aaa\_list\_name_} 
Default Value: 
Login authentication is not enabled. 
Uses the default set with aaa authentication login.

Page 27 
Internal Only - General 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-k1.html#GUID-
297BDF33-4841-441C-83F3-4DA51C3C7284 
Additional Information: 
This control is usefull if http or https server is enabled, which involves global config 
contains a line starting with "ip http server" or "ip http secure-server". Then, look for ('no 
ip http server' and 'no ip http secure-server') or 'ip http authentication ' config lines 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 5.6 Centralize Account Management 
 Centralize account management through a directory or identity service.  ● ● 
v7 
16.2 Configure Centralized Point of Authentication 
 Configure access for all accounts through as few centralized points of 
authentication as possible, including network, security, and cloud systems. 
 ● ●

Page 28 
Internal Only - General 
1.1.6 Set 'aaa accounting' to log all privileged use commands 
using 'commands 15' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Runs accounting for all commands at the specified privilege level. 
Rationale: 
Authentication, authorization and accounting (AAA) systems provide an authoritative 
source for managing and monitoring access for devices. Centralizing control improves 
consistency of access control, the services that may be accessed once authenticated 
and accountability by tracking services accessed. Additionally, centralizing access 
control simplifies and reduces administrative costs of account provisioning and de-
provisioning, especially when managing a large number of devices. AAA Accounting 
provides a management and audit trail for user and administrative sessions through 
TACACS+. 
Impact: 
Enabling 'aaa accounting' for privileged commands records and sends activity to the 
accounting servers and enables organizations to monitor and analyze privileged activity. 
Audit: 
Perform the following to determine if aaa accounting for commands is required: 
Verify a command string result returns 
hostname#show running-config | incl aaa accounting commands 
Remediation: 
Configure AAA accounting for commands. 
hostname(config)#aaa accounting commands 15 {default | list-name | guarantee-
first} 
{start-stop | stop-only | none} {radius | group group-name} 
Default Value: 
AAA accounting is disabled. 
Additional Information: 
Valid privilege level entries are integers from 0 through 15.

Page 29 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.2 Collect Audit Logs 
 Collect audit logs. Ensure that logging, per the enterprise’s audit log 
management process, has been enabled across enterprise assets. 
● ● ● 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 30 
Internal Only - General 
1.1.7 Set 'aaa accounting connection' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Provides information about all outbound connections made from the network access 
server. 
Rationale: 
Authentication, authorization and accounting (AAA) systems provide an authoritative 
source for managing and monitoring access for devices. Centralizing control improves 
consistency of access control, the services that may be accessed once authenticated 
and accountability by tracking services accessed. Additionally, centralizing access 
control simplifies and reduces administrative costs of account provisioning and de-
provisioning, especially when managing a large number of devices. AAA Accounting 
provides a management and audit trail for user and administrative sessions through 
RADIUS and TACACS+. 
Impact: 
Implementing aaa accounting connection creates accounting records about connections 
from the network access server. Organizations should regular monitor these connection 
records for exceptions, remediate issues, and report findings regularly. 
Audit: 
Perform the following to determine if aaa accounting for connection is required: 
Verify a command string result returns 
hostname#show running-config | incl aaa accounting connection 
Remediation: 
Configure AAA accounting for connections. 
hostname(config)#aaa accounting connection {default | list-name | guarantee-
first}  
{start-stop | stop-only | none} {radius | group group-name} 
Default Value: 
AAA accounting is not enabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a1.html#GUID-
0520BCEF-89FB-4505-A5DF-D7F1389F1BBA

Page 31 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 5.6 Centralize Account Management 
 Centralize account management through a directory or identity service.  ● ● 
v7 
16.2 Configure Centralized Point of Authentication 
 Configure access for all accounts through as few centralized points of 
authentication as possible, including network, security, and cloud systems. 
 ● ●

Page 32 
Internal Only - General 
1.1.8 Set 'aaa accounting exec' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Runs accounting for the EXEC shell session. 
Rationale: 
Authentication, authorization and accounting (AAA) systems provide an authoritative 
source for managing and monitoring access for devices. Centralizing control improves 
consistency of access control, the services that may be accessed once authenticated 
and accountability by tracking services accessed. Additionally, centralizing access 
control simplifies and reduces administrative costs of account provisioning and de-
provisioning, especially when managing a large number of devices. AAA Accounting 
provides a management and audit trail for user and administrative sessions through 
RADIUS and TACACS+. 
Impact: 
Enabling aaa accounting exec creates accounting records for the EXEC terminal 
sessions on the network access server. These records include start and stop times, 
usernames, and date information. Organizations should regularly monitor these records 
for exceptions, remediate issues, and report findings. 
Audit: 
Perform the following to determine if aaa accounting for EXEC shell session is required: 
Verify a command string result returns 
hostname#show running-config | incl aaa accounting exec 
Remediation: 
Configure AAA accounting for EXEC shell session. 
hostname(config)#aaa accounting exec {default | list-name | guarantee-first}  
{start-stop | stop-only | none} {radius | group group-name} 
Default Value: 
AAA accounting is not enabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a1.html#GUID-
0520BCEF-89FB-4505-A5DF-D7F1389F1BBA

Page 33 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.2 Collect Audit Logs 
 Collect audit logs. Ensure that logging, per the enterprise’s audit log 
management process, has been enabled across enterprise assets. 
● ● ● 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 34 
Internal Only - General 
1.1.9 Set 'aaa accounting network' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Runs accounting for all network-related service requests. 
Rationale: 
Authentication, authorization and accounting (AAA) systems provide an authoritative 
source for managing and monitoring access for devices. Centralizing control improves 
consistency of access control, the services that may be accessed once authenticated 
and accountability by tracking services accessed. Additionally, centralizing access 
control simplifies and reduces administrative costs of account provisioning and de-
provisioning, especially when managing a large number of devices. AAA Accounting 
provides a management and audit trail for user and administrative sessions through 
RADIUS and TACACS+. 
Impact: 
Implementing aaa accounting network creates accounting records for a method list 
including ARA, PPP, SLIP, and NCPs sessions. Organizations should regular monitor 
these records for exceptions, remediate issues, and report findings. 
Audit: 
Perform the following to determine if aaa accounting for connection is required: 
Verify a command string result returns 
hostname#show running-config | incl aaa accounting network 
Remediation: 
Configure AAA accounting for connections. 
hostname(config)#aaa accounting network {default | list-name | guarantee-
first}  
{start-stop | stop-only | none} {radius | group group-name} 
Default Value: 
AAA accounting is not enabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a1.html#GUID-
0520BCEF-89FB-4505-A5DF-D7F1389F1BBA

Page 35 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.2 Collect Audit Logs 
 Collect audit logs. Ensure that logging, per the enterprise’s audit log 
management process, has been enabled across enterprise assets. 
● ● ● 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 36 
Internal Only - General 
1.1.10 Set 'aaa accounting system' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Performs accounting for all system-level events not associated with users, such as 
reloads. 
Rationale: 
Authentication, authorization and accounting (AAA) systems provide an authoritative 
source for managing and monitoring access for devices. Centralizing control improves 
consistency of access control, the services that may be accessed once authenticated 
and accountability by tracking services accessed. Additionally, centralizing access 
control simplifies and reduces administrative costs of account provisioning and de-
provisioning, especially when managing a large number of devices. AAA Accounting 
provides a management and audit trail for user and administrative sessions through 
RADIUS and TACACS+. 
Impact: 
Enabling aaa accounting system creates accounting records for all system-level events. 
Organizations should regular monitor these records for exceptions, remediate issues, 
and report findings regularly. 
Audit: 
Perform the following to determine if aaa accounting system is required: 
Verify a command string result returns 
hostname#show running-config | incl aaa accounting system 
Remediation: 
Configure AAA accounting system. 
hostname(config)#aaa accounting system {default | list-name | guarantee-
first}  
{start-stop | stop-only | none} {radius | group group-name} 
Default Value: 
AAA accounting is not enabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a1.html#GUID-
0520BCEF-89FB-4505-A5DF-D7F1389F1BBA

Page 37 
Internal Only - General 
Additional Information: 
When system accounting is used and the accounting server is unreachable at system 
startup time, the system will not be accessible for approximately two minutes. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.2 Collect Audit Logs 
 Collect audit logs. Ensure that logging, per the enterprise’s audit log 
management process, has been enabled across enterprise assets. 
● ● ● 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 38 
Internal Only - General 
1.2 Access Rules 
Rules in the access class enforce controls for device administrative connections.

Page 39 
Internal Only - General 
1.2.1 Set 'privilege 1' for local users (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Sets the privilege level for the user. 
Rationale: 
Default device configuration does not require strong user authentication potentially 
enabling unfettered access to an attacker that is able to reach the device. Creating a 
local account with privilege level 1 permissions only allows the local user to access the 
device with EXEC-level permissions and will be unable to modify the device without 
using the enable password. In addition, require the use of an encrypted password as 
well (see Section 1.1.4.4 - Require Encrypted User Passwords). 
Impact: 
Organizations should create policies requiring all local accounts with 'privilege level 1' 
with encrypted passwords to reduce the risk of unauthorized access. Default 
configuration settings do not provide strong user authentication to the device. 
Audit: 
Perform the following to determine if a user with an encrypted password is enabled: 
Verify all username results return "privilege 1" 
hostname#show running-config | incl privilege 
Remediation: 
Set the local user to privilege level 1. 
hostname(config)#username <LOCAL_USERNAME> privilege 1  
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/s1/sec-cr-t2-z.html#GUID-
34B3E43E-0F79-40E8-82B6-A4B5F1AFF1AD 
CIS Controls: 
Controls Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 40 
Internal Only - General 
Controls Version Control IG 1 IG 2 IG 3 
v7 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 41 
Internal Only - General 
1.2.2 Set 'transport input ssh' for 'line vty' connections 
(Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Selects the Secure Shell (SSH) protocol. 
Rationale: 
Configuring VTY access control restricts remote access to only those authorized to 
manage the device and prevents unauthorized users from accessing the system. 
Impact: 
To reduce risk of unauthorized access, organizations should require all VTY 
management line protocols to be limited to ssh. 
Audit: 
Perform the following to determine if SSH is the only transport method for incoming VTY 
logins: 
The result should show only "ssh" for "transport input" 
hostname#show running-config | sec vty    
Remediation: 
Apply SSH to transport input on all VTY management lines 
hostname(config)#line vty <line-number> <ending-line-number> 
hostname(config-line)#transport input ssh  
References: 
1. http://www.cisco.com/en/US/docs/ios/termserv/command/reference/tsv_s1.html#
wp1069219 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
6.5 Require MFA for Administrative Access 
 Require MFA for all administrative access accounts, where supported, on all 
enterprise assets, whether managed on-site or through a third-party provider. 
● ● ●

Page 42 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v7 
4.5 Use Multifactor Authentication For All Administrative 
Access 
 Use multi-factor authentication and encrypted channels for all administrative 
account access. 
 ● ●

Page 43 
Internal Only - General 
1.2.3 Set 'no exec' for 'line aux 0' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
The 'no exec' command restricts a line to outgoing connections only. 
Rationale: 
Unused ports should be disabled, if not required, since they provide a potential access 
path for attackers. Some devices include both an auxiliary and console port that can be 
used to locally connect to and configure the device. The console port is normally the 
primary port used to configure the device; even when remote, backup administration is 
required via console server or Keyboard, Video, Mouse (KVM) hardware. The auxiliary 
port is primarily used for dial-up administration via an external modem; instead, use 
other available methods. 
Impact: 
Organizations can reduce the risk of unauthorized access by disabling the 'aux' port 
with the 'no exec' command. Conversely, not restricting access through the 'aux' port 
increases the risk of remote unauthorized access. 
Audit: 
Perform the following to determine if the EXEC process for the aux port is disabled: 
Verify no exec 
hostname#show running-config all | sec aux 
Verify you see the following "no exec" 
Remediation: 
Disable the EXEC process on the auxiliary port. 
hostname(config)#line aux 0 
hostname(config-line)#no exec 
References: 
1. https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9200/software/releas
e/17-6/command_reference/b_176_9200_cr.html

Page 44 
Internal Only - General 
Additional Information: 
Some Cisco devices don't even have an auxiliary port, therefore this control will fail. 
Adjusting this control logic: check for existence of auxiliary port (ie check for 'none_exist' 
of a section with 'aux') combined with a logical OR with the current control. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 45 
Internal Only - General 
1.2.4 Create 'access-list' for use with 'line vty' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Access lists control the transmission of packets on an interface, control Virtual Terminal 
Line (VTY) access, and restrict the contents of routing updates. The Cisco IOS software 
stops checking the extended access list after a match occurs. 
Rationale: 
VTY ACLs control what addresses may attempt to log in to the router. Configuring VTY 
lines to use an ACL, restricts the sources where a user can manage the device. You 
should limit the specific host(s) and or network(s) authorized to connect to and configure 
the device, via an approved protocol, to those individuals or systems authorized to 
administer the device. For example, you could limit access to specific hosts, so that only 
network managers can configure the devices only by using specific network 
management workstations. Make sure you configure all VTY lines to use the same ACL. 
Impact: 
Organizations can reduce the risk of unauthorized access by implementing access-lists 
for all VTY lines. Conversely, using VTY lines without access-lists increases the risk of 
unauthorized access. 
Audit: 
Perform the following to determine if the ACL is created: 
Verify the appropriate access-list definitions 
hostname#sh ip access-list <vty_acl_number> 
Remediation: 
Configure the VTY ACL that will be used to restrict management access to the device. 
hostname(config)#access-list <vty_acl_number> permit tcp 
<vty_acl_block_with_mask> any 
hostname(config)#access-list <vty_acl_number> permit tcp host <vty_acl_host> 
any 
hostname(config)#deny ip any any log 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a2.html#GUID-
9EA733A3-1788-4882-B8C3-AB0A2949120C

Page 46 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
12.8 Establish and Maintain Dedicated Computing 
Resources for All Administrative Work 
 Establish and maintain dedicated computing resources, either physically or 
logically separated, for all administrative tasks or tasks requiring administrative 
access. The computing resources should be segmented from the enterprise's 
primary network and not be allowed internet access. 
  ● 
v7 
11.7 Manage Network Infrastructure Through a Dedicated 
Network 
 Manage the network infrastructure across network connections that are 
separated from the business use of that network, relying on separate VLANs or, 
preferably, on entirely different physical connectivity for management sessions for 
network devices. 
 ● ●

Page 47 
Internal Only - General 
1.2.5 Set 'access-class' for 'line vty' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
The 'access-class' setting restricts incoming and outgoing connections between a 
particular vty (into a Cisco device) and the networking devices associated with 
addresses in an access list. 
Rationale: 
Restricting the type of network devices, associated with the addresses on the access-
list, further restricts remote access to those devices authorized to manage the device 
and reduces the risk of unauthorized access. 
Impact: 
Applying 'access'class' to line VTY further restricts remote access to only those devices 
authorized to manage the device and reduces the risk of unauthorized access. 
Conversely, using VTY lines with 'access class' restrictions increases the risks of 
unauthorized access. 
Audit: 
Perform the following to determine if the ACL is set: 
Verify you see the access-class defined 
hostname#sh run | sec vty <line-number> <ending-line-number> 
Remediation: 
Configure remote management access control restrictions for all VTY lines. 
hostname(config)#line vty <line-number> <ending-line-number> 
hostname(config-line)# access-class <vty_acl_number> in 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a2.html#GUID-
FB9BC58A-F00A-442A-8028-1E9E260E54D3

Page 48 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
12.8 Establish and Maintain Dedicated Computing 
Resources for All Administrative Work 
 Establish and maintain dedicated computing resources, either physically or 
logically separated, for all administrative tasks or tasks requiring administrative 
access. The computing resources should be segmented from the enterprise's 
primary network and not be allowed internet access. 
  ● 
v7 
11.7 Manage Network Infrastructure Through a Dedicated 
Network 
 Manage the network infrastructure across network connections that are 
separated from the business use of that network, relying on separate VLANs or, 
preferably, on entirely different physical connectivity for management sessions for 
network devices. 
 ● ●

Page 49 
Internal Only - General 
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
If no input is detected during the interval, the EXEC facility resumes the current 
connection. If no connections exist, the EXEC facility returns the terminal to the idle 
state and disconnects the incoming session. 
Rationale: 
This prevents unauthorized users from misusing abandoned sessions. For example, if 
the network administrator leaves for the day and leaves a computer open with an 
enabled login session accessible. There is a trade-off here between security (shorter 
timeouts) and usability (longer timeouts). Review your local policies and operational 
needs to determine the best timeout value. In most cases, this should be no more than 
10 minutes. 
Impact: 
Organizations should prevent unauthorized use of unattended or abandoned sessions 
by an automated control. Enabling 'exec-timeout' with an appropriate length of minutes 
or seconds prevents unauthorized access of abandoned sessions. 
Audit: 
Perform the following to determine if the timeout is configured: 
Verify you return a result 
hostname#show running-config all | sec line aux 0 
Remediation: 
Configure device timeout (10 minutes or less) to disconnect sessions after a fixed idle 
time. 
hostname(config)#line aux 0 
hostname(config-line)#exec-timeout <timeout_in_minutes> <timeout_in_seconds> 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/D_through_E.html#GUID-76805E6F-9E89-4457-
A9DC-5944C8FE5419

Page 50 
Internal Only - General 
Additional Information: 
Some Cisco devices don't even have an auxiliary port, therefore this control will fail. 
Adjusting this control logic: check for existence of auxiliary port (ie check for 'none_exist' 
of a section with 'aux') combined with a logical OR with the current control. in addition 
adjusting the regex to match the control's title. Adjusting the 'audit' section. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.3 Configure Automatic Session Locking on Enterprise 
Assets 
 Configure automatic session locking on enterprise assets after a defined period 
of inactivity. For general purpose operating systems, the period must not exceed 
15 minutes. For mobile end-user devices, the period must not exceed 2 minutes. 
● ● ● 
v7 16.11 Lock Workstation Sessions After Inactivity 
 Automatically lock workstation sessions after a standard period of inactivity. ● ● ●

Page 51 
Internal Only - General 
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 'line 
console 0' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
If no input is detected during the interval, the EXEC facility resumes the current 
connection. If no connections exist, the EXEC facility returns the terminal to the idle 
state and disconnects the incoming session. 
Rationale: 
This prevents unauthorized users from misusing abandoned sessions. For example, if 
the network administrator leaves for the day and leaves a computer open with an 
enabled login session accessible. There is a trade-off here between security (shorter 
timeouts) and usability (longer timeouts). Review your local policies and operational 
needs to determine the best timeout value. In most cases, this should be no more than 
10 minutes. 
Impact: 
Organizations should prevent unauthorized use of unattended or abandoned sessions 
by an automated control. Enabling 'exec-timeout' with an appropriate length reduces the 
risk of unauthorized access of abandoned sessions. 
Audit: 
Perform the following to determine if the timeout is configured: 
Verify you return a result 
hostname#show running-config all | section line con 0 
Remediation: 
Configure device timeout (10 minutes or less) to disconnect sessions after a fixed idle 
time. 
hostname(config)#line con 0 
hostname(config-line)#exec-timeout <timeout_in_minutes> <timeout_in_seconds> 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/D_through_E.html#GUID-76805E6F-9E89-4457-
A9DC-5944C8FE5419

Page 52 
Internal Only - General 
Additional Information: 
adjusting the 'audit procedure': removing the 'Note' and changing the command to use 
from 'sh run | sec line con 0' to 'show running-config all | section line con 0'. 
Adjusting the regex from '^\s*(exec-timeout)\s*((10)|([0-9]))\s*$' to '^\s*(exec-
timeout)\s+((10\s+0\s*$)|([0-9]\s+([0-9]\s*$|[1-5][0-9]\s*$)))' 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.3 Configure Automatic Session Locking on Enterprise 
Assets 
 Configure automatic session locking on enterprise assets after a defined period 
of inactivity. For general purpose operating systems, the period must not exceed 
15 minutes. For mobile end-user devices, the period must not exceed 2 minutes. 
● ● ● 
v7 16.11 Lock Workstation Sessions After Inactivity 
 Automatically lock workstation sessions after a standard period of inactivity. ● ● ●

Page 53 
Internal Only - General 
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 'line 
vty' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
If no input is detected during the interval, the EXEC facility resumes the current 
connection. If no connections exist, the EXEC facility returns the terminal to the idle 
state and disconnects the incoming session. 
Rationale: 
This prevents unauthorized users from misusing abandoned sessions. For example, if 
the network administrator leaves for the day and leaves a computer open with an 
enabled login session accessible. There is a trade-off here between security (shorter 
timeouts) and usability (longer timeouts). Review your local policies and operational 
needs to determine the best timeout value. In most cases, this should be no more than 
10 minutes. 
Impact: 
Organizations should prevent unauthorized use of unattended or abandoned sessions 
by an automated control. Enabling 'exec-timeout' with an appropriate length of minutes 
or seconds prevents unauthorized access of abandoned sessions. 
Audit: 
Perform the following to determine if the timeout is configured: 
Verify you return a result NOTE: If you set an exec-timeout of 10 minutes, this will not 
show up in the configuration 
hostname#sh line vty <tty_line_number> | begin Timeout 
Remediation: 
Configure device timeout (10 minutes or less) to disconnect sessions after a fixed idle 
time. 
hostname(config)#line vty {line_number} [ending_line_number] 
hostname(config-line)#exec-timeout <<span>timeout_in_minutes> 
<timeout_in_seconds</span>> 
References: 
1. https://www.cisco.com/c/en/us/td/docs/switches/datacenter/mds9000/sw/comma
nd/b_cisco_mds_9000_cr_book/l_commands.html#wp3716128869

Page 54 
Internal Only - General 
Additional Information: 
Adjusting the 'Audit Procedure': command currently listed will show the settings and 
status of line vty xx, but then you need to check all line vty from 0 to 98... And it doesn't 
really match what the Artifact is checking... I would suggest to use the 'show running-
config all | section line vty' command in the 'audit procedure' and look for the line 
starting with exec-timeout. 
adjusting the artifact for the regex to match with the control's title (...less than or equal to 
10 minutes). All line vty should have exec-timeout less than or equal to 10 minutes, then 
adjusting the Artifact's "# of config lines to match" from 'at least one' to 'all' 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.3 Configure Automatic Session Locking on Enterprise 
Assets 
 Configure automatic session locking on enterprise assets after a defined period 
of inactivity. For general purpose operating systems, the period must not exceed 
15 minutes. For mobile end-user devices, the period must not exceed 2 minutes. 
● ● ● 
v7 16.11 Lock Workstation Sessions After Inactivity 
 Automatically lock workstation sessions after a standard period of inactivity. ● ● ●

Page 55 
Internal Only - General 
1.2.9 Set 'http Secure-server' limit (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Device management includes the ability to control the number of administrators and 
management sessions that manage a device. Limiting the number of allowed 
administrators and sessions per administrator based on account type, role, or access 
type is helpful in limiting risks related to denial-of-service (DoS) attacks. 
Rationale: 
This requirement addresses concurrent sessions for administrative accounts and does 
not address concurrent sessions by a single administrator via multiple administrative 
accounts. The maximum number of concurrent sessions should be defined based upon 
mission needs and the operational environment for each system. At a minimum, limits 
must be set for SSH, HTTPS, account of last resort, and root account sessions. Center 
for Internet Security recommends a limit of 2 
Audit: 
The result should show ip http secure-server with max connections on following line 
hostname#show run | inc ip http secure-server 
Remediation: 
hostname(config)#ip http max-connections 2 
References: 
1. NIST SP 800-53 :: AC-10 
Additional Information: 
limiting http max connections is useful only if http server or http secure-server, at least 
one, is enable. if HTTP server is disabled (Artifact #2 "HTTP server is disabled") and 
HTTPS server is disabled (Artifact #3 "HTTPS server is disabled"), then this control is 
passed, otherwise it needs to limit connections to a maximum of 2 (Artifact #1 "Max http 
secure-server limit"). Adjusting the regex from '^\sip\s+http\s+max-connections\s+0-2?$' 
to '^\sip\s+http\s+max-connections\s+[0-2]\s*$'

Page 56 
Internal Only - General 
CIS Controls: 
Controls Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v7 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 57 
Internal Only - General 
1.2.10 Set 'exec-timeout' to less than or equal to 10 min on 'ip 
http' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
If no input is detected during the interval, the EXEC facility resumes the current 
connection. If no connections exist, the EXEC facility returns the terminal to the idle 
state and disconnects the incoming session. 
Rationale: 
This prevents unauthorized users from misusing abandoned sessions. For example, if 
the network administrator leaves for the day and leaves a computer open with an 
enabled login session accessible. There is a trade-off here between security (shorter 
timeouts) and usability (longer timeouts). Review your local policies and operational 
needs to determine the best timeout value. In most cases, this should be no more than 
10 minutes. 
This prevents unauthorized users from misusing abandoned sessions. For example, if 
the network administrator leaves for the day and leaves a computer open with an 
enabled login session accessible. There is a trade-off here between security (shorter 
timeouts) and usability (longer timeouts). Review your local policies and operational 
needs to determine the best timeout value. In most cases, this should be no more than 
10 minutes. 
Audit: 
Perform the following to determine if the timeout is configured: 
sh run | beg ip http timeout-policy 
Remediation: 
Configure device timeout (10 minutes or less) to disconnect sessions after a fixed idle 
time. 
ip http timeout-policy idle 600 life {nnnn} requests {nn} 
Default Value: 
disabled

Page 58 
Internal Only - General 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/D_through_E.html#GUID-76805E6F-9E89-4457-
A9DC-5944C8FE5419 
Additional Information: 
limiting http exec-timeout to 10 minutes is useful only if http server or http secure-server, 
at least one, is enable. if HTTP server is disabled (Artifact #2 "HTTP server is disabled") 
and HTTPS server is disabled (Artifact #3 "HTTPS server is disabled"), then this control 
is passed, otherwise it needs to limit exec-timeout to a maximum of 10 minutes (600 
seconds) (Artifact #1 "Max http secure-server limit"). 
CIS Controls: 
Controls Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v7 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 59 
Internal Only - General 
1.3 Banner Rules 
Rules in the banner class communicate legal rights to users.

Page 60 
Internal Only - General 
1.3.1 Set the 'banner-text' for 'banner exec' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
This command specifies a message to be displayed when an EXEC process is created 
(a line is activated, or an incoming connection is made to a vty). Follow this command 
with one or more blank spaces and a delimiting character of your choice. Then enter 
one or more lines of text, terminating the message with the second occurrence of the 
delimiting character. 
When a user connects to a router, the message-of-the-day (MOTD) banner appears 
first, followed by the login banner and prompts. After the user logs in to the router, the 
EXEC banner or incoming banner will be displayed, depending on the type of 
connection. For a reverse Telnet login, the incoming banner will be displayed. For all 
other connections, the router will display the EXEC banner. 
Rationale: 
"Network banners are electronic messages that provide notice of legal rights to users of 
computer networks. From a legal standpoint, banners have four primary functions. 
• First, banners may be used to generate consent to real-time monitoring under 
Title III. 
• Second, banners may be used to generate consent to the retrieval of stored files 
and records pursuant to ECPA. 
• Third, in the case of government networks, banners may eliminate any Fourth 
Amendment "reasonable expectation of privacy" that government employees or 
other users might otherwise retain in their use of the government's network under 
O'Connor v. Ortega, 480 U.S. 709 (1987). 
• Fourth, in the case of a non-government network, banners may establish a 
system administrator's "common authority" to consent to a law enforcement 
search pursuant to United States v. Matlock, 415 U.S. 164 (1974)." (US 
Department of Justice APPENDIX A: Sample Network Banner Language) 
Impact: 
Organizations provide appropriate legal notice(s) and warning(s) to persons accessing 
their networks by using a 'banner-text' for the banner exec command. 
Audit: 
Perform the following to determine if the exec banner is set:

Page 61 
Internal Only - General 
 
hostname#sh running-config | beg banner exec 
If the command does not return a result, the banner is not enabled 
Remediation: 
Configure the EXEC banner presented to a user when accessing the devices enable 
prompt. 
 
hostname(config)#banner exec c 
Enter TEXT message. End with the character 'c'. 
<banner-text> 
c 
Default Value: 
No banner is set by default 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/A_through_B.html#GUID-0DEF5B57-A7D9-
4912-861F-E837C82A3881 
Additional Information: 
The default is no banner. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
14.1 Establish and Maintain a Security Awareness 
Program 
 Establish and maintain a security awareness program. The purpose of a security 
awareness program is to educate the enterprise’s workforce on how to interact with 
enterprise assets and data in a secure manner. Conduct training at hire and, at a 
minimum, annually. Review and update content annually, or when significant 
enterprise changes occur that could impact this Safeguard. 
● ● ● 
v7 
17.3 Implement a Security Awareness Program 
 Create a security awareness program for all workforce members to complete on 
a regular basis to ensure they understand and exhibit the necessary behaviors and 
skills to help ensure the security of the organization. The organization's security 
awareness program should be communicated in a continuous and engaging 
manner. 
● ● ●

Page 62 
Internal Only - General 
1.3.2 Set the 'banner-text' for 'banner login' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Follow the banner login command with one or more blank spaces and a delimiting 
character of your choice. Then enter one or more lines of text, terminating the message 
with the second occurrence of the delimiting character. 
When a user connects to the router, the message-of-the-day (MOTD) banner (if 
configured) appears first, followed by the login banner and prompts. After the user 
successfully logs in to the router, the EXEC banner or incoming banner will be 
displayed, depending on the type of connection. For a reverse Telnet login, the 
incoming banner will be displayed. For all other connections, the router will display the 
EXEC banner. 
Rationale: 
"Network banners are electronic messages that provide notice of legal rights to users of 
computer networks. From a legal standpoint, banners have four primary functions. 
• First, banners may be used to generate consent to real-time monitoring under 
Title III. 
• Second, banners may be used to generate consent to the retrieval of stored files 
and records pursuant to ECPA. 
• Third, in the case of government networks, banners may eliminate any Fourth 
Amendment "reasonable expectation of privacy" that government employees or 
other users might otherwise retain in their use of the government's network under 
O'Connor v. Ortega, 480 U.S. 709 (1987). 
• Fourth, in the case of a non-government network, banners may establish a 
system administrator's "common authority" to consent to a law enforcement 
search pursuant to United States v. Matlock, 415 U.S. 164 (1974)." (US 
Department of Justice APPENDIX A: Sample Network Banner Language) 
Impact: 
Organizations provide appropriate legal notice(s) and warning(s) to persons accessing 
their networks by using a 'banner-text' for the banner login command. 
Audit: 
Perform the following to determine if the login banner is set: 
 
hostname#show running-config | beg banner login 
If the command does not return a result, the banner is not enabled.

Page 63 
Internal Only - General 
Remediation: 
Configure the device so a login banner presented to a user attempting to access the 
device. 
 
hostname(config)#banner login c 
Enter TEXT message. End with the character 'c'. 
<banner-text> 
c 
Default Value: 
No banner is set by default 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/A_through_B.html#GUID-FF0B6890-85B8-
4B6A-90DD-1B7140C5D22F 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
14.1 Establish and Maintain a Security Awareness 
Program 
 Establish and maintain a security awareness program. The purpose of a security 
awareness program is to educate the enterprise’s workforce on how to interact with 
enterprise assets and data in a secure manner. Conduct training at hire and, at a 
minimum, annually. Review and update content annually, or when significant 
enterprise changes occur that could impact this Safeguard. 
● ● ● 
v7 
17.3 Implement a Security Awareness Program 
 Create a security awareness program for all workforce members to complete on 
a regular basis to ensure they understand and exhibit the necessary behaviors and 
skills to help ensure the security of the organization. The organization's security 
awareness program should be communicated in a continuous and engaging 
manner. 
● ● ●

Page 64 
Internal Only - General 
1.3.3 Set the 'banner-text' for 'banner motd' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
This MOTD banner is displayed to all terminals connected and is useful for sending 
messages that affect all users (such as impending system shutdowns). Use the no 
exec-banner or no motd-banner command to disable the MOTD banner on a line. The 
no exec-banner command also disables the EXEC banner on the line. 
When a user connects to the router, the MOTD banner appears before the login prompt. 
After the user logs in to the router, the EXEC banner or incoming banner will be 
displayed, depending on the type of connection. For a reverse Telnet login, the 
incoming banner will be displayed. For all other connections, the router will display the 
EXEC banner. 
Rationale: 
"Network banners are electronic messages that provide notice of legal rights to users of 
computer networks. From a legal standpoint, banners have four primary functions. 
• First, banners may be used to generate consent to real-time monitoring under 
Title III. 
• Second, banners may be used to generate consent to the retrieval of stored files 
and records pursuant to ECPA. 
• Third, in the case of government networks, banners may eliminate any Fourth 
Amendment "reasonable expectation of privacy" that government employees or 
other users might otherwise retain in their use of the government's network under 
O'Connor v. Ortega, 480 U.S. 709 (1987). 
• Fourth, in the case of a non-government network, banners may establish a 
system administrator's "common authority" to consent to a law enforcement 
search pursuant to United States v. Matlock, 415 U.S. 164 (1974)." (US 
Department of Justice APPENDIX A: Sample Network Banner Language) 
Impact: 
Organizations provide appropriate legal notice(s) and warning(s) to persons accessing 
their networks by using a 'banner-text' for the banner motd command. 
Audit: 
Perform the following to determine if the login banner is set: 
 
hostname#sh running-config | beg banner motd 
If the command does not return a result, the banner is not enabled.

Page 65 
Internal Only - General 
Remediation: 
Configure the message of the day (MOTD) banner presented when a user first connects 
to the device. 
 
hostname(config)#banner motd c 
Enter TEXT message. End with the character 'c'. 
<banner-text> 
c 
Default Value: 
No banner is set by default 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/A_through_B.html#GUID-7416C789-9561-
44FC-BB2A-D8D8AFFB77DD 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
14.1 Establish and Maintain a Security Awareness 
Program 
 Establish and maintain a security awareness program. The purpose of a security 
awareness program is to educate the enterprise’s workforce on how to interact with 
enterprise assets and data in a secure manner. Conduct training at hire and, at a 
minimum, annually. Review and update content annually, or when significant 
enterprise changes occur that could impact this Safeguard. 
● ● ● 
v7 
17.3 Implement a Security Awareness Program 
 Create a security awareness program for all workforce members to complete on 
a regular basis to ensure they understand and exhibit the necessary behaviors and 
skills to help ensure the security of the organization. The organization's security 
awareness program should be communicated in a continuous and engaging 
manner. 
● ● ●

Page 66 
Internal Only - General 
1.3.4 Set the 'banner-text' for 'webauth banner' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
This banner is displayed to all terminals connected and is useful for sending messages 
that affect all users (such as impending system shutdowns). Use the no exec-banner or 
no motd-banner command to disable the banner on a line. The no exec-banner 
command also disables the EXEC banner on the line. 
When a user connects to the router, the MOTD banner appears before the login prompt. 
After the user logs in to the router, the EXEC banner or incoming banner will be 
displayed, depending on the type of connection. For a reverse Telnet login, the 
incoming banner will be displayed. For all other connections, the router will display the 
EXEC banner. 
Rationale: 
"Network banners are electronic messages that provide notice of legal rights to users of 
computer networks. From a legal standpoint, banners have four primary functions. 
• First, banners may be used to generate consent to real-time monitoring under 
Title III. 
• Second, banners may be used to generate consent to the retrieval of stored files 
and records pursuant to ECPA. 
• Third, in the case of government networks, banners may eliminate any Fourth 
Amendment "reasonable expectation of privacy" that government employees or 
other users might otherwise retain in their use of the government's network under 
O'Connor v. Ortega, 480 U.S. 709 (1987). 
• Fourth, in the case of a non-government network, banners may establish a 
system administrator's "common authority" to consent to a law enforcement 
search pursuant to United States v. Matlock, 415 U.S. 164 (1974)." (US 
Department of Justice APPENDIX A: Sample Network Banner Language) 
Impact: 
Organizations provide appropriate legal notice(s) and warning(s) to persons accessing 
their networks by using a 'banner-text' for the banner motd command. 
Audit: 
Perform the following to determine if the login banner is set: 
 
hostname#show ip admission auth-proxy-banner http 
If the command does not return a result, the banner is not enabled.

Page 67 
Internal Only - General 
Remediation: 
Configure the webauth banner presented when a user connects to the device. 
hostname(config)#ip admission auth-proxy-banner http {banner-text | filepath} 
Default Value: 
No banner is set by default 
References: 
1. https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9500/software/releas
e/16-
9/configuration_guide/sec/b_169_sec_9500_cg/configuring_web_based_authenti
cation.html 
Additional Information: 
Set the 'banner-text' for 'webauth banner' is useful only if http server or http secure-
server, at least one, is enabled. if HTTP server is disabled (Artifact #1 "HTTP server is 
disabled") and HTTPS server is disabled (Artifact #2 "HTTPS server is disabled"), then 
this control is passed, otherwise 'ip admission auth-proxy-banner http' must exist 
(Artifact #3 "check for ip admission auth-proxy-banner http"). 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
14.1 Establish and Maintain a Security Awareness 
Program 
 Establish and maintain a security awareness program. The purpose of a security 
awareness program is to educate the enterprise’s workforce on how to interact with 
enterprise assets and data in a secure manner. Conduct training at hire and, at a 
minimum, annually. Review and update content annually, or when significant 
enterprise changes occur that could impact this Safeguard. 
● ● ● 
v7 
17.3 Implement a Security Awareness Program 
 Create a security awareness program for all workforce members to complete on 
a regular basis to ensure they understand and exhibit the necessary behaviors and 
skills to help ensure the security of the organization. The organization's security 
awareness program should be communicated in a continuous and engaging 
manner. 
● ● ●

Page 68 
Internal Only - General 
1.4 Password Rules 
Rules in the password class enforce secure, local device authentication credentials.

Page 69 
Internal Only - General 
1.4.1 Set 'password' for 'enable secret' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Use the enable secret command to provide an additional layer of security over the 
enable password. The enable secret command provides better security by storing the 
enable secret password using a nonreversible cryptographic function. The added layer 
of security encryption provides is useful in environments where the password crosses 
the network or is stored on a TFTP server. 
Rationale: 
In Cisco IOS XE, password types 8 and 9 are considered more secure than older 
password types because they use strong hashing algorithms. 
• Type 8 passwords use PBKDF2 with SHA-256, which is an improvement over 
older MD5-based hashing methods. 
• Type 9 passwords use SCRYPT, which is designed to be memory-intensive, 
making it harder for attackers to brute-force. 
Cisco generally recommends using Type 8 or Type 9 for better security, but Type 9 is 
the default in IOS XE. However, some auditors may prefer Type 8 because Type 9 is 
not NIST-approved, especially in U.S. Defense and Public Sector environments. 
Impact: 
While enable secret in Cisco IOS XE enhances security, it can have some potential 
drawbacks: 
• Password Recovery Complexity: If the password is lost, recovering access can 
be difficult, 
• Performance Overhead: Stronger hashing algorithms (like SCRYPT) can slightly 
increase CPU usage, especially on older hardware. 
• Configuration Management Issues: If not properly documented, administrators 
may struggle with access control changes. 
• Potential Lockout Risks: If centralized authentication fails and the enable secret 
is unknown, network administrators could be locked out. 
• Security Misconfiguration: If weak passwords are used, even with encryption, 
attackers could still brute-force them. 
Audit: 
Perform the following to determine enable secret is set: 
If the command does not return a result, the enable password is not set.

Page 70 
Internal Only - General 
hostname#sh run | incl enable secret  
Remediation: 
Configure a strong, enable secret password. 
hostname(config)#enable secret 9 {ENABLE_SECRET_PASSWORD}  
Default Value: 
No enable secret password setup by default 
References: 
1. https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9600/software/releas
e/16-
12/configuration_guide/sec/b_1612_sec_9600_cg/controlling_switch_access_wit
h_passwords_and_privilege_levels.html 
Additional Information: 
Note: You cannot recover a lost encrypted password. You must clear NVRAM and set a 
new password. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
5.4 Restrict Administrator Privileges to Dedicated 
Administrator Accounts 
 Restrict administrator privileges to dedicated administrator accounts on 
enterprise assets. Conduct general computing activities, such as internet 
browsing, email, and productivity suite use, from the user’s primary, non-privileged 
account. 
● ● ● 
v7 
4.3 Ensure the Use of Dedicated Administrative Accounts 
 Ensure that all users with administrative account access use a dedicated or 
secondary account for elevated activities. This account should only be used for 
administrative activities and not internet browsing, email, or similar activities. 
● ● ●

Page 71 
Internal Only - General 
1.4.2 Enable 'service password-encryption' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
When password encryption is enabled, the encrypted form of the passwords is 
displayed when a more system:running-config command is entered. 
Rationale: 
This requires passwords to be encrypted in the configuration file to prevent 
unauthorized users from learning the passwords just by reading the configuration. When 
not enabled, many of the device's passwords will be rendered in plain text in the 
configuration file. This service ensures passwords are rendered as encrypted strings 
preventing an attacker from easily determining the configured value. 
Impact: 
Organizations implementing 'service password-encryption' reduce the risk of 
unauthorized users learning clear text passwords to Cisco IOS configuration files. 
However, the algorithm used is not designed to withstand serious analysis and should 
be treated like clear-text. 
Audit: 
Perform the following to determine if a user with an encrypted password is enabled: 
Ensure a result that matches the command return 
 
hostname#sh run | incl service password-encryption 
Remediation: 
Enable password encryption service to protect sensitive access passwords in the device 
configuration. 
 
hostname(config)#service password-encryption 
Default Value: 
Service password encryption is not set by default 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/s1/sec-cr-s1.html#GUID-
CC0E305A-604E-4A74-8A1A-975556CE5871

Page 72 
Internal Only - General 
Additional Information: 
Caution: This command does not provide a high level of network security. If you use this 
command, you should also take additional network security measures. 
Note: You cannot recover a lost encrypted password. You must clear NVRAM and set a 
new password. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
3.11 Encrypt Sensitive Data at Rest 
 Encrypt sensitive data at rest on servers, applications, and databases containing 
sensitive data. Storage-layer encryption, also known as server-side encryption, 
meets the minimum requirement of this Safeguard. Additional encryption methods 
may include application-layer encryption, also known as client-side encryption, 
where access to the data storage device(s) does not permit access to the plain-text 
data.  
 ● ● 
v7 16.4 Encrypt or Hash all Authentication Credentials 
 Encrypt or hash with a salt all authentication credentials when stored.  ● ●

Page 73 
Internal Only - General 
1.4.3 Set 'username secret' for all local users (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Username secret password type 5 and enable secret password type 5 must be migrated 
to the stronger password type 8 or 9. IF a device is upgraded from IOS XE 16.9 or later 
the type 5 is auto converted to type 9. 
The username secret command provides an additional layer of security over the 
username password. 
Rationale: 
Default device configuration does not require strong user authentication potentially 
enabling unfettered access to an attacker that is able to reach the device. Creating a 
local account with an encrypted password enforces login authentication and provides a 
fallback authentication mechanism for configuration in a named method list in a situation 
where centralized authentication, authorization, and accounting services are 
unavailable. 
Impact: 
Organizations implementing 'username secret' across their enterprise reduce the risk of 
unauthorized users gaining access to Cisco IOS devices by applying a MD5 hash and 
encrypting user passwords. 
Audit: 
Perform the following to determine if a user with an encrypted password is enabled: 
If a result does not return with secret, the feature is not enabled 
 
 hostname#show run | incl username 
Remediation: 
Create a local user with an encrypted, complex (not easily guessed) password. 
 
hostname(config)#username {{em}LOCAL_USERNAME{/em}} secret 
{{em}LOCAL_PASSWORD{/em}} 
Default Value: 
No passwords are set by default

Page 74 
Internal Only - General 
References: 
1. https://www.cisco.com/c/en/us/td/docs/switches/lan/catalyst9600/software/releas
e/16-
12/configuration_guide/sec/b_1612_sec_9600_cg/controlling_switch_access_wit
h_passwords_and_privilege_levels.html 
Additional Information: 
if any local user is defined ensure that "secret 9" is used for each of them (Artifact #1 
"check all username are using secret 9 only"), so that reverting the hash to the 
password is difficult. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
3.11 Encrypt Sensitive Data at Rest 
 Encrypt sensitive data at rest on servers, applications, and databases containing 
sensitive data. Storage-layer encryption, also known as server-side encryption, 
meets the minimum requirement of this Safeguard. Additional encryption methods 
may include application-layer encryption, also known as client-side encryption, 
where access to the data storage device(s) does not permit access to the plain-text 
data.  
 ● ● 
v7 16.4 Encrypt or Hash all Authentication Credentials 
 Encrypt or hash with a salt all authentication credentials when stored.  ● ●

Page 75 
Internal Only - General 
1.5 SNMP Rules 
Simple Network Management Protocol (SNMP) provides a standards-based interface to 
manage and monitor network devices. This section provides guidance on the secure 
configuration of SNMP parameters. 
The recommendations in this Section apply to Organizations using SNMP. 
Organizations using SNMP should review and implement the recommendations in this 
section.

Page 76 
Internal Only - General 
1.5.1 Set 'no snmp-server' to disable SNMP when unused 
(Manual) 
Profile Applicability: 
•  Level 1 
Description: 
If not in use, disable simple network management protocol (SNMP), read and write 
access. 
Rationale: 
SNMP read access allows remote monitoring and management of the device. 
Impact: 
Organizations not using SNMP should require all SNMP services to be disabled by 
running the 'no snmp-server' command. 
Audit: 
Verify the result reads "SNMP agent not enabled" 
hostname#show snmp community 
Remediation: 
Disable SNMP read and write access if not in used to monitor and/or manage device. 
hostname(config)#no snmp-server  
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
book.html 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ●

Page 77 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 78 
Internal Only - General 
1.5.2 Unset 'private' for 'snmp-server community' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
An SNMP community string permits read-only access to all objects. 
Rationale: 
The default community string "private" is well known. Using easy to guess, well known 
community string poses a threat that an attacker can effortlessly gain unauthorized 
access to the device. 
Impact: 
To reduce the risk of unauthorized access, Organizations should disable default, easy 
to guess, settings such as the 'private' setting for snmp-server community. 
Audit: 
Perform the following to determine if the public community string is enabled: 
Ensure private does not show as a result 
hostname# show snmp community 
Remediation: 
Disable the default SNMP community string private 
hostname(config)#no snmp-server community {private} 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s2.html#GUID-2F3F13E4-EE81-4590-871D-6AE1043473DE 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 79 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 80 
Internal Only - General 
1.5.3 Unset 'public' for 'snmp-server community' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
An SNMP community string permits read-only access to all objects. 
Rationale: 
The default community string "public" is well known. Using easy to guess, well known 
community string poses a threat that an attacker can effortlessly gain unauthorized 
access to the device. 
Impact: 
To reduce the risk of unauthorized access, Organizations should disable default, easy 
to guess, settings such as the 'public' setting for snmp-server community. 
Audit: 
Perform the following to determine if the public community string is enabled: Ensure 
public does not show as a result 
 
hostname# show snmp community 
Remediation: 
Disable the default SNMP community string "public" 
 
hostname(config)#no snmp-server community {public}  
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s2.html#GUID-2F3F13E4-EE81-4590-871D-6AE1043473DE 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 81 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 82 
Internal Only - General 
1.5.4 Do not set 'RW' for any 'snmp-server community' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Specifies read-write access. Authorized management stations can both retrieve and 
modify MIB objects. 
Rationale: 
Enabling SNMP read-write enables remote management of the device. Unless 
absolutely necessary, do not allow simple network management protocol (SNMP) write 
access. 
Impact: 
To reduce the risk of unauthorized access, Organizations should disable the SNMP 
'write' access for snmp-server community. 
Audit: 
Perform the following to determine if a read/write community string is enabled: 
Verify the result does not show a community string with a "RW" 
 
hostname#show run | incl snmp-server community 
Remediation: 
Disable SNMP write access. 
 
hostname(config)#no snmp-server community {<em>write_community_string</em>}  
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s2.html#GUID-2F3F13E4-EE81-4590-871D-6AE1043473DE 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 83 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 84 
Internal Only - General 
1.5.5 Set the ACL for each 'snmp-server community' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
This feature specifies a list of IP addresses that are allowed to use the community string 
to gain access to the SNMP agent. 
Rationale: 
If ACLs are not applied, then anyone with a valid SNMP community string can 
potentially monitor and manage the router. An ACL should be defined and applied for all 
SNMP access to limit access to a small number of authorized management stations 
segmented in a trusted management zone. If possible, use SNMPv3 which uses 
authentication, authorization, and data privatization (encryption). 
Impact: 
To reduce the risk of unauthorized access, Organizations should enable access control 
lists for all snmp-server communities and restrict the access to appropriate trusted 
management zones. If possible, implement SNMPv3 to apply authentication, 
authorization, and data privatization (encryption) for additional benefits to the 
organization. 
Audit: 
Perform the following to determine if an ACL is enabled: 
Verify the result shows a number after the community string 
 
hostname#show run | incl snmp-server community 
Remediation: 
Configure authorized SNMP community string and restrict access to authorized 
management systems. 
 
hostname(config)#snmp-server community <<em>community_string</em>> ro 
{<em>snmp_access-list_number |  
<span>snmp_access-list_name</span></em><span>}</span> 
Default Value: 
No ACL is set for SNMP

Page 85 
Internal Only - General 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s2.html#GUID-2F3F13E4-EE81-4590-871D-6AE1043473DE 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
12.8 Establish and Maintain Dedicated Computing 
Resources for All Administrative Work 
 Establish and maintain dedicated computing resources, either physically or 
logically separated, for all administrative tasks or tasks requiring administrative 
access. The computing resources should be segmented from the enterprise's 
primary network and not be allowed internet access. 
  ● 
v7 
11.7 Manage Network Infrastructure Through a Dedicated 
Network 
 Manage the network infrastructure across network connections that are 
separated from the business use of that network, relying on separate VLANs or, 
preferably, on entirely different physical connectivity for management sessions for 
network devices. 
 ● ●

Page 86 
Internal Only - General 
1.5.6 Create an 'access-list' for use with SNMP (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
You can use access lists to control the transmission of packets on an interface, control 
Simple Network Management Protocol (SNMP) access, and restrict the contents of 
routing updates. The Cisco IOS software stops checking the extended access list after a 
match occurs. 
Rationale: 
SNMP ACLs control what addresses are authorized to manage and monitor the device 
via SNMP. If ACLs are not applied, then anyone with a valid SNMP community string 
may monitor and manage the router. An ACL should be defined and applied for all 
SNMP community strings to limit access to a small number of authorized management 
stations segmented in a trusted management zone. 
Audit: 
Perform the following to determine if the ACL is created: 
Verify you the appropriate access-list definitions 
 
hostname#sh ip access-list <<em>snmp_acl_number</em>>  
Remediation: 
Configure SNMP ACL for restricting access to the device from authorized management 
stations segmented in a trusted management zone. 
 
hostname(config)#access-list <<em>snmp_acl_number</em>> permit 
<<em>snmp_access-list</em>> 
hostname(config)#access-list deny any log  
Default Value: 
SNMP does not use an access list. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-a2.html#GUID-
9EA733A3-1788-4882-B8C3-AB0A2949120C

Page 87 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
12.8 Establish and Maintain Dedicated Computing 
Resources for All Administrative Work 
 Establish and maintain dedicated computing resources, either physically or 
logically separated, for all administrative tasks or tasks requiring administrative 
access. The computing resources should be segmented from the enterprise's 
primary network and not be allowed internet access. 
  ● 
v7 
11.7 Manage Network Infrastructure Through a Dedicated 
Network 
 Manage the network infrastructure across network connections that are 
separated from the business use of that network, relying on separate VLANs or, 
preferably, on entirely different physical connectivity for management sessions for 
network devices. 
 ● ●

Page 88 
Internal Only - General 
1.5.7 Set 'snmp-server host' when using SNMP (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
SNMP notifications can be sent as traps to authorized management systems. 
Rationale: 
If SNMP is enabled for device management and device alerts are required, then ensure 
the device is configured to submit traps only to authorize management systems. 
Impact: 
Organizations using SNMP should restrict sending SNMP messages only to explicitly 
named systems to reduce unauthorized access. 
Audit: 
Perform the following to determine if SNMP traps are enabled: 
If the command returns configuration values, then SNMP is enabled. 
 
hostname#show run | incl snmp-server 
Remediation: 
Configure authorized SNMP trap community string and restrict sending messages to 
authorized management systems. 
 
hostname(config)#snmp-server host {ip_address} {trap_community_string} 
{notification-type}  
Default Value: 
A recipient is not specified to receive notifications. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s5.html#GUID-D84B2AB5-6485-4A23-8C26-73E50F73EE61

Page 89 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
12.8 Establish and Maintain Dedicated Computing 
Resources for All Administrative Work 
 Establish and maintain dedicated computing resources, either physically or 
logically separated, for all administrative tasks or tasks requiring administrative 
access. The computing resources should be segmented from the enterprise's 
primary network and not be allowed internet access. 
  ● 
v7 
11.7 Manage Network Infrastructure Through a Dedicated 
Network 
 Manage the network infrastructure across network connections that are 
separated from the business use of that network, relying on separate VLANs or, 
preferably, on entirely different physical connectivity for management sessions for 
network devices. 
 ● ●

Page 90 
Internal Only - General 
1.5.8 Set 'snmp-server enable traps snmp' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
SNMP notifications can be sent as traps to authorized management systems. 
Rationale: 
SNMP has the ability to submit traps . 
Impact: 
Organizations using SNMP should restrict trap types only to explicitly named traps to 
reduce unintended traffic. Enabling SNMP traps without specifying trap type will enable 
all SNMP trap types. 
Audit: 
Perform the following to determine if SNMP traps are enabled: 
If the command returns configuration values, then SNMP is enabled. 
 
hostname#show run | incl snmp-server 
Remediation: 
Enable SNMP traps. 
hostname(config)#snmp-server enable traps snmp authentication linkup linkdown 
coldstart  
Default Value: 
SNMP notifications are disabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s3.html#GUID-EB3EB677-A355-42C6-A139-85BA30810C54

Page 91 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
12.8 Establish and Maintain Dedicated Computing 
Resources for All Administrative Work 
 Establish and maintain dedicated computing resources, either physically or 
logically separated, for all administrative tasks or tasks requiring administrative 
access. The computing resources should be segmented from the enterprise's 
primary network and not be allowed internet access. 
  ● 
v7 
11.7 Manage Network Infrastructure Through a Dedicated 
Network 
 Manage the network infrastructure across network connections that are 
separated from the business use of that network, relying on separate VLANs or, 
preferably, on entirely different physical connectivity for management sessions for 
network devices. 
 ● ●

Page 92 
Internal Only - General 
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3 
(Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Specifies authentication of a packet with encryption when using SNMPv3 
Rationale: 
SNMPv3 provides much improved security over previous versions by offering options 
for Authentication and Encryption of messages. When configuring a user for SNMPv3 
you have the option of using a range of encryption schemes, or no encryption at all, to 
protect messages in transit. AES128 is the minimum strength encryption method that 
should be deployed. 
Impact: 
Organizations using SNMP can significantly reduce the risks of unauthorized access by 
using the 'snmp-server group v3 priv' setting to encrypt messages in transit. 
Audit: 
Verify the result show the appropriate group name and security model 
 
hostname#show snmp group 
Remediation: 
For each SNMPv3 group created on your router add privacy options by issuing the 
following command... 
 
hostname(config)#snmp-server group {<em>group_name</em>} v3 priv 
Default Value: 
No SNMP server groups are configured. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s5.html#GUID-56E87D02-C56F-4E2D-A5C8-617E31740C3F

Page 93 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
6.5 Require MFA for Administrative Access 
 Require MFA for all administrative access accounts, where supported, on all 
enterprise assets, whether managed on-site or through a third-party provider. 
● ● ● 
v7 
4.5 Use Multifactor Authentication For All Administrative 
Access 
 Use multi-factor authentication and encrypted channels for all administrative 
account access. 
 ● ●

Page 94 
Internal Only - General 
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' when 
using SNMPv3 (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Specify the use of a minimum of 128-bit AES algorithm for encryption when using 
SNMPv3. 
Rationale: 
SNMPv3 provides much improved security over previous versions by offering options 
for Authentication and Encryption of messages. When configuring a user for SNMPv3 
you have the option of using a range of encryption schemes, or no encryption at all, to 
protect messages in transit. AES128 is the minimum strength encryption method that 
should be deployed. 
Impact: 
Organizations using SNMP can significantly reduce the risks of unauthorized access by 
using the 'snmp-server user' setting with appropriate authentication and privacy 
protocols to encrypt messages in transit. 
Audit: 
Verify the result show the appropriate user name and security settings 
 
hostname#show snmp user 
Remediation: 
For each SNMPv3 user created on your router add privacy options by issuing the 
following command. 
 
hostname(config)#snmp-server user {user_name} {group_name} v3 auth sha 
{auth_password} priv aes 128 {priv_password} {acl_name_or_number} 
Default Value: 
SNMP username as not set by default. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/snmp/command/nm-snmp-cr-
s5.html#GUID-4EED4031-E723-4B84-9BBF-610C3CF60E31

Page 95 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
6.5 Require MFA for Administrative Access 
 Require MFA for all administrative access accounts, where supported, on all 
enterprise assets, whether managed on-site or through a third-party provider. 
● ● ● 
v7 
4.5 Use Multifactor Authentication For All Administrative 
Access 
 Use multi-factor authentication and encrypted channels for all administrative 
account access. 
 ● ● 
 
2 Control Plane 
The control plane covers monitoring, route table updates, and generally the dynamic 
operation of the router. Services, settings, and data streams that support and document 
the operation, traffic handling, and dynamic status of the router. Examples of control 
plane services include: logging (e.g. Syslog), routing protocols, status protocols like 
CDP and HSRP, network topology protocols like STP, and traffic security control 
protocols like IKE. Network control protocols like ICMP, NTP, ARP, and IGMP directed 
to or sent by the router itself also fall into this area. 
2.1 Global Service Rules 
Rules in the global service class enforce server and service controls that protect against 
attacks or expose the device to exploitation. 
2.1.1 Setup SSH 
Ensure use of SSH remote console sessions to Cisco routers.

Page 96 
Internal Only - General 
2.1.1.1 Configure Prerequisites for the SSH Service 
[This space intentionally left blank]

Page 97 
Internal Only - General 
2.1.1.1.1 Set the 'hostname' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
The hostname is used in prompts and default configuration filenames. 
Rationale: 
The domain name is prerequisite for setting up SSH. 
Impact: 
Organizations should plan the enterprise network and identify an appropriate host name 
for each router. 
Audit: 
Perform the following to determine if the local time zone is configured: 
Verify the result shows the summer-time recurrence is configured properly. 
 
hostname#sh run | incl hostname 
Remediation: 
Configure an appropriate host name for the router. 
 
hostname(config)#hostname {<em>router_name</em>} 
Default Value: 
The default hostname is Router. 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/F_through_K.html#GUID-F3349988-EC16-
484A-BE81-4C40110E6625 
CIS Controls: 
Controls Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 98 
Internal Only - General 
Controls Version Control IG 1 IG 2 IG 3 
v7 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 99 
Internal Only - General 
2.1.1.1.2 Set the 'ip domain-name' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Define a default domain name that the Cisco IOS software uses to complete unqualified 
hostnames 
Rationale: 
The domain name is a prerequisite for setting up SSH. 
Impact: 
Organizations should plan the enterprise network and identify an appropriate domain 
name for the router. 
Audit: 
Perform the following to determine if the domain name is configured: 
Verify the domain name is configured properly. 
 
hostname#sh run | incl domain-name 
Remediation: 
Configure an appropriate domain name for the router. 
 
hostname (config)#ip domain-name {<em>domain-name</em>} 
Default Value: 
No domain is set. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/ipaddr/command/ipaddr-
i3.html#GUID-A706D62B-9170-45CE-A2C2-7B2052BE2CAB 
CIS Controls: 
Controls Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 100 
Internal Only - General 
Controls Version Control IG 1 IG 2 IG 3 
v7 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 101 
Internal Only - General 
2.1.1.1.3 Set 'modulus' to greater than or equal to 2048 for 'crypto 
key generate rsa' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Use this command to generate RSA key pairs for your Cisco device. 
RSA keys are generated in pairs--one public RSA key and one private RSA key. 
Rationale: 
An RSA key pair is a prerequisite for setting up SSH and should be at least 2048 bits. 
NOTE: IOS does NOT display the modulus bit value in the Audit Procedure. 
Impact: 
Organizations should plan and implement enterprise network cryptography and 
generate an appropriate RSA key pairs, such as 'modulus', greater than or equal to 
2048. 
Audit: 
Perform the following to determine if the RSA key pair is configured: 
 
 hostname#sh crypto key mypubkey rsa 
Remediation: 
Generate an RSA key pair for the router. 
 
hostname(config)#crypto key generate rsa general-keys modulus <em>2048</em> 
Default Value: 
RSA key pairs do not exist. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/a1/sec-cr-c4.html#GUID-
2AECF701-D54A-404E-9614-D3AAB049BC13

Page 102 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
16.11 Leverage Vetted Modules or Services for Application 
Security Components 
 Leverage vetted modules or services for application security components, such 
as identity management, encryption, and auditing and logging. Using platform 
features in critical security functions will reduce developers’ workload and minimize 
the likelihood of design or implementation errors. Modern operating systems provide 
effective mechanisms for identification, authentication, and authorization and make 
those mechanisms available to applications. Use only standardized, currently 
accepted, and extensively reviewed encryption algorithms. Operating systems also 
provide mechanisms to create and maintain secure audit logs. 
 ● ● 
v7 
18.5 Use Only Standardized and Extensively Reviewed 
Encryption Algorithms 
 Use only standardized and extensively reviewed encryption algorithms. 
 ● ●

Page 103 
Internal Only - General 
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less 
(Automated) 
Profile Applicability: 
•  Level 1 
Description: 
The time interval that the router waits for the SSH client to respond before 
disconnecting an uncompleted login attempt. 
Rationale: 
This reduces the risk of an administrator leaving an authenticated session logged in for 
an extended period of time. 
Impact: 
Organizations should implement a security policy requiring minimum timeout settings for 
all network administrators and enforce the policy through the 'ip ssh timeout' command. 
Audit: 
Perform the following to determine if the SSH timeout is configured: 
Verify the timeout is configured properly. 
 
hostname#sh ip ssh 
Remediation: 
Configure the SSH timeout 
 
hostname(config)#ip ssh time-out [<em>60</em>]  
Default Value: 
SSH in not enabled by default. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i3.html#GUID-
5BAC7A2B-0A25-400F-AEE9-C22AE08513C6 
Additional Information: 
This cannot exceed 120 seconds. 
Adjusting Artifact's title and regex from 'timeout' to 'time-out'

Page 104 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.3 Configure Automatic Session Locking on Enterprise 
Assets 
 Configure automatic session locking on enterprise assets after a defined period 
of inactivity. For general purpose operating systems, the period must not exceed 
15 minutes. For mobile end-user devices, the period must not exceed 2 minutes. 
● ● ● 
v7 16.11 Lock Workstation Sessions After Inactivity 
 Automatically lock workstation sessions after a standard period of inactivity. ● ● ●

Page 105 
Internal Only - General 
2.1.1.1.5 Set maximum value for 'ip ssh authentication-retries' 
(Automated) 
Profile Applicability: 
•  Level 1 
Description: 
The number of retries before the SSH login session disconnects. 
Rationale: 
This limits the number of times an unauthorized user can attempt a password without 
having to establish a new SSH login attempt. This reduces the potential for success 
during online brute force attacks by limiting the number of login attempts per SSH 
connection. 
Impact: 
Organizations should implement a security policy limiting the number of authentication 
attempts for network administrators and enforce the policy through the 'ip ssh 
authentication-retries' command. 
Audit: 
Perform the following to determine if SSH authentication retries is configured: 
Verify the authentication retries is configured properly. 
 
hostname#sh ip ssh 
Remediation: 
Configure the SSH timeout: 3 or less 
 
hostname(config)#ip ssh authentication-retries [<em>3</em>] 
Default Value: 
SSH is not enabled by default. When set, the default value is 3. When set using the 
default value it will not display under a show running-configuration. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i3.html#GUID-
5BAC7A2B-0A25-400F-AEE9-C22AE08513C6

Page 106 
Internal Only - General 
CIS Controls: 
Controls Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v7 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 107 
Internal Only - General 
2.1.1.2 Set version 2 for 'ip ssh version' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Specify the version of Secure Shell (SSH) to be run on a router 
Rationale: 
SSH Version 1 has been subject to a number of serious vulnerabilities and is no longer 
considered to be a secure protocol, resulting in the adoption of SSH Version 2 as an 
Internet Standard in 2006. 
Cisco routers support both versions, but due to the weakness of SSH Version 1 only the 
later standard should be used. 
Impact: 
To reduce the risk of unauthorized access, organizations should implement a security 
policy to review their current protocols to ensure the most secure protocol versions are 
in use. 
Audit: 
Perform the following to determine if SSH version 2 is configured: 
Verify that SSH version 2 is configured properly. 
 
hostname#sh ip ssh 
Remediation: 
Configure the router to use SSH version 2 
 
hostname(config)#ip ssh version 2 
Default Value: 
SSH is not enabled by default. When enabled, SSH operates in compatibility mode 
(versions 1 and 2 supported). 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i3.html#GUID-
170AECF1-4B5B-462A-8CC8-999DEDC45C21

Page 108 
Internal Only - General 
CIS Controls: 
Controls Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v7 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped

Page 109 
Internal Only - General 
2.1.2 Set 'no cdp run' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Disable Cisco Discovery Protocol (CDP) service at device level. 
Rationale: 
The Cisco Discovery Protocol is a proprietary protocol that Cisco devices use to identify 
each other on a LAN segment. It is useful only in network monitoring and 
troubleshooting situations but is considered a security risk because of the amount of 
information provided from queries. In addition, there have been published denial-of-
service (DoS) attacks that use CDP. CDP should be completely disabled unless 
necessary. 
Impact: 
To reduce the risk of unauthorized access, organizations should implement a security 
policy restricting network protocols and explicitly require disabling all insecure or 
unnecessary protocols. 
Audit: 
Perform the following to determine if CDP is enabled: 
Verify the result shows "CDP is not enabled" 
 
hostname#show cdp 
Remediation: 
Disable Cisco Discovery Protocol (CDP) service globally. 
 
hostname(config)#no cdp run 
Default Value: 
Enabled on all platforms except the Cisco 10000 Series Edge Services Router 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/cdp/command/cdp-cr-
a1.html#GUID-E006FAC8-417E-4C3F-B732-4D47B0447750

Page 110 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 111 
Internal Only - General 
2.1.3 Set 'no ip bootp server' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Disable the Bootstrap Protocol (BOOTP) service on your routing device. 
Rationale: 
BootP allows a router to issue IP addresses. This should be disabled unless there is a 
specific requirement. 
Impact: 
To reduce the risk of unauthorized access, organizations should implement a security 
policy restricting network protocols and explicitly require disabling all insecure or 
unnecessary protocols such as 'ip bootp server'. 
Audit: 
Perform the following to determine if bootp is enabled: 
Verify a "no ip bootp server" result returns 
 
hostname#show run | incl bootp 
Remediation: 
Disable the bootp server. 
 
hostname(config)#ip dhcp bootp ignore 
Default Value: 
Enabled 
References: 
1. Cisco IOS software receives Cisco Discovery Protocol information 
Additional Information: 
Adjusting the Artifact's logic reversing it: check that 'ip bootp server' does not exist in the 
global config.

Page 112 
Internal Only - General 
Doing so allows CIS CAT Pro assessor to provide the right result, because CIS CAT 
Pro Assessor relies on a 'sh run' results and as 'no ip bootp server' is the default setting 
it doesn't show up in 'sh run' results. When bootp server is enabled, the line 'ip bootp 
server' exists in global config and shows up in 'sh run' results. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 113 
Internal Only - General 
2.1.4 Set 'no service dhcp' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Disable the Dynamic Host Configuration Protocol (DHCP) server and relay agent 
features on your router. 
Rationale: 
The DHCP server supplies automatic configuration parameters, such as dynamic IP 
address, to requesting systems. A dedicated server located in a secured management 
zone should be used to provide DHCP services instead. Attackers can potentially be 
used for denial-of-service (DoS) attacks. 
Impact: 
To reduce the risk of unauthorized access, organizations should implement a security 
policy restricting network protocols and explicitly require disabling all insecure or 
unnecessary protocols such as the Dynamic Host Configuration Protocol (DHCP). 
Audit: 
Perform the following to determine if the DHCP service is enabled: 
Verify no result returns 
 
hostname#show run | incl dhcp 
Remediation: 
Disable the DHCP server. 
 
hostname(config)#<strong>no service dhcp</strong> 
Default Value: 
Enabled by default, but also requires a DHCP pool to be set to activate the DHCP 
server. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/ipaddr/command/ipaddr-
r1.html#GUID-1516B259-AA28-4839-B968-8DDBF0B382F6

Page 114 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 115 
Internal Only - General 
2.1.5 Set 'service tcp-keepalives-in' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Generate keepalive packets on idle incoming network connections. 
Rationale: 
Stale connections use resources and could potentially be hijacked to gain illegitimate 
access. The TCP keepalives-in service generates keepalive packets on idle incoming 
network connections (initiated by remote host). This service allows the device to detect 
when the remote host fails and drop the session. If enabled, keepalives are sent once 
per minute on idle connections. The connection is closed within five minutes if no 
keepalives are received or immediately if the host replies with a reset packet. 
Impact: 
To reduce the risk of unauthorized access, organizations should implement a security 
policy restricting how long to allow terminated sessions and enforce this policy through 
the use of 'tcp-keepalives-in' command. 
Audit: 
Perform the following to determine if the feature is enabled: 
Verify a command string result returns 
 
hostname#show run | incl service tcp 
Remediation: 
Enable TCP keepalives-in service: 
 
hostname(config)#service tcp-keepalives-in 
Default Value: 
Disabled by default. 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/R_through_setup.html#GUID-1489ABA3-2428-
4A64-B252-296A035DB85E

Page 116 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 117 
Internal Only - General 
2.1.6 Set 'service tcp-keepalives-out' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Generate keepalive packets on idle outgoing network connections. 
Rationale: 
Stale connections use resources and could potentially be hijacked to gain illegitimate 
access. The TCP keepalives-in service generates keepalive packets on idle incoming 
network connections (initiated by remote host). This service allows the device to detect 
when the remote host fails and drop the session. If enabled, keepalives are sent once 
per minute on idle connections. The closes connection is closed within five minutes if no 
keepalives are received or immediately if the host replies with a reset packet. 
Impact: 
To reduce the risk of unauthorized access, organizations should implement a security 
policy restricting how long to allow terminated sessions and enforce this policy through 
the use of 'tcp-keepalives-out' command. 
Audit: 
Perform the following to determine if the feature is enabled: 
Verify a command string result returns 
 
hostname#show run | incl service tcp 
Remediation: 
Enable TCP keepalives-out service: 
 
hostname(config)#service tcp-keepalives-out 
Default Value: 
Disabled by default. 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/R_through_setup.html#GUID-9321ECDC-6284-
4BF6-BA4A-9CEEF5F993E5

Page 118 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 119 
Internal Only - General 
2.1.7 Set 'no service pad' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Disable X.25 Packet Assembler/Disassembler (PAD) service. 
Rationale: 
If the PAD service is not necessary, disable the service to prevent intruders from 
accessing the X.25 PAD command set on the router. 
Impact: 
To reduce the risk of unauthorized access, organizations should implement a security 
policy restricting unnecessary services such as the 'PAD' service. 
Audit: 
Perform the following to determine if the feature is disabled: 
Verify no result returns 
 
hostname#show run all| incl service pad 
Remediation: 
Disable the PAD service. 
 
hostname(config)#no service pad 
Default Value: 
Enabled by default. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/wan/command/wan-s1.html#GUID-
C5497B77-3FD4-4D2F-AB08-1317D5F5473B 
Additional Information: 
Reverting the Artifact logic will satisfy this control and will make this control independent 
of the way the assessor tool used checks the configuration (CIS CAT Pro Assessor 
check in 'sh run' results instead of 'sh run all' results)

Page 120 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 121 
Internal Only - General 
2.2 Logging Rules 
Rules in the logging class enforce controls that provide a record of system activity and 
events.

Page 122 
Internal Only - General 
2.2.1 Set 'logging enable' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Enable logging of system messages. 
Rationale: 
Logging provides a chronological record of activities on the Cisco device and allows 
monitoring of both operational and security related events. 
Impact: 
Enabling the Cisco IOS 'logging enable' command enforces the monitoring of 
technology risks for the organizations' network devices. 
Audit: 
Perform the following to determine if the feature is enabled: 
Verify no result returns 
 
hostname#show run | i logging host 
Remediation: 
Enable system logging. 
 
hostname(config)#archive 
hostname(config-archive)#log config 
hostname(config-archive-log-cfg)#logging enable 
hostname(config-archive-log-cfg)#end 
Default Value: 
Logging is not enabled/ 
References: 
1. https://community.cisco.com/t5/networking-knowledge-base/how-to-configure-
logging-in-cisco-ios/ta-p/3132434

Page 123 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 124 
Internal Only - General 
2.2.2 Set 'buffer size' for 'logging buffered' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Enable system message logging to a local buffer. 
Rationale: 
The device can copy and store log messages to an internal memory buffer. The 
buffered data is available only from a router exec or enabled exec session. This form of 
logging is useful for debugging and monitoring when logged in to a router. 
Impact: 
Data forensics is effective for managing technology risks and an organization can 
enforce such policies by enabling the 'logging buffered' command. 
Audit: 
Perform the following to determine if the feature is enabled: 
Verify a command string result returns 
 
hostname#show run | incl logging buffered 
Remediation: 
Configure buffered logging (with minimum size). Recommended size is 64000. 
 
hostname(config)#logging buffered [<em>log_buffer_size</em>] 
Default Value: 
No logging buffer is set by default 
References: 
1. http://www.cisco.com/en/US/docs/ios/netmgmt/command/reference/nm_09.html#
wp1060051

Page 125 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 126 
Internal Only - General 
2.2.3 Set 'logging console critical' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Verify logging to device console is enabled and limited to a rational severity level to 
avoid impacting system performance and management. 
Rationale: 
This configuration determines the severity of messages that will generate console 
messages. Logging to console should be limited only to those messages required for 
immediate troubleshooting while logged into the device. This form of logging is not 
persistent; messages printed to the console are not stored by the router. Console 
logging is handy for operators when they use the console. 
Impact: 
Logging critical messages at the console is important for an organization managing 
technology risk. The 'logging console' command should capture appropriate severity 
messages to be effective. 
Audit: 
Perform the following to determine if the feature is enabled: 
Verify a command string result returns 
 
hostname#show run | incl logging console 
Remediation: 
Configure console logging level. 
 
hostname(config)#logging console critical 
Default Value: 
Tthe default is to log all messages 
Additional Information: 
The console is a slow display device. In message storms some logging messages may 
be silently dropped when the console queue becomes full. Set severity levels 
accordingly.

Page 127 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 128 
Internal Only - General 
2.2.4 Set IP address for 'logging host' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Log system messages and debug output to a remote host. 
Rationale: 
Cisco routers can send their log messages to a Unix-style Syslog service. A syslog 
service simply accepts messages and stores them in files or prints them according to a 
simple configuration file. This form of logging is best because it can provide protected 
long-term storage for logs (the devices internal logging buffer has limited capacity to 
store events.) In addition, logging to an external system is highly recommended or 
required by most security standards. If desired or required by policy, law and/or 
regulation, enable a second syslog server for redundancy. 
Impact: 
Logging is an important process for an organization managing technology risk. The 
'logging host' command sets the IP address of the logging host and enforces the logging 
process. 
Audit: 
Perform the following to determine if a syslog server is enabled: 
Verify one or more IP address(es) returns 
 
hostname#sh log | incl logging host 
Remediation: 
Designate one or more syslog servers by IP address. 
 
hostname(config)#logging host {syslog_server} 
Default Value: 
System logging messages are not sent to any remote host. 
References: 
1. http://www.cisco.com/en/US/docs/ios/netmgmt/command/reference/nm_09.html#
wp1082864

Page 129 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
13.1 Centralize Security Event Alerting 
 Centralize security event alerting across enterprise assets for log correlation 
and analysis. Best practice implementation requires the use of a SIEM, which 
includes vendor-defined event correlation alerts. A log analytics platform configured 
with security-relevant correlation alerts also satisfies this Safeguard. 
 ● ● 
v8 13.11 Tune Security Event Alerting Thresholds 
 Tune security event alerting thresholds monthly, or more frequently.   ● 
v7 
6.6 Deploy SIEM or Log Analytic tool 
 Deploy Security Information and Event Management (SIEM) or log analytic tool 
for log correlation and analysis. 
 ● ● 
v7 
6.8 Regularly Tune SIEM 
 On a regular basis, tune your SIEM system to better identify actionable events 
and decrease event noise. 
  ●

Page 130 
Internal Only - General 
2.2.5 Set 'logging trap informational' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Limit messages logged to the syslog servers based on severity level informational. 
Rationale: 
This determines the severity of messages that will generate simple network 
management protocol (SNMP) trap and or syslog messages. This setting should be set 
to either "debugging" (7) or "informational" (6), but no lower. 
Impact: 
Logging is an important process for an organization managing technology risk. The 
'logging trap' command sets the severity of messages and enforces the logging 
process. 
Audit: 
Perform the following to determine if a syslog server for SNMP traps is enabled: 
Verify "level informational" returns 
 
hostname#sh log | incl trap logging 
Remediation: 
Configure SNMP trap and syslog logging level. 
 
hostname(config)#logging trap informational 
Default Value: 
Disabled 
References: 
1. http://www.cisco.com/en/US/docs/ios/netmgmt/command/reference/nm_09.html#
wp1015177

Page 131 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 132 
Internal Only - General 
2.2.6 Set 'service timestamps debug datetime' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Configure the system to apply a time stamp to debugging messages or system logging 
messages 
Rationale: 
Including timestamps in log messages allows correlating events and tracing network 
attacks across multiple devices. Enabling service timestamp to mark the time log 
messages were generated simplifies obtaining a holistic view of events enabling faster 
troubleshooting of issues or attacks. 
Impact: 
Logging is an important process for an organization managing technology risk and 
establishing a timeline of events is critical. The 'service timestamps' command sets the 
date and time on entries sent to the logging host and enforces the logging process. 
Audit: 
Perform the following to determine if the additional detail is enabled: 
Verify a command string result returns 
 
hostname#sh run | incl service timestamps 
Remediation: 
Configure debug messages to include timestamps. 
 
hostname(config)#service timestamps debug datetime {<em>msec</em>} show-
timezone 
Default Value: 
Time stamps are applied to debug and logging messages. 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/R_through_setup.html#GUID-DC110E59-D294-
4E3D-B67F-CCB06E607FC6

Page 133 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 134 
Internal Only - General 
2.2.7 Set 'logging source interface' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Specify the source IPv4 or IPv6 address of system logging packets 
Rationale: 
This is required so that the router sends log messages to the logging server from a 
consistent IP address. 
Impact: 
Logging is an important process for an organization managing technology risk and 
establishing a consistent source of messages for the logging host is critical. The 'logging 
source interface loopback' command sets a consistent IP address to send messages to 
the logging host and enforces the logging process. 
Audit: 
Perform the following to determine if logging services are bound to a source interface: 
Verify a command string result returns 
 
hostname#sh run | incl logging source 
Remediation: 
Bind logging to the loopback interface. 
 
hostname(config)#logging source-interface loopback 
{<em>loopback_interface_number</em>} 
Default Value: 
The wildcard interface address is used. 
References: 
1. http://www.cisco.com/en/US/docs/ios/netmgmt/command/reference/nm_09.html#
wp1095099

Page 135 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 136 
Internal Only - General 
2.2.8 Set 'login success/failure logging' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Without generating audit records that are specific to the security and mission needs of 
the organization, it would be difficult to establish, correlate, and investigate the events 
relating to an incident or identify those responsible for one. 
Rationale: 
Audit records can be generated from various components within the information system 
(e.g., module or policy filter). 
Audit: 
hostname(config)#sho running-config | inc login on- 
Remediation: 
hostname(config)#login on-failure log 
hostname(config)#login on-success log 
hostname(config)#end 
References: 
1. https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/config-mgmt/configuration/xe-
16-6/config-mgmt-xe-16-6-book/cm-config-logger.pdf 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.5 Collect Detailed Audit Logs 
 Configure detailed audit logging for enterprise assets containing sensitive data. 
Include event source, date, username, timestamp, source addresses, destination 
addresses, and other useful elements that could assist in a forensic investigation. 
 ● ● 
v7 
6.3 Enable Detailed Logging 
 Enable system logging to include detailed information such as an event source, 
date, user, timestamp, source addresses, destination addresses, and other useful 
elements. 
 ● ●

Page 137 
Internal Only - General 
2.3 NTP Rules 
Network Time Protocol allows administrators to set the system time on all of their 
compatible systems from a single source, ensuring a consistent time stamp for logging 
and authentication protocols. NTP is an internet standard, defined in RFC1305.

Page 138 
Internal Only - General 
2.3.1 Require Encryption Keys for NTP 
Encryption keys should be set for NTP Servers.

Page 139 
Internal Only - General 
2.3.1.1 Set 'ntp authenticate' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Enable NTP authentication. 
Rationale: 
Using authenticated NTP ensures the Cisco device only permits time updates from 
authorized NTP servers. 
Impact: 
Organizations should establish three Network Time Protocol (NTP) hosts to set 
consistent time across the enterprise. Enabling the 'ntp authenticate' command enforces 
authentication between NTP hosts. 
Audit: 
From the command prompt, execute the following commands: 
hostname#show run | include ntp  
Remediation: 
Configure NTP authentication: 
 
hostname(config)#ntp authenticate 
Default Value: 
NTP authentication is not enabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/bsm/command/bsm-cr-
n1.html#GUID-8BEBDAF4-6D03-4C3E-B8D6-6BCBC7D0F324 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.4 Standardize Time Synchronization 
 Standardize time synchronization. Configure at least two synchronized time 
sources across enterprise assets, where supported. 
 ● ●

Page 140 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v7 
6.1 Utilize Three Synchronized Time Sources 
 Use at least three synchronized time sources from which all servers and 
network devices retrieve time information on a regular basis so that timestamps 
in logs are consistent. 
 ● ●

Page 141 
Internal Only - General 
2.3.1.2 Set 'ntp authentication-key' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Define an authentication key for Network Time Protocol (NTP). 
Rationale: 
Using an authentication key provides a higher degree of security as only authenticated 
NTP servers will be able to update time for the Cisco device. 
Impact: 
Organizations should establish three Network Time Protocol (NTP) hosts to set 
consistent time across the enterprise. Enabling the 'ntp authentication-key' command 
enforces encrypted authentication between NTP hosts. 
Audit: 
From the command prompt, execute the following commands: 
 
hostname#show run | include ntp authentication-key 
Remediation: 
Configure at the NTP key ring and encryption key using the following command 
 
hostname(config)#ntp authentication-key {ntp_key_id} md5 {ntp_key_hash} 
Default Value: 
No authentication key is defined for NTP. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/bsm/command/bsm-cr-
n1.html#GUID-0435BFD1-D7D7-41D4-97AC-7731C11226BC

Page 142 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.4 Standardize Time Synchronization 
 Standardize time synchronization. Configure at least two synchronized time 
sources across enterprise assets, where supported. 
 ● ● 
v7 
6.1 Utilize Three Synchronized Time Sources 
 Use at least three synchronized time sources from which all servers and 
network devices retrieve time information on a regular basis so that timestamps 
in logs are consistent. 
 ● ●

Page 143 
Internal Only - General 
2.3.1.3 Set the 'ntp trusted-key' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Ensure you authenticate the identity of a system to which Network Time Protocol (NTP) 
will synchronize 
Rationale: 
This authentication function provides protection against accidentally synchronizing the 
system to another system that is not trusted, because the other system must know the 
correct authentication key. 
Impact: 
Organizations should establish three Network Time Protocol (NTP) hosts to set 
consistent time across the enterprise. Enabling the 'ntp trusted-key' command enforces 
encrypted authentication between NTP hosts. 
Audit: 
From the command prompt, execute the following commands: 
 
hostname#show run | include ntp trusted-key  
The above command should return any NTP server(s) configured with encryption keys. 
This value should be the same as the total number of servers configured as tested in. 
Remediation: 
Configure the NTP trusted key using the following command 
 
hostname(config)#ntp trusted-key {ntp_key_id} 
Default Value: 
Authentication of the identity of the system is disabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/bsm/command/bsm-cr-
n1.html#GUID-89CA798D-0F12-4AE8-B382-DE10CBD261DB

Page 144 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.4 Standardize Time Synchronization 
 Standardize time synchronization. Configure at least two synchronized time 
sources across enterprise assets, where supported. 
 ● ● 
v7 
6.1 Utilize Three Synchronized Time Sources 
 Use at least three synchronized time sources from which all servers and 
network devices retrieve time information on a regular basis so that timestamps 
in logs are consistent. 
 ● ●

Page 145 
Internal Only - General 
2.3.1.4 Set 'key' for each 'ntp server' (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
Specifies the authentication key for NTP. 
Rationale: 
This authentication feature provides protection against accidentally synchronizing the 
ntp system to another system that is not trusted, because the other system must know 
the correct authentication key. 
Impact: 
Organizations should establish three Network Time Protocol (NTP) hosts to set 
consistent time across the enterprise. Enabling the 'ntp server key' command enforces 
encrypted authentication between NTP hosts. 
Audit: 
From the command prompt, execute the following commands: 
 
hostname#show run | include ntp server 
Remediation: 
Configure each NTP Server to use a key ring using the following command. 
 
hostname(config)#ntp server {<em>ntp-server_ip_address</em>}{key 
<em>ntp_key_id</em>}  
Default Value: 
No NTP key is set by default 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.4 Standardize Time Synchronization 
 Standardize time synchronization. Configure at least two synchronized time 
sources across enterprise assets, where supported. 
 ● ●

Page 146 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v7 
6.1 Utilize Three Synchronized Time Sources 
 Use at least three synchronized time sources from which all servers and 
network devices retrieve time information on a regular basis so that timestamps 
in logs are consistent. 
 ● ●

Page 147 
Internal Only - General 
2.3.2 Set 'ip address' for 'ntp server' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Use this command if you want to allow the system to synchronize the system software 
clock with the specified NTP server. 
Rationale: 
To ensure that the time on your Cisco router is consistent with other devices in your 
network, at least two (and preferably at least three) NTP Server/s external to the router 
should be configured. 
Ensure you also configure consistent timezone and daylight savings time setting for all 
devices. For simplicity, the default of Coordinated Universal Time (UTC). 
Impact: 
Organizations should establish multiple Network Time Protocol (NTP) hosts to set 
consistent time across the enterprise. Enabling the 'ntp server ip address' enforces 
encrypted authentication between NTP hosts. 
Audit: 
From the command prompt, execute the following commands: 
 
hostname#sh ntp associations 
Remediation: 
Configure at least one external NTP Server using the following commands 
 
hostname(config)#ntp server {ntp-server_ip_address} 
or  
hostname(config)#ntp server {ntp server vrf [vrf name] ip address} 
Default Value: 
No servers are configured by default. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/bsm/command/bsm-cr-
n1.html#GUID-255145EB-D656-43F0-B361-D9CBCC794112 
2. https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/bsm/command/bsm-cr-
book/bsm-cr-n1.html#wp3294676008

Page 148 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.4 Standardize Time Synchronization 
 Standardize time synchronization. Configure at least two synchronized time 
sources across enterprise assets, where supported. 
 ● ● 
v7 
6.1 Utilize Three Synchronized Time Sources 
 Use at least three synchronized time sources from which all servers and 
network devices retrieve time information on a regular basis so that timestamps 
in logs are consistent. 
 ● ●

Page 149 
Internal Only - General 
2.4 Loopback Rules 
When a router needs to initiate connections to remote hosts, for example for SYSLOG 
or NTP, it will use the nearest interface for the packets source address. This can cause 
issues due to the possible variation in source, potentially causing packets to be denied 
by intervening firewalls or handled incorrectly by the receiving host. To prevent these 
problems the router should be configured with a Loopback interface and any services 
should be bound to this address.

Page 150 
Internal Only - General 
2.4.1 Create a single 'interface loopback' (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Configure a single loopback interface. 
Rationale: 
Software-only loopback interface that emulates an interface that is always up. It is a 
virtual interface supported on all platforms. 
Alternate loopback addresses create a potential for abuse, mis-configuration, and 
inconsistencies. Additional loopback interfaces must be documented and approved prior 
to use by local security personnel. 
Impact: 
Organizations should plan and establish 'loopback interfaces' for the enterprise network. 
Loopback interfaces enable critical network information such as OSPF Router IDs and 
provide termination points for routing protocol sessions. 
Audit: 
Perform the following to determine if a loopback interface is defined: 
Verify an IP address returns for the defined loopback interface 
hostname#sh ip int brief | incl Loopback 
Remediation: 
Define and configure one loopback interface. Below is an example 
hostname(config)#interface loopback 0 
hostname(config-if)#ip address 10.10.10.10 255.255.255.0 
Default Value: 
There are no loopback interfaces defined by default. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/interface/command/ir-
i1.html#GUID-0D6BDFCD-3FBB-4D26-A274-C1221F8592DF

Page 151 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 152 
Internal Only - General 
2.4.2 Set AAA 'source-interface' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Force AAA to use the IP address of a specified interface for all outgoing AAA packets 
Rationale: 
This is required so that the AAA server (RADIUS or TACACS+) can easily identify 
routers and authenticate requests by their IP address. 
Impact: 
Organizations should design and implement authentication, authorization, and 
accounting (AAA) services for effective monitoring of enterprise network devices. 
Binding AAA services to the source-interface loopback enables these services. 
Audit: 
Perform the following to determine if AAA services are bound to a source interface: 
Verify a command string result returns 
 
hostname#sh run | incl tacacs source | radius source 
Remediation: 
Bind AAA services to the loopback interface. 
 
Hostname(config)#ip radius source-interface loopback 
{loopback_interface_number} 
or 
Hostname(config)#aaa group server tacacs+ {group_name} hostname(config-sg-
tacacs+)#ip tacacs source-interface {loopback_interface_number} 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i2.html#GUID-
22E8B211-751F-48E0-9C76-58F0FE0AABA8 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i3.html#GUID-
54A00318-CF69-46FC-9ADC-313BFC436713

Page 153 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 5.6 Centralize Account Management 
 Centralize account management through a directory or identity service.  ● ● 
v7 
16.2 Configure Centralized Point of Authentication 
 Configure access for all accounts through as few centralized points of 
authentication as possible, including network, security, and cloud systems. 
 ● ●

Page 154 
Internal Only - General 
2.4.3 Set 'ntp source' to Loopback Interface (Automated) 
Profile Applicability: 
•  Level 1 
Description: 
Use a particular source address in Network Time Protocol (NTP) packets. 
Rationale: 
Set the source address to be used when sending NTP traffic. This may be required if 
the NTP servers you peer with filter based on IP address. 
Impact: 
Organizations should plan and implement network time protocol (NTP) services to 
establish official time for all enterprise network devices. Setting 'ntp source loopback' 
enforces the proper IP address for NTP services. 
Audit: 
Perform the following to determine if NTP services are bound to a source interface: 
Verify a command string result returns 
 
hostname#sh run | incl ntp source 
Remediation: 
Bind the NTP service to the loopback interface. 
 
hostname(config)#ntp source loopback {<em>loopback_interface_number}</em> 
Default Value: 
Source address is determined by the outgoing interface. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/bsm/command/bsm-cr-
n1.html#GUID-DF29FBFB-E1C0-4E5C-9013-D4CE59CA0B88

Page 155 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
8.4 Standardize Time Synchronization 
 Standardize time synchronization. Configure at least two synchronized time 
sources across enterprise assets, where supported. 
 ● ● 
v7 
6.1 Utilize Three Synchronized Time Sources 
 Use at least three synchronized time sources from which all servers and 
network devices retrieve time information on a regular basis so that timestamps 
in logs are consistent. 
 ● ●

Page 156 
Internal Only - General 
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface 
(Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Specify the IP address of an interface as the source address for TFTP connections. 
Rationale: 
This is required so that the TFTP servers can easily identify routers and authenticate 
requests by their IP address. 
Impact: 
Organizations should plan and implement trivial file transfer protocol (TFTP) services in 
the enterprise by setting 'tftp source-interface loopback', which enables the TFTP 
servers to identify routers and authenticate requests by IP address. 
Audit: 
Perform the following to determine if TFTP services are bound to a source interface: 
Verify a command string result returns 
 
hostname#sh run | incl tftp source-interface 
Remediation: 
Bind the TFTP client to the loopback interface. 
 
hostname(config)#ip tftp source-interface loopback 
{<em>loobpback_interface_number</em>} 
Default Value: 
The address of the closest interface to the destination is selected as the source 
address. 
References: 
1. http://www.cisco.com/en/US/docs/ios-
xml/ios/fundamentals/command/F_through_K.html#GUID-9AA27050-A578-
47CD-9F1D-5A8E2B449209

Page 157 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ● 
 
3 Data Plane 
Services and settings related to the data passing through the router (as opposed to 
direct to it). The data plane is for everything not in control or management planes. 
Settings on a router concerned with the data plane include interface access lists, firewall 
functionality (e.g. CBAC), NAT, and IPSec. Settings for traffic-affecting services like 
unicast RPF verification and CAR/QoS also fall into this area.

Page 158 
Internal Only - General 
3.1 Routing Rules 
Unneeded services should be disabled.

Page 159 
Internal Only - General 
3.1.1 Set 'no ip source-route' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Disable the handling of IP datagrams with source routing header options. 
Rationale: 
Source routing is a feature of IP whereby individual packets can specify routes. This 
feature is used in several kinds of attacks. Cisco routers normally accept and process 
source routes. Unless a network depends on source routing, it should be disabled. 
Impact: 
Organizations should plan and implement network policies to ensure unnecessary 
services are explicitly disabled. The 'ip source-route' feature has been used in several 
attacks and should be disabled. 
Audit: 
Verify the command string result returns 
 
hostname#sh run | incl ip source-route 
Remediation: 
Disable source routing. 
 
hostname(config)#no ip source-route 
Default Value: 
Enabled by default 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/ipaddr/command/ipaddr-
i4.html#GUID-C7F971DD-358F-4B43-9F3E-244F5D4A3A93 
Additional Information: 
Reverting the Artifact logic will satisfy this control and will make this control independent 
of the way the assessor tool used checks the configuration (CIS CAT Pro Assessor 
check in 'sh run' results instead of 'sh run all' results)

Page 160 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 161 
Internal Only - General 
3.1.2 Set 'no ip proxy-arp' (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
Disable proxy ARP on all interfaces. 
Rationale: 
Address Resolution Protocol (ARP) provides resolution between IP and MAC 
Addresses (or other Network and Link Layer addresses on none IP networks) within a 
Layer 2 network. 
Proxy ARP is a service where a device connected to one network (in this case the Cisco 
router) answers ARP Requests which are addressed to a host on another network, 
replying with its own MAC Address and forwarding the traffic on to the intended host. 
Sometimes used for extending broadcast domains across WAN links, in most cases 
Proxy ARP on enterprise networks is used to enable communication for hosts with mis-
configured subnet masks, a situation which should no longer be a common problem. 
Proxy ARP effectively breaks the LAN Security Perimeter, extending a network across 
multiple Layer 2 segments. Using Proxy ARP can also allow other security controls such 
as PVLAN to be bypassed. 
Impact: 
Organizations should plan and implement network policies to ensure unnecessary 
services are explicitly disabled. The 'ip proxy-arp' feature effectively breaks the LAN 
security perimeter and should be disabled. 
Audit: 
Verify the proxy ARP status 
 
hostname#sh ip int {<em>interface</em>} | incl proxy-arp  
Remediation: 
Disable proxy ARP on all interfaces. 
 
hostname(config)#interface {interface} 
hostname(config-if)#no ip proxy-arp 
  
Default Value: 
Enabled

Page 162 
Internal Only - General 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/ipaddr/command/ipaddr-
i4.html#GUID-AEB7DDCB-7B3D-4036-ACF0-0A0250F3002E 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 163 
Internal Only - General 
3.1.3 Set 'no interface tunnel' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Verify no tunnel interfaces are defined. 
Rationale: 
Tunnel interfaces should not exist in general. They can be used for malicious purposes. 
If they are necessary, the network admin's should be well aware of them and their 
purpose. 
Impact: 
Organizations should plan and implement enterprise network security policies that 
disable insecure and unnecessary features that increase attack surfaces such as 'tunnel 
interfaces'. 
Audit: 
Verify no tunnel interfaces are defined 
 
hostname#sh ip int brief | incl tunnel  
Remediation: 
Remove any tunnel interfaces. 
 
hostname(config)#no interface tunnel {<em>instance</em>} 
Default Value: 
No tunnel interfaces are defined 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/interface/command/ir-
i1.html#GUID-0D6BDFCD-3FBB-4D26-A274-C1221F8592DF

Page 164 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 165 
Internal Only - General 
3.1.4 Set 'ip verify unicast source reachable-via' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Examines incoming packets to determine whether the source address is in the 
Forwarding Information Base (FIB) and permits the packet only if the source is 
reachable through the interface on which the packet was received (sometimes referred 
to as strict mode). 
Rationale: 
Enabled uRPF helps mitigate IP spoofing by ensuring only packet source IP addresses 
only originate from expected interfaces. Configure unicast reverse-path forwarding 
(uRPF) on all external or high risk interfaces. 
Impact: 
Organizations should plan and implement enterprise security policies that protect the 
confidentiality, integrity, and availability of network devices. The 'unicast Reverse-Path 
Forwarding' (uRPF) feature dynamically uses the router table to either accept or drop 
packets when arriving on an interface. 
Audit: 
Verify uRPF is running on the appropriate interface(s) 
 
hostname#sh ip int {<em>interface</em>} | incl verify source  
Remediation: 
Configure uRPF. 
 
hostname(config)#interface {<em>interface_name</em>} 
hostname(config-if)#ip verify unicast source reachable-via rx allow-default 
Default Value: 
Unicast RPF is disabled. 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i3.html#GUID-
2ED313DB-3D3F-49D7-880A-047463632757 
2. https://community.cisco.com/t5/routing/ip-verify-unicast-source-reachable-via-
rx/td-p/1710172

Page 166 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 0.0 Explicitly Not Mapped 
 Explicitly Not Mapped    
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 167 
Internal Only - General 
3.2 Border Router Filtering 
A border-filtering device connects "internal" networks such as desktop networks, DMZ 
networks, etc., to "external" networks such as the Internet. If this group is chosen, then 
ingress and egress filter rules will be required.

Page 168 
Internal Only - General 
3.2.1 Set 'ip access-list extended' to Forbid Private Source 
Addresses from External Networks (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
This command places the router in access-list configuration mode, where you must 
define the denied or permitted access conditions by using the deny and permit 
commands. 
Rationale: 
Configuring access controls can help prevent spoofing attacks. To reduce the 
effectiveness of IP spoofing, configure access control to deny any traffic from the 
external network that has a source address that should reside on the internal network. 
Include local host address or any reserved private addresses (RFC 1918). 
Ensure the permit rule(s) above the final deny rule only allow traffic according to your 
organization's least privilege policy. 
Impact: 
Organizations should plan and implement enterprise security policies that explicitly 
separate internal from external networks. Adding 'ip access-list' explicitly permitting and 
denying internal and external networks enforces these policies. 
Audit: 
Verify you have the appropriate access-list definitions 
 
hostname#sh ip access-list {<em>name | number</em>} 
Remediation: 
Configure ACL for private source address restrictions from external networks.

Page 169 
Internal Only - General 
 
hostname(config)#ip access-list extended {<span><em>name | number</em>}  
</span><span>hostname(config-nacl)#deny ip 
{</span><em>internal_networks</em>} any log 
hostname(config<span>-nacl</span>)#deny ip 127.0.0.0 0.255.255.255 any log 
hostname(config<span>-nacl</span>)#deny ip 10.0.0.0 0.255.255.255 any log 
hostname(config<span>-nacl</span>)#deny ip 0.0.0.0 0.255.255.255 any log 
hostname(config<span>-nacl</span>)#deny ip 172.16.0.0 0.15.255.255 any log 
hostname(config<span>-nacl</span>)#deny ip 192.168.0.0 0.0.255.255 any log 
hostname(config<span>-nacl</span>)#deny ip 192.0.2.0 0.0.0.255 any log 
hostname(config<span>-nacl</span>)#deny ip 169.254.0.0 0.0.255.255 any log 
hostname(config<span>-nacl</span>)#deny ip 224.0.0.0 31.255.255.255 any log 
hostname(config<span>-nacl</span>)#deny ip host 255.255.255.255 any log 
hostname(config<span>-nacl</span>)#permit {protocol} {source_ip} 
{source_mask} {destination} {destination_mask} log 
hostname(config<span>-nacl</span>)#deny any any log 
hostname(config)#interface <external_<em>interface</em>> 
hostname(config-if)#access-group <<em>access-list</em>> in 
  
Default Value: 
No access list defined 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i1.html#GUID-
BD76E065-8EAC-4B32-AF25-04BA94DD2B11 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ●

Page 170 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 171 
Internal Only - General 
3.2.2 Set inbound 'ip access-group' on the External Interface 
(Manual) 
Profile Applicability: 
•  Level 2 
Description: 
This command places the router in access-list configuration mode, where you must 
define the denied or permitted access conditions by using the deny and permit 
commands. 
Rationale: 
Configuring access controls can help prevent spoofing attacks. To reduce the 
effectiveness of IP spoofing, configure access control to deny any traffic from the 
external network that has a source address that should reside on the internal network. 
Include local host address or any reserved private addresses (RFC 1918). 
Ensure the permit rule(s) above the final deny rule only allow traffic according to your 
organization's least privilege policy. 
Impact: 
Organizations should plan and implement enterprise security policies explicitly 
permitting and denying access based upon access lists. Using the 'ip access-group' 
command enforces these policies by explicitly identifying groups permitted access. 
Audit: 
Verify the access-group is applied to the appropriate interface 
 
hostname#sh run | sec interface {<em>external_interface</em>} 
Remediation: 
Apply the access-group for the external (untrusted) interface 
 
hostname(config)#interface {external_interface} 
hostname(config-if)#ip access-group {name | number} in 
Default Value: 
No access-group defined

Page 172 
Internal Only - General 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/interface/command/ir-
i1.html#GUID-0D6BDFCD-3FBB-4D26-A274-C1221F8592DF 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/security/d1/sec-cr-i1.html#GUID-
D9FE7E44-7831-4C64-ACB8-840811A0C993 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ● 
 
3.3 Neighbor Authentication 
Enable routing authentication.

Page 173 
Internal Only - General 
3.3.1 Require EIGRP Authentication if Protocol is Used 
Verify enhanced interior gateway routing protocol (EIGRP) authentication is enabled, if 
routing protocol is used, where feasible.

Page 174 
Internal Only - General 
3.3.1.1 Set 'key chain' (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
Define an authentication key chain to enable authentication for routing protocols. A key 
chain must have at least one key and can have up to 2,147,483,647 keys. 
NOTE: Only DRP Agent, EIGRP, and RIPv2 use key chains. 
Rationale: 
Routing protocols such as DRP Agent, EIGRP, and RIPv2 use key chains for 
authentication. 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using 'key chains' for routing 
protocols enforces these policies. 
Audit: 
Verify the appropriate key chain is defined 
 
hostname#sh run | sec key chain  
Remediation: 
Establish the key chain. 
 
hostname(config)#key chain {<em>key-chain_name</em>} 
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_pi/command/iri-cr-
a1.html#GUID-A62E89F5-0B8B-4CF0-B4EB-08F2762D88BB

Page 175 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 176 
Internal Only - General 
3.3.1.2 Set 'key' (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
Configure an authentication key on a key chain. 
Rationale: 
This is part of the routing authentication setup 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using 'key numbers' for key 
chains for routing protocols enforces these policies. 
Audit: 
Verify the appropriate key chain is defined 
 
hostname#sh run | sec key chain  
Remediation: 
Configure the key number. 
 
hostname(config-keychain)#key {<em>key-number</em>} 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_pi/command/iri-cr-
a1.html#GUID-3F31B2E0-0E4B-4F49-A4A8-8ADA1CA0D73F 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ●

Page 177 
Internal Only - General 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 178 
Internal Only - General 
3.3.1.3 Set 'key-string' (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
Configure the authentication string for a key. 
Rationale: 
This is part of the routing authentication setup 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using 'key strings' for key chains 
for routing protocols enforces these policies. 
Audit: 
Verify the appropriate key chain is defined 
 
hostname#sh run | sec key chain  
Remediation: 
Configure the key string. 
 
hostname(config-keychain-key)#key-string <<em>key-string</em>>  
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_pi/command/iri-cr-
a1.html#GUID-D7A8DC18-2E16-4EA5-8762-8B68B94CC43E

Page 179 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 180 
Internal Only - General 
3.3.1.4 Set 'address-family ipv4 autonomous-system' (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
Configure the EIGRP address family. 
Rationale: 
Rationale: EIGRP is a true multi-protocol routing protocol and the 'address-family' 
feature enables restriction of exchanges with specific neighbors 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using 'address-family' for EIGRP 
enforces these policies by restricting the exchanges between predefined network 
devices. 
Audit: 
Verify the appropriate address family is set 
 
hostname#sh run | sec router eigrp  
Remediation: 
Configure the EIGRP address family. 
 
hostname(config)#router eigrp <<em>virtual-instance-name</em>> 
hostname(config-router)#address-family ipv4 autonomous-system {<em>eigrp_as-
number</em>} 
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
i1.html#GUID-67388D6C-AE9C-47CA-8C35-2A2CF9FA668E 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
a1.html#GUID-C03CFC8A-3CE3-4CF9-9D65-52990DBD3377

Page 181 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 182 
Internal Only - General 
3.3.1.5 Set 'af-interface default' (Manual) 
Profile Applicability: 
•  Level 2 
Description: 
Defines user defaults to apply to EIGRP interfaces that belong to an address-family. 
Rationale: 
Part of the EIGRP address-family setup 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using 'af-interface default' for 
EIGRP interfaces enforces these policies by restricting the exchanges between 
predefined network devices. 
Audit: 
Verify the setting 
 
hostname#sh run | sec router eigrp  
Remediation: 
Configure the EIGRP address family. 
 
hostname(config)#router eigrp <<em>virtual-instance-name</em>> 
hostname(config-router)#address-family ipv4 autonomous-system {<em>eigrp_as-
number</em>} 
hostname(config-router-af)#af-interface default  
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
i1.html#GUID-67388D6C-AE9C-47CA-8C35-2A2CF9FA668E 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
a1.html#GUID-C03CFC8A-3CE3-4CF9-9D65-52990DBD3377 
3. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
a1.html#GUID-DC0EF1D3-DFD4-45DF-A553-FA432A3E7233

Page 183 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 184 
Internal Only - General 
3.3.1.6 Set 'authentication key-chain' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Configure the EIGRP address family key chain. 
Rationale: 
This is part of the EIGRP authentication configuration 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using the address-family 'key 
chain' for EIGRP enforces these policies by restricting the exchanges between 
predefined network devices. 
Audit: 
Verify the appropriate key chain is set 
 
hostname#sh run | sec router eigrp  
Remediation: 
Configure the EIGRP address family key chain. 
 
hostname(config)#router eigrp <virtual-instance-name> 
hostname(config-router)#address-family ipv4 autonomous-system {eigrp_as-
number} 
hostname(config-router-af)#af-interface {interface-name} 
hostname(config-router-af-interface)#authentication key-chain {eigrp_key-
chain_name} 
Default Value: 
No key chains are specified for EIGRP 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
i1.html#GUID-67388D6C-AE9C-47CA-8C35-2A2CF9FA668E 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
a1.html#GUID-C03CFC8A-3CE3-4CF9-9D65-52990DBD3377 
3. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
a1.html#GUID-6B6ED6A3-1AAA-4EFA-B6B8-9BF11EEC37A0

Page 185 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 186 
Internal Only - General 
3.3.1.7 Set 'authentication mode md5' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Configure authentication to prevent unapproved sources from introducing unauthorized 
or false service messages. 
Rationale: 
This is part of the EIGRP authentication configuration 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using the 'authentication mode' 
for EIGRP address-family or service-family packets enforces these policies by 
restricting the type of authentication between network devices. 
Audit: 
Verify the appropriate address family authentication mode is set 
 
hostname#sh run | sec router eigrp  
Remediation: 
Configure the EIGRP address family authentication mode. 
 
hostname(config)#router eigrp <virtual-instance-name> 
hostname(config-router)#address-family ipv4 autonomous-system {eigrp_as-
number} 
hostname(config-router-af)#af-interface {interface-name} 
hostname(config-router-af-interface)#authentication mode md5 
Default Value: 
Not defined 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
i1.html#GUID-67388D6C-AE9C-47CA-8C35-2A2CF9FA668E 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
a1.html#GUID-C03CFC8A-3CE3-4CF9-9D65-52990DBD3377 
3. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
a1.html#GUID-A29E0EF6-4CEF-40A7-9824-367939001B73

Page 187 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 188 
Internal Only - General 
3.3.1.8 Set 'ip authentication key-chain eigrp' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Specify the type of authentication used in Enhanced Interior Gateway Routing Protocol 
(EIGRP) packets per interface. 
Rationale: 
Configuring EIGRP authentication key-chain number and name to restrict packet 
exchanges between network devices. 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Configuring the interface with 'ip 
authentication key chain' for EIGRP by name and number enforces these policies by 
restricting the exchanges between network devices. 
Audit: 
Verify the appropriate key chain is set on the appropriate interface(s) 
hostname#sh ip eigrp int 
hostname#sh run int {<em>interface_name</em>} | incl key-chain  
Remediation: 
Configure the interface with the EIGRP key chain. 
 
hostname(config)#interface {<em>interface_name</em>} 
hostname(config-if)#ip authentication key-chain eigrp {<em>eigrp_as-
number</em>} {<em>eigrp_key-chain_name</em>} 
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/interface/command/ir-
i1.html#GUID-0D6BDFCD-3FBB-4D26-A274-C1221F8592DF 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
i1.html#GUID-0B344B46-5E8E-4FE2-A3E0-D92410CE5E91

Page 189 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 190 
Internal Only - General 
3.3.1.9 Set 'ip authentication mode eigrp' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Configure authentication to prevent unapproved sources from introducing unauthorized 
or false routing messages. 
Rationale: 
This is part of the EIGRP authentication configuration 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Configuring the interface with 'ip 
authentication mode' for EIGRP by number and mode enforces these policies by 
restricting the exchanges between network devices. 
Audit: 
Verify the appropriate authentication mode is set on the appropriate interface(s) 
hostname#sh ip eigrp int 
hostname#sh run int {<em>interface_name</em>} | incl authentication mode 
Remediation: 
Configure the interface with the EIGRP authentication mode. 
 
hostname(config)#interface {<em>interface_name</em>} 
hostname(config-if)#ip authentication mode eigrp {<em><span>eigrp_as-
number</span></em><span>}</span> md5 
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/interface/command/ir-
i1.html#GUID-0D6BDFCD-3FBB-4D26-A274-C1221F8592DF 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_eigrp/command/ire-
i1.html#GUID-8D1B0697-8E96-4D8A-BD20-536956D68506

Page 191 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 192 
Internal Only - General 
3.3.2 Require OSPF Authentication if Protocol is Used 
Verify open shortest path first (OSPF) authentication is enabled, where feasible.

Page 193 
Internal Only - General 
3.3.2.1 Set 'authentication message-digest' for OSPF area 
(Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Enable MD5 authentication for OSPF. 
Rationale: 
This is part of the OSPF authentication setup. 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Configuring the area 
'authentication message-digest' for OSPF enforces these policies by restricting 
exchanges between network devices. 
Audit: 
Verify message digest for OSPF is defined 
hostname#sh run | sec router ospf  
Remediation: 
Configure the Message Digest option for OSPF. 
 
hostname(config)#router ospf <<em>ospf_process-id</em>> 
hostname(config-router)#area <<em>ospf_area-id</em>> authentication message-
digest  
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_ospf/command/ospf-
i1.html#GUID-3D5781A3-F8DF-4760-A551-6A3AB80A42ED 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_ospf/command/ospf-
a1.html#GUID-81D0F753-D8D5-494E-9A10-B15433CFD445

Page 194 
Internal Only - General 
Additional Information: 
The authentication type must be the same for all routers and access servers in an area. 
The authentication password for all OSPF routers on a network must be the same if 
they are to communicate with each other via OSPF 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 195 
Internal Only - General 
3.3.2.2 Set 'ip ospf message-digest-key md5' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Enable Open Shortest Path First (OSPF) Message Digest 5 (MD5) authentication. 
Rationale: 
This is part of the OSPF authentication setup 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Configuring the proper 
interface(s) for 'ip ospf message-digest-key md5' enforces these policies by restricting 
exchanges between network devices. 
Audit: 
Verify the appropriate md5 key is defined on the appropriate interface(s) 
 
hostname#sh run int {<em>interface</em>} 
Remediation: 
Configure the appropriate interface(s) for Message Digest authentication 
 
hostname(config)#interface {<em>interface_name</em>} 
hostname(config-if)#ip ospf message-digest-key {<em>ospf_md5_key-id</em>} md5 
{<em>ospf_md5_key</em>} 
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/interface/command/ir-
i1.html#GUID-0D6BDFCD-3FBB-4D26-A274-C1221F8592DF 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_ospf/command/ospf-
i1.html#GUID-939C79FF-8C09-4D5A-AEB5-DAF25038CA18

Page 196 
Internal Only - General 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 197 
Internal Only - General 
3.3.3 Require BGP Authentication if Protocol is Used 
Border Gateway Protocol (BGP)is a path vector protocol used for interior and exterior 
gateway routing on some networks. 
BGP is a complex protocol, with many configuration options which may have effects 
which are not immediately obvious. 
Verify Border Gateway Protocol (BGP) authentication is enabled, if routing protocol is 
used, where feasible.

Page 198 
Internal Only - General 
3.3.3.1 Set 'neighbor password' (Manual) 
Profile Applicability: 
•  Level 1 
Description: 
Enable message digest5 (MD5) authentication on a TCP connection between two BGP 
peers 
Rationale: 
Enforcing routing authentication reduces the likelihood of routing poisoning and 
unauthorized routers from joining BGP routing. 
Impact: 
Organizations should plan and implement enterprise security policies that require 
rigorous authentication methods for routing protocols. Using the 'neighbor password' for 
BGP enforces these policies by restricting the type of authentication between network 
devices. 
Audit: 
Verify you see the appropriate neighbor password is defined: 
 
hostname#sh run | sec router bgp  
Remediation: 
Configure BGP neighbor authentication where feasible. 
 
hostname(config)#router bgp <<em>bgp_as-number</em>> 
hostname(config-router)#neighbor <<em>bgp_neighbor-ip</em> | <em>peer-group-
name</em>> password <<em>password</em>>  
Default Value: 
Not set 
References: 
1. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_bgp/command/bgp-
n1.html#GUID-A8900842-ECF3-42D3-B188-921BE0EC060B 
2. http://www.cisco.com/en/US/docs/ios-xml/ios/iproute_bgp/command/bgp-
m1.html#GUID-159A8006-F0DF-4B82-BB71-C39D2C134205

Page 199 
Internal Only - General 
Additional Information: 
MD5 authentication between two BGP peers, meaning that each segment sent on the 
TCP connection between the peers is verified. MD5 authentication must be configured 
with the same password on both BGP peers. 
CIS Controls: 
Controls 
Version Control IG 1 IG 2 IG 3 
v8 
4.2 Establish and Maintain a Secure Configuration 
Process for Network Infrastructure 
 Establish and maintain a secure configuration process for network devices. 
Review and update documentation annually, or when significant enterprise 
changes occur that could impact this Safeguard. 
● ● ● 
v8 
4.4 Implement and Manage a Firewall on Servers 
 Implement and manage a firewall on servers, where supported. Example 
implementations include a virtual firewall, operating system firewall, or a third-
party firewall agent. 
● ● ● 
v8 
4.5 Implement and Manage a Firewall on End-User 
Devices 
 Implement and manage a host-based firewall or port-filtering tool on end-user 
devices, with a default-deny rule that drops all traffic except those services and 
ports that are explicitly allowed. 
● ● ● 
v7 
9.2 Ensure Only Approved Ports, Protocols and Services 
Are Running 
 Ensure that only network ports, protocols, and services listening on a system 
with validated business needs, are running on each system. 
 ● ●

Page 200 
Internal Only - General 
Appendix: Summary Table 
CIS Benchmark Recommendation Set 
Correctly 
Yes No 
1 Management Plane 
1.1 Local Authentication, Authorization and Accounting (AAA) Rules 
1.1.1 Enable 'aaa new-model' (Automated)   
1.1.2 Enable 'aaa authentication login' (Automated)   
1.1.3 Enable 'aaa authentication enable default' (Automated)   
1.1.4 Set 'login authentication for 'line vty' (Automated)   
1.1.5 Set 'login authentication for 'ip http' (Automated)   
1.1.6 Set 'aaa accounting' to log all privileged use commands 
using 'commands 15' (Automated) 
  
1.1.7 Set 'aaa accounting connection' (Automated)   
1.1.8 Set 'aaa accounting exec' (Automated)   
1.1.9 Set 'aaa accounting network' (Automated)   
1.1.10 Set 'aaa accounting system' (Automated)   
1.2 Access Rules 
1.2.1 Set 'privilege 1' for local users (Manual)   
1.2.2 Set 'transport input ssh' for 'line vty' connections 
(Automated) 
  
1.2.3 Set 'no exec' for 'line aux 0' (Automated)   
1.2.4 Create 'access-list' for use with 'line vty' (Manual)   
1.2.5 Set 'access-class' for 'line vty' (Automated)  

Page 201 
Internal Only - General 
CIS Benchmark Recommendation Set 
Correctly 
Yes No 
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0' (Automated) 
  
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 
'line console 0' (Automated) 
  
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 
'line vty' (Automated) 
  
1.2.9 Set 'http Secure-server' limit (Automated)   
1.2.10 Set 'exec-timeout' to less than or equal to 10 min on 'ip 
http' (Automated) 
  
1.3 Banner Rules 
1.3.1 Set the 'banner-text' for 'banner exec' (Automated)   
1.3.2 Set the 'banner-text' for 'banner login' (Automated)   
1.3.3 Set the 'banner-text' for 'banner motd' (Automated)   
1.3.4 Set the 'banner-text' for 'webauth banner' (Automated)   
1.4 Password Rules 
1.4.1 Set 'password' for 'enable secret' (Automated)   
1.4.2 Enable 'service password-encryption' (Automated)   
1.4.3 Set 'username secret' for all local users (Automated)   
1.5 SNMP Rules 
1.5.1 Set 'no snmp-server' to disable SNMP when unused 
(Manual) 
  
1.5.2 Unset 'private' for 'snmp-server community' (Automated)   
1.5.3 Unset 'public' for 'snmp-server community' (Automated)  

Page 202 
Internal Only - General 
CIS Benchmark Recommendation Set 
Correctly 
Yes No 
1.5.4 Do not set 'RW' for any 'snmp-server community' 
(Manual) 
  
1.5.5 Set the ACL for each 'snmp-server community' (Manual)   
1.5.6 Create an 'access-list' for use with SNMP (Manual)   
1.5.7 Set 'snmp-server host' when using SNMP (Automated)   
1.5.8 Set 'snmp-server enable traps snmp' (Automated)   
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3 
(Automated) 
  
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' 
when using SNMPv3 (Manual) 
  
2 Control Plane 
2.1 Global Service Rules 
2.1.1 Setup SSH 
2.1.1.1 Configure Prerequisites for the SSH Service 
2.1.1.1.1 Set the 'hostname' (Automated)   
2.1.1.1.2 Set the 'ip domain-name' (Automated)   
2.1.1.1.3 Set 'modulus' to greater than or equal to 2048 for 'crypto 
key generate rsa' (Manual) 
  
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less 
(Automated) 
  
2.1.1.1.5 Set maximum value for 'ip ssh authentication-retries' 
(Automated) 
  
2.1.1.2 Set version 2 for 'ip ssh version' (Manual)   
2.1.2 Set 'no cdp run' (Manual)  

Page 203 
Internal Only - General 
CIS Benchmark Recommendation Set 
Correctly 
Yes No 
2.1.3 Set 'no ip bootp server' (Manual)   
2.1.4 Set 'no service dhcp' (Automated)   
2.1.5 Set 'service tcp-keepalives-in' (Automated)   
2.1.6 Set 'service tcp-keepalives-out' (Automated)   
2.1.7 Set 'no service pad' (Manual)   
2.2 Logging Rules 
2.2.1 Set 'logging enable' (Automated)   
2.2.2 Set 'buffer size' for 'logging buffered' (Automated)   
2.2.3 Set 'logging console critical' (Automated)   
2.2.4 Set IP address for 'logging host' (Automated)   
2.2.5 Set 'logging trap informational' (Automated)   
2.2.6 Set 'service timestamps debug datetime' (Automated)   
2.2.7 Set 'logging source interface' (Automated)   
2.2.8 Set 'login success/failure logging' (Automated)   
2.3 NTP Rules 
2.3.1 Require Encryption Keys for NTP 
2.3.1.1 Set 'ntp authenticate' (Automated)   
2.3.1.2 Set 'ntp authentication-key' (Automated)   
2.3.1.3 Set the 'ntp trusted-key' (Automated)   
2.3.1.4 Set 'key' for each 'ntp server' (Manual)   
2.3.2 Set 'ip address' for 'ntp server' (Automated)   
2.4 Loopback Rules

Page 204 
Internal Only - General 
CIS Benchmark Recommendation Set 
Correctly 
Yes No 
2.4.1 Create a single 'interface loopback' (Automated)   
2.4.2 Set AAA 'source-interface' (Manual)   
2.4.3 Set 'ntp source' to Loopback Interface (Automated)   
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface 
(Manual) 
  
3 Data Plane 
3.1 Routing Rules 
3.1.1 Set 'no ip source-route' (Manual)   
3.1.2 Set 'no ip proxy-arp' (Manual)   
3.1.3 Set 'no interface tunnel' (Manual)   
3.1.4 Set 'ip verify unicast source reachable-via' (Manual)   
3.2 Border Router Filtering 
3.2.1 Set 'ip access-list extended' to Forbid Private Source 
Addresses from External Networks (Manual) 
  
3.2.2 Set inbound 'ip access-group' on the External Interface 
(Manual) 
  
3.3 Neighbor Authentication 
3.3.1 Require EIGRP Authentication if Protocol is Used 
3.3.1.1 Set 'key chain' (Manual)   
3.3.1.2 Set 'key' (Manual)   
3.3.1.3 Set 'key-string' (Manual)   
3.3.1.4 Set 'address-family ipv4 autonomous-system' (Manual)   
3.3.1.5 Set 'af-interface default' (Manual)  

Page 205 
Internal Only - General 
CIS Benchmark Recommendation Set 
Correctly 
Yes No 
3.3.1.6 Set 'authentication key-chain' (Manual)   
3.3.1.7 Set 'authentication mode md5' (Manual)   
3.3.1.8 Set 'ip authentication key-chain eigrp' (Manual)   
3.3.1.9 Set 'ip authentication mode eigrp' (Manual)   
3.3.2 Require OSPF Authentication if Protocol is Used 
3.3.2.1 Set 'authentication message-digest' for OSPF area 
(Manual) 
  
3.3.2.2 Set 'ip ospf message-digest-key md5' (Manual)   
3.3.3 Require BGP Authentication if Protocol is Used 
3.3.3.1 Set 'neighbor password' (Manual)  

Page 206 
Internal Only - General 
Appendix: CIS Controls v7 IG 1 Mapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0'   
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 
'line console 0'   
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 
'line vty'   
1.3.1 Set the 'banner-text' for 'banner exec'   
1.3.2 Set the 'banner-text' for 'banner login'   
1.3.3 Set the 'banner-text' for 'banner motd'   
1.3.4 Set the 'banner-text' for 'webauth banner'   
1.4.1 Set 'password' for 'enable secret'   
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less  

Page 207 
Internal Only - General 
Appendix: CIS Controls v7 IG 2 Mapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
1.1.1 Enable 'aaa new-model'   
1.1.2 Enable 'aaa authentication login'   
1.1.3 Enable 'aaa authentication enable default'   
1.1.4 Set 'login authentication for 'line vty'   
1.1.5 Set 'login authentication for 'ip http'   
1.1.6 Set 'aaa accounting' to log all privileged use commands 
using 'commands 15'   
1.1.7 Set 'aaa accounting connection'   
1.1.8 Set 'aaa accounting exec'   
1.1.9 Set 'aaa accounting network'   
1.1.10 Set 'aaa accounting system'   
1.2.2 Set 'transport input ssh' for 'line vty' connections   
1.2.3 Set 'no exec' for 'line aux 0'   
1.2.4 Create 'access-list' for use with 'line vty'   
1.2.5 Set 'access-class' for 'line vty'   
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0'   
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 
'line console 0'   
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 
'line vty'   
1.3.1 Set the 'banner-text' for 'banner exec'   
1.3.2 Set the 'banner-text' for 'banner login'   
1.3.3 Set the 'banner-text' for 'banner motd'   
1.3.4 Set the 'banner-text' for 'webauth banner'   
1.4.1 Set 'password' for 'enable secret'   
1.4.2 Enable 'service password-encryption'   
1.4.3 Set 'username secret' for all local users  

Page 208 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
1.5.1 Set 'no snmp-server' to disable SNMP when unused   
1.5.2 Unset 'private' for 'snmp-server community'   
1.5.3 Unset 'public' for 'snmp-server community'   
1.5.4 Do not set 'RW' for any 'snmp-server community'   
1.5.5 Set the ACL for each 'snmp-server community'   
1.5.6 Create an 'access-list' for use with SNMP   
1.5.7 Set 'snmp-server host' when using SNMP   
1.5.8 Set 'snmp-server enable traps snmp'   
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3   
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' 
when using SNMPv3   
2.1.1.1.3 Set 'modulus' to greater than or equal to 2048 for 'crypto 
key generate rsa'   
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less   
2.1.2 Set 'no cdp run'   
2.1.3 Set 'no ip bootp server'   
2.1.4 Set 'no service dhcp'   
2.1.5 Set 'service tcp-keepalives-in'   
2.1.6 Set 'service tcp-keepalives-out'   
2.1.7 Set 'no service pad'   
2.2.1 Set 'logging enable'   
2.2.2 Set 'buffer size' for 'logging buffered'   
2.2.3 Set 'logging console critical'   
2.2.4 Set IP address for 'logging host'   
2.2.5 Set 'logging trap informational'   
2.2.6 Set 'service timestamps debug datetime'   
2.2.7 Set 'logging source interface'   
2.2.8 Set 'login success/failure logging'   
2.3.1.1 Set 'ntp authenticate'   
2.3.1.2 Set 'ntp authentication-key'   
2.3.1.3 Set the 'ntp trusted-key'   
2.3.1.4 Set 'key' for each 'ntp server'  

Page 209 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
2.3.2 Set 'ip address' for 'ntp server'   
2.4.1 Create a single 'interface loopback'   
2.4.2 Set AAA 'source-interface'   
2.4.3 Set 'ntp source' to Loopback Interface   
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface   
3.1.1 Set 'no ip source-route'   
3.1.2 Set 'no ip proxy-arp'   
3.1.3 Set 'no interface tunnel'   
3.1.4 Set 'ip verify unicast source reachable-via'   
3.2.1 Set 'ip access-list extended' to Forbid Private Source 
Addresses from External Networks   
3.2.2 Set inbound 'ip access-group' on the External Interface   
3.3.1.1 Set 'key chain'   
3.3.1.2 Set 'key'   
3.3.1.3 Set 'key-string'   
3.3.1.4 Set 'address-family ipv4 autonomous-system'   
3.3.1.5 Set 'af-interface default'   
3.3.1.6 Set 'authentication key-chain'   
3.3.1.7 Set 'authentication mode md5'   
3.3.1.8 Set 'ip authentication key-chain eigrp'   
3.3.1.9 Set 'ip authentication mode eigrp'   
3.3.2.1 Set 'authentication message-digest' for OSPF area   
3.3.2.2 Set 'ip ospf message-digest-key md5'   
3.3.3.1 Set 'neighbor password'  

Page 210 
Internal Only - General 
Appendix: CIS Controls v7 IG 3 Mapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
1.1.1 Enable 'aaa new-model'   
1.1.2 Enable 'aaa authentication login'   
1.1.3 Enable 'aaa authentication enable default'   
1.1.4 Set 'login authentication for 'line vty'   
1.1.5 Set 'login authentication for 'ip http'   
1.1.6 Set 'aaa accounting' to log all privileged use commands 
using 'commands 15'   
1.1.7 Set 'aaa accounting connection'   
1.1.8 Set 'aaa accounting exec'   
1.1.9 Set 'aaa accounting network'   
1.1.10 Set 'aaa accounting system'   
1.2.2 Set 'transport input ssh' for 'line vty' connections   
1.2.3 Set 'no exec' for 'line aux 0'   
1.2.4 Create 'access-list' for use with 'line vty'   
1.2.5 Set 'access-class' for 'line vty'   
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0'   
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 
'line console 0'   
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 
'line vty'   
1.3.1 Set the 'banner-text' for 'banner exec'   
1.3.2 Set the 'banner-text' for 'banner login'   
1.3.3 Set the 'banner-text' for 'banner motd'   
1.3.4 Set the 'banner-text' for 'webauth banner'   
1.4.1 Set 'password' for 'enable secret'   
1.4.2 Enable 'service password-encryption'   
1.4.3 Set 'username secret' for all local users  

Page 211 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
1.5.1 Set 'no snmp-server' to disable SNMP when unused   
1.5.2 Unset 'private' for 'snmp-server community'   
1.5.3 Unset 'public' for 'snmp-server community'   
1.5.4 Do not set 'RW' for any 'snmp-server community'   
1.5.5 Set the ACL for each 'snmp-server community'   
1.5.6 Create an 'access-list' for use with SNMP   
1.5.7 Set 'snmp-server host' when using SNMP   
1.5.8 Set 'snmp-server enable traps snmp'   
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3   
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' 
when using SNMPv3   
2.1.1.1.3 Set 'modulus' to greater than or equal to 2048 for 'crypto 
key generate rsa'   
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less   
2.1.2 Set 'no cdp run'   
2.1.3 Set 'no ip bootp server'   
2.1.4 Set 'no service dhcp'   
2.1.5 Set 'service tcp-keepalives-in'   
2.1.6 Set 'service tcp-keepalives-out'   
2.1.7 Set 'no service pad'   
2.2.1 Set 'logging enable'   
2.2.2 Set 'buffer size' for 'logging buffered'   
2.2.3 Set 'logging console critical'   
2.2.4 Set IP address for 'logging host'   
2.2.5 Set 'logging trap informational'   
2.2.6 Set 'service timestamps debug datetime'   
2.2.7 Set 'logging source interface'   
2.2.8 Set 'login success/failure logging'   
2.3.1.1 Set 'ntp authenticate'   
2.3.1.2 Set 'ntp authentication-key'   
2.3.1.3 Set the 'ntp trusted-key'   
2.3.1.4 Set 'key' for each 'ntp server'  

Page 212 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
2.3.2 Set 'ip address' for 'ntp server'   
2.4.1 Create a single 'interface loopback'   
2.4.2 Set AAA 'source-interface'   
2.4.3 Set 'ntp source' to Loopback Interface   
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface   
3.1.1 Set 'no ip source-route'   
3.1.2 Set 'no ip proxy-arp'   
3.1.3 Set 'no interface tunnel'   
3.1.4 Set 'ip verify unicast source reachable-via'   
3.2.1 Set 'ip access-list extended' to Forbid Private Source 
Addresses from External Networks   
3.2.2 Set inbound 'ip access-group' on the External Interface   
3.3.1.1 Set 'key chain'   
3.3.1.2 Set 'key'   
3.3.1.3 Set 'key-string'   
3.3.1.4 Set 'address-family ipv4 autonomous-system'   
3.3.1.5 Set 'af-interface default'   
3.3.1.6 Set 'authentication key-chain'   
3.3.1.7 Set 'authentication mode md5'   
3.3.1.8 Set 'ip authentication key-chain eigrp'   
3.3.1.9 Set 'ip authentication mode eigrp'   
3.3.2.1 Set 'authentication message-digest' for OSPF area   
3.3.2.2 Set 'ip ospf message-digest-key md5'   
3.3.3.1 Set 'neighbor password'  

Page 213 
Internal Only - General 
Appendix: CIS Controls v7 Unmapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
 No unmapped recommendations to CIS Controls v7  

Page 214 
Internal Only - General 
Appendix: CIS Controls v8 IG 1 Mapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
1.1.6 Set 'aaa accounting' to log all privileged use commands 
using 'commands 15'   
1.1.8 Set 'aaa accounting exec'   
1.1.9 Set 'aaa accounting network'   
1.1.10 Set 'aaa accounting system'   
1.2.2 Set 'transport input ssh' for 'line vty' connections   
1.2.3 Set 'no exec' for 'line aux 0'   
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0'   
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 
'line console 0'   
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 
'line vty'   
1.3.1 Set the 'banner-text' for 'banner exec'   
1.3.2 Set the 'banner-text' for 'banner login'   
1.3.3 Set the 'banner-text' for 'banner motd'   
1.3.4 Set the 'banner-text' for 'webauth banner'   
1.4.1 Set 'password' for 'enable secret'   
1.5.1 Set 'no snmp-server' to disable SNMP when unused   
1.5.2 Unset 'private' for 'snmp-server community'   
1.5.3 Unset 'public' for 'snmp-server community'   
1.5.4 Do not set 'RW' for any 'snmp-server community'   
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3   
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' 
when using SNMPv3   
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less   
2.1.2 Set 'no cdp run'   
2.1.3 Set 'no ip bootp server'  

Page 215 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
2.1.4 Set 'no service dhcp'   
2.1.5 Set 'service tcp-keepalives-in'   
2.1.6 Set 'service tcp-keepalives-out'   
2.1.7 Set 'no service pad'   
2.4.1 Create a single 'interface loopback'   
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface   
3.1.1 Set 'no ip source-route'   
3.1.2 Set 'no ip proxy-arp'   
3.1.3 Set 'no interface tunnel'   
3.1.4 Set 'ip verify unicast source reachable-via'   
3.2.1 Set 'ip access-list extended' to Forbid Private Source 
Addresses from External Networks   
3.2.2 Set inbound 'ip access-group' on the External Interface   
3.3.1.1 Set 'key chain'   
3.3.1.2 Set 'key'   
3.3.1.3 Set 'key-string'   
3.3.1.4 Set 'address-family ipv4 autonomous-system'   
3.3.1.5 Set 'af-interface default'   
3.3.1.6 Set 'authentication key-chain'   
3.3.1.7 Set 'authentication mode md5'   
3.3.1.8 Set 'ip authentication key-chain eigrp'   
3.3.1.9 Set 'ip authentication mode eigrp'   
3.3.2.1 Set 'authentication message-digest' for OSPF area   
3.3.2.2 Set 'ip ospf message-digest-key md5'   
3.3.3.1 Set 'neighbor password'  

Page 216 
Internal Only - General 
Appendix: CIS Controls v8 IG 2 Mapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
1.1.1 Enable 'aaa new-model'   
1.1.2 Enable 'aaa authentication login'   
1.1.3 Enable 'aaa authentication enable default'   
1.1.4 Set 'login authentication for 'line vty'   
1.1.5 Set 'login authentication for 'ip http'   
1.1.6 Set 'aaa accounting' to log all privileged use commands 
using 'commands 15'   
1.1.7 Set 'aaa accounting connection'   
1.1.8 Set 'aaa accounting exec'   
1.1.9 Set 'aaa accounting network'   
1.1.10 Set 'aaa accounting system'   
1.2.2 Set 'transport input ssh' for 'line vty' connections   
1.2.3 Set 'no exec' for 'line aux 0'   
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0'   
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 
'line console 0'   
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 
'line vty'   
1.3.1 Set the 'banner-text' for 'banner exec'   
1.3.2 Set the 'banner-text' for 'banner login'   
1.3.3 Set the 'banner-text' for 'banner motd'   
1.3.4 Set the 'banner-text' for 'webauth banner'   
1.4.1 Set 'password' for 'enable secret'   
1.4.2 Enable 'service password-encryption'   
1.4.3 Set 'username secret' for all local users   
1.5.1 Set 'no snmp-server' to disable SNMP when unused   
1.5.2 Unset 'private' for 'snmp-server community'  

Page 217 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
1.5.3 Unset 'public' for 'snmp-server community'   
1.5.4 Do not set 'RW' for any 'snmp-server community'   
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3   
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' 
when using SNMPv3   
2.1.1.1.3 Set 'modulus' to greater than or equal to 2048 for 'crypto 
key generate rsa'   
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less   
2.1.2 Set 'no cdp run'   
2.1.3 Set 'no ip bootp server'   
2.1.4 Set 'no service dhcp'   
2.1.5 Set 'service tcp-keepalives-in'   
2.1.6 Set 'service tcp-keepalives-out'   
2.1.7 Set 'no service pad'   
2.2.1 Set 'logging enable'   
2.2.2 Set 'buffer size' for 'logging buffered'   
2.2.3 Set 'logging console critical'   
2.2.4 Set IP address for 'logging host'   
2.2.5 Set 'logging trap informational'   
2.2.6 Set 'service timestamps debug datetime'   
2.2.7 Set 'logging source interface'   
2.2.8 Set 'login success/failure logging'   
2.3.1.1 Set 'ntp authenticate'   
2.3.1.2 Set 'ntp authentication-key'   
2.3.1.3 Set the 'ntp trusted-key'   
2.3.1.4 Set 'key' for each 'ntp server'   
2.3.2 Set 'ip address' for 'ntp server'   
2.4.1 Create a single 'interface loopback'   
2.4.2 Set AAA 'source-interface'   
2.4.3 Set 'ntp source' to Loopback Interface   
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface   
3.1.1 Set 'no ip source-route'  

Page 218 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
3.1.2 Set 'no ip proxy-arp'   
3.1.3 Set 'no interface tunnel'   
3.1.4 Set 'ip verify unicast source reachable-via'   
3.2.1 Set 'ip access-list extended' to Forbid Private Source 
Addresses from External Networks   
3.2.2 Set inbound 'ip access-group' on the External Interface   
3.3.1.1 Set 'key chain'   
3.3.1.2 Set 'key'   
3.3.1.3 Set 'key-string'   
3.3.1.4 Set 'address-family ipv4 autonomous-system'   
3.3.1.5 Set 'af-interface default'   
3.3.1.6 Set 'authentication key-chain'   
3.3.1.7 Set 'authentication mode md5'   
3.3.1.8 Set 'ip authentication key-chain eigrp'   
3.3.1.9 Set 'ip authentication mode eigrp'   
3.3.2.1 Set 'authentication message-digest' for OSPF area   
3.3.2.2 Set 'ip ospf message-digest-key md5'   
3.3.3.1 Set 'neighbor password'  

Page 219 
Internal Only - General 
Appendix: CIS Controls v8 IG 3 Mapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
1.1.1 Enable 'aaa new-model'   
1.1.2 Enable 'aaa authentication login'   
1.1.3 Enable 'aaa authentication enable default'   
1.1.4 Set 'login authentication for 'line vty'   
1.1.5 Set 'login authentication for 'ip http'   
1.1.6 Set 'aaa accounting' to log all privileged use commands 
using 'commands 15'   
1.1.7 Set 'aaa accounting connection'   
1.1.8 Set 'aaa accounting exec'   
1.1.9 Set 'aaa accounting network'   
1.1.10 Set 'aaa accounting system'   
1.2.2 Set 'transport input ssh' for 'line vty' connections   
1.2.3 Set 'no exec' for 'line aux 0'   
1.2.4 Create 'access-list' for use with 'line vty'   
1.2.5 Set 'access-class' for 'line vty'   
1.2.6 Set 'exec-timeout' to less than or equal to 10 minutes for 
'line aux 0'   
1.2.7 Set 'exec-timeout' to less than or equal to 10 minutes 
'line console 0'   
1.2.8 Set 'exec-timeout' to less than or equal to 10 minutes 
'line vty'   
1.3.1 Set the 'banner-text' for 'banner exec'   
1.3.2 Set the 'banner-text' for 'banner login'   
1.3.3 Set the 'banner-text' for 'banner motd'   
1.3.4 Set the 'banner-text' for 'webauth banner'   
1.4.1 Set 'password' for 'enable secret'   
1.4.2 Enable 'service password-encryption'   
1.4.3 Set 'username secret' for all local users  

Page 220 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
1.5.1 Set 'no snmp-server' to disable SNMP when unused   
1.5.2 Unset 'private' for 'snmp-server community'   
1.5.3 Unset 'public' for 'snmp-server community'   
1.5.4 Do not set 'RW' for any 'snmp-server community'   
1.5.5 Set the ACL for each 'snmp-server community'   
1.5.6 Create an 'access-list' for use with SNMP   
1.5.7 Set 'snmp-server host' when using SNMP   
1.5.8 Set 'snmp-server enable traps snmp'   
1.5.9 Set 'priv' for each 'snmp-server group' using SNMPv3   
1.5.10 Require 'aes 128' as minimum for 'snmp-server user' 
when using SNMPv3   
2.1.1.1.3 Set 'modulus' to greater than or equal to 2048 for 'crypto 
key generate rsa'   
2.1.1.1.4 Set 'seconds' for 'ip ssh timeout' for 60 seconds or less   
2.1.2 Set 'no cdp run'   
2.1.3 Set 'no ip bootp server'   
2.1.4 Set 'no service dhcp'   
2.1.5 Set 'service tcp-keepalives-in'   
2.1.6 Set 'service tcp-keepalives-out'   
2.1.7 Set 'no service pad'   
2.2.1 Set 'logging enable'   
2.2.2 Set 'buffer size' for 'logging buffered'   
2.2.3 Set 'logging console critical'   
2.2.4 Set IP address for 'logging host'   
2.2.5 Set 'logging trap informational'   
2.2.6 Set 'service timestamps debug datetime'   
2.2.7 Set 'logging source interface'   
2.2.8 Set 'login success/failure logging'   
2.3.1.1 Set 'ntp authenticate'   
2.3.1.2 Set 'ntp authentication-key'   
2.3.1.3 Set the 'ntp trusted-key'   
2.3.1.4 Set 'key' for each 'ntp server'  

Page 221 
Internal Only - General 
Recommendation Set 
Correctly 
Yes No 
2.3.2 Set 'ip address' for 'ntp server'   
2.4.1 Create a single 'interface loopback'   
2.4.2 Set AAA 'source-interface'   
2.4.3 Set 'ntp source' to Loopback Interface   
2.4.4 Set 'ip tftp source-interface' to the Loopback Interface   
3.1.1 Set 'no ip source-route'   
3.1.2 Set 'no ip proxy-arp'   
3.1.3 Set 'no interface tunnel'   
3.1.4 Set 'ip verify unicast source reachable-via'   
3.2.1 Set 'ip access-list extended' to Forbid Private Source 
Addresses from External Networks   
3.2.2 Set inbound 'ip access-group' on the External Interface   
3.3.1.1 Set 'key chain'   
3.3.1.2 Set 'key'   
3.3.1.3 Set 'key-string'   
3.3.1.4 Set 'address-family ipv4 autonomous-system'   
3.3.1.5 Set 'af-interface default'   
3.3.1.6 Set 'authentication key-chain'   
3.3.1.7 Set 'authentication mode md5'   
3.3.1.8 Set 'ip authentication key-chain eigrp'   
3.3.1.9 Set 'ip authentication mode eigrp'   
3.3.2.1 Set 'authentication message-digest' for OSPF area   
3.3.2.2 Set 'ip ospf message-digest-key md5'   
3.3.3.1 Set 'neighbor password'  

Page 222 
Internal Only - General 
Appendix: CIS Controls v8 Unmapped 
Recommendations 
Recommendation Set 
Correctly 
Yes No 
 No unmapped recommendations to CIS Controls v8  

Page 223 
Internal Only - General 
Appendix: Change History 
Date Version Changes for this version 
Oct 29, 2024 2.2.1 Benchmark Cisco IOS XE 
17 - Assessment failing (ip 
ssh authenticatio-retries) 
due to regular expression 
not matching configuration 
(Ticket 22102) 
May 22, 2025 2.2.1 Set 'no ip bootp server' 
failing validation (Ticket 
21935) 
May 22, 2025 2.2.1 "ip ssh authentication-
retries" failed validation 
(Ticket 21934) 
May 22, 2025 2.2.1 Set 'seconds' for 'ip ssh 
timeout' for 60 seconds or 
less false positive (Ticket 
21918) 
May 22, 2025 2.2.1 Set 'exec-timeout' to less 
than or equal to 10 min on 
'ip http' false positive (Ticket 
21917) 
May 22, 2025 2.2.1 "transport input none" can 
be entered in configuration 
but doesn't appear in config 
so can't be validated (Ticket 
21915) 
May 22, 2025 2.2.1 SCAP for routers and 
switches supporting the 
latest benchmarks. (Ticket 
21575)