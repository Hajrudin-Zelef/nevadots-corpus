- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-09-2019 08:08 AM - edited 02-21-2020 08:55 AM
Which protocol suits or configs must you enter to build a IPSEC tunnel on ASA and use AH along with ESP? I understand you can use AH in conjunction with ESP but I don't know how you actually configure that or confirm it is being used on a IPSEC tunnel on a ASA or router.
Solved! Go to Solution.
Accepted Solutions
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-09-2019 08:59 AM
An easy way to determine which is in use on an active tunnel is to use the command "show crypto ipsec sa" you will then be able to determine whether you have AH or ESP SAs.
HTH
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-09-2019 08:59 AM
i would strongly suggest to understand each one of them before you implementing..good old school document :
http://www.firewall.cx/networking-topics/protocols/870-ipsec-modes.html
=====️ Preenayamo Vasudevam ️=====
***** Rate All Helpful Responses *****
- Mark as New
- Bookmark
- Subscribe
- Mute
- Subscribe to RSS Feed
- Permalink
- Report Inappropriate Content
03-09-2019 08:59 AM
An easy way to determine which is in use on an active tunnel is to use the command "show crypto ipsec sa" you will then be able to determine whether you have AH or ESP SAs.
HTH