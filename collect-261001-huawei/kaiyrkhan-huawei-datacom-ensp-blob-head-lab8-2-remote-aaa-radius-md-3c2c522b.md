AAA (Authentication, Authorization, Accounting):
Authentication - User identity (қолданушының жеке басын растау)
Authorization - User Permissions (қолданушының жүйеде жұмыс жасау қолжетімділігі немесе құқығы)
Accounting - User Actions Logging (қолданушының жүйеде жасаған әрекеттері)
Негізгі қолдану аясы:
HWTACACS - желілік құрылғыларды басқару (SSH, Telnet)
RADIUS - қолданушылардың желіге (WiFi, VPN) кіруі
| Device | Role | interface | IP Address / Prefix | Operating System | 
|---|---|---|---|---|
| Ubuntu | RADIUS Server | ens34 | 172.16.128.10 /24 | Linux | 
|  |  | ens32 | DHCP Assigned |  | 
| R1 | RADIUS Client | g0/0/0 | 172.16.128.11 /24 | Huawei VRP | 
| R2 | RADIUS Client | g0/0/0 | 172.16.128.12 /24 | Huawei VRP | 
| R3 | Network Engineer | g0/0/0 | 172.16.128.101 /24 | Huawei VRP | 
| Debian | Network Engineer | ens34 | 172.16.128.102 /24 | Linux | 
|  |  | ens32 | DHCP Assigned |  | 
| Host Machine | Bridge | Loopback1 | 172.16.128.254 /24 | Windows | 
- Basic Device Configuration
  - Configure IP Address
- Create RADIUS Server Template
- Configure AAA Scheme
- Configure AAA Domain
- Enable SSH Server
- Configure VTY User Interface
- Configure Local Backup Authentication
- Verify the Configuration
VMware Workstation Pro ➜ Virtual Machine Settings ➜ Add Hardware Wizard ➜ ...
VMware Workstation Pro ➜ Edit ➜ Virtual Network Editor ➜ Change Settings
student@ubuntu:~$ lsb_release -a
Ubuntu 24.04.4 LTS
student@ubuntu:~$ uname -rs
Linux 6.8.0-101-generic x86_64 GNU/Linuxstudent@ubuntu:~$ ip addressstudent@ubuntu:~$ sudo nano /etc/netplan/50-cloud-init.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens32:
      dhcp4: true
    ens34:
      dhcp4: false
      addresses:
        - 172.16.128.10/24
CTRL+O, ENTER, CTRL+X
ЕСКЕРТУ: YAML файлында бос орындар (indentation) өте маңызды. Әр қатарда 2 бос орын қолдануды ұмытпаңыз! (Tab пернесін қолданбаған дұрыс)
student@ubuntu:~$ sudo netplan apply
немесе
student@ubuntu:~$ sudo netplan trystudent@ubuntu:~$ ip addressstudent@ubuntu:~$ networkctl status
Ping from Ubuntu to Host Machine (Loopback 1)
student@ubuntu:~$ ping -c4 172.16.128.254
Windows+R ➜ Turn off Windows Defender Firewall
FreeRADIUS пакетін (package) орнату
student@ubuntu:~$ sudo apt update
student@ubuntu:~$ sudo apt install -y freeradius freeradius-utils
Daemon-ды жүктеу және автожүктеу қызметін қосу
student@ubuntu:~$ sudo systemctl status freeradius
student@ubuntu:~$ sudo systemctl start freeradius
student@ubuntu:~$ sudo systemctl is-enabled freeradius
student@ubuntu:~$ sudo systemctl enable freeradius
FreeRADIUS пакетінің конфигурациялық файлдар тізімі
student@ubuntu:~$ sudo ls -l /etc/freeradius/3.0/
RADIUS клиенттерді қосу
student@ubuntu:~$ sudo nano /etc/freeradius/3.0/clients.conf
# IPv4 Client
client 172.16.128.0/24 {
    ipaddr = 172.16.128.0/24
    secret = Datacom@123
    nastype = other
}
CTRL+O, ENTER, CTRL+X
немесе
# IPv4 Client
client RADIUS_Client1 {
    ipaddr = 172.16.128.11
    secret = Datacom@123
    shortname = R1
    require_message_authenticator = no
    nastype = other
}
Қолданушыларды қосу
student@ubuntu:~$ sudo nano /etc/freeradius/3.0/users
user1   Cleartext-Password := "Huawei@123"
CTRL+O, ENTER, CTRL+X
ЕСКЕРТУ! Production ортада міндетті түрде MySQL/MariaDB сияқты мәліметтер қорын қолданып, құпиясөзді хэштеу керек!
Huawei Vendor-Specific Attributes (VSA) қосу
student@ubuntu:~$ sudo nano /etc/freeradius/3.0/users
user1   Cleartext-Password := "Huawei@123"
        Service-Type = Login-User,
        Huawei-Exec-Privilege = 15
CTRL+O, ENTER, CTRL+X
немесе
student@ubuntu:~$ sudo nano /etc/freeradius/3.0/users
user1   Cleartext-Password := "Huawei@123"
        Huawei-Exec-Privilege = 15,
        Service-Type = NAS-Prompt-User
CTRL+O, ENTER, CTRL+X
Конфигурациялық файлдың қатесін тексеру
student@ubuntu:~$ sudo freeradius -CX
"Configuration appears to be OK" деген хабарлама шықса, қате жоқ!
Daemon-ды қайта жүктеу
student@ubuntu:~$ sudo systemctl restart freeradius
student@ubuntu:~$ sudo systemctl reload freeradius
UFW конфигурациясы
student@ubuntu:~$ sudo ufw status
student@ubuntu:~$ sudo ufw enable
RADIUS порттарын ашу
student@ubuntu:~$ sudo ufw allow from 172.16.128.0/24 to any port 1812,1813 proto udp
student@ubuntu:~$ sudo ufw allow from 172.16.128.0/24 to any port 22 proto tcp
student@ubuntu:~$ sudo ufw reload
student@ubuntu:~$ sudo ufw status
1812 - Authentication Port Number
1813 - Accounting Port Number
Verify the Configuration
RADIUS Server (Ubuntu)
student@ubuntu:~$ sudo radtest user1 Huawei@123 127.0.0.1 0 testing123
Received Access-Acceptstudent@ubuntu:~$ sudo radtest user1 Huawei@123 172.16.128.10 0 Datacom@123
Received Access-Acceptstudent@debian:~$ ip addressstudent@debian:~$ sudo nano /etc/network/interfaces
# allow-hotplug ens32
auto ens32
iface ens32 inet dhcp
auto ens34
iface ens34 inet static
        address 172.16.128.102
        netmask 255.255.255.0student@debian:~$ ip addressPing from Debian to Ubuntu
student@debian:~$ ping -c2 172.16.128.10
64 bytes from 172.16.128.10: icmp_seq=1 ttl=64 time=1.56 ms
64 bytes from 172.16.128.10: icmp_seq=2 ttl=64 time=0.508 ms
Verify the Configuration
student@debian:~$ sudo apt install -y freeradius-utils
student@debian:~$ sudo radtest user1 Huawei@123 172.16.128.10 0 Datacom@123
Received Access-Accept
Configure the IP Address
int g0/0/0
 ip address 172.16.128.11 24
display ip int brief
Interface                         IP Address/Mask      Physical   Protocol  
GigabitEthernet0/0/0              172.16.128.11/24     up         up        
GigabitEthernet0/0/1              unassigned           down       down      
GigabitEthernet0/0/2              unassigned           down       down      
Ping from Router to Ubuntu
[R1] ping 172.16.128.10
Reply from 172.16.128.10: bytes=56 Sequence=1 ttl=64 time=40 ms
Reply from 172.16.128.10: bytes=56 Sequence=2 ttl=64 time=20 ms
Create a RADIUS Server Template
radius-server template LAN1
 radius-server authentication 172.16.128.10 1812
 radius-server accounting 172.16.128.10 1813
 radius-server shared-key cipher Datacom@123
 quit
Қосымша ақпарат!
radius-server template LAN1
radius-server authentication 172.16.128.10 1812 weight 80
radius-server accounting 172.16.128.10 1813 weight 80
radius-server authentication 172.16.128.9 1812 weight 20
radius-server accounting 172.16.128.9 1813 weight 20
Configure the AAA Scheme
aaa
 authentication-scheme RADIUS
  authentication-mode radius local
  quit
 accounting-scheme RADIUS
  accounting-mode radius
  quit
Configure the AAA Domain
aaa
 domain LAB.LOCAL
  authentication-scheme RADIUS
  accounting-scheme RADIUS
  radius-server LAN1
  quit
 quitdomain LAB.LOCAL admin
Configure the Global default Domain for administrations
domain default_admin admin
domain LAB.LOCAL admin
domain default domain LAB.LOCAL
Verify the Configuration
[R1] test-aaa user1 Huawei@123 radius-template LAN1
Info: Account test time out!
RADIUS серверді "Debug" режимге қосу
student@ubuntu:~$ sudo systemctl stop freeradius
student@ubuntu:~$ sudo freeradius -X
FreeRADIUS Version 3.2.5
Copyright (C) 1999-2023 The FreeRADIUS server project and contributors
There is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE
You may redistribute copies of FreeRADIUS under the terms of the
GNU General Public License
For more information about these matters, see the file named COPYRIGHT
Starting - reading configuration files ... [R1] test-aaa user1 Huawei@123 radius-template LAN1
Info: Account test succeeded!Starting - reading configuration files ...
CTRL+C
student@ubuntu:~$ sudo systemctl start freeradius
student@ubuntu:~$ sudo systemctl status freeradius
[R1]test-aaa user1 Huawei@123 radius-template LAN1
Info: Account test succeeded!
Қосымша ақпарат!
[R1] radius-server test-template LAN1 172.16.128.10 1812 user1 password Huawei@123
SSH server permit interface
ssh server permit interface GigabitEthernet 0/0/2
ssh server permit interface Vlanif 1
ssh server permit interface all
немесе
ssh server-source -i Vlanif 1
ssh server-source all-interface
Enable the SSH Server
stelnet server enable
display ssh server status
rsa local-key-pair create
Warning: Confirm to replace them! Continue? [Y/N] Y
Input the bits in the modulus[default = 2048]: 2048
Configure the VTY User Interface
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound ssh
Configure Local Backup Authentication
aaa
 local-user student password irreversible-cipher Huawei@123
 local-user student service-type terminal ssh
 local-user student privilege level 15
student қолданушыны жүйеден жою: undo local-user student
Troubleshooting Commands
display current-configuration section aaa
display radius-server configuration template LAN1
display domain name LAB.LOCALstudent@ubuntu:~$ sudo tcpdump -i any udp port 1812 -n
Network Engineer (Huawei VRP Router)
[R3] ssh client first-time enable
[R3] stelnet 172.16.128.11
Please input the username: user1
The server is not authenticated. Continue to access it? (y/n)[n]: y
Save the server's public key? (y/n)[n]: y
Enter password: Huawei@123
R1-ге SSH арқылы кіргеннен кейін, төмендегі командаларды орындап көріңіз!
[R1] display privilege state — Privilege Level тексеру
[R1] display users
Network Engineer (Debian Linux)
student@debian:~$ ssh \
-oKexAlgorithms=+diffie-hellman-group1-sha1 \
-oHostKeyAlgorithms=+ssh-rsa \
-oPubkeyAcceptedAlgorithms=+ssh-rsa \
-oCiphers=+aes128-cbc \
user1@172.16.128.11
немесе
student@debian:~$ sudo nano ~/.ssh/config
Host 172.16.128.11
    KexAlgorithms +diffie-hellman-group1-sha1
    HostKeyAlgorithms +ssh-rsa
    PubkeyAcceptedAlgorithms +ssh-rsa
    Ciphers +aes128-cbc
CTRL+O, ENTER, CTRL+X
student@debian:~$ ssh user1@172.16.128.11
Accounting
student@ubuntu:~$ sudo ls -l /var/log/freeradius/radacct/
student@ubuntu:~$ sudo ls -l /var/log/freeradius/radacct/172.16.128.11/
student@ubuntu:~$ tail -f /var/log/freeradius/radacct/172.16.128.11/detail-YYYYMMDD[R1] aaa
      recording-scheme RADIUS
      recording-mode radius LAN1
      quit[R1] aaa
      domain LAB.LOCAL
      command-recording-scheme RADIUS
      quit[R1] command-privilege level 15 recording-scheme RADIUS[R3] stelnet 172.16.128.11
     <R1> system-view
     [R1] display versionstudent@ubuntu:~$ sudo ls -l /var/log/freeradius/radacct/172.16.128.11/
student@ubuntu:~$ tail -f /var/log/freeradius/radacct/172.16.128.11/detail-YYYYMMDD[R1] radius-server template LAN1
     radius-server source interface g0/0/0
     quit
Access Control List (ACL)
[R1] acl 2000
      rule permit source 172.16.128.101 0.0.0.0
      rule permit source 172.16.128.102 0.0.0.0
      rule deny source any
      quit
[R1] user-interface vty 0 4
      acl 2000 inbound
      quit
Idle-Timeout (Автоматты түрде сессияны жабу)
[R1] user-interface vty 0 4
      idle-timeout 10 0  # 10 минут, 0 секунд
      quit