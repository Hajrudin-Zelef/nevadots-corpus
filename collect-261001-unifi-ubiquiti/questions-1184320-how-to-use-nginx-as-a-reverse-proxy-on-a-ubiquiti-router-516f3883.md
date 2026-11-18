NOTE: Upgrading to firmware 2.x or above will allow you to use the new Stretch repository as documented in the tutorial linked to in the question.  The Stretch repository updates the nginx package to 1.10 which supports websockets and HTTP2.  However, the steps and scripts below are still applicable under 2.x and therefore the answer is not being updated to reflect new firmware/package versions to avoid confusion for anyone who doesn't want to upgrade to the 2.x firmware (initial releases of the firmware caused several headaches for early adopters and there are still known performance degradation issues in the 2.x firmware versions).
NOTE 2: This answer assumes you have adequate knowledge of common Linux commands and/or at least have the ability to log into your router via SSH.  If you do not, then you should go brush up on your skills in order to meet those pre-requisites before continuing as I do not plan on covering the basics of Linux scripting or commands in the answer or in subsequent comments.
Background Info & Disclaimer (1.2 is safe, but 1.14 works)
Prior to firmware 2.x releases, the Ubiquiti routers are based off of Debian 7.11 - ie. wheezy - and thus the best you'll get without breaking the norms is nginx 1.6 (by using the wheezy-backports repository rather than or in addition to wheezy).  This will work for websocket support, but newer options like http2 will still be missing.
To get newer versions like 1.14 you'll have to step out of your comfort zone a bit and start using the newer Debian 9.5 repositories - ie stretch and stretch-backports.  I say "step out your comfort zone" not due to a bad experience, but because logic would dictate that there's probably a reason Ubiquiti is using wheezy on the router in firmware versions prior to 2.x.  Thus some risk is implied with using stretch packages when they depend on upgrading other packages that the router may be using (ie. libc6).
So, while I can't say the solution below won't break ANY feature on the router,  I can say that I use a fair amount of the router features (including openvpn server/client, load balancing, and VLANs) and I haven't had any problems.
I've also been able to successfully re-flash the current version of the Ubiquiti EdgeMax OS/firmware (2.0.8) without any issue which reverts all the packages back to their stock/default versions.  (but note that re-flashing the firmware also wipes any tweaks/customization you've made that you didn't store in the /config directory - always store them under /config somewhere and then add a startup script to /config/scripts/post-config.d that maintains a link to them in other locations as needed)
So, I feel pretty comfortable saying that future Ubiquiti updates will likely install fine after this install - though you'd have to reinstall nginx again (possibly using the script below).
Now that the usual disclaimers are out of the way, here's what's working for me.
My Solution (break the rules and get 1.14)
Safety first
- Backup your config.
- If you've already been tinkering with trying to install nginx, it might also be a good idea to go ahead and re-install the latest firmware just to get everything back to a good base point (careful if you've put any tweaks/customizations outside of the usual CLI configure or web interface options - you might lose them).
- Since you'll be tinkering with the services that drive the web interface it would probably be smart to familiarize yourself with the commands needed to flash firmware updates from SSH as well (TIP: add system image https://dl.ui.com/firmwares/edgemax/v1.10.x/ER-e100.v1.10.10.5210345.tar ).
Now for the fun part
SSH* into your router and run the following script (or equivalent statements) which will:
- Setup the appropriate Debian repositories (stretch andstretch-backports )
- Setup the repository priorities to prefer stretch-backports (if you want to use nginx 1.10.3 then modify the script below to preferstretch rather thanstretch-backports by removing it from the priority - 910)
- Configure a startup script that will be used to create the nginx log directory at each system boot (otherwise nginx won't start when the router reboots)
- Download/install the latest stable nginx-light package (1.14.0 as of this writing).
* You should NOT use the CLI feature in the web interface as a substitute for SSH because the web interface will likely become temporarily inoperable as a part of this process.
#! /bin/bash
vcfg=/opt/vyatta/sbin/vyatta-cfg-cmd-wrapper
echo Updating package repositories ...
echo
$vcfg begin
$vcfg delete system package
$vcfg set system package repository stretch url http://http.us.debian.org/debian
$vcfg set system package repository stretch components "main contrib non-free"
$vcfg set system package repository stretch distribution stretch
$vcfg set system package repository stretch-backports url http://http.us.debian.org/debian
$vcfg set system package repository stretch-backports components "main contrib non-free"
$vcfg set system package repository stretch-backports distribution stretch-backports
$vcfg commit
$vcfg end
apt-get update
echo
echo Setting repository priorities ...
echo
echo "Package: *
Pin: release a=stretch
Pin-Priority: 900
Package: *
Pin: release a=stretch-backports
Pin-Priority: 910">/etc/apt/preferences.d/stretch
echo
echo Temporarily stopping the current web interface ...
echo
kill -SIGTERM $(cat /var/run/lighttpd.pid)
echo
echo Installing nginx-light ...
echo
echo "#! /bin/bash
[ -d /var/log/nginx ] || mkdir /var/log/nginx">/config/scripts/post-config.d/create_nginx_log_dir
chmod a+x /config/scripts/post-config.d/create_nginx_log_dir
[ -d /var/log/nginx ] || mkdir /var/log/nginx
apt-get install nginx-light -V -y
echo
echo Restarting the old web interface ...
echo
service nginx stop
/usr/sbin/lighttpd -f /etc/lighttpd/lighttpd.conf
echo
echo Updating the nginx default site listen on non-standard ports ...
echo
sed -i -E 's/^(\s*)(listen\s+(:|\[|\])*)([0-9]+)(;|\s)/\1\2\4\4\5/g' /etc/nginx/sites-enabled/default
echo
echo Starting the nginx service ...
echo
service nginx start
echo
echo Installation complete.
[ Answer "yEnter" to the prompt about restarting services ]
Now you should have a working nginx installation and you should be able to test it by navigating to http://<router IP address>:8080 in your web browser.  Note the use port of 8080 which should have been configured by the sed command in the script above rather than the default of port 80 which would have likely conflicted with your default router web interface.
Is nginx going to be serving as a reverse proxy for your router?
I personally prefer to use a non-standard port (553 in the example config below) for the lighttpd service/GUI provided by default on the router and then use the nginx (listening on standard ports 80/443) as a reverse proxy for lighttpd.  This lets me use one domain name for all my local network web sites including the router (ie. router.myhouse.com, nas.myhouse.com, etc.) If you wish to do the same you can change your GUI port using the configure -> set service gui http(s)-port ### statement or by making the same change in the Config Tree section of the web interface/GUI.
Configure nginx with a reverse proxy config file
Next you'll want to configure nginx to act as a reverse proxy for some resource.  Generally this will be a local network resource accessible by using a specific host/domain name in the URL that resolves to the router's nginx listening port.
To do this you create an nginx config file in your /etc/nginx/sites-enabled directory (or better yet create it under /config/user-data and then create a link to that under the /etc/nginx/sites-enabled directory - this will make it easier to recover from upgrades that reset the /etc directory contents).
There are plenty of examples of nginx configuration files online, but the example below may prove beneficial if you plan on putting your router interface (or some other web site that uses websockets) behind the nginx reverse proxy.  Or if you're looking specifically to put a Synology NAS behind it (the photo station in particular is a bit tricky).
upstream edgemax {
  server 192.168.1.1:553;
  keepalive 32;
}
upstream nas {
  server 192.168.1.7:5001;
  keepalive 32;
}
upstream nasphoto {
  server 192.168.1.5:443;
  keepalive 32;
}
upstream nasfile {
  server 192.168.1.7:7001;
  keepalive 32;
}
server {
  listen 443 ssl http2;
  server_name router.*;
  
  ssl_certificate /config/user-data/ssl_chain_key.pem;
  ssl_certificate_key /config/user-data/ssl_chain_key.pem;
  
  client_max_body_size 0;
  proxy_http_version 1.1;
  proxy_buffering off;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "Upgrade";
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  
  location / {
    proxy_pass https:
  }
}
server {
  listen 443 ssl http2;
  server_name nas.*;
  
  ssl_certificate /config/user-data/ssl_chain_key.pem;
  ssl_certificate_key /config/user-data/ssl_chain_key.pem;
  client_max_body_size 0;
  proxy_http_version 1.1;
  proxy_buffering off;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "Upgrade";
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
  location / {
    proxy_pass https:
  }
  
  location /photo {
    proxy_pass https:
  }
}
server {
  listen 443 ssl http2;
  server_name files.*;
  ssl_certificate /config/user-data/ssl_chain_key.pem;
  ssl_certificate_key /config/user-data/ssl_chain_key.pem;
  client_max_body_size 0;
  proxy_http_version 1.1;
  proxy_buffering off;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "Upgrade";
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
  location / {
    proxy_pass https:
  }
}
server {
  listen 443 ssl http2;
  server_name photos.*;
 
  ssl_certificate /config/user-data/ssl_chain_key.pem;
  ssl_certificate_key /config/user-data/ssl_chain_key.pem;
  client_max_body_size 0;
  proxy_http_version 1.1;
  proxy_buffering off;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection "Upgrade";
  proxy_set_header Host $host;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
  
  rewrite ^/photo/(.*)$ /$1;
  
  location / {
    proxy_pass https:
  }
}
Once the reverse proxy config file is in place you can run sudo service nginx restart to make it effective.  Refer to /var/log/nginx/error.log if there are problems.
TIP #1: If you plan on using a non-standard port for your nginx reverse proxy (ie. the config for your reverse proxy says something other than listen 80 and/or listen 443) then you'd probably be well served to replace proxy_set_header Host $host; in the above config with proxy_set_header Host $http_host;.  This is especially important for websockets that seem to always want to make the web clients request traffic from the default ports otherwise - leading to interfaces that are missing components or live data.  (You can also try proxy_set_header Host $host:$server_port;, but note that this is distinctly different as it will always add a port to the header even when one wasn't used in the original request)
TIP #2: Also note the use of server_name router.* which simply means that using the URL router.<anything>.<anything> to browse the nginx reverse proxy will cause that section of the config to be applied.  (Most examples you'll find use a full/explicit domain name like router.domain.com)
This is useful not only for the purpose of potentially changing domain names/dns names (ie. works for router.home.com as well as router.home.net without changing the config), but also helps keep your server_name entries short.  This is important because having long names will often lead to errors saying something to the effect of "increase your bucket size" when you start nginx.  If you must have long names then you'll likely need to tweak the values of server_names_hash_max_size and/or server_names_hash_bucket_size within your config.
UPDATE:
I have successfully updated my router from EdgeOS version 1.9.1 to every version between and including 2.0.9-hotfix2 with essentially zero issues.  The router did not have nginx on it after the update (as expected), but I was able to run my script (the first code block in this answer) and it came right back.  Keeping your customizations under /config and using scripts under /config/scripts/post-config.d to maintain any changes that don't survive a reboot/upgrade is the key to smooth sailing here.
UPDATE #2:
This process works on smaller routers like the ER-X as well.  However, due to limited storage available on the device you'll be forced to remove the inactive firmware image to make room for nginx.  This needs to be done after each update as well.  To remove the inactive firmware image, run delete system image and answer yes to the prompt(s).
-- My ER-X update process --
- Backup my config!!!!
- Update to the latest firmware and reboot (and verify basic functionality other than nginx)
- Remove the old firmware (which is now the inactive image) using the delete system image command (show system storage reports 67% in use before removal and 33% in use afterwards)
- Verify external pings work (ping 8.8.8.8) and reboot the router if they do not.  This is a known issue in recent firmware versions and has nothing to do with this process or the scripts used.
- Use the scripts/steps above to re-install nginx (show system storage reports 67% in use after installation)
- Run sudo apt-get clean to free up space used by temp files and such (show system storage reports 52% in use after cleanup)