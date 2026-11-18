/System scheduler or /System scripts? 
        
        
        
    
    
    Our scripts that only ever run on a schedule and we never run manually get dropped into /system scheduler whereas scripts that we only run manually or run both manually and on a schedule get dropped into /system scripts.
What is best practice and/or common methodology?
Edit: the longest script that we put directly into scheduler is three lines.
Meilleurs
            Ouvrir les options de tri des commentaires
            
        
    
       
            
          
      
      
        Meilleurs
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Top
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Nouvelles
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Controversées
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Anciennes
        
      
    
    
    
  
  
      
 
    
        
    
       
            
          
      
      
        Questions et réponses
        
      
    
    
    
  
  
      
 
    
Section des commentaires
Put all your scripts in /system scripts. Use /system scheduler to call up your scripts.
I also don’t bother to create scripts for items that can be taken care of with a single short command in a scheduler item. E.G.
/system backup save name=“diskN/nightly.backup”
Yeah, but I think OP was considering putting full-blown scripts, not just one-liners, into the scheduler.