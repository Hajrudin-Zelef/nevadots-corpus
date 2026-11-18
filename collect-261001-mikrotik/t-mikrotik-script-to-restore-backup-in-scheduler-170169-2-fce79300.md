post by xitriyded on Oct 7, 2023
      
        
          
            
              
              
                
                  
                
                
                  
                    Hi!
I’m trying to create a script to restore backup. My script is very simple, but it’s not working from scheduler
:local backupfile "MikroTik_2011UiAS_408-20231006-0923.backup" 
/system backup load name=$backupfile
Any suggestions on how to restore backup from *.backup file?
 
                
                
                  
                
               
             
              
          
        
      
 
          
  
  post by xitriyded on Oct 7, 2023
      
        
          
            
              
              
                
                  
                
                
                  
                    Found solution
Script
:local backupfile "MikroTik_2011UiAS_408-20231006-0923.backup"
/system backup load name=$backupfile password=""
/y
In Scheduler, in On Event: just type script’s name.
 
                
                
                  
                
               
             
          
        
      
 
          
  
  post by rextended on Oct 12, 2023
      
        
          
            
              
              
                
                  
                
                
                  
                    /y at the end is wrong and useless.
 
                
                
                  
                
               
             
          
        
      
 
            
          
  
  post by xitriyded on Feb 15, 2024
      
        
          
            
              
              
                
                  
                
                
                  
                    
Not working without it though
 
                  
                  
                 
                
                
                  
                
               
             
          
        
      
 
          
  
  post by rextended on Feb 15, 2024
      
        
          
            
              
              
                
                  
                
                
                  
                    
Still wrong, becayse "/" before the y is useless, and is still useless inside the scheduler.
"y" only can be used just if the script is tested on terminal.
It's like this and there's nothing you can do about it.
 
                  
                  
                 
                
                
                  
                
               
             
          
        
      
 
          
  
  post by jaclaz on Feb 15, 2024
      
        
          
            
              
              
                
                  
                
                
                  
                    Ok, I have to ask.
Why is a static value assigned to a variable?
I.e., wouldn’t this be the same?
/system backup load name="MikroTik_2011UiAS_408-20231006-0923.backup" password=""
 
                
                
                  
                
               
             
          
        
      
 
          
  
  post by rextended on Feb 15, 2024
          
  
  post by jaclaz on Feb 16, 2024
            
          
  
  post by miku on Mar 23, 2024