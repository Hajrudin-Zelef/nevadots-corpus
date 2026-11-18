VLAN on bridge 
        
     
     Désolé, cette publication a été retirée par les filtres de Reddit.  
         
        
        
        
        
          
        
        
        
        
         Meilleurs
            Ouvrir les options de tri des commentaires
            
        
    
       
            
          
      
      
        Meilleurs
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Top
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Nouvelles
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Controversées
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Anciennes
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Questions et réponses
        
      
    
    
    
  
  
      
 
    
Section des commentaires
Try this. Create vlan 6 interface on Eth1. Add that vlan interface to the existing bridge.
Personally, I’d put a small managed switch in front of the router to properly split the traffic. That way nothing confusing has to happen.
Commentaire supprimé par un membre de l’équipe de modération
Your ether1 is configured for internet access. In other words, it’s not in a bridge the router is using the default route to route out the ether1 interface and is masquerading (NAT) all traffic going out ether1.
By adding the VLAN6 interface to ether1, your added tagged traffic on a virtual interface (VLAN6). You then added that to the existing bridge which turns it into I tagged traffic on the bridge.
Maybe this would help you: https://forum.mikrotik.com/viewtopic.php?f=13&t=143620&sid=1cce1b9f0cad629b3263ebec6b11e5ab