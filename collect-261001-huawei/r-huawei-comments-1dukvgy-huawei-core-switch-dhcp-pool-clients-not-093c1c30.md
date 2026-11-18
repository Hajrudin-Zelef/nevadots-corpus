Huawei Core Switch DHCP Pool: Clients Not Recognizing DNS Server 
        
        
        
    
    
    Hey everyone,
I'm encountering a puzzling issue with my new Huawei core switch and would appreciate some advice or insights. Here's the situation:
We recently needed to create a new DHCP pool at my company: one for servers and VMs (still managed by our Windows Server DHCP), and another for clients (created on the Huawei core switch). The problem arises with clients connected through the Huawei DHCP pool.
Clients on the Huawei DHCP pool seem unable to recognize our DNS server hosted on the Windows Server. Consequently, they're bypassing all security profiles set on our FortiGate firewall.
I've ensured that the DHCP configuration on the Huawei switch includes the correct DNS server IP address (that of our Windows Server). However, clients are either not receiving this information correctly or are unable to utilize it.
Has anyone experienced a similar issue or could provide suggestions on how to troubleshoot this? Any help would be greatly appreciated!
Thanks in advance!
Section des commentaires
Soyez la première personne à commenter
Personne n’a encore répondu à cette publication. Partage ton avis pour lancer la conversation.