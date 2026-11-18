I have queue tree name int-up_bw_duyungoffice and int-down_bw_duyungoffice. Can i grep the words duyungoffice in MikroTik CLI? So the print result is information for int-up_bw_duyungoffice and int-down_bw_duyungoffice.
Default script that i know just like below:
> queue tree print where name=int-down_bw_duyungoffice
Flags: X - disabled, I - invalid     
 0   name="int-down_bw_duyungoffice" 
     parent=TOTAL INTL DOWN - 1Mbps - Blok 1 (1:16) 
     packet-mark=packet-down_bw_duyungoffice limit-at=64k 
     queue=pcq-down-client-1Mbps(1:16) priority=8 max-limit=1M burst-limit=0 
     burst-threshold=0 burst-time=0s
But that script only shows int-down_bw_duyungoffice only.