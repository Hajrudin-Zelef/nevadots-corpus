ЕСКЕРТУ! Бұл зертханалық жұмыс нақты физикалық құрылғыны (Physical Device) қолданып жасалған!
Құрылғының моделі: Huawei S3710-H24P4S-A Switch
Login authentication
Password: P@s$w0rd_&1234
Confirm password: P@s$w0rd_&1234display versiondisplay interface briefinterface Vlanif 1
 ip address 192.168.1.1 24
Step1 - Configure Local User Authentication and Authorization
aaa
 local-user student password irreversible-cipher Huawei@123
 local-user student service-type terminal ssh
 local-user student privilege level 3
Step2 - Configure VTY Lines
user-interface vty 0 4
 authentication-mode aaa
 protocol inbound ssh
Step3 - Generate RSA Key
rsa local-key-pair create
Warning: Confirm to replace them! Continue? [Y/N] Y
Input the bits in the modulus[default = 3072]: 2048
Step4 - SSH server Permit interface
ssh server-source -i Vlanif 1
Step5 - Enable SSH
stelnet server enable
display ssh server status
Step6 - Verification
ssh student@192.168.1.1