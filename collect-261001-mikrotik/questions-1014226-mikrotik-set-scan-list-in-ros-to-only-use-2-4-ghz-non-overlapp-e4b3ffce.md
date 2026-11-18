How do I set the scan-list in MikroTik RouterOS to use only 2.4 GHz non-overlapping channel frequencies?
                    
                        Add a comment
                    
                 | 
            
                
            
        
         
    1 Answer 1
To set the scan-list to ONLY use 2.4 GHz non-overlapping channel 1,6 & 11 in MikroTik RouterOS (currently v6.46.5):
/interface wireless set scan-list=2412,2437,2462 numbers=wlan1
Obviously change wlan1 to the correct value as necessary for the radio you're working with in your MikroTik-