I am a newbie in MikroTik RouterOS.
How can I add a rule or script to beep when some one tries to connect to a special port on one of interfaces?
For example I need to know if someone is trying to ftp to my server from outside world.
As far as I know, there is no way to do that directly.
Workaround could be using firewall rule to add source ip to addresslist:
/ip firewall filter add action=add-src-to-address-list address-list=beeplist
    address-list-timeout=1m1s chain=input disabled=no dst-port=21 protocol=tcp
and then run script to check'n'beep:
:if ([ :len [ip firewall address-list find where list=beeplist]]>0) do={:beep}
periodically via scheduler:
/system scheduler add interval=60s name=beeplist on-event=":if ([ :len [ip firewall address-list find where list=beeplist]]>0) do={:beep}"
Entries in address-list are going to be deleted via address-list-timeout settings.
Still, you are going to hear a lot of beeps...