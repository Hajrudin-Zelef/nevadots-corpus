I suggest you check the MikroTik manual regarding IPsec. There's a lot of info and examples there.
Here's an XAuth example from the manual.
  Simple Mutual PSK XAuth Config
  
  Server side config:
/ip ipsec peer
add address=2.2.2.1 auth-method=pre-shared-key-xauth secret="123" passive=yes
/ip ipsec user
add name=test password=345
  
  Client side config:
/ip ipsec peer
add address=2.2.2.2 auth-method=pre-shared-key-xauth secret="123" \
  xauth-login=test xauth-password=345
  
  Note: On server side it is mandatory to set passive to yes when XAuth
  is used.