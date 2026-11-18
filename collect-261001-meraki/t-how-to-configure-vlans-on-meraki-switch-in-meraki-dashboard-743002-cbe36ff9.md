post by timjacob on Dec 12, 2019
      
        
          
            
              
              
                
                  
                
                
                  
                    We have been using D-Link switches, and I have all ports of one particular switch tagged for VLAN 20 and also for VLAN 40. We are replacing it with a Meraki MS125-48 Switch. I cannot see where to tag VLANS on it. Can anyone point me in the right direction?
Many thanks
 
                
                
                  
                
               
             
              
          
        
        
  
 
          
  
  post by MI50 on Dec 12, 2019
      
        
          
            
              
              
                
                  
                
                
                  
                    Your paying for Meraki support
give them a call great support…get you fixed up real quick
 
                
                
                  
                
               
             
          
        
      
  
 
          
  
  post by timjacob on Dec 12, 2019
      
        
          
            
          
        
      
  
 
          
  
  post by mattcavallin on Dec 12, 2019
      
        
          
            
              
              
                
                  
                
                
                  
                    From the Meraki dashboard, click on
- 
Switch>Switches>Switch you want to edit
- 
Click on “Configure ports on this switch”
- 
Check the boxes for the ports you want to configure
- 
Click Edit
- 
For Type - select Trunk
- 
For Allowed VLAN’s, enter 20,40. You could also enter a range, like 20-40
- 
Click Update
Edit: Changed directions to edit multiple ports at once instead of a single port
 
                
                
                  
                
               
             
          
        
      
 
          
  
  post by MI50 on Dec 13, 2019
          
  
  post by eugenebrowne3970 on Dec 13, 2019
          
  
  post by timjacob on Dec 19, 2019