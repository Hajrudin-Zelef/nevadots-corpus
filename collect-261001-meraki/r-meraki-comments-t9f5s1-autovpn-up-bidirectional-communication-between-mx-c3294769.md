Auto-VPN Up Bi-Directional communication between MX Hub and spokes down 
        
    I have a MX Hub working as a mesh hub, and three remote sites with mx devices working as a mesh spokes. The auto-vpn is up, but there is no bi-directional traffic between the hub and spokes.
I have an open ticket with Meraki support, and so far they have reset one link for the vpn-registry. However we still see no change.
Any recommendations?
Meilleurs
            Ouvrir les options de tri des commentaires
            
        
    
       
            
          
      
      
        Meilleurs
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Top
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Nouvelles
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Controversées
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Anciennes
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Questions et réponses
        
      
    
    
    
  
  
      
 
    
Section des commentaires
Worked with a higher level of support and spent most of our time troubleshooting from the spoke devices. After we verified that the spoke devices could successfully ping the MX hub inside IP, we moved onto the MX Hub itself and found the MX Hub could no longer ping the next hops (switch) default gateway and that is where the issue was. We changed the MX inside IP to another VLAN and everything came back up.
Check what IPs your WAN is using at the HUB. Have you tried rebooting your ISP router?