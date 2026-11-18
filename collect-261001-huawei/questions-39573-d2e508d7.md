three routers are connected where
  Router A connected to router B
A (gig0/0/0  ip address 10.10.1.1)====> B(gig0/0/0 ip address 10.10.1.2    
A(gig0/0/0=> ip address 10.11.1.1===>B(gig0/0/1  ip address 10.11.1.2))
Router A connected toRouter C
(A (gig0/0/2=> ip address 10.12.1.1 )====>  C (gig0/0/0  ip address 10.12.1.2)
An IS-IS process is running among nodes. At Router A, Can apply a route policy so that link between Router A to Router B only one link active for certain routes based on next hope Address match condition. for example
if routes 157.1.1.1/32, 57.1.1.1/32, 47.1.1.1/32 has two next hop 10.10.1.1 and 10.11.1.1 for Same destination Router B.how can i apply effectively in ISIS process the below policy:-
ip ip-prefix Next_add index 10 permit 10.11.1.0 30 
ip ip-prefix rouisis index 10 permit 157.1.1.1 32
ip ip-prefix rouisis index 20 permit 57.1.1.1 32  
ip ip-prefix rouisis index 30 permit 47.1.1.1 32 
route-policy isispref permit node 20  
if-match ip-prefix rouisis 
if-match ip next-hop ip-prefix Next_add
apply preference 200