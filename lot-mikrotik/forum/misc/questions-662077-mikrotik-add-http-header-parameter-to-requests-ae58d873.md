This question already has an answer here:

I want to add a parameter of MikroTik to all HTTP header requests using MikroTik. So all client and servers that connected to the MikroTik can trace the parameter in their HTTP header.

How can I achive this?

|  | This question already has an answer here: I want to add a parameter of MikroTik to all HTTP header requests using MikroTik. So all client and servers that connected to the MikroTik can trace the parameter in their HTTP header. How can I achive this? | 

|  | You cannot manipulate the HTTP headers with Mikrotik. You need a reverse proxy like HAProxy or Nginx to achieve this. **Update:** Here I describe a solution on how you can add Nginx or HAProxy support on Mikrotik NAT to two different servers on the same port via hostname with Mikrotik RB2011 |