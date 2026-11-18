In the attached topology, there is a network of four Mikrotik routers. They are connected to each other using the OSPF routing protocol. Can I create a routing from between ‘Mikrotik1 and Mikrotik4’ directly using the BGP protocol based on the IP loopbacks or another? If it is not possible, what is the most productive method? With which this can be done.

Attached are the results of my experience and the settings for routers 1 and 4.


Mikrotik1.conf.rsc (1.78 KB)

Mikrotik4.conf.rsc (1.48 KB)


             
            
           
          
            
            
              Yes you can, if you see all Loopback’s IP’s on all routers.

             
            
           
          
            
            
              
all loopback’s it’s see some of them, but the result it as you see