Téléchargé 69 fois
 Ouvre dans une nouvelle fenêtre Ouvre un site Web externe Ouvre un site Web externe dans une nouvelle fenêtre 
      Ce site Web utilise des technologies telles que les cookies pour activer les fonctionnalités essentielles du site, ainsi que pour analyses, personnalisation et publicité ciblée. Pour en savoir plus, consultez le lien suivant :     Politique de confidentialité    
   Préférences en matière de conservation des données
Passer au contenu principal
Téléchargé parJoel W. King
PPTX, PDF4 269 vues
The document introduces the use of Meraki APIs for managing cloud-based networks, emphasizing the development of network programmability and automation using Python and Ansible. It covers practical use cases, tools required for integration, and the necessary skills for network engineers and developers in network operations. Additionally, it highlights resources for workshops and collaboration among engineers to enhance their expertise in network management.
PPTX
Programmability and Automation in Data Center Networks: A talk on Hot Air Bal...
parJoel W. King
29 diapositives1.4K vues
PDF
Windows 10 Creators Update: what’s on tap for business users - Ionut Balan
parITCamp
34 diapositives1.3K vues
Azure tales: a real world CQRS and ES Deep Dive - Andrea Saltarello
parITCamp
41 diapositives1K vues
Provisioning Windows instances at scale on Azure, AWS and OpenStack - Adrian ...
parITCamp
49 diapositives1.8K vues
APIdays Helsinki 2019 - How API Will Help Win the Deals - the Case of Infrast...
parapidays
13 diapositives209 vues
Expanding your impact with programmability in the data center
parCisco Canada
27 diapositives369 vues
Cory Guynn - API Magic and Applications on the Network - Codemotion Milan 2018
parCodemotion
18 diapositives99 vues
18 facets of the OpenAPI specification - Cisco Live US 2023
parCisco DevNet
74 diapositives88 vues
Understanding the Power of the Cisco Platform - 4_30_24 #60PartnerSuccess - P...
parSergioDurn16
27 diapositives74 vues
Optimizing Write-Intensive Database Workloads Masterclass: Why Scaling Writes...
parScyllaDB
42 diapositives11 vues
Comprehensive Azure Migration Readiness Guide: Inventory, Security, Networkin...
paracaptacloud
10 diapositives56 vues
Potential Scope, Advantages, and Challenges of RAS and Bio-floc Technology in...
parB. BHASKAR
6 diapositives15 vues
Agile Gurugram & Delhi National Capital Region 2026 _ The AI Shift_ Skill Gap...
parAgileNetwork
11 diapositives27 vues
How to Make AI-Assisted Writing More Authentic: A Practical Guide to Original...
parAdeel Ali 
16 diapositives33 vues
Foundations and Future of AI & Autonomous Systems: From Basics to Machine Aut...
parAdeel Ali 
9 diapositives26 vues
Introducing a third platform, such as the new MacBook Neo, may multiply compl...
47 diapositives6 vues
When AI Agents Work Together: Exploring A2A, MCP, and Connected AI Frameworks...
24 diapositives10 vues
Comprehensive Guide to Cyber Attacks on Networks and Prevention Techniques
pargm750668
13 diapositives71 vues
BotGentz AI Playbook: Seamless AI Agents Integrated into Your Workflow
parZloadr
8 diapositives33 vues
Comprehensive MEAN Stack Developer Roadmap for 2026: From Fundamentals to Job...
15 diapositives8 vues
- 1. API 102: Programmingwith Meraki APIs Engineering and Innovations - Network Solutions,WorldWideTechnology @joel_w_king @joelwking JoelW. King
- 2. Joel W. King PrincipalArchitect,WorldWideTechnology Joelis Principal Architect of Engineering and Innovations for Network Solutions atWorldWide Technology (WWT). He is responsible for Software-Defined Networking (SDN) and network programmability initiatives. He was previously a technical architect at NetApp (Video Surveillance, Big Data) and Cisco (EncryptedVoIP andVideo) developing validated design guides.
- 3. Abstract Description This isan introduction to using the Meraki Dashboard Application Program Interfaces (APIs).The goal is to demonstrate how query and update a Meraki cloud managed network using the dashboard APIs.We demonstrate the process of learning how to use theAPI documentation and tools to query the API and generate sample code.We show the development and testing of Python code to manage the connection to the Meraki cloud, and then integrate the code as a module in the Ansible framework. We start from nothing and finish with a functional piece of software. Several practical use cases are illustrated. Starting with theAPI documentation, use Postman to query theAPIs, then generate python code from Postman, to showing how to python class and methods can be developed and tested in an IDE, and how to incorporate that into a simpleAnsible module which is used to create a VLAN programmatically.
- 4. Objective Why we needNetwork Programmability Developers Meraki, a Network Automation Learning Environment Use Cases and Workshops Network Automation Integration Cyber Security Integration Meraki Provisioner Data Center Management Switch
- 5. CLI COMMAND LINE INTERFACE DASHBOARD APPLICATIONPROGRAM INTERFACES Rapid adoption of Software-Defined WAN (SD-WAN) NETWORK OPERATIONS CLOUD MANAGED SSH MANAGED
- 6. Perspective Software Engineers • Degreein Computer Science • Attention to detail, logical, structured thinking • Multiple programming languages (Java, Python, PHP, Go) • Experience with source code and version control • Project management frameworks (Agile/Scrum, Lean, Kanban) Network Engineers • Degree in Electrical Engineering, certifications – CCIE, CCNA • Adept at troubleshooting, performance monitoring • Knowledge of routing / control protocols (BGP, OSPF, EIGRP), (QoS, PfR, PoE, NTP, DHCP) • Understand WAN/LAN technologies (Ethernet, Frame Relay, DMVPN, WAAS and MPLS).
- 7. Developing theTeam SECURITY OPERATIONS APPLICATION DELIVERY LOAD BALANCERS DATACENTER NETWORKING CAMPUS BRANCH / WAN WIRELESS VOIP COLLABORATION LINUX ADMIN VIRTUALIZATION NETWORK PROGRAMMABILITY DEVELOPER Programming fundamentals Data serialization formats (JSON,YAML, XML) Application Programming Interfaces (APIs) Device programmability (NXOS-API,ASA-API, Meraki APIs) Process and Procedures for managing Network Operations
- 8. What is…. Cisco Meraki asolution include wireless, switching, security, EMM, communications, and security cameras, all centrally managed from the web. Chrome Postman Web REST client that allows you to enter and monitor HTTP requests and responses Integrated Development Environment (IDE) … software that acts as text editor, debugger and compiler all in one sometimes-bloated but generally useful package.(1) Software Development Kit (SDK) Code written using layers of abstraction to simplify software development. Ansible by Red Hat Open source IT automation software and licensed option, AnsibleTower.
- 9. Meraki, a NetworkAutomation Learning Environment
- 10. 1 Meraki APIs documentation 2 Enable APIaccess 3 Chrome Postman Collection 4 Sample Code Ideal Learning Environment 5 Develop within a Framework developers.merkai.com dashboard.meraki.com ansible.com phantom.us ANSIBLE-MERAKI
- 11.
- 12. • Network operationssuffer a lack of process maturity. • New skills needed in NetOps – NFV / virtualization, Linux, working knowledge of APIs, markup languages. • Workshop resources demonstrate using the Meraki provisioning API to create aVLAN The API is the new CLI NetOps 2.0, Super-NetOps, Infrastructure as Code, Programmable Networks
- 13. • Dashboard APIPostman Collection (developers.meraki.com) developers.meraki.com/post/157014824756/dashboard-api-postman-collection • Workshop Instructions andVagrantfile github.com/joelwking/devnet-create-meraki-api/blob/master/netops/ • Ansible Hacking github.com/joelwking/ansible-hacking • Meraki code github.com/joelwking/ansible-meraki Workshop Resources
- 14. the Set-Up DASHBOARD.MERAKI.COM IDE TOOLS AND DOCUMENTATION API PYCHARMPRO VIRTUAL BOX ANSIBLE ANSIBLE-HACKING ANSIBLE-MERAKI DEVNET-CREATE-MERAKI-API CODE SAMPLES AND CONFIGURATION(S)
- 15.
- 16. Phantom Cyber About us Phantomis the first community- powered security automation & orchestration platform. Phantom’s open and extensible architecture helps you work smarter, respond faster, and strengthen your defenses. Visit www.phantom.us/join to get the Free Community version of Phantom.
- 17. Meraki Dashboard Integrated withinPhantom Cyber’s application framework Locate devices usingAPIs
- 18.
- 19. • Simplified WebGUI to the Meraki dashboard • Enforces business rules for deployment by 3rd party ‘feet on the street’ • Deploying 50 locations per week • Technician uses laptop and barcode scanner • Customer supplied CSV file defines street address, GPS locations, network addressing, etc. Meraki Provisioner Asynchrony Labs, creates and deploying custom applications and mobile solutions leveraging Cisco Meraki technology. www.asynchrony.com/
- 20.
- 21. Meraki as amanagement switch in the data center • NexusTop of Rack (ToR) switch mgmt interface connected to Meraki switch / security appliance • Mgmt interface receives IP address via DHCP • API on Service Request System provides MAC address and hostname ofToR switch • Ansible playbook: – Queries Service Request System for Nexus switch MAC and hostname, Meraki networkId andVLAN – Queries Meraki API to determine mgmt IP assigned via DHCP – Creates hostname entry in Infobox – Generates Fixed IP assignments via Meraki API INFOBLOX SERVICE REQUEST SYSTEM DASHBOARD.MERAKI.COM CONTROL NODE MERAKI SWITCH NEXUS TOP OF RACK
- 23. Result Benefits: • Meraki asa management switch forTop of Rack (ToR) data center switches simplifies operations • No static IP configuration required onToR switch interface mgmt0 • Subsequent playbooks reference ToR switch by the Infoblox DNS entry • PowerOn Auto Provisioning (POAP) USB device - for initial (admin userid and password) config Resources: https://github.com/joelwking/code-samples/blob/master/api-server/SimpleHTTPclient.yml https://github.com/joelwking/devnet-create-meraki-api/blob/master/management_switch/meraki_mgmt_sw.yml
- 24. Workshop Project • Ansibleplaybook uses the URI module to update the fixedIpAssignment • Meraki dashboard uses a HTTP 302 redirect • Python Requests module handles redirects Write Ansible module to replace URI module for this function
- 25.
- 26. Meraki Partner Portal www.merakipartners.com MerakiDeveloper Portal developers.meraki.com/tagged/Automation Experimental Python library leveraging theCisco Meraki Provisioning RESTfulAPI github.com/meraki/provisioning-lib Phantom | Meraki App | Blog github.com/joelwking/Phantom-Cyber#meraki-app blog.phantom.us/2016/05/09/community-double-play/ Ansible - 3WaysToTryTower Free www.ansible.com/tower-trial Resources Eligible partner engineers* that attend will receive a free stack of promo equipment and licensing.
- 27. Create synergy betweendevelopers and engineers We learn best by a physical manipulation of our environment – Meraki APIs and gear Use cases and workshops facilitate learning Key take-aways
- #2 API 102: Programming with Meraki APIs.
- #6 Our customer base is migrating from traditional WAN network infrastructure using CLI (command line interface) to cloud managed solutions, like cisco Meraki. While the cloud managed dashboards increase network operations efficiency, API interfaces enable programmatic interfaces to other systems within the enterprise.
- #8 Traditional networking job roles will be evolving to software-enabled network roles.
- #9 (1) A Beginner's Guide to Integrated Development Environments – Mashable mashable.com/2010/10/06/ide-guide/
- #11 http://developers.meraki.com http://dashboard.meraki.com/api_docs If you have any API related issues or need help, please email support@meraki.com If you are looking for an update on the latest new API features please visit developers.meraki.com/news If you are looking for where to get started with the Meraki developer platform, please visit developers.meraki.com/start For sales and escalations, please contact api-team@meraki.com
- #13 Network function virtualization (NFV)
- #18 https://github.com/joelwking/Phantom-Cyber/tree/master/meraki