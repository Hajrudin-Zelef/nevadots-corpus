On the mikrotik wiki there are lot of examples on how to use the http proxy feature.
Anyway:
On the IP/Proxy menu you can set the options of the embedded http proxy: in the "/ip proxy access" part you can define you access policies in terms of http host names or ip addresses.
Then, you have two options to make it working as a trasparent proxy.
A fully trasparent proxy means that the users do not have to configure it in their browser.
http://wiki.mikrotik.com/wiki/How_to_make_transparent_web_proxy
BUT, doing so, you cannot filter HTTPS protocol.
If you want to filter also https using your proxy, I can advise to use a "proxy auto-discover" mechanism, like WPAD.
http://findproxyforurl.com/wpad-introduction/
Is this enough to start?