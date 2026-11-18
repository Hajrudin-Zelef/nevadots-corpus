A community-contributed subreddit for all things Mikrotik. General ISP and network discussion also permitted. Please ensure if you're asking a question you have checked the Wiki First: https://help.mikrotik.com

# 
       RouterOS upgrade URLs after 7.13 

      
    Hi everyone!

I have a 4011 in my home running RouterOS 7.12.1.That router has a cron script which curls into https://upgrade.mikrotik.com/routeros/LATEST.7 to check for new versions but it doesn't seem to work anymore (it says that the latest version is 7.12.1!). Do anyone of you know to which URL should I make a request to check for new versions?

Thanks!

Use ros.

/system package update check-for-updates; :delay 5s; :set deviceOsVerAvail [/system package update get latest-version];

Brave to script updates without reboots

That only gives info if there IS an update

maybe read before commenting_

Thank you so much, that’s what I was looking for!

This may be happening because of changes made in wireless package. You have to install 17.12.1 before installing the current one 17.14

I've got a mikroktik hlg lte18 kit and a hlg lte6 kit and I've got a 3 mobile sercomm 4g+ 2122gr router. The 3 mob one blows the others away on speeds and I was thinking that the 2 mikroktik devices would be better

https://github.com/beeyev/Mikrotik-RouterOS-automatic-backup-and-update

http://upgrade.mikrotik.com/routeros/NEWESTa7.stable

from : https://forum.mikrotik.com/viewtopic.php?t=200103